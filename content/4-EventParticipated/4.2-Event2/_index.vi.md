---
title: "Sự kiện 2"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

## Địa điểm & Thời gian

- **Địa điểm:** Bitexco Financial Tower  
- **Thời gian:** 17/11/2025  

---

## Mục tiêu sự kiện

- Làm rõ **tư duy DevOps**: không chỉ là tool mà là cách làm việc giữa Dev và Ops.
- Giới thiệu hệ sinh thái **DevOps services trên AWS**: CodeCommit, CodeBuild, CodeDeploy, CodePipeline, CloudFormation/CDK, ECS/EKS, CloudWatch, X-Ray,…
- Hướng dẫn cách **thiết kế CI/CD pipeline** trên AWS với nhiều chiến lược deploy (blue/green, canary, rolling).
- Trình bày vai trò của **Infrastructure as Code (IaC)** trong việc quản lý hạ tầng.
- Tổng hợp các thực hành tốt về **monitoring, logging, tracing** để vận hành hệ thống ổn định.

---

## Diễn giả
 
- **Thịnh Nguyễn**
- **Bảo Huỳnh**
- **Hoàng Long**
- **Trần Vĩ**

---

## Key Highlights

### 1. Khởi động & bối cảnh DevOps

- **Check-in & networking:**  
  Tham dự viên giao lưu, chia sẻ về stack hiện đang dùng (on-prem, AWS, hybrid).
- **Giới thiệu tổng quan:**  
  Diễn giả nhắc lại lịch sử DevOps, vì sao mô hình Dev–Ops tách biệt dễ gây “đứt gãy” và chậm trễ.
- **Đặt vấn đề:**  
  Làm thế nào để tăng tần suất release, giảm lỗi khi deploy, và rút ngắn thời gian khôi phục sự cố.

### 2. DevOps mindset & đo lường

- **Mindset:**  
  - Dev và Ops cùng chịu trách nhiệm về chất lượng và độ ổn định của sản phẩm.  
  - Ưu tiên pipeline tự động và release nhỏ, đều đặn.
- **Metrics:**  
  - DORA metrics (Deployment Frequency, Lead Time, Change Failure Rate, Time to Restore).  
  - MTTR – nhìn dưới góc độ business chứ không chỉ kỹ thuật.
- **Ý nghĩa:**  
  Dùng số liệu để đánh giá quy trình, tránh tranh luận cảm tính.

### 3. CI/CD trên AWS

- **Bộ công cụ chính:**
  - **CodeCommit**: repository Git.  
  - **CodeBuild**: build/test tự động.  
  - **CodeDeploy**: triển khai lên EC2/ECS/Lambda với các chiến lược khác nhau.  
  - **CodePipeline**: “trục xương sống” để gắn các bước CI/CD.
- **Chiến lược triển khai:**
  - **Blue/Green:** vận hành song song 2 “màu” và chuyển traffic khi đã kiểm tra ổn.  
  - **Canary:** đưa một phần traffic nhỏ sang phiên bản mới trước.  
  - **Rolling:** cập nhật từng nhóm server để hệ thống không bị down toàn bộ.
- **Demo pipeline:**  
  Minh hoạ một pipeline đơn giản từ commit → build → test → deploy.

### 4. Infrastructure as Code & Container

- **CloudFormation & CDK:**
  - CloudFormation: template hạ tầng dưới dạng YAML/JSON.  
  - CDK: định nghĩa hạ tầng bằng code (TypeScript/Python/…) → dễ modular hóa, tái sử dụng.
- **Container trên AWS:**
  - **ECR:** registry chứa image, scan bảo mật, lifecycle policy.  
  - **ECS/EKS:** hai hướng chạy container (managed vs Kubernetes chuẩn).  
  - **App Runner:** phù hợp cho web/container đơn giản, ít cấu hình.
- **Bài học:**  
  Khi hạ tầng đã là code, việc nhân bản môi trường (dev, staging, prod) trở nên nhất quán và ít lỗi hơn.

### 5. Monitoring & Observability

- **CloudWatch:**
  - Thu thập metrics (CPU, RAM, latency…), log, custom metrics.  
  - Tạo alarm và gửi notification qua SNS.
- **X-Ray:**
  - Trace request qua nhiều service, giúp “nhìn xuyên” microservices.
- **Observability:**
  - Kết hợp metric + log + trace để giảm thời gian điều tra sự cố.

---

## Key Takeaways

### 1. Tư duy DevOps

- Tập trung vào **dòng chảy giá trị (value stream)** hơn là chức danh.  
- Đo lường bằng dữ liệu, tối ưu pipeline dựa trên metric thay vì cảm giác.

### 2. Thiết kế pipeline

- Pipeline nên:
  - Tự động hóa tối đa các bước lặp lại.  
  - Có stage kiểm thử, staging environment trước khi lên production.  
  - Có cơ chế rollback rõ ràng (đặc biệt với blue/green hoặc canary).

### 3. IaC & Container

- Hạ tầng nên:
  - Được mô tả hoàn toàn bằng CloudFormation/CDK → version control, code review.  
  - Được kiểm thử (lint, validate template) trước khi apply.
- Container giúp:
  - Môi trường dev/test/prod gần giống nhau, giảm lỗi do “khác máy”.

### 4. Monitoring là bắt buộc

- Không có monitoring thì rất khó làm DevOps đúng nghĩa.  
- Alert nên được thiết kế xoay quanh **trải nghiệm người dùng** (error rate, latency, downtime) thay vì chỉ CPU cao/thấp.

---

## Ứng dụng vào công việc

- Bắt đầu xây một **pipeline CI/CD tối thiểu** cho project (kể cả project cá nhân).
- Thử mô tả hạ tầng hiện tại bằng **CloudFormation/CDK** dù quy mô nhỏ.
- Thiết lập CloudWatch Alarm cơ bản:
  - API 5xx, latency, CPU EC2, lỗi build pipeline, v.v.
- Dần dần chuẩn hoá cách deploy:
  - Không deploy bằng “ssh vào server và gõ lệnh tay” nữa.

---

## Trải nghiệm sự kiện

Event “DevOps on AWS” giúp mình:

- Xâu chuỗi lại kiến thức rời rạc về CI/CD, IaC, container, monitoring thành **một bức tranh hoàn chỉnh**.  
- Thấy rõ DevOps trên AWS không chỉ là nhiều dịch vụ rời rạc, mà có thể kết nối thành một **pipeline liền mạch từ code tới production**.  
- Có thêm động lực để áp dụng DevOps một cách bài bản vào các dự án cá nhân và dự án clickstream của mình.

---

## Một số hình ảnh sự kiện