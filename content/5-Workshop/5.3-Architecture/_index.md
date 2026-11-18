---
title: "Architecture & Data Flow"
weight: 3
chapter: false
pre: "<b> 5.3. </b> "
---

# 5.3. Architecture & Data Flow

## Draw.io diagram — personal completion required

Create the **final diagram yourself** using the AWS Architecture Icons library in draw.io. Include AWS Cloud boundary, AWS Region, VPC boundary, public and private subnets in two Availability Zones (if actually configured), external systems, request arrows and ownership annotations. Save the manually reviewed `.drawio` source and export a clear PNG. Do not submit a fully AI-generated draft as your own final architecture.

**Image placeholder:** `static/images/5-workshop/architecture/btb-architecture-final.png` (add a real, reviewed image and insert the provided `workshop-image` shortcode).

## Target logical request flows

```text
Frontend: Browser → Route 53 DNS → CloudFront (+ WAF if attached)
                → private S3 origin (OAC) → React/TypeScript assets
API:      Browser → Route 53 DNS → HTTPS ALB (regional WAF if attached)
                → ECS Fargate tasks in VPC → Spring Boot modules
Data:     Spring Boot → Supabase PostgreSQL (external; TLS)
Images:   Browser/Backend → Cloudinary (external, per application policy)
AI:       Spring Boot → Gemini/Spring AI (external, only if enabled)
Map:      Browser/Backend → Vietmap (external, when enabled)
Ops:      ECS image ← ECR; task secrets ← Secrets Manager; logs → CloudWatch Logs
```

**Networking caveat:** Public ALB routing does not provide internet access to private ECS tasks. Obtain the teammate's implemented NAT Gateway/VPC endpoint egress design and verify that it supports external Supabase/Gemini/Cloudinary connections. Endpoints alone cannot reach arbitrary external providers.

**TLS caveat:** CloudFront's viewer certificate is in `us-east-1`; the ALB listener's ACM certificate is in the ALB Region. When the frontend and API use separate hostnames, explain both HTTPS paths rather than incorrectly drawing all API traffic through CloudFront.

## Ownership boundaries

| Owner | Services |
|---|---|
| **This workshop** | VPC, IAM, Budgets, ECR, ECS/Fargate, ALB, S3, CloudFront, Route 53, ACM, Secrets Manager, WAF, CloudWatch Logs |
| **Teammate — architecture context only** | Bedrock, SQS, SES, CodePipeline/CodeBuild, CloudWatch Metrics/Alarms, CloudTrail, NAT Gateway/VPC endpoints |
| **External integrations** | Supabase PostgreSQL, Cloudinary, Gemini, Vietmap |

The application retains role authorization, quantity-tier pricing, Quote approval, all-or-nothing stock reservation and Order lifecycle inside Spring Boot and its database; AWS components supply hosting, networking and operations.

## Security and reliability

- ECS tasks do not expose application ports to the public internet; inbound traffic comes from ALB security group.
- IAM roles follow least privilege; external passwords and tokens are stored outside Git and injected safely.
- S3 Block Public Access and CloudFront OAC protect frontend assets.
- ALB health check targets an explicitly allowed safe endpoint.
- For multi-AZ resilience use health checks, properly placed subnets and sufficient desired task count, after evaluating cost and dependencies.
- WAF on CloudFront does **not** automatically protect a separate direct ALB endpoint.

**Personal architecture checklist:** exact AWS icons, labelled Region/AZ/VPC, ingress/egress flow, protocol/port, data stores, AWS vs third-party distinction, cost-bearing components, owner labels, actual screenshots.
