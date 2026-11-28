---
title: "Chuẩn bị"
date: 2026-11-27
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Tài khoản AWS

1. Một tài khoản AWS của bạn. Nhóm dùng **một account riêng, trống**, chạy bằng credit, để workshop không đụng tới workload khác.
2. Bật MFA cho root user, sau đó không dùng root nữa.
3. **Billing → Budgets**: tạo cost budget, cảnh báo ở mức 25/50/75/90 %. Bỏ chọn **Credits** trong phần cài đặt chi phí, nếu không biểu đồ luôn bằng 0.
4. **IAM → Users**: tạo user quản trị (ví dụ `seal-admin`) có quyền đăng nhập console, policy `AdministratorAccess` và MFA.

{{% notice warning %}}
Account ở gói Free sẽ bị đóng khi hết credit hoặc sau 6 tháng, và bị giới hạn cấu hình (EC2 tối đa `t3.small`, RDS `db.t4g.micro` Single-AZ). Template mặc định theo các mức này. Nếu stack báo lỗi về gói, hãy bàn với nhóm trước khi nâng lên gói Paid.
{{% /notice %}}

![Tổng quan budget fcaj-v3-monthly](/images/5-Workshop/5.2-Prerequisites/budget.png)

![Cảnh báo 50/80/100 % của budget fcaj-v3-monthly](/images/5-Workshop/5.2-Prerequisites/budget-alerts.png)

*Budget `fcaj-v3-monthly` (50 USD/tháng) do stack monitoring tạo ở mục [5.10](../5.10-testing-validation/), cảnh báo khi chi phí thực tế vượt 50, 80 và 100 %. Budget tạo tay ở bước 3 giám sát account cho tới khi có stack này.*

#### Công cụ trên máy

| Công cụ | Phiên bản dùng | Kiểm tra |
|---|---|---|
| AWS CLI | v2 | `aws --version` |
| Git | bất kỳ | `git --version` |
| Docker Desktop | có buildx | `docker version` |
| Node.js | 20+ | `node -v` |
| PowerShell | 7 (hoặc Git Bash) | `$PSVersionTable.PSVersion` |

Đăng nhập CLI bằng IAM user và kiểm tra danh tính. ARN phải kết thúc bằng tên user của bạn, không phải `root`:

```powershell
aws login
aws sts get-caller-identity --query "[Account,Arn]" --output text
```

#### Các tài khoản khác

- **Một tên miền.** Nhóm mua tên miền ở nhà đăng ký trong nước (iNET) và giao toàn bộ DNS cho Route 53 ở bước [5.4](../5.4-network-data/).
- **Repository SEAL** đã clone về máy. Mọi lệnh bên dưới chạy ở thư mục gốc của repository.
- **DeepSeek API key** cho AI worker (trả phí ngoài AWS). Gửi source code của sinh viên tới model bên ngoài cần ban tổ chức đồng ý.
- Tùy chọn: OAuth app của Google và GitHub để đăng nhập mạng xã hội.

#### Region

Mọi thứ chạy ở **Asia Pacific (Singapore) `ap-southeast-1`**, trừ chứng chỉ CloudFront và WAF web ACL: CloudFront bắt buộc hai thứ này ở **US East (N. Virginia) `us-east-1`**.
