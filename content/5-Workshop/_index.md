---
title: "Workshop"
date: 2026-11-23
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying SecureTable – A Secure Serverless Restaurant Management System on AWS

### Overview

This workshop shows how to redeploy the whole **SecureTable** system with Terraform and automation scripts, from infrastructure to security testing and cleanup. By following it end to end you will have:

- A web app served through **Amazon CloudFront** (HTTPS), with data stored in **Amazon DynamoDB** and **Amazon S3**.
- Sign-in with **Amazon Cognito** (mandatory MFA, admin-only account creation), an API on **Amazon API Gateway** with a Cognito authorizer, and business logic in **AWS Lambda**.
- An **AWS WAF** layer toggled by a single flag, monitoring with **Amazon CloudWatch**, alerts through **Amazon SNS** and cost control with **AWS Budgets**.
- A **DevSecOps** pipeline on GitHub Actions and a single command that tears everything down to 0 USD.

### Contents

1. [Introduction](5.1-workshop-overview/)
2. [Prerequisites](5.2-prerequiste/)
3. [Deploying the infrastructure with Terraform](5.3-deploy-infrastructure/)
4. [Cognito accounts and API testing](5.4-cognito-api/)
5. [Frontend on S3 + CloudFront](5.5-frontend/)
6. [Security: WAF and the DevSecOps pipeline](5.6-security/)
7. [Monitoring, testing and cost](5.7-monitoring/)
8. [Cleaning up resources](5.8-cleanup/)

### Proof of a working product

- **Demo video:** *(team to add link)*
- **Product link (CloudFront):** *(team to add during the demo)*
