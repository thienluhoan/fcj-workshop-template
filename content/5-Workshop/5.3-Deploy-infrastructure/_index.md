---
title: "Deploying the infrastructure with Terraform"
date: 2026-11-23
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Code layout
```text
infra/terraform/
├── main.tf              # calls 7 modules
├── variables.tf         # region, budget, enable_waf flag, Cognito groups...
├── outputs.tf           # API URL, User Pool ID, buckets, CloudFront...
├── modules/
│   ├── sns/             # securetable-alerts topic
│   ├── budgets/         # 10 USD, alerts at 50/80/100%
│   ├── dynamodb/        # 5 on-demand tables + GSI + PITR
│   ├── cognito/         # User Pool, MFA ON, admin-only, 5 groups
│   ├── apigateway/      # REST API, authorizer, throttling, /health
│   ├── s3_cloudfront/   # 3 buckets + CloudFront OAC + security headers
│   └── waf_v2/          # Web ACL (toggled by enable_waf)
└── tests/securetable.tftest.hcl   # 10 tests, mock provider
```

### Step 1 – Pre-deployment checks (free)
```powershell
cd infra/terraform
terraform fmt -check -recursive
terraform init -backend=false
terraform validate
terraform test          # runs with a mock provider, no AWS account needed
```
Expected: `Success! The configuration is valid.` and `10 passed, 0 failed`.

### Step 2 – Deploy and verify automatically
```powershell
cd ../..
.\scripts\deploy_and_test.ps1             # WAF off (default)
.\scripts\deploy_and_test.ps1 -OpenBrowser # also opens the browser
```
The script:
1. Checks Terraform, AWS CLI and credentials (`aws sts get-caller-identity`).
2. Runs `terraform plan -out=securetable.tfplan` then `terraform apply`.
3. Exports outputs to `backend-config/deployed_env.json` and `deployed_env.env` (`VITE_...` variables for the frontend, table names for the backend).
4. Calls AWS to verify the real state: 5 DynamoDB tables `ACTIVE`, User Pool MFA `ON`, 5 Cognito groups, S3 Block Public Access, SNS subscription, API stage, CloudFront.
5. Sends `GET /health` and expects `{"status":"healthy"}`.

*(Screenshot: green script output – team to add)*

### Step 3 – Confirm the SNS email
Open the `alert_emails` inbox and click **Confirm subscription**. Until confirmed, alarms and Budgets cannot send email.

### Check in the Console
- **DynamoDB → Tables**: `securetable-orders-dev`, `securetable-menu-items-dev`, `securetable-inventory-dev`, `securetable-daily-finance-dev`, `securetable-audit-logs-dev`.
- **Cognito → User pools**: `securetable-users-dev`, Groups tab lists ADMIN, INVENTORY, RECEPTION, WAITER, KITCHEN.
- **API Gateway → APIs**: stage `dev`, Cognito authorizer.
