---
title: "Week 6 Worklog"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

During internship Week 6 (19–23 October 2026), corresponding to project Week 1, I focused on proposal scope and coordination as tech lead. The project aims to upgrade SEAL on AWS and investigate local AI integration. The table organizes preparation activities by day; the architecture, services and demo scenario remain proposals to be implemented and validated.

**Office location:** Bitexco Financial Tower, 2 Hai Trieu Street, Saigon Ward, Ho Chi Minh City.

### Week 6 Objectives

- Define the problem, inherited scope, PoC boundaries and an end-to-end demo scenario.
- Assign proposal inputs to the frontend, backend, infrastructure and AI work areas.
- Identify dependencies involving data, access, deployment and the AI job flow.
- Update the technical rename and consolidate requirements for the next phase.

### Daily Worklog

| Day | Work | Start Date | Completion Date | References |
| --- | --- | --- | --- | --- |
| Monday | Created issues for the lead, frontend, Backend 1, Backend 2 and AI work areas, describing objectives, deliverables, dependencies and proposed proposal deadlines. Opened a PR updating the README direction. | 19 October 2026 | 19 October 2026 | [Issue #4: Lead](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [#5 Frontend](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/5); [#6 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/6); [#7 Backend 2](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/7); [#8 AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/8); [PR #3](https://github.com/nhatnhm1405/fcaj-hackathon-system/pull/3) |
| Tuesday | Prepared the proposal direction by distinguishing inherited SEAL functionality from proposed AWS and AI work, and considered dependencies between data, frontend and worker components. Reviewed IAM policies and roles for workload access. | 20 October 2026 | 20 October 2026 | [Issue #4](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [#6 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/6); [#8 AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/8); [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| Wednesday | Attended the office (attendance calendar: Completed). Documented the problem, objectives, scope and proposed demo scenario in the lead issue. Opened and merged the SEAL-to-FCAJ technical rename PR into `develop`, covering application identifiers, configuration and documentation. | 21 October 2026 | 21 October 2026 | [Issue #4: specification and demo](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [PR #10](https://github.com/nhatnhm1405/fcaj-hackathon-system/pull/10); personal attendance calendar (Completed) |
| Thursday | Reviewed the proposed architecture: frontend on S3/CloudFront, backend on EC2, data and files on RDS/S3, and SQS connecting a local AI worker. Identified job/result, access control and worker-offline questions for the team to resolve. | 22 October 2026 | 22 October 2026 | [Issue #4: proposed scenario](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [Issue #7: deployment and messaging](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/7); [Issue #8: AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/8) |
| Friday | Consolidated expected deliverables and dependencies across work areas, including data flow, cost estimates, security and demo validation. Recorded topics for further review before deployment rather than treating the proposal as an operating system. | 23 October 2026 | 23 October 2026 | [Issue #4: proposal](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [#5 Frontend](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/5); [#6 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/6); [AWS Well-Architected](https://docs.aws.amazon.com/wellarchitected/latest/framework/the-pillars-of-the-framework.html) |

### Achievements

- Documented the problem, objectives, scope and two proposed demo scenarios in the lead issue.
- Created issues to divide proposal inputs by role and identify dependencies between contributors.
- Merged the technical rename PR into `develop`; it had not thereby been merged into `main`.
- Identified directions for AWS deployment and local AI; the end-to-end demo and infrastructure still need implementation and testing.

### Lessons Learned

- A proposal should distinguish existing functionality, planned implementation and evidence needed to validate results.
- Each contributor's work needs clear deliverables and dependencies to support integration.
- A local AI architecture should consider worker downtime, access permissions and job tracking.
- Security and cost need attention during design.

### Next Steps

Continue developing the proposal, the team's own architecture, the job/result contract and the deployment plan. Track progress through issues and test each part before concluding that the integration works.
