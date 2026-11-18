---
title: "Amazon Route 53"
weight: 3
chapter: false
pre: "<b> 5.7.3. </b> "
---

# 5.7.3. Amazon Route 53

## Purpose and BTB use case

Resolve the frontend hostname to CloudFront and API hostname to the ALB. Route 53 is DNS, not a proxy; HTTPS termination occurs at CloudFront and ALB.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Verify which registrar/DNS service is authoritative for the group domain before changing DNS.
2. Open **Route 53 → Hosted zones** and create/use a **public hosted zone** for the authorized domain.
3. Update the registrar name servers only if moving authoritative DNS to Route 53 and after verifying existing records.
4. Add **A/AAAA Alias** records (as applicable) for web hostname → CloudFront distribution and API hostname → ALB.
5. Wait for DNS propagation; query public resolvers and test HTTPS for both subdomains.
6. Record hosted zone cost and DNS query volume assumptions in the cost worksheet.

## CLI or query example

```powershell
nslookup <FRONTEND_DOMAIN>
nslookup <API_DOMAIN>
# Then verify both names in a browser using https://
```

## Validation checklist

- [ ] DNS resolves frontend and API records to intended AWS targets.
- [ ] Certificate hostnames match, and HTTPS serves the correct apps.

## Pitfalls / security

Do not create conflicting public DNS records or claim a domain is owned without confirmation.

## Required personal screenshots

**Capture after execution:** Hosted zone, Alias records and `nslookup` results.

Use filenames like `static/images/5-workshop/5.7.3-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.7.3-01.png" alt="Amazon Route 53 evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html>
