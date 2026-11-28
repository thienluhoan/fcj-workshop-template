---
title: "Overview"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### The problem

A hackathon has a short, intense life: a few quiet months, then a contest month where hundreds of students register, submit and get judged within days. Judges read dozens of repositories under time pressure. SEAL must therefore:

- stay available during the contest, even if one server or one Availability Zone fails;
- keep student data and secrets private;
- cost little between seasons and scale up only for the contest;
- never let a slow AI model block the judge's screen.

#### What you will build

| Layer | AWS services | Role |
|---|---|---|
| Edge | Route 53, CloudFront, WAF, ACM (us-east-1) | One HTTPS entry point, static files from S3, API calls forwarded to the origin |
| Network | VPC, subnets in 2 AZs, Internet Gateway, NAT Gateway, S3 Gateway Endpoint | Public, private app and private DB tiers |
| Compute | ALB, ACM (ap-southeast-1), EC2 Auto Scaling, Systems Manager | Spring Boot API on 1–2 instances, deployed without SSH |
| Data | RDS for MySQL, S3, Parameter Store | Relational data, uploads and AI snapshots, encrypted configuration |
| AI | SQS (jobs, results, two DLQs), Lambda | Asynchronous repository analysis |
| Operations | CloudWatch, SNS, Budgets, CloudTrail, ECR, GitHub Actions | Alarms, cost alerts, audit, CI/CD |

#### How this workshop is organised

Each lab deploys one CloudFormation stack, in the same order the team used for production:

1. `fcaj-poc` and `fcaj-v3-base`: data stores, network, database ([5.4](../5.4-network-data/)).
2. `fcaj-v3-runtime`: NAT, ALB, Auto Scaling group ([5.5](../5.5-compute-origin/)).
3. `fcaj-v3-edge` in us-east-1: CloudFront, WAF, DNS ([5.6](../5.6-edge-security/)).
4. `fcaj-v3-ai-worker`: Lambda AI worker ([5.7](../5.7-ai-worker/)).
5. `fcaj-v3-ses`, `fcaj-v3-cicd`, `fcaj-v3-monitoring`, `fcaj-v3-audit` ([5.8](../5.8-email-ses/), [5.9](../5.9-cicd/), [5.10](../5.10-testing-validation/)).

{{% notice info %}}
All templates live in the team repository under `infra/cloudformation/` and `infra/v3/`. Values such as the account ID, hosted zone ID, secrets and instance IDs are written as placeholders (`<ACCOUNT_ID>`, `<ZONE_ID>`, `<DOMAIN>`…). Replace them with your own values and never commit real secrets.
{{% /notice %}}

**Estimated time:** 3–4 hours (most of it waiting for RDS, certificates and CloudFront).

**Estimated cost:** about USD 5 per day while the runtime stack is running; see [5.11](../5.11-cost/).
