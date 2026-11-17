---
title: "Worklog Tuần 7"
date: 2026-10-26
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

Tuần 7 thực tập (26–30/10/2026) tương ứng với **Week 2 của dự án** FCAJ Hackathon System. Tôi tiếp tục vai trò tech lead, chuẩn bị chuyển từ khảo sát và đề xuất sang giai đoạn triển khai. Các mục tiêu dưới đây dựa trên các issue của dự án; kết quả thực tế sẽ được cập nhật khi có minh chứng.

**Địa điểm văn phòng:** Bitexco Financial Tower, số 2 Hải Triều, phường Sài Gòn, TP.HCM.

### Mục tiêu tuần 7

- Hoàn thiện proposal, kế hoạch kiến trúc và phân công các phần việc triển khai.
- Điều phối các phụ thuộc giữa frontend, backend, hạ tầng AWS và AI local.
- Hướng đến một luồng tích hợp tối thiểu có thể kiểm thử, bắt đầu bằng worker mock.
- Theo dõi quyền truy cập, ngân sách, blocker và minh chứng bàn giao.

### Nhật ký công việc theo ngày

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tạo issue lead và bốn issue cho frontend, Backend 1, Backend 2 và AI nhằm phân công giai đoạn triển khai. Trong issue lead, ghi nhận đã soạn proposal và kế hoạch vẽ kiến trúc; việc đưa tài liệu lên nơi làm việc chung vẫn đang chờ. | 26/10/2026 | 26/10/2026 | [Issue #11: Lead](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/11); [#12 Frontend](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/12); [#13 Backend 1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/13); [#14 Backend 2](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/14); [#15 AI](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/15) |
| 3 | Lên văn phòng (lịch chuyên cần: Completed). | 27/10/2026 | 27/10/2026 | Lịch chuyên cần cá nhân (Completed) |
| 4 | Lên văn phòng (lịch chuyên cần: Completed). | 28/10/2026 | 28/10/2026 | Lịch chuyên cần cá nhân (Completed) |
| 5 | [CẦN CẬP NHẬT: ghi nhận công việc thực tế sau ngày 29/10.] | 29/10/2026 | Chưa cập nhật | [Issue #11](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/11) |
| 6 | [CẦN CẬP NHẬT: ghi nhận công việc thực tế sau ngày 30/10.] | 30/10/2026 | Chưa cập nhật | [Issue #11](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/11) |

### Kết quả đã ghi nhận

- Có issue lead và bốn issue triển khai với mục tiêu, người phụ trách và tiêu chí bàn giao.
- Issue lead ghi nhận proposal và kế hoạch vẽ kiến trúc đã được soạn; tài liệu chưa được xác nhận đã chia sẻ cho cả nhóm.

### Công việc dự kiến

- Chia sẻ tài liệu, phối hợp hoàn thiện sơ đồ runtime và thống nhất contract job/result.
- Phối hợp nhóm triển khai hạ tầng và pipeline, kết nối frontend và API job.
- Kiểm thử luồng frontend → backend → RDS/S3 → SQS → worker mock → kết quả; phần tích hợp và demo chưa được xác nhận hoàn thành.
- Theo dõi benchmark AI local, quyền tài khoản, ngân sách và blocker.

Nguồn: [Issue #11](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/11), [#12](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/12), [#13](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/13), [#14](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/14), [#15](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/15).
