---
title: "Blog 1 - Load Balancing and Database Bottlenecks"
date: 2026-11-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

### Publication Details

- **Author:** Nguyen Huynh Minh Nhat (Facebook display name: Nhat Nguyen).
- **Published:** 8 November 2026, at 10:18 p.m. as shown in the supplied screenshot.
- **Published in:** AWS Study Group VN on Facebook.
- **Original post:** [Read on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2325121871586105).

### Load Balancing Distributes Traffic, but Why Might a Website Still Be Slow?

**Scenario:** An event-registration website runs on two EC2 instances behind an Application Load Balancer (ALB), sharing an Amazon RDS database. Registration opens, traffic surges, and pages keep loading even though the targets appear healthy. Adding EC2 instances is a common response, but it helps when application processing capacity is the actual bottleneck.

{{< event-photo src="images/blogs/load-balancing/architecture.png" alt="Users send requests through an ALB to two EC2 instances sharing an Amazon RDS database." caption="The blog's illustrative architecture: ALB, two EC2 instances and a shared RDS database." >}}

#### What Load Balancing Helps With

ALB distributes requests across targets. When health checks mark a target unhealthy and healthy targets remain, new requests can be routed to those healthy targets. Combined with Auto Scaling, more EC2 instances can be added as demand grows. This helps when application CPU or memory is overloaded and the work can be distributed.

#### When the Database Is the Bottleneck

If both instances are waiting for RDS, adding EC2 capacity may simply add more waiting clients. Each instance can also create its own connection pool. For example, at 20 connections per instance, scaling from two to six instances increases the total from 40 to 120. Costs rise, and database pressure may increase.

#### Healthy Does Not Describe the Entire User Experience

A health check may show that the application responds without establishing that users can successfully register. The article also discusses the risk of querying the database through `/health`: if Auto Scaling uses ELB health checks, a slow database may trigger instance replacement. New instances then open more connections, potentially increasing pressure.

The suggested approach is to keep health checks lightweight and separately observe metrics such as `TargetResponseTime` and `HTTPCode_Target_5XX_Count`.

#### Investigate Before Scaling

- High EC2 CPU, fast RDS responses and low connection usage can suggest insufficient application capacity, making additional EC2 instances worth considering.
- Low EC2 utilization with slow responses calls for examining database waits, `DatabaseConnections`, `ReadLatency` and `WriteLatency`, followed by queries, indexes and connection pools.
- RDS Proxy is discussed as a way to manage and reuse database connections. Slow queries still require separate optimization.

Change one factor at a time and repeat the same workload to compare results. Changing several factors together makes it harder to identify what helped.

ALB distributes requests to EC2, but adding instances may not improve performance when the bottleneck is the database. **What would you check first?**

This is a hypothetical technical scenario; the connection counts are illustrative examples.

### Publication Evidence

{{< event-photo src="images/blogs/load-balancing/facebook-publication.png" alt="Screenshot of Nhat Nguyen's AWS Study Group VN post, dated November 8 at 10:18 p.m." caption="Author-supplied screenshot of the published Blog 1 post in AWS Study Group VN." >}}

### References

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
- [Health checks for Application Load Balancer target groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html)
- [CloudWatch metrics for Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html)
- [Amazon RDS Proxy](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)
