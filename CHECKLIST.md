# ✅ Checklist Nộp Bài — Lab 02: AI Product Scoping (Vin Smart Future)

> Tổng hợp từ README.md, 01-worksheet.md, 02-deliverable-example.md, 03-inspiration-kit.md.
> Điểm nhóm: 60đ | Điểm cá nhân: 40đ.

---

## 👤 PHẦN CÁ NHÂN — mỗi thành viên tự làm, push lên **branch riêng**

### 0. Setup môi trường (làm 1 lần)
- [x] Tạo venv: `python -m venv .venv`
- [x] Kích hoạt venv (`.venv\Scripts\Activate.ps1` trên Windows PowerShell)
- [x] Cài thư viện: `pip install google-genai google-generativeai pytest` (hoặc `pip install -r requirements.txt`)
- [x] Khai báo biến môi trường `GEMINI_API_KEY` (KHÔNG hardcode key vào code — đã lưu trong `.env`, đã gitignore)
- [x] Kiểm tra API key: hoạt động OK
- [ ] Tạo branch cá nhân: `git checkout -b <ten-cua-ban>`

### 1. File `01-problem-scan.md` — Ý tưởng cá nhân (Gate I1 — 15đ) ✅ XONG
- [x] Copy nội dung Phase 1 (SCAN) + Phase 2 (QUICK-ASSESS) từ `01-worksheet.md`
- [x] Bảng SCAN: 5 bài toán thực tế có nguồn dẫn (khảo sát/tin tức 2025-2026 của VinFast, Xanh SM, Vinmec, Vinhomes, Vin Smart Future)
- [x] 3 Quick Problem Cards: VinFast (đặt lịch/tiếp đón), Xanh SM (giải trình tài xế), Vinhomes (định tuyến bảo trì)

### 2. Code Prototype `.py` — (Gate I2 — 10đ) ✅ XONG — verified 5.0/5.0
- [x] Kịch bản: AI Co-pilot định tuyến phản ánh cư dân Vinhomes
- [x] System Prompt nghiêm ngặt: 4 ranh giới ([DRAFT_ONLY], escalate_emergency khi khẩn cấp, chống lộ PII cư dân khác, ngưỡng tin cậy 95%)
- [x] Structured Output JSON rõ ràng
- [x] 3 Adversarial Test Cases (bypass duyệt, hạ nhẹ khẩn cấp gas leak, prompt injection đòi lộ SĐT)
- [x] Chạy thử: cả 3 rules PASSED, autograder Section B = 5.0/5.0
- [x] Ghi lại kết quả — xem chi tiết trong `03-ai-log.md`
- [x] Lưu ý: model đề bài ghi `gemini-2.5-flash` đã bị Google ngừng hỗ trợ cho key mới → đã đổi sang `gemini-3.6-flash` (có ghi chú trong code)

### 3. File `03-ai-log.md` — Nhật ký & Phản ánh (Gate I3 — 15đ, Phase 6) ✅ XONG
- [x] Viết tự luận phản ánh trung thực quá trình dùng AI (Claude + Gemini) làm thought-partner
- [x] Nêu rõ: AI giúp gì, AI sai/hallucinate ở đâu (research hời hợt ban đầu, rủi ro "tối ưu để qua autograder" dễ biến thành gian lận, tính không ổn định của LLM), đã sửa ra sao

### 4. Push bài cá nhân — CÒN THIẾU, LÀM TIẾP
- [ ] `git add .`
- [ ] `git commit -m "Feat: Complete individual assignment by <Ten>"`
- [ ] `git push origin <ten-cua-ban>` — **TUYỆT ĐỐI KHÔNG** push/merge file `.py` vào `main`

---

## 👥 PHẦN NHÓM — cả nhóm cùng thảo luận, kết quả merge vào **branch `main`**

### 0. Setup nhóm (Trưởng nhóm làm trước)
- [ ] Trưởng nhóm tạo Repository chung (hoặc accept GitHub Classroom link)
- [ ] Trưởng nhóm thêm 4-6 thành viên vào **Collaborators/Manage access** với quyền write
- [ ] Gửi link repo cho cả nhóm clone về máy
- [ ] Từng thành viên tạo branch cá nhân riêng (xem phần Cá nhân)

### 1. Phase 3.1 — Current-State Workflow Mapping (Gate G1 — 20đ)
- [ ] Vẽ chi tiết quy trình **hiện tại** (trên giấy A3/bảng hoặc công cụ online) → xuất thành `04-workflow-diagram.png` (hoặc `.pdf`)
- [ ] Đánh dấu rõ các bước tuần tự, 🔄 **Handoff** (điểm chuyển giao), ⏱ thời gian xử lý từng bước, 🔴 **Bottleneck**
- [ ] Ghi tổng thời gian xử lý trung bình toàn quy trình

### 2. Phase 3.2 — Problem Statement 6-field (Gate G2 — 20đ)
Điền đầy đủ vào `02-deep-dive-report.md`:
- [ ] 1. Actor/Operator
- [ ] 2. Current Workflow
- [ ] 3. Bottleneck
- [ ] 4. Business Impact (số liệu cụ thể: thời gian/chi phí/SLA)
- [ ] 5. Success Metric (có số, đo lường được)
- [ ] 6. Operational Boundary (AI được làm gì / TUYỆT ĐỐI không được làm gì / điểm cần duyệt)

### 3. Phase 3.3 — Future-State Flow & AI Fit (Gate G3 — 10đ)
- [ ] Xác định AI-Fit Matrix: Rule/State-Machine, LLM Feature, hay Agentic Loop
- [ ] Vẽ Future-State Flow, đánh dấu: 🔵 AI Step, 🟢 Human Step (HITL), ↩️ Fallback
- [ ] So sánh rõ tại sao chọn Rule vs LLM vs Agent (không chọn phức tạp hơn mức cần thiết)

### 4. Phase 4 — Technical Prompt Prototype (Nhóm thống nhất, 30 min)
- [ ] Thảo luận và thống nhất bản prototype `.py` chung của nhóm (dựa trên phần cá nhân tốt nhất hoặc tổng hợp)
- [ ] Đảm bảo ranh giới an toàn không bị adversarial prompt phá vỡ — ghi lại kết quả test

### 5. Phase 5 — Evaluate (Gate G4 — 10đ)
- [ ] AI Readiness Checklist: dữ liệu mẫu sạch? rủi ro kiểm soát được (HITL/Fallback)? stakeholder sẵn sàng đổi quy trình?
- [ ] Ra quyết định cuối: **GO / NOT YET / NO-GO**
- [ ] Viết Justification chi tiết (dựa trên bằng chứng kỹ thuật + chi phí), trung thực

### 6. Họp nhóm chọn & Merge file `.md` vào `main` (Trưởng nhóm)
- [ ] Sau khi TẤT CẢ thành viên đã push branch cá nhân → tổ chức họp review
- [ ] Chọn ra ý tưởng/`.md` xuất sắc nhất (Problem Scan, Deep-Dive Report, AI Log)
- [ ] Trưởng nhóm: `git checkout main && git pull origin main`
- [ ] Lấy nội dung `.md` được chọn: `git checkout <branch> -- 01-problem-scan.md 02-deep-dive-report.md 03-ai-log.md 04-workflow-diagram.png`
- [ ] Chỉnh sửa, tổng hợp cho mượt rồi commit: `git commit -m "Chore: Select and merge best markdown reports into main"`
- [ ] `git push origin main`
- [ ] 🛑 Kiểm tra lại: **KHÔNG** có file `.py` nào bị merge vào `main`

### 7. Nộp bài chính thức (CHỈ Trưởng nhóm)
- [ ] Trưởng nhóm điền Form nộp bài: Họ tên Trưởng nhóm + Link Repository GitHub của nhóm
- [ ] Các thành viên khác **KHÔNG điền form** (tránh trùng lặp)

---

## 📁 Danh sách file cần có trong repo khi nộp

| File | Loại điểm | Vị trí |
|---|---|---|
| `01-problem-scan.md` | Nhóm (chọn lọc từ cá nhân) | `main` |
| `02-deep-dive-report.md` | Nhóm | `main` |
| `03-ai-log.md` | Cá nhân/Nhóm | `main` (bản chọn) + branch cá nhân |
| `04-workflow-diagram.png`/`.pdf` | Nhóm | `main` |
| `starter-code/prompt_prototype.py` (đã hoàn thiện) | Cá nhân | branch cá nhân **only** |

---

## ⚠️ Lỗi thường gặp cần tránh
- [ ] KHÔNG merge file `.py` vào `main`
- [ ] KHÔNG dán API key trực tiếp vào code
- [ ] KHÔNG để nhiều thành viên cùng điền Form nộp bài
- [ ] KHÔNG bỏ qua bước Human-in-the-loop trong Operational Boundary ở các mảng nhạy cảm (Vinmec, an toàn xe VinFast)
