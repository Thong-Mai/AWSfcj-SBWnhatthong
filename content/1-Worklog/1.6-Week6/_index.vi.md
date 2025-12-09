---
title: "Worklog Tuần 6"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu cần làm tuần 6:

- Hệ thống hóa lại kiến thức **Database**: RDBMS, NoSQL, OLTP, OLAP, khóa chính/ngoại, index, partition, log, buffer.
- Tìm hiểu các dịch vụ database quản lý bởi AWS: **RDS, Aurora, ElastiCache, Redshift**.
- Bắt đầu phác thảo các yêu cầu cho project Clickstream (nhưng **chưa chính thức bắt tay làm dự án**).
- Làm quen với việc ước tính chi phí và suy nghĩ về kiến trúc tổng thể.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                      |
| --- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------- |
| 1   | Ôn lại các khái niệm database: **bảng, session, primary key, foreign key, index, partition, execution plan, log, buffer, OLTP/OLAP**.| 13/10/2025   | 14/10/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)          |
| 2   | Học tổng quan các dịch vụ database trên AWS: **RDS, Aurora, ElastiCache, Redshift**, use case chính của từng dịch vụ.               | 15/10/2025   | 15/10/2025      |                     |
| 3   | Ghi chú sơ bộ yêu cầu dự án: cần web thương mại điện tử, nơi lưu log clickstream, chỗ xử lý & phân tích; liệt kê dịch vụ khả thi.   | 16/10/2025   | 16/10/2025      |                        |
| 4   | Phác thảo kiến trúc mức cao: CloudFront, S3, Cognito, API Gateway/Lambda, EC2 + PostgreSQL, CloudWatch, SNS… (ở mức ý tưởng).      | 17/10/2025   | 17/10/2025      |                  |
| 5   | Thử dùng AWS Pricing Calculator để ước lượng chi phí sơ bộ cho một mô hình nhỏ (EC2, RDS, S3, CloudFront, Lambda).                 | 18/10/2025   | 18/10/2025      |                             |

### Kết quả đạt được tuần 6:

- Củng cố lại nền tảng về **database**:
  - Phân biệt OLTP vs OLAP.
  - Nhớ lại vai trò của primary key, foreign key, index, partition.
- Hiểu thêm về:
  - **RDS & Aurora** cho transactional workload.
  - **Redshift** cho phân tích dữ liệu lớn.
  - **ElastiCache** cho caching tăng tốc truy vấn.
- Bắt đầu hình dung **dự án Clickstream** sẽ cần:
  - Một nơi thu thập dữ liệu (web + SDK + API).
  - Một nơi lưu và xử lý (S3 + ETL + PostgreSQL).
  - Một nơi trực quan hóa (Shiny/Một công cụ BI).
- Việc phác thảo kiến trúc & ước lượng chi phí giúp:
  - Mình chuẩn bị tốt hơn trước khi **chính thức vào giai đoạn dự án từ Tuần 7**.