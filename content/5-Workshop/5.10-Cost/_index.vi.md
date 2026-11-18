---
title: "Dự toán và tối ưu chi phí"
weight: 10
chapter: false
pre: "<b> 5.10. </b> "
---

# 5.10. Dự toán và tối ưu chi phí

**Minh chứng bắt buộc:** bảng estimate có ngày từ **[AWS Pricing Calculator](https://calculator.aws/)**, dùng Region và tải giống môi trường đang trình bày. **Không tự bịa giá hàng tháng.** Thêm link/export hoặc ảnh kèm giả định.

## Điền giả định tải thực tế

| Dịch vụ | Giả định cần có | Giá trị của nhóm |
|---|---|---|
| ECS/Fargate | vCPU, RAM GiB, giờ chạy, số task | TODO |
| Application Load Balancer | Giờ chạy ALB và LCU | TODO |
| Amazon ECR | Dung lượng image, số version, storage | TODO |
| Amazon S3 | GB lưu, requests, outbound | TODO |
| Amazon CloudFront | GB truyền ra theo khu vực, HTTPS requests | TODO |
| Route 53 | Số hosted zone và DNS query | TODO |
| Secrets Manager | Số secret, lượt gọi API | TODO |
| CloudWatch Logs | GB ingest và thời gian lưu | TODO |
| AWS WAF | Web ACL, rule/rule group, requests | TODO |
| Egress mạng (dùng chung) | NAT Gateway/VPC endpoints nếu bạn cùng nhóm đã deploy | TODO (phối hợp) |

Bổ sung chi phí public IPv4, lưu lượng qua AZ, data transfer và khoản phí thực tế khác. Tài nguyên dùng chung vẫn thuộc **tổng chi phí dự án**, dù không hướng dẫn triển khai trong workshop cá nhân này.

## Các cách tối ưu

1. Gắn tag và so sánh chi phí thực tế/dự báo với AWS Budgets.
2. Không để ECS/ALB luôn chạy sau khi demo nếu không cần.
3. Điều chỉnh CPU/RAM theo đo đạc; multi-task/multi-AZ chỉ khi có nhu cầu và tài liệu chứng minh.
4. Cài log retention, ECR lifecycle policy và rà soát S3 object/version.
5. So sánh trade-off cost/availability của egress với bạn phụ trách; không bỏ đường mạng cần cho Supabase.

**Bằng chứng nộp:** ảnh/export estimate, giả định calculator, tổng mới và giải thích trade-off. Chưa có input/Region thì chưa thể tính tổng đáng tin cậy.
