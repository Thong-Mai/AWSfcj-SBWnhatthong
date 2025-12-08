---
title: "Blog 3"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Bảo mật kết nối SAP HANA Cloud bằng AWS PrivateLink

by Sejun Kim, Arne Knoeller, Thuan Bui Thi, and Whye Loong Wong | vào ngày 26 tháng 8 năm 2025 | trong [AWS PrivateLink](https://aws.amazon.com/privatelink/), [SAP on AWS](https://aws.amazon.com/sap/), [Best Practices](https://aws.amazon.com/blogs/awsforsap/) | [Liên kết cố định](https://aws.amazon.com/blogs/awsforsap/secure-sap-hanacloud-connectivity-using-aws-privatelink/)

![anh](/images/C48-1.png)

## Giới thiệu

Trong bối cảnh doanh nghiệp hiện đại, các tổ chức cần những nền tảng cơ sở dữ liệu có thể xử lý đồng thời cả workload **transactional** và **analytical**, đồng thời vẫn đáp ứng chặt chẽ các yêu cầu về bảo mật và tuân thủ. **SAP HANA Cloud trên AWS**, một dịch vụ cốt lõi của SAP Business Technology Platform (SAP BTP), được thiết kế để đáp ứng các yêu cầu đó như một dịch vụ **database‑as‑a‑service** cloud‑native, được quản lý hoàn toàn.

Doanh nghiệp sử dụng SAP HANA Cloud để vận hành các ứng dụng lõi như ERP, CRM, HR, logistics, và để phục vụ các giải pháp phân tích như **SAP Analytics Cloud**, **SAP Datasphere** hay các ứng dụng tùy biến xây dựng bằng SAP Cloud Application Programming Model (CAP).  

Bài viết này trình bày cách tăng cường **bảo mật đường truyền mạng** đến SAP HANA Cloud bằng cách tích hợp với **AWS PrivateLink**, giúp lưu lượng truy cập đi qua kết nối private trong mạng AWS thay vì ra internet công cộng.

## Tại sao nên dùng SAP HANA Cloud trên AWS?

Các instance SAP HANA Cloud trên AWS chạy trên hạ tầng **Amazon EC2 dùng AWS Graviton**. So với các instance x86 tương đương, Graviton mang lại tỷ lệ **price–performance** tốt hơn cho analytical workload, giảm chi phí compute và mức tiêu thụ năng lượng, từ đó giúp giảm **carbon footprint** của các deployment SAP HANA Cloud. Khách hàng chạy SAP HANA Cloud trên AWS sẽ hưởng lợi trực tiếp từ cải thiện hiệu năng và yếu tố bền vững này.

![anh](/images/C48-2.png)

## Kết nối tới SAP HANA Cloud trên AWS

Theo mặc định, ứng dụng kết nối tới SAP HANA Cloud thông qua một **endpoint công khai trên internet**. Client từ on‑premises hoặc các cloud khác gửi lưu lượng qua internet, được bảo vệ bằng TLS, để truy cập cơ sở dữ liệu.

Tuy nhiên, khách hàng trong các ngành chịu quy định nghiêm ngặt (tài chính, y tế, khu vực công, v.v.) thường có chính sách **không cho phép** truy cập trực tiếp qua public endpoint, kể cả khi đã mã hóa. Các tổ chức này thường yêu cầu:

- Đường truyền mạng chỉ dùng **dải IP private** (RFC 1918).  
- Giảm tối đa bề mặt tấn công, không có public database endpoint.  
- Phân tách rõ ràng giữa traffic ứng dụng và traffic quản trị.

Để đáp ứng các yêu cầu đó, SAP và AWS hỗ trợ kết nối SAP HANA Cloud thông qua **AWS PrivateLink**, giúp lưu lượng luôn nằm trên backbone AWS và expose database vào VPC của bạn thông qua **interface endpoint private**.

![anh](/images/C48-3.png)

## AWS PrivateLink là gì?

**AWS PrivateLink** cung cấp kết nối private giữa các VPC, dịch vụ AWS và dịch vụ đối tác – bao gồm SAP HANA Cloud – mà không để traffic đi ra internet công cộng.

Một số đặc điểm chính:

- Lưu lượng đi qua **interface VPC endpoint**, tức các elastic network interface có địa chỉ IP private trong subnet của VPC.  
- Bạn có thể gắn **security group** và **endpoint policy** để kiểm soát tài nguyên nào được phép truy cập endpoint.  
- Không cần VPC peering hay AWS Transit Gateway, và cũng không phải đồng bộ dải CIDR giữa VPC consumer và provider.  
- Tất cả gói tin đi trên mạng toàn cầu của AWS, giúp cải thiện bảo mật và độ trễ.

Khi SAP HANA Cloud được publish như một endpoint service và VPC của bạn kết nối tới nó thông qua interface endpoint, các client bên trong VPC (và các môi trường kết nối như on‑premises qua AWS Direct Connect hoặc VPN) có thể truy cập SAP HANA Cloud bằng **kết nối IP private**.

![anh](/images/C48-4.png)

## Các use case tiêu biểu với SAP HANA Cloud và AWS PrivateLink

Dưới đây là hai kịch bản phổ biến nơi việc tích hợp PrivateLink mang lại giá trị lớn.

### Use case 1 – Bảo mật truy cập quản trị SAP HANA Cloud

Database administrator và developer thường cần quyền truy cập mở rộng để thực hiện thay đổi schema, tuning và bảo trì SAP HANA Cloud. Nếu cho phép loại kết nối nhiều đặc quyền này đi qua public endpoint, rủi ro bảo mật sẽ tăng lên đáng kể.

Với AWS PrivateLink:

- SAP HANA Cloud chỉ có thể được truy cập từ **các VPC hoặc mạng doanh nghiệp được phê duyệt**.  
- Các công cụ quản trị như **SAP HANA Cockpit** hoặc SQL client kết nối thông qua **interface endpoint private**.  
- Việc truy cập được kiểm soát bằng **security group**, **network ACL** và các cơ chế nhận diện như SSO hoặc multi‑factor authentication.

Mẫu kiến trúc này cung cấp mức bảo mật cấp doanh nghiệp cho traffic của admin và developer trong khi vẫn giữ nguyên đường đi của traffic ứng dụng thông thường.

![anh](/images/C48-5.png)

### Use case 2 – Tích hợp data platform trên AWS một cách an toàn

Nhiều khách hàng xây dựng data platform trên AWS và cần tích hợp **dữ liệu SAP** từ SAP HANA Cloud vào stack phân tích của họ. Các pattern thường gặp:

- Dùng **AWS Glue** để trích xuất dữ liệu từ SAP HANA Cloud và load vào **Amazon S3** hoặc các dịch vụ downstream như Amazon Redshift.  
- Dùng **Amazon Athena** với SAP HANA connector để chạy federated query trực tiếp lên SAP HANA Cloud từ các công cụ phân tích trên AWS.

Trong cả hai pattern, AWS PrivateLink đảm bảo rằng:

- Dữ liệu di chuyển giữa các dịch vụ AWS và SAP HANA Cloud qua **đường truyền private**.  
- Không có public IP hay internet gateway nào nằm trên đường đi của dữ liệu.  
- Các yêu cầu bảo mật và tuân thủ cho dữ liệu nghiệp vụ nhạy cảm được đáp ứng dễ dàng hơn.

![anh](/images/C48-6.png)

## Thiết lập SAP HANA Cloud với AWS PrivateLink – mức tổng quan

Bài blog gốc minh họa một kịch bản cấu hình cho use case 1 (truy cập quản trị an toàn). Ở mức high‑level, các bước như sau:

1. **Bật AWS PrivateLink cho instance SAP HANA Cloud** trong SAP HANA Cloud Central và lấy **endpoint service ID**.  
2. **Tạo interface VPC endpoint** trong AWS account của bạn, trỏ tới SAP HANA Cloud PrivateLink service.  
3. **Thêm VPC endpoint ID vào allow list** trong cấu hình instance SAP HANA Cloud.  
4. **Tạo private hosted zone trong Amazon Route 53** và tạo bản ghi DNS ánh xạ hostname của SAP HANA Cloud tới DNS name của PrivateLink endpoint.  
5. **Kiểm tra private routing** từ một Amazon EC2 instance bên trong VPC để đảm bảo traffic HANA đi qua endpoint.  
6. (Tuỳ chọn, cho kịch bản hybrid) **Cấu hình Route 53 Resolver inbound endpoint** để DNS on‑premises có thể resolve private hostname của HANA thông qua VPC của bạn.

Bài viết gốc cũng cung cấp screenshot minh họa từng bước trong SAP HANA Cloud Central và AWS Management Console.

### Bước 1 – Bật AWS PrivateLink trong SAP HANA Cloud

Trong SAP HANA Cloud Central:

- Chọn instance SAP HANA Cloud.  
- Mở **View Configuration** và chuyển sang tab **Connections**.  
- Bật tùy chọn hiển thị **AWS PrivateLink endpoint IDs**.  
- Giới hạn Allowed connections để traffic phải xuất phát từ Cloud Foundry trong BTP region và/hoặc các dải IP cụ thể.

Thiết lập này tạo ra một hoặc nhiều **PrivateLink endpoint service ID** mà bạn sẽ dùng khi tạo VPC endpoint.

![anh](/images/C48-7.png)

### Bước 2 – Tạo interface VPC endpoint

Trong AWS Management Console, ở phần **Amazon VPC**:

1. Chọn **Endpoints** → **Create endpoint**.  
2. Chọn loại *PrivateLink‑ready partner services*.  
3. Dán **service name** tương ứng với SAP HANA Cloud PrivateLink endpoint service và bấm verify.  
4. Chọn **VPC**, **Availability Zone** và **subnet** nơi bạn muốn đặt endpoint.  
5. Gán **security group** cho phép inbound HTTPS (TCP 443) từ các dải CIDR nơi admin hoặc ứng dụng của bạn đang chạy.

Khi endpoint đạt trạng thái **Available**, hãy ghi lại:

- **Endpoint ID** – dùng để thêm vào allow list trong SAP HANA Cloud.  
- **DNS name** – sẽ cấu hình trong Route 53.  
- **Private IP address** – hữu ích cho việc kiểm tra và troubleshooting mạng.

![anh](/images/C48-8.png)

### Bước 3 – Thêm VPC endpoint vào allow list của SAP HANA Cloud

Quay lại SAP HANA Cloud Central, thêm VPC endpoint ID vào danh sách **Allowed endpoints** của instance. Sau khi thay đổi được áp dụng, chỉ những lưu lượng đi qua các PrivateLink endpoint đã được phê duyệt mới truy cập được database thông qua kết nối private.

Các bước tiếp theo (cấu hình DNS Route 53, kiểm thử và thiết lập DNS forwarding cho môi trường hybrid) tuân theo các pattern quen thuộc của AWS và đảm bảo ứng dụng client sử dụng private hostname thay vì public endpoint mặc định.

## Cân nhắc về chi phí

Khi bật PrivateLink cho SAP HANA Cloud:

- **Không có thêm chi phí phía SAP BTP** cho việc kích hoạt AWS PrivateLink; SAP là bên quản lý Network Load Balancer phía sau dịch vụ.  
- Bạn chỉ phải trả cho các **tài nguyên AWS trong account của mình**, như interface VPC endpoint và chi phí xử lý dữ liệu cho traffic PrivateLink.

Bạn có thể dùng AWS Pricing Calculator để so sánh kiến trúc **có và không có high availability** (ví dụ endpoint ở nhiều Availability Zone) nhằm ước tính chi phí vận hành.

## Tiếp theo nên làm gì?

Để tìm hiểu sâu hơn:

- Xem tài liệu AWS **“What is AWS PrivateLink?”** và hướng dẫn **“Connect your VPC to services using AWS PrivateLink.”**  
- Tham khảo tài liệu SAP HANA Cloud và **SAP HANA Cloud Database Administration Guide** để nắm chi tiết cấu hình PrivateLink và networking.  
- Thử lab workshop đi kèm để thực hành cấu hình SAP HANA Cloud với AWS PrivateLink trong môi trường sandbox.

Bằng cách kết hợp SAP HANA Cloud với AWS PrivateLink, bạn có thể xây dựng các data platform bảo mật và hiệu năng cao, giữ lưu lượng database quan trọng trong mạng private nhưng vẫn tận dụng được sự linh hoạt của cloud.
