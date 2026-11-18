---
title: "AWS WAF"
weight: 4
chapter: false
pre: "<b> 5.7.4. </b> "
---

# 5.7.4. AWS WAF

## Purpose and BTB use case

Filter selected malicious or excessive web requests to BTB using Web ACL rules. CloudFront and a direct ALB API are **different** attachment scopes.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Identify whether WAF is actually deployed. If still a planned P1 security feature, document it as proposed and do not fabricate screenshots.
2. Open **AWS WAF → Web ACLs → Create web ACL**, choose scope **CloudFront (Global)** for the frontend distribution (managed in us-east-1).
3. Select conservative managed rule groups and/or a rate-based rule, estimate request load, and decide Count mode before blocking where risk of false positives exists.
4. Associate the Web ACL with the CloudFront distribution and verify its association.
5. If the API is exposed directly via ALB and API filtering is required, create/use a **Regional Web ACL** in the ALB Region and associate it with the ALB separately.
6. Safely test a known rule condition without sending harmful traffic; review sampled requests and avoid accidentally blocking legitimate customer sessions.
7. Record rule, request/count/block behavior and WAF pricing components.



## Validation checklist

- [ ] Web ACL is attached to exactly the intended resources.
- [ ] Sampled requests/counts show expected rule actions.

## Pitfalls / security

WAF on CloudFront does not cover direct ALB requests. WAF can incur Web ACL/rule/request charges.

## Required personal screenshots

**Capture after execution:** Web ACL scope, rules, association, Count/Block sampled requests.

Use filenames like `static/images/5-workshop/5.7.4-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.7.4-01.png" alt="AWS WAF evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html>
