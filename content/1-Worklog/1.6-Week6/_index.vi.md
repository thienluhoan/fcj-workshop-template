---
title: "Worklog Tuần 6"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

- Học Terraform từ cơ bản tới module: init/plan/apply/destroy, biến, output, state, module.
- Đóng gói ứng dụng bằng Docker, hiểu chiến lược backup/restore và tự động hóa tiết kiệm chi phí bằng Lambda.
- Dựng bản hạ tầng đầu tiên cho đề tài quản lý nguyên liệu (Smart Kitchen) bằng Terraform.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tutorial Terraform Get Started (AWS)<br>- Đọc chuỗi bài Terraform Series (Viblo) phần IaC cơ bản<br>- Thực hành: cài Terraform, tạo/sửa/xóa EC2 + S3 bằng code | 19/10/2026 | 19/10/2026 | [HashiCorp – Terraform Get Started (AWS)](https://developer.hashicorp.com/terraform/tutorials/aws-get-started)<br>[Viblo – Terraform Series bài 1](https://viblo.asia/p/terraform-series-bai-1-infrastructure-as-code-va-terraform-maGK7Bqa5j2)<br>[Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |
| 3 | - Đọc Terraform Language Documentation:<br>&emsp;+ module, locals, data source<br>&emsp;+ count / for_each, depends_on, lifecycle<br>- Đọc code module mẫu terraform-aws-modules | 20/10/2026 | 20/10/2026 | [Terraform Language Documentation](https://developer.hashicorp.com/terraform/language)<br>[Terraform – Modules](https://developer.hashicorp.com/terraform/language/modules)<br>[terraform-aws-modules (GitHub)](https://github.com/terraform-aws-modules) |
| 4 | - Workshop Deploy Application on Docker:<br>&emsp;+ Chạy ứng dụng local, dựng VPC/SG/IAM role cho ECR<br>&emsp;+ RDS cho ứng dụng, cấu hình EC2<br>&emsp;+ Deploy bằng Docker image và Docker Compose<br>&emsp;+ Push image lên ECR / Docker Hub | 21/10/2026 | 21/10/2026 | [000015 – Docker](https://000015.awsfcaj.com/)<br>[Docker Get started](https://docs.docker.com/get-started/) |
| 5 | - Workshop Deploy AWS Backup to the System:<br>&emsp;+ Tạo backup plan<br>&emsp;+ Thông báo khi job backup chạy/lỗi<br>&emsp;+ Thử restore | 22/10/2026 | 22/10/2026 | [000013 – 1. Introduction](https://000013.awsfcaj.com/1-introduce/)<br>[000013 – 3. Create Backup plan](https://000013.awsfcaj.com/3-createbackupplan/)<br>[000013 – 4. Set up notifications](https://000013.awsfcaj.com/4-enablenoti/)<br>[000013 – 5. Test Restore](https://000013.awsfcaj.com/5-testrestore/) |
| 6 | - Workshop Optimizing EC2 Costs with Lambda:<br>&emsp;+ Gắn tag cho instance<br>&emsp;+ IAM role cho Lambda<br>&emsp;+ Lambda stop/start instance, thông báo Slack<br>&emsp;+ Kiểm thử và dọn dẹp | 23/10/2026 | 23/10/2026 | [000022 – Lambda tối ưu chi phí EC2](https://000022.awsfcaj.com/) |
| CN | - Viết module Terraform: vpc, security_groups, ec2, rds, secrets, budgets<br>- Chuẩn bị cấu hình backend Spring Boot: Dockerfile, schema SQL, đọc secret<br>- Viết script cost-saver (tắt EC2/RDS ngoài giờ) và script diễn tập DR cho RDS | 25/10/2026 | 25/10/2026 | Code Terraform 3-tier (lưu trữ)<br>Cấu hình backend<br>Script vận hành<br>[Terraform AWS Provider – aws_db_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance)<br>[000003 – 7. IaC template VPC (tham khảo cấu trúc mạng)](https://000003.awsfcaj.com/7-infrastructureascode/) |

### Kết quả đạt được tuần 6:

- Hoàn thành tutorial Terraform Get Started (AWS) và chuỗi bài Terraform tiếng Việt; viết được module có input/output.
- Đóng gói ứng dụng bằng Docker, chạy với Docker Compose và đẩy image lên Amazon ECR.
- Tạo backup plan bằng AWS Backup, thử restore; hiểu RPO/RTO.
- Viết Lambda dừng/bật EC2 theo tag và lịch.
- Dựng bộ Terraform 3-tier: VPC (public, private app, private DB, NAT, Flow Logs), Security Group theo tầng, EC2 có instance profile, RDS PostgreSQL, Secrets Manager, Budgets.
- Chuẩn bị cấu hình backend Spring Boot (Dockerfile, schema SQL, đọc secret) và 2 script vận hành: cost-saver và diễn tập DR cho RDS.
