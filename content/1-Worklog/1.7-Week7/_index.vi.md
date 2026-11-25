---
title: "Worklog Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 7:

- Tự động hóa quy trình sao lưu và khôi phục dữ liệu tập trung với **AWS Backup**.
- Thiết lập kết nối mạng riêng giữa các VPC bằng **VPC Peering**.
- Quản lý và định tuyến mạng tập trung quy mô lớn bằng **AWS Transit Gateway**.
- Đóng gói ứng dụng thành Container với **Docker** và chạy thử nghiệm trên môi trường máy chủ.
- Triển khai và quản lý ứng dụng containerized nâng cao trên **Amazon Elastic Container Service (AWS ECS)**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Triển khai kế hoạch sao lưu hệ thống với AWS Backup:**<br>&emsp;+ Khởi tạo Backup Vault và thiết lập khóa mã hóa AWS KMS cho vault.<br>&emsp;+ Tạo Backup Plan: Cấu hình tần suất sao lưu, thời gian lưu trữ (Retention) và lifecycle.<br>&emsp;+ Khởi tạo Resource Assignment để tự động backup tài nguyên (EBS, RDS, EC2).<br>&emsp;+ Thực thi On-demand Backup Job và chạy thử nghiệm Restore Job để kiểm tra dữ liệu.<br>&emsp;+ Dọn dẹp các bản sao lưu thử nghiệm. | 26/10/2026   | 26/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Kết nối các đám mây riêng ảo (VPC) với tính năng ghép nối VPC (VPC Peering):**<br>&emsp;+ Khởi tạo 2 VPC riêng biệt (VPC A và VPC B) với CIDR không trùng lặp.<br>&emsp;+ Tạo VPC Peering Connection và chấp nhận yêu cầu kết nối.<br>&emsp;+ Cấu hình lại Route Tables ở 2 VPC để định tuyến lưu lượng qua Peering Connection.<br>&emsp;+ Cập nhật Security Groups cho phép giao tiếp nội bộ giữa 2 VPC.<br>&emsp;+ Kiểm tra kết nối ping/SSH giữa 2 máy chủ EC2 ở 2 VPC và dọn dẹp tài nguyên. | 27/10/2026   | 27/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Quản lý tập trung các kết nối với AWS Transit Gateway:**<br>&emsp;+ Khởi tạo AWS Transit Gateway (TGW).<br>&emsp;+ Tạo VPC Attachments để kết nối nhiều VPC vào Transit Gateway.<br>&emsp;+ Cấu hình Transit Gateway Route Tables để điều phối lưu lượng giữa các mạng.<br>&emsp;+ Cập nhật Subnet Route Tables của các VPC hướng traffic về Transit Gateway.<br>&emsp;+ Kiểm thử kết nối liên thông giữa các VPC và tiến hành dọn dẹp tài nguyên. | 28/10/2026   | 28/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Triển khai ứng dụng bằng Docker:**<br>&emsp;+ Cài đặt Docker Engine và Docker Compose trên máy chủ EC2 Linux.<br>&emsp;+ Viết `Dockerfile` để đóng gói ứng dụng web mẫu.<br>&emsp;+ Thực hành các lệnh Docker cơ bản: `docker build`, `docker run`, `docker ps`, `docker logs`.<br>&emsp;+ Đẩy (Push) Docker Image lên container registry và kiểm tra ứng dụng chạy trên cổng container.<br>&emsp;+ Dọn dẹp container và image thử nghiệm. | 29/10/2026   | 29/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Triển khai ứng dụng lên Amazon Elastic Container Service (Amazon ECS):**<br>&emsp;+ Khởi tạo Amazon ECR (Elastic Container Registry) và push Docker Image lên ECR.<br>&emsp;+ Khởi tạo ECS Cluster (sử dụng AWS Fargate / EC2 launch type).<br>&emsp;+ Định nghĩa Task Definition (cấu hình CPU, RAM, Container Image, Port Mapping).<br>&emsp;+ Tạo và chạy ECS Service để tự động duy trì số lượng Task.<br>&emsp;+ Kiểm tra khả năng truy cập ứng dụng qua Load Balancer/Public IP và dọn dẹp tài nguyên. | 30/10/2026   | 30/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 7:

- Làm chủ công cụ **AWS Backup**, tự động hóa toàn bộ chiến lược sao lưu và kiểm thử quy trình khôi phục dữ liệu an toàn.
- Thiết lập thành công kết nối **VPC Peering**, hiểu rõ cơ chế định tuyến bảng Route Table và phân quyền Security Group liên VPC.
- Triển khai mô hình kết nối tập trung với **AWS Transit Gateway**, đơn giản hóa việc quản lý mạng cho kiến trúc đa VPC.
- Đóng gói ứng dụng thành công bằng **Docker**, làm quen với quy trình xây dựng image và quản lý container trên Linux.
- Triển khai hoàn chỉnh ứng dụng Containerized lên **Amazon ECS** kết hợp với **Amazon ECR**, nắm vững các khái niệm ECS Cluster, Task Definition và ECS Service.
- Dọn dẹp tài nguyên sạch sẽ sau mỗi bài thực hành để đảm bảo không phát sinh chi phí AWS ngoài ý muốn.
