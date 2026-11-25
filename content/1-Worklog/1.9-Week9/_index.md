---
title: "Week 9 Worklog"
date: 2026-11-09
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 9 Objectives:

- Design and implement advanced NoSQL database architectures using **Amazon DynamoDB**, optimizing scalability and query performance.
- Master resource commitment models to optimize operational costs on AWS through **Savings Plans**, **Reserved Instances (RI)**, and **Reserved DB Instances**.
- Practice evaluating and selecting appropriate server instance sizes (**Right-sizing**) to eliminate resource waste for **Amazon EC2**.
- Build monitoring systems and visualize cloud resource usage and cost data on the AWS platform.
- Perform in-depth analysis of Cost & Usage data by combining **AWS Glue** and **Amazon Athena**.

### Tasks to Implement This Week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Advanced Architecture with Amazon DynamoDB:**<br>&emsp;+ Understand partitioning architecture, primary key design (Partition Key, Sort Key), and secondary indexes (GSI, LSI).<br>&emsp;+ Configure read/write capacity modes (Provisioned vs. On-Demand) and Auto Scaling for DynamoDB tables.<br>&emsp;+ Practice implementing DynamoDB Streams integrated with AWS Lambda for event-driven data processing.<br>&emsp;+ Test query performance, enable DynamoDB Accelerator (DAX) / Point-in-Time Recovery, and clean up resources. | 09/11/2026   | 09/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Optimize cost with Savings Plans, Reserved Instance, Reserved DB Instance:**<br>&emsp;+ Analyze the differences and scope between Compute Savings Plans, EC2 Instance Savings Plans, and Reserved Instances (RI).<br>&emsp;+ Explore database cost optimization mechanisms using Amazon RDS Reserved DB Instances.<br>&emsp;+ Use **AWS Cost Explorer** to review savings purchase recommendations based on historical usage.<br>&emsp;+ Practice calculating long-term cost optimization scenarios and planning appropriate resource commitments. | 10/11/2026   | 10/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Choose the right serving size for Amazon EC2 Resource Optimization:**<br>&emsp;+ Collect system performance metrics (CPU, RAM, Network, Disk I/O) via **Amazon CloudWatch** and CloudWatch Agent.<br>&emsp;+ Use **AWS Compute Optimizer** to identify over-provisioned or under-provisioned EC2 instances.<br>&emsp;+ Practice modifying instance types (Right-sizing) to match actual workload demands.<br>&emsp;+ Verify application stability after configuration changes and clean up lab resources. | 11/11/2026   | 11/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Visualize Cost of Usage on AWS:**<br>&emsp;+ Enable and configure **AWS Cost and Usage Reports (CUR)** for periodic delivery to Amazon S3.<br>&emsp;+ Explore cost visualization tools including **AWS Cost Explorer**, **AWS Budgets**, and **Amazon QuickSight**.<br>&emsp;+ Set up Budget Alerts when actual or forecasted costs exceed defined thresholds.<br>&emsp;+ Build visual dashboards to track spending by service, Region, and Cost Allocation Tags. | 12/11/2026   | 12/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Advanced usage cost analysis with AWS Glue and Amazon Athena:**<br>&emsp;+ Configure **AWS Glue Crawler** to automatically extract metadata from Cost and Usage Reports (CUR) stored in Amazon S3.<br>&emsp;+ Use **Amazon Athena** to write complex SQL queries analyzing granular costs by resource, usage hour, and transaction type.<br>&emsp;+ Optimize query performance and cost by converting data formats to Apache Parquet and implementing data partitioning.<br>&emsp;+ Clean up all data tables, Crawlers, and test resources after completing the lab. | 13/11/2026   | 13/11/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 9 Achievements:

- Proficiently designed advanced data models and scalable architectures on **Amazon DynamoDB**, optimizing query speed and automated event-driven processing.
- Understood and effectively applied long-term discount policies such as **Savings Plans**, **Reserved Instances**, and **Reserved DB Instances** to optimize infrastructure budgets.
- Mastered the performance monitoring workflow and executed **Right-sizing** for **Amazon EC2** using **AWS Compute Optimizer** and **CloudWatch**, balancing performance and cost.
- Completely set up a budget tracking, alerting, and AWS cost visualization system transparently.
- Successfully utilized **AWS Glue** and **Amazon Athena** together to query and analyze large-scale cost data (CUR) for Cloud Financial Management (FinOps).
- Ensured all test resources were thoroughly cleaned up after each lab to prevent unintended AWS charges.
