---
title: "Blog 1 - Load Balancing và điểm nghẽn database"
date: 2026-11-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

### Thông tin bài đăng

- **Tác giả:** Nguyễn Huỳnh Minh Nhật (tên hiển thị Facebook: Nhật Nguyễn).
- **Ngày đăng:** 08/11/2026, lúc 22:18 theo ảnh bài đăng.
- **Nơi đăng:** AWS Study Group VN trên Facebook.
- **Bài gốc:** [Xem bài đăng trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2325121871586105).

### Amazon Load Balancing sinh ra để chia tải, nhưng nếu website vẫn chậm thì chuyện gì có thể đang diễn ra?

**Bài toán:** Một website đăng ký sự kiện chạy trên hai EC2 phía sau Application Load Balancer (ALB), cùng truy vấn một Amazon RDS database. Khi mở đăng ký, lượng truy cập tăng vọt và trang cứ loading dù các target vẫn healthy. Phản xạ thường gặp là thêm EC2 để chia request cho nhiều máy hơn. Tuy nhiên, cách này hiệu quả khi tầng ứng dụng thực sự thiếu khả năng xử lý.

{{< event-photo src="images/blogs/load-balancing/architecture.png" alt="Người dùng gửi request qua ALB tới hai EC2 cùng truy vấn một Amazon RDS database." caption="Sơ đồ tình huống minh họa trong bài: ALB, hai EC2 và một database RDS dùng chung." >}}

#### Load balancing giúp gì?

ALB phân phối request tới các target. Khi health check đánh dấu một target unhealthy và còn target khỏe, request mới được chuyển tới các target khỏe. Kết hợp Auto Scaling, hệ thống có thể bổ sung EC2 khi tải tăng. Điều này hữu ích khi CPU hoặc bộ nhớ của tầng ứng dụng quá tải và công việc có thể phân chia giữa nhiều máy.

#### Khi database mới là điểm nghẽn

Nếu cả hai EC2 đang chờ RDS trả kết quả, thêm máy có thể chỉ tạo thêm các máy cùng đứng chờ. Mỗi EC2 mới còn có connection pool riêng. Ví dụ minh họa: mỗi máy giữ 20 kết nối, tăng từ 2 lên 6 EC2 làm tổng kết nối tăng từ 40 lên 120. Chi phí tăng trong khi áp lực lên database có thể lớn hơn.

#### Healthy chưa phản ánh toàn bộ trải nghiệm

Health check thường chỉ cho thấy ứng dụng còn phản hồi, chưa chứng minh người dùng đăng ký thành công. Bài viết cũng lưu ý việc đưa truy vấn database vào `/health`: khi Auto Scaling sử dụng ELB health checks, database chậm có thể khiến nhiều EC2 bị thay thế; máy mới tiếp tục mở kết nối và làm tình trạng quá tải nặng thêm.

Hướng xử lý được thảo luận là giữ health check nhẹ, đồng thời theo dõi trải nghiệm qua các metric như `TargetResponseTime` và `HTTPCode_Target_5XX_Count`.

#### Kiểm tra trước khi tăng máy

- Nếu CPU EC2 cao, RDS phản hồi nhanh và số kết nối còn thấp, có thể tầng ứng dụng đang thiếu sức xử lý; bổ sung EC2 là hướng cần xem xét.
- Nếu EC2 ít bận nhưng phản hồi chậm, cần kiểm tra thời gian chờ database, `DatabaseConnections`, `ReadLatency` và `WriteLatency`; xem lại query, index và connection pool.
- RDS Proxy được đề cập như một cách quản lý, tái sử dụng kết nối tới database; query chậm vẫn cần tối ưu riêng.

Mỗi lần chỉ nên thay đổi một yếu tố rồi chạy lại cùng kịch bản tải để so sánh. Thay đổi nhiều yếu tố cùng lúc làm khó xác định nguyên nhân cải thiện.

ALB chia request cho EC2, nhưng nếu nghẽn ở database thì thêm EC2 chưa chắc làm website nhanh hơn. **Bạn sẽ kiểm tra gì đầu tiên trong tình huống này?**

Đây là tình huống giả định để phân tích kỹ thuật; các con số kết nối là ví dụ minh họa.

### Minh chứng bài đã đăng

{{< event-photo src="images/blogs/load-balancing/facebook-publication.png" alt="Ảnh bài đăng của Nhật Nguyễn trong AWS Study Group VN, ngày 8 tháng 11 lúc 22:18." caption="Ảnh xác nhận bài Blog 1 đã đăng trên AWS Study Group VN do tác giả cung cấp." >}}

### Tài liệu tham khảo

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
- [Health checks for Application Load Balancer target groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html)
- [CloudWatch metrics for Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html)
- [Amazon RDS Proxy](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)
