---
title: "Application Load Balancer"
weight: 2
chapter: false
pre: "<b> 5.6.2. </b> "
---

# 5.6.2. Application Load Balancer

## Purpose and BTB use case

Route incoming HTTPS API traffic to healthy Spring Boot tasks. An ALB is the public application entry point while the ECS container port is accessible only from its security group.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Decide `BACKEND_PORT` and a **non-sensitive**, intentionally exposed `HEALTH_PATH`; test it in the built container first.
2. Open **EC2 → Target groups → Create target group**; select **IP addresses** for Fargate/`awsvpc` tasks (not instance targets).
3. Choose application VPC, target port, protocol and health-check path; set thresholds/timeouts after measuring Spring Boot boot time.
4. Open **EC2 → Load Balancers → Create Application Load Balancer**; choose internet-facing IPv4 or dualstack as appropriate, two public subnets/AZs and ALB security group.
5. Create listener/rules directing traffic to the target group. Enable **HTTPS 443** with an ACM certificate **in the ALB Region** when the certificate has been issued in 5.7.1.
6. Use HTTP 80 only as a redirect to HTTPS in the final setup; do not send login credentials over temporary HTTP while testing.
7. After ECS service registers task IPs, inspect target health and test the HTTPS API endpoint.



## Validation checklist

- [ ] Target group type is `ip`; health check becomes healthy after ECS deployment.
- [ ] HTTPS listener serves the correct API host and certificate.

## Pitfalls / security

A CloudFront WAF does not automatically inspect requests going directly to this ALB; attach regional WAF separately if required.

## Required personal screenshots

**Capture after execution:** Target group type/health check, ALB listeners, healthy targets and HTTPS request response.

Use filenames like `static/images/5-workshop/5.6.2-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.6.2-01.png" alt="Application Load Balancer evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/register-multiple-targetgroups.html>
