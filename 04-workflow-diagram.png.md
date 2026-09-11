[📱 Khách hàng gặp sự cố]
 (Tạo ticket khiếu nại trên VinFast App / Gọi Call Center)
           │
           ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Bước 1: Tiếp nhận khiếu nại                             │
 │ 🔄 HANDOFF: Ticket chuyển từ hệ thống về CSKH Tier-1    │
 │ ⏱ Thời gian: 2 phút                                     │
 └─────────────────────────┬───────────────────────────────┘
                           │
                           ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Bước 2: Truy xuất dữ liệu hệ thống trạm sạc             │
 │ 🔴 BOTTLENECK 1: CSKH phải mở cổng quản trị OCPP,       │
 │    copy/paste mã VIN hoặc Session ID để tìm log sạc.    │
 │ ⏱ Thời gian: 6 phút                                     │
 └─────────────────────────┬───────────────────────────────┘
                           │
                           ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Bước 3: Phân tích nguyên nhân lỗi                       │
 │ 🔴 BOTTLENECK 2: CSKH phải tự đọc raw log, tra cứu bảng │
 │    mã lỗi (Hex/Text) để đoán xem lỗi do trụ, do xe, hay │
 │    do khách hàng cắm sạc sai cách.                      │
 │ ⏱ Thời gian: 7 phút                                     │
 └─────────────────────────┬───────────────────────────────┘
                           │
                           ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Bước 4: Đối soát tài chính                              │
 │ 🧮 TÁC VỤ: Tính toán số lượng kWh thực tế đã nạp thành  │
 │    công so với số tiền cọc/đã trừ trong ví khách hàng.  │
 │ ⏱ Thời gian: 3 phút                                     │
 └─────────────────────────┬───────────────────────────────┘
                           │
                           ▼
 ┌─────────────────────────────────────────────────────────┐
 │ Bước 5: Phản hồi & Xử lý bồi hoàn                       │
 │ 📝 TÁC VỤ: Copy template email, điền nguyên nhân kỹ     │
 │    thuật (đã dịch ra ngôn ngữ dễ hiểu), tạo lệnh refund.│
 │ ⏱ Thời gian: 4 phút                                     │
 └─────────────────────────┬───────────────────────────────┘
                           │
                           ▼
 [✅ KẾT THÚC: Email gửi đi, tiền hoàn về ví khách hàng]