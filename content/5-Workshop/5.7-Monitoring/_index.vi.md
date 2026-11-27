---
title: "Giám sát, kiểm thử và chi phí"
date: 2026-11-23
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

### Log và metric
- **CloudWatch Logs**: mỗi Lambda có log group `/aws/lambda/securetable-*` giữ 7 ngày. Lỗi 403 từ decorator được ghi kèm user và nhóm để điều tra.
- **Metric**: API Gateway `Count`, `4XXError`, `5XXError`, `Latency`; Lambda `Duration`, `Errors`, `Throttles`; DynamoDB `ConsumedRead/WriteCapacityUnits`; WAF `BlockedRequests`.
- **Dashboard** `SecureTable-Live-Monitoring` gom các metric trên vào một màn hình.

### Cảnh báo
| Alarm | Điều kiện | Gửi tới |
| --- | --- | --- |
| API 5xx | `5XXError` > 5 trong 3 phút | SNS `securetable-alerts` → email |
| Request tăng đột biến | `Count` vượt ngưỡng trong 5 phút | SNS → email |
| AWS Budgets | Dự báo 50%, thực tế 80% và 100% của 10 USD | Email + SNS |

Thử alarm: gọi một route lỗi nhiều lần (hoặc đặt tạm ngưỡng thấp) và kiểm tra email.

### Kiểm thử tải
Dùng Locust giả lập 100 người dùng gọi `GET /menu` và `POST /orders`, tăng dần trong 2 phút. Ghi lại độ trễ p50/p95, tỉ lệ lỗi, số Lambda chạy đồng thời và chi phí phát sinh.

*(Bảng kết quả và ảnh dashboard – nhóm bổ sung sau khi chạy)*

### Chi phí
Với 1 nhà hàng (khoảng 200.000 request/tháng), chi phí ước tính khoảng **1,5 USD/tháng khi WAF tắt** và **khoảng 2,1 USD/tháng** trong tháng bật WAF 2 ngày để pentest và demo. Bảng chi tiết xem ở mục [Proposal – Ước tính ngân sách](../../2-proposal/).
