**Họ và Tên:** Nguyễn Hoàng Gia An \
**MSSV:** 2A202603799 \
**Lớp:** L3B-H209

## BỐI CẢNH:
Tôi là Nguyễn Hoàng Gia An, AI Product Engineer tại Vin Smart Future. Qua quá trình khảo sát thực địa tại Xanh SM, chúng tôi nhận thấy một điểm nghẽn vận hành nghiêm trọng: Mỗi ngày, hệ thống ghi nhận hàng chục ngàn chuyến xe bị hủy, kéo theo một khối lượng khổng lồ phản hồi phi cấu trúc từ ghi chú tài xế, lịch sử chat CSKH cho đến ghi âm tổng đài. Hiện tại, đội ngũ vận hành đang phải đọc thủ công từng ticket, bóc tách dữ liệu vào các file Excel rời rạc để làm báo cáo hồi cứu. Quy trình thủ công này không chỉ tiêu tốn hàng trăm giờ công mỗi ngày mà còn tạo ra độ trễ thông tin từ 3 đến 7 ngày, khiến chúng ta hoàn toàn bị động và không thể mở rộng quy mô giám sát khi số lượng cuốc xe tăng trưởng nóng.

## BÁO CÁO SCANNING VẬN HÀNH TOÀN DIỆN (PHASE 1)
**Đơn vị thực hiện:** Vin Smart Future — AI Product Strategy Group  
**Mục tiêu:** Quét lọc toàn bộ các điểm nghẽn vận hành trên 5 công ty thành viên Vingroup thông qua **4 Lăng kính (4 Lenses)**, loại bỏ tư duy "thấy AI là lao vào", chỉ giữ lại các bài toán có tác động kinh doanh đo đếm được.

---

## 1. BẢNG TỔNG HỢP SCAN MATRIX (SLIDE TỔNG QUAN)

| # | Đơn vị thành viên | Lăng kính chính | Điểm nghẽn vận hành thực địa | Bản chất kỹ thuật (Reality Check) |
|---|---|---|---|---|
| **01** | **Vinmec** | **Tốn thời gian** | Bác sĩ mất 25–40 phút/ca đọc dữ liệu rời rạc để gõ Tóm tắt Bệnh án xuất viện; bệnh nhân chờ 3–5 tiếng. | **CẦN LLM (Feature + HITL):** Dữ liệu văn bản tự do, thuật ngữ y khoa phức tạp, không thể viết rule. |
| **02** | **Xanh SM** | **Pain từ tài xế và công ty** | CSKH ngập trong 3.000+ khiếu nại tài xế/ngày về trừ điểm và phạt hủy chuyến; tỷ lệ tài xế ức chế rời bỏ nền tảng tăng cao. | **HYBRID:** 70% dùng Rule-based đối chiếu GPS/Event logs; 30% dùng Small LLM phân loại nội dung giải trình. |
| **03** | **Vinhomes** | **Nâng cấp hệ thống** | Chatbot cư dân dạng kịch bản (cây thư mục) xử lý kém; điều phối viên tốn 5–7 phút đọc ticket mô tả tự do để gán đúng đội kỹ thuật. | **CẦN LLM:** Trích xuất thực thể (phòng, lỗi, mức khẩn cấp) & Tự động định tuyến (Zero-shot Routing). |
| **04** | **VinFast** | **Lặp đi lặp lại** | Kỹ sư xưởng ngập trong hàng triệu log cảnh báo lỗi xe (DTC); 80% là lỗi thoáng qua do sụt áp ắc-quy 12V nhưng vẫn tốn công kiểm tra. | **TUYỆT ĐỐI KHÔNG DÙNG LLM:** Dữ liệu dạng bảng/time-series; giải bằng **Rule Engine + Machine Learning cổ điển**. |
| **05** | **Vinpearl** | **Pain từ khách du lịch** | Khách du lịch tại siêu quần thể (Phú Quốc/Nha Trang) bị quá tải thông tin; CSKH quá tải giải quyết dồn ứ giờ cao điểm tại VinWonders/Safari. | **CẦN LLM AGENT CÓ KIỂM SOÁT:** Phân tích ngữ cảnh khách, gợi ý lộ trình thời gian thực theo thời tiết & hàng chờ. |
