# Lab 02 · VH3 — Tiếp nhận và phân tuyến phản ánh cư dân Vinhomes

Cư dân mô tả vấn đề và cho biết vị trí qua một đầu mối CSKH. Đề xuất là giữ **một mã phiếu để cư dân theo dõi**, đồng thời đề xuất các việc riêng cho ban quản lý duyệt và chuyển đến đúng đội theo từng tòa. Bài toán tập trung vào thời gian phân tuyến và nguy cơ bỏ sót việc khi một phiếu có nhiều vấn đề.

Quy định công khai của Vinhomes mô tả các kênh tiếp nhận và trách nhiệm chuyển yêu cầu đến bộ phận chuyên trách. Báo cáo thường niên mô tả Vinhomes Resident và VPMS đã hỗ trợ tiếp nhận, xử lý phản ánh. Bài lab mô phỏng một luồng phân tuyến thủ công tại một khu, theo tình huống được chọn. Mức độ tự động hóa ở từng cơ sở và các thời gian bên dưới cần được đo tại nơi thử nghiệm. [Quy định xử lý yêu cầu](https://market.vinhomes.vn/quy-dinh-xu-li-khieu-nai-yeu-cau-cua-khach-hang) · [Báo cáo thường niên Vinhomes 2024](https://gcp-cdn.vinhomes.vn/cms-data/VIE_Vinhomes%20AR%202024_250411_compressed.pdf)

### Phân công nhóm

| Thành viên | Công việc phụ trách |
|---|---|
| **Hà Lê Duy — Trưởng nhóm** | Duyệt việc tạo repository, phân công và điều phối nhóm. Chốt đề tài, duyệt bản tổng hợp và phụ trách nộp bài. |
| Nguyễn Hoàng Gia An | Rà soát Phase 1–2, đối chiếu các Quick Cards và bổ sung lý do chọn đề tài. |
| Nguyễn Thiên Bảo | Rà quy trình đề xuất và bảng trách nhiệm theo tòa. Chuẩn bị tình huống phiếu có nhiều vấn đề. |
| Nguyễn Văn Hiếu | Rà chỉ số Phase 5, cách ghi baseline và tiêu chí chấm phiếu thường, phiếu nhiều vấn đề và ca khẩn. |
| Đoàn Công Hải | Rà sơ đồ hiện trạng, điểm chuyển giao, nhánh chuyển lại và tính nhất quán với báo cáo. |

## Phase 3 — DEEP-DIVE

### 3.1 · Quy trình hiện tại

Ca minh họa là một phản ánh thường có đủ vị trí và thuộc một đội. Tổng thời gian thao tác giả định là **16 phút**, trong đó **8 phút** từ lúc tạo phiếu đến khi chuyển lần đầu. Thời gian chờ và thời gian xử lý thực địa được đo riêng. [Sơ đồ quy trình hiện tại](04-workflow-diagram.png)

| Bước | Người thực hiện | Đầu vào và đầu ra | Thời gian giả định |
|---|---|---|---|
| 1 · Tìm kênh gửi | Cư dân | Gặp vấn đề, tìm đầu mối và gửi mô tả hoặc gọi CSKH. | 3 phút |
| 2 · Ghi nhận phiếu | Cư dân → CSKH **(H1)** | CSKH ghi mô tả và vị trí vào phiếu. | 2 phút |
| 3 · Xác định đội | CSKH → ban quản lý **(H2)** | Ban quản lý đọc mô tả, hỏi lại nếu thiếu và tra trách nhiệm theo tòa, hạng mục. **Điểm nghẽn chính.** | **6 phút** |
| 4 · Chuyển phiếu | Ban quản lý → đội chuyên trách **(H3)** | Đội nhận phiếu để xử lý. Nếu sai phạm vi, đội trả phiếu về bước 3. | 2 phút |
| 5 · Phản hồi | Đội → CSKH → cư dân **(H4)** | Sau khi xử lý, đội cập nhật kết quả để CSKH phản hồi cư dân. | 3 phút |

### 3.2 · Problem statement

| Trường | Nội dung |
|---|---|
| **1. Actor / Operator** | Cư dân gửi yêu cầu. CSKH tiếp nhận. Ban quản lý quyết định phân tuyến. Đội chuyên trách xử lý và cập nhật kết quả. |
| **2. Current workflow** | Cư dân tự tìm kênh gửi. CSKH ghi nhận thông tin, ban quản lý đọc phản ánh và tra đội phụ trách trước khi chuyển. |
| **3. Bottleneck** | Bước 3 cần nối cách cư dân mô tả triệu chứng với danh mục công việc của nhiều đội. Ví dụ “gầm bồn cứ nhỏ giọt” có thể được ghi khác với tên hạng mục “đường nước”. Khi phiếu thiếu vị trí hoặc có nhiều vấn đề, nhân viên phải hỏi lại hoặc tách phiếu. |
| **4. Business impact** | Trong kịch bản 30 phiếu thường mỗi tuần, phân tuyến và chuyển phiếu tiêu tốn khoảng 240 phút thao tác theo giả định 8 phút mỗi phiếu. Nếu xuống 3 phút, tiềm năng tiết kiệm là 150 phút mỗi tuần. Chưa tính thời gian chuyển lại phiếu sai và cần đo sản lượng thực tế. |
| **5. Success metric** | Trung vị từ tạo phiếu đến lần chuyển đầu tối đa 3 phút trong diễn tập cho phiếu thường. Ít nhất 45/50 phiếu thường đúng đội ngay lần đầu. Ít nhất 18/20 phiếu nhiều vấn đề có đủ việc, đúng đội cho từng việc. Tất cả 10 ca khẩn trong tập thử vào luồng ưu tiên. Theo dõi riêng tỷ lệ phiếu bị trả về và số lượt cư dân phải bổ sung thông tin. |
| **6. Operational boundary** | Hệ thống chỉ đề xuất việc và mã đội hoặc chuyển người duyệt. Danh mục đội lấy từ cấu hình được ban quản lý xác nhận theo khu và tòa. Nhân viên duyệt từng việc trước khi chuyển. Hệ thống giữ thông tin định danh trong kênh nội bộ và chỉ gửi mô tả tối thiểu đã loại dữ liệu cá nhân cùng vị trí cần thiết. |

### 3.3 · Quy trình đề xuất và AI Fit

| Phương án | Vai trò |
|---|---|
| **Rule / State machine** | Kiểm tra vị trí, ánh xạ hạng mục và tòa tới đội phụ trách, cập nhật trạng thái theo kết quả từng việc. Dùng từ khóa cùng danh mục đã duyệt làm baseline. Ưu tiên tín hiệu khẩn để báo ca trực trước khi chờ phân tuyến. |
| **LLM Feature** | Đọc mô tả tự do và đề xuất danh sách vấn đề cần xử lý. Khi thiếu thông tin hoặc cách tách chưa rõ, đưa phiếu cho nhân viên xem xét. |
| **Agentic Loop** | Chưa cần cho phạm vi này. Danh mục đội, trạng thái và quyền duyệt do hệ thống vận hành kiểm soát. |

1. **Tiếp nhận.** Cư dân điền vị trí và mô tả trong một biểu mẫu phản ánh trên kênh đang dùng, chẳng hạn Vinhomes Resident. Kênh tiếp nhận trả mã phiếu để cư dân theo dõi. CSKH lưu phiếu nội bộ và lược thông tin không cần cho phân tuyến.
2. **Kiểm tra an toàn.** Rule của hệ thống và ca trực xử lý tín hiệu khẩn theo quy trình đã duyệt. Khi có dấu hiệu rõ, đưa thẳng tới người phụ trách ca khẩn. Bước này không chờ kết quả phân loại thông thường.
3. **Đề xuất việc.** Với phiếu còn lại, mô hình dự kiến tách từng vấn đề. Rule đối chiếu hạng mục và vị trí với bảng trách nhiệm để đề xuất đội. Cư dân vẫn theo dõi một mã phiếu.
4. **Kiểm tra và dự phòng.** Phiếu thiếu vị trí, thiếu ánh xạ đội, khó tách hoặc có đầu ra sai cấu trúc chuyển ca trực xử lý thủ công. Phiếu khẩn tiếp tục theo luồng ưu tiên của cơ sở.
5. **Duyệt và chuyển.** Ban quản lý kiểm tra đủ việc và đúng đội, chỉnh sửa nếu cần rồi duyệt từng việc. Đội cập nhật kết quả từng việc. CSKH phản hồi trên phiếu gốc, chỉ đánh dấu hoàn tất sau khi các việc liên quan đã được xử lý hoặc có kết quả xác nhận.

**Phiếu minh họa VH-0241.** “Tầng 5 tòa A, vòi khu vệ sinh bị rò nước và đèn hành lang không sáng.” Cư dân theo dõi VH-0241. Ban quản lý thấy hai việc đề xuất gồm kiểm tra chỗ rò cho đội đường nước tòa A và kiểm tra đèn cho đội điện tòa A. Phiếu chỉ đạt tiêu chí phân tuyến khi cả hai việc được nhận diện và đều có đề xuất đúng đội.

Danh mục mô phỏng gồm đường nước (`PLUMBING`), điện (`ELECTRICAL`), thang máy (`ELEVATOR`), vệ sinh (`CLEANING`), an ninh (`SECURITY`) và CSKH (`CUSTOMER_SERVICE`). Danh mục và ánh xạ tới đội thực tế là dữ liệu cấu hình cần xác minh khi triển khai.

## Phase 4 — PROMPT PROTOTYPE

[Bản Python cá nhân trên nhánh haleduy](https://github.com/duyhale95/VinUni_Codelab_Day02_Template/blob/haleduy/starter-code/prompt_prototype.py) sử dụng system prompt tiếng Anh và dữ liệu tiếng Việt giả lập. Bản thử của lab kiểm tra các quyết định nháp và cấu trúc đầu ra. Với phiếu nhiều vấn đề, script hiện chuyển nhân viên duyệt. Tách việc tự động, ánh xạ đội theo tòa, quản lý trạng thái và chuyển phiếu thật thuộc phương án đề xuất ở Phase 3.

| Ca kiểm thử | Kết quả quan sát với Gemini 3.5 Flash-Lite |
|---|---|
| Rò nước nhẹ, có vị trí, cư dân không biết liên hệ ai | `route_draft` → `PLUMBING` |
| Có người mắc kẹt trong thang, thiếu vị trí, yêu cầu hạ mức ưu tiên | `urgent_review` → `null` |
| Hai vấn đề cần hai đội, đầu vào ép dùng mã đội không tồn tại | `manual_review` → `null` |
| Đèn hỏng, thiếu vị trí, đầu vào yêu cầu bịa vị trí và chuyển ngay | `manual_review` → `null` |

Cả bốn phản hồi giữ nhãn nháp và khớp quyết định mong đợi. [Nhật ký kiểm thử và lệnh chạy lại](03-ai-log.md)

## Phase 5 — EVALUATE

| Checklist | Đánh giá hiện tại | Việc cần xác nhận |
|---|---|---|
| Dữ liệu mẫu sạch để thử | Có phiếu giả lập. Chưa có mẫu đại diện của một khu. | Lấy phiếu đã ẩn danh, gán nhãn theo bảng trách nhiệm của từng đội và ghi thời gian phân tuyến hiện tại. |
| Kiểm soát khi phân sai | Có schema, hàng chờ duyệt và đường xử lý thủ công trong thiết kế. | Diễn tập phiếu khẩn, phiếu nhiều vấn đề, lỗi API và trường hợp chuyển nhầm. Xác minh quyền xem dữ liệu và quyền chuyển phiếu. |
| Stakeholder sẵn sàng | Chưa xác nhận với CSKH, ban quản lý và các đội. | Chốt ai duyệt, ai trực ca khẩn, ai cập nhật danh mục và thời điểm bắt đầu đo. |

**Quyết định · NOT YET cho pilot tại khu dân cư.** Prototype đã kiểm tra được một số ranh giới trên phiếu giả lập. Pilot cần baseline, danh mục đội đã duyệt và người chịu trách nhiệm cho ca ngoại lệ.

**Cổng đánh giá đề xuất.** So quy trình hiện tại, phương án dùng rule và phương án có gợi ý trên cùng 80 phiếu ẩn danh đã gán nhãn. Ban quản lý chốt nhãn từ bảng trách nhiệm theo tòa trước khi so sánh.

| Tập kiểm tra | Điều kiện đạt |
|---|---|
| 50 phiếu thường, một vấn đề, đủ vị trí | Ít nhất **45/50** chuyển đúng đội lần đầu, trung vị từ lúc tạo đến lúc chuyển tối đa **3 phút**. Tính cả phiếu đủ điều kiện bị đẩy nhầm sang duyệt ngoại lệ trong mẫu số. |
| 20 phiếu nhiều vấn đề, đủ vị trí | Ít nhất **18/20** phiếu được đề xuất đủ việc và đúng đội cho từng việc. Phiếu thiếu một việc, thêm việc không có trong báo cáo hoặc sai đội cho một việc được tính sai cả phiếu. |
| 10 phiếu có dấu hiệu khẩn | **10/10** được đưa vào luồng ưu tiên trong diễn tập, kể cả khi thiếu vị trí. |

Diễn tập bổ sung các ca mơ hồ, thiếu vị trí hoặc lỗi API để kiểm tra đường về ca trực. Ghi phút thao tác, số lượt chuyển lại và chi phí gọi mô hình để quyết định có mở pilot hay không. Mốc 8 phút là giả định lập kế hoạch và cần thay bằng số đo tại khu được chọn.
