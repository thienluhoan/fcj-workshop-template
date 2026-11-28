---
title: "Architecture and data flow"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Architecture diagram

The team drew this diagram in draw.io. It shows the normal-season layout: one NAT gateway and a Single-AZ database. The "contest season" labels mark the parts that are switched on only for a contest (a second NAT gateway and a Multi-AZ standby).

![SEAL AWS architecture](/images/5-Workshop/5.3-Architecture/seal-aws-v3.webp)

#### Network layout

| Tier | Subnets (AZ A / AZ B) | Contents | Route to the Internet |
|---|---|---|---|
| Public | `10.20.0.0/24`, `10.20.1.0/24` | ALB, NAT gateway | Internet Gateway |
| Private app | `10.20.10.0/24`, `10.20.11.0/24` | EC2 API instances | NAT gateway; S3 through the Gateway Endpoint |
| Private DB | `10.20.20.0/24`, `10.20.21.0/24` | RDS MySQL | None |

Security groups chain the tiers: the ALB accepts HTTPS **only from the CloudFront origin-facing prefix list**, the API accepts port 8443 only from the ALB, and the database accepts port 3306 only from the API.

#### Data flow

The numbers match the arrows on the diagram.

| # | Flow | Details |
|---|---|---|
| ① | User → CloudFront | Route 53 resolves the domain to CloudFront. WAF inspects every request. TLS uses the ACM certificate in us-east-1. |
| ② | CloudFront → S3 (frontend) | Static React files. The bucket is private and only CloudFront can read it through Origin Access Control. A CloudFront Function rewrites deep links to `index.html`. |
| ③ | CloudFront → ALB → EC2 | `/api/*`, `/oauth2/authorization/*`, `/login/oauth2/*` and `/uploads/*` go to `origin.<DOMAIN>`. CloudFront adds a secret `X-Origin-Verify` header; the ALB returns 403 without it. The ALB forwards to nginx on port 8443, which proxies to Spring Boot on loopback port 8080. |
| ④ | EC2 → RDS | SQL over TLS with certificate verification, using a least-privilege application login. |
| ⑤ | EC2 → S3 (application) | Uploads and AI snapshots, through the S3 Gateway Endpoint (no NAT cost). |
| ⑥ | EC2 → Internet | GitHub API (commit SHA, file tree, up to 50 source files) and OAuth token exchange, through the NAT gateway. |
| ⑦ | EC2 → SQS Jobs | When a judge asks for an AI review, the API writes a snapshot (manifest, sources, rubric) to S3, saves the job and an outbox row in RDS, then sends a message. The judge's request returns at once. |
| ⑧ | SQS → Lambda | An event source mapping triggers the AI worker, at most 2 jobs at a time. Failed messages go to the Job DLQ. |
| ⑨ | Lambda ↔ S3 | The worker reads the snapshot (checksums verified) and writes `results/{jobId}/output.json` once. |
| ⑩ | Lambda → DeepSeek | Secrets are masked and the input is capped at 240,000 characters before the call. The model output is validated against a JSON schema. |
| ⑪ | Lambda → SQS Results | A short message with the result location. Failures go to the Result DLQ. |
| ⑫ | EC2 ← SQS Results | The API polls the queue, stores the result in RDS and marks the job `SUCCEEDED` or `FAILED`. |
| ⑬ | EC2 → SES | Password-reset OTP and notifications from a verified domain with DKIM. |
| ⑭ | Browser ↔ Google/GitHub | OAuth sign-in. ALB stickiness keeps the OAuth state on one instance. |
| ⑮ | GitHub Actions → AWS | CI/CD: push image to ECR (15a), sync web to S3 (15b), deploy (15c), Run Command (15d), instances pull the image (15e). |
| ⑯ | EC2/Lambda → Parameter Store | Runtime configuration and keys as SecureString parameters, encrypted with KMS. |
| ⑰ | CloudWatch → SNS | Alarms and Budgets send email to the operations team. |

#### Mapping to the AWS Well-Architected Framework

| Pillar | How SEAL applies it |
|---|---|
| **Operational excellence** | All infrastructure is CloudFormation. Releases go through GitHub Actions and Systems Manager Run Command, one instance at a time, with automatic rollback. No SSH: Session Manager only. Logs, alarms and CloudTrail. |
| **Security** | WAF with managed rules and rate limits on login and OTP. Private subnets for API and DB. The origin only accepts CloudFront traffic (prefix list + secret header). Secrets in Parameter Store SecureString. TLS everywhere, S3 Block Public Access, least-privilege IAM roles, GitHub OIDC instead of access keys. |
| **Reliability** | Two Availability Zones, Auto Scaling group with ELB health checks on `/health/ready`, SQS with dead-letter queues and an outbox so no AI job is lost, automated RDS backups. Multi-AZ RDS and a second NAT for contests. |
| **Performance efficiency** | CloudFront caches static files close to users. The AI work runs asynchronously in Lambda so the API stays fast. The Auto Scaling group carries a CPU 60 % target-tracking policy; with the current setting (min = max = 2) it cannot scale yet, see [5.5](../5.5-compute-origin/). |
| **Cost optimization** | Graviton `db.t4g.micro`, `t3.small` instances, one NAT gateway out of season, S3 Gateway Endpoint instead of NAT traffic, serverless AI worker, the runtime stack can be deleted between sessions, Budgets alerts. |
