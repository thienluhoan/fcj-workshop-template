---
title: "Cost and optimization"
date: 2026-11-27
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

#### AWS Pricing Calculator estimates

Region **ap-southeast-1**, on-demand prices, 730 hours per month, before credits and taxes. SEAL runs in two modes:

| Mode | Configuration | Estimate |
|---|---|---|
| **Off-season** | 1 API instance, 1 NAT gateway, RDS Single-AZ | [USD 130.80 / month](https://calculator.aws/#/estimate?id=3e1217ee16a09c0ad04b2b440b5b804da413ac03) |
| **Contest season** | 2 API instances, 2 NAT gateways, RDS Multi-AZ | [USD 230.29 / month](https://calculator.aws/#/estimate?id=f8aacf3bec82418faf417658ccbd51a097d02c64) |

The two links cover the AWS infrastructure. SQS, Lambda, SES and SNS stay within the free tier, and DeepSeek is billed outside AWS, so they are added in the breakdown below.

![AWS Pricing Calculator, off-season estimate](/images/5-Workshop/5.11-Cost/calculator-offseason.png)

*The off-season estimate in AWS Pricing Calculator: the summary at the top (upfront, monthly and 12-month total) and the list of services with their region and monthly cost. Open the link in the table above to see each service's configuration.*

#### Breakdown

| Service | Assumption | Off-season (USD) | Contest (USD) |
|---|---|---|---|
| EC2 (API) | `t3.small`, USD 0.0264/h, 1 or 2 instances | 19.27 | 38.54 |
| EBS | 20 GB gp3 per instance | 1.92 | 3.84 |
| Application Load Balancer | USD 0.0252/h + 0.5 or 1.5 LCU | 21.32 | 27.16 |
| NAT Gateway | USD 0.059/h + USD 0.059/GB, 1 or 2 gateways | 43.37 | 87.32 |
| Public IPv4 | USD 0.005/h, ALB (2) + NAT (1 or 2) | 10.95 | 14.60 |
| RDS for MySQL | `db.t4g.micro`, 20 GB gp3, Single-AZ or Multi-AZ | 21.01 | 42.75 |
| AWS WAF | 1 web ACL, 5 rules, 1M or 5M requests | 10.60 | 13.00 |
| Route 53 | 1 hosted zone | 0.50 | 0.50 |
| S3 | Frontend, uploads, AI results, CloudTrail logs | 0.30 | 0.30 |
| CloudFront | Always-free tier (1 TB, 10M requests) | 0.00 | 0.00 |
| SQS, Lambda, SES, SNS | Mostly free tier | 0.50 | 1.50 |
| KMS | AWS managed keys | 0.00 | 0.00 |
| CloudWatch | Logs, metrics, alarms | 2.00 | 4.00 |
| ECR | About 1 GB of images | 0.10 | 0.10 |
| DeepSeek API (external) | About USD 0.014 per job; 40 or 600 jobs | 0.55 | 8.28 |
| **Total** | | **132.39** | **241.89** |

- **Over 12 months** (9 off-season + 3 contest months): about **USD 1,917**.
- **Configuration running for the demo**: 2 API instances (one per AZ), 1 NAT gateway, RDS Single-AZ, about **USD 148 / month**, or USD 5 per day.

{{% notice info %}}
The **NAT gateway is the largest item**, more than the servers themselves. That is why S3 traffic goes through the free Gateway Endpoint, the Lambda worker runs outside the VPC (it calls DeepSeek without NAT), and a second NAT gateway is added only for the contest.
{{% /notice %}}

#### Optimizations applied

| Optimization | Effect |
|---|---|
| Stateless runtime stack (NAT, ALB, EC2) can be **deleted between sessions** and recreated in about 10 minutes | While studying, 4 h × 20 days with 1 instance costs about USD 41 / month instead of 126 |
| Seasonal switches: `DesiredCapacity`, `MultiAZ`, second NAT | Pay for high availability only during the contest |
| S3 Gateway Endpoint | No NAT data charge for uploads and AI snapshots |
| Lambda outside the VPC, triggered by SQS | No idle cost, no NAT traffic for model calls |
| Graviton `db.t4g.micro`, burstable `t3.small` in standard credit mode | Right-sized for the load; no surprise CPU credit charges |
| CloudFront always-free tier, long cache for hashed assets | Almost all static traffic served from the edge for free |
| AWS managed KMS keys | No USD 1/month per customer key |
| Log retention (Lambda 14 days, CloudTrail 90 days) | Logs do not grow forever |
| AWS Budgets on usage **before** credits, alerts to SNS | Spending is visible even while credits cover it |
| `RDS stop` for breaks of a few days | Only storage is billed (RDS restarts itself after 7 days) |

#### Further options

- **Compute Savings Plan or Reserved Instance** for the database once the platform runs year-round (about 30–40 % less).
- **NAT instance** (`t4g.nano`) instead of a NAT gateway outside the contest, at the cost of managing it.
- **ECS Fargate** to drop the per-instance OS management; cost is similar at this size.
