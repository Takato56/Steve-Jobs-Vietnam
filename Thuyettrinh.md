

## 1. Bối cảnh
Cùng với quá trình chuyển đổi số, ngày càng nhiều thủ tục hành chính đã được đưa lên Cổng Dịch vụ công, giúp người dân có thể thực hiện trực tuyến mà không cần đến trực tiếp cơ quan nhà nước.
Tuy nhiên, việc thực hiện thủ tục vẫn chưa thực sự dễ dàng, người dân vẫn gặp nhiều khó khăn khi thực hiện các thủ tục hành chính. 


## Đối với người dân

Thứ nhất, họ **không biết cần chuẩn bị những giấy tờ gì**.

Nhiều người phải đọc các văn bản pháp luật dài, khó hiểu hoặc gọi điện hỏi cơ quan tiếp nhận.

---

Thứ hai, họ **không biết mình đã điền đúng hay chưa**.

Sai một trường thông tin, thiếu một chữ ký hay nhập sai định dạng CCCD đều chỉ được phát hiện sau khi nộp hồ sơ.

Điều này dẫn đến việc phải đi lại nhiều lần.

---

Thứ ba, khi cần hỗ trợ, số lượng cán bộ có hạn nên người dân phải chờ đợi hoặc đến trực tiếp để hỏi những vấn đề rất đơn giản.

---

## Đối với cơ quan Nhà nước

Ngược lại, cán bộ tiếp nhận cũng gặp nhiều khó khăn:

- tiếp nhận nhiều hồ sơ thiếu thông tin
- mất thời gian hướng dẫn các câu hỏi giống nhau
- giảm hiệu quả xử lý hồ sơ chuyên môn.

---

# 3. Giải pháp

Để giải quyết bài toán này, nhóm xây dựng **GovEase AI Copilot** — một trợ lý AI hướng dẫn thủ tục hành chính.

Giải pháp tập trung vào **3 năng lực cốt lõi**, đúng theo yêu cầu của đề bài.

---

## Năng lực 1 — Guided Intake

Người dân chỉ cần mô tả nhu cầu bằng ngôn ngữ tự nhiên.

Ví dụ:

> "Tôi vừa sinh em bé và muốn làm giấy khai sinh."

AI sẽ:

- xác định đúng thủ tục,
- hỏi làm rõ nếu còn thiếu thông tin,
- sau đó sinh ra checklist hồ sơ,
- hướng dẫn từng bước,
- đồng thời đưa ví dụ minh họa để người dân dễ thực hiện.

Toàn bộ hướng dẫn đều được trích xuất từ **nguồn dữ liệu chính thức** của Cổng Dịch vụ công Quốc gia.

---

## Năng lực 2 — Pre-submission Checking

Sau khi người dân điền biểu mẫu,

AI sẽ kiểm tra hồ sơ trước khi nộp.

Hệ thống phát hiện:

- trường còn thiếu,
- định dạng sai,
- thông tin không nhất quán,
- và đưa ra gợi ý sửa ngay lập tức.

Nhờ đó, người dân có thể sửa lỗi trước khi gửi hồ sơ đến cơ quan tiếp nhận.

---

## Năng lực 3 — Seamless Integration

Thay vì xây dựng một ứng dụng mới,

AI được thiết kế để tích hợp trực tiếp vào các Cổng Dịch vụ công hiện có.

Có thể triển khai dưới dạng:

- REST API
- chatbot
- hoặc widget nhúng.

Điều này giúp đơn vị triển khai không cần thay đổi hạ tầng hiện tại.

---

# 4. AI hoạt động như thế nào? (1.5–2 phút)

Được thiết kế gồm 4 bước.

### Bước 1. Xây dựng kho tri thức

Đầu tiên, thu thập dữ liệu từ các nguồn chính thức như Cổng Dịch vụ công Quốc gia, biểu mẫu hành chính và các văn bản hướng dẫn.
--> dữ liệu được chuẩn hóa thành Markdown và JSON, chia thành các đơn vị nhỏ theo từng bước thực hiện, từng loại giấy tờ và từng trường biểu mẫu. Mỗi dữ liệu đều được gắn metadata như mã thủ tục và nguồn trích dẫn để dễ dàng truy xuất.
-->  dữ liệu được embedding và lưu trong **Chroma Vector Database**, tạo thành kho tri thức phục vụ AI.


### Bước 2. Hybrid RAG – Truy xuất thông tin chính xác

Khi người dùng gửi yêu cầu, AI **không trả lời ngay**.

Hệ thống sẽ trước tiên xác định thủ tục phù hợp, sau đó truy xuất thông tin bằng cơ chế **Hybrid RAG**, kết hợp nhiều phương pháp:

- Lọc theo đúng thủ tục hành chính.
- Keyword Search để tìm các quy định chính xác.
- Vector Search để hiểu ý nghĩa câu hỏi.
- Cuối cùng, Reranking để chọn ra những tài liệu liên quan nhất.

Nhờ đó, AI luôn tạo câu trả lời dựa trên tài liệu chính thức thay vì tự suy diễn.


### Bước 3. Sinh hướng dẫn

Sau khi có đầy đủ ngữ cảnh, LLM sẽ tạo hướng dẫn theo cấu trúc dễ hiểu.

Thay vì trả lời bằng một đoạn văn dài, AI sẽ hiển thị:

- Danh sách giấy tờ cần chuẩn bị.
- Quy trình thực hiện từng bước.
- Ví dụ điền các trường khó.
- Những lưu ý quan trọng.
- Và đặc biệt là **trích dẫn nguồn chính thức** để người dùng kiểm chứng.

Điều này giúp người dân vừa dễ theo dõi, vừa yên tâm về tính chính xác của thông tin.

---

### Bước 4. Kiểm tra hồ sơ trước khi nộp

Đây là điểm nổi bật nhất của giải pháp.

Hệ thống sử dụng **hai lớp kiểm tra**.

Lớp đầu tiên là **Rule Engine**, giúp phát hiện các lỗi có quy tắc rõ ràng như:

- thiếu trường bắt buộc,
- sai định dạng CCCD,
- sai ngày tháng,
- thiếu giấy tờ,
- hoặc thông tin không đúng định dạng.

Sau đó, **LLM Semantic Validation** sẽ kiểm tra những lỗi mà Rule Engine khó phát hiện, chẳng hạn:

- thông tin giữa các giấy tờ không khớp,
- quan hệ giữa các thành viên trong hồ sơ không hợp lý,
- hoặc dữ liệu mâu thuẫn giữa các trường.

Cuối cùng, AI trả về danh sách lỗi kèm giải thích và hướng dẫn sửa, giúp người dân hoàn thiện hồ sơ trước khi nộp.


# 5. Demo

Đầu tiên, người dùng chỉ cần nhập nhu cầu bằng ngôn ngữ tự nhiên, ví dụ: **"Tôi muốn đăng ký khai sinh cho con."**

Nếu thông tin chưa đủ, AI sẽ hỏi thêm một câu để làm rõ, sau đó xác định đúng thủ tục.

Tiếp theo, AI trả về **checklist hồ sơ**, **biểu mẫu cần điền** và **hướng dẫn từng bước**, kèm **nguồn tham khảo chính thức** để người dùng dễ dàng chuẩn bị.

Sau khi hoàn thành biểu mẫu, người dùng nhập thông tin đã điền vào hệ thống. AI sẽ tự động kiểm tra, phát hiện các lỗi như **thiếu trường thông tin, sai định dạng hoặc dữ liệu chưa nhất quán**, đồng thời đưa ra gợi ý chỉnh sửa.

Khi tất cả lỗi đã được khắc phục, hệ thống xác nhận hồ sơ đã sẵn sàng để nộp


# 6. Điểm khác biệt

Điểm khác biệt lớn nhất của giải pháp không nằm ở chatbot.
--> AI được xây dựng như **một Copilot dành riêng cho thủ tục hành chính**.

Khác với chatbot thông thường:
- AI luôn trả lời dựa trên dữ liệu chính thức thông qua Hybrid RAG.
- Mọi hướng dẫn đều có trích dẫn nguồn để đảm bảo tính minh bạch.
- Hệ thống kết hợp Rule Engine và LLM nên vừa kiểm tra được lỗi định dạng, vừa phát hiện các xung đột ngữ nghĩa.
- Kết quả trả về ở dạng checklist có cấu trúc thay vì văn bản dài, giúp người dân dễ thực hiện.
- Cuối cùng, giải pháp được thiết kế dưới dạng API và widget nên có thể tích hợp nhanh vào Cổng Dịch vụ công mà không cần xây dựng lại hệ thống.


# 7. Khả năng phát triển

Trong hackathon, nhóm xây dựng MVP cho hai thủ tục phổ biến là:

- đăng ký khai sinh
- đăng ký thường trú.

Trong giai đoạn tiếp theo, hệ thống có thể mở rộng theo ba hướng.
- Mở rộng kho tri thức để hỗ trợ hàng trăm thủ tục hành chính khác.
- kết nối trực tiếp với các hệ thống dịch vụ công của địa phương để tự động điền và kiểm tra biểu mẫu theo thời gian thực.
- khai thác dữ liệu tương tác để phân tích các lỗi phổ biến, từ đó giúp cơ quan quản lý cải thiện quy trình và nâng cao chất lượng dịch vụ công.
