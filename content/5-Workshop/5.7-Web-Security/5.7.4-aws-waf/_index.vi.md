---
title: "AWS WAF"
weight: 4
chapter: false
pre: "<b> 5.7.4. </b> "
---

# 5.7.4. AWS WAF

## Mục tiêu và tình huống BTB

Lọc request bất thường đến BTB bằng Web ACL rule. CloudFront và API trực tiếp qua ALB là **hai phạm vi gắn WAF khác nhau**.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Xác nhận WAF đã triển khai thật hay mới P1; nếu chưa triển khai phải ghi đề xuất, không bịa screenshot.
2. Vào **AWS WAF → Web ACLs → Create web ACL**, chọn scope **CloudFront (Global)** cho frontend (quản lý tại us-east-1).
3. Chọn managed rule groups/rate-based rule phù hợp tải; có thể chạy Count mode trước khi Block để tránh false positive.
4. Associate Web ACL với CloudFront distribution và kiểm tra trạng thái.
5. Nếu API truy cập ALB trực tiếp và cần bảo vệ API, tạo/dùng **Regional Web ACL** cùng Region ALB rồi attach **riêng**.
6. Kiểm thử an toàn điều kiện rule có thể mô phỏng, xem sampled requests và tránh chặn nhầm Customer.
7. Ghi rule, kết quả count/block và thành phần chi phí WAF.



## Checklist xác nhận

- [ ] Web ACL attach đúng resource mục tiêu.
- [ ] Sampled requests/count phản ánh rule action dự kiến.

## Lưu ý và bảo mật

WAF CloudFront không bảo vệ request ALB trực tiếp. WAF có thể tính phí Web ACL/rule/request.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Web ACL scope, rule, association, sampled request Count/Block.

Tên file ví dụ `static/images/5-workshop/5.7.4-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.7.4-01.png" alt="AWS WAF" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html>
