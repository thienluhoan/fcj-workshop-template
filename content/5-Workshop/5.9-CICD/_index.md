---
title: "CI/CD with GitHub Actions"
date: 2026-11-27
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

Every merge to `main` releases the backend and the frontend automatically. GitHub Actions signs in to AWS with **OIDC**: no access key is stored in GitHub, and each run gets credentials that expire within an hour.

#### Step 1. Deploy the CI/CD stack

`Repository` is the immutable OIDC subject of your repository (owner and repository IDs), which survives a rename. Read it with `gh api repos/<OWNER>/<REPO>/actions/oidc/customization/sub`.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-cicd --template-file infra/v3/cicd.yaml --capabilities CAPABILITY_NAMED_IAM --parameter-overrides Repository=<OIDC_SUBJECT> DistributionId=<DISTRIBUTION_ID>
```

The stack creates:

- the GitHub **OIDC identity provider**;
- the role `fcaj-v3-github-release`, which can only be assumed by a workflow running in the GitHub **environment `aws-v3`** of that repository;
- the SSM document **`fcaj-v3-release`**, a fixed command that runs `/usr/local/bin/seal-release`. GitHub can start a release, but cannot run any other command on the instances.

The role may push to the one ECR repository, update the `release` parameter, sync the web bucket, invalidate the distribution and send that one document to instances tagged `SealRole=api`.

#### Step 2. Configure GitHub

1. **Settings → Environments → New environment** `aws-v3`. Under **Deployment branches**, allow only `main`.
2. **Settings → Secrets and variables → Actions → Variables**: add `AWS_V3_ROLE_ARN` (stack output) and `AWS_V3_DISTRIBUTION_ID`. Neither is a secret.

![GitHub environment aws-v3](/images/5-Workshop/5.9-CICD/github-environment.png)

*Environment `aws-v3` in the repository settings:*

- ***Deployment branches and tags**: **Selected branches and tags**, with one rule, `main`. A workflow on any other branch cannot enter the environment, so it cannot assume the AWS role either.*
- *No **Required reviewers** or **Wait timer**: every merge to `main` releases straight away. For a larger team, adding a reviewer here gives a manual approval before production.*
- *The two variables are set at repository level (step 2), so they do not appear on this page.*

#### Step 3. The pipeline

`.github/workflows/aws-v3-release.yml` runs on every push to `main`:

| Job | Steps |
|---|---|
| Backend | JUnit tests, MySQL 8.4 integration tests in a disposable database, build the Docker image from the tested JAR |
| Frontend | `npm ci`, Vitest, `npm run build` |
| Release (environment `aws-v3`) | 1. Assume the role through OIDC. 2. Push the image with an immutable tag `v3-<sha>-<run>-<attempt>` and read its **digest**. 3. Upload hashed frontend assets. 4. Write `<image>@sha256:<digest> <sha>` to `/fcaj/v3/backend/release`. 5. Run `fcaj-v3-release` on **one instance at a time**. 6. Upload `index.html` and invalidate CloudFront. 7. `curl` the public site. |

The release script `.github/scripts/release-v3-backend.sh` waits for each instance to report success. If one fails, it writes the previous release value back and runs the document again, so the fleet returns to the last good image. The ALB keeps serving from the other instance while one restarts.

{{<mermaid>}}
graph LR
  A[Push to main] --> B[Tests]
  B --> C[Image to ECR by digest]
  C --> D[SSM parameter release]
  D --> E[Run Command, host 1]
  E --> F[Run Command, host 2]
  F --> G[S3 sync + CloudFront invalidation]
  E -- failure --> R[Restore previous release]
{{</mermaid>}}

![Successful run of Release AWS v3](/images/5-Workshop/5.9-CICD/actions-run.png)

*Run #9 of **Release AWS v3**, triggered by a push to `main` (commit `c0f469a`, the same SHA as the newest `v3-` image in ECR below). `backend` (tests and image build, 1 m 58 s) and `frontend` (29 s) run in parallel; `release` starts only after both pass. Total 4 m 26 s, status **Success**.*

![Steps of the release job](/images/5-Workshop/5.9-CICD/actions-release-steps.png)

*The `release` job: it signs in to AWS through OIDC (`configure-aws-credentials`, no stored key), pushes the image, uploads the frontend, then **Release backend one host at a time** (1 m 40 s for both instances), publishes the new frontend and finally checks the public website.*

![ECR repository fcaj-poc-backend with v3- images](/images/5-Workshop/5.9-CICD/ecr-images.png)

*Repository `fcaj-poc-backend`:*

- *Top: **Tag mutability: Immutable**, so a `v3-…` tag can never be pushed twice to point at a different image; encryption AES-256.*
- *Bottom: the images filtered on `v3-`. Each tag is `v3-<commit sha>-<run id>-<attempt>`, one per pipeline run, and the newest one is the image the instances last pulled.*
- *There is no lifecycle policy yet, so old images stay (31 in the repository). Adding one that keeps the last few `v3-` images is a cheap improvement.*
- *The repository URI and ARN (which contain the account ID) are masked.*

{{% notice note %}}
The pipeline does not change the database schema and does not deploy the Lambda worker: both are deliberate, reviewed steps ([5.5](../5.5-compute-origin/), [5.7](../5.7-ai-worker/)).
{{% /notice %}}
