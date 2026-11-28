---
title: "Xử lý sự cố và bài học"
date: 2026-11-27
weight: 12
chapter: false
pre: " <b> 5.12. </b> "
---

Đây là các vấn đề nhóm gặp khi chuyển SEAL từ một máy chủ thử nghiệm sang kiến trúc này, hoặc phát hiện khi rà soát thiết kế trước khi chạy thật, và cách giải quyết từng vấn đề.

#### Sự cố và cách xử lý

| Vấn đề | Nguyên nhân | Cách xử lý |
|---|---|---|
| `fcaj-v3-base` treo ở `RegionalCertificate` | Name server ở nhà đăng ký vẫn là của nhà đăng ký | Thay bằng bốn name server của Route 53, chờ `nslookup -type=NS` hiện đúng |
| `docker login` vào ECR lỗi trên Windows | Token ECR dài hơn giới hạn 2.560 byte của Windows Credential Manager | Đăng nhập bằng thư mục cấu hình Docker tạm, xóa sau khi push (pipeline chạy Linux nên không gặp) |
| Đăng nhập Google, GitHub hỏng, avatar trả 404 | CloudFront chỉ chuyển `/api/*` | Thêm behavior cho `/oauth2/authorization/*`, `/login/oauth2/*` và `/uploads/*` |
| `/login` hoặc `/oauth2/redirect` báo lỗi backend khi F5 | Chuyển cả `/oauth2/*` hoặc `/login/*` về API | Chỉ route đúng các đường dẫn của backend; phần còn lại thuộc React |
| Lỗi API trả về HTML với mã 200 | Custom error page "403/404 → `/index.html`" áp dụng cho mọi origin | Thay bằng CloudFront Function chỉ gắn vào behavior S3 |
| ALB health check lỗi | Spring Boot chỉ nghe HTTP thường ở cổng 8080 | nginx nhận HTTPS ở 8443 và chuyển tiếp tới loopback 8080; cổng backend không bao giờ lộ ra |
| Rủi ro: OAuth lỗi ngẫu nhiên "invalid state" | State OAuth nằm trong bộ nhớ một máy, ALB luân phiên giữa các máy | Bật stickiness `lb_cookie` một giờ trên target group |
| WAF chặn file upload lớn | `SizeRestrictions_BODY` trong common rule set giới hạn body 8 KB | Đổi riêng rule đó sang Count; ứng dụng tự giới hạn upload 20 MB |
| Rủi ro: AI job bị xử lý hai lần | Timeout Lambda (180 giây) dài hơn visibility timeout của hàng đợi (120 giây) | `JobVisibilityTimeout=1080` (6 lần timeout Lambda); worker ghi kết quả một lần (if-none-match) và idempotent |
| Gọi AI lỗi từ tháng 7/2026 | DeepSeek ngừng `deepseek-chat` | Chuyển sang `deepseek-flash` và tắt "thinking" để token suy nghĩ không chiếm giới hạn output |
| Rủi ro: thu thập repository lỗi khi đang chấm | Hai máy dùng chung một IP NAT và giới hạn ẩn danh 60 request/giờ của GitHub | Cấp cho backend một GitHub token chỉ đọc qua `runtime-env` |
| Rủi ro: API không co giãn khi tải tăng trong mùa thi | `MinSize` = `DesiredCapacity` = `MaxSize` = 2, policy CPU 60 % không còn chỗ để thêm máy | Trước mùa thi nâng `MaxSize` (ví dụ 4) và theo dõi CPU, số kết nối RDS |
| Lần release đầu trên database mới bị lỗi | Chưa có user DB của ứng dụng trước khi khôi phục | Đúng như dự kiến; chạy script khôi phục, script tự release ở cuối |
| Biểu đồ Budgets luôn bằng 0 | Chế độ xem mặc định đã trừ credit | Bỏ chọn **Credits** trong cài đặt chi phí của budget |
| Pricing Calculator ra hơn 300 USD | Giá trị mặc định: `db.m1.large` Multi-AZ, RDS Proxy, Savings Plans cho EC2, regional NAT trên 5 AZ | Nhập rõ từng trường; đối chiếu từng dòng với bảng giá |

#### Bài học rút ra

- **Count trước, Block sau.** Mọi rule WAF đều bắt đầu ở chế độ Count. Đọc sampled requests cho thấy cả lớp học dùng chung một IP public, nên giới hạn chung cho auth được nâng lên 300 request/5 phút, còn giới hạn chặt 100 chỉ áp cho endpoint mật khẩu và OTP.
- **Một điểm vào duy nhất.** Đặt API sau cùng domain CloudFront giúp hết lỗi CORS, giữ cookie first-party và chỉ cần một WAF bảo vệ tất cả. Đổi lại phải route đường dẫn cẩn thận.
- **Khóa origin bằng hai lớp.** Security group chỉ nhận prefix list của CloudFront chặn mạng khác; header bí mật chặn các distribution CloudFront khác.
- **Máy chủ không trạng thái là máy chủ rẻ.** Mọi trạng thái nằm ở RDS, S3 và Parameter Store, nên có thể xóa cả stack runtime mỗi tối, và Auto Scaling thay máy bất cứ lúc nào.
- **Phát hành theo digest, không theo tag.** Tag có thể bị dời; digest thì không. Tham số release luôn chỉ đúng cái đang chạy, nên rollback chỉ là đổi một dòng.
- **AI bất đồng bộ.** Hàng đợi giữa API và model giúp màn hình giám khảo luôn nhanh, và lỗi model hiện rõ (DLQ, alarm) thay vì ẩn trong timeout.
