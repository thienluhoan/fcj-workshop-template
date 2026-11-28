---
title: "Workshop"
date: 2026-11-27
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying SEAL, a hackathon judging platform, on AWS

#### Overview

**SEAL** is a web platform for running a student hackathon end to end: team registration, rounds and timers, submissions, judging with rubrics, results and awards. Judges can also ask an **AI assistant** to read a team's GitHub repository and propose a first evaluation, which the judge then reviews.

This workshop rebuilds the production deployment of SEAL on AWS, step by step, using the **AWS CloudFormation** templates the team wrote. At the end you will have:

- a React frontend on **Amazon S3** served through **Amazon CloudFront**, protected by **AWS WAF** and a custom domain in **Amazon Route 53** with **AWS Certificate Manager** certificates;
- a Spring Boot API on **Amazon EC2** in an **Auto Scaling group** behind an **Application Load Balancer**, in private subnets of a dedicated **Amazon VPC**;
- an **Amazon RDS for MySQL** database in isolated subnets;
- an asynchronous AI pipeline: **Amazon SQS** queues with dead-letter queues and an **AWS Lambda** worker that calls the DeepSeek API;
- transactional email through **Amazon SES**;
- monitoring with **Amazon CloudWatch** alarms, **Amazon SNS** and **AWS Budgets**, auditing with **AWS CloudTrail**;
- a CI/CD pipeline in **GitHub Actions** that pushes images to **Amazon ECR** and rolls them out with **AWS Systems Manager**, signed in through OIDC without long-lived keys.

#### Content

1. [Overview](5.1-overview/)
2. [Prerequisites](5.2-prerequisites/)
3. [Architecture and data flow](5.3-architecture/)
4. [Network and data layer](5.4-network-data/)
5. [Compute and origin](5.5-compute-origin/)
6. [Edge and security](5.6-edge-security/)
7. [AI worker](5.7-ai-worker/)
8. [Email with SES](5.8-email-ses/)
9. [CI/CD with GitHub Actions](5.9-cicd/)
10. [Testing and validation](5.10-testing-validation/)
11. [Cost and optimization](5.11-cost/)
12. [Troubleshooting and lessons learned](5.12-troubleshooting/)
13. [Clean up](5.13-cleanup/)
14. [Demo](5.14-demo/)
