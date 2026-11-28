---
title: "Chi phí và tối ưu"
date: 2026-11-27
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

#### Ước tính trên AWS Pricing Calculator

Region **ap-southeast-1**, giá on-demand, 730 giờ mỗi tháng, trước credit và thuế. SEAL chạy theo hai chế độ:

| Chế độ | Cấu hình | Ước tính |
|---|---|---|
| **Giữa các mùa** | 1 instance API, 1 NAT gateway, RDS Single-AZ | [130,80 USD/tháng](https://calculator.aws/#/estimate?id=3e1217ee16a09c0ad04b2b440b5b804da413ac03) |
| **Mùa thi** | 2 instance API, 2 NAT gateway, RDS Multi-AZ | [230,29 USD/tháng](https://calculator.aws/#/estimate?id=f8aacf3bec82418faf417658ccbd51a097d02c64) |

Hai link trên gồm hạ tầng AWS. SQS, Lambda, SES và SNS nằm trong free tier, còn DeepSeek tính phí ngoài AWS, nên được cộng thêm trong bảng chi tiết dưới đây.

![AWS Pricing Calculator, ước tính giữa các mùa](/images/5-Workshop/5.11-Cost/calculator-offseason.png)

*Bản ước tính giữa các mùa thi trên AWS Pricing Calculator: phần tổng ở trên (upfront, hằng tháng và 12 tháng) và danh sách dịch vụ kèm region và chi phí hằng tháng. Mở link trong bảng trên để xem cấu hình của từng dịch vụ.*

#### Chi tiết

| Dịch vụ | Giả định | Giữa các mùa (USD) | Mùa thi (USD) |
|---|---|---|---|
| EC2 (API) | `t3.small`, 0,0264 USD/giờ, 1 hoặc 2 instance | 19,27 | 38,54 |
| EBS | 20 GB gp3 mỗi instance | 1,92 | 3,84 |
| Application Load Balancer | 0,0252 USD/giờ + 0,5 hoặc 1,5 LCU | 21,32 | 27,16 |
| NAT Gateway | 0,059 USD/giờ + 0,059 USD/GB, 1 hoặc 2 gateway | 43,37 | 87,32 |
| IPv4 public | 0,005 USD/giờ, ALB (2) + NAT (1 hoặc 2) | 10,95 | 14,60 |
| RDS for MySQL | `db.t4g.micro`, 20 GB gp3, Single-AZ hoặc Multi-AZ | 21,01 | 42,75 |
| AWS WAF | 1 web ACL, 5 rule, 1 triệu hoặc 5 triệu request | 10,60 | 13,00 |
| Route 53 | 1 hosted zone | 0,50 | 0,50 |
| S3 | Frontend, file upload, kết quả AI, log CloudTrail | 0,30 | 0,30 |
| CloudFront | Gói miễn phí vĩnh viễn (1 TB, 10 triệu request) | 0,00 | 0,00 |
| SQS, Lambda, SES, SNS | Phần lớn trong free tier | 0,50 | 1,50 |
| KMS | Key do AWS quản lý | 0,00 | 0,00 |
| CloudWatch | Log, metric, alarm | 2,00 | 4,00 |
| ECR | Khoảng 1 GB image | 0,10 | 0,10 |
| DeepSeek API (bên ngoài) | Khoảng 0,014 USD mỗi job; 40 hoặc 600 job | 0,55 | 8,28 |
| **Tổng** | | **132,39** | **241,89** |

- **Trong 12 tháng** (9 tháng giữa các mùa + 3 tháng thi): khoảng **1.917 USD**.
- **Cấu hình đang chạy khi demo**: 2 instance API (mỗi AZ một), 1 NAT gateway, RDS Single-AZ, khoảng **148 USD/tháng**, tức 5 USD mỗi ngày.

{{% notice info %}}
**NAT gateway là khoản lớn nhất**, còn hơn cả máy chủ. Vì vậy traffic S3 đi qua Gateway Endpoint miễn phí, Lambda worker chạy ngoài VPC (gọi DeepSeek không qua NAT), và NAT gateway thứ hai chỉ bật trong mùa thi.
{{% /notice %}}

#### Các tối ưu đã áp dụng

| Tối ưu | Tác dụng |
|---|---|
| Stack runtime không chứa dữ liệu (NAT, ALB, EC2) **xóa được giữa các buổi**, dựng lại trong khoảng 10 phút | Khi học, 4 giờ × 20 ngày với 1 instance chỉ khoảng 41 USD/tháng thay vì 126 |
| Công tắc theo mùa: `DesiredCapacity`, `MultiAZ`, NAT thứ hai | Chỉ trả tiền cho tính sẵn sàng cao trong mùa thi |
| S3 Gateway Endpoint | Không mất phí dữ liệu NAT cho file upload và snapshot AI |
| Lambda ngoài VPC, kích hoạt bởi SQS | Không tốn tiền khi rảnh, không có traffic NAT khi gọi model |
| RDS Graviton `db.t4g.micro`, `t3.small` ở chế độ credit standard | Đúng kích thước với tải; không bị tính thêm CPU credit bất ngờ |
| Gói miễn phí vĩnh viễn của CloudFront, cache dài cho asset có hash | Gần như toàn bộ traffic tĩnh phục vụ miễn phí từ edge |
| KMS key do AWS quản lý | Không mất 1 USD/tháng cho mỗi customer key |
| Giới hạn thời gian giữ log (Lambda 14 ngày, CloudTrail 90 ngày) | Log không phình mãi |
| AWS Budgets tính chi phí **trước** credit, cảnh báo qua SNS | Thấy được mức chi ngay cả khi credit đang trả |
| `RDS stop` khi nghỉ vài ngày | Chỉ tính phí lưu trữ (RDS tự bật lại sau 7 ngày) |

#### Hướng tối ưu thêm

- **Compute Savings Plan hoặc Reserved Instance** cho database khi nền tảng chạy quanh năm (giảm khoảng 30–40 %).
- **NAT instance** (`t4g.nano`) thay NAT gateway ngoài mùa thi, đổi lại phải tự quản lý.
- **ECS Fargate** để bỏ việc quản lý hệ điều hành từng máy; chi phí tương đương ở quy mô này.
