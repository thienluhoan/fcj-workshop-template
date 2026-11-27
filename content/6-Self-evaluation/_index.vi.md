---
title: "Tự đánh giá"
date: 2026-11-28
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

Trong thời gian thực tập tại chương trình **First Cloud AI Journey – Công ty TNHH Amazon Web Services Việt Nam** từ ngày 14/09/2026, em đã có cơ hội học hệ thống các dịch vụ AWS qua workshop, rồi áp dụng vào dự án nhóm **SecureTable** với vai trò **Cloud Architecture & DevSecOps Lead**.

Phần việc chính của em:
- Thiết kế kiến trúc serverless.
- Viết toàn bộ hạ tầng bằng Terraform (7 module, 10 test).
- Xây pipeline DevSecOps trên GitHub Actions.
- Viết script triển khai và dọn dẹp tự động.
- Viết module phân quyền RBAC cho backend.
- Giữ chi phí trong ngân sách 10 USD/tháng.

Qua đó em cải thiện kỹ năng Infrastructure as Code, bảo mật đám mây, CI/CD, phân tích chi phí, viết tài liệu kỹ thuật và làm việc nhóm qua pull request.

Để phản ánh khách quan quá trình thực tập, em tự đánh giá theo các tiêu chí dưới đây:

| STT | Tiêu chí | Tốt | Khá | Trung bình | Nhận xét |
| --- | --- | :---: | :---: | :---: | --- |
| 1 | Kiến thức chuyên môn | ✅ | ☐ | ☐ | Nắm được nền tảng AWS (IAM, VPC, EC2, S3, RDS, DynamoDB, Lambda, API Gateway, Cognito, KMS, WAF, CloudWatch) và áp dụng được vào kiến trúc thật. Viết Terraform có module và test, giải thích được vì sao chọn serverless, vì sao bỏ GuardDuty/CMK. |
| 2 | Khả năng tự học & nghiên cứu | ✅ | ☐ | ☐ | Tự học qua các workshop của First Cloud Journey và tài liệu chính thức. Tự xây lộ trình 100 nguồn học Cloud & DevSecOps. Tra AWS Price List để tính chi phí thay vì đoán. |
| 3 | Tính chủ động trong công việc | ✅ | ☐ | ☐ | Chủ động phát hiện bẫy chi phí WAF, khoảng trống phân quyền của Cognito authorizer, chủ động rà soát nền hạ tầng theo barem và vá lỗi trước khi nhóm dựng phần lõi. |
| 4 | Kỷ luật & tuân thủ quy định | ☐ | ✅ | ☐ | Tuân thủ giờ giấc và nội quy văn phòng, tham gia đủ sự kiện. Tuy nhiên có giai đoạn em không ghi worklog đều đặn hằng ngày và phải bổ sung lại sau. |
| 5 | Kỹ năng giao tiếp | ☐ | ✅ | ☐ | Viết tài liệu kỹ thuật và 3 bài blog rõ ràng. Cần cải thiện việc trình bày kiến trúc bằng lời trước người khác và chủ động cập nhật tiến độ cho nhóm thường xuyên hơn. |
| 6 | Làm việc nhóm | ✅ | ☐ | ☐ | Thiết kế "hợp đồng bàn giao" (tên bảng, biến môi trường, decorator RBAC) để Backend/Frontend làm song song. Làm việc qua pull request, không tự merge, chờ nhóm trưởng review. |
| 7 | Năng lực giải quyết vấn đề | ✅ | ☐ | ☐ | Xử lý 6 sự cố trong một ngày: CI đỏ do action bị gỡ tag, test lệch giữa OpenTofu và Terraform, file nhạy cảm bị đẩy lên repo, lộ access key qua ảnh chụp. Mỗi sự cố đều tìm đúng nguyên nhân và ghi lại cách xử lý. |
| 8 | Đóng góp cho Workshop | ✅ | ☐ | ☐ | Phụ trách toàn bộ phần hạ tầng, bảo mật và CI/CD của dự án; sơ đồ kiến trúc, bảng chi phí và file summary của nhóm; đóng góp 3 bài blog. |

### Cần cải thiện
- **Ghi worklog ngay trong ngày** thay vì để dồn rồi viết bù.
- **Trình bày bằng lời**: luyện giải thích kiến trúc ngắn gọn, đi theo luồng nghiệp vụ, trước khi đi vào chi tiết dịch vụ.
- **Cẩn thận với secret**: không chụp màn hình khi thao tác với credential; chuyển hẳn CI sang OIDC để không còn access key dài hạn.
- **Tự kiểm chứng nội dung có AI hỗ trợ**: hiểu và kiểm tra từng dòng trước khi đưa vào repo nhóm.
