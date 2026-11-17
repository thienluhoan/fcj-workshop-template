---
title: "Week 5 Worklog"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

During Week 5 (12–16 October 2026), I began preparing the FCAJ Hackathon System as tech lead. The team planned to build on the existing SEAL system while I revisited AWS shared responsibility, IAM and cost management. Preparation activities are organized by day for presentation; documented milestones are linked in the table.

**Office location:** Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City.

### Week 5 Objectives

- Define the direction for developing the FCAJ Hackathon System from SEAL.
- Review the inherited scope, possible AWS use and initial work areas.
- Study shared responsibility, IAM policies and roles, and cloud cost considerations.

### Daily Worklog

| Day | Work | Start Date | Completion Date | References |
| --- | --- | --- | --- | --- |
| Monday | Reviewed the hackathon management problem and the idea of using the existing system as the project foundation. Studied the Shared Responsibility Model to distinguish AWS security responsibilities from those of the team. | 12 October 2026 | 12 October 2026 | [Project repository](https://github.com/nhatnhm1405/fcaj-hackathon-system); [Shared Responsibility Model](https://www.youtube.com/watch?v=tsobAlSg19g) |
| Tuesday | Initialized the repository from the SEAL project and updated the README to reflect the FCAJ Hackathon direction. Reviewed existing application components as a basis for planning. | 13 October 2026 | 13 October 2026 | [Initial commit](https://github.com/nhatnhm1405/fcaj-hackathon-system/commit/3547942); [README direction update](https://github.com/nhatnhm1405/fcaj-hackathon-system/commit/f42a168) |
| Wednesday | Attended the office (attendance calendar: Completed). Created issues for the SEAL-to-FCAJ technical rename and AWS architecture ideas, and made a commit renaming technical identifiers. Reviewed IAM policies and roles in relation to proposed resource permissions. | 14 October 2026 | 14 October 2026 | [Issue #1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/1); [Issue #2](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/2); [Rename commit](https://github.com/nhatnhm1405/fcaj-hackathon-system/commit/3c0de1e3ee9f053c43bf58a355164f0e1ad039c2); [IAM](https://www.youtube.com/watch?v=N_vlJGAqZxo), [IAM Policy](https://www.youtube.com/watch?v=XXjEpIRWJBk), [IAM Role](https://www.youtube.com/watch?v=pJWANviu8QM); personal attendance calendar (Completed) |
| Thursday | Considered how EC2, VPC and S3 could support the system, and distinguished workload IAM permissions from application user roles. Reviewed how IAM roles allow workloads to access resources. | 15 October 2026 | 15 October 2026 | [Issue #2: AWS architecture](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/2); [Create IAM role](https://www.youtube.com/watch?v=j8Gj2DJtZUE), [Using IAM role](https://www.youtube.com/watch?v=Z0liKVSjt_Q); [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| Friday | Organized the work areas to prepare for the proposal: frontend, backend, infrastructure and AI. Reviewed cost optimization and identified Region, workload and operating hours as inputs to an estimate. | 16 October 2026 | 16 October 2026 | [Project repository](https://github.com/nhatnhm1405/fcaj-hackathon-system); [AWS Cost Optimization and Support](https://www.youtube.com/watch?v=IY61YlmXQe8); [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |

### Achievements

- Established the FCAJ Hackathon System repository based on SEAL and recorded its initial direction.
- Created initial issues for the technical rename and AWS architecture ideas, and committed the rename for further work on the development branch.
- Consolidated shared responsibility and IAM concepts, along with inputs needed for a cost estimate.

### Lessons Learned

- Existing functionality should be distinguished from proposed AWS and AI improvements.
- Application user roles and workload IAM permissions address different access questions.
- Cost estimates need explicit workload, Region and operating-schedule assumptions.

### Next Steps

Prepare the proposal and assign frontend, backend, infrastructure and AI work in Week 6.
