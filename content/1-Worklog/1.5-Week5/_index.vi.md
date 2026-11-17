---
title: "Worklog Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

Trong tuần 5 (12–16/10/2026), tôi bắt đầu chuẩn bị dự án FCAJ Hackathon System với vai trò tech lead. Nhóm định hướng phát triển từ hệ thống SEAL sẵn có, đồng thời tôi ôn lại các nội dung về trách nhiệm bảo mật, IAM và chi phí AWS. Các hoạt động chuẩn bị được sắp xếp theo ngày để trình bày; những mốc có ghi nhận cụ thể được dẫn nguồn trong bảng.

**Địa điểm văn phòng:** Bitexco Financial Tower, số 2 Hải Triều, phường Sài Gòn, TP.HCM.

### Mục tiêu tuần 5

- Xác định hướng phát triển dự án FCAJ Hackathon System từ hệ thống SEAL.
- Xem xét phạm vi kế thừa, nhu cầu sử dụng AWS và phân chia đầu việc ban đầu.
- Tìm hiểu trách nhiệm bảo mật, IAM policy và role, cùng cách xem xét chi phí cloud.

### Nhật ký công việc theo ngày

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu bài toán quản lý hackathon và định hướng sử dụng hệ thống hiện có làm nền tảng cho dự án. Ôn Shared Responsibility Model để phân biệt trách nhiệm bảo mật của AWS và của nhóm khi sử dụng cloud. | 12/10/2026 | 12/10/2026 | [Repo dự án](https://github.com/nhatnhm1405/fcaj-hackathon-system); [Shared Responsibility Model](https://www.youtube.com/watch?v=tsobAlSg19g) |
| 3 | Khởi tạo repository từ dự án SEAL và cập nhật README theo định hướng FCAJ Hackathon. Xem xét các thành phần ứng dụng hiện có làm cơ sở cho kế hoạch phát triển. | 13/10/2026 | 13/10/2026 | [Commit khởi tạo](https://github.com/nhatnhm1405/fcaj-hackathon-system/commit/3547942); [Cập nhật định hướng README](https://github.com/nhatnhm1405/fcaj-hackathon-system/commit/f42a168) |
| 4 | Lên văn phòng (lịch chuyên cần: Completed). Tạo issue về đổi tên kỹ thuật từ SEAL sang FCAJ và issue khảo sát ý tưởng kiến trúc AWS; thực hiện commit đổi tên các định danh kỹ thuật. Ôn IAM policy và role để liên hệ với quyền truy cập tài nguyên dự kiến. | 14/10/2026 | 14/10/2026 | [Issue #1](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/1); [Issue #2](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/2); [Commit đổi tên](https://github.com/nhatnhm1405/fcaj-hackathon-system/commit/3c0de1e3ee9f053c43bf58a355164f0e1ad039c2); [IAM](https://www.youtube.com/watch?v=N_vlJGAqZxo), [IAM Policy](https://www.youtube.com/watch?v=XXjEpIRWJBk), [IAM Role](https://www.youtube.com/watch?v=pJWANviu8QM); lịch chuyên cần cá nhân (Completed) |
| 5 | Xem xét hướng sử dụng EC2, VPC và S3 cho hệ thống, đồng thời phân biệt quyền IAM của workload với quyền của người dùng trong ứng dụng. Tìm hiểu cách IAM role hỗ trợ workload truy cập tài nguyên. | 15/10/2026 | 15/10/2026 | [Issue #2: kiến trúc AWS](https://github.com/nhatnhm1405/fcaj-hackathon-system/issues/2); [Create IAM role](https://www.youtube.com/watch?v=j8Gj2DJtZUE), [Using IAM role](https://www.youtube.com/watch?v=Z0liKVSjt_Q); [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| 6 | Tổng hợp các nhóm việc cần chuẩn bị cho giai đoạn proposal, gồm frontend, backend, hạ tầng và AI. Ôn cost optimization, nhận diện các giả định về Region, tải và thời gian chạy cần có khi dự toán. | 16/10/2026 | 16/10/2026 | [Repo dự án](https://github.com/nhatnhm1405/fcaj-hackathon-system); [AWS Cost Optimization and Support](https://www.youtube.com/watch?v=IY61YlmXQe8); [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |

### Kết quả đạt được

- Có repository FCAJ Hackathon System dựa trên hệ thống SEAL và định hướng phát triển ban đầu.
- Tạo các issue đầu tiên về đổi tên kỹ thuật và ý tưởng kiến trúc AWS; có commit đổi tên để tiếp tục xử lý trên nhánh phát triển.
- Hệ thống hóa kiến thức về Shared Responsibility, IAM và các yếu tố cần cân nhắc khi dự toán chi phí.

### Bài học rút ra

- Cần phân biệt chức năng kế thừa từ dự án cũ với các thay đổi AWS và AI dự định thực hiện.
- Quyền của người dùng trong ứng dụng và quyền IAM của workload giải quyết các vấn đề khác nhau.
- Dự toán cần đi cùng giả định rõ ràng về workload, Region và lịch vận hành.

### Bước tiếp theo

Chuẩn bị proposal và phân công các nhóm frontend, backend, hạ tầng và AI trong Week 6.
