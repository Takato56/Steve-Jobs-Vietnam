# AI Copilot for Public Services

1. Giới thiệu

AI Copilot for Public Services là trợ lý AI giúp người dân thực hiện thủ tục hành chính trực tuyến. Hệ thống xác định đúng thủ tục, hướng dẫn chuẩn bị hồ sơ, kiểm tra tính hợp lệ trước khi nộp và cung cấp hướng dẫn dựa trên quy định chính thức.

2. Vấn đề

Dù nhiều thủ tục đã số hóa, người dân vẫn gặp khó:

- Xác định sai thủ tục.
- Hiểu và áp dụng quy định phức tạp.
- Chuẩn bị hồ sơ thiếu hoặc điền sai.
- Chỉ phát hiện lỗi sau khi nộp và bị yêu cầu bổ sung.

Hệ quả:

- Tăng hồ sơ trả lại.
- Kéo dài thời gian xử lý.
- Áp lực cho cơ quan tiếp nhận.

3. Giải pháp

AI Copilot đồng hành cùng người dân trước khi nộp hồ sơ, với 3 năng lực chính:

- **Guided Intake:** hiểu nhu cầu bằng ngôn ngữ tự nhiên, xác định thủ tục, hỏi làm rõ và tạo checklist hồ sơ theo từng trường hợp.
- **Pre-submission Validation:** phát hiện thiếu giấy tờ, sai định dạng, vi phạm điều kiện và mâu thuẫn thông tin trước khi nộp.
- **Grounded AI:** mọi hướng dẫn đều dựa trên nguồn dữ liệu chính thức và kèm trích dẫn quy định.

4. Công nghệ
- LLM
- Retrieval-Augmented Generation (Hybrid RAG)
- Embedding Model + Vector Database (Chroma)
- Rule-based Validation Engine
- Python (FastAPI) – Backend
- React / Next.js – Frontend
- Firebase – Authentication & Hosting
- REST API và Widget để tích hợp với Cổng Dịch vụ công


5. Giá trị mang lại
5.1. Người dân
- Chuẩn bị đúng hồ sơ ngay từ lần đầu.
- Giảm thời gian tra cứu và bổ sung hồ sơ.
- Nâng cao trải nghiệm dịch vụ công trực tuyến.

5.2. Cơ quan nhà nước
- Giảm tỷ lệ hồ sơ không hợp lệ.
- Giảm khối lượng xử lý hồ sơ bổ sung.
- Nâng cao hiệu quả vận hành và chất lượng dịch vụ.

6. Kế hoạch sau Hackathon
- Mở rộng từ 2–3 thủ tục thí điểm sang nhiều lĩnh vực hành chính.
- Tự động cập nhật dữ liệu từ nguồn quy định chính thức.
- Tích hợp với Cổng Dịch vụ công cấp tỉnh qua API hoặc widget.
- Thêm khả năng đọc và kiểm tra tài liệu (OCR) từ ảnh/PDF.
- Phát triển thành nền tảng AI Copilot cho toàn bộ hệ sinh thái dịch vụ công số.
