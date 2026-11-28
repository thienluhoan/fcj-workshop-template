---
title: "Blog 3 (Group blog) - Upload lên S3 bằng Presigned URL"
date: 2026-11-04
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

### Thông tin bài đăng

- **Người đăng:** KT Trang, thành viên cùng nhóm.
- **Ngày đăng:** 04/11/2026, lúc 13:37 theo ảnh bài đăng.
- **Nơi đăng:** AWS Study Group VN trên Facebook.
- **Ghi nhận:** Blog của nhóm, được đưa vào báo cáo theo cách tính blog theo nhóm.
- **Bài gốc:** [Xem bài đăng trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/posts/2325113631586929).

### Làm thế nào để upload file lên S3 mà không đưa access key cho client?

Tính năng upload ảnh đại diện hay file PDF gần như app nào cũng có. Nếu cho file đi qua backend rồi mới đẩy lên S3, backend phải gánh toàn bộ băng thông. Còn để client upload thẳng lên S3 thì không thể đưa access key cho frontend. Presigned URL giải quyết vấn đề này.

#### 1. Presigned URL là gì?

Đây là URL được ký bằng credential của IAM principal tạo ra nó. URL cho phép thực hiện một thao tác cụ thể, như GET hoặc PUT, trên một object cụ thể trong thời gian giới hạn. Bucket vẫn giữ private.

Quyền của URL không vượt quá quyền của người ký. Role của backend không có `s3:PutObject` thì URL tạo ra vẫn bị 403.

#### 2. Luồng upload

1. Client gọi API kèm JWT để xin quyền upload.
2. Backend kiểm tra user, tự tạo key như `uploads/{userId}/{uuid}.jpg`, rồi ký URL hết hạn sau 5 phút.
3. Backend trả presigned URL cho client.
4. Client PUT file thẳng lên S3 bằng URL đó; file không đi qua backend.

{{< event-photo src="images/blogs/s3-presigned-url/upload-flow.png" alt="Client xin presigned URL qua API Gateway và Lambda; backend ký bằng IAM role, client PUT file thẳng lên S3 private." caption="Sơ đồ trong bài của KT Trang: backend cấp quyền upload có thời hạn, S3 nhận file trực tiếp từ client." >}}

#### 3. Thời hạn đặt bao lâu chưa chắc được bấy lâu

- Tạo bằng SDK hoặc CLI thì thời hạn tối đa là 7 ngày; tạo trên Console thì từ 1 phút đến 12 giờ.
- URL hết hạn theo thời gian đã đặt hoặc khi credential dùng để ký hết hạn, tùy điều kiện nào đến trước. Ký bằng role của Lambda, ECS hay EC2 có thể làm URL hết hạn sớm hơn thời hạn đã đặt.

#### 4. Những lưu ý

- Ai có link đều dùng được và có thể dùng nhiều lần đến khi hết hạn. Nên đặt thời hạn ngắn và không ghi URL vào log.
- Nếu key đã tồn tại, object cũ có thể bị ghi đè. Không nên dùng tên file client gửi lên làm key.
- Presigned PUT khó giới hạn dung lượng file. Khi cần chặn file lớn, có thể dùng presigned POST với điều kiện `content-length-range`.
- Sau khi upload vẫn nên kiểm tra loại file và nội dung phía server.

S3 lo phần truyền file, còn quyết định ai được upload, upload file nào và trong bao lâu vẫn nằm ở backend.

### Minh chứng bài đã đăng

{{< event-photo src="images/blogs/s3-presigned-url/facebook-publication.png" alt="Bài đăng của KT Trang trong AWS Study Group VN, ngày 4 tháng 11 lúc 13:37." caption="Ảnh minh chứng Blog 3 do KT Trang đăng, được người dùng cung cấp cho báo cáo nhóm." >}}

### Tài liệu tham khảo

- [Amazon S3 User Guide: Download and upload objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)
