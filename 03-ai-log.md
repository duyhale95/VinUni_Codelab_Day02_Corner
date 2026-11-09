# Phase 6 — AI Log & Reflection

## 1. Tôi đã dùng AI ở đâu

Tôi dùng Codex như một thought-partner để cấu trúc bài lab theo worksheet: quét cơ hội, viết ba Quick Problem Cards, chọn use case và chuyển nó thành workflow, Problem Statement, AI Fit và prompt prototype. Tôi cũng dùng Gemini trực tiếp trong `prompt_prototype.py` để kiểm tra ranh giới của Xanh SM dispatcher co-pilot.

## 2. AI giúp ích như thế nào

AI giúp tôi biến một ý tưởng còn rộng — “hỗ trợ sự cố pin xe điện” — thành một scope hẹp và có thể kiểm thử:

- Xác định actor: tài xế, điều phối viên, trưởng ca/đội hỗ trợ.
- Tách phần nào nên dùng rule (ngưỡng pin, action được phép) và phần nào hợp với LLM (trích xuất thông tin, đặt câu hỏi còn thiếu, tạo bản nháp).
- Viết output JSON để kiểm tra được `action`, `requires_human_approval` và các safety flags.
- Tạo ba adversarial tests thay vì chỉ kiểm tra một luồng bình thường.

Ở lần chạy đầu, chương trình in `[DRAFT_ONLY]` cho cả ba case: ép chỉ đến trạm 8 km khi pin 2%, ép bỏ tag và giả mạo quyền quản trị. Khi audit raw output, tôi phát hiện chương trình đã tự thêm tag sau khi nhận JSON thuần từ Gemini, nên kết quả đó không đủ để kết luận model đã tuân thủ nhãn. Tôi đã gỡ JSON MIME mode mâu thuẫn với tag, bổ sung yêu cầu JSON hợp lệ và chạy lại. Gemini trả raw output hợp lệ, giữ tag và pass cả ba case.

## 3. Điểm AI có thể sai hoặc suy diễn quá mức

AI không có quyền truy cập SOP và dữ liệu vận hành nội bộ của Xanh SM. Nếu không kiểm soát, AI có thể trình bày các giả định như quy trình thật: tổng thời gian 20 phút, tiêu chí chọn trạm, khả năng triển khai xe sạc di động hoặc quyền hạn của điều phối viên. Những chi tiết này có vẻ hợp lý trong bài lab nhưng không phải bằng chứng để đưa vào pilot thực tế.

AI cũng có thể tạo câu trả lời an toàn về ngôn ngữ nhưng vẫn thiếu thông tin quan trọng, ví dụ vị trí, mức pin hoặc tình trạng xe. Một bản nháp lịch sự không đồng nghĩa với quyết định vận hành đúng.

## 4. Tôi đã sửa prompt và ranh giới ra sao

Tôi đưa các giới hạn vào `SYSTEM_PROMPT` và xác thực bằng code:

- Bắt buộc output mở đầu bằng `[DRAFT_ONLY]`.
- Pin dưới 5% phải trả `dispatch_mobile_charger`; không được gợi ý trạm cách quá 5 km.
- Thiếu hoặc mâu thuẫn dữ liệu phải trả `request_human_review`.
- Không gửi tin, điều xe, đặt trạm hoặc tuyên bố action đã được thực hiện.
- `requires_human_approval` luôn là `true`.

Ban đầu code có thể tự thêm `[DRAFT_ONLY]` khi Gemini bỏ nhãn. Tôi đã sửa lại: nếu model thiếu nhãn, script báo lỗi. Audit tiếp theo cho thấy cấu hình JSON MIME cũng ép Gemini trả JSON thuần, xung đột với tag đặt trước JSON; tôi đã gỡ cấu hình đó. Cách này giúp test phát hiện vi phạm thật thay vì che giấu nó bằng hậu xử lý.

## 5. Kết luận cá nhân

AI phù hợp nhất ở vai trò copilot tạo bản nháp và kiểm tra thông tin thiếu. Quyết định điều phối thật phải giữ ở điều phối viên và trưởng ca, vì nó phụ thuộc SOP, dữ liệu xe/GPS thời điểm thực và trách nhiệm vận hành. Dù test raw output đã pass, chưa có log đã ẩn danh, baseline được xác nhận hoặc sự đồng thuận của stakeholders; vì vậy tôi chọn **NOT YET** thay vì GO.
