---
title: "Sự kiện 1"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Địa điểm & Thời gian

- **Địa điểm:** Bitexco Financial Tower  
- **Thời gian:** 15/11/2025  

---

## Mục tiêu sự kiện

- Mang đến cái nhìn **thực tế** về các năng lực AI/ML/GenAI trên AWS, trọng tâm là **Amazon Bedrock**.
- Giúp người tham dự biết cách **lựa chọn Foundation Model** (Claude, Llama, Titan, …) theo từng nhóm bài toán cụ thể.
- Giới thiệu các kỹ thuật **prompt engineering** (few-shot, prompt có cấu trúc, pattern reasoning) để cải thiện chất lượng câu trả lời.
- Trình bày kiến trúc **RAG** và cách tận dụng **Knowledge Base** để tăng độ chính xác về mặt dữ liệu.
- Làm quen với **Bedrock Agents** cho các quy trình nhiều bước, có tích hợp thêm tool/API.
- Nhấn mạnh cách áp dụng **Guardrails** để kiểm soát nội dung, policy và đảm bảo GenAI “an toàn”.
- Xem demo một **chatbot GenAI** hoàn chỉnh được xây dựng end-to-end bằng Amazon Bedrock.

---

## Diễn giả

- **Hoàng Kha**  
- **Hữu Nghị**  
- **Hoàng Anh**

---

## Key Highlights

### 1. Chào mừng & Giới thiệu

- **Check-in & networking:**  
  Người tham dự làm thủ tục đăng ký, gặp gỡ và trao đổi nhanh với các anh chị/em khác đang làm việc với AWS.
- **Giới thiệu agenda:**  
  Đi qua lịch trình buổi sáng, mục tiêu học tập và những gì người tham gia có thể mang về sau workshop.
- **Ice-breaker:**  
  Một hoạt động khởi động nhẹ để mọi người chia sẻ kỳ vọng, làm không khí bớt ngại ngùng.
- **Giới thiệu diễn giả:**  
  Ba diễn giả chia sẻ ngắn về kinh nghiệm và góc nhìn của mình trong mảng AI/ML/GenAI trên AWS.

### 2. Generative AI với Amazon Bedrock

- **Foundation Models:**  
  Giải thích Claude, Llama, Titan là gì, khác nhau ở đâu và cách chọn model theo nhu cầu: chatbot hỗ trợ nội bộ, tóm tắt văn bản, phân loại nội dung, tác vụ reasoning…
- **Prompt engineering:**  
  Thực hành các kiểu prompt:
  - Prompt có cấu trúc (context → yêu cầu → ràng buộc).
  - Few-shot (cho ví dụ mẫu).
  - Cách đặt câu hỏi để model “hiểu đúng ý” hơn.
- **Reasoning patterns:**  
  Minh hoạ cách yêu cầu model suy nghĩ theo từng bước, hoặc tách bài toán lớn thành các sub-task, để kết quả ổn định hơn trong những tác vụ khó.

### 3. RAG, Agents & Guardrails

- **RAG (Retrieval-Augmented Generation):**  
  - Tách riêng bước truy vấn dữ liệu (retrieve) từ kho tri thức.  
  - Thêm phần context phù hợp rồi mới cho model sinh câu trả lời.  
  - Nhấn mạnh: muốn trả lời đúng kiến thức nội bộ thì retrieval rất quan trọng.
- **Bedrock Agents:**  
  - Tạo “tác nhân” có thể gọi thêm tool (API, tìm kiếm, truy xuất dữ liệu).  
  - Hữu ích cho các workflow nhiều bước như trợ lý nghiệp vụ, automation các quy trình lặp lại.
- **Guardrails:**  
  - Cấu hình các lớp bảo vệ để lọc content nhạy cảm, hạn chế chủ đề không mong muốn.  
  - Đảm bảo ứng dụng GenAI tuân thủ chính sách doanh nghiệp và quy định pháp lý.

### 4. Live Demo

- Xem walkthrough xây dựng **chatbot GenAI** trên Bedrock:
  - Chọn model.
  - Thiết kế prompt.
  - Kết nối với Knowledge Base theo kiểu RAG.
  - Thêm Guardrails cơ bản.
  - Chạy thử kịch bản hỏi–đáp giống với môi trường doanh nghiệp thực tế.

---

## Key Takeaways

### 1. Tư duy thiết kế (Design Mindset)

- **Bắt đầu từ use case:**  
  Đầu tiên phải rõ bài toán (FAQ bot, trợ lý tra cứu tài liệu, automation quy trình…) rồi mới ngược lại chọn model/kiến trúc.
- **Grounding quan trọng:**  
  Với các hệ thống dùng kiến thức nội bộ, độ chính xác dựa rất nhiều vào cách retrieve & chọn context, chứ không phải chỉ “viết prompt thật dài”.
- **Safety không phải chuyện thêm sau:**  
  Guardrails và policy nên được thiết kế ngay từ đầu, để tránh phải “vá lỗi” sau khi đã đưa vào production.

### 2. Kiến trúc kỹ thuật

- **Lựa chọn model:**  
  Cân bằng giữa chất lượng, chi phí, latency và loại tác vụ (chat, reasoning, summarization,…).
- **Prompt engineering:**  
  Dùng prompt có cấu trúc + ví dụ cụ thể để giảm mơ hồ, dễ tái sử dụng cho nhiều tình huống.
- **RAG workflow:**  
  - Tìm tài liệu phù hợp.  
  - Ghép context.  
  - Sinh câu trả lời.  
  - (Tuỳ chọn) log lại nguồn/context để truy vết.
- **Agents & tools:**  
  Khi quy trình cần nhiều bước hoặc phải gọi API/dịch vụ khác, Agents giúp tự động hoá thay vì viết tay hết logic.
- **Guardrails:**  
  Là lớp “chốt chặn” để giảm rủi ro nội dung, đặc biệt quan trọng khi app GenAI phục vụ end-user.

### 3. Ứng dụng vào công việc

- **Prototype chatbot Bedrock nhỏ:**  
  Bắt đầu với một kho kiến thức hạn chế (FAQ, policy nội bộ) để kiểm thử chất lượng câu trả lời.
- **Xây “thư viện prompt”:**  
  Lưu lại các mẫu prompt tốt cho những tác vụ phổ biến: tóm tắt, phân loại, Q&A, trích xuất thông tin.
- **Từng bước thêm RAG:**  
  Khi yêu cầu về độ chính xác cao, chuyển từ “chỉ prompt” sang RAG để câu trả lời bám sát dữ liệu thật.
- **Dùng Agents có chủ đích:**  
  Chỉ đưa Agents vào khi workflow thực sự cần nhiều bước & tool, tránh phức tạp không cần thiết.
- **Thiết lập guardrails & logging:**  
  Đặt policy, log prompt/response, theo dõi để cải tiến liên tục.

---

## Trải nghiệm sự kiện

Tham gia “AI/ML/GenAI on AWS” giúp mình:

- Hình dung rõ cách **Amazon Bedrock** có thể dùng để xây dựng ứng dụng GenAI chạy được trong môi trường production, chứ không chỉ là demo.
- Đi từ nền tảng (chọn model, viết prompt) đến các pattern nâng cao như:
  - RAG,
  - Agents,
  - Guardrails.
- Phần live demo giúp “nối dây” các khái niệm lại thành một **workflow chatbot end-to-end**, có thể tùy biến cho hệ thống tri thức nội bộ trong doanh nghiệp.

---

## Một số hình ảnh sự 

![anh](/images/event1.png) 