---
title: "Network and data layer"
date: 2026-11-27
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

In this lab you hand the domain to Route 53, then deploy two stacks: `fcaj-poc` (application bucket, SQS queues, ECR, log group) and `fcaj-v3-base` (VPC, security groups, RDS, web bucket, IAM role, regional certificate).

#### Step 1. Create the hosted zone and delegate the domain

Create the hosted zone **once**. A second zone gets a different set of name servers.

```powershell
aws route53 create-hosted-zone --name <DOMAIN> --caller-reference seal-v3-1
```

Note the `Id` (`<ZONE_ID>` is the `Z…` part after `/hostedzone/`) and the four `NameServers`.

At your registrar (the team used iNET), open the domain, choose **change name servers** (not "add DNS record") and enter the four `ns-….awsdns-….` names. Wait until this command returns those four names; it takes from a few minutes to 24 hours:

```powershell
nslookup -type=NS <DOMAIN> 8.8.8.8
```

![Domain name servers at iNET moved to Route 53](/images/5-Workshop/5.4-Network-Data/inet-nameservers.png)

*The domain page at iNET after the name server change: **DNS management** shows Amazon Route 53 and iNET keeps no DNS records (0), so every record is now served by Route 53. The owner name is masked.*

![Route 53 hosted zone, first part of the record list](/images/5-Workshop/5.4-Network-Data/hosted-zone.png)

![Route 53 hosted zone, rest of the record list](/images/5-Workshop/5.4-Network-Data/hosted-zone-records.png)

*The public hosted zone once the whole workshop is done (13 records). Right after this step the zone holds only **NS** (the four name servers entered at iNET) and **SOA**. The other records are created by later stacks:*

| Record | Type | Created in |
|---|---|---|
| Apex domain → CloudFront | A, AAAA (alias) | [5.6](../5.6-edge-security/), edge stack |
| `origin.` → ALB | A (alias) | [5.5](../5.5-compute-origin/), runtime stack |
| `_…` ACM certificate validation (two certificates: us-east-1 and ap-southeast-1) | CNAME | Step 2 below and the edge stack |
| Three DKIM records, `_dmarc`, `mail.` (MX and SPF for MAIL FROM) | CNAME, TXT, MX | [5.8](../5.8-email-ses/), SES stack |

#### Step 2. Deploy the data stack

`JobVisibilityTimeout=1080` keeps a job hidden for six times the Lambda timeout (180 s), as AWS recommends for SQS event sources.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-poc --template-file infra/cloudformation/poc.yaml --parameter-overrides JobVisibilityTimeout=1080
```

The stack creates the application bucket `fcaj-poc-data-<ACCOUNT_ID>-ap-southeast-1`, the queues `fcaj-poc-ai-jobs` and `fcaj-poc-ai-results` with their dead-letter queues (5 receives before a message moves to the DLQ), the ECR repository `fcaj-poc-backend` and a log group.

![Problem statements uploaded to the application bucket](/images/5-Workshop/5.4-Network-Data/s3-problems.png)

*The application bucket after a coordinator uploads the problem statement of a track:*

- *Files are stored under `poc/problems/<eventId>/<trackId>/<random UUID>.pdf`, here event 3, track 5. The original file name is not used as the key, so paths cannot be guessed and two files with the same name never overwrite each other.*
- ***Show versions** is on: the bucket is versioned, so every object has a **Version ID**. The folder holds the file uploaded today and an older one from the day before, each with its own key.*
- *Other prefixes in `poc/` hold avatars and the AI job snapshots (`inputs/`, `rubrics/`, `results/`, see [5.7](../5.7-ai-worker/)).*
- *The account ID in the bucket name is masked.*

![User avatars in the application bucket](/images/5-Workshop/5.4-Network-Data/s3-avatars.png)

*Prefix `poc/avatars/` after a user changes their profile picture:*

- *Each avatar is stored as `<random UUID>.<ext>`. The backend accepts 1 byte to 5 MB and decides the type (png, gif, webp or jpg) from the file content, not from the file name.*
- *The bucket stays private. The browser loads `https://<DOMAIN>/uploads/avatars/<name>`; the `/uploads/*` behavior sends it to the ALB and the backend reads the object from S3. Responses carry `Cache-Control: no-cache`, so a new picture shows at once.*
- *When a user replaces an avatar, the old object is deleted only after the database change commits; if it rolls back, the new upload is deleted instead.*
- *The account ID in the bucket name is masked.*

#### Step 3. Deploy the base stack

The ALB security group only admits CloudFront. Look up the CloudFront origin-facing prefix list in your region:

```powershell
aws ec2 describe-managed-prefix-lists --region ap-southeast-1 --filters Name=prefix-list-name,Values=com.amazonaws.global.cloudfront.origin-facing --query "PrefixLists[0].PrefixListId" --output text
```

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-base --template-file infra/v3/base.yaml --capabilities CAPABILITY_IAM --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> CloudFrontPrefixListId=<PL_ID>
```

This takes 10–20 minutes. What the template creates:

- VPC `10.20.0.0/16` with six subnets in two AZs (public, app, DB) and three route tables. The DB route table has no route to the Internet.
- An **S3 Gateway Endpoint** on the app route table, so S3 traffic never pays for NAT.
- Three chained security groups: ALB ← CloudFront prefix list on 443, API ← ALB on 8443, DB ← API on 3306.
- **RDS MySQL 8.4** `db.t4g.micro`, 20 GB gp3, encrypted, not public, deletion protection on, a snapshot on delete, master password managed by AWS in Secrets Manager. A parameter group sets `lower_case_table_names=1` and `utf8mb4`, which can only be set at creation.
- The private web bucket (versioned, encrypted, Block Public Access) for the frontend.
- The IAM role and instance profile of the API instances.
- The ACM certificate for `origin.<DOMAIN>`, validated through DNS in the hosted zone.

{{% notice tip %}}
If the stack waits a long time on `RegionalCertificate`, the name servers at the registrar are not yet the Route 53 ones. Fix step 1 and the certificate validates by itself.
{{% /notice %}}

Read the outputs; you will need `DatabaseAddress`, `WebBucketDomain` and `MasterSecretArn`:

```powershell
aws cloudformation describe-stacks --region ap-southeast-1 --stack-name fcaj-v3-base --query "Stacks[0].Outputs[].[OutputKey,OutputValue]" --output table
```

![Resource map of the fcaj-v3 VPC](/images/5-Workshop/5.4-Network-Data/vpc-resource-map.png)

*Resource map of the `fcaj-v3` VPC (10.20.0.0/16) after step 2: six subnets split across two AZs, one route table per tier.*

- `v3-public`: default route to the Internet Gateway, used by the ALB and the NAT gateway.
- `v3-app`: three routes, namely local, `0.0.0.0/0` through the NAT gateway, and the S3 prefix list through the Gateway Endpoint (`vpce-…`), so S3 traffic never crosses the NAT.
- `v3-db`: only the local route, so the database has no path to the Internet.
- The route table with no subnet associations is the main route table AWS creates with every VPC and is not used.
- The everyday setup has a single NAT gateway. Contest season adds a second one for AZ B ([5.11](../5.11-cost/)).
![RDS fcaj-v3-mysql, Connectivity & security tab](/images/5-Workshop/5.4-Network-Data/rds.png)

*The `fcaj-v3-mysql` database is **Available**: MySQL Community, class `db.t4g.micro`, placed in `ap-southeast-1a` (Single-AZ in the everyday setup). **Internet access gateway: Disabled** means the database accepts no connection from the Internet; only the API instances in the `sg-app` security group reach port 3306. The endpoint and the admin user name are masked.*

#### Step 4. Store the configuration in Parameter Store

The team's script generates the database passwords and the origin secret without printing them and never overwrites an existing parameter. Add `-DryRun` to preview.

```powershell
.\infra\v3\put-parameters.ps1 -Domain <DOMAIN> -Release "<image@digest> <git sha>"
```

| Parameter | Type | Content |
|---|---|---|
| `/fcaj/v3/backend/runtime-env` | SecureString | Spring profile, JDBC URL with `sslMode=VERIFY_IDENTITY`, app DB login, JWT secret, bucket and queue URLs, feature flags |
| `/fcaj/v3/backend/migration-env` | SecureString | Separate DB login for schema migrations |
| `/fcaj/v3/backend/release` | String | Image URI pinned by digest and its Git SHA |
| `/fcaj/v3/edge/origin-secret` | SecureString | Value of the `X-Origin-Verify` header |
| `/fcaj/v3/worker/deepseek-api-key` | SecureString | Entered by hand in the console in [5.7](../5.7-ai-worker/) |

The release value comes from the first image build. Build and push it as in [5.9](../5.9-cicd/) step 1, or let the pipeline do it after the stacks exist.

{{% notice warning %}}
Never paste these values in chat, tickets or Git. The application uses its own logins (`fcaj_v3_runtime`, `fcaj_v3_migration`), not the RDS master user, whose password AWS rotates.
{{% /notice %}}
