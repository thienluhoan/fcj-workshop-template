---
title: "Kiểm thử, xác nhận và demo"
weight: 9
chapter: false
pre: "<b> 5.9. </b> "
---

# 5.9. Kiểm thử, xác nhận và demo

**Đây là kế hoạch test, chưa phải kết quả PASS.** Phải điền output/ảnh có ngày thực tế. Không bịa URL API hay ghi tính năng mới ở mức thiết kế là đã chạy.

| ID | Tình huống | Kỳ vọng | Kết quả / minh chứng thực tế |
|---|---|---|---|
| T01 | Mở `https://<FRONTEND_DOMAIN>` | React load qua CloudFront | TODO ảnh + thời điểm |
| T02 | Refresh route con React | SPA route vẫn hoạt động | TODO ảnh |
| T03 | Guest gọi API Catalog công khai thật | Trả đúng response hợp lệ | TODO cURL output |
| T04 | Guest gọi endpoint chỉ dành Customer | Trả 401/403 đúng contract | TODO response |
| T05 | Customer thử duyệt Quote của Staff | Spring Security từ chối | TODO response |
| T06 | Xác nhận Quote thiếu 1 SKU | Không tạo Order thành công/giữ tồn một phần | TODO snapshot DB/API |
| T07 | ECS service/task và ALB targets | Running=Desired, Healthy | TODO ảnh Console |
| T08 | Lỗi startup/ứng dụng | Có trong CloudWatch, không lộ secret | TODO log |
| T09 | Yêu cầu S3 object ẩn danh | Bị từ chối, OAC hoạt động | TODO HTTP status |
| T10 | Test WAF (nếu đã chạy) | Count/Block phù hợp cấu hình | TODO sampled request |
| T11 | DNS và TLS web/API | Domain và chứng chỉ đúng | TODO browser/CLI |

### Ví dụ test API dạng đọc dữ liệu

```bash
curl -i https://<API_DOMAIN>/<ACTUAL_PUBLIC_CATALOG_PATH>
curl -I https://<FRONTEND_DOMAIN>/
```

Dùng **path API thực tế** từ Swagger/OpenAPI. Không tự mặc định health endpoint nào cũng được public. Dùng test data và che email/JWT/dữ liệu khách hàng.

### Phối hợp giám sát

CloudWatch **Metrics/Alarms** là phần bạn cùng nhóm. Có thể trích minh chứng họ đã cung cấp cho demo tích hợp, nhưng bài này **không hướng dẫn tạo alarm**. Log ở 5.8 do ông tự triển khai.

### Live demo / video

- **Link demo:** TODO (đã xác thực truy cập)
- **Video/ảnh minh chứng:** TODO
- **Ngày, Region, phiên bản ứng dụng:** TODO
- **Hạn chế / test thất bại:** TODO (ghi trung thực)
