---
title: "Worklog Tuần 4"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu cần làm tuần 4:

- Học các khái niệm chính về **Amazon S3**: bucket, object, quyền truy cập, storage class, endpoint.
- Tìm hiểu cơ chế **backup, archival (Glacier)** và khái niệm **RTO/RPO**.
- Thực hành một số lab liên quan đến backup, S3 static website.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                      |
| --- | ------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | --------------------------------------------------- |
| 1   | Học tổng quan **S3: bucket, object, key, prefix, ACL, bucket policy, storage classes, S3 endpoint, static website**.    | 29/09/2025   | 29/09/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)           |
| 2   | Tìm hiểu **Glacier, Snowball, Snowmobile**, các phương án lưu trữ lâu dài & chuyển dữ liệu khối lượng lớn.             | 30/09/2025   | 30/09/2025      |           |
| 3   | Học khái niệm **Disaster Recovery, RTO, RPO, AWS Backup** và khi nào cần áp dụng.                                      | 01/10/2025   | 01/10/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)               |
| 4   | Làm lab backup: tạo S3 bucket, cấu hình Backup Plan, test backup/restore, verify dữ liệu, rồi dọn dẹp tài nguyên.      | 02/10/2025   | 02/10/2025      |                |
| 5   | Làm lại lab S3 static website lần nữa để quen tay, thử thêm vài file hình ảnh & chỉnh sửa quyền truy cập.             | 03/10/2025   | 03/10/2025      |                    |

### Kết quả đạt được tuần 4:

- Hiểu rõ hơn **S3 là object storage**:
  - Mỗi file là một object với metadata, lưu trong bucket.
  - Rất phù hợp cho log, ảnh, file tĩnh, dữ liệu raw (ví dụ: clickstream raw).
- Nắm được:
  - Sự khác nhau giữa các **storage class** (Standard, IA, Glacier) và khi nào nên dùng.
  - Cách kiểm soát truy cập bằng **ACL, bucket policy**.
- Hiểu các khái niệm DR:
  - RTO, RPO, ý nghĩa của chúng trong thiết kế hệ thống chịu lỗi.
- Lab backup giúp mình:
  - Thấy được quy trình “tạo kế hoạch backup → kiểm tra restore → dọn dẹp”.
- Việc lặp lại static website trên S3:
  - Giúp mình tự tin hơn khi thao tác trên S3, không còn sợ “bấm nhầm”.