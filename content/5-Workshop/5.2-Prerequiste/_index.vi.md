---
title: "Các bước chuẩn bị"
date: 2026-11-23
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

### Tài khoản và quyền
- Một tài khoản AWS có **AWS Budgets** đã bật cảnh báo; root có MFA và không dùng cho việc hằng ngày.
- IAM user hoặc role dùng để deploy, có quyền tạo các dịch vụ: DynamoDB, Cognito, API Gateway, Lambda, IAM role, S3, CloudFront, WAFv2, SNS, Budgets, CloudWatch Logs.
- Region khuyến nghị: **ap-southeast-1 (Singapore)**.

### Công cụ cần cài
| Công cụ | Phiên bản | Kiểm tra |
| --- | --- | --- |
| Terraform | ≥ 1.9 | `terraform version` |
| AWS CLI | v2 | `aws --version` |
| PowerShell | 7+ (Windows/macOS/Linux) | `$PSVersionTable.PSVersion` |
| Python | 3.12 (chạy test backend) | `python --version` |
| Node.js | 20 (build frontend) | `node --version` |
| Git | bất kỳ | `git --version` |

### Cấu hình credential
```powershell
aws configure            # hoặc: aws login / aws configure sso
aws sts get-caller-identity
```
Lệnh thứ hai phải in ra đúng tài khoản và user/role mong muốn.

{{% notice warning %}}
Không chụp màn hình khi chạy `aws configure` và không commit access key lên Git. Nếu lỡ lộ key, vô hiệu hoá và tạo key mới ngay.
{{% /notice %}}

### Lấy mã nguồn
```powershell
git clone <repo-cua-nhom>
cd <repo-cua-nhom>
Copy-Item infra/terraform/terraform.tfvars.example infra/terraform/terraform.tfvars
```
Mở `terraform.tfvars`, sửa `alert_emails` thành email thật (script deploy sẽ chặn email mẫu). Giữ `enable_waf = false` khi phát triển.
