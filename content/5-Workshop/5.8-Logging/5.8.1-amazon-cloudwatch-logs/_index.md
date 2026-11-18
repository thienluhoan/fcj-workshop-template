---
title: "Amazon CloudWatch Logs"
weight: 1
chapter: false
pre: "<b> 5.8.1. </b> "
---

# 5.8.1. Amazon CloudWatch Logs

## Purpose and BTB use case

Centralize Spring Boot container output from Fargate, ECS startup failure clues and application errors without logging sensitive customer records or credentials. Teammate-owned CloudWatch Metrics/Alarms are outside this lab.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Open **CloudWatch → Logs → Log groups** in the same Region as ECS and create a log group such as `/ecs/btb-backend`.
2. Choose a retention policy based on the team’s data/incident policy, instead of leaving indefinite retention without review.
3. Confirm ECS task definition uses `awslogs` with correct region, log group and stream prefix; execution role must have required permissions.
4. Deploy backend and inspect log streams for Spring Boot readiness, health failures, database connection errors, errors with request correlation IDs, and restart events.
5. Use CloudWatch Logs Insights to query recent errors and response correlations if log fields exist; keep timestamps and query text as evidence.
6. Ensure password, auth headers, JWTs, personal information and database connection secrets are not printed in the logs.

## CLI or query example

```sql
fields @timestamp, @message
| filter @message like /ERROR|Exception/
| sort @timestamp desc
| limit 20
```

## Validation checklist

- [ ] Each running ECS task produces expected log streams.
- [ ] Queries return plausible startup/error records, without leaking secrets.

## Pitfalls / security

Logs are distinct from CloudWatch metrics/alarms, which remain with the teammate.

## Required personal screenshots

**Capture after execution:** Log group, retention settings, log streams and a safe Logs Insights query.

Use filenames like `static/images/5-workshop/5.8.1-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.8.1-01.png" alt="Amazon CloudWatch Logs evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html>
