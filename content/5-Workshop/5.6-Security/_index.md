---
title: "Security: WAF and the DevSecOps pipeline"
date: 2026-11-23
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Enable WAF for pentest/demo
```powershell
.\scripts\deploy_and_test.ps1 -EnableWaf
```
The Web ACL `securetable-waf-dev` is attached to the API Gateway stage with 4 rules: `AWSManagedRulesCommonRuleSet`, `AWSManagedRulesSQLiRuleSet`, `AWSManagedRulesKnownBadInputsRuleSet` and `RateLimitPerIP` (default 1,000 requests / 5 minutes / IP).

### Before vs after WAF
```powershell
curl.exe -s -o NUL -w "%{http_code}" "$api/menu?q=' OR '1'='1"
curl.exe -s -o NUL -w "%{http_code}" "$api/menu?q=<script>alert(1)</script>"
```
| Payload | WAF off | WAF on |
| --- | --- | --- |
| `' OR '1'='1` (SQLi) | Reaches the authorizer/Lambda | **403** at WAF |
| `<script>alert(1)</script>` (XSS) | Reaches the authorizer/Lambda | **403** at WAF |
| Request burst over the limit | 429 from throttling | **403** from the rate-based rule |

See blocked requests under **WAF & Shield → Web ACLs → securetable-waf-dev → Sampled requests** and the `BlockedRequests` metric in CloudWatch. After the pentest, run `deploy_and_test.ps1` again (without `-EnableWaf`) to turn WAF off and protect the budget.

### DevSecOps pipeline (GitHub Actions)
The workflow `.github/workflows/devsecops.yml` runs on every pull request. A red job blocks the merge.

| Job | Tools | Blocks when |
| --- | --- | --- |
| Secrets & IaC Security | gitleaks 8.24.3 (SHA256-verified, full history), Checkov, Trivy (pinned by SHA) | Secrets leaked; IaC misconfiguration; CRITICAL/HIGH vulnerabilities |
| Backend Python | pytest, Bandit (`-ll`), Flake8 | Failing tests; medium+ security issues |
| Terraform | `fmt -check`, `validate`, `terraform test` | Format/syntax errors; infrastructure security tests fail |
| Frontend | `npm audit`, build | Critical vulnerabilities in dependencies |

*(Screenshot: pull request Checks tab 4/4 green – team to add)*
