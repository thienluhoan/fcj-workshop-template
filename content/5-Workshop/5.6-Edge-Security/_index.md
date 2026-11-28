---
title: "Edge and security"
date: 2026-11-27
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

The `fcaj-v3-edge` stack is deployed in **us-east-1**, because CloudFront only uses ACM certificates and WAF web ACLs from that region. A rule in the template refuses any other region.

#### Step 1. Build and upload the frontend

```powershell
cd front-end/src/seal-web
npm ci
$env:VITE_API_URL = 'https://<DOMAIN>'
npm run build
```

Hashed assets get a one-year cache, `index.html` is never cached, so a new release shows up at once:

```powershell
aws s3 sync dist s3://fcaj-v3-web-<ACCOUNT_ID>-ap-southeast-1 --exclude index.html --cache-control "public,max-age=31536000,immutable"
aws s3 cp dist/index.html s3://fcaj-v3-web-<ACCOUNT_ID>-ap-southeast-1/index.html --cache-control "no-cache,max-age=0,must-revalidate"
```

#### Step 2. Deploy the edge stack

Go back to the repository root. `<WEB_BUCKET_DOMAIN>` is the `WebBucketDomain` output of the base stack; `$ORIGIN` is still set from [5.5](../5.5-compute-origin/).

```powershell
aws cloudformation deploy --region us-east-1 --stack-name fcaj-v3-edge --template-file infra/v3/edge.yaml --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> WebBucketDomain=<WEB_BUCKET_DOMAIN> OriginSecret=$ORIGIN
```

CloudFront takes 5–15 minutes. The template creates:

**ACM certificate** for `<DOMAIN>`, validated in the hosted zone.

**Two origins**

| Origin | Target | Protection |
|---|---|---|
| `web` | Private S3 web bucket | Origin Access Control (SigV4). The bucket policy only allows this distribution. |
| `api` | `origin.<DOMAIN>` (ALB), HTTPS only, TLS 1.2 | Custom header `X-Origin-Verify` with the origin secret |

**Behaviors**

| Path | Origin | Cache | Note |
|---|---|---|---|
| Default `*` | web | CachingOptimized | CloudFront Function rewrites paths without a file extension to `/index.html` (SPA deep links) |
| `/api/*` | api | CachingDisabled, all viewer headers | All methods |
| `/oauth2/authorization/*`, `/login/oauth2/*` | api | CachingDisabled | OAuth start and callback |
| `/uploads/*` | api | CachingDisabled | GET and HEAD only |

{{% notice note %}}
Do not route `/oauth2/*` or `/login/*` as a whole: the React app owns `/oauth2/redirect` and `/login`. Also, the classic "403/404 → index.html" custom error page would turn every API error into an HTML 200 page; the CloudFront Function applies to the S3 behavior only, so API errors stay JSON.
{{% /notice %}}

**AWS WAF web ACL `fcaj-v3`**

| Priority | Rule | Type | Purpose |
|---|---|---|---|
| 0 | `ip-reputation` | AWS managed `AmazonIpReputationList` | Known malicious IPs |
| 1 | `known-bad-inputs` | AWS managed `KnownBadInputsRuleSet` | Log4j, path traversal and similar payloads |
| 2 | `common` | AWS managed `CommonRuleSet` | OWASP Top 10 style rules. `SizeRestrictions_BODY` is set to Count because the app accepts uploads up to 20 MB. |
| 3 | `login-attempts` | Rate-based, 100 per 5 min per IP | `POST` to login, register, forgot-password, verify-reset-otp, reset-password: password and OTP guessing |
| 4 | `login-rate` | Rate-based, 300 per 5 min per IP | Any request under `/api/auth/`, `/oauth2/`, `/login/`. A whole classroom shares one public IP, so this limit is generous. |

Both `WafMode` and `LoginLimitMode` start in **Count**. Run a round of tests, read the sampled requests, then switch to **Block**:

```powershell
aws cloudformation deploy --region us-east-1 --stack-name fcaj-v3-edge --template-file infra/v3/edge.yaml --parameter-overrides WafMode=Block LoginLimitMode=Block
```

**DNS**: A and AAAA alias records from `<DOMAIN>` to the distribution. IPv6 and HTTP/2 are on, viewers are redirected to HTTPS with a minimum of TLS 1.2, and the managed SecurityHeaders policy adds HSTS and related headers.

![CloudFront distribution Behaviors tab](/images/5-Workshop/5.6-Edge-Security/cloudfront-behaviors.png)

*The distribution's five behaviors. CloudFront evaluates them by **Precedence** from 0 and uses the first match:*

- *0–3 (`/api/*`, `/oauth2/authorization/*`, `/login/oauth2/*`, `/uploads/*`) go to the **api** origin (ALB) with **CachingDisabled**, since the data is dynamic and per user. **AllViewer** forwards the viewer's headers, cookies and query strings to the backend, so sessions and CSRF work.*
- *4, **Default (\*)**, goes to the **web** origin (S3 through OAC) with **CachingOptimized**: React's static files are cached at the edge.*
- *Every behavior uses **Redirect HTTP to HTTPS**. The **SecurityHeadersPolicy** (HSTS, `X-Content-Type-Options`, `X-Frame-Options`…) is attached to the API, uploads and the website. The two OAuth behaviors do not carry it, since their responses are only redirects to Google/GitHub or back to the React page.*
- *Only the two Spring Security OAuth paths are routed, never the wider `/oauth2/*` or `/login/*`, because `/oauth2/redirect` and `/login` are React pages ([5.12](../5.12-troubleshooting/)).*
- *The ARN (which contains the account ID) is masked.*

![WAF web ACL fcaj-v3 rules](/images/5-Workshop/5.6-Edge-Security/waf-rules.png)

*The `fcaj-v3` web ACL (scope **Global (CloudFront)**) lists its five rules in priority order:*

- *`ip-reputation`, `known-bad-inputs` and `common` are AWS managed rule groups (priority 0–2) that block known bad IPs, exploit patterns and common web attacks.*
- *`login-attempts` (priority 3) limits the sign-in and OTP paths to 100 requests per IP in 5 minutes. The rule first ran in Count mode to check real traffic without blocking anyone ([5.10](../5.10-testing-validation/)); after the demo it was switched to **Block** with `LoginLimitMode=Block`, so the *Running in Count mode* label no longer shows under it.*
- *`login-rate` (priority 4) blocks an IP that sends more than 300 requests in 5 minutes.*
- *The WCU badges show how much web ACL capacity each rule uses.*

![ACM certificate for the domain in us-east-1](/images/5-Workshop/5.6-Edge-Security/acm-certificate.png)

*The certificate CloudFront serves, in **ACM, region us-east-1** (CloudFront only accepts certificates from this region): status **Issued**, type **Amazon Issued**, validated through **DNS** records in Route 53, key **RSA 2048**. ACM renews it automatically before it expires, as long as the validation CNAME stays in the hosted zone.*

#### Step 3. Quick check

```powershell
curl.exe -si https://<DOMAIN>/api/does-not-exist
```

The answer must be a JSON 401 or 404 from the API, not an HTML page with status 200. Open `https://<DOMAIN>/dashboard` directly and press F5: the React page must load.
