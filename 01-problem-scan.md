# Lab 02 · Quét cơ hội và thẻ bài toán cá nhân

## Phase 1 — SCAN

Nguồn công khai xác nhận bối cảnh nghiệp vụ. Các điểm nghẽn dưới đây là giả thuyết để kiểm chứng với nhân viên vận hành. Cột đo lường đề xuất chỉ số cho pilot.

| Mã | Đơn vị | Lens | Bài toán và điểm nghẽn cần kiểm chứng | Đo bằng gì? |
|---|---|---|---|---|
| **VH1** | Vinhomes | Tốn thời gian | **Tìm hạng mục còn treo khi tái nghiệm thu căn hộ.** Kỹ thuật viên phải nối từng mục trong biên bản ban đầu với ảnh và ghi chú khắc phục. Một ghi chú “đã xử lý cửa” có thể khớp với nhiều cửa. Gợi ý liên kết theo phòng, hạng mục và mã lỗi để kỹ thuật viên xác nhận. [Bối cảnh bàn giao và khắc phục khiếm khuyết](https://gcp-cdn.vinhomes.vn/cms-data/1_1723028369.pdf) | Phút rà soát mỗi biên bản, tỷ lệ mục còn mở bị bỏ sót. |
| **VH2** | Vinhomes | Lặp lại | **So phạm vi thi công giữa đơn đăng ký và bản mô tả của nhà thầu.** Ban quản lý phải phát hiện trường hợp đơn ghi “lắp tủ” nhưng phương án đính kèm có thêm hạng mục tháo vách. Trích đoạn mâu thuẫn để nhân viên yêu cầu bổ sung hoặc chuyển kỹ thuật xem xét. [Tài liệu dự án về đăng ký thi công](https://gcp-cdn.vinhomes.vn/cms-data/2024_1724474003.pdf) | Tỷ lệ phát hiện chênh lệch phạm vi trên bộ hồ sơ gán nhãn, số lượt yêu cầu bổ sung. |
| **VH3** | Vinhomes | Pain từ người khác | **Tiếp nhận và phân tuyến phản ánh cư dân.** Cư dân gặp sự cố như rò nước hoặc đèn hành lang hỏng nhưng không biết đội nào phụ trách. Trong kịch bản khảo sát, CSKH hoặc ban quản lý phải đọc phản ánh, hỏi lại vị trí nếu thiếu và chuyển phiếu cho đội chuyên trách. Cư dân gửi qua một đầu mối, nhân viên duyệt tuyến được đề xuất. [Quy định tiếp nhận và chuyển yêu cầu](https://market.vinhomes.vn/quy-dinh-xu-li-khieu-nai-yeu-cau-cua-khach-hang) | Phút từ tạo phiếu đến lần chuyển đầu, tỷ lệ đến đúng đội ngay lần đầu. |
| **VM1** | Vinmec | Lặp lại | **Rà chứng từ trước khi gửi hồ sơ bảo lãnh viện phí.** Nhân viên viện phí phải dò xem từng chứng từ theo checklist đã duyệt có thực sự nằm trong bộ hồ sơ. Ví dụ, tên tệp ghi “bảng kê” nhưng nội dung là một chứng từ khác. Gợi ý mục thiếu hoặc cần đọc lại, kèm trang đối chiếu. [Quy trình thanh toán bảo hiểm tại Vinmec](https://www.vinmec.com/vie/bai-viet/huong-dan-thu-tuc-thanh-toan-bao-hiem-tai-vinmec-vi) | Phút rà mỗi hồ sơ, tỷ lệ hồ sơ phải bổ sung chứng từ sau lần gửi đầu. |
| **VM2** | Vinmec | Tốn thời gian | **Phát hiện lệch lịch lấy mẫu và lịch khám theo đoàn.** Điều phối viên phải đối chiếu thông báo đổi giờ hoặc địa điểm với hai lịch liên quan. Trích xuất yêu cầu thay đổi từ tin nhắn, dùng rule kiểm tra lịch và đưa ca lệch vào danh sách xác minh. [Ví dụ luồng đặt lịch khám doanh nghiệp](https://sktqdn.vinmec.com/sktqdn/faq) | Tỷ lệ ca lệch lịch được phát hiện trước giờ hẹn, tỷ lệ cảnh báo nhầm. |
| **VM3** | Vinmec | AI có thể tốt hơn | **Phát hiện phiếu hướng dẫn trước khám dùng nhầm phiên bản.** Khi gói khám hoặc địa điểm thay đổi, nhân viên tiếp đón cần đối chiếu phiếu đang chuẩn bị gửi với bản hướng dẫn đã duyệt cho lịch mới. AI chỉ đánh dấu khác biệt và dẫn nguồn để nhân viên y tế xác nhận. [Bối cảnh hướng dẫn chuẩn bị khám](https://www.vinmec.com/vie/bai-viet/kham-suc-khoe-tong-quat-can-chuan-bi-nhung-gi-vi) | Tỷ lệ nhận ra phiếu sai phiên bản, thời gian đối chiếu mỗi lịch hẹn. |
| **VP1** | Vinpearl | Pain từ người khác | **Đối chiếu đồ thất lạc qua mô tả đa ngôn ngữ.** Tiền sảnh cần nối lời báo như “blue pouch with a fish charm” với sổ đồ nhặt được ghi “túi xanh có móc cá”. Lọc theo thời gian và khu vực, xếp hạng ứng viên theo mô tả, rồi để nhân viên xác minh chủ sở hữu. [Nhiệm vụ xử lý đồ thất lạc của tiền sảnh](https://careers.vinpearl.com/job/vinpearl-nha-trang-truong-bo-phan-tien-sanh-4416) | Tỷ lệ món đúng nằm trong top 3, tỷ lệ ca không có đồ khớp được đánh dấu đúng, phút tra cứu. |
| **VP2** | Vinpearl | Tốn thời gian | **Giữ thông tin nhất quán khi đổi chặng ra đảo Hòn Tre.** Nếu phương tiện thay đổi, nhân viên tiền sảnh cần đối chiếu các nhóm khách đang chờ với trạng thái đã xác nhận từ bến và điểm đón xe điện. AI soạn nháp bàn giao, dùng giờ và địa điểm từ dữ liệu do ca trực cung cấp. [Luồng check-in và ra đảo](https://vinpearl.com/vi/huong-dan-thu-tuc-va-quy-trinh-check-in-vinpearl-nha-trang) · [Điểm nối xe điện](https://vinpearl.com/vi/review-vinpearl-resort-spa-nha-trang-bay) | Phút cập nhật các điểm bàn giao, số hướng dẫn mâu thuẫn trong một lượt diễn tập. |
| **VP3** | Vinpearl | AI có thể tốt hơn | **Tìm thay đổi phút chót trong phương án hội nghị.** Điều phối viên phải so bản bàn giao đã duyệt với chuỗi yêu cầu mới về sơ đồ ghế, giờ giải lao và thiết bị. Tóm tắt phần thay đổi theo đội phụ trách, kèm đoạn xác nhận và cờ cho các yêu cầu còn mâu thuẫn. [Bối cảnh dịch vụ hội họp](https://vinpearl.com/vi/hotels/vinpearl-resort-nha-trang/meeting-and-events) | Tỷ lệ thay đổi quan trọng được phát hiện, phút lập bản bàn giao. |

## Phase 2 — QUICK-ASSESS

Ba thẻ được chọn vì có người chịu trách nhiệm rõ, đầu vào mô phỏng được và kết quả có thể chấm theo nhãn. Thời gian dưới đây là giả định lập kế hoạch. Các ngưỡng là mục tiêu thử nghiệm.

### VH3 · Vinhomes — Tiếp nhận và phân tuyến phản ánh cư dân

- **Actor.** Cư dân cần báo sự cố qua một đầu mối. CSKH hoặc ban quản lý chịu trách nhiệm phân tuyến. Đội chuyên trách tiếp nhận để xử lý.
- **Quy trình giả định.** (1) Cư dân tìm kênh gửi → (2) CSKH ghi phiếu → (3) ban quản lý đọc, xác minh vị trí và tra đội phụ trách → (4) chuyển phiếu → (5) đội xử lý cập nhật kết quả để CSKH phản hồi.
- **Điểm nghẽn.** Bước 3–4 mất khoảng 8 phút thao tác mỗi phiếu đủ dữ liệu. Cư dân mô tả triệu chứng theo cách của mình, còn nhân viên phải tra đội theo từng tòa và hạng mục. Phiếu chuyển sai phải quay lại bước phân tuyến.
- **Can thiệp và kiến trúc đề xuất.** Cư dân gửi qua một đầu mối và theo dõi bằng một mã phiếu. LLM đề xuất các việc cần xử lý. Rule đối chiếu từng việc với danh mục đội theo tòa, ưu tiên tín hiệu khẩn và chuyển trường hợp thiếu dữ liệu cho ca trực. Ban quản lý duyệt từng việc trước khi chuyển.
- **Metric.** Trung vị từ lúc tạo phiếu đến lần chuyển đầu tối đa 3 phút trong diễn tập, so với giả định 8 phút. Ít nhất 45/50 phiếu thường đúng đội ngay lần đầu và 18/20 phiếu nhiều vấn đề được đề xuất đủ việc, đúng đội cho từng việc. Tất cả ca khẩn trong tập thử phải đi vào luồng ưu tiên.

### VP1 · Vinpearl — Đối chiếu đồ thất lạc qua mô tả đa ngôn ngữ

- **Actor.** Nhân viên tiền sảnh tra sổ. Khách chờ kết quả và nhân viên giữ đồ chờ xác nhận bàn giao.
- **Quy trình giả định.** (1) Nhận báo mất → (2) mở sổ đồ nhặt được → (3) dò ứng viên theo thời gian, khu vực và mô tả → (4) hỏi khách đặc điểm xác minh → (5) ghi kết quả và phối hợp bàn giao nếu đã xác minh.
- **Điểm nghẽn.** Bước 3 mất khoảng 7 phút mỗi ca có ứng viên. Cùng một đặc điểm có thể được viết bằng hai ngôn ngữ hoặc nhiều cách gọi.
- **Can thiệp và kiến trúc.** Rule lọc theo thời gian và khu vực. LLM gợi ý tối đa ba mã từ tập đã lọc. Nhân viên tra lại sổ, xác minh chủ và quyết định bàn giao.
- **Metric.** Trung vị bước đối chiếu tối đa 3 phút, so với giả định 7 phút. Mã đúng xuất hiện trong top 3 ở ít nhất 90% ca có món khớp. Ít nhất 90% ca có đủ mô tả và không có món khớp được đánh dấu đúng.

### VM1 · Vinmec — Rà chứng từ trước khi gửi hồ sơ bảo lãnh viện phí

- **Actor.** Nhân viên viện phí chuẩn bị hồ sơ gửi đơn vị bảo hiểm.
- **Quy trình giả định.** (1) Chọn checklist đã duyệt → (2) tập hợp tệp → (3) rà từng chứng từ và đánh dấu mục thiếu → (4) bổ sung hoặc xin bản rõ hơn → (5) duyệt và gửi hồ sơ.
- **Điểm nghẽn.** Bước 3 mất khoảng 8 phút mỗi bộ hồ sơ. Tên tệp, tiêu đề chứng từ và nội dung thực tế có thể lệch nhau.
- **Can thiệp và kiến trúc.** LLM gợi ý loại chứng từ và trang đối chiếu. Rule so với checklist đã duyệt. Nhân viên viện phí xác nhận nội dung trước khi gửi.
- **Metric.** Trung vị thời gian rà tối đa 4 phút. Phát hiện ít nhất 95% mục thiếu hoặc đính nhầm trên bộ hồ sơ giả lập gán nhãn.

### Lý do nhóm chọn VH3

| Phương án đã thảo luận | Giá trị và điều kiện kiểm chứng |
|---|---|
| **Vinhomes — Phân tuyến phản ánh cư dân** | Có thể kiểm chứng tại một khu với phiếu đã ẩn danh, bảng trách nhiệm theo tòa và nhật ký chuyển việc. Đo riêng thời gian phân tuyến và số việc bị bỏ sót, kể cả khi một phiếu cần nhiều đội. |
| **Vinmec — Tóm tắt bệnh án xuất viện** | Có tiềm năng giảm thời gian soạn hồ sơ. Để đánh giá cần hồ sơ lâm sàng được phép sử dụng và bác sĩ chấm sai sót. Tổng thời gian xuất viện còn phụ thuộc các khâu thanh toán và thủ tục. |
| **Xanh SM — Xử lý sự cố sạc pin thực địa** | Có tiềm năng hỗ trợ điều phối trong tình huống khẩn. Cần dữ liệu vị trí, trạng thái trạm và quy trình cứu hộ đã xác nhận cho đội xe để thử an toàn. |

**Nhóm chọn VH3 cho Phase 3–6** vì có thể thử ở quy mô một khu, đo ngay tại bước chuyển việc và giữ quyền quyết định ở ban quản lý. Một phiếu chỉ được tính đúng nếu mọi vấn đề trong phiếu đều được nhận diện và chuyển đến đúng đội. [Báo cáo phân tích sâu](02-deep-dive-report.md)
