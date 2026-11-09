# 01 — Problem Scan & Quick Assessment

**Họ và tên:** Nguyễn Thiên Bảo  
**MSSV:** 2A202603861  
**Email:** 26ai.baont5@vinuni.edu.vn  
**Branch:** `nguyenthienbao`  
**Học phần:** Lab 02 — AI Product Scoping — 09/11/2026

> Phạm vi: bài tập thiết kế sản phẩm theo bối cảnh Vin Smart Future. Chưa sử dụng dữ liệu nội bộ hoặc phỏng vấn doanh nghiệp. Toàn bộ sản lượng, thời gian và chi phí trong bài là giả định lập kế hoạch, không phải số liệu công bố của Vinhomes. Tác động là dự báo có điều kiện, chưa phải kết quả triển khai.

## Phase 1 — SCAN: Xác định cơ hội

| # | Đơn vị | Lens | Bài toán / bottleneck | Giá trị doanh nghiệp dự kiến |
|---|---|---|---|---|
| 1 | Xanh SM | Tốn thời gian | Nhân viên phải đọc và tổng hợp mô tả sự cố từ tài xế trước khi lập phiếu hỗ trợ. | Rút ngắn thời gian tiếp nhận, giảm thiếu thông tin khi bàn giao. |
| 2 | VinFast | Lặp lại | Đối soát mã phiên sạc, hóa đơn và ngoại lệ bằng bảng tính thủ công. | Giảm công đối chiếu và lỗi số học; ưu tiên rule-based. |
| **3** | **Vinhomes** | **AI có thể tốt hơn; Lặp lại** | **CSKH đọc mô tả tự do của cư dân và chuyển phiếu sang đúng tổ kỹ thuật; nội dung nhiều vấn đề dễ bị chuyển sai.** | **Kịch bản cơ sở: giải phóng 1.000 giờ công/tháng, giảm 1.200 lượt chuyển sai/tháng nếu đạt các mục tiêu thiết kế.** |
| 4 | Vinpearl | Tốn thời gian | Nhân viên trích xuất ngày, số phòng và yêu cầu từ email đặt phòng đoàn. | Tăng năng lực tiếp nhận và giảm bỏ sót yêu cầu trước khi duyệt. |
| 5 | Vinmec | Pain từ người khác | Nhân viên y tế phải tổng hợp thông tin từ nhiều ghi chú khi soạn nháp hồ sơ. | Giảm thao tác soạn nháp, vẫn bắt buộc chuyên môn kiểm tra. |
| 6 | Vinhomes | Pain từ người khác | Nhân viên phải gộp các phản ánh trùng về một sự cố ở khu vực chung. | Hạn chế giao việc trùng và hỗ trợ theo dõi tiến độ nhất quán. |

Danh sách bao phủ cả bốn lenses của worksheet. Chọn ba cơ hội để đánh giá nhanh: #3, #2 và #4.

## Phase 2 — QUICK-ASSESS: Ba Quick Problem Cards

### Card A — Vinhomes Resident Triage Co-pilot

| Trường | Nội dung |
|---|---|
| Bài toán một câu | Hỗ trợ CSKH tách các vấn đề trong phản ánh tiếng Việt, đề xuất nhãn và tổ tiếp nhận để người phụ trách duyệt trước khi chuyển phiếu. |
| Đơn vị / Actor | Vinhomes; nhân viên CSKH, điều phối kỹ thuật; cư dân là người hưởng lợi gián tiếp. |
| Workflow thủ công | Nhận phiếu → đọc và tách ý → tra danh mục/tổ phụ trách → soạn phản hồi → kiểm tra và chuyển phiếu. |
| Bottleneck | Đọc/tách ý và tra tổ: giả định 4 trong tổng 6 phút/phiếu; dễ bỏ sót một vấn đề khi cùng phản ánh điện và nước. |
| AI hỗ trợ ở đâu? | LLM gợi ý danh sách vấn đề và bản nháp; rule kiểm tra nhãn, vị trí, quyền duyệt và điều kiện khẩn cấp. |
| Metric | Thời gian thao tác trung bình của phiếu được hỗ trợ: 6 → 2 phút; giảm tỷ lệ phiếu phải chuyển lại từ giả định 12% xuống mục tiêu ≤ 4%; macro-F1 đa nhãn ≥ 0,90. |
| Dự báo tác động | 20.000 phiếu/tháng × 75% áp dụng × 4 phút tiết kiệm / 60 = **1.000 giờ/tháng**, tương đương **12.000 giờ/năm**. |
| Quick architecture | LLM Feature + Rule/State-Machine; so sánh với baseline rule + mẫu phản hồi cố định. |
| Boundary | Chỉ nháp; không tự chuyển/đóng phiếu, tiết lộ hồ sơ cư dân, cam kết SLA hoặc phê duyệt bồi thường. Ca nguy cấp đề xuất người phụ trách xem ngay. |

### Card B — VinFast: Đối soát hóa đơn sạc

| Trường | Nội dung |
|---|---|
| Bài toán một câu | Phát hiện phiên thiếu, trùng và lệch số tiền giữa bảng giao dịch và hóa đơn. |
| Đơn vị / Actor | VinFast; kế toán đối soát. |
| Workflow thủ công | Nhận hai bảng → chuẩn hóa mã → so khớp → rà ngoại lệ → duyệt. |
| Bottleneck | Đối chiếu và rà ngoại lệ: giả định 20/30 phút cho một lô 100 dòng. |
| Cách hỗ trợ | Code join theo mã và tính chênh lệch; chưa cần LLM cho số học. |
| Metric / tác động | Mục tiêu 30 → 5 phút/lô; 200 lô/tháng có thể giải phóng 200 × 25 / 60 = **83,3 giờ/tháng**. |
| Quick architecture | Rule; NO-GO cho việc dùng LLM làm lõi đối soát. |
| Boundary | Chỉ báo ngoại lệ, không thanh toán hoặc sửa sổ. |

### Card C — Vinpearl: Trích xuất yêu cầu đặt phòng đoàn

| Trường | Nội dung |
|---|---|
| Bài toán một câu | Tạo nháp cấu trúc ngày ở, số phòng và yêu cầu đặc biệt từ email đoàn khách. |
| Đơn vị / Actor | Vinpearl; nhân viên đặt phòng. |
| Workflow thủ công | Nhận email → đọc điều kiện → nhập bảng → đối chiếu → phản hồi. |
| Bottleneck | Đọc email và nhập lại: giả định 8 trong tổng 12 phút/email. |
| AI hỗ trợ | Trích xuất có dẫn lại nội dung nguồn và đánh dấu trường chưa rõ. |
| Metric / tác động | Mục tiêu 12 → 5 phút/email; 2.000 email/tháng × 60% áp dụng × 7 / 60 = **140 giờ/tháng**. |
| Quick architecture | LLM Feature + kiểm tra trường và người duyệt. |
| Boundary | Không tự xác nhận phòng, giá, ưu đãi hay thu tiền. |

## Lựa chọn ý tưởng và dự báo lợi ích

Chọn **Card A — Vinhomes Resident Triage Co-pilot** cho phần phân tích sâu và báo cáo. Bài code cá nhân giữ tình huống pin Xanh SM theo starter code. Bài toán xử lý ngôn ngữ tự do có lý do dùng LLM rõ hơn đối soát số học; đầu ra nháp cho phép giữ quyền quyết định ở CSKH. Theo các kịch bản giả định, Card A có quy mô cơ hội giờ công lớn nhất; đây là so sánh để ưu tiên kiểm chứng, chưa phải bằng chứng doanh nghiệp thực sự có các sản lượng nêu trên.

Ngoài **1.000 giờ công/tháng** trong kịch bản cơ sở, mục tiêu chuyển sai 12% → 4% trên 15.000 phiếu được hỗ trợ tương ứng **1.200 lượt chuyển lại có thể tránh/tháng**. Không cộng thêm số giờ từ lượt chuyển lại vào 1.000 giờ vì có nguy cơ đếm trùng. Lợi ích gồm giảm thao tác hành chính, tăng khả năng tiếp nhận giờ cao điểm, hạn chế cư dân phải nhắc lại nội dung và giữ đủ các vấn đề khi bàn giao kỹ thuật.

## Phản biện trước khi đầu tư

1. **Sản lượng chưa xác minh:** cần log để kiểm tra giả định 20.000 phiếu/tháng trong một phạm vi nhiều khu/tòa; không gán sản lượng này cho một tòa đơn lẻ.
2. **Rule có thể đã đủ:** so sánh LLM với từ khóa + mẫu tin trên cùng dữ liệu; chỉ giữ LLM khi lợi ích đo được lớn hơn chi phí và công sửa nháp.
3. **Giờ công không tự biến thành tiền mặt:** thời gian giải phóng chủ yếu là năng lực phục vụ; chỉ có tiết kiệm ngân sách khi giảm làm thêm, thuê ngoài hoặc có cơ chế sử dụng năng lực rõ ràng.
4. **Sai phân loại có hậu quả vận hành:** ca nhiều ý, khẩn cấp, thiếu vị trí và yêu cầu trái quyền phải có test riêng, validator và người duyệt. Chưa đủ cơ sở triển khai tự động.
