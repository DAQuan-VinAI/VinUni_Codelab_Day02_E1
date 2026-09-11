# Nhật ký làm việc cùng AI (AI Reflection Log)

- **Bài học về Scoping bài toán AI:**
  Thay vì cố gắng biến AI thành một "Agent toàn năng" tự động bấm hoàn tiền và giao tiếp trực tiếp với khách hàng (rủi ro vận hành và tài chính cực cao), việc giới hạn ranh giới (Operational Boundary) ở vai trò "Trợ lý hỗ trợ CSKH Tier-1" với cơ chế HITL giúp giải pháp trở nên khả thi và an toàn tuyệt đối.

- **Bài học về Prompting & Ranh giới an toàn:**
  - Cần sử dụng Structured Outputs (Pydantic Schema) thay vì prompt text tự do để đảm bảo hệ thống backend đọc được kết quả với tỷ lệ lỗi parser bằng 0.
  - Các kỹ thuật Prompt Injection đòi hỏi System Instruction phải có ranh giới cấm rõ ràng (Banned Actions) và cơ chế phân luồng Fallback khi độ tin cậy thấp hoặc có cảnh báo rủi ro an toàn (`safety_flag`).