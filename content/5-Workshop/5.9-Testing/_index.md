---
title: "Testing, Validation & Demo"
weight: 9
chapter: false
pre: "<b> 5.9. </b> "
---

# 5.9. Testing, Validation & Demo

**This is a test plan, not a record of passed tests.** Replace every expected/result status with actual evidence from a dated run. Do not invent API URLs or claim integrations that remain under design.

| ID | Scenario | Expected observation | Actual / evidence |
|---|---|---|---|
| T01 | Open `https://<FRONTEND_DOMAIN>` | React homepage loads via CloudFront | TODO screenshot + timestamp |
| T02 | Refresh a React nested route | SPA route remains usable | TODO screenshot |
| T03 | Unauthenticated request to known public catalog API | Documented successful response | TODO cURL output |
| T04 | Guest attempts Customer-only endpoint | Authorized 401/403 response per real API | TODO response |
| T05 | Staff-only Quote approval attempted by Customer | Access denied by Spring Security | TODO response |
| T06 | Confirm Quote when any one SKU is insufficient | No successful Order or partial stock reservation | TODO DB/API snapshot |
| T07 | ECS service/tasks and ALB targets | Desired=running, targets healthy | TODO Console screenshot |
| T08 | ECS startup / test error | Logged in CloudWatch without secrets | TODO Logs screenshot |
| T09 | S3 direct anonymous object request | Denied; CloudFront OAC works | TODO HTTP status |
| T10 | WAF test (only if deployed) | Rule Count/Block matches expectation | TODO sampled request |
| T11 | DNS + TLS for web and API | Hostnames and certs match | TODO browser/CLI evidence |

### Example read-only API commands

```bash
curl -i https://<API_DOMAIN>/<ACTUAL_PUBLIC_CATALOG_PATH>
curl -I https://<FRONTEND_DOMAIN>/
```

Use the **real** catalog path from the Swagger/OpenAPI specification. Do not assume every `/health` endpoint is intentionally public. Use test-only data and redact email/JWT/customer records.

### Operations hand-off

CloudWatch **Metrics/Alarms** belong to the teammate. This lab may reference the evidence they provide for the integrated demo but does **not** set up their alarms. Collect log evidence yourself under 5.8.

### Live demo / video

- **Demo URL:** TODO (verified reachable)
- **Recorded demonstration:** TODO (video or screenshots)
- **Date, Region and application version:** TODO
- **Limitations / failed tests:** TODO (report transparently)
