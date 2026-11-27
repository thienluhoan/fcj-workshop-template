---
title: "Week 3 Worklog"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

- Learn relational (Amazon RDS) and NoSQL (Amazon DynamoDB) databases on AWS.
- Understand event-driven architecture with Amazon SNS/SQS and content delivery with CloudFront.
- List Cloud-Native project ideas and survey a real company to pick a topic with real value.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Get Started with Amazon RDS workshop:<br>&emsp;+ VPC, Security Group, DB subnet group<br>&emsp;+ Created RDS, launched EC2, deployed the app<br>&emsp;+ Backup & restore, cleanup | 09/28/2026 | 09/28/2026 | [000005 – Amazon RDS](https://000005.awsfcaj.com/)<br>[000005 – 6. Backup and restore](https://000005.awsfcaj.com/6-backup/) |
| Tue | - Listed 11 AWS Cloud-Native project ideas<br>- Designed a construction-company survey (17 questions, sections A–D)<br>- Wrote a script generating the HTML form and an automated check script<br>- Checked desktop / mobile / print layouts | 09/29/2026 | 09/29/2026 | [FCJ – 8. FCJ Workforce Program](https://cloudjourney.awsfcaj.com/8-fcjworkforce/)<br>Project idea list<br>Construction company survey<br>Form build & check scripts<br>QA screenshots desktop/mobile/print |
| Wed | - Amazon DynamoDB workshop:<br>&emsp;+ Core components, primary key, secondary index, capacity mode<br>&emsp;+ CRUD, Query, GSI on console and CloudShell<br>&emsp;+ Python boto3: create table, write/read, Query vs Scan | 09/30/2026 | 09/30/2026 | [000060 – 1. Introduction](https://000060.awsfcaj.com/1-introduce/)<br>[000060 – 2. Console & CloudShell](https://000060.awsfcaj.com/2-prerequiste/)<br>[000060 – 3. AWS SDK (Python)](https://000060.awsfcaj.com/3-gettingstartedwithawssdk/) |
| Thu | - Event-driven Architecture with SNS and SQS workshop:<br>&emsp;+ Architecture, choosing the SQS queue type<br>&emsp;+ Pub/Sub and fan-out<br>&emsp;+ Basic and advanced message filtering | 10/01/2026 | 10/01/2026 | [000077 – 1.1 Architecture & Event Flow](https://000077.awsfcaj.com/1-introduce/1.1-architecture/)<br>[000077 – 1.3 SQS Queue Selection & Deep Dive](https://000077.awsfcaj.com/1-introduce/1.3-sqs/)<br>[000077 – 3. Simple Pub/Sub & Fan-Out](https://000077.awsfcaj.com/3-simplesubpub/)<br>[000077 – 4. Message Filtering](https://000077.awsfcaj.com/4-messagefiltering/)<br>[000077 – 5. Advanced Message Filtering](https://000077.awsfcaj.com/5-advancemessage/) |
| Fri | - CloudFront with S3 bucket origin workshop<br>- Read the Origin Access Control (OAC) documentation | 10/02/2026 | 10/02/2026 | [000094 – 1. Create S3 bucket](https://000094.awsfcaj.com/1.1-t%E1%BA%A1o-nh%C3%B3m-s3/)<br>[000094 – 3. Configure Amazon CloudFront](https://000094.awsfcaj.com/1.3-c%E1%BA%A5u-h%C3%ACnh-amazon-cloudfront/)<br>[AWS Docs – Restrict access to an S3 origin (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)<br>[AWS Docs – Invalidating files](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html) |
| Sat | - Consolidated survey answers from the director of the EVG construction company<br>- Built a multi-sector survey (HTML form) for further surveys<br>- Wrote the analysis and AWS project proposal for EVG | 10/03/2026 | 10/03/2026 | Survey answers<br>Multi-sector survey form<br>EVG project proposal<br>[AWS Pricing Calculator](https://calculator.aws/) |

### Week 3 Achievements:

- Deployed RDS in private subnets, connected it from an app on EC2, tried backup/restore.
- Worked with DynamoDB via console, CloudShell and Python (boto3): CRUD, Query, Scan, GSI.
- Built an SNS → SQS fan-out with message filtering via filter policies.
- Served an S3 website through CloudFront; understand caching and invalidation.
- Listed 11 Cloud-Native project ideas; designed a 17-question survey (sections A–D) for a construction company, generated the HTML automatically and checked desktop/mobile/print layouts.
- Collected survey answers from the director of the EVG construction company and wrote an analysis and project proposal (MVP, minimal AWS architecture, 9–10 week plan, acceptance criteria); built an extra multi-sector survey.
