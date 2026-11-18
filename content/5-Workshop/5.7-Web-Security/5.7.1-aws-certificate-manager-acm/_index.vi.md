---
title: "AWS Certificate Manager (ACM)"
weight: 1
chapter: false
pre: "<b> 5.7.1. </b> "
---

# 5.7.1. AWS Certificate Manager (ACM)

## Mục tiêu và tình huống BTB

Cấp và xác minh chứng chỉ TLS công khai cho web/API. CloudFront viewer cần ACM ở `us-east-1`; ALB cần certificate cùng Region ALB.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Xác định hostname web/API, ví dụ `www.example.com`, `api.example.com`; kiểm tra quyền sở hữu zone.
2. Mở **ACM** tại **us-east-1**, yêu cầu public certificate cho CloudFront viewer, dùng DNS validation.
3. Tạo DNS validation record đúng tại DNS đang có thẩm quyền, chờ **Issued**.
4. Chuyển tới **Region của ALB**, request/validate certificate cho API nếu ALB khác Region.
5. Gắn certificate web vào CloudFront và certificate API vào HTTPS listener ALB; kiểm tra hostname/hạn chứng chỉ.



## Checklist xác nhận

- [ ] Certificate cần dùng đều Issued tại Region phù hợp.
- [ ] HTTPS hostname dùng certificate đúng.

## Lưu ý và bảo mật

Certificate sai Region sẽ không dùng được cho CloudFront/ALB tương ứng.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** ACM details và DNS validation record (che thông tin cần thiết).

Tên file ví dụ `static/images/5-workshop/5.7.1-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.7.1-01.png" alt="AWS Certificate Manager (ACM)" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cnames-and-https-requirements.html>
