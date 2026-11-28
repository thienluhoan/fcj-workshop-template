---
title: "Kiểm thử và xác minh"
date: 2026-11-27
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

Bài này dựng alarm và audit log, sau đó chứng minh hệ thống chạy đúng, và **khi lỗi thì lỗi an toàn**, bằng log, metric và alarm của CloudWatch.

#### Bước 1. Triển khai giám sát và audit

`LoadBalancerName` là phần `app/...` trong ARN của ALB, `TargetGroupName` là phần `targetgroup/...`.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-monitoring --template-file infra/v3/monitoring.yaml --parameter-overrides AlertEmail=<OPS_EMAIL> LoadBalancerName=<app/...> TargetGroupName=<targetgroup/...> MonthlyBudgetUsd=50
```

Bấm xác nhận email đăng ký SNS; trước đó sẽ không nhận được cảnh báo nào.

| Alarm | Metric | Kích hoạt khi |
|---|---|---|
| `fcaj-v3-web-no-healthy-host` | `AWS/ApplicationELB` HealthyHostCount | Ít hơn 1 target healthy |
| `fcaj-v3-web-5xx` | HTTPCode_Target_5XX_Count | Từ 10 lỗi server trở lên |
| `fcaj-v3-db-cpu` | `AWS/RDS` CPUUtilization | Trên 80 % |
| `fcaj-v3-db-storage` | FreeStorageSpace | Dưới 3 GiB |
| `fcaj-v3-ai-job-dlq` | `AWS/SQS` số message trong Job DLQ | Lớn hơn 0 |
| `fcaj-v3-ai-result-dlq` | số message trong Result DLQ | Lớn hơn 0 |
| `fcaj-v3-ai-worker-errors` | `AWS/Lambda` Errors | Từ 3 trở lên |
| Budget | Chi phí thực tế trong tháng, trước credit | 50 %, 80 %, 100 % ngân sách |

Tất cả gửi về SNS topic `fcaj-v3-alerts`.

CloudTrail ghi mọi lời gọi API ở mọi region vào bucket ứng dụng, thư mục `cloudtrail/`, có kiểm tra toàn vẹn file log, tự xóa sau 90 ngày. Triển khai lại `fcaj-poc` trước để bucket policy cho CloudTrail ghi, rồi:

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-audit --template-file infra/v3/audit.yaml
```

{{% notice info %}}
Trang **Alarms** còn hiện hai alarm `TargetTracking-fcaj-v3-runtime-AutoScalingGroup-…` mà AWS tự tạo khi gắn policy target tracking CPU 60 % cho Auto Scaling group: **AlarmHigh** (CPU cao, cần thêm máy) và **AlarmLow** (CPU dưới 42 %, muốn bớt máy). Khi hệ thống ít tải, **AlarmLow nằm ở trạng thái ALARM gần như suốt ngày; đây là bình thường**, không phải sự cố, và cũng không bớt được máy vì `MinSize` là 2 ([5.5](../5.5-compute-origin/)). Hành động của hai alarm này là gọi scaling policy `ScaleOnCpu`, không gửi email qua SNS. Không xóa hay sửa chúng bằng tay, vì Auto Scaling sẽ tự quản lý. Gõ `fcaj-v3` vào ô lọc không ẩn được chúng, vì tên của chúng cũng chứa chuỗi đó, nên danh sách hiện 9 alarm: 7 alarm ở trên cộng hai alarm này.
{{% /notice %}}

![Danh sách alarm CloudWatch](/images/5-Workshop/5.10-Testing-Validation/alarms-ok.png)

*Trang **Alarms** có 9 alarm: 7 alarm `fcaj-v3-*` trong bảng trên, tất cả **OK**, cùng hai alarm target tracking. **AlarmLow** ở trạng thái **In alarm** vì CPU thấp, đúng như ghi chú ở trên.*

#### Bước 2. Kiểm tra chức năng

| Luồng | Kiểm tra | Kết quả mong đợi |
|---|---|---|
| ①② | Nhấn F5 ở `/`, `/login`, `/dashboard`, `/oauth2/redirect` | Trang React, không 403 |
| ③ | `curl.exe -si https://<DOMAIN>/api/csrf` | 204 và cookie `XSRF-TOKEN` có `Secure` |
| ③ | `curl.exe -m 10 -sI https://origin.<DOMAIN>/` từ máy cá nhân | Timeout |
| ④ | Đăng nhập, tạo và sửa dữ liệu | Không lỗi |
| ⑤ | Tải lên file 15 MB rồi refresh | File vẫn còn (S3, không phải ổ đĩa máy) |
| ⑥–⑫ | Một lần AI Assist ([5.7](../5.7-ai-worker/)) | `SUCCEEDED`, hai DLQ bằng 0 |
| ⑬ | Quên mật khẩu với địa chỉ đã xác minh | Nhận email OTP |
| ⑭ | Đăng nhập Google và GitHub, mỗi loại 10 lần | Không lỗi state |

#### Bước 3. Đọc log

**CloudWatch → Logs Insights**, log group `/fcaj/poc/backend`. Mỗi dòng log có request ID, nên theo dõi được một request xuyên suốt:

```sql
fields @timestamp, @message
| filter @message like /ERROR|WARN/
| sort @timestamp desc
| limit 50
```

Log group `/aws/lambda/fcaj-v3-ai-worker`, thời gian chạy và bộ nhớ của từng job:

```sql
filter @type = "REPORT"
| stats count(*) as jobs, avg(@duration) as avgMs, max(@duration) as maxMs, max(@maxMemoryUsed / 1000 / 1000) as maxMemMB by bin(1h)
```

![Truy vấn Logs Insights trên dòng REPORT của Lambda](/images/5-Workshop/5.10-Testing-Validation/logs-insights.png)

*Câu truy vấn Lambda trên 3 ngày gần nhất, nhóm theo giờ:*

- *`jobs` là số job chạy trong giờ đó; `avgMs` và `maxMs` là thời gian chạy, khoảng 15–23 giây mỗi job.*
- *`maxMemMB` quanh 100 MB, nên 512 MB còn dư nhiều. Giảm bộ nhớ cũng không tiết kiệm được bao nhiêu, vì CPU của Lambda tăng theo bộ nhớ.*
- *Dòng 1,91 ms là một lần gọi quá ngắn để kịp gọi DeepSeek, nên không phải một job chấm thật.*

#### Bước 4. Các tình huống lỗi

**A. Một máy chết khi đang dùng.** Vào **EC2 → Instances**, terminate một máy `fcaj-v3-api` trong lúc đang dùng web.

- Mong đợi: web vẫn chạy nhờ AZ còn lại. Trong **Target groups**, target chuyển `draining`, rồi Auto Scaling group tạo máy thay thế, khoảng 5 phút sau thành `healthy`.
- Metric: `HealthyHostCount` giảm từ 2 xuống 1 rồi về 2. Alarm `no-healthy-host` **không** kích hoạt vì vẫn còn một target healthy.

![Activity history của Auto Scaling group](/images/5-Workshop/5.10-Testing-Validation/asg-activity.png)

*Activity history của Auto Scaling group (đọc từ dưới lên):*

1. *24/11, 22:03: triển khai lần đầu với `min 1, max 2, desired 1`, group tạo máy thứ nhất ([5.5](../5.5-compute-origin/), bước 1).*
2. *25/11, 09:43: triển khai lại với `min 2, max 2, desired 2`, group tạo máy thứ hai ở AZ còn lại (bước 3).*
3. *25/11, 09:57:59: group ghi nhận máy thứ nhất đã bị terminate (EC2 health check chạy định kỳ nên ghi nhận trễ hơn ALB khoảng 2 phút, xem biểu đồ bên dưới) và đưa máy ra khỏi dịch vụ.*
4. *25/11, 09:58:01: chỉ **2 giây** sau, group tạo máy thay thế "in response to an unhealthy instance needing to be replaced". Trong lúc đó máy ở AZ kia vẫn phục vụ request.*

*Instance ID đã được che.*

![HealthyHostCount trong lúc thử tình huống A](/images/5-Workshop/5.10-Testing-Validation/healthy-host-count.png)

*`HealthyHostCount` của target group (Minimum, chu kỳ 1 phút, giờ Việt Nam, 25/11):*

- *09:40–09:48: mức 1, khi group mới có một máy. Máy thứ hai tạo lúc 09:43 cần khoảng 5 phút để khởi động, release và qua health check.*
- *09:49: lên 2, mỗi AZ một máy.*
- *09:56: xuống 1 ngay khi một máy bị terminate. ALB ngừng gửi request tới máy đó, mọi request đi sang máy ở AZ còn lại.*
- *10:03: về lại 2, khi máy thay thế (tạo lúc 09:58) qua health check `/health/ready`.*

*Trong khoảng 7 phút chỉ còn một máy, web vẫn hoạt động. Số máy healthy không về 0 nên alarm `no-healthy-host` không kích hoạt, đúng như mong đợi.*

**B. Dò mật khẩu.** Gửi hơn 100 lần đăng nhập sai từ một IP trong 5 phút:

```powershell
1..120 | ForEach-Object { curl.exe -s -o NUL -w "%{http_code} " -X POST https://<DOMAIN>/api/auth/login -H "Content-Type: application/json" -d '{\"email\":\"nobody@example.invalid\",\"password\":\"wrong\"}' }
```

- Mong đợi khi `LoginLimitMode=Block`: sau khi WAF tính xong tần suất (có thể mất một hai phút), mã trả về đổi từ 401/403 (CSRF) sang **403 do WAF chặn**.
- Metric: **WAF → Web ACLs → fcaj-v3 → Traffic overview**, rule `login-attempts` hiện số request bị chặn. Ở chế độ Count, rule hiện số request được đếm và không chặn gì.

![Sampled requests của rule login-attempts](/images/5-Workshop/5.10-Testing-Validation/waf-login-attempts.png)

*WAF → `fcaj-v3` → **Sampled requests**, metric `login-attempts`, sau khi chạy tình huống B hai lần (120 rồi 90 request):*

- *Mọi request được lấy mẫu đều là `POST /api/auth/login` với hành động **COUNT**: rule thấy IP vượt 100 request trong 5 phút nhưng, ở chế độ Count, vẫn cho đi qua. Bản thân backend trả 403 (CSRF), nên không có phiên đăng nhập nào.*
- *Một đợt 120 request không phải lúc nào cũng đủ để có mẫu, vì WAF cần thời gian để tính tốc độ; đợt thứ hai, gửi chậm hơn, đã làm rule khớp.*
- *Với `LoginLimitMode=Block`, cột hành động sẽ hiện **BLOCK**.*
- *Cột source IP đã được che.*

**C. Một AI job lỗi liên tục.** Worker báo record lỗi về SQS để thử lại; sau 5 lần nhận, message chuyển sang Job DLQ. Để kiểm tra trọn đường cảnh báo mà không phải chờ năm lần visibility timeout, đặt trạng thái alarm bằng tay:

```powershell
aws cloudwatch set-alarm-state --region ap-southeast-1 --alarm-name fcaj-v3-ai-job-dlq --state-value ALARM --state-reason "Workshop test"
```

- Mong đợi: email từ SNS trong vòng một phút; alarm tự về OK ở lần đánh giá sau vì DLQ đang trống.
- Khi có message thật trong DLQ, đọc log Lambda theo `jobId`, sửa nguyên nhân, rồi vào **SQS → fcaj-poc-ai-jobs-dlq → Start DLQ redrive** để gửi lại.

**D. Một bản phát hành lỗi.** Push commit có image không qua được `/health/ready`. Pipeline dừng sau máy đầu tiên, ghi lại release cũ và triển khai lại; máy thứ hai không bao giờ chạy image lỗi và web vẫn hoạt động.

#### Bước 5. Audit

**CloudTrail → Event history**: lọc `Event name = SendCommand` để xem ai phát hành bản nào, lúc nào, hoặc `PutParameter` để xem thay đổi cấu hình. Giá trị secret không bao giờ xuất hiện trong các bản ghi này.
