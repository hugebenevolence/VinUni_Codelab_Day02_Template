# 01 - Problem Scan & Quick Cards

## Phase 1: Scan

| # | Subsidiary | Lens | Mô tả bài toán | Tín hiệu đo lường ban đầu |
|---:|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian | Điều phối viên xử lý thủ công cuộc gọi tài xế báo hết pin hoặc lỗi sạc. | 15 phút/sự cố; khoảng 80 sự cố/ngày tại Hà Nội. |
| 2 | VinFast | Lặp lại | Đối soát hóa đơn sạc giữa trạm, xe và đối tác theo tuần. | 2 nhân viên x 2 ngày/tuần; dễ lệch mã xe và thời gian sạc. |
| 3 | Vinhomes | AI-upgrade | Phân loại và gợi ý trả lời khiếu nại cư dân trên ứng dụng. | 12 giờ phản hồi trung vị; nhiều câu trả lời rập khuôn. |
| 4 | Vinmec | Tốn thời gian | Bác sĩ viết tóm tắt xuất viện từ hồ sơ dài. | 20-30 phút/bệnh nhân; cần kiểm duyệt y khoa. |
| 5 | Vinpearl | Pain từ người khác | Nhân viên tổng đài tìm thông tin đặt phòng, vé và chính sách đổi lịch. | 8-10 phút/cuộc; khách phải chờ khi nhiều hệ thống liên quan. |
| 6 | Xanh SM | Pain từ người khác | Tóm tắt lý do khách hủy chuyến từ ghi âm và ghi chú tài xế. | 1-2 giờ/ngày cho phân tích thủ công; dữ liệu không đồng nhất. |

## Phase 2: Quick Problem Cards

### Card 1 - Xanh SM: Sự cố pin ngoài đường

- **Bài toán:** Tài xế báo pin dưới mức an toàn; điều phối viên phải tìm vị trí, trạm phù hợp và soạn hướng dẫn.
- **Actor chịu đau:** Tài xế phải chờ; dispatcher bị quá tải trong giờ cao điểm; khách có thể bị trễ chuyến.
- **Workflow hiện tại:** (1) nhận cuộc gọi -> (2) ghi biển số và mức pin -> (3) tra GPS trên bản đồ -> (4) tra trạm còn chỗ và tương thích -> (5) viết/gửi chỉ dẫn hoặc gọi cứu hộ.
- **Bottleneck:** Bước 3-4, khoảng 10 phút/lượt; dữ liệu nằm ở nhiều màn hình và dễ chọn sai trạm.
- **AI hỗ trợ:** Trích xuất thông tin cuộc gọi/ghi chú, kiểm tra dữ liệu trạm và tạo bản nháp hướng dẫn.
- **Metric:** Giảm 15 phút xuống dưới 3 phút/lượt; ít nhất 98% đề xuất đúng loại xe và trong phạm vi an toàn.
- **Architecture:** LLM Feature kết hợp rule-based safety gate và HITL.

### Card 2 - Vinhomes: Route khiếu nại cư dân

- **Bài toán:** Phân loại khiếu nại và đề xuất bản nháp trả lời cho CSKH.
- **Actor chịu đau:** Nhân viên CSKH và cư dân chờ phản hồi.
- **Workflow hiện tại:** Nhận ticket -> đọc nội dung -> tìm nhóm xử lý -> tra chính sách -> viết phản hồi -> chuyển duyệt.
- **Bottleneck:** Đọc và phân loại ticket, khoảng 6 phút/ticket; khiếu nại phí/pháp lý có rủi ro cao.
- **AI hỗ trợ:** Phân loại, trích xuất trường dữ liệu và soạn draft dựa trên kho chính sách đã duyệt.
- **Metric:** 85% ticket được route dưới 10 giây; không tự trả lời nhóm pháp lý/tài chính.
- **Architecture:** LLM Feature, bắt buộc chuyển người với nhóm rủi ro cao.

### Card 3 - VinFast: Đối soát hóa đơn sạc

- **Bài toán:** Đối chiếu hóa đơn, mã xe và log trạm sạc giữa các hệ thống.
- **Actor chịu đau:** Nhân viên tài chính vận hành và quản lý trạm.
- **Workflow hiện tại:** Xuất CSV -> chuẩn hóa mã -> so khớp thời gian/khối lượng -> tìm dòng lệch -> gửi yêu cầu xác minh.
- **Bottleneck:** Chuẩn hóa dữ liệu và xử lý ngoại lệ, khoảng 2 ngày/tuần.
- **AI hỗ trợ:** Chỉ giải thích nguyên nhân dòng lệch và tạo danh sách cần xác minh; việc tính tiền vẫn là rule-based.
- **Metric:** Giảm thời gian đối soát 2 ngày xuống 4 giờ; 95% dòng lệch được nhóm đúng.
- **Architecture:** Rule/state machine là lõi, LLM chỉ hỗ trợ giải thích.

## Lựa chọn bài toán

Nhóm chọn **Card 1 - Xanh SM: Sự cố pin ngoài đường** vì đây là vấn đề thời gian thực, có metric rõ, dữ liệu đầu vào tương đối có cấu trúc và có thể giới hạn rủi ro bằng rule safety gate cùng phê duyệt của dispatcher. Card 2 cần thêm kho chính sách và kiểm soát pháp lý; Card 3 phần lõi phù hợp với code đối soát thông thường hơn là LLM.
