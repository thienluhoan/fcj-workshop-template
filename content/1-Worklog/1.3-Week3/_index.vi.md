---
title: "Worklog Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

Trong tuần thứ ba (28/09/2026–02/10/2026), tôi học về kết nối VPC và EC2, đồng thời tham dự watch party AWS Cloud and AI Day Hanoi tại TP.HCM vào ngày 29/09. Tôi sắp xếp các chủ đề Transit Gateway và compute quanh ngày sự kiện; nội dung AWS Backup được chuyển sang đầu Tuần 4.

Tôi cũng lên văn phòng tham dự Fireside Chat with Dr. Werner ngày 02/10/2026.

### Mục tiêu tuần 3

- Cấu hình VPC Peering và hiểu các điều kiện về định tuyến, bảo mật, DNS để kết nối hai VPC.
- Tìm hiểu Transit Gateway attachments, association, propagation và mối liên hệ giữa VPC route tables với TGW route tables.
- Nắm cách lựa chọn EC2 instance type, AMI, key pair, snapshot, EBS và instance store.
- Tìm hiểu User Data, instance metadata, EC2 Auto Scaling và vai trò của EFS, FSx, Lightsail, AWS MGN.
- Tham dự watch party AWS Cloud and AI Day Hanoi và liên hệ chủ đề keynote với kỹ thuật Cloud và kiểm chứng phần mềm.

### Các công việc đã hoàn thành trong tuần

| Thứ | Công việc đã hoàn thành | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Hoàn thành Lab19: chuẩn bị môi trường hai VPC, cấu hình Security Group và Network ACL, tạo VPC Peering, thêm route hai phía, bật Cross-Peer DNS, kiểm tra kết nối qua private IP và dọn tài nguyên lab. | 28/09/2026 | 28/09/2026 | [Playlist Module 02, Lab19 - 9 video](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab19](https://000019.awsstudygroup.com/); [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-basics.html), [peering DNS](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-dns.html) |
| 3 | Lên văn phòng tại Bitexco; buổi tham dự này đồng thời được ghi nhận cho chuyên cần văn phòng và Event 2. Tham dự AWS Cloud and AI Day Hanoi qua watch party tại TP.HCM và theo dõi keynote được phát từ Hà Nội. Tôi ghi nhận các chủ đề về ứng dụng Cloud và AI trong doanh nghiệp, invisible engineering, và kiểm chứng phần mềm có AI hỗ trợ bằng quyền truy cập phù hợp, bằng chứng và trách nhiệm của con người. | 29/09/2026 | 29/09/2026 | [Event 2: AWS Cloud and AI Day Hanoi - HCMC Watch Party](https://aws.amazon.com/vi/events/cloud-day-hanoi/); xác nhận chuyên cần của cá nhân ngày 17/11/2026 |
| 4 | Hoàn thành Lab20: kết nối bốn VPC qua Transit Gateway, cấu hình attachments, association/propagation và route tables, kiểm tra kết nối EC2, dùng Reachability Analyzer để xem khả năng kết nối theo cấu hình mạng, rồi dọn tài nguyên lab. | 30/09/2026 | 30/09/2026 | [Playlist Module 02, Lab20 - 7 video](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab20](https://000020.awsstudygroup.com/); [How Transit Gateway works](https://docs.aws.amazon.com/vpc/latest/tgw/how-transit-gateways-work.html), [Reachability Analyzer](https://docs.aws.amazon.com/vpc/latest/reachability/how-reachability-analyzer-works.html) |
| 5 | Học về compute VM, tiêu chí chọn EC2 instance type, AMI, key pair và backup; so sánh Amazon EBS với instance store về cách gắn kết, vòng đời dữ liệu và tình huống sử dụng. | 01/10/2026 | 01/10/2026 | [Compute VM](https://www.youtube.com/watch?v=-t5h4N6vfBs), [AMI / Backup / Key Pair](https://www.youtube.com/watch?v=yAR6QRT3N1k), [EBS](https://www.youtube.com/watch?v=hKr_TfGP7NY), [Instance store](https://www.youtube.com/watch?v=6IHNDJ85aoQ); [EC2 instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html), [EBS](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html), [Instance store persistence](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-store-lifetime.html) |
| 6 | Lên văn phòng tham dự Fireside Chat with Dr. Werner (08:30–12:00); xem phần chi tiết bên dưới. Tìm hiểu User Data và instance metadata, cơ chế EC2 Auto Scaling, cùng vai trò của EFS, FSx, Lightsail và AWS Application Migration Service trong khởi tạo instance, co giãn, lưu trữ tệp và di chuyển ứng dụng. | 02/10/2026 | 02/10/2026 | [User Data](https://www.youtube.com/watch?v=_v_43Wi7zjo), [Metadata](https://www.youtube.com/watch?v=Ew3QRaKJQSA), [EC2 Auto Scaling](https://www.youtube.com/watch?v=bbLcPitXJSY), [Compute services overview](https://www.youtube.com/watch?v=hFVYG8WqfU0); [EC2 User Data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html), [EC2 instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html); [Fireside Chat with Dr. Werner](https://luma.com/j4y481jz) |

### Buổi văn phòng: Fireside Chat with Dr. Werner

- **Ngày và giờ chương trình:** Thứ Sáu, 02/10/2026, 08:30–12:00 (UTC+7).
- **Địa điểm:** Bitexco Financial Tower, số 2 Hải Triều, phường Sài Gòn, TP.HCM.
- **Diễn giả:** Dr. Werner Vogels, được giới thiệu là CTO của Amazon.
- **Người tổ chức:** Huỳnh Hoàng Long.

Tôi đã tham dự và ghi nhận buổi này vào chuyên cần văn phòng.

Theo [chương trình công bố](https://luma.com/j4y481jz), các chủ đề gồm Cloud và AI, vận hành hệ thống quy mô toàn cầu, đổi mới và lãnh đạo công nghệ. Chương trình còn liệt kê hỏi đáp và giao lưu.

### Kết quả đạt được tuần 3

- Hoàn thành 24 video trong khóa học và dùng tài liệu AWS EC2 instance types để bổ sung cho một video không có transcript; tham dự watch party ngày 29/09.
- Cấu hình VPC Peering trong lab và xác định các điều kiện kết nối giữa hai VPC, gồm CIDR không chồng lấn, route hai phía và quy tắc bảo mật phù hợp.
- Xây dựng mô hình nhiều VPC qua Transit Gateway; phân biệt route trong VPC với route trong TGW, cũng như vai trò của attachment, association và propagation.
- Tổng hợp tiêu chí lựa chọn EC2 instance type và so sánh tính bền dữ liệu của EBS với instance store theo vòng đời instance.
- Phân biệt User Data với metadata, đồng thời tìm hiểu vai trò của Auto Scaling, EFS, FSx, Lightsail và MGN.
- Liên hệ các chủ đề keynote tại watch party với kỹ thuật Cloud thực tế, quản trị quyền truy cập, kiểm chứng phần mềm bằng bằng chứng và trách nhiệm của con người.

### Bài học rút ra sau khi học

- Tôi rút ra rằng VPC Peering ở trạng thái active chưa đủ để đảm bảo ứng dụng kết nối được. Tôi cần kiểm tra CIDR, route hai phía, Security Group, Network ACL và phân giải tên miền theo đúng luồng lưu lượng.
- Tôi phân biệt được VPC route table với TGW route table. Association chọn bảng TGW xử lý traffic đến từ attachment, còn propagation đưa route của attachment vào bảng được chọn.
- Reachability Analyzer giúp tôi đánh giá khả năng kết nối dựa trên cấu hình mạng; kết quả này không thay thế việc kiểm tra kết nối của ứng dụng đang chạy.
- Tôi hiểu lựa chọn EBS hay instance store phụ thuộc vào hiệu năng và yêu cầu lưu giữ dữ liệu. Instance store phù hợp với dữ liệu tạm; dữ liệu cần tồn tại lâu dài cần được đặt trên loại lưu trữ bền vững và cấu hình vòng đời phù hợp.
- Tôi hiểu AMI và User Data hỗ trợ khởi tạo instance nhất quán, còn metadata cung cấp thông tin instance cho quy trình vận hành. Các khái niệm này giúp tôi hiểu rõ hơn thiết kế Auto Scaling.
- Từ watch party, tôi rút ra rằng phát triển có AI hỗ trợ cần đi cùng quyền truy cập phù hợp, bằng chứng kiểm thử và sự đánh giá có trách nhiệm của con người. Tốc độ phát triển vẫn cần nền tảng kỹ thuật đáng tin cậy và quy trình kiểm chứng.

### Bước tiếp theo

Ở Tuần 4, tôi bắt đầu với bài lab AWS Backup và restore vào ngày 05/10, sau đó tiếp tục với S3, CloudFront và các chủ đề lưu trữ tiếp theo trong lộ trình học.
