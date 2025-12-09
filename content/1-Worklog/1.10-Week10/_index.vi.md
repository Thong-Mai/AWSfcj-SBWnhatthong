---
title: "Worklog Tuần 10"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu cần làm tuần 10:

- Rà lại phần **CloudFront, Amplify**, chọn hướng dùng LocalStack.
- Chỉnh sửa lại kiến trúc cho phù hợp với thiết kế mới (Amplify, PostgreSQL, v.v.).
- Bắt đầu dùng **Docker** để đóng gói môi trường, và chuẩn bị **template báo cáo**.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                 | Ngày bắt đầu       | Ngày hoàn thành | Nguồn tài liệu    |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | --------------- | ----------------- |
| 1   | Họp: xem lại CloudFront, quyết định dùng **LocalStack bản Base** để hỗ trợ nhiều dịch vụ hơn (đặc biệt là Cognito), đổi database web sang Supabase, chỉnh lại flow kiến trúc.                            | 09/11/2025 20:30   | 09/11/2025      | |
| 2   | Thảo luận: liệt kê các thư viện Python cần dùng, kiểm tra version, chỉnh sửa lại proposal cho phù hợp với kiến trúc mới.                                          | 09/11/2025         | 10/11/2025      |            |
| 3   | Họp: tìm hiểu Docker, cách cài thư viện vào image, chuẩn bị idea “dùng Docker + LocalStack” làm môi trường chuẩn; đồng thời sửa lại architecture và bản tiếng Việt của tài liệu.                         | 11/11/2025 20:00   | 11/11/2025 23:30||
| 4   | Họp ngắn: chỉnh sửa kiến trúc lên **phiên bản v9**, thống nhất các service bắt buộc phải có trong mô hình.                                                        | 12/11/2025 20:00   | 12/11/2025 23:00| |
| 5   | Cá nhân: tham gia chuẩn bị **file docx template** (phần liên quan đến worklog và workshop), lên ý tưởng sẽ đưa lên Hugo như thế nào.                              | 12/11/2025         | 13/11/2025      |        |

### Kết quả đạt được tuần 10:

- CloudFront, Amplify, Supabase, LocalStack đã được:
  - Đưa vào kiến trúc một cách rõ ràng hơn.
- Kiến trúc **v9**:
  - Rõ ràng hơn về luồng dữ liệu và vị trí từng service.
- Bắt đầu làm quen với **Docker**:
  - Chuẩn bị tốt cho việc đóng gói môi trường dev (LocalStack, Terraform, v.v.).
- Template báo cáo docx:
  - Đã có phác thảo, giúp việc viết report/Workshop sau này nhẹ hơn rất nhiều.