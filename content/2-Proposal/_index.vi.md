---
title: "Đề xuất SEAL Hackathon Platform trên AWS"
date: 2026-11-27
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

**Người phụ trách:** Nguyễn Huỳnh Minh Nhật, Tech Lead; lớp FCAJ - FPT - Season 4.

Bản đề xuất mô tả kiến trúc mục tiêu, tiêu chí nghiệm thu và dự toán cho SEAL/FCAJ Hackathon. Các tiêu chí và kết quả kỳ vọng là mục tiêu của dự án; minh chứng triển khai và kiểm thử được trình bày trong Workshop.

## SEAL Hackathon Platform trên AWS
### Nền tảng web Multi-AZ tích hợp trợ lý AI chấm thi cho FCAJ Hackathon

#### 1. Tóm tắt điều hành
SEAL (Software Engineering Agile League) là cuộc thi hackathon học thuật do Khoa Kỹ thuật Phần mềm và PDP tổ chức ba mùa mỗi năm tại Trường Đại học FPT TP.HCM. Nhóm đã xây dựng Hệ thống quản lý SEAL Hackathon (React SPA, API Java 21 Spring Boot, MySQL) trong môn học. Trong chương trình First Cloud AI Journey, nhóm đưa hệ thống này lên AWS (tên kỹ thuật mới là **FCAJ Hackathon**) và bổ sung **Trợ lý AI chấm thi**, đọc repository GitHub của đội thi tại một commit cố định và trả về gợi ý theo rubric kèm dẫn chứng.

Kiến trúc mục tiêu chạy tại **Asia Pacific (Singapore) ap-southeast-1**. Frontend được Amazon CloudFront phân phối từ S3 bucket private và được AWS WAF bảo vệ. API chạy trong EC2 Auto Scaling group ở private subnet phía sau Application Load Balancer, trải trên hai Availability Zone, dữ liệu lưu trong Amazon RDS for MySQL. AI job được tách rời qua Amazon SQS và xử lý bằng AWS Lambda gọi DeepSeek API. Việc triển khai được tự động hóa bằng GitHub Actions với OIDC, Amazon ECR và Systems Manager Run Command. Thiết kế mở rộng trong **mùa thi** (thêm NAT gateway thứ hai, RDS Multi-AZ standby, instance API thứ hai) và thu nhỏ giữa các mùa để kiểm soát chi phí.

#### Mục tiêu và phạm vi
**Đối tượng sử dụng:** ban tổ chức (Khoa Kỹ thuật Phần mềm / PDP), giám khảo nội bộ và khách mời, mentor, các đội thi đến từ Đại học FPT và các trường đối tác.

**Mục tiêu và tiêu chí hoàn thành**
| Mục tiêu | Tiêu chí hoàn thành |
|---|---|
| Chạy toàn bộ quy trình cuộc thi trên AWS | Sự kiện, đăng ký đội, nộp bài, giám khảo chấm điểm và xếp hạng hoạt động trên môi trường AWS qua domain riêng `sealaws.io.vn` (CloudFront) |
| Chịu lỗi trong mùa thi | Tắt một instance API hoặc failover RDS không làm dừng việc nộp bài; Auto Scaling tự thay instance |
| Hỗ trợ giám khảo bằng AI | Giám khảo yêu cầu phân tích và nhận kết quả đúng schema, có dẫn chứng theo file và dòng; API không phải chờ model |
| Bảo mật mặc định | Không có tài nguyên public trong app subnet và DB subnet, không SSH, không access key dài hạn, không có secret trong Git hoặc image |
| Vận hành tự động và giám sát được | Merge vào `main` sẽ triển khai image đã test theo digest; alarm gửi email cho nhóm qua SNS |
| Kiểm soát chi phí | Có estimate trên Pricing Calculator cho cả hai chế độ; cấu hình cảnh báo Budgets; xóa toàn bộ tài nguyên theo checklist dọn dẹp |

**Trong phạm vi:** chuyển hệ thống web hiện tại lên AWS, Trợ lý AI chấm thi cho repository GitHub public, CI/CD, giám sát, dự toán chi phí và dọn dẹp tài nguyên.

**Ngoài phạm vi:** tự động chấm điểm không qua giám khảo, build hoặc chạy code của đội thi, repository private, fine-tune model, khôi phục thảm họa đa region và ứng dụng di động.

#### 2. Tuyên bố vấn đề
#### Vấn đề hiện tại
- Hệ thống hiện chạy bằng Docker container trên một máy duy nhất. Khi máy lỗi, redeploy hoặc đầy ổ đĩa, cả cuộc thi bị dừng; file tải lên lưu trên filesystem cục bộ.
- Giám khảo chấm bằng các file Excel riêng hoặc tự đọc từng repository. Khi mỗi vòng có nhiều đội, việc chấm rất chậm và điểm của cùng một bài nộp có thể chênh lệch lớn giữa các giám khảo.
- Chưa có quy trình triển khai tự động và có thể truy vết. Secret nằm trong file môi trường, chưa có log tập trung, cảnh báo hay cảnh báo chi phí.
- Lưu lượng rất không đều: gần như không có giữa các mùa, sau đó tăng vọt vào hạn nộp bài và ngày chấm.

#### Giải pháp
- **Hosting ổn định:** S3 private + CloudFront (Origin Access Control) cho React SPA; ALB + EC2 Auto Scaling trong private app subnet ở hai AZ cho API; RDS MySQL trong private DB subnet; file lưu bền vững trên S3.
- **Bảo vệ tại biên:** AWS WAF gắn với CloudFront, HTTPS xuyên suốt với AWS Certificate Manager (một chứng chỉ ở us-east-1 cho CloudFront, một chứng chỉ ở ap-southeast-1 cho ALB), Route 53 quản lý DNS.
- **AI chấm thi bất đồng bộ:** API thu thập snapshot repository qua GitHub API, ghi vào S3, lưu AI job và gửi vào hàng đợi SQS Jobs; API không chờ model. Lambda worker đọc snapshot và phân tích bằng DeepSeek API, ghi kết quả JSON vào S3 và gửi sự kiện trạng thái vào hàng đợi SQS Results để API xử lý. Cả hai hàng đợi đều có dead-letter queue. **Kết quả AI chỉ là gợi ý; giám khảo quyết định điểm chính thức.**
- **Vận hành an toàn:** GitHub Actions assume IAM role qua STS (OIDC, không dùng access key dài hạn), đẩy image lên ECR, upload frontend lên S3 và triển khai API bằng SSM Run Command (đóng port 22). Secret lưu dạng SecureString trong Parameter Store, mã hóa bằng KMS. CloudTrail, CloudWatch, SNS và AWS Budgets đảm nhận audit, giám sát và cảnh báo chi phí.

#### Lợi ích và hoàn vốn đầu tư (ROI)
- Cuộc thi không còn phụ thuộc vào một máy: trong mùa thi, một instance API hoặc một AZ gặp sự cố vẫn không làm dừng việc nộp bài.
- Giám khảo có bản phân tích sơ bộ cho từng bài nộp, kèm dẫn chứng theo file và dòng, giúp rút ngắn thời gian review và tạo mốc chung để thảo luận. Chi phí AI khoảng **0,014 USD cho mỗi bài nộp được phân tích** (Mục 6).
- Mỗi bản phát hành truy vết được từ Git commit tới ECR image digest và instance đang chạy; mọi thay đổi trên AWS được CloudTrail ghi lại.
- Mở rộng theo mùa giúp chi phí hàng tháng khoảng **132 USD giữa các mùa** và **242 USD trong tháng thi**, theo giá on-demand trước credit. Với nhóm, dự án là bài thực hành về mạng, HA, serverless, bảo mật và CI/CD trên AWS; nền tảng tái sử dụng được cho các mùa SEAL sau.

#### 3. Kiến trúc giải pháp
Nền tảng dùng kiến trúc ba tầng (biên, ứng dụng, dữ liệu) trong một VPC `10.20.0.0/16` gồm public subnet, private app subnet và private DB subnet trên hai Availability Zone. Các dịch vụ ứng dụng không cần nằm trong VPC (S3, SQS, Lambda, SES) giữ ở cấp region và được truy cập qua endpoint có xác thực IAM; API truy cập S3 qua **S3 Gateway Endpoint** để lưu lượng file không đi qua NAT gateway.

{{< event-photo src="images/5-Workshop/5.3-Architecture/seal-aws-v3.webp" alt="Kiến trúc SEAL trên AWS với hai AZ, CloudFront, ALB, EC2, RDS và Lambda AI worker." caption="Sơ đồ kiến trúc v3 của nhóm, dùng chung với phần Workshop." >}}

**Luồng xử lý (số thứ tự khớp với sơ đồ)**

| # | Luồng |
|---|---|
| 1 | Người dùng mở React SPA; trình duyệt gửi request HTTPS tới CloudFront (DNS ở Route 53, lọc bởi WAF, chứng chỉ từ ACM us-east-1). |
| 2 | CloudFront phục vụ file tĩnh từ S3 bucket frontend private qua Origin Access Control. |
| 3 | CloudFront chuyển `/api/*` tới Application Load Balancer; ALB chuyển tiếp tới Auto Scaling group qua HTTPS cổng 8443. |
| 4 | API đọc và ghi dữ liệu nghiệp vụ trong RDS for MySQL (primary ở AZ A). |
| 5 | API lưu avatar, đề thi và đọc kết quả AI trong S3 bucket ứng dụng qua S3 Gateway Endpoint. |
| 6 | Các lời gọi ra ngoài (GitHub API để lấy commit SHA, cây thư mục và tối đa 50 file nguồn của repository; trao đổi token OAuth) đi qua NAT gateway và Internet Gateway. |
| 7 | Khi giám khảo yêu cầu phân tích, API ghi snapshot (manifest, source, rubric) vào S3 bucket ứng dụng, lưu job và outbox trong RDS, rồi gửi job vào hàng đợi SQS Jobs. |
| 8 | SQS kích hoạt Lambda AI worker. |
| 9 | Lambda đọc snapshot từ S3 (kiểm tra checksum và kích thước) và ghi kết quả JSON đã kiểm tra schema vào S3 bucket ứng dụng (`results/{jobId}/output.json`, chỉ ghi một lần). |
| 10 | Lambda gọi DeepSeek API với bản snapshot repository đã được làm sạch và giới hạn kích thước. |
| 11 | Lambda gửi sự kiện `COMPLETED` hoặc `FAILED` vào hàng đợi SQS Results. |
| 12 | API nhận sự kiện từ hàng đợi Results, kiểm tra hợp lệ và cập nhật trạng thái job trong RDS. |
| 13 | API gửi email OTP và email phê duyệt qua Amazon SES. |
| 14 | Trình duyệt đăng nhập bằng Google hoặc GitHub OAuth. |
| 15 | GitHub Actions lấy credential tạm thời từ STS (OIDC): 15a đẩy image lên ECR, 15b upload frontend lên S3, 15c/15d triển khai bằng SSM Run Command tới Auto Scaling group, 15e instance kéo image từ ECR. |
| 16 | Lambda đọc DeepSeek API key từ Parameter Store (SecureString, khóa KMS). |
| 17 | CloudWatch alarm và AWS Budgets gửi thông báo tới nhóm vận hành qua SNS. |

#### Các dịch vụ AWS sử dụng
- **Amazon Route 53**: hosted zone và bản ghi DNS cho frontend và API.
- **Amazon CloudFront**: CDN và điểm vào HTTPS duy nhất; SPA fallback về `index.html`; origin cho `/api/*` là ALB.
- **AWS WAF**: web ACL gắn với CloudFront, dùng ba AWS managed rule group (IP reputation, known bad inputs, common) và hai rate-based rule cho các endpoint đăng nhập, đăng ký, quên mật khẩu/OTP và OAuth.
- **AWS Certificate Manager**: chứng chỉ public cho CloudFront (us-east-1) và ALB (ap-southeast-1).
- **Amazon VPC**: hai AZ, public subnet (NAT), private app subnet, private DB subnet, Internet Gateway, S3 Gateway Endpoint, ba security group (`sg-alb`, `sg-app`, `sg-db`).
- **Elastic Load Balancing (ALB)**: listener HTTPS, health check, target group cổng 8443.
- **Amazon EC2 Auto Scaling**: API Spring Boot chạy trong Docker; tối thiểu 1 instance giữa các mùa, 2 instance (mỗi AZ một) trong mùa thi.
- **NAT Gateway**: kết nối Internet chiều ra cho instance private; NAT thứ hai ở AZ B chỉ bật trong mùa thi.
- **Amazon RDS for MySQL**: Single-AZ giữa các mùa, Multi-AZ standby trong mùa thi; bật backup tự động.
- **Amazon S3**: bucket frontend (chỉ CloudFront đọc) và bucket ứng dụng (file, kết quả AI, log CloudTrail, tách theo prefix).
- **Amazon SQS**: hàng đợi Jobs và Results, mỗi hàng đợi có một DLQ.
- **AWS Lambda**: AI worker viết bằng Python (đọc snapshot từ S3, che secret, giới hạn kích thước, gọi DeepSeek, kiểm tra schema đầu ra).
- **Amazon SES**: email giao dịch từ `no-reply@sealaws.io.vn`, domain đã xác minh với DKIM; đang ở sandbox, cần xin production access trước khi gửi cho người dùng thật.
- **AWS Systems Manager**: Parameter Store (SecureString) và Run Command.
- **AWS KMS, AWS IAM, AWS STS**: khóa mã hóa do AWS quản lý (`aws/ssm`, `aws/rds`, `aws/s3`), role theo nguyên tắc quyền tối thiểu, liên kết OIDC cho GitHub Actions.
- **Amazon ECR**: image API gắn tag theo commit SHA, triển khai theo digest.
- **Amazon CloudWatch, Amazon SNS, AWS CloudTrail, AWS Budgets**: log, metric, alarm, thông báo, audit API và cảnh báo chi phí.

#### Lý do lựa chọn dịch vụ
| Quyết định | Lựa chọn | Phương án đã cân nhắc | Lý do |
|---|---|---|---|
| Compute cho API | EC2 Auto Scaling + ALB | Một EC2, ECS Fargate, Elastic Beanstalk | Giữ nguyên Docker image và stack Spring Boot nhóm đang chạy; bổ sung khả năng tự phục hồi và multi-AZ. Fargate là hướng sau khi nhóm đã quen ECS. |
| AI worker | AWS Lambda kích hoạt bởi SQS | Worker trên EC2, máy GPU cục bộ | AI job chỉ phát sinh theo đợt trong vòng chấm; Lambda không tốn phí khi không chạy và tự mở rộng theo hàng đợi. |
| Tách rời xử lý | Amazon SQS + DLQ | API gọi HTTP trực tiếp tới worker | Lời gọi model mất vài giây đến vài phút; hàng đợi giữ request web nhanh và giữ lại job lỗi để chạy lại. |
| Database | RDS for MySQL | MySQL trên EC2, Aurora Serverless | Cùng engine với schema hiện tại; backup được quản lý và Multi-AZ chỉ bật khi cần; chi phí tối thiểu thấp hơn Aurora với tải này. |
| Hosting frontend | S3 + CloudFront (OAC) | Amplify Hosting, container trên EC2 | SPA tĩnh, chi phí thấp, cache toàn cầu và dùng chung một điểm vào HTTPS với API. |
| Model AI | DeepSeek API | Amazon Bedrock, tự host model | Giá token thấp và worker đã hỗ trợ sẵn (Gemini làm dự phòng). Bedrock là phương án khi cần giữ lưu lượng AI bên trong AWS. |

#### Đối chiếu AWS Well-Architected
| Trụ cột | Cách thiết kế đáp ứng |
|---|---|
| Operational Excellence | CI/CD với OIDC, triển khai theo image digest có health check và rollback, Run Command thay cho SSH, template CloudFormation, log và alarm CloudWatch, runbook |
| Security | WAF tại biên, app subnet và DB subnet private, security group xếp tầng, IAM role quyền tối thiểu cho từng thành phần, parameter mã hóa KMS, không access key dài hạn, audit bằng CloudTrail |
| Reliability | Hai AZ, health check của ALB kết hợp Auto Scaling thay instance, RDS Multi-AZ và backup tự động trong mùa thi, SQS retry với DLQ, consumer idempotent và outbox |
| Performance Efficiency | CloudFront cache file tĩnh, xử lý AI bất đồng bộ, Auto Scaling theo tải, S3 Gateway Endpoint cho lưu lượng file |
| Cost Optimization | Mở rộng và thu nhỏ theo mùa, serverless trả theo lượt dùng cho AI và hàng đợi, tận dụng Free Tier, instance burstable đúng kích thước, cảnh báo Budgets và checklist dọn dẹp |

#### Thiết kế thành phần
- **Frontend:** React 18 + TypeScript + Vite. `VITE_API_URL` được gán lúc build trong CI. Asset có hash được cache lâu; `index.html` cache ngắn và được invalidate sau mỗi lần deploy.
- **Bảo mật mạng:** `sg-alb` chỉ nhận HTTPS từ prefix list origin-facing của CloudFront; `sg-app` chỉ nhận 8443 từ `sg-alb`; `sg-db` chỉ nhận 3306 từ `sg-app`. App subnet và DB subnet không có IP public. Không dùng SSH; quản trị qua Session Manager và Run Command.
- **API:** Java 21 Spring Boot trong Docker, đọc cấu hình từ Parameter Store bằng instance role. Schema database do Flyway quản lý; runtime dùng `ddl-auto=validate`.
- **Vòng đời AI job:** `PENDING_DISPATCH -> QUEUED -> SUCCEEDED | FAILED`. Message job (`fcaj.ai.message.v1`) gồm `jobId`, `submissionId`, `commitSha`, `inputRevision`, `schemaVersion` và tham chiếu S3 tới manifest, source, rubric kèm SHA-256. API lưu job và bản ghi outbox trong cùng một transaction nên crash không làm mất job; consumer xử lý idempotent vì SQS giao message ít nhất một lần.
- **An toàn của AI worker:** không bao giờ chạy code của repository; backend chỉ lấy file văn bản có đuôi cho phép, bỏ thư mục ẩn, thư viện phụ thuộc, build output, file lock và file lớn hơn 100 KB, chọn tối đa 50 file liên quan nhất (tổng tối đa 1 MB) và kiểm tra hash từng file; Lambda che secret và cắt còn tối đa 240.000 ký tự trước khi gửi tới model; tiêu chí thuyết trình chỉ do giám khảo chấm, AI không chấm; loại bỏ kết quả không khớp schema đầu ra. Visibility timeout của SQS đặt lớn hơn timeout của Lambda; message lỗi chuyển vào DLQ sau số lần thử giới hạn.
- **Phân quyền:** chỉ giám khảo được phân công vào vòng thi và ban tổ chức mới được yêu cầu hoặc xem kết quả AI. AI không bao giờ ghi vào bảng Score chính thức.
- **Giám sát:** log của API và Lambda trên CloudWatch; alarm cho ALB 5xx, target không healthy, CPU và dung lượng trống của RDS, số message trong DLQ và tuổi của message cũ nhất.

#### 4. Triển khai kỹ thuật
**Các giai đoạn triển khai**
1. **Khảo sát và thiết kế:** đánh giá hệ thống hiện tại, so sánh phương án hosting, vẽ kiến trúc đầu tiên và thu thập số liệu chi phí.
2. **Proof of concept:** một EC2 + RDS + S3 + SQS + CloudFront, baseline Flyway, contract AI job và mock worker để kiểm thử luồng hàng đợi từ đầu đến cuối.
3. **Tự động hóa và tích hợp AI:** GitHub Actions với OIDC, ECR, Run Command; Lambda worker gọi DeepSeek API; SES và Parameter Store.
4. **Tính sẵn sàng cao và gia cố (v3):** đưa API vào private subnet sau ALB + Auto Scaling, thêm NAT, WAF, S3 Gateway Endpoint, RDS Multi-AZ cho mùa thi, alarm và Budgets.
5. **Kiểm thử, viết tài liệu và dọn dẹp:** kiểm thử bảo mật âm tính, kiểm thử failover và tải, kiểm thử restore, viết workshop, báo cáo chi phí cuối cùng và dọn tài nguyên.

**Yêu cầu kỹ thuật**
- **Ứng dụng:** React 18/TypeScript/Vite, Java 21/Spring Boot/Spring Security/JPA, MySQL 8.4 với Flyway, worker Python 3 dùng `boto3` và kiểm tra schema đầu vào/đầu ra.
- **AWS:** thiết kế VPC trên hai AZ, ALB và launch template của Auto Scaling, RDS Multi-AZ, CloudFront OAC và custom origin, rule WAF, IAM quyền tối thiểu và trust policy OIDC, SQS + Lambda event source mapping, Parameter Store và KMS.
- **Infrastructure as Code:** template AWS CloudFormation cho toàn bộ hạ tầng: `infra/cloudformation/poc.yaml` (bucket dữ liệu, SQS + DLQ, ECR, log group) và `infra/v3/*.yaml` (base, runtime, edge, ai-worker, ses, cicd, monitoring, audit).
- **CI/CD:** GitHub Actions chạy JUnit, test tích hợp MySQL 8.4 và Vitest, build image một lần, đẩy lên ECR và triển khai đúng digest đó bằng Run Command, sau đó kiểm tra health endpoint; rollback bằng cách triển khai lại digest trước.
- **Kiểm thử:** JUnit/Mockito, Vitest, pytest và k6 để kiểm thử tải cho API nộp bài và chấm điểm.

#### 5. Lộ trình & Mốc triển khai

**Thời gian thực tập:** 14/09/2026–14/12/2026. **Hạn kết thúc dự án:** 29/11/2026.

Các tuần dưới đây là tuần thực tập, tính từ Thứ Hai 14/09/2026. Week 1 của dự án tương ứng Tuần 6 thực tập. Bảng là kế hoạch và tiêu chí bàn giao; việc hoàn thành từng hạng mục được đối chiếu với kết quả triển khai và minh chứng kiểm thử.

| Tuần thực tập | Khoảng thời gian | Nội dung dự án | Đầu ra / mốc mục tiêu |
|---|---|---|---|
| Tuần 1–4 | 14/09–11/10/2026 | Học nền tảng AWS và tham gia các hoạt động đã ghi trong worklog. | Kiến thức và materials làm cơ sở cho dự án. |
| Tuần 5 | 12–18/10/2026 | Khởi động hướng phát triển từ SEAL sang FCAJ Hackathon; rà soát hệ thống kế thừa và phân chia các phần việc ban đầu. | Repository, định hướng và các issue khảo sát ban đầu. |
| Tuần 6 (Week 1 dự án) | 19–25/10/2026 | Khảo sát bài toán, xác định phạm vi, phân công proposal, phác thảo kiến trúc và đầu vào chi phí. | Bản proposal và tiêu chí demo để nhóm rà soát. |
| Tuần 7–8 | 26/10–08/11/2026 | PoC với EC2, RDS, S3, SQS và CloudFront; Flyway baseline, contract AI job và mock worker. | Mục tiêu có một AI job chạy trọn luồng PoC, kèm log và kết quả kiểm thử. |
| Tuần 9–10 | 09–22/11/2026 | CI/CD với OIDC, ECR và Run Command; Lambda AI worker gọi DeepSeek, SES và Parameter Store. | Mục tiêu triển khai theo image digest và tích hợp xử lý AI bất đồng bộ. |
| Tuần 11 | 23–29/11/2026 | Hoàn thiện v3: ALB + Auto Scaling, private subnet, NAT, WAF, cấu hình RDS theo chế độ vận hành, monitoring và Budgets. Rà soát bảo mật, kiểm thử tải/failover/restore; hoàn thiện workshop, demo, chi phí, bàn giao và checklist dọn dẹp. | Chốt phạm vi và bàn giao dự án chậm nhất 29/11/2026; kết quả test/demo phải có minh chứng. |
| Tuần 12–14 | 30/11–14/12/2026 | | |

**Các mốc mục tiêu:** proposal và phạm vi ở Tuần 6; PoC trọn luồng ở Tuần 8; CI/CD và AI ở Tuần 10; hoàn thiện v3, kiểm thử, tài liệu và bàn giao trong Tuần 11, trước hoặc trong ngày 29/11/2026.

#### 6. Ước tính ngân sách

Các số liệu dưới đây là dự toán và giả định từ tài liệu proposal được cung cấp, không phải hóa đơn thực tế. Free Tier, credit và đơn giá cần được đối chiếu theo tài khoản và thời điểm vận hành.

Ước tính cho **ap-southeast-1**, giá on-demand, 730 giờ mỗi tháng, trước credit và thuế. Có hai chế độ vận hành: **giữa các mùa** (1 instance API, 1 NAT, RDS Single-AZ) và **mùa thi** (2 instance API, 2 NAT, RDS Multi-AZ). Bảng dự toán chi tiết được lập trên AWS Pricing Calculator với cùng thông số đầu vào: [giữa các mùa](https://calculator.aws/#/estimate?id=3e1217ee16a09c0ad04b2b440b5b804da413ac03) (130,80 USD/tháng) và [mùa thi](https://calculator.aws/#/estimate?id=f8aacf3bec82418faf417658ccbd51a097d02c64) (230,29 USD/tháng). Hai estimate này chỉ gồm hạ tầng AWS; SQS, Lambda, SES, SNS nằm trong free tier và DeepSeek là chi phí bên ngoài nên được cộng riêng trong bảng dưới. Bảng dưới đây là bảng phân tích sơ bộ dùng làm đầu vào.

#### Chi phí hạ tầng

| Dịch vụ | Giả định | Giữa các mùa (USD/tháng) | Mùa thi (USD/tháng) |
|---|---|---|---|
| EC2 (API) | t3.small Linux, 0,0264 USD/giờ; 1 hoặc 2 instance | 19,27 | 38,54 |
| EBS | 20 GB gp3 mỗi instance | 1,92 | 3,84 |
| Application Load Balancer | 0,0252 USD/giờ + khoảng 0,5 hoặc 1,5 LCU | 21,32 | 27,16 |
| NAT Gateway | 0,059 USD/giờ + 0,059 USD/GB; 1 hoặc 2 gateway | 43,37 | 87,32 |
| Địa chỉ IPv4 public | 0,005 USD/giờ; ALB (2) + NAT (1 hoặc 2) | 10,95 | 14,60 |
| RDS for MySQL | db.t4g.micro, 20 GB gp3; Single-AZ hoặc Multi-AZ | 21,01 | 42,75 |
| AWS WAF | 1 web ACL, 5 rule, 1 triệu hoặc 5 triệu request | 10,60 | 13,00 |
| Route 53 | 1 hosted zone | 0,50 | 0,50 |
| S3 | Frontend, file ứng dụng, kết quả AI, log CloudTrail (vài GB) | 0,30 | 0,30 |
| CloudFront | Nằm trong gói miễn phí vĩnh viễn (1 TB, 10 triệu request) | 0,00 | 0,00 |
| SQS, Lambda, SES, SNS | Phần lớn trong free tier; request long polling | 0,50 | 1,50 |
| KMS | Key do AWS quản lý | 0,00 | 0,00 |
| CloudWatch | Log, metric và alarm | 2,00 | 4,00 |
| ECR | Khoảng 1 GB image | 0,10 | 0,10 |
| DeepSeek API (bên ngoài) | Khoảng 0,014 USD/job (30K token đầu vào + 4K token đầu ra, giá giờ cao điểm); 40 hoặc 600 job | 0,55 | 8,28 |
| **Tổng** | | **132,39** | **241,89** |

- **Kịch bản chi phí vận hành dài hạn 12 tháng (không phải lịch triển khai dự án):** 9 tháng giữa các mùa + 3 tháng thi (mỗi mùa một tháng) = **khoảng 1.917 USD**.
- **Cấu hình tham chiếu cho demo:** 2 instance API (mỗi AZ một), 1 NAT gateway, RDS Single-AZ, khoảng 148 USD/tháng.
- Chứng chỉ public của ACM, S3 Gateway Endpoint, IAM, STS, Run Command và trail quản lý CloudTrail đầu tiên không tính thêm phí. AWS Budgets nằm trong số budget miễn phí.
- Trong thời gian thực tập, nhóm dùng AWS credit trên account demo; chi phí thực tế được đối chiếu với các ngưỡng Budgets hằng tuần.

#### Tối ưu chi phí
- **Công suất theo mùa:** instance API thứ hai, NAT gateway thứ hai và RDS Multi-AZ chỉ chạy trong mùa thi. NAT gateway là khoản lớn nhất nên đây là phần tiết kiệm nhiều nhất.
- **Trả theo lượt dùng:** Lambda, SQS, SES và SNS gần như không tốn phí khi không có job; CloudFront nằm trong gói miễn phí vĩnh viễn.
- **Chọn đúng kích thước:** bắt đầu với t3.small và db.t4g.micro, điều chỉnh theo metric CloudWatch. Instance Graviton (t4g) cho API rẻ hơn khoảng 20% nhưng cần build image arm64.
- **Vòng đời lưu trữ:** S3 Lifecycle xóa kết quả AI cũ và chuyển log CloudTrail sang lớp lưu trữ rẻ hơn; log group CloudWatch có thời hạn lưu cố định; ECR lifecycle policy chỉ giữ các image gần nhất.
- **Giảm lưu lượng qua NAT:** lưu lượng S3 đi qua S3 Gateway Endpoint miễn phí thay vì NAT gateway.
- **Dọn dẹp:** workshop có checklist dọn dẹp (Auto Scaling group, ALB, NAT gateway và Elastic IP, RDS và snapshot, S3 bucket, ECR, Lambda, SQS, log group, alarm, WAF, hosted zone).

#### 7. Đánh giá rủi ro
##### Ma trận rủi ro
| Rủi ro | Ảnh hưởng | Xác suất |
|---|---|---|
| Vượt chi phí (NAT, tài nguyên chạy không, quên tắt tài nguyên mùa thi) | Trung bình | Trung bình |
| Lỗi AZ hoặc instance trong lúc thi | Cao | Thấp |
| Kết quả AI sai, thiên lệch hoặc bị prompt injection từ repository | Cao | Trung bình |
| DeepSeek API gián đoạn, giới hạn tốc độ hoặc đổi giá | Trung bình | Trung bình |
| Lộ secret hoặc IAM role có quyền quá rộng | Cao | Thấp |
| Lỗi migration database khi phát hành | Cao | Thấp |
| Thành viên đang học AWS; trễ tiến độ | Trung bình | Trung bình |

##### Chiến lược giảm thiểu
- **Chi phí:** cảnh báo Budgets ở mức 50%, 80% và 100%; rà soát billing hằng tuần; tạo và xóa tài nguyên mùa thi theo checklist; script dọn dẹp khi kết thúc.
- **Tính sẵn sàng:** health check của ALB kết hợp Auto Scaling thay thế instance, hai AZ và RDS Multi-AZ trong mùa thi; backup RDS tự động và kiểm thử restore theo kế hoạch.
- **Chất lượng AI:** kết quả AI chỉ mang tính tham khảo, có dẫn chứng theo file và dòng, theo rubric và schema có phiên bản; giám khảo giữ quyết định cuối; bộ kiểm thử có repository chứa prompt injection.
- **Nhà cung cấp AI:** retry có backoff và DLQ; job có hạn hết hiệu lực; worker có provider Gemini dự phòng; model ID và giá được cố định trong cấu hình.
- **Bảo mật:** dùng OIDC thay cho access key, mỗi thành phần một role quyền tối thiểu, secret trong Parameter Store với KMS, WAF giới hạn tốc độ, không SSH, CloudTrail để audit.
- **Phát hành:** migration Flyway theo hướng bổ sung, triển khai theo image digest, chặn bằng health check và rollback về digest trước.

##### Kế hoạch dự phòng
- Nếu nhà cung cấp AI lỗi, giám khảo tiếp tục chấm thủ công; job đã gửi chờ trong SQS hoặc vào DLQ và được chạy lại sau.
- Nếu chi phí vượt ngân sách, chuyển về chế độ giữa các mùa (1 instance, 1 NAT, RDS Single-AZ) và tắt các môi trường không thiết yếu.
- Nếu một AZ lỗi ngoài mùa thi, Auto Scaling tạo instance mới ở AZ còn hoạt động và RDS được khôi phục từ bản backup tự động gần nhất.

#### 8. Kết quả kỳ vọng
##### Cải tiến kỹ thuật
- Nền tảng thi vẫn hoạt động khi mất một instance hoặc một AZ trong mùa thi.
- Giám khảo nhận gợi ý AI kèm dẫn chứng trong vài phút mà không làm chậm request web.
- Phát hành tự động, truy vết được và rollback được; không có access key AWS dài hạn và không mở cổng SSH.
- Có log, alarm, audit trail và cảnh báo chi phí ngay từ đầu.

##### Giá trị dài hạn
- Nền tảng tái sử dụng cho mọi mùa SEAL (Spring, Summer, Fall), chỉ mở rộng khi cần.
- Điểm theo từng tiêu chí của từng giám khảo và của AI tạo thành bộ dữ liệu cho nghiên cứu về độ nhất quán giữa các giám khảo.
- Workshop song ngữ có tài liệu đầy đủ và template CloudFormation để các nhóm FCAJ sau tham khảo.
