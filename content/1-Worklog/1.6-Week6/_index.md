---
title: "Week 6 Worklog"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

- Learn Terraform from basics to modules: init/plan/apply/destroy, variables, outputs, state, modules.
- Package apps with Docker, understand backup/restore strategy and automate cost saving with Lambda.
- Build the first infrastructure for the ingredient-management topic (Smart Kitchen) with Terraform.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Terraform Get Started (AWS) tutorial<br>- Read the Terraform Series (Viblo), IaC basics<br>- Practice: installed Terraform, created/changed/destroyed EC2 + S3 with code | 10/19/2026 | 10/19/2026 | [HashiCorp – Terraform Get Started (AWS)](https://developer.hashicorp.com/terraform/tutorials/aws-get-started)<br>[Viblo – Terraform Series part 1](https://viblo.asia/p/terraform-series-bai-1-infrastructure-as-code-va-terraform-maGK7Bqa5j2)<br>[Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |
| Tue | - Read the Terraform Language Documentation:<br>&emsp;+ module, locals, data source<br>&emsp;+ count / for_each, depends_on, lifecycle<br>- Read terraform-aws-modules sample code | 10/20/2026 | 10/20/2026 | [Terraform Language Documentation](https://developer.hashicorp.com/terraform/language)<br>[Terraform – Modules](https://developer.hashicorp.com/terraform/language/modules)<br>[terraform-aws-modules (GitHub)](https://github.com/terraform-aws-modules) |
| Wed | - Deploy Application on Docker workshop:<br>&emsp;+ Ran the app locally, set up VPC/SG/IAM role for ECR<br>&emsp;+ RDS for the app, EC2 configuration<br>&emsp;+ Deployed with a Docker image and Docker Compose<br>&emsp;+ Pushed the image to ECR / Docker Hub | 10/21/2026 | 10/21/2026 | [000015 – Docker](https://000015.awsfcaj.com/)<br>[Docker Get started](https://docs.docker.com/get-started/) |
| Thu | - Deploy AWS Backup to the System workshop:<br>&emsp;+ Created a backup plan<br>&emsp;+ Notifications for backup jobs/failures<br>&emsp;+ Restore test | 10/22/2026 | 10/22/2026 | [000013 – 1. Introduction](https://000013.awsfcaj.com/1-introduce/)<br>[000013 – 3. Create Backup plan](https://000013.awsfcaj.com/3-createbackupplan/)<br>[000013 – 4. Set up notifications](https://000013.awsfcaj.com/4-enablenoti/)<br>[000013 – 5. Test Restore](https://000013.awsfcaj.com/5-testrestore/) |
| Fri | - Optimizing EC2 Costs with Lambda workshop:<br>&emsp;+ Tagged instances<br>&emsp;+ IAM role for Lambda<br>&emsp;+ Lambda stop/start instances, Slack notifications<br>&emsp;+ Testing and cleanup | 10/23/2026 | 10/23/2026 | [000022 – EC2 cost optimization with Lambda](https://000022.awsfcaj.com/) |
| Sun | - Wrote Terraform modules: vpc, security_groups, ec2, rds, secrets, budgets<br>- Prepared the Spring Boot backend config: Dockerfile, SQL schema, secret loading<br>- Wrote a cost-saver script (stop EC2/RDS off-hours) and an RDS DR drill script | 10/25/2026 | 10/25/2026 | 3-tier Terraform code (archived)<br>Backend configuration<br>Operations scripts<br>[Terraform AWS Provider – aws_db_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance)<br>[000003 – 7. VPC IaC template (network reference)](https://000003.awsfcaj.com/7-infrastructureascode/) |

### Week 6 Achievements:

- Completed the Terraform Get Started (AWS) tutorial and the Vietnamese Terraform series; can write modules with inputs/outputs.
- Packaged an app with Docker, ran it with Docker Compose and pushed the image to Amazon ECR.
- Created a backup plan with AWS Backup and tested a restore; understand RPO/RTO.
- Wrote Lambdas that stop/start EC2 by tag and schedule.
- Built a 3-tier Terraform stack: VPC (public, private app, private DB, NAT, Flow Logs), tiered Security Groups, EC2 with an instance profile, RDS PostgreSQL, Secrets Manager, Budgets.
- Prepared the Spring Boot backend configuration (Dockerfile, SQL schema, secret loading) and 2 ops scripts: cost-saver and an RDS DR drill.
