# Lab 02 — Problem Scan & Quick Assess

**Nhóm/use case được chọn:** Vinmec — trợ lý soạn thảo tóm tắt xuất viện (Discharge Summary Drafting).  
**Lưu ý về số liệu:** Các con số thời gian, khối lượng và baseline dưới đây là *giả định để thiết kế pilot*. Chúng phải được xác minh bằng log vận hành và ý kiến của Vinmec trước khi triển khai.

## Phase 1 — Scan

| # | Công ty thành viên | Lens | Bài toán/bottleneck quan sát được |
|---:|---|---|---|
| 1 | Vinmec | Tốn thời gian | Bác sĩ phải đọc EHR, kết quả xét nghiệm và ghi chú điều trị để tự viết tóm tắt xuất viện; phần tổng hợp lặp lại, dễ bỏ sót thay đổi thuốc. |
| 2 | Vinmec | Lặp lại | Nhân viên bảo hiểm tìm dữ liệu lâm sàng, đối chiếu checklist từng hãng và điền bộ hồ sơ xin phê duyệt. |
| 3 | VinFast | Tốn thời gian | Cố vấn dịch vụ đọc mô tả lỗi tự do, lịch sử sửa chữa và chính sách để phân luồng yêu cầu bảo hành. |
| 4 | VinFast | Pain từ stakeholder | Khách đặt lịch bảo dưỡng phải chờ xác nhận/đổi lịch; slot trống do no-show làm giảm công suất xưởng. |
| 5 | Vinpearl | AI-upgrade | Nhân viên xử lý các yêu cầu đổi phòng, dịch vụ và lịch đặt qua nhiều kênh; câu trả lời phụ thuộc việc tra PMS thủ công. |
| 6 | Vinhomes | Lặp lại | Phản ánh cư dân dạng văn bản tự do phải được đọc, phân loại và chuyển đúng đội kỹ thuật/tòa nhà. |

## Phase 2 — Quick Problem Cards

### Card 1 — Vinmec: Draft tóm tắt xuất viện

| Mục | Nội dung |
|---|---|
| Bài toán | Rút ngắn việc soạn bản nháp tóm tắt xuất viện từ EHR, xét nghiệm và ghi chú điều trị, nhưng bác sĩ vẫn là người ký duyệt. |
| Actor | Bác sĩ điều trị và điều dưỡng/nhân viên hồ sơ bệnh án. |
| Workflow hiện tại | 1) Bác sĩ mở EHR → 2) đọc ghi chú, chẩn đoán, xét nghiệm, thuốc → 3) tổng hợp bằng tay vào mẫu → 4) kiểm tra/đính chính → 5) ký và giao bệnh nhân. |
| Bottleneck | Bước 2–3, khoảng **18 phút/hồ sơ** trong giả định pilot; thông tin nằm ở nhiều trường và ghi chú tự do. |
| AI hỗ trợ | LLM tóm tắt có cấu trúc, chỉ tạo **bản nháp**, dẫn nguồn theo mục EHR và cảnh báo trường thiếu. Rule-based kiểm tra trường bắt buộc. |
| Metric | Median thời gian soạn nháp từ 18 xuống **≤8 phút**; ≥95% bản nháp có đủ trường bắt buộc; **0** bản được phát hành không có bác sĩ ký. |
| Quick architecture | **LLM feature + rule validation**, không phải agent tự trị. |

### Card 2 — VinFast: Triage yêu cầu bảo hành

| Mục | Nội dung |
|---|---|
| Bài toán | Hỗ trợ cố vấn dịch vụ tạo đề xuất phân luồng claim bảo hành từ mô tả khách hàng và lịch sử xe. |
| Actor | Cố vấn dịch vụ/nhân viên bảo hành. |
| Workflow hiện tại | 1) Nhận yêu cầu → 2) tìm VIN và lịch sử → 3) đọc mô tả/hình ảnh → 4) đối chiếu chính sách → 5) yêu cầu bổ sung hoặc chuyển kỹ thuật. |
| Bottleneck | Bước 3–4, giả định **12 phút/claim**; mô tả tiếng Việt không chuẩn và chính sách nhiều ngoại lệ. |
| AI hỗ trợ | LLM trích xuất triệu chứng và draft checklist; rule engine kiểm tra VIN, thời hạn và điều kiện chính sách. |
| Metric | 80% hồ sơ có checklist nháp trong ≤3 phút; giảm 25% hồ sơ thiếu thông tin; không có quyết định từ chối tự động. |
| Quick architecture | **LLM feature + rules**; kỹ sư/cố vấn duyệt mọi quyết định. |

### Card 3 — Vinpearl: Phân loại yêu cầu khách lưu trú

| Mục | Nội dung |
|---|---|
| Bài toán | Phân loại và soạn phản hồi nháp cho yêu cầu đổi phòng/dịch vụ của khách, sau đó chuyển đúng bộ phận. |
| Actor | Nhân viên lễ tân và guest-relations. |
| Workflow hiện tại | 1) Nhận chat/email/cuộc gọi → 2) mở PMS kiểm tra booking → 3) xác định loại yêu cầu/độ khẩn → 4) liên hệ bộ phận → 5) phản hồi khách. |
| Bottleneck | Bước 2–4, giả định **9 phút/yêu cầu**; handoff giữa lễ tân, housekeeping và kỹ thuật. |
| AI hỗ trợ | LLM phân loại, tóm tắt và draft phản hồi; workflow/rules định tuyến theo property, urgency và giờ hoạt động. |
| Metric | ≥85% yêu cầu định tuyến đúng ngay lần đầu; first-response median từ 9 xuống **≤3 phút**; CSAT không giảm. |
| Quick architecture | **LLM feature + workflow rules**; không tự xác nhận đổi phòng hay tạo phí. |

## Lựa chọn để deep-dive

Chọn **Card 1 — Vinmec** vì phần tạo văn bản/tổng hợp là điểm mạnh của LLM, còn các điều kiện an toàn có thể giữ bằng rule validation, phân quyền và human-in-the-loop. Đây chỉ phù hợp để pilot với dữ liệu đã được phê duyệt, khử định danh khi có thể và có đánh giá lâm sàng trước khi dùng thực tế.
