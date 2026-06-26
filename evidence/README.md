# Phân tích so sánh Prompt V1 và Prompt V2 (RAGAS Evaluation)

**LangSmith Project URL:** [day22-lab](https://apac.smith.langchain.com/o/b630a2ce-0d12-47e2-bb9f-20cc5c42ea8c/projects/p/fdd2e360-da4d-4ede-94a5-460cbae13342?timeModel=%7B%22duration%22%3A%221d%22%7D)

Dưới đây là phân tích chi tiết về kết quả đánh giá bằng framework RAGAS đối với hai phiên bản System Prompt của hệ thống RAG:

## 1. Kết quả định lượng
Bảng so sánh chi tiết điểm số trung bình của hai phiên bản:

| Chỉ số (Metric) | Bản V1 (Ngắn gọn) | Bản V2 (Expert JSON) | Bản chiến thắng (Winner) |
| :--- | :---: | :---: | :---: |
| **Faithfulness** | **0.9532** | **0.9393** | **V1** |
| **Answer Relevancy** | **0.9125** | **0.9034** | **V1** |
| **Context Recall** | **1.0000** | **1.0000** | **Hòa** |
| **Context Precision** | **0.9450** | **0.9450** | **Hòa** |

---

## 2. Phân tích kết quả

### Độ trung thực (Faithfulness)
* **V1 (0.9532) > V2 (0.9393):** Phiên bản V1 yêu cầu trả lời ngắn gọn (2-4 câu) nên LLM tập trung trích xuất thông tin trực tiếp từ ngữ cảnh. Trong khi đó, phiên bản V2 yêu cầu trả lời theo phong cách chuyên gia (3-5 câu) kèm cấu trúc JSON phức tạp. Việc tạo câu trả lời dài và chi tiết hơn làm tăng khả năng LLM tự sinh các từ nối hoặc suy luận bổ sung, làm giảm nhẹ độ trung thực so với văn bản gốc.

### Độ liên quan của câu trả lời (Answer Relevancy)
* **V1 (0.9125) > V2 (0.9034):** V1 trả lời trực diện vào câu hỏi một cách tự nhiên. Đối với V2, do mô hình phải phân chia tài nguyên tính toán để định dạng cấu trúc JSON (`answer` và `confidence`), đôi khi làm giảm độ cô đọng của nội dung chính, dẫn đến điểm liên quan bị giảm nhẹ.

### Khả năng truy xuất (Context Recall & Context Precision)
* **Cả 2 phiên bản bằng nhau (1.0000 và 0.9450):** Điểm số này phản ánh chất lượng của bộ tìm kiếm (Retriever) là hoàn toàn giống nhau vì cả 2 phiên bản đều sử dụng chung một cấu trúc FAISS Vectorstore đã được index trước đó để truy vấn ngữ cảnh.

---

## 3. Kết luận và Khuyến nghị
* **Sử dụng V1** khi cần một trợ lý ảo trả lời nhanh, ngắn gọn và có độ chính xác/trung thực thực tế cao nhất đối với người dùng cuối.
* **Sử dụng V2** khi hệ thống cần kết nối dữ liệu tự động với các API hay dịch vụ khác (định dạng JSON giúp dễ lập trình phân tích) và chấp nhận đánh đổi một tỷ lệ rất nhỏ độ trung thực để có cấu trúc dữ liệu mong muốn.
