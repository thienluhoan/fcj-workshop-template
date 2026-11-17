---
title: "Week 3 Worklog"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

During the third week (September 28–October 2, 2026), I studied VPC connectivity and EC2, and attended the AWS Cloud and AI Day Hanoi watch party in Ho Chi Minh City on September 29. I moved the Transit Gateway and compute topics around the event day, with AWS Backup scheduled for the start of Week 4.

I also attended the office for Fireside Chat with Dr. Werner on 2 October 2026.

### Week 3 objectives

- Configure VPC Peering and understand the routing, security, and DNS conditions for connectivity between two VPCs.
- Study Transit Gateway attachments, association, propagation, and the relationship between VPC and TGW route tables.
- Understand EC2 instance selection, AMIs, key pairs, snapshots, EBS, and instance store.
- Study User Data, instance metadata, EC2 Auto Scaling, and the roles of EFS, FSx, Lightsail, and AWS MGN.
- Attend the AWS Cloud and AI Day Hanoi watch party and connect the keynote topics to cloud engineering and software validation.

### Tasks completed this week

| Day | Completed work | Start date | Completion date | Learning sources |
| --- | --- | --- | --- | --- |
| Monday | Completed Lab19 by preparing a two-VPC environment, configuring Security Groups and Network ACLs, creating VPC Peering, adding routes on both sides, enabling Cross-Peer DNS, testing private-IP connectivity, and cleaning up the lab resources. | 09/28/2026 | 09/28/2026 | [Module 02 playlist, Lab19 - 9 videos](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab19](https://000019.awsstudygroup.com/); [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-basics.html), [peering DNS](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-dns.html) |
| Tuesday | Attended the office at Bitexco; this session is recorded for both office attendance and Event 2. Attended the AWS Cloud and AI Day Hanoi program through the Ho Chi Minh City watch party and followed the keynote streamed from Hanoi. I reflected on enterprise cloud and AI use cases, invisible engineering, and validating AI-assisted software with appropriate access controls, evidence, and human accountability. | 09/29/2026 | 09/29/2026 | [Event 2: AWS Cloud and AI Day Hanoi - HCMC Watch Party](https://aws.amazon.com/vi/events/cloud-day-hanoi/); personal attendance confirmation on 17 November 2026 |
| Wednesday | Completed Lab20 by connecting four VPCs through Transit Gateway, configuring attachments, association/propagation, and route tables, checking EC2 connectivity, using Reachability Analyzer to review reachability based on the network configuration, and cleaning up the lab resources. | 09/30/2026 | 09/30/2026 | [Module 02 playlist, Lab20 - 7 videos](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab20](https://000020.awsstudygroup.com/); [How Transit Gateway works](https://docs.aws.amazon.com/vpc/latest/tgw/how-transit-gateways-work.html), [Reachability Analyzer](https://docs.aws.amazon.com/vpc/latest/reachability/how-reachability-analyzer-works.html) |
| Thursday | Studied compute VMs, EC2 instance type selection, AMIs, key pairs, and backups; compared Amazon EBS with instance store in terms of attachment, data lifecycle, and use cases. | 10/01/2026 | 10/01/2026 | [Compute VM](https://www.youtube.com/watch?v=-t5h4N6vfBs), [AMI / Backup / Key Pair](https://www.youtube.com/watch?v=yAR6QRT3N1k), [EBS](https://www.youtube.com/watch?v=hKr_TfGP7NY), [Instance store](https://www.youtube.com/watch?v=6IHNDJ85aoQ); [EC2 instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html), [EBS](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html), [Instance store persistence](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-store-lifetime.html) |
| Friday | Attended the office for Fireside Chat with Dr. Werner (08:30–12:00); see the session details below. Studied User Data and instance metadata, the EC2 Auto Scaling mechanism, and the roles of EFS, FSx, Lightsail, and AWS Application Migration Service in instance initialization, scaling, file storage, and application migration. | 10/02/2026 | 10/02/2026 | [User Data](https://www.youtube.com/watch?v=_v_43Wi7zjo), [Metadata](https://www.youtube.com/watch?v=Ew3QRaKJQSA), [EC2 Auto Scaling](https://www.youtube.com/watch?v=bbLcPitXJSY), [Compute services overview](https://www.youtube.com/watch?v=hFVYG8WqfU0); [EC2 User Data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html), [EC2 instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html); [Fireside Chat with Dr. Werner](https://luma.com/j4y481jz) |

### Office Session: Fireside Chat with Dr. Werner

- **Date and scheduled time:** Friday, 2 October 2026, 08:30–12:00 (UTC+7).
- **Location:** Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City.
- **Speaker:** Dr. Werner Vogels, introduced as Amazon's CTO.
- **Host:** Huynh Hoang Long.

I attended this session, which is recorded as an office attendance.

The [published program](https://luma.com/j4y481jz) covered cloud and AI, operating systems at global scale, innovation and technology leadership. It also listed Q&A and networking.

### Week 3 achievements

- Completed 24 course videos and used the AWS EC2 instance types documentation to supplement a video without a transcript; attended the watch party on September 29.
- Configured VPC Peering in the lab and identified the conditions required for connectivity between two VPCs, including non-overlapping CIDRs, routes on both sides, and suitable security rules.
- Built a multi-VPC Transit Gateway lab and distinguished VPC routes from TGW routes, including the roles of attachments, association, and propagation.
- Consolidated EC2 instance selection criteria and compared EBS and instance store data persistence across instance lifecycle actions.
- Distinguished User Data from instance metadata and reviewed the roles of Auto Scaling, EFS, FSx, Lightsail, and MGN.
- Connected the watch party keynote themes to practical cloud engineering, access governance, evidence-based software validation, and human responsibility.

### Lessons learned

- I learned that an active VPC Peering connection alone does not ensure application connectivity. I need to check CIDRs, routes on both sides, Security Groups, Network ACLs, and name resolution along the traffic path.
- I distinguished VPC route tables from TGW route tables. Association selects the TGW route table used for traffic arriving from an attachment, while propagation adds that attachment's routes to the selected table.
- Reachability Analyzer helps me assess reachability based on network configuration; its result does not replace testing a running application connection.
- I learned that choosing EBS or instance store depends on performance and data-retention needs. Instance store is suited to temporary data, while data that must persist needs appropriate durable storage and lifecycle settings.
- I learned how AMIs and User Data support consistent instance initialization, while metadata provides instance information for operational processes. These concepts help me understand Auto Scaling designs.
- From the watch party, I took away that AI-assisted development needs appropriate access controls, test evidence, and accountable human review. Faster delivery still depends on reliable engineering foundations and validation.

### Next step

In Week 4, I will begin with the AWS Backup and restore lab on October 5, then continue with S3, CloudFront, and the next storage topics in the learning path.
