---
title: "5.6. Summary & Clean up"
weight: 56
---

# Summary & Clean up

Phần cuối cùng này tóm tắt những gì bạn đã xây dựng và hướng dẫn cách **dọn dẹp resource AWS** để tránh phát sinh chi phí ngoài ý muốn.

---

## 5.6.1 Bạn đã xây dựng được những gì?

Sau khi hoàn thành workshop và các bài LAB0–LAB5, bạn đã dựng được một **Clickstream Analytics Platform** hoàn chỉnh:

1. **Lớp User-Facing**
   - Ứng dụng Next.js (`ClickSteam.NextJS`) trên Amplify + CloudFront  
   - Xác thực người dùng bằng Cognito  
   - PostgreSQL OLTP (`clickstream_web`) trên `SBW_EC2_WebDB` (public subnet)  

2. **Lớp Ingestion & Raw Data**
   - API Gateway HTTP API: `clickstream-http-api` (route `POST /clickstream`)  
   - Lambda Ingest: `clickstream-lambda-ingest`  
   - S3 Raw bucket: `clickstream-s3-ingest/events/YYYY/MM/DD/event-<uuid>.json`  

3. **Lớp Analytics Private**
   - VPC với public & private subnets (`SBW_Project_VPC`)  
   - S3 Gateway Endpoint, SSM Interface Endpoints  
   - Data Warehouse trên EC2: `SBW_EC2_ShinyDWH`, DB `clickstream_dw`  
   - ETL Lambda trong VPC: `SBW_Lamda_ETL`, được trigger bởi `SBW_ETL_HOURLY_RULE`  
   - R Shiny dashboards (`sbw_dashboard`) chỉ truy cập qua SSM port forwarding  

Tổng thể, kiến trúc này cho thấy cách thiết kế một **batch-based analytics platform** an toàn, tối ưu chi phí, chủ yếu dùng serverless + hai EC2.

---

## 5.6.2 Các điểm chính cần ghi nhớ

- **Separation of concerns**:
  - OLTP và Analytics tách trên 2 EC2 khác nhau, thuộc các domain logic khác nhau.  
- **Security**:
  - DW và Shiny chạy trong private subnet, không có public IP.  
  - SSM Session Manager thay thế SSH truyền thống.  
  - S3 Gateway Endpoint giữ traffic S3 trong private network của AWS.  
- **Tối ưu chi phí**:
  - Không sử dụng NAT Gateway.  
  - ETL dùng serverless (Lambda + EventBridge).  
  - S3 làm storage giá rẻ cho dữ liệu thô.  
- **Dễ mở rộng**:
  - Thiết kế hiện tại là batch-based, nhưng có thể mở rộng sang real-time, analytics phức tạp hơn hoặc chuyển sang các công nghệ DW khác.

---

## 5.6.3 Dọn dẹp Resource

Nếu bạn dùng AWS account cá nhân hoặc sandbox chung, rất quan trọng phải **clean up** sau khi lab xong:

1. **Amplify & CloudFront**
   - Xóa Amplify app (`ClickSteam.NextJS`).  
   - Thao tác này cũng xóa CloudFront distribution và S3 hosting bucket do Amplify tạo.

2. **API Gateway & Lambda**
   - Xóa `clickstream-http-api`.  
   - Xóa các Lambda:
     - `clickstream-lambda-ingest`  
     - `SBW_Lamda_ETL`  

3. **EventBridge**
   - Xóa rule `SBW_ETL_HOURLY_RULE`.  

4. **S3 Buckets**
   - Làm rỗng (empty) rồi xóa:
     - `clickstream-s3-ingest` (RAW clickstream)  
     - `clickstream-s3-sbw` (assets) nếu không dùng cho dự án khác  

5. **EC2 Instances**
   - Stop hoặc terminate:
     - `SBW_EC2_WebDB`  
     - `SBW_EC2_ShinyDWH`  
   - Release Elastic IP (nếu có gán).

6. **VPC & Networking**
   - Xóa VPC endpoints (S3 Gateway, SSM Interface Endpoints).  
   - Xóa route tables, subnets, Internet Gateway.  
   - Cuối cùng, xóa `SBW_Project_VPC` nếu không còn dùng.

7. **RDS / Database khác (nếu có)**
   - Workshop này dùng PostgreSQL trên EC2; nếu bạn tạo thêm RDS hoặc DB khác để thử nghiệm, nhớ xóa luôn.

---

## 5.6.4 Bước tiếp theo & Hướng mở rộng

Nếu muốn tiếp tục phát triển nền tảng:

- Thay DW trên EC2 bằng **Amazon Redshift Serverless** hoặc DW managed khác.  
- Thêm luồng **real-time ingestion** với Amazon Kinesis + Lambda.  
- Xây logic **sessionization**, segmentation người dùng, attribution model trong ETL.  
- Hardening kiến trúc:
  - Di chuyển OLTP lên **Amazon RDS** trong private subnets.  
  - Thêm backend/API layer giữa Amplify và database.  
  - Thiết lập CloudWatch metrics & alarms cho:
    - Lỗi Lambda,  
    - Độ trễ ETL,  
    - Mức giảm/tăng bất thường trong số lượng events.  

Chúc mừng – bạn đã hoàn thành một “mini data platform” khá hoàn chỉnh trên AWS, với kiến trúc, bảo mật và chi phí đủ chuẩn để làm nền tảng cho các bài học nâng cao sau này. 🎉
