# Lab 02 — Problem Scan & Quick Assessment

---

## 🔍 Phase 1 — SCAN (Quét Bài Toán)

### 📝 Danh sách 5+ bài toán tiềm năng

| # | Công ty thành viên | Lens | Mô tả ngắn bài toán |
|---|-------------------|------|---------------------|
| 1 | VinFast | Repetitive (Lặp lại) | Nhân viên phải xử lý hàng trăm yêu cầu bảo hành pin xe điện mỗi ngày, phải đọc mô tả lỗi, kiểm tra lịch sử xe, và phân loại mức độ nghiêm trọng để chuyển đến kỹ thuật viên phù hợp |
| 2 | Xanh SM | Time-consuming (Tốn thời gian) | Tổng đài viên mất 5-8 phút để xử lý khiếu nại khách hàng về cước phí, phải tra cứu lịch sử chuyến đi, tính toán lại giá, và soạn email giải thích chi tiết |
| 3 | Vinhomes | AI-upgrade | Cư dân phải gọi điện hoặc gửi email để đặt lịch sử dụng tiện ích chung (hồ bơi, phòng gym, sân tennis), thời gian chờ phản hồi 2-24 giờ |
| 4 | Vinmec | Stakeholder Pain | Bác sĩ phàn nàn phải mất 15-20 phút sau mỗi ca khám để gõ tóm tắt bệnh án, kê đơn, và lời khuyên vào hệ thống HIS |
| 5 | VinWonders | Repetitive (Lặp lại) | Nhân viên CSKH phải trả lời hàng trăm câu hỏi giống nhau về giờ mở cửa, giá vé, chiều cao tối thiểu cho trò chơi, chính sách hoàn vé |
| 6 | VinFast | Time-consuming | Trung tâm chăm sóc khách hàng phải xử lý yêu cầu test-drive, kiểm tra lịch showroom, xác nhận khung giờ trống, và gọi lại khách hàng để xác nhận |
| 7 | Vinhomes | AI-upgrade | Ban quản lý nhận hàng trăm đánh giá/góp ý từ cư dân mỗi tuần, cần phải đọc, phân loại theo chủ đề (an ninh, vệ sinh, tiện ích), và soạn phản hồi cá nhân hóa |

---

## 🃏 Phase 2 — QUICK ASSESS (3 Problem Cards)

### Problem Card #1: VinFast - Hệ thống Phân loại Yêu cầu Bảo hành Pin Thông minh

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu):                                           │
│ Tự động phân loại và định tuyến yêu cầu bảo hành pin xe    │
│ điện dựa trên mô tả của khách hàng                          │
│                                                             │
│ Công ty thành viên: [X] VinFast  [ ] Xanh SM  [ ] Vinhomes │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ - Nhân viên Bộ phận Chăm sóc Khách hàng VinFast            │
│ - Khách hàng (chờ đợi lâu để được xử lý)                   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng gửi email/form mô tả vấn đề pin            │
│   ──> 2. Nhân viên đọc mô tả, tra cứu lịch sử xe           │
│   ──> 3. Nhân viên phân loại mức độ (Critical/High/Medium) │
│   ──> 4. Chuyển tiếp đến kỹ thuật viên chuyên trách        │
│   ──> 5. Kỹ thuật viên liên hệ lại khách hàng              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Bước 2-3: Đọc và phân loại (⏱ 8-12 phút/yêu cầu)           │
│ - Nhân viên thường phân loại sai mức độ nghiêm trọng       │
│ - Phải tra cứu nhiều hệ thống (CRM, warranty DB, manual)   │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│ Bước 2-3: LLM đọc mô tả khách hàng, trích xuất triệu chứng,│
│ tra cứu knowledge base, và đề xuất phân loại + kỹ thuật viên│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Giảm thời gian xử lý từ 10 min ──> dưới 2 min            │
│ - Tăng độ chính xác phân loại từ 72% ──> 90%+              │
│ - Giảm thời gian chờ phản hồi cho khách hàng từ 4h ──> 30m │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent│
│                                                             │
│ Lý do chọn LLM:                                             │
│ - Cần hiểu ngôn ngữ tự nhiên (mô tả triệu chứng đa dạng)   │
│ - Cần reasoning để kết hợp lịch sử xe + triệu chứng        │
│ - Rule-based không đủ linh hoạt với ngôn ngữ tự do         │
└─────────────────────────────────────────────────────────────┘
```

---

### Problem Card #2: Vinmec - Trợ lý Ghi chép Bệnh án Thông minh

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu):                                           │
│ Tự động tạo tóm tắt bệnh án từ ghi âm cuộc trò chuyện      │
│ giữa bác sĩ và bệnh nhân                                    │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes │
│                     [X] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ - Bác sĩ (mất thời gian ghi chép sau khám)                 │
│ - Bệnh nhân (bác sĩ ít tập trung vào giao tiếp)            │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bác sĩ khám và trò chuyện với bệnh nhân (15-20 min)   │
│   ──> 2. Bác sĩ ghi tay/đánh máy tóm tắt (15 min)          │
│   ──> 3. Nhập vào hệ thống HIS (5 min)                     │
│   ──> 4. In và lưu trữ hồ sơ                               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Bước 2: Ghi chép tóm tắt (⏱ 15 phút/ca khám)               │
│ - Bác sĩ mệt sau nhiều ca khám, ghi chép không đầy đủ      │
│ - Giảm thời gian tương tác với bệnh nhân                   │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│ Bước 1-2: Ghi âm cuộc trò chuyện → Speech-to-text → LLM    │
│ trích xuất thông tin quan trọng → Tạo tóm tắt có cấu trúc  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Giảm thời gian ghi chép từ 15 min ──> dưới 3 min         │
│ - Bác sĩ review và chỉnh sửa thay vì viết từ đầu           │
│ - Tăng số ca khám/ngày từ 20 ──> 24 ca                     │
│ - Điểm hài lòng bệnh nhân tăng từ 7.8 ──> 8.5/10           │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent│
│                                                             │
│ Lý do chọn LLM:                                             │
│ - Cần xử lý ngôn ngữ tự nhiên phức tạp (thuật ngữ y khoa)  │
│ - Cần trích xuất thông tin có cấu trúc từ hội thoại tự do  │
│ - Cần tạo văn bản tóm tắt mạch lạc, ngắn gọn               │
└─────────────────────────────────────────────────────────────┘
```

---

### Problem Card #3: Vinhomes - Chatbot Quản lý Tiện ích Cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu):                                           │
│ Chatbot tự động xử lý đặt lịch sử dụng tiện ích chung      │
│ (hồ bơi, gym, sân tennis) cho cư dân                        │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [X] Vinhomes │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ - Cư dân (phải gọi điện/email, chờ phản hồi lâu)           │
│ - Nhân viên BQL (phải trả lời câu hỏi lặp lại suốt ngày)   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gọi hotline/gửi email yêu cầu đặt lịch         │
│   ──> 2. NV BQL kiểm tra lịch trống trên Excel/Google Cal  │
│   ──> 3. NV gọi lại/email xác nhận hoặc đề xuất giờ khác   │
│   ──> 4. Cư dân xác nhận                                   │
│   ──> 5. NV cập nhật lịch và gửi thông báo                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Bước 2-3: Kiểm tra và phản hồi (⏱ 15-30 phút, đôi khi 24h) │
│ - Thời gian chờ phản hồi quá lâu, cư dân không hài lòng    │
│ - Nhân viên quên cập nhật lịch → double booking            │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│ Bước 1-5: Chatbot nhận yêu cầu → Tra cứu lịch realtime →   │
│ Đặt lịch tự động → Gửi xác nhận qua Zalo/Email             │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Giảm thời gian đặt lịch từ 30 min - 24h ──> dưới 2 min   │
│ - Giảm tỷ lệ double booking từ 12% ──> dưới 2%             │
│ - Tăng số lượt sử dụng tiện ích từ 1200 ──> 1800/tháng     │
│ - Giải phóng 60% thời gian nhân viên BQL                   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [X] Agent│
│                                                             │
│ Lý do chọn Agent:                                           │
│ - Cần tương tác qua lại nhiều lần với người dùng           │
│ - Cần gọi API kiểm tra lịch realtime, xử lý xung đột       │
│ - Cần xác nhận, gửi thông báo, và cập nhật database        │
│ - Cần xử lý nhiều intent (đặt lịch, hủy, thay đổi, hỏi quy định)│
└─────────────────────────────────────────────────────────────┘
```

---

## 💡 Reflection (Suy ngẫm)

### Câu hỏi tự đánh giá:
1. **Bài toán nào khả thi nhất?**
   - Problem Card #3 (Vinhomes Chatbot) vì đã có nhiều giải pháp tương tự trên thị trường, công nghệ trưởng thành.

2. **Bài toán nào thách thức nhất?**
   - Problem Card #2 (Vinmec Medical Scribe) vì liên quan đến dữ liệu y tế nhạy cảm, yêu cầu độ chính xác cao, và cần tuân thủ quy định pháp lý nghiêm ngặt.

3. **Bài toán nào mang lại impact lớn nhất?**
   - Problem Card #2 có impact cao nhất vì giải phóng thời gian bác sĩ, tăng chất lượng chăm sóc bệnh nhân, và có thể mở rộng ra toàn hệ thống Vinmec.

4. **Lens nào giúp tôi tìm được nhiều bài toán nhất?**
   - Lens "Repetitive" (Lặp lại) vì nhiều quy trình vận hành của Vingroup có tính chất lặp lại cao.

---

**Người thực hiện:** Bùi Tùng Dương  
**Ngày hoàn thành:** 11/09/2026  
**Branch:** DuongBT


