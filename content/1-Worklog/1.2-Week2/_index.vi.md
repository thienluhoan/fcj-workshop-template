---
title: "Worklog Tuần 2"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

- Hoàn tất workshop VPC: tùy chỉnh/khắc phục sự cố VPN và dọn sạch tài nguyên đúng thứ tự.
- Làm quen Infrastructure as Code bằng AWS CloudFormation.
- Nắm vững EC2: đổi instance type, EBS snapshot, custom AMI, khôi phục truy cập.
- Hiểu vì sao dùng IAM Role thay access key; thao tác AWS bằng AWS CLI.
- Host website tĩnh trên S3 và tăng tốc/bảo vệ bằng CloudFront.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - 5.2.5 → 5.2.8: tùy chỉnh tunnel, cấu hình thay thế, troubleshooting VPN<br>- Module 6: dọn dẹp toàn bộ tài nguyên lab VPC | 21/09/2026 | 21/09/2026 | [000003 – 5.2.5 Customizing AWS VPN Tunnel](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.5-modifyvpn/)<br>[000003 – 5.2.6 Alternative VPN Configurations](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.6-alternativeconfigurations/)<br>[000003 – 5.2.7 VPN Troubleshooting Guide](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.7-troubleshootingguide/)<br>[000003 – 5.2.8 AWS Official VPN Troubleshooting Guide](https://000003.awsfcaj.com/5-vpnsitetosite/5.2-vpnsitetosite/5.2.8-awsofficialtroubleshooting/)<br>[000003 – 6. Cleaning Up Resources](https://000003.awsfcaj.com/6-cleanup/) |
| 3 | - 000003 module 7: template CloudFormation cho VPC Multi-AZ<br>- Workshop CloudFormation: CloudShell, viết template, tạo/cập nhật/xóa stack | 22/09/2026 | 22/09/2026 | [000003 – 7. Infrastructure as Code Templates](https://000003.awsfcaj.com/7-infrastructureascode/)<br>[000037 – 1. Introduction](https://000037.awsfcaj.com/1-introduce/)<br>[000037 – 3.1 Launch CloudShell](https://000037.awsfcaj.com/3-cloudformationbasic/3.1-launchawscloudshell/)<br>[000037 – 3.2 Create CloudFormation Template](https://000037.awsfcaj.com/3-cloudformationbasic/3.2-createcloudformationtemplate/)<br>[000037 – 4.3 Drift Detection](https://000037.awsfcaj.com/4-cloudformationadvance/4.3-driftdetection/) |
| 4 | - Workshop Introduction to Amazon EC2:<br>&emsp;+ Launch Windows Server 2025 và Amazon Linux, kết nối RDP/SSH<br>&emsp;+ Đổi instance type, snapshot EBS<br>&emsp;+ Tạo custom AMI, launch từ AMI<br>&emsp;+ Khôi phục truy cập Linux khi mất key pair | 23/09/2026 | 23/09/2026 | [000004 – Launch instance](https://000004.awsfcaj.com/4-launchlinuxinstance/)<br>[000004 – 5. Amazon EC2 Basic](https://000004.awsfcaj.com/5-amazonec2basic/) |
| 5 | - Workshop IAM Role cho ứng dụng: so sánh access key vs IAM role trên EC2<br>- Workshop AWS CLI: cài đặt, cấu hình, thao tác S3, SNS, IAM, VPC, EC2 | 24/09/2026 | 24/09/2026 | [000048 – IAM Role cho EC2](https://000048.awsfcaj.com/3-iamroleec2/)<br>[000011 – AWS CLI](https://000011.awsfcaj.com/) |
| 6 | - Workshop Start with Amazon S3:<br>&emsp;+ Tạo bucket, bật static website hosting<br>&emsp;+ Block Public Access, public object<br>&emsp;+ Đưa website qua CloudFront, chặn truy cập trực tiếp<br>&emsp;+ Versioning, move object, replication đa region | 25/09/2026 | 25/09/2026 | [000057 – S3 static website](https://000057.awsfcaj.com/3-staticwebsite/)<br>[000057 – 7. CloudFront](https://000057.awsfcaj.com/7-cloudfront/)<br>[000057 – 8–10 Versioning/Replication](https://000057.awsfcaj.com/8-versioning/) |
| CN | - Viết lại worklog 19/09 và 20/09 từ ghi chép<br>- Rà soát Billing / Cost Explorer theo dịch vụ<br>- Ôn lại kiến thức VPC – EC2 – S3 của 2 tuần | 27/09/2026 | 27/09/2026 | [AWS Docs – Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)<br>[000007 – AWS Budgets](https://000007.awsfcaj.com/)<br>Worklog gốc 19/09, 20/09 |

### Kết quả đạt được tuần 2:

- Dọn sạch tài nguyên lab VPC (VPN, VGW, CGW, NAT Gateway, EIP, EC2, endpoint, VPC) theo đúng thứ tự phụ thuộc.
- Tạo, cập nhật và xóa stack CloudFormation; hiểu Parameters, Resources, Outputs, intrinsic functions.
- Thực hành EC2 nâng cao: đổi instance type, snapshot EBS, tạo AMI riêng và launch lại từ AMI.
- Chuyển từ access key sang IAM Role (instance profile) cho ứng dụng trên EC2; cài và dùng AWS CLI với S3, SNS, IAM, VPC, EC2.
- Host website tĩnh trên S3, đưa qua CloudFront và chặn truy cập public trực tiếp vào bucket; thử versioning, move object, replication.
- Bắt đầu thói quen xem Billing/Cost Explorer cuối tuần và viết lại worklog từ ghi chép.
