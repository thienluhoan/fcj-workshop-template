---
title: "Amazon CloudFront"
weight: 2
chapter: false
pre: "<b> 5.7.2. </b> "
---

# 5.7.2. Amazon CloudFront

## Purpose and BTB use case

Distribute the React frontend globally over HTTPS while keeping S3 private via OAC. The backend API remains a **separate ALB hostname** in this lab unless the team explicitly implements CloudFront API origin routing.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Open **CloudFront → Create distribution**, choose the S3 REST origin (not S3 website endpoint).
2. Create/select **Origin Access Control (OAC)** and apply the generated bucket policy granting this distribution access to S3 objects.
3. Set viewer protocol policy to **Redirect HTTP to HTTPS** and default root object to `index.html`.
4. For React SPA routes, configure appropriate custom error handling / routing fallback (`403` or `404` to `/index.html` as appropriate) while avoiding accidental masking of API errors.
5. Add custom web domain (alternate domain name) and ACM viewer certificate from **us-east-1**.
6. Publish and test the CloudFront distribution; invalidate changed assets or apply versioned caching policies when updating frontend.
7. Verify direct public S3 access is denied and frontend requests do not expose API secrets.

## CLI or query example

```bash
# Invalidate after a new frontend upload if needed:
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths '/*'
```

## Validation checklist

- [ ] HTTPS web loads on distribution and custom domain.
- [ ] Deep link reload works; S3 public read denied.

## Pitfalls / security

SPA fallback should be configured for frontend assets and routes, not to silently turn every backend API 404 into HTML.

## Required personal screenshots

**Capture after execution:** Distribution origins, OAC policy, cache behavior, certificate and working React page.

Use filenames like `static/images/5-workshop/5.7.2-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.7.2-01.png" alt="Amazon CloudFront evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html>
