---
title: "Worklog Tuần 8"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 8:

- Xây dựng quy trình tích hợp và triển khai liên tục (CI/CD) tự động với **AWS CodePipeline**.
- Tự động hóa hoàn toàn quy trình đóng gói và phát hành ứng dụng từ kho lưu trữ mã nguồn đến môi trường triển khai.
- Tích hợp hạ tầng lưu trữ On-premises với Đám mây AWS bằng **AWS Storage Gateway (File Gateway)**.
- Khởi tạo và cấu hình hệ thống tệp chia sẻ băng thông rộng chuẩn Windows native bằng **Amazon FSx for Windows File Server**.
- Tìm hiểu kiến trúc và thực hành xây dựng giải pháp hồ dữ liệu **Data Lake** trên nền tảng AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Triển khai ứng dụng với AWS CodePipeline:**<br>&emsp;+ Tìm hiểu kiến trúc CI/CD cơ bản (Source, Build, Deploy stages).<br>&emsp;+ Khởi tạo kho lưu trữ mã nguồn (Source stage với AWS CodeCommit / GitHub).<br>&emsp;+ Tạo Pipeline cơ bản trong AWS CodePipeline kết nối với kho mã nguồn.<br>&emsp;+ Kiểm thử luồng chạy khởi tạo tài nguyên ban đầu và dọn dẹp tài nguyên bài lab. | 02/11/2026   | 02/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Tự động hóa triển khai ứng dụng bằng AWS CodePipeline:**<br>&emsp;+ Tích hợp **AWS CodeBuild** để tự động biên dịch ứng dụng và chạy các bài kiểm thử unit test.<br>&emsp;+ Tích hợp **AWS CodeDeploy** để tự động hóa việc đẩy ứng dụng lên danh sách các máy chủ EC2 / ECS.<br>&emsp;+ Thực hành push mã nguồn mới (Commit code) và theo dõi quy trình tự động hóa tự triggered từ Source $\rightarrow$ Build $\rightarrow$ Deploy.<br>&emsp;+ Kiểm tra kết quả cập nhật ứng dụng thành công và dọn dẹp tài nguyên. | 03/11/2026   | 03/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Lưu trữ dữ liệu không giới hạn trên AWS với File Storage Gateway:**<br>&emsp;+ Khởi tạo S3 Bucket làm kho lưu trữ phía sau (Backend Storage).<br>&emsp;+ Triển khai AWS Storage Gateway Appliance (File Gateway) trên máy chủ EC2/Virtual Machine.<br>&emsp;+ Cấu hình NFS / SMB File Share kết nối tới S3 Bucket.<br>&emsp;+ Thực hành mount tệp tin từ client nội bộ lên File Gateway và kiểm tra dữ liệu được đồng bộ tự động lên Amazon S3.<br>&emsp;+ Dọn dẹp tài nguyên. | 04/11/2026   | 04/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Triển khai kho lưu trữ đa năng cho Windows bằng FSx:**<br>&emsp;+ Khởi tạo Amazon FSx for Windows File Server hệ thống tệp chuẩn Windows SMB.<br>&emsp;+ Kết nối FSx với AWS Directory Service / Active Directory để quản lý quyền truy cập tập trung.<br>&emsp;+ Mount ổ đĩa FSx vào máy chủ EC2 Windows thông qua giao thức SMB.<br>&emsp;+ Kiểm tra hiệu năng đọc/ghi, tính năng Shadow Copies và dọn dẹp tài nguyên sau bài lab. | 05/11/2026   | 05/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Xây dựng Data Lake trên AWS:**<br>&emsp;+ Tìm hiểu mô hình kiến trúc Data Lake chuẩn trên AWS (Storage, Ingestion, Processing, Analytics).<br>&emsp;+ Khởi tạo S3 Buckets phân chia các tầng dữ liệu (Raw Data, Processed Data).<br>&emsp;+ Tích hợp **AWS Glue Crawler** để tự động quét schema và lưu vào **AWS Glue Data Catalog**.<br>&emsp;+ Thực thi truy vấn phân tích dữ liệu trực tiếp bằng **Amazon Athena**.<br>&emsp;+ Dọn dẹp các tài nguyên dữ liệu thử nghiệm. | 06/11/2026   | 06/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 8:

- Xây dựng hoàn chỉnh đường ống **CI/CD tự động** bằng **AWS CodePipeline**, kết hợp **CodeCommit, CodeBuild, và CodeDeploy** để tự động hóa toàn bộ quy trình phát hành phần mềm.
- Nắm vững giải pháp lưu trữ lai (Hybrid Cloud Storage) sử dụng **AWS Storage Gateway (File Gateway)**, giúp mở rộng không gian lưu trữ local lên **Amazon S3** một cách minh bạch.
- Làm chủ việc cấu hình và quản trị hệ thống chia sẻ tệp doanh nghiệp chuẩn Windows sử dụng **Amazon FSx for Windows File Server**.
- Thiết lập thành công mô hình **Data Lake** cơ bản trên AWS, biết cách lưu trữ dữ liệu trên S3, trích xuất Schema tự động bằng **AWS Glue** và truy vấn dữ liệu nhanh chóng bằng **Amazon Athena**.
- Đảm bảo thu dọn sạch sẽ toàn bộ tài nguyên thử nghiệm sau mỗi bài lab để tối ưu hóa chi phí sử dụng AWS.
