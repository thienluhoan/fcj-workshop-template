---
title: "Week 9 Worklog"
date: 2026-11-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

- Understand GitHub Actions deeply and explain every line of the team's DevSecOps pipeline.
- Know what each scanner (gitleaks, Checkov, Trivy, Bandit) catches and misses.
- Prepare CI to deploy to AWS with OIDC, without access keys.
- Improve IaC tests and run checks locally before committing.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Read the GitHub Actions Documentation<br>- Took GitHub Skills courses (Hello GitHub Actions, Test with Actions)<br>- Re-read `devsecops.yml` line by line | 11/09/2026 | 11/09/2026 | [GitHub Actions Documentation](https://docs.github.com/en/actions)<br>[GitHub Skills](https://skills.github.com)<br>Team CI workflow<br>[Martin Fowler – Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html) |
| Tue | - Read Security hardening for GitHub Actions<br>- Read OWASP Top 10 CI/CD Security Risks<br>- Built a self-review checklist for the team workflow | 11/10/2026 | 11/10/2026 | [GitHub – Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)<br>[OWASP Top 10 CI/CD Security Risks](https://owasp.org/www-project-top-10-ci-cd-security-risks/)<br>[SLSA – Supply-chain Levels for Software Artifacts](https://slsa.dev) |
| Wed | - Read the gitleaks, Checkov, Trivy, Bandit documentation<br>- Mapped the pipeline to the OWASP DevSecOps Guideline<br>- Table: what each tool catches / misses | 11/11/2026 | 11/11/2026 | [Gitleaks](https://github.com/gitleaks/gitleaks)<br>[Checkov](https://www.checkov.io)<br>[Trivy](https://trivy.dev)<br>[Bandit](https://bandit.readthedocs.io)<br>[OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/) |
| Thu | - Read configure-aws-credentials and GitHub's OIDC docs<br>- Designed an IAM OIDC provider + role for CI<br>- Wrote a trust policy locking `aud` and `sub` | 11/12/2026 | 11/12/2026 | [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)<br>[GitHub – Configuring OIDC in AWS](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)<br>[IAM Security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| Fri | - Read Terraform Tests and Mocking<br>- Explored TFLint<br>- Explored pre-commit (fmt, gitleaks before each commit) | 11/13/2026 | 11/13/2026 | [Terraform – Tests](https://developer.hashicorp.com/terraform/language/tests)<br>[Terraform – Mocking](https://developer.hashicorp.com/terraform/language/tests/mocking)<br>[TFLint](https://github.com/terraform-linters/tflint)<br>[pre-commit](https://pre-commit.com)<br>Project tests |

### Week 9 Achievements:

- Can explain the 4 jobs of `devsecops.yml`: Secrets & IaC Security, Backend Python, Terraform, Frontend Build & Audit.
- Built a pipeline self-review checklist from the OWASP Top 10 CI/CD Security Risks and GitHub's security hardening guide.
- Understand the configuration and limits of gitleaks, Checkov, Trivy and Bandit; mapped the pipeline to the OWASP DevSecOps Guideline.
- Understand the GitHub → AWS STS OIDC flow and how to write a trust policy that locks both `aud` and `sub`.
- Learned `terraform test` (run blocks, mock providers, `expect_failures`), TFLint and pre-commit hooks.
