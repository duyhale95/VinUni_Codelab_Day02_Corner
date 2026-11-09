```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Bác sĩ mất thời gian đọc dữ liệu lâm sàng rời rạc │
│ từ HIS/LIS/PACS để gõ tay Tóm tắt Bệnh án xuất viện.        │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ điều trị (kiệt sức vì làm ngoài giờ),   │
│ Bệnh nhân (chờ 3-5 tiếng để thanh toán ra viện).            │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Bác sĩ đi buồng, ra y lệnh cho bệnh nhân xuất viện     │
│   → 2. Lội ngược dòng HIS/LIS/PACS đọc lại 5-10 ngày bệnh án│
│   → 3. Tự gõ tay bản Tóm tắt bệnh án xuất viện theo mẫu BYT │
│   → 4. Tra cứu danh mục và gõ mã chẩn đoán ICD-10           │
│   → 5. Ký số điện tử và chuyển hồ sơ sang bộ phận bảo hiểm  │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 25 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3-4            │
│ (Trích xuất dữ liệu HIS -> Soạn nháp tóm tắt -> Gợi ý ICD)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian bác sĩ gõ hồ sơ từ 25 min ──> dưới 4 min.  │
│ - Giảm thời gian chờ xuất viện từ 210 min ──> dưới 60 min.  │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Drafting + HITL Ký số) │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: CSKH xử lý thủ công hàng ngàn khiếu nại của tài   │
│ xế về việc bị trừ điểm tín nhiệm / phạt hủy chuyến oan.     │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (bị trừ tiền/khóa app, dễ bỏ nền tảng), │
│ CSKH (quá tải duyệt hàng ngàn ticket phúc tra/ngày).        │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gửi đơn giải trình khiếu nại kèm lý do trên App │
│   → 2. CSKH đọc nội dung giải trình và phân loại lý do hủy  │
│   → 3. Tra cứu thủ công log tọa độ GPS, thời gian chờ và chat│
│   → 4. Ra quyết định: Bác bỏ hoặc Hủy phạt/Cộng lại điểm    │
│   → 5. Soạn tin nhắn phản hồi kết quả phúc tra cho tài xế   │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 8 phút/lượt)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Rule kiểm tra GPS/thời gian chờ; LLM đọc hiểu giải trình)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian xử lý khiếu nại từ 8 phút ──> dưới 2 phút. │
│ - Tự động hóa duyệt 70% các ca có bằng chứng GPS rõ ràng.   │
│                                                             │
│ Quick Architecture: [x] Rule + LLM (Hybrid Auto-Triage)     │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Phân loại ý kiến cư dân đa kênh (App, Zalo, Call) │
│ và tự động tạo phiếu điều phối (Work-order) cho kỹ thuật.   │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Điều phối viên BQL (ngợp tin nhắn, gán sai),   │
│ Cư dân (chờ sửa chữa kỹ thuật căn hộ quá lâu).              │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi phản ánh tự do: "Hành lang tòa S2 hôi và tối"│
│   → 2. Nhân viên trực tổng đài đọc tin nhắn, đoán vị trí lỗi│
│   → 3. Nhập tay tạo ticket công việc vào phần mềm quản lý   │
│   → 4. Gọi điện hoặc gán việc thủ công cho đội Kỹ thuật/Môi │
│        trường phụ trách cụm tòa đó                          │
│   → 5. Kỹ thuật đến kiểm tra (thường phát hiện gán sai đội) │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3-4 (⏱ 7-10 phút/ticket; 18% gán nhầm)
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3-4            │
│ (Đọc hiểu văn cảnh -> Trích xuất Tòa/Lỗi -> Auto-dispatch)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian tạo & điều phối từ 10 min ──> dưới 30 giây.│
│ - Giảm tỷ lệ gán nhầm bộ phận từ 18% ──> dưới 3%.           │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Text Classifier & NER) │
└─────────────────────────────────────────────────────────────┘
```