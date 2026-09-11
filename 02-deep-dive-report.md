# Lab 02 — Deep Dive Report: AI Product Analysis

---

## 🎯 Bài toán được chọn để phân tích sâu

**Vinmec - Trợ lý Ghi chép Bệnh án Thông minh (Medical Scribe AI)**

---

## 🏗️ Phase 3.1 — Current-State Workflow Mapping

### Quy trình hiện tại: Ghi chép Bệnh án sau Khám bệnh

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CURRENT-STATE WORKFLOW                           │
└─────────────────────────────────────────────────────────────────────┘

[Bệnh nhân đến khám]
         │
         ▼
┌────────────────────────┐
│ 1. Bác sĩ tiếp nhận    │  ⏱ 2 phút
│    và xem hồ sơ cũ    │  - Đọc lịch sử bệnh án trước
└────────────────────────┘  - Xem kết quả xét nghiệm
         │
         ▼
┌────────────────────────┐
│ 2. Khám và trò chuyện  │  ⏱ 15-20 phút
│    với bệnh nhân       │  - Hỏi triệu chứng
└────────────────────────┘  - Khám lâm sàng
         │                  - Giải thích chẩn đoán
         ▼
┌────────────────────────┐
│ 3. Kê đơn thuốc        │  ⏱ 3-5 phút
│                        │  - Chọn thuốc
└────────────────────────┘  - Ghi liều lượng
         │
         ▼ 🔄 HANDOFF (Bệnh nhân rời phòng khám)
         │
         ▼
┌────────────────────────┐  🔴 BOTTLENECK
│ 4. Ghi chép tóm tắt    │  ⏱ 12-18 phút (trung bình 15 min)
│    bệnh án THỦ CÔNG    │  - Ghi lại triệu chứng chính
└────────────────────────┘  - Kết quả khám
         │                  - Chẩn đoán
         │                  - Kế hoạch điều trị
         │                  - Lời khuyên cho bệnh nhân
         ▼
         │  VẤN ĐỀ:
         │  • Bác sĩ mệt mỏi sau nhiều ca → ghi chép sơ sài
         │  • Thông tin quan trọng có thể bị bỏ sót
         │  • Bác sĩ phải nhớ lại cuộc trò chuyện
         │
         ▼
┌────────────────────────┐
│ 5. Nhập vào hệ thống   │  ⏱ 3-5 phút
│    HIS (Hospital Info) │  - Copy/paste vào form
└────────────────────────┘  - Chọn mã ICD-10
         │                  - Upload đơn thuốc
         ▼
┌────────────────────────┐
│ 6. In và lưu trữ       │  ⏱ 2 phút
└────────────────────────┘

═══════════════════════════════════════════════════════════════
TỔNG THỜI GIAN: 37-52 phút/ca khám (trung bình: 45 phút)
Trong đó: 20 phút là công việc HÀNH CHÍNH (ghi chép, nhập liệu)
═══════════════════════════════════════════════════════════════
```

### Phân tích Bottleneck chi tiết

| Bottleneck | Mô tả | Impact |
|------------|-------|--------|
| 🔴 **Bước 4: Ghi chép thủ công** | Bác sĩ phải viết/đánh máy tóm tắt chi tiết sau mỗi ca khám | **44% thời gian** của toàn bộ quy trình |
| ⚠️ **Chất lượng không đồng đều** | Bác sĩ mệt → ghi chép ngắn gọn, thiếu chi tiết | Ảnh hưởng đến chất lượng chăm sóc lâu dài |
| ⚠️ **Giảm thời gian tương tác** | Bác sĩ vừa khám vừa lo ghi chép → ít eye contact | Bệnh nhân cảm thấy không được quan tâm |
| ⚠️ **Burnout** | Bác sĩ làm việc 8-10 giờ, trong đó 3-4 giờ là ghi chép | Mệt mỏi, giảm hiệu suất |

---

## 📋 Phase 3.2 — Problem Statement (6-field)

| Field | Nội dung chi tiết |
|-------|-------------------|
| **1. Actor / Operator** | **Bác sĩ lâm sàng tại Vinmec** (500+ bác sĩ, xử lý 15,000+ ca khám/ngày) |
| **2. Current Workflow** | Bác sĩ khám bệnh 15-20 phút, sau đó dành 15 phút ghi chép tóm tắt bệnh án thủ công (triệu chứng, chẩn đoán, kế hoạch điều trị) vào sổ giấy/máy tính, rồi nhập vào hệ thống HIS. Không có công cụ hỗ trợ tự động. |
| **3. Bottleneck** | **Ghi chép tóm tắt bệnh án (15 min/ca)** tốn thời gian, dễ sai sót, và làm giảm thời gian tương tác với bệnh nhân. Bác sĩ phải nhớ lại chi tiết cuộc trò chuyện sau khi bệnh nhân rời đi. Chất lượng ghi chép giảm dần theo thời gian làm việc (burnout). |
| **4. Business Impact** | • **Mất 3-4 giờ/ngày** của mỗi bác sĩ cho công việc hành chính<br>• Giảm **20-25% năng suất khám bệnh** (có thể khám thêm 4-5 ca/ngày nếu không ghi chép)<br>• **Chi phí cơ hội:** 500 bác sĩ × 4 giờ × 250 ngày = 500,000 giờ/năm = ~25 triệu USD/năm (assuming $50/giờ)<br>• **Điểm hài lòng bệnh nhân:** 7.8/10 (phàn nàn: bác sĩ ít giao tiếp, vội vàng) |
| **5. Success Metric** | • **Giảm thời gian ghi chép** từ 15 phút → **dưới 3 phút** (bác sĩ chỉ cần review & approve)<br>• **Tăng số ca khám** từ 20 → **24 ca/ngày** (+20%)<br>• **Tăng điểm hài lòng** từ 7.8 → **8.5+/10**<br>• **Độ chính xác:** 95%+ tóm tắt được bác sĩ approve không cần chỉnh sửa lớn |
| **6. Operational Boundary** | ✅ **AI ĐƯỢC PHÉP:**<br>• Ghi âm cuộc trò chuyện (có đồng ý của bệnh nhân)<br>• Chuyển đổi speech-to-text<br>• Trích xuất thông tin và tạo tóm tắt có cấu trúc<br>• Đề xuất mã ICD-10<br><br>🛑 **AI TUYỆT ĐỐI KHÔNG:**<br>• Tự động lưu tóm tắt mà không có sự duyệt của bác sĩ (HITL bắt buộc)<br>• Tự ý thay đổi chẩn đoán hoặc đơn thuốc<br>• Lưu trữ ghi âm sau khi tạo tóm tắt (xóa ngay, tuân thủ GDPR/privacy)<br>• Truy cập hồ sơ bệnh nhân khác không liên quan<br><br>🟢 **HUMAN-IN-THE-LOOP:**<br>• Bác sĩ **PHẢI** review và approve/edit tóm tắt trước khi lưu vào HIS<br>• Nút "Regenerate" nếu tóm tắt không chính xác<br>• Nút "Manual Override" để ghi chép thủ công nếu AI lỗi |

---

## 🔮 Phase 3.3 — Future-State Flow & AI Fit

### AI Fit Matrix

| Phương pháp | Khả thi? | Lý do |
|-------------|----------|-------|
| **Rule-based / Regex** | ❌ Không | Ngôn ngữ tự nhiên trong cuộc trò chuyện quá phức tạp, đa dạng, không thể dùng rule cứng |
| **Traditional ML** | ⚠️ Khó | Cần labeled data lớn, không linh hoạt với các trường hợp mới |
| **LLM (GPT/Gemini)** | ✅ **Khả thi cao** | LLM có khả năng:<br>• Hiểu ngôn ngữ tự nhiên phức tạp<br>• Trích xuất thông tin có cấu trúc<br>• Tạo văn bản tóm tắt mạch lạc<br>• Reasoning để nhận diện thông tin quan trọng |
| **Agentic Loop** | ⚠️ Không cần thiết | Bài toán này không cần multi-step tool use, chỉ cần 1 lần inference |

**Lựa chọn:** **LLM Feature (Single-shot with structured output)**

### Future-State Workflow (Có tích hợp AI)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FUTURE-STATE WORKFLOW (WITH AI)                  │
└─────────────────────────────────────────────────────────────────────┘

[Bệnh nhân đến khám]
         │
         ▼
┌────────────────────────┐
│ 1. Bác sĩ tiếp nhận    │  ⏱ 2 phút
│    + NHẤN "START REC"  │  🎙️ Bắt đầu ghi âm (có consent bệnh nhân)
└────────────────────────┘
         │
         ▼
┌────────────────────────┐
│ 2. Khám và trò chuyện  │  ⏱ 15-20 phút
│    HOÀN TOÀN TỰ NHIÊN  │  🎙️ Ghi âm trong nền
└────────────────────────┘  - Bác sĩ tập trung 100% vào bệnh nhân
         │                  - Không cần lo ghi chép
         ▼
┌────────────────────────┐
│ 3. Kê đơn thuốc        │  ⏱ 3-5 phút
└────────────────────────┘
         │
         ▼
┌────────────────────────┐
│ 4. NHẤN "STOP & GEN"   │  🤖 AI Processing
│    🔵 AI Step          │  ⏱ 30-45 giây
└────────────────────────┘
         │
         ├─> Speech-to-Text (Whisper/Gemini)
         ├─> LLM trích xuất thông tin
         └─> Tạo tóm tắt có cấu trúc
         │
         ▼
┌────────────────────────┐  🟢 HUMAN-IN-THE-LOOP (HITL)
│ 5. Bác sĩ REVIEW       │  ⏱ 2-3 phút
│    tóm tắt AI tạo      │  
└────────────────────────┘  
         │                   ACTIONS:
         ├─> ✅ [APPROVE] → Lưu vào HIS
         ├─> ✏️ [EDIT] → Sửa chi tiết → Approve
         └─> 🔄 [REGENERATE] → AI tạo lại
         │
         ▼
┌────────────────────────┐
│ 6. Tự động lưu HIS     │  ⏱ 10 giây (automated)
│    + XÓA GHI ÂM        │  🗑️ Privacy compliance
└────────────────────────┘

═══════════════════════════════════════════════════════════════
TỔNG THỜI GIAN: 23-31 phút/ca khám (trung bình: 27 phút)
TIẾT KIỆM: 18 phút/ca (40% thời gian!)
═══════════════════════════════════════════════════════════════
```

### Structured Output Template (JSON)

```json
{
  "patient_id": "VM123456",
  "visit_date": "2026-09-11",
  "chief_complaint": "Đau đầu, chóng mặt kéo dài 3 ngày",
  "history_present_illness": "Bệnh nhân than phiền đau đầu vùng thái dương hai bên, tăng dần trong 3 ngày qua. Kèm theo chóng mặt nhẹ khi đứng dậy. Không sốt, không nôn. Ngủ kém, stress công việc.",
  "vital_signs": {
    "blood_pressure": "140/85 mmHg",
    "heart_rate": "78 bpm",
    "temperature": "36.8°C"
  },
  "physical_examination": "Tri giác tỉnh, tiếp xúc tốt. Không có dấu hiệu thần kinh khu trú. Cổ không cứng. Tim phổi bình thường.",
  "diagnosis": {
    "primary": "Đau đầu căng thẳng (Tension headache)",
    "icd10_code": "G44.2"
  },
  "treatment_plan": "Paracetamol 500mg x 2 viên khi đau. Nghỉ ngơi, tránh stress. Tái khám sau 7 ngày nếu không cải thiện.",
  "medications": [
    {
      "name": "Paracetamol",
      "dosage": "500mg",
      "frequency": "Khi đau, tối đa 3g/ngày",
      "duration": "7 ngày"
    }
  ],
  "follow_up": "Tái khám sau 1 tuần nếu triệu chứng không giảm hoặc xuất hiện triệu chứng mới",
  "confidence_score": 0.94
}
```

### Fallback Strategy (Kế hoạch dự phòng)

| Tình huống lỗi | Fallback Action |
|----------------|-----------------|
| **AI trả về confidence < 0.7** | Cảnh báo bác sĩ, highlight các phần không chắc chắn, đề xuất ghi chép thủ công |
| **Speech-to-text lỗi (ồn ào)** | Hiển thị cảnh báo "Chất lượng âm thanh kém", đề xuất ghi lại hoặc ghi chép thủ công |
| **API timeout (> 60s)** | Fallback về workflow thủ công, lưu ghi âm để xử lý sau (batch mode) |
| **Bác sĩ reject tóm tắt 3 lần** | Chuyển sang manual mode, gửi feedback để cải thiện model |

---

## 🏁 Phase 5 — EVALUATE & DECISION

### AI Readiness Checklist

| Câu hỏi | Trả lời | Ghi chú |
|---------|---------|---------|
| 1. Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | ✅ **CÓ** | Vinmec có 10,000+ bệnh án mẫu (đã de-identify). Có thể simulate 100 cuộc trò chuyện với bác sĩ để test. |
| 2. Rủi ro khi AI sai có nằm trong tầm kiểm soát? | ✅ **CÓ** | HITL bắt buộc: Bác sĩ review 100% tóm tắt trước khi lưu. AI chỉ là trợ lý, không tự quyết định. |
| 3. Stakeholders sẵn sàng thay đổi quy trình? | ⚠️ **CẦN XÁC NHẬN** | Cần pilot với 10-20 bác sĩ early-adopter để thu thập feedback. Một số bác sĩ lớn tuổi có thể kháng cự công nghệ. |
| 4. Tuân thủ quy định pháp lý? | ⚠️ **CẦN XỬ LÝ** | Cần đảm bảo tuân thủ:<br>• Luật Khám chữa bệnh VN<br>• GDPR/Privacy (xóa ghi âm sau xử lý)<br>• Consent bệnh nhân (opt-in)<br>• Data residency (data không ra khỏi VN) |
| 5. Có ngân sách & resources? | ✅ **CÓ** | Vingroup có ngân sách đầu tư AI. Team Vin Smart Future có kỹ sư AI và infra cloud. |

### Phân tích Chi phí - Lợi ích (Cost-Benefit)

**Chi phí (Cost):**
- Development: 6 tháng × 3 engineers × $5,000/tháng = **$90,000**
- API cost (Gemini): 500 bác sĩ × 20 ca/ngày × 250 ngày × $0.10/ca = **$250,000/năm**
- Training & change management: **$50,000**
- **TỔNG: $390,000 năm đầu**

**Lợi ích (Benefit):**
- Tiết kiệm thời gian: 500 bác sĩ × 3 giờ/ngày × 250 ngày × $50/giờ = **$18.75 triệu/năm**
- Tăng doanh thu: +4 ca/ngày × 500 bác sĩ × $50/ca × 250 ngày = **$25 triệu/năm**
- Cải thiện trải nghiệm bệnh nhân → tăng retention 5% → **$2 triệu/năm**
- **TỔNG: $45.75 triệu/năm**

**ROI:** ($45.75M - $0.39M) / $0.39M = **11,569%** 🚀

### Quyết định cuối cùng

☑️ **GO (Bắt đầu xây dựng Prototype)**

**Justification (Lý giải quyết định):**

1. **Impact cao:** Tiết kiệm 18 phút/ca khám = 3-4 giờ/ngày cho mỗi bác sĩ. Với 500 bác sĩ, đây là khoản tiết kiệm khổng lồ.

2. **Khả thi kỹ thuật:** LLM hiện tại (Gemini 2.5, GPT-4) đã đủ mạnh để xử lý bài toán này. Đã có nhiều sản phẩm tương tự trên thị trường (Nuance DAX, Suki.ai).

3. **Rủi ro được kiểm soát:** HITL bắt buộc đảm bảo bác sĩ luôn là người ra quyết định cuối cùng. AI chỉ là trợ lý.

4. **ROI cực cao:** Payback period < 1 tháng. Lợi ích vượt xa chi phí.

5. **Phù hợp chiến lược:** Vingroup đang đẩy mạnh chuyển đổi số và AI. Đây là use case showcase tốt.

**Phương án triển khai:**
- **Phase 1 (3 tháng):** Pilot với 10 bác sĩ tại 1 phòng khám Vinmec Hà Nội
- **Phase 2 (3 tháng):** Mở rộng lên 50 bác sĩ, thu thập feedback, fine-tune model
- **Phase 3 (6 tháng):** Rollout toàn quốc 500+ bác sĩ

**Rủi ro cần theo dõi:**
- Adoption rate của bác sĩ (mục tiêu: >80% sử dụng thường xuyên)
- Độ chính xác của AI (mục tiêu: >90% approved without major edits)
- Tuân thủ pháp lý (cần consult với luật sư y tế)

---

## 📊 Summary Metrics

| Metric | Hiện tại | Mục tiêu (sau 6 tháng) | Cải thiện |
|--------|----------|------------------------|-----------|
| Thời gian ghi chép/ca | 15 phút | 3 phút | **-80%** |
| Số ca khám/ngày | 20 ca | 24 ca | **+20%** |
| Điểm hài lòng bệnh nhân | 7.8/10 | 8.5/10 | **+9%** |
| Thời gian hành chính/ngày | 4 giờ | 1 giờ | **-75%** |
| Chi phí cơ hội tiết kiệm | - | $18.75M/năm | - |

---

**Người thực hiện:** Bùi Tùng Dương  
**Ngày hoàn thành:** 11/09/2026  
**Branch:** DuongBT

