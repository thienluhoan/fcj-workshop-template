---
title: "Workshop"
date: 2026-11-27
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai SEAL, nền tảng chấm thi hackathon, trên AWS

#### Tổng quan

**SEAL** là nền tảng web vận hành trọn một cuộc thi hackathon sinh viên: đăng ký đội, vòng thi và đồng hồ, nộp bài, chấm theo rubric, kết quả và giải thưởng. Giám khảo có thể nhờ **trợ lý AI** đọc repository GitHub của đội và đề xuất đánh giá ban đầu, sau đó giám khảo tự xem lại.

Workshop này dựng lại môi trường production của SEAL trên AWS từng bước, bằng các template **AWS CloudFormation** do nhóm viết. Kết thúc workshop, bạn có:

- frontend React trên **Amazon S3**, phân phối qua **Amazon CloudFront**, bảo vệ bằng **AWS WAF**, tên miền riêng trên **Amazon Route 53** và chứng chỉ **AWS Certificate Manager**;
- API Spring Boot trên **Amazon EC2** trong **Auto Scaling group**, sau **Application Load Balancer**, nằm trong private subnet của một **Amazon VPC** riêng;
- database **Amazon RDS for MySQL** trong subnet cô lập;
- luồng AI bất đồng bộ: hàng đợi **Amazon SQS** có dead-letter queue và **AWS Lambda** worker gọi DeepSeek API;
- email giao dịch qua **Amazon SES**;
- giám sát bằng alarm **Amazon CloudWatch**, **Amazon SNS**, **AWS Budgets** và ghi vết bằng **AWS CloudTrail**;
- CI/CD bằng **GitHub Actions**: đẩy image lên **Amazon ECR**, triển khai bằng **AWS Systems Manager**, đăng nhập AWS qua OIDC, không dùng access key dài hạn.

#### Nội dung

1. [Tổng quan](5.1-overview/)
2. [Chuẩn bị](5.2-prerequisites/)
3. [Kiến trúc và luồng dữ liệu](5.3-architecture/)
4. [Lớp mạng và dữ liệu](5.4-network-data/)
5. [Compute và origin](5.5-compute-origin/)
6. [Edge và bảo mật](5.6-edge-security/)
7. [AI worker](5.7-ai-worker/)
8. [Email với SES](5.8-email-ses/)
9. [CI/CD với GitHub Actions](5.9-cicd/)
10. [Kiểm thử và xác minh](5.10-testing-validation/)
11. [Chi phí và tối ưu](5.11-cost/)
12. [Xử lý sự cố và bài học](5.12-troubleshooting/)
13. [Dọn dẹp tài nguyên](5.13-cleanup/)
14. [Demo](5.14-demo/)
