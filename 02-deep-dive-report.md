# 02 — Deep Dive Report & Evaluate
> **Lab 02: AI Product Scoping — Vin Smart Future**
> **Đề tài được chọn:** Hệ thống tầm soát sai lệch cột sống thời gian thực sử dụng AI thị giác (Computer Vision) — Vinmec Smart Healthcare

---

## 🔬 PHASE 3 — DEEP DIVE: Problem Statement (6-Field Canvas)

### 📌 Bài toán được chọn: Tầm soát tư thế cột sống tự động cho nhân viên & bệnh nhân Vinmec

| # | Trường | Nội dung |
|---|--------|----------|
| **1** | **WHO — Ai bị ảnh hưởng?** | Nhân viên văn phòng Vinmec (hành chính, IT, kế toán) làm việc > 6h/ngày tại máy tính; bệnh nhân ngoại trú có triệu chứng đau cổ vai gáy; bệnh nhân phục hồi chức năng sau phẫu thuật cột sống |
| **2** | **WHAT — Bài toán cụ thể là gì?** | Không có cơ chế phát hiện sớm, liên tục và chi phí thấp để nhận diện sai lệch tư thế cột sống (gù lưng, cổ rùa, lệch hông). Bệnh chỉ được phát hiện khi đã tiến triển mãn tính, lúc đó việc điều trị tốn kém và mất nhiều thời gian hơn nhiều |
| **3** | **WHERE — Xảy ra ở đâu?** | Văn phòng làm việc, phòng khám ngoại trú Vinmec, khu vực phục hồi chức năng, và cả môi trường làm việc tại nhà (WFH) của nhân viên |
| **4** | **WHEN — Khi nào xảy ra?** | Liên tục trong suốt ca làm việc 6–10h/ngày; không được giám sát trong hầu hết thời gian ngồi làm việc; chỉ được kiểm tra khi có triệu chứng đau rõ ràng (trung bình sau 6–18 tháng ngồi sai tư thế mãn tính) |
| **5** | **WHY — Tại sao quan trọng?** | 80% người trưởng thành sẽ gặp đau lưng mãn tính ít nhất 1 lần trong đời. Chi phí điều trị cột sống giai đoạn muộn (phẫu thuật, MRI, vật lý trị liệu dài hạn) cao hơn gấp 10–20 lần phòng ngừa sớm. Tại Vinmec, đây là cơ hội tạo ra gói dịch vụ "Preventive Healthcare" cao cấp và bền vững |
| **6** | **HOW MUCH — Quy mô tác động?** | Vinmec có > 15,000 nhân viên nội bộ + hàng chục nghìn bệnh nhân ngoại trú/tháng. Thị trường giải pháp phòng ngừa sức khỏe cơ xương khớp tại Đông Nam Á ước đạt 2.3 tỷ USD vào 2027. Hệ thống này giúp giảm 20–30% số ca chuyển khoa điều trị chuyên sâu |

---

## 🔄 PHASE 3 — FUTURE STATE FLOW: Quy trình Tương lai tích hợp AI

### Current State (AS-IS) — Quy trình Hiện tại (Có vấn đề)

```
[Nhân viên ngồi làm việc]
        ↓
[Không có giám sát tư thế] ← 🔴 BOTTLENECK: Không phát hiện được
        ↓ (6–18 tháng sau)
[Xuất hiện đau mãn tính]
        ↓
[Đặt lịch khám → Chờ]    ← 🔴 BOTTLENECK: Chờ đợi, tốn thời gian
        ↓
[Chụp X-quang / MRI]     ← 🔴 BOTTLENECK: Chi phí cao, bức xạ
        ↓
[Bác sĩ chẩn đoán]
        ↓
[Điều trị giai đoạn muộn] ← Tốn kém, hồi phục chậm
```

---

### Future State (TO-BE) — Quy trình Tương lai với AI

```
[Camera RGB thường / Webcam máy tính]
        ↓
┌─────────────────────────────────────────────┐
│  RULE-BASED ENGINE (Phát hiện tức thì)      │
│  MediaPipe Pose Estimation → Tọa độ 33 điểm │
│  neo khớp → Tính góc cơ sinh học thời gian  │
│  thực (góc cổ, góc lưng, góc hông...)       │
└─────────────────────────────────────────────┘
        ↓
[So sánh với ngưỡng lâm sàng tiêu chuẩn]
        ↓ (nếu góc vượt ngưỡng)
┌─────────────────────────────────────────────┐
│  LLM / DECISION SUPPORT ENGINE              │
│  → Chấm điểm Posture Score (0–100)         │
│  → Phân loại mức độ: Bình thường / Cảnh   │
│    báo / Nghiêm trọng                       │
│  → Tự động đề xuất bài tập phục hồi        │
│    chức năng tương ứng                       │
└─────────────────────────────────────────────┘
        ↓
[Gửi thông báo cảnh báo tức thì cho người dùng]
        ↓
┌─────────────────────────────────────────────┐
│  HUMAN-IN-THE-LOOP 🩺                        │
│  Nếu Posture Score < 60 liên tục ≥ 7 ngày: │
│  → Hệ thống đề xuất đặt lịch khám Vinmec  │
│  → Bác sĩ xem lịch sử dữ liệu tư thế của │
│    bệnh nhân (không cần chụp hình từ đầu)  │
└─────────────────────────────────────────────┘
        ↓
[FALLBACK 🔄]
Nếu camera bị che / ánh sáng kém / nhận dạng
không đủ tin cậy (confidence < 0.75):
→ Hệ thống thông báo "Không thể phân tích"
→ Không đưa ra chẩn đoán sai
→ Ghi log để bác sĩ xem xét thủ công
```

### 🤖 Phân loại AI trong hệ thống

| Tầng | Loại AI | Chức năng |
|------|---------|-----------|
| Tầng 1 | **Rule-based** | Phát hiện điểm neo khớp + tính góc cơ sinh học theo công thức chuẩn y khoa |
| Tầng 2 | **LLM / Prompt AI** | Diễn giải kết quả góc → ngôn ngữ tự nhiên dễ hiểu + đề xuất bài tập cá nhân hóa |
| Tầng 3 | **Agentic Loop** (tương lai) | Theo dõi xu hướng tư thế theo tuần/tháng → tự động điều chỉnh ngưỡng cảnh báo theo thể trạng từng người |

---

## 📊 PHASE 5 — EVALUATE: Đánh giá Độ Sẵn Sàng Triển Khai

### ✅ Checklist Đánh giá

#### 🗃️ Data Readiness (Dữ liệu)

| Tiêu chí | Trạng thái | Ghi chú |
|----------|-----------|---------|
| Có dữ liệu huấn luyện không? | ✅ Có | MediaPipe đã được Google huấn luyện trên dataset đa dạng hàng triệu ảnh |
| Dữ liệu đủ đa dạng không? | ⚠️ Một phần | Chưa bao quát hết: BMI cao, trang phục rộng, ánh sáng yếu → cần thu thập thêm data Việt Nam |
| Có thể thu thập data mới không? | ✅ Có | Vinmec có thể hợp tác thu thập data tư thế bệnh nhân với sự đồng ý |
| Dữ liệu có nhãn y tế chuẩn không? | ⚠️ Cần bổ sung | Cần bác sĩ vật lý trị liệu Vinmec gán nhãn ngưỡng góc lâm sàng |

#### 🔧 Technical Feasibility (Kỹ thuật)

| Tiêu chí | Trạng thái | Ghi chú |
|----------|-----------|---------|
| Công nghệ lõi đã có chưa? | ✅ Có sẵn | MediaPipe Pose / BlazePose là open-source, miễn phí |
| Phần cứng yêu cầu? | ✅ Thấp | Chỉ cần webcam thường 720p trở lên, không cần GPU chuyên dụng |
| Latency có đủ nhanh không? | ✅ Đạt | MediaPipe chạy 30fps trên CPU thông thường |
| Có thể deploy trên web/mobile? | ✅ Có | Hỗ trợ Python, JavaScript (TensorFlow.js), Android, iOS |

#### ⚕️ Clinical Validity (Y tế)

| Tiêu chí | Trạng thái | Ghi chú |
|----------|-----------|---------|
| Ngưỡng góc lâm sàng có cơ sở không? | ✅ Có | Dựa trên chuẩn quốc tế (AMA, SPINE Journal): cổ > 15°, lưng > 20° là nguy hiểm |
| Có sự tư vấn của bác sĩ chuyên khoa? | ⚠️ Cần | Cần hợp tác với Khoa Cơ xương khớp Vinmec để hiệu chỉnh |
| Hệ thống có thay thế bác sĩ không? | ❌ Không | Chỉ hỗ trợ tầm soát sớm — không thay thế chẩn đoán hình ảnh chuyên sâu |
| Tuân thủ quy định y tế (HIPAA/PDPA)? | ⚠️ Cần xem xét | Cần đánh giá tuân thủ Luật bảo vệ dữ liệu cá nhân Việt Nam |

#### 💰 Business Value (Giá trị kinh doanh)

| Tiêu chí | Trạng thái | Ghi chú |
|----------|-----------|---------|
| Bài toán có đủ lớn không? | ✅ Có | 15,000+ nhân viên Vinmec + triệu bệnh nhân/năm |
| ROI có rõ ràng không? | ✅ Có | Phòng ngừa tiết kiệm chi phí điều trị gấp 10–20 lần |
| Người dùng có chịu dùng không? | ✅ Cao | Không cần thiết bị đặc biệt, dùng webcam có sẵn |
| Có thể tạo doanh thu không? | ✅ Có | Gói "Vinmec AI Wellness" tích hợp vào chương trình chăm sóc sức khỏe nhân viên |

---

### 🏁 Quyết định Cuối cùng

> ## ✅ GO — Tiến hành triển khai thí điểm (Pilot)

**Lý do GO:**
- ✅ Công nghệ lõi (MediaPipe) đã **sẵn sàng và miễn phí**
- ✅ Chi phí triển khai ban đầu **rất thấp** (không cần phần cứng đặc biệt)
- ✅ Giá trị y tế **rõ ràng và đo lường được**
- ✅ Phù hợp với chiến lược **Smart Healthcare** của Vinmec
- ⚠️ Cần hiệu chỉnh ngưỡng lâm sàng với bác sĩ chuyên khoa trước khi phát hành chính thức
- ⚠️ Cần thu thập thêm data để giảm thiên vị thuật toán cho người dùng Việt Nam

**Phạm vi Pilot đề xuất:**
1. Triển khai thí điểm cho **100 nhân viên văn phòng** Vinmec Hà Nội (3 tháng)
2. Thu thập phản hồi + data tư thế thực tế → Hiệu chỉnh mô hình
3. Đánh giá kết quả → Quyết định mở rộng toàn hệ thống Vinmec
