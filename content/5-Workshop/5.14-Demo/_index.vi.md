---
title: "Demo"
date: 2026-11-27
weight: 14
chapter: false
pre: " <b> 5.14. </b> "
---

#### Link production

**[https://sealaws.io.vn](https://sealaws.io.vn)**

Môi trường chỉ chạy trong thời gian đánh giá để kiểm soát chi phí ([5.13](../5.13-cleanup/)). Nếu API trả 502/504, nghĩa là stack runtime đang tạm dừng; video bên dưới ghi lại cùng luồng demo.

#### Video demo

{{< youtube nkpH7XdOSsc >}}

#### Nội dung demo

Video đi qua một cuộc thi hackathon từ đầu đến cuối, theo góc nhìn của từng vai trò:

| Vai trò | Thao tác trong demo |
|---|---|
| Điều phối viên | Tạo sự kiện, cấu hình vòng thi, rubric chấm điểm và giải thưởng, mở đăng ký |
| Đội thi | Đăng ký tài khoản (email hoặc Google/GitHub), lập đội, tham gia sự kiện, nộp bài kèm link repository trước khi đồng hồ hết giờ |
| Giám khảo | Xem bài được phân công, chấm theo rubric, dùng AI Assist để có gợi ý đánh giá repository |
| Điều phối viên | Tổng hợp điểm, công bố kết quả và trao giải |

Phần hạ tầng AWS phía sau từng thao tác được trình bày ở các mục [5.3](../5.3-architecture/)–[5.10](../5.10-testing-validation/).

{{% notice note %}}
Tài khoản demo dùng địa chỉ thuộc tên miền dành riêng `.invalid` và không có mật khẩu, nên không ai đăng nhập được và không email nào tới người thật. Thông tin đăng nhập thử nghiệm thật không bao giờ được đăng trên trang này.
{{% /notice %}}
