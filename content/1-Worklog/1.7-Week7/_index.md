---
title: "Week 7 Worklog"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

- Re-evaluate the 3-tier architecture against cost and the program's Serverless criterion.
- Design the DynamoDB data model and the authorization mechanism for the restaurant system.
- Agree on the SecureTable project plan with the team and refactor Terraform to Serverless.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Estimated the 3-tier cost with AWS Pricing Calculator<br>- Read the AWS Well-Architected Framework (6 pillars)<br>- Read the Serverless Applications Lens (Security, Cost Optimization) | 10/26/2026 | 10/26/2026 | [AWS Pricing Calculator](https://calculator.aws/)<br>[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)<br>[Serverless Applications Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)<br>[Amazon VPC – Pricing (NAT Gateway)](https://aws.amazon.com/vpc/pricing/) |
| Tue | - Listed access patterns: ordering, kitchen screen, inventory, finance, audit log<br>- Designed 5 on-demand DynamoDB tables + GSI `status_created_at_idx`<br>- Read the DynamoDB Developer Guide and DynamoDB Guide | 10/27/2026 | 10/27/2026 | [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)<br>[DynamoDB Guide (Alex DeBrie)](https://www.dynamodbguide.com)<br>[000060 – 1.3 Secondary Index](https://000060.awsfcaj.com/1-introduce/1.3-secondaryindexes/) |
| Wed | - Analysed the Cognito → API Gateway → Lambda flow<br>- Found a gap: the authorizer does not authorise by group<br>- Designed 4 groups OWNER / MANAGER / STAFF / KITCHEN and the 401 vs 403 convention | 10/28/2026 | 10/28/2026 | [Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)<br>[API Gateway – Cognito user pool authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)<br>[OWASP Top 10:2025](https://owasp.org/Top10/2025/) |
| Thu | - Finalised the SecureTable project plan with the team<br>- Pushed code to the team repo (initial commit)<br>- Wrote modules `dynamodb`, `apigateway`, `waf_v2`<br>- Moved EC2/RDS/VPC modules to `legacy_3tier_modules`, created `dynamodb_env.example` | 10/29/2026 | 10/29/2026 | Project plan<br>Backend environment variables<br>Terraform modules<br>[Terraform – aws_dynamodb_table](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table)<br>[Terraform – aws_api_gateway_rest_api](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/api_gateway_rest_api) |
| Fri | - Costed WAF: 5 USD/Web ACL + 1 USD/rule → ≈ 9 USD/month<br>- Added an `enable_waf` flag in Terraform (off by default)<br>- Wrote `rbac_decorator.py` and `test_rbac_decorator.py` | 10/30/2026 | 10/30/2026 | [AWS WAF – Pricing](https://aws.amazon.com/waf/pricing/)<br>[AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)<br>Authorization code<br>[pytest documentation](https://docs.pytest.org) |

### Week 7 Achievements:

- Showed the 3-tier design exceeds the budget (EC2 + RDS + NAT running 24/7 ≈ 20–50 USD/month vs a 10 USD cap) → decided to go 100% Serverless.
- Designed 5 on-demand DynamoDB tables (`orders`, `menu_items`, `inventory`, `daily_finance`, `audit_logs`) and the `status_created_at_idx` GSI for the kitchen screen.
- Found that the Cognito authorizer authenticates but does not authorise by group → designed a 4-group RBAC contract and wrote `rbac_decorator.py` with unit tests.
- The team agreed on the SecureTable plan (4 roles, 12 AWS services, 4-person split, 8-week roadmap); code pushed to the team repo.
- Wrote Terraform modules `dynamodb`, `apigateway` (REST, `/health`, CORS, throttling 50 rps/burst 100), `waf_v2` with an `enable_waf` flag; moved EC2/RDS/VPC modules to `legacy_3tier_modules`.
