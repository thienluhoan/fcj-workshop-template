---
title: "Worklog Tuần 11"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 11:

- Xây dựng, lưu trữ và tích hợp cơ chế xác thực người dùng cho ứng dụng trang đơn (**Single Page Application - SPA**) trên AWS.
- Trải nghiệm và tích hợp các dịch vụ Trí tuệ nhân tạo được quản lý sẵn (**AWS AI Services**) vào ứng dụng thực tế.
- Nắm vững cách điều phối các luồng xử lý nghiệp vụ phân tán bằng **AWS Step Functions**.
- Phát triển kiến trúc phi máy chủ (**Serverless**) sử dụng **AWS Lambda** tương tác trực tiếp với **Amazon S3** và **Amazon DynamoDB**.
- Hoàn thiện ứng dụng Serverless End-to-End bằng cách kết nối giao diện Frontend với **Amazon API Gateway**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Tạo và xác thực ứng dụng trang đơn (Create and authenticate Single Page Application):**<br>&emsp;+ Triển khai giao diện Single Page Application (SPA) lên **Amazon S3** (Static Website Hosting) kết hợp **Amazon CloudFront**.<br>&emsp;+ Khởi tạo **Amazon Cognito User Pool** để quản lý đăng ký, đăng nhập và xác thực người dùng.<br>&emsp;+ Tích hợp AWS Amplify / Cognito SDK vào ứng dụng Frontend để xử lý luồng cấp phát và kiểm tra JWT Token.<br>&emsp;+ Kiểm thử tính năng đăng ký, xác thực tài khoản và dọn dẹp tài nguyên sau bài lab. | 23/11/2026   | 23/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Trải nghiệm các dịch vụ AI trên AWS (Experience AI services on AWS):**<br>&emsp;+ Khám phá nhóm dịch vụ AI quản lý sẵn của AWS: **Amazon Rekognition** (nhận diện hình ảnh), **Amazon Comprehend** (xử lý ngôn ngữ tự nhiên), **Amazon Polly** & **Transcribe**.<br>&emsp;+ Thực hành gọi API của các dịch vụ AI thông qua AWS SDK / AWS Console để phân tích văn bản, trích xuất nhãn hình ảnh và chuyển đổi giọng nói.<br>&emsp;+ Đánh giá độ chính xác, chi phí và khả năng tích hợp nhanh chóng vào hệ thống mà không cần huấn luyện mô hình từ đầu.<br>&emsp;+ Dọn dẹp tài nguyên thử nghiệm sau khi hoàn thành. | 24/11/2026   | 24/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Làm quen với AWS Step Functions (Getting Started with AWS Step Functions):**<br>&emsp;+ Tìm hiểu kiến trúc máy trạng thái (**State Machine**) và ngôn ngữ định nghĩa **Amazon States Language (ASL)**.<br>&emsp;+ Thiết kế luồng xử lý công việc (Workflow) kết hợp nhiều trạng thái (Task, Choice, Parallel, Wait) thông qua Workflow Studio.<br>&emsp;+ Tích hợp Step Functions điều phối nhiều hàm AWS Lambda và cấu hình cơ chế xử lý lỗi (Retry, Catch).<br>&emsp;+ Theo dõi trực quan quá trình thực thi từng bước của State Machine và dọn dẹp tài nguyên. | 25/11/2026   | 25/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Serverless - Lambda tương tác với S3 và DynamoDB (Serverless - Lambda interacting with S3 and DynamoDB):**<br>&emsp;+ Cấu hình quyền thực thi (**IAM Execution Role**) tuân thủ nguyên tắc đặc quyền tối thiểu cho hàm **AWS Lambda**.<br>&emsp;+ Thiết lập **S3 Event Notification** để tự động kích hoạt Lambda khi có tệp tin mới được tải lên bucket.<br>&emsp;+ Lập trình hàm Lambda xử lý dữ liệu từ S3 và thực hiện các thao tác CRUD (Put, Get, Update, Query) vào bảng **Amazon DynamoDB**.<br>&emsp;+ Kiểm tra nhật ký hoạt động qua **Amazon CloudWatch Logs** và dọn dẹp tài nguyên. | 26/11/2026   | 26/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Serverless - Xây dựng Frontend gọi API Gateway (Serverless - Build Frontend to call API Gateway):**<br>&emsp;+ Khởi tạo và cấu hình **Amazon API Gateway** (REST API / HTTP API) đóng vai trò làm cổng giao tiếp cho hệ thống backend.<br>&emsp;+ Tích hợp API Gateway với các hàm **AWS Lambda** và cấu hình **CORS** (Cross-Origin Resource Sharing) cho phép truy cập từ trình duyệt.<br>&emsp;+ Kết nối ứng dụng Frontend (SPA) gọi các endpoint API để gửi và hiển thị dữ liệu thời gian thực từ DynamoDB.<br>&emsp;+ Kiểm thử toàn bộ luồng dữ liệu End-to-End (Frontend $\rightarrow$ API Gateway $\rightarrow$ Lambda $\rightarrow$ DynamoDB) và dọn dẹp tài nguyên. | 27/11/2026   | 27/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 11:

- Triển khai thành công ứng dụng **Single Page Application (SPA)** tốc độ cao với **S3, CloudFront** và bảo mật luồng xác thực người dùng bằng **Amazon Cognito**.
- Nắm vững cách ứng dụng các dịch vụ **AWS AI Services** (Rekognition, Comprehend, Polly, Transcribe) để bổ sung tính năng thông minh vào ứng dụng một cách nhanh chóng.
- Làm chủ **AWS Step Functions** trong việc thiết kế, điều phối và giám sát các quy trình nghiệp vụ phức tạp theo kiến trúc Serverless.
- Thành thạo việc phát triển kiến trúc hướng sự kiện với **AWS Lambda** kết nối liền mạch với **Amazon S3** và **Amazon DynamoDB**.
- Hoàn thiện mô hình ứng dụng **Serverless Full-stack**, kết nối giao diện người dùng với Backend thông qua **Amazon API Gateway** và đảm bảo thu dọn sạch sẽ tài nguyên sau mỗi bài lab.
