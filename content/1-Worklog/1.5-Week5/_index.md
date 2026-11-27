---
title: "Week 5 Worklog"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

- Understand how AWS authenticates/authorises a request and use Conditions in IAM.
- Encrypt data with AWS KMS and audit key usage with CloudTrail.
- Protect web apps with AWS WAF; manage secrets with Secrets Manager.
- Apply security best practices to Amazon S3.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - IAM Role & Condition workshop:<br>&emsp;+ How AWS authenticates requests, the assume-role process<br>&emsp;+ Created an admin role, configured switch role<br>&emsp;+ Limited switch role by IP and time<br>- Read IAM Security best practices | 10/12/2026 | 10/12/2026 | [000044 – IAM Role & Condition](https://000044.awsfcaj.com/)<br>[IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| Tue | - Encryption at Rest with AWS KMS workshop:<br>&emsp;+ Policies/roles, groups/users<br>&emsp;+ Created a KMS key, encrypted S3 objects<br>&emsp;+ CloudTrail + Athena to audit key usage<br>&emsp;+ Sharing encrypted data | 10/13/2026 | 10/13/2026 | [000033 – KMS](https://000033.awsfcaj.com/)<br>[KMS pricing](https://aws.amazon.com/kms/pricing/) |
| Wed | - AWS Web Application Firewall workshop:<br>&emsp;+ Web ACL with managed rules<br>&emsp;+ Custom rule, advanced custom rule<br>&emsp;+ Rule testing, request logging<br>- Read WAF/DDoS fundamentals | 10/14/2026 | 10/14/2026 | [000026 – AWS WAF](https://000026.awsfcaj.com/3-useawswaf/)<br>[WAF pricing](https://aws.amazon.com/waf/pricing/) |
| Thu | - Using AWS Secrets Manager with Amazon RDS and AWS Fargate workshop:<br>&emsp;+ Stored RDS credentials in Secrets Manager<br>&emsp;+ Fargate app reading the secret at runtime<br>&emsp;+ Secret rotation | 10/15/2026 | 10/15/2026 | [000096 – 1. Introduction](https://000096.awsfcaj.com/1-intro/)<br>[000096 – 3. Using on RDS](https://000096.awsfcaj.com/3-rds-phase/)<br>[000096 – 4. Using on Fargate](https://000096.awsfcaj.com/4-fargate-phase/)<br>[AWS Secrets Manager – Pricing](https://aws.amazon.com/secrets-manager/pricing/) |
| Fri | - S3 Security Best Practices workshop:<br>&emsp;+ Enforce HTTPS, enforce SSE-S3<br>&emsp;+ Block public ACLs, S3 Block Public Access<br>&emsp;+ Restrict access through a VPC endpoint<br>&emsp;+ AWS Config to detect public buckets, IAM Access Analyzer | 10/16/2026 | 10/16/2026 | [000069 – S3 Security Best Practices](https://000069.awsfcaj.com/) |

### Week 5 Achievements:

- Created IAM roles with conditions limiting switch role by IP and by time; understand the explicit-deny rule.
- Created a KMS key, encrypted S3 objects with SSE-KMS and queried CloudTrail with Athena to see who used the key.
- Created a Web ACL with managed rules, custom rules, a rate-based rule and logging; understand WAF pricing.
- Stored and rotated database credentials with Secrets Manager.
- Applied 7 S3 security controls: enforce HTTPS, enforce encryption, block public ACLs, Block Public Access, restrict via VPC endpoint, AWS Config, IAM Access Analyzer.
