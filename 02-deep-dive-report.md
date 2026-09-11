# 02 — Deep-Dive Report: Xanh SM — Giải trình vi phạm tài xế

**Dự án:** Lab 02 — AI Product Scoping (Vin Smart Future — Vingroup)
**Bài toán được chọn:** Trợ lý AI hỗ trợ xử lý giải trình vi phạm (Fraud Warning / tỉ lệ nhận chuyến thấp) của tài xế Xanh SM

---

## 🏛️ Bối cảnh & lý do chọn đề tài

Xanh SM (GSM) vận hành đội xe điện lớn nhất Việt Nam (thị phần dẫn đầu thị trường gọi xe công nghệ). Để chống gian lận, hệ thống tự động gắn cảnh báo "Fraud Warning Level" cho tài xế có tỉ lệ nhận chuyến bất thường thấp hoặc dấu hiệu gian lận — công ty đã phải bổ sung tính năng **"Giải trình trực tuyến"** (12/2025) chính vì quy trình xử lý giải trình cũ gây tranh cãi kéo dài, thiếu minh bạch với tài xế ([kenh14.vn](https://kenh14.vn/thong-bao-den-tai-xe-xanh-sm-215251215220945705.chn), [xanhsm.com](https://www.xanhsm.com/news/cap-nhat-bo-quy-tac-ung-xu-danh-cho-doi-tac-tai-xe-bike)). Đây là bài toán vận hành nội bộ thật, có nguồn xác thực, và **khác biệt hoàn toàn** với các đề tài khác trong nhóm (không trùng với ý tưởng "Xanh SM hết pin thực địa" mà 2 thành viên khác đã chọn).

---

## 🏗️ Phase 3.1 — Current-State Workflow Mapping

> Xem sơ đồ trực quan tại [`04-workflow-diagram.png`](04-workflow-diagram.png)

| Bước | Mô tả | Actor | Thời gian | Input → Output |
|---|---|---|---|---|
| 1 | Hệ thống tự động gắn cảnh báo vi phạm (Fraud Warning Level) khi phát hiện tỉ lệ nhận chuyến bất thường thấp | Hệ thống | Tự động (0 phút) | Log chuyến đi → Cảnh báo |
| 2 | 🔄 Tài xế nộp giải trình (text/ảnh minh chứng) qua app | Tài xế | ~5-10 phút (phía tài xế, không tính vào workload Compliance) | Cảnh báo → Giải trình |
| 3 | 🔴 Nhân viên Compliance đọc & hiểu nội dung giải trình (nhiều case viết dài dòng, thiếu cấu trúc) | Compliance | **5 phút** | Giải trình → Ghi chú |
| 4 | 🔴 🔄 Đối chiếu GPS, lịch sử chuyến, lịch sử vi phạm trên **nhiều dashboard nội bộ khác nhau** | Compliance | **7 phút** | Ghi chú → Bằng chứng |
| 5 | Ra quyết định (gỡ cảnh báo / duy trì xử lý) & phản hồi tài xế | Compliance | 3 phút | Bằng chứng → Quyết định |

**🔴 Bottleneck chính:** Bước 3 & 4 — chiếm 12/15 phút (80% thời gian xử lý), do phải đọc hiểu văn bản tự do và tra cứu chéo dữ liệu từ nhiều hệ thống rời rạc.
**🔄 Handoff:** Hệ thống → Tài xế → Compliance → (nhiều dashboard nội bộ) → Tài xế.
**⏱ Tổng thời gian xử lý trung bình:** ~15 phút/case (không tính thời gian tài xế tự soạn giải trình).

---

## 📌 Phase 3.2 — Problem Statement (6-Field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên Compliance / Trust & Safety tại trung tâm vận hành Xanh SM, phụ trách xử lý các case tài xế bị hệ thống tự động cảnh báo vi phạm. |
| **2. Current Workflow** | Hệ thống tự động cảnh báo → tài xế nộp giải trình qua app → nhân viên Compliance đọc thủ công, đối chiếu GPS/lịch sử chuyến trên nhiều dashboard rời rạc → ra quyết định gỡ/duy trì cảnh báo. 5 bước, ~15 phút/case, hoàn toàn thủ công ở khâu đọc-hiểu và đối chiếu. |
| **3. Bottleneck** | Bước 3 & 4: đọc hiểu nội dung giải trình tự do (không có cấu trúc chuẩn) + tra cứu chéo dữ liệu từ nhiều hệ thống khác nhau (GPS, lịch sử chuyến, lịch sử vi phạm) — không có công cụ tổng hợp một chỗ. |
| **4. Business Impact** | *(Giả định minh họa dựa trên quy mô đội tài xế dẫn đầu thị trường của Xanh SM, chưa phải số liệu chính thức được công bố)*: với quy mô hàng chục nghìn tài xế toàn quốc, ước tính hàng trăm case giải trình/ngày → tồn đọng case khiến tài xế bị treo cảnh báo lâu hơn cần thiết, ảnh hưởng trực tiếp thu nhập, tăng nguy cơ khiếu nại vượt cấp và tài xế rời bỏ nền tảng (churn) nếu cảm thấy thiếu minh bạch — đúng vấn đề mà Xanh SM đã phải ra tính năng "Giải trình trực tuyến" để xoa dịu. |
| **5. Success Metric** | 1. Giảm thời gian xử lý trung bình từ **15 phút → dưới 5 phút/case**.<br>2. Tăng tỉ lệ case được phản hồi trong **24h lên ≥ 95%**.<br>3. Không thay đổi độ chính xác quyết định cuối cùng (con người vẫn quyết định 100%, AI chỉ hỗ trợ). |
| **6. Operational Boundary** | AI chỉ được: tóm tắt nội dung giải trình + tự động đối chiếu dữ liệu GPS/lịch sử qua API nội bộ đã whitelist + đề xuất quyết định sơ bộ dạng **draft kèm độ tin cậy**. AI **TUYỆT ĐỐI KHÔNG được**: tự động gỡ hoặc duy trì cảnh báo mà không có nhân viên Compliance duyệt (vì ảnh hưởng trực tiếp đến thu nhập/tư cách hoạt động của tài xế — bắt buộc Human-in-the-loop), bịa đặt dữ liệu khi thông tin thiếu/mâu thuẫn (phải trả về "cần xem xét thủ công"). |

---

## 🔄 Phase 3.3 — Future-State Flow & AI Fit

### AI-Fit Matrix

| Kiến trúc | Đánh giá |
|---|---|
| **Rule-based** | Chỉ đủ kiểm tra ngưỡng số liệu đơn giản (vd: tỉ lệ nhận chuyến < X%), không thể đọc hiểu nội dung giải trình bằng ngôn ngữ tự nhiên → không đủ giải quyết bottleneck chính. |
| **Agentic Loop (tự trị hoàn toàn)** | ❌ **LOẠI BỎ**: để AI tự tra cứu nhiều hệ thống VÀ tự ra quyết định cuối cùng là rủi ro quá lớn vì ảnh hưởng trực tiếp thu nhập tài xế — nếu AI sai/ảo giác, hậu quả khó khắc phục và gây mất niềm tin nghiêm trọng hơn cả quy trình thủ công hiện tại. |
| **LLM Feature + Rule Guardrail + HITL bắt buộc** | ✅ **LỰA CHỌN**: LLM tóm tắt + đối chiếu dữ liệu (giải quyết đúng bottleneck bước 3-4), rule kiểm tra ngưỡng dữ liệu, con người luôn là người quyết định cuối. |

### Sơ đồ quy trình tương lai (Future-State Flow)

```text
Tài xế nộp giải trình
   → 🔵 AI: tóm tắt giải trình + tự động đối chiếu GPS/lịch sử chuyến qua API nội bộ
   → 🔵 AI: đề xuất quyết định sơ bộ (gỡ cảnh báo / duy trì / cần thêm bằng chứng) kèm độ tin cậy
   → 🟢 Nhân viên Compliance xem đề xuất (dạng draft), duyệt hoặc chỉnh sửa
   → Hệ thống gửi phản hồi chính thức cho tài xế

↩️ Fallback: Nếu dữ liệu GPS/lịch sử thiếu, mâu thuẫn, hoặc độ tin cậy AI thấp
   → luôn trả về "cần xem xét thủ công", KHÔNG đưa ra đề xuất đoán mò
   → nhân viên xử lý theo quy trình thủ công hiện tại
```

⏱ **Ước tính thời gian quy trình mới:** ~4 phút/case (giảm ~73% so với 15 phút hiện tại), chủ yếu là thời gian nhân viên Compliance xem & duyệt đề xuất.

---

## 🏁 Phase 5 — Evaluate

### AI Readiness Checklist

- [x] **Dữ liệu sẵn sàng:** Xanh SM đã triển khai hệ thống Fraud Warning Level (từ 10/2025) và có log GPS/lịch sử chuyến đi trên toàn bộ đội xe — có thể truy xuất qua API nội bộ.
- [x] **Rủi ro trong tầm kiểm soát:** Human-in-the-loop bắt buộc cho mọi quyết định ảnh hưởng thu nhập tài xế; AI chỉ đề xuất, không tự hành động.
- [ ] **Sự sẵn sàng của Stakeholders:** Cần đội Compliance đồng thuận quy trình mới (chuyển từ "tự đọc từ đầu" sang "review đề xuất AI") — cần thời gian đào tạo và xây dựng niềm tin vào công cụ.

### Quyết định

> ## ⚠️ GO có điều kiện (Pilot phạm vi hẹp trước khi mở rộng)

**Justification:**
1. **Chi phí thấp, rủi ro giới hạn:** Chỉ cần LLM Feature (không cần Agent phức tạp), toàn bộ quyết định ảnh hưởng tài xế vẫn do con người duyệt — không có "blast radius" lớn nếu AI sai.
2. **Giá trị đo lường rõ ràng:** Giảm thời gian xử lý, tăng tỉ lệ phản hồi nhanh — trực tiếp giải quyết đúng nguyên nhân gốc khiến Xanh SM phải ra mắt tính năng "Giải trình trực tuyến" (thiếu minh bạch, xử lý chậm).
3. **Điều kiện trước khi mở rộng toàn quốc:** Cần pilot tại 1 trung tâm điều vận trong 2-4 tuần, đo lường độ khớp giữa đề xuất AI và quyết định thực tế của nhân viên Compliance (mục tiêu ≥ 90% khớp) trước khi triển khai diện rộng. Nếu tỉ lệ khớp thấp hoặc phát hiện thiên vị (bias) trong đề xuất, dừng mở rộng và điều chỉnh lại system prompt/dữ liệu huấn luyện.
