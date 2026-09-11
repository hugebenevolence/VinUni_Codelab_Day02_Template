# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow Mapping

Quy trình xử lý sự cố hết pin thực địa hiện tại của điều phối viên Xanh SM:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴 │     │ ⏱ 5 phút 🔴 │
│ In: Điện thoại│     │ In: Biển số  │     │ In: Vị trí GPS│     │ In: Dữ liệu  │
│ Out: Log sự cố│     │ Out: Toạ độ  │     │ Out: Địa chỉ │     │ Out: SMS     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Gọi xe cứu   │
                                                               │ hộ (nếu cần) │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ 1 phút     │
                                                               └──────────────┘
🔴 = Bottleneck
⏱ Tổng thời gian xử lý thủ công: 15 phút/lượt.
```

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo sự cố pin hoặc pin yếu, điều phối viên phải tra cứu vị trí GPS trên bản đồ nội bộ, kiểm tra các trạm sạc VinFast còn trụ trống gần nhất, viết tin nhắn chỉ dẫn, và nếu mức pin nguy hiểm thì gọi xe cứu hộ. Toàn bộ quy trình làm thủ công mất khoảng 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 và Bước 4 là điểm nghẽn chính. Việc tra cứu trụ sạc phù hợp theo vị trí, loại xe và khả năng sạc, đồng thời viết hướng dẫn bằng tiếng Việt thân thiện, chiếm ~10 phút. |
| **4. Business Impact** | Mỗi ngày có khoảng 80 sự cố pin thực địa tại Hà Nội. Điều này làm mất khoảng 20 giờ lao động của đội điều vận mỗi ngày, kéo dài thời gian chờ của tài xế và làm giảm hiệu suất điều phối, ảnh hưởng trực tiếp đến tỷ lệ khách hàng hủy chuyến và doanh thu. |
| **5. Success Metric** | 1. Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút.<br>2. Tỷ lệ đề xuất trạm sạc và chỉ đường chính xác đạt ≥ 98%.<br>3. Giảm số lần điều phối viên phải soạn tin thủ công xuống 80%. |
| **6. Operational Boundary** | AI được phép tự động truy xuất vị trí xe, tra cứu trạm sạc gần nhất, và tạo tin nhắn nháp hướng dẫn cho dispatcher. **CẤM:** AI không được gửi tự động mà không có phê duyệt của điều phối viên; không được đề xuất trạm sạc quá xa khi pin dưới 5% và không được chọn trạm không phù hợp với loại cổng sạc của xe. |

## 3.3. Future-State Flow & AI Fit

* **AI Fit:** Chọn **LLM Feature**. Đây là quy trình có cấu trúc rõ, chủ yếu là xử lý thông tin, soạn nháp và hỗ trợ quyết định trong thời gian thực, không cần tự động hóa toàn bộ bằng agent.
* **Quy trình tương lai:**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 AI pull  │     │ 🔵 AI draft │     │ 🟢 Human    │
│ gọi sự cố    │ ──→ │ vị trí &     │ ──→ │ chỉ dẫn     │ ──→ │ review &    │
│              │     │ trạm gần    │     │ tối ưu      │     │ gửi tin     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI không tự tin,
                                                               dispatcher tự viết
                                                               tin nhắn như cũ.
```

### Human-in-the-loop và fallback
- **Human-in-the-loop:** Điều phối viên duyệt tin nhắn trước khi gửi tới tài xế.
- **Fallback:** Nếu API trạm sạc không trả lời hoặc AI không tin chắc, hệ thống buộc quay lại quy trình thủ công cũ hoặc yêu cầu người dùng kiểm tra lại.

---

# 🏁 Phase 5 — EVALUATE

## AI Readiness Checklist
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test.
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL và Fallback).
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[x] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

> Lưu ý: Đây là bài lab mô phỏng, nên việc đánh dấu NO-GO hoàn toàn dựa trên mục tiêu học tập và khả năng định nghĩa ranh giới an toàn. Trong thực tế, bài toán này vẫn là một use case phù hợp cho AI feature nếu có dữ liệu và quy trình phê duyệt rõ ràng.

### Justification
Bài toán này có lợi ích rõ ràng, nhưng nếu chỉ xem xét "độ sẵn sàng thực thi" trong trường hợp đóng gói cho mục đích Lab, giải pháp AI cần được kiểm soát chặt chẽ và có dữ liệu thực tế để đánh giá. Vì vậy, nó phù hợp với mô hình **LLM feature + HITL + fallback**, chứ không phải giải pháp agentic tự động hoàn toàn. Dựa trên logic bài toán, ranh giới an toàn rõ ràng, metric đạt được dễ đo và quy trình có thể bị sai sót nếu không có người duyệt, nên đây là một use case đáng triển khai ở mức prototype và không nên loại bỏ ngay.
