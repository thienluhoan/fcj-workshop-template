---
title: "Worklog Tuần 6"
date: 2026-10-19
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

Trong tuần 6 thực tập (19–23/10/2026), tương ứng với Week 1 của dự án, tôi tập trung xác định phạm vi proposal và điều phối các nhóm việc với vai trò tech lead. Dự án hướng đến nâng cấp SEAL trên AWS và nghiên cứu tích hợp AI local. Bảng dưới sắp xếp các hoạt động chuẩn bị theo ngày; kiến trúc, dịch vụ và kịch bản demo vẫn là đề xuất cần triển khai và kiểm chứng.

**Địa điểm văn phòng:** Bitexco Financial Tower, số 2 Hải Triều, phường Sài Gòn, TP.HCM.

### Mục tiêu tuần 6

- Xác định bài toán, phần kế thừa, phạm vi PoC và một kịch bản demo xuyên suốt.
- Phân công đầu vào proposal cho frontend, backend, hạ tầng và AI.
- Làm rõ các phụ thuộc về dữ liệu, quyền truy cập, triển khai và luồng AI job.
- Cập nhật việc đổi tên kỹ thuật và tổng hợp yêu cầu cho giai đoạn tiếp theo.

### Nhật ký công việc theo ngày

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tạo các issue cho lead, frontend, Backend 1, Backend 2 và AI; mô tả mục tiêu, đầu ra, phụ thuộc và thời hạn dự kiến cho proposal. Mở PR cập nhật định hướng README. | 19/10/2026 | 19/10/2026 | [Issue #4: Lead](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [#5 Frontend](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/5); [#6 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/6); [#7 Backend 2](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/7); [#8 AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/8); [PR #3](https://github.com/nhatnhm1405/fcaj-hackathon-system/pull/3) |
| 3 | Chuẩn bị hướng proposal: phân biệt ứng dụng SEAL kế thừa với các phần AWS và AI dự kiến; xem xét cách phối hợp các nhóm dữ liệu, frontend và worker. Ôn IAM policy/role để liên hệ quyền AWS với từng workload. | 20/10/2026 | 20/10/2026 | [Issue #4](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [#6 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/6); [#8 AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/8); [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 4 | Lên văn phòng (lịch chuyên cần: Completed). Ghi tổng quan bài toán, mục tiêu, phạm vi và kịch bản demo trong issue lead. Mở và merge PR đổi tên kỹ thuật SEAL → FCAJ vào nhánh `develop`; nội dung bao gồm các định danh trong ứng dụng, cấu hình và tài liệu. | 21/10/2026 | 21/10/2026 | [Issue #4: tổng quan spec và demo](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [PR #10](https://github.com/nhatnhm1405/fcaj-hackathon-system/pull/10); lịch chuyên cần cá nhân (Completed) |
| 5 | Xem xét kiến trúc đề xuất: frontend trên S3/CloudFront, backend trên EC2, dữ liệu và tệp trên RDS/S3, SQS kết nối worker AI local. Nhận diện các điểm cần thống nhất về job/result, phân quyền và khả năng worker offline. | 22/10/2026 | 22/10/2026 | [Issue #4: kịch bản đề xuất](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [Issue #7: deployment và messaging](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/7); [Issue #8: AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/8) |
| 6 | Tổng hợp đầu ra dự kiến và phụ thuộc giữa các nhóm, gồm luồng dữ liệu, dự toán, bảo mật và cách kiểm chứng demo. Ghi nhận các nội dung cần tiếp tục review trước khi triển khai, thay vì xem kiến trúc đề xuất là sản phẩm đã vận hành. | 23/10/2026 | 23/10/2026 | [Issue #4: proposal](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/4); [#5 Frontend](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/5); [#6 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/6); [AWS Well-Architected](https://docs.aws.amazon.com/wellarchitected/latest/framework/the-pillars-of-the-framework.html) |

### Kết quả đạt được

- Ghi nhận bài toán, mục tiêu, phạm vi và hai kịch bản demo đề xuất trong issue của lead.
- Tạo các issue để phân chia đầu vào proposal theo vai trò và xác định các phụ thuộc giữa thành viên.
- Merge PR đổi tên kỹ thuật vào `develop`; việc này chưa đồng nghĩa nội dung đã được merge vào `main`.
- Xác định các hướng cần nghiên cứu cho triển khai AWS và AI local; demo end-to-end và hạ tầng vẫn cần được xây dựng, kiểm thử.

### Bài học rút ra

- Một proposal cần tách rõ những chức năng đã có, phần dự kiến triển khai và tiêu chí chứng minh kết quả.
- Đầu việc của từng thành viên cần có đầu ra và phụ thuộc để các thành phần có thể tích hợp.
- Kiến trúc có AI local cần tính đến trạng thái worker offline, quyền truy cập và cách theo dõi job.
- Phân quyền và chi phí cần được xem xét ngay trong phương án thiết kế.

### Bước tiếp theo

Tiếp tục hoàn thiện proposal, kiến trúc do nhóm tự thiết kế, contract job/result và kế hoạch triển khai. Theo dõi tiến độ bằng issues và kiểm thử từng phần trước khi kết luận luồng tích hợp hoạt động.
