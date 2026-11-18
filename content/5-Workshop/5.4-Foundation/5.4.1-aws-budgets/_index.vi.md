---
title: "AWS Budgets"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b> "
---

# 5.4.1. AWS Budgets

## Mục tiêu và tình huống BTB

Theo dõi chi phí trước khi chạy Fargate, ALB, WAF và truyền dữ liệu. Budget gửi cảnh báo chứ không tự tắt tài nguyên.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Vào **Billing and Cost Management → Budgets → Create budget** bằng tài khoản/role có quyền xem billing.
2. Chọn **Cost budget**, chu kỳ hàng tháng và nhập ngân sách do nhóm duyệt (không tự bịa mức USD).
3. Thêm ngưỡng cảnh báo theo % chi phí thực tế do nhóm chọn, email người nhận và kiểm tra nhận thông báo.
4. Gắn tag `Project=BTB` cho tài nguyên; kích hoạt cost allocation tags tại Billing nếu cần.
5. Ghi lại tên Budget, tiền tệ, chu kỳ, ngưỡng và bằng chứng hoạt động; che email cá nhân trong ảnh.



## Checklist xác nhận

- [ ] Budget hiện trong Console và có trạng thái phù hợp.
- [ ] So sánh actual/forecast với giả định ở Pricing Calculator.

## Lưu ý và bảo mật

AWS Budgets không bảo đảm giới hạn chi phí cứng; cần kiểm tra chi phí thường xuyên.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Trang chi tiết Budget, ngưỡng/alert, Cost Explorer hoặc màn hình chi phí.

Tên file ví dụ `static/images/5-workshop/5.4.1-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.4.1-01.png" alt="AWS Budgets" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html>
