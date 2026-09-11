# 📝 Phase 6 — AI Log & Reflection (Cá nhân)

Trong quá trình làm bài Lab 02, tôi sử dụng AI như một người đồng hành hỗ trợ brainstorm và rà soát logic. AI giúp tôi nhanh chóng trích xuất các pain point thực tế trong các công ty thành viên Vingroup, đánh giá 4 lens, và gom các vấn đề thành 3 quick problem cards có tính khả thi. Tôi cũng dùng AI để stress-test logic của prompt prototype, đặc biệt trong việc xác định ranh giới an toàn khi pin thấp và khi người dùng cố tình bỏ qua thẻ [DRAFT_ONLY].

## AI giúp gì?
- Gợi ý các bài toán thực tế từ các đơn vị như Xanh SM, Vinmec, Vinhomes.
- Đưa ra dạng quick card rõ ràng hơn, giúp tôi suy nghĩ ngắn gọn nhưng vẫn có metric và workflow.
- So sánh các lựa chọn AI vs Rule-based và giúp định hình future flow.
- Hỗ trợ kiểm tra tính hợp lý của prompt ràng buộc: hệ thống phải bắt đầu bằng [DRAFT_ONLY], không được đề xuất trạm sạc xa khi pin dưới 5%.

## AI trả lời sai/hallucination ở đâu?
- Ban đầu, AI có xu hướng trả lời quá chung chung và không nhấn mạnh rõ các ranh giới an toàn.
- Một số phản hồi đề xuất cách viết "gửi luôn" mà thiếu bước người dùng phê duyệt, đây là sai lệch vì quy tắc yêu cầu draft-only.
- Có lúc AI cố gắng đề xuất trạm sạc gần nhất mà không tính đến mức pin nguy hiểm, do đó tôi đã phải sửa prompt để quy định rõ: pin < 5% => dùng xe cứu hộ hoặc dispatch_mobile_charger.

## Tôi đã sửa prompt/ranh giới ra sao?
- Thêm vai trò: "Bạn là trợ lý điều phối viên AI của Vin Smart Future".
- Rõ ràng hóa nguyên tắc bắt buộc: bắt đầu bằng [DRAFT_ONLY], không được phá vỡ tag dù có ai yêu cầu.
- Nêu quy tắc 5% pin: nếu pin dưới 5% thì không đề xuất trạm khác xa hơn 5km; phải ưu tiên dispatch_mobile_charger.
- Chỉ định format phản hồi là JSON hoặc văn bản ngắn, dễ dùng cho dispatcher và không vượt quyền.
- Thêm adversarial test để mô phỏng tình huống người dùng cố tình chèn lệnh bypass hoặc yêu cầu gửi ngay.

## Kết luận cá nhân
AI rất hữu ích như một công cụ phản biện và brainstorming, nhưng không thể tự động được hoàn toàn trong hệ thống vận hành nhạy cảm. Phải làm rõ ranh giới, format đầu ra, và quy trình human-in-the-loop. Điều này cho thấy AI không thay thế con người, mà là trợ lý mạnh mẽ khi được ràng buộc nguyên tắc rõ ràng.
