---
title: "Troubleshooting and lessons learned"
date: 2026-11-27
weight: 12
chapter: false
pre: " <b> 5.12. </b> "
---

These are problems the team met while moving SEAL from a single test server to this architecture, or found while reviewing the design before go-live, and how each was solved.

#### Problems and fixes

| Problem | Cause | Fix |
|---|---|---|
| `fcaj-v3-base` hangs on `RegionalCertificate` | Name servers at the registrar were still the registrar's own | Replace them with the four Route 53 name servers, wait for `nslookup -type=NS` to show them |
| `docker login` to ECR fails on Windows | The ECR token is longer than the 2,560-byte limit of Windows Credential Manager | Log in with a temporary Docker config folder and delete it after the push (the pipeline avoids this on Linux) |
| Google and GitHub sign-in break, avatars return 404 | CloudFront only forwarded `/api/*` | Add behaviors for `/oauth2/authorization/*`, `/login/oauth2/*` and `/uploads/*` |
| `/login` or `/oauth2/redirect` shows a backend error after F5 | Forwarding all of `/oauth2/*` or `/login/*` to the API | Route only the exact backend paths; React owns the rest |
| API errors arrive as HTML with status 200 | Custom error page "403/404 → `/index.html`" applies to every origin | Replace it with a CloudFront Function on the S3 behavior only |
| ALB health check fails | Spring Boot listens on plain HTTP 8080 | nginx terminates HTTPS on 8443 and proxies to loopback 8080; the backend port is never exposed |
| Risk: OAuth fails at random with "invalid state" | OAuth state is kept in the memory of one instance, and the ALB alternates instances | Enable `lb_cookie` stickiness for one hour on the target group |
| Large uploads blocked by WAF | `SizeRestrictions_BODY` in the common rule set limits bodies to 8 KB | Override that one rule to Count; the application itself limits uploads to 20 MB |
| Risk: an AI job runs twice | Lambda timeout (180 s) longer than the queue visibility timeout (120 s) | `JobVisibilityTimeout=1080` (6 × the Lambda timeout); the worker writes results once (if-none-match) and is idempotent |
| AI calls fail after July 2026 | DeepSeek retired `deepseek-chat` | Switch to `deepseek-flash` and disable "thinking" so its tokens do not eat the output limit |
| Risk: repository collection fails during judging | Both instances share one NAT IP and the anonymous GitHub limit of 60 requests per hour | Give the backend a read-only GitHub token through `runtime-env` |
| Risk: the API cannot scale out under contest load | `MinSize` = `DesiredCapacity` = `MaxSize` = 2, so the CPU 60 % policy has no room to add instances | Before the contest raise `MaxSize` (for example to 4) and watch CPU and RDS connections |
| The first release on a fresh database fails | Application DB users do not exist before the restore | Expected; run the restore script, it releases at the end |
| Budgets chart stays at 0 | Credits are subtracted in the default cost view | Untick **Credits** in the budget's cost settings |
| Pricing Calculator shows more than USD 300 | Defaults: `db.m1.large` Multi-AZ, RDS Proxy, Savings Plans for EC2, regional NAT across 5 AZs | Set every field explicitly; compare each line with the price list |

#### Lessons learned

- **Count before Block.** Every WAF rule started in Count mode. Reading the sampled requests showed that a classroom shares one public IP, so the general auth limit was raised to 300 per 5 minutes and a stricter 100 limit was put only on password and OTP endpoints.
- **One entry point.** Putting the API behind the same CloudFront domain removed CORS problems, kept cookies first-party and let one WAF protect everything. The cost is careful path routing.
- **Make the origin private by two locks.** A security group that only accepts the CloudFront prefix list blocks other networks; the secret header blocks other CloudFront distributions.
- **Stateless servers are cheap servers.** Because all state lives in RDS, S3 and Parameter Store, the whole runtime stack can be deleted every evening, and Auto Scaling can replace an instance at any time.
- **Release by digest, not by tag.** A tag can be moved; a digest cannot. The release parameter always names exactly what runs, which makes rollback a one-line change.
- **Asynchronous AI.** Putting a queue between the API and the model kept the judge's screen fast and made model failures visible (DLQ, alarm) instead of hidden in timeouts.
