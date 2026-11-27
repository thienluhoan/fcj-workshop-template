---
title: "Bảo mật: WAF và pipeline DevSecOps"
date: 2026-11-23
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Bật WAF khi pentest/demo
```powershell
.\scripts\deploy_and_test.ps1 -EnableWaf
```
Web ACL `securetable-waf-dev` được gắn vào stage API Gateway với 4 rule: `AWSManagedRulesCommonRuleSet`, `AWSManagedRulesSQLiRuleSet`, `AWSManagedRulesKnownBadInputsRuleSet` và `RateLimitPerIP` (mặc định 1.000 request / 5 phút / IP).

### So sánh trước và sau khi bật WAF
```powershell
curl.exe -s -o NUL -w "%{http_code}" "$api/menu?q=' OR '1'='1"
curl.exe -s -o NUL -w "%{http_code}" "$api/menu?q=<script>alert(1)</script>"
```
| Payload | WAF tắt | WAF bật |
| --- | --- | --- |
| `' OR '1'='1` (SQLi) | Request tới authorizer/Lambda | **403** tại WAF |
| `<script>alert(1)</script>` (XSS) | Request tới authorizer/Lambda | **403** tại WAF |
| Gửi dồn dập vượt ngưỡng | 429 do throttling | **403** do rate-based rule |

Xem request bị chặn tại **WAF & Shield → Web ACLs → securetable-waf-dev → Sampled requests** và metric `BlockedRequests` trong CloudWatch. Xong pentest thì chạy lại `deploy_and_test.ps1` (không có `-EnableWaf`) để tắt WAF và giữ ngân sách.

### Pipeline DevSecOps (GitHub Actions)
Workflow `.github/workflows/devsecops.yml` chạy trên mọi pull request. Job đỏ thì không được merge.

| Job | Công cụ | Chặn khi |
| --- | --- | --- |
| Secrets & IaC Security | gitleaks 8.24.3 (kiểm SHA256, quét toàn bộ lịch sử), Checkov, Trivy (ghim SHA) | Lộ secret; cấu hình IaC sai; lỗ hổng CRITICAL/HIGH |
| Backend Python | pytest, Bandit (`-ll`), Flake8 | Test fail; lỗi bảo mật mức medium trở lên |
| Terraform | `fmt -check`, `validate`, `terraform test` | Sai format/cú pháp; test bảo mật hạ tầng fail |
| Frontend | `npm audit`, build | Thư viện có lỗ hổng critical |

*(Ảnh: tab Checks của pull request 4/4 xanh – nhóm bổ sung)*
