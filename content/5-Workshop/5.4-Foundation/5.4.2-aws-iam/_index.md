---
title: "AWS IAM"
weight: 2
chapter: false
pre: "<b> 5.4.2. </b> "
---

# 5.4.2. AWS IAM

## Purpose and BTB use case

Separate human deployment access from the ECS task **execution role** (ECR pulls/logs/secret injection) and application **task role** (AWS API access only if needed). Application user roles remain enforced by Spring Security.

> **Deployment status:** Planned instructional steps; do not mark checks complete without real AWS Console evidence.

## Step-by-step lab

1. Open **IAM → Roles**; decide which principals deploy and which roles ECS tasks require. Avoid root user and static long-lived keys.
2. Create ECS **task execution role** trusted by `ecs-tasks.amazonaws.com`; start from narrowly applicable ECS execution permissions and restrict resource ARNs.
3. Grant additional Secrets Manager read and KMS decrypt permissions to the execution role **only** for secrets/keys referenced by ECS container injection.
4. Create a separate **task role** only if Spring Boot needs AWS API calls; do not give it AdminAccess or reuse the execution role.
5. Confirm the human deployer can pass **only approved roles** using restricted `iam:PassRole` where applicable.
6. Review IAM Policy Simulator / task startup results and document role ARNs, policies and trust principals without publishing secrets.



## Validation checklist

- [ ] Trusted entity for ECS roles is correct.
- [ ] No broad public application privileges or hard-coded credentials.

## Pitfalls / security

Do not confuse AWS IAM policies with Guest/Customer/E-commerce/Logistics/Delivery/Admin authorization.

## Required personal screenshots

**Capture after execution:** Execution-role trust and permissions, task-role permissions and least-privilege notes.

Use filenames like `static/images/5-workshop/5.4.2-01.png`. Do not upload credentials or customer data. Add a screenshot using `{{</* workshop-image src="images/5-workshop/5.4.2-01.png" alt="AWS IAM evidence" caption="Recorded from actual deployment" */>}}` **only after the file exists**.

**AWS documentation:** <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html>
