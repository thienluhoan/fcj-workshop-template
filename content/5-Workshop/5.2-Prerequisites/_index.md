---
title: "Prerequisites"
date: 2026-11-27
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### AWS account

1. An AWS account you own. The team used a **dedicated, empty account** funded by credits, so the workshop never touches other workloads.
2. Turn on MFA for the root user, then stop using root.
3. **Billing → Budgets**: create a cost budget with alerts at 25/50/75/90 %. Untick **Credits** in the cost settings, otherwise the chart always shows 0.
4. **IAM → Users**: create an admin user (for example `seal-admin`) with console access, the `AdministratorAccess` policy and MFA.

{{% notice warning %}}
A Free-plan account closes when the credits run out or after 6 months, and limits instance sizes (EC2 up to `t3.small`, RDS `db.t4g.micro` Single-AZ). The templates default to those sizes. If a stack fails with a plan error, decide with your team before upgrading to a Paid plan.
{{% /notice %}}

![Budget fcaj-v3-monthly overview](/images/5-Workshop/5.2-Prerequisites/budget.png)

![Budget fcaj-v3-monthly alerts at 50/80/100 %](/images/5-Workshop/5.2-Prerequisites/budget-alerts.png)

*The `fcaj-v3-monthly` budget (USD 50 per month) that the monitoring stack creates in [5.10](../5.10-testing-validation/), with alerts at 50, 80 and 100 % of actual cost. The manual budget from step 3 covers the account until that stack exists.*

#### Tools on your machine

| Tool | Version used | Check |
|---|---|---|
| AWS CLI | v2 | `aws --version` |
| Git | any | `git --version` |
| Docker Desktop | with buildx | `docker version` |
| Node.js | 20+ | `node -v` |
| PowerShell | 7 (or Git Bash) | `$PSVersionTable.PSVersion` |

Sign in to the CLI with your IAM user and check the identity. The ARN must end with your user name, not `root`:

```powershell
aws login
aws sts get-caller-identity --query "[Account,Arn]" --output text
```

#### Other accounts

- **A domain name.** The team bought one from a Vietnamese registrar (iNET) and delegated all of its DNS to Route 53 in [5.4](../5.4-network-data/).
- **The SEAL repository** cloned locally. Every command below runs from its root folder.
- **A DeepSeek API key** for the AI worker (paid outside AWS). Sending students' source code to an external model needs the organiser's consent.
- Optional: Google and GitHub OAuth apps for social login.

#### Region

Everything runs in **Asia Pacific (Singapore) `ap-southeast-1`**, except the CloudFront certificate and the WAF web ACL, which CloudFront requires in **US East (N. Virginia) `us-east-1`**.
