---
title: "Amazon ECS with AWS Fargate"
weight: 3
chapter: false
pre: "<b> 5.6.3. </b> "
---

# 5.6.3. Amazon ECS with AWS Fargate

## Purpose and BTB use case

Run the Spring Boot modular monolith without managing EC2 hosts. ECS handles desired tasks and deployment health; business transactions remain governed by Spring Boot/PostgreSQL.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Open **ECS → Clusters → Create cluster**, name a cluster and use Fargate capacity as appropriate.
2. Register a **Task definition** compatible with Fargate; choose Linux architecture matching ECR image, valid CPU/memory sizes and `awsvpc` networking.
3. Set task execution role and optional application task role distinctly. Add ECR image URI with immutable tag/digest, `BACKEND_PORT`, non-secret environment values and secret ARNs.
4. Configure the `awslogs` log driver pointing to a created CloudWatch Log Group (or permitted automatic group creation).
5. Create an ECS **Service** in the cluster; choose private subnets, ECS SG, no public IP for the private design, desired count based on cost/availability, and attach ALB target group.
6. Confirm teammate-managed egress allows image pulling, AWS APIs and external Supabase/TLS before troubleshooting startup.
7. Inspect Tasks, Events, Deployments, container logs and ALB target health; only claim HA with actual multi-task/multi-AZ evidence.
8. Run Flyway schema migrations under a controlled policy; avoid assuming multi-task concurrent startup migrations are safe.

## CLI or query example

```bash
# Observe actual service and task status. Do not copy sample IDs.
aws ecs describe-services --cluster <CLUSTER_NAME> --services <SERVICE_NAME> --region <AWS_REGION> --query 'services[0].{status:status,running:runningCount,desired:desiredCount}'
```

## Validation checklist

- [ ] Desired/running tasks converge; no persistent task failures.
- [ ] ALB target health is healthy and API responds; external DB connectivity is verified.

## Pitfalls / security

One Fargate task is cost-conscious but not task-level highly available. Do not describe a modular monolith as multiple microservices.

## Required personal screenshots

**Capture after execution:** Cluster, task definition, service deployment, running tasks, ALB health, CloudWatch logs.

Use filenames like `static/images/5-workshop/5.6.3-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.6.3-01.png" alt="Amazon ECS with AWS Fargate evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html>
