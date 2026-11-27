---
title: "Nhật ký công việc"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

Worklog này ghi lại quá trình thực tập 12 tuần (từ 14/09/2026, ghi tới ngày nộp bài 30/11/2026) của tôi trong chương trình **First Cloud Journey (FCJ)**. Tôi đảm nhận vai trò **Cloud Architecture & DevSecOps Lead (Thành viên 3)** trong nhóm 4 người xây dựng dự án **SecureTable – hệ thống quản lý nhà hàng an toàn trên AWS**.

**Cách tôi thực hiện worklog:** mỗi ngày ghi lại việc đã làm/đang làm, nội dung chi tiết, nguồn tài liệu (link tới đúng bài lab/tài liệu) và bài học rút ra; cuối tuần tổng hợp thành trang worklog tuần theo mẫu: mục tiêu tuần, bảng công việc theo ngày và kết quả đạt được.

**Hành trình gồm 3 chặng:**

- **Tuần 1–5:** học nền tảng AWS qua các workshop FCJ (tài khoản, chi phí, IAM, VPC, EC2, S3, RDS, DynamoDB, Lambda, API Gateway, Cognito, KMS, WAF…) song song với khảo sát thực tế để chọn đề tài.
- **Tuần 6–8:** dựng hạ tầng bằng Terraform, chuyển kiến trúc từ 3-tier sang 100% Serverless, rà soát bảo mật và đưa pipeline DevSecOps (gitleaks, Checkov, Trivy, Bandit, pytest, terraform test) vào CI.
- **Tuần 9–12:** củng cố CI/CD & DevSecOps, viết blog kỹ thuật, rồi đấu nối Lambda – API Gateway – Cognito – CloudFront, giám sát, kiểm thử tải, pentest WAF và diễn tập khôi phục hệ thống.

**Nội dung từng tuần:**

**Tuần 1:** Làm quen AWS: tài khoản, chi phí, IAM và mạng VPC

**Tuần 2:** Hoàn tất lab VPC, làm quen IaC với CloudFormation; EC2, IAM Role, AWS CLI và S3

**Tuần 3:** Cơ sở dữ liệu & messaging trên AWS; tìm đề tài qua khảo sát thực tế

**Tuần 4:** Serverless cơ bản (Lambda, API Gateway, Cognito) và chốt hướng đề tài

**Tuần 5:** Bảo mật trên AWS: IAM nâng cao, KMS, WAF, Secrets Manager và bảo mật S3

**Tuần 6:** Terraform và bản hạ tầng đầu tiên (kiến trúc 3-tier Smart Kitchen)

**Tuần 7:** Chuyển sang kiến trúc 100% Serverless – khởi động dự án SecureTable

**Tuần 8:** Hoàn thiện nền hạ tầng, rà soát bảo mật và đưa DevSecOps vào CI

**Tuần 9:** Củng cố CI/CD & DevSecOps: GitHub Actions, công cụ quét, OIDC và test IaC

**Tuần 10:** Git nâng cao, viết blog kỹ thuật, threat model và lộ trình học Cloud/DevSecOps

**Tuần 11:** Đấu nối hệ thống (Lambda, API Gateway, CloudFront) và hoàn thiện báo cáo thực tập

**Tuần 12:** Rà soát cuối và nộp Workshop (hạn 30/11/2026)
