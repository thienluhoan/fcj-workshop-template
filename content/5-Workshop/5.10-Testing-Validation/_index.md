---
title: "Testing and validation"
date: 2026-11-27
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

This lab sets up alarms and audit logging, then proves that the system works, and that it **fails safely**, using CloudWatch logs, metrics and alarms.

#### Step 1. Deploy monitoring and audit

`LoadBalancerName` is the `app/...` part of the ALB ARN, `TargetGroupName` the `targetgroup/...` part.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-monitoring --template-file infra/v3/monitoring.yaml --parameter-overrides AlertEmail=<OPS_EMAIL> LoadBalancerName=<app/...> TargetGroupName=<targetgroup/...> MonthlyBudgetUsd=50
```

Confirm the SNS subscription e-mail; no alert arrives before that.

| Alarm | Metric | Fires when |
|---|---|---|
| `fcaj-v3-web-no-healthy-host` | `AWS/ApplicationELB` HealthyHostCount | Fewer than 1 healthy target |
| `fcaj-v3-web-5xx` | HTTPCode_Target_5XX_Count | 10 or more server errors |
| `fcaj-v3-db-cpu` | `AWS/RDS` CPUUtilization | Above 80 % |
| `fcaj-v3-db-storage` | FreeStorageSpace | Below 3 GiB |
| `fcaj-v3-ai-job-dlq` | `AWS/SQS` visible messages in the Job DLQ | Above 0 |
| `fcaj-v3-ai-result-dlq` | visible messages in the Result DLQ | Above 0 |
| `fcaj-v3-ai-worker-errors` | `AWS/Lambda` Errors | 3 or more |
| Budget | Actual monthly cost before credits | 50 %, 80 %, 100 % of the budget |

All of them notify the SNS topic `fcaj-v3-alerts`.

CloudTrail records every API call in every region to the application bucket under `cloudtrail/`, with log-file validation, and the files expire after 90 days. Deploy `fcaj-poc` again first so the bucket policy allows CloudTrail to write, then:

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-audit --template-file infra/v3/audit.yaml
```

{{% notice info %}}
The **Alarms** page also lists two `TargetTracking-fcaj-v3-runtime-AutoScalingGroup-…` alarms that AWS creates when the CPU 60 % target-tracking policy is attached to the Auto Scaling group: **AlarmHigh** (CPU high, add an instance) and **AlarmLow** (CPU below 42 %, remove an instance). Under light load **AlarmLow stays in ALARM almost all day; this is normal**, not an incident, and it cannot remove an instance because `MinSize` is 2 ([5.5](../5.5-compute-origin/)). Their action is the `ScaleOnCpu` scaling policy, so they send no SNS e-mail. Do not delete or edit them by hand, because Auto Scaling manages them. Typing `fcaj-v3` in the filter box does not hide them, because their names contain that text too, so the list shows 9 alarms: the seven above plus these two.
{{% /notice %}}

![CloudWatch alarms list](/images/5-Workshop/5.10-Testing-Validation/alarms-ok.png)

*The **Alarms** page shows 9 alarms: the 7 `fcaj-v3-*` alarms from the table above, all **OK**, plus the two target-tracking alarms. **AlarmLow** is **In alarm** because the CPU is low, which is the expected state described in the note above.*

#### Step 2. Functional checks

| Flow | Test | Expected |
|---|---|---|
| ①② | Press F5 on `/`, `/login`, `/dashboard`, `/oauth2/redirect` | React page, no 403 |
| ③ | `curl.exe -si https://<DOMAIN>/api/csrf` | 204 and a `Secure` `XSRF-TOKEN` cookie |
| ③ | `curl.exe -m 10 -sI https://origin.<DOMAIN>/` from your machine | Timeout |
| ④ | Sign in, create and edit data | No error |
| ⑤ | Upload a 15 MB file, then refresh | File still there (S3, not local disk) |
| ⑥–⑫ | One AI Assist request ([5.7](../5.7-ai-worker/)) | `SUCCEEDED`, both DLQs at 0 |
| ⑬ | Forgot password to a verified address | OTP e-mail arrives |
| ⑭ | Google and GitHub sign-in, 10 times each | No state error |

#### Step 3. Read the logs

**CloudWatch → Logs Insights**, log group `/fcaj/poc/backend`. Every line carries the request ID, so one request can be followed across the log:

```sql
fields @timestamp, @message
| filter @message like /ERROR|WARN/
| sort @timestamp desc
| limit 50
```

Log group `/aws/lambda/fcaj-v3-ai-worker`, duration and memory of each job:

```sql
filter @type = "REPORT"
| stats count(*) as jobs, avg(@duration) as avgMs, max(@duration) as maxMs, max(@maxMemoryUsed / 1000 / 1000) as maxMemMB by bin(1h)
```

![Logs Insights query on the Lambda REPORT lines](/images/5-Workshop/5.10-Testing-Validation/logs-insights.png)

*The Lambda query over the last 3 days, grouped by hour:*

- *`jobs` is how many jobs ran in that hour; `avgMs` and `maxMs` are their duration, about 15–23 seconds per job.*
- *`maxMemMB` stays around 100 MB, so 512 MB leaves plenty of room. Lowering memory would not save much, since Lambda CPU also scales with memory.*
- *The 1.91 ms row is an invocation far too short to have called DeepSeek, so it is not a reviewed job.*

#### Step 4. Error scenarios

**A. One instance dies during use.** In **EC2 → Instances**, terminate one `fcaj-v3-api` instance while using the site.

- Expected: the site keeps working through the other AZ. In **Target groups**, the target goes `draining`, then the Auto Scaling group launches a replacement that turns `healthy` after about 5 minutes.
- Metric: `HealthyHostCount` drops from 2 to 1, then returns to 2. `no-healthy-host` does **not** fire, because one target is still healthy.

![Auto Scaling group activity history](/images/5-Workshop/5.10-Testing-Validation/asg-activity.png)

*The Auto Scaling group activity history (read bottom-up):*

1. *24 Nov, 22:03: first deploy with `min 1, max 2, desired 1`; the group launches the first instance ([5.5](../5.5-compute-origin/), step 1).*
2. *25 Nov, 09:43: redeploy with `min 2, max 2, desired 2`; the group launches a second instance in the other AZ (step 3).*
3. *25 Nov, 09:57:59: the group registers that the first instance was terminated (the periodic EC2 health check notices about 2 minutes after the ALB, see the chart below) and takes it out of service.*
4. *25 Nov, 09:58:01: only **2 seconds** later the group launches a replacement "in response to an unhealthy instance needing to be replaced". Meanwhile the instance in the other AZ keeps serving requests.*

*Instance IDs are masked.*

![HealthyHostCount during scenario A](/images/5-Workshop/5.10-Testing-Validation/healthy-host-count.png)

*Target group `HealthyHostCount` (Minimum, 1-minute period, Vietnam time, 25 Nov):*

- *09:40–09:48: 1, while the group had a single instance. The second instance, launched at 09:43, needs about 5 minutes to boot, release and pass the health check.*
- *09:49: up to 2, one instance per AZ.*
- *09:56: down to 1 as soon as one instance is terminated. The ALB stops sending it requests, and every request goes to the instance in the other AZ.*
- *10:03: back to 2 when the replacement (launched at 09:58) passes the `/health/ready` check.*

*During the 7 minutes with a single instance the site kept working. The healthy count never reached 0, so the `no-healthy-host` alarm did not fire, as expected.*

**B. Brute-force sign-in.** Send more than 100 failed login attempts from one IP within 5 minutes:

```powershell
1..120 | ForEach-Object { curl.exe -s -o NUL -w "%{http_code} " -X POST https://<DOMAIN>/api/auth/login -H "Content-Type: application/json" -d '{\"email\":\"nobody@example.invalid\",\"password\":\"wrong\"}' }
```

- Expected with `LoginLimitMode=Block`: once WAF has evaluated the rate (it can take a minute or two), responses change from 401/403 (CSRF) to **403 from WAF**.
- Metric: **WAF → Web ACLs → fcaj-v3 → Traffic overview**, rule `login-attempts` shows blocked requests. In Count mode the same rule shows counted requests and nothing is blocked.

![WAF sampled requests for login-attempts](/images/5-Workshop/5.10-Testing-Validation/waf-login-attempts.png)

*WAF → `fcaj-v3` → **Sampled requests**, metric `login-attempts`, after running scenario B twice (120 then 90 requests):*

- *Every sampled request is `POST /api/auth/login` with action **COUNT**: the rule saw the IP pass 100 requests in 5 minutes but, in Count mode, let them through. The backend itself answered 403 (CSRF), so nothing was signed in.*
- *A single burst of 120 requests was not always enough for samples to appear, because WAF needs time to evaluate the rate; a second, slower burst made the rule match.*
- *With `LoginLimitMode=Block` the action column shows **BLOCK** instead.*
- *The source IP column is masked.*

**C. An AI job keeps failing.** The worker reports a failed record back to SQS, which retries it; after 5 receives the message moves to the Job DLQ. To test the whole alert path without waiting for five visibility timeouts, set the alarm state by hand:

```powershell
aws cloudwatch set-alarm-state --region ap-southeast-1 --alarm-name fcaj-v3-ai-job-dlq --state-value ALARM --state-reason "Workshop test"
```

- Expected: an e-mail from SNS within a minute; the alarm returns to OK at the next evaluation because the DLQ is empty.
- When a real message lands in the DLQ, read the Lambda log for its `jobId`, fix the cause, then use **SQS → fcaj-poc-ai-jobs-dlq → Start DLQ redrive** to send it back.

**D. A bad release.** Push a commit whose image fails `/health/ready`. The pipeline stops after the first instance, writes the previous release back and redeploys it; the second instance never runs the bad image and the site stays up.

#### Step 5. Audit

**CloudTrail → Event history**: filter on `Event name = SendCommand` to see who released what and when, or `PutParameter` to see configuration changes. Secret values never appear in these records.
