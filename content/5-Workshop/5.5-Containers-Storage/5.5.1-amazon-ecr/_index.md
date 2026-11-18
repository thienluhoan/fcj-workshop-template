---
title: "Amazon ECR"
weight: 1
chapter: false
pre: "<b> 5.5.1. </b> "
---

# 5.5.1. Amazon ECR

## Purpose and BTB use case

Store the Docker image for the BTB Spring Boot modular monolith. ECR is a registry, not the runtime; ECS/Fargate starts the image.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Confirm the Spring Boot Dockerfile builds successfully using the repository’s real backend source directory and Java runtime.
2. Open **Amazon ECR → Private repositories → Create repository**; name it `btb-backend` or the team-approved name, enable scanning/encryption settings as agreed.
3. Use **View push commands** for the Region/account; sign in through the AWS CLI ECR authorization token.
4. Build with the correct platform, tag the image with an immutable version/commit SHA (avoid relying only on `latest`), and push to ECR.
5. Copy the exact image URI/digest into the ECS task definition; document image scan findings and remediate material vulnerabilities.

## CLI or query example

```bash
# Replace every placeholder and run in a shell with Docker + AWS CLI configured.
aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com
docker build -t btb-backend:<IMAGE_TAG> <BACKEND_SOURCE_DIRECTORY>
docker tag btb-backend:<IMAGE_TAG> <ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/btb-backend:<IMAGE_TAG>
docker push <ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/btb-backend:<IMAGE_TAG>
```

## Validation checklist

- [ ] Tagged image is visible in the private repository.
- [ ] Image architecture matches Fargate task runtime.

## Pitfalls / security

Avoid checking Docker credentials or registry tokens into Git.

## Required personal screenshots

**Capture after execution:** ECR repository, image tags and image push terminal output (hide account-sensitive details as needed).

Use filenames like `static/images/5-workshop/5.5.1-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.5.1-01.png" alt="Amazon ECR evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html>
