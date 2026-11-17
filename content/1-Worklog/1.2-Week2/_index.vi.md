---
title: "Worklog Tuần 2"
date: 2026-09-21
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

Trong tuần thứ hai (21/09/2026–25/09/2026), tôi học các chủ đề AWS networking và thực hành với VPC, EC2 cùng Route 53 Resolver. Tôi ghi lại nội dung theo từng ngày, từ nền tảng mạng đến kiểm tra Hybrid DNS và dọn dẹp tài nguyên.

**Địa điểm văn phòng:** Bitexco Financial Tower, số 2 Hải Triều, phường Sài Gòn, TP.HCM.

### Mục tiêu tuần 2

- Hiểu cách tổ chức VPC, CIDR, subnet và route table; xác định đường đi của lưu lượng mạng.
- Phân biệt Security Group và Network ACL, đồng thời xem xét cả lưu lượng đi và chiều phản hồi khi kiểm tra kết nối.
- Xây dựng môi trường public/private subnet, kết nối EC2 và tìm hiểu cách quản trị máy private qua EC2 Instance Connect Endpoint.
- So sánh VPC Peering, Transit Gateway, Site-to-Site VPN, Direct Connect và các loại load balancer ở mức lựa chọn kiến trúc.
- Cấu hình các thành phần Hybrid DNS với Route 53 Resolver và kiểm tra phân giải tên miền.
- Xác định phạm vi quản lý của CloudFormation và dọn dẹp các tài nguyên theo quan hệ phụ thuộc.

### Các công việc đã hoàn thành trong tuần

| Thứ | Công việc đã hoàn thành | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Lên văn phòng (lịch chuyên cần: Completed). Đã học nền tảng VPC, CIDR, subnet, route table, IGW và NAT; phân biệt SG với NACL; tìm hiểu tổng quan VPC Peering, Transit Gateway, Site-to-Site VPN, Direct Connect và load balancer. | 21/09/2026 | 21/09/2026 | [Module 02-01](https://www.youtube.com/watch?v=O9Ac_vGHquM), [02-02](https://www.youtube.com/watch?v=BPuD1l2hEQ4), [02-03](https://www.youtube.com/watch?v=CXU8D3kyxIc); [VPC CIDR](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-cidr-blocks.html), [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html); lịch chuyên cần cá nhân do người dùng cung cấp ngày 17/11/2026 (Completed) |
| 3 | Đã tìm hiểu cách subnet gắn với route table, vai trò của IGW và NAT Gateway, quy tắc SG/NACL và cách dùng VPC Resource Map để quan sát các thành phần mạng. | 22/09/2026 | 22/09/2026 | [Playlist Module 02, video 4–11](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab03](https://000003.awsstudygroup.com/); [Subnet sizing](https://docs.aws.amazon.com/vpc/latest/userguide/subnet-sizing.html) |
| 4 | Đã cấu hình VPC mẫu `10.10.0.0/16` với hai public và hai private subnet trên hai AZ, IGW, route table, security groups và EC2; kiểm tra kết nối, NAT Gateway và truy cập private EC2 qua EICE. | 23/09/2026 | 23/09/2026 | [Playlist Module 02, video 12–20](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ Lab03](https://000003.awsstudygroup.com/); [EC2 Instance Connect Endpoint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-with-ec2-instance-connect-endpoint.html) |
| 5 | Đã chuẩn bị môi trường Hybrid DNS bằng CloudFormation cho network và RDGW, cấu hình security group và truy cập RDGW; triển khai Managed Microsoft AD riêng theo hướng dẫn FCJ để mô phỏng DNS on-premises. | 24/09/2026 | 24/09/2026 | [Playlist Module 02, video 21–25](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ CloudFormation](https://000010.awsstudygroup.com/2-prerequiste/2.2-launchcloudformation/), [FCJ Managed AD](https://000010.awsstudygroup.com/4-setupad/) |
| 6 | Đã cấu hình Route 53 outbound endpoint, forwarding rule cho `onprem.example.com` và inbound endpoint; kiểm tra truy vấn DNS trong lab, sau đó dọn endpoint, rule/association, directory và stack theo phạm vi quản lý. | 25/09/2026 | 25/09/2026 | [Playlist Module 02, video 26–31](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i); [FCJ kiểm tra kết quả](https://000010.awsstudygroup.com/5-setuphyriddns/5.4-results/), [FCJ cleanup](https://000010.awsstudygroup.com/6-cleanup/); [Outbound queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-outbound-queries.html), [Inbound queries](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-forwarding-inbound-queries.html) |

### Kết quả đạt được tuần 2

- Hoàn thành 31 video Module 02, Lab03 và Lab10; tổng hợp kiến thức theo năm ngày học và liên hệ nội dung lý thuyết với các môi trường lab.
- Phân biệt được VPC, subnet, route table, IGW, NAT Gateway, Security Group và Network ACL; hiểu cách các thành phần phối hợp để chuyển tiếp hoặc kiểm soát lưu lượng.
- Cấu hình môi trường VPC mẫu với public/private subnet, EC2, các tuyến mạng cần thiết và kiểm tra những luồng kết nối chính trong lab.
- Tìm hiểu cách dùng EC2 Instance Connect Endpoint để truy cập EC2 private qua private IP; nhận biết IAM, security group và routing vẫn ảnh hưởng đến khả năng kết nối.
- Cấu hình các thành phần Route 53 Resolver cho bài Hybrid DNS và kiểm tra phân giải domain mô phỏng `onprem.example.com`.
- Thực hiện cleanup theo phạm vi tài nguyên: xử lý các thành phần tạo riêng và các tài nguyên thuộc CloudFormation stack.

### Bài học rút ra sau khi học

- Tôi rút ra rằng khi phân tích kết nối, cần lần theo toàn bộ đường đi của traffic: địa chỉ và subnet, route table, cổng mạng, security group, NACL và dịch vụ đích. Có route phù hợp chưa đủ để kết luận kết nối sẽ thành công.
- Tôi phân biệt được Security Group có trạng thái với Network ACL không lưu trạng thái. Vì vậy, khi dùng NACL, tôi cần xem xét cả rule cho lưu lượng đi và chiều phản hồi, cùng thứ tự đánh giá rule.
- Tôi hiểu public/private là đặc điểm của đường định tuyến của subnet; việc một EC2 có public IP là yếu tố riêng. Tôi cũng rút ra cần quy hoạch CIDR từ sớm để hạn chế xung đột khi kết nối nhiều mạng; AWS cho phép liên kết thêm secondary CIDR trong các điều kiện phù hợp.
- EICE giúp tôi hiểu một phương thức quản trị EC2 private qua private IP mà không cần gán public IP cho máy đích. Tuy vậy, quyền IAM, security group và routing vẫn cần được cấu hình phù hợp.
- Tôi phân biệt được hướng truy vấn của Route 53 inbound và outbound endpoint. Một lần truy vấn thành công chỉ xác nhận luồng đã thử; cần kiểm tra riêng từng hướng và khả năng truy cập ứng dụng khi phạm vi yêu cầu rộng hơn.
- Tôi rút ra cleanup là một phần của quy trình lab. Trước khi xóa, cần nhận diện tài nguyên do stack quản lý và tài nguyên tạo riêng, rồi xử lý theo quan hệ phụ thuộc.

### Bước tiếp theo

Ở Tuần 3, tôi chuyển sang Multi-VPC, VPC Peering và Transit Gateway, đồng thời tìm hiểu cách triển khai và kết nối EC2 trong kiến trúc có nhiều mạng.
