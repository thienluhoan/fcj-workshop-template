---
title: "Edge và bảo mật"
date: 2026-11-27
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Stack `fcaj-v3-edge` triển khai ở **us-east-1**, vì CloudFront chỉ dùng chứng chỉ ACM và WAF web ACL ở region này. Template có rule từ chối mọi region khác.

#### Bước 1. Build và tải frontend lên

```powershell
cd front-end/src/seal-web
npm ci
$env:VITE_API_URL = 'https://<DOMAIN>'
npm run build
```

File asset có hash được cache một năm, `index.html` không bao giờ cache, nên bản mới hiện ra ngay:

```powershell
aws s3 sync dist s3://fcaj-v3-web-<ACCOUNT_ID>-ap-southeast-1 --exclude index.html --cache-control "public,max-age=31536000,immutable"
aws s3 cp dist/index.html s3://fcaj-v3-web-<ACCOUNT_ID>-ap-southeast-1/index.html --cache-control "no-cache,max-age=0,must-revalidate"
```

#### Bước 2. Triển khai stack edge

Quay về thư mục gốc của repository. `<WEB_BUCKET_DOMAIN>` là output `WebBucketDomain` của stack base; biến `$ORIGIN` vẫn còn từ bước [5.5](../5.5-compute-origin/).

```powershell
aws cloudformation deploy --region us-east-1 --stack-name fcaj-v3-edge --template-file infra/v3/edge.yaml --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> WebBucketDomain=<WEB_BUCKET_DOMAIN> OriginSecret=$ORIGIN
```

CloudFront mất 5–15 phút. Template tạo:

**Chứng chỉ ACM** cho `<DOMAIN>`, xác thực trong hosted zone.

**Hai origin**

| Origin | Đích | Bảo vệ |
|---|---|---|
| `web` | Bucket web S3 private | Origin Access Control (SigV4). Bucket policy chỉ cho distribution này đọc. |
| `api` | `origin.<DOMAIN>` (ALB), chỉ HTTPS, TLS 1.2 | Header riêng `X-Origin-Verify` mang origin secret |

**Behavior**

| Đường dẫn | Origin | Cache | Ghi chú |
|---|---|---|---|
| Mặc định `*` | web | CachingOptimized | CloudFront Function đổi đường dẫn không có đuôi file thành `/index.html` (deep link của SPA) |
| `/api/*` | api | CachingDisabled, chuyển mọi header | Mọi method |
| `/oauth2/authorization/*`, `/login/oauth2/*` | api | CachingDisabled | Bắt đầu và callback OAuth |
| `/uploads/*` | api | CachingDisabled | Chỉ GET và HEAD |

{{% notice note %}}
Không route cả `/oauth2/*` hay `/login/*`: React đang dùng `/oauth2/redirect` và `/login`. Ngoài ra, cách quen thuộc "403/404 → index.html" bằng custom error page sẽ biến mọi lỗi API thành trang HTML 200; CloudFront Function chỉ gắn vào behavior S3, nên lỗi API vẫn là JSON.
{{% /notice %}}

**AWS WAF web ACL `fcaj-v3`**

| Ưu tiên | Rule | Loại | Mục đích |
|---|---|---|---|
| 0 | `ip-reputation` | AWS managed `AmazonIpReputationList` | IP độc hại đã biết |
| 1 | `known-bad-inputs` | AWS managed `KnownBadInputsRuleSet` | Log4j, path traversal và payload tương tự |
| 2 | `common` | AWS managed `CommonRuleSet` | Rule kiểu OWASP Top 10. `SizeRestrictions_BODY` để Count vì ứng dụng nhận file tới 20 MB. |
| 3 | `login-attempts` | Rate-based, 100 request/5 phút/IP | `POST` tới login, register, forgot-password, verify-reset-otp, reset-password: chống dò mật khẩu và OTP |
| 4 | `login-rate` | Rate-based, 300 request/5 phút/IP | Mọi request dưới `/api/auth/`, `/oauth2/`, `/login/`. Cả lớp học dùng chung một IP public, nên giới hạn này để rộng. |

`WafMode` và `LoginLimitMode` đều bắt đầu ở **Count**. Chạy một vòng kiểm thử, đọc sampled requests, rồi chuyển sang **Block**:

```powershell
aws cloudformation deploy --region us-east-1 --stack-name fcaj-v3-edge --template-file infra/v3/edge.yaml --parameter-overrides WafMode=Block LoginLimitMode=Block
```

**DNS**: bản ghi alias A và AAAA từ `<DOMAIN>` tới distribution. Bật IPv6 và HTTP/2, chuyển người dùng sang HTTPS với tối thiểu TLS 1.2, managed policy SecurityHeaders thêm HSTS và các header liên quan.

![Tab Behaviors của distribution CloudFront](/images/5-Workshop/5.6-Edge-Security/cloudfront-behaviors.png)

*Năm behavior của distribution, CloudFront xét theo thứ tự **Precedence** từ 0 và dùng behavior đầu tiên khớp:*

- *0–3 (`/api/*`, `/oauth2/authorization/*`, `/login/oauth2/*`, `/uploads/*`) đi tới origin **api** (ALB) với **CachingDisabled**, vì đây là dữ liệu động và riêng từng người dùng. **AllViewer** chuyển nguyên header, cookie và query string của người dùng tới backend, nên phiên đăng nhập và CSRF hoạt động.*
- *4, **Default (\*)**, đi tới origin **web** (S3 qua OAC) với **CachingOptimized**: file tĩnh của React được cache tại edge.*
- *Mọi behavior đều **Redirect HTTP to HTTPS**. **SecurityHeadersPolicy** (HSTS, `X-Content-Type-Options`, `X-Frame-Options`…) gắn trên API, file upload và trang web. Hai behavior OAuth không gắn policy này, vì response của chúng chỉ là redirect sang Google/GitHub hoặc quay về trang React.*
- *Chỉ route đúng hai đường dẫn OAuth của Spring Security, không mở rộng thành `/oauth2/*` hay `/login/*`, vì `/oauth2/redirect` và `/login` là trang của React ([5.12](../5.12-troubleshooting/)).*
- *ARN (có account ID) đã được che.*

![Các rule của WAF web ACL fcaj-v3](/images/5-Workshop/5.6-Edge-Security/waf-rules.png)

*Web ACL `fcaj-v3` (phạm vi **Global (CloudFront)**) có năm rule, xếp theo thứ tự ưu tiên:*

- *`ip-reputation`, `known-bad-inputs` và `common` là các managed rule group của AWS (ưu tiên 0–2), chặn IP xấu đã biết, mẫu khai thác lỗ hổng và các kiểu tấn công web phổ biến.*
- *`login-attempts` (ưu tiên 3) giới hạn đường dẫn đăng nhập và OTP ở 100 request mỗi IP trong 5 phút. Ban đầu rule chạy ở chế độ Count để xem lưu lượng thật mà không chặn nhầm ai ([5.10](../5.10-testing-validation/)); sau buổi demo rule được chuyển sang **Block** bằng `LoginLimitMode=Block`, nên dưới tên rule không còn dòng *Running in Count mode*.*
- *`login-rate` (ưu tiên 4) chặn IP gửi quá 300 request trong 5 phút.*
- *Nhãn WCU cho biết mỗi rule dùng bao nhiêu capacity của web ACL.*

![Chứng chỉ ACM của tên miền ở us-east-1](/images/5-Workshop/5.6-Edge-Security/acm-certificate.png)

*Chứng chỉ mà CloudFront dùng, nằm trong **ACM, region us-east-1** (CloudFront chỉ nhận chứng chỉ ở region này): trạng thái **Issued**, loại **Amazon Issued**, xác thực bằng bản ghi **DNS** trong Route 53, khóa **RSA 2048**. ACM tự gia hạn trước khi hết hạn, miễn là bản ghi CNAME xác thực vẫn còn trong hosted zone.*

#### Bước 3. Kiểm tra nhanh

```powershell
curl.exe -si https://<DOMAIN>/api/does-not-exist
```

Kết quả phải là JSON 401 hoặc 404 từ API, không phải trang HTML với mã 200. Mở thẳng `https://<DOMAIN>/dashboard` và nhấn F5: trang React phải hiện ra.
