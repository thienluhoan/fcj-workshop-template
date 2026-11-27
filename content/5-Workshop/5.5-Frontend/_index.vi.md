---
title: "Đưa frontend lên S3 + CloudFront"
date: 2026-11-23
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Bước 1 – Build với cấu hình vừa deploy
```powershell
Copy-Item backend-config/deployed_env.env frontend/.env    # chứa VITE_API_GATEWAY_URL, VITE_COGNITO_*
cd frontend
npm ci
npm run build                                               # tạo thư mục dist/
```

### Bước 2 – Đồng bộ lên S3 và làm mới cache
```powershell
$out = Get-Content ../backend-config/deployed_env.json | ConvertFrom-Json
aws s3 sync dist/ "s3://$($out.frontend_s3_bucket)" --delete
aws cloudfront create-invalidation --distribution-id $out.cloudfront_distribution_id --paths "/index.html"
```
Các file JS/CSS do Vite build đã có hash trong tên, nên chỉ cần invalidate `index.html`.

### Bước 3 – Kiểm tra
- Mở `https://<cloudfront_domain_name>`: giao diện tải qua HTTPS, response có header `Strict-Transport-Security`, `Content-Security-Policy`, `X-Frame-Options`.
- Mở thẳng URL S3 của bucket: phải trả **403 AccessDenied**, vì bucket chỉ cho CloudFront đọc qua Origin Access Control.

*(Ảnh: trang đăng nhập qua CloudFront và lỗi 403 khi truy cập thẳng S3 – nhóm bổ sung)*
