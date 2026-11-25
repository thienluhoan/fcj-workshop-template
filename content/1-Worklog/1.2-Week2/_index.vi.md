---
title: "Worklog Tuần 2"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần:

- Tổng quan kiến thức cần đạt được trong lộ trình học.
- Nắm vững khái niệm và thực hành cơ bản với CloudWatch Rule, Lambda (Serverless), VPC và EC2 Instance.
- Cấu hình Security Group và tự động hóa giám sát, thông báo qua Slack.
- Tham gia sự kiện Kickoff FCAJ Builderathon 2026.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu và đọc qua tài liệu tổng quan.<br>- Tổng hợp kiến thức và xác định các mục tiêu cần đạt được sau khóa học. | 21/09/2026   | 21/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu CloudWatch Rule (đóng vai trò như đồng hồ hẹn giờ/cảm biến sự kiện để kích hoạt Trigger).<br>- Tìm hiểu cơ chế Trigger (nhận lệnh & xử lý theo kiến trúc Serverless, tối ưu chi phí).<br>- Tìm hiểu VPC (Virtual Private Cloud) & EC2 Instances.<br>- **Thực hành:** Tạo thành công máy chủ ảo EC2 trong VPC workflow. | 22/09/2026   | 22/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Thực hành:**<br>&emsp;+ Tạo thành công Security Group trong VPC.<br>&emsp;+ Khởi tạo thành công EC2 instance và tiến hành gán tag (thẻ) quản lý cho instance. | 23/09/2026   | 23/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tạo tài khoản Webhook và tích hợp với Slack.<br>- Tạo IAM Role và hàm AWS Lambda.<br>- Kiểm thử (Testing result) chức năng của Lambda.<br>- Dọn dẹp tài nguyên sau khi hoàn thành.<br>- **Kết quả:** Hoàn thành bài lab tự động tắt máy chủ EC2 và gửi thông báo về Slack bằng AWS Lambda. | 24/09/2026   | 24/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Tham gia sự kiện Kickoff FCAJ Builderathon 2026. | 26/09/2026   | 26/09/2026      | |


### Kết quả đạt được trong tuần:

- Nắm được bức tranh tổng quan về lộ trình kiến thức cần hoàn thành.
- Hiểu được mô hình hoạt động của các dịch vụ Serverless (CloudWatch Events/Rules, AWS Lambda) và cách tối ưu hóa chi phí.
- Khởi tạo và quản lý thành công các tài nguyên mạng & máy chủ cơ bản: **VPC, Security Group, EC2 Instance** (có gắn Tag đầy đủ).
- Xây dựng thành công kịch bản tự động hóa đơn giản: **Tự động bật/tắt EC2 instance theo lịch và gửi thông báo về kênh Slack qua Webhook**.
- Đã thu dọn sạch sẽ các tài nguyên thử nghiệm để tránh phát sinh chi phí thừa.
- Tham gia đầy đủ sự kiện Kickoff FCAJ Builderathon 2026 để nắm thông tin và định hướng cuộc thi.
