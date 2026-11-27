---
title: "Week 2 Worklog"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

- Finish the VPC workshop: customise/troubleshoot the VPN and clean up resources in the right order.
- Get started with Infrastructure as Code using AWS CloudFormation.
- Master EC2: changing instance type, EBS snapshots, custom AMIs, recovering access.
- Understand why IAM Roles beat access keys; operate AWS with the AWS CLI.
- Host a static website on S3 and speed it up / protect it with CloudFront.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - 5.2.5 → 5.2.8: tunnel customisation, alternative configurations, VPN troubleshooting<br>- Module 6: cleaned up all VPC lab resources | 09/21/2026 | 09/21/2026 | [000003 – 5.2.5 Customizing AWS VPN Tunnel](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.5-modifyvpn/)<br>[000003 – 5.2.6 Alternative VPN Configurations](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.6-alternativeconfigurations/)<br>[000003 – 5.2.7 VPN Troubleshooting Guide](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.7-troubleshootingguide/)<br>[000003 – 5.2.8 AWS Official VPN Troubleshooting Guide](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.8-awsofficialtroubleshooting/)<br>[000003 – 6. Cleaning Up Resources](https://000003.awsfcaj.com/6-cleanup/) |
| Tue | - 000003 module 7: CloudFormation template for a multi-AZ VPC<br>- CloudFormation workshop: CloudShell, writing templates, creating/updating/deleting stacks | 09/22/2026 | 09/22/2026 | [000003 – 7. Infrastructure as Code Templates](https://000003.awsfcaj.com/7-infrastructureascode/)<br>[000037 – 1. Introduction](https://000037.awsfcaj.com/1-introduce/)<br>[000037 – 3.1 Launch CloudShell](https://000037.awsfcaj.com/3-cloudformationbasic/3.1-launchawscloudshell/)<br>[000037 – 3.2 Create CloudFormation Template](https://000037.awsfcaj.com/3-cloudformationbasic/3.2-createcloudformationtemplate/)<br>[000037 – 4.3 Drift Detection](https://000037.awsfcaj.com/4-cloudformationadvance/4.3-driftdetection/) |
| Wed | - Introduction to Amazon EC2 workshop:<br>&emsp;+ Launched Windows Server 2025 and Amazon Linux, connected via RDP/SSH<br>&emsp;+ Changed instance type, EBS snapshots<br>&emsp;+ Created a custom AMI, launched from the AMI<br>&emsp;+ Recovered Linux access after losing the key pair | 09/23/2026 | 09/23/2026 | [000004 – Launch instance](https://000004.awsfcaj.com/4-launchlinuxinstance/)<br>[000004 – 5. Amazon EC2 Basic](https://000004.awsfcaj.com/5-amazonec2basic/) |
| Thu | - IAM Role for applications workshop: access key vs IAM role on EC2<br>- AWS CLI workshop: install, configure, work with S3, SNS, IAM, VPC, EC2 | 09/24/2026 | 09/24/2026 | [000048 – IAM Role for EC2](https://000048.awsfcaj.com/3-iamroleec2/)<br>[000011 – AWS CLI](https://000011.awsfcaj.com/) |
| Fri | - Start with Amazon S3 workshop:<br>&emsp;+ Created a bucket, enabled static website hosting<br>&emsp;+ Block Public Access, public objects<br>&emsp;+ Served the site through CloudFront, blocked direct access<br>&emsp;+ Versioning, moving objects, cross-region replication | 09/25/2026 | 09/25/2026 | [000057 – S3 static website](https://000057.awsfcaj.com/3-staticwebsite/)<br>[000057 – 7. CloudFront](https://000057.awsfcaj.com/7-cloudfront/)<br>[000057 – 8–10 Versioning/Replication](https://000057.awsfcaj.com/8-versioning/) |
| Sun | - Rewrote the 09/19 and 09/20 worklogs from notes<br>- Reviewed Billing / Cost Explorer by service<br>- Revised the VPC – EC2 – S3 material of the first two weeks | 09/27/2026 | 09/27/2026 | [AWS Docs – Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)<br>[000007 – AWS Budgets](https://000007.awsfcaj.com/)<br>Original worklogs 09/19, 09/20 |

### Week 2 Achievements:

- Cleaned up all VPC lab resources (VPN, VGW, CGW, NAT Gateway, EIP, EC2, endpoint, VPC) in dependency order.
- Created, updated and deleted CloudFormation stacks; understand Parameters, Resources, Outputs and intrinsic functions.
- Practised advanced EC2: changed instance type, took EBS snapshots, built a custom AMI and launched from it.
- Moved from access keys to an IAM Role (instance profile) for apps on EC2; installed and used the AWS CLI with S3, SNS, IAM, VPC and EC2.
- Hosted a static website on S3, served it through CloudFront and blocked direct public bucket access; tried versioning, moving objects and replication.
- Started a weekly habit of checking Billing/Cost Explorer and rewriting the worklog from notes.
