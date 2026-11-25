---
title: "Worklog Tuần 10"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 10:

- Thực hành chuyển đổi (Migrate) ứng dụng kiến trúc nguyên khối (**Monolith**) lên nền tảng đám mây AWS để chuẩn bị cho quá trình hiện đại hóa.
- Thiết lập quy trình tự động phát hành ứng dụng (**Auto-release**) giúp rút ngắn thời gian triển khai và giảm thiểu lỗi thủ công.
- Tách biệt và xây dựng các dịch vụ nhỏ độc lập (**Microservices**) từ hệ thống nguyên khối ban đầu.
- Tái cấu trúc dữ liệu và luồng xử lý công việc (**Data & Workflow Restructuring**) để đảm bảo tính độc lập giữa các dịch vụ.
- Triển khai cơ chế giao tiếp bất đồng bộ và hướng sự kiện (**Messaging & Eventing**) cho kiến trúc Microservices trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Chuyển đổi ứng dụng nguyên khối (Application Migrate Monolith):**<br>&emsp;+ Đánh giá kiến trúc ứng dụng Monolith hiện tại, xác định các thành phần phụ thuộc (Database, Storage, Network).<br>&emsp;+ Đóng gói ứng dụng bằng Docker Container và đẩy image lên **Amazon ECR**.<br>&emsp;+ Triển khai ứng dụng Monolith lên **Amazon ECS** hoặc EC2 kết hợp với **Application Load Balancer (ALB)**.<br>&emsp;+ Kiểm tra khả năng vận hành ổn định của hệ thống sau khi di chuyển lên AWS. | 16/11/2026   | 16/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Tự động hóa phát hành ứng dụng (Auto-release apps):**<br>&emsp;+ Cấu hình quy trình CI/CD tự động tích hợp với **AWS CodePipeline**, **CodeBuild** và **CodeDeploy** / ECS.<br>&emsp;+ Áp dụng chiến lược triển khai không gián đoạn (Blue/Green hoặc Rolling Deployment) để giảm thiểu downtime.<br>&emsp;+ Thiết lập cơ chế tự động hoàn tác (Auto Rollback) khi phát hiện lỗi trong quá trình phát hành phiên bản mới.<br>&emsp;+ Thực hành cập nhật mã nguồn và kiểm tra luồng phát hành tự động từ đầu đến cuối. | 17/11/2026   | 17/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Xây dựng dịch vụ vi mô (Create a Microservice):**<br>&emsp;+ Phân tích ranh giới nghiệp vụ (Bounded Context) để tách một chức năng độc lập ra khỏi ứng dụng Monolith.<br>&emsp;+ Xây dựng và đóng gói Microservice độc lập, triển khai lên **Amazon ECS** hoặc **AWS Lambda**.<br>&emsp;+ Cấu hình **Amazon API Gateway** hoặc định tuyến theo đường dẫn (Path-based Routing) trên ALB để điều hướng lưu lượng truy cập.<br>&emsp;+ Kiểm thử giao tiếp và đảm bảo Microservice hoạt động độc lập mà không làm gián đoạn hệ thống chính. | 18/11/2026   | 18/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Tái cấu trúc dữ liệu và luồng xử lý (Data and workflow restructuring):**<br>&emsp;+ Tách cơ sở dữ liệu dùng chung (Shared Database) thành các kho dữ liệu riêng biệt (Database-per-Service) sử dụng **Amazon RDS** và **Amazon DynamoDB**.<br>&emsp;+ Đồng bộ hóa và di chuyển dữ liệu giữa các dịch vụ mà không làm gián đoạn tính toàn vẹn dữ liệu.<br>&emsp;+ Sử dụng **AWS Step Functions** để điều phối (Orchestrate) các luồng xử lý nghiệp vụ phức tạp qua nhiều dịch vụ.<br>&emsp;+ Kiểm tra tính nhất quán của dữ liệu và xử lý ngoại lệ trong luồng công việc mới. | 19/11/2026   | 19/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Giao tiếp và hướng sự kiện trong Microservices (Microservices Messaging and Eventing):**<br>&emsp;+ Giảm sự phụ thuộc chặt chẽ (Decoupling) giữa các Microservices bằng mô hình giao tiếp bất đồng bộ.<br>&emsp;+ Sử dụng **Amazon SQS** để xếp hàng đợi thông điệp và **Amazon SNS** cho mô hình Pub/Sub phân phối thông báo.<br>&emsp;+ Tích hợp **Amazon EventBridge** để định tuyến các sự kiện nghiệp vụ giữa các dịch vụ AWS và ứng dụng.<br>&emsp;+ Kiểm thử luồng truyền nhận sự kiện, xử lý tin nhắn lỗi (Dead-Letter Queue) và dọn dẹp tài nguyên bài lab. | 20/11/2026   | 20/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 10:

- Hoàn thành việc di chuyển ứng dụng **Monolith** lên hạ tầng đám mây AWS một cách an toàn, tạo tiền đề cho việc hiện đại hóa kiến trúc.
- Xây dựng thành công quy trình **Auto-release** tự động hóa hoàn toàn việc kiểm thử, đóng gói và phát hành ứng dụng với độ tin cậy cao.
- Nắm vững phương pháp phân tách chức năng (Strangler Fig Pattern) để chuyển đổi từng phần của Monolith sang **Microservices** độc lập.
- Làm chủ kỹ thuật tái cấu trúc cơ sở dữ liệu riêng biệt cho từng dịch vụ và điều phối quy trình nghiệp vụ bằng **AWS Step Functions**.
- Triển khai thành công kiến trúc hướng sự kiện (Event-Driven Architecture) sử dụng **Amazon SQS, SNS và EventBridge**, giúp hệ thống mở rộng linh hoạt và chịu lỗi tốt.
- Đảm bảo thu dọn sạch sẽ toàn bộ tài nguyên thử nghiệm sau mỗi bài lab để tối ưu hóa chi phí sử dụng AWS.
