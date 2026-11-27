---
title: "Prerequisites"
date: 2026-11-23
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Account and permissions
- An AWS account with **AWS Budgets** alerts enabled; root has MFA and is not used for daily work.
- An IAM user or role for deployment with permission to create: DynamoDB, Cognito, API Gateway, Lambda, IAM roles, S3, CloudFront, WAFv2, SNS, Budgets, CloudWatch Logs.
- Recommended region: **ap-southeast-1 (Singapore)**.

### Tools
| Tool | Version | Check |
| --- | --- | --- |
| Terraform | ≥ 1.9 | `terraform version` |
| AWS CLI | v2 | `aws --version` |
| PowerShell | 7+ (Windows/macOS/Linux) | `$PSVersionTable.PSVersion` |
| Python | 3.12 (backend tests) | `python --version` |
| Node.js | 20 (frontend build) | `node --version` |
| Git | any | `git --version` |

### Credentials
```powershell
aws configure            # or: aws login / aws configure sso
aws sts get-caller-identity
```
The second command must print the expected account and user/role.

{{% notice warning %}}
Never take screenshots while running `aws configure` and never commit access keys to Git. If a key leaks, deactivate it and create a new one immediately.
{{% /notice %}}

### Get the source code
```powershell
git clone <team-repo>
cd <team-repo>
Copy-Item infra/terraform/terraform.tfvars.example infra/terraform/terraform.tfvars
```
Open `terraform.tfvars` and set `alert_emails` to a real address (the deploy script blocks the placeholder). Keep `enable_waf = false` during development.
