---
title: "Worklog Tuần 1"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

- Làm quen chương trình First Cloud Journey (FCJ): cách học theo workshop, cộng đồng AWS Study Group.
- Tạo và bảo mật tài khoản AWS ngay từ ngày đầu: MFA cho root, IAM user thay root, AWS Budgets cảnh báo chi phí.
- Nhận đủ credit thực hành bằng 5 nhiệm vụ khám phá dịch vụ (EC2, Bedrock, Budgets, Lambda, RDS).
- Hiểu và tự dựng mạng Amazon VPC nhiều AZ, triển khai EC2 public/private, giám sát và kết nối hybrid bằng Site-to-Site VPN.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Làm quen chương trình FCJ và cộng đồng AWS Study Group<br>- Hoàn thành Lab01-01 → Lab01-04:<br>&emsp;+ Tạo tài khoản AWS, bật MFA cho root<br>&emsp;+ Tạo AWS Budgets + cảnh báo qua email<br>&emsp;+ Tìm hiểu AWS Support Center, quy trình mở case<br>&emsp;+ Tạo IAM Admin Group và IAM Admin User | 14/09/2026 | 14/09/2026 | [000001 – Tạo tài khoản](https://000001.awsfcaj.com/3-chi%E1%BA%BFn-l%C6%B0%E1%BB%A3c-nh%E1%BA%ADn-%C4%91%E1%BB%A7-200-credit/)<br>[MFA cho root](https://docs.aws.amazon.com/IAM/latest/UserGuide/enable-virt-mfa-for-root.html)<br>[000007 – AWS Budgets](https://000007.awsfcaj.com/1-create-budget/)<br>[000009 – AWS Support](https://000009.awsfcaj.com/1-support-plans/)<br>[000002 – IAM Admin Group/User](https://000002.awsfcaj.com/2-create-admin-user-and-group/) |
| 3 | - Hoàn thành 5/5 nhiệm vụ AWS credit:<br>&emsp;+ Launch EC2 Linux, SSH bằng key pair<br>&emsp;+ Bedrock Playground với foundation model<br>&emsp;+ Cost Budget + ngưỡng cảnh báo<br>&emsp;+ Web app bằng Lambda Function URL<br>&emsp;+ Tạo Aurora/RDS rồi dọn dẹp | 15/09/2026 | 15/09/2026 | [000001 – 5 nhiệm vụ nhận credit](https://000001.awsfcaj.com/4-h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-chi-ti%E1%BA%BFt-5-nhi%E1%BB%87m-v%E1%BB%A5-ki%E1%BA%BFm-ti%E1%BB%81n/)<br>[000001 – Dịch vụ cần tránh](https://000001.awsfcaj.com/6-danh-s%C3%A1ch-s%C3%A1t-th%E1%BB%A7-credit-c%E1%BA%A7n-tr%C3%A1nh/) |
| 4 | - Bắt đầu workshop *Amazon VPC and AWS Site-to-Site VPN*<br>- Hoàn thành module 1 (Introduction):<br>&emsp;+ Subnets, Route Table, Internet Gateway, NAT Gateway<br>- Hoàn thành mục 2.1 Security Group | 16/09/2026 | 16/09/2026 | [000003 – 1. Introduction](https://000003.awsfcaj.com/1-introduce/)<br>[000003 – 1.1 Subnets](https://000003.awsfcaj.com/1-introduce/1.1-subnets/)<br>[000003 – 1.2 Route Table](https://000003.awsfcaj.com/1-introduce/1.2-routetable/)<br>[000003 – 1.3 Internet Gateway](https://000003.awsfcaj.com/1-introduce/1.3-internetgateway/)<br>[000003 – 1.4 NAT Gateway](https://000003.awsfcaj.com/1-introduce/1.4-natgateway/)<br>[000003 – 2.1 Security Group](https://000003.awsfcaj.com/2-firewallinvpc/2.1-securitygroup/) |
| 5 | - Hoàn thành 2.2 Network ACLs, 2.3 VPC Resource Map<br>- Hoàn thành module 3 (3.1 → 3.6): VPC, Subnet, IGW, Route Table, SG, VPC Flow Logs<br>- Mục 4.1: tạo EC2 trong public subnet | 17/09/2026 | 17/09/2026 | [000003 – 2.2 NACLs](https://000003.awsfcaj.com/2-firewallinvpc/2.2-networkacls/)<br>[000003 – 2.3 Resource Map](https://000003.awsfcaj.com/2-firewallinvpc/2.3-vpc-resource-map/)<br>[000003 – 3. Preparation Steps](https://000003.awsfcaj.com/3-prerequisite/)<br>[000003 – 4.1 Create EC2](https://000003.awsfcaj.com/4-createec2server/4.1-createec2/) |
| 6 | - Hoàn thành 4.2 Test Connection<br>- Hoàn thành 4.3 Create NAT Gateway<br>- Hoàn thành 4.4 Using Reachability Analyzer | 18/09/2026 | 18/09/2026 | [000003 – 4.2 Test Connection](https://000003.awsfcaj.com/4-createec2server/4.2-connectec2/)<br>[000003 – 4.3 Create NAT Gateway](https://000003.awsfcaj.com/4-createec2server/4.3-natgateway/)<br>[000003 – 4.4 Using Reachability Analyzer](https://000003.awsfcaj.com/4-createec2server/4.4-createreachabilityanalyzer/) |
| 7 | - Hoàn thành 4.5 EC2 Instance Connect Endpoint<br>- Hoàn thành 4.6 Systems Manager Session Manager<br>- Hoàn thành 4.7 CloudWatch Monitoring & Alerting (Alarm + SNS) | 19/09/2026 | 19/09/2026 | [000003 – 4.5 Create EC2 Instance Connect Endpoint](https://000003.awsfcaj.com/4-createec2server/4.5-eicendpoint/)<br>[000003 – 4.6 AWS Systems Manager Session Manager](https://000003.awsfcaj.com/4-createec2server/4.6-sessionmanager/)<br>[000003 – 4.7 CloudWatch Monitoring & Alerting](https://000003.awsfcaj.com/4-createec2server/4.7-cloudwatchmonitoring/) |
| CN | - Module 5 – Site-to-Site VPN:<br>&emsp;+ 5.1 Tạo môi trường VPN (VPC + EC2 mô phỏng on-prem)<br>&emsp;+ 5.2 Tạo VGW, CGW, VPN Connection<br>&emsp;+ Tải cấu hình Customer Gateway | 20/09/2026 | 20/09/2026 | [000003 – 5.1 VPN environment](https://000003.awsfcaj.com/5-vpnsitetosite/5.1-createvpnenv/)<br>[000003 – 5.2 Configure VPN](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/) |

### Kết quả đạt được tuần 1:

- Có tài khoản AWS an toàn: root bật MFA và không dùng cho việc hằng ngày; thao tác bằng IAM user thuộc nhóm Admin, có MFA.
- Đã cấu hình AWS Budgets kèm cảnh báo email, biết mở case với AWS Support gói Basic.
- Hoàn thành 5/5 nhiệm vụ credit, nắm cách dùng nhanh EC2, Bedrock Playground, Lambda Function URL, RDS/Aurora và dọn tài nguyên ngay sau khi thử.
- Hoàn thành module 1–5 của workshop *Amazon VPC and AWS Site-to-Site VPN*:
  - Tự dựng VPC 10.10.0.0/16 với 2 public + 2 private subnet trên 2 AZ, IGW, Route Table, Security Group, NACL, VPC Flow Logs.
  - Triển khai EC2, NAT Gateway, kiểm tra đường đi bằng Reachability Analyzer.
  - Truy cập EC2 private không cần bastion (EC2 Instance Connect Endpoint, Session Manager) và giám sát bằng CloudWatch Alarm + SNS.
  - Dựng kết nối Site-to-Site VPN (VGW, CGW, 2 tunnel IPsec) giữa VPC và mạng on-premises mô phỏng.
- Phân biệt rõ Security Group (stateful, cấp ENI) và Network ACL (stateless, cấp subnet).
