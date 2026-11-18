---
title: "Dọn dẹp tài nguyên"
weight: 11
chapter: false
pre: "<b> 5.11. </b> "
---

# 5.11. Dọn dẹp tài nguyên

**Quan trọng:** phân biệt tài nguyên **riêng của lab** với hạ tầng chung/production của nhóm. Không xóa VPC, NAT, endpoints, pipeline, log group hay domain dùng chung. Hỏi người phụ trách trước khi xóa. Đây là hướng dẫn, **không phải xác nhận đã cleanup**.

1. **Lưu minh chứng:** chụp demo cuối, trạng thái dịch vụ, screenshot và Pricing Calculator. Sao lưu cấu hình cần thiết nhưng không copy secret.
2. **ECS/Fargate:** giảm desired task về 0, xóa ECS service sau khi task dừng, xóa cluster/task definition của lab khi phù hợp.
3. **ALB:** gỡ listener HTTPS/HTTP, xóa ALB và target group riêng khi không còn tham chiếu; chú ý DNS alias.
4. **WAF:** disassociate Web ACL khỏi CloudFront/ALB, sau đó xóa ACL/rules riêng nếu không dùng nữa.
5. **CloudFront:** gỡ alias/association nếu cần, disable distribution, chờ deploy hoàn tất rồi xóa.
6. **Route 53/ACM:** chỉ xóa record thử nghiệm thuộc lab; giữ domain/hosted zone dùng chung. Xóa certificate không còn dùng sau khi dỡ ALB/CloudFront.
7. **S3:** xóa frontend object và version (nếu có), xóa bucket riêng sau khi CloudFront không còn dùng. Không xóa ảnh Cloudinary khi cleanup S3.
8. **ECR:** chỉ xóa repository/image riêng của lab sau khi ECS không tham chiếu.
9. **Secrets Manager:** lên lịch xóa secret riêng theo recovery window; đảm bảo service thật không còn tham chiếu.
10. **CloudWatch Logs:** xuất log cần lưu theo policy; chỉ xóa log group riêng, giữ log chung nếu có.
11. **VPC/IAM:** chỉ gỡ SG/subnet/route table/VPC khi **phối hợp** với bạn phụ trách NAT/endpoints; chỉ xóa IAM role/policy riêng đã hết dùng.
12. **Budgets:** giữ budget nếu còn theo dõi dự án chung; nếu không, xóa khi được phép. Kiểm tra Billing/Cost Explorer để phát hiện tài nguyên sót.

**Kiểm tra:** liệt kê lại từng dịch vụ và khoản phí dự kiến còn lại (ví dụ domain/hosted zone dùng chung). Đừng khẳng định phí đã dừng trước khi dữ liệu billing cập nhật.
