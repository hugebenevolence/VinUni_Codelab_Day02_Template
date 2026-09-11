# 01 — Problem Scan & Quick-Assess (Cá nhân)

> **Người thực hiện:** [Điền tên bạn]
> **Vai trò:** AI Product Engineer tại Vin Smart Future
> **Phương pháp:** Các bài toán dưới đây được chọn lọc dựa trên khảo sát/tin tức chính thức 2025-2026 của Vingroup và các công ty thành viên (nguồn dẫn kèm theo mỗi dòng), ưu tiên các điểm nghẽn **còn đang tồn đọng** (chưa được AI/quy trình mới giải quyết dứt điểm) thay vì các vấn đề đã có giải pháp tốt (ví dụ: chẩn đoán hình ảnh AI tại Vinmec, trợ lý ảo hỏi-đáp chung tại Vinhomes — hai mảng này đã được xử lý hiệu quả nên không đưa vào danh sách cơ hội).

---

# 🔍 Phase 1 — SCAN

| # | Subsidiary | Lens | Mô tả ngắn bài toán | Nguồn / căn cứ |
|---|---|---|---|---|
| 1 | **VinFast** | Tốn thời gian | Chương trình "Kiến tạo Dịch vụ 5 Sao" (6–8/2026) đang khảo sát vì khách hàng còn phàn nàn ở khâu **đặt lịch hẹn, tiếp đón, tư vấn bàn giao xe** tại xưởng dịch vụ. | Khảo sát 300.000 khách hàng (10/2025) về thái độ NV & tốc độ sửa chữa; chương trình 5-Sao 2026 — [tuoitre.vn](https://tuoitre.vn/khoahocphothong/vinfast-khao-sat-y-kien-khach-hang-nham-nang-cao-chat-luong-dich-vu-104261745.htm), [vinfastauto.com](https://vinfastauto.com/vn_vi/vinfast-tiep-tuc-trien-khai-chuong-trinh-kien-tao-dich-vu-5-sao-dong-nhat-chuan-dich-vu-nang-tam-trai-nghiem-khach-hang) |
| 2 | **Xanh SM** | Pain từ người khác | Tài xế bị cảnh báo gian lận/tỉ lệ nhận chuyến thấp phải "giải trình" — công ty vừa phải bổ sung tính năng **Giải trình trực tuyến** (12/2025) vì quy trình cũ gây tranh cãi kéo dài, thiếu minh bạch. | Fraud Warning Level (10/2025) + Giải trình trực tuyến (12/2025) — [kenh14.vn](https://kenh14.vn/thong-bao-den-tai-xe-xanh-sm-215251215220945705.chn), [xanhsm.com](https://www.xanhsm.com/news/cap-nhat-bo-quy-tac-ung-xu-danh-cho-doi-tac-tai-xe-bike) |
| 3 | **Vinmec** | Stakeholder Pain | Dù đã đặt lịch hẹn, bệnh nhân vẫn khó liên hệ hotline để đặt/xác nhận lịch khám và vẫn chờ lâu tại viện — khác với chẩn đoán hình ảnh/hồ sơ bệnh án (đã được AI xử lý tốt, giảm 67-80% thời gian). | [pharmacity.vn](https://www.pharmacity.vn/benh-vien-vinmec.htm), [dantri.com.vn](https://dantri.com.vn/suc-khoe/dot-pha-cong-nghe-ai-tai-vinmec-tiet-kiem-80-thoi-gian-xu-ly-ho-so-y-te-20241206211249245.htm) |
| 4 | **Vinhomes** | Lặp lại | Trợ lý ảo trên App Resident mới xử lý hỏi-đáp chung; các **yêu cầu bảo trì/phản ánh sự cố cụ thể theo từng tòa nhà** vẫn cần định tuyến thủ công đến đúng ban quản lý địa phương. | Trợ lý ảo ra mắt trên Vinhomes Resident/Online chỉ hỗ trợ hỏi đáp — [vingroup.net](https://vingroup.net/tin-tuc-su-kien/bai-viet/2598/ra-mat-tro-ly-ao-tren-ung-dung-vinhomes-resident-va-vinhomes-online) |
| 5 | **Vin Smart Future (toàn hệ sinh thái)** | AI-upgrade | V-App (ra mắt 4/2026) mới tích hợp LLM cho CSKH ở mức super-app chung; quy trình CSKH riêng của từng công ty con (VinFast, Xanh SM...) vẫn vận hành rời rạc, chưa đồng bộ vào nền tảng LLM chung. | Chiến lược LLM trên V-App công bố tại ĐHCĐ Vingroup 2026 — [vnexpress.net](https://vnexpress.net/vingroup-phat-trien-ngon-ngu-llm-trong-chien-luoc-ai-5067101.html) |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

## Quick Problem Card #1 — VinFast: Đặt lịch & Tiếp đón dịch vụ

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                               │
│ Bài toán: Khách hàng đặt lịch bảo dưỡng/sửa chữa qua app/    │
│ hotline nhưng lịch không đồng bộ với thực tế xưởng dịch vụ,  │
│ khâu tiếp đón & tư vấn bàn giao thiếu chuẩn hóa.             │
│ Công ty thành viên: [x] VinFast                              │
│                                                               │
│ Ai đang đau (Actor)? Cố vấn dịch vụ (Service Advisor) &      │
│ khách hàng đến xưởng.                                        │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                         │
│   1. KH đặt lịch qua app/hotline                             │
│   → 2. Lễ tân xác nhận thủ công, đối chiếu lịch xưởng thực tế│
│   → 3. KH đến xưởng, lễ tân tiếp nhận xe, ghi nhận yêu cầu   │
│   → 4. Cố vấn dịch vụ tư vấn tình trạng xe/hạng mục sửa      │
│   → 5. Bàn giao xe & tư vấn kết quả cho KH                   │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ ~15-20        │
│ phút chờ xác nhận lịch trùng, tư vấn bàn giao không đồng     │
│ nhất giữa các đại lý)                                        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (đồng bộ lịch   │
│ trống thực) & Bước 4 (soạn báo cáo tư vấn chuẩn hóa - draft) │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xác nhận lịch từ ~15 phút → dưới 3 phút;      │
│ 90% báo cáo tư vấn bàn giao đạt chuẩn nội dung thống nhất.   │
│                                                               │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule cho lịch)  │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Problem Card #2 — Xanh SM: Giải trình vi phạm của tài xế

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                               │
│ Bài toán: Tài xế bị hệ thống cảnh báo gian lận/tỉ lệ nhận    │
│ chuyến thấp phải nộp giải trình, nhưng đội Compliance xử lý  │
│ thủ công từng case, đối chiếu dữ liệu chậm, thiếu minh bạch. │
│ Công ty thành viên: [x] Xanh SM (GSM)                        │
│                                                               │
│ Ai đang đau (Actor)? Nhân viên Compliance (quá tải case) &   │
│ Tài xế (chờ đợi, bức xúc vì thiếu minh bạch).                │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                         │
│   1. Hệ thống tự động cảnh báo vi phạm (Fraud Warning Level) │
│   → 2. Tài xế nộp giải trình qua app (text/ảnh minh chứng)   │
│   → 3. Nhân viên Compliance đọc thủ công từng giải trình     │
│   → 4. Đối chiếu dữ liệu chuyến đi, GPS, lịch sử vi phạm     │
│   → 5. Ra quyết định (gỡ cảnh báo / duy trì xử lý)           │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ ước tính      │
│ 10-15 phút/case do khối lượng lớn, đọc & đối chiếu thủ công) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4 (tóm tắt giải │
│ trình, đối chiếu dữ liệu tự động, đề xuất quyết định sơ bộ)  │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý 1 case từ ~15 phút → dưới 5 phút;       │
│ Tăng tỉ lệ case được phản hồi trong 24h lên 95%.             │
│                                                               │
│ Quick Architecture: [x] LLM Feature (luôn có HITL vì ảnh     │
│ hưởng thu nhập tài xế — AI chỉ đề xuất, người duyệt)         │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Problem Card #3 — Vinhomes: Định tuyến yêu cầu bảo trì/phản ánh

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                               │
│ Bài toán: Cư dân gửi phản ánh sự cố (điện/nước/an ninh...)   │
│ qua App Resident, nhưng nhân viên tổng đài Ban Quản lý phải  │
│ đọc và định tuyến thủ công đến đúng bộ phận kỹ thuật của     │
│ đúng tòa nhà — trợ lý ảo hiện tại chỉ hỗ trợ hỏi-đáp chung.  │
│ Công ty thành viên: [x] Vinhomes                             │
│                                                               │
│ Ai đang đau (Actor)? Nhân viên tổng đài/lễ tân Ban Quản lý & │
│ cư dân chờ xử lý sự cố.                                      │
│                                                               │
│ Workflow thủ công hiện tại (5 bước):                         │
│   1. Cư dân gửi phản ánh (text mô tả sự cố) qua App Resident │
│   → 2. Nhân viên đọc và phân loại loại sự cố                 │
│   → 3. Xác định mức độ khẩn cấp                              │
│   → 4. Định tuyến thủ công đến đúng bộ phận kỹ thuật/tòa nhà │
│   → 5. Bộ phận kỹ thuật xử lý & phản hồi cư dân              │
│                                                               │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4 (⏱ ~8-10 phút/case │
│ do khối lượng phản ánh lớn, nhiều tòa nhà, dễ định tuyến sai)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4 (tự động phân │
│ loại loại sự cố + mức độ khẩn cấp + định tuyến ngay từ đầu)  │
│                                                               │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian định tuyến ban đầu từ ~10 phút → dưới 1 phút; │
│ 90% phản ánh được định tuyến đúng bộ phận ngay lần đầu.      │
│                                                               │
│ Quick Architecture: [x] LLM Feature (classification &        │
│ routing, fallback về nhân viên nếu độ tin cậy thấp)          │
└─────────────────────────────────────────────────────────────┘
```

---

## 📌 Ghi chú lựa chọn (để chuẩn bị cho Deep-Dive nhóm)
Cả 3 card đều có căn cứ thực tế từ khảo sát/tính năng mới ra mắt 2025-2026, cho thấy đây là các điểm nghẽn **đang được chính Vingroup xác nhận là chưa giải quyết dứt điểm**, phù hợp để nhóm tiếp tục Deep-Dive (Phase 3) chọn ra 1 bài toán duy nhất.
