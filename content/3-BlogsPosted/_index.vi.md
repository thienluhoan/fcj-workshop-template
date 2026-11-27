---
title: "Các bài blogs đã đăng"
date: 2026-11-18
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Nhóm đã viết 3 bài blog kỹ thuật và đăng lên cộng đồng **AWS Study Group**. Cả 3 bài đều lấy từ những vấn đề thật gặp phải khi làm lab và dự án SecureTable: upload file trong ứng dụng serverless, chi phí NAT Gateway khi học lab VPC, và sự cố lộ access key khi cấu hình CI/CD. Mỗi bài có sơ đồ kiến trúc, cách tính chi phí theo giá region Singapore, các lỗi hay gặp và cách kiểm chứng.

### [Blog 1 - Ảnh 5 MB mà upload không được? Đừng bắt Lambda đi "khuân" file](3.1-blog1/)

Bài này giải thích vì sao không nên để file đi qua API Gateway và Lambda: API Gateway giới hạn payload 10 MB, Lambda giới hạn 6 MB, còn mã hoá base64 làm file phình thêm khoảng 33%. Cách làm được khuyến nghị là dùng **S3 Presigned URL**. Backend chỉ ký một URL ngắn hạn cho đúng một key, client upload thẳng lên S3, sau đó Lambda kiểm tra nội dung file qua sự kiện S3. Bài có đoạn code ký presigned POST giới hạn dung lượng, so sánh chi phí, nhắc dùng lifecycle rule dọn file mồ côi và multipart upload dang dở, và liệt kê 7 lỗi hay gặp. [Xem bài gốc trên AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2341282956636663)

### [Blog 2 - Hoá đơn AWS có dòng "NatGateway-Bytes" to bất thường? Có thể bạn đang trả tiền để gọi S3](3.2-blog2/)

Bài này phân tích vì sao traffic từ private subnet tới S3, DynamoDB và ECR đi vòng qua NAT Gateway và bị tính 0,059 USD/GB. Thêm **Gateway Endpoint** (miễn phí) vào route table là cắt được khoản phí này mà không phải sửa code. Bài cũng tính điểm hoà vốn của Interface Endpoint (khoảng 390 GB/tháng khi đặt ở 2 AZ), nêu các lưu ý về endpoint policy, ECR và multi-AZ, và cách kiểm chứng bằng Cost Explorer, CloudWatch và VPC Flow Logs. [Xem bài gốc trên AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2341791103252515)

### [Blog 3 - Vẫn dán access key AWS vào GitHub Secrets? Có cách để CI không phải giữ key nào](3.3-blog3/)

Bài này hướng dẫn thay access key dài hạn bằng **OIDC**. GitHub Actions xin token, AWS STS kiểm tra trust policy rồi cấp credential tạm thời có hạn 1 giờ. Bài nhấn mạnh lỗi nguy hiểm nhất là trust policy chỉ kiểm tra `aud` mà quên `sub`, vì khi đó repo bất kỳ cũng có thể assume role. Ngoài ra bài nêu các bẫy hay gặp (environment làm đổi `sub`, định dạng `sub` mới, pull request) và cách kiểm chứng bằng CloudTrail. [Xem bài gốc trên AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2342465539851738)
