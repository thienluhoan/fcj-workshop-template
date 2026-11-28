---
title: "Kiến trúc và luồng dữ liệu"
date: 2026-11-27
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Sơ đồ kiến trúc

Nhóm tự vẽ sơ đồ này bằng draw.io. Sơ đồ thể hiện cấu hình ngày thường: một NAT gateway và database Single-AZ. Các nhãn "contest season" đánh dấu phần chỉ bật trong mùa thi (NAT gateway thứ hai và RDS standby Multi-AZ).

![Kiến trúc SEAL trên AWS](/images/5-Workshop/5.3-Architecture/seal-aws-v3.webp)

#### Bố trí mạng

| Tầng | Subnet (AZ A / AZ B) | Thành phần | Đường ra Internet |
|---|---|---|---|
| Public | `10.20.0.0/24`, `10.20.1.0/24` | ALB, NAT gateway | Internet Gateway |
| Private app | `10.20.10.0/24`, `10.20.11.0/24` | EC2 chạy API | NAT gateway; S3 đi qua Gateway Endpoint |
| Private DB | `10.20.20.0/24`, `10.20.21.0/24` | RDS MySQL | Không có |

Security group nối các tầng theo chuỗi: ALB chỉ nhận HTTPS **từ prefix list origin-facing của CloudFront**, API chỉ nhận cổng 8443 từ ALB, database chỉ nhận cổng 3306 từ API.

#### Luồng dữ liệu

Số thứ tự khớp với mũi tên trên sơ đồ.

| # | Luồng | Chi tiết |
|---|---|---|
| ① | Người dùng → CloudFront | Route 53 trỏ tên miền về CloudFront. WAF kiểm tra mọi request. TLS dùng chứng chỉ ACM ở us-east-1. |
| ② | CloudFront → S3 (frontend) | File React tĩnh. Bucket private, chỉ CloudFront đọc được qua Origin Access Control. CloudFront Function đổi deep link về `index.html`. |
| ③ | CloudFront → ALB → EC2 | `/api/*`, `/oauth2/authorization/*`, `/login/oauth2/*` và `/uploads/*` đi tới `origin.<DOMAIN>`. CloudFront gắn header bí mật `X-Origin-Verify`; thiếu header thì ALB trả 403. ALB chuyển tới nginx cổng 8443, nginx chuyển tiếp tới Spring Boot ở cổng loopback 8080. |
| ④ | EC2 → RDS | SQL qua TLS có kiểm tra chứng chỉ, dùng login ứng dụng quyền tối thiểu. |
| ⑤ | EC2 → S3 (application) | File upload và snapshot AI, đi qua S3 Gateway Endpoint (không tốn phí NAT). |
| ⑥ | EC2 → Internet | GitHub API (commit SHA, cây thư mục, tối đa 50 file nguồn) và trao đổi token OAuth, qua NAT gateway. |
| ⑦ | EC2 → SQS Jobs | Khi giám khảo yêu cầu AI đánh giá, API ghi snapshot (manifest, source, rubric) vào S3, lưu job và outbox trong RDS rồi gửi message. Request của giám khảo trả về ngay. |
| ⑧ | SQS → Lambda | Event source mapping kích hoạt AI worker, tối đa 2 job cùng lúc. Message lỗi vào Job DLQ. |
| ⑨ | Lambda ↔ S3 | Worker đọc snapshot (kiểm tra checksum) và ghi `results/{jobId}/output.json` một lần duy nhất. |
| ⑩ | Lambda → DeepSeek | Che secret và giới hạn 240.000 ký tự trước khi gọi. Kết quả model được kiểm tra theo JSON schema. |
| ⑪ | Lambda → SQS Results | Message ngắn chứa vị trí kết quả. Lỗi vào Result DLQ. |
| ⑫ | EC2 ← SQS Results | API đọc hàng đợi, lưu kết quả vào RDS, đánh dấu job `SUCCEEDED` hoặc `FAILED`. |
| ⑬ | EC2 → SES | OTP đặt lại mật khẩu và thông báo, gửi từ domain đã xác minh có DKIM. |
| ⑭ | Trình duyệt ↔ Google/GitHub | Đăng nhập OAuth. ALB stickiness giữ state OAuth trên một instance. |
| ⑮ | GitHub Actions → AWS | CI/CD: đẩy image lên ECR (15a), sync web lên S3 (15b), deploy (15c), Run Command (15d), instance kéo image (15e). |
| ⑯ | EC2/Lambda → Parameter Store | Cấu hình runtime và key ở dạng SecureString, mã hóa bằng KMS. |
| ⑰ | CloudWatch → SNS | Alarm và Budgets gửi email cho đội vận hành. |

#### Đối chiếu AWS Well-Architected Framework

| Trụ cột | SEAL áp dụng thế nào |
|---|---|
| **Operational excellence** | Toàn bộ hạ tầng là CloudFormation. Phát hành qua GitHub Actions và Systems Manager Run Command, từng instance một, tự rollback. Không SSH, chỉ Session Manager. Có log, alarm và CloudTrail. |
| **Security** | WAF với managed rule và giới hạn tần suất cho đăng nhập, OTP. API và DB ở private subnet. Origin chỉ nhận traffic từ CloudFront (prefix list + header bí mật). Secret ở Parameter Store SecureString. TLS ở mọi chặng, S3 Block Public Access, IAM role quyền tối thiểu, GitHub OIDC thay cho access key. |
| **Reliability** | Hai Availability Zone, Auto Scaling group với ELB health check `/health/ready`, SQS có dead-letter queue và outbox để không mất AI job, RDS backup tự động. RDS Multi-AZ và NAT thứ hai cho mùa thi. |
| **Performance efficiency** | CloudFront cache file tĩnh gần người dùng. Phần AI chạy bất đồng bộ trên Lambda nên API luôn nhanh. Auto Scaling group có sẵn policy target tracking CPU 60 %; ở cấu hình hiện tại (min = max = 2) policy chưa co giãn được, xem [5.5](../5.5-compute-origin/). |
| **Cost optimization** | RDS Graviton `db.t4g.micro`, instance `t3.small`, một NAT gateway ngoài mùa thi, S3 Gateway Endpoint thay cho traffic NAT, AI worker serverless, xóa được stack runtime giữa các buổi, cảnh báo Budgets. |
