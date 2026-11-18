---
title: "Amazon ECS với AWS Fargate"
weight: 3
chapter: false
pre: "<b> 5.6.3. </b> "
---

# 5.6.3. Amazon ECS với AWS Fargate

## Mục tiêu và tình huống BTB

Chạy Spring Boot modular monolith không phải quản trị EC2. ECS quản lý số task và deployment health; transaction nghiệp vụ vẫn do Spring Boot/PostgreSQL đảm bảo.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Vào **ECS → Clusters → Create cluster**, đặt tên Cluster và chọn Fargate phù hợp.
2. Đăng ký **Task definition** tương thích Fargate; chọn Linux architecture khớp ECR image, CPU/memory hợp lệ và mạng `awsvpc`.
3. Chọn riêng task execution role và task role nếu cần; thêm image URI có tag/digest, `BACKEND_PORT`, env không nhạy cảm và secret ARN.
4. Cấu hình `awslogs` log driver trỏ Log Group đã tạo (hoặc cho phép auto-create).
5. Tạo ECS **Service**, chọn private subnets, ECS SG, không gán public IP cho thiết kế private, desired count theo ngân sách/HA và target group ALB.
6. Xác nhận outbound do bạn cùng nhóm cung cấp để pull image, gọi AWS API và kết nối Supabase/TLS.
7. Xem Tasks, Events, Deployments, log container và Target Health ALB; chỉ tuyên bố HA nếu có đủ bằng chứng multi-task/multi-AZ.
8. Chạy Flyway migration theo quy trình kiểm soát; không giả định nhiều task chạy migration đồng thời luôn an toàn.

## Ví dụ CLI hoặc câu truy vấn

```bash
# Xem service/task thực tế, không sử dụng ID mẫu.
aws ecs describe-services --cluster <CLUSTER_NAME> --services <SERVICE_NAME> --region <AWS_REGION> --query 'services[0].{status:status,running:runningCount,desired:desiredCount}'
```

## Checklist xác nhận

- [ ] Desired/running task đạt số lượng dự kiến; không có lỗi task kéo dài.
- [ ] ALB target Healthy, API phản hồi; xác minh DB bên ngoài.

## Lưu ý và bảo mật

Một Fargate task giúp tiết kiệm nhưng không HA ở mức task. Không mô tả modular monolith thành microservices.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Cluster, task definition, service deployment, running tasks, ALB health và log.

Tên file ví dụ `static/images/5-workshop/5.6.3-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.6.3-01.png" alt="Amazon ECS với AWS Fargate" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html>
