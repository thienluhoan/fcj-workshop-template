---
title: "Week 11 Worklog"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

<!-- PRIVATE NOTE (not rendered): some days this week are still in progress/planned. Update the tasks and achievements to match what actually happened. -->

### Week 11 Objectives:

- Wire my infrastructure to the team's code: Lambda, API Gateway routes behind a Cognito authorizer, frontend on S3 + CloudFront.
- Run an end-to-end test with the team to collect demo evidence.
- Draft the report sections required by the submission guide: worklog, events, architecture, summary, self-evaluation, feedback.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Recorded the daily/weekly worklog in the FCAJ template (vi/en)<br>- Read the Workshop submission guide, FCAJ Submission and the grading rubric<br>- Matched event photos with the event announcements in the group chat<br>- Updated architecture diagram v2, drafted the summary .md and cost table | 11/23/2026 | 11/23/2026 | [FCAJ Rules – Project](https://hcm-rules.awsfcaj.com/project/content)<br>[FCAJ Rules – Submission](https://hcm-rules.awsfcaj.com/instructions/fcaj-submission)<br>[FCAJ – Worklog sample](https://workshop-sample.awsfcaj.com/vi/1-worklog/) |
| Tue | - Write module `infra/terraform/modules/lambda/`<br>&emsp;+ Package code with `archive_file`, Python 3.12 runtime<br>&emsp;+ `securetable-menu-handler`, `securetable-orders-handler`<br>&emsp;+ Dedicated IAM role: only GetItem/PutItem/UpdateItem/Query on the right table<br>- Add a test asserting no `dynamodb:*` | 11/24/2026 | 11/24/2026 | [Terraform – aws_lambda_function](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function)<br>[Terraform – archive_file](https://registry.terraform.io/providers/hashicorp/archive/latest/docs/data-sources/file)<br>[000133 – 2. Create Lambda functions](https://000133.awsfcaj.com/2-create-lambda-functions/)<br>[IAM Security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| Wed | - Route `GET/POST /menu` → Menu Lambda<br>- Route `GET/POST /orders`, `PATCH /orders/{id}/pay` → Orders Lambda<br>- Cognito authorizer (except `/health`), `aws_lambda_permission`<br>- Test with Postman: 200 / 401 / 403 | 11/25/2026 | 11/25/2026 | [API Gateway – Lambda proxy integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html)<br>[API Gateway – Cognito authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)<br>[000135 – 2. Config API Gateway](https://000135.awsfcaj.com/2-config-api-gw/)<br>[000135 – 3. Test APIs By Postman](https://000135.awsfcaj.com/3-test-api-by-postman/) |
| Thu | - Script `aws s3 sync dist/` + `cloudfront create-invalidation`<br>- Verify OAC: direct S3 access returns 403<br>- Dashboard `SecureTable-Live-Monitoring`, 5xx alarm → SNS | 11/26/2026 | 11/26/2026 | [AWS CLI – s3 sync](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html)<br>[CloudFront – Invalidation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html)<br>[000085 – 2.3 Creating Alerts with CloudWatch Alarm](https://000085.awsfcaj.com/2-cloudwatch-monitor/2-3-cloudwatch-alarm/)<br>[Terraform – aws_cloudwatch_metric_alarm](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm) |
| Fri | - `deploy_and_test.ps1 -OpenBrowser` with the whole team<br>- Flow: sign in (MFA) → order → DynamoDB → kitchen sees the order<br>- Enable WAF and fire an SQLi payload with the AppSec member, capture the 403<br>- `nuke_and_teardown.ps1` after the session | 11/27/2026 | 11/27/2026 | Personal plan – Phase 1<br>[000026 – 3.4 Testing New Rule](https://000026.awsfcaj.com/3-useawswaf/3.4-testingnewrule/)<br>[000135 – 4. Test APIs With Front-end](https://000135.awsfcaj.com/4-test-front-end/) |
| Sat | - Rewrite the 4 event reports in my own words<br>- Finish the Self-evaluation and Sharing & Feedback<br>- Review the worklog, update planned days to what actually happened | 11/28/2026 | 11/28/2026 | [FCAJ Rules – Required report content](https://hcm-rules.awsfcaj.com/project/content)<br>[FCAJ – Sharing and Feedback sample](https://workshop-sample.awsfcaj.com/7-feedback/) |

### Week 11 Achievements:

- The 12-week worklog recorded in the FCAJ template (daily and weekly, bilingual).
- Architecture diagram v2 updated from v1.0.0: WAF attached to API Gateway, correct IAM icon, 3 S3 buckets matching Terraform, AWS Budgets added, 14 numbered flow steps.
- Draft summary .md (Problem, Solution, Workflow, Additional, Cost) and a cost estimate based on Singapore prices.
- `lambda/` module and `/menu`, `/orders` routes behind the Cognito authorizer; frontend served through CloudFront (OAC).
- End-to-end test run with the team, resources torn down to zero afterwards.
