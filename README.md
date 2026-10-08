# Track 1 · Day 22 — AI Monetization: Pricing · GTM · Evidence

- **Học viên:** Phạm Quốc Đạt
- **Mã học viên (MHV):** `2A202602384`
- **Khóa học:** `K04-L34-P2-T1`
- **Tên dự án:** Agent luyện tập tư vấn cho sales xe VinFast mới (AI Sales Training & Simulation Agent)
- **Tên Repository:** `Track1_Day22_2A202602384_PhamQuocDat`

---

## 1. Tóm tắt các con số cốt lõi (Executive Summary)

Mọi con số dưới đây được tính toán từ mô hình tài chính và đồng bộ 100% giữa file Excel và One-Pager:

| Chỉ số cốt lõi | Giá trị | Đơn vị | Ý nghĩa / Ghi chú |
| :--- | :---: | :---: | :--- |
| **Định nghĩa 1 Job** | — | — | 1 ca mô phỏng tư vấn xe VinFast được sales hoàn thành và nhận báo cáo đánh giá hợp lệ |
| **Cost/Job** (thuần COGS) | **$0.25** | $/job | Tương đương ~6,464 ₫/ca (Đủ 5 thành phần: LLM có cache, Infra, Retry 8%, HITL Biến thể B). Mẫu số là **820 ca hoàn thành**. |
| **Giá sàn** (3 × Cost/Job) | **$0.75** | $/job | Tương đương ~19,393 ₫/ca (Hệ số an toàn tối thiểu 3.0×) |
| **Giá bán đề xuất** | **$0.99** | $/job | Tương đương ~25,740 ₫/ca (Bội số **3.98×** Cost/Job, tham chiếu Intercom Fin $0.99) |
| **Gross Margin (GM)** | **74.89%** | % | Nằm trong dải an toàn (≥ 60%), không vượt quá 85% (tránh bẫy nghi ngờ quên chi phí) |
| **Breakeven Containment** | **72.68%** | % | Ngưỡng containment tối thiểu để GM ≥ 60% (Thực tế đạt **82.00%**, an toàn) |
| **Value Metric đã chọn** | **Outcome** | — | Chấm theo ca hoàn thành (Attribution: **9/10**, Autonomy: **9/10**) |
| **Ngân sách CAC / khách** | **$2,695.92** | $/đại lý | ARPU $300/tháng × GM 74.89% × Payback 12 tháng (Phân khúc SMB) |
| **CAC thực tế Direct Sales** | **$25,200.00** | $/đại lý | Cost per opp $6,300 / Win rate 25% → Lệch **9.35 lần** ngân sách CAC |
| **Kênh GTM 90 ngày đầu** | **Partner-Led** | — | Cắm vào chuỗi đại lý ủy quyền VinFast & Nền tảng LMS đào tạo Base.vn |

---

## 2. Danh mục tài liệu nộp bài (Deliverables)

Theo đúng quy chuẩn đặt tên file tại Mục 6.1: `[Tên]_Day22_model.xlsx` và `[Tên]_Day22_onepager.pdf`:

```text
Track1_Day22_2A202602384_PhamQuocDat/
├── PhamQuocDat_Day22_model.xlsx       # [Artefact 1] File Excel 5 tab tính toán hoàn chỉnh
├── PhamQuocDat_Day22_onepager.pdf     # [Artefact 2] Monetization One-Pager định dạng PDF
├── PhamQuocDat_Day22_onepager.docx    # Monetization One-Pager định dạng Word
├── README.md                          # Tài liệu tổng quan dự án & đối chiếu rubric
└── ai-support-log.md                  # Nhật ký thực thi 2 prompt phản biện (§4.7.1 & §4.7.3)
```

- [Mô hình Excel chính](PhamQuocDat_Day22_model.xlsx)
- [Monetization One-Pager (PDF)](PhamQuocDat_Day22_onepager.pdf)
- [Monetization One-Pager (Word)](PhamQuocDat_Day22_onepager.docx)
- [Nhật ký phản biện AI Support Log](ai-support-log.md)

---

## 3. Cấu trúc mô hình tài chính (Excel 5 Tabs)

1. **Tab `1_Cost_Job`:**
   - S1: Job định nghĩa theo giá trị khách hàng coi trọng; chọn Biến thể B (chịu chi phí escalation vào COGS).
   - S2: 1,000 ca thử / tháng; containment 82% → 820 ca hoàn thành (mẫu số chuẩn), 180 ca escalate.
   - S3: Claude Haiku 4.5 ($1.00/$5.00/1M token, cache write $1.25, cache read $0.10). Tiết kiệm **38.64%** nhờ prompt caching.
   - S5: Infra $0.005/ca (Pinecone vector DB, embedding, telemetry logging).
   - S6: Retry 8% ($0.00162/ca) — không để 0%.
   - S7: HITL gồm QA nội bộ 5% ($15/tháng) và Escalation 180 ca ($162/tháng), lương fully-loaded $9/h.
   - Kết quả: Cost/Job = **$0.2486/job** (~6,464 ₫/job).

2. **Tab `2_Pricing`:**
   - Giá sàn: 3 × $0.2486 = **$0.7459/job**.
   - Neo giá trần: Neo theo giá trị tiết kiệm ($0.40 - $1.00/job) và nhân công thay thế ($0.75 - $1.05/job).
   - Giá bán đề xuất: **$0.99/job** → Bội số **3.98×** (Đạt ≥ 3×), Gross Margin **74.89%** (An toàn ≥ 60%, không > 85%).
   - Breakeven Containment: **72.68%** (nhỏ hơn containment thực tế 82.00% → Đạt có lãi lành mạnh).

3. **Tab `3_Value_Metric`:**
   - Attribution: **9/10** (Trace log attempt_id, eval đối chiếu barem 84.5%, định nghĩa thanh toán chặt).
   - Autonomy: **9/10** (AI chạy độc lập 24/7, sales tự học buổi tối, containment 82%).
   - Model gợi ý và chốt: **Outcome** ($0.99/ca hoàn thành).
   - Benchmark: Intercom Fin ($0.99/res) và Zendesk AI ($1.50/res).

4. **Tab `4_Channel_Fit`:**
   - Ngân sách CAC: **$2,695.92/đại lý** (ARPU $300/tháng, GM 74.89%, payback 12 tháng SMB).
   - Direct Sales Test: Cost per opp $6,300, win rate 25% → CAC thực tế **$25,200/đại lý** (gấp 9.35 lần ngân sách CAC) → Bất khả thi.
   - Chấm điểm kênh: Partner-Led đạt **27/30 điểm** (vượt trội so với PLG 14 và Sales-Led 13).
   - Chốt kênh: **Partner-Led** qua mạng lưới đại lý VinFast & nền tảng LMS Base.vn.

5. **Tab `5_90Day_Plan`:**
   - Pain Moment: 20h00 - 21h30 tối + Sales ôn luyện tình huống tư vấn xe VF trước sát hạch + App VinFast Sales Portal / LMS.
   - Điểm nhúng: Tab "Luyện tập tình huống" nhúng SDK/iframe trực tiếp trong portal đại lý.
   - 90-Day Plan 3 giai đoạn: Tháng 1 (Học, 2 đại lý, 300 ca) → Tháng 2-3 (Đòn bẩy, 10 đại lý, 1,500 ca) → Tháng 4+ (Mở rộng, 35 đại lý, 5,000 ca).
   - Evidence Pack: Đầy đủ 3 tài sản (Eval Results, Risk Checklist, Pilot Report) kèm số liệu và deadline cụ thể.
   - Bài test người lạ: Trả lời 3/3 câu hỏi, số câu hỏi lại: 2 câu (đạt mục tiêu ≤ 3).

6. **Tab `6_Benchmarks`:**
   - Ghi rõ ngày cập nhật giá API: **08/10/2026**.

---

## 4. Final Checklist — Đối chiếu 10 Tiêu chuẩn (§5.8)

| # | Tiêu chí checklist | Trạng thái | Minh chứng |
| :---: | :--- | :---: | :--- |
| 1 | Tab 1 — đủ 5 thành phần chi phí, không ô trống | ✅ ĐẠT | LLM (có cache), Speech (0 có lý do), Infra ($0.005), Retry (8%), HITL ($177/tháng) |
| 2 | Tab 1 — mẫu số là JOB HOÀN THÀNH | ✅ ĐẠT | Mẫu số là B11 = 820 ca hoàn thành (không chia cho 1,000) |
| 3 | Tab 2 — Giá bán ≥ 3 × Cost/Job, GM ≥ 60% | ✅ ĐẠT | Bội số 3.98× (≥ 3×), Gross Margin 74.89% (≥ 60%) |
| 4 | Tab 2 — Breakeven containment đã tính & so với eval | ✅ ĐẠT | Breakeven = 72.68% < 82.00% (Eval thực tế) |
| 5 | Tab 3 — Value Metric + Decision Note + 2 benchmark | ✅ ĐẠT | Outcome, 3 câu Decision Note, Intercom Fin & Zendesk AI có URL |
| 6 | Tab 4 — Ngân sách CAC, deal/AE/ngày, 1 kênh duy nhất | ✅ ĐẠT | Ngân sách CAC $2,695.92, 0.28 deal/ngày, chốt duy nhất Partner-Led |
| 7 | Tab 5 — 90-day plan có số, Evidence Pack có deadline | ✅ ĐẠT | Kế hoạch 3 giai đoạn đầy đủ số/người; Evidence pack có ngày & deadline |
| 8 | Ghi ngày kiểm tra giá API ở tab 6_Benchmarks | ✅ ĐẠT | Ghi rõ ngày kiểm tra: 08/10/2026 tại ô B3 |
| 9 | One-Pager — 3 khối, mọi số khớp Excel | ✅ ĐẠT | Đồng bộ 100% giữa file Word/PDF và Excel Model |
| 10 | Đã chạy ít nhất 2 prompt phản biện (§4.7) | ✅ ĐẠT | Ghi lại chi tiết trong tệp `ai-support-log.md` (Prompt 4.7.1 & 4.7.3) |

---

## 5. Tự đánh giá Rubric (100 Điểm)

- **Tiêu chí 1: Cost/Job Rigor (30/30):** Mô hình chi phí chặt chẽ, phân biệt đúng Biến thể B, tính đủ 5 thành phần, mẫu số chuẩn, kiểm thử nhạy cảm gãy khi containment < 72.68%.
- **Tiêu chí 2: Value Metric Justification (25/25):** Sử dụng ma trận Attribution (9/10) × Autonomy (9/10), có 2 benchmark quốc tế kèm link, giải trình chi tiết tình huống chịu lỗ.
- **Tiêu chí 3: Channel Evidence (20/20):** Chứng minh bằng số sự bất khả thi của Sales-Led (lệch 9.35 lần CAC), scorecard 6 tiêu chí chọn Partner-Led có tên đối tác cụ thể và động lực hợp tác.
- **Tiêu chí 4: Pain Moment & 90-Day Plan (15/15):** Pain moment chuẩn 3 phần (giờ + việc + app), điểm nhúng SDK cụ thể, kế hoạch 3 giai đoạn có KPI lượng hóa và người chịu trách nhiệm.
- **Tiêu chí 5: Evidence Pack Readiness (10/10):** Đầy đủ 3 tài sản bán hàng với số liệu eval 84.5%, giải trình 3 câu hỏi rủi ro cốt lõi và deadline cụ thể cho báo cáo pilot.

**Tổng điểm tự đánh giá:** **100/100 (Grade Band: Outstanding).**
