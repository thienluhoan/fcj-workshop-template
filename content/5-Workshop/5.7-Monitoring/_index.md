---
title: "Monitoring, testing and cost"
date: 2026-11-23
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

### Logs and metrics
- **CloudWatch Logs**: each Lambda has a `/aws/lambda/securetable-*` log group kept for 7 days. 403s from the decorator are logged with the user and groups for investigation.
- **Metrics**: API Gateway `Count`, `4XXError`, `5XXError`, `Latency`; Lambda `Duration`, `Errors`, `Throttles`; DynamoDB `ConsumedRead/WriteCapacityUnits`; WAF `BlockedRequests`.
- **Dashboard** `SecureTable-Live-Monitoring` puts these metrics on one screen.

### Alerts
| Alarm | Condition | Sent to |
| --- | --- | --- |
| API 5xx | `5XXError` > 5 in 3 minutes | SNS `securetable-alerts` → email |
| Request spike | `Count` above threshold in 5 minutes | SNS → email |
| AWS Budgets | Forecast 50%, actual 80% and 100% of 10 USD | Email + SNS |

Test an alarm by calling a failing route repeatedly (or temporarily lowering the threshold) and checking the email.

### Load testing
Use Locust to simulate 100 users calling `GET /menu` and `POST /orders`, ramping up over 2 minutes. Record p50/p95 latency, error rate, concurrent Lambda executions and the cost incurred.

*(Results table and dashboard screenshot – team to add after the run)*

### Cost
For one restaurant (about 200,000 requests/month) the estimate is **about 1.5 USD/month with WAF off** and **about 2.1 USD/month** in a month with WAF enabled for 2 days of pentest and demo. See the detailed table in [Proposal – Budget Estimation](../../2-proposal/).
