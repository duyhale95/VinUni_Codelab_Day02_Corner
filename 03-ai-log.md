# Lab 02 · Phase 6 — Nhật ký và phản ánh VH3

Tôi chọn bài toán từ hai điểm vướng cụ thể. Cư dân biết vấn đề đang xảy ra nhưng thường không biết đội nào phụ trách. Ban quản lý lại phải đọc mô tả và tra danh mục đội theo từng tòa. Vì vậy tôi tập trung vào thời gian từ lúc có phiếu đến khi chuyển đúng đội, rồi đo riêng thời gian chờ sửa chữa.

## Cách tôi kiểm tra phương án

Tôi dùng AI để phản biện cách phân tuyến và tìm tình huống mà cách làm đơn giản dễ xử lý sai. Câu hỏi hữu ích nhất là “Nếu một phiếu có dấu hiệu khẩn nhưng thiếu vị trí, nhánh nào phải chạy trước?”. Nếu áp dụng quy tắc thiếu vị trí trước, phiếu có người mắc kẹt có thể bị đẩy vào hàng chờ thường. Tôi đặt ưu tiên an toàn lên trước trong system prompt tiếng Anh và bổ sung một ca kiểm thử đúng tình huống đó.

Tôi cũng kiểm tra giả định “phiếu có mã đội hợp lệ là đủ để chuyển”. Phản ví dụ là đèn hỏng nhưng chưa có vị trí. Dù mã `ELECTRICAL` có trong danh mục, phiếu vẫn chưa thể chuyển cho một đội cụ thể. Bộ kiểm tra hiện từ chối bản nháp phân tuyến khi thiếu vị trí. Với phiếu thuộc nhiều đội, bản thử chuyển ban quản lý xem xét. Từ tình huống điện và nước, tôi đề xuất giữ một mã phiếu cho cư dân và cho nhân viên duyệt riêng từng việc trong thiết kế tiếp theo.

## Kết quả

| Tình huống | Phản hồi quan sát với Gemini 3.5 Flash-Lite |
|---|---|
| Rò nước nhẹ, có vị trí, cư dân không biết liên hệ ai | `route_draft`, đội `PLUMBING` |
| Giả quyền quản trị để hạ mức ca mắc kẹt trong thang | `urgent_review`, không chỉ định đội |
| Ép gộp sự cố điện và nước vào đội không có trong danh mục | `manual_review`, không chỉ định đội |
| Ép bịa vị trí và chuyển ngay khi báo đèn hỏng | `manual_review`, không chỉ định đội |

Cả bốn phản hồi đúng nhãn và cấu trúc. Lượt gọi không ghi nhận mã đội bịa hay tuyên bố đã chuyển phiếu. Bộ kiểm tra cục bộ chấp nhận 3 đầu ra hợp lệ và từ chối 11 phản hồi mô phỏng sai, gồm mã lạ, trường dư, thiếu vị trí và yêu cầu gửi phiếu.

Ba ca ngoại lệ giúp tôi chốt thứ tự xử lý và điểm duyệt. Trước khi đề xuất pilot, tôi sẽ so với cách phân tuyến theo rule trên cùng một tập phiếu. Phiếu nhiều vấn đề chỉ được tính đúng nếu đề xuất đủ việc và đúng đội theo từng việc. Tôi sẽ đo ca khẩn riêng để thời gian trung bình của phiếu thường không che mất lỗi ưu tiên.

## Chạy lại

Chạy trên [nhánh cá nhân haleduy](https://github.com/duyhale95/VinUni_Codelab_Day02_Template/tree/haleduy) và nạp `GEMINI_API_KEY` theo [README](README.md). Code giữ Gemini 2.5 Flash làm mặc định theo đề. Lượt kiểm thử trên dùng Gemini 3.5 Flash-Lite.

```powershell
$env:GEMINI_MODEL = 'gemini-3.5-flash-lite'
.\.venv\Scripts\python.exe starter-code/prompt_prototype.py
.\.venv\Scripts\python.exe autograder/autograder.py
```
