---
title: "Email với SES"
date: 2026-11-27
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

SEAL gửi mã đặt lại mật khẩu và thông báo cuộc thi. Amazon SES gửi các email này từ `no-reply@<DOMAIN>`, một địa chỉ chỉ gửi trên tên miền của nhóm.

#### Bước 1. Triển khai stack SES

`TestRecipient` là một địa chỉ email của bạn; SES sẽ gửi link xác minh tới đó.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-ses --template-file infra/v3/ses.yaml --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> TestRecipient=<YOUR_EMAIL>
```

Template tạo các bản ghi sau trong hosted zone Route 53:

| Bản ghi | Mục đích |
|---|---|
| Domain identity với **Easy DKIM** (3 bản ghi CNAME) | Thư được ký, bên nhận kiểm tra được thư thật sự từ tên miền |
| Custom MAIL FROM `mail.<DOMAIN>` (MX + SPF) | Căn chỉnh SPF cho thư trả về |
| DMARC `p=none` | Bên nhận báo cáo kết quả, chưa từ chối thư |

Bấm link SES gửi tới `TestRecipient`. Trong **SES → Identities**, tên miền phải hiện **Verified** và DKIM **Successful**.

![Domain identity SES, DKIM và MAIL FROM](/images/5-Workshop/5.8-Email-SES/ses-identity.png)

*Domain identity trong SES (ap-southeast-1):*

- *Identity status **Verified**.*
- *DKIM **Successful**, dùng Easy DKIM với khóa RSA 2048-bit.*
- *Custom MAIL FROM `mail.<DOMAIN>` **Successful**, nên domain nhận thư trả về khớp với SPF.*
- *ARN của identity đã được che. Tài khoản vẫn ở SES sandbox, nên chỉ gửi được tới địa chỉ đã xác minh.*

#### Bước 2. Gửi thử một email

```powershell
aws sesv2 send-email --region ap-southeast-1 --from-email-address no-reply@<DOMAIN> --destination "ToAddresses=<YOUR_EMAIL>" --content "Simple={Subject={Data=SEAL test},Body={Text={Data=Hello from SES}}}"
```

Ở thư nhận được, mục **Show original** phải hiện `DKIM: PASS` và `SPF: PASS`.

Khi backend đã được cấu hình địa chỉ gửi, ứng dụng tự gửi thư qua cùng identity này. Ví dụ dưới đây: điều phối viên duyệt một tài khoản mới và người dùng nhận được thông báo:

![Email duyệt tài khoản do SEAL gửi qua SES](/images/5-Workshop/5.8-Email-SES/account-approved-email.png)

*Email **Account approved** từ `no-reply@<DOMAIN>`, do backend gửi qua SES khi điều phối viên duyệt một đăng ký. Thư vào **Hộp thư đến**, không rơi vào spam, vì DKIM, SPF (custom MAIL FROM) và DMARC đều khớp với tên miền. Account vẫn ở SES sandbox, nên thư chỉ gửi được vì địa chỉ người nhận đã được xác minh trước.*

{{% notice warning %}}
Account SES mới nằm trong **sandbox**: chỉ gửi tới địa chỉ đã xác minh, 200 thư mỗi ngày. Nhóm giữ sandbox trong suốt workshop vì database khôi phục có địa chỉ email thật của sinh viên. Chỉ xin production access sau khi đã làm sạch dữ liệu và cấu hình địa chỉ gửi trong backend.
{{% /notice %}}
