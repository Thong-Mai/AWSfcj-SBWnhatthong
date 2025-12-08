---
title: "5.4. Building the Private Analytics Layer"
weight: 54
---

# Building the Private Analytics Layer

This section focuses on everything **inside the VPC**: the private subnets, VPC endpoints, ETL Lambda, and the PostgreSQL Data Warehouse with R Shiny.

> Hands-on details for this section are mainly in **LAB1 – Networking & EC2** and **LAB3 – EventBridge & Lambda ETL**.

---

## 5.4.1 VPC, Subnets, and Route Tables

- **VPC CIDR**: `10.0.0.0/16`  
- **Public subnet**: `10.0.0.0/20` → `SBW_Project-subnet-public1-ap-southeast-1a`  
  - Hosts `SBW_EC2_WebDB` (OLTP EC2).  
- **Private subnet**: `10.0.128.0/20` → `SBW_Project-subnet-private1-ap-southeast-1a`  
  - Hosts `SBW_EC2_ShinyDWH` and `SBW_Lamda_ETL`.  

**Public Route Table**

- `10.0.0.0/16 → local`  
- `0.0.0.0/0 → Internet Gateway`  

**Private Route Table**

- `10.0.0.0/16 → local`  
- S3 prefix list → Gateway VPC Endpoint for S3  
- No `0.0.0.0/0` to IGW or NAT Gateway  

This makes the analytics layer **fully private** with no direct Internet access.

---

## 5.4.2 VPC Endpoints (S3 & SSM)

### S3 Gateway VPC Endpoint

- Allows **private S3 access** for:
  - `SBW_Lamda_ETL`  
  - `SBW_EC2_ShinyDWH` (for backups or future features)  
- Eliminates the need for a NAT Gateway.

### SSM Interface Endpoints

- `com.amazonaws.ap-southeast-1.ssm`  
- `com.amazonaws.ap-southeast-1.ssmmessages`  
- `com.amazonaws.ap-southeast-1.ec2messages`  

These endpoints enable **Session Manager** to manage and port-forward into `SBW_EC2_ShinyDWH` without any public IP or SSH port.

---

## 5.4.3 Data Warehouse on EC2 – `SBW_EC2_ShinyDWH`

On the private EC2 instance:

- PostgreSQL DB: `clickstream_dw` (schema `public`)  
- Main table: `clickstream_events` with fields:

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

Additional aggregated tables (sessions, funnels, etc.) can be added later.

The instance is reachable only from:

- `SBW_Lamda_ETL` (via `sg_Lambda_ETL` → `sg_analytics_ShinyDWH` on port `5432`)  
- Localhost on the EC2, used by R Shiny  

---

## 5.4.4 ETL Lambda – `SBW_Lamda_ETL` (VPC-Enabled)

ETL Lambda is where batch processing happens.

**VPC configuration:**

- Subnet: `SBW_Project-subnet-private1-ap-southeast-1a`  
- SG: `sg_Lambda_ETL`  

**Environment variables:**

- `DWH_HOST`, `DWH_PORT=5432`, `DWH_USER`, `DWH_PASSWORD`, `DWH_DATABASE=clickstream_dw`  
- `RAW_BUCKET=clickstream-s3-ingest`  
- `AWS_REGION=ap-southeast-1`  

**Task:**

1. Identify files in `s3://clickstream-s3-ingest/events/YYYY/MM/DD/` for the target batch window.  
2. For each JSON file:
   - Parse event payload.  
   - Map fields into the DW schema (`clickstream_events`).  
3. Insert rows into PostgreSQL:
   - Ideally in batches, in a transaction.  

**IAM role:**

- `s3:GetObject`, `s3:ListBucket` on `clickstream-s3-ingest`.  
- PostgreSQL access is controlled by DB user/password, not IAM.  
- `AWSLambdaVPCAccessExecutionRole` (or equivalent) for ENI management.  

---

## 5.4.5 EventBridge Scheduling – `SBW_ETL_HOURLY_RULE`

EventBridge drives the **batch nature** of the platform:

- **Rule name**: `SBW_ETL_HOURLY_RULE`  
- **Schedule**: `rate(1 hour)`  
- **Target**: `SBW_Lamda_ETL`  

Whenever the rule triggers:

1. ETL Lambda runs inside the private subnet.  
2. Reads new raw events from S3 via the Gateway Endpoint.  
3. Loads processed data into `clickstream_dw`.  

You can also trigger the ETL Lambda **manually** (from the Lambda console) for ad-hoc backfills or testing.

---

## 5.4.6 Security Groups & Connectivity Summary

- `sg_Lambda_ETL`:
  - Outbound to S3 endpoint and `sg_analytics_ShinyDWH:5432`.  

- `sg_analytics_ShinyDWH`:
  - Inbound `5432/tcp` from `sg_Lambda_ETL`.  
  - Inbound `3838/tcp` for Shiny (accessible only via SSM port forwarding).  

Because there is **no route from the private subnet to the Internet**, you gain:

- Smaller attack surface  
- More predictable egress patterns  
- Lower networking cost (no NAT Gateway)  

---

## 5.4.7 Hands-on Mapping (to LABs)

- Create VPC, subnets, route tables, endpoints, and EC2:
  - **LAB1 – Networking & EC2**  
- Build the ETL Lambda + EventBridge integration:
  - **LAB3 – EventBridge & Lambda ETL**  
