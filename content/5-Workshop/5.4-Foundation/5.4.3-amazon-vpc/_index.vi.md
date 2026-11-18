---
title: "Amazon VPC"
weight: 3
chapter: false
pre: "<b> 5.4.3. </b> "
---

# 5.4.3. Amazon VPC

## Mục tiêu và tình huống BTB

Cô lập Fargate chạy Spring Boot khỏi inbound Internet, để public ALB kết nối ECS private. Bạn cùng nhóm lo NAT và VPC endpoints; dùng cấu hình outbound đã xác thực của bạn đó.

> **Trạng thái:** Đây là hướng dẫn dự kiến, chỉ đánh dấu hoàn thành khi có bằng chứng AWS Console thật.

## Thực hành từng bước

1. Chọn CIDR không trùng (ví dụ `10.20.0.0/16` **chỉ minh họa**) và Region, ghi rõ quyết định.
2. Vào **VPC → Create VPC**, bật hỗ trợ DNS/DNS hostname phù hợp hệ thống.
3. Tạo public subnet trong ít nhất hai AZ cho ALB và private subnet cho ECS theo kiến trúc đã chốt.
4. Attach Internet Gateway và cấu hình route `0.0.0.0/0` của public subnet tới Internet Gateway.
5. Phối hợp bạn cùng nhóm để route table **private** có đường outbound đã chọn; kiểm tra TLS tới Supabase và bên cung cấp AI/hình ảnh.
6. Tạo **ALB security group** cho cổng viewer phù hợp; **ECS security group** chỉ nhận `BACKEND_PORT` từ ALB security group.
7. Kiểm tra route, subnet association, IP khả dụng và nguồn vào SG trước khi tạo ECS service.



## Checklist xác nhận

- [ ] Public subnet của ALB có route IGW; private subnet ECS có outbound đã xác thực.
- [ ] Port backend không mở từ `0.0.0.0/0`.

## Lưu ý và bảo mật

ALB public không cấp outbound Internet cho ECS private; không tạo NAT/endpoint ở workshop cá nhân, nhưng phải xác minh phần bạn phụ trách.

## Ảnh chụp thực tế cần bổ sung

**Chụp sau khi triển khai:** VPC resource map, route tables, danh sách subnet, inbound rules của ALB/ECS SG.

Tên file ví dụ `static/images/5-workshop/5.4.3-01.png`. Không upload credential hay dữ liệu khách hàng. Chỉ chèn `{{</* workshop-image src="images/5-workshop/5.4.3-01.png" alt="Amazon VPC" caption="Ảnh thực tế sau triển khai" */>}}` **khi đã có file ảnh**.

**Tài liệu AWS:** <https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html>
