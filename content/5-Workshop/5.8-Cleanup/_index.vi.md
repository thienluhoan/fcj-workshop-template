---
title: "Dọn dẹp tài nguyên"
date: 2026-11-23
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

### Dọn bằng một lệnh
```powershell
.\scripts\nuke_and_teardown.ps1          # nhập DESTROY để xác nhận
.\scripts\nuke_and_teardown.ps1 -Force   # không hỏi lại
```
Script thực hiện theo thứ tự:
1. Huỷ multipart upload dang dở, xoá toàn bộ object, version và delete marker trong 3 bucket. Bucket có versioning không thể xoá bằng `terraform destroy` nếu còn version cũ.
2. `terraform destroy -auto-approve`, tự thử lại nếu gặp lỗi phụ thuộc.
3. Xoá các log group CloudWatch do API Gateway/Lambda tự tạo, tránh tiền lưu trữ log ngầm.
4. Quét lại DynamoDB, Cognito, API Gateway, WAF ở `ap-southeast-1` để xác nhận **không còn tài nguyên** nào.

### Kiểm tra thủ công
- **Billing → Bills** và **Cost Explorer** vào hôm sau: không còn dịch vụ phát sinh chi phí.
- Kiểm tra **mọi region**, không chỉ Singapore, phòng trường hợp tạo nhầm tài nguyên ở region khác.
- Huỷ đăng ký SNS email nếu không dùng nữa. Giữ lại AWS Budgets để tiếp tục được cảnh báo.

### Diễn tập khôi phục
Chạy `nuke_and_teardown.ps1 -Force` rồi `deploy_and_test.ps1` và bấm giờ. Toàn bộ hạ tầng phải được dựng lại và `/health` trả `healthy` mà không cần thao tác tay trên Console.
