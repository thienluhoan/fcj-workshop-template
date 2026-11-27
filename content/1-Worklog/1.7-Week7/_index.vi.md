---
title: "Worklog Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

- Đánh giá lại kiến trúc 3-tier theo chi phí và tiêu chí Serverless của chương trình.
- Thiết kế dữ liệu DynamoDB và cơ chế phân quyền cho hệ thống quản lý nhà hàng.
- Cùng nhóm chốt kế hoạch dự án SecureTable và tái cấu trúc Terraform sang Serverless.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Ước tính chi phí kiến trúc 3-tier bằng AWS Pricing Calculator<br>- Đọc AWS Well-Architected Framework (6 trụ cột)<br>- Đọc Serverless Applications Lens (Security, Cost Optimization) | 26/10/2026 | 26/10/2026 | [AWS Pricing Calculator](https://calculator.aws/)<br>[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)<br>[Serverless Applications Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)<br>[Amazon VPC – Pricing (NAT Gateway)](https://aws.amazon.com/vpc/pricing/) |
| 3 | - Liệt kê access pattern: gọi món, màn hình bếp, kho, thu chi, nhật ký<br>- Thiết kế 5 bảng DynamoDB on-demand + GSI `status_created_at_idx`<br>- Đọc DynamoDB Developer Guide, DynamoDB Guide | 27/10/2026 | 27/10/2026 | [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)<br>[DynamoDB Guide (Alex DeBrie)](https://www.dynamodbguide.com)<br>[000060 – 1.3 Secondary Index](https://000060.awsfcaj.com/1-introduce/1.3-secondaryindexes/) |
| 4 | - Phân tích luồng Cognito → API Gateway → Lambda<br>- Phát hiện khoảng trống: authorizer không phân quyền theo nhóm<br>- Thiết kế 4 nhóm OWNER / MANAGER / STAFF / KITCHEN và quy ước 401 vs 403 | 28/10/2026 | 28/10/2026 | [Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)<br>[API Gateway – Cognito user pool authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)<br>[OWASP Top 10:2025](https://owasp.org/Top10/2025/) |
| 5 | - Cùng nhóm hoàn thiện kế hoạch dự án SecureTable<br>- Đưa code lên repo nhóm (initial commit)<br>- Viết module `dynamodb`, `apigateway`, `waf_v2`<br>- Chuyển module EC2/RDS/VPC vào `legacy_3tier_modules`, tạo `dynamodb_env.example` | 29/10/2026 | 29/10/2026 | Kế hoạch dự án<br>Biến môi trường cho Backend<br>Module Terraform<br>[Terraform – aws_dynamodb_table](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table)<br>[Terraform – aws_api_gateway_rest_api](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/api_gateway_rest_api) |
| 6 | - Tính chi phí WAF: 5 USD/Web ACL + 1 USD/rule → ≈ 9 USD/tháng<br>- Thêm cờ `enable_waf` trong Terraform (mặc định tắt)<br>- Viết `rbac_decorator.py` và `test_rbac_decorator.py` | 30/10/2026 | 30/10/2026 | [AWS WAF – Pricing](https://aws.amazon.com/waf/pricing/)<br>[AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)<br>Code phân quyền<br>[pytest documentation](https://docs.pytest.org) |

### Kết quả đạt được tuần 7:

- Chứng minh kiến trúc 3-tier vượt ngân sách (EC2 + RDS + NAT chạy 24/7 ≈ 20–50 USD/tháng so với trần 10 USD) → quyết định chuyển 100% Serverless.
- Thiết kế 5 bảng DynamoDB on-demand (`orders`, `menu_items`, `inventory`, `daily_finance`, `audit_logs`) và GSI `status_created_at_idx` cho màn hình bếp.
- Phát hiện Cognito authorizer chỉ xác thực, không phân quyền theo nhóm → thiết kế hợp đồng RBAC 4 nhóm và viết `rbac_decorator.py` kèm unit test.
- Nhóm thống nhất kế hoạch SecureTable (4 vai trò, 12 dịch vụ AWS, phân công 4 người, lộ trình 8 tuần); đưa code lên repo nhóm.
- Viết module Terraform `dynamodb`, `apigateway` (REST, `/health`, CORS, throttling 50 rps/burst 100), `waf_v2` với cờ `enable_waf`; chuyển module EC2/RDS/VPC vào `legacy_3tier_modules`.
