---
title: "Tổng quan"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Bài toán

Một cuộc thi hackathon có vòng đời ngắn và dồn dập: vài tháng yên ắng, rồi một tháng thi với hàng trăm sinh viên đăng ký, nộp bài và được chấm trong vài ngày. Giám khảo phải đọc hàng chục repository trong thời gian ngắn. Vì vậy SEAL cần:

- luôn hoạt động trong mùa thi, kể cả khi một máy chủ hay một Availability Zone gặp sự cố;
- giữ kín dữ liệu sinh viên và secret;
- chi phí thấp giữa các mùa, chỉ tăng tài nguyên khi thi;
- không để model AI chậm làm treo màn hình chấm của giám khảo.

#### Bạn sẽ xây dựng

| Lớp | Dịch vụ AWS | Vai trò |
|---|---|---|
| Edge | Route 53, CloudFront, WAF, ACM (us-east-1) | Một điểm vào HTTPS, file tĩnh từ S3, chuyển lời gọi API về origin |
| Mạng | VPC, subnet ở 2 AZ, Internet Gateway, NAT Gateway, S3 Gateway Endpoint | Tầng public, private app và private DB |
| Compute | ALB, ACM (ap-southeast-1), EC2 Auto Scaling, Systems Manager | API Spring Boot trên 1–2 instance, triển khai không cần SSH |
| Dữ liệu | RDS for MySQL, S3, Parameter Store | Dữ liệu quan hệ, file upload và snapshot AI, cấu hình mã hóa |
| AI | SQS (jobs, results, hai DLQ), Lambda | Phân tích repository bất đồng bộ |
| Vận hành | CloudWatch, SNS, Budgets, CloudTrail, ECR, GitHub Actions | Cảnh báo, theo dõi chi phí, audit, CI/CD |

#### Cấu trúc workshop

Mỗi bài lab triển khai một stack CloudFormation, theo đúng thứ tự nhóm đã dùng cho production:

1. `fcaj-poc` và `fcaj-v3-base`: nơi lưu dữ liệu, mạng, database ([5.4](../5.4-network-data/)).
2. `fcaj-v3-runtime`: NAT, ALB, Auto Scaling group ([5.5](../5.5-compute-origin/)).
3. `fcaj-v3-edge` ở us-east-1: CloudFront, WAF, DNS ([5.6](../5.6-edge-security/)).
4. `fcaj-v3-ai-worker`: Lambda AI worker ([5.7](../5.7-ai-worker/)).
5. `fcaj-v3-ses`, `fcaj-v3-cicd`, `fcaj-v3-monitoring`, `fcaj-v3-audit` ([5.8](../5.8-email-ses/), [5.9](../5.9-cicd/), [5.10](../5.10-testing-validation/)).

{{% notice info %}}
Mọi template nằm trong repository của nhóm, thư mục `infra/cloudformation/` và `infra/v3/`. Account ID, hosted zone ID, secret, instance ID được thay bằng placeholder (`<ACCOUNT_ID>`, `<ZONE_ID>`, `<DOMAIN>`…). Hãy thay bằng giá trị của bạn và không bao giờ commit secret thật.
{{% /notice %}}

**Thời gian ước tính:** 3–4 giờ (phần lớn là chờ RDS, chứng chỉ và CloudFront).

**Chi phí ước tính:** khoảng 5 USD mỗi ngày khi stack runtime đang chạy; xem [5.11](../5.11-cost/).
