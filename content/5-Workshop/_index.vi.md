---
title: "Workshop"
date: 2026-11-23
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai SecureTable – Hệ thống quản lý nhà hàng Serverless an toàn trên AWS

### Tổng quan

Workshop này hướng dẫn triển khai lại toàn bộ hệ thống **SecureTable** bằng Terraform và script tự động, từ hạ tầng tới kiểm thử bảo mật và dọn dẹp tài nguyên. Người đọc làm theo từ đầu đến cuối sẽ có:

- Một ứng dụng web chạy qua **Amazon CloudFront** (HTTPS), dữ liệu lưu trong **Amazon DynamoDB** và **Amazon S3**.
- Đăng nhập bằng **Amazon Cognito** (MFA bắt buộc, chỉ admin tạo tài khoản), API qua **Amazon API Gateway** với Cognito authorizer, xử lý bằng **AWS Lambda**.
- Lớp bảo vệ **AWS WAF** bật/tắt bằng một cờ, giám sát bằng **Amazon CloudWatch**, cảnh báo qua **Amazon SNS**, kiểm soát chi phí bằng **AWS Budgets**.
- Pipeline **DevSecOps** trên GitHub Actions và một lệnh dọn sạch tài nguyên để chi phí về 0 USD.

### Nội dung

1. [Giới thiệu](5.1-workshop-overview/)
2. [Các bước chuẩn bị](5.2-prerequiste/)
3. [Triển khai hạ tầng bằng Terraform](5.3-deploy-infrastructure/)
4. [Tài khoản Cognito và kiểm thử API](5.4-cognito-api/)
5. [Đưa frontend lên S3 + CloudFront](5.5-frontend/)
6. [Bảo mật: WAF và pipeline DevSecOps](5.6-security/)
7. [Giám sát, kiểm thử và chi phí](5.7-monitoring/)
8. [Dọn dẹp tài nguyên](5.8-cleanup/)

### Minh chứng sản phẩm chạy thực tế

- **Video demo:** *(nhóm bổ sung link video)*
- **Link sản phẩm (CloudFront):** *(nhóm bổ sung khi demo)*
