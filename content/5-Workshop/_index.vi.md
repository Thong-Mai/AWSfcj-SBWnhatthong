---
title: "5. Workshop"
menuTitle: "5. Workshop"
weight: 5
chapter: false
pre: "<b>5. </b>"
---

# Workshop  
# Batch-Based Clickstream Analytics Platform

![Architecture](/images/architecture.png)

#### Tổng quan

Workshop này triển khai một **nền tảng phân tích Clickstream theo kiểu batch (Batch-Based Clickstream Analytics Platform)** cho một website thương mại điện tử bán sản phẩm máy tính.

Hệ thống thu thập các sự kiện (events) clickstream từ frontend, lưu dữ liệu JSON thô trong **Amazon S3**, xử lý events theo lịch ETL định kỳ (**AWS Lambda + EventBridge**), và nạp dữ liệu phân tích vào một **PostgreSQL Data Warehouse trên EC2** nằm trong private subnet.

Các dashboard phân tích tương tác được xây dựng bằng **R Shiny**, chạy trên cùng EC2 với Data Warehouse, và được truy cập thông qua **AWS Systems Manager Session Manager** (không cần SSH).

Nền tảng được thiết kế với các tiêu chí:

- Tách biệt rõ ràng giữa workload **OLTP và Analytics**  
- Backend analytics chỉ chạy trong private subnet (**không có truy cập public vào DW**)  
- Sử dụng các thành phần serverless của AWS để tối ưu chi phí và khả năng mở rộng  
- Giảm tối đa số lượng “moving parts” để tăng độ ổn định và đơn giản  
- Không cần SSH: quản trị qua **SSM Session Manager** vào EC2 chạy DW / Shiny  

#### Các thành phần kiến trúc chính

**Miền Frontend & OLTP**

- Ứng dụng Next.js: **`ClickSteam.NextJS`** được host bằng **AWS Amplify Hosting**  
- **Amazon CloudFront** làm CDN toàn cầu  
- **Amazon Cognito** User Pool để xác thực người dùng  
- PostgreSQL OLTP chạy trên EC2: **`SBW_EC2_WebDB`** (public subnet)  
  - Database: `clickstream_web` (schema `public`)  
  - Port: `5432`  

**Miền Ingestion & Data Lake**

- **Amazon API Gateway (HTTP API)**: `clickstream-http-api`  
  - Route: `POST /clickstream`  
- **Lambda Ingest**: `clickstream-lambda-ingest`  
  - Validate payload, enrich metadata, ghi file JSON vào S3  
- **S3 Raw Clickstream Bucket**: `clickstream-s3-ingest`  
  - Prefix: `events/YYYY/MM/DD/`  
  - Mẫu tên file: `event-<uuid>.json`  
  - `RAW_BUCKET = clickstream-s3-ingest`  

**Miền Analytics & Data Warehouse**

- **EC2 private cho DW + Shiny**: `SBW_EC2_ShinyDWH` (private subnet `10.0.128.0/20`)  
  - Database DW: `clickstream_dw` (schema `public`)  
  - Bảng chính: `clickstream_events` với các field:
    - `event_id, event_timestamp, event_name`  
    - `user_id, user_login_state, identity_source, client_id, session_id, is_first_visit`  
    - `context_product_id, context_product_name, context_product_category, context_product_brand`  
    - `context_product_price, context_product_discount_price, context_product_url_path`  
  - R Shiny Server chạy trên port `3838`, app path `/sbw_dashboard`  

- **Lambda ETL**: `SBW_Lamda_ETL` (chạy trong VPC)  
  - Đọc JSON thô từ `clickstream-s3-ingest`  
  - Transform thành các dòng dữ liệu dạng SQL-ready  
  - Insert vào bảng `clickstream_dw.public.clickstream_events`  

- **EventBridge Rule**: `SBW_ETL_HOURLY_RULE`  
  - Lịch chạy: `rate(1 hour)`  

- **VPC & Networking**

  - VPC CIDR: `10.0.0.0/16`  
  - Public subnet: `10.0.0.0/20` → `SBW_Project-subnet-public1-ap-southeast-1a` (EC2 OLTP)  
  - Private subnet: `10.0.128.0/20` → `SBW_Project-subnet-private1-ap-southeast-1a` (DW, Shiny, ETL Lambda)  
  - **S3 Gateway VPC Endpoint** để truy cập S3 trong private network  
  - **SSM Interface Endpoints** (SSM, SSMMessages, EC2Messages) cho Session Manager  

- **Truy cập quản trị (SSM)**  
  - Port forwarding:
    - `localPort = 3838`  
    - `portNumber = 3838`  
  - Shiny URL khi truy cập từ local: `http://localhost:3838/sbw_dashboard`  

#### Bản đồ nội dung

1. **[5.1. Objectives & Scope](5.1-objectives--scope/)**  
2. **[5.2. Architecture Walkthrough](5.2-architecture-walkthrough/)**  
3. **[5.3. Implementing Clickstream Ingestion](5.3-implementing-clickstream-ingestion/)**  
4. **[5.4. Building the Private Analytics Layer](5.4-building-private-analytics-layer/)**  
5. **[5.5. Visualizing Analytics with Shiny Dashboards](5.5-visualizing-analytics-with-shiny-dashboards/)**  
6. **[5.6. Summary & Clean up](5.6-summary-cleanup/)**
