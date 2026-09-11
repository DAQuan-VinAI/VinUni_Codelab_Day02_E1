# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
| --- | --- | --- | --- |
| 1 | **Vinpearl** | Lặp lại | Nhân viên lễ tân phải kiểm tra, nhập liệu và đối chiếu thủ công danh sách khách đoàn check-in/check-out với các công ty du lịch đối tác vào khung giờ cao điểm mỗi ngày. |
| 2 | **Vinmec** | Tốn thời gian | Nhân viên bộ phận tài chính - kế toán mất hàng giờ để kiểm tra và đối chiếu các khoản bảo hiểm y tế lệch tuyến, bảo hiểm tư nhân thủ công cho từng bộ hồ sơ điều trị nội trú. |
| 3 | **VinFast** | AI-upgrade | Hệ thống hướng dẫn sử dụng và sổ tay xe điện hiện tại ở dạng văn bản tĩnh, khó tìm kiếm; AI có thể nâng cấp thành trợ lý ảo thông minh tích hợp trên màn hình xe để giải đáp tức thì mọi thắc mắc kỹ thuật cho tài xế bằng giọng nói. |
| 4 | **Vinhomes** | Pain từ người khác | Ban quản lý tòa nhà thường xuyên nhận phàn nàn từ cư dân vì quy trình đăng ký thẻ xe tháng, khách ra vào hoặc đăng ký thi công căn hộ rườm rà, mất nhiều thời gian chờ đợi duyệt giấy tờ trực tiếp. |
| 5 | **VinWonders** | Lặp lại | Nhân viên kiểm soát cổng ra vào phải liên tục thủ công kiểm tra, đối chiếu mã vé đoàn và quét mã QR phân khúc khách ưu tiên vào các dịp lễ hội cao điểm. |
| 6 | **Vinmec** | AI-upgrade | Hệ thống tư vấn đặt lịch khám bệnh trực tuyến hiện tại chỉ dùng cây kịch bản tĩnh (rule-based), dễ gặp lỗi bỏ sót triệu chứng cấp cứu; AI có thể nâng cấp thành trợ lý phân loại mức độ bệnh thông minh để ưu tiên ca nặng. |

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: 1, 3 và 5


```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: So khớp và phân bổ lại cuốc xe khi khách hàng      │
│ yêu cầu thay đổi điểm đến giữa chừng.                       │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (đi vòng/lệch route), Khách hàng (chờ đợi)│
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách hàng gọi điện hoặc nhắn tin yêu cầu đổi điểm đến │
│   → 2. Tổng đài viên/Hệ thống dừng cuốc xe hiện tại         │
│   → 3. Tính toán thủ công quãng đường và cước phí phát sinh │
│   → 4. Cập nhật lại lộ trình trên ứng dụng của tài xế       │
│   → 5. Thông báo chi phí mới cho khách hàng                 │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 8-10 phút/lượt)              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (AI tự động tính toán lại route tối ưu và update cước phí)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý đổi điểm đến từ 10 phút ──> dưới 1 phút│
│                                                             │
│ Quick Architecture: [x] Optimization Algorithm + LLM Routing│
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: So khớp hóa đơn sạc điện và đối chiếu số liệu     │
│ trạm sạc đối tác hằng tuần tại VinFast.                     │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Nhân viên Kế toán / Vận hành trạm sạc          │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tải file log dữ liệu sạc khổng lồ từ hệ thống đối tác  │
│   → 2. Xuất dữ liệu giao dịch sạc thực tế từ server VinFast │
│   → 3. Dùng Excel để so khớp thủ công từng dòng dữ liệu lệch│
│   → 4. Lọc và tổng hợp các khoản chênh lệch, sai số         │
│   → 5. Lập báo cáo giải trình và gửi đối tác xác nhận       │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 4-6 giờ/tuần)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (ML Matching Pipeline tự động đối chiếu và gắn nhãn lệch)   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian đối chiếu dữ liệu từ 6 giờ ──> dưới 15 phút  │
│                                                             │
│ Quick Architecture: [x] Data Pipeline & Fuzzy Matching      │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán: Bác sĩ viết tóm tắt hồ sơ xuất viện tại Vinmec    │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ (quá tải, mất thời gian hành chính)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Đọc lại toàn bộ lịch sử điều trị và kết quả xét nghiệm │
│   → 2. Tổng hợp các chỉ số lâm sàng quan trọng thủ công     │
│   → 3. Soạn thảo văn bản tóm tắt hồ sơ bệnh án xuất viện    │
│   → 4. Kiểm tra lại thông tin và ký duyệt hồ sơ             │
│   → 5. In ấn và bàn giao cho bệnh nhân                      │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 20-30 phút/bệnh nhân)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (LLM trích xuất dữ liệu bệnh án và tự động generate draft)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian viết tóm tắt từ 25 phút ──> dưới 3 phút      │
│                                                             │
│ Quick Architecture: [x] LLM Medical Document Summarization  │
└─────────────────────────────────────────────────────────────┘
```