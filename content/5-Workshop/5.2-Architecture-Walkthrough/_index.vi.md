---
title: "Architecture Walkthrough"
weight: 52
chapter: false
pre: " <b> 5.2. </b> "
---

![Architecture](/images/architecture.png)
<p align="center"><em>Figure: Architecture Batch-base Clickstream Analytics Platform.</em></p>


## 5.2.1 Miền User-Facing

### Frontend – Amplify + CloudFront + Cognito

- **Ứng dụng Next.js**: `ClickSteam.NextJS`  
- Được host bởi **AWS Amplify Hosting** với CloudFront tích hợp sẵn làm CDN.  
- Ví dụ URL:  
  - `https://main.d2q6im0b1720uc.amplifyapp.com/`  
- **Amazon Cognito User Pool** xử lý đăng ký, đăng nhập, cấp token.  
- Frontend:
  - Render trang catalog sản phẩm (SSR)  
  - Dựa trên Cognito token để biết user đã đăng nhập hay chưa  
  - Gửi clickstream events tới `clickstream-http-api`  

### Cơ sở dữ liệu OLTP – `SBW_EC2_WebDB` 

- EC2 instance: `SBW_EC2_WebDB` trong public subnet `SBW_Project-subnet-public1-ap-southeast-1a`  
- PostgreSQL lắng nghe trên port `5432`  
- Database: `clickstream_web` (schema `public`)  

Một số bảng điển hình:

- `users`  
- `products`  
- `orders`  
- `order_items`  
- `inventory`  

Ứng dụng Amplify kết nối qua **Prisma** sử dụng `DATABASE_URL` trỏ tới endpoint public của EC2 này.  
> Trong production “chuẩn chỉnh”, OLTP DB thường chạy trên Amazon RDS trong private subnet, phía sau một backend API layer. Trong workshop này, ta chấp nhận EC2 DB public để tập trung vào phần analytics.

---

## 5.2.2 Miền Ingestion & Data Lake

### API Gateway HTTP API – `clickstream-http-api`

- HTTP API public expose:
  - `POST /clickstream`  
- Integration: Lambda `clickstream-lambda-ingest`  

### Lambda Ingest – `clickstream-lambda-ingest`

- Lambda stateless (không gắn VPC) có nhiệm vụ:
  - Nhận JSON payload từ frontend  
  - Validate và enrich event (timestamp, session ID, login state, context sản phẩm, …)  
  - Ghi JSON thô vào S3 bucket `clickstream-s3-ingest` với path dạng:
    - `events/YYYY/MM/DD/event-<uuid>.json`  

IAM cho function này được hạn chế tới:

- `s3:PutObject` trên bucket `clickstream-s3-ingest`  
- Quyền CloudWatch Logs để ghi log  

### S3 Raw Clickstream Bucket – `clickstream-s3-ingest`

- Region: `ap-southeast-1` (Singapore)  
- Bucket chứa **JSON clickstream thô**:
  - Không chứa dữ liệu đã xử lý / curated  
- Cấu trúc:
  - `events/YYYY/MM/DD/`  
- Mẫu tên file:
  - `event-<uuid>.json`  

Bucket riêng `clickstream-s3-sbw` dùng để chứa **website assets** (ảnh sản phẩm, static files) và **không** dùng cho clickstream.

---

## 5.2.3 Miền Analytics & Data Warehouse

### EC2 DW + Shiny – `SBW_EC2_ShinyDWH` (Private Subnet)

- EC2 nằm trong private subnet: `SBW_Project-subnet-private1-ap-southeast-1a`  
- Không có public IP  
- Gắn IAM role `AmazonSSMManagedInstanceCore`  

Trên instance này:

1. **PostgreSQL Data Warehouse**
   - Database: `clickstream_dw` (schema `public`)  
   - Bảng chính: `clickstream_events` với các field:
     - Event info: `event_id`, `event_timestamp`, `event_name`  
     - User/session info: `user_id`, `user_login_state`, `identity_source`, `client_id`, `session_id`, `is_first_visit`  
     - Product context: `context_product_id`, `context_product_name`, `context_product_category`, `context_product_brand`, `context_product_price`, `context_product_discount_price`, `context_product_url_path`  

2. **R Shiny Server**
   - Lắng nghe port `3838`  
   - Host app dashboard: `sbw_dashboard`  
   - URL nội bộ: `http://localhost:3838/sbw_dashboard`  

### ETL Lambda – `SBW_Lamda_ETL` (chạy trong VPC)

- Lambda được cấu hình để chạy **bên trong VPC**:
  - Subnet: `SBW_Project-subnet-private1-ap-southeast-1a`  
  - Security group: `sg_Lambda_ETL`  
- Environment variables:
  - `DWH_HOST`, `DWH_PORT=5432`, `DWH_USER`, `DWH_PASSWORD`, `DWH_DATABASE=clickstream_dw`  
  - `RAW_BUCKET=clickstream-s3-ingest`  
  - `AWS_REGION=ap-southeast-1`  

ETL Lambda:

- Liệt kê các file mới trong `clickstream-s3-ingest/events/YYYY/MM/DD/`  
- Đọc và parse JSON  
- Map dữ liệu thành các dòng cho bảng `clickstream_events`  
- Insert batch vào Data Warehouse  

### EventBridge Rule – `SBW_ETL_HOURLY_RULE`

- Quy tắc EventBridge dùng để lên lịch ETL:
  - `rate(1 hour)`  
- Target: `SBW_Lamda_ETL`  
- Đảm bảo clickstream events mới được xử lý ít nhất mỗi giờ một lần.

---

## 5.2.4 Thiết kế Networking & Security

### VPC & Subnets

- VPC CIDR: `10.0.0.0/16`  
- Public subnet:
  - `10.0.0.0/20` – `SBW_Project-subnet-public1-ap-southeast-1a` (EC2 OLTP)  
- Private subnet:
  - `10.0.128.0/20` – `SBW_Project-subnet-private1-ap-southeast-1a` (DW, Shiny, ETL)  

**Public Route Table**

- `10.0.0.0/16 → local`  
- `0.0.0.0/0 → Internet Gateway`  

**Private Route Table**

- `10.0.0.0/16 → local`  
- S3 prefix list → Gateway VPC Endpoint cho S3  
- Không có route `0.0.0.0/0` tới IGW hoặc NAT Gateway  

> Quyết định quan trọng: **Không dùng NAT Gateway**.  
> Các thành phần private (DW, Shiny, ETL) truy cập S3 qua Gateway Endpoint, quản trị qua SSM Interface Endpoints.

### VPC Endpoints

- **Gateway Endpoint** cho S3:
  - Cho phép ETL Lambda và DW EC2 truy cập `clickstream-s3-ingest` trong private network.  
- **Interface Endpoints**:
  - `com.amazonaws.ap-southeast-1.ssm`  
  - `com.amazonaws.ap-southeast-1.ssmmessages`  
  - `com.amazonaws.ap-southeast-1.ec2messages`  

Nhờ đó **SSM Session Manager** có thể hoạt động hoàn toàn trong mạng riêng của AWS.

### Security Groups

- `sg_oltp_webDB`
  - Inbound:
    - `5432/tcp` từ Amplify / IP admin  
    - `22/tcp` từ IP admin (tùy chọn; có thể bỏ nếu dùng SSM hoàn toàn)  
- `sg_analytics_ShinyDWH`
  - Inbound:
    - `5432/tcp` từ `sg_Lambda_ETL`  
    - `3838/tcp` cho Shiny (chỉ truy cập qua SSM port forward)  
- `sg_Lambda_ETL`
  - Outbound tới `sg_analytics_ShinyDWH` trên port `5432`  
  - Outbound tới S3 thông qua Gateway Endpoint  
- `sg_ec2_VPC_Interface_endpoint_SSM`
  - Inbound `443/tcp` từ private subnets cho các endpoint SSM  

---

## 5.2.5 Mapping

- **VPC + EC2** → xem **LAB1 – Networking & EC2**  
- **S3 + Lambda Ingest + API Gateway** → xem **LAB2 – S3, Lambda Ingest & API Gateway**  
- **ETL Lambda + EventBridge + DW** → xem **LAB3 – EventBridge & Lambda ETL**  
- **Amplify Frontend + Clickstream SDK** → xem **LAB4 – Frontend (Amplify) & Clickstream SDK**  
- **R Shiny + kiểm thử end-to-end** → xem **LAB5 – R Shiny Analytics & Validation**  
