---
title: "Worklog Tuần 2"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu cần làm tuần 2:

- Nắm được các khái niệm networking cơ bản trong AWS: **VPC, Subnet, Route Table, Security Group, Internet Gateway, NAT**.
- Hiểu cách **kiểm soát luồng traffic** ra/vào hệ thống trong VPC.
- Thực hành tạo VPC, Subnet, EC2 và kiểm tra kết nối SSH.
- Làm quen với việc đọc sơ đồ mạng trên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                              |
| --- | ------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------------------------- |
| 1   | Học tổng quan về **VPC, CIDR, Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group, NACL**.    | 15/09/2025   | 17/09/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup) |
| 2   | Xem demo các mô hình kết nối: **VPC peering, VPN, Direct Connect, Transit Gateway** (ở mức khái niệm).       | 18/09/2025   | 18/09/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)                         |
| 3   | Thực hành lab: tạo **VPC riêng**, 2 subnet (public/private), Internet Gateway, route table cho từng subnet.  | 19/09/2025   | 19/09/2025      |                             |
| 4   | Tạo EC2 trong public subnet, gán Security Group, test SSH từ máy cá nhân (MobaXterm/Putty).                  | 20/09/2025   | 20/09/2025      |                             |
| 5   | Ghi lại sơ đồ mạng (vẽ tay hoặc tool online) thể hiện VPC, subnet, EC2, IGW, route table đã cấu hình.        | 21/09/2025   | 21/09/2025      |                                    |

### Kết quả đạt được tuần 2:

- Hiểu rõ hơn **VPC như một “mạng riêng” trên cloud**:
  - Biết phân biệt public subnet và private subnet.
  - Hiểu vai trò của Internet Gateway và NAT Gateway trong việc ra/vào Internet.
- Thực hành thành công:
  - Tạo VPC, subnet, route table, gắn IGW.
  - Tạo một EC2 trong public subnet và SSH vào được.
- Bắt đầu quen với việc **vẽ lại kiến trúc mạng** để:
  - Nhìn tổng quan luồng traffic.
  - Dễ trao đổi với team hơn khi nói về kiến trúc sau này.
- Đây là bước nền tảng cho việc sau này:
  - Deploy PostgreSQL trên EC2 trong private subnet.
  - Thiết kế kiến trúc mạng cho cả dự án Clickstream.