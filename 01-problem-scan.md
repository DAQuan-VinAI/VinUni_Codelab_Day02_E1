I,Phase 1
📝 List bài toán quét qua 4 Lenses
Mô tả ngắn bài toán
1,VinFast (Repetitive & Time-consuming):Tự động phân loại, đối soát log lỗi phiên sạc gián đoạn tại trạm sạc công cộng và lập đề xuất hoàn tiền cọc/phí sạc cho khách hàng.
2,Xanh SMS(takeholder Pain):Tự động phân tích và xử lý khiếu nại của tài xế về lỗi định vị GPS/sai lệch cuốc xe gây ảnh hưởng tỷ lệ nhận chuyến và thưởng ca.
3,Vinhomes(Time-consuming & AI-upgrade):Phân loại khiếu nại/sự cố kỹ thuật của cư dân trên ứng dụng Vinhomes Resident và soạn dự thảo phản hồi chuẩn SLA 5 sao.
4,Vinmec(Time-consuming):Trích xuất và cấu trúc hóa hồ sơ bệnh án/kết quả xét nghiệm từ bệnh viện tuyến trước (file scan/PDF) vào hệ thống EMR trước giờ khám.
5,Vinpearl(AI-upgrade & Repetitive):Trợ lý hội thoại đa ngôn ngữ tư vấn cá nhân hóa lịch trình vui chơi và đặt bàn nhà hàng theo thời gian thực cho khách lưu trú.


II,Phase2

Chọn Top 3 bài toán có tính khả thi kỹ thuật và tác động vận hành rõ ràng nhất:

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tự động đối soát log lỗi sạc và tạo đề xuất xử lý │
│ khiếu nại sạc xe điện ngắt quãng tại trạm sạc công cộng.    │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH & Kỹ thuật Vận hành Trạm│
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận ticket ──> 2. Tra cứu log trụ sạc & xe trên OCPP  │
│   ──> 3. Xác định lỗi (do trụ, lưới hay do xe)              │
│   ──> 4. Tính toán tiền hoàn & gõ mail giải thích.          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 12-18 min/vé) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4.       │
│                                                             │
│ Đo thành công bằng gì: Giảm thời gian xử lý khiếu nại sạc   │
│ từ 20 phút ──> dưới 3 phút/ticket; độ chính xác xác định lỗi │
│ đạt trên 90%.                                               │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phân luồng sự cố kỹ thuật căn hộ và soạn thảo phản│
│ hồi sơ bộ cho cư dân trên ứng dụng Vinhomes Resident.       │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Lễ tân / Ban Quản lý Tòa nhà │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Đọc tin nhắn/ticket ──> 2. Đánh giá tính khẩn cấp     │
│   ──> 3. Tag bộ phận (Điện nước/Xây dựng/An ninh)           │
│   ──> 4. Viết phản hồi xác nhận cho cư dân theo template.   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 6-10 min/vé)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2, 3 và 4.    │
│                                                             │
│ Đo thành công bằng gì: 90% ticket được phân loại đúng đội   │
│ kỹ thuật dưới 15 giây; giảm 70% thời gian phản hồi cư dân.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Trích xuất và thẩm định tự động khiếu nại sai lệch│
│ cước phí/lộ trình từ ảnh chụp màn hình GPS của tài xế.      │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Đội ngũ Vận hành Tài xế (Driver Ops)   │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Mở ticket khiếu nại ──> 2. Xem ảnh chụp màn hình & log │
│   ──> 3. Đo khoảng cách thực tế vs khoảng cách trên app     │
│   ──> 4. Quyết định bù cước và gửi thông báo cho tài xế.    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 8-12 min/vé)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 và 3.          │
│                                                             │
│ Đo thành công bằng gì: Thời gian xử lý khiếu nại giảm từ    │
│ 24 giờ ──> dưới 30 phút; 80% vụ việc rõ ràng tự động duyệt. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘