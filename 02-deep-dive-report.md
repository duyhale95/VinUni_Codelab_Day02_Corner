# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## Bài toán được chọn: Xanh SM — Xử lý sự cố sạc pin thực địa

---

## 3.1. Current-State Workflow Mapping (25 min)

**Quy trình xử lý sự cố hết pin thực địa của điều phối viên Xanh SM:**

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe   │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: Điện thoại│     │ In: Biển số  │     │ In: Vị trí GPS│     │ In: Raw data │
│ Out: Log sự cố│     │ Out: Tọa độ  │     │ Out: Địa chỉ │     │ Out: SMS     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Gọi xe cứu   │
                                                               │ hộ (nếu cần) │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ 1 phút     │
                                                               └──────────────┘
🔴 = Bottlenecks (Bước 3 & 4 chiếm 10/15 phút tổng quy trình)
🔄 = Handoff: Điện thoại → Log nội bộ → Dashboard trạm sạc → App tài xế
⏱ Tổng thời gian xử lý thủ công: 15 phút/lượt.
```

---

## 3.2. Problem Statement (6-field) & Metrics (15 min)

Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM, làm việc 24/7, phụ trách quản lý đội xe điện VF5/VFe34/VF8 tại Hà Nội và TP. Hồ Chí Minh. |
| **2. Current Workflow** | Khi tài xế báo hết pin qua điện thoại, dispatcher phải: (1) ghi log sự cố, (2) tra cứu GPS xe trên bản đồ nội bộ, (3) mở Dashboard trạm sạc VinFast để tìm trụ còn trống phù hợp với loại xe, (4) soạn tin nhắn chỉ dẫn gửi qua App tài xế, (5) gọi đội cứu hộ nếu pin < 5%. Toàn bộ 5 bước thủ công, mất trung bình 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & 4 (mất 10 phút): Tra cứu thủ công trụ sạc trống phù hợp với dòng xe (cổng CCS2 cho VF8, Type 2 cho VFe34) và soạn thảo tin nhắn hướng dẫn đường đi chi tiết bằng tiếng Việt thân thiện. Dispatcher phải đồng thời xử lý nhiều ca sự cố, dẫn đến sai sót và delay. |
| **4. Business Impact** | ~80 sự cố pin/ngày tại Hà Nội → lãng phí **20 giờ làm việc/ngày** của team điều vận. Tài xế chờ trung bình 15 phút/sự cố → xe không đón khách → rò rỉ doanh thu ước tính **15%** trong giờ sự cố. Tỉ lệ khách hủy chuyến tăng cao do tài xế phải hủy cuốc đang chạy. |
| **5. Success Metric** | 1. Giảm tổng thời gian xử lý sự cố từ **15 phút → dưới 3 phút** (efficiency gain ≥ 80%). 2. Tỉ lệ chỉ dẫn đúng địa điểm và đúng loại cổng sạc phù hợp đạt **≥ 98%** (quality). 3. Giảm thời gian dispatcher bị chiếm dụng/sự cố xuống **< 1 phút** (human review only). |
| **6. Operational Boundary** | **AI được phép:** Truy xuất API định vị xe, API trạm sạc VinFast còn trống, tự động soạn thảo tin nhắn hướng dẫn dạng **nháp [DRAFT_ONLY]**. **TUYỆT ĐỐI CẤM:** AI không được tự động gửi tin mà không có dispatcher phê duyệt (bắt buộc HITL); không được đề xuất trạm sạc không phù hợp loại cổng sạc; không được chỉ dẫn trạm sạc cách xa > 5km khi pin < 5% — phải trigger dispatch xe sạc lưu động. |

---

## 3.3. Future-State Flow & AI Fit (25 min)

* **AI Fit:** ✅ **LLM Feature** (không cần Agentic Loop vì quy trình có cấu trúc cố định, rủi ro điều phối sai có thể khiến xe cạn pin giữa đường).
* **So sánh AI-Fit Matrix:**

| Loại | Phù hợp không? | Lý do |
|---|---|---|
| Rule / State-Machine | ❌ Không đủ | Không xử lý được ngôn ngữ tự nhiên từ tài xế, không soạn tin nhắn linh hoạt. |
| **LLM Feature** | ✅ **Chọn** | Phù hợp nhất: Hiểu context, soạn tin nhắn tiếng Việt tự nhiên, xử lý exception (pin < 5%). |
| Agentic Loop | ❌ Overkill | Quy trình đủ đơn giản, không cần AI tự chủ điều hành — tăng rủi ro không kiểm soát. |

* **Quy trình tương lai (Future-State Flow):**

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Auto-pull │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ gọi sự cố    │ ──→ │ vị trí &     │ ──→ │ SMS chỉ dẫn  │ ──→ │ click duyệt  │
│              │     │ trạm sạc trống│    │ & chỉ đường  │     │ & gửi tài xế │
│ ⏱ 2 phút     │     │ ⏱ <1 phút    │     │ ⏱ <1 phút    │     │ ⏱ 1 phút     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI draft lỗi hoặc
                                                               pin < 5% → dispatch
                                                               xe sạc lưu động tự động.
                                                               Dispatcher tự viết
                                                               tay lại như quy trình cũ.
```

**🔵 AI Step:** Tự động truy xuất GPS + API trạm sạc → Soạn thảo tin nhắn hướng dẫn.
**🟢 Human Step (HITL):** Dispatcher review bản draft 1-click → Phê duyệt & gửi.
**↩️ Fallback:** Pin < 5% → AI tự động trigger `dispatch_mobile_charger` JSON. Nếu AI lỗi → Dispatcher xử lý thủ công như cũ.

⏱ **Tổng thời gian tương lai: ~3 phút/lượt** (giảm 80% so với 15 phút hiện tại).

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] **Dữ liệu sẵn sàng:** Xanh SM có sẵn API GPS xe, Dashboard trạm sạc VinFast với dữ liệu real-time về trụ còn trống và loại cổng sạc. Log sự cố pin từ 6 tháng qua (~14,400 records) đủ để test và fine-tune.
2. [x] **Rủi ro kiểm soát được:** Mọi output AI đều là bản nháp [DRAFT_ONLY], dispatcher phải click phê duyệt mới gửi → Human-in-the-loop bảo vệ toàn bộ. Fallback về quy trình thủ công nếu AI fail.
3. [x] **Stakeholders sẵn sàng:** Team điều vận Xanh SM đã phàn nàn về khối lượng công việc thủ công — đây là pain point thực sự, không phải nhu cầu tạo ra. Dispatcher chỉ cần học thao tác "1-click approve", không thay đổi lớn.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Dự án đạt **GO** vì đủ 3 tiêu chí quan trọng:
>
> 1. **Bài toán cụ thể, metric rõ ràng:** 80 sự cố/ngày × 15 phút = 20 giờ lãng phí/ngày. Mục tiêu giảm xuống 3 phút có thể đo lường trực tiếp trên hệ thống log hiện có.
>
> 2. **Giải pháp công nghệ đơn giản và hiệu quả (LLM Feature):** Không cần xây dựng Agent phức tạp. Chỉ cần 1 LLM call với System Prompt nghiêm ngặt + API tra cứu trạm sạc → kết quả ngay. Chi phí phát triển thấp, có thể ra mắt MVP trong 2-3 tuần.
>
> 3. **Ranh giới an toàn được kiểm soát chặt chẽ:** Toàn bộ output là [DRAFT_ONLY], dispatcher phải approve. Fallback về quy trình thủ công 100% nếu AI sai. Rule cứng: pin < 5% → xe sạc lưu động (không AI tự quyết). Đã được kiểm tra qua adversarial prompt testing trong Phase 4.
>
> **Rủi ro duy nhất:** Độ chính xác của API trạm sạc VinFast (trụ có thể báo "trống" nhưng thực tế bị hỏng). **Giảm thiểu:** Thêm bước dispatcher xác nhận tình trạng thực tế trước khi approve gửi.

---
