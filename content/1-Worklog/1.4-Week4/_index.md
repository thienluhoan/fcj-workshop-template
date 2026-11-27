---
title: "Week 4 Worklog"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

- Learn the main serverless building blocks: AWS Lambda, Amazon API Gateway, AWS SAM, Amazon Cognito.
- Analyse more surveys from other user groups and map them against the program's graduation requirements to settle the topic.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Serverless workshop – Lambda with S3 and DynamoDB:<br>&emsp;+ Lambda resizing images when new objects land in S3<br>&emsp;+ IAM policy for Lambda to access S3<br>&emsp;+ Lambda writing data to DynamoDB | 10/05/2026 | 10/05/2026 | [000078 – Lambda + S3](https://000078.awsfcaj.com/2-resize-image-function/)<br>[000078 – Lambda + DynamoDB](https://000078.awsfcaj.com/3-write-data-to-dynaomodb/) |
| Tue | - Wrote a Node.js script reading the Excel survey files → JSON<br>- Analysed each user group: eatery cook, homemaker, parent in education, farming<br>- Proposed lightweight fixes first, an AWS project second, and how to measure impact | 10/06/2026 | 10/06/2026 | Survey reader script<br>Analysis document |
| Wed | - Read the graduation requirements (Junior/Middle level)<br>- Built a requirement → implementation → evidence matrix<br>- Compared option A (ingredient management) and option B (EVG)<br>- 9–10 week plan and a recommendation | 10/07/2026 | 10/07/2026 | Workshop requirements<br>Requirement mapping document<br>[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |
| Thu | - Serverless with Lambda, API Gateway and SAM workshop:<br>&emsp;+ Deployed frontend and backend with SAM<br>&emsp;+ Loaded DynamoDB data, tested the configuration<br>&emsp;+ Realtime feature and image processing with Lambda | 10/08/2026 | 10/08/2026 | [000066 – Lambda, API Gateway, SAM](https://000066.awsfcaj.com/) |
| Fri | - Serverless – Authentication with Amazon Cognito workshop:<br>&emsp;+ Created a User Pool<br>&emsp;+ API + Lambda protected by a Cognito authorizer<br>&emsp;+ Tested sign-in from the frontend | 10/09/2026 | 10/09/2026 | [000081 – 2. Create User Pool](https://000081.awsfcaj.com/2-create-user-pool/)<br>[000081 – 3. Create API and Lambda function](https://000081.awsfcaj.com/3-create-api-and-lambda-function/)<br>[000081 – 4. Test with Front-end](https://000081.awsfcaj.com/4-test-front-end/)<br>[AWS Docs – Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)<br>[AWS Docs – Cognito user pool authorizer for REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html) |

### Week 4 Achievements:

- Wrote a Lambda that processes images when new files land in S3 and a Lambda that writes to DynamoDB, each with its own IAM policy.
- Deployed a sample serverless app with AWS SAM (API Gateway + Lambda + DynamoDB + frontend).
- Created a Cognito User Pool and protected an API with a Cognito authorizer, tested with a frontend.
- Analysed 3 new surveys (eatery cook, homemaker, parent working in education, …) with a script reading the Excel files.
- Built a *requirement → implementation → evidence* matrix; chose **ingredient/restaurant management** (option A) as the preferred topic.
