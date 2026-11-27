---
title: "Worklog Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

- Hiểu cách AWS xác thực/ủy quyền một request và dùng điều kiện (Condition) trong IAM.
- Mã hóa dữ liệu với AWS KMS và kiểm toán việc dùng khóa bằng CloudTrail.
- Bảo vệ ứng dụng web bằng AWS WAF; quản lý bí mật bằng Secrets Manager.
- Áp dụng best practice bảo mật cho Amazon S3.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Workshop IAM Role & Condition:<br>&emsp;+ Cách AWS xác thực request, quy trình assume role<br>&emsp;+ Tạo admin role, cấu hình switch role<br>&emsp;+ Giới hạn switch role theo IP và thời gian<br>- Đọc IAM Security best practices | 12/10/2026 | 12/10/2026 | [000044 – IAM Role & Condition](https://000044.awsfcaj.com/)<br>[IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 3 | - Workshop Encryption at Rest with AWS KMS:<br>&emsp;+ Tạo policy/role, group/user<br>&emsp;+ Tạo KMS key, mã hóa object S3<br>&emsp;+ CloudTrail + Athena kiểm toán việc dùng khóa<br>&emsp;+ Chia sẻ dữ liệu đã mã hóa | 13/10/2026 | 13/10/2026 | [000033 – KMS](https://000033.awsfcaj.com/)<br>[KMS pricing](https://aws.amazon.com/kms/pricing/) |
| 4 | - Workshop AWS Web Application Firewall:<br>&emsp;+ Web ACL với managed rules<br>&emsp;+ Custom rule, advanced custom rule<br>&emsp;+ Kiểm thử rule, bật request logging<br>- Đọc kiến thức nền WAF/DDoS | 14/10/2026 | 14/10/2026 | [000026 – AWS WAF](https://000026.awsfcaj.com/3-useawswaf/)<br>[WAF pricing](https://aws.amazon.com/waf/pricing/) |
| 5 | - Workshop Using AWS Secrets Manager with Amazon RDS and AWS Fargate:<br>&emsp;+ Lưu thông tin đăng nhập RDS trong Secrets Manager<br>&emsp;+ Ứng dụng trên Fargate đọc secret lúc chạy<br>&emsp;+ Xoay vòng secret | 15/10/2026 | 15/10/2026 | [000096 – 1. Introduction](https://000096.awsfcaj.com/1-intro/)<br>[000096 – 3. Using on RDS](https://000096.awsfcaj.com/3-rds-phase/)<br>[000096 – 4. Using on Fargate](https://000096.awsfcaj.com/4-fargate-phase/)<br>[AWS Secrets Manager – Pricing](https://aws.amazon.com/secrets-manager/pricing/) |
| 6 | - Workshop S3 Security Best Practices:<br>&emsp;+ Bắt buộc HTTPS, bắt buộc SSE-S3<br>&emsp;+ Chặn ACL public, S3 Block Public Access<br>&emsp;+ Giới hạn truy cập qua VPC endpoint<br>&emsp;+ AWS Config phát hiện bucket public, IAM Access Analyzer | 16/10/2026 | 16/10/2026 | [000069 – S3 Security Best Practices](https://000069.awsfcaj.com/) |

### Kết quả đạt được tuần 5:

- Tạo IAM role có điều kiện giới hạn switch role theo IP và theo thời gian; nắm nguyên tắc explicit deny.
- Tạo KMS key, mã hóa object S3 bằng SSE-KMS, truy vấn CloudTrail bằng Athena để xem ai dùng khóa.
- Tạo Web ACL với managed rules, custom rule, rate-based rule và bật logging; hiểu cách tính phí WAF.
- Lưu và xoay vòng thông tin đăng nhập database bằng Secrets Manager.
- Áp dụng 7 biện pháp bảo mật S3: bắt buộc HTTPS, bắt buộc mã hóa, chặn ACL public, Block Public Access, giới hạn qua VPC endpoint, AWS Config, IAM Access Analyzer.
