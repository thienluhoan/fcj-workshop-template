---
title: "Workshop"
weight: 5
chapter: false
pre: "<b> 5. </b> "
---

# Deploying Bite to Business on AWS

This **hands-on infrastructure workshop** documents deployment of the **Bite to Business (BTB)** food-commerce web application. BTB is the direct seller to individuals/households and business customers; it is **not** a multi-vendor marketplace. The backend is a modular-monolith Spring Boot application; the frontend is React/TypeScript.

**Scope:** Amazon VPC, AWS IAM, AWS Budgets, Amazon ECR, Amazon S3, AWS Secrets Manager, Application Load Balancer, Amazon ECS with AWS Fargate, AWS Certificate Manager, Amazon CloudFront, Amazon Route 53, AWS WAF, and Amazon CloudWatch Logs.

**Teammate-owned services are excluded from this lab:** Amazon Bedrock, SQS, SES, CodePipeline/CodeBuild, CloudWatch Metrics/Alarms, CloudTrail, NAT Gateway and VPC endpoints. They may appear in the **shared architecture** for context only.

**Documentation status / Trạng thái tài liệu:** Draft instructions. Resource names, values, test results, demo links and screenshots must be replaced with real evidence before submission. / Bản nháp hướng dẫn. Cần thay tên tài nguyên, thông số, kết quả kiểm thử, link demo và ảnh chụp bằng minh chứng thực tế trước khi nộp.

## Contents

1. [Overview](5.1-overview/)
2. [Prerequisites](5.2-prerequisites/)
3. [Architecture & Data Flow](5.3-architecture/)
4. [Foundation Setup](5.4-foundation/)
5. [Container & Storage](5.5-containers-storage/)
6. [Backend Deployment](5.6-backend/)
7. [Web Delivery & Security](5.7-web-security/)
8. [Logging & Troubleshooting](5.8-logging/)
9. [Testing, Validation & Demo](5.9-testing/)
10. [Cost Estimation & Optimization](5.10-cost/)
11. [Cleanup](5.11-cleanup/)

**Before submission:** personally reconstruct and verify the AWS diagram using official AWS icons; perform the deployment; record actual Console screenshots, command outputs, measured results, cost calculator estimates, and live-demo evidence. The sample site and this draft are not proof that AWS resources exist.
