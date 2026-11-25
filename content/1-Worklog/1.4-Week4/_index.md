---
title: "Worklog Week 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 4 Objectives:

- Understand and practice AWS resource management using **Resource Tags** and **Resource Groups**.
- Implement fine-grained Attribute-Based Access Control (ABAC) using AWS IAM to restrict EC2 resource management.
- Automate OS patch management and remote server command execution using **AWS Systems Manager (SSM)**.
- Securely manage EC2 instances without exposing SSH/RDP ports using **AWS SSM Session Manager** (Session Logging & Port Forwarding).

### Tasks Implemented This Week:
| Day | Work Details | Start Date | Completion Date | Reference Link |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - Research concepts of AWS resource tagging and organization via Resource Groups. | 05/10/2026   | 05/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Hands-on with Resource Tags:**<br>&emsp;+ Launch EC2 instances with tags.<br>&emsp;+ Manage, query, and filter resources by tags via AWS Console and AWS CLI.<br>&emsp;+ Create Resource Groups to logically group resources.<br>&emsp;+ Clean up test resources. | 06/10/2026   | 06/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Managing EC2 Tag Access via AWS IAM:**<br>&emsp;+ Create IAM Users, IAM Roles, and analyze IAM policies.<br>&emsp;+ **Policy Verification Details:**<br>&emsp;&emsp;• `ec2-list-read`: Read-only access to EC2 in `us-east-1` & `us-west-1`.<br>&emsp;&emsp;• `ec2-create-tags`: Permission to apply tags upon instance creation.<br>&emsp;&emsp;• `ec2-create-tags-existing`: Permission to tag existing instances given `Key=Team`, `Value=Alpha`.<br>&emsp;&emsp;• `ec2-run-instances`: Launch instances in `us-east-1`/`us-west-1` requiring tag `Team=Alpha`.<br>&emsp;&emsp;• `ec2-manage-instances`: Start/Stop/Reboot/Terminate instances in `us-east-1`/`us-west-1` tagged with `Team=Alpha`.<br>&emsp;+ Clean up lab resources. | 07/10/2026   | 07/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Patch Management & Command Execution with AWS Systems Manager:**<br>&emsp;+ Set up VPC, Subnets, and launch 2 Windows EC2 instances (`Windows-Lab-SSM-1`, `Windows-Lab-SSM-2`).<br>&emsp;+ Assign SSM IAM Roles to EC2 instances for registration under Managed Nodes in Fleet Manager.<br>&emsp;+ Verify successful Terminal Session connections on both nodes.<br>&emsp;+ Execute Patch Manager using `Scan and install` operation with `Do not reboot my instances` option.<br>&emsp;+ Confirm successful patch execution status on AWS Console & clean up resources. | 08/10/2026   | 08/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Working with Amazon Systems Manager - Session Manager:**<br>&emsp;+ **Session Logging:** Create an S3 Bucket, configure Session Manager logging, attach IAM write policies, and inspect logs on S3.<br>&emsp;+ **Port Forwarding:** Create IAM User `Portfwd` (`AmazonSSMFullAccess`), install AWS CLI & Session Manager Plugin on local Windows machine; run `aws ssm start-session` to forward RDP port 3389 to `localhost:9999` for secure RDP access into Private Subnet EC2.<br>&emsp;+ Verify session audit history in Session History & clean up resources. | 09/10/2026   | 09/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 4 Key Accomplishments:

- Mastered resource management and filtering techniques using **Resource Tags & Resource Groups** across AWS Console and CLI.
- Successfully implemented **Attribute-Based Access Control (ABAC)** using IAM policies, enforcing instance operations strictly based on tag conditions (`Team=Alpha`) and specific AWS regions (`us-east-1`, `us-west-1`).
- Centrally managed Windows EC2 instances using **AWS SSM Fleet Manager & Patch Manager**, performing OS patch scanning and installation without requiring server reboots.
- Deployed a secure, bastion-less access architecture:
  - Automatically logged and stored full command execution histories (Session Logs) into an **Amazon S3 Bucket**.
  - Configured **Port Forwarding** via Session Manager to establish Remote Desktop (RDP) connections to Windows instances in private subnets via `localhost:9999`.
- Terminated all temporary lab instances and revoked test permissions to maintain clean environment hygiene and prevent unnecessary cloud costs.
