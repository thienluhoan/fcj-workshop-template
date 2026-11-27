---
title: "Triển khai hạ tầng bằng Terraform"
date: 2026-11-23
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Cấu trúc mã nguồn
```text
infra/terraform/
├── main.tf              # gọi 7 module
├── variables.tf         # region, ngân sách, cờ enable_waf, nhóm Cognito...
├── outputs.tf           # API URL, User Pool ID, bucket, CloudFront...
├── modules/
│   ├── sns/             # topic securetable-alerts
│   ├── budgets/         # 10 USD, cảnh báo 50/80/100%
│   ├── dynamodb/        # 5 bảng on-demand + GSI + PITR
│   ├── cognito/         # User Pool, MFA ON, admin-only, 5 nhóm
│   ├── apigateway/      # REST API, authorizer, throttling, /health
│   ├── s3_cloudfront/   # 3 bucket + CloudFront OAC + security headers
│   └── waf_v2/          # Web ACL (bật bằng enable_waf)
└── tests/securetable.tftest.hcl   # 10 test, mock provider
```

### Bước 1 – Kiểm tra trước khi deploy (không tốn tiền)
```powershell
cd infra/terraform
terraform fmt -check -recursive
terraform init -backend=false
terraform validate
terraform test          # chạy với mock provider, không cần tài khoản AWS
```
Kết quả mong đợi: `Success! The configuration is valid.` và `10 passed, 0 failed`.

### Bước 2 – Deploy và kiểm tra tự động
```powershell
cd ../..
.\scripts\deploy_and_test.ps1             # WAF tắt (mặc định)
.\scripts\deploy_and_test.ps1 -OpenBrowser # mở thêm trình duyệt
```
Script thực hiện:
1. Kiểm tra Terraform, AWS CLI và credential (`aws sts get-caller-identity`).
2. `terraform plan -out=securetable.tfplan` rồi `terraform apply`.
3. Xuất thông số ra `backend-config/deployed_env.json` và `deployed_env.env` (biến `VITE_...` cho frontend, tên bảng cho backend).
4. Gọi AWS kiểm tra trạng thái thật: 5 bảng DynamoDB `ACTIVE`, User Pool có MFA `ON`, 5 nhóm Cognito, S3 Block Public Access, SNS subscription, API stage, CloudFront.
5. Gửi `GET /health`, mong đợi `{"status":"healthy"}`.

*(Ảnh minh hoạ: kết quả script màu xanh – nhóm bổ sung)*

### Bước 3 – Xác nhận email SNS
Mở hộp thư của `alert_emails` và bấm **Confirm subscription**. Nếu chưa xác nhận, alarm và Budgets sẽ không gửi được email.

### Xem kết quả trên Console
- **DynamoDB → Tables**: `securetable-orders-dev`, `securetable-menu-items-dev`, `securetable-inventory-dev`, `securetable-daily-finance-dev`, `securetable-audit-logs-dev`.
- **Cognito → User pools**: `securetable-users-dev`, tab Groups có ADMIN, INVENTORY, RECEPTION, WAITER, KITCHEN.
- **API Gateway → APIs**: stage `dev`, Authorizer Cognito.
