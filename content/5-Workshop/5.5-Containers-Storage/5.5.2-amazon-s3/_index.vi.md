---
title: "Amazon S3"
weight: 2
chapter: false
pre: "<b> 5.5.2. </b> "
---

# 5.5.2. Amazon S3

## Mục tiêu và tình huống BTB

Lưu **frontend build** React/TypeScript; ảnh sản phẩm/nội dung tiếp tục dùng Cloudinary. Bucket web để private, CloudFront đọc qua Origin Access Control.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Trong frontend, chạy script cài/build thật (Vite thường là `npm ci`, `npm run build`), kiểm tra thư mục `dist/`.
2. Vào **Amazon S3 → Create bucket**, chọn tên bucket duy nhất và Region đúng kế hoạch.
3. Bật **Block all public access**; không cho ACL/bucket policy public-read.
4. Upload **nội dung bản build**, không upload cả thư mục source, qua Console hoặc `aws s3 sync`.
5. Sau khi tạo CloudFront OAC ở 5.7.2, áp dụng bucket policy CloudFront sinh có Distribution ARN chính xác.
6. Kiểm tra website tải qua CloudFront, còn truy cập trực tiếp S3 object ẩn danh bị từ chối.

## Ví dụ CLI hoặc câu truy vấn

```bash
# Với Vite, sau khi xác nhận thư mục output thực tế là dist/:
npm ci
npm run build
aws s3 sync dist/ s3://<FRONTEND_BUCKET>/ --delete
```

## Checklist xác nhận

- [ ] S3 bucket private; có entry file và asset đã hash.
- [ ] CloudFront đọc được qua OAC.

## Lưu ý và bảo mật

Không dùng S3 static-website public endpoint làm OAC origin của CloudFront.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** S3 Block Public Access, object build, bucket policy OAC và request public bị chặn.

Tên file ví dụ `static/images/5-workshop/5.5.2-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.5.2-01.png" alt="Amazon S3" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html>
