# 02 — Báo Cáo Phân Tích Sâu Bài Toán AI (Deep-Dive Report)

**Dự án:** Lab 02 — AI Product Scoping (Vin Smart Future — Vingroup)  
**Tác giả:** Trần Cao Thắng (2A202602520)  
**Nhánh Git:** `2A202602520-Tran-Cao-Thang`  
**Bài toán lựa chọn:** Trợ lý điều phối thông minh xử lý sự cố sạc pin thực địa cho Taxi điện Xanh SM (GSM)  

---

## 🏛️ 1. Bối Cảnh Thực Tế & Động Lực Dự Án

Là kỹ sư AI thuộc **Vin Smart Future**, chúng tôi phối hợp cùng Khối Vận Hành của **GSM (Xanh SM)** nhằm giải quyết bài toán rò rỉ hiệu suất điều vận xe taxi điện tại các thành phố lớn.

Trong quá trình vận hành thực tế tại Trung tâm Điều vận Xanh SM Hà Nội, đội xe với hàng nghìn phương tiện (VF e34, VF 5 Plus, VF 8) hoạt động liên tục 24/7. Vào các khung giờ cao điểm (thời tiết mưa gió, kẹt xe), số lượng tài xế gặp sự cố pin yếu hoặc trạm sạc dự kiến bị quá tải tăng đột biến. Quy trình điều phối thủ công hiện tại đang là điểm nghẽn nghiêm trọng, khiến thời gian xử lý kéo dài và gây tổn thất lớn về doanh thu và trải nghiệm khách hàng.

---

## 🏗️ 2. Phase 3 — DEEP-DIVE: Phân Tích Chi Tiết Bài Toán

### 2.1. Current-State Workflow Mapping (Sơ đồ quy trình hiện tại)

Hiện tại, quy trình xử lý sự cố pin thực địa của một điều phối viên (Dispatcher) diễn ra hoàn toàn thủ công qua 5 bước tuần tự:

```text
┌────────────────┐     ┌────────────────┐     ┌────────────────┐     ┌────────────────┐     ┌────────────────┐
│ Bước 1         │     │ Bước 2         │     │ Bước 3 🔴      │     │ Bước 4 🔴      │     │ Bước 5         │
│ Nhận cuộc gọi  │ ──> │ Tra cứu định vị│ ──> │ Tra cứu trạm   │ ──> │ Soạn tin nhắn  │ ──> │ Gửi tin nhắn   │
│ & log sự cố    │ 🔄  │ & % pin xe     │ 🔄  │ sạc còn trống  │     │ chỉ dẫn đường  │     │ hoặc gọi xe    │
│                │     │                │     │ đúng chuẩn xe  │     │ đi cho tài xế  │     │ cứu hộ pin     │
│ Ai: Dispatcher │     │ Ai: Dispatcher │     │ Ai: Dispatcher │     │ Ai: Dispatcher │     │ Ai: Dispatcher │
│ ⏱ 1.5 phút    │     │ ⏱ 1.5 phút    │     │ ⏱ 6.0 phút    │     │ ⏱ 5.0 phút    │     │ ⏱ 1.0 phút    │
│ In: Cuộc gọi   │     │ In: Biển số xe │     │ In: Tọa độ GPS │     │ In: Dữ liệu trạm│    │ In: Bản nháp SMS│
│ Out: Ticket ID │     │ Out: Tọa độ, % │     │ Out: Địa chỉ   │     │ Out: SMS Text  │     │ Out: Dispatch  │
└────────────────┘     └────────────────┘     └────────────────┘     └────────────────┘     └────────────────┘

Ký hiệu:
🔴 Bottlenecks: Bước 3 & Bước 4 ngốn đến 11 phút (chiếm 73% thời gian).
🔄 Handoffs: Điểm chuyển giao giữa các hệ thống rời rạc (Tổng đài -> GPS Map -> Dashboard Trụ Sạc VinFast).
⏱ Tổng thời gian xử lý trung bình: 15.0 phút / sự cố.
```

---

### 2.2. Problem Statement (6-Field) — Chuẩn Vin Smart Future

Bảng 6 trường thông tin phân tích bài toán chuẩn xác:

| Trường thông tin | Nội dung chi tiết |
|:---|:---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM (GSM), trực tiếp quản lý và hỗ trợ tài xế xe taxi điện. |
| **2. Current Workflow** | Khi tài xế gọi điện/báo khẩn cấp xe sắp hết pin hoặc trụ sạc dự kiến bị đầy, điều phối viên nhận thông tin, mở bản đồ xe nội bộ để kiểm tra GPS và dung lượng pin, chuyển sang hệ thống VinFast Charging Dashboard để rà soát trụ sạc tương thích còn trống trong khu vực, gõ tin nhắn văn bản chỉ dẫn đường đi cụ thể, và gọi xe cứu hộ nếu pin cạn kiệt. Quy trình 5 bước thủ công mất trung bình **15 phút/lượt**. |
| **3. Bottlenecks** | **Bước 3 & Bước 4 (mất 11/15 phút):** Điều phối viên phải lọc thủ công hàng chục trạm sạc để tìm trụ trống đúng chuẩn cổng sạc (CCS2 cho VF5/VF8), tính toán khoảng cách ước tính và soạn thảo tin nhắn hướng dẫn bằng tiếng Việt thân thiện, rõ ràng. |
| **4. Business Impact** | - Mỗi ngày tại Hà Nội ghi nhận trung bình **~80 sự cố pin/trạm sạc**.<br>- Gây lãng phí **20 giờ làm việc/ngày** của đội ngũ điều vận.<br>- Xe dừng hoạt động gây sụt giảm doanh thu ước tính **120.000.000 VNĐ/tháng** do bỏ lỡ các cuốc đón khách.<br>- Tăng tỉ lệ tài xế hoảng loạn và nguy cơ xe chết máy giữa đường gây ùn tắc giao thông đô thị và ảnh hưởng xấu đến thương hiệu Xanh SM. |
| **5. Success Metrics** | 1. **Hiệu suất vận hành (Efficiency):** Giảm thời gian xử lý sự cố từ **15 phút xuống dưới 3 phút/lượt** (giảm 80% thời gian).<br>2. **Chất lượng đề xuất (Quality):** Tỉ lệ đề xuất trạm sạc chính xác, đúng cổng sạc và còn trụ trống đạt **>= 98%**.<br>3. **An toàn pin tuyệt đối (Safety):** 100% các trường hợp pin nguy cấp (< 5%) được kích hoạt phương án cứu hộ pin di động ngay lập tức, không để xe bị cạn pin giữa đường. |
| **6. Operational Boundaries (Ranh giới an toàn)** | **CÁC RANH GIỚI BẮT BUỘC BẰNG QUY TẮC & CODE:**<br>1. **Bắt buộc Human-in-the-loop (HITL):** Mọi tin nhắn chỉ dẫn do AI sinh ra BẮT BUỘC phải bắt đầu bằng thẻ `[DRAFT_ONLY]`. AI TUYỆT ĐỐI KHÔNG ĐƯỢC tự động gửi tin nhắn trực tiếp cho tài xế nếu chưa có cú click duyệt từ điều phối viên con người.<br>2. **Ranh giới khoảng cách khi pin nguy cấp (< 5%):** Khi dung lượng pin của xe dưới 5%, AI TUYỆT ĐỐI KHÔNG ĐƯỢC đề xuất trạm sạc cách xa quá 5km. AI bắt buộc phải kích hoạt phương án điều xe sạc di động: `{"action": "dispatch_mobile_charger", "reason": "..."}`.<br>3. **Ranh giới tương thích phần cứng:** Không đề xuất trạm sạc không hỗ trợ chuẩn cổng sạc của xe. |

---

### 2.3. Future-State Flow & AI Fit (Quy trình tương lai & Phân tích kiến trúc)

#### 📊 Ma trận đánh giá mức độ phù hợp công nghệ (AI-Fit Matrix):

| Kiến trúc cân nhắc | Ưu điểm | Nhược điểm | Đánh giá & Quyết định |
|:---|:---|:---|:---|
| **Rule-based Engine (Không dùng AI)** | Chạy cực nhanh, tính toán khoảng cách và kiểm tra pin < 5% bằng code logic tuyệt đối chính xác. | Không thể tạo văn bản hướng dẫn linh hoạt, không tự động tổng hợp lưu ý giao thông, ngôn từ rập khuôn, tài xế khó theo dõi khi đang lái xe. | **Cần thiết cho tầng Guardrails**, nhưng chưa đủ để giải quyết điểm nghẽn soạn thảo tin nhắn. |
| **Autonomous Multi-Agent Loop** | Tự động phân tích, tự tìm đường và tự động điều phối không cần con người can thiệp. | **Rủi ro vận hành quá lớn (Catastrophic Risk):** Nếu Agent bị ảo giác (hallucination) điều xe đến sai trạm hoặc gửi nhầm lệnh cứu hộ sẽ gây thiệt hại hàng triệu đồng và làm tê liệt giao thông. | **LOẠI BỎ (NO-GO):** Không an toàn cho dịch vụ giao thông thực tế. |
| **LLM Feature + Rule Guardrails + HITL** | Tận dụng LLM để sinh văn bản chỉ dẫn chuyên nghiệp, tốc độ dưới 3 giây; tích hợp Rule kiểm tra pin khắt khe; con người làm chốt chặn cuối cùng kiểm duyệt. | Cần thiết lập System Prompt nghiêm ngặt và cơ chế Fallback khi API gián đoạn. | **LỰA CHỌN TỐI ƯU (SELECTED):** Cân bằng hoàn hảo giữa tự động hóa tốc độ cao và an toàn vận hành tuyệt đối. |

#### 🔄 Sơ đồ quy trình tương lai (Future-State Flow):

```text
┌────────────────┐     ┌────────────────────────────────────────────────────────┐     ┌────────────────┐
│ Bước 1         │     │ Bước 2 & Bước 3: HỆ THỐNG AI CO-PILOT                   │     │ Bước 4 🟢      │
│ Tài xế báo     │ ──> │ 🔵 [API Call]: Tự động pull GPS xe & trạm sạc trống    │ ──> │ Điều phối viên │
│ sự cố pin      │     │ 🔵 [LLM + Guardrail]: Sinh bản nháp chỉ dẫn            │     │ Review bản nháp│
│                │     │    - Bắt buộc gắn thẻ: [DRAFT_ONLY]                    │     │ & Click gửi    │
│ Ai: Dispatcher │     │    - Nếu Pin < 5%: Kích hoạt dispatch_mobile_charger   │     │                │
│ ⏱ 0.5 phút    │     │ ⏱ Xử lý tự động: 2.0 giây                              │     │ ⏱ 1.0 phút    │
└────────────────┘     └────────────────────────────────────────────────────────┘     └────────────────┘
                                                  │
                                                  ▼ (Khi API lỗi / Timeout > 5s)
                                       ┌────────────────────────────────────────┐
                                       │ ↩️ Fallback Plan:                      │
                                       │ Tự động chuyển về bản đồ cứu hộ        │
                                       │ truyền thống để Dispatcher xử lý tay.   │
                                       └────────────────────────────────────────┘

⏱ Tổng thời gian quy trình mới: DƯỚI 2.5 PHÚT (Giảm 83% thời gian so với cũ).
```

---

## 🏁 3. Phase 5 — EVALUATE: Đánh Giá Độ Sẵn Sàng & Quyết Định

### 3.1. AI Readiness Checklist

- [x] **1. Dữ liệu sẵn sàng (Data Readiness):**  
  GSM và VinFast đã đồng bộ hệ thống Telematics trên toàn bộ xe taxi điện (tọa độ GPS, SoC pin, dòng sạc) và hệ thống IoT trạm sạc toàn quốc theo thời gian thực (trụ nào đang sạc, trụ nào hỏng, trụ nào trống). Dữ liệu hoàn toàn sạch và có thể truy xuất qua Internal REST API.
- [x] **2. Rủi ro trong tầm kiểm soát (Risk Control):**  
  Toàn bộ rủi ro về sai lệch trạm sạc hoặc cạn pin giữa đường được khóa chặt bởi 2 lớp bảo vệ:
  - Tầng 1: Rule Guardrail tự động kích hoạt cứu hộ nếu pin dưới 5%.
  - Tầng 2: Cơ chế Human-in-the-loop bắt buộc chèn tag `[DRAFT_ONLY]`, không bao giờ để AI tự ý phát lệnh gửi tin nhắn ra ngoài.
- [x] **3. Sự sẵn sàng của Stakeholders (Organizational Readiness):**  
  Đội ngũ điều phối viên Xanh SM rất mong mỏi công cụ này vì họ đang chịu áp lực quá tải trong giờ cao điểm. Quy trình mới không làm xáo trộn cấu trúc quản lý mà chỉ đóng vai trò trợ lý tăng năng suất làm việc.

---

### 3.2. Quyết Định Của Ban Giám Đốc Vin Smart Future

👉 **Quyết định:** **GO (Tiến hành xây dựng Prototype và thử nghiệm Pilot)**

### 3.3. Justification (Luận cứ kinh tế và kỹ thuật)

1. **Hiệu quả kinh tế (ROI vượt trội):**  
   Với chi phí API cực thấp của Gemini Flash (~0.0001 USD/request), chi phí vận hành cho 80 sự cố/ngày chỉ tốn chưa tới 1.000 VNĐ/ngày. Đổi lại, hệ thống tiết kiệm hơn 600 giờ công lao động mỗi tháng của đội ngũ điều vận và giữ cho hàng chục chiếc xe tiếp tục vận hành trên đường, mang lại giá trị gia tăng hàng trăm triệu đồng cho Xanh SM.
2. **Độ tin cậy kỹ thuật cao:**  
   Không đòi hỏi huấn luyện mô hình phức tạp hay xây dựng hạ tầng GPU đắt đỏ; giải pháp sử dụng Prompt Engineering kết hợp Structured Guardrails trên nền tảng Gemini Flash có độ trễ cực thấp (< 2 giây), dễ dàng tích hợp vào phần mềm điều vận hiện có của GSM.
3. **Tuân thủ chuẩn mực an toàn Vingroup:**  
   Ranh giới vận hành rõ ràng, lấy con người làm trọng tâm kiểm duyệt, tuyệt đối không tiềm ẩn nguy cơ gây tai nạn hay ảnh hưởng đến thương hiệu của Tập đoàn.
