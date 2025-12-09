---
title: "Sự kiện 3"
weight: 1
chapter: false
pre: " <b> 4.3. </b> "
---

## Địa điểm & Thời gian

- **Địa điểm:** Bitexco Financial Tower  
- **Thời gian:** 29/11/2025  

---

## Mục tiêu sự kiện

- Giới thiệu **Security Pillar** trong AWS Well-Architected Framework một cách hệ thống.
- Làm rõ **Shared Responsibility Model** và các lỗi bảo mật thường gặp trên cloud.
- Trình bày 5 nhóm nội dung chính:
  1. Identity & Access Management  
  2. Detection (phát hiện)  
  3. Infrastructure Protection  
  4. Data Protection  
  5. Incident Response
- Hướng dẫn cách áp dụng các nguyên tắc này vào hệ thống chạy trên AWS (multi-account, VPC, S3, EC2, RDS,…).

---

## Diễn giả

- **Huỳnh Hoàng Long**
- **Đinh Lê Hoàng Anh**
- **Trần Đức Anh**
- **Nguyễn Tuấn Thịnh**
- **Nguyễn Đỗ Thành Đạt**
- **Kha Van**
- **Thịnh Lâm**
- **Việt Nguyễn**
- **Mendel Grabski (Long)**
- **Tinh Truong**

---

## Key Highlights

### 1. Bức tranh tổng quan về Security trên AWS

- **Vai trò của Security Pillar:**
  - Là một trong các trụ cột quan trọng của Well-Architected.  
  - Nếu security bị “bỏ bê”, các pillar khác (cost, performance, reliability) cũng bị ảnh hưởng.
- **Shared Responsibility Model:**
  - AWS bảo vệ hạ tầng vật lý, hypervisor, một số lớp dịch vụ managed.
  - Khách hàng chịu trách nhiệm cấu hình, quản lý tài khoản, bảo vệ dữ liệu và ứng dụng của mình.
- **Các lỗi phổ biến:**
  - S3 public không cần thiết.
  - Security Group mở 0.0.0.0/0 ở port nhạy cảm.
  - Không bật MFA, dùng access key cũ không rotate.

### 2. Identity & Access Management (IAM)

- **Thiết kế quyền truy cập:**
  - Dùng role thay vì cài access key dài hạn trên server.  
  - Áp dụng nguyên tắc **Least Privilege**.
- **Công cụ hỗ trợ:**
  - IAM Identity Center, AWS Organizations, Service Control Policies (SCP), Permission Boundaries.  
  - IAM Access Analyzer để rà soát các quyền chia sẻ ra ngoài (public, cross-account).
- **Best practices:**
  - Bắt buộc MFA cho user quan trọng.  
  - Chuẩn hoá quy trình cấp/thay/thu hồi quyền.

### 3. Detection – Giám sát & Phát hiện

- **Dịch vụ cốt lõi:**
  - CloudTrail: ghi lại API call.  
  - GuardDuty: phát hiện hành vi bất thường.  
  - Security Hub: tập trung hoá các phát hiện bảo mật.
- **Log & sự kiện:**
  - Thu thập VPC Flow Logs, ALB/NLB logs, S3 access logs,…  
  - Dùng EventBridge để chuyển sự kiện bảo mật đến kênh cảnh báo hoặc workflow xử lý.
- **Tư duy detection-as-code:**
  - Định nghĩa rule phát hiện dưới dạng code để có thể version, review, rollback.

### 4. Infrastructure Protection

- **Network segmentation:**
  - Thiết kế public/private subnet rõ ràng, hạn chế truy cập trực tiếp từ Internet tới DB.  
  - Dùng Security Group và NACL đúng vai trò.
- **Bảo vệ lớp edge:**
  - Dùng AWS WAF, AWS Shield, AWS Network Firewall cho các ứng dụng tiếp xúc Internet.
- **Baseline cho workload:**
  - Chuẩn hoá cấu hình OS, patch, hardening cho EC2/ECS/EKS,…

### 5. Data Protection

- **Mã hoá & key management:**
  - AWS KMS để quản lý keys, policy, rotation.  
  - Encrypt at-rest cho S3, EBS, RDS, DynamoDB,…  
  - Encrypt in-transit với TLS/HTTPS.
- **Quản lý secrets:**
  - AWS Secrets Manager, SSM Parameter Store.  
  - Tránh để password/API key trong code.
- **Phân loại dữ liệu:**
  - Nhận diện dữ liệu nhạy cảm, áp policy truy cập phù hợp.

### 6. Incident Response

- **Quy trình cơ bản:**
  - Phát hiện → khoanh vùng → xử lý → phục hồi → rút kinh nghiệm (post-mortem).
- **Playbook:**  
  Chuẩn bị kịch bản sẵn cho:
  - Lộ access key.  
  - S3 bucket bị public.  
  - EC2 có dấu hiệu bị tấn công.
- **Tự động hoá:**
  - Dùng Lambda, Step Functions, Systems Manager để tự động hoá một phần các hành động response, giúp giảm MTTR.

---

## Key Takeaways

### 1. Security by design

- Bảo mật phải được thiết kế **từ lúc vẽ kiến trúc**, không phải tới cuối mới xử lý.
- Defense in depth: nhiều lớp bảo vệ nhỏ sẽ an toàn hơn một lớp tường lửa duy nhất.

### 2. Quy hoạch IAM & tài khoản

- Tổ chức tài khoản (multi-account) + SCP giúp:
  - Giới hạn thiệt hại khi có sự cố.  
  - Tách biệt môi trường dev/stage/prod rõ ràng.

### 3. Logging & Detection

- Không có log, gần như “mù” khi sự cố xảy ra.  
- Cần bật log đúng chỗ và có cơ chế đọc/alert chứ không chỉ bật rồi để đó.

### 4. Chuẩn bị trước cho Incident Response

- Nếu chờ tới lúc sự cố xảy ra mới nghĩ cách xử lý thì thường đã trễ.  
- Playbook, runbook, kịch bản test định kỳ là rất quan trọng.

---

## Ứng dụng vào công việc

- Kiểm tra lại:
  - S3 bucket, Security Group, IAM users/roles trong các project cá nhân.  
  - Bật MFA, tắt access key không dùng.
- Thiết lập:
  - CloudTrail, GuardDuty, Security Hub cơ bản trong account học tập.  
  - Một vài CloudWatch Alarm liên quan đến bảo mật.
- Thử viết:
  - Một kịch bản incident response nhỏ (ví dụ: lộ key) và mô phỏng cách xử lý.

---

## Trải nghiệm sự kiện

Event “AWS Well-Architected: Security Pillar” giúp mình:

- Nhìn bảo mật dưới góc độ **kiến trúc tổng thể** chứ không phải vài tuỳ chọn rời rạc trong Console.
- Hiểu thêm về cách AWS thiết kế bộ công cụ security để:
  - Phòng ngừa,
  - Phát hiện,
  - Và phản ứng với sự cố.  
- Rút kinh nghiệm để khi xây hệ thống trên AWS (như dự án clickstream), mình phải nghĩ về security từ đầu, thay vì “để đó đã, tính sau”.

---

## Một số hình ảnh sự kiện

![anh](/images/event3.png) 