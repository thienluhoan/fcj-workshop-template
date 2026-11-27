---
title: "Frontend on S3 + CloudFront"
date: 2026-11-23
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Step 1 – Build with the deployed configuration
```powershell
Copy-Item backend-config/deployed_env.env frontend/.env    # contains VITE_API_GATEWAY_URL, VITE_COGNITO_*
cd frontend
npm ci
npm run build                                               # creates dist/
```

### Step 2 – Sync to S3 and refresh the cache
```powershell
$out = Get-Content ../backend-config/deployed_env.json | ConvertFrom-Json
aws s3 sync dist/ "s3://$($out.frontend_s3_bucket)" --delete
aws cloudfront create-invalidation --distribution-id $out.cloudfront_distribution_id --paths "/index.html"
```
Vite already puts a hash in JS/CSS file names, so only `index.html` needs invalidating.

### Step 3 – Verify
- Open `https://<cloudfront_domain_name>`: the UI loads over HTTPS and responses include `Strict-Transport-Security`, `Content-Security-Policy` and `X-Frame-Options`.
- Open the bucket's S3 URL directly: it must return **403 AccessDenied**, because only CloudFront can read the bucket through Origin Access Control.

*(Screenshots: sign-in page via CloudFront and the 403 on direct S3 access – team to add)*
