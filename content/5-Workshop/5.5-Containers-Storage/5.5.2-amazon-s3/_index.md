---
title: "Amazon S3"
weight: 2
chapter: false
pre: "<b> 5.5.2. </b> "
---

# 5.5.2. Amazon S3

## Purpose and BTB use case

Host **only compiled frontend assets** from React/TypeScript. Product/media images stay on Cloudinary. The website bucket remains private and is fetched by CloudFront through Origin Access Control.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. In the frontend source, run the real installation/build scripts (for Vite typically `npm ci` and `npm run build`); inspect generated `dist/`.
2. Open **Amazon S3 → Create bucket**; choose a globally unique bucket name and same deployment region as planned.
3. Keep **Block all public access** enabled. Do not enable public ACLs or public-read bucket policies.
4. Upload the build contents (not the parent source folder) using S3 Console or `aws s3 sync`.
5. After creating CloudFront OAC in 5.7.2, apply the specific CloudFront-generated S3 bucket policy with the distribution ARN.
6. Confirm users access files through CloudFront, while direct unauthenticated S3 object access is denied.

## CLI or query example

```bash
# For a Vite build, after confirming that dist/ is your actual output directory:
npm ci
npm run build
aws s3 sync dist/ s3://<FRONTEND_BUCKET>/ --delete
```

## Validation checklist

- [ ] S3 bucket is private; frontend entry file and hashed assets exist.
- [ ] CloudFront can read using OAC.

## Pitfalls / security

Do not configure a public S3 static-website endpoint as the CloudFront OAC origin.

## Required personal screenshots

**Capture after execution:** S3 bucket Block Public Access, build objects, OAC bucket policy and denied public object request.

Use filenames like `static/images/5-workshop/5.5.2-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.5.2-01.png" alt="Amazon S3 evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html>
