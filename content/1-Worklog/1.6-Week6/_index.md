---
title: "Worklog Week 6"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 6 Objectives:

- Protect web applications and APIs from security threats using **AWS WAF**.
- Manage data encryption centrally and control key lifecycles with **AWS KMS**.
- Automate system backup and disaster recovery plans using **AWS Backup**.
- Establish direct private network connectivity between VPCs using **VPC Peering**.
- Implement centralized and scalable cross-VPC routing using **AWS Transit Gateway**.

### Tasks Implemented This Week:
| Day | Work Details | Start Date | Completion Date | Reference Link |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Protect applications and APIs with AWS WAF:**<br>&emsp;+ Create a Web ACL in AWS WAF.<br>&emsp;+ Configure Managed Rule Groups against SQL Injection, XSS, and Rate-based limits.<br>&emsp;+ Associate the Web ACL with an Application Load Balancer (ALB) or API Gateway.<br>&emsp;+ Test blocking suspicious traffic and inspect monitoring logs.<br>&emsp;+ Clean up test resources. | 19/10/2026   | 19/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Key Management with AWS Key Management Service (AWS KMS):**<br>&emsp;+ Create a Customer Managed Key (CMK) in AWS KMS.<br>&emsp;+ Configure KMS Key Policies for administrative and usage permissions.<br>&emsp;+ Perform data encryption and decryption on S3 Buckets and EBS volumes using KMS keys.<br>&emsp;+ Inspect key access logs in AWS CloudTrail.<br>&emsp;+ Clean up resources. | 20/10/2026   | 20/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Implement system backup plans with AWS Backup:**<br>&emsp;+ Create a Backup Vault and configure encryption settings.<br>&emsp;+ Create a Backup Plan: Define backup schedules, retention periods, and lifecycle rules.<br>&emsp;+ Assign target resources (EBS volumes, RDS databases, EC2 instances).<br>&emsp;+ Execute an on-demand backup job to test functionality.<br>&emsp;+ Perform a test restore job and verify restored data integrity.<br>&emsp;+ Clean up resources. | 21/10/2026   | 21/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Connect Virtual Private Clouds using VPC Peering:**<br>&emsp;+ Provision 2 separate VPCs (VPC A and VPC B) with non-overlapping CIDR blocks.<br>&emsp;+ Create a VPC Peering Connection and accept the peering request.<br>&emsp;+ Update Route Tables in both VPCs to route cross-VPC traffic via the peering connection.<br>&emsp;+ Update Security Groups to allow inter-VPC traffic.<br>&emsp;+ Verify connectivity via ICMP/SSH between EC2 instances in both VPCs & clean up resources. | 22/10/2026   | 22/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Centralized connection management with AWS Transit Gateway:**<br>&emsp;+ Launch an AWS Transit Gateway (TGW).<br>&emsp;+ Create VPC Attachments to connect multiple VPCs to the Transit Gateway.<br>&emsp;+ Configure Transit Gateway Route Tables to control traffic routing between VPCs.<br>&emsp;+ Update Subnet Route Tables in connected VPCs pointing to the Transit Gateway.<br>&emsp;+ Test multi-VPC connectivity and clean up resources post-lab. | 23/10/2026   | 23/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 6 Key Accomplishments:

- Successfully deployed **AWS WAF** to safeguard web applications from common web exploits (SQLi, XSS, rate attacks).
- Mastered data encryption principles using **AWS KMS**, configuring Key Policies, and auditing key access history in CloudTrail.
- Built a centralized backup strategy using **AWS Backup**, automating scheduled backups and verifying restore operations.
- Established **VPC Peering** connections, mastering route table updates and security group settings for inter-VPC communication.
- Implemented **AWS Transit Gateway**, effectively simplifying multi-VPC network architecture and centralizing routing management.
- Safely terminated all temporary lab infrastructure post-practice to optimize cloud expenditures.
