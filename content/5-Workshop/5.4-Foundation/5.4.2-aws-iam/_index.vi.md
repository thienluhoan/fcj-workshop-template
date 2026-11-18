---
title: "AWS IAM"
weight: 2
chapter: false
pre: "<b> 5.4.2. </b> "
---

# 5.4.2. AWS IAM

## Mục tiêu và tình huống BTB

Tách quyền triển khai của con người khỏi **task execution role** (pull ECR/logs/inject secret) và **task role** của ứng dụng (chỉ cấp API AWS khi cần). Phân quyền người dùng website do Spring Security đảm nhiệm.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Vào **IAM → Roles**, phân định quyền triển khai và vai trò ECS; không dùng root user hoặc access key dài hạn.
2. Tạo **task execution role** có trusted principal `ecs-tasks.amazonaws.com`; cấp quyền ECS execution cần thiết, giới hạn ARN tài nguyên.
3. Cấp thêm quyền Secrets Manager và KMS decrypt cho execution role **chỉ** nếu dùng secret/key để ECS inject vào container.
4. Tạo **task role** riêng nếu Spring Boot cần gọi AWS API; không cấp AdminAccess hay dùng chung role execution.
5. Đảm bảo quyền `iam:PassRole` của người deploy chỉ áp dụng cho role được phê duyệt.
6. Kiểm tra policy/trust và kết quả khởi động task; lưu ARN/policy, tuyệt đối không công khai secret.



## Checklist xác nhận

- [ ] Trusted entity của role ECS chính xác.
- [ ] Không có quyền quá rộng hoặc khóa truy cập hard-code.

## Lưu ý và bảo mật

Không nhầm AWS IAM với quyền Guest/Customer/E-commerce/Logistics/Delivery/Admin của ứng dụng.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Trust policy của execution role, policy task role, ghi chú Least Privilege.

Tên file ví dụ `static/images/5-workshop/5.4.2-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.4.2-01.png" alt="AWS IAM" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html>
