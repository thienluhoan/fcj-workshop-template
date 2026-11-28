---
title: "AI worker"
date: 2026-11-27
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Trợ lý AI đọc repository của đội và đề xuất điểm theo rubric. Một lần gọi model mất từ vài giây tới vài phút, nên không bao giờ được chạy trong request HTTP của giám khảo. SEAL chia việc như sau:

1. **API (EC2)** lấy commit SHA, cây thư mục và tối đa 50 file văn bản liên quan (mỗi file 100 KB, tổng 1 MB) qua GitHub API, ghi snapshot (manifest, source, rubric) vào S3, lưu job và outbox trong RDS, rồi gửi message vào **SQS Jobs**. Job bắt đầu ở `PENDING_DISPATCH`, sau đó thành `QUEUED`.
2. **Lambda worker** (ngoài VPC, Python 3.12, 512 MB, timeout 180 giây) nhận từng message một, tối đa 2 message song song. Worker kiểm tra SHA-256 của mọi input, che secret, giới hạn prompt 240.000 ký tự, gọi DeepSeek, kiểm tra kết quả theo JSON schema và ghi `results/{jobId}/output.json` một lần duy nhất.
3. Worker gửi message ngắn vào **SQS Results**. API đọc hàng đợi này, lưu kết quả vào RDS và đánh dấu job `SUCCEEDED` hoặc `FAILED`.

Message lỗi 5 lần sẽ chuyển sang **dead-letter queue**, nơi alarm CloudWatch phát hiện ([5.10](../5.10-testing-validation/)). Worker không bao giờ chạy code của sinh viên, và tiêu chí thuyết trình do giám khảo chấm.

#### Bước 1. Lưu DeepSeek key

Vào **Systems Manager → Parameter Store → Create parameter**, tạo `/fcaj/v3/worker/deepseek-api-key`, loại **SecureString**, dán key vào. Không truyền key qua dòng lệnh vì nó sẽ nằm lại trong lịch sử shell.

#### Bước 2. Đóng gói và triển khai worker

Gói được đặt tên theo commit Git, template bắt buộc điều này:

```powershell
$AISHA = git rev-parse HEAD
git archive --format=zip -o "$env:TEMP\ai-worker-$AISHA.zip" HEAD:ai-worker src
aws s3 cp "$env:TEMP\ai-worker-$AISHA.zip" "s3://fcaj-poc-data-<ACCOUNT_ID>-ap-southeast-1/lambda/ai-worker-$AISHA.zip"
```

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-ai-worker --template-file infra/v3/ai-worker.yaml --capabilities CAPABILITY_IAM --parameter-overrides "CodeKey=lambda/ai-worker-$AISHA.zip"
```

Role của Lambda chỉ có đúng quyền worker cần: đọc `poc/inputs/*`, `poc/rubrics/*`, `poc/results/*`, ghi `poc/results/*`, nhận message từ hàng đợi jobs, gửi vào hàng đợi results, đọc đúng một tham số DeepSeek, ghi log group riêng (giữ 14 ngày).

{{% notice tip %}}
`TriggerEnabled=false` tạm dừng xử lý mà không mất job: message nằm chờ trong hàng đợi tới khi bật lại trigger.
{{% /notice %}}

![Tổng quan Lambda fcaj-v3-ai-worker](/images/5-Workshop/5.7-AI-Worker/lambda-overview.png)

*Function `fcaj-v3-ai-worker` do stack `fcaj-v3-ai-worker` tạo: SQS là trigger duy nhất và không có destination, vì kết quả được gửi lại qua hàng đợi kết quả. **General configuration**: bộ nhớ 512 MB, timeout 3 phút. ARN của function đã được che.*

![Trigger SQS của Lambda worker](/images/5-Workshop/5.7-AI-Worker/lambda-trigger.png)

*Trigger trên hàng đợi `fcaj-poc-ai-jobs`, trạng thái **Enabled**:*

- ***Batch size: 1**: mỗi lần gọi xử lý một job, nên một repository chậm không làm kẹt các job khác.*
- ***Maximum concurrency: 2**: tối đa hai job gọi DeepSeek cùng lúc, giới hạn chi phí và tốc độ gọi API.*
- ***Report batch item failures: Yes**: job lỗi quay lại hàng đợi để thử lại, sau 5 lần nhận thì chuyển sang Job DLQ.*
- *ARN của hàng đợi và của event source mapping đã được che.*

#### Bước 3. Bật AI job ở backend

```powershell
.\infra\v3\set-ai-jobs.ps1 -State on
```

Script đổi `APP_AI_JOBS_ENABLED` và `APP_AI_JOBS_DELIVERY_ENABLED` trong `runtime-env`, không in ra nội dung khác. Các máy nhận thay đổi ở lần release kế tiếp:

```powershell
aws ssm send-command --region ap-southeast-1 --document-name AWS-RunShellScript --targets "Key=tag:SealRole,Values=api" --max-concurrency 1 --max-errors 0 --parameters "commands=/usr/local/bin/seal-release"
```

#### Bước 4. Smoke test

`infra/v3/ai-smoke.sh` trỏ một bài nộp demo sang một repository public nhỏ và mở vòng chấm trong 2 giờ. Script chỉ đổi đúng hai dòng và từ chối chạy nếu dữ liệu không khớp. Chạy trên máy API qua Session Manager:

```bash
sudo bash ai-smoke.sh on
```

Đăng nhập bằng một giám khảo mà script in ra, mở bài nộp và bấm **AI Assist**. Job đi từ `QUEUED` sang `SUCCEEDED` trong khoảng một phút. Trả dữ liệu về như cũ:

```bash
sudo bash ai-smoke.sh off
```

![Trang chấm điểm của giám khảo với kết quả AI Assist](/images/5-Workshop/5.7-AI-Worker/ai-assist-result.webp)

*Trang **Score Submissions** của giám khảo trong một sự kiện thử: vòng chung kết đang **ACTIVE**, đồng hồ chấm **RUNNING**, bài nộp vẫn **NOT SCORED**. Sau khi bấm **AI Assist**, khung **AI Judge Assistant** hiện model đã dùng (`deepseek-flash`), phần tóm tắt repository, **Repository analysis** về cấu trúc mã nguồn, **Strengths** (điểm mạnh) và **Points to probe** (những điểm giám khảo nên hỏi lại đội).*

![Nhận xét theo từng tiêu chí của AI](/images/5-Workshop/5.7-AI-Worker/ai-assist-criteria.webp)

*Ở **Per-criteria notes**, AI đưa một nhận xét cho mỗi tiêu chí trong rubric kèm điểm tham khảo (`[Tham khảo: 8.0/10.0]`). Đây chỉ là gợi ý: không có điểm nào được điền vào form, giám khảo vẫn tự nhập và nộp điểm. Tiêu chí **Presentation** được ghi *giám khảo chấm trực tiếp*, vì phụ thuộc phần thuyết trình mà AI không thấy được. Dòng cuối nhắc lại trợ lý không chấm hay thay đổi điểm chính thức.*

{{% notice note %}}
Dòng cuối vẫn ghi "Mock assistance for integration testing only": backend gắn cố định câu này vào mọi kết quả, kể cả kết quả thật từ DeepSeek. Đây là nhãn còn sót từ giai đoạn thử nghiệm và cần đổi sang nhãn dành cho production.
{{% /notice %}}

![Log stream CloudWatch của Lambda worker](/images/5-Workshop/5.7-AI-Worker/lambda-log.png)

*Một log stream trong `/aws/lambda/fcaj-v3-ai-worker` có hai job. Mỗi job là một nhóm `START` / `END` / `REPORT`; dòng `REPORT` cho biết thời gian tính phí (ở đây khoảng 18–23 giây, phần lớn là chờ DeepSeek) và **Max Memory Used** khoảng 100 MB, thấp hơn nhiều so với 512 MB đã cấu hình. Log không in ra API key.*
