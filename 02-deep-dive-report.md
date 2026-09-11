# Deep-Dive Report — Vinmec Discharge Summary Drafting

## 1. Current-state workflow

**Phạm vi:** một ca nội trú đã có quyết định xuất viện; không hỗ trợ chẩn đoán hoặc chỉ định điều trị.

| Bước | Người/hệ thống | Input → output | Thời gian pilot giả định | Điểm kiểm soát |
|---:|---|---|---:|---|
| 1 | Bác sĩ | Quyết định xuất viện → yêu cầu tạo tóm tắt | 2 phút | Bác sĩ xác nhận ca đủ điều kiện. |
| 2 | Điều dưỡng/nhân viên hồ sơ | EHR, ghi chú, xét nghiệm, đơn thuốc → gói dữ liệu | 4 phút | 🔄 Handoff giữa EHR và người soạn. |
| 3 | Bác sĩ | Đọc nhiều nguồn, chọn diễn biến/thuốc/hướng dẫn → nội dung thô | 8 phút | 🔴 Bottleneck: dữ liệu rải rác, văn bản tự do. |
| 4 | Bác sĩ | Điền mẫu, diễn đạt cho bệnh nhân → bản tóm tắt | 6 phút | 🔴 Bottleneck: dễ bỏ sót theo dõi hoặc thay đổi thuốc. |
| 5 | Bác sĩ + EHR | Kiểm tra, ký số → bản phát hành | 3 phút | 🔄 Handoff sang EHR/bệnh nhân. |

**Tổng thời gian baseline giả định: 23 phút/ca.** Cần đo lại trên tối thiểu 30 ca đại diện trước khi đặt KPI chính thức.

## 2. Problem statement (6 fields)

| Field | Nội dung |
|---|---|
| 1. Actor / Operator | Bác sĩ điều trị là người chịu trách nhiệm lâm sàng; điều dưỡng/nhân viên hồ sơ chuẩn bị dữ liệu và hỗ trợ hoàn thiện. |
| 2. Current workflow | Bác sĩ truy cập EHR, đọc ghi chú tiến triển, chẩn đoán, xét nghiệm, thuốc và thủ thuật; sau đó tự tổng hợp vào mẫu xuất viện, kiểm tra rồi ký. |
| 3. Bottleneck | Đọc–tổng hợp–diễn đạt ở bước 3–4 chiếm 14/23 phút theo baseline giả định. Thông tin dễ mâu thuẫn giữa nguồn, đặc biệt là thuốc lúc ra viện và lịch tái khám. |
| 4. Business impact | Với giả định 40 ca/ngày, baseline tương đương khoảng 15,3 giờ bác sĩ/ngày. Chậm hồ sơ kéo dài thời gian hoàn tất xuất viện và làm giảm thời gian tư vấn trực tiếp. Đây là giả định cần xác minh, không phải số liệu vận hành đã công bố. |
| 5. Success metric | Trong pilot 4 tuần: (a) median thời gian từ yêu cầu đến bản nháp ≤8 phút; (b) ≥95% bản nháp đạt đủ trường bắt buộc sau rule check; (c) tỷ lệ chấp nhận sau lần review đầu ≥70%; (d) 0 bản phát hành không có chữ ký bác sĩ; (e) không tăng lỗi lâm sàng so với baseline audit. |
| 6. Operational boundary | AI chỉ đọc dữ liệu được cấp quyền và tạo bản nháp có nguồn tham chiếu. AI **không** chẩn đoán, kê/đổi thuốc, suy diễn dữ liệu thiếu, ký, ghi ngược vào EHR hay gửi cho bệnh nhân. Bác sĩ phải review, sửa và ký. Ca có dữ liệu thiếu/mâu thuẫn hoặc confidence thấp phải chuyển về quy trình thủ công. |

## 3. AI fit and architecture decision

| Lựa chọn | Phù hợp? | Lý do |
|---|---|---|
| Rule / state machine | Có, nhưng không đủ | Rất phù hợp để kiểm tra trường bắt buộc, quyền truy cập, consent và trạng thái ký; không diễn đạt tốt tóm tắt từ ghi chú tự do. |
| LLM feature | **Có — chọn** | Tóm tắt và chuyển nội dung y khoa sang ngôn ngữ dễ hiểu trong một mẫu cấu trúc; đầu ra bị giới hạn bởi schema và review. |
| Agentic loop | Không | Tự gọi công cụ/ghi dữ liệu qua nhiều bước làm tăng rủi ro không tương xứng; không cần thiết cho MVP. |

## 4. Future-state workflow

```text
[1] Bác sĩ yêu cầu bản nháp
        |
        v
[2] Rule gate: quyền truy cập, consent, ca hợp lệ, trường tối thiểu
        |-- thiếu/mâu thuẫn --> ↩ Fallback: quy trình thủ công + báo thiếu dữ liệu
        v
[3] 🔵 LLM tạo JSON bản nháp + nguồn EHR theo từng mục
        |
        v
[4] Rule validator: schema, trường bắt buộc, thuốc/lịch tái khám hiện diện
        |-- lỗi/low confidence --> ↩ Fallback: không hiển thị bản nháp; bác sĩ tự soạn
        v
[5] 🟢 Bác sĩ review, chỉnh sửa, đối chiếu nguồn và ký
        |-- từ chối --> log lý do, quay lại soạn thủ công
        v
[6] EHR phát hành sau chữ ký bác sĩ
```

**Human-in-the-loop:** bước 5 là bắt buộc; màn hình phải hiển thị nhãn “AI draft — not for release”, nguồn dữ liệu và audit trail (người dùng, phiên bản prompt/model, thời điểm, sửa đổi).

**Privacy & security:** triển khai sau khi có Data Protection Impact Assessment, RBAC tối thiểu cần thiết, mã hóa khi truyền/lưu, retention policy và thỏa thuận xử lý dữ liệu với nhà cung cấp. Pilot nên dùng dữ liệu khử định danh hoặc môi trường đã được phê duyệt.

## 5. Evaluate — AI readiness

| Checklist | Trạng thái | Bằng chứng/việc cần làm |
|---|---|---|
| Có dữ liệu mẫu/log sạch để test? | △ Chưa xác nhận | Chuẩn bị 30–50 ca đã khử định danh, có bản tóm tắt do bác sĩ duyệt làm gold standard. |
| Rủi ro AI sai được kiểm soát? | ✓ Có điều kiện | Schema/rules, chặn ghi tự động, HITL bắt buộc, fallback và audit log. Cần kiểm thử bởi hội đồng lâm sàng. |
| Stakeholder sẵn sàng đổi quy trình? | △ Chưa xác nhận | Tổ chức walkthrough với bác sĩ, điều dưỡng, hồ sơ bệnh án, IT security và legal; đào tạo cách review/override. |

### Decision: NOT YET

Không nên đưa vào vận hành ngay. Mặc dù use case phù hợp với LLM feature và có boundary rõ ràng, dữ liệu được phê duyệt, baseline đáng tin cậy, đánh giá lâm sàng và xác nhận stakeholder vẫn chưa có trong bài lab. Bước tiếp theo là **prototype offline trên dữ liệu khử định danh**, đo KPI và phân tích lỗi; chỉ xem xét GO cho pilot giới hạn sau khi các điều kiện đó được ký duyệt.
