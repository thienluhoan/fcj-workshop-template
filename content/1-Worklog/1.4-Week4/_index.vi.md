---
title: "Worklog Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}


### Mục tiêu tuần 4:

- Hiểu và thực hành quản lý tài nguyên AWS bằng **Resource Tags** và **Resource Groups**.
- Phân quyền chi tiết trên AWS IAM dựa trên Tag (Attribute-Based Access Control - ABAC) cho EC2 instance.
- Quản lý bản vá lỗi (Patch Management) và tự động hóa điều khiển máy chủ Windows qua **AWS Systems Manager (SSM)**.
- Kết nối bảo mật và quản lý phiên làm việc không cần mở cổng SSH/RDP public thông qua **AWS SSM Session Manager** (Session Logging & Port Forwarding).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu lý thuyết về quản lý tài nguyên AWS bằng thẻ (Tags) và Nhóm tài nguyên (Resource Groups). | 05/10/2026   | 05/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Thực hành với Resource Tags:**<br>&emsp;+ Khởi tạo phiên bản EC2 kèm theo Tags.<br>&emsp;+ Quản lý, truy vấn và lọc tài nguyên theo Tag trên Console & AWS CLI.<br>&emsp;+ Tạo Resource Groups để gom nhóm tài nguyên.<br>&emsp;+ Dọn dẹp tài nguyên thử nghiệm. | 06/10/2026   | 06/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Quản lý quyền truy cập thẻ tài nguyên EC2 bằng AWS IAM:**<br>&emsp;+ Khởi tạo IAM Users, IAM Roles và phân tích các IAM Policy.<br>&emsp;+ **Kiểm tra Policy chi tiết:**<br>&emsp;&emsp;• `ec2-list-read`: Chỉ đọc thông tin EC2 tại region `us-east-1` & `us-west-1`.<br>&emsp;&emsp;• `ec2-create-tags`: Cho phép gắn tag khi khởi tạo EC2.<br>&emsp;&emsp;• `ec2-create-tags-existing`: Gắn tag trên instance có sẵn với điều kiện `Key=Team`, `Value=Alpha`.<br>&emsp;&emsp;• `ec2-run-instances`: Tạo EC2 tại `us-east-1`/`us-west-1` bắt buộc tag `Team=Alpha`.<br>&emsp;&emsp;• `ec2-manage-instances`: Thao tác Start/Stop/Reboot/Terminate EC2 tại `us-east-1`/`us-west-1` có tag `Team=Alpha`.<br>&emsp;+ Dọn dẹp các tài nguyên lab. | 07/10/2026   | 07/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Quản lý bản vá lỗi và chạy lệnh trên máy chủ Windows với AWS Systems Manager:**<br>&emsp;+ Thiết lập VPC, Subnet và khởi tạo 2 máy chủ EC2 Windows (`Windows-Lab-SSM-1`, `Windows-Lab-SSM-2`).<br>&emsp;+ Gán IAM Role cho EC2 để đăng ký Managed Nodes trên Fleet Manager.<br>&emsp;+ Kiểm tra kết nối Terminal Session thành công trên cả 2 node.<br>&emsp;+ Thực thi Patch Manager với chế độ `Scan and install`, tùy chọn `Do not reboot my instances`.<br>&emsp;+ Xác nhận kết quả thực thi patch thành công trên AWS Console & dọn dẹp tài nguyên. | 08/10/2026   | 08/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Làm việc với Amazon Systems Manager - Session Manager:**<br>&emsp;+ **Lưu trữ Log:** Tạo S3 Bucket, cấu hình Session Manager ghi log hoạt động lệnh, gán IAM Role ghi log vào S3 và kiểm tra dữ liệu nhật ký.<br>&emsp;+ **Port Forwarding:** Tạo IAM User `Portfwd` (quyền `AmazonSSMFullAccess`), cài đặt AWS CLI & Session Manager Plugin trên Windows client; thực thi lệnh `aws ssm start-session` chuyển tiếp cổng RDP 3389 sang cổng `localhost:9999` để truy cập EC2 Windows trong Private Subnet.<br>&emsp;+ Xác nhận lịch sử phiên trong Session History & dọn dẹp tài nguyên. | 09/10/2026   | 09/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt međược trong tuần 4:

- Làm chủ kỹ thuật quản lý và lọc tài nguyên qua **Resource Tags & Resource Groups** trên cả Console và AWS CLI.
- Thiết lập thành công mô hình ủy quyền **ABAC (Attribute-Based Access Control)** với IAM Policy, giới hạn chính xác thao tác quản lý EC2 dựa theo Condition Tag (`Team=Alpha`) và Region (`us-east-1`, `us-west-1`).
- Quản lý tập trung các máy chủ Windows EC2 thông qua **AWS SSM Fleet Manager & Patch Manager**, thực thi thành công việc quét và cài đặt bản vá hệ điều hành không cần khởi động lại máy chủ.
- Triển khai giải pháp kết nối an toàn không phụ thuộc vào IP Public / mở port Bastion Host:
  - Tự động ghi nhận và lưu trữ toàn bộ lịch sử câu lệnh phiên (Session Logs) lên **Amazon S3**.
  - Thực hiện **Port Forwarding** thành công, giúp kết nối Remote Desktop (RDP) đến EC2 Windows trong Private Subnet thông qua cổng local `localhost:9999`.
- Thực hiện thu hồi quyền và dọn dẹp đầy đủ tài nguyên sau các buổi thực hành.
