---
title: "Blog 1"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Các dịch vụ AWS mở rộng quy mô lên tầm cao mới cho Prime Day 2025: các chỉ số và cột mốc quan trọng

by Channy Yun (윤석찬) | vào ngày 26 tháng 8 năm 2025 | trong [Announcements](https://aws.amazon.com/blogs/aws/category/news/announcements/), [Events](https://aws.amazon.com/blogs/aws/category/news/events/), [News](https://aws.amazon.com/blogs/aws/category/news/) | [Liên kết cố định](https://aws.amazon.com/blogs/aws/aws-services-scale-to-new-heights-for-prime-day-2025-key-metrics-and-milestones/)

![anh](/images/C46-1.png)

Amazon Prime Day 2025 là sự kiện mua sắm Amazon Prime Day lớn nhất từ trước đến nay, lập kỷ lục cả về khối lượng bán hàng và tổng số mặt hàng được bán ra trong sự kiện kéo dài 4 ngày. Các thành viên Prime đã tiết kiệm được hàng tỷ đô la khi mua sắm hàng triệu ưu đãi của Amazon trong sự kiện này.  

Năm nay đánh dấu một sự chuyển đổi đáng kể trong trải nghiệm Prime Day thông qua những tiến bộ trong các sản phẩm **generative AI** từ Amazon và AWS. Khách hàng đã sử dụng **Alexa+** — trợ lý cá nhân thế hệ tiếp theo của Amazon hiện đã có sẵn trong chương trình truy cập sớm cho hàng triệu khách hàng — cùng với trợ lý mua sắm do AI hỗ trợ **Rufus** và **AI Shopping Guides**.  

Những tính năng này, được xây dựng trên 15 năm kinh nghiệm đổi mới đám mây và chuyên môn **machine learning (ML)** từ AWS, kết hợp với kinh nghiệm bán lẻ và tiêu dùng sâu sắc từ Amazon, đã giúp khách hàng nhanh chóng khám phá các ưu đãi và nhận thông tin sản phẩm, bổ sung cho dịch vụ giao hàng nhanh và miễn phí mà các thành viên Prime được hưởng quanh năm.  

Là một phần trong truyền thống hàng năm của chúng tôi để kể cho bạn nghe về cách AWS đã cung cấp năng lượng cho Prime Day để đạt được doanh số kỷ lục, tôi muốn chia sẻ các dịch vụ và những chỉ số đứng đầu bảng xếp hạng từ AWS đã giúp trải nghiệm mua sắm tuyệt vời của bạn trở nên khả thi.

![anh](/images/C46-2.png)

## Prime Day 2025 – tất cả các con số

Trong những tuần trước các sự kiện mua sắm lớn như Prime Day, các trung tâm hoàn thiện đơn hàng và trạm giao hàng của Amazon làm việc để chuẩn bị sẵn sàng và đảm bảo hoạt động diễn ra hiệu quả và an toàn. Ví dụ, hệ thống lưu trữ và truy xuất tự động (**ASRS**) của Amazon vận hành một đội robot di động công nghiệp toàn cầu di chuyển hàng hóa xung quanh các trung tâm hoàn thiện đơn hàng của Amazon.  

**AWS Outposts**, một dịch vụ được quản lý hoàn toàn giúp mở rộng trải nghiệm AWS tại chỗ, cung cấp năng lượng cho các ứng dụng phần mềm quản lý việc chỉ huy và điều khiển Amazon ASRS và hỗ trợ giao hàng trong ngày và ngày hôm sau thông qua việc xử lý các lệnh robot quan trọng với độ trễ thấp.  

Trong Prime Day 2025, AWS Outposts tại một trong những trung tâm hoàn thiện đơn hàng lớn nhất của Amazon đã gửi hơn **524 triệu** lệnh đến hơn **7.000** robot, đạt khối lượng cao nhất là **8 triệu lệnh mỗi giờ** — tăng **160%** so với Prime Day 2024.

![anh](/images/C46-3.png)

Dưới đây là một số chỉ số thú vị và đáng kinh ngạc hơn:

- **Amazon Elastic Compute Cloud (Amazon EC2)** – Trong Prime Day 2025, **AWS Graviton**, một họ bộ xử lý được thiết kế để mang lại hiệu suất giá tốt nhất cho các workload đám mây chạy trong Amazon EC2, đã cung cấp năng lượng cho hơn **40%** khối lượng tính toán Amazon EC2 được Amazon.com sử dụng. Amazon cũng đã triển khai hơn **87.000** chip **AWS Inferentia** và **AWS Trainium** – các chip silicon tùy chỉnh cho **deep learning** và đào tạo cũng như suy luận **generative AI** – để cung cấp năng lượng cho **Amazon Rufus** trong Prime Day.

- **Amazon SageMaker AI** – Amazon SageMaker AI, một dịch vụ được quản lý hoàn toàn quy tụ một bộ công cụ rộng lớn để cho phép **machine learning (ML)** hiệu suất cao, chi phí thấp, đã xử lý hơn **626 tỷ** yêu cầu suy luận trong Prime Day 2025.

- **Amazon Elastic Container Service (Amazon ECS) và AWS Fargate** – Amazon ECS là một dịch vụ điều phối container được quản lý hoàn toàn hoạt động liền mạch với **AWS Fargate**, một công cụ tính toán **serverless** cho các container. Trong Prime Day 2025, Amazon ECS đã khởi chạy trung bình **18,4 triệu** tác vụ mỗi ngày trên AWS Fargate, tăng **77%** so với mức trung bình của Prime Day năm trước.

- **AWS Fault Injection Service (AWS FIS)** – Chúng tôi đã chạy hơn **6.800** thử nghiệm AWS FIS — nhiều hơn **8 lần** so với năm 2024 — để kiểm tra khả năng phục hồi và đảm bảo Amazon.com vẫn có độ sẵn sàng cao vào Prime Day. Sự gia tăng đáng kể này có được là nhờ hai cải tiến: hỗ trợ mới của Amazon ECS cho các thử nghiệm **network fault injection** trên AWS Fargate, và việc tích hợp kiểm thử FIS vào các pipeline tích hợp liên tục và phân phối liên tục (**CI/CD**).

- **AWS Lambda** – AWS Lambda, một dịch vụ tính toán **serverless** cho phép bạn chạy mã mà không cần quản lý cơ sở hạ tầng, đã xử lý hơn **1,7 nghìn tỷ** lượt gọi mỗi ngày trong Prime Day 2025.

- **Amazon API Gateway** – Trong Prime Day 2025, Amazon API Gateway, một dịch vụ được quản lý hoàn toàn giúp dễ dàng tạo, duy trì và bảo mật các API ở mọi quy mô, đã xử lý hơn **1 nghìn tỷ** yêu cầu dịch vụ nội bộ — tăng **30%** yêu cầu trung bình mỗi ngày so với Prime Day 2024.

- **Amazon CloudFront** – Amazon CloudFront, một dịch vụ mạng phân phối nội dung (**CDN**) giúp phân phối nội dung một cách an toàn với độ trễ thấp và tốc độ truyền cao, đã phân phối hơn **3 nghìn tỷ** yêu cầu HTTP trong tuần lễ Prime Day toàn cầu năm 2025, tăng **43%** yêu cầu so với Prime Day 2024.

- **Amazon Elastic Block Store (Amazon EBS)** – Trong Prime Day 2025, Amazon EBS, dịch vụ lưu trữ khối hiệu suất cao của chúng tôi, đã đạt đỉnh **20,3 nghìn tỷ** hoạt động I/O, di chuyển tới khoảng **1 exabyte** dữ liệu mỗi ngày.

- **Amazon Aurora** – Vào Prime Day, Amazon Aurora, một hệ thống quản lý cơ sở dữ liệu quan hệ (**RDBMS**) được xây dựng cho hiệu suất và độ sẵn sàng cao ở quy mô toàn cầu cho **PostgreSQL**, **MySQL** và **DSQL**, đã xử lý **500 tỷ** giao dịch, lưu trữ **4.071 terabyte** dữ liệu và truyền **999 terabyte** dữ liệu.

- **Amazon DynamoDB** – Amazon DynamoDB, một cơ sở dữ liệu **NoSQL** phân tán, được quản lý hoàn toàn, **serverless**, cung cấp năng lượng cho nhiều hệ thống và tài sản có lưu lượng truy cập cao của Amazon bao gồm **Alexa**, các trang web Amazon.com và tất cả các trung tâm hoàn thiện đơn hàng của Amazon. Trong suốt Prime Day, các nguồn này đã thực hiện **hàng chục nghìn tỷ** lệnh gọi đến DynamoDB API. DynamoDB duy trì độ sẵn sàng cao trong khi cung cấp phản hồi ở mức mili giây một chữ số và đạt đỉnh **151 triệu yêu cầu mỗi giây**.

- **Amazon ElastiCache** – Trong Prime Day, Amazon ElastiCache, một dịch vụ **caching** được quản lý hoàn toàn cung cấp độ trễ micro giây, đã đạt đỉnh phục vụ hơn **1,5 triệu tỷ** yêu cầu hàng ngày và hơn **1,4 nghìn tỷ** yêu cầu trong một phút.

- **Amazon Kinesis Data Streams** – Amazon Kinesis Data Streams, một dịch vụ **data streaming serverless** được quản lý hoàn toàn, đã xử lý mức cao nhất là **807 triệu** bản ghi mỗi giây trong Prime Day 2025.

- **Amazon Simple Queue Service (Amazon SQS)** – Trong Prime Day 2025, Amazon SQS – một dịch vụ **message queuing** được quản lý hoàn toàn dành cho các microservice, hệ thống phân tán và ứng dụng serverless – đã lập kỷ lục lưu lượng truy cập cao nhất mới là **166 triệu** tin nhắn mỗi giây.

- **Amazon GuardDuty** – Trong Prime Day 2025, Amazon GuardDuty, một dịch vụ phát hiện mối đe dọa thông minh, đã giám sát trung bình **8,9 nghìn tỷ** sự kiện log mỗi giờ, tăng **48,9%** so với Prime Day năm ngoái.

- **AWS CloudTrail** – AWS CloudTrail, dịch vụ theo dõi hoạt động của người dùng và việc sử dụng API trên AWS, cũng như trong các môi trường **hybrid** và **multicloud**, đã xử lý hơn **2,5 nghìn tỷ** sự kiện trong Prime Day 2025, so với **976 tỷ** sự kiện vào năm 2024.

## Chuẩn bị để mở rộng quy mô

Nếu bạn đang chuẩn bị cho các sự kiện quan trọng đối với doanh nghiệp, các buổi ra mắt sản phẩm và di chuyển hệ thống tương tự, tôi khuyên bạn nên tận dụng chương trình mới được đổi tên của chúng tôi là **AWS Countdown** (trước đây gọi là **AWS Infrastructure Event Management**, hay **IEM**).  

Chương trình hỗ trợ toàn diện này giúp đánh giá sự sẵn sàng vận hành, xác định và giảm thiểu rủi ro, cũng như lập kế hoạch dung lượng, bằng cách sử dụng các playbook đã được chứng minh do các chuyên gia của AWS phát triển. Chúng tôi đã mở rộng để bao gồm: hỗ trợ triển khai **generative AI** để giúp bạn tự tin ra mắt và mở rộng quy mô các sáng kiến AI; hỗ trợ di chuyển và hiện đại hóa, bao gồm cả **hiện đại hóa mainframe**; và tối ưu hóa cơ sở hạ tầng cho các lĩnh vực chuyên biệt bao gồm hệ thống bầu cử, hoạt động bán lẻ, dịch vụ chăm sóc sức khỏe, và các sự kiện thể thao và game.  

Tôi mong chờ được xem những kỷ lục nào khác sẽ bị phá vỡ vào năm tới!

![anh](/images/C46-4.png)

— Channy

---

![anh](/images/C46-5.png)

### Channy Yun (윤석찬)

Channy là một Nhà truyền bá Công nghệ Cấp cao (**Principal Developer Advocate**) của AWS. Với niềm đam mê web mở và tâm hồn của một blogger, anh ấy yêu thích việc học hỏi và chia sẻ công nghệ do cộng đồng thúc đẩy.
