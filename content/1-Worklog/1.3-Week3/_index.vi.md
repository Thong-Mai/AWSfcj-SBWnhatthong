---
title: "Worklog Tuần 3"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu cần làm tuần 3:

- Học các khái niệm chính của **Amazon EC2** và cách lựa chọn instance type.
- Thực hành tạo, dừng, khởi động EC2, hiểu về **EBS, AMI, key pair**.
- Làm một số lab về **backup/restore** và **static website** trên S3 để kết nối kiến thức compute & storage.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                     |
| --- | ------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------------------------------------------- |
| 1   | Học các khái niệm **EC2, instance type, AMI, EBS, instance store, user data, metadata, Auto Scaling**.      | 22/09/2025   | 23/09/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)         |
| 2   | Thực hành tạo EC2, gắn EBS, test start/stop, đổi type nhỏ để hiểu ảnh hưởng tài nguyên & chi phí.          | 24/09/2025   | 24/09/2025      |                 |
| 3   | Làm lab **backup plan**: tạo backup, chạy thử restore, sau đó dọn dẹp tài nguyên tránh tốn phí.           | 25/09/2025   | 25/09/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)            |
| 4   | Thực hành **static website trên S3**: upload file HTML, cấu hình static hosting, test truy cập.            | 26/09/2025   | 26/09/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)           |
| 5   | Ghi chú lại so sánh: EC2 + Web server vs S3 static hosting trong những tình huống đơn giản.               | 27/09/2025   | 27/09/2025      |                         |

### Kết quả đạt được tuần 3:

- Nắm được **vai trò của EC2**:
  - Giống “máy chủ ảo” có thể chọn cấu hình CPU/RAM/disk theo nhu cầu.
  - Biết cách sử dụng EBS như ổ đĩa gắn ngoài, phân biệt với instance store.
- Biết cách:
  - Tạo, dừng, khởi động, thay đổi cấu hình một EC2 instance.
  - Gắn key pair để SSH và quản lý kết nối an toàn.
- Hoàn thành lab backup:
  - Hiểu ý nghĩa của việc **backup & restore** trong production.
- Làm được website tĩnh trên S3:
  - Hiểu rằng với những trang đơn giản, S3 + static hosting là giải pháp nhẹ, rẻ, ít phải quản lý.
- Bắt đầu hình dung sau này:
  - EC2 sẽ được dùng cho **PostgreSQL + R Shiny** trong dự án Clickstream.