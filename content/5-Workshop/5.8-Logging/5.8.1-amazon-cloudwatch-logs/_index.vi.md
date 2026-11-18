---
title: "Amazon CloudWatch Logs"
weight: 1
chapter: false
pre: "<b> 5.8.1. </b> "
---

# 5.8.1. Amazon CloudWatch Logs

## Mục tiêu và tình huống BTB

Tập trung log Spring Boot/Fargate để phân tích startup và lỗi, không log thông tin nhạy cảm của khách hay credential. CloudWatch Metrics/Alarms do bạn cùng nhóm làm, không triển khai trong bài này.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Mở **CloudWatch → Logs → Log groups** cùng Region ECS và tạo group ví dụ `/ecs/btb-backend`.
2. Chọn retention theo chính sách dữ liệu/xử lý sự cố, không để giữ vĩnh viễn mà không xem xét.
3. Đảm bảo ECS task definition dùng `awslogs` đúng region/group/stream prefix, execution role có quyền ghi log.
4. Deploy backend, xem stream về Spring Boot readiness, lỗi health/DB, lỗi có correlation ID và sự kiện restart.
5. Nếu có cấu trúc log phù hợp, dùng CloudWatch Logs Insights tìm lỗi gần đây; chụp query và thời gian chạy.
6. Kiểm tra log không in password, auth header, JWT, PII hay chuỗi kết nối DB chứa secret.

## Ví dụ CLI hoặc câu truy vấn

```sql
fields @timestamp, @message
| filter @message like /ERROR|Exception/
| sort @timestamp desc
| limit 20
```

## Checklist xác nhận

- [ ] Mỗi ECS task chạy có stream log phù hợp.
- [ ] Query trả log startup/error thực tế và không làm lộ secret.

## Lưu ý và bảo mật

Logs khác Metrics/Alarms và phần đó thuộc bạn cùng nhóm.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Log group, retention, log stream, query Logs Insights an toàn.

Tên file ví dụ `static/images/5-workshop/5.8.1-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.8.1-01.png" alt="Amazon CloudWatch Logs" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html>
