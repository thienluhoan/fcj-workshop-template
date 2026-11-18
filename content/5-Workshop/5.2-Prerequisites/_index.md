---
title: "Prerequisites"
weight: 2
chapter: false
pre: "<b> 5.2. </b> "
---

# 5.2. Prerequisites

## Accounts and permissions

Prepare a dedicated AWS account/role with permissions narrowly scoped to VPC, IAM, Budgets, ECR, ECS/Fargate, ELB, S3, CloudFront, ACM, Route 53, Secrets Manager, WAF and CloudWatch Logs. Do not use root credentials for daily work. Verify domain ownership and access to the external Supabase database and image provider.

## Tools

- Git, Docker (Linux containers), AWS CLI v2, Node.js/npm, Java 21+ and the backend build tool used in the actual repository.
- React/TypeScript source and Spring Boot source in the team's **approved** Git branches.
- AWS Region, service port, health endpoint, account ID, test domain/subdomains, database host/port, and container architecture selected by the team.
- AWS Pricing Calculator and a way to capture screenshots; never put real tokens, passwords or JWT keys into images.

## Local checks (safe commands)

```powershell
git --version
docker --version
aws --version
node --version
java -version
aws sts get-caller-identity
aws configure list
```

For the website build, follow the frontend repository's scripts (typically `npm ci` then `npm run build`); inspect `package.json` rather than assuming paths. For Spring Boot, select the project's actual Maven/Gradle command and inspect `Dockerfile` before building.

## Configuration worksheet — fill from deployment

| Placeholder | Meaning |
|---|---|
| `AWS_REGION` | AWS Region hosting ECS, VPC, ALB, ECR and logs |
| `ACCOUNT_ID` | AWS account number; do not confuse with secrets |
| `BACKEND_PORT` | Port bound by Spring Boot container |
| `HEALTH_PATH` | Health path allowed for ALB checks (e.g., a deliberately exposed readiness endpoint) |
| `FRONTEND_BUCKET` | Unique, private S3 bucket |
| `FRONTEND_DOMAIN` | Domain for React, e.g. www subdomain |
| `API_DOMAIN` | Subdomain for ALB API |
| `ECR_REPOSITORY` | Backend image repository |

**Dependency:** Private ECS tasks need an approved outbound route to Supabase and, if used, Gemini/Cloudinary. NAT Gateway and VPC endpoints are the teammate's implementation scope; obtain the deployed networking details before starting ECS.

**Evidence to capture:** CLI versions (without secrets), IAM identity, chosen Region and configuration worksheet.
