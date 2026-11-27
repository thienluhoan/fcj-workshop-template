---
title: "Worklog Tuần 11"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

<!-- GHI CHÚ RIÊNG (không hiển thị trên web): tuần này còn ngày ở trạng thái Đang làm/Kế hoạch. Sau khi làm xong, sửa lại nội dung và phần Kết quả cho đúng thực tế. -->

### Mục tiêu tuần 11:

- Đấu nối phần hạ tầng của mình với code của nhóm: Lambda, route API Gateway có Cognito authorizer, frontend trên S3 + CloudFront.
- Chạy thử end-to-end cùng nhóm để có minh chứng demo.
- Soạn bản nháp các phần báo cáo theo hướng dẫn nộp bài: worklog, events, kiến trúc, summary, tự đánh giá, feedback.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Ghi lại worklog ngày/tuần theo form FCAJ (vi/en)<br>- Đọc hướng dẫn nộp Workshop, FCAJ Submission, barem chấm<br>- Đối chiếu ảnh với lịch sự kiện trong nhóm chat<br>- Cập nhật sơ đồ kiến trúc v2, soạn summary .md và bảng chi phí | 23/11/2026 | 23/11/2026 | [FCAJ Rules – Project](https://hcm-rules.awsfcaj.com/project/content)<br>[FCAJ Rules – Submission](https://hcm-rules.awsfcaj.com/instructions/fcaj-submission)<br>[FCAJ – Mẫu worklog](https://workshop-sample.awsfcaj.com/vi/1-worklog/) |
| 3 | - Viết module `infra/terraform/modules/lambda/`<br>&emsp;+ Đóng gói code bằng `archive_file`, runtime Python 3.12<br>&emsp;+ `securetable-menu-handler`, `securetable-orders-handler`<br>&emsp;+ IAM role riêng: chỉ GetItem/PutItem/UpdateItem/Query trên đúng bảng<br>- Thêm test kiểm tra không có `dynamodb:*` | 24/11/2026 | 24/11/2026 | [Terraform – aws_lambda_function](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function)<br>[Terraform – archive_file](https://registry.terraform.io/providers/hashicorp/archive/latest/docs/data-sources/file)<br>[000133 – 2. Create Lambda functions](https://000133.awsfcaj.com/2-create-lambda-functions/)<br>[IAM Security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 4 | - Route `GET/POST /menu` → Lambda Menu<br>- Route `GET/POST /orders`, `PATCH /orders/{id}/pay` → Lambda Orders<br>- Cognito authorizer (trừ `/health`), `aws_lambda_permission`<br>- Kiểm thử bằng Postman: 200 / 401 / 403 | 25/11/2026 | 25/11/2026 | [API Gateway – Lambda proxy integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html)<br>[API Gateway – Cognito authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)<br>[000135 – 2. Config API Gateway](https://000135.awsfcaj.com/2-config-api-gw/)<br>[000135 – 3. Test APIs By Postman](https://000135.awsfcaj.com/3-test-api-by-postman/) |
| 5 | - Script `aws s3 sync dist/` + `cloudfront create-invalidation`<br>- Kiểm tra OAC: truy cập thẳng S3 bị 403<br>- Dashboard `SecureTable-Live-Monitoring`, alarm 5xx → SNS | 26/11/2026 | 26/11/2026 | [AWS CLI – s3 sync](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html)<br>[CloudFront – Invalidation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html)<br>[000085 – 2.3 Creating Alerts with CloudWatch Alarm](https://000085.awsfcaj.com/2-cloudwatch-monitor/2-3-cloudwatch-alarm/)<br>[Terraform – aws_cloudwatch_metric_alarm](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm) |
| 6 | - `deploy_and_test.ps1 -OpenBrowser` cùng cả nhóm<br>- Luồng: đăng nhập (MFA) → gọi món → DynamoDB → bếp thấy đơn<br>- Bật WAF thử payload SQLi cùng thành viên AppSec, chụp 403<br>- `nuke_and_teardown.ps1` sau buổi thử | 27/11/2026 | 27/11/2026 | Kế hoạch cá nhân – Giai đoạn 1<br>[000026 – 3.4 Testing New Rule](https://000026.awsfcaj.com/3-useawswaf/3.4-testingnewrule/)<br>[000135 – 4. Test APIs With Front-end](https://000135.awsfcaj.com/4-test-front-end/) |
| 7 | - Viết lại bài thu hoạch 4 sự kiện theo văn phong của mình<br>- Hoàn thiện Tự đánh giá và Sharing & Feedback<br>- Rà worklog, sửa các ngày kế hoạch theo thực tế | 28/11/2026 | 28/11/2026 | [FCAJ Rules – Nội dung bắt buộc của báo cáo](https://hcm-rules.awsfcaj.com/project/content)<br>[FCAJ – Mẫu Sharing and Feedback](https://workshop-sample.awsfcaj.com/7-feedback/) |

### Kết quả đạt được tuần 11:

- Worklog 12 tuần được ghi lại theo đúng form FCAJ (ngày + tuần, song ngữ).
- Sơ đồ kiến trúc v2 cập nhật từ bản v1.0.0: WAF gắn vào API Gateway, đúng icon IAM, 3 bucket S3 khớp Terraform, thêm AWS Budgets, đánh số 14 bước luồng xử lý.
- Bản nháp file summary .md (Problem, Solution, Workflow, Additional, Cost) và bảng dự toán chi phí theo giá Singapore.
- Module `lambda/` và các route `/menu`, `/orders` sau Cognito authorizer; frontend phục vụ qua CloudFront (OAC).
- Chạy thử end-to-end cùng nhóm, dọn tài nguyên về 0 sau buổi thử.
