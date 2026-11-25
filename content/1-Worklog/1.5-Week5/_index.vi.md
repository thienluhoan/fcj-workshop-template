---
title: "Worklog Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới međây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 5:

- Tìm hiểu và thực hành tự động hóa triển khai hạ tầng dưới dạng mã với **AWS CloudFormation**.
- Thiết lập quản lý danh tính tập trung và Đăng nhập một lần với **AWS IAM Identity Center (SSO)** cho tổ chức.
- Tăng cường bảo mật và kiểm soát quyền hạn IAM thông qua **Permission Boundaries** (Ranh giới quyền).
- Kiểm soát chặt chẽ việc chuyển giao vai trò IAM bằng các điều kiện nâng cao (Role Assumption Conditions).
- Giám sát, đánh giá và kiểm tra các tiêu chuẩn tuân thủ bảo mật hệ thống bằng **AWS Security Hub**.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Khởi tạo cơ sở hạ tầng dưới dạng mã với AWS CloudFormation:**<br>&emsp;+ Tìm hiểu cấu trúc tệp Template (JSON/YAML) trong CloudFormation.<br>&emsp;+ Viết template khởi tạo VPC, Subnet và máy chủ EC2.<br>&emsp;+ Tiến hành tạo Stack trên CloudFormation Console.<br>&emsp;+ Kiểm tra quá trình tạo tài nguyên và thực hành cập nhật/xóa Stack.<br>&emsp;+ Dọn dẹp tài nguyên sau bài thực hành. | 12/10/2026   | 12/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Thiết lập Đăng nhập một lần (AWS SSO / IAM Identity Center) cho Tổ chức:**<br>&emsp;+ Kích hoạt AWS IAM Identity Center trong AWS Organizations.<br>&emsp;+ Tạo người dùng (Users) và nhóm người dùng (Groups) trong Identity Center.<br>&emsp;+ Khởi tạo Permission Sets (gán quyền Administrator Access / Read-Only Access).<br>&emsp;+ Gán Permission Sets và tài khoản AWS tương ứng cho từng Group.<br>&emsp;+ Đăng nhập vào AWS Access Portal để kiểm tra việc truy cập nhiều tài khoản qua SSO. | 13/10/2026   | 13/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Giới hạn quyền người dùng bằng ranh giới quyền IAM (Permission Boundaries):**<br>&emsp;+ Tạo một IAM Policy đóng vai trò là Permission Boundary (ví dụ: giới hạn chỉ cho phép thao tác trên S3 và EC2).<br>&emsp;+ Tạo IAM User/Role mới và gắn Permission Boundary vừa tạo.<br>&emsp;+ Gán thêm quyền `AdministratorAccess` cho IAM User/Role đó.<br>&emsp;+ Thực hành kiểm chứng: Người dùng không thể thực hiện các thao tác ngoài phạm vi Permission Boundary (ví dụ: truy cập DynamoDB) dù có quyền Admin.<br>&emsp;+ Dọn dẹp tài nguyên. | 14/10/2026   | 14/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Hạn chế chuyển giao vai trò theo điều kiện (Role Assumption Conditions):**<br>&emsp;+ Tạo IAM Role để chuyển giao (AssumeRole).<br>&emsp;+ Cấu hình Trust Policy với điều kiện hạn chế như `aws:PrincipalTag`, `aws:SourceIp`, hoặc `ExternalId`.<br>&emsp;+ Thực hành giả định vai trò bằng AWS CLI (`aws sts assume-role`).<br>&emsp;+ Kiểm tra kết quả: Xác nhận lệnh thành công khi thỏa mãn điều kiện và bị từ chối (Access Denied) khi vi phạm điều kiện.<br>&emsp;+ Dọn dẹp tài nguyên. | 15/10/2026   | 15/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Kiểm tra các tiêu chuẩn bảo mật với AWS Security Hub:**<br>&emsp;+ Bật dịch vụ AWS Security Hub trên Console.<br>&emsp;+ Kích hoạt các tiêu chuẩn bảo mật (ví dụ: AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark).<br>&emsp;+ Chạy quá trình đánh giá và xem các phát hiện bảo mật (Findings).<br>&emsp;+ Phân tích các lỗi cấu hình hạ tầng bị cảnh báo và thực hiện các bước khắc phục thử nghiệm.<br>&emsp;+ Tiến hành dọn dẹp tài nguyên. | 16/10/2026   | 16/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được trong tuần 5:

- Nắm vững cách viết template và tự động hóa toàn bộ quy trình khởi tạo, cập nhật, xóa tài nguyên hạ tầng bằng **AWS CloudFormation**.
- Cấu hình thành công **AWS IAM Identity Center (SSO)** để quản lý danh tính tập trung và phân quyền truy cập an toàn cho người dùng trong môi trường AWS đa tài khoản.
- Áp dụng thành công **IAM Permission Boundaries** để thiết lập ranh giới quyền lực tối đa, ngăn chặn việc leo thang đặc quyền khi phân quyền cho developer hoặc người dùng khác.
- Thiết lập các chính sách kiểm soát chuyển giao vai trò khắt khe dựa trên **điều kiện nâng cao (Conditions)**, tăng cường tính bảo mật cho mô hình giả định quyền (AssumeRole).
- Triển khai và làm quen với **AWS Security Hub**, tổng hợp các cảnh báo bảo mật và kiểm tra mức độ tuân thủ theo các tiêu chuẩn bảo mật ngành.
- Dọn dẹp tài nguyên sau các bài thực hành để đảm bảo an toàn và tối ưu chi phí.
