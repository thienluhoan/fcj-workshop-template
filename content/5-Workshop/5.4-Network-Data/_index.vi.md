---
title: "Lớp mạng và dữ liệu"
date: 2026-11-27
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Trong bài này bạn giao tên miền cho Route 53, rồi triển khai hai stack: `fcaj-poc` (bucket ứng dụng, hàng đợi SQS, ECR, log group) và `fcaj-v3-base` (VPC, security group, RDS, bucket web, IAM role, chứng chỉ regional).

#### Bước 1. Tạo hosted zone và trỏ tên miền

Chỉ tạo hosted zone **một lần**. Tạo lần hai sẽ ra bộ name server khác.

```powershell
aws route53 create-hosted-zone --name <DOMAIN> --caller-reference seal-v3-1
```

Ghi lại `Id` (`<ZONE_ID>` là phần `Z…` sau `/hostedzone/`) và bốn dòng `NameServers`.

Ở nhà đăng ký tên miền (nhóm dùng iNET), mở tên miền, chọn **đổi name server** (không phải "thêm bản ghi DNS") và nhập đủ bốn tên `ns-….awsdns-….`. Chờ tới khi lệnh sau trả đúng bốn tên đó, mất từ vài phút tới 24 giờ:

```powershell
nslookup -type=NS <DOMAIN> 8.8.8.8
```

![Name server của tên miền ở iNET đã chuyển sang Route 53](/images/5-Workshop/5.4-Network-Data/inet-nameservers.png)

*Trang quản lý tên miền ở iNET sau khi đổi name server: mục **Quản lý DNS** hiện Amazon Route 53 và iNET không còn giữ bản ghi DNS nào (0), mọi bản ghi từ đây do Route 53 phục vụ. Tên chủ thể đã được che.*

![Hosted zone Route 53, phần đầu danh sách bản ghi](/images/5-Workshop/5.4-Network-Data/hosted-zone.png)

![Hosted zone Route 53, phần còn lại của danh sách bản ghi](/images/5-Workshop/5.4-Network-Data/hosted-zone-records.png)

*Hosted zone public sau khi hoàn thành cả workshop (13 bản ghi). Lúc vừa tạo ở bước này, zone chỉ có **NS** (bốn name server đã nhập ở iNET) và **SOA**. Các bản ghi còn lại do những stack sau tự tạo:*

| Bản ghi | Loại | Tạo ở |
|---|---|---|
| Tên miền gốc → CloudFront | A, AAAA (alias) | [5.6](../5.6-edge-security/), stack edge |
| `origin.` → ALB | A (alias) | [5.5](../5.5-compute-origin/), stack runtime |
| `_…` xác thực chứng chỉ ACM (hai chứng chỉ: us-east-1 và ap-southeast-1) | CNAME | Bước 2 bên dưới và stack edge |
| Ba bản ghi DKIM, `_dmarc`, `mail.` (MX và SPF cho MAIL FROM) | CNAME, TXT, MX | [5.8](../5.8-email-ses/), stack SES |

#### Bước 2. Triển khai stack dữ liệu

`JobVisibilityTimeout=1080` giữ job ẩn trong 6 lần timeout của Lambda (180 giây), đúng khuyến nghị của AWS cho SQS event source.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-poc --template-file infra/cloudformation/poc.yaml --parameter-overrides JobVisibilityTimeout=1080
```

Stack tạo bucket ứng dụng `fcaj-poc-data-<ACCOUNT_ID>-ap-southeast-1`, hai hàng đợi `fcaj-poc-ai-jobs`, `fcaj-poc-ai-results` kèm dead-letter queue (nhận lỗi 5 lần thì message chuyển sang DLQ), ECR repository `fcaj-poc-backend` và một log group.

![Đề bài được tải lên bucket ứng dụng](/images/5-Workshop/5.4-Network-Data/s3-problems.png)

*Bucket ứng dụng sau khi điều phối viên tải đề bài của một track lên:*

- *File được lưu theo dạng `poc/problems/<eventId>/<trackId>/<UUID ngẫu nhiên>.pdf`, ở đây là sự kiện 3, track 5. Tên file gốc không được dùng làm key, nên không ai đoán được đường dẫn và hai file trùng tên không ghi đè nhau.*
- *Đang bật **Show versions**: bucket có versioning, nên mỗi object có một **Version ID**. Thư mục này có file tải lên hôm nay và một file cũ từ hôm trước, mỗi file một key riêng.*
- *Các prefix khác trong `poc/` chứa avatar và snapshot của AI job (`inputs/`, `rubrics/`, `results/`, xem [5.7](../5.7-ai-worker/)).*
- *Account ID trong tên bucket đã được che.*

![Avatar người dùng trong bucket ứng dụng](/images/5-Workshop/5.4-Network-Data/s3-avatars.png)

*Prefix `poc/avatars/` sau khi người dùng đổi ảnh đại diện:*

- *Mỗi avatar lưu dưới tên `<UUID ngẫu nhiên>.<đuôi>`. Backend nhận file từ 1 byte tới 5 MB và xác định loại ảnh (png, gif, webp hoặc jpg) theo nội dung file, không theo tên file.*
- *Bucket vẫn private. Trình duyệt tải `https://<DOMAIN>/uploads/avatars/<tên>`; behavior `/uploads/*` chuyển request tới ALB, backend đọc object từ S3 rồi trả về. Response có `Cache-Control: no-cache`, nên đổi ảnh là thấy ngay.*
- *Khi người dùng thay avatar, object cũ chỉ bị xóa sau khi thay đổi trong database commit thành công; nếu rollback thì ảnh vừa tải lên bị xóa.*
- *Account ID trong tên bucket đã được che.*

#### Bước 3. Triển khai stack base

Security group của ALB chỉ cho CloudFront đi vào. Tìm prefix list origin-facing của CloudFront trong region:

```powershell
aws ec2 describe-managed-prefix-lists --region ap-southeast-1 --filters Name=prefix-list-name,Values=com.amazonaws.global.cloudfront.origin-facing --query "PrefixLists[0].PrefixListId" --output text
```

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-base --template-file infra/v3/base.yaml --capabilities CAPABILITY_IAM --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> CloudFrontPrefixListId=<PL_ID>
```

Mất khoảng 10–20 phút. Template tạo:

- VPC `10.20.0.0/16` với sáu subnet ở hai AZ (public, app, DB) và ba route table. Route table của DB không có đường ra Internet.
- **S3 Gateway Endpoint** gắn vào route table của app, để traffic S3 không tốn phí NAT.
- Ba security group nối chuỗi: ALB ← prefix list CloudFront cổng 443, API ← ALB cổng 8443, DB ← API cổng 3306.
- **RDS MySQL 8.4** `db.t4g.micro`, 20 GB gp3, mã hóa, không public, bật deletion protection, tự chụp snapshot khi xóa, mật khẩu master do AWS quản lý trong Secrets Manager. Parameter group đặt `lower_case_table_names=1` và `utf8mb4`, hai giá trị chỉ đặt được lúc tạo.
- Bucket web private (versioning, mã hóa, Block Public Access) cho frontend.
- IAM role và instance profile cho các máy API.
- Chứng chỉ ACM cho `origin.<DOMAIN>`, xác thực bằng DNS trong hosted zone.

{{% notice tip %}}
Nếu stack chờ lâu ở `RegionalCertificate`, nghĩa là name server ở nhà đăng ký chưa phải của Route 53. Sửa lại bước 1, chứng chỉ sẽ tự xác thực.
{{% /notice %}}

Xem output; bạn sẽ cần `DatabaseAddress`, `WebBucketDomain` và `MasterSecretArn`:

```powershell
aws cloudformation describe-stacks --region ap-southeast-1 --stack-name fcaj-v3-base --query "Stacks[0].Outputs[].[OutputKey,OutputValue]" --output table
```

![Resource map của VPC fcaj-v3](/images/5-Workshop/5.4-Network-Data/vpc-resource-map.png)

*Resource map của VPC `fcaj-v3` (10.20.0.0/16) sau bước 2: sáu subnet chia đều cho hai AZ, mỗi tầng một route table.*

- `v3-public`: route mặc định ra Internet Gateway, dùng cho ALB và NAT gateway.
- `v3-app`: ba route, gồm local, `0.0.0.0/0` qua NAT gateway và prefix list S3 qua Gateway Endpoint (`vpce-…`), nên traffic tới S3 không đi qua NAT.
- `v3-db`: chỉ có route local, database không có đường ra Internet.
- Route table không có subnet nào gắn vào là route table chính (main) AWS tạo kèm VPC, không dùng tới.
- Ở cấu hình ngày thường chỉ có một NAT gateway. Mùa thi bật thêm NAT gateway thứ hai cho AZ B ([5.11](../5.11-cost/)).
![RDS fcaj-v3-mysql, tab Connectivity & security](/images/5-Workshop/5.4-Network-Data/rds.png)

*Database `fcaj-v3-mysql` ở trạng thái **Available**: MySQL Community, class `db.t4g.micro`, đặt ở `ap-southeast-1a` (ngày thường chạy Single-AZ). **Internet access gateway: Disabled** nghĩa là database không nhận kết nối từ Internet; chỉ máy API trong security group `sg-app` vào được cổng 3306. Endpoint và tên user quản trị đã được che.*

#### Bước 4. Lưu cấu hình vào Parameter Store

Script của nhóm tự sinh mật khẩu database và origin secret, không in ra màn hình, không ghi đè tham số đã có. Thêm `-DryRun` để xem trước.

```powershell
.\infra\v3\put-parameters.ps1 -Domain <DOMAIN> -Release "<image@digest> <git sha>"
```

| Tham số | Loại | Nội dung |
|---|---|---|
| `/fcaj/v3/backend/runtime-env` | SecureString | Spring profile, JDBC URL có `sslMode=VERIFY_IDENTITY`, login DB của app, JWT secret, bucket và URL hàng đợi, các cờ tính năng |
| `/fcaj/v3/backend/migration-env` | SecureString | Login DB riêng để migrate schema |
| `/fcaj/v3/backend/release` | String | Image URI ghim theo digest và Git SHA |
| `/fcaj/v3/edge/origin-secret` | SecureString | Giá trị header `X-Origin-Verify` |
| `/fcaj/v3/worker/deepseek-api-key` | SecureString | Tự nhập trên console ở bước [5.7](../5.7-ai-worker/) |

Giá trị release lấy từ lần build image đầu tiên. Build và đẩy image như bước 1 của [5.9](../5.9-cicd/), hoặc để pipeline làm sau khi đã có stack.

{{% notice warning %}}
Không dán các giá trị này vào chat, ticket hay Git. Ứng dụng dùng login riêng (`fcaj_v3_runtime`, `fcaj_v3_migration`), không dùng master user của RDS vì AWS tự xoay mật khẩu của user đó.
{{% /notice %}}
