---
title: "Workshop"
weight: 5
chapter: false
pre: "<b> 5. </b> "
---

# Triển khai Bite to Business lên AWS

Đây là **workshop thực hành về hạ tầng** cho ứng dụng bán thực phẩm **Bite to Business (BTB)**. BTB trực tiếp bán hàng cho cá nhân/hộ gia đình và doanh nghiệp/hợp tác xã; **không phải sàn nhiều người bán**. Backend dùng Spring Boot modular monolith; frontend dùng React/TypeScript.

**Phạm vi của cá nhân:** Amazon VPC, AWS IAM, AWS Budgets, Amazon ECR, Amazon S3, AWS Secrets Manager, Application Load Balancer, Amazon ECS với AWS Fargate, AWS Certificate Manager, Amazon CloudFront, Amazon Route 53, AWS WAF và Amazon CloudWatch Logs.

**Không hướng dẫn triển khai phần bạn cùng nhóm:** Amazon Bedrock, SQS, SES, CodePipeline/CodeBuild, CloudWatch Metrics/Alarms, CloudTrail, NAT Gateway và VPC endpoints. Các dịch vụ này chỉ xuất hiện trong **sơ đồ tổng thể** để giải thích kết nối.

**Documentation status / Trạng thái tài liệu:** Draft instructions. Resource names, values, test results, demo links and screenshots must be replaced with real evidence before submission. / Bản nháp hướng dẫn. Cần thay tên tài nguyên, thông số, kết quả kiểm thử, link demo và ảnh chụp bằng minh chứng thực tế trước khi nộp.

## Mục lục

1. [Tổng quan](5.1-overview/)
2. [Chuẩn bị](5.2-prerequisites/)
3. [Kiến trúc và luồng dữ liệu](5.3-architecture/)
4. [Thiết lập nền tảng](5.4-foundation/)
5. [Container và lưu trữ](5.5-containers-storage/)
6. [Triển khai Backend](5.6-backend/)
7. [Phân phối web và bảo mật](5.7-web-security/)
8. [Nhật ký và xử lý lỗi](5.8-logging/)
9. [Kiểm thử, xác nhận và demo](5.9-testing/)
10. [Dự toán và tối ưu chi phí](5.10-cost/)
11. [Dọn dẹp tài nguyên](5.11-cleanup/)

**Trước khi nộp:** Tự vẽ và kiểm tra sơ đồ AWS bằng icon chính thức; trực tiếp thực hành, ghi lại ảnh AWS Console, output lệnh, kết quả đo, bảng dự toán chi phí và minh chứng demo. Bản nháp và workshop mẫu không thay thế cho triển khai thực tế.
