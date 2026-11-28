---
title: "CI/CD với GitHub Actions"
date: 2026-11-27
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

Mỗi lần merge vào `main`, backend và frontend được phát hành tự động. GitHub Actions đăng nhập AWS bằng **OIDC**: GitHub không lưu access key nào, mỗi lần chạy nhận credential hết hạn trong một giờ.

#### Bước 1. Triển khai stack CI/CD

`Repository` là OIDC subject cố định của repository (ID của owner và repository), vẫn đúng khi đổi tên repository. Lấy bằng `gh api repos/<OWNER>/<REPO>/actions/oidc/customization/sub`.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-cicd --template-file infra/v3/cicd.yaml --capabilities CAPABILITY_NAMED_IAM --parameter-overrides Repository=<OIDC_SUBJECT> DistributionId=<DISTRIBUTION_ID>
```

Stack tạo:

- **OIDC identity provider** của GitHub;
- role `fcaj-v3-github-release`, chỉ workflow chạy trong **environment `aws-v3`** của repository đó mới assume được;
- SSM document **`fcaj-v3-release`**, một lệnh cố định chạy `/usr/local/bin/seal-release`. GitHub chỉ kích hoạt được việc phát hành, không chạy được lệnh nào khác trên máy.

Role chỉ được đẩy image vào đúng một ECR repository, cập nhật tham số `release`, sync bucket web, invalidate distribution và gửi đúng document đó tới các máy có tag `SealRole=api`.

#### Bước 2. Cấu hình GitHub

1. **Settings → Environments → New environment** `aws-v3`. Ở **Deployment branches**, chỉ cho phép `main`.
2. **Settings → Secrets and variables → Actions → Variables**: thêm `AWS_V3_ROLE_ARN` (output của stack) và `AWS_V3_DISTRIBUTION_ID`. Cả hai đều không phải secret.

![Environment aws-v3 trên GitHub](/images/5-Workshop/5.9-CICD/github-environment.png)

*Environment `aws-v3` trong phần cài đặt repository:*

- ***Deployment branches and tags**: **Selected branches and tags**, chỉ có một rule là `main`. Workflow chạy trên nhánh khác không vào được environment, nên cũng không assume được role AWS.*
- *Không bật **Required reviewers** hay **Wait timer**: mỗi lần merge vào `main` là phát hành ngay. Với nhóm lớn hơn, thêm người duyệt ở đây sẽ có bước phê duyệt thủ công trước production.*
- *Hai biến được đặt ở cấp repository (bước 2), nên không hiện trên trang này.*

#### Bước 3. Pipeline

`.github/workflows/aws-v3-release.yml` chạy mỗi lần push vào `main`:

| Job | Các bước |
|---|---|
| Backend | Test JUnit, test tích hợp MySQL 8.4 trên database tạm, build Docker image từ JAR đã test |
| Frontend | `npm ci`, Vitest, `npm run build` |
| Release (environment `aws-v3`) | 1. Assume role qua OIDC. 2. Đẩy image với tag cố định `v3-<sha>-<run>-<attempt>` và lấy **digest**. 3. Tải asset frontend có hash lên S3. 4. Ghi `<image>@sha256:<digest> <sha>` vào `/fcaj/v3/backend/release`. 5. Chạy `fcaj-v3-release` trên **từng máy một**. 6. Tải `index.html` và invalidate CloudFront. 7. `curl` kiểm tra trang public. |

Script `.github/scripts/release-v3-backend.sh` chờ từng máy báo thành công. Nếu một máy lỗi, script ghi lại giá trị release cũ và chạy lại document, đưa cả cụm về image tốt gần nhất. Trong lúc một máy khởi động lại, ALB vẫn phục vụ bằng máy còn lại.

{{<mermaid>}}
graph LR
  A[Push vào main] --> B[Test]
  B --> C[Image lên ECR theo digest]
  C --> D[SSM parameter release]
  D --> E[Run Command, máy 1]
  E --> F[Run Command, máy 2]
  F --> G[S3 sync + CloudFront invalidation]
  E -- lỗi --> R[Khôi phục release cũ]
{{</mermaid>}}

![Một lần chạy Release AWS v3 thành công](/images/5-Workshop/5.9-CICD/actions-run.png)

*Lần chạy #9 của **Release AWS v3**, kích hoạt bởi push vào `main` (commit `c0f469a`, cùng SHA với image `v3-` mới nhất trong ECR bên dưới). `backend` (test và build image, 1 phút 58 giây) và `frontend` (29 giây) chạy song song; `release` chỉ bắt đầu khi cả hai đều qua. Tổng 4 phút 26 giây, trạng thái **Success**.*

![Các bước của job release](/images/5-Workshop/5.9-CICD/actions-release-steps.png)

*Job `release`: đăng nhập AWS qua OIDC (`configure-aws-credentials`, không lưu key), đẩy image, tải frontend lên, rồi **Release backend one host at a time** (1 phút 40 giây cho cả hai máy), phát hành frontend mới và cuối cùng kiểm tra website công khai.*

![ECR repository fcaj-poc-backend với các image v3-](/images/5-Workshop/5.9-CICD/ecr-images.png)

*Repository `fcaj-poc-backend`:*

- *Phần trên: **Tag mutability: Immutable**, nên một tag `v3-…` không thể bị push lại để trỏ sang image khác; mã hóa AES-256.*
- *Phần dưới: các image lọc theo `v3-`. Mỗi tag có dạng `v3-<commit sha>-<run id>-<attempt>`, mỗi lần chạy pipeline một tag, và image mới nhất là image các máy vừa kéo về.*
- *Chưa có lifecycle policy, nên image cũ vẫn còn (31 image trong repository). Thêm một policy giữ lại vài image `v3-` gần nhất là cải tiến rẻ.*
- *URI và ARN của repository (có account ID) đã được che.*

{{% notice note %}}
Pipeline không thay đổi schema database và không triển khai Lambda worker: cả hai là bước có chủ đích, cần review ([5.5](../5.5-compute-origin/), [5.7](../5.7-ai-worker/)).
{{% /notice %}}
