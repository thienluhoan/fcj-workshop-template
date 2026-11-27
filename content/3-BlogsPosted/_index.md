---
title: "Blogs Posted"
date: 2026-11-18
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Our team wrote 3 technical blog posts and published them in the **AWS Study Group** community. All three come from real problems we ran into in labs and in the SecureTable project: file uploads in a serverless app, NAT Gateway costs during the VPC lab, and an access key leak while configuring CI/CD. Each post includes an architecture diagram, a cost calculation based on Singapore prices, common mistakes and how to verify the result.

### [Blog 1 - A 5 MB photo that won't upload? Stop making Lambda carry your files](3.1-blog1/)

This post explains why files should not go through API Gateway and Lambda: API Gateway caps payloads at 10 MB, Lambda at 6 MB, and base64 encoding adds about 33%. The recommended approach is an **S3 presigned URL**. The backend signs a short-lived URL for exactly one key, the client uploads straight to S3, and a Lambda then validates the file content through an S3 event. The post includes a presigned POST snippet with a size limit and a cost comparison. It also recommends lifecycle rules for orphan files and unfinished multipart uploads, and lists 7 common mistakes. [Read the original post on AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2341282956636663)

### [Blog 2 - A huge "NatGateway-Bytes" line on your AWS bill? You may be paying to call S3](3.2-blog2/)

This post shows why traffic from private subnets to S3, DynamoDB and ECR detours through the NAT Gateway at 0.059 USD/GB. Adding a free **Gateway Endpoint** to the route table removes that charge without code changes. The post also calculates the Interface Endpoint break-even point (about 390 GB/month across 2 AZs), covers endpoint policies, ECR requirements and multi-AZ NAT, and explains how to verify with Cost Explorer, CloudWatch and VPC Flow Logs. [Read the original post on AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2341791103252515)

### [Blog 3 - Still pasting AWS access keys into GitHub Secrets? Let your CI hold no keys at all](3.3-blog3/)

This post replaces long-lived access keys with **OIDC**. GitHub Actions requests a token, and AWS STS checks the trust policy and issues 1-hour temporary credentials. It highlights the most dangerous mistake: a trust policy that checks `aud` but not `sub`, which lets any repository assume the role. It also covers common traps (environments change `sub`, the new `sub` format, pull requests) and how to verify with CloudTrail. [Read the original post on AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2342465539851738)
