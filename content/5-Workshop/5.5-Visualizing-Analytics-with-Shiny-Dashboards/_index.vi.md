---
title: "Visualizing Analytics with Shiny Dashboards"
weight: 55
---

## 5.5.1 R Shiny trên EC2 Data Warehouse

R Shiny được cài trên `SBW_EC2_ShinyDWH` và chạy cùng với PostgreSQL:

- Lắng nghe trên port `3838`  
- Serve ứng dụng `sbw_dashboard`  
- Kết nối tới `clickstream_dw` qua `localhost:5432`  

Stack điển hình:

- R + các package:
  - `shiny`, `shinydashboard`  
  - `DBI`, `RPostgres`  
  - `dplyr`, `ggplot2`, …  

Shiny app được deploy tại:

- `/srv/shiny-server/sbw_dashboard/`  
  - `app.R` hoặc `ui.R` + `server.R`  

---

## 5.5.2 Nội dung Dashboard

Dashboard có thể được chia thành nhiều tab, ví dụ:

1. **Overview**
   - Tổng số events trong khoảng thời gian chọn  
   - Số user, số session unique  
   - Top loại event  

2. **Product Analytics**
   - Top sản phẩm theo số lượt xem (`event_name = 'product_view'`)  
   - Conversion rate từ view → add-to-cart → checkout theo từng sản phẩm  

3. **Funnel Analysis**
   - Các bước:
     - `page_view` → `product_view` → `add_to_cart` → `checkout`  
   - Tỷ lệ rớt (drop-off) ở mỗi bước  

4. **Time-based Trends**
   - Số events theo giờ / theo ngày  
   - Khung giờ có hoạt động cao nhất  

Tất cả biểu đồ này được build dựa trên các truy vấn tới `clickstream_dw.public.clickstream_events` và các bảng aggregate bổ sung mà bạn xây thêm.

---

## 5.5.3 Truy cập an toàn qua SSM Session Manager

Vì `SBW_EC2_ShinyDWH` **không có public IP** và không mở cổng SSH, nên truy cập được thực hiện qua **AWS Systems Manager Session Manager**.

### Các bước

1. Mở **Session Manager** trong AWS Console.  
2. Start một session tới instance `SBW_EC2_ShinyDWH`.  
3. Thiết lập **port forwarding**:
   - `localPort = 3838`  
   - `portNumber = 3838`  

4. Trên máy local, mở trình duyệt và truy cập:
   - `http://localhost:3838/sbw_dashboard`  

Lúc này bạn có thể duyệt dashboard như chạy trên máy local, dù thực tế app nằm trong private subnet.

---

## 5.5.4 Xác nhận luồng dữ liệu End-to-End

Một quy trình test điển hình:

1. **Sinh hành vi người dùng**:
   - Mở URL Amplify: `https://main.d2q6im0b1720uc.amplifyapp.com/`  
   - Duyệt danh sách sản phẩm, trang chi tiết sản phẩm  
   - Thêm hàng vào giỏ, tiến hành checkout  

2. **Kiểm tra S3**:
   - Vào bucket `clickstream-s3-ingest/events/YYYY/MM/DD/`  
   - Xác nhận có các file `event-<uuid>.json` mới  

3. **Chạy ETL hoặc chờ EventBridge**:
   - Gọi `SBW_Lamda_ETL` thủ công hoặc đợi `SBW_ETL_HOURLY_RULE`  
   - Kiểm tra CloudWatch logs để chắc chắn rằng dữ liệu đã được load  

4. **Query DW**:
   - Từ EC2 hoặc client DB:
     - `SELECT * FROM public.clickstream_events ORDER BY event_timestamp DESC LIMIT 50;`  

5. **Refresh Shiny dashboard**:
   - Xác nhận số liệu / biểu đồ phản ánh đúng các tương tác vừa sinh ra  


