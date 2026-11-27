---
title: "Worklog Tuần 8"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

- Hoàn thiện sơ đồ kiến trúc, kế hoạch cá nhân 4 giai đoạn và bộ script deploy/teardown.
- Áp dụng các quyết định tối ưu chi phí – bảo mật của nhóm vào Terraform.
- Rà soát toàn bộ nền hạ tầng, vá lỗi nghiêm trọng và đưa pipeline DevSecOps chạy chặn (blocking) trên mọi pull request.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Vẽ sơ đồ kiến trúc SecureTable v1.0.0 (draw.io)<br>- Viết kế hoạch hành động cá nhân tuần 1–8 (4 giai đoạn)<br>- Viết `deploy_and_test.ps1` và `nuke_and_teardown.ps1`<br>- Nhận đề xuất tối ưu kiến trúc từ nhóm trưởng | 02/11/2026 | 02/11/2026 | Sơ đồ kiến trúc<br>Kế hoạch cá nhân<br>Script<br>[draw.io](https://www.drawio.com/)<br>[AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)<br>[C4 model](https://c4model.com) |
| 3 | - Đánh giá đề xuất bỏ GuardDuty, Secrets Manager, KMS CMK<br>- Chọn mã hóa mặc định (SSE-S3, AWS-owned key)<br>- Thiết kế tách 3 bucket S3 và lifecycle sang Glacier<br>- Thiết kế SNS `securetable-alerts` và Budgets 3 ngưỡng | 03/11/2026 | 03/11/2026 | [Amazon GuardDuty – Pricing](https://aws.amazon.com/guardduty/pricing/)<br>[AWS KMS – Pricing](https://aws.amazon.com/kms/pricing/)<br>[AWS Secrets Manager – Pricing](https://aws.amazon.com/secrets-manager/pricing/)<br>[Amazon S3 – Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)<br>[Well-Architected – Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) |
| 4 | - Viết module `s3_cloudfront` (3 bucket, OAC, lifecycle)<br>- Viết module `sns` và cập nhật `budgets`<br>- `terraform validate` thành công<br>- Viết tài liệu tổng hợp dự án SecureTable | 04/11/2026 | 04/11/2026 | Tài liệu tổng hợp<br>Module<br>[Terraform – aws_cloudfront_origin_access_control](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudfront_origin_access_control)<br>[Terraform – aws_budgets_budget](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/budgets_budget)<br>[CloudFront – Restrict access to S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) |
| 5 | - Rà soát nền hạ tầng theo rubric tốt nghiệp (đạt khoảng 40–60%)<br>- Vá 6 lỗi nghiêm trọng: Cognito MFA bật, chỉ admin tạo tài khoản, nuke an toàn…<br>- Viết `securetable.tftest.hcl` (10 test), cấu hình `.checkov.yaml`<br>- Đọc đặc tả nghiệp vụ quản lý nguyên liệu của Backend | 05/11/2026 | 05/11/2026 | Báo cáo rà soát<br>[Terraform – Tests](https://developer.hashicorp.com/terraform/language/tests)<br>[Terraform – Mocking](https://developer.hashicorp.com/terraform/language/tests/mocking)<br>[Checkov](https://www.checkov.io)<br>[Cognito – Tạo tài khoản bởi admin](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-create-user-accounts.html)<br>[Cognito – MFA](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-mfa.html)<br>[DynamoDB – Transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html) |
| 6 | - Ghép repo nhóm, mở PR #1 (bản gốc) và PR #2 (bản vá 26 file)<br>- Sửa CI: ghim trivy-action theo SHA, gitleaks CLI quét toàn lịch sử, CI chạy cho mọi PR<br>- Sửa test S3 lệch giữa OpenTofu và Terraform → 10/10<br>- Xử lý sự cố: xoay vòng access key bị lộ, gỡ file khảo sát khỏi repo, xóa EC2 bị bỏ quên | 06/11/2026 | 06/11/2026 | Workflow CI<br>Nhật ký triển khai 06/11<br>[Gitleaks](https://github.com/gitleaks/gitleaks)<br>[Trivy Action](https://github.com/aquasecurity/trivy-action)<br>[GitHub – Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)<br>[GitHub – Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)<br>[AWS – Manage access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) |

### Kết quả đạt được tuần 8:

- Sơ đồ kiến trúc v1.0.0 (draw.io) và kế hoạch cá nhân 4 giai đoạn có ma trận bàn giao, tiêu chí kiểm định từng milestone.
- Script `deploy_and_test.ps1` (apply, xuất `deployed_env.json`, probe `/health`) và `nuke_and_teardown.ps1` (dọn S3 versions, destroy, hậu kiểm 0 tài nguyên).
- Bỏ GuardDuty, Secrets Manager, KMS CMK; tách 3 bucket S3 (frontend/assets/backup + Glacier sau 90 ngày); thêm SNS `securetable-alerts` và Budgets 3 ngưỡng.
- Bản vá review 05/11 (26 file, +1091/−307): Cognito MFA bật, chỉ admin tạo tài khoản, nuke an toàn, `terraform test` 10 test, Checkov có danh sách ngoại lệ ghi lý do, CI chặn khi có lỗi.
- CI 4/4 job xanh: gitleaks CLI quét toàn bộ lịch sử, Checkov, Trivy (ghim SHA), Bandit, Flake8, pytest 22/22, terraform fmt/validate/test 10/10, npm audit.
- Xử lý 6 sự cố trong ngày 06/11 (lộ access key qua ảnh chụp, file khảo sát bị đẩy lên repo, CI đỏ, test lệch giữa OpenTofu và Terraform, PR không chạy CI, file `tfplan` không bị ignore); xóa EC2 Windows bị bỏ quên ở us-east-1.
