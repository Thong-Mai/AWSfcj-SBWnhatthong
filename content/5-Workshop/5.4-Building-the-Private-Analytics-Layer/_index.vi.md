---
title: "Building the Private Analytics Layer"
weight: 54
---

## 5.4.1 VPC, Subnets và Route Tables

- **VPC CIDR**: `10.0.0.0/16`  
- **Public subnet**: `10.0.0.0/20` → `SBW_Project-subnet-public1-ap-southeast-1a`  
  - Chứa `SBW_EC2_WebDB` (EC2 OLTP).  
- **Private subnet**: `10.0.128.0/20` → `SBW_Project-subnet-private1-ap-southeast-1a`  
  - Chứa `SBW_EC2_ShinyDWH` và `SBW_Lamda_ETL`.  

**Public Route Table**

- `10.0.0.0/16 → local`  
- `0.0.0.0/0 → Internet Gateway`  

**Private Route Table**

- `10.0.0.0/16 → local`  
- S3 prefix list → Gateway VPC Endpoint cho S3  
- Không có route `0.0.0.0/0` tới IGW hoặc NAT Gateway  

Điều này giúp lớp analytics **hoàn toàn private**, không truy cập trực tiếp ra Internet.

---

## 5.4.2 VPC Endpoints (S3 & SSM)

![Các VPC Endpoint cho S3 & SSM](/images/aws-vpc-endpoints-s3-ssm.png)

### S3 Gateway VPC Endpoint

- Cho phép **truy cập S3 trong private network** cho:
  - `SBW_Lamda_ETL`  
  - `SBW_EC2_ShinyDWH` (cho backup hoặc các chức năng khác trong tương lai)  
- Không cần dùng NAT Gateway.

### SSM Interface Endpoints

- `com.amazonaws.ap-southeast-1.ssm`  
- `com.amazonaws.ap-southeast-1.ssmmessages`  
- `com.amazonaws.ap-southeast-1.ec2messages`  

Các endpoint này cho phép **Session Manager** quản lý và port-forward tới `SBW_EC2_ShinyDWH` mà không cần public IP hay cổng SSH.

---

## 5.4.3 Data Warehouse trên EC2 – `SBW_EC2_ShinyDWH`

Trên EC2 private này:

- PostgreSQL DB: `clickstream_dw` (schema `public`)  
- Bảng chính: `clickstream_events` với các field:

```text
event_id
event_timestamp
event_name
user_id
user_login_state
identity_source
client_id
session_id
is_first_visit
context_product_id
context_product_name
context_product_category
context_product_brand
context_product_price
context_product_discount_price
context_product_url_path
```

Bạn có thể thêm các bảng tổng hợp (sessions, funnels, …) sau này.

Instance chỉ cho phép truy cập từ:

- `SBW_Lamda_ETL` (qua `sg_Lambda_ETL` → `sg_analytics_ShinyDWH` trên port `5432`)  
- Localhost trên chính EC2, được R Shiny sử dụng để connect DB  

---

## 5.4.4 ETL Lambda – `SBW_Lamda_ETL` (chạy trong VPC)

ETL Lambda là nơi xử lý batch chính.

**Cấu hình VPC:**

- Subnet: `SBW_Project-subnet-private1-ap-southeast-1a`  
- Security group: `sg_Lambda_ETL`  

**Environment variables:**

- `DWH_HOST`, `DWH_PORT=5432`, `DWH_USER`, `DWH_PASSWORD`, `DWH_DATABASE=clickstream_dw`  
- `RAW_BUCKET=clickstream-s3-ingest`  
- `AWS_REGION=ap-southeast-1`  

**Nhiệm vụ:**

1. Xác định danh sách files trong `s3://clickstream-s3-ingest/events/YYYY/MM/DD/` cho batch cần xử lý.  
2. Với mỗi file JSON:
   - Parse payload event.  
   - Map field tương ứng với schema DW (`clickstream_events`).  
3. Insert các dòng vào PostgreSQL:
   - Nên thực hiện theo batch, trong transaction.  

**IAM role:**

- `s3:GetObject`, `s3:ListBucket` trên `clickstream-s3-ingest`.  
- Truy cập PostgreSQL được kiểm soát bởi user/password DB, không dùng IAM auth.  
- Gắn policy kiểu `AWSLambdaVPCAccessExecutionRole` (hoặc tương đương) để Lambda quản lý ENI.

---

## 5.4.5 Lên lịch bằng EventBridge – `SBW_ETL_HOURLY_RULE`

![EventBridge rule](/images/aws-eventbridge-sbw-etl-hourly-rule.png)

EventBridge giúp nền tảng hoạt động theo kiểu **batch**:

- **Tên rule**: `SBW_ETL_HOURLY_RULE`  
- **Schedule**: `rate(1 hour)`  
- **Target**: `SBW_Lamda_ETL`  

Mỗi lần rule chạy:

1. ETL Lambda chạy trong private subnet.  
2. Đọc các events mới từ S3 qua Gateway Endpoint.  
3. Nạp dữ liệu đã xử lý vào `clickstream_dw`.  

Bạn cũng có thể **trigger ETL Lambda thủ công** (từ Lambda console) cho mục đích backfill hoặc test.

---

## 5.4.6 Tóm tắt Security Groups & Connectivity

- `sg_Lambda_ETL`:
  - Outbound tới S3 endpoint và `sg_analytics_ShinyDWH:5432`.  

- `sg_analytics_ShinyDWH`:
  - Inbound `5432/tcp` từ `sg_Lambda_ETL`.  
  - Inbound `3838/tcp` cho Shiny (chỉ dùng qua SSM port forwarding).  

Vì **không có route từ private subnet ra Internet**, bạn được lợi:

- Giảm bề mặt tấn công  
- Dễ kiểm soát đường đi traffic outbound  
- Chi phí networking thấp hơn (không NAT Gateway)  

---

## 5.4.7 Mapping sang LABs

- Tạo VPC, subnets, route tables, endpoints, EC2:
  - **LAB1 – Networking & EC2**  
- Xây ETL Lambda + EventBridge:
  - **LAB3 – EventBridge & Lambda ETL**  
