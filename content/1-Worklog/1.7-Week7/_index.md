---
title: "Worklog Week 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}


### Week 7 Objectives:

- Automate centralized backup and data recovery workflows using **AWS Backup**.
- Establish private network connectivity between VPCs using **VPC Peering**.
- Manage and centralize scalable inter-VPC routing using **AWS Transit Gateway**.
- Containerize applications with **Docker** and run them on Linux server environments.
- Deploy and manage containerized applications using **Amazon Elastic Container Service (AWS ECS)**.

### Tasks Implemented This Week:
| Day | Work Details | Start Date | Completion Date | Reference Link |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| Mon | - **Implement system backup plans with AWS Backup:**<br>&emsp;+ Create a Backup Vault and configure AWS KMS encryption key.<br>&emsp;+ Create a Backup Plan: Configure backup frequency, retention period, and lifecycle settings.<br>&emsp;+ Set up Resource Assignments for automated resource backup (EBS, RDS, EC2).<br>&emsp;+ Execute an On-demand Backup Job and perform a test Restore Job to verify data integrity.<br>&emsp;+ Clean up test backups. | 26/10/2026   | 26/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Tue | - **Connect Virtual Private Clouds using VPC Peering:**<br>&emsp;+ Provision 2 separate VPCs (VPC A and VPC B) with non-overlapping CIDR blocks.<br>&emsp;+ Create a VPC Peering Connection and accept the peering request.<br>&emsp;+ Update Route Tables in both VPCs to route cross-VPC traffic via the peering connection.<br>&emsp;+ Update Security Groups to allow inter-VPC communication.<br>&emsp;+ Test ICMP/SSH connectivity between EC2 instances in both VPCs & clean up resources. | 27/10/2026   | 27/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Wed | - **Centralized connection management with AWS Transit Gateway:**<br>&emsp;+ Launch an AWS Transit Gateway (TGW).<br>&emsp;+ Create VPC Attachments to connect multiple VPCs to the Transit Gateway.<br>&emsp;+ Configure Transit Gateway Route Tables to control inter-VPC traffic.<br>&emsp;+ Update Subnet Route Tables in connected VPCs pointing to the Transit Gateway.<br>&emsp;+ Verify multi-VPC connectivity and clean up resources post-lab. | 28/10/2026   | 28/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Thu | - **Application Deployment using Docker:**<br>&emsp;+ Install Docker Engine and Docker Compose on an EC2 Linux instance.<br>&emsp;+ Write a `Dockerfile` to containerize a sample web application.<br>&emsp;+ Practice basic Docker commands: `docker build`, `docker run`, `docker ps`, `docker logs`.<br>&emsp;+ Push Docker Image to a registry and verify application availability on the container port.<br>&emsp;+ Clean up test containers and images. | 29/10/2026   | 29/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| Fri | - **Deploy application to Amazon Elastic Container Service (Amazon ECS):**<br>&emsp;+ Create an Amazon ECR repository and push the Docker Image to ECR.<br>&emsp;+ Create an ECS Cluster (using AWS Fargate / EC2 launch type).<br>&emsp;+ Configure Task Definitions (CPU, RAM, Container Image, Port Mapping).<br>&emsp;+ Create and launch an ECS Service to maintain running tasks automatically.<br>&emsp;+ Test application access via Load Balancer/Public IP and clean up resources. | 30/10/2026   | 30/10/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 7 Key Accomplishments:

- Mastered **AWS Backup** for centralized backup strategies and verified disaster recovery practices.
- Successfully established **VPC Peering**, mastering inter-VPC route configurations and security group rules.
- Implemented **AWS Transit Gateway** to centralize network management across multiple VPCs.
- Containerized web applications using **Docker**, mastering image build and container management workflows on Linux.
- Successfully deployed containerized workloads to **Amazon ECS** integrated with **Amazon ECR**, gaining practical experience with ECS Clusters, Task Definitions, and Services.
- Safely terminated all temporary lab infrastructure post-practice to prevent unexpected AWS cloud charges.
