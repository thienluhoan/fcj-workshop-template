---
title: "Cleanup"
weight: 11
chapter: false
pre: "<b> 5.11. </b> "
---

# 5.11. Cleanup

**Important:** first identify resources **owned by this personal lab** versus shared production/team infrastructure. Do not delete a shared VPC, NAT Gateway, endpoints, CI/CD pipeline, log group or domain used by other people. Confirm deletion permissions with the team. This is an ordered guide, not evidence cleanup has occurred.

1. **Capture evidence first:** record final demo, service state, screenshots and cost calculator output. Back up any essential configuration without copying secrets.
2. **ECS/Fargate:** set backend service desired tasks to 0, delete the service after it drains, then delete the lab cluster/task definition revisions where appropriate.
3. **ALB:** remove lab HTTPS/HTTP listeners, delete the lab ALB and target group once unreferenced; note DNS alias dependencies.
4. **WAF:** disassociate personal Web ACLs from CloudFront/ALB and remove the lab ACL/rules only after confirmed unused.
5. **CloudFront:** remove aliases/associations if required, disable the distribution, wait for deployment, then delete; this can take time.
6. **Route 53/ACM:** remove only test alias/validation DNS records the lab owns; retain real domain/hosted zone if shared. Remove unused certificates after ALB/CloudFront dependencies are gone.
7. **S3:** remove the lab's compiled frontend objects and versions if applicable, then delete its bucket after CloudFront no longer references it. Never delete Cloudinary product assets as part of S3 cleanup.
8. **ECR:** delete only the lab's ECR repository/images after ECS is detached.
9. **Secrets Manager:** schedule deletion of lab-only secrets according to AWS recovery-window rules; ensure no live service still references them.
10. **CloudWatch Logs:** export required diagnostic records under policy, then delete only lab log groups; keep retention if shared.
11. **VPC/IAM:** remove only unused lab security groups, subnets/route tables and VPC **after coordinating** with the teammate managing NAT/endpoints. Delete only lab IAM roles/policies no longer referenced.
12. **Budgets:** keep the budget if monitoring ongoing shared spending; otherwise remove it with permission. Revisit Billing/Cost Explorer to confirm no overlooked resources.

**Verification:** re-list the service inventories and record remaining expected costs (e.g., shared domain/hosted zone). Do not claim charges have stopped until billing catches up.
