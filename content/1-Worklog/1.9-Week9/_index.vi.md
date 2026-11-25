---
title: "Worklog Tuần 9"
date: 2026-11-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 9:

- Thiết kế và triển khai kiến trúc cơ sở dữ liệu NoSQL nâng cao với **Amazon DynamoDB**, tối ưu hóa khả năng mở rộng và hiệu suất truy vấn.
- Nắm vững các mô hình cam kết tài nguyên để tối ưu chi phí vận hành trên AWS thông qua **Savings Plans**, **Reserved Instances (RI)** và **Reserved DB Instances**.
- Thực hành đánh giá và lựa chọn kích thước tài nguyên máy chủ phù hợp (**Right-sizing**) nhằm loại bỏ lãng phí cho **Amazon EC2**.
- Xây dựng hệ thống giám sát và trực quan hóa dữ liệu chi phí sử dụng tài nguyên trên nền tảng AWS.
- Thực hiện phân tích chuyên sâu dữ liệu chi phí và mức độ sử dụng (Cost & Usage) bằng cách kết hợp **AWS Glue** và **Amazon Athena**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Kiến trúc nâng cao với Amazon DynamoDB:**<br>&emsp;+ Tìm hiểu kiến trúc phân vùng (Partitioning), thiết kế khóa chính (Partition Key, Sort Key) và chỉ mục phụ (GSI, LSI).<br>&emsp;+ Cấu hình chế độ đọc/ghi (Provisioned vs On-Demand) và tính năng Auto Scaling cho bảng DynamoDB.<br>&emsp;+ Thực hành triển khai DynamoDB Streams kết hợp AWS Lambda để xử lý dữ liệu hướng sự kiện (Event-driven).<br>&emsp;+ Kiểm thử hiệu năng truy vấn, bật DynamoDB Accelerator (DAX) / Point-in-Time Recovery và dọn dẹp tài nguyên. | 09/11/2026   | 09/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Tối ưu chi phí với Savings Plans, Reserved Instance, Reserved DB Instance:**<br>&emsp;+ Phân tích sự khác biệt và phạm vi áp dụng giữa Compute Savings Plans, EC2 Instance Savings Plans và Reserved Instances (RI).<br>&emsp;+ Tìm hiểu cơ chế tối ưu chi phí cơ sở dữ liệu với Amazon RDS Reserved DB Instances.<br>&emsp;+ Sử dụng **AWS Cost Explorer** để xem các đề xuất mua gói tiết kiệm (Recommendations) dựa trên lịch sử sử dụng thực tế.<br>&emsp;+ Thực hành tính toán bài toán tối ưu chi phí dài hạn và lập kế hoạch cam kết tài nguyên phù hợp. | 10/11/2026   | 10/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Lựa chọn kích thước phù hợp để tối ưu hóa tài nguyên Amazon EC2:**<br>&emsp;+ Thu thập các chỉ số hiệu năng hệ thống (CPU, RAM, Network, Disk I/O) thông qua **Amazon CloudWatch** và CloudWatch Agent.<br>&emsp;+ Sử dụng **AWS Compute Optimizer** để phân tích các máy chủ EC2 đang chạy dư thừa (Over-provisioned) hoặc thiếu tài nguyên (Under-provisioned).<br>&emsp;+ Thực hành điều chỉnh lại loại máy chủ (Instance Type / Right-sizing) phù hợp với tải thực tế.<br>&emsp;+ Kiểm tra độ ổn định của ứng dụng sau khi thay đổi cấu hình và dọn dẹp tài nguyên bài lab. | 11/11/2026   | 11/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Trực quan hóa chi phí sử dụng trên AWS:**<br>&emsp;+ Kích hoạt và cấu hình **AWS Cost and Usage Reports (CUR)** lưu trữ định kỳ vào Amazon S3.<br>&emsp;+ Tìm hiểu các công cụ trực quan hóa chi phí như **AWS Cost Explorer**, **AWS Budgets** và **Amazon QuickSight**.<br>&emsp;+ Thiết lập cảnh báo ngân sách (Budget Alerts) khi chi phí thực tế hoặc dự báo vượt ngưỡng cho phép.<br>&emsp;+ Xây dựng biểu đồ trực quan theo dõi chi phí theo từng dịch vụ, khu vực (Region) và thẻ tài nguyên (Cost Allocation Tags). | 12/11/2026   | 12/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Phân tích chi phí sử dụng nâng cao với AWS Glue và Amazon Athena:**<br>&emsp;+ Cấu hình **AWS Glue Crawler** tự động thu thập metadata từ dữ liệu báo cáo chi phí (CUR) trên Amazon S3.<br>&emsp;+ Sử dụng **Amazon Athena** để viết các câu truy vấn SQL phức tạp phân tích chi tiết chi phí theo từng tài nguyên, giờ sử dụng và loại giao dịch.<br>&emsp;+ Tối ưu hóa hiệu năng và chi phí truy vấn bằng cách chuyển đổi định dạng dữ liệu sang Apache Parquet và phân vùng dữ liệu (Partitioning).<br>&emsp;+ Dọn dẹp toàn bộ các bảng dữ liệu, Crawler và tài nguyên thử nghiệm sau bài lab. | 13/11/2026   | 13/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 9:

- Thiết kế thành thạo mô hình dữ liệu và kiến trúc mở rộng nâng cao trên **Amazon DynamoDB**, tối ưu hóa tốc độ truy vấn và khả năng xử lý tự động theo sự kiện.
- Hiểu rõ cách thức hoạt động và áp dụng hiệu quả các chính sách giảm giá dài hạn như **Savings Plans**, **Reserved Instances** và **Reserved DB Instances** để tối ưu hóa ngân sách hạ tầng.
- Nắm vững quy trình giám sát hiệu năng và thực hiện **Right-sizing** cho **Amazon EC2** bằng **AWS Compute Optimizer** và **CloudWatch**, đảm bảo cân bằng giữa hiệu suất và chi phí.
- Thiết lập hoàn chỉnh hệ thống theo dõi, cảnh báo ngân sách và trực quan hóa dữ liệu chi phí AWS một cách trực quan, minh bạch.
- Ứng dụng thành công bộ đôi **AWS Glue** và **Amazon Athena** để truy vấn và phân tích dữ liệu chi phí quy mô lớn (CUR) phục vụ công tác quản trị tài chính đám mây (FinOps).
- Đảm bảo thu dọn sạch sẽ toàn bộ tài nguyên thử nghiệm sau mỗi bài lab để không phát sinh chi phí ngoài ý muốn.
