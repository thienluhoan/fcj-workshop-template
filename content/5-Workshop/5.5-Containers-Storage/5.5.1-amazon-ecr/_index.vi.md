---
title: "Amazon ECR"
weight: 1
chapter: false
pre: "<b> 5.5.1. </b> "
---

# 5.5.1. Amazon ECR

## Mục tiêu và tình huống BTB

Lưu Docker image cho Spring Boot modular monolith. ECR là kho chứa image, không phải nơi thực thi; ECS/Fargate chạy image.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Kiểm tra Dockerfile build được từ đúng thư mục backend, dùng Java runtime hợp với dự án.
2. Vào **Amazon ECR → Private repositories → Create repository**, đặt `btb-backend` hoặc tên được nhóm chốt; bật scanning/encryption phù hợp.
3. Xem **View push commands** của Region/account; đăng nhập Docker vào ECR qua AWS CLI token.
4. Build đúng platform, tag bằng version/commit SHA (không chỉ dựa vào `latest`), push lên ECR.
5. Ghi đúng image URI/digest vào ECS task definition; xem kết quả scan và xử lý lỗ hổng đáng kể.

## Ví dụ CLI hoặc câu truy vấn

```bash
# Thay toàn bộ placeholder; chạy bằng shell đã cấu hình Docker + AWS CLI.
aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com
docker build -t btb-backend:<IMAGE_TAG> <BACKEND_SOURCE_DIRECTORY>
docker tag btb-backend:<IMAGE_TAG> <ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/btb-backend:<IMAGE_TAG>
docker push <ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/btb-backend:<IMAGE_TAG>
```

## Checklist xác nhận

- [ ] Image có tag hiển thị trong private repository.
- [ ] Kiến trúc image tương thích Fargate task.

## Lưu ý và bảo mật

Không commit Docker credential hay registry token vào Git.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** ECR repository, image tags và output push image (che thông tin nhạy cảm nếu cần).

Tên file ví dụ `static/images/5-workshop/5.5.1-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.5.1-01.png" alt="Amazon ECR" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html>
