---
title: "Week 1 Worklog"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

- Get to know the First Cloud Journey (FCJ) program: workshop-based learning and the AWS Study Group community.
- Create and secure the AWS account from day one: root MFA, an IAM user instead of root, AWS Budgets cost alerts.
- Earn the full practice credit through 5 service-exploration activities (EC2, Bedrock, Budgets, Lambda, RDS).
- Understand and build a multi-AZ Amazon VPC, deploy public/private EC2, monitor it and connect a hybrid network with Site-to-Site VPN.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Got to know the FCJ program and the AWS Study Group community<br>- Completed Lab01-01 → Lab01-04:<br>&emsp;+ Created the AWS account, enabled root MFA<br>&emsp;+ Created AWS Budgets + email alerts<br>&emsp;+ Explored AWS Support Center and the case process<br>&emsp;+ Created the IAM Admin Group and IAM Admin User | 09/14/2026 | 09/14/2026 | [000001 – Create AWS account](https://000001.awsfcaj.com/3-chi%E1%BA%BFn-l%C6%B0%E1%BB%A3c-nh%E1%BA%ADn-%C4%91%E1%BB%A7-200-credit/)<br>[Root user MFA](https://docs.aws.amazon.com/IAM/latest/UserGuide/enable-virt-mfa-for-root.html)<br>[000007 – AWS Budgets](https://000007.awsfcaj.com/1-create-budget/)<br>[000009 – AWS Support](https://000009.awsfcaj.com/1-support-plans/)<br>[000002 – IAM Admin Group/User](https://000002.awsfcaj.com/2-create-admin-user-and-group/) |
| Tue | - Completed all 5 AWS credit activities:<br>&emsp;+ Launched EC2 Linux, SSH with a key pair<br>&emsp;+ Bedrock Playground with a foundation model<br>&emsp;+ Cost Budget + alert thresholds<br>&emsp;+ Web app with a Lambda Function URL<br>&emsp;+ Created Aurora/RDS and cleaned it up | 09/15/2026 | 09/15/2026 | [000001 – 5 credit activities](https://000001.awsfcaj.com/4-h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-chi-ti%E1%BA%BFt-5-nhi%E1%BB%87m-v%E1%BB%A5-ki%E1%BA%BFm-ti%E1%BB%81n/)<br>[000001 – Services to avoid](https://000001.awsfcaj.com/6-danh-s%C3%A1ch-s%C3%A1t-th%E1%BB%A7-credit-c%E1%BA%A7n-tr%C3%A1nh/) |
| Wed | - Started the *Amazon VPC and AWS Site-to-Site VPN* workshop<br>- Completed module 1 (Introduction):<br>&emsp;+ Subnets, Route Table, Internet Gateway, NAT Gateway<br>- Completed 2.1 Security Group | 09/16/2026 | 09/16/2026 | [000003 – 1. Introduction](https://000003.awsfcaj.com/1-introduce/)<br>[000003 – 1.1 Subnets](https://000003.awsfcaj.com/1-introduce/1.1-subnets/)<br>[000003 – 1.2 Route Table](https://000003.awsfcaj.com/1-introduce/1.2-routetable/)<br>[000003 – 1.3 Internet Gateway](https://000003.awsfcaj.com/1-introduce/1.3-internetgateway/)<br>[000003 – 1.4 NAT Gateway](https://000003.awsfcaj.com/1-introduce/1.4-natgateway/)<br>[000003 – 2.1 Security Group](https://000003.awsfcaj.com/2-firewallinvpc/2.1-securitygroup/) |
| Thu | - Completed 2.2 Network ACLs, 2.3 VPC Resource Map<br>- Completed module 3 (3.1 → 3.6): VPC, Subnet, IGW, Route Table, SG, VPC Flow Logs<br>- 4.1: created EC2 in a public subnet | 09/17/2026 | 09/17/2026 | [000003 – 2.2 NACLs](https://000003.awsfcaj.com/2-firewallinvpc/2.2-networkacls/)<br>[000003 – 2.3 Resource Map](https://000003.awsfcaj.com/2-firewallinvpc/2.3-vpc-resource-map/)<br>[000003 – 3. Preparation Steps](https://000003.awsfcaj.com/3-prerequisite/)<br>[000003 – 4.1 Create EC2](https://000003.awsfcaj.com/4-createec2server/4.1-createec2/) |
| Fri | - Completed 4.2 Test Connection<br>- Completed 4.3 Create NAT Gateway<br>- Completed 4.4 Using Reachability Analyzer | 09/18/2026 | 09/18/2026 | [000003 – 4.2 Test Connection](https://000003.awsfcaj.com/4-createec2server/4.2-connectec2/)<br>[000003 – 4.3 Create NAT Gateway](https://000003.awsfcaj.com/4-createec2server/4.3-natgateway/)<br>[000003 – 4.4 Using Reachability Analyzer](https://000003.awsfcaj.com/4-createec2server/4.4-createreachabilityanalyzer/) |
| Sat | - Completed 4.5 EC2 Instance Connect Endpoint<br>- Completed 4.6 Systems Manager Session Manager<br>- Completed 4.7 CloudWatch Monitoring & Alerting (Alarm + SNS) | 09/19/2026 | 09/19/2026 | [000003 – 4.5 Create EC2 Instance Connect Endpoint](https://000003.awsfcaj.com/4-createec2server/4.5-eicendpoint/)<br>[000003 – 4.6 AWS Systems Manager Session Manager](https://000003.awsfcaj.com/4-createec2server/4.6-sessionmanager/)<br>[000003 – 4.7 CloudWatch Monitoring & Alerting](https://000003.awsfcaj.com/4-createec2server/4.7-cloudwatchmonitoring/) |
| Sun | - Module 5 – Site-to-Site VPN:<br>&emsp;+ 5.1 Built the VPN environment (VPC + EC2 simulating on-prem)<br>&emsp;+ 5.2 Created VGW, CGW, VPN Connection<br>&emsp;+ Downloaded the Customer Gateway configuration | 09/20/2026 | 09/20/2026 | [000003 – 5.1 VPN environment](https://000003.awsfcaj.com/5-vpnsitetosite/5.1-createvpnenv/)<br>[000003 – 5.2 Configure VPN](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/) |

### Week 1 Achievements:

- A secured AWS account: root has MFA and is not used day to day; daily work uses an MFA-protected IAM user in the Admin group.
- AWS Budgets configured with email alerts; know how to open cases with AWS Basic Support.
- Completed all 5 credit activities, got hands-on with EC2, Bedrock Playground, Lambda Function URL, RDS/Aurora and cleaned up right after.
- Completed modules 1–5 of the *Amazon VPC and AWS Site-to-Site VPN* workshop:
  - Built VPC 10.10.0.0/16 with 2 public + 2 private subnets across 2 AZs, IGW, Route Tables, Security Groups, NACLs and VPC Flow Logs.
  - Deployed EC2 and a NAT Gateway, verified paths with Reachability Analyzer.
  - Reached private EC2 without a bastion (EC2 Instance Connect Endpoint, Session Manager) and monitored it with CloudWatch Alarm + SNS.
  - Set up a Site-to-Site VPN (VGW, CGW, 2 IPsec tunnels) between the VPC and a simulated on-premises network.
- Clearly distinguish Security Groups (stateful, ENI level) from Network ACLs (stateless, subnet level).
