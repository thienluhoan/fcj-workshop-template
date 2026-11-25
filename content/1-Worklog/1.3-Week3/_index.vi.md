---
title: "Worklog Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 3:

- Tìm hiểu tổng quan và cách sử dụng cơ bản công cụ giám sát Grafana.
- Tham dự sự kiện công nghệ AWS Cloud and AI Day.
- Thiết lập hạ tầng mạng & máy chủ EC2 Linux với phân quyền IAM chi tiết.
- Cài đặt, cấu hình và khởi chạy thành công Grafana trên máy chủ EC2 Linux.
- Tích hợp CloudWatch với Grafana để giám sát hệ thống và dọn dẹp tài nguyên sau bài lab.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu tổng quan và học cách sử dụng cơ bản công cụ Grafana. | 28/09/2026   | 28/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tham dự sự kiện AWS Cloud and AI Day. | 29/09/2026   | 29/09/2026      | |
| 4   | - **Thực hành:**<br>&emsp;+ Tạo VPC và Subnet cho máy chủ EC2 Linux.<br>&emsp;+ Tạo Security Group (nhóm bảo mật) và khởi tạo EC2 Linux.<br>&emsp;+ Tạo IAM User và IAM Role.<br>&emsp;+ Gán IAM Role cho EC2 instance. | 30/09/2026   | 30/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Cài đặt Grafana:**<br>&emsp;+ Kết nối với máy chủ EC2 Linux bằng PuTTY.<br>&emsp;+ Thêm YUM repository để tải Grafana, cấu hình và kiểm tra nội dung tệp `grafana.repo`.<br>&emsp;+ Tiến hành cài đặt và khởi chạy Grafana.<br>&emsp;+ Truy cập giao diện web Grafana qua Public IPv4 trên cổng 3000.<br>- **Kết quả:** Đăng nhập thành công vào giao diện quản trị Grafana. | 01/10/2026   | 01/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Giám sát bằng Grafana:**<br>&emsp;+ Thực hành kết nối Grafana với AWS CloudWatch (Data Source).<br>&emsp;+ Xây dựng dashboard giám sát tài nguyên.<br>&emsp;+ Dọn dẹp các tài nguyên đã tạo sau khi hoàn thành bài thực hành. | 02/10/2026   | 02/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 3:

- Cập nhật kiến thức mới từ sự kiện **AWS Cloud and AI Day**.
- Nắm vững quy trình khởi tạo VPC, Subnet, Security Group và phân quyền **IAM Role cho EC2 Instance** để tương tác với các dịch vụ AWS an toàn.
- Thành thạo việc kết nối SSH qua **PuTTY**, thêm Repo và cài đặt dịch vụ trên môi trường Linux.
- Đã truy cập thành công Grafana UI qua cổng **3000** sử dụng Public IPv4.
- Tích hợp thành công **CloudWatch làm Data Source cho Grafana** để thực hiện giám sát chỉ số hạ tầng (metrics).
- Dọn dẹp tài nguyên AWS đúng quy trình sau khi lab xong để tránh chi phí phát sinh.
