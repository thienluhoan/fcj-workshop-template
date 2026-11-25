---
title: "Week 11 Worklog"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 11 Objectives:

- Build, host, and integrate user authentication for a **Single Page Application (SPA)** on AWS.
- Explore and integrate pre-trained, managed **AWS AI Services** into practical application workflows.
- Master orchestrating distributed business workflows using **AWS Step Functions**.
- Develop event-driven **Serverless** functions with **AWS Lambda** interacting directly with **Amazon S3** and **Amazon DynamoDB**.
- Complete an end-to-end Serverless application by connecting a Frontend interface to **Amazon API Gateway**.

### Tasks to Implement This Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Create and authenticate Single Page Application:**<br>&emsp;+ Deploy a Single Page Application (SPA) on **Amazon S3** (Static Website Hosting) accelerated by **Amazon CloudFront**.<br>&emsp;+ Set up an **Amazon Cognito User Pool** to manage user sign-up, sign-in, and authentication.<br>&emsp;+ Integrate AWS Amplify / Cognito SDK into the Frontend to handle JWT token issuance and validation.<br>&emsp;+ Test user registration and authentication flows, then clean up lab resources. | 23/11/2026   | 23/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Experience AI services on AWS:**<br>&emsp;+ Explore managed AWS AI services including **Amazon Rekognition** (image analysis), **Amazon Comprehend** (NLP), **Amazon Polly**, and **Amazon Transcribe**.<br>&emsp;+ Practice invoking AI service APIs via AWS SDK / AWS Console to analyze text sentiment, extract image labels, and convert speech.<br>&emsp;+ Evaluate accuracy, pricing, and rapid integration capabilities without building custom ML models from scratch.<br>&emsp;+ Clean up test resources upon completion. | 24/11/2026   | 24/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Getting Started with AWS Step Functions:**<br>&emsp;+ Understand **State Machine** architecture and **Amazon States Language (ASL)** definitions.<br>&emsp;+ Design multi-step workflows combining various states (Task, Choice, Parallel, Wait) using Workflow Studio.<br>&emsp;+ Configure Step Functions to orchestrate multiple AWS Lambda functions with built-in error handling (Retry, Catch).<br>&emsp;+ Visually monitor step-by-step execution logs in the State Machine console and clean up resources. | 25/11/2026   | 25/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Serverless - Lambda interacting with S3 and DynamoDB:**<br>&emsp;+ Configure least-privilege **IAM Execution Roles** for **AWS Lambda** functions.<br>&emsp;+ Set up **S3 Event Notifications** to automatically trigger Lambda functions upon object uploads.<br>&emsp;+ Write Lambda logic to process S3 files and perform CRUD operations (Put, Get, Update, Query) on **Amazon DynamoDB** tables.<br>&emsp;+ Inspect execution logs via **Amazon CloudWatch Logs** and clean up resources. | 26/11/2026   | 26/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Serverless - Build Frontend to call API Gateway:**<br>&emsp;+ Create and configure **Amazon API Gateway** (REST API / HTTP API) as the backend entry point.<br>&emsp;+ Integrate API Gateway endpoints with **AWS Lambda** functions and enable **CORS** (Cross-Origin Resource Sharing) for browser access.<br>&emsp;+ Connect the Frontend SPA to invoke API endpoints and render real-time data from DynamoDB.<br>&emsp;+ Validate the complete end-to-end data flow (Frontend $\rightarrow$ API Gateway $\rightarrow$ Lambda $\rightarrow$ DynamoDB) and clean up resources. | 27/11/2026   | 27/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 11 Achievements:

- Successfully deployed a fast, secure **Single Page Application (SPA)** using **S3, CloudFront**, and **Amazon Cognito** authentication.
- Gained practical experience leveraging **AWS AI Services** (Rekognition, Comprehend, Polly, Transcribe) to rapidly add intelligent capabilities to applications.
- Mastered **AWS Step Functions** to design, orchestrate, and monitor complex Serverless workflows.
- Built event-driven **Serverless** pipelines integrating **AWS Lambda** seamlessly with **Amazon S3** and **Amazon DynamoDB**.
- Delivered a full-stack **Serverless** web application connecting Frontend to Backend via **Amazon API Gateway**, ensuring complete resource cleanup after every lab.
