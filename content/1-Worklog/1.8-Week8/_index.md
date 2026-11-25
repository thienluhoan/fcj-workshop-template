---
title: "Worklog Week 8"
date: 2026-11-02
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 8 Objectives:

- Build an automated Continuous Integration and Continuous Deployment (CI/CD) pipeline using **AWS CodePipeline**.
- Fully automate application packaging and deployment workflows from source repositories to target environments.
- Integrate hybrid cloud storage between on-premises and AWS using **AWS Storage Gateway (File Gateway)**.
- Provision and configure fully managed native Windows file systems using **Amazon FSx for Windows File Server**.
- Learn Data Lake architectural patterns and build a functional **Data Lake** foundation on AWS.

### Tasks Implemented This Week:
| Day | Work Details | Start Date | Completion Date | Reference Link |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Deploy applications with AWS CodePipeline:**<br>&emsp;+ Understand core CI/CD architecture (Source, Build, Deploy stages).<br>&emsp;+ Set up a source repository (Source stage with AWS CodeCommit / GitHub).<br>&emsp;+ Create a basic pipeline in AWS CodePipeline connected to the repository.<br>&emsp;+ Test initial pipeline triggering and clean up test resources. | 02/11/2026   | 02/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Automate application deployment using AWS CodePipeline:**<br>&emsp;+ Integrate **AWS CodeBuild** to compile application code and execute unit tests.<br>&emsp;+ Integrate **AWS CodeDeploy** to automate code deployment onto target EC2/ECS instances.<br>&emsp;+ Perform code commits and monitor automated pipeline execution from Source $\rightarrow$ Build $\rightarrow$ Deploy.<br>&emsp;+ Verify successful application deployment updates and clean up resources post-lab. | 03/11/2026   | 03/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Scalable cloud storage with File Storage Gateway:**<br>&emsp;+ Create an S3 Bucket to serve as backend storage.<br>&emsp;+ Deploy an AWS Storage Gateway Appliance (File Gateway) on an EC2 instance / Virtual Machine.<br>&emsp;+ Configure NFS / SMB File Shares mapped to the S3 Bucket.<br>&emsp;+ Mount file shares on client machines and verify automatic file synchronization to Amazon S3.<br>&emsp;+ Clean up lab resources. | 04/11/2026   | 04/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Deploy multi-purpose enterprise storage for Windows using FSx:**<br>&emsp;+ Provision an Amazon FSx for Windows File Server instance supporting SMB protocol.<br>&emsp;+ Connect FSx with AWS Directory Service / Active Directory for centralized access management.<br>&emsp;+ Mount the FSx shared drive onto a Windows EC2 instance via SMB.<br>&emsp;+ Test read/write performance, Shadow Copies features, and clean up resources post-lab. | 05/11/2026   | 05/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Build a Data Lake on AWS:**<br>&emsp;+ Learn standard Data Lake architecture patterns on AWS (Storage, Ingestion, Processing, Analytics).<br>&emsp;+ Create S3 Buckets representing data lake layers (Raw Data, Processed Data).<br>&emsp;+ Configure **AWS Glue Crawlers** to automatically infer data schemas into the **AWS Glue Data Catalog**.<br>&emsp;+ Perform serverless SQL data querying directly using **Amazon Athena**.<br>&emsp;+ Clean up test data lake resources. | 06/11/2026   | 06/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 8 Key Accomplishments:

- Built an end-to-end automated **CI/CD pipeline** with **AWS CodePipeline**, orchestrating **CodeCommit, CodeBuild, and CodeDeploy** for continuous software delivery.
- Mastered hybrid cloud storage architecture using **AWS Storage Gateway (File Gateway)**, seamlessly extending local file storage capacity into **Amazon S3**.
- Configured and managed native Windows file shares with **Amazon FSx for Windows File Server**, integrating with Active Directory authentication.
- Successfully built a foundational **Data Lake** architecture on AWS, managing data layers on S3, discovering schemas with **AWS Glue**, and executing serverless analytics queries via **Amazon Athena**.
- Ensured thorough resource teardown post-lab to maintain clean AWS account hygiene and avoid unwanted charges.
