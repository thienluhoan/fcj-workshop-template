---
title: "Amazon DocumentDB: Giải pháp Database tương thích MongoDB trên AWS Cloud"
date: 2026-11-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo và chia sẻ kiến thức. Vui lòng không sao chép nguyên văn cho bài báo cáo của bạn, kể cả nội dung warning này.
{{% /notice %}}

# AMAZON DOCUMENTDB: GIẢI PHÁP DATABASE TƯƠNG THÍCH MONGODB TRÊN AWS CLOUD

Hãy tưởng tượng một ứng dụng thương mại điện tử đang phát triển và mỗi ngày phải xử lý hàng triệu thông tin sản phẩm, đơn hàng, người dùng và lịch sử giao dịch.

Những dữ liệu này thường có cấu trúc linh hoạt, không phải lúc nào cũng phù hợp với mô hình bảng và cột cố định của cơ sở dữ liệu quan hệ (Relational Database).

Nếu doanh nghiệp tự xây dựng và quản lý hệ thống MongoDB trên máy chủ riêng (On-premises), họ có thể phải đối mặt với nhiều vấn đề:

- Phải tự quản lý máy chủ, database và quá trình bảo trì.
- Khó mở rộng tài nguyên khi lượng dữ liệu và số lượng request tăng cao.
- Phải tự xây dựng cơ chế backup và phục hồi dữ liệu.
- Khó đảm bảo tính sẵn sàng khi xảy ra sự cố máy chủ.
- Tốn thời gian quản lý hạ tầng thay vì tập trung phát triển ứng dụng.

Để giải quyết những bài toán này, AWS cung cấp một dịch vụ database có tên **Amazon DocumentDB (with MongoDB compatibility)**.

## 1. AMAZON DOCUMENTDB LÀ GÌ?

Amazon DocumentDB là dịch vụ cơ sở dữ liệu dạng document (Document Database) được AWS quản lý hoàn toàn (Fully Managed Database Service).

Điểm đặc biệt của dịch vụ này là khả năng tương thích với MongoDB.

Điều đó có nghĩa là developer có thể sử dụng mã nguồn ứng dụng tương thích MongoDB, MongoDB Drivers và những công cụ quen thuộc để kết nối và tương tác với Amazon DocumentDB.

Thay vì phải tự quản lý toàn bộ hệ thống database, AWS sẽ đảm nhận nhiều công việc vận hành như:

- Quản lý hạ tầng database.
- Tự động mở rộng dung lượng lưu trữ.
- Sao lưu và phục hồi dữ liệu.
- Giám sát tình trạng hoạt động.
- Hỗ trợ High Availability và Automatic Failover.

Amazon DocumentDB cung cấp hai mô hình triển khai cluster chính:

**1. Instance-based Clusters**

Mô hình cluster sử dụng các database instances để xử lý workload. Bao gồm một Primary Instance phục vụ đọc và ghi dữ liệu cùng các Replica Instances tùy chọn để tăng khả năng xử lý truy vấn đọc và tính sẵn sàng.

**2. Elastic Clusters**

Mô hình cluster phân tán được thiết kế cho các workload quy mô lớn. Dữ liệu và khả năng xử lý được phân phối trên nhiều shard thông qua cơ chế Sharding, phù hợp với các ứng dụng có nhu cầu lưu trữ và xử lý dữ liệu lớn.

Amazon DocumentDB hướng đến những bài toán như:

- Xây dựng ứng dụng sử dụng Document Database.
- Lưu trữ và quản lý dữ liệu có cấu trúc linh hoạt.
- Phát triển ứng dụng cần khả năng mở rộng và tính sẵn sàng cao.
- Chuyển đổi ứng dụng tương thích MongoDB sang môi trường AWS Cloud.

**Lưu ý:** MongoDB compatibility không đồng nghĩa Amazon DocumentDB hỗ trợ toàn bộ tính năng của MongoDB. Developer cần kiểm tra phiên bản driver, engine, câu lệnh và các tính năng được hỗ trợ trước khi thực hiện migration.

## 2. AMAZON DOCUMENTDB HOẠT ĐỘNG NHƯ THẾ NÀO?

Đây là phần đáng chú ý nhất về mặt kiến trúc.

Amazon DocumentDB không chỉ đơn thuần là một database được đặt trên Cloud. Dịch vụ sử dụng kiến trúc tách biệt giữa Compute Layer và Storage Layer để hỗ trợ khả năng mở rộng, tính sẵn sàng và độ bền dữ liệu.

### A. Cluster – Thành phần trung tâm của hệ thống

Trong Amazon DocumentDB, Cluster là thành phần trung tâm quản lý database instances và hệ thống lưu trữ dùng chung.

Một Instance-based Cluster bao gồm:

- **Primary Instance:** Instance chính chịu trách nhiệm xử lý các thao tác ghi dữ liệu và cũng hỗ trợ đọc dữ liệu.
- **Replica Instances:** Các instance bản sao chỉ hỗ trợ đọc dữ liệu, giúp tăng khả năng xử lý truy vấn đọc.
- **Cluster Volume:** Khu vực lưu trữ dùng chung chứa dữ liệu database của toàn bộ cluster.

Một Instance-based Cluster hỗ trợ tổng cộng từ 0 đến 16 instances, bao gồm một Primary Instance và tối đa 15 Replica Instances.

Điểm quan trọng là tất cả các thao tác ghi đều phải được thực hiện thông qua Primary Instance, trong khi cả Primary Instance và Replica Instances đều hỗ trợ đọc dữ liệu.

### B. Compute Layer và Storage Layer – Tách biệt xử lý và lưu trữ

Một đặc điểm quan trọng của Amazon DocumentDB là kiến trúc tách biệt tài nguyên tính toán và lưu trữ.

**Compute Layer**

Cung cấp tài nguyên CPU và RAM thông qua các database instances.

**Storage Layer**

Quản lý dữ liệu database thông qua Cluster Volume.

Dữ liệu trong Cluster Volume được sao chép thành 6 bản trên 3 Availability Zones trong cùng một AWS Region.

Điều này giúp hệ thống tăng khả năng chịu lỗi và giảm nguy cơ mất dữ liệu khi một thành phần hạ tầng gặp sự cố.

Ngoài ra, việc tách biệt hai lớp giúp AWS có thể quản lý và mở rộng tài nguyên tính toán cũng như lưu trữ độc lập với nhau.

### C. Automatic Storage Scaling – Tự động mở rộng dung lượng

Một vấn đề thường gặp khi vận hành database là phải dự đoán trước dung lượng lưu trữ cần thiết.

Nếu cấp phát quá ít, hệ thống có thể thiếu dung lượng khi dữ liệu tăng nhanh.

Nếu cấp phát quá nhiều, doanh nghiệp có thể lãng phí tài nguyên.

Amazon DocumentDB giải quyết vấn đề này bằng cơ chế tự động mở rộng Storage Volume.

Một số đặc điểm quan trọng:

- Storage tự động tăng khi nhu cầu lưu trữ tăng.
- Dung lượng được mở rộng theo từng mức 10 GiB.
- Instance-based Cluster sử dụng Engine Version 8.0 trở lên hỗ trợ tối đa 256 TiB.
- Các Engine Version instance-based trước đó hỗ trợ tối đa 128 TiB.
- Elastic Clusters có kiến trúc lưu trữ riêng và hỗ trợ dung lượng tối đa 4 PiB.
- Không cần cấp phát trước dung lượng dư thừa để dự phòng tăng trưởng dữ liệu.

Nhờ đó, doanh nghiệp có thể tập trung phát triển ứng dụng thay vì liên tục quản lý dung lượng lưu trữ database.

### D. Read Replicas – Tăng khả năng xử lý truy vấn đọc

Khi một ứng dụng có lượng người dùng lớn, số lượng truy vấn đọc database có thể tăng rất nhanh.

Nếu tất cả request đều tập trung vào một instance, hệ thống có thể gặp tình trạng quá tải.

Amazon DocumentDB hỗ trợ tối đa 15 Replica Instances trong một Instance-based Cluster để tăng Read Throughput.

Các replica sử dụng chung hệ thống lưu trữ bên dưới. Vì vậy, chúng không cần duy trì những bản sao database riêng biệt thông qua cơ chế sao chép dữ liệu giữa các instances.

Ngoài ra, dịch vụ cung cấp **Reader Endpoint** để ứng dụng có thể kết nối đọc dữ liệu thông qua một endpoint chung thay vì phải tự theo dõi từng replica khi chúng được thêm hoặc xóa.

Một điểm cần lưu ý là Reader Endpoint cân bằng các kết nối đọc, không phải từng truy vấn database riêng lẻ.

## 3. AMAZON DOCUMENTDB ĐẢM BẢO HIGH AVAILABILITY NHƯ THẾ NÀO?

Đối với những ứng dụng quan trọng, database không chỉ cần lưu trữ dữ liệu mà còn phải duy trì hoạt động ngay cả khi xảy ra sự cố.

Amazon DocumentDB cung cấp một số cơ chế đáng chú ý để tăng tính sẵn sàng.

### A. Multi-AZ – Phân phối dữ liệu trên nhiều Availability Zones

Amazon DocumentDB sao chép 6 bản dữ liệu trên 3 Availability Zones.

Việc phân phối dữ liệu trên nhiều khu vực độc lập giúp tăng khả năng chịu lỗi khi một Availability Zone gặp sự cố.

Kiến trúc lưu trữ này cũng giúp đảm bảo độ bền dữ liệu mà không phụ thuộc vào số lượng database instances trong cluster.

### B. Automatic Failover – Tự động chuyển đổi khi xảy ra lỗi

Hãy tưởng tượng Primary Instance đột ngột gặp sự cố.

Nếu cluster đã có Replica Instances được triển khai tại các Availability Zones khác nhau, Amazon DocumentDB có thể tự động chuyển đổi vai trò để một replica đủ điều kiện trở thành Primary Instance mới.

Nếu chưa có replica và xảy ra sự cố instance, dịch vụ sẽ cố gắng tự động tạo một instance thay thế.

Cơ chế này giúp giảm sự phụ thuộc vào việc can thiệp thủ công khi hệ thống gặp lỗi.

### C. Automated Backup và Point-in-Time Recovery

Amazon DocumentDB cung cấp khả năng backup tự động liên tục kết hợp với cơ chế Incremental Backup.

Một số đặc điểm quan trọng:

- Hỗ trợ Point-in-Time Recovery (PITR).
- Có thể khôi phục cluster về một thời điểm cụ thể trong khoảng thời gian lưu giữ backup, tối đa đến khoảng 5 phút trước thời điểm hiện tại.
- Thời gian lưu giữ Automated Backup có thể cấu hình từ 1 đến 35 ngày.
- Automated Backup được lưu trữ trong hệ thống Amazon S3 do AWS quản lý.
- Quá trình backup được thiết kế để không ảnh hưởng đến hiệu năng của cluster.

Nhờ đó, doanh nghiệp có thể khôi phục dữ liệu khi xảy ra lỗi thao tác, thay đổi ngoài ý muốn hoặc sự cố bất ngờ.

## 4. AMAZON DOCUMENTDB KẾT HỢP VỚI NHỮNG DỊCH VỤ AWS NÀO?

Một điểm quan trọng khi học AWS là không nên nhìn nhận từng dịch vụ một cách độc lập.

Amazon DocumentDB có thể kết hợp với nhiều dịch vụ AWS để xây dựng một hệ thống Cloud hoàn chỉnh.

Một số dịch vụ đáng chú ý:

- **Amazon VPC:** Cung cấp môi trường mạng riêng để triển khai và cô lập database.
- **AWS KMS:** Quản lý encryption keys để mã hóa dữ liệu lưu trữ.
- **Amazon S3:** Lưu trữ các bản backup tự động thông qua hệ thống backup do AWS quản lý.
- **Amazon CloudWatch:** Theo dõi hiệu năng và tình trạng hoạt động của database.
- **AWS CloudTrail:** Ghi lại các hoạt động API để phục vụ kiểm tra và kiểm toán.
- **AWS CloudFormation:** Hỗ trợ triển khai cluster và instance thông qua Infrastructure as Code (IaC).

**Ví dụ một kiến trúc ứng dụng đơn giản:**

1. Người dùng gửi request đến ứng dụng.
2. Backend Application kết nối đến Amazon DocumentDB thông qua MongoDB-compatible Driver.
3. Các thao tác ghi dữ liệu được gửi đến Primary Instance.
4. Các truy vấn đọc có thể được phân phối đến Replica Instances thông qua Reader Endpoint.
5. Dữ liệu được lưu trữ trong Cluster Volume và sao chép trên nhiều Availability Zones.
6. Amazon CloudWatch theo dõi hiệu năng và tình trạng hoạt động.
7. Amazon DocumentDB thực hiện Automated Backup liên tục để hỗ trợ phục hồi dữ liệu.

Đây là cách các thành phần phối hợp để tạo thành một hệ thống database có khả năng mở rộng và chịu lỗi trên AWS Cloud.

## 5. DỮ LIỆU TRONG AMAZON DOCUMENTDB CÓ ĐƯỢC BẢO MẬT KHÔNG?

Database thường chứa những thông tin quan trọng của doanh nghiệp như thông tin khách hàng, sản phẩm, giao dịch và dữ liệu nội bộ.

Vì vậy, bảo mật là yếu tố không thể bỏ qua.

Amazon DocumentDB cung cấp một số cơ chế bảo mật đáng chú ý:

- **Amazon VPC:** Giới hạn môi trường mạng nơi database hoạt động.
- **Network Access Control:** Sử dụng Security Groups và các cấu hình mạng để kiểm soát truy cập đến database cluster.
- **Encryption at Rest:** Mã hóa dữ liệu lưu trữ thông qua AWS KMS.
- **Encrypted Backups:** Automated Backups, Snapshots và các bản sao dữ liệu trong cluster được bảo vệ khi cluster bật encryption.
- **TLS:** Hỗ trợ kết nối bảo mật thông qua Transport Layer Security.

Những cơ chế này giúp doanh nghiệp bảo vệ dữ liệu trong quá trình truyền tải, lưu trữ và vận hành hệ thống database.

## 6. CHI PHÍ SỬ DỤNG AMAZON DOCUMENTDB ĐƯỢC TÍNH NHƯ THẾ NÀO?

Đây là phần rất quan trọng khi triển khai database trên AWS.

Amazon DocumentDB tính phí dựa trên một số thành phần chính:

**1. Instance Hours**

Chi phí sử dụng database instances, phụ thuộc vào instance class và thời gian hoạt động.

**2. I/O Requests**

Chi phí dựa trên số lượng yêu cầu đọc/ghi dữ liệu lên hệ thống lưu trữ, tùy thuộc vào cấu hình storage được lựa chọn.

**3. Backup Storage**

Có thể phát sinh chi phí khi dung lượng backup vượt quá mức lưu trữ backup miễn phí được áp dụng.

**4. Data Transfer**

Có thể phát sinh chi phí truyền dữ liệu ra ngoài AWS hoặc giữa các AWS Regions theo quy định tính phí.

Ngoài ra, Amazon DocumentDB cung cấp nhiều mô hình và cấu hình tính phí khác nhau, bao gồm:

- Provisioned Instances.
- Amazon DocumentDB Serverless.
- Standard và I/O-Optimized Storage Configurations.

AWS hiện cung cấp chương trình dùng thử miễn phí trong một tháng cho người dùng mới đủ điều kiện, với giới hạn sử dụng và phạm vi Region áp dụng cụ thể.

Do đó, khi triển khai thực tế, cần theo dõi tài nguyên đang sử dụng và kiểm tra bảng giá theo Region để tránh phát sinh chi phí ngoài dự kiến.

## 7. AMAZON DOCUMENTDB CÓ THỂ ĐƯỢC QUẢN LÝ BẰNG NHỮNG CÔNG CỤ NÀO?

AWS cung cấp nhiều phương thức để developer và administrator tương tác với Amazon DocumentDB.

### A. AWS Management Console

Giao diện web cho phép người dùng:

- Tạo và quản lý Cluster.
- Tạo và thay đổi Instances.
- Theo dõi trạng thái database.
- Cấu hình các thành phần liên quan.

### B. AWS CLI

Cho phép quản lý Amazon DocumentDB thông qua các câu lệnh trên Terminal.

Điều này đặc biệt hữu ích khi cần tự động hóa các thao tác quản trị hoặc tích hợp vào quy trình triển khai hệ thống.

### C. MongoDB Drivers

Developer có thể sử dụng MongoDB Drivers tương thích để xây dựng ứng dụng và kết nối với Amazon DocumentDB.

Đây là một trong những điểm nổi bật giúp developer đã quen với MongoDB có thể tiếp cận dịch vụ mà không phải thay đổi hoàn toàn công cụ phát triển.

Tuy nhiên, trước khi migration ứng dụng hiện có, developer cần kiểm tra các phiên bản driver và tính năng MongoDB được Amazon DocumentDB hỗ trợ.

## 8. NHỮNG ĐIỀU CẦN GHI NHỚ

Amazon DocumentDB không chỉ là một dịch vụ database trên Cloud.

Điểm mạnh của dịch vụ nằm ở khả năng:

- Cung cấp Document Database tương thích với MongoDB.
- Quản lý hạ tầng database theo mô hình Fully Managed.
- Tách biệt Compute Layer và Storage Layer.
- Tự động mở rộng dung lượng lưu trữ.
- Tăng khả năng xử lý truy vấn đọc bằng Replica Instances.
- Hỗ trợ Multi-AZ và Automatic Failover.
- Backup tự động và Point-in-Time Recovery.
- Tích hợp với hệ sinh thái AWS để xây dựng kiến trúc Cloud hoàn chỉnh.

Một bài học quan trọng khi học Cloud Engineering và Backend Development:

Việc lựa chọn database không chỉ phụ thuộc vào cách dữ liệu được lưu trữ mà còn phải xem xét khả năng mở rộng, hiệu năng, tính sẵn sàng, bảo mật, khả năng tương thích và chi phí vận hành.

Amazon DocumentDB là một ví dụ điển hình cho cách AWS cung cấp giải pháp Database-as-a-Service, giúp developer tập trung nhiều hơn vào việc phát triển ứng dụng thay vì phải tự quản lý toàn bộ hạ tầng database.

**Góc nhìn cá nhân:** Database không đơn thuần là nơi lưu trữ dữ liệu cho ứng dụng. Đây là một thành phần quan trọng trong toàn bộ kiến trúc hệ thống. Hiểu cách database mở rộng, xử lý sự cố, quản lý dữ liệu và kiểm soát quyền truy cập là điều cần thiết khi xây dựng các ứng dụng Cloud có khả năng vận hành trong môi trường thực tế.

## 9. TÀI LIỆU THAM KHẢO – AWS OFFICIAL DOCUMENTATION

- [What is Amazon DocumentDB (with MongoDB compatibility)?](https://docs.aws.amazon.com/documentdb/latest/devguide/what-is.html)
- [How Amazon DocumentDB Works](https://docs.aws.amazon.com/documentdb/latest/devguide/how-it-works.html)
- [Understanding Amazon DocumentDB Clusters](https://docs.aws.amazon.com/documentdb/latest/devguide/db-clusters-understanding.html)
- [Amazon DocumentDB High Availability and Replication](https://docs.aws.amazon.com/documentdb/latest/devguide/replication.html)
- [Backing Up and Restoring in Amazon DocumentDB](https://docs.aws.amazon.com/documentdb/latest/devguide/backup_restore.html)
- [Amazon DocumentDB Pricing](https://aws.amazon.com/documentdb/pricing/)
- [Amazon DocumentDB FAQs](https://aws.amazon.com/documentdb/faqs/)
<img width="929" height="540" alt="image" src="https://github.com/user-attachments/assets/28cfcdfd-8c7b-4fd6-9bec-4aa0491f0589" />

#AWS #AmazonDocumentDB #MongoDB #AWSCloud #CloudComputing #Database #BackendDevelopment #CloudEngineering #NoSQL #DatabaseArchitecture

Link: https://www.facebook.com/nguyen.hoang.khang.751420/
