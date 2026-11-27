---
title: "Worklog Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

- Nắm các khối Serverless chính: AWS Lambda, Amazon API Gateway, AWS SAM, Amazon Cognito.
- Phân tích thêm khảo sát từ các nhóm người dùng khác và đối chiếu với yêu cầu tốt nghiệp của workshop để chốt hướng đề tài.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Workshop Serverless – Lambda với S3 và DynamoDB:<br>&emsp;+ Lambda resize ảnh khi có object mới trên S3<br>&emsp;+ IAM policy cho Lambda truy cập S3<br>&emsp;+ Lambda ghi dữ liệu vào DynamoDB | 05/10/2026 | 05/10/2026 | [000078 – Lambda + S3](https://000078.awsfcaj.com/2-resize-image-function/)<br>[000078 – Lambda + DynamoDB](https://000078.awsfcaj.com/3-write-data-to-dynaomodb/) |
| 3 | - Viết script Node.js đọc các file khảo sát Excel → JSON<br>- Phân tích từng nhóm người dùng: đầu bếp quán ăn, người nội trợ, phụ huynh làm giáo dục, nông nghiệp<br>- Đề xuất giải pháp nhẹ trước, dự án AWS sau, cách đo hiệu quả | 06/10/2026 | 06/10/2026 | Script đọc khảo sát<br>Bản phân tích |
| 4 | - Đọc yêu cầu tốt nghiệp (mức Junior/Middle)<br>- Lập ma trận yêu cầu → triển khai → minh chứng<br>- So sánh phương án A (quản lý nguyên liệu) và B (EVG)<br>- Kế hoạch 9–10 tuần, khuyến nghị lựa chọn | 07/10/2026 | 07/10/2026 | Yêu cầu workshop<br>Tài liệu đối chiếu<br>[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |
| 5 | - Workshop Serverless with Lambda, API Gateway and SAM:<br>&emsp;+ Deploy frontend và backend bằng SAM<br>&emsp;+ Nạp dữ liệu DynamoDB, kiểm thử cấu hình<br>&emsp;+ Tính năng realtime và xử lý ảnh bằng Lambda | 08/10/2026 | 08/10/2026 | [000066 – Lambda, API Gateway, SAM](https://000066.awsfcaj.com/) |
| 6 | - Workshop Serverless – Authentication with Amazon Cognito:<br>&emsp;+ Tạo User Pool<br>&emsp;+ API + Lambda bảo vệ bằng Cognito authorizer<br>&emsp;+ Kiểm thử đăng nhập từ frontend | 09/10/2026 | 09/10/2026 | [000081 – 2. Create User Pool](https://000081.awsfcaj.com/2-create-user-pool/)<br>[000081 – 3. Create API and Lambda function](https://000081.awsfcaj.com/3-create-api-and-lambda-function/)<br>[000081 – 4. Test with Front-end](https://000081.awsfcaj.com/4-test-front-end/)<br>[AWS Docs – Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)<br>[AWS Docs – Cognito user pool authorizer cho REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html) |

### Kết quả đạt được tuần 4:

- Viết Lambda xử lý ảnh khi có file mới trên S3 và Lambda ghi dữ liệu vào DynamoDB, có IAM policy riêng.
- Triển khai ứng dụng serverless mẫu bằng AWS SAM (API Gateway + Lambda + DynamoDB + frontend).
- Tạo Cognito User Pool và bảo vệ API bằng Cognito authorizer, kiểm thử với frontend.
- Phân tích 3 khảo sát mới (đầu bếp quán ăn, người nội trợ, phụ huynh làm giáo dục, …) bằng script đọc file Excel.
- Lập ma trận *yêu cầu tốt nghiệp → cách triển khai → minh chứng*; chọn hướng **quản lý nguyên liệu/nhà hàng** (phương án A) làm đề tài ưu tiên.
