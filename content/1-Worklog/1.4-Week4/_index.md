---
title: "Week 4 Worklog"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

During the fourth week (October 5–9, 2026), I worked through AWS Backup, S3 File Gateway, and S3/CloudFront labs, alongside the Module 04 storage lectures. I prepared the backup environment on Monday and completed its notification and restore-testing flow on Friday.

**Office location:** Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City.

### Week 4 objectives

- Configure AWS Backup policies and distinguish backup plans, vaults, resource assignments, and recovery points.
- Understand S3 File Gateway, SMB file shares, and the role of the local cache.
- Deploy an S3 static website and distribute its content through CloudFront with appropriate origin access permissions.
- Practice S3 Versioning, object movement, and Cross-Region Replication (CRR).
- Compare storage types, storage classes, lifecycle policies, CORS, and disaster recovery approaches.
- Complete backup notifications and restore checks, then clean up the lab resources.

### Tasks completed this week

| Day | Completed work | Start date | Completion date | Learning sources |
| --- | --- | --- | --- | --- |
| Monday | Attended the office (attendance calendar: Completed). Prepared S3 storage for the CloudFormation template and Lambda package, deployed the backup lab infrastructure, and configured a backup plan, rule, vault, and resource assignment. Reviewed the backup/restore flow and application recovery criteria for the checks later in the week. | 10/05/2026 | 10/05/2026 | FCJ Lab13: [Introduction](https://000013.awsstudygroup.com/1-introduce/), [S3 preparation](https://000013.awsstudygroup.com/2-prerequiste/2.1-creates3bucket/), [Infrastructure](https://000013.awsstudygroup.com/2-prerequiste/2.2-deployinfras/), [Backup plan](https://000013.awsstudygroup.com/3-createbackupplan/); personal attendance calendar supplied on 17 November 2026 (Completed) |
| Tuesday | Deployed S3 File Gateway on EC2, created an SMB file share, connected it to a Windows client, and checked that a file written through the share appeared in S3; cleaned up Lab24. Then created a separate S3 bucket, uploaded the website files, configured static website hosting and the lab's public access settings, and checked the website endpoint. | 10/06/2026 | 10/06/2026 | [FCJ Lab24 - Storage Gateway](https://000024.awsstudygroup.com/), [Connect file share](https://000024.awsstudygroup.com/2-useawsstoragegw/2.3-mountfileshare/), [Cleanup](https://000024.awsstudygroup.com/3-cleanup/); FCJ Lab57: [Create bucket](https://000057.awsstudygroup.com/2-prerequiste/2.1-creates3bucket/), [Upload data](https://000057.awsstudygroup.com/2-prerequiste/2.2-uploaddata/), [Static website](https://000057.awsstudygroup.com/3-staticwebsite/), [Public access settings](https://000057.awsstudygroup.com/4-blockpublicaccess/), [Object permissions](https://000057.awsstudygroup.com/5-publicobject/), [Test website](https://000057.awsstudygroup.com/6-testwebsite/) |
| Wednesday | Continued Lab57 by re-enabling S3 Block Public Access and configuring CloudFront with an S3 origin and bucket policy. Practiced versioning and version recovery, moving objects between buckets, and CRR; checked the distributed content and replication, then cleaned up the lab resources. | 10/07/2026 | 10/07/2026 | FCJ Lab57: [Block public access](https://000057.awsstudygroup.com/7-cloudfront/7.1-block_all_public_access/), [Configure CloudFront](https://000057.awsstudygroup.com/7-cloudfront/7.2-configure_amazon_cloudfront/), [Test CloudFront](https://000057.awsstudygroup.com/7-cloudfront/7.3-test_cloudfront/), [Versioning](https://000057.awsstudygroup.com/8-versioning/), [Move objects](https://000057.awsstudygroup.com/9-moveobject/), [Replication](https://000057.awsstudygroup.com/10-s3ccr/), [Cleanup](https://000057.awsstudygroup.com/11-cleanup/) |
| Thursday | Studied block, file, and object storage; S3 Access Points, storage classes, lifecycle policies, CORS, and access permissions. Reviewed archive retrieval, hybrid and offline data transfer concepts, and disaster recovery strategies in relation to Recovery Time Objective (RTO) and Recovery Point Objective (RPO). | 10/08/2026 | 10/08/2026 | Module 04: [Storage overview](https://www.youtube.com/watch?v=hsCfP0IxoaM), [S3, Access Points and storage classes](https://www.youtube.com/watch?v=_yunukwcAwc), [Static website, CORS and archive topics](https://www.youtube.com/watch?v=mPBjB6Ltl_Q), [Data transfer, Storage Gateway and Backup](https://www.youtube.com/watch?v=YXn8Q_Hpsu4) |
| Friday | Extended Monday's backup environment by configuring Backup Vault Notifications through the AWS CLI and SNS. Created an on-demand backup and checked the lab's Lambda-based restore and application validation flow through jobs, notifications, and CloudWatch Logs; completed resource cleanup. | 10/09/2026 | 10/09/2026 | FCJ Lab13: [Notifications](https://000013.awsstudygroup.com/4-enablenoti/), [Test restore](https://000013.awsstudygroup.com/5-testrestore/), [Cleanup](https://000013.awsstudygroup.com/6-cleanup/) |

### Week 4 achievements

- Completed the learning material from 4 Module 04 videos and 26 FCJ pages, organized into five daily topic groups.
- Set up the AWS Backup lab and configured its plan, vault, and resource assignment; completed the notification and restore-testing flow at the end of the week.
- Configured an SMB file share through S3 File Gateway, checked the client-to-S3 write path, and cleaned up the gateway lab.
- Deployed an S3 static website and continued with CloudFront distribution and origin access configuration, distinguishing the two access paths.
- Practiced S3 version management, object movement, and CRR, and understood the purpose and scope of each mechanism.
- Consolidated storage selection and recovery criteria, including access patterns, retention, archive retrieval, and the distinction between CORS and access permissions.

### Lessons learned

- I learned that a completed backup job and successful application recovery are separate outcomes. Restore checks need criteria tied to the application's behavior and data; a job status alone does not establish that RTO or RPO targets have been met.
- I distinguished file access through SMB from object access in S3. The gateway cache supports access to files, while changes made directly in S3 require consideration of the cache refresh mechanism.
- I understood that the ACL-based website exercise and the bucket policy used for CloudFront serve different access paths. Origin type and permissions must match how the content is intended to be accessed.
- I learned to check S3 versions and CDN caching separately. Recovering an object version does not by itself ensure that every client immediately receives that version through CloudFront.
- I understood that CRR asynchronously copies eligible objects according to its configuration. It does not replace the full backup and recovery strategy for an application.
- I learned to balance access patterns, retrieval time, retention, and RTO/RPO when choosing storage and recovery approaches. I also distinguished browser CORS settings from the permissions that authorize access to S3 objects.
- I learned to compare job states, notifications, and logs when reviewing a restore test. Cleanup also requires distinguishing stack-managed resources from separately created backup resources, stored data, and test resources.

### Next step

In Week 5, I will continue with the next course modules and begin the project phase.
