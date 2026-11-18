---
title: "Application Load Balancer"
weight: 2
chapter: false
pre: "<b> 5.6.2. </b> "
---

# 5.6.2. Application Load Balancer

## Mục tiêu và tình huống BTB

Định tuyến HTTPS API tới Spring Boot task khỏe. ALB là điểm vào ứng dụng công khai; port container ECS chỉ nhận từ security group của ALB.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Chốt `BACKEND_PORT`, `HEALTH_PATH` an toàn không nhạy cảm; chạy thử endpoint trong container.
2. Vào **EC2 → Target groups → Create target group**, chọn loại **IP addresses** cho Fargate/`awsvpc` (không phải instance).
3. Chọn VPC, target port/protocol và health-check path; điều chỉnh timeout theo thời gian Spring Boot khởi động.
4. Vào **EC2 → Load Balancers → Create Application Load Balancer**, chọn internet-facing theo thiết kế, hai public subnet/AZ và ALB security group.
5. Tạo listener/rule trỏ tới target group. Bật **HTTPS 443** với chứng chỉ ACM **cùng Region ALB** sau bước 5.7.1.
6. Ở cấu hình cuối, HTTP 80 chỉ redirect sang HTTPS; không gửi login/password qua HTTP tạm khi test.
7. Khi ECS đăng ký task IP, kiểm tra Target health và thử API HTTPS.



## Checklist xác nhận

- [ ] Target group loại `ip`; health check Healthy sau triển khai ECS.
- [ ] HTTPS listener dùng domain API và certificate đúng.

## Lưu ý và bảo mật

WAF gắn vào CloudFront không tự kiểm tra request đi thẳng vào ALB; muốn bảo vệ API phải gắn WAF Regional riêng.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** Target group type/health check, ALB listener, targets Healthy, response HTTPS.

Tên file ví dụ `static/images/5-workshop/5.6.2-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.6.2-01.png" alt="Application Load Balancer" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/register-multiple-targetgroups.html>
