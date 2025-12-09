---
title: "Worklog Tuần 13"
weight: 2
chapter: false
pre: " <b> 1.13 </b> "
---

### Mục tiêu cần làm tuần 13:

- Triển khai web lên **AWS Amplify** (môi trường thật), bật CloudFront, tích hợp Cognito.
- Thiết kế dataframe lớn cho dữ liệu clickstream và chia thành nhiều bảng.
- Deploy các service cần thiết lên AWS bằng Terraform.
- Hoàn thiện phần còn lại của dự án: xử lý dữ liệu, vẽ biểu đồ, chuẩn bị Workshop (Anh/Việt).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                           | Ngày bắt đầu       | Ngày hoàn thành | Nguồn tài liệu        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | --------------- | --------------------- |
| 1   | Họp: triển khai web lên **AWS Amplify**, bật CloudFront trong Amplify, kiểm tra S3 dành riêng cho Amplify, tích hợp Cognito vào web; thiết kế dataframe lớn cho clickstream và chia ra ~5 bảng; chạy Terraform để deploy các service cần thiết, tạo tài nguyên trên AWS cloud.                                         | 01/12/2025 22:30   | 02/12/2025 03:00|  |
| 2   | Nhận nhiệm vụ cá nhân: **tìm hiểu CloudWatch & SNS**, xem nên bật monitoring cho những dịch vụ nào, metric nào quan trọng (5xx, 4xx, latency, CPU, billing,…), cách gửi alert qua SNS.                                                                                        | 02/12/2025         | 04/12/2025      |  |
| 3   | Họp: hoàn thiện phần còn lại của dự án – fix lỗi NaN trong bảng dữ liệu, vẽ biểu đồ phân tích (traffic, hành vi, sản phẩm), chốt nhiệm vụ Workshop ,  up code Shiny lên Git.                                                | 07/12/2025 00:00   | 08/12/2025 00:00|   |
| 4   | Cá nhân: viết **Workshop bản tiếng Việt**, giữ nguyên thuật ngữ chuyên ngành cần thiết, nhấn mạnh phần: kiến trúc, quy trình xử lý dữ liệu, khó khăn & giải pháp, bài học rút ra.                                                                                            | 07/12/2025         | 08/12/2025      |              |
| 5   | Cá nhân: rà soát lại toàn bộ Worklog, note lại những điểm nổi bật để sau này dùng cho phần báo cáo/tự đánh giá.                                                                                                                       | 08/12/2025         | 08/12/2025      |           |

### Kết quả đạt được tuần 13:

- Web đã:
  - Chạy trên **AWS Amplify** với CloudFront phía trước.
  - Tích hợp thành công **Cognito**, chuẩn bị tốt cho việc gắn user với event clickstream.
- Dataframe clickstream:
  - Đã được thiết kế và chia thành nhiều bảng hợp lý, phục vụ cho phân tích.
- Terraform:
  - Dùng để deploy các dịch vụ chính lên AWS thật, giúp hạ tầng có thể tái tạo được.
- **CloudWatch & SNS**:
  - Mình đã nắm được cách theo dõi metric và gửi thông báo khi có bất thường (ở mức thiết kế & tìm hiểu).
- Dự án:
  - Đã fix lỗi NaN trong dữ liệu, vẽ được các biểu đồ chính.
  - Code Shiny đã được up lên Git.
- Workshop:
  - Bản tiếng Việt do mình viết đã tổng hợp lại:
    - Bối cảnh, kiến trúc, pipeline dữ liệu.
    - Khó khăn (deploy, dữ liệu, hạ tầng, chi phí).
    - Các hướng cải thiện nếu có thêm thời gian.