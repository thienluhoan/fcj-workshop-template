---
title: "Dọn dẹp tài nguyên"
date: 2026-11-27
weight: 13
chapter: false
pre: " <b> 5.13. </b> "
---

{{% notice warning %}}
Xóa là không lấy lại được. Thống nhất với nhóm trước, sao lưu những gì muốn giữ (snapshot database, file đã upload), và kiểm tra tên tài nguyên trước mỗi lệnh.
{{% /notice %}}

#### Cách A. Tạm dừng giữa các buổi (giữ nguyên dữ liệu)

Chỉ xóa stack tính tiền theo giờ. Trang web vẫn mở được, API trả 502/504 tới khi triển khai lại.

```powershell
aws cloudformation deploy --region ap-southeast-1 --stack-name fcaj-v3-monitoring --template-file infra/v3/monitoring.yaml --parameter-overrides LoadBalancerName= TargetGroupName=
aws cloudformation delete-stack --region ap-southeast-1 --stack-name fcaj-v3-runtime
```

Lệnh đầu xóa tên ALB trong alarm để không báo động nhầm khi runtime không còn. Nếu nghỉ vài ngày, dừng thêm database (RDS tự bật lại sau 7 ngày):

```powershell
aws rds stop-db-instance --region ap-southeast-1 --db-instance-identifier fcaj-v3-mysql
```

Bật lại: start RDS, triển khai stack runtime như bước [5.5](../5.5-compute-origin/) với `DesiredCapacity=2 HealthCheckType=ELB`, rồi cập nhật tên ALB và target group mới vào stack monitoring. Các máy tự release vì database đã sẵn sàng.

#### Cách B. Xóa toàn bộ

Xóa theo thứ tự ngược với lúc tạo, chờ từng stack xóa xong (`aws cloudformation wait stack-delete-complete …`):

| # | Stack | Region | Ghi chú |
|---|---|---|---|
| 1 | `fcaj-v3-monitoring` | ap-southeast-1 | Alarm, SNS topic, budget |
| 2 | `fcaj-v3-cicd` | ap-southeast-1 | Xóa luôn environment `aws-v3` và các variable trên GitHub |
| 3 | `fcaj-v3-ses` | ap-southeast-1 | |
| 4 | `fcaj-v3-ai-worker` | ap-southeast-1 | |
| 5 | `fcaj-v3-edge` | **us-east-1** | CloudFront mất khoảng 15 phút để tắt |
| 6 | `fcaj-v3-runtime` | ap-southeast-1 | |
| 7 | `fcaj-v3-audit` | ap-southeast-1 | Dừng CloudTrail |
| 8 | `fcaj-v3-base` | ap-southeast-1 | Tắt deletion protection của RDS trước; snapshot cuối được giữ lại |
| 9 | `fcaj-poc` | ap-southeast-1 | |

```powershell
aws rds modify-db-instance --region ap-southeast-1 --db-instance-identifier fcaj-v3-mysql --no-deletion-protection --apply-immediately
aws cloudformation delete-stack --region ap-southeast-1 --stack-name fcaj-v3-base
```

#### Tài nguyên được giữ lại có chủ đích

Một số tài nguyên có `DeletionPolicy: Retain` hoặc `Snapshot` để lỡ tay xóa stack cũng không mất dữ liệu. Xóa bằng tay khi đã chắc chắn:

| Tài nguyên | Cách xóa |
|---|---|
| Bucket web `fcaj-v3-web-…` và bucket dữ liệu `fcaj-poc-data-…` (có versioning) | S3 console → **Empty** (xóa mọi version) → **Delete** |
| Hàng đợi SQS `fcaj-poc-ai-*` (4 hàng đợi) | SQS console → Delete |
| ECR repository `fcaj-poc-backend` | `aws ecr delete-repository --repository-name fcaj-poc-backend --force` |
| Log group `/fcaj/poc/backend` | CloudWatch → Log groups → Delete |
| Snapshot cuối và snapshot thủ công của RDS | RDS → Snapshots → Delete (mỗi snapshot tốn phí lưu trữ hằng tháng) |
| Tham số `/fcaj/v3/*` | `aws ssm delete-parameters --names …` |
| Hosted zone Route 53 | Xóa các bản ghi còn lại rồi xóa zone (0,50 USD/tháng khi còn tồn tại). Trỏ tên miền về DNS của nhà đăng ký nếu vẫn dùng tiếp. |

#### Kiểm tra cuối

- **Billing → Bills**: ngày hôm sau không còn phí đang chạy cho EC2, NAT gateway, ELB, RDS.
- **Resource Explorer** hoặc **Tag Editor** tìm `fcaj` ở cả hai region không còn gì ngoài thứ bạn chủ đích giữ.
- **Cost Explorer**, nhóm theo dịch vụ, giảm về gần 0 trong 48 giờ.
