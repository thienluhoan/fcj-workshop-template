---
title: "Amazon CloudFront"
weight: 2
chapter: false
pre: "<b> 5.7.2. </b> "
---

# 5.7.2. Amazon CloudFront

## Mục tiêu và tình huống BTB

Phân phối React qua HTTPS, giữ S3 private nhờ OAC. API backend dùng **domain ALB riêng** trong lab này, trừ khi nhóm thực sự triển khai CloudFront API origin routing.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Vào **CloudFront → Create distribution**, chọn S3 REST origin, **không** dùng website endpoint public.
2. Tạo/chọn **Origin Access Control (OAC)** và áp bucket policy chỉ cho distribution đọc object S3.
3. Đặt **Redirect HTTP to HTTPS** và default root object `index.html`.
4. Với React SPA routes, cấu hình fallback lỗi phù hợp (`403`/`404` về `/index.html` khi cần), tránh che lỗi API.
5. Thêm alternate domain và ACM viewer certificate ở **us-east-1**.
6. Publish, test distribution; invalidation khi cập nhật asset hoặc dùng caching versioned.
7. Kiểm tra truy cập S3 public bị chặn và frontend không lộ secret.

## Ví dụ CLI hoặc câu truy vấn

```bash
# Invalidation sau khi upload bản frontend mới nếu cần:
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths '/*'
```

## Checklist xác nhận

- [ ] Web HTTPS chạy qua distribution và domain riêng.
- [ ] Refresh deep link hoạt động; S3 public read bị chặn.

## Lưu ý và bảo mật

SPA fallback dành cho frontend, không biến lỗi API 404 thành HTML.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Distribution origins, OAC policy, cache behavior, certificate và trang React chạy thật.

Tên file ví dụ `static/images/5-workshop/5.7.2-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.7.2-01.png" alt="Amazon CloudFront" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html>
