---
title: "Overview"
weight: 1
chapter: false
pre: "<b> 5.1. </b> "
---

# 5.1. Project Overview

## Business problem

Bite to Business directly sells food to households and organizations such as restaurants, hotels, shops, canteens and cooperatives. It centralizes product browsing, quantity-tier pricing, order requests (Quotes), stock control, packing and deliveries. The website is not a marketplace and does not offer customers a choice of independent sellers.

## Roles and key workflows

| Role | Main responsibility |
|---|---|
| Guest | Browse public products, public pricing and reviews; request registration |
| Customer | Manage Quotes, confirm eligible purchases, track Orders and review products |
| E-commerce Executive | Manage catalog, volume pricing, promotions, review/approve Quotes |
| Logistics Staff | Receive stock, prepare packages, assign deliveries |
| Delivery Staff | View assigned trip, confirm delivery completion |
| Admin | Approve accounts, manage Staff roles and monitor operations |

**Core transaction:** Catalog → choose SKU/quantity → backend calculates Volume Pricing → Customer submits Quote → E-commerce Executive approval → Customer confirms → backend atomically checks/reserves **all** required stock → Order → logistics packages → delivery. If even one line lacks stock, confirmation fails as a whole; a draft Quote does **not** reserve stock.

## Technical scope

- **Frontend:** React, TypeScript, Tailwind CSS, i18n; compiled static files on private Amazon S3 behind CloudFront.
- **Backend:** Java/Spring Boot modular monolith, container image in ECR, ECS tasks on Fargate, ALB as API entry point.
- **Database:** External Supabase PostgreSQL; this workshop does not create Amazon RDS.
- **Media:** Cloudinary hosts product and site images; S3 holds the compiled frontend, not the product-image catalog.
- **AI and maps:** Gemini/Spring AI/A2UI and Vietmap are application-level/external integrations. Amazon Bedrock belongs to the teammate's separate scope and must not be claimed as implemented here.

## Practical success criteria

1. React pages load via HTTPS with a private S3 origin; refresh on a deep link works.
2. Spring Boot exposes an authorized API via HTTPS ALB and stable Fargate tasks.
3. Backend can reach Supabase over its approved secure connection; a known public Catalog endpoint responds.
4. Permissions for Customer vs Staff remain enforced in Spring Security (AWS IAM is not a substitute).
5. ECS startup/health/error logs are visible in CloudWatch; request filtering and DNS behave as configured.
6. Measured deployment evidence, a cost estimate and cleanup procedure are published.

**Not claimed:** real payment-gateway integration, completed AI/Bedrock deployment, or production-grade high availability without measured evidence.

**Evidence to capture:** final React home page; API response; architecture image; ECS/ALB deployment state.
