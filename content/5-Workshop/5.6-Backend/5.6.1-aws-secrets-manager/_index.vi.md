---
title: "AWS Secrets Manager"
weight: 1
chapter: false
pre: "<b> 5.6.1. </b> "
---

# 5.6.1. AWS Secrets Manager

## Mục tiêu và tình huống BTB

Bảo vệ thông tin kết nối Supabase, khóa JWT và token của dịch vụ ngoài đang dùng. ECS inject tham chiếu secret vào Spring Boot, không lưu secret plaintext trong Git.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Kiểm kê giá trị nhạy cảm (DB/password, JWT key, token Gemini/Cloudinary chỉ nếu đang dùng).
2. Vào **Secrets Manager → Store a new secret**, chọn kiểu secret key-value hoặc JSON và khóa mã hóa phù hợp.
3. Đặt tên không lộ dữ liệu, ví dụ `btb/demo/backend-db`, lưu cùng Region với ECS.
4. Giới hạn ECS execution role tới ARN secret chính xác; nếu dùng KMS key tự quản thì cấp decrypt tối thiểu.
5. Trong ECS task definition dùng **Secrets → ValueFrom** theo biến môi trường, không ghi giá trị vào log.
6. Triển khai task revision, kiểm tra startup và quyền đọc; khi đổi secret cần có kế hoạch rotate/redeploy.



## Checklist xác nhận

- [ ] Metadata secret hiển thị mà không lộ giá trị.
- [ ] ECS task start bằng quyền role đọc secret.

## Lưu ý và bảo mật

Không công bố secret thật hay ảnh chụp giá trị chưa che.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Tên/ARN secret, ánh xạ task đã che nội dung, IAM policy.

Tên file ví dụ `static/images/5-workshop/5.6.1-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.6.1-01.png" alt="AWS Secrets Manager" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/secrets-envvar-secrets-manager.html>
