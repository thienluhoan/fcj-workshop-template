---
title: "AI worker"
date: 2026-11-27
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

The AI assistant reads a team's repository and proposes scores against the rubric. A model call takes from several seconds to a few minutes, so it must never run inside the judge's HTTP request. SEAL splits the work:

1. **The API (EC2)** reads the commit SHA, the file tree and up to 50 relevant text files (100 KB each, 1 MB in total) through the GitHub API, writes a snapshot (manifest, sources, rubric) to S3, saves the job and an outbox row in RDS and sends a message to **SQS Jobs**. The job starts as `PENDING_DISPATCH`, then becomes `QUEUED`.
2. **The Lambda worker** (outside the VPC, Python 3.12, 512 MB, 180 s timeout) receives one message at a time, at most 2 in parallel. It verifies the SHA-256 of every input, masks secrets, caps the prompt at 240,000 characters, calls DeepSeek, validates the answer against a JSON schema and writes `results/{jobId}/output.json` once.
3. It sends a short message to **SQS Results**. The API polls that queue, stores the result in RDS and marks the job `SUCCEEDED` or `FAILED`.

A message that fails 5 times moves to a **dead-letter queue**, where a CloudWatch alarm picks it up ([5.10](../5.10-testing-validation/)). The worker never runs the students' code, and presentation criteria are left to the human judges.

#### Step 1. Store the DeepSeek key

In **Systems Manager → Parameter Store → Create parameter**, create `/fcaj/v3/worker/deepseek-api-key`, type **SecureString**, and paste the key there. Do not pass it on the command line, where it would stay in the shell history.

#### Step 2. Package and deploy the worker

The package is named after its Git commit, which the template enforces:

```powershell
$AISHA = git rev-parse HEAD
git archive --format=zip -o "$env:TEMP\ai-worker-$AISHA.zip" HEAD:ai-worker src
aws s3 cp "$env:TEMP\ai-worker-$AISHA.zip" "s3://fcaj-poc-data-<ACCOUNT_ID>-ap-southeast-1/lambda/ai-worker-$AISHA.zip"
```

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-ai-worker --template-file infra/v3/ai-worker.yaml --capabilities CAPABILITY_IAM --parameter-overrides "CodeKey=lambda/ai-worker-$AISHA.zip"
```

The Lambda role is scoped to exactly what the worker needs: read `poc/inputs/*`, `poc/rubrics/*`, `poc/results/*`, write `poc/results/*`, consume the jobs queue, send to the results queue, read the one DeepSeek parameter, write its own log group (14-day retention).

{{% notice tip %}}
`TriggerEnabled=false` pauses processing without losing jobs: messages wait in the queue until you turn the trigger back on.
{{% /notice %}}

![Lambda fcaj-v3-ai-worker function overview](/images/5-Workshop/5.7-AI-Worker/lambda-overview.png)

*Function `fcaj-v3-ai-worker`, created by the `fcaj-v3-ai-worker` stack: SQS is its only trigger and it has no destination, because results go back through the results queue. **General configuration**: 512 MB memory, 3-minute timeout. The function ARN is masked.*

![SQS trigger of the Lambda worker](/images/5-Workshop/5.7-AI-Worker/lambda-trigger.png)

*The trigger on queue `fcaj-poc-ai-jobs`, state **Enabled**:*

- ***Batch size: 1**: each invocation handles one job, so a slow repository does not hold up others.*
- ***Maximum concurrency: 2**: at most two jobs call DeepSeek at once, which caps cost and API rate.*
- ***Report batch item failures: Yes**: a failed job goes back to the queue to be retried and, after 5 receives, to the Job DLQ.*
- *The queue and event source mapping ARNs are masked.*

#### Step 3. Turn AI jobs on in the backend

```powershell
.\infra\v3\set-ai-jobs.ps1 -State on
```

The script flips `APP_AI_JOBS_ENABLED` and `APP_AI_JOBS_DELIVERY_ENABLED` in `runtime-env` without printing anything else. The instances pick up the change at the next release:

```powershell
aws ssm send-command --region ap-southeast-1 --document-name AWS-RunShellScript --targets "Key=tag:SealRole,Values=api" --max-concurrency 1 --max-errors 0 --parameters "commands=/usr/local/bin/seal-release"
```

#### Step 4. Smoke test

`infra/v3/ai-smoke.sh` points one demo submission to a small public repository and opens judging for 2 hours. It changes exactly two rows and refuses to run on anything else. Run it on an API instance through Session Manager:

```bash
sudo bash ai-smoke.sh on
```

Sign in as one of the judges it prints, open the submission and click **AI Assist**. The job goes `QUEUED` → `SUCCEEDED` within about a minute. Restore the rows afterwards:

```bash
sudo bash ai-smoke.sh off
```

![Judge scoring page with the AI Assist result](/images/5-Workshop/5.7-AI-Worker/ai-assist-result.webp)

*The judge's **Score Submissions** page during a test event: the final round is **ACTIVE**, the judging timer is **RUNNING**, and the submission is still **NOT SCORED**. After **AI Assist**, the **AI Judge Assistant** panel shows the model used (`deepseek-flash`), a summary of the repository, a **Repository analysis** of its structure, **Strengths** and **Points to probe**, questions the judge can ask the team.*

![Per-criteria notes from the AI](/images/5-Workshop/5.7-AI-Worker/ai-assist-criteria.webp)

*Under **Per-criteria notes** the AI gives one note per rubric criterion with a reference mark (`[Tham khảo: 8.0/10.0]`). These marks are only suggestions: nothing is written to the score form, and the judge still enters and submits every score. **Presentation** is marked *judged in person*, because it depends on the team's pitch, which the AI cannot see. The footer repeats that the assistant does not assign or change official scores.*

{{% notice note %}}
The footer still says "Mock assistance for integration testing only": the backend adds this fixed disclaimer to every result, including real DeepSeek ones. The wording is a leftover from testing and should be changed to a production label.
{{% /notice %}}

![CloudWatch log stream of the Lambda worker](/images/5-Workshop/5.7-AI-Worker/lambda-log.png)

*A log stream in `/aws/lambda/fcaj-v3-ai-worker` with two jobs. Each job is a `START` / `END` / `REPORT` group; the `REPORT` line gives the billed duration (here about 18–23 seconds, mostly waiting for DeepSeek) and **Max Memory Used** of about 100 MB, well below the 512 MB configured. No API key appears in the log.*
