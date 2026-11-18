---
title: "AWS Budgets"
weight: 1
chapter: false
pre: "<b> 5.4.1. </b> "
---

# 5.4.1. AWS Budgets

## Purpose and BTB use case

Track expected spend before running Fargate, ALB, WAF and data transfer. A budget sends alerts; it does not automatically shut off resources.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Open **Billing and Cost Management → Budgets → Create budget** in the AWS account authorized to view billing.
2. Choose a **Cost budget**, select a monthly period and an estimated budget amount approved by the group (do not invent a USD threshold).
3. Define actual-cost alert thresholds (for example percentages selected by the team), add a monitored email recipient and confirm notification delivery.
4. Set useful tags such as `Project=BTB` on resources; where available, activate cost allocation tags in Billing.
5. Record the budget name, currency, period, threshold, recipient and test/confirmation evidence without publishing personal email addresses.



## Validation checklist

- [ ] Budget configuration is visible and status is active.
- [ ] Compare actual vs forecast cost with the calculator assumptions.

## Pitfalls / security

AWS Budgets cannot guarantee a hard spending cap; review usage regularly.

## Required personal screenshots

**Capture after execution:** Budget detail page, threshold/alert settings, Cost Explorer or cost overview.

Use filenames like `static/images/5-workshop/5.4.1-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.4.1-01.png" alt="AWS Budgets evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html>
