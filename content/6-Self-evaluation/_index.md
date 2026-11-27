---
title: "Self-Assessment"
date: 2026-11-28
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

During my internship in the **First Cloud AI Journey program – Amazon Web Services Vietnam Co., Ltd.**, starting 09/14/2026, I learned AWS services systematically through workshops and then applied them to our team project **SecureTable** as the **Cloud Architecture & DevSecOps Lead**.

My main responsibilities were:
- Designing the serverless architecture.
- Writing all infrastructure in Terraform (7 modules, 10 tests).
- Building the DevSecOps pipeline on GitHub Actions.
- Writing the automated deploy and teardown scripts.
- Writing the backend RBAC authorization module.
- Keeping cost within the 10 USD/month budget.

Through this work I improved my skills in Infrastructure as Code, cloud security, CI/CD, cost analysis, technical writing and pull-request-based teamwork.

To reflect my internship objectively, I assessed myself against the following criteria:

| No. | Criteria | Good | Fair | Average | Comment |
| --- | --- | :---: | :---: | :---: | --- |
| 1 | Professional knowledge | ✅ | ☐ | ☐ | Solid AWS fundamentals (IAM, VPC, EC2, S3, RDS, DynamoDB, Lambda, API Gateway, Cognito, KMS, WAF, CloudWatch) applied to a real architecture. I write modular, tested Terraform and can explain why we chose serverless and dropped GuardDuty/CMK. |
| 2 | Self-learning & research | ✅ | ☐ | ☐ | Learned through the First Cloud Journey workshops and official docs. Built my own 100-source Cloud & DevSecOps roadmap. Used the AWS Price List to calculate costs instead of guessing. |
| 3 | Proactiveness | ✅ | ☐ | ☐ | Spotted the WAF cost trap and the authorization gap of the Cognito authorizer, and reviewed the foundation against the rubric and fixed issues before the team built the core. |
| 4 | Discipline & compliance | ☐ | ✅ | ☐ | Followed office hours and rules and attended the required events. However, for a period I did not keep my worklog daily and had to fill it in afterwards. |
| 5 | Communication | ☐ | ✅ | ☐ | Wrote clear technical documents and 3 blog posts. I need to improve presenting architecture verbally and updating the team on progress more often. |
| 6 | Teamwork | ✅ | ☐ | ☐ | Designed a "hand-off contract" (table names, environment variables, RBAC decorator) so Backend/Frontend could work in parallel. Worked through pull requests and never self-merged; waited for the team lead's review. |
| 7 | Problem solving | ✅ | ☐ | ☐ | Handled 6 incidents in one day: red CI from a removed action tag, a test mismatch between OpenTofu and Terraform, sensitive files pushed to the repo, and an access key exposed in a screenshot. For each one I found the root cause and documented the fix. |
| 8 | Contribution to the Workshop | ✅ | ☐ | ☐ | Owned the project's infrastructure, security and CI/CD; the architecture diagram, cost estimate and team summary file; contributed 3 blog posts. |

### Areas for improvement
- **Write the worklog the same day** instead of catching up later.
- **Verbal presentation**: practise explaining the architecture briefly, following the business flow, before going into service details.
- **Handle secrets carefully**: never screenshot while working with credentials; move CI fully to OIDC so no long-lived access keys remain.
- **Verify AI-assisted content**: understand and check every line before it goes into the team repo.
