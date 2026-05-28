# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, phản hồi rất ổn định và nhất quán — gọi nhiều lần sẽ cho kết quả gần như giống hệt nhau, nội dung chính xác và có cấu trúc rõ ràng. Khi tăng lên 0.5 và 1.0, phản hồi bắt đầu đa dạng hơn, sử dụng từ ngữ phong phú hơn và đôi khi đề cập đến những sự thật ít phổ biến hơn. Ở mức 1.5, phản hồi trở nên rất sáng tạo nhưng đôi khi lan man, kém mạch lạc và có thể chứa thông tin không chính xác hoàn toàn.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature từ 0.0 đến 0.3 cho chatbot hỗ trợ khách hàng. Lý do là vì trong bối cảnh hỗ trợ khách hàng, độ chính xác và tính nhất quán của thông tin là ưu tiên hàng đầu. Temperature thấp đảm bảo chatbot đưa ra câu trả lời đáng tin cậy, không bịa đặt thông tin, và giữ cho trải nghiệm người dùng đồng nhất qua nhiều lần tương tác. Việc sáng tạo quá mức có thể dẫn đến thông tin sai lệch, gây mất niềm tin của khách hàng.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> - Tổng số token mỗi ngày: 10,000 × 3 × 350 = 10,500,000 token (10,500 K tokens)
> - Chi phí GPT-4o: 10,500 × $0.010 = **$105/ngày**
> - Chi phí GPT-4o-mini: 10,500 × $0.0006 = **$6.3/ngày**
> - Tỉ lệ: $105 / $6.3 ≈ **16.67 lần**
> - Kết luận: GPT-4o đắt hơn GPT-4o-mini khoảng **16.7 lần** cho cùng một khối lượng công việc.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> **GPT-4o xứng đáng:** Trong các ứng dụng yêu cầu suy luận phức tạp như phân tích tài liệu pháp lý, chẩn đoán y tế hỗ trợ, hoặc tóm tắt báo cáo nghiên cứu khoa học — nơi mà sai sót nhỏ có thể gây hậu quả nghiêm trọng. Chất lượng phản hồi vượt trội của GPT-4o biện minh cho chi phí cao hơn.
>
> **GPT-4o-mini phù hợp hơn:** Trong các tác vụ đơn giản, lặp lại với khối lượng lớn như phân loại email, trích xuất thông tin cơ bản từ biểu mẫu, tự động trả lời câu hỏi thường gặp (FAQ), hoặc sinh nội dung mẫu. Những tác vụ này không đòi hỏi khả năng suy luận sâu, nên GPT-4o-mini đáp ứng đủ tốt với chi phí thấp hơn rất nhiều.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng trong các ứng dụng chatbot tương tác thời gian thực, nơi người dùng đang chờ đợi phản hồi và cần cảm giác rằng hệ thống đang "suy nghĩ" và phản hồi ngay lập tức. Khi phản hồi dài (vài trăm token trở lên), streaming giúp người dùng bắt đầu đọc ngay từ những token đầu tiên thay vì phải chờ toàn bộ phản hồi hoàn thành — điều này giảm đáng kể thời gian chờ cảm nhận (perceived latency) và cải thiện trải nghiệm người dùng. Ngược lại, non-streaming phù hợp hơn trong các pipeline xử lý hàng loạt (batch processing) hoặc khi kết quả cần được xử lý toàn bộ trước khi hiển thị — ví dụ: phân tích cảm xúc văn bản, trích xuất dữ liệu có cấu trúc (JSON), hoặc khi kết quả được dùng làm đầu vào cho bước xử lý tiếp theo trong chuỗi pipeline. Trong những trường hợp này, streaming tạo thêm phức tạp không cần thiết mà không mang lại lợi ích rõ ràng cho người dùng cuối.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả tests pass: `pytest tests/ -v`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
