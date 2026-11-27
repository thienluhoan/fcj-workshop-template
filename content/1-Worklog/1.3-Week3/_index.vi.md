---
title: "Worklog Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

- Nắm cơ sở dữ liệu quan hệ (Amazon RDS) và NoSQL (Amazon DynamoDB) trên AWS.
- Hiểu kiến trúc hướng sự kiện với Amazon SNS/SQS và phân phối nội dung bằng CloudFront.
- Lên danh sách ý tưởng dự án Cloud-Native và khảo sát doanh nghiệp thật để chọn đề tài có giá trị thực.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Workshop Get Started with Amazon RDS:<br>&emsp;+ VPC, Security Group, DB subnet group<br>&emsp;+ Tạo RDS, launch EC2, deploy ứng dụng<br>&emsp;+ Backup & restore, dọn dẹp | 28/09/2026 | 28/09/2026 | [000005 – Amazon RDS](https://000005.awsfcaj.com/)<br>[000005 – 6. Backup and restore](https://000005.awsfcaj.com/6-backup/) |
| 3 | - Lập danh sách 11 ý tưởng dự án AWS Cloud-Native<br>- Thiết kế bảng khảo sát công ty thi công (17 câu, 4 phần A–D)<br>- Viết script sinh form HTML và script kiểm tra tự động<br>- Kiểm tra hiển thị desktop / mobile / bản in | 29/09/2026 | 29/09/2026 | [FCJ – 8. FCJ Workforce Program](https://cloudjourney.awsfcaj.com/8-fcjworkforce/)<br>Danh sách ý tưởng dự án<br>Bảng khảo sát công ty thi công<br>Script sinh form và kiểm tra<br>Ảnh QA desktop/mobile/print |
| 4 | - Workshop Amazon DynamoDB:<br>&emsp;+ Thành phần cốt lõi, primary key, secondary index, capacity mode<br>&emsp;+ CRUD, Query, GSI trên console và CloudShell<br>&emsp;+ Python boto3: tạo bảng, ghi/đọc, Query vs Scan | 30/09/2026 | 30/09/2026 | [000060 – 1. Introduction](https://000060.awsfcaj.com/1-introduce/)<br>[000060 – 2. Console & CloudShell](https://000060.awsfcaj.com/2-prerequiste/)<br>[000060 – 3. AWS SDK (Python)](https://000060.awsfcaj.com/3-gettingstartedwithawssdk/) |
| 5 | - Workshop Event-driven Architecture with SNS and SQS:<br>&emsp;+ Kiến trúc, chọn loại SQS queue<br>&emsp;+ Pub/Sub và fan-out<br>&emsp;+ Message filtering cơ bản và nâng cao | 01/10/2026 | 01/10/2026 | [000077 – 1.1 Architecture & Event Flow](https://000077.awsfcaj.com/1-introduce/1.1-architecture/)<br>[000077 – 1.3 SQS Queue Selection & Deep Dive](https://000077.awsfcaj.com/1-introduce/1.3-sqs/)<br>[000077 – 3. Simple Pub/Sub & Fan-Out](https://000077.awsfcaj.com/3-simplesubpub/)<br>[000077 – 4. Message Filtering](https://000077.awsfcaj.com/4-messagefiltering/)<br>[000077 – 5. Advanced Message Filtering](https://000077.awsfcaj.com/5-advancemessage/) |
| 6 | - Workshop CloudFront with S3 bucket origin<br>- Đọc tài liệu Origin Access Control (OAC) | 02/10/2026 | 02/10/2026 | [000094 – 1. Create S3 bucket](https://000094.awsfcaj.com/1.1-t%E1%BA%A1o-nh%C3%B3m-s3/)<br>[000094 – 3. Configure Amazon CloudFront](https://000094.awsfcaj.com/1.3-c%E1%BA%A5u-h%C3%ACnh-amazon-cloudfront/)<br>[AWS Docs – Restrict access to an S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)<br>[AWS Docs – Invalidating files](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html) |
| 7 | - Tổng hợp câu trả lời khảo sát của giám đốc công ty thi công EVG<br>- Làm bảng khảo sát đa ngành (form HTML) để khảo sát thêm<br>- Viết bản phân tích và đề xuất dự án AWS cho EVG | 03/10/2026 | 03/10/2026 | Câu trả lời khảo sát<br>Form khảo sát đa ngành<br>Đề xuất dự án EVG<br>[AWS Pricing Calculator](https://calculator.aws/) |

### Kết quả đạt được tuần 3:

- Dựng RDS trong private subnet, kết nối từ ứng dụng trên EC2, thử backup/restore.
- Thao tác DynamoDB bằng console, CloudShell và Python (boto3): CRUD, Query, Scan, GSI.
- Dựng mô hình pub/sub fan-out SNS → SQS và lọc message bằng filter policy.
- Phân phối website S3 qua CloudFront, hiểu cache và invalidation.
- Lập danh sách 11 ý tưởng dự án Cloud-Native; thiết kế bảng khảo sát 17 câu (4 phần A–D) cho công ty thi công, tự động sinh HTML và kiểm tra hiển thị desktop/mobile/bản in.
- Thu được câu trả lời khảo sát từ giám đốc công ty thi công EVG, viết bản phân tích và đề xuất dự án (MVP, kiến trúc AWS tối giản, kế hoạch 9–10 tuần, tiêu chí nghiệm thu); làm thêm bảng khảo sát đa ngành.
