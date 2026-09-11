 Phase 3 — DEEP-DIVE (Bài toán: VinFast EV Charging Dispute & Triage)
 Chọn bài toán trọng tâm: Hệ thống AI phân tích lỗi sạc & hỗ trợ CSKH xử lý khiếu nại trạm sạc VinFast.3.1. 
 
Current-State Workflow Mapping[Khách gửi khiếu nại]
        │
        ▼
[🔴 Handoff: CSKH tiếp nhận qua Call Center/App] (⏱ 2 min)
        │
        ▼
[🔴 Bottleneck 1: Truy cập hệ thống OCPP tìm Log sạc theo VIN/Trụ] (⏱ 6 min)
        │
        ▼
[🔴 Bottleneck 2: Đọc mã lỗi hex/text kỹ thuật để hiểu nguyên nhân] (⏱ 7 min)
        │
        ▼
[Tính toán số kWh thực sạc vs tiền trừ tài khoản] (⏱ 3 min)
        │
        ▼
[Soạn tin nhắn/email giải thích nguyên nhân & tạo lệnh hoàn tiền] (⏱ 4 min)

===> Tổng thời gian vận hành trung bình: 22 phút / lượt ticket
3.2. Problem Statement (6-field) & MetricsField

Nội dung chi tiết
1. Actor / OperatorChuyên viên CSKH Tier-1 và Kỹ sư Vận hành Trạm sạc VinFast.
2. Current WorkflowNhận khiếu nại gián đoạn sạc $\rightarrow$ tra cứu thủ công lịch sử sạc trên cổng quản trị OCPP $\rightarrow$ dịch mã lỗi kỹ thuật $\rightarrow$ tính số tiền chênh lệch $\rightarrow$ gửi email xin lỗi và tạo phiếu đề xuất hoàn tiền.
3. BottleneckĐọc và giải mã dữ liệu log kỹ thuật (EVSE Error Code, BMS Stop Reason) tốn thời gian và dễ nhầm lẫn giữa lỗi do người dùng cắm sai cách với lỗi phần cứng trụ sạc.
4. Business ImpactThời gian chờ phản hồi lên tới 48h khiến khách hàng bức xúc; tỷ lệ bồi hoàn sai gây tổn thất ngân sách hoặc giảm điểm CSI (Customer Satisfaction Index) của hệ sinh thái VinFast.
5. Success Metric- SLA: 90% ticket được phân tích log và tạo bản thảo phản hồi trong vòng dưới 10 giây.- Độ chính xác: 95% khớp nguyên nhân kỹ thuật do Kỹ sư Tier-2 xác nhận.- Thời gian xử lý của nhân viên: Giảm từ 22 phút xuống dưới 3 phút/ticket.
6. Operational BoundaryAI ĐƯỢC PHÉP: Trích xuất mã lỗi, phân loại nhóm nguyên nhân, tính toán số tiền chênh lệch dự kiến, soạn thảo email giải thích thân thiện.TUYỆT ĐỐI KHÔNG ĐƯỢC: Tự động kích hoạt lệnh chuyển tiền ngân hàng; không tự ý tuyên bố lỗi do phần cứng VinFast khi chưa đủ dữ liệu; không đưa ra các cam kết pháp lý vượt thẩm quyền chính sách bảo hành.

3.3. Future-State Flow & AI Fit
Mức độ AI Fit: LLM Feature kết hợp State-Machine (Structured Extraction & Triage).

Quy trình tương lai:

Ticket khiếu nại gửi về kèm mã giao dịch/Session ID.

🔵 AI Step: LLM nhận raw text khiếu nại + raw log JSON của phiên sạc từ OCPP. Phân tích nguyên nhân cốt lõi, trích xuất số tiền cần bù, soạn thảo thư giải thích theo chuẩn giọng điệu VinFast.

🟢 Human Step (HITL): Nhân viên CSKH xem màn hình đối chiếu (bên trái: log tóm tắt; bên phải: dự thảo phản hồi + số tiền đề xuất). Nhân viên bấm nút Duyệt hoàn tiền & Gửi thư hoặc chỉnh sửa nếu cần.

↩️ Fallback: Nếu chỉ số tin cậy (confidence score) của model <0.85 hoặc xuất hiện lỗi an toàn pin nghiêm trọng (Thermal Runaway Alert, Insulation Fault), hệ thống tự động gắn tag và chuyển thẳng về cho Kỹ sư Tier-2 xử lý trực tiếp.