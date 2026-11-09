# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Điều phối viên phải thủ công tra cứu và phân bổ lại cuốc xe khi tài xế báo sự cố pin giữa đường (mất 15 phút/lượt, xảy ra ~80 lần/ngày tại Hà Nội). |
| 2 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20–30 phút/bệnh nhân để soạn thảo tóm tắt hồ sơ xuất viện thủ công, dẫn đến quá tải và chậm trễ trong quy trình khám chữa bệnh. |
| 3 | **Vinhomes** | AI-upgrade | Hệ thống CSKH phân loại và phản hồi các khiếu nại của cư dân Vinhomes Resident App hoàn toàn thủ công, phản hồi rập khuôn, mất 12 tiếng/ticket. |
| 4 | **VinFast** | Lặp lại | Nhân viên đối soát hóa đơn sạc điện đối tác hằng tuần hoàn toàn thủ công qua Excel, dễ sai sót và mất 2–3 ngày/chu kỳ. |
| 5 | **Vinpearl** | Stakeholder Pain | Khách du lịch phàn nàn chatbot đặt vé / hỏi thông tin tại Vinpearl trả lời chậm, không hiểu tiếng địa phương và không gợi ý được combo phù hợp theo ngân sách. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

> **Nhóm đã chọn top 3:** #1 (Xanh SM – Sự cố pin), #2 (Vinmec – Tóm tắt xuất viện), #3 (Vinhomes – CSKH).
> Bài toán được chọn để Deep-Dive: **Card #1 — Xanh SM Xử lý sự cố sạc pin thực địa**.

---

## Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo hết pin giữa đường cần điều   │
│ phối cứu hộ hoặc chỉ dẫn trạm sạc gần nhất khẩn cấp.       │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (chờ đợi), Điều phối viên (quá tải)    │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận báo hết pin               │
│   → 2. Dispatcher tra cứu vị trí GPS xe trên bản đồ         │
│   → 3. Tra cứu trụ sạc VinFast còn trống gần nhất (5 phút) │
│   → 4. Viết tin nhắn chỉ dẫn gửi qua App tài xế (5 phút)   │
│   → 5. Gọi xe cứu hộ nếu pin < 5% (1 phút)                 │
│                                                             │
│ Bước nào tốn nhất? Bước 3–4 (⏱ 10 phút/lượt)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3–4             │
│ (Auto-pull vị trí → Tra trạm trống → Draft tin chỉ dẫn)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm thời gian xử lý từ 15 phút ──> dưới 3 phút.          │
│   Tỉ lệ chỉ dẫn đúng trạm/đúng loại cổng sạc đạt 98%.      │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

---

## Card #2 — Vinmec: Tóm tắt hồ sơ xuất viện bằng AI

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Bác sĩ Vinmec phải tự tay tóm tắt toàn bộ hồ sơ  │
│ bệnh án và viết báo cáo xuất viện cho từng bệnh nhân.       │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ điều trị (quá tải hành chính)          │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ đọc lại toàn bộ hồ sơ bệnh án điện tử          │
│   → 2. Thủ công soạn tóm tắt lý do nhập viện, diễn biến    │
│   → 3. Nhập kết quả xét nghiệm, thuốc điều trị vào mẫu     │
│   → 4. Ký xác nhận và lưu hồ sơ xuất viện (20–30 phút)     │
│                                                             │
│ Bước nào tốn nhất? Bước 2–3 (⏱ 20 phút/bệnh nhân)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3             │
│ (AI đọc EMR → Draft bản tóm tắt → Bác sĩ review & ký)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm thời gian soạn tóm tắt từ 20 phút ──> dưới 5 phút.  │
│   Độ chính xác so với bác sĩ review đạt ≥ 90%.             │
│                                                             │
│ Quick Architecture: [x] LLM Feature (RAG trên EMR)          │
└─────────────────────────────────────────────────────────────┘
```

---

## Card #3 — Vinhomes: Phân loại & phản hồi khiếu nại cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: CSKH Vinhomes xử lý thủ công hàng trăm khiếu nại │
│ cư dân/ngày qua App, phản hồi rập khuôn, chậm 12 tiếng.    │
│ Công ty thành viên: [x] Vinhomes                           │
│                                                             │
│ Ai đang đau? Nhân viên CSKH (quá tải), Cư dân (chờ lâu)   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhân viên đọc ticket khiếu nại từ App                  │
│   → 2. Phân loại theo danh mục (phí, kỹ thuật, an ninh...) │
│   → 3. Soạn thảo phản hồi và route sang bộ phận xử lý      │
│   → 4. Theo dõi, đóng ticket sau khi giải quyết            │
│                                                             │
│ Bước nào tốn nhất? Bước 2–3 (⏱ 8 phút/ticket)             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3             │
│ (AI phân loại → Draft phản hồi → Nhân viên duyệt & gửi)   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   Giảm thời gian phản hồi từ 12 tiếng ──> dưới 2 tiếng.    │
│   Tỉ lệ phân loại đúng danh mục đạt ≥ 92%.                 │
│                                                             │
│ Quick Architecture: [x] LLM Feature + Rule-based Router     │
└─────────────────────────────────────────────────────────────┘
```

---

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---
