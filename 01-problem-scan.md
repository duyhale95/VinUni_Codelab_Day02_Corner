# Phase 1 — Problem Scan & Quick-Assessment

**Vai trò:** AI Product Engineer, Vin Smart Future  
**Nguyên tắc scoping:** Các mô tả về sản phẩm và kênh phục vụ dưới đây được đối chiếu với nguồn công khai. Số phút, tỷ lệ và lưu lượng chưa có dữ liệu nội bộ được ghi là **giả định cần đo bằng log/pilot**, không phải dữ kiện vận hành của doanh nghiệp.

## 1. Scan: các cơ hội vận hành

| # | Công ty thành viên | Lens | Bài toán quan sát được | Vì sao đáng khảo sát |
|---:|---|---|---|---|
| 1 | Vinhomes | Tốn thời gian | Phân loại và chuyển xử lý yêu cầu của cư dân từ tổng đài, email, Ban quản lý và ứng dụng. | Một yêu cầu có thể liên quan đến kỹ thuật, an ninh, vệ sinh hoặc CSKH; việc gán sai nhóm làm tăng vòng chuyển tiếp. |
| 2 | Vinhomes | Pain từ stakeholder | Tóm tắt tình trạng ticket và soạn phản hồi tiến độ cho cư dân. | Cư dân cần biết ai đang xử lý và khi nào có cập nhật; nhân viên phải đọc nhiều ghi chú từ các bộ phận trước khi phản hồi. |
| 3 | VinFast | AI-upgrade | Hỗ trợ khách hàng gặp lỗi sạc hoặc cần tìm thông tin trạm sạc trong ứng dụng. | Ứng dụng đã hiển thị mức pin, trạng thái xe và hỗ trợ tìm trạm sạc; câu hỏi tự do vẫn cần được diễn giải thành hướng dẫn an toàn. |
| 4 | Vinmec | Lặp lại | Tiền kiểm thông tin đặt lịch trực tuyến: chuyên khoa, thời gian, giấy tờ và hướng dẫn trước khám. | Online Vinmec đã cho phép tìm dịch vụ và đặt lịch; những câu hỏi hành chính có thể lặp lại nhưng không được để AI tư vấn chẩn đoán. |
| 5 | Xanh SM | Pain từ stakeholder | Điều phối hỗ trợ khi tài xế báo pin xe điện ở mức nguy cấp giữa ca làm việc. | Tài xế cần hướng dẫn nhanh, trong khi điều phối viên phải kiểm tra vị trí, mức pin, quy tắc an toàn và phương án hỗ trợ. |
| 6 | Vinpearl / VinWonders | Lặp lại | Tóm tắt đánh giá sau lưu trú/vui chơi và phân loại theo dịch vụ cần cải thiện. | Đánh giá ngôn ngữ tự do thường lặp lại theo chủ đề như phòng, hàng chờ, vệ sinh và nhân viên; cần giữ nguyên bằng chứng gốc để kiểm tra. |

### Căn cứ công khai dùng để khoanh vùng

- Quy định của Vinhomes nêu nhiều kênh tiếp nhận như tổng đài, email, Ban quản lý và các bộ phận xử lý; tài liệu cũng mô tả luồng tiếp nhận, xử lý và phản hồi khách hàng. [Quy định xử lý khiếu nại/yêu cầu của Vinhomes](https://gcp-cdn.vinhomes.vn/cms-data/3_VHM_Quy%20dinh%20xu%20ly%20khieu%20nai%20yeu%20cau%20cua%20KH.pdf)
- Ứng dụng Vinhomes Resident kết nối cư dân với Ban quản lý và hỗ trợ các yêu cầu, tiện ích trong khu đô thị. [Vinhomes](https://vinhomes.vn/vi/nhung-la-thu-cam-on-tu-cu-dan-va-dich-vu-tu-trai-tim-vinhomes)
- Ứng dụng VinFast cung cấp theo dõi trạng thái xe, mức pin thời gian thực và tìm trạm sạc. [VinFast](https://vinfastauto.com/vn_vi/su-dung-tinh-nang-lien-quan-den-pin-tren-ung-dung-vinfast)
- Xanh SM vận hành dịch vụ di chuyển bằng xe điện và có kênh hỗ trợ tài xế. Bài toán sự cố pin bên dưới là **tình huống lab giả định**, cần xác nhận quy trình thực tế trước khi triển khai. [Xanh SM](https://tuyentaixe.xanhsm.com/kol-koc)
- Online Vinmec hỗ trợ tìm thông tin dịch vụ và đặt lịch khám trực tuyến. [Vinmec](https://www.vinmec.com/vie/bai-viet/huong-dan-su-dung-online-vinmec-dat-lich-mua-goi)

## 2. Quick Problem Cards

### Card 1 — Vinhomes: triage yêu cầu cư dân và tạo bản nháp chuyển việc

| Mục | Nội dung |
|---|---|
| **Bài toán** | Nhân viên tiếp nhận cần đọc yêu cầu cư dân từ nhiều kênh, xác định mức khẩn, gán đúng bộ phận và ghi ticket; việc này có thể tạo vòng chuyển tiếp khi nội dung thiếu cấu trúc. |
| **Công ty thành viên** | Vinhomes |
| **Actor chịu áp lực** | Nhân viên tổng đài/Ban quản lý tiếp nhận; đội kỹ thuật, an ninh, vệ sinh và CSKH nhận ticket; cư dân chờ cập nhật. |
| **Workflow hiện tại (giả định cần xác nhận)** | 1. Cư dân gửi phản ánh qua app/điện thoại/email. → 2. Nhân viên đọc hoặc ghi nhận thông tin. → 3. Tra quy trình và gán bộ phận. → 4. Bộ phận nhận ticket hỏi lại nếu thiếu thông tin. → 5. CSKH cập nhật cư dân. |
| **Bottleneck** | Bước 2–3: diễn giải văn bản tự do và chọn nhóm xử lý. Baseline cần đo: thời gian từ tiếp nhận đến gán lần đầu, tỷ lệ chuyển sai/điều phối lại và tỷ lệ ticket thiếu trường bắt buộc. |
| **AI hỗ trợ** | LLM trích xuất loại sự vụ, địa điểm, mức khẩn và thông tin thiếu; đề xuất nhóm nhận việc và soạn **bản nháp** xác nhận. Rule engine quyết định các sự vụ khẩn theo policy. |
| **Metric pilot đề xuất** | Trong tập ticket đã gán nhãn: ≥90% đề xuất nhóm xử lý đúng top-1; giảm median thời gian triage tối thiểu 40%; tỷ lệ ticket bị chuyển lại không tăng so với baseline. |
| **Kiến trúc nhanh** | **LLM Feature + Rule router**, không phải Agent. |
| **Ranh giới ban đầu** | AI không tự gửi phản hồi, không tự đóng ticket, không hứa thời hạn xử lý và không phân loại sự cố an toàn khẩn cấp nếu thiếu dữ liệu. Nhân viên duyệt mọi đề xuất trước khi tạo/gửi ticket. |

### Card 2 — Xanh SM: copilot cho điều phối viên xử lý pin nguy cấp

| Mục | Nội dung |
|---|---|
| **Bài toán** | Điều phối viên Xanh SM nhận báo cáo pin nguy cấp từ tài xế và phải nhanh chóng lấy vị trí, kiểm tra điều kiện an toàn, tra phương án hỗ trợ rồi soạn hướng dẫn; trả lời sai có thể khiến xe đi quá xa khi pin gần cạn. |
| **Công ty thành viên** | Xanh SM (GSM) |
| **Actor chịu áp lực** | Tài xế đang gặp sự cố; điều phối viên; đội cứu hộ/kỹ thuật nếu cần escalation. |
| **Workflow hiện tại (giả định cần xác nhận)** | 1. Tài xế gọi/nhắn báo pin thấp. → 2. Điều phối viên xác minh biển số, vị trí, mức pin và tình trạng an toàn. → 3. Tra chính sách, trạm sạc và phương án hỗ trợ. → 4. Soạn hướng dẫn hoặc xin duyệt phương án cứu hộ. → 5. Gọi lại tài xế và theo dõi kết quả. |
| **Bottleneck** | Bước 2–4: tổng hợp mô tả tự do, kiểm tra nhiều nguồn và chọn thông điệp phù hợp dưới áp lực thời gian. Baseline cần đo: thời gian từ báo sự cố đến draft đầu tiên, tỷ lệ escalation và tỷ lệ hướng dẫn bị sửa. |
| **AI hỗ trợ** | LLM tóm tắt thông tin đầu vào, hỏi dữ liệu thiếu và tạo bản nháp dựa trên knowledge base đã duyệt. Rule engine chặn khuyến nghị không an toàn theo ngưỡng pin/khoảng cách. |
| **Metric pilot đề xuất** | ≥95% bản nháp có đủ biển số, vị trí, mức pin và cảnh báo cần thiết; giảm ≥30% thời gian tạo bản nháp; 100% tình huống pin dưới 5% vào đúng luồng escalation. |
| **Kiến trúc nhanh** | **Rule + LLM Feature**. |
| **Ranh giới ban đầu** | AI không điều khiển xe, không xác nhận trạm còn chỗ, không tự điều cứu hộ và không thay thế hướng dẫn khẩn cấp hoặc kỹ thuật viên. Mọi tin gửi tài xế là `[DRAFT_ONLY]` trước khi điều phối viên duyệt. |

### Card 3 — Vinmec: trợ lý tiền kiểm yêu cầu đặt lịch, trong phạm vi hành chính

| Mục | Nội dung |
|---|---|
| **Bài toán** | Khách đặt lịch trực tuyến đặt câu hỏi tự do về chuyên khoa, giấy tờ, thời gian và dịch vụ; nhân viên phải lặp lại các hướng dẫn hành chính trước khi hoàn tất lịch hẹn. |
| **Công ty thành viên** | Vinmec |
| **Actor chịu áp lực** | Khách hàng; nhân viên tổng đài/lễ tân xác minh lịch hẹn và thông tin hồ sơ. |
| **Workflow hiện tại (giả định cần xác nhận)** | 1. Khách tìm dịch vụ hoặc đặt lịch. → 2. Khách hỏi thông tin qua kênh hỗ trợ. → 3. Nhân viên xác định đây là câu hỏi hành chính hay lâm sàng. → 4. Trả lời theo nội dung đã duyệt hoặc chuyển nhân sự y tế. → 5. Xác nhận lịch. |
| **Bottleneck** | Bước 3: tách câu hỏi hành chính khỏi triệu chứng/yêu cầu tư vấn y khoa; sai ở bước này có rủi ro cao. |
| **AI hỗ trợ** | Dùng rule để nhận diện ý định được phép; LLM chỉ soạn lại hướng dẫn từ nguồn đã duyệt, hoặc hỏi thông tin còn thiếu. Nội dung lâm sàng bắt buộc chuyển nhân sự phù hợp. |
| **Metric pilot đề xuất** | ≥95% yêu cầu hành chính được định tuyến đúng; 100% câu hỏi có triệu chứng/tư vấn chẩn đoán được escalation; giảm ≥25% thời gian trả lời các câu hỏi hành chính lặp lại. |
| **Kiến trúc nhanh** | **Rule router + LLM Feature phạm vi hẹp**. |
| **Ranh giới ban đầu** | Không chẩn đoán, không đề xuất điều trị, không xếp mức độ khẩn cấp y khoa, không tự đặt/hủy lịch và không dùng dữ liệu bệnh án ngoài mục đích đã được cấp quyền. |

## 3. So sánh và lựa chọn sơ bộ cho Deep-Dive

| Tiêu chí | Card 1: Vinhomes | Card 2: Xanh SM | Card 3: Vinmec |
|---|---:|---:|---:|
| Dễ có dữ liệu pilot | Cao: ticket và trạng thái xử lý | Trung bình: cần dữ liệu sự cố đã ẩn danh | Trung bình: dữ liệu nhạy cảm, cần kiểm soát chặt |
| Rủi ro khi AI sai | Trung bình, kiểm soát bằng human review | Cao, kiểm soát bằng rule bắt buộc và human review | Cao vì liên quan chăm sóc sức khỏe |
| Giá trị thời gian thực | Cao | Cao | Trung bình |
| Phạm vi prototype an toàn | Cao: triage + bản nháp | Cao: chỉ tạo draft và escalation | Hẹp: hành chính, không lâm sàng |

**Đề xuất chọn Card 2 cho Phase 2–5:** Xanh SM xử lý pin nguy cấp nhất quán với prompt prototype của lab, có actor, handoff và ranh giới an toàn rõ. Scope prototype chỉ tạo bản nháp và đề xuất escalation; không cho phép AI điều xe hay gửi tin. Trước pilot cần xác nhận SOP, ngưỡng an toàn, quyền truy cập vị trí và baseline vận hành với bộ phận điều phối.
