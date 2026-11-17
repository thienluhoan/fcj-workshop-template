---
title: "Worklog Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

Trong tuần thứ tư (05/10/2026–09/10/2026), tôi thực hành các bài lab AWS Backup, S3 File Gateway và S3/CloudFront, đồng thời học lý thuyết lưu trữ trong Module 04. Tôi chuẩn bị môi trường backup vào Thứ Hai và hoàn thiện luồng thông báo, kiểm tra khôi phục vào Thứ Sáu.

**Địa điểm văn phòng:** Bitexco Financial Tower, số 2 Hải Triều, phường Sài Gòn, TP.HCM.

### Mục tiêu tuần 4

- Cấu hình chính sách AWS Backup và phân biệt backup plan, vault, resource assignment, recovery point.
- Hiểu S3 File Gateway, SMB file share và vai trò của bộ nhớ đệm cục bộ.
- Triển khai website tĩnh trên S3 và phân phối nội dung qua CloudFront với quyền truy cập origin phù hợp.
- Thực hành S3 Versioning, di chuyển object và Cross-Region Replication (CRR).
- So sánh loại lưu trữ, storage classes, lifecycle, CORS và các phương án khôi phục thảm họa.
- Hoàn thiện thông báo, kiểm tra restore và dọn tài nguyên lab.

### Các công việc đã hoàn thành trong tuần

| Thứ | Công việc đã hoàn thành | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Lên văn phòng (lịch chuyên cần: Completed). Chuẩn bị S3 chứa CloudFormation template và Lambda package, triển khai hạ tầng lab backup, cấu hình backup plan, rule, vault và resource assignment. Tìm hiểu luồng backup/restore và tiêu chí kiểm tra ứng dụng cho phần kiểm thử cuối tuần. | 05/10/2026 | 05/10/2026 | FCJ Lab13: [Giới thiệu](https://000013.awsstudygroup.com/1-introduce/), [Chuẩn bị S3](https://000013.awsstudygroup.com/2-prerequiste/2.1-creates3bucket/), [Triển khai hạ tầng](https://000013.awsstudygroup.com/2-prerequiste/2.2-deployinfras/), [Backup plan](https://000013.awsstudygroup.com/3-createbackupplan/); lịch chuyên cần cá nhân do người dùng cung cấp ngày 17/11/2026 (Completed) |
| 3 | Triển khai S3 File Gateway trên EC2, tạo SMB file share, kết nối từ Windows và kiểm tra tệp ghi qua share xuất hiện trên S3; dọn tài nguyên Lab24. Sau đó tạo bucket riêng, tải mã nguồn website, cấu hình static website hosting cùng quyền truy cập public theo lab và kiểm tra website endpoint. | 06/10/2026 | 06/10/2026 | [FCJ Lab24 - Storage Gateway](https://000024.awsstudygroup.com/), [Kết nối file share](https://000024.awsstudygroup.com/2-useawsstoragegw/2.3-mountfileshare/), [Cleanup](https://000024.awsstudygroup.com/3-cleanup/); FCJ Lab57: [Tạo bucket](https://000057.awsstudygroup.com/2-prerequiste/2.1-creates3bucket/), [Tải dữ liệu](https://000057.awsstudygroup.com/2-prerequiste/2.2-uploaddata/), [Static website](https://000057.awsstudygroup.com/3-staticwebsite/), [Public access](https://000057.awsstudygroup.com/4-blockpublicaccess/), [Quyền object](https://000057.awsstudygroup.com/5-publicobject/), [Kiểm tra website](https://000057.awsstudygroup.com/6-testwebsite/) |
| 4 | Tiếp tục Lab57: bật lại S3 Block Public Access, cấu hình CloudFront với S3 origin và bucket policy. Thực hành versioning, khôi phục phiên bản, di chuyển object giữa các bucket và CRR; kiểm tra nội dung phân phối, replication rồi dọn tài nguyên lab. | 07/10/2026 | 07/10/2026 | FCJ Lab57: [Chặn public access](https://000057.awsstudygroup.com/7-cloudfront/7.1-block_all_public_access/), [Cấu hình CloudFront](https://000057.awsstudygroup.com/7-cloudfront/7.2-configure_amazon_cloudfront/), [Kiểm tra CloudFront](https://000057.awsstudygroup.com/7-cloudfront/7.3-test_cloudfront/), [Versioning](https://000057.awsstudygroup.com/8-versioning/), [Di chuyển object](https://000057.awsstudygroup.com/9-moveobject/), [Replication](https://000057.awsstudygroup.com/10-s3ccr/), [Cleanup](https://000057.awsstudygroup.com/11-cleanup/) |
| 5 | Học và so sánh block, file, object storage; S3 Access Points, storage classes, lifecycle, CORS và quyền truy cập. Tổng hợp kiến thức về truy xuất dữ liệu lưu trữ lạnh, mô hình hybrid, chuyển dữ liệu ngoại tuyến và các chiến lược DR theo mục tiêu thời gian khôi phục (RTO) và mục tiêu điểm khôi phục dữ liệu (RPO). | 08/10/2026 | 08/10/2026 | Module 04: [Tổng quan storage](https://www.youtube.com/watch?v=hsCfP0IxoaM), [S3, Access Points và storage classes](https://www.youtube.com/watch?v=_yunukwcAwc), [Static website, CORS và archive](https://www.youtube.com/watch?v=mPBjB6Ltl_Q), [Chuyển dữ liệu, Storage Gateway và Backup](https://www.youtube.com/watch?v=YXn8Q_Hpsu4) |
| 6 | Bổ sung Backup Vault Notifications qua AWS CLI và SNS cho môi trường đã chuẩn bị đầu tuần. Tạo on-demand backup, kiểm tra luồng Lambda khôi phục và kiểm thử ứng dụng thông qua jobs, thông báo, CloudWatch Logs; hoàn tất cleanup tài nguyên. | 09/10/2026 | 09/10/2026 | FCJ Lab13: [Thông báo](https://000013.awsstudygroup.com/4-enablenoti/), [Kiểm tra restore](https://000013.awsstudygroup.com/5-testrestore/), [Cleanup](https://000013.awsstudygroup.com/6-cleanup/) |

### Kết quả đạt được tuần 4

- Hoàn thành nội dung học từ 4 video Module 04 và 26 trang FCJ, được tổng hợp thành 5 nhóm hoạt động theo ngày.
- Thiết lập môi trường AWS Backup cùng plan, vault và resource assignment; hoàn thiện luồng thông báo, kiểm tra khôi phục vào cuối tuần.
- Cấu hình SMB file share qua S3 File Gateway, kiểm tra đường ghi dữ liệu từ client đến S3 và dọn tài nguyên lab gateway.
- Triển khai S3 static website, sau đó cấu hình phân phối qua CloudFront và quyền truy cập origin; phân biệt được hai đường truy cập.
- Thực hành quản lý version, di chuyển object và CRR; hiểu mục đích và phạm vi của từng cơ chế.
- Tổng hợp tiêu chí lựa chọn storage và khôi phục dữ liệu, gồm access pattern, retention, archive retrieval và sự khác biệt giữa CORS với quyền truy cập.

### Bài học rút ra sau khi học

- Tôi hiểu backup job hoàn tất và ứng dụng khôi phục thành công là hai kết quả khác nhau. Tiêu chí kiểm tra restore cần gắn với hoạt động và dữ liệu của ứng dụng; trạng thái job không đủ để kết luận đã đáp ứng RTO/RPO.
- Tôi phân biệt được truy cập tệp qua SMB với truy cập object trên S3. Cache của gateway hỗ trợ truy cập tệp, còn thay đổi trực tiếp trên S3 cần được xem xét cùng cơ chế refresh cache.
- Tôi hiểu phần website dùng ACL trong lab và bucket policy cho CloudFront phục vụ các đường truy cập khác nhau. Loại origin và quyền truy cập phải phù hợp với cách phân phối nội dung mong muốn.
- Tôi rút ra cần kiểm tra version trên S3 và cache của CDN riêng biệt. Khôi phục phiên bản object không tự bảo đảm mọi client nhận ngay phiên bản đó qua CloudFront.
- Tôi hiểu CRR sao chép bất đồng bộ các object đủ điều kiện theo cấu hình. Cơ chế này không thay thế toàn bộ chiến lược backup và recovery của ứng dụng.
- Tôi rút ra cần cân đối access pattern, thời gian truy xuất, retention và RTO/RPO khi chọn phương án lưu trữ, khôi phục. Tôi cũng phân biệt cấu hình CORS của trình duyệt với quyền cho phép truy cập object trên S3.
- Tôi hiểu cần đối chiếu trạng thái jobs, thông báo và log khi kiểm tra restore. Cleanup cũng cần phân biệt tài nguyên thuộc stack với backup resources, dữ liệu và tài nguyên kiểm thử được tạo riêng.

### Bước tiếp theo

Ở Tuần 5, tôi tiếp tục các module tiếp theo trong khóa học và bắt đầu giai đoạn project.
