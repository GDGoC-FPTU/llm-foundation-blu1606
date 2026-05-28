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
> Ở mức temperature = 0.0, câu trả lời có tính xác định cao (deterministic), các lần gọi cho ra kết quả giống hệt nhau về cả cấu trúc và thông tin. Khi tăng lên 0.5 và 1.0, câu chữ trở nên tự nhiên, phong phú và sáng tạo hơn. Tuy nhiên, ở mức 1.5, phản hồi bắt đầu bị hỗn loạn, xuất hiện hiện tượng "ảo giác" (hallucination) và câu cú không còn mạch lạc do mô hình chọn các token có xác suất cực thấp.


**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Mình sẽ đặt temperature từ 0.0 đến 0.2. Lý do là chatbot hỗ trợ khách hàng cần sự chính xác tuyệt đối, nhất quán (cùng một câu hỏi về chính sách phải luôn trả về cùng một đáp án chuẩn) và cần triệt tiêu tối đa hiện tượng ảo giác thông tin để tránh đưa ra thông tin sai lệch cho khách hàng.



---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> GPT-4o đắt hơn GPT-4o-mini đúng 33.33 lần. Biểu giá của GPT-4o ($5.00/$20.00 cho 1M tokens input/output) gấp chính xác 33.33 lần so với GPT-4o-mini ($0.15/$0.60). Với bất kỳ tỷ lệ phân bổ input/output nào cho workload 30.000 lượt gọi/ngày này, chi phí sử dụng GPT-4o vẫn luôn cao hơn gấp ~33.3 lần.


**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> GPT-4o xứng đáng: Khi thực hiện các tác vụ đòi hỏi khả năng suy luận logic sâu sắc, giải quyết bài toán phức tạp, phân tích tài chính đa chiều hoặc sinh mã nguồn (coding) phức tạp. GPT-4o-mini tốt hơn: Khi thực hiện các tác vụ đơn giản, lặp đi lặp lại với tần suất cực lớn như phân loại cảm xúc văn bản (sentiment analysis), trích xuất thông tin thực thể (data extraction), hoặc tóm tắt các đoạn chat ngắn.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất: Trong các ứng dụng chatbot trò chuyện tương tác trực tiếp với con người. Việc mô hình lớn mất vài giây để sinh câu trả lời hoàn chỉnh sẽ gây cảm giác chờ đợi khó chịu; streaming hiển thị các từ ngay khi chúng được sinh ra (Time to First Token cực thấp), tạo cảm giác phản hồi tức thì và nâng cao UX. Non-streaming phù hợp hơn: Khi gọi các API xử lý ngầm (background/batch processing), trích xuất dữ liệu dạng cấu trúc (JSON để code parse tiếp), hoặc tự động hóa quy trình nơi hệ thống cần nhận trọn vẹn kết quả đầu ra trước khi thực hiện hành động tiếp theo.


## Danh Sách Kiểm Tra Nộp Bài
- [ ] Tất cả tests pass: `pytest tests/ -v`
- [ ] `call_openai` đã triển khai và kiểm thử
- [ ] `call_openai_mini` đã triển khai và kiểm thử
- [ ] `compare_models` đã triển khai và kiểm thử
- [ ] `streaming_chatbot` đã triển khai và kiểm thử
- [ ] `retry_with_backoff` đã triển khai và kiểm thử
- [ ] `batch_compare` đã triển khai và kiểm thử
- [ ] `format_comparison_table` đã triển khai và kiểm thử
- [ ] `exercises.md` đã điền đầy đủ
- [ ] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
