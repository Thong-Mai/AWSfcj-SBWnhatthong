---
title: "5.3. Implementing Clickstream Ingestion"
weight: 53
---

# Implementing Clickstream Ingestion

Phần này giải thích cách các sự kiện clickstream được capture trên browser, gửi lên AWS và lưu dưới dạng JSON thô trong S3.

> Chi tiết thao tác từng bước nằm trong **LAB2 – S3, Lambda Ingest & API Gateway** và một phần **LAB4 – Frontend (Amplify) & Clickstream SDK**.

---

## 5.3.1 Tổng quan luồng Ingestion

Luồng dữ liệu ở mức high-level:

1. Người dùng tương tác với frontend Next.js (`ClickSteam.NextJS`) trên Amplify.  
2. Frontend JavaScript gom metadata clickstream vào một payload JSON.  
3. Browser gửi request `POST` tới `clickstream-http-api` tại route `POST /clickstream`.  
4. API Gateway forward request tới `clickstream-lambda-ingest`.  
5. Lambda Ingest validate, enrich event rồi ghi file JSON vào:
   - `s3://clickstream-s3-ingest/events/YYYY/MM/DD/event-<uuid>.json`  

Thiết kế này giúp lớp ingestion **stateless** và **append-only**, rất phù hợp cho ETL batch phía sau.

---

## 5.3.2 Thiết kế S3 Bucket

Có 2 bucket liên quan:

1. **Asset Bucket** – `clickstream-s3-sbw`  
   - Chứa website assets: ảnh sản phẩm, static files.  
   - Không dùng để lưu clickstream.

2. **RAW Clickstream Bucket** – `clickstream-s3-ingest`  
   - Chỉ lưu JSON clickstream thô.  
   - Partition theo ngày:
     - `events/YYYY/MM/DD/`  
   - Tên file:
     - `event-<uuid>.json`  

Partition theo ngày giúp ETL batch đơn giản hơn (ví dụ xử lý “folder hôm nay” hoặc “N giờ gần nhất”).

---

## 5.3.3 Thiết kế Lambda Ingest – `clickstream-lambda-ingest`

### Nhiệm vụ

Function `clickstream-lambda-ingest`:

- Parse JSON payload nhận được từ API Gateway.  
- Bổ sung metadata nếu thiếu:
  - `event_timestamp` (server tạo)  
  - `client_id`, `session_id` (từ header hoặc body)  
  - `user_login_state`, `identity_source` (từ token / cookie nếu có)  
- Ghi event (cộng thêm metadata) vào S3.

### Quyền IAM

Execution role cần có:

- `s3:PutObject` trên:
  - `arn:aws:s3:::clickstream-s3-ingest/events/*`  
- Quyền CloudWatch Logs để ghi log.

Function này **không** cần quyền đọc (read) S3.

---

## 5.3.4 API Gateway HTTP API – `clickstream-http-api`

HTTP API cung cấp endpoint HTTPS public cho ingestion:

- Route:
  - `POST /clickstream` → Lambda `clickstream-lambda-ingest`  

Khuyến nghị cấu hình:

- Bật **CORS** để frontend Amplify có thể gọi từ domain của nó.  
- Bật **access logs** đẩy lên CloudWatch Log Group để debug.  
- (Tùy chọn) Thêm **API key** hoặc **Cognito authorizer** nếu muốn giới hạn quyền ghi events.

---

## 5.3.5 Logic Clickstream SDK ở Frontend

Frontend không nhất thiết cần một SDK phức tạp, nhưng nên:

- Duy trì một **`client_id`** trong `localStorage` (giữ lâu dài giữa các session).  
- Sinh một **`session_id`** cho mỗi phiên (session).  
- Gắn các field người dùng (optional):
  - `user_id`, `user_login_state`, `identity_source` (ví dụ từ Cognito).  
- Gắn context sản phẩm trên trang product:
  - `context_product_id`, `context_product_name`, `context_product_category`, `context_product_brand`,  
  - `context_product_price`, `context_product_discount_price`, `context_product_url_path`  

Với mỗi tương tác quan trọng (page view, product view, add to cart, checkout):

- Tạo payload JSON chứa các field ở trên.  
- Gửi request `fetch()`:

```ts
await fetch("https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/clickstream", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(eventPayload),
});
```

Payload này về sau sẽ được ETL chuyển thành một dòng dữ liệu trong `clickstream_dw.public.clickstream_events`.

---

## 5.3.6 Kiểm thử & xác nhận

Để kiểm tra ingestion:

1. Dùng UI Amplify:
   - Truy cập một vài trang sản phẩm  
   - Thêm sản phẩm vào giỏ hàng  
2. Kiểm tra bucket `clickstream-s3-ingest`:
   - Vào thư mục `events/YYYY/MM/DD/`  
   - Xác nhận có các file `event-<uuid>.json` mới.  
3. Mở một file JSON:
   - Kiểm tra metadata (timestamp, session ID, context sản phẩm) đã đầy đủ.  
4. Xem log:
   - Access logs của API Gateway  
   - Logs của Lambda function  

Nếu mọi thứ OK, lớp **ingestion** đã sẵn sàng cho ETL ở phần 5.4 và LAB3.
