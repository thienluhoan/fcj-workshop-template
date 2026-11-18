---
title: "AWS Certificate Manager (ACM)"
weight: 1
chapter: false
pre: "<b> 5.7.1. </b> "
---

# 5.7.1. AWS Certificate Manager (ACM)

## Purpose and BTB use case

Issue and validate public TLS certificates for web and API subdomains. CloudFront viewer certificates require ACM in `us-east-1`; ALB certificates must be in the ALB Region.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. List the intended DNS names, such as `www.example.com` and `api.example.com`; verify ownership and existing hosted zone.
2. Open **ACM** in **us-east-1** and request a public certificate for the CloudFront viewer hostname; select DNS validation.
3. Create the provided DNS validation records in the authoritative DNS zone and wait for **Issued**.
4. Switch to the **ALB Region** and request/validate the API hostname certificate if the ALB is in a different Region.
5. Attach the web certificate in CloudFront settings and the API certificate to ALB HTTPS listener; verify certificate hostnames and validity.



## Validation checklist

- [ ] Both relevant certificates show Issued in their correct Regions.
- [ ] HTTPS hosts use the expected certificates.

## Pitfalls / security

A certificate in the wrong Region cannot be selected for the relevant CloudFront/ALB attachment.

## Required personal screenshots

**Capture after execution:** ACM details and DNS validation records (without unnecessary domain-sensitive details).

Use filenames like `static/images/5-workshop/5.7.1-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.7.1-01.png" alt="AWS Certificate Manager (ACM) evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cnames-and-https-requirements.html>
