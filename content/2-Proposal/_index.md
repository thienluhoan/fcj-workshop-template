---
title: "Proposal"
date: 2026-10-18
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SecureTable – Restaurant Management System on AWS

## An Ingredient Management and QR Code Ordering System Built on AWS Serverless

---

### 1. Project Overview

**SecureTable** is a restaurant management system designed for small-scale restaurants, focusing on food ordering, ingredient management, billing, and revenue tracking. Customers scan a fixed QR code at their table to view the menu and place orders, while staff use dedicated interfaces to carry out their tasks.

The system follows a **serverless architecture** on AWS to reduce server administration and facilitate future scaling. The project is deployed for testing in the **AWS Asia Pacific (Singapore) Region – `ap-southeast-1`**.

**User roles:**

- **Guest:** Scan a QR code, view the menu, and place orders without creating an account.
- **Reception:** Manage tables and open or close bills.
- **Kitchen:** Mark dishes as completed or reject them.
- **Inventory:** Record stock receipts, update inventory, and adjust ingredient quantities.
- **Admin:** View financial reports and activity logs.

### 2. Project Objectives

- Build an AWS-hosted restaurant management website.
- Synchronize data across ordering, kitchen operations, inventory, and billing.
- Track ingredients received, reserved, consumed, and lost.
- Implement staff access control and log important actions for traceability.
- Automate reporting, backups, and system monitoring.
- Apply **AWS Well-Architected** principles for security, reliability, performance, operations, and cost optimization.

### 3. Problem Statement

In many small restaurants, ingredient records, order taking, and revenue reporting still rely on notebooks or separate tools. This can lead to inaccurate inventory figures, delays in passing orders to the kitchen, and difficulty tracking changes to data.

**Proposed solution:** SecureTable uses a centralized data system. When a customer places an order, the order information is sent to the kitchen and linked to reserved ingredient quantities. As dishes are processed and bills are closed, inventory and financial records are updated according to business rules. Managers can review reports and audit logs instead of manually reconciling records.

### 4. Solution Architecture

![Architecture](../images/2-Proposal/SecureTable_Architecture.svg)

| AWS Service | Role |
|---|---|
| **Amazon S3 + Amazon CloudFront** | Store and deliver the web interface over HTTPS. |
| **Amazon API Gateway** | Receive and route API requests. |
| **AWS Lambda** | Handle GuestServices, Ordering, Reception, Kitchen, Inventory, AdminDashboard, Report, and Backup. |
| **Amazon DynamoDB** | Store table, bill, order, ingredient, financial, and audit data. |
| **Amazon Cognito + AWS IAM** | Authenticate staff and restrict access to AWS resources. |
| **Amazon EventBridge Scheduler + Amazon SNS** | Schedule reports and backups and send email notifications. |
| **Amazon CloudWatch + AWS CloudTrail** | Monitor, alert, and track activity across AWS. |
| **AWS WAF** | Filter certain types of abnormal web requests (if enabled in the deployment). |

**Main workflows:**

1. **Guest:** Scan QR code → CloudFront → API Gateway → GuestServices/Ordering → DynamoDB. A guest session is valid only while its associated bill is open.
2. **Staff:** Sign in through Cognito → Call API Gateway → Business-specific Lambda → DynamoDB.
3. **Kitchen/Inventory:** Process dishes or ingredients; use conditional updates and transactions when needed to maintain data consistency.
4. **Automation:** EventBridge Scheduler → Report/Backup Lambda → SNS, or export DynamoDB data to S3.

**Well-Architected approach:** Apply least-privilege IAM, HTTPS, and Cognito for security; DynamoDB transactions and backups for reliability; CloudWatch for operations; Lambda and DynamoDB for on-demand scalability; and resource usage monitoring for cost optimization.

### 5. Implementation Timeline

| Phase | Activities |
|---|---|
| **Weeks 1–2** | Analyze requirements and design the architecture and DynamoDB data model. |
| **Weeks 3–4** | Deploy the S3/CloudFront frontend, Cognito, API Gateway, and core Lambda functions. |
| **Weeks 5–6** | Complete Ordering, Kitchen, Inventory, Reception, Finance/Audit, and reporting features. |
| **Weeks 7–8** | Add monitoring, security, and backups; test the system and prepare the demo and Workshop documentation. |

### 6. Estimated Budget

The budget covers **one month of testing and demonstration** in the AWS Singapore Region (`ap-southeast-1`), assuming a serverless architecture and low traffic. The amounts below are **planning estimates**, not actual invoices or figures verified through AWS Pricing Calculator.

#### 6.1. Budget Assumptions

| Parameter | Assumption |
|---|---|
| Restaurant size | 10 tables, approximately 20–30 bills per day |
| API traffic | Approximately 40,000 REST API requests per month |
| Backend processing | Approximately 60,000 Lambda invocations per month, mostly at 256 MB |
| Data | Approximately 1 GB in DynamoDB |
| Reporting and backups | Scheduled reports; backups of approximately 13 tables each week |
| CloudFront & WAF | Assumes the **CloudFront Free Plan**, with WAF included in the plan |

#### 6.2. Estimated Monthly Costs

| No. | AWS Service | Basis of Estimate | Cost (USD/month) |
|:---:|---|---|---:|
| 1 | Amazon CloudFront & S3 | Static frontend; Free Plan, with an allowance for S3 storage/requests outside the plan | **$0.50** |
| 2 | Amazon API Gateway (REST) | 40,000 requests/month | **$0.20** |
| 3 | AWS Lambda | 60,000 invocations/month, mostly at 256 MB | **$0.30** |
| 4 | Amazon DynamoDB | Approximately 1 GB of data and low read/write traffic | **$1.00** |
| 5 | Amazon Cognito | Small number of staff accounts, assumed eligible for free usage | **$0.00** |
| 6 | Amazon CloudWatch | Low-volume logs and several standard alarms | **$1.00** |
| 7 | EventBridge Scheduler & SNS | Report/backup schedules and notification emails | **$0.10** |
| 8 | AWS WAF | Assumed to be included in the CloudFront Free Plan | **$0.00** |
| 9 | DynamoDB PITR, Export & S3 Backup | Allowance for PITR, weekly exports, and backup storage | **$2.50** |
| 10 | AWS CloudTrail | Default Event History only | **$0.00** |
| | **Total estimated service cost** | | **$5.60** |
| | **Contingency allowance** | | **$2.00** |
| | **Total estimated budget** | | **$7.60/month** |

**Total estimated budget for 12 months: $91.20**, assuming the same configuration and usage levels are maintained. Actual costs may vary depending on usage, individual table sizes, AWS Free Tier/credits, and additional enabled features. If AWS WAF is billed separately, its charges must be added.

Reference: [AWS Pricing Calculator](https://calculator.aws/).

### 7. Risks and Mitigation Measures

| Risk | Mitigation |
|---|---|
| A static QR code is copied and used to place orders remotely | Restrict sessions to open bills; allow Reception to review disputed orders. Additional authentication is a possible future improvement. |
| Concurrent orders cause inventory inconsistencies | Use DynamoDB transactions, conditional updates, and idempotency for critical operations. |
| Lambda/API errors or timeouts | Monitor with CloudWatch, configure alerts, and allow safe retries for operations designed to support them. |
| Unauthorized access or excessive permissions | Use Cognito for staff, least-privilege IAM, and backend authorization checks. |
| Data loss or budget overruns | Perform scheduled backups, test recovery, use AWS Budgets, and review resources regularly. |

### 8. Expected Outcomes

SecureTable aims to deliver an **MVP that demonstrates the complete workflow**: Reception opens a table → a customer scans the QR code and places an order → Kitchen processes the order → Inventory updates ingredient quantities → Reception closes the bill → Admin reviews reports and activity logs.

From a technical perspective, the project should demonstrate that AWS services work together as shown in the architecture diagram, business data is updated consistently, access is controlled, and monitoring, reporting, and backup capabilities are in place. In the long term, the system could be expanded to support online payments, multiple branches, and stronger QR code verification mechanisms.