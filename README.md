# 📝 IELTS Essay Scorer with LLM

Đây là một sổ ghi chép Google Colab triển khai một API chấm điểm bài luận IELTS (Task 2) sử dụng mô hình Ngôn ngữ lớn (LLM) được tinh chỉnh từ Hugging Face. API này có thể được sử dụng để đánh giá các bài luận dựa trên các tiêu chí chấm điểm chính thức của IELTS: Task Achievement, Coherence and Cohesion, Lexical Resource, và Grammatical Range and Accuracy.

## ✨ Tính năng

- **Đánh giá chi tiết theo tiêu chí IELTS**: Cung cấp phản hồi chuyên sâu cho từng tiêu chí chấm điểm IELTS.
- **Trích dẫn và ví dụ cụ thể**: Đánh giá bao gồm các trích dẫn trực tiếp từ bài luận của thí sinh để minh họa điểm mạnh và điểm yếu.
- **Đề xuất band điểm**: Đưa ra các band điểm ước tính cho từng tiêu chí và tổng thể.
- **Sửa lỗi ngữ pháp và từ vựng**: Xác định và đưa ra sửa lỗi cho các lỗi phổ biến.
- **Phản hồi hành động**: Cung cấp tóm tắt, điểm mạnh và điểm yếu cụ thể với các gợi ý cải thiện.
- **Triển khai API với Flask và ngrok**: Cho phép truy cập dịch vụ chấm điểm thông qua một API công khai.
- **Xử lý số lượng từ**: Tự động chấm 0 điểm cho bài luận dưới 100 từ và áp dụng trừ điểm cho bài luận từ 100 đến 249 từ.

## 🚀 Bắt đầu

### 1. Chuẩn bị môi trường Colab

Chạy tất cả các ô mã trong sổ ghi chép theo thứ tự. Sổ ghi chép sẽ tự động:

- Cài đặt các thư viện Python cần thiết (`bitsandbytes`, `flask`, `pyngrok`, `transformers`, `accelerate`, `flask_cors`, `peft`, `sentencepiece`).
- Tải mô hình LLM và tokenizer từ Hugging Face (`NGVT21/IELTS_Writing`).
- Khởi tạo ứng dụng Flask.
- Thiết lập ngrok tunnel để tạo một URL công khai cho API.

### 2. Thiết lập ngrok Auth Token

Để sử dụng `ngrok`, bạn cần có một Auth Token. Vui lòng truy cập [trang web ngrok](https://dashboard.ngrok.com/get-started/your-authtoken), đăng ký/đăng nhập và sao chép Auth Token của bạn. Sau đó, dán nó vào dòng sau trong ô mã cài đặt:

```python
ngrok.set_auth_token("YOUR_NGROK_AUTH_TOKEN_HERE")
```

Bạn cũng có thể lưu token vào Colab Secrets để sử dụng lại.

### 3. Sử dụng API

Sau khi tất cả các ô mã đã chạy thành công, bạn sẽ thấy một URL công khai hiển thị trong output cuối cùng, ví dụ:

```
🌍 URL Công khai (Public URL): https://your-unique-id.ngrok-free.dev
👉 Endpoint để chấm điểm: https://your-unique-id.ngrok-free.dev/score
```

Bạn có thể gửi yêu cầu POST đến endpoint `/score` này với payload JSON chứa `task` và `essay`.

**Endpoint**: `/score`
**Phương thức**: `POST`
**Content-Type**: `application/json`

**Ví dụ về Payload (JSON Request Body)**:

```json
{
  "task": "Some people think that the government should ban dangerous sports, while others think that people should be free to choose any sport activities. Discuss both views and give your own opinion.",
  "essay": "Dangerous sports like skydiving and rock climbing have become increasingly popular. Some argue for a ban due to the inherent risks, while others champion individual freedom. This essay will discuss both perspectives and present a balanced view."
}
```

**Ví dụ về Phản hồi (JSON Response)**:

```json
{
  "coherence_cohesion": {
    "assessment": "...",
    "suggested_band_score": "..."
  },
  "feedback": {
    "strengths": [
      "..."
    ],
    "summary": "...",
    "weaknesses": [
      "..."
    ]
  },
  "grammatical_range_accuracy": {
    "assessment": "...",
    "examples_of_complex_structures": [
      "..."
    ],
    "mistakes_rectified": [
      {
        "correction": "...",
        "original": "..."
      }
    ],
    "suggested_band_score": "..."
  },
  "lexical_resource": {
    "assessment": "...",
    "examples_of_good_vocabulary": [
      "..."
    ],
    "mistakes_rectified": [
      {
        "correction": "...",
        "original": "..."
      }
    ],
    "suggested_band_score": "..."
  },
  "overall_band_score": "...",
  "task_achievement": {
    "assessment": "...",
    "suggested_band_score": "..."
  }
}
```

## 🛠️ Công nghệ sử dụng

- **Python**: Ngôn ngữ lập trình chính.
- **Flask**: Framework web để xây dựng API.
- **Pyngrok**: Thư viện Python để tương tác với ngrok và tạo public URL.
- **Hugging Face Transformers**: Thư viện để tải và sử dụng mô hình LLM.
- **Accelerate & BitsAndBytes**: Để tối ưu hóa việc sử dụng GPU và chạy mô hình lớn trên phần cứng hạn chế (Colab T4).
- **PyTorch**: Framework deep learning cơ bản.
- **CORS**: Để xử lý các yêu cầu cross-origin cho API.

## ⚠️ Lưu ý

- **Thời gian khởi động**: Việc tải mô hình có thể mất một chút thời gian tùy thuộc vào tốc độ mạng.
- **Giới hạn tài nguyên Colab**: Mô hình này được cấu hình để chạy hiệu quả trên GPU Colab T4 thông qua lượng tử hóa 4-bit. Hiệu suất có thể thay đổi.
- **Độ chính xác của ngrok URL**: URL công khai của ngrok là tạm thời và sẽ thay đổi mỗi khi bạn chạy lại sổ ghi chép (hoặc ngrok tunnel bị ngắt kết nối).
- **Rule chấm điểm**:
    - Bài luận dưới 100 từ sẽ nhận 0 điểm.
    - Bài luận từ 100 đến 149 từ sẽ bị trừ điểm theo công thức: 1 điểm cho mỗi 20 từ thiếu so với 150 từ, tối đa 3 điểm trừ.
