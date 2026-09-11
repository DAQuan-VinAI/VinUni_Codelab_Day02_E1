# AI Log & Reflection — Lab 02

## Mục tiêu và cách dùng AI

Tôi dùng AI như một thought partner để mở rộng danh sách pain point, kiểm tra logic của quick cards và phản biện rủi ro trong use case Vinmec discharge summary. Tôi không dùng AI như nguồn xác nhận số liệu vận hành hoặc quyết định lâm sàng.

## Nhật ký tương tác

| Lần | Prompt/tác vụ | AI giúp gì | Điều tôi kiểm tra hoặc sửa |
|---:|---|---|---|
| 1 | Brainstorm 5 quy trình tốn thời gian ở Vinmec/VinFast/Vinpearl | Đề xuất nhóm cơ hội theo tính lặp lại, dữ liệu đầu vào và người chịu trách nhiệm. | Loại các ý tưởng quá rộng; giữ các workflow có actor, handoff và metric có thể đo. |
| 2 | “Đóng vai CFO và trưởng vận hành, phản biện Card discharge summary.” | Chỉ ra rủi ro KPI thiếu baseline và nguy cơ coi bản nháp như tư vấn y tế. | Gắn nhãn toàn bộ số liệu là giả định pilot; bổ sung audit baseline và KPI an toàn. |
| 3 | So sánh rule, LLM feature và agent | Làm rõ rule phù hợp với validation, LLM phù hợp với tóm tắt, agent chưa cần thiết. | Chọn LLM feature + rule validation thay vì agentic loop. |
| 4 | Stress-test boundary | Đề xuất các cách prompt injection có thể yêu cầu bỏ qua review hoặc “ký hộ”. | Viết boundary cấm AI chẩn đoán/kê thuốc/ký/gửi; thêm fallback khi thiếu hay mâu thuẫn dữ liệu. |

## Một điểm AI trả lời chưa tốt

AI từng gợi ý KPI như “giảm 70% thời gian” và giả định rằng mọi trường EHR đều sẵn sàng để trích xuất. Đây là kết luận quá tự tin: cấu trúc dữ liệu, quyền truy cập, chất lượng ghi chú và quy trình ký đều chưa được xác minh. Nếu dùng nguyên văn, nhóm có thể hứa hẹn lợi ích không có chứng cứ.

Tôi sửa bằng cách chuyển các số liệu thành **pilot assumptions**, thêm yêu cầu đo baseline trên ca đại diện, đặt metric an toàn bên cạnh metric tốc độ và đưa quyết định cuối thành **NOT YET** thay vì GO.

## Reflection

AI hữu ích nhất khi giúp tạo phương án thay thế và đóng vai người phản biện khó tính. Giá trị của tôi là kiểm tra bối cảnh, xác định điều gì không được phép suy diễn và thiết kế cơ chế chịu trách nhiệm. Với dữ liệu y tế, một câu trả lời trôi chảy không phải là bằng chứng đúng; bác sĩ, bảo mật dữ liệu và quy trình review vẫn là các control bắt buộc.
