# 03 — AI Interaction Log (Nhật Ký Tương Tác AI)
> **Lab 02: AI Product Scoping — Vin Smart Future**
> **Chủ đề:** Sử dụng AI làm trợ lý đồng hành trong quá trình scoping đề tài nghiên cứu hệ thống tầm soát cột sống

---

## 📋 Tổng quan Quá trình Sử dụng AI

| Thông tin | Chi tiết |
|-----------|---------|
| **Công cụ AI sử dụng** | Gemini 2.0 Flash, Claude Sonnet |
| **Mục đích chính** | Brainstorm ý tưởng, cấu trúc hóa bài toán, đánh giá tính khả thi kỹ thuật |
| **Số phiên làm việc** | 4 phiên chính |
| **Thời gian tổng** | ~3 giờ |

---

## 🤝 Session 1 — Brainstorm Bài Toán Vingroup

### Prompt ban đầu:
> *"Tôi muốn làm một dự án AI cho Lab AI Product Scoping của VinUni. Hãy gợi ý cho tôi 10 bài toán thực tế tại các công ty Vingroup mà AI có thể giải quyết."*

### AI đã giúp được gì ✅:
- Liệt kê nhanh 10 bài toán phủ nhiều lĩnh vực: logistics, healthcare, education, retail
- Gợi ý framework đánh giá bài toán (Impact × Feasibility Matrix)
- Phân loại các bài toán theo loại AI phù hợp (Rule-based / ML / LLM)

### AI trả lời sai / Hallucination ⚠️:
- AI tự bịa ra con số *"Vinmec xử lý 5 triệu bệnh nhân/tháng"* — **sai thực tế**, số chính xác thấp hơn nhiều
- AI gợi ý dùng *"Vingroup AI API"* — **không tồn tại**, Vingroup không có API AI công khai
- AI mô tả MediaPipe là sản phẩm của OpenAI — **sai hoàn toàn**, MediaPipe là của **Google**

### Cách tôi xử lý 🔧:
- Cross-check số liệu với báo cáo thường niên Vinmec 2023 (nguồn chính thức)
- Sửa prompt: *"Chỉ đưa ra thông tin kỹ thuật, không bịa số liệu kinh doanh. Nếu không chắc, hãy nói 'cần xác minh'."*
- Kết quả sau khi sửa prompt: AI trả lời thận trọng hơn, có ghi chú "(cần xác minh)" với các con số

---

## 🔬 Session 2 — Thiết kế Kiến trúc Hệ thống

### Prompt:
> *"Hãy giúp tôi thiết kế kiến trúc pipeline cho hệ thống phân tích tư thế cột sống sử dụng camera RGB thường và MediaPipe Pose Estimation. Cần tính toán các góc cơ sinh học nào?"*

### AI đã giúp được gì ✅:
- Mô tả đúng pipeline: Camera → Pose Estimation → Landmark Extraction → Angle Calculation → Decision
- Liệt kê chính xác các góc cơ sinh học quan trọng: Forward Head Posture (FHP), Thoracic Kyphosis, Lumbar Lordosis, Pelvic Tilt
- Gợi ý thư viện Python phù hợp: `mediapipe`, `opencv-python`, `numpy` cho phép tính góc

### AI trả lời sai / Hallucination ⚠️:
- AI bảo dùng `landmark[0]` để lấy tọa độ mũi — **đúng một phần nhưng sai index**: MediaPipe dùng `landmark[0]` cho nose, nhưng AI lại mô tả sai thứ tự một số điểm neo khác (nhầm giữa left/right shoulder)
- AI đề xuất hàm `mp.solutions.pose.get_angle()` — **hàm này không tồn tại** trong MediaPipe library, phải tự viết hàm tính góc từ tọa độ 3D

### Cách tôi xử lý 🔧:
- Kiểm tra trực tiếp MediaPipe documentation chính thức tại `developers.google.com/mediapipe`
- Tự viết hàm `calculate_angle(a, b, c)` sử dụng `numpy.arctan2` thay vì tin vào hàm AI bịa ra
- Sửa prompt: *"Hãy chỉ đề xuất các hàm và thư viện mà bạn XÁC NHẬN TỒN TẠI trong phiên bản MediaPipe 0.10+. Đừng suy đoán."*

---

## 📊 Session 3 — Nghiên cứu Ngưỡng Lâm Sàng

### Prompt:
> *"Theo tiêu chuẩn y tế quốc tế, các góc tư thế nào được coi là bình thường và góc nào cần cảnh báo? Đặc biệt với Forward Head Posture và Thoracic Kyphosis."*

### AI đã giúp được gì ✅:
- Cung cấp thông tin nền về Forward Head Posture: bình thường < 10°, cảnh báo 10–25°, nguy hiểm > 25°
- Giải thích Cobb Angle dùng trong chẩn đoán vẹo cột sống (Scoliosis): bình thường < 10°
- Tóm tắt tài liệu tham khảo từ SPINE Journal và American Medical Association (AMA)

### AI trả lời sai / Hallucination ⚠️:
- AI trích dẫn *"theo nghiên cứu của Harvard Medical School năm 2021..."* với số liệu cụ thể — khi **Google Scholar không tìm thấy bài báo này**
- AI đưa ra ngưỡng Thoracic Kyphosis *"bình thường là < 45°"* — **sai**, tiêu chuẩn lâm sàng phổ biến là 20–45° là bình thường, > 45° mới là bất thường (Hyperkyphosis)

### Cách tôi xử lý 🔧:
- Tìm kiếm trực tiếp trên PubMed và Google Scholar để xác minh ngưỡng lâm sàng
- Bổ sung ranh giới rõ ràng trong prompt: *"Chỉ trả lời dựa trên tài liệu y học bạn biết chắc chắn. Ghi rõ nguồn trích dẫn cụ thể. Nếu không có nguồn chính xác, hãy nói 'cần tham khảo bác sĩ chuyên khoa'."*
- Quyết định: Các ngưỡng lâm sàng chính thức sẽ được **hiệu chỉnh với bác sĩ Vinmec** trước khi tích hợp vào hệ thống

---

## 🎯 Session 4 — Đánh giá Rủi ro & Bias

### Prompt:
> *"Hệ thống AI phân tích tư thế của tôi có thể gặp những vấn đề gì về Algorithmic Bias? Cách giảm thiểu?"*

### AI đã giúp được gì ✅:
- Phân tích sắc nét 4 loại bias chính:
  1. **Appearance Bias**: Model kém chính xác với người mặc quần áo rộng, màu tối
  2. **Body Diversity Bias**: Kém chính xác với BMI rất cao hoặc rất thấp
  3. **Lighting Bias**: Hiệu suất giảm trong điều kiện ánh sáng thấp hoặc ngược sáng
  4. **Ethnicity Bias**: Dữ liệu huấn luyện gốc thiếu đại diện người châu Á (dáng người, tỷ lệ xương khớp khác biệt)
- Gợi ý các giải pháp cụ thể: data augmentation, fine-tuning với data Việt Nam, confidence threshold

### AI trả lời sai / Hallucination ⚠️:
- AI tự tin nói MediaPipe *"đã được fine-tune cho người châu Á trong phiên bản 0.10"* — **sai**, không có tài liệu nào xác nhận điều này
- AI đề xuất dùng *"FairML library của Google"* để kiểm tra bias — **thư viện này không tồn tại**

### Cách tôi xử lý 🔧:
- Kiểm tra release notes MediaPipe trên GitHub → không tìm thấy bằng chứng về Asian-specific fine-tuning
- Sử dụng **Google's What-If Tool** và **IBM AI Fairness 360** (tồn tại và có tài liệu) thay thế
- Thêm vào đề cương mục "Limitations & Bias" với mức độ thận trọng phù hợp

---

## 💡 Bài Học Rút Ra

### AI phù hợp nhất để:
- ✅ **Brainstorm nhanh** và mở rộng ý tưởng ban đầu
- ✅ **Giải thích khái niệm** kỹ thuật phức tạp bằng ngôn ngữ đơn giản
- ✅ **Cấu trúc hóa** tài liệu và tạo outline nhanh
- ✅ **Gợi ý hướng giải quyết** rộng để tôi nghiên cứu tiếp

### AI KHÔNG đáng tin khi:
- ❌ **Trích dẫn số liệu cụ thể** (tỷ lệ %, số bệnh nhân, năm nghiên cứu)
- ❌ **Tên hàm/thư viện lập trình cụ thể** — phải luôn kiểm tra documentation chính thức
- ❌ **Thông tin y tế lâm sàng** — phải xác minh với bác sĩ chuyên khoa
- ❌ **Thông tin về sản phẩm/công ty** — phải xác minh từ nguồn chính thức

### Nguyên tắc sử dụng AI hiệu quả:
> *"Dùng AI như một sinh viên thông minh nhưng hay bịa: Luôn phân công AI brainstorm và tổng hợp, còn việc fact-check cuối cùng là trách nhiệm của mình."*
