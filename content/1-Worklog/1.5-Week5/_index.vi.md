---
title: "Worklog Tuần 5"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu cần làm tuần 5:

- Hiểu mô hình **AWS Shared Responsibility** và các nguyên tắc bảo mật cơ bản.
- Học về **IAM, Organizations, Cognito, Identity Center (SSO), KMS, Security Hub**.
- Thực hành tạo user, group, role, policy; bật MFA.
- Bắt đầu làm quen với khái niệm **clickstream** và yêu cầu của **Track 4**.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                      |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | --------------------------------------------------- |
| 1   | Học **Shared Responsibility Model**, ranh giới trách nhiệm giữa AWS & khách hàng, các best practice bảo mật cơ bản.                                | 06/10/2025   | 06/10/2025      | [AWS Study Group](https://www.youtube.com/@AWSStudyGroup)      |
| 2   | Học về **IAM, Organizations, Identity Center (SSO), KMS, Security Hub**, các khái niệm chính & use case phổ biến.                                 | 07/10/2025   | 07/10/2025      |                      |
| 3   | Thực hành: tạo IAM users, groups, roles, gắn policy; test login bằng user mới + bật MFA; thử giới hạn quyền & kiểm tra lỗi AccessDenied.          | 08/10/2025   | 09/10/2025      |                            |
| 4   | Tìm hiểu **Amazon Cognito**: user pool, identity pool, luồng đăng ký/đăng nhập, token; ghi chú để sau dùng cho web thương mại điện tử.           | 09/10/2025   | 09/10/2025      |                   |
| 5   | Đọc tài liệu về **clickstream, session, event, funnel** và yêu cầu **Track 4 – Session Management** trong chương trình; trao đổi với team.       | 10/10/2025   | 10/10/2025      |       |

### Kết quả đạt được tuần 5:

- Nắm rõ hơn về **bảo mật trên AWS**:
  - Ai chịu trách nhiệm phần nào (AWS vs mình).
  - Tầm quan trọng của việc không dùng root cho công việc hàng ngày.
- Thực hành thành công:
  - Tạo IAM user/group/role, gắn policy, bật MFA, thử giới hạn quyền truy cập.
- Hiểu sơ bộ về:
  - **Cognito** cho bài toán authentication trên web/app.
  - **KMS, Security Hub** ở mức khái niệm.
- Bắt đầu quen với các thuật ngữ:
  - **Clickstream, session, event**, hiểu rằng chúng liên quan trực tiếp đến đề tài dự án sắp làm.
- Tuần này đóng vai trò **cầu nối**:
  - Từ phần học nền tảng AWS → chuẩn bị tâm lý & kiến thức cho dự án Clickstream ở các tuần sau.