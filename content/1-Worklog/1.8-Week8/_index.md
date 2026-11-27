---
title: "Week 8 Worklog"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

- Finish the architecture diagram, a 4-phase personal plan and the deploy/teardown scripts.
- Apply the team's cost/security decisions to Terraform.
- Review the whole foundation, fix critical issues and make the DevSecOps pipeline blocking on every pull request.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Drew the SecureTable architecture diagram v1.0.0 (draw.io)<br>- Wrote a personal action plan for weeks 1–8 (4 phases)<br>- Wrote `deploy_and_test.ps1` and `nuke_and_teardown.ps1`<br>- Received architecture optimisation proposals from the team lead | 11/02/2026 | 11/02/2026 | Architecture diagram<br>Personal plan<br>Scripts<br>[draw.io](https://www.drawio.com/)<br>[AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)<br>[C4 model](https://c4model.com) |
| Tue | - Evaluated dropping GuardDuty, Secrets Manager, KMS CMK<br>- Chose default encryption (SSE-S3, AWS-owned key)<br>- Designed 3 separate S3 buckets and lifecycle to Glacier<br>- Designed SNS `securetable-alerts` and a 3-threshold Budget | 11/03/2026 | 11/03/2026 | [Amazon GuardDuty – Pricing](https://aws.amazon.com/guardduty/pricing/)<br>[AWS KMS – Pricing](https://aws.amazon.com/kms/pricing/)<br>[AWS Secrets Manager – Pricing](https://aws.amazon.com/secrets-manager/pricing/)<br>[Amazon S3 – Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)<br>[Well-Architected – Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) |
| Wed | - Wrote module `s3_cloudfront` (3 buckets, OAC, lifecycle)<br>- Wrote module `sns` and updated `budgets`<br>- `terraform validate` succeeded<br>- Wrote the SecureTable project summary document | 11/04/2026 | 11/04/2026 | Project summary document<br>Modules<br>[Terraform – aws_cloudfront_origin_access_control](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudfront_origin_access_control)<br>[Terraform – aws_budgets_budget](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/budgets_budget)<br>[CloudFront – Restrict access to S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) |
| Thu | - Reviewed the foundation against the graduation rubric (≈ 40–60% covered)<br>- Fixed 6 critical issues: Cognito MFA on, admin-only sign-up, safe nuke…<br>- Wrote `securetable.tftest.hcl` (10 tests), configured `.checkov.yaml`<br>- Read the backend's ingredient-management business spec | 11/05/2026 | 11/05/2026 | Review report<br>[Terraform – Tests](https://developer.hashicorp.com/terraform/language/tests)<br>[Terraform – Mocking](https://developer.hashicorp.com/terraform/language/tests/mocking)<br>[Checkov](https://www.checkov.io)<br>[Cognito – Admin-created user accounts](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-create-user-accounts.html)<br>[Cognito – MFA](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-mfa.html)<br>[DynamoDB – Transactions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html) |
| Fri | - Merged into the team repo, opened PR #1 (original) and PR #2 (26-file patch)<br>- Fixed CI: pinned trivy-action by SHA, gitleaks CLI over full history, CI on every PR<br>- Fixed the S3 test mismatch between OpenTofu and Terraform → 10/10<br>- Incidents: rotated an exposed access key, removed survey files from the repo, deleted a forgotten EC2 | 11/06/2026 | 11/06/2026 | CI workflow<br>Deployment log 11/06<br>[Gitleaks](https://github.com/gitleaks/gitleaks)<br>[Trivy Action](https://github.com/aquasecurity/trivy-action)<br>[GitHub – Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)<br>[GitHub – Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)<br>[AWS – Manage access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) |

### Week 8 Achievements:

- Architecture diagram v1.0.0 (draw.io) and a 4-phase personal plan with a hand-off matrix and per-milestone pass/fail checks.
- `deploy_and_test.ps1` (apply, export `deployed_env.json`, probe `/health`) and `nuke_and_teardown.ps1` (empty S3 versions, destroy, post-teardown audit for zero resources).
- Dropped GuardDuty, Secrets Manager and the KMS CMK; split into 3 S3 buckets (frontend/assets/backup + Glacier after 90 days); added SNS `securetable-alerts` and a 3-threshold Budget.
- Review patch of 11/05 (26 files, +1091/−307): Cognito MFA on, admin-only sign-up, safe nuke, 10 `terraform test` tests, Checkov skip list with written reasons, blocking CI.
- CI 4/4 jobs green: gitleaks CLI over full history, Checkov, Trivy (pinned by SHA), Bandit, Flake8, pytest 22/22, terraform fmt/validate/test 10/10, npm audit.
- Handled 6 incidents on 11/06 (access key exposed in a screenshot, survey file pushed to the repo, red CI, OpenTofu vs Terraform test mismatch, PR not triggering CI, `tfplan` not ignored); deleted a forgotten Windows EC2 in us-east-1.
