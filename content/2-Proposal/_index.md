---
title: "SEAL Hackathon Platform on AWS Proposal"
date: 2026-11-27
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

**Owner:** Nguyen Huynh Minh Nhat, Tech Lead; class FCAJ - FPT - Season 4.

This proposal describes the target architecture, acceptance criteria and cost estimates for SEAL/FCAJ Hackathon. Success criteria and expected outcomes are project targets; deployment and test evidence are documented in the Workshop.

## SEAL Hackathon Platform on AWS
### A Multi-AZ Web Platform with an AI Judge Assistant for the FCAJ Hackathon

#### 1. Executive Summary
SEAL (Software Engineering Agile League) is the academic hackathon run three times a year by the Software Engineering Department and PDP at FPT University HCMC. Our team already built the SEAL Hackathon Management System (React SPA, Java 21 Spring Boot API, MySQL) as a course project. In the First Cloud AI Journey program we move this system to AWS (renamed technically to **FCAJ Hackathon**) and add an **AI Judge Assistant** that reads a team's GitHub repository at a fixed commit and returns rubric-based suggestions with evidence.

The target architecture runs in **Asia Pacific (Singapore) ap-southeast-1**. The frontend is served by Amazon CloudFront from a private S3 bucket and protected by AWS WAF. The API runs in a private EC2 Auto Scaling group behind an Application Load Balancer across two Availability Zones, with data in Amazon RDS for MySQL. AI jobs are decoupled through Amazon SQS and processed by AWS Lambda, which calls the DeepSeek API. Deployment is automated by GitHub Actions with OIDC, Amazon ECR and Systems Manager Run Command. The design scales up during **contest season** (second NAT gateway, RDS Multi-AZ standby, second API instance) and scales down between seasons to control cost.

#### Objectives and Scope
**Target users:** event coordinators (SE Department / PDP staff), internal and guest judges, mentors, and student teams from FPT University and partner universities.

**Objectives and success criteria**
| Objective | Success criterion |
|---|---|
| Run the full contest flow on AWS | Event, team registration, submission, judge scoring and ranking work on the AWS environment through the custom domain `sealaws.io.vn` (CloudFront) |
| Tolerate failures during contest season | Terminating one API instance, or failing over RDS, does not stop submissions; Auto Scaling replaces the instance automatically |
| Support judges with AI | A judge requests an analysis and receives a schema-valid result with file and line evidence; the API never waits for the model |
| Secure by default | No public app or DB subnet resources, no SSH, no long-term access keys, no secrets in Git or images |
| Automated and observable operations | A merge to `main` deploys the tested image by digest; alarms notify the team by email through SNS |
| Controlled cost | A Pricing Calculator estimate for both modes; Budgets alerts configured; all resources removed by the clean-up checklist |

**In scope:** migration of the existing web system to AWS, AI Judge Assistant for public GitHub repositories, CI/CD, monitoring, cost estimation and clean-up.

**Out of scope:** automatic scoring without judges, building or running contestants' code, private repositories, model fine-tuning, multi-region disaster recovery and mobile apps.

#### 2. Problem Statement
#### What's the Problem?
- The current system runs as Docker containers on a single host. A host failure, redeploy or full disk stops the whole contest, and uploaded files are stored on the local filesystem.
- Judges score submissions in separate spreadsheets or by reading each repository by hand. With many teams per round, this is slow, and scores for the same submission can differ a lot between judges.
- There is no automated, auditable deployment. Secrets live in environment files, and there is no central log, alarm or cost alert.
- Traffic is very uneven: almost nothing between seasons, then peaks at submission deadlines and judging days.

#### The Solution
- **Reliable hosting:** private S3 + CloudFront (Origin Access Control) for the React SPA; ALB + EC2 Auto Scaling in private app subnets in two AZs for the API; RDS MySQL in private DB subnets; persistent files in S3.
- **Edge protection:** AWS WAF on CloudFront, HTTPS end to end with AWS Certificate Manager (one certificate in us-east-1 for CloudFront, one in ap-southeast-1 for the ALB), Route 53 for DNS.
- **Asynchronous AI judging:** the API collects a repository snapshot through the GitHub API, writes it to S3, saves an AI job and sends it to an SQS Jobs queue; the API never waits for the model. A Lambda worker reads the snapshot and analyses it with the DeepSeek API, writes the JSON result to S3 and publishes a status event to an SQS Results queue that the API consumes. Both queues have a dead-letter queue. **AI output is only a suggestion; judges keep the final score.**
- **Secure operations:** GitHub Actions assumes an IAM role through STS (OIDC, no long-term keys), pushes images to ECR, uploads the frontend to S3 and deploys the API with SSM Run Command (port 22 closed). Secrets are SecureString parameters in Parameter Store encrypted with KMS. CloudTrail, CloudWatch, SNS and AWS Budgets cover audit, monitoring and cost alerts.

#### Benefits and Return on Investment
- Contest availability no longer depends on one machine: one API instance or one AZ can fail during contest season without stopping submissions.
- Judges get a first-pass analysis per submission with file and line evidence, which shortens review time and gives a common baseline for discussion. The AI cost is about **USD 0.014 per analysed submission** (Section 6).
- Every release is traceable from Git commit to ECR image digest to the running instance, and every AWS change is recorded in CloudTrail.
- Seasonal scaling keeps the monthly bill around **USD 132 between seasons** and **USD 242 during a contest month**, at on-demand prices before credits. For the team, the project is a hands-on study of networking, HA, serverless, security and CI/CD on AWS, and the platform is reusable for future SEAL seasons.

#### 3. Solution Architecture
The platform uses a three-tier architecture (edge, application, data) inside one VPC `10.20.0.0/16` with public, private app and private DB subnets in two Availability Zones. Application services that do not need a VPC (S3, SQS, Lambda, SES) stay regional and are reached through IAM-authorised endpoints; the API reaches S3 through an **S3 Gateway Endpoint** so that file traffic does not pass through the NAT gateway.

{{< event-photo src="images/5-Workshop/5.3-Architecture/seal-aws-v3.webp" alt="SEAL AWS architecture across two AZs with CloudFront, ALB, EC2, RDS and a Lambda AI worker." caption="The team architecture diagram v3, also used in the Workshop section." >}}

**Request and data flows (numbers match the diagram)**

| # | Flow |
|---|---|
| 1 | Users open the React SPA; the browser sends HTTPS requests to CloudFront (DNS in Route 53, filtered by WAF, certificate from ACM us-east-1). |
| 2 | CloudFront serves static files from the private frontend S3 bucket through Origin Access Control. |
| 3 | CloudFront forwards `/api/*` to the Application Load Balancer, which sends traffic to the Auto Scaling group over HTTPS port 8443. |
| 4 | The API reads and writes business data in RDS for MySQL (primary in AZ A). |
| 5 | The API stores avatars, problem statements and reads AI results in the application S3 bucket through the S3 Gateway Endpoint. |
| 6 | Outbound calls (GitHub API to read the commit SHA, the file tree and up to 50 source files of the repository; OAuth token exchange) leave through the NAT gateway and Internet Gateway. |
| 7 | When a judge requests an analysis, the API writes the snapshot (manifest, source, rubric) to the application S3 bucket, saves the job and an outbox row in RDS, then sends a job message to the SQS Jobs queue. |
| 8 | SQS triggers the Lambda AI worker. |
| 9 | Lambda reads the snapshot from S3 (checksum and size verified) and writes the schema-validated JSON result to the application S3 bucket (`results/{jobId}/output.json`, write-once). |
| 10 | Lambda calls the DeepSeek API with a size-limited, cleaned snapshot of the repository. |
| 11 | Lambda sends a `COMPLETED` or `FAILED` event to the SQS Results queue. |
| 12 | The API consumes the Results queue, validates the event and updates the job state in RDS. |
| 13 | The API sends OTP and approval emails through Amazon SES. |
| 14 | The browser signs in with Google or GitHub OAuth. |
| 15 | GitHub Actions gets temporary credentials from STS (OIDC): 15a push image to ECR, 15b upload frontend to S3, 15c/15d deploy with SSM Run Command to the Auto Scaling group, 15e instances pull the image from ECR. |
| 16 | Lambda reads the DeepSeek API key from Parameter Store (SecureString, KMS key). |
| 17 | CloudWatch alarms and AWS Budgets notify the operations team through SNS. |

#### AWS Services Used
- **Amazon Route 53**: hosted zone and DNS records for the frontend and API.
- **Amazon CloudFront**: CDN and single HTTPS entry point; SPA fallback to `index.html`; origin for `/api/*` is the ALB.
- **AWS WAF**: web ACL on CloudFront with three AWS managed rule groups (IP reputation, known bad inputs, common) and two rate-based rules for the login, registration, password reset/OTP and OAuth endpoints.
- **AWS Certificate Manager**: public certificates for CloudFront (us-east-1) and the ALB (ap-southeast-1).
- **Amazon VPC**: two AZs, public subnets (NAT), private app subnets, private DB subnets, Internet Gateway, S3 Gateway Endpoint, three security groups (`sg-alb`, `sg-app`, `sg-db`).
- **Elastic Load Balancing (ALB)**: HTTPS listener, health checks, target group on port 8443.
- **Amazon EC2 Auto Scaling**: Spring Boot API in Docker; minimum 1 instance between seasons, 2 instances (one per AZ) in contest season.
- **NAT Gateway**: outbound Internet for private instances; a second NAT in AZ B only in contest season.
- **Amazon RDS for MySQL**: Single-AZ between seasons, Multi-AZ standby in contest season; automated backups.
- **Amazon S3**: frontend bucket (CloudFront only) and application bucket (files, AI results, CloudTrail logs under separate prefixes).
- **Amazon SQS**: Jobs and Results queues, each with a DLQ.
- **AWS Lambda**: Python AI worker (reads the snapshot from S3, masks secrets, enforces size limits, calls DeepSeek, validates the output schema).
- **Amazon SES**: transactional emails from `no-reply@sealaws.io.vn`, domain verified with DKIM; still in the sandbox, production access is required before mailing real users.
- **AWS Systems Manager**: Parameter Store (SecureString) and Run Command.
- **AWS KMS, AWS IAM, AWS STS**: AWS managed encryption keys (`aws/ssm`, `aws/rds`, `aws/s3`), least-privilege roles, OIDC federation for GitHub Actions.
- **Amazon ECR**: API images tagged by commit SHA, deployed by digest.
- **Amazon CloudWatch, Amazon SNS, AWS CloudTrail, AWS Budgets**: logs, metrics, alarms, notifications, API audit and cost alerts.

#### Why These Services
| Decision | Chosen | Alternatives considered | Reason |
|---|---|---|---|
| API compute | EC2 Auto Scaling + ALB | Single EC2, ECS Fargate, Elastic Beanstalk | Keeps the existing Docker image and Spring Boot stack the team already runs; adds self-healing and multi-AZ. Fargate is a later option once the team is comfortable with ECS. |
| AI worker | AWS Lambda triggered by SQS | Worker on EC2, local GPU machine | AI jobs are bursty and only happen in judging rounds; Lambda costs nothing when idle and scales with the queue. |
| Decoupling | Amazon SQS + DLQ | Direct HTTP call from API to worker | Model calls take seconds to minutes; the queue keeps web requests fast and keeps failed jobs for replay. |
| Database | RDS for MySQL | MySQL on EC2, Aurora Serverless | Same engine as the current schema; managed backups and Multi-AZ only when needed; lower minimum cost than Aurora for this load. |
| Frontend hosting | S3 + CloudFront (OAC) | Amplify Hosting, container on EC2 | Static SPA, low cost, global caching, and one HTTPS entry point shared with the API. |
| AI model | DeepSeek API | Amazon Bedrock, self-hosted model | Low token price and already supported by the worker code (Gemini as fallback). Bedrock is the option if AI traffic must stay inside AWS. |

#### Well-Architected Alignment
| Pillar | How the design addresses it |
|---|---|
| Operational Excellence | CI/CD with OIDC, deploy by image digest with health check and rollback, Run Command instead of SSH, CloudFormation templates, CloudWatch logs and alarms, runbooks |
| Security | WAF at the edge, private app and DB subnets, chained security groups, least-privilege IAM roles per component, KMS-encrypted parameters, no long-term keys, CloudTrail audit |
| Reliability | Two AZs, ALB health checks with Auto Scaling replacement, RDS Multi-AZ and automated backups in contest season, SQS retries with DLQs, idempotent consumers and outbox |
| Performance Efficiency | CloudFront caching for static files, asynchronous AI processing, Auto Scaling by load, S3 Gateway Endpoint for file traffic |
| Cost Optimization | Seasonal scale up and down, serverless pay-per-use for AI and queues, Free Tier usage, right-sized burstable instances, Budgets alerts and clean-up checklist |

#### Component Design
- **Frontend:** React 18 + TypeScript + Vite. `VITE_API_URL` is set at build time in CI. Hashed assets are cached long; `index.html` is short-cached and invalidated after each deploy.
- **Network security:** `sg-alb` accepts HTTPS only from the CloudFront origin-facing prefix list; `sg-app` accepts 8443 only from `sg-alb`; `sg-db` accepts 3306 only from `sg-app`. App and DB subnets have no public IPs. No SSH; administration uses Session Manager and Run Command.
- **API:** Java 21 Spring Boot on Docker, reading configuration from Parameter Store with the instance role. Database schema is managed by Flyway; runtime uses `ddl-auto=validate`.
- **AI job lifecycle:** `PENDING_DISPATCH -> QUEUED -> SUCCEEDED | FAILED`. The job message (`fcaj.ai.message.v1`) carries `jobId`, `submissionId`, `commitSha`, `inputRevision`, `schemaVersion` and S3 references to the manifest, source and rubric with their SHA-256. The API saves the job and an outbox row in one transaction, so a crash cannot lose a job; consumers are idempotent because SQS delivers at least once.
- **AI worker safety:** never runs code from the repository; the backend takes only text files with allowed extensions, skips hidden folders, dependencies, build output, lock files and files over 100 KB, keeps the 50 most relevant files (1 MB total) and verifies each file hash; Lambda masks secrets and truncates to 240,000 characters before calling the model; presentation criteria are left to the judges and never scored by AI; rejects results that do not match the output schema. SQS visibility timeout is set above the Lambda timeout, and failed messages go to the DLQ after a fixed number of retries.
- **Access control:** only judges assigned to the round, and coordinators, can request or read an AI result. The AI never writes to the official Score table.
- **Observability:** CloudWatch logs from the API and Lambda; alarms on ALB 5xx, unhealthy targets, RDS CPU and free storage, DLQ depth and oldest message age.

#### 4. Technical Implementation
**Implementation Phases**
1. **Survey and design:** review the current system, compare hosting options, draw the first architecture and collect cost inputs.
2. **Proof of concept:** single EC2 + RDS + S3 + SQS + CloudFront, Flyway baseline, AI job contract and a mock worker to test the queue flow end to end.
3. **Automation and AI integration:** GitHub Actions with OIDC, ECR, Run Command; Lambda worker with the DeepSeek API; SES and Parameter Store.
4. **High availability and hardening (v3):** move the API into private subnets behind ALB + Auto Scaling, add NAT, WAF, S3 Gateway Endpoint, RDS Multi-AZ for contest season, alarms and Budgets.
5. **Test, document and clean up:** security negative tests, failover and load tests, restore test, workshop write-up, final cost report and resource cleanup.

**Technical Requirements**
- **Application:** React 18/TypeScript/Vite, Java 21/Spring Boot/Spring Security/JPA, MySQL 8.4 with Flyway, Python 3 worker with `boto3` and Pydantic-style schema validation.
- **AWS:** VPC design across two AZs, ALB and Auto Scaling launch templates, RDS Multi-AZ, CloudFront OAC and custom origins, WAF rules, IAM least privilege and OIDC trust policies, SQS + Lambda event source mapping, Parameter Store and KMS.
- **Infrastructure as Code:** AWS CloudFormation templates for the whole infrastructure: `infra/cloudformation/poc.yaml` (data bucket, SQS + DLQ, ECR, log group) and `infra/v3/*.yaml` (base, runtime, edge, ai-worker, ses, cicd, monitoring, audit).
- **CI/CD:** GitHub Actions runs JUnit, MySQL 8.4 integration tests and Vitest, builds the image once, pushes it to ECR and deploys the same digest with Run Command, then checks the health endpoint; rollback redeploys the previous digest.
- **Testing:** JUnit/Mockito, Vitest, pytest, and k6 for load on the submission and scoring APIs.

#### 5. Timeline & Milestones

**Internship period:** 14 September–14 December 2026. **Project end date:** 29 November 2026.

The weeks below are internship weeks, starting on Monday, 14 September 2026. Project Week 1 corresponds to internship Week 6. This table sets out the plan and handover criteria; completion of each item is assessed against deployment results and test evidence.

| Internship week | Dates | Project activities | Deliverables / target milestone |
|---|---|---|---|
| Weeks 1–4 | 14 September–11 October 2026 | AWS foundation learning and participation in the activities recorded in the worklog. | Knowledge and materials supporting project preparation. |
| Week 5 | 12–18 October 2026 | Start the SEAL-to-FCAJ Hackathon direction; review the inherited system and initial work areas. | Repository, initial direction and research issues. |
| Week 6 (Project Week 1) | 19–25 October 2026 | Review the problem, define scope, assign proposal inputs, draft the architecture and collect cost assumptions. | Proposal draft and demo criteria for team review. |
| Weeks 7–8 | 26 October–8 November 2026 | PoC with EC2, RDS, S3, SQS and CloudFront; Flyway baseline, AI job contract and mock worker. | Target one end-to-end PoC AI job with logs and test results. |
| Weeks 9–10 | 9–22 November 2026 | CI/CD with OIDC, ECR and Run Command; Lambda AI worker using DeepSeek, SES and Parameter Store. | Target deployment by image digest and asynchronous AI integration. |
| Week 11 | 23–29 November 2026 | Finalize v3: ALB + Auto Scaling, private subnets, NAT, WAF, RDS operating modes, monitoring and Budgets. Review security, load/failover/restore tests; finalize the workshop, demo, cost report, handover and cleanup checklist. | Close scope and hand over the project by 29 November 2026; support test/demo results with evidence. |
| Weeks 12–14 | 30 November–14 December 2026 | | |

**Target milestones:** proposal and scope in Week 6; end-to-end PoC in Week 8; CI/CD and AI in Week 10; v3, tests, documentation and handover in Week 11, no later than 29 November 2026.

#### 6. Budget Estimation

The figures below are estimates and assumptions from the supplied proposal, rather than actual invoices. Free Tier eligibility, credits and rates need to be checked for the account and operating period.

Estimate for **ap-southeast-1**, on-demand prices, 730 hours per month, before credits and taxes. Two operating modes are priced: **off-season** (1 API instance, 1 NAT, RDS Single-AZ) and **contest season** (2 API instances, 2 NAT, RDS Multi-AZ). The detailed estimate is built in the AWS Pricing Calculator with the same inputs: [off-season](https://calculator.aws/#/estimate?id=3e1217ee16a09c0ad04b2b440b5b804da413ac03) (USD 130.80 per month) and [contest season](https://calculator.aws/#/estimate?id=f8aacf3bec82418faf417658ccbd51a097d02c64) (USD 230.29 per month). These two estimates cover AWS infrastructure only; SQS, Lambda, SES and SNS stay within the free tier and DeepSeek is an external cost, so they are added separately in the table below. The table below is the preliminary breakdown used as input.

#### Infrastructure Costs

| Service | Assumption | Off-season (USD/month) | Contest season (USD/month) |
|---|---|---|---|
| EC2 (API) | t3.small Linux, USD 0.0264/hour; 1 or 2 instances | 19.27 | 38.54 |
| EBS | 20 GB gp3 per instance | 1.92 | 3.84 |
| Application Load Balancer | USD 0.0252/hour + about 0.5 or 1.5 LCU | 21.32 | 27.16 |
| NAT Gateway | USD 0.059/hour + USD 0.059/GB; 1 or 2 gateways | 43.37 | 87.32 |
| Public IPv4 addresses | USD 0.005/hour; ALB (2) + NAT (1 or 2) | 10.95 | 14.60 |
| RDS for MySQL | db.t4g.micro, 20 GB gp3; Single-AZ or Multi-AZ | 21.01 | 42.75 |
| AWS WAF | 1 web ACL, 5 rules, 1M or 5M requests | 10.60 | 13.00 |
| Route 53 | 1 hosted zone | 0.50 | 0.50 |
| S3 | Frontend, application files, AI results, CloudTrail logs (a few GB) | 0.30 | 0.30 |
| CloudFront | Within the always-free tier (1 TB, 10M requests) | 0.00 | 0.00 |
| SQS, Lambda, SES, SNS | Mostly within free tiers; long polling requests | 0.50 | 1.50 |
| KMS | AWS managed keys | 0.00 | 0.00 |
| CloudWatch | Logs, metrics and alarms | 2.00 | 4.00 |
| ECR | About 1 GB of images | 0.10 | 0.10 |
| DeepSeek API (external) | About USD 0.014/job (30K input + 4K output tokens, peak rate); 40 or 600 jobs | 0.55 | 8.28 |
| **Total** | | **132.39** | **241.89** |

- **Long-term 12-month operating-cost scenario (separate from the project delivery schedule):** 9 off-season months + 3 contest months (one per season) = **about USD 1,917**.
- **Reference configuration for the demo:** 2 API instances (one per AZ), 1 NAT gateway, Single-AZ RDS, about USD 148 per month.
- ACM public certificates, S3 Gateway Endpoint, IAM, STS, Run Command and the first CloudTrail management trail have no extra charge. AWS Budgets is within the free budget allowance.
- During the internship the team uses AWS credits on the demo account; real billing is tracked against the Budgets thresholds every week.

#### Cost Optimization
- **Seasonal capacity:** second API instance, second NAT gateway and RDS Multi-AZ run only in contest season. NAT gateways are the largest item, so this is the biggest saving.
- **Pay per use:** Lambda, SQS, SES and SNS cost almost nothing when there are no jobs; CloudFront stays within the always-free tier.
- **Right-sizing:** start with t3.small and db.t4g.micro and resize from CloudWatch metrics. A Graviton (t4g) API instance is about 20% cheaper but needs an arm64 image build.
- **Storage lifecycle:** S3 Lifecycle rules expire old AI results and move CloudTrail logs to cheaper storage classes; CloudWatch log groups have a fixed retention period; ECR lifecycle policy keeps only recent images.
- **Less NAT traffic:** S3 traffic goes through the free S3 Gateway Endpoint instead of the NAT gateway.
- **Clean-up:** the workshop includes a clean-up checklist (Auto Scaling group, ALB, NAT gateways and Elastic IPs, RDS and snapshots, S3 buckets, ECR, Lambda, SQS, log groups, alarms, WAF, hosted zone).

#### 7. Risk Assessment
##### Risk Matrix
| Risk | Impact | Probability |
|---|---|---|
| Cost overrun (NAT, idle resources, forgotten contest-season resources) | Medium | Medium |
| AZ or instance failure during a contest | High | Low |
| AI result wrong, biased or influenced by prompt injection in the repository | High | Medium |
| DeepSeek API outage, rate limit or price change | Medium | Medium |
| Leaked secret or over-privileged IAM role | High | Low |
| Database migration error during release | High | Low |
| Team members still learning AWS; schedule slip | Medium | Medium |

##### Mitigation Strategies
- **Cost:** Budgets alerts at 50%, 80% and 100%; weekly billing review; contest-season resources created and removed by a checklist; cleanup script at the end.
- **Availability:** ALB health checks with Auto Scaling replacement, two AZs and RDS Multi-AZ during contest season; automated RDS backups and a planned restore test.
- **AI quality:** AI output is advisory only, shows evidence per file and line, follows a versioned rubric and schema; judges keep the final decision; prompt-injection test repositories in the test set.
- **AI provider:** retries with backoff and a DLQ; job expiry; the worker keeps a Gemini fallback provider; model ID and price are pinned in configuration.
- **Security:** OIDC instead of access keys, least-privilege roles per component, secrets in Parameter Store with KMS, WAF rate limits, no SSH, CloudTrail for audit.
- **Release:** additive Flyway migrations, deploy by image digest, health check gate and rollback to the previous digest.

##### Contingency Plans
- If the AI provider fails, judges continue scoring manually; queued jobs wait in SQS or move to the DLQ and are replayed later.
- If cost passes the budget, scale back to the off-season mode (1 instance, 1 NAT, Single-AZ RDS) and stop non-essential environments.
- If an AZ fails outside contest season, Auto Scaling launches a new instance in the healthy AZ and RDS is restored from the latest automated backup.

#### 8. Expected Outcomes
##### Technical Improvements
- The contest platform survives the loss of one instance or one AZ during contest season.
- Judges receive AI suggestions with evidence within minutes, without blocking the web requests.
- Releases are automated, traceable and reversible; no long-term AWS keys and no open SSH port.
- Logs, alarms, audit trail and cost alerts are in place from day one.

##### Long-term Value
- A reusable platform for every SEAL season (Spring, Summer, Fall) that can scale up only when needed.
- Per-judge and AI scores per criterion form a dataset for research on scoring consistency between judges.
- A documented, bilingual workshop and CloudFormation templates that future FCAJ teams can follow.
