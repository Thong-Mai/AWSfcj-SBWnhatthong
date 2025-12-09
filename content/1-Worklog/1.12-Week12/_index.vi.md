---
title: "Worklog Tuần 12"
weight: 2
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu cần làm tuần 12:

- Kiểm thử sâu hơn web thương mại điện tử, triển khai lên **Vercel**.
- Nắm vững cách sử dụng **LocalStack Pro** và **Terraform** trên môi trường giả lập.
- Chuẩn bị cho việc triển khai Amplify cả trên LocalStack lẫn AWS thật.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                 | Ngày bắt đầu       | Ngày hoàn thành | Nguồn tài liệu         |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | --------------- | ---------------------- |
| 1   | Họp: test web thương mại điện tử (xem, thêm giỏ, mua hàng), thử dùng Clerk để login nhưng không dùng được, quyết định chuyển sang Cognito; thử deploy web lên Vercel, fix nhiều lỗi build trước khi thành công.                                                                            | 23/11/2025 23:00   | 24/11/2025 01:00|   |
| 2   | Ghi chú các lỗi Vercel: cấu hình `next.config.ts`, `package.json`, duplicate config, ESLint build, thiếu biến môi trường; lưu lại để sau viết vào phần “Khó khăn & giải pháp”.                                                                                                          | 24/11/2025         | 24/11/2025      |               |
| 3   | Họp: học cách sử dụng **LocalStack Pro** (config API Key, chạy docker image, apply Terraform vào LocalStack), đặt mission cho từng thành viên (Amplify, CloudFront, Cognito local, PostgreSQL EC2, S3 ảnh).                                                                               | 24/11/2025 20:00   | 24/11/2025 22:00|  |
| 4   | Họp: chạy Terraform, tìm hiểu LocalStack có thể host được những gì, tạo project test Amplify trên LocalStack, đồng thời plan triển khai Amplify trên AWS thật & tích hợp CloudFront, S3.                                                         | 30/11/2025 15:00   | 01/12/2025 01:30|     |
| 5   | Cá nhân: thử `terraform apply` trên LocalStack, xem log, ghi lại các lỗi thường gặp và cách xử lý, chuẩn bị cho bước deploy lên AWS thật.                                                                                                        | 01/12/2025         | 01/12/2025      |    |

### Kết quả đạt được tuần 12:

- Web đã:
  - Được **deploy thành công lên Vercel** sau nhiều lần sửa lỗi.
  - Login sử dụng Cognito thay cho Clerk.
- LocalStack Pro:
  - Đã cấu hình xong, hiểu được những dịch vụ nào có thể test trên local.
- Terraform:
  - Bước đầu chạy được trên LocalStack, nắm được quy trình `init → plan → apply`.
- Nhiều kinh nghiệm thực tế về:
  - Lỗi build Vercel, deploy Amplify, cách debug khi dùng Terraform trên môi trường mô phỏng.