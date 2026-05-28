# Kế Hoạch Triển Khai - Nền Tảng LLM API (Lab A)

Kế hoạch này chi tiết hóa cách hoàn thiện các TODO trong `template.py` của bài Lab A (GitHub Classroom), tích hợp các dịch vụ LLM khác nhau (OpenAI, Google Gemini, Anthropic Claude), đo lường hiệu năng và chi phí, và xây dựng chatbot tương tác streaming.

## Proposed Changes

### Core Implementation

Triển khai toàn bộ các hàm cốt lõi và các phần nâng cao của `template.py`.

#### [MODIFY] [template.py](file:///c:/Users/LENOVO/github-classroom/GDGoC-FPTU/starter-code/template.py)

1. **Task 1: `call_openai`**
   - Sửa hàm trả về đầy đủ 3 thành phần: `(response_text, latency, usage)` thay vì thiếu `usage` như template hiện tại.
   - Trích xuất `usage` từ `response.usage.prompt_tokens` và `response.usage.completion_tokens`.

2. **Task 2: `call_gemini`**
   - Khởi tạo client sử dụng SDK `google-genai` mới: `from google import genai`.
   - Thiết lập cấu hình với `types.GenerateContentConfig`.
   - Trích xuất token usage từ `response.usage_metadata.prompt_token_count` và `response.usage_metadata.candidates_token_count`.

3. **Task 3: `call_anthropic`**
   - Khởi tạo client `anthropic.Anthropic`.
   - Gọi `client.messages.create` và đo đạc latency.
   - Trích xuất token usage từ `response.usage.input_tokens` and `response.usage.output_tokens`.

4. **Task 4: `compare_models`**
   - Gọi cả 3 model: GPT-4o, GPT-4o-mini, và Gemini 2.5 Flash.
   - Tính toán chi phí chính xác bằng USD cho từng lượt gọi dựa trên `PRICING_1M_TOKENS`.
   - Công thức: `Cost = (input_tokens * input_rate_per_1M + output_tokens * output_rate_per_1M) / 1,000,000`.

5. **Task 5: `streaming_chatbot`**
   - Chạy vòng lặp vô hạn tương tác trên CLI.
   - Sử dụng `client.models.generate_content_stream` của Gemini 2.5 Flash để stream text mượt mà.
   - Duy trì lịch sử 3 lượt hội thoại gần nhất (tối đa 6 tin nhắn trong history).

6. **Bonus Tasks (A, B, C)**
   - **Retry with Backoff**: Triển khai thuật toán retry cấp số nhân (`base_delay * 2^attempt`).
   - **Batch Compare**: Chạy so sánh nhiều prompts song song hoặc tuần tự.
   - **Format Table**: Tạo bảng Markdown so sánh các thuộc tính Prompt, Model, Response (truncate 50 chars), Latency, Tokens (In/Out), Cost (USD).

---

## Verification Plan

### Automated Tests
- Chạy bộ kiểm thử tự động của bài Lab:
  ```bash
  pytest tests/ -v
  ```
  *(Tất cả bài kiểm thử sử dụng mock nên không yêu cầu API key thật hay kết nối mạng khi test)*

### Manual Verification
- Thiết lập API key thật (nếu có) và chạy thử nghiệm thủ công:
  ```bash
  python starter-code/template.py
  ```
- Kiểm tra tính tương tác của chatbot dòng lệnh.
