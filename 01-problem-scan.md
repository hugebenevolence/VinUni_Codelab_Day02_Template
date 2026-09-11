# 01 — Problem Scan & Quick Assess
> **Lab 02: AI Product Scoping — Vin Smart Future**

---

## 🔍 PHASE 1 — SCAN: Bảng Quét Cơ Hội Bài Toán Thực Tế (Vingroup)

> Mục tiêu: Liệt kê ít nhất 5 bài toán thực tế tại các công ty thành viên Vingroup mà AI có thể giải quyết.

| # | Công ty Vingroup | Bộ phận / Đối tượng | Bài toán / Pain Point | Tần suất xảy ra | Hướng giải quyết bằng AI |
|---|---|---|---|---|---|
| 1 | **Vinmec** (Bệnh viện) | Bệnh nhân ngoại trú & nhân viên văn phòng | Phát hiện sai lệch tư thế cột sống (gù lưng, cổ rùa, lệch hông) muộn do thiếu tầm soát liên tục | Hàng ngày — hàng chục triệu người ngồi máy tính > 8h/ngày | Computer Vision + Pose Estimation phân tích tư thế qua webcam thường, cảnh báo thời gian thực |
| 2 | **VinFast** (Nhà máy sản xuất xe) | Công nhân dây chuyền lắp ráp | Công nhân thường xuyên làm việc ở tư thế cúi người, xoay hông lặp đi lặp lại → nguy cơ chấn thương cơ xương khớp nghề nghiệp cao | Liên tục trong ca làm việc 8–12h | AI phân tích tư thế lao động qua camera nhà xưởng, cảnh báo khi phát hiện tư thế nguy hiểm |
| 3 | **Vinschool** (Trường học) | Học sinh tiểu học & THCS | Học sinh ngồi sai tư thế trong giờ học kéo dài → vẹo cột sống học đường ngày càng tăng | Hàng ngày trong 6–8 tiếng học | Hệ thống camera lớp học + AI tự động phát hiện và gửi thông báo đến giáo viên / phụ huynh |
| 4 | **VinHomes** (Khu đô thị) | Cư dân làm việc tại nhà (WFH) | Không gian làm việc tại nhà không được thiết kế công thái học → đau cổ vai gáy mãn tính | Hàng ngày — xu hướng WFH tăng mạnh sau COVID | Ứng dụng điện thoại dùng camera selfie phân tích tư thế ngồi làm việc và đề xuất điều chỉnh |
| 5 | **Vinmec** (Phục hồi chức năng) | Bệnh nhân phục hồi sau phẫu thuật / chấn thương | Bác sĩ không thể theo dõi liên tục chất lượng bài tập phục hồi chức năng tại nhà của bệnh nhân | Mỗi ngày trong chu kỳ phục hồi 4–12 tuần | AI phân tích video bệnh nhân tự quay bài tập → chấm điểm kỹ thuật → gửi phản hồi về Vinmec |
| 6 | **VinBus** (Vận tải công cộng) | Tài xế xe buýt | Tài xế ngồi lái liên tục > 4h → mỏi lưng, giảm tập trung → nguy cơ tai nạn | Mỗi ca trực | AI giám sát tư thế tài xế qua camera cabin, cảnh báo khi phát hiện mệt mỏi hoặc tư thế nguy hiểm |

---

## ⚡ PHASE 2 — QUICK-ASSESS: 3 Quick Problem Cards

> Mục tiêu: Phân tích sâu 3 bài toán tiềm năng nhất để chọn đề tài chính thức.

---

### 🃏 Problem Card #1 — Ưu tiên cao nhất ✅

| Trường | Nội dung |
|--------|----------|
| **Tên bài toán** | Tầm soát sai lệch tư thế cột sống thời gian thực cho nhân viên văn phòng Vinmec |
| **Actor (Ai gặp bài toán?)** | Nhân viên văn phòng tại hệ thống Vinmec (hành chính, kế toán, IT) + Bệnh nhân khám ngoại trú |
| **Quy trình hiện tại (AS-IS)** | Nhân viên ngồi làm việc → không tự nhận ra tư thế sai → sau nhiều tháng bắt đầu đau → đặt lịch khám → chụp X-quang / MRI → bác sĩ chẩn đoán và điều trị |
| **Bottleneck (Điểm nghẽn chính)** | 🔴 Không có cơ chế phát hiện sớm: bệnh chỉ được phát hiện khi đã ở giai đoạn mãn tính. Chụp X-quang tốn kém, có hại bức xạ, không thể làm liên tục |
| **AI Solution** | Computer Vision + Pose Estimation (MediaPipe/BlazePose) phân tích tư thế qua webcam máy tính → tính góc cơ sinh học → cảnh báo tức thì khi phát hiện tư thế nguy hiểm → đề xuất bài tập phục hồi tự động |
| **Metric thành công** | ① Độ chính xác phát hiện tư thế sai ≥ 85% ② Giảm thời gian từ khi có triệu chứng đến chẩn đoán ③ Số người dùng tự kiểm tra hàng tuần tăng ≥ 30% |
| **Rủi ro chính** | Thiên vị thuật toán do dữ liệu huấn luyện chưa đa dạng (BMI, trang phục, ánh sáng); không thay thế chẩn đoán hình ảnh chuyên sâu |

---

### 🃏 Problem Card #2

| Trường | Nội dung |
|--------|----------|
| **Tên bài toán** | Giám sát an toàn lao động tư thế công nhân dây chuyền VinFast |
| **Actor (Ai gặp bài toán?)** | Công nhân dây chuyền lắp ráp xe VinFast; Bộ phận An toàn Lao động (HSE) |
| **Quy trình hiện tại (AS-IS)** | Giám sát viên HSE kiểm tra tư thế thủ công theo ca → ghi biên bản → nhắc nhở trực tiếp → tốc độ xử lý chậm, không thể bao quát toàn bộ dây chuyền cùng lúc |
| **Bottleneck (Điểm nghẽn chính)** | 🔴 Con người không thể quan sát 100% điểm làm việc đồng thời; dữ liệu chấn thương nghề nghiệp chỉ được ghi nhận SAU KHI tai nạn xảy ra |
| **AI Solution** | Camera IP gắn tại dây chuyền + AI phân tích góc cơ thể (cúi lưng > 30°, xoay cổ bất thường) theo thời gian thực → cảnh báo âm thanh + đèn tín hiệu tức thì → log dữ liệu tư thế nguy hiểm lên dashboard HSE |
| **Metric thành công** | ① Giảm ≥ 20% số ca chấn thương nghề nghiệp liên quan cơ xương khớp/năm ② Thời gian phát hiện tư thế nguy hiểm < 3 giây |
| **Rủi ro chính** | Trang phục bảo hộ (mũ, áo dày) làm khó nhận dạng điểm neo khớp; cần cân bằng giữa giám sát và quyền riêng tư của công nhân |

---

### 🃏 Problem Card #3

| Trường | Nội dung |
|--------|----------|
| **Tên bài toán** | Phát hiện vẹo cột sống học đường cho học sinh Vinschool |
| **Actor (Ai gặp bài toán?)** | Học sinh tiểu học & THCS Vinschool; Giáo viên; Y tế học đường; Phụ huynh |
| **Quy trình hiện tại (AS-IS)** | Y tế học đường kiểm tra tư thế học sinh 1–2 lần/năm bằng quan sát thủ công → phát hiện vẹo cột sống muộn → chuyển khám chuyên khoa |
| **Bottleneck (Điểm nghẽn chính)** | 🔴 Tần suất kiểm tra quá thấp (1–2 lần/năm) trong khi trẻ ngồi học sai tư thế hàng ngày; thiếu kênh phản hồi đến phụ huynh kịp thời |
| **AI Solution** | Camera lớp học (hoặc app tablet cá nhân) + AI phân tích tư thế ngồi của từng học sinh → cảnh báo giáo viên qua dashboard → gửi báo cáo tư thế hàng tuần về phụ huynh qua app Vinschool |
| **Metric thành công** | ① Tỷ lệ phát hiện vẹo cột sống giai đoạn sớm (Cobb angle < 20°) tăng ≥ 40% ② Phụ huynh nhận báo cáo hàng tuần ③ Giảm tỷ lệ học sinh cần can thiệp y tế muộn |
| **Rủi ro chính** | Lo ngại về quyền riêng tư / giám sát liên tục trẻ em trong lớp học; cần có sự đồng thuận của nhà trường, phụ huynh và cơ quan quản lý |
