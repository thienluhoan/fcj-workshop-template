---
title: "Worklog Week 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 5 Objectives:

- Learn and practice Infrastructure as Code (IaC) provisioning using **AWS CloudFormation**.
- Set up centralized identity management and Single Sign-On (**AWS IAM Identity Center / SSO**) across organizational accounts.
- Enforce security boundaries and restrict maximum IAM permissions using **Permission Boundaries**.
- Restrict and control role assumption workflows based on fine-grained conditions.
- Monitor, assess, and audit security compliance standards using **AWS Security Hub**.

### Tasks Implemented This Week:
| Day | Work Details | Start Date | Completion Date | Reference Link |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Infrastructure as Code with AWS CloudFormation:**<br>&emsp;+ Understand CloudFormation template structure (JSON/YAML).<br>&emsp;+ Write a template to provision VPC, Subnets, and EC2 instances.<br>&emsp;+ Create a Stack via CloudFormation Console.<br>&emsp;+ Monitor resource creation progress and practice updating/deleting Stacks.<br>&emsp;+ Clean up test resources post-lab. | 12/10/2026   | 12/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Set up Single Sign-On (AWS SSO / IAM Identity Center) for Organization:**<br>&emsp;+ Enable AWS IAM Identity Center within AWS Organizations.<br>&emsp;+ Create Users and Groups in Identity Center.<br>&emsp;+ Define Permission Sets (e.g., AdministratorAccess, ReadOnlyAccess).<br>&emsp;+ Assign Permission Sets and targeted AWS accounts to user groups.<br>&emsp;+ Log in via AWS Access Portal to test multi-account access via SSO. | 13/10/2026   | 13/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Restrict user privileges with IAM Permission Boundaries:**<br>&emsp;+ Create an IAM Policy to act as a Permission Boundary (e.g., allowing operations only on S3 and EC2).<br>&emsp;+ Create a new IAM User/Role and attach the Permission Boundary.<br>&emsp;+ Attach `AdministratorAccess` to the IAM User/Role.<br>&emsp;+ Perform verification test: Confirm user cannot perform actions outside boundary scope (e.g., DynamoDB access) despite full Admin policy.<br>&emsp;+ Clean up resources. | 14/10/2026   | 14/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Restrict role assumptions with conditions (Role Assumption Conditions):**<br>&emsp;+ Create an IAM Role for assumption.<br>&emsp;+ Configure Trust Policy with contextual conditions such as `aws:PrincipalTag`, `aws:SourceIp`, or `ExternalId`.<br>&emsp;+ Practice assuming roles via AWS CLI (`aws sts assume-role`).<br>&emsp;+ Verify results: Successful execution when conditions match, and Access Denied when conditions fail.<br>&emsp;+ Clean up resources. | 15/10/2026   | 15/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Audit security standards with AWS Security Hub:**<br>&emsp;+ Enable AWS Security Hub service on Console.<br>&emsp;+ Activate security standards (e.g., AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark).<br>&emsp;+ Run compliance checks and review security Findings.<br>&emsp;+ Analyze flagged misconfigurations and practice remediation steps.<br>&emsp;+ Clean up test resources. | 16/10/2026   | 16/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 5 Key Accomplishments:

- Mastered template authoring and automated end-to-end provisioning, updating, and teardown of cloud infrastructure using **AWS CloudFormation**.
- Successfully configured **AWS IAM Identity Center (SSO)** for centralized identity management and streamlined secure access across multi-account environments.
- Implemented **IAM Permission Boundaries** to establish maximum permission guardrails, effectively preventing privilege escalation risks when delegating permissions.
- Configured fine-grained policy **Conditions** to restrict role-assumption workflows, enhancing security governance over cross-role operations.
- Deployed and navigated **AWS Security Hub** to centralize security alerts and audit infrastructure compliance against industry security standards.
- Cleaned up all lab resources safely to maintain resource hygiene and avoid unexpected cloud costs.
