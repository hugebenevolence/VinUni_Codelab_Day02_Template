# 02 - Deep-Dive Report: Xanh SM Battery Incident Co-pilot

## 1. Current-State Workflow

**Phạm vi:** Trung tâm điều vận Hà Nội, tài xế báo hết pin hoặc lỗi sạc trong thời gian đang nhận/chờ chuyến.

1. **Nhận cuộc gọi/ghi chú sự cố** - dispatcher nghe và ghi biển số, vị trí, mức pin. ⏱ 2 phút
2. **Tra vị trí xe** - mở bản đồ vận hành và đối chiếu GPS. 🔄 Handoff từ tài xế sang dispatcher. ⏱ 2 phút
3. **Tìm trạm phù hợp** - mở dashboard trạm, lọc theo khoảng cách, trạng thái trụ và loại xe. 🔴 Bottleneck. ⏱ 5 phút
4. **Soạn chỉ dẫn** - viết địa chỉ, lưu ý đường đi và phương án cứu hộ vào tin nhắn. 🔴 Bottleneck. ⏱ 5 phút
5. **Gọi cứu hộ hoặc gửi hướng dẫn** - dispatcher kiểm tra lần cuối, gửi qua app tài xế hoặc gọi đội cứu hộ. 🔄 Handoff. ⏱ 1 phút

**Tổng thời gian trung bình: 15 phút/lượt.** Dữ liệu minh họa cho prototype được giả định từ log vận hành mẫu và phải được xác thực bằng dữ liệu thật trước pilot.

## 2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| Actor / Operator | Dispatcher Xanh SM tại trung tâm điều vận; đội cứu hộ là nhóm thực hiện khi xe không thể tự di chuyển. |
| Current Workflow | Dispatcher nhận điện thoại/ghi chú, mở bản đồ GPS, tra dashboard trạm VinFast, kiểm tra loại xe và trạng thái trụ, sau đó soạn tin nhắn hoặc gọi cứu hộ. Năm bước, trung bình 15 phút/lượt. |
| Bottleneck | Tra cứu chéo vị trí, loại xe, mức pin và trạng thái trạm mất khoảng 10 phút. Việc viết chỉ dẫn thủ công gây chậm và có nguy cơ gửi nhầm thông tin. |
| Business Impact | Với giả định 80 ca/ngày, quy trình tiêu tốn khoảng 1.200 phút, tương đương 20 giờ dispatcher/ngày. Xe dừng lâu làm giảm năng lực nhận chuyến, tăng thời gian chờ và nguy cơ hủy chuyến. Đây là baseline cần đo lại bằng log thật. |
| Success Metric | Trong pilot, 90% ca đủ dữ liệu có draft trong dưới 30 giây; thời gian xử lý end-to-end giảm từ 15 xuống dưới 3 phút; 98% đề xuất không vi phạm khoảng cách/loại xe; 100% tin gửi có phê duyệt người. |
| Operational Boundary | AI được phép trích xuất dữ liệu, tra API trạm đã được whitelist và tạo draft JSON. AI không được gửi tin, tự điều xe, tự thay đổi chuyến, bịa vị trí/trạm/ETA hoặc bỏ qua HITL. Pin < 5% là critical: không đề xuất trạm quá 5 km, chuyển action `dispatch_mobile_charger`. Thiếu hoặc mâu thuẫn dữ liệu thì `needs_human_review`. |

## 3. AI-Fit Matrix

| Phương án | Vai trò | Đánh giá |
|---|---|---|
| Rule/state machine | Kiểm tra pin < 5%, khoảng cách <= 5 km, tương thích cổng sạc, quyền gửi | Bắt buộc cho safety gate vì dễ kiểm thử và audit. |
| LLM Feature | Đọc ghi chú tự nhiên, chuẩn hóa yêu cầu và tạo tin nhắn tiếng Việt | **Chọn** làm lớp hỗ trợ, không có quyền hành động. |
| Agentic loop | Tự gọi nhiều công cụ và tự điều phối | Không chọn trong pilot vì blast radius lớn và khó kiểm soát thời gian thực. |

## 4. Future-State Flow

```text
Tài xế báo sự cố
  -> Rule gate kiểm tra schema, mức pin, vị trí
  -> [AI] trích xuất thông tin và tạo JSON draft từ dữ liệu đã whitelist
  -> Rule gate kiểm tra lại khoảng cách, loại xe, action và [DRAFT_ONLY]
  -> [HITL] dispatcher review, sửa nếu cần, bấm duyệt
  -> hệ thống mới được gửi tin / mở ticket cứu hộ

Nếu API lỗi, dữ liệu thiếu, confidence < 0.85 hoặc rule fail:
  -> [Fallback] needs_human_review
  -> dispatcher xử lý theo quy trình thủ công hiện tại
```

### Structured output contract

```json
{
  "action": "draft_station_guidance | dispatch_mobile_charger | needs_human_review",
  "message": "[DRAFT_ONLY] ...",
  "reason": "...",
  "confidence": 0.0,
  "requires_human_approval": true
}
```

## 5. Readiness Checklist và quyết định

- [x] Có thể tạo bộ log ẩn danh gồm cuộc gọi/ghi chú, GPS, mức pin, loại xe và trạng thái trạm để test.
- [x] Rủi ro được giới hạn bởi rule gate, confidence threshold, fallback và HITL.
- [ ] Stakeholder cần xác nhận quy trình mới và quyền truy cập API trước pilot.

### Quyết định: GO có điều kiện

Bắt đầu prototype/pilot nội bộ với scope hẹp, không tự động gửi tin và không điều khiển xe. Lý do là phần tạo draft ngôn ngữ có thể giảm thao tác lặp, trong khi quyết định an toàn được giữ ở rule engine và dispatcher. Trước khi mở rộng cần đo baseline thật trong ít nhất hai tuần, kiểm tra độ chính xác 98% trên bộ case có ground truth, thử prompt injection và rà soát quyền truy cập. Nếu dữ liệu trạm không ổn định hoặc tỷ lệ vi phạm safety gate vượt 2%, dừng mở rộng và quay về workflow thủ công.
