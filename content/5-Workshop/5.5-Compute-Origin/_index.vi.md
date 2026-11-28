---
title: "Compute và origin"
date: 2026-11-27
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Stack `fcaj-v3-runtime` chứa mọi thứ tính tiền theo giờ: NAT gateway, Application Load Balancer và Auto Scaling group các máy API. Stack này không chứa dữ liệu, nên có thể xóa sau mỗi buổi và triển khai lại sau ([5.13](../5.13-cleanup/)).

#### Bước 1. Triển khai stack runtime

Đọc origin secret vào biến, không in ra màn hình:

```powershell
$ORIGIN = aws ssm get-parameter --region ap-southeast-1 --with-decryption --name /fcaj/v3/edge/origin-secret --query Parameter.Value --output text
```

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-runtime --template-file infra/v3/runtime.yaml --parameter-overrides DomainName=<DOMAIN> HostedZoneId=<ZONE_ID> OriginSecret=$ORIGIN
```

Template tạo:

| Tài nguyên | Cấu hình | Lý do |
|---|---|---|
| NAT gateway + Elastic IP | Ở public subnet A, làm default route cho subnet app | Gọi ra GitHub, nhà cung cấp OAuth và ECR |
| ALB, listener HTTPS 443 | `ELBSecurityPolicy-TLS13-1-2-2021-06`, chứng chỉ cho `origin.<DOMAIN>` | TLS từ CloudFront tới origin |
| Default action | Trả cố định **403** | Request không có header bí mật bị từ chối |
| Listener rule | Chỉ forward khi `X-Origin-Verify` đúng origin secret | Chỉ CloudFront tới được API |
| Target group | HTTPS 8443, health check `/health/ready` mỗi 15 giây, stickiness 1 giờ | Health có kiểm tra database; state OAuth nằm trên một máy |
| Launch template | Ubuntu 24.04, `t3.small`, gp3 mã hóa, bắt buộc IMDSv2 | Không có SSH key, chỉ vào qua Session Manager |
| Auto Scaling group | Subnet app A và B, min = desired, tối đa 2, ELB health check, policy target tracking CPU 60 % | Trải đều hai AZ, tự thay máy hỏng |
| Bản ghi Route 53 | `origin.<DOMAIN>` → ALB | Tên origin mà CloudFront dùng |

Khi khởi động, user data cài Docker và nginx, tạo chứng chỉ tự ký cho nginx cổng 8443 và cài `/usr/local/bin/seal-release`. Script này dùng chung cho lần khởi động đầu, mỗi lần scale-out và mỗi lần phát hành. Nó:

1. đọc `runtime-env`, `migration-env` và `release` từ Parameter Store;
2. kiểm tra image là URI ECR ghim theo digest `sha256` và revision là Git SHA 40 ký tự;
3. kéo image, kiểm tra schema database, chạy container chỉ trên `127.0.0.1:8080`, rồi chờ `/health/ready`.

Log container đi vào log group CloudWatch `/fcaj/poc/backend` qua driver `awslogs` của Docker.

![ALB của stack runtime, tab Listeners and rules](/images/5-Workshop/5.5-Compute-Origin/alb-listener.png)

*ALB `internet-facing` ở trạng thái **Active**, trải trên hai subnet public ở `ap-southeast-1a` và `1b`. Chỉ có một listener **HTTPS:443** dùng policy `ELBSecurityPolicy-TLS13-1-2-2021-06` và chứng chỉ ACM của tên miền; không có listener HTTP 80. **Default action** trả `403 Forbidden`, nên request nào không mang header bí mật của CloudFront đều bị từ chối. Request hợp lệ đi theo rule forward (cột *Rules*: 2 rule, gồm rule forward và rule mặc định). ARN (có account ID) đã được che.*
![Rule của listener HTTPS:443](/images/5-Workshop/5.5-Compute-Origin/alb-listener-rules.png)

*Hai rule của listener HTTPS:443. **Priority 1**: nếu header `X-Origin-Verify` bằng giá trị bí mật mà CloudFront gắn vào mỗi request, chuyển tiếp tới target group của API. **Last (default)**: mọi request còn lại nhận `403 Forbidden`. Kết hợp với security group chỉ mở cho prefix list của CloudFront, origin không thể bị gọi trực tiếp. Dòng "Target group stickiness: Off" là stickiness ở cấp action forward (dùng khi chia cho nhiều target group); stickiness `lb_cookie` của SEAL được bật trên chính target group. Giá trị header và ARN đã được che.*

#### Bước 2. Nạp database

Lần khởi động đầu tiên, bước release sẽ lỗi vì database chưa có user của ứng dụng. Điều này đúng như dự kiến, stack vẫn hoàn tất.

Nhóm khôi phục database có sẵn từ một bản dump riêng (chứa email thật và hash mật khẩu, nên **không** có trong repository). Script `infra/v3/restore-db.sh`, chạy một lần trên máy API qua **Session Manager**:

- kiểm tra SHA-256 của dump và từ chối chạy nếu database không trống;
- hỏi mật khẩu master của RDS (trong Secrets Manager, output `MasterSecretArn`);
- nạp dump, tạo hai login `fcaj_v3_runtime` và `fcaj_v3_migration` chỉ với quyền cần thiết;
- đánh dấu schema V1 làm baseline Flyway, chạy V2 (AI job) và V3, rồi khởi động backend.

```bash
sudo /snap/bin/aws s3 cp s3://fcaj-poc-data-<ACCOUNT_ID>-ap-southeast-1/poc/restore/restore-db.sh /root/restore-db.sh
sudo bash /root/restore-db.sh
```

Kết quả đúng kết thúc bằng `RESTORE_OK 31 tables` và `RELEASE_OK`. Sau đó xóa dump trên máy và trên S3, **gồm mọi version của object**.

Script `restore-db.sh` chỉ chạy được một lần trên database trống, nên ảnh dưới đây chụp lần chạy lại `seal-release` trên một máy đang chạy (Session Manager, `sudo /usr/local/bin/seal-release`). Đây cũng chính là bước mà `restore-db.sh` gọi ở cuối, và là lệnh pipeline CI/CD chạy trên từng máy ([5.9](../5.9-cicd/)).

![Session Manager chạy seal-release, kết thúc bằng RELEASE_OK](/images/5-Workshop/5.5-Compute-Origin/session-manager-release.png)

*Đọc từ trên xuống:*

- *Script kéo image từ ECR **theo digest** `sha256:8a28…`, không theo tag. Image đã có sẵn nên Docker báo "Image is up to date".*
- *Script chép truststore RDS và `deploy-verified.sh` ra khỏi image.*
- *Flyway kết nối RDS bằng TLS (`sslMode=VERIFY_IDENTITY`) và xác nhận **4 migration** khớp với lịch sử, rồi in `PASS: candidate schema and migration history ready for deploy`. Schema lệch thì script dừng ở đây và không đụng tới container.*
- *Container `seal-backend` chỉ nghe ở `127.0.0.1:8080`. Image không đổi nên container vẫn giữ nguyên ("Up 15 hours"), không có downtime.*
- *Dòng cuối `RELEASE_OK c0f469a…` là commit SHA mà `/health/version` trả về. Script chỉ in dòng này khi phiên bản đúng và `/health/ready` đã sẵn sàng.*
- *Account ID trong địa chỉ ECR và endpoint RDS đã được che.*

Cảnh báo "credentials are stored unencrypted" ở đầu là token ECR mà `docker login` lưu trong `/root/.docker/config.json`. Token này chỉ đọc được bởi root, hết hạn sau 12 giờ và chỉ có quyền pull image. Có thể bỏ cảnh báo bằng credential helper `amazon-ecr-credential-helper`.

#### Bước 3. Kiểm tra target và mở rộng sang hai AZ

**EC2 → Target groups → Targets** phải hiện máy ở trạng thái `healthy`. Sau đó chạy hai máy, mỗi AZ một máy, dùng ELB health check để group tự thay máy khi ứng dụng không còn trả lời:

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-runtime --template-file infra/v3/runtime.yaml --parameter-overrides DesiredCapacity=2 HealthCheckType=ELB
```

![Auto Scaling group của stack runtime, tab Details](/images/5-Workshop/5.5-Compute-Origin/asg-details.png)

*Auto Scaling group sau khi triển khai lại với `DesiredCapacity=2`:*

- **Desired capacity 2**, *Scaling limits* **2 - 2** (min = max = 2), trạng thái **At desired capacity**.
- **Launch template**: `t3.small`, AMI Ubuntu 24.04, một security group (của tầng app), **không có key pair**, vì máy chỉ được truy cập qua Session Manager, không mở SSH.
- **Network**: hai subnet app ở `ap-southeast-1a` và `1b`, chia đều kiểu *Balanced best effort*, nên hai máy luôn nằm ở hai AZ khác nhau.
- ARN của group và ARN của user tạo launch template (có account ID) đã được che.

{{% notice info %}}
Template đặt `MinSize` bằng `DesiredCapacity` và `MaxSize` là 2. Khi chạy 2 máy, min = max = 2 nên policy target tracking CPU 60 % **không thể** thêm hay bớt máy: Auto Scaling group lúc này làm nhiệm vụ **tự thay máy hỏng** (ELB health check) và giữ mỗi AZ một máy. Muốn co giãn thật theo tải trong mùa thi, giữ `MinSize=2` và nâng `MaxSize` (ví dụ 4), chấp nhận chi phí EC2 tăng theo.
{{% /notice %}}

![Target group với hai target healthy](/images/5-Workshop/5.5-Compute-Origin/target-group-healthy.png)

*Target group của API: target type **Instance**, giao thức **HTTPS:8443** (nginx trên mỗi máy nhận TLS rồi chuyển vào loopback 8080). Cả **2 target Healthy, 0 Unhealthy**, mỗi máy `fcaj-v3-api` ở một AZ (`ap-southeast-1a` và `1b`), nên mất một AZ thì API vẫn chạy. ARN và instance ID đã được che.*

Origin **không** được trả lời trực tiếp từ Internet. Từ máy của bạn, lệnh sau phải lỗi (timeout), vì ALB chỉ nhận prefix list của CloudFront:

```powershell
curl.exe -m 10 -sI https://origin.<DOMAIN>/
```
