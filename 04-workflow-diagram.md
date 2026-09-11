```text
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ Bước 1       │    │ Bước 2       │    │ Bước 3       │    │ Bước 4       │
│ Khách xuất   │    │ Nhân viên    │    │ Đối chiếu    │    │ Tra cứu thủ  │
│ trình mã vé  │ ──→│ cầm máy quét │ ──→│ danh sách vé │ ──→│ công phân    │
│ đoàn / QR    │    │ QR quét mã   │    │ đoàn trên file│   │ khúc khách   │
│              │    │              │    │ Excel/giấy   │    │ ưu tiên      │
│ Ai: Nhân viên│    │ Ai: Nhân viên│    │ Ai: Nhân viên│    │ Ai: Nhân viên│
│ ⏱ 30 giây   │    │ ⏱ 30 giây   │    │ ⏱ 2 phút 🔴  │    │ ⏱ 1.5 phút 🔴│
│ In: Vé QR/Giấy│   │ In: Sóng RFID│    │ In: List Excel│   │ In: Sổ tay   │
│ Out: Tín hiệu│    │ Out: Log quét│    │ Out: Trạng thái│  │ Out: Quyết định│
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
                                                                    │
                                                                    ▼
                                                            ┌──────────────┐
                                                            │ Bước 5       │
                                                            │ Mở cổng cho  │
                                                            │ khách qua    │
                                                            │              │
                                                            │ Ai: Nhân viên│
                                                            │ ⏱ 30 giây   │
                                                            └──────────────┘
🔴 = Bottlenecks
⏱ Tổng thời gian xử lý thủ công cho mỗi đoàn khách lớn: ~5 phút/lượt.

```
