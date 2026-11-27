---
title: "Week 10 Worklog"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

- Get solid with team Git (branching, rebase, stacked PRs, history rewriting) after the week 8 incidents.
- Write 3 technical blog posts sharing AWS knowledge with the community.
- Threat-model the sign-in → API call → data storage flow of SecureTable.
- Build a structured personal learning roadmap for Cloud & DevSecOps.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Read Pro Git chapters 1–3<br>- Practised on Learn Git Branching (rebase, cherry-pick)<br>- Reviewed stacked PRs and safe history rewriting | 11/16/2026 | 11/16/2026 | [Pro Git (book)](https://git-scm.com/book/en/v2)<br>[Learn Git Branching](https://learngitbranching.js.org)<br>[git push (--force-with-lease)](https://git-scm.com/docs/git-push) |
| Tue | - Analysed 3 reference blog posts (Terraform, WAF + API Gateway, SQS)<br>- Built a common post structure and chose 3 topics<br>- Wrote post 1: Uploading files directly to S3 with presigned URLs<br>- Generated diagrams for the 3 posts with a script | 11/17/2026 | 11/17/2026 | [S3 – Presigned URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)<br>[API Gateway – Quotas](https://docs.aws.amazon.com/apigateway/latest/developerguide/limits.html)<br>[Lambda – Quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)<br>Blog drafts |
| Wed | - Wrote post 2: NAT Gateway and VPC Endpoint<br>- Wrote post 3: GitHub Actions deploying to AWS without access keys<br>- Wrote a reusable prompt for drafting blog posts with another AI tool | 11/18/2026 | 11/18/2026 | [VPC – Gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)<br>[Amazon VPC – Pricing](https://aws.amazon.com/vpc/pricing/)<br>[GitHub – Configuring OIDC in AWS](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)<br>Blog drafts<br>[AWS Study Group – Facebook group](https://www.facebook.com/groups/awsstudygroupfcj) |
| Thu | - Read OWASP Top 10:2025 and the Threat Modeling Manifesto<br>- Tried Threat Composer (AWS Labs)<br>- Drafted a threat model for the Cognito → API → DynamoDB flow | 11/19/2026 | 11/19/2026 | [OWASP Top 10:2025](https://owasp.org/Top10/2025/)<br>[Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org)<br>[Threat Composer (AWS Labs)](https://github.com/awslabs/threat-composer)<br>[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org) |
| Fri | - Curated 100 learning sources in 8 stages A–H<br>- Linked each source to a SecureTable task<br>- Defined a suggested certification order | 11/20/2026 | 11/20/2026 | [Cloud & DevSecOps learning roadmap (personal)](https://claude.ai/artifact/CJmXgmuQfzQasmVM5xU4wr)<br>[roadmap.sh – DevOps](https://roadmap.sh/devops)<br>[roadmap.sh – DevSecOps](https://roadmap.sh/devsecops)<br>[AWS Skill Builder](https://skillbuilder.aws) |

### Week 10 Achievements:

- Comfortable with the Git operations used in the project: rebase, retargeting stacked PRs, `push --force-with-lease`.
- Finished 3 blog posts with diagrams: (1) Uploading files directly to S3 with presigned URLs, (2) NAT Gateway vs VPC Endpoint, (3) GitHub Actions deploying to AWS without access keys (OIDC).
- A blog writing template derived from 3 reference posts and a reusable prompt.
- Draft threat model for the Cognito → API Gateway → Lambda → DynamoDB flow, mapped to OWASP Top 10:2025.
- A *Cloud & DevSecOps* roadmap of 100 curated sources in 8 stages (A–H) with progress tracking and a suggested certification order.
