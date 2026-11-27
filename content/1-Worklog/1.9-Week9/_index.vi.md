---
title: "Worklog Tuần 9"
date: 2026-11-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

- Hiểu sâu GitHub Actions và giải thích được từng dòng của pipeline DevSecOps nhóm đang dùng.
- Biết mỗi công cụ quét (gitleaks, Checkov, Trivy, Bandit) bắt được gì và bỏ sót gì.
- Chuẩn bị cho CI tự deploy lên AWS bằng OIDC, không cần access key.
- Nâng chất lượng test IaC và kiểm tra ngay trên máy trước khi commit.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đọc GitHub Actions Documentation<br>- Làm khóa GitHub Skills (Hello GitHub Actions, Test with Actions)<br>- Đọc lại `devsecops.yml` từng dòng | 09/11/2026 | 09/11/2026 | [GitHub Actions Documentation](https://docs.github.com/en/actions)<br>[GitHub Skills](https://skills.github.com)<br>Workflow của nhóm<br>[Martin Fowler – Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html) |
| 3 | - Đọc Security hardening for GitHub Actions<br>- Đọc OWASP Top 10 CI/CD Security Risks<br>- Lập checklist tự review workflow của nhóm | 10/11/2026 | 10/11/2026 | [GitHub – Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)<br>[OWASP Top 10 CI/CD Security Risks](https://owasp.org/www-project-top-10-ci-cd-security-risks/)<br>[SLSA – Supply-chain Levels for Software Artifacts](https://slsa.dev) |
| 4 | - Đọc tài liệu gitleaks, Checkov, Trivy, Bandit<br>- Đối chiếu pipeline với OWASP DevSecOps Guideline<br>- Ghi bảng: mỗi công cụ bắt được gì / bỏ sót gì | 11/11/2026 | 11/11/2026 | [Gitleaks](https://github.com/gitleaks/gitleaks)<br>[Checkov](https://www.checkov.io)<br>[Trivy](https://trivy.dev)<br>[Bandit](https://bandit.readthedocs.io)<br>[OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/) |
| 5 | - Đọc configure-aws-credentials và tài liệu OIDC của GitHub<br>- Thiết kế IAM OIDC provider + role cho CI<br>- Viết trust policy khóa `aud` và `sub` | 12/11/2026 | 12/11/2026 | [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)<br>[GitHub – Configuring OIDC in AWS](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)<br>[IAM Security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 6 | - Đọc Terraform Tests và Mocking<br>- Tìm hiểu TFLint<br>- Tìm hiểu pre-commit (fmt, gitleaks chạy trước commit) | 13/11/2026 | 13/11/2026 | [Terraform – Tests](https://developer.hashicorp.com/terraform/language/tests)<br>[Terraform – Mocking](https://developer.hashicorp.com/terraform/language/tests/mocking)<br>[TFLint](https://github.com/terraform-linters/tflint)<br>[pre-commit](https://pre-commit.com)<br>Test của dự án |

### Kết quả đạt được tuần 9:

- Giải thích được 4 job của `devsecops.yml`: Secrets & IaC Security, Backend Python, Terraform, Frontend Build & Audit.
- Lập checklist tự review pipeline theo OWASP Top 10 CI/CD Security Risks và hướng dẫn security hardening của GitHub.
- Nắm cấu hình và giới hạn của gitleaks, Checkov, Trivy, Bandit; đối chiếu pipeline với OWASP DevSecOps Guideline.
- Hiểu luồng OIDC GitHub → AWS STS và cách viết trust policy khóa cả `aud` lẫn `sub`.
- Nắm `terraform test` (run block, mock provider, `expect_failures`), TFLint và pre-commit hook.
