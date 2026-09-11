# 01 — Problem Scan & Quick Problem Cards (Vin Smart Future)

**Dự án:** Lab 02 — AI Product Scoping (Vin Smart Future — Vingroup)  
**Tác giả:** Trần Cao Thắng (2A202602520)  
**Nhánh Git:** `2A202602520-Tran-Cao-Thang`  
**Đơn vị:** Vin Smart Future — AI Engineering Team  

---

## 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội Tối Ưu Vận Hành (4 Lenses)

Áp dụng 4 lăng kính (Lặp lại, Tốn thời gian, AI-upgrade, Pain từ người khác) để rà soát toàn diện các hoạt động vận hành tại các công ty thành viên thuộc Tập đoàn Vingroup:

| # | Công ty thành viên | Lăng kính (Lens) | Tên bài toán / Bottleneck | Mô tả chi tiết bài toán |
|:---:|:---|:---|:---|:---|
| **1** | **Xanh SM (GSM)** | **Tốn thời gian** | **Xử lý sự cố sạc pin / hết pin thực địa** | Khi tài xế báo pin nguy cấp hoặc gặp sự cố trạm sạc trên đường đón khách, điều phối viên mất 15-20 phút tra cứu bản đồ, tìm trụ sạc tương thích còn trống và soạn tin nhắn hướng dẫn đường đi thủ công. |
| **2** | **Xanh SM (GSM)** | **Lặp lại** | **Tối ưu hóa điểm đón taxi điện (Smart Dispatching)** | Điều phối viên phải liên tục đọc tin nhắn mô tả điểm đón phức tạp từ tài xế/khách hàng (ngõ ngách, cổng phụ khu đô thị) và định vị lại trên bản đồ hàng trăm lần mỗi ngày. |
| **3** | **Vinhomes** | **AI-upgrade** | **Phân loại & điều hướng phản ánh cư dân (Resident Feedback Triage)** | Phản ánh của cư dân trên App Vinhomes Resident (mất nước, tiếng ồn, hỏng đèn, an ninh) được CSKH tiếp nhận và chuyển tiếp thủ công sang các ban quản lý tòa nhà, phản hồi chậm trễ từ 12–24 giờ. |
| **4** | **VinFast** | **AI-upgrade** | **Trợ lý chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt** | Khách hàng mô tả lỗi bằng ngôn ngữ tự nhiên (ví dụ: *"bánh trước kêu lục cục khi qua gờ giảm tốc"*), nhân viên tiếp nhận mất 20 phút tra cứu sổ tay kỹ thuật để phân loại mã lỗi và xếp lịch kỹ thuật viên. |
| **5** | **Vinmec** | **Tốn thời gian** | **Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary)** | Bác sĩ điều trị mất 20–30 phút/bệnh nhân để tổng hợp các kết quả xét nghiệm, chẩn đoán hình ảnh và diễn tiến lâm sàng từ bệnh án điện tử thành bản tóm tắt xuất viện bằng ngôn từ dễ hiểu cho người bệnh. |
| **6** | **Vinpearl** | **Pain từ người khác** | **Lọc & phân tích khẩn cấp đánh giá khách hàng (Review Sentiment)** | Quản lý khách sạn mất 2-3 giờ mỗi ngày đọc các review rải rác trên Booking.com, Agoda, Google Maps, thường xuyên bỏ sót các phản ánh nghiêm trọng về vệ sinh phòng hoặc thái độ phục vụ để xử lý kịp thời. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Lựa chọn top 3 bài toán có tiềm năng tạo đột phá và giá trị kinh tế lớn nhất để đánh giá nhanh:

---

### 📇 Quick Problem Card #1: Xanh SM — Xử lý sự cố sạc pin thực địa

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán (1 câu): Hỗ trợ điều phối viên Xanh SM tra cứu trạm sạc phù hợp     │
│ và soạn thảo tin nhắn hướng dẫn tài xế khi xe báo sự cố pin nguy cấp.       │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)?                                                        │
│ - Điều phối viên (Dispatcher): Quá tải vì thao tác thủ công giờ cao điểm.   │
│ - Tài xế Xanh SM: Lo lắng, chờ đợi xe cạn pin dẫn đến tê liệt đón khách.   │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Nhận cuộc gọi/báo cáo khẩn cấp từ tài xế qua tổng đài                  │
│   → 2. Tra cứu tọa độ GPS xe và % pin trên Dashboard nội bộ                 │
│   → 3. Tra cứu trạm sạc VinFast còn trụ trống & đúng loại cổng (CCS2/GBT)   │
│   → 4. Soạn thảo văn bản hướng dẫn/chỉ đường gửi App tài xế                 │
│   → 5. Liên hệ điều xe sạc lưu động nếu pin cạn kiệt (< 5%)                 │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & Bước 4 (⏱ 10-12 phút/lượt)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & Bước 4                       │
│ (Trích xuất dữ liệu vị trí -> Gợi ý trạm sạc trống -> Tự động draft tin)     │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.               │
│ - Tỉ lệ chỉ dẫn chính xác trạm sạc tương thích đạt >= 98%.                  │
│ - Giảm 70% số vụ xe chết máy giữa đường do cạn kiệt pin.                    │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule kiểm tra pin khắt khe)    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 Quick Problem Card #2: Vinhomes — Phân loại & điều hướng phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán (1 câu): Tự động đọc hiểu phản ánh của cư dân trên App Vinhomes     │
│ Resident, phân loại mức độ khẩn cấp và chuyển tiếp đến đúng bộ phận xử lý.   │
│ Công ty thành viên: [x] Vinhomes                                            │
│                                                                             │
│ Ai đang đau (Actor)?                                                        │
│ - Cư dân Vinhomes: Bực bội vì phản hồi chậm trễ khi gặp sự cố khẩn.         │
│ - Nhân viên CSKH: Mất 8 tiếng/ngày đọc và chuyển tiếp hàng nghìn ticket.     │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Tiếp nhận ticket phản ánh từ ứng dụng Vinhomes Resident                │
│   → 2. Đọc nội dung, xác định tòa nhà, căn hộ và mức độ nghiêm trọng        │
│   → 3. Phân loại thủ công vào chuyên mục (Điện/Nước/An ninh/Vệ sinh)         │
│   → 4. Chuyển tiếp ticket tới Trưởng ban quản lý cụm tòa nhà liên quan      │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 3 (⏱ 8-10 phút/ticket)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & Bước 3                       │
│ (Phân loại văn bản tự động, trích xuất thực thể, gán nhãn độ khẩn cấp)     │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ - Giảm thời gian định tuyến ticket từ 4 giờ ──> dưới 30 giây.               │
│ - Độ chính xác phân loại chuyên mục đạt >= 92%.                             │
│ - Tăng chỉ số hài lòng của cư dân (CSAT) thêm 25%.                          │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Text Classification & Named Entity)     │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 Quick Problem Card #3: VinFast — Chẩn đoán sơ bộ lỗi xe từ mô tả khách hàng

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Bài toán (1 câu): Phân tích mô tả tiếng Việt bằng ngôn ngữ tự nhiên của chủ  │
│ xe điện VinFast để dự đoán nhóm lỗi cơ khí/điện tử và xếp lịch xưởng dịch vụ.│
│ Công ty thành viên: [x] VinFast                                             │
│                                                                             │
│ Ai đang đau (Actor)?                                                        │
│ - Cố vấn dịch vụ xưởng (Service Advisor): Mất thời gian hỏi đi hỏi lại.     │
│ - Chủ xe: Lo lắng không biết xe có an toàn để tiếp tục vận hành hay không.  │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Nhận cuộc gọi hoặc form đăng ký dịch vụ bảo dưỡng/sửa chữa             │
│   → 2. Nghe khách miêu tả hiện tượng lạ bằng ngôn ngữ tự do                 │
│   → 3. Đối chiếu kinh nghiệm cá nhân và tra cứu tài liệu hướng dẫn kỹ thuật │
│   → 4. Soạn phiếu yêu cầu kỹ thuật và phân bổ cầu nâng xưởng phù hợp        │
│                                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & Bước 3 (⏱ 15 phút/lượt)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & Bước 3                       │
│ (Đọc hiểu mô tả âm thanh, vị trí, ngữ cảnh -> Mapping với DTC code dự kiến) │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                       │
│ - Rút ngắn thời gian tiếp nhận xe từ 20 phút ──> dưới 5 phút.               │
│ - Độ chính xác dự đoán ban đầu nhóm bộ phận cần kiểm tra >= 85%.             │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Retrieval-Augmented Generation) │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết Định Lựa Chọn & Phản Biện (Group Selection Rationale)

Sau khi rà soát và phản biện đa chiều dựa trên 3 trụ cột: **Tác động kinh doanh (Impact)**, **Độ khả thi kỹ thuật (Feasibility)**, và **Mức độ rủi ro (Risk)**, nhóm quyết định:

### 🏆 Bài toán được lựa chọn đi tiếp vào Deep-Dive:
👉 **Quick Problem Card #1: Xanh SM — Xử lý sự cố sạc pin / hết pin thực địa**

### 💡 Lý giải nguyên nhân lựa chọn và hoãn các bài toán khác:
1. **Vì sao chọn Card #1 (Xanh SM Sự cố sạc):**
   - **Tác động trực tiếp theo thời gian thực:** Đây là bài toán sống còn ảnh hưởng trực tiếp đến chuỗi vận hành của đội xe taxi điện Xanh SM. Mỗi phút xe dừng hoạt động đều gây thất thoát doanh thu thực tế và ảnh hưởng đến trải nghiệm đón trả khách.
   - **Ranh giới an toàn rõ ràng và có thể kiểm chứng được bằng code:** Ranh giới giữa trạm sạc gần/xa và ngưỡng pin khẩn cấp (< 5%) là tiêu chí định lượng hoàn hảo để thiết lập kiểm soát an toàn bằng code (Guardrails).
   - **Độ sẵn sàng dữ liệu:** Hệ thống quản lý xe của GSM đã có sẵn API định vị GPS và tình trạng trạm sạc VinFast theo thời gian thực.
2. **Vì sao hoãn Card #2 (Vinhomes CSKH):**
   - Mặc dù lưu lượng ticket lớn, nhưng quy trình xử lý khiếu nại dân sự liên quan đến quyền sở hữu, hợp đồng thuê và các khoản phí dịch vụ đòi hỏi rà soát pháp lý chặt chẽ. Nếu AI gán nhầm nhãn hoặc phản hồi sai lệch có thể tạo ra rủi ro pháp lý cao cho ban quản lý. Cần xây dựng rule-based router vững chắc trước khi dùng LLM.
3. **Vì sao hoãn Card #3 (VinFast Chẩn đoán lỗi xe):**
   - Tiếng ồn cơ khí và lỗi vận hành xe điện là lĩnh vực an toàn sinh mạng khắt khe (Safety-Critical). Dữ liệu mô tả âm thanh bằng văn bản có độ nhiễu cao, dễ dẫn đến hiện tượng chẩn đoán sai lệch (hallucination), đòi hỏi phải kết hợp dữ liệu telemetry OBD-II phần cứng chuyên sâu trước khi ứng dụng AI.
