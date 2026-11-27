---
title: "Worklog Tuần 10"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

- Nắm chắc Git khi làm nhóm (branch, rebase, PR xếp chồng, viết lại lịch sử) sau các sự cố tuần 8.
- Viết 3 bài blog kỹ thuật chia sẻ kiến thức AWS cho cộng đồng.
- Mô hình hóa mối đe dọa cho luồng đăng nhập – gọi API – lưu dữ liệu của SecureTable.
- Xây lộ trình học cá nhân có hệ thống cho mảng Cloud & DevSecOps.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đọc Pro Git chương 1–3<br>- Luyện Learn Git Branching (rebase, cherry-pick)<br>- Ôn lại PR xếp chồng và viết lại lịch sử an toàn | 16/11/2026 | 16/11/2026 | [Pro Git (sách)](https://git-scm.com/book/en/v2)<br>[Learn Git Branching](https://learngitbranching.js.org)<br>[git push (--force-with-lease)](https://git-scm.com/docs/git-push) |
| 3 | - Phân tích 3 bài blog tham khảo (Terraform, WAF + API Gateway, SQS)<br>- Lập khung bài viết chung và chọn 3 chủ đề<br>- Viết bài 1: Upload file thẳng lên S3 bằng Presigned URL<br>- Vẽ sơ đồ minh họa cho 3 bài bằng script | 17/11/2026 | 17/11/2026 | [S3 – Presigned URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)<br>[API Gateway – Quotas](https://docs.aws.amazon.com/apigateway/latest/developerguide/limits.html)<br>[Lambda – Quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)<br>Bản nháp blog |
| 4 | - Viết bài 2: NAT Gateway và VPC Endpoint<br>- Viết bài 3: GitHub Actions deploy AWS không cần access key<br>- Viết prompt mẫu để tạo bài blog với công cụ AI khác | 18/11/2026 | 18/11/2026 | [VPC – Gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)<br>[Amazon VPC – Pricing](https://aws.amazon.com/vpc/pricing/)<br>[GitHub – Configuring OIDC in AWS](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)<br>Bản nháp blog<br>[AWS Study Group – nhóm Facebook](https://www.facebook.com/groups/awsstudygroupfcj) |
| 5 | - Đọc OWASP Top 10:2025 và Threat Modeling Manifesto<br>- Thử Threat Composer (AWS Labs)<br>- Nháp threat model luồng Cognito → API → DynamoDB | 19/11/2026 | 19/11/2026 | [OWASP Top 10:2025](https://owasp.org/Top10/2025/)<br>[Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org)<br>[Threat Composer (AWS Labs)](https://github.com/awslabs/threat-composer)<br>[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org) |
| 6 | - Tổng hợp 100 nguồn học chọn lọc, chia 8 chặng A–H<br>- Gắn mỗi nguồn với phần việc trong SecureTable<br>- Xác định thứ tự chứng chỉ gợi ý | 20/11/2026 | 20/11/2026 | [Lộ trình Cloud & DevSecOps (artifact cá nhân)](https://claude.ai/artifact/CJmXgmuQfzQasmVM5xU4wr)<br>[roadmap.sh – DevOps](https://roadmap.sh/devops)<br>[roadmap.sh – DevSecOps](https://roadmap.sh/devsecops)<br>[AWS Skill Builder](https://skillbuilder.aws) |

### Kết quả đạt được tuần 10:

- Thành thạo các thao tác Git dùng trong dự án: rebase, đổi base của PR xếp chồng, `push --force-with-lease`.
- Hoàn thành 3 bài blog kèm sơ đồ: (1) Upload file thẳng lên S3 bằng Presigned URL, (2) NAT Gateway và VPC Endpoint, (3) GitHub Actions deploy AWS không cần access key (OIDC).
- Có khung viết blog rút ra từ phân tích 3 bài tham khảo và prompt mẫu để tái sử dụng.
- Bản nháp threat model cho luồng Cognito → API Gateway → Lambda → DynamoDB, ánh xạ với OWASP Top 10:2025.
- Lộ trình *Cloud & DevSecOps* gồm 100 nguồn chọn lọc, 8 chặng A–H, có theo dõi tiến độ; thứ tự chứng chỉ gợi ý.
