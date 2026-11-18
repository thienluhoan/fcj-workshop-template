---
title: "Amazon VPC"
weight: 3
chapter: false
pre: "<b> 5.4.3. </b> "
---

# 5.4.3. Amazon VPC

## Purpose and BTB use case

Isolate Spring Boot Fargate workloads from public ingress while connecting the public ALB to private ECS tasks. The teammate handles NAT and VPC endpoints; use their verified egress design.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Choose a non-overlapping CIDR plan (example `10.20.0.0/16` **illustrative only**) and a Region; record decisions before creating resources.
2. Open **VPC → Create VPC** and create a VPC with DNS support/hostnames enabled as appropriate for the deployment.
3. Create public subnets in at least two AZs for the ALB and private subnets for ECS tasks if the planned architecture requires it.
4. Attach an Internet Gateway and configure the public subnet route table with a default route to it.
5. Coordinate with the teammate to ensure the **private** route table has the intended outbound path; validate external TLS access to Supabase and image/AI providers.
6. Create **ALB security group** allowing intended viewer ports; create **ECS security group** allowing `BACKEND_PORT` only from the ALB security group.
7. Inspect routes, subnet associations, available IPs and security group sources before creating Fargate service.



## Validation checklist

- [ ] ALB public subnets have IGW routes; ECS private subnets use verified outbound networking.
- [ ] Backend port is not open to `0.0.0.0/0`.

## Pitfalls / security

A public ALB does not provide outbound internet to private ECS; do not create NAT/endpoints in this personal lab, but validate teammate configuration.

## Required personal screenshots

**Capture after execution:** VPC resource map, route tables, subnet list, ALB/ECS security group inbound rules.

Use filenames like `static/images/5-workshop/5.4.3-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.4.3-01.png" alt="Amazon VPC evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html>
