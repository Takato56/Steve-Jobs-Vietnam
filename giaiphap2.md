# Giải pháp kiểm tra hồ sơ trước khi nộp

## 1. Bối cảnh và điều kiện triển khai

Người dân thường không biết thông tin trong hồ sơ đã được điền đúng hay chưa và chỉ phát hiện lỗi sau khi cán bộ tiếp nhận kiểm tra. GovEase-AI có thể hỗ trợ giải quyết vấn đề này nếu Cổng Dịch vụ công Quốc gia cung cấp đầy đủ biểu mẫu chính thức, cấu trúc trường thông tin và phiên bản áp dụng cho từng thủ tục.

Đây là **phương án phát triển trong tương lai**, không phải tính năng đang hoạt động trong DEMO hiện tại. Hệ thống chỉ được triển khai sau khi dữ liệu biểu mẫu có nguồn chính thức, được quản lý phiên bản và được chuyên gia nghiệp vụ phê duyệt.

## 2. Giải pháp đề xuất

### 2.1. Số hóa biểu mẫu chính thức

- Tải biểu mẫu theo đúng mã thủ tục từ nguồn chính thức.
- Chuyển biểu mẫu PDF hoặc DOCX thành `form-schema.json` có cấu trúc.
- Xác định tên trường, kiểu dữ liệu, trường bắt buộc, lựa chọn hợp lệ, quan hệ giữa các trường và giấy tờ liên quan.
- Gắn mỗi schema với URL nguồn, mã thủ tục, phiên bản biểu mẫu, ngày hiệu lực và căn cứ pháp lý.
- Không tự suy diễn trường hoặc quy tắc nếu biểu mẫu nguồn không thể hiện rõ.

### 2.2. Xây dựng bộ quy tắc kiểm tra

Hệ thống kiểm tra hồ sơ theo ba lớp:

1. **Kiểm tra định dạng**
   - Phát hiện trường bắt buộc còn thiếu.
   - Kiểm tra kiểu dữ liệu, ngày tháng, email, số điện thoại và độ dài giá trị.
   - Kiểm tra định dạng công khai của mã định danh nhưng không tuyên bố đã xác minh danh tính.

2. **Kiểm tra logic**
   - Ngày sinh không được nằm trong tương lai.
   - Ngày cấp giấy tờ phải phù hợp với ngày sinh và thời điểm lập hồ sơ.
   - Địa chỉ phải có đủ các cấp hành chính được biểu mẫu yêu cầu.
   - Các trường có điều kiện chỉ xuất hiện hoặc trở thành bắt buộc khi đúng trường hợp áp dụng.

3. **Kiểm tra chéo hồ sơ**
   - Đối chiếu thông tin được lặp lại giữa các phần của biểu mẫu.
   - So sánh dữ liệu khai báo với tài liệu người dùng cung cấp.
   - Phát hiện khác biệt về họ tên, ngày sinh, địa chỉ hoặc quan hệ giữa những người liên quan.

Mỗi quy tắc phải có mã định danh, mức độ nghiêm trọng, trường bị ảnh hưởng, thông báo dễ hiểu, hướng dẫn sửa và nguồn pháp lý hỗ trợ.

### 2.3. AI hỗ trợ kiểm tra ngữ nghĩa

AI được sử dụng cho những vấn đề khó biểu diễn bằng quy tắc cố định, chẳng hạn:

- Nội dung giải trình quá chung chung hoặc không trả lời đúng yêu cầu của trường.
- Thông tin giữa các tài liệu có dấu hiệu mâu thuẫn.
- Người dùng có thể đã chọn sai biểu mẫu hoặc thiếu giấy tờ theo trường hợp cụ thể.
- Giải nghĩa trường thông tin bằng ngôn ngữ dễ hiểu.

Kết quả do AI tạo ra chỉ được xem là cảnh báo cần kiểm tra lại. AI không được tự kết luận hồ sơ hợp pháp, xác minh danh tính hoặc cam kết hồ sơ sẽ được cơ quan có thẩm quyền chấp thuận.

## 3. Luồng sử dụng dự kiến

1. Người dùng mô tả nhu cầu và chatbot xác định thủ tục phù hợp.
2. Hệ thống tải đúng biểu mẫu chính thức đang có hiệu lực.
3. Người dùng điền biểu mẫu theo từng bước và có thể tải lên giấy tờ hỗ trợ.
4. Quy tắc xác định chạy ngay tại từng trường; kiểm tra chéo và kiểm tra ngữ nghĩa chạy khi đủ dữ liệu.
5. Hệ thống hiển thị lỗi tại đúng vị trí và giải thích cách sửa.
6. Người dùng xem báo cáo tổng hợp trước khi nộp hồ sơ qua kênh chính thức.

Kết quả kiểm tra được chia thành bốn nhóm:

- **Lỗi cần sửa:** vi phạm quy tắc rõ ràng hoặc thiếu dữ liệu bắt buộc.
- **Cần kiểm tra lại:** có dấu hiệu bất thường nhưng chưa đủ căn cứ để kết luận.
- **Gợi ý cải thiện:** nội dung có thể được trình bày rõ ràng hoặc đầy đủ hơn.
- **Đã hoàn thành kiểm tra kỹ thuật:** không phát hiện lỗi trong phạm vi quy tắc và dữ liệu hiện có.

Hệ thống không hiển thị thông báo “Hồ sơ chắc chắn hợp lệ”. Thông báo phù hợp là: **“Không phát hiện lỗi trong phạm vi kiểm tra tự động. Kết quả tiếp nhận và giải quyết cuối cùng thuộc cơ quan có thẩm quyền.”**

## 4. Grounding và khả năng giải trình

Mỗi lỗi, cảnh báo hoặc gợi ý phải liên kết được với:

- Trường thông tin bị ảnh hưởng.
- Quy tắc đã được kích hoạt.
- Giá trị hoặc mâu thuẫn được phát hiện.
- Cách sửa được đề xuất.
- Mã thủ tục và phiên bản biểu mẫu.
- URL DVCQG hoặc căn cứ pháp lý hỗ trợ.
- Thời điểm nguồn được kiểm tra gần nhất.

Các nhận định không có căn cứ phải bị loại bỏ hoặc được gắn nhãn rõ là gợi ý AI chưa được xác nhận.

## 5. Quản trị phiên bản và độ mới dữ liệu

Hệ thống cần theo dõi nguồn DVCQG định kỳ. Khi biểu mẫu hoặc quy định thay đổi:

1. Phát hiện thay đổi bằng checksum, metadata hoặc nội dung biểu mẫu.
2. Đánh dấu phiên bản cũ là hết hiệu lực nhưng vẫn giữ lại để truy vết.
3. Tạm dừng kiểm tra tự động đối với phiên bản mới chưa được review.
4. Tạo bản nháp schema và bộ quy tắc mới.
5. Yêu cầu chuyên gia nghiệp vụ kiểm tra và phê duyệt.
6. Chạy regression test trước khi phát hành.
7. Lưu phiên bản biểu mẫu và quy tắc đã dùng trong từng báo cáo kiểm tra.

## 6. Kiến trúc dữ liệu đề xuất

```text
data2/procedures/<sector>/<procedure-code>/
|-- procedure.json
|-- source.pdf
|-- forms/
|   |-- official-form.pdf
|   `-- form-schema.json
|-- validation-rules.json
|-- examples.json
`-- review.json
```

Trong đó:

- `form-schema.json` mô tả giao diện và cấu trúc dữ liệu của biểu mẫu.
- `validation-rules.json` chứa các quy tắc xác định có nguồn dẫn chứng.
- `examples.json` chứa ví dụ đã được review, không chứa dữ liệu cá nhân thật.
- `review.json` lưu trạng thái phê duyệt, người review, phiên bản và ngày hiệu lực.

## 7. An toàn và bảo vệ dữ liệu

- Chỉ yêu cầu dữ liệu thật khi cần thiết; ưu tiên dữ liệu giả lập trong DEMO.
- Mã hóa dữ liệu khi truyền và khi lưu.
- Không ghi nội dung giấy tờ hoặc thông tin định danh vào log ứng dụng.
- Giới hạn thời gian lưu hồ sơ nháp và cho phép người dùng chủ động xóa.
- Quét tệp tải lên, giới hạn loại tệp, kích thước và quyền truy cập.
- Tách nội dung tài liệu người dùng khỏi chỉ dẫn hệ thống để hạn chế prompt injection.
- Không dùng dữ liệu hồ sơ để huấn luyện mô hình nếu chưa có sự đồng ý phù hợp.

## 8. Tiêu chí sẵn sàng triển khai

Tính năng chỉ nên được đưa vào pilot khi đáp ứng đầy đủ các điều kiện sau:

- Có biểu mẫu chính thức và đầy đủ cho từng thủ tục trong phạm vi pilot.
- Mỗi trường và mỗi quy tắc đều có nguồn và phiên bản truy vết được.
- Bộ quy tắc đã được chuyên gia nghiệp vụ phê duyệt.
- Có regression test cho các trường hợp đúng, sai và trường hợp biên.
- Đo được precision, recall và tỷ lệ cảnh báo sai của từng loại quy tắc.
- Có quy trình cập nhật nguồn, SLA xử lý thay đổi và cơ chế tạm dừng phiên bản lỗi thời.
- Giao diện thể hiện rõ giới hạn của kiểm tra tự động và thẩm quyền quyết định cuối cùng.

## 9. Giá trị dự kiến

Khi đủ điều kiện triển khai, giải pháp giúp người dân phát hiện lỗi trước khi đến cơ quan tiếp nhận, hiểu rõ cách sửa và giảm số lần phải bổ sung hồ sơ. Tuy nhiên, GovEase-AI vẫn chỉ đóng vai trò hỗ trợ chuẩn bị và kiểm tra kỹ thuật; hệ thống không thay thế việc xác minh hoặc quyết định của cán bộ có thẩm quyền.
