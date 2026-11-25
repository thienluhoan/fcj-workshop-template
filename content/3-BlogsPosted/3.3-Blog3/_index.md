---
title: "Amazon DocumentDB: A MongoDB-Compatible Database Solution on AWS Cloud"
date: 2026-11-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

{{% notice warning %}}
⚠️ **Disclaimer:** The following content is intended for learning and reference purposes only. Please do not copy it verbatim into your own reports, including this warning.
{{% /notice %}}

# AMAZON DOCUMENTDB: A MONGODB-COMPATIBLE DATABASE SOLUTION ON AWS CLOUD

Imagine an e-commerce application that is growing rapidly and needs to process millions of product records, orders, user profiles, and transaction histories every day.

These types of data often have flexible structures that do not always fit naturally into the fixed rows and columns of relational databases.

If a business builds and manages its own MongoDB infrastructure on on-premises servers, it may face several challenges:

- Managing database servers, infrastructure, and maintenance independently.
- Scaling resources when data volumes and request traffic increase.
- Building backup and data recovery mechanisms.
- Maintaining high availability when server failures occur.
- Spending valuable time managing infrastructure instead of developing applications.

To address these challenges, AWS provides a database service called **Amazon DocumentDB (with MongoDB compatibility)**.

## 1. WHAT IS AMAZON DOCUMENTDB?

Amazon DocumentDB is a fully managed document database service provided by AWS.

Its most notable feature is compatibility with MongoDB.

This means developers can use MongoDB-compatible application code, drivers, and familiar development tools to connect to and interact with Amazon DocumentDB.

Instead of managing the entire database infrastructure themselves, developers can rely on AWS to handle many operational tasks, including:

- Database infrastructure management.
- Automatic storage scaling.
- Automated backup and recovery.
- Database monitoring.
- High availability and automatic failover.

Amazon DocumentDB provides two main cluster deployment models:

**1. Instance-based Clusters**

A cluster architecture that uses database instances to process workloads. It includes a primary instance for read and write operations and optional replica instances for read scaling and availability.

**2. Elastic Clusters**

A distributed cluster architecture designed for large-scale workloads. It uses sharding to distribute data and processing across multiple shards, supporting large-scale applications with high throughput and storage requirements.

Amazon DocumentDB supports use cases such as:

- Building applications with document databases.
- Storing and managing flexible data structures.
- Developing applications that require scalability and high availability.
- Migrating MongoDB-compatible applications to AWS Cloud.

**Important:** MongoDB compatibility does not mean that Amazon DocumentDB supports every MongoDB feature. Developers should verify driver, engine-version, command, and feature compatibility before migration.

## 2. HOW DOES AMAZON DOCUMENTDB WORK?

This is one of the most interesting aspects of its architecture.

Amazon DocumentDB is not simply a database running on cloud servers. It uses a separated architecture between the Compute Layer and Storage Layer to support scalability, availability, and data durability.

### A. Cluster – The Core Component of the System

In Amazon DocumentDB, a cluster is the central resource that manages database instances and shared storage.

An instance-based cluster consists of:

- **Primary Instance:** The main instance responsible for processing write operations and supporting read operations.
- **Replica Instances:** Additional instances that handle read-only operations and support read scaling.
- **Cluster Volume:** A shared storage volume containing the cluster's database data.

An instance-based cluster supports 0 to 16 instances in total, consisting of one primary instance and up to 15 replicas.

An important point is that all write operations must go through the Primary Instance, while both the Primary Instance and Replica Instances can handle read operations.

### B. Compute Layer and Storage Layer – Separating Processing and Storage

One of the key architectural characteristics of Amazon DocumentDB is the separation of computing resources and storage resources.

**Compute Layer**

Provides CPU and memory resources through database instances.

**Storage Layer**

Manages database data through the Cluster Volume.

The Cluster Volume automatically maintains six copies of data across three Availability Zones within an AWS Region.

This architecture increases fault tolerance and reduces the risk of data loss when an infrastructure component fails.

Furthermore, separating these two layers allows AWS to scale computing and storage resources independently.

### C. Automatic Storage Scaling – Automatically Expanding Storage Capacity

A common challenge when managing databases is estimating future storage requirements.

If too little storage is available, the system may run out of capacity as data grows.

If too much storage is provisioned in advance, the organization may waste resources.

Amazon DocumentDB addresses this challenge through automatic storage scaling.

Key characteristics include:

- Storage automatically increases as database data grows.
- Storage expands in 10 GiB increments.
- Instance-based clusters using Engine Version 8.0 and later support up to 256 TiB of storage.
- Earlier instance-based engine versions support up to 128 TiB.
- Elastic Clusters support a different storage architecture with capacity up to 4 PiB.
- No need to manually provision additional storage capacity in advance.

This allows organizations to focus on application growth instead of continuously managing database storage capacity.

### D. Read Replicas – Improving Read Performance

When an application has a large number of users, database read requests can increase significantly.

If all requests are concentrated on a single instance, the system may experience performance bottlenecks.

Amazon DocumentDB supports up to 15 Replica Instances in an instance-based cluster to increase read throughput.

These replicas share the underlying cluster storage volume. Therefore, they do not need to maintain separate copies of the database through instance-to-instance data replication.

Amazon DocumentDB also provides a **Reader Endpoint** that distributes read-only connections across available replicas.

This allows applications to connect to a shared endpoint instead of manually tracking individual replicas.

An important distinction is that the Reader Endpoint balances connections, not individual database queries.

## 3. HOW DOES AMAZON DOCUMENTDB PROVIDE HIGH AVAILABILITY?

For business-critical applications, a database must do more than store information. It must remain available even when infrastructure failures occur.

Amazon DocumentDB provides several mechanisms to improve availability.

### A. Multi-AZ – Distributing Data Across Availability Zones

Amazon DocumentDB maintains six copies of cluster data across three Availability Zones.

Distributing data across multiple independent Availability Zones improves fault tolerance when an individual Availability Zone experiences an outage.

This storage architecture also helps maintain data durability independently of the number of database instances in the cluster.

### B. Automatic Failover – Automatically Recovering from Instance Failures

Imagine that the Primary Instance suddenly becomes unavailable.

If the cluster has Replica Instances deployed across different Availability Zones, Amazon DocumentDB can automatically promote an eligible replica to become the new Primary Instance.

If no replica is available when an instance failure occurs, the service attempts to create a replacement instance automatically.

This mechanism reduces the need for manual intervention when database failures occur.

### C. Automated Backup and Point-in-Time Recovery

Amazon DocumentDB provides continuous automated backups and incremental backup mechanisms.

Important characteristics include:

- Supports Point-in-Time Recovery (PITR).
- Allows cluster restoration to a specific point within the configured backup retention period, up to approximately five minutes before the current time.
- Automated backup retention can be configured from 1 to 35 days.
- Automated backups are stored in AWS-managed Amazon S3 storage.
- Backup operations are designed not to impact cluster performance.

These capabilities help organizations recover their databases after accidental changes, operational mistakes, or unexpected failures.

## 4. WHICH AWS SERVICES CAN WORK WITH AMAZON DOCUMENTDB?

An important lesson when learning AWS is that cloud services should not always be viewed independently.

Amazon DocumentDB can integrate with multiple AWS services to build a complete cloud-based application architecture.

Some important services include:

- **Amazon VPC:** Provides an isolated network environment for deploying database clusters.
- **AWS KMS:** Manages encryption keys used to protect stored database data.
- **Amazon S3:** Stores automated database backups through AWS-managed backup infrastructure.
- **Amazon CloudWatch:** Monitors database performance, metrics, and operational health.
- **AWS CloudTrail:** Records API activities for auditing and security monitoring.
- **AWS CloudFormation:** Automates cluster and instance deployment through Infrastructure as Code (IaC).

**Example: A simplified application architecture**

1. A user sends a request to the application.
2. The Backend Application connects to Amazon DocumentDB through a MongoDB-compatible driver.
3. Write operations are sent to the Primary Instance.
4. Read queries can be distributed across Replica Instances through the Reader Endpoint.
5. Database data is stored in the Cluster Volume and replicated across multiple Availability Zones.
6. Amazon CloudWatch monitors database performance and operational metrics.
7. Amazon DocumentDB continuously performs automated backups to support data recovery.

This demonstrates how multiple AWS components work together to build a scalable and fault-tolerant database architecture.

## 5. IS DATA IN AMAZON DOCUMENTDB SECURE?

Databases often contain critical business information, including customer profiles, product records, transactions, and internal company data.

Therefore, security is an essential part of database architecture.

Amazon DocumentDB provides several security capabilities:

- **Amazon VPC:** Isolates the network environment where the database operates.
- **Network Access Control:** Uses security groups and network configurations to control access to database clusters.
- **Encryption at Rest:** Encrypts stored data using AWS KMS.
- **Encrypted Backups:** Automated backups, snapshots, and cluster replicas are protected when encryption is enabled for the cluster.
- **TLS:** Supports secure connections through Transport Layer Security.

These mechanisms help organizations protect data during transmission, storage, and database operations.

## 6. HOW IS AMAZON DOCUMENTDB PRICED?

This is an important consideration when deploying databases on AWS.

Amazon DocumentDB pricing depends on several main components:

**1. Instance Hours**

Charges for database instances based on their instance class and running time.

**2. I/O Requests**

Charges based on the number of input/output operations performed against the database storage layer, depending on the selected storage configuration.

**3. Backup Storage**

Charges may apply when backup storage exceeds the free backup storage allowance.

**4. Data Transfer**

Charges may apply for data transferred outside AWS or between AWS Regions, depending on the applicable pricing rules.

Amazon DocumentDB also provides different pricing models and configurations, including:

- Provisioned instances.
- Amazon DocumentDB Serverless.
- Standard and I/O-Optimized storage configurations.

AWS currently offers a one-month free trial for eligible new users, subject to usage limits and regional availability.

Therefore, when deploying a real-world database, developers should monitor their resources and check the pricing page for their selected AWS Region to avoid unexpected costs.

## 7. WHICH TOOLS CAN BE USED TO MANAGE AMAZON DOCUMENTDB?

AWS provides several methods for developers and administrators to interact with Amazon DocumentDB.

### A. AWS Management Console

A web-based interface that allows users to:

- Create and manage clusters.
- Create and modify database instances.
- Monitor database status.
- Configure related database resources.

### B. AWS CLI

Allows users to manage Amazon DocumentDB through commands in a terminal.

This is particularly useful when automating administrative operations or integrating database management into deployment workflows.

### C. MongoDB Drivers

Developers can use compatible MongoDB drivers to build applications and connect to Amazon DocumentDB.

This is one of the service's most notable advantages because developers who are already familiar with MongoDB can work with Amazon DocumentDB without completely changing their development tools.

However, developers should verify the supported MongoDB driver versions and features before migrating existing applications.

## 8. KEY TAKEAWAYS

Amazon DocumentDB is more than just a database service running on the Cloud.

Its main strengths include:

- Providing a document database with MongoDB compatibility.
- Managing database infrastructure through a Fully Managed service.
- Separating Compute Layer and Storage Layer.
- Automatically scaling storage capacity.
- Improving read performance through Replica Instances.
- Supporting Multi-AZ architecture and Automatic Failover.
- Providing automated backups and Point-in-Time Recovery.
- Integrating with the AWS ecosystem to build complete cloud architectures.

An important lesson when learning Cloud Engineering and Backend Development:

Choosing a database is not just about how data is stored.

You also need to consider scalability, performance, availability, security, compatibility, and operational costs.

Amazon DocumentDB is a practical example of how AWS provides a Database-as-a-Service solution, allowing developers to focus more on application development instead of managing the entire database infrastructure themselves.

**My takeaway:** A database is not simply a place to store application data. It is a critical component of the entire system architecture. Understanding how a database scales, handles failures, manages data, and controls access is essential when building production-ready cloud applications.

## 9. REFERENCES – AWS OFFICIAL DOCUMENTATION

- [What is Amazon DocumentDB (with MongoDB compatibility)?](https://docs.aws.amazon.com/documentdb/latest/devguide/what-is.html)
- [How Amazon DocumentDB Works](https://docs.aws.amazon.com/documentdb/latest/devguide/how-it-works.html)
- [Understanding Amazon DocumentDB Clusters](https://docs.aws.amazon.com/documentdb/latest/devguide/db-clusters-understanding.html)
- [Amazon DocumentDB High Availability and Replication](https://docs.aws.amazon.com/documentdb/latest/devguide/replication.html)
- [Backing Up and Restoring in Amazon DocumentDB](https://docs.aws.amazon.com/documentdb/latest/devguide/backup_restore.html)
- [Amazon DocumentDB Pricing](https://aws.amazon.com/documentdb/pricing/)
- [Amazon DocumentDB FAQs](https://aws.amazon.com/documentdb/faqs/)
<img width="929" height="540" alt="image" src="https://github.com/user-attachments/assets/62ab38d4-5858-480f-92b7-e238295e71d5" />

#AWS #AmazonDocumentDB #MongoDB #AWSCloud #CloudComputing #Database #BackendDevelopment #CloudEngineering #NoSQL #DatabaseArchitecture

Link: https://www.facebook.com/nguyen.hoang.khang.751420/
