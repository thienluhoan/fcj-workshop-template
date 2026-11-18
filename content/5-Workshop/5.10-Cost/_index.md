---
title: "Cost Estimation & Optimization"
weight: 10
chapter: false
pre: "<b> 5.10. </b> "
---

# 5.10. Cost Estimation & Optimization

**Required submission artifact:** a dated estimate from **[AWS Pricing Calculator](https://calculator.aws/)** using the same Region and workloads as your documented environment. **Do not invent monthly prices.** Include a link/export or screenshots with assumptions.

## Fill in the actual workload assumptions

| Component | Required assumption | Your value |
|---|---|---|
| ECS/Fargate | vCPU, GiB memory, runtime hours, task count | TODO |
| Application Load Balancer | ALB hours and LCU usage | TODO |
| Amazon ECR | Image size, number of image versions, storage | TODO |
| Amazon S3 | Stored GB, requests, outgoing traffic | TODO |
| Amazon CloudFront | Regional outbound GB, HTTPS requests | TODO |
| Route 53 | Hosted zones and query count | TODO |
| Secrets Manager | Number of secrets, API requests | TODO |
| CloudWatch Logs | Ingestion GB and retention/storage | TODO |
| AWS WAF | Web ACLs, rules/rule groups, requests | TODO |
| Networking egress (shared) | NAT Gateway / VPC endpoints if deployed by teammate | TODO (coordinate) |

Record any public IPv4, cross-AZ, data-transfer and other relevant charges. Shared resources from teammates still affect the **total project** estimate, even when their setup is not covered by this individual lab.

## Optimization actions

1. Tag resources and review actual spend/forecast with AWS Budgets.
2. Avoid unnecessary always-on lab tasks/ALBs after demonstrations.
3. Right-size ECS CPU/memory after load observations; use multi-task/Multi-AZ only when needed and documented.
4. Set CloudWatch log retention, ECR lifecycle policy and review S3 objects/version history.
5. Compare the cost/availability trade-offs of outbound network design with the teammate; do not remove required egress to Supabase.

**Submit:** pricing estimate screenshot/export, calculator assumptions, revised total and a short explanation of trade-offs. There is no reliable total until inputs and Region are confirmed.
