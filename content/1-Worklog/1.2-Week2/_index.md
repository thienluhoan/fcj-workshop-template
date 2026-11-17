---
title: "Week 2 Worklog"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

During the second week (September 21–25, 2026), I studied AWS networking and worked with VPC, EC2, and Route 53 Resolver. I recorded the work by day, moving from network fundamentals to Hybrid DNS testing and resource cleanup.

**Office location:** Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City.

### Week 2 objectives

- Understand how VPCs, CIDRs, subnets, and route tables are organized, and trace network traffic paths.
- Distinguish Security Groups from Network ACLs and consider both outbound and return traffic when checking connectivity.
- Build a public/private subnet environment, connect EC2 instances, and learn to manage private instances through an EC2 Instance Connect Endpoint.
- Compare VPC Peering, Transit Gateway, Site-to-Site VPN, Direct Connect, and load balancers at an architecture-selection level.
- Configure Hybrid DNS components with Route 53 Resolver and test domain-name resolution.
- Identify CloudFormation ownership boundaries and clean up resources according to their dependencies.

### Tasks completed this week

| Day | Completed work | Start date | Completion date | Learning sources |
| --- | --- | --- | --- | --- |
| Monday | Attended the office (attendance calendar: Completed). Studied VPC, CIDR, subnets, route tables, IGW, and NAT; distinguished Security Groups from Network ACLs; reviewed VPC Peering, Transit Gateway, Site-to-Site VPN, Direct Connect, and load balancers. | 09/21/2026 | 09/21/2026 | [Module 02-01](https://www.youtube.com/watch?v=O9Ac_vGHquM), [02-02](https://www.youtube.com/watch?v=BPuD1l2hEQ4), [02-03](https://www.youtube.com/watch?v=CXU8D3kyxIc); [VPC CIDR](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-cidr-blocks.html), [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html); personal attendance calendar supplied on 17 November 2026 (Completed) |
| Tuesday | Reviewed subnet associations with route tables, the roles of IGW and NAT Gateway, Security Group and Network ACL rules, and how the VPC Resource Map helps visualize network components. | 09/22/2026 | 09/22/2026 | [Module 02 playlist, videos 4–11](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab03](https://000003.awsstudygroup.com/); [Subnet sizing](https://docs.aws.amazon.com/vpc/latest/userguide/subnet-sizing.html) |
| Wednesday | Configured a sample `10.10.0.0/16` VPC with two public and two private subnets across two AZs, IGW, route tables, security groups, and EC2 instances; tested connectivity, NAT Gateway, and private EC2 access through EICE. | 09/23/2026 | 09/23/2026 | [Module 02 playlist, videos 12–20](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab03](https://000003.awsstudygroup.com/); [EC2 Instance Connect Endpoint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-with-ec2-instance-connect-endpoint.html) |
| Thursday | Prepared the Hybrid DNS environment with CloudFormation for the network and RDGW, configured the security group and RDGW access, and separately deployed Managed Microsoft AD following FCJ to simulate on-premises DNS. | 09/24/2026 | 09/24/2026 | [Module 02 playlist, videos 21–25](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ CloudFormation](https://000010.awsstudygroup.com/2-prerequiste/2.2-launchcloudformation/), [FCJ Managed AD](https://000010.awsstudygroup.com/4-setupad/) |
| Friday | Configured a Route 53 outbound endpoint, a forwarding rule for `onprem.example.com`, and an inbound endpoint; tested DNS queries in the lab, then cleaned up endpoints, rules/associations, the directory, and the stack according to their ownership boundaries. | 09/25/2026 | 09/25/2026 | [Module 02 playlist, videos 26–31](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ test results](https://000010.awsstudygroup.com/5-setuphyriddns/5.4-results/), [FCJ cleanup](https://000010.awsstudygroup.com/6-cleanup/); [Outbound queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-outbound-queries.html), [Inbound queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-inbound-queries.html) |

### Week 2 achievements

- Completed 31 Module 02, Lab03, and Lab10 videos; organized the material across five study days and connected concepts with the lab environments.
- Distinguished VPCs, subnets, route tables, IGW, NAT Gateway, Security Groups, and Network ACLs; understood how these components work together to route or control traffic.
- Configured a sample VPC with public/private subnets, EC2 instances, required routes, and tested the main connectivity paths in the lab.
- Learned to use an EC2 Instance Connect Endpoint to reach a private EC2 instance through its private IP, while recognizing that IAM, security groups, and routing still affect access.
- Configured Route 53 Resolver components for the Hybrid DNS lab and tested resolution for the simulated `onprem.example.com` domain.
- Cleaned up resources according to ownership: handling separately created components and resources managed by the CloudFormation stack.

### Lessons learned

- I learned to trace the full traffic path when analyzing connectivity: address and subnet, route table, network interface, security group, NACL, and destination service. A suitable route alone does not prove that a connection will succeed.
- I distinguished stateful Security Groups from stateless Network ACLs. When using a NACL, I need to consider rules for both outbound and return traffic, as well as rule evaluation order.
- I learned that public/private describes a subnet's routing, while an EC2 instance's public IP is a separate factor. I also concluded that CIDRs should be planned early to reduce conflicts when connecting networks; AWS allows secondary CIDRs to be associated under supported conditions.
- EICE helped me understand one way to manage a private EC2 instance through its private IP without assigning a public IP to the target. IAM permissions, security groups, and routing still need to be configured appropriately.
- I distinguished the query direction of Route 53 inbound and outbound endpoints. A successful query confirms only the path tested; each direction and application reachability need separate checks when the scope requires them.
- I learned that cleanup is part of the lab process. Before deleting resources, I need to identify which are managed by the stack and which were created separately, then handle them according to their dependencies.

### Next step

In Week 3, I will move on to Multi-VPC, VPC Peering, and Transit Gateway, and study how to deploy and connect EC2 instances in a multi-network architecture.
