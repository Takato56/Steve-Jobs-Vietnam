# AI Copilot for Public Services Flowchart
'''mermaid

flowchart TD
    A([Bắt đầu])

    B[Người dân truy cập<br/>Cổng Dịch vụ công]

    C[Nhập nhu cầu bằng ngôn ngữ tự nhiên<br/><i>Ví dụ: "Tôi muốn đăng ký khai sinh cho con"</i>]

    D[AI phân tích yêu cầu]

    E{Đã xác định<br/>được thủ tục?}

    F[AI hỏi 1 câu<br/>làm rõ]

    G[AI xác định<br/>đúng thủ tục]

    H[Hiển thị hướng dẫn<br/>• Checklist hồ sơ<br/>• Quy trình thực hiện<br/>• Giấy tờ cần chuẩn bị<br/>• Nguồn tham chiếu]

    I[Người dân chuẩn bị hồ sơ]

    J[Yêu cầu AI kiểm tra<br/>trước khi nộp]

    K[AI kiểm tra hồ sơ<br/>• Thiếu giấy tờ<br/>• Thiếu thông tin<br/>• Sai định dạng<br/>• Mâu thuẫn dữ liệu]

    L{Có lỗi?}

    M[Hiển thị lỗi<br/>và gợi ý sửa]

    N[Hồ sơ sẵn sàng<br/>để nộp]

    O[Người dân chỉnh sửa]

    P[Kiểm tra lại]

    Q([Hoàn thành])

    A --> B
    B --> C
    C --> D
    D --> E

    E -- Không --> F
    F --> D

    E -- Có --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> L

    L -- Có --> M
    M --> O
    O --> P
    P --> K

    L -- Không --> N
    N --> Q
'''
