---
title: "Bản đề xuất"
date: 2026-10-18
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# SecureTable – Restaurant Management System on AWS

## Hệ thống quản lý nguyên liệu và gọi món bằng mã QR trên nền tảng AWS Serverless

---

### 1. Tổng quan dự án

**SecureTable** là hệ thống quản lý nhà hàng quy mô nhỏ, tập trung vào việc gọi món, quản lý nguyên liệu, hóa đơn và theo dõi doanh thu. Khách hàng quét mã QR cố định tại bàn để xem thực đơn và đặt món; nhân viên sử dụng các giao diện riêng để xử lý nghiệp vụ.

Hệ thống hướng đến mô hình **Serverless** trên AWS nhằm giảm công việc vận hành máy chủ và thuận tiện cho việc mở rộng. Dự án được triển khai thử nghiệm tại **AWS Region Asia Pacific (Singapore) – `ap-southeast-1`**.

**Nhóm người dùng:**

- **Guest:** Quét QR, xem thực đơn, gọi món không cần tài khoản.
- **Reception:** Quản lý bàn, mở và đóng hóa đơn.
- **Kitchen:** Xác nhận hoàn tất hoặc từ chối món.
- **Inventory:** Nhập kho, cập nhật và điều chỉnh nguyên liệu.
- **Admin:** Xem báo cáo tài chính và lịch sử thao tác.

### 2. Mục tiêu dự án

- Xây dựng website quản lý nhà hàng hoạt động trên AWS.
- Đồng bộ dữ liệu giữa đặt món, bếp, kho và hóa đơn.
- Theo dõi nguyên liệu nhập, đặt giữ, tiêu thụ và thất thoát.
- Phân quyền nhân viên; ghi nhận các thao tác quan trọng để truy vết.
- Tự động hóa báo cáo, sao lưu và giám sát hệ thống.
- Áp dụng các nguyên tắc **AWS Well-Architected** về bảo mật, độ tin cậy, hiệu năng, vận hành và chi phí.

### 3. Vấn đề cần giải quyết

Ở nhiều quán ăn nhỏ, việc ghi chép nguyên liệu, tiếp nhận đơn hàng và tổng hợp doanh thu còn phụ thuộc vào sổ sách hoặc công cụ riêng lẻ. Điều này dễ gây sai lệch tồn kho, chậm truyền đơn đến bếp và khó kiểm tra các thay đổi dữ liệu.

**Giải pháp:** SecureTable sử dụng một hệ thống dữ liệu tập trung. Khi khách đặt món, thông tin được gửi đến bếp và liên kết với lượng nguyên liệu đặt giữ. Khi món được xử lý và hóa đơn được đóng, dữ liệu tồn kho và tài chính được cập nhật theo quy tắc nghiệp vụ. Người quản lý có thể tra cứu báo cáo và Audit Logs thay vì đối chiếu thủ công.

### 4. Kiến trúc giải pháp

![Architecture](../../images/2-Proposal/SecureTable_Architecture.svg)

| Dịch vụ AWS | Vai trò |
|---|---|
| **Amazon S3 + Amazon CloudFront** | Lưu trữ và phân phối giao diện web qua HTTPS. |
| **Amazon API Gateway** | Tiếp nhận và định tuyến yêu cầu API. |
| **AWS Lambda** | Xử lý GuestServices, Ordering, Reception, Kitchen, Inventory, AdminDashboard, Report và Backup. |
| **Amazon DynamoDB** | Lưu trữ bàn, bill, đơn hàng, nguyên liệu, tài chính và audit. |
| **Amazon Cognito + AWS IAM** | Xác thực nhân viên và giới hạn quyền truy cập tài nguyên. |
| **Amazon EventBridge Scheduler + Amazon SNS** | Lập lịch báo cáo, backup và gửi thông báo email. |
| **Amazon CloudWatch + AWS CloudTrail** | Giám sát, cảnh báo và theo dõi hoạt động trên AWS. |
| **AWS WAF** | Lọc một số loại request web bất thường (nếu được bật trong bản triển khai). |

**Các luồng chính:**

1. **Guest:** Quét QR → CloudFront → API Gateway → GuestServices/Ordering → DynamoDB. Guest Session chỉ hợp lệ khi bill tương ứng đang mở.
2. **Staff:** Đăng nhập qua Cognito → gọi API Gateway → Lambda theo nghiệp vụ → DynamoDB.
3. **Kitchen/Inventory:** Xử lý món hoặc nguyên liệu; cập nhật dữ liệu bằng các điều kiện và transaction khi cần duy trì tính nhất quán.
4. **Automation:** EventBridge Scheduler → Report/Backup Lambda → SNS hoặc xuất dữ liệu DynamoDB sang S3.

**Định hướng Well-Architected:** Dùng IAM theo quyền tối thiểu, HTTPS và Cognito cho bảo mật; DynamoDB Transactions và backup cho độ tin cậy; CloudWatch cho vận hành; Lambda và DynamoDB cho khả năng mở rộng theo nhu cầu; theo dõi sử dụng tài nguyên để tối ưu chi phí.

### 5. Lộ trình triển khai

| Giai đoạn | Nội dung |
|---|---|
| **Tuần 1–2** | Phân tích yêu cầu, thiết kế kiến trúc và dữ liệu DynamoDB. |
| **Tuần 3–4** | Triển khai frontend S3/CloudFront, Cognito, API Gateway và các Lambda chính. |
| **Tuần 5–6** | Hoàn thiện Ordering, Kitchen, Inventory, Reception, Finance/Audit và báo cáo. |
| **Tuần 7–8** | Bổ sung monitoring, bảo mật, backup; kiểm thử và chuẩn bị demo, tài liệu Workshop. |

### 6. Ngân sách dự kiến

Ngân sách được lập cho **01 tháng vận hành thử nghiệm và demo** tại AWS Region Singapore (`ap-southeast-1`), với mô hình Serverless và lưu lượng thấp. Các số tiền dưới đây là **dự toán kế hoạch**, chưa phải hóa đơn thực tế hoặc kết quả được xác nhận từ AWS Pricing Calculator.

#### 6.1. Giả định lập dự toán

| Thông số | Giả định |
|---|---|
| Quy mô nhà hàng | 10 bàn, khoảng 20–30 hóa đơn/ngày |
| Lưu lượng API | Khoảng 40.000 REST API requests/tháng |
| Xử lý backend | Khoảng 60.000 Lambda invocations/tháng, chủ yếu 256 MB |
| Dữ liệu | Khoảng 1 GB DynamoDB |
| Báo cáo và sao lưu | Báo cáo định kỳ; backup khoảng 13 bảng/tuần |
| CloudFront & WAF | Giả định dùng **CloudFront Free Plan**, WAF nằm trong quyền lợi gói |

#### 6.2. Bảng dự toán chi phí hàng tháng

| STT | Dịch vụ AWS | Cơ sở ước tính | Chi phí (USD/tháng) |
|:---:|---|---|---:|
| 1 | Amazon CloudFront & S3 | Frontend tĩnh; Free Plan, dự phòng lưu trữ/request S3 ngoài gói | **$0.50** |
| 2 | Amazon API Gateway (REST) | 40.000 request/tháng | **$0.20** |
| 3 | AWS Lambda | 60.000 invocation/tháng, memory chủ yếu 256 MB | **$0.30** |
| 4 | Amazon DynamoDB | Khoảng 1 GB dữ liệu và read/write lưu lượng thấp | **$1.00** |
| 5 | Amazon Cognito | Số lượng tài khoản nhân viên ít, giả định đủ điều kiện miễn phí | **$0.00** |
| 6 | Amazon CloudWatch | Logs dung lượng thấp và một số standard alarms | **$1.00** |
| 7 | EventBridge Scheduler & SNS | Lịch báo cáo/backup và email thông báo | **$0.10** |
| 8 | AWS WAF | Nằm trong CloudFront Free Plan theo giả định | **$0.00** |
| 9 | DynamoDB PITR, Export & S3 Backup | Dự phòng PITR, xuất dữ liệu hàng tuần và lưu trữ bản sao | **$2.50** |
| 10 | AWS CloudTrail | Chỉ sử dụng Event History mặc định | **$0.00** |
| | **Tổng chi phí dịch vụ** | | **$5.60** |
| | **Dự phòng phát sinh** | | **$2.00** |
| | **Tổng ngân sách dự kiến** | | **$7.60/tháng** |

**Tổng ngân sách dự kiến trong 12 tháng: $91.20**, nếu duy trì cùng cấu hình và mức sử dụng. Số tiền thực tế có thể thay đổi theo mức sử dụng, dung lượng từng bảng, AWS Free Tier/credits và tính năng bật thêm. Nếu WAF được dùng theo cơ chế tính phí độc lập, cần cộng phí WAF riêng.
Tham khảo: [AWS Pricing Calculator](https://calculator.aws/).

### 7. Rủi ro và biện pháp giảm thiểu

| Rủi ro | Hướng xử lý |
|---|---|
| QR tĩnh bị sao chép và dùng để đặt món từ xa | Giới hạn session theo bill đang mở; Reception kiểm tra đơn tranh chấp. Xác thực bổ sung là hướng phát triển tiếp theo. |
| Đặt món đồng thời gây sai lệch tồn kho | Dùng DynamoDB Transactions, điều kiện cập nhật và idempotency cho thao tác quan trọng. |
| Lambda/API lỗi hoặc timeout | Theo dõi CloudWatch, cảnh báo và cho phép thử lại các tác vụ được thiết kế an toàn. |
| Rò rỉ quyền truy cập | Cognito cho nhân viên, IAM theo quyền tối thiểu và kiểm tra quyền tại backend. |
| Mất dữ liệu hoặc vượt ngân sách | Sao lưu định kỳ, thử khôi phục; dùng AWS Budgets và rà soát tài nguyên. |

### 8. Kết quả kỳ vọng

SecureTable hướng tới một **MVP có thể demo trọn vẹn** quy trình: Reception mở bàn → khách quét QR và gọi món → Kitchen xử lý → Inventory cập nhật nguyên liệu → Reception đóng bill → Admin xem báo cáo và lịch sử thao tác.

Về kỹ thuật, dự án cần chứng minh các AWS service phối hợp đúng theo sơ đồ, dữ liệu nghiệp vụ được cập nhật nhất quán, có kiểm soát truy cập, monitoring, báo cáo và sao lưu. Về lâu dài, hệ thống có thể mở rộng thêm thanh toán trực tuyến, nhiều chi nhánh và cơ chế xác minh QR chặt chẽ hơn.
