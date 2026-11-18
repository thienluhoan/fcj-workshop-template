---
title: "Chuẩn bị"
weight: 2
chapter: false
pre: "<b> 5.2. </b> "
---

# 5.2. Điều kiện tiên quyết

## Tài khoản và quyền

Chuẩn bị tài khoản/role AWS có quyền đúng mức cần thiết cho VPC, IAM, Budgets, ECR, ECS/Fargate, ELB, S3, CloudFront, ACM, Route 53, Secrets Manager, WAF và CloudWatch Logs. Không dùng root user để thao tác thường ngày. Xác nhận quyền sở hữu domain và quyền truy cập Supabase/nhà cung cấp hình ảnh bên ngoài.

## Công cụ

- Git, Docker (Linux containers), AWS CLI v2, Node.js/npm, Java 21+ và công cụ build backend đang dùng thực tế.
- Source React/TypeScript và Spring Boot ở nhánh đã được nhóm thống nhất.
- Region AWS, port backend, health endpoint, Account ID, domain/subdomain, DB host/port, kiến trúc container.
- AWS Pricing Calculator và công cụ chụp screenshot; che mọi token, password và khóa JWT trong ảnh.

## Kiểm tra máy (PowerShell)

```powershell
git --version
docker --version
aws --version
node --version
java -version
aws sts get-caller-identity
aws configure list
```

Build frontend theo `package.json` thật (thường `npm ci`, `npm run build`), không đoán đường dẫn. Backend dùng lệnh Maven/Gradle chính xác của repository; xem `Dockerfile` trước khi build.

## Phiếu cấu hình cần điền

| Biến | Ý nghĩa |
|---|---|
| `AWS_REGION` | Region của ECS, VPC, ALB, ECR và log |
| `ACCOUNT_ID` | Số tài khoản AWS, không phải secret |
| `BACKEND_PORT` | Port Spring Boot lắng nghe trong container |
| `HEALTH_PATH` | API health cho ALB, ví dụ readiness endpoint được cho phép có chủ ý |
| `FRONTEND_BUCKET` | Bucket S3 private, tên duy nhất |
| `FRONTEND_DOMAIN` | Domain web React |
| `API_DOMAIN` | Subdomain API của ALB |
| `ECR_REPOSITORY` | Repository Docker image backend |

**Phụ thuộc:** ECS private phải có đường outbound hợp lệ tới Supabase và nếu sử dụng thì Gemini/Cloudinary. NAT Gateway/VPC endpoints do bạn cùng nhóm triển khai, cần lấy thông tin thực tế trước khi chạy ECS.

**Ảnh cần chụp:** phiên bản tool (không lộ secrets), AWS identity, Region, bảng thông số đã dùng.
