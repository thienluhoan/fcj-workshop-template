---
title: "AWS Secrets Manager"
weight: 1
chapter: false
pre: "<b> 5.6.1. </b> "
---

# 5.6.1. AWS Secrets Manager

## Purpose and BTB use case

Protect Supabase connection details, JWT signing material and enabled third-party API tokens. ECS injects references into the Spring Boot container without storing plaintext in Git.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Inventory sensitive runtime values (DB connection, password, JWT key, Gemini/Cloudinary tokens only if actually enabled).
2. Open **Secrets Manager → Store a new secret**; choose an appropriate secret type/key-value or plaintext JSON and encryption key.
3. Give each secret a meaningful non-production-leaking name, e.g. `btb/demo/backend-db`, then save in the same Region as ECS.
4. Restrict the ECS execution role to the exact secret ARN; if a customer-managed KMS key is used, grant necessary decrypt access.
5. In ECS task definition, use **Secrets → ValueFrom** references for relevant environment variable names; do not echo values in logs.
6. Deploy a task revision, check startup and verify only authorized roles can read secrets; plan rotation/redeployment after secret changes.



## Validation checklist

- [ ] Secret metadata appears without publishing value.
- [ ] ECS task starts with role-based secret access.

## Pitfalls / security

Never publish actual secrets or screenshots showing unmasked values.

## Required personal screenshots

**Capture after execution:** Secret name/ARN, redacted task secret mappings, permission policy.

Use filenames like `static/images/5-workshop/5.6.1-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.6.1-01.png" alt="AWS Secrets Manager evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/secrets-envvar-secrets-manager.html>
