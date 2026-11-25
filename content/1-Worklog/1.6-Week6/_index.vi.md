---
title: "Worklog Tuần 6"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 6:

- Bảo vệ ứng dụng web và API trước các mối đe dọa an ninh mạng bằng **AWS WAF**.
- Quản lý mã hóa dữ liệu tập trung và quản lý vòng đời khóa mã hóa bằng **AWS KMS**.
- Tự động hóa quy trình sao lưu và phục hồi dữ liệu hệ thống với **AWS Backup**.
- Thiết lập kết nối mạng trực tiếp giữa hai VPC bằng **VPC Peering**.
- Triển khai mô hình kết nối mạng tập trung quy mô lớn bằng **AWS Transit Gateway**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Bảo vệ ứng dụng và API bằng Tường lửa ứng dụng web (AWS WAF):**<br>&emsp;+ Khởi tạo Web ACL trong AWS WAF.<br>&emsp;+ Cấu hình các bộ quy tắc (Managed Rule Groups) phòng chống SQL Injection, Cross-Site Scripting (XSS), và Rate-based rules.<br>&emsp;+ Gán Web ACL vào Application Load Balancer (ALB) hoặc API Gateway.<br>&emsp;+ Kiểm thử khả năng chặn truy cập bất thường và xem logs giám sát.<br>&emsp;+ Dọn dẹp tài nguyên thử nghiệm. | 19/10/2026   | 19/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Quản lý khóa với Dịch vụ Quản lý Khóa (AWS KMS):**<br>&emsp;+ Tạo Customer Managed Key (CMK) trong AWS KMS.<br>&emsp;+ Thiết lập KMS Key Policy để phân quyền sử dụng và quản trị khóa.<br>&emsp;+ Thực hành mã hóa / giải mã dữ liệu trên S3 Bucket và ổ đĩa EBS volume bằng KMS Key.<br>&emsp;+ Kiểm tra nhật ký truy cập khóa trong AWS CloudTrail.<br>&emsp;+ Dọn dẹp tài nguyên. | 20/10/2026   | 20/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Triển khai kế hoạch sao lưu hệ thống với AWS Backup:**<br>&emsp;+ Khởi tạo Backup Vault và cấu hình phương thức mã hóa cho bản sao lưu.<br>&emsp;+ Tạo Backup Plan: Định nghĩa lịch backup (Schedule), thời gian lưu trữ (Retention period) và quy tắc lifecycle.<br>&emsp;+ Gán tài nguyên (Resource Assignment) cần backup như EBS, RDS, EC2.<br>&emsp;+ Thực thi tác vụ sao lưu (Backup Job) thủ công để kiểm thử.<br>&emsp;+ Thực hành khôi phục (Restore Job) thử nghiệm và kiểm tra kết quả.<br>&emsp;+ Dọn dẹp tài nguyên. | 21/10/2026   | 21/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Kết nối các đám mây riêng ảo (VPC) với tính năng ghép nối VPC (VPC Peering):**<br>&emsp;+ Khởi tạo 2 VPC riêng biệt (VPC A và VPC B) với dải CIDR không chồng lấn.<br>&emsp;+ Tạo VPC Peering Connection và chấp nhận yêu cầu kết nối (Accept Peering Request).<br>&emsp;+ Cập nhật Route Table ở cả 2 VPC để định tuyến lưu lượng qua Peering Connection.<br>&emsp;+ Cấu hình Security Group cho phép lưu lượng qua lại giữa 2 VPC.<br>&emsp;+ Kiểm tra kết nối ICMP/SSH giữa 2 máy chủ EC2 ở 2 VPC và dọn dẹp tài nguyên. | 22/10/2026   | 22/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Quản lý tập trung các kết nối với AWS Transit Gateway:**<br>&emsp;+ Khởi tạo AWS Transit Gateway (TGW).<br>&emsp;+ Đính kèm các VPC (VPC Attachments) vào Transit Gateway.<br>&emsp;+ Cấu hình Transit Gateway Route Tables để kiểm soát luồng giao thông mạng giữa các VPC.<br>&emsp;+ Cập nhật Subnet Route Tables của các VPC hướng về Transit Gateway.<br>&emsp;+ Kiểm thử kết nối liên thông đa VPC và tiến hành dọn dẹp tài nguyên. | 23/10/2026   | 23/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 6:

- Triển khai thành công **AWS WAF** để bảo vệ ứng dụng web khỏi các kiểu tấn công phổ biến (SQLi, XSS, Brute-force).
- Nắm vững cơ chế mã hóa dữ liệu an toàn với **AWS KMS**, biết cách phân quyền Key Policy và kiểm vết thao tác qua CloudTrail.
- Xây dựng chiến lược sao lưu tập trung, tự động hóa quy trình Backup & Restore bằng **AWS Backup** giúp đảm bảo tính sẵn sàng của dữ liệu.
- Làm chủ kỹ thuật thiết lập kết nối **VPC Peering**, hiểu rõ cách cấu hình Route Table và Security Group để giao tiếp giữa các VPC.
- Triển khai thành công **AWS Transit Gateway**, đơn giản hóa sơ đồ mạng phức tạp và quản lý tập trung lưu lượng cho mô hình nhiều VPC.
- Thực hiện thu dọn toàn bộ tài nguyên thử nghiệm sau các bài thực hành để tối ưu hóa chi phí AWS.
