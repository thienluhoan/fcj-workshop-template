---
title: "Tổng quan"
weight: 1
chapter: false
pre: "<b> 5.1. </b> "
---

# 5.1. Tổng quan dự án

## Bài toán thực tế

Bite to Business trực tiếp bán thực phẩm cho cá nhân/hộ gia đình và tổ chức như nhà hàng, khách sạn, cửa hàng, đơn vị suất ăn và hợp tác xã. Website tập trung danh mục, giá theo số lượng, yêu cầu mua (Quote), quản lý kho, đóng gói và giao hàng. Đây **không phải** sàn thương mại điện tử nhiều nhà bán.

## Vai trò và quy trình nghiệp vụ

| Vai trò | Nhiệm vụ chính |
|---|---|
| Guest | Xem sản phẩm/giá/review công khai, gửi đăng ký |
| Customer | Tạo Quote, xác nhận mua hợp lệ, theo dõi Order, review |
| E-commerce Executive | Quản lý danh mục, giá bậc, khuyến mãi và duyệt Quote |
| Logistics Staff | Nhập kho, chuẩn bị package, phân công chuyến |
| Delivery Staff | Xem chuyến được phân công, xác nhận hoàn tất giao |
| Admin | Duyệt tài khoản, quản lý role Staff và giám sát |

**Luồng giao dịch chính:** Catalog → chọn SKU/số lượng → backend tính Volume Pricing → Customer gửi Quote → E-commerce Executive duyệt → Customer xác nhận → backend kiểm tra và giữ **đủ toàn bộ** tồn kho theo transaction → Order → Logistics chuẩn bị package → giao hàng. Thiếu một dòng hàng thì không xác nhận một phần; Quote nháp **chưa giữ tồn kho**.

## Phạm vi công nghệ

- **Frontend:** React, TypeScript, Tailwind CSS, i18n; build tĩnh đặt trên S3 private, phân phối qua CloudFront.
- **Backend:** Java/Spring Boot dạng modular monolith, Docker image lưu ECR, ECS Fargate chạy tác vụ, ALB nhận API.
- **Database:** Supabase PostgreSQL bên ngoài AWS, **không tạo Amazon RDS** trong bài này.
- **Hình ảnh:** Cloudinary lưu ảnh sản phẩm/nội dung; S3 chỉ dùng cho frontend build.
- **AI và bản đồ:** Gemini/Spring AI/A2UI và Vietmap thuộc tích hợp ứng dụng; Bedrock là phần bạn cùng nhóm và chưa được khẳng định đã tích hợp vào BTB ở bài này.

## Tiêu chí hoàn thành

1. React tải được qua HTTPS, S3 origin là private; tải lại URL trang con thành công.
2. Spring Boot có API qua HTTPS ALB và Fargate chạy ổn định.
3. Backend kết nối Supabase theo cấu hình bảo mật được duyệt; API Catalog công khai trả dữ liệu.
4. Spring Security vẫn phân quyền đúng giữa Customer/Staff; AWS IAM không thay thế phân quyền nghiệp vụ.
5. Log ECS xuất hiện trong CloudWatch; DNS và WAF hoạt động đúng quy tắc đã cấu hình.
6. Có ảnh thực tế, dự toán chi phí và quy trình Cleanup.

**Không tuyên bố:** đã tích hợp cổng thanh toán thực, đã triển khai Bedrock, hay có HA production khi chưa có bằng chứng.

**Ảnh cần chụp:** trang React đã deploy, API response, sơ đồ kiến trúc, trạng thái ECS/ALB.
