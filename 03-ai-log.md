# 03 — AI Log & Reflection (Cá nhân)

> **Người thực hiện:** [Điền tên bạn]
> **Công cụ AI sử dụng:** Claude (Claude Code) làm trợ lý đồng hành xuyên suốt buổi lab; Google Gemini (`gemini-3.6-flash`) là model được stress-test trong bài Code Prototype.

---

## 1. AI đã giúp tôi những gì?

Trong buổi lab này tôi dùng Claude như một "đồng nghiệp lập trình" hơn là một công cụ trả lời một chiều:

- **Đọc & tổng hợp tài liệu:** Claude đọc toàn bộ 4 file `.md` của đề bài (README, worksheet, deliverable-example, inspiration-kit) và tự dựng ra một checklist việc-cá-nhân/việc-nhóm để tôi bám theo suốt buổi, tránh bỏ sót gate chấm điểm nào.
- **Research thực tế:** Khi tôi yêu cầu bài toán trong SCAN phải có căn cứ thật (không chỉ dựa vào ví dụ hư cấu trong inspiration-kit), Claude đã chủ động search nhiều vòng các bài báo, khảo sát chính thức của VinFast/Xanh SM/Vinmec/Vinhomes/Vingroup trong 2025-2026, rồi tự lọc ra những vấn đề **còn tồn đọng thật** thay vì những vấn đề Vingroup đã giải quyết tốt (ví dụ AI chẩn đoán hình ảnh ở Vinmec).
- **Viết & debug code:** Claude viết `SYSTEM_PROMPT`, hàm `evaluate_prompt()` dùng SDK `google-genai`, và 3 adversarial test cases. Khi chạy thử thất bại, Claude tự chẩn đoán và sửa liên tiếp 3 lỗi môi trường thật (không phải lỗi logic): file `.env` sai định dạng (thừa khoảng trắng quanh dấu `=`), lỗi `UnicodeEncodeError` do console Windows không đọc được emoji, và model `gemini-2.5-flash` bị Google ngừng hỗ trợ cho key mới (lỗi 404) — đề bài viết cho model cũ nhưng thực tế API đã đổi sang `gemini-3.6-flash`.
- **Đối chiếu điểm số khách quan:** Thay vì chỉ nói "chắc ổn rồi", Claude chủ động chạy `autograder.py` thật để tôi thấy điểm số cụ thể (4.5/5 → 5.0/5) thay vì đoán mò.

## 2. AI đã sai / hallucinate ở đâu?

- **Thiếu căn cứ thực tế ở vòng đầu:** Ở bảng SCAN đầu tiên, Claude chỉ paraphrase lại các ví dụ có sẵn trong `03-inspiration-kit.md` (vốn là ví dụ minh hoạ mang tính hư cấu của đề bài) mà không tự đặt câu hỏi liệu đó có phải vấn đề *thật* đang xảy ra hay không. Tôi phải yêu cầu rõ "research dựa trên các bài nghiên cứu/khảo sát thực tế" thì Claude mới đổi cách tiếp cận.
- **Đề xuất một hướng có thể coi là "gian lận" nếu tôi không tỉnh táo:** Khi autograder chỉ chấm 4.5/5 vì thiếu từ khóa, một trong hai lựa chọn Claude đưa ra là "thêm từ khóa hợp lý" — nếu làm ẩu, đây rất dễ biến thành hành vi nhồi các cụm từ vô nghĩa (`"5%"`, `"dispatch_mobile_charger"`) chỉ để đánh lừa máy chấm điểm tự động, không phản ánh đúng ranh giới an toàn thật của bài toán tôi chọn (Vinhomes). Tôi đã yêu cầu chỉ thêm luật nào **thật sự hợp lý về mặt kỹ thuật** (ngưỡng độ tin cậy phân loại 95% → chuyển người duyệt), và Claude cũng tự nêu rõ sẽ không nhồi từ khóa giả vì "không trung thực" — nhưng ranh giới giữa "tối ưu để qua bài chấm" và "gian lận máy chấm" là điều bản thân tôi phải chủ động cảnh giác, không thể phó mặc hoàn toàn cho AI tự quyết định đúng-sai đạo đức.
- **Kết quả không ổn định do bản chất LLM:** Khi chạy adversarial test 3 lần, có 1 lần "Rule 3" báo Failed dù logic prompt không đổi — nguyên nhân là Gemini trả lời khác nhau giữa các lần gọi (không có `temperature=0`) và cách kiểm tra ban đầu của Claude (so khớp chuỗi con thô như `"090" in output`) quá dễ bị false-positive/false-negative. Đây không hẳn là "sai" của Claude mà là một giới hạn cần lường trước khi thiết kế bài kiểm thử ranh giới AI — bài học thực tế cho chính bài toán scoping AI của tôi.

## 3. Tôi đã sửa prompt/ranh giới với AI như thế nào?

- Tôi không chấp nhận câu trả lời "chắc đúng rồi" — mọi lần Claude nói code chạy được, tôi (thông qua yêu cầu) đều bắt chạy thật bằng `autograder.py` và script thật với API key thật để có bằng chứng bằng số điểm cụ thể.
- Tôi liên tục hỏi lại để làm rõ ranh giới giữa "làm đúng chuẩn" và "làm cho qua máy chấm" (ví dụ câu hỏi "cái code là làm cá nhân hay là nhóm nhỉ", hay từ chối phương án dùng nguyên kịch bản mẫu có sẵn để giữ đúng bài toán cá nhân tôi chọn), buộc AI phải giải thích rõ trade-off thay vì tự ý quyết định thay tôi.
- Khi phát hiện kết quả không ổn định (test 3 lúc pass lúc fail), tôi yêu cầu Claude sửa tận gốc (thêm `temperature=0`, dùng regex số điện thoại chuẩn) thay vì chấp nhận "thỉnh thoảng fail cũng được".

## 4. Bài học rút ra

Dùng AI hiệu quả nhất khi tôi đóng vai trò **người ra quyết định và người kiểm chứng**, còn AI đóng vai trò người thực thi nhanh + phản biện. Những chỗ AI dễ sai nhất không phải là cú pháp code, mà là những chỗ cần **phán đoán về tính trung thực và mức độ chính xác thực tế** — đúng như ranh giới an toàn (Operational Boundary) mà chính bài toán Vinhomes tôi chọn cũng đang cố giải quyết cho AI thật.
