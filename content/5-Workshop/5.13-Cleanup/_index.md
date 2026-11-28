---
title: "Clean up"
date: 2026-11-27
weight: 13
chapter: false
pre: " <b> 5.13. </b> "
---

{{% notice warning %}}
Deleting is permanent. Agree with your team first, back up anything you want to keep (database snapshot, uploaded files), and check the resource names before each command.
{{% /notice %}}

#### Option A. Pause between sessions (keeps all data)

Delete only the stack billed by the hour. The website still loads, the API answers 502/504 until you deploy it again.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-monitoring --template-file infra/v3/monitoring.yaml --parameter-overrides LoadBalancerName= TargetGroupName=
aws cloudformation delete-stack --region ap-southeast-1 --stack-name fcaj-v3-runtime
```

The first command empties the ALB names in the alarms so they do not fire while the runtime is gone. For a break of several days, also stop the database (it starts again by itself after 7 days):

```powershell
aws rds stop-db-instance --region ap-southeast-1 --db-instance-identifier fcaj-v3-mysql
```

To resume: start RDS, deploy the runtime stack as in [5.5](../5.5-compute-origin/) with `DesiredCapacity=2 HealthCheckType=ELB`, then set the new ALB and target group names in the monitoring stack. The instances release by themselves because the database is already set up.

#### Option B. Remove everything

Delete in the reverse order of creation, waiting for each stack to finish (`aws cloudformation wait stack-delete-complete …`):

| # | Stack | Region | Note |
|---|---|---|---|
| 1 | `fcaj-v3-monitoring` | ap-southeast-1 | Alarms, SNS topic, budget |
| 2 | `fcaj-v3-cicd` | ap-southeast-1 | Also delete the `aws-v3` environment and variables on GitHub |
| 3 | `fcaj-v3-ses` | ap-southeast-1 | |
| 4 | `fcaj-v3-ai-worker` | ap-southeast-1 | |
| 5 | `fcaj-v3-edge` | **us-east-1** | CloudFront takes about 15 minutes to disable |
| 6 | `fcaj-v3-runtime` | ap-southeast-1 | |
| 7 | `fcaj-v3-audit` | ap-southeast-1 | Stops CloudTrail |
| 8 | `fcaj-v3-base` | ap-southeast-1 | Turn off RDS deletion protection first; a final snapshot is kept |
| 9 | `fcaj-poc` | ap-southeast-1 | |

```powershell
aws rds modify-db-instance --region ap-southeast-1 --db-instance-identifier fcaj-v3-mysql --no-deletion-protection --apply-immediately
aws cloudformation delete-stack --region ap-southeast-1 --stack-name fcaj-v3-base
```

#### Resources kept on purpose

Some resources have `DeletionPolicy: Retain` or `Snapshot` so that deleting a stack by mistake never loses data. Remove them by hand once you are sure:

| Resource | How to remove |
|---|---|
| Web bucket `fcaj-v3-web-…` and data bucket `fcaj-poc-data-…` (versioned) | S3 console → **Empty** (removes all versions) → **Delete** |
| SQS queues `fcaj-poc-ai-*` (4 queues) | SQS console → Delete |
| ECR repository `fcaj-poc-backend` | `aws ecr delete-repository --repository-name fcaj-poc-backend --force` |
| Log group `/fcaj/poc/backend` | CloudWatch → Log groups → Delete |
| RDS final snapshot and manual snapshots | RDS → Snapshots → Delete (each one costs storage every month) |
| Parameters `/fcaj/v3/*` | `aws ssm delete-parameters --names …` |
| Route 53 hosted zone | Delete the remaining records, then the zone (USD 0.50 per month while it exists). Point the domain back at the registrar's DNS if you keep using it. |

#### Final check

- **Billing → Bills**: no running charges for EC2, NAT gateway, ELB, RDS the next day.
- **Resource Explorer** or **Tag Editor** search for `fcaj` in both regions returns nothing you did not mean to keep.
- **Cost Explorer**, grouped by service, falls to near zero within 48 hours.
