---
title: "5.3. Implementing Clickstream Ingestion"
weight: 53
---

# Implementing Clickstream Ingestion

This section explains how clickstream events are captured in the browser, sent to AWS, and stored as raw JSON in S3.

> Hands-on details for this section are in **LAB2 – S3, Lambda Ingest & API Gateway** and partly **LAB4 – Frontend (Amplify) & Clickstream SDK**.

---

## 5.3.1 Ingestion Flow Overview

High-level data flow:

1. User interacts with the Next.js frontend (`ClickSteam.NextJS`) hosted on Amplify.  
2. Frontend JavaScript bundles clickstream metadata into a JSON payload.  
3. Browser sends `POST` requests to `clickstream-http-api` at `POST /clickstream`.  
4. API Gateway forwards the request to `clickstream-lambda-ingest`.  
5. Lambda Ingest validates and enriches the event, then writes a JSON file to:
   - `s3://clickstream-s3-ingest/events/YYYY/MM/DD/event-<uuid>.json`  

This design keeps ingestion **stateless** and **append-only**, which is ideal for downstream batch ETL.

---

## 5.3.2 S3 Bucket Design

Two buckets are relevant:

1. **Asset Bucket** – `clickstream-s3-sbw`  
   - Stores website assets: product images, static files.  
   - Not used for clickstream events.

2. **RAW Clickstream Bucket** – `clickstream-s3-ingest`  
   - Stores only raw clickstream JSON events.  
   - Partitioned by date:
     - `events/YYYY/MM/DD/`  
   - File naming:
     - `event-<uuid>.json`  

Partitioning by date makes batch ETL easier (e.g., process “today’s folder” or “last N hours”).

---

## 5.3.3 Lambda Ingest Design – `clickstream-lambda-ingest`

### Responsibilities

The `clickstream-lambda-ingest` function:

- Parses incoming JSON payloads from API Gateway.  
- Attaches metadata if missing:
  - `event_timestamp` (server side)  
  - `client_id`, `session_id` (from headers or body)  
  - `user_login_state`, `identity_source` (from tokens / cookies where available)  
- Writes the event as-is (plus metadata) to S3.

### IAM Permissions

The execution role should allow:

- `s3:PutObject` on:
  - `arn:aws:s3:::clickstream-s3-ingest/events/*`  
- CloudWatch Logs APIs to write logs.

No read permissions are needed for this function.

---

## 5.3.4 API Gateway HTTP API – `clickstream-http-api`

The HTTP API provides a public HTTPS endpoint for ingestion:

- Route:
  - `POST /clickstream` → Lambda `clickstream-lambda-ingest`  

Recommended options:

- Enable **CORS** so the Amplify frontend can call it from its own domain.  
- Enable **access logs** to a CloudWatch log group for debugging.  
- (Optional) Attach an **API key** or **Cognito authorizer** if you want to restrict ingestion.

---

## 5.3.5 Frontend Clickstream SDK (Logic)

The frontend doesn’t need a full SDK, but should:

- Maintain a **`client_id`** in `localStorage` (persistent across sessions).  
- Generate per-session **`session_id`**.  
- Attach optional user fields:
  - `user_id`, `user_login_state`, `identity_source` (e.g., Cognito).  
- Attach product context on product pages:
  - `context_product_id`, `context_product_name`, `context_product_category`, `context_product_brand`,  
  - `context_product_price`, `context_product_discount_price`, `context_product_url_path`  

On every relevant interaction (page view, product view, add to cart, checkout):

- Build a JSON payload including the fields above.  
- Call `fetch()`:

```ts
await fetch("https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/clickstream", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(eventPayload),
});
```

The payload will eventually be turned into a row in `clickstream_dw.public.clickstream_events`.

---

## 5.3.6 Testing & Validation

To validate ingestion:

1. Use the Amplify app UI:
   - Browse a few product pages  
   - Add items to cart  
2. Check S3 bucket `clickstream-s3-ingest`:
   - Navigate to `events/YYYY/MM/DD/`  
   - Confirm new `event-<uuid>.json` files appear.  
3. Inspect one JSON file:
   - Verify that metadata (timestamps, session IDs, product context) is present.  
4. Review logs:
   - API Gateway access logs  
   - Lambda function logs  

If all looks good, the **ingestion layer is ready** for ETL in section 5.4 and LAB3.
