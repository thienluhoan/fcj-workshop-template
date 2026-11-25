---
title: "Week 10 Worklog"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 10 Objectives:

- Practice migrating a **Monolithic** application to the AWS cloud as a foundation for application modernization.
- Establish an automated application release pipeline (**Auto-release**) to accelerate deployment cycles and minimize manual errors.
- Decompose and build independent **Microservices** from the existing monolithic system.
- Restructure data storage and business workflows (**Data & Workflow Restructuring**) to ensure service autonomy.
- Implement asynchronous messaging and event-driven communication (**Messaging & Eventing**) for Microservices architectures on AWS.

### Tasks to Implement This Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Application Migrate Monolith:**<br>&emsp;+ Assess the existing monolithic application architecture and identify dependencies (Database, Storage, Network).<br>&emsp;+ Containerize the application using Docker and push the image to **Amazon ECR**.<br>&emsp;+ Deploy the monolithic application onto **Amazon ECS** or EC2 behind an **Application Load Balancer (ALB)**.<br>&emsp;+ Verify system stability and baseline performance after migrating to AWS. | 16/11/2026   | 16/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Auto-release apps:**<br>&emsp;+ Configure an automated CI/CD pipeline integrating **AWS CodePipeline**, **CodeBuild**, and **CodeDeploy** / ECS.<br>&emsp;+ Apply zero-downtime deployment strategies (Blue/Green or Rolling Deployment) to minimize service interruption.<br>&emsp;+ Set up automated rollback mechanisms upon detecting errors during a new release.<br>&emsp;+ Practice pushing code updates and validating the end-to-end automated release workflow. | 17/11/2026   | 17/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Create a Microservice:**<br>&emsp;+ Analyze bounded contexts to decouple an independent business capability from the monolith.<br>&emsp;+ Build, package, and deploy the standalone microservice on **Amazon ECS** or **AWS Lambda**.<br>&emsp;+ Configure **Amazon API Gateway** or path-based routing on ALB to route traffic appropriately.<br>&emsp;+ Test service communication and ensure the microservice operates independently without disrupting the core system. | 18/11/2026   | 18/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Data and workflow restructuring:**<br>&emsp;+ Decouple a shared database into dedicated data stores (Database-per-Service) using **Amazon RDS** and **Amazon DynamoDB**.<br>&emsp;+ Synchronize and migrate data across services while preserving data integrity.<br>&emsp;+ Use **AWS Step Functions** to orchestrate complex multi-service business workflows.<br>&emsp;+ Validate data consistency and exception handling within the restructured workflow. | 19/11/2026   | 19/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Microservices Messaging and Eventing:**<br>&emsp;+ Decouple microservices using asynchronous communication patterns.<br>&emsp;+ Utilize **Amazon SQS** for message queuing and **Amazon SNS** for Pub/Sub notification distribution.<br>&emsp;+ Integrate **Amazon EventBridge** to route business events across AWS services and application components.<br>&emsp;+ Test event flows, configure Dead-Letter Queues (DLQ) for failed messages, and clean up lab resources. | 20/11/2026   | 20/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 10 Achievements:

- Successfully migrated a **Monolithic** application onto AWS cloud infrastructure, laying the groundwork for architectural modernization.
- Built a reliable **Auto-release** pipeline automating testing, packaging, and deployment workflows.
- Mastered functional decomposition techniques (Strangler Fig Pattern) to incrementally transition monolith components into standalone **Microservices**.
- Gained hands-on expertise in restructuring data per service and orchestrating distributed workflows using **AWS Step Functions**.
- Implemented an Event-Driven Architecture using **Amazon SQS, SNS, and EventBridge**, enhancing system scalability and fault tolerance.
- Ensured all test resources were thoroughly cleaned up after each lab to optimize AWS costs.
