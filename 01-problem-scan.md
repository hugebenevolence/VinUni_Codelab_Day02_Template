# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng 4 Lenses quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Tài xế báo sự cố hết pin hoặc pin yếu giữa đường, đội điều phối phải liên tục tra cứu trạm sạc gần nhất và soạn tin nhắn chỉ đường thủ công. |
| 2 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công các phản hồi khẩn cấp từ tài xế về sự cố sạc pin, mất 10–15 phút/lượt vì phải tra cứu vị trí, trạm sạc và viết hướng dẫn. |
| 3 | **VinFast** | Lặp lại | So khớp hóa đơn sạc điện và đối chiếu dữ liệu trạm sạc đối tác, phần lớn là thao tác lặp lại và kiểm tra thủ công mỗi tuần. |
| 4 | **Vinhomes** | AI-upgrade | Hệ thống phản hồi cư dân trên app và hotline còn xử lý rập khuôn, làm chậm phản hồi khiếu nại và tăng tải cho nhân viên CSKH. |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ phải tóm tắt hồ sơ bệnh án xuất viện và hỗ trợ chẩn đoán nhanh, gây tải công việc và mất thời gian trong khâu quản lý hồ sơ. |
| 6 | **Vinpearl / VinWonders** | Tốn thời gian | Nhân viên chăm sóc khách hàng phải soạn tin nhắn cá nhân hóa cho du khách, hướng dẫn dịch vụ và xử lý lịch trình ở hàng trăm booking mỗi ngày. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

## Quick Problem Card #1 — Xanh SM xử lý sự cố pin

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo hết pin giữa đường cần hỗ trợ │
│ nhanh để tiếp tục hành trình hoặc gọi cứu hộ.               │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau (Actor)? Tài xế và điều phối viên.             │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                       │
│   1. Tài xế gọi tổng đài sự cố pin                          │
│   → 2. Điều phối viên tra cứu vị trí GPS xe                 │
│   → 3. Tra cứu trạm sạc VinFast còn trụ trống gần nhất      │
│   → 4. Soạn tin nhắn chỉ dẫn cho tài xế                    │
│   → 5. Gọi xe cứu hộ nếu pin dưới ngưỡng an toàn          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 12 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4 (tra cứu và │
│ tự động soạn nháp chỉ dẫn).                                 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút.   │
│                                                             │
│ Quick Architecture: [x] LLM Feature                          │
└─────────────────────────────────────────────────────────────┘
```

## Quick Problem Card #2 — Vinhomes phản hồi cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phản hồi khiếu nại, yêu cầu hỗ trợ cư dân diễn ra │
│ qua nhiều kênh và cần được xử lý nhanh và nhất quán.        │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH, cư dân.                │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                       │
│   1. Cư dân gửi khiếu nại qua app/hotline                   │
│   → 2. CSKH đọc nội dung, trích lọc chủ đề                  │
│   → 3. Tra cứu thông tin căn hộ / hợp đồng / lịch sử        │
│   → 4. Soạn phản hồi thủ công và chuyển tiếp               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ 20 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động phân loại và  │
│ draft phản hồi đầu tiên cho CSKH.                           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian phản hồi trung bình từ 12 tiếng xuống dưới 2 │
│ tiếng và tăng độ nhất quán câu trả lời.                    │
│                                                             │
│ Quick Architecture: [x] LLM Feature                          │
└─────────────────────────────────────────────────────────────┘
```

## Quick Problem Card #3 — Vinmec tóm tắt hồ sơ bệnh án

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Bác sĩ mất thời gian tóm tắt hồ sơ bệnh án xuất  │
│ viện và thông tin lâm sàng trước khi bàn giao.              │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ, nhân viên y tế.                │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                       │
│   1. Nhân viên thu thập dữ liệu bệnh án                    │
│   → 2. Bác sĩ đọc và lọc thông tin quan trọng               │
│   → 3. Viết bản tóm tắt xuất viện / chăm sóc tiếp theo      │
│   → 4. Gửi lại cho bệnh viện hoặc bệnh nhân                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 20-30 phút)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tạo bản tóm tắt đầu  │
│ tiên và gợi ý nội dung cần xác nhận.                        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian tóm tắt từ 25 phút xuống dưới 5 phút / hồ sơ│
│                                                             │
│ Quick Architecture: [x] LLM Feature                          │
└─────────────────────────────────────────────────────────────┘
```

---

## Quyết định lựa chọn bài toán

Nhóm chọn bài toán **Xanh SM xử lý sự cố pin thực địa** vì đây là bài toán có dữ liệu hiện trường rõ ràng, tác động trực tiếp đến hiệu suất vận hành và có thể giải quyết bằng giải pháp AI đơn giản nhưng rất hiệu quả. Nó phù hợp với định nghĩa AI Feature: có quy trình rõ ràng, rủi ro có thể kiểm soát bằng human-in-the-loop và fallback.
