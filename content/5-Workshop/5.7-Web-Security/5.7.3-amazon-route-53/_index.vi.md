---
title: "Amazon Route 53"
weight: 3
chapter: false
pre: "<b> 5.7.3. </b> "
---

# 5.7.3. Amazon Route 53

## Mục tiêu và tình huống BTB

Phân giải domain frontend tới CloudFront và API tới ALB. Route 53 chỉ làm DNS, không proxy request; HTTPS termination tại CloudFront và ALB.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Xác nhận nhà đăng ký/nhà DNS đang quản lý domain có thẩm quyền trước khi thay DNS.
2. Vào **Route 53 → Hosted zones**, tạo/dùng **public hosted zone** cho domain được phép.
3. Chỉ đổi nameserver ở registrar khi thực sự chuyển authoritative DNS sang Route 53 và đã kiểm tra record cũ.
4. Tạo record **A/AAAA Alias** (nếu cần) cho web → CloudFront, API → ALB.
5. Chờ DNS propagation, truy vấn resolver công khai và test HTTPS cả hai subdomain.
6. Ghi phí hosted zone và giả định DNS query vào bảng Cost.

## Ví dụ CLI hoặc câu truy vấn

```powershell
nslookup <FRONTEND_DOMAIN>
nslookup <API_DOMAIN>
# Sau đó mở cả hai hostname qua https://
```

## Checklist xác nhận

- [ ] DNS frontend/API phân giải đúng AWS target.
- [ ] Hostname khớp certificate và HTTPS mở đúng ứng dụng.

## Lưu ý và bảo mật

Không tạo record public xung đột hay tự nhận domain đã sở hữu khi chưa xác nhận.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Hosted zone, Alias records, kết quả `nslookup`.

Tên file ví dụ `static/images/5-workshop/5.7.3-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.7.3-01.png" alt="Amazon Route 53" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html>
