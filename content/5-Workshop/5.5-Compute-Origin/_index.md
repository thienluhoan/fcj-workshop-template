---
title: "Compute and origin"
date: 2026-11-27
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

The `fcaj-v3-runtime` stack holds everything billed by the hour: the NAT gateway, the Application Load Balancer and the Auto Scaling group of API instances. Because it holds no data, you can delete it after a session and deploy it again later ([5.13](../5.13-cleanup/)).

#### Step 1. Deploy the runtime stack

Read the origin secret into a variable without printing it:

```powershell
$ORIGIN = aws ssm get-parameter --region ap-southeast-1 --with-decryption --name /fcaj/v3/edge/origin-secret --query Parameter.Value --output text
```

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-runtime --template-file infra/v3/runtime.yaml --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> OriginSecret=$ORIGIN
```

What the template creates:

| Resource | Setting | Why |
|---|---|---|
| NAT gateway + Elastic IP | In public subnet A, default route of the app subnets | Outbound calls to GitHub, OAuth providers and ECR |
| ALB, HTTPS listener 443 | `ELBSecurityPolicy-TLS13-1-2-2021-06`, certificate for `origin.<DOMAIN>` | TLS from CloudFront to the origin |
| Default action | Fixed response **403** | Anything without the secret header is refused |
| Listener rule | Forward only when `X-Origin-Verify` equals the origin secret | Only CloudFront can reach the API |
| Target group | HTTPS 8443, health check `/health/ready` every 15 s, stickiness 1 h | Health includes the database; OAuth state stays on one instance |
| Launch template | Ubuntu 24.04, `t3.small`, encrypted gp3, IMDSv2 required | No SSH key, access through Session Manager only |
| Auto Scaling group | App subnets A and B, min = desired, max 2, ELB health checks, CPU 60 % target-tracking policy | Spread across two AZs, replaces failed instances |
| Route 53 record | `origin.<DOMAIN>` → ALB | Origin name used by CloudFront |

At boot, the user data installs Docker and nginx, writes a self-signed certificate for nginx on port 8443 and installs `/usr/local/bin/seal-release`. That one script is used for the first boot, for every scale-out and for every release. It:

1. reads `runtime-env`, `migration-env` and `release` from Parameter Store;
2. checks that the image is an ECR URI pinned by `sha256` digest and that the revision is a 40-character Git SHA;
3. pulls the image, verifies the database schema, starts the container on `127.0.0.1:8080` only, and waits for `/health/ready`.

Container logs go to the CloudWatch log group `/fcaj/poc/backend` through the Docker `awslogs` driver.

![Runtime ALB, Listeners and rules tab](/images/5-Workshop/5.5-Compute-Origin/alb-listener.png)

*The `internet-facing` ALB is **Active** across the two public subnets in `ap-southeast-1a` and `1b`. It has a single **HTTPS:443** listener with the `ELBSecurityPolicy-TLS13-1-2-2021-06` policy and the domain's ACM certificate; there is no HTTP 80 listener. The **default action** returns `403 Forbidden`, so any request without CloudFront's secret header is refused. Valid requests follow the forward rule (the *Rules* column shows 2: the forward rule and the default). The ARN (which contains the account ID) is masked.*
![Rules of the HTTPS:443 listener](/images/5-Workshop/5.5-Compute-Origin/alb-listener-rules.png)

*The two rules of the HTTPS:443 listener. **Priority 1**: when the `X-Origin-Verify` header equals the secret value CloudFront adds to every request, forward to the API target group. **Last (default)**: every other request gets `403 Forbidden`. Together with the security group that only admits the CloudFront prefix list, nobody can call the origin directly. "Target group stickiness: Off" is the stickiness of the forward action (used when splitting across several target groups); SEAL's `lb_cookie` stickiness is set on the target group itself. The header value and the ARN are masked.*

#### Step 2. Load the database

On the very first launch the release step fails because the database has no application users yet. This is expected and the stack still completes.

The team restored its existing database from a private dump (it contains real e-mail addresses and password hashes, so it is **not** in the repository). The script `infra/v3/restore-db.sh`, run once on an API instance through **Session Manager**:

- checks the SHA-256 of the dump and refuses to run if the database is not empty;
- asks for the RDS master password (from Secrets Manager, output `MasterSecretArn`);
- loads the dump, creates the two application logins `fcaj_v3_runtime` and `fcaj_v3_migration` with only the privileges they need;
- marks schema V1 as the Flyway baseline, applies V2 (AI jobs) and V3, then starts the backend.

```bash
sudo /snap/bin/aws s3 cp s3://fcaj-poc-data-<ACCOUNT_ID>-ap-southeast-1/poc/restore/restore-db.sh /root/restore-db.sh
sudo bash /root/restore-db.sh
```

The expected output ends with `RESTORE_OK 31 tables` and `RELEASE_OK`. Afterwards, delete the dump from the instance and from S3, **including all object versions**.

`restore-db.sh` only runs once, on an empty database, so the screenshot below shows a re-run of `seal-release` on a running instance (Session Manager, `sudo /usr/local/bin/seal-release`). It is the same step `restore-db.sh` calls at the end, and the command the CI/CD pipeline runs on each instance ([5.9](../5.9-cicd/)).

![Session Manager running seal-release, ending with RELEASE_OK](/images/5-Workshop/5.5-Compute-Origin/session-manager-release.png)

*Reading top to bottom:*

- *The script pulls the image from ECR **by digest** `sha256:8a28…`, not by tag. The image is already present, so Docker reports "Image is up to date".*
- *It copies the RDS truststore and `deploy-verified.sh` out of the image.*
- *Flyway connects to RDS over TLS (`sslMode=VERIFY_IDENTITY`), validates that **4 migrations** match the history, and prints `PASS: candidate schema and migration history ready for deploy`. On a schema mismatch the script stops here and never touches the container.*
- *The `seal-backend` container listens only on `127.0.0.1:8080`. The image did not change, so the container is kept as is ("Up 15 hours"), with no downtime.*
- *The final `RELEASE_OK c0f469a…` is the commit SHA returned by `/health/version`. The script prints it only when the version matches and `/health/ready` passes.*
- *The account ID in the ECR address and the RDS endpoint are masked.*

The "credentials are stored unencrypted" warning at the top refers to the ECR token that `docker login` saves in `/root/.docker/config.json`. Only root can read it, it expires after 12 hours, and it can only pull images. The `amazon-ecr-credential-helper` removes the warning.

#### Step 3. Check the targets and scale to two AZs

**EC2 → Target groups → Targets** must show the instance as `healthy`. Then run two instances, one per AZ, with ELB health checks so the group replaces an instance whose application stops answering:

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-runtime --template-file infra/v3/runtime.yaml --parameter-overrides DesiredCapacity=2 HealthCheckType=ELB
```

![Runtime Auto Scaling group, Details tab](/images/5-Workshop/5.5-Compute-Origin/asg-details.png)

*The Auto Scaling group after the redeploy with `DesiredCapacity=2`:*

- **Desired capacity 2**, *Scaling limits* **2 - 2** (min = max = 2), status **At desired capacity**.
- **Launch template**: `t3.small`, Ubuntu 24.04 AMI, one security group (the app tier's), **no key pair**, because instances are reached only through Session Manager and SSH is never opened.
- **Network**: the two app subnets in `ap-southeast-1a` and `1b` with *Balanced best effort* distribution, so the two instances always sit in different AZs.
- The group ARN and the ARN of the user that created the launch template (both contain the account ID) are masked.

{{% notice info %}}
The template sets `MinSize` equal to `DesiredCapacity` and `MaxSize` to 2. With two instances, min = max = 2, so the CPU 60 % target-tracking policy **cannot** add or remove instances: the Auto Scaling group here **replaces failed instances** (ELB health checks) and keeps one instance per AZ. To really scale with load during a contest, keep `MinSize=2` and raise `MaxSize` (for example to 4), accepting the higher EC2 cost.
{{% /notice %}}

![Target group with two healthy targets](/images/5-Workshop/5.5-Compute-Origin/target-group-healthy.png)

*The API target group: target type **Instance**, protocol **HTTPS:8443** (nginx on each instance terminates TLS and forwards to loopback 8080). Both **2 targets are Healthy, 0 Unhealthy**, one `fcaj-v3-api` instance per AZ (`ap-southeast-1a` and `1b`), so the API survives the loss of one AZ. The ARN and instance IDs are masked.*

The origin must **not** answer from the Internet directly. From your machine this request must fail (timeout), because the ALB only accepts the CloudFront prefix list:

```powershell
curl.exe -m 10 -sI https://origin.<DOMAIN>/
```
