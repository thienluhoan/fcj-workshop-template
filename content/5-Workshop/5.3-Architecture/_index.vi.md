---
title: "Kiến trúc và luồng dữ liệu"
weight: 3
chapter: false
pre: "<b> 5.3. </b> "
---

# 5.3. Kiến trúc và luồng dữ liệu

## Sơ đồ draw.io — tự hoàn thiện

Ông cần **tự vẽ và kiểm tra bản kiến trúc cuối** bằng thư viện AWS Architecture Icons trong draw.io. Thể hiện ranh giới AWS Cloud, Region, VPC, public/private subnet ở hai Availability Zone (nếu đã triển khai thật), dịch vụ ngoài AWS, mũi tên request và chú thích phạm vi phụ trách. Lưu file `.drawio` tự chỉnh sửa và xuất PNG rõ nét. Không nộp nguyên sơ đồ do AI sinh 100% như sản phẩm tự thiết kế.

**Vị trí ảnh sau khi tự hoàn thiện:** `static/images/5-workshop/architecture/btb-architecture-final.png` (thêm ảnh thật và shortcode `workshop-image` được cung cấp).

## Luồng dữ liệu mục tiêu

```text
Frontend: Browser → Route 53 DNS → CloudFront (+ WAF nếu có)
                → S3 private (OAC) → tài nguyên React/TypeScript
API:      Browser → Route 53 DNS → HTTPS ALB (+ WAF vùng nếu có)
                → ECS Fargate trong VPC → các module Spring Boot
Dữ liệu:  Spring Boot → Supabase PostgreSQL (dịch vụ ngoài; TLS)
Ảnh:      Browser/Backend → Cloudinary (theo thiết kế ứng dụng)
AI:       Spring Boot → Gemini/Spring AI (nếu đã bật)
Bản đồ:   Browser/Backend → Vietmap (nếu đã bật)
Vận hành: ECS lấy image từ ECR; secret từ Secrets Manager; log → CloudWatch Logs
```

**Lưu ý mạng:** ALB public **không** tạo outbound internet cho ECS private. Cần lấy cấu hình NAT Gateway/VPC endpoints thực tế từ bạn cùng nhóm và kiểm tra kết nối tới Supabase/Gemini/Cloudinary. VPC endpoints không thay thế internet outbound tới nhà cung cấp ngoài AWS.

**Lưu ý HTTPS:** Chứng chỉ ACM của CloudFront viewer phải ở `us-east-1`; chứng chỉ của ALB cùng Region với ALB. Nếu frontend/API dùng subdomain riêng thì phải vẽ rõ hai đường HTTPS, không tự giả định tất cả API đi qua CloudFront.

## Phân chia phạm vi

| Người phụ trách | Dịch vụ |
|---|---|
| **Workshop của ông** | VPC, IAM, Budgets, ECR, ECS/Fargate, ALB, S3, CloudFront, Route 53, ACM, Secrets Manager, WAF, CloudWatch Logs |
| **Bạn cùng nhóm — chỉ đưa vào sơ đồ** | Bedrock, SQS, SES, CodePipeline/CodeBuild, CloudWatch Metrics/Alarms, CloudTrail, NAT Gateway/VPC endpoints |
| **Tích hợp ngoài AWS** | Supabase PostgreSQL, Cloudinary, Gemini, Vietmap |

Spring Boot/database giữ phân quyền, giá bậc số lượng, duyệt Quote, giữ đủ tồn kho, trạng thái Order; AWS cung cấp hạ tầng lưu trữ/triển khai/mạng/giám sát.

## Bảo mật và độ tin cậy

- Không mở container port ECS trực tiếp ra internet; inbound chỉ từ ALB security group.
- IAM role theo Least Privilege; token/password không nằm trong Git và được cấp cho runtime an toàn.
- Bật S3 Block Public Access và CloudFront OAC.
- ALB health check dùng endpoint an toàn được mở quyền có chủ đích.
- Nếu cần HA hai AZ, kiểm tra subnet, health check và số task đủ, cân nhắc chi phí.
- WAF trên CloudFront **không tự bảo vệ** ALB API truy cập riêng.

**Checklist sơ đồ:** đúng icon AWS, Region/AZ/VPC, luồng ingress/egress, giao thức/port, nơi lưu dữ liệu, phân biệt AWS và dịch vụ ngoài, tài nguyên tính phí, owner và ảnh chụp thực tế.
