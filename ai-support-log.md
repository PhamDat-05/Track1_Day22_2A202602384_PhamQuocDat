# AI Support Log & Critique Notes — Day 22

**Học viên:** Phạm Quốc Đạt · **MHV:** 2A202602384  
**Sản phẩm:** Agent luyện tập tư vấn cho sales xe VinFast mới (AI Sales Training & Simulation Agent)  
**Quy tắc:** Thực thi ít nhất 2 prompt phản biện theo yêu cầu tại §4.7 của bài lab và ghi nhận phản hồi (Accept / Reject / Partial).

---

## 1. Prompt 1 — Cost/Job Stress Test (§4.7.1)

### Ngữ cảnh đưa vào phản biện (Tab 1 & Tab 2)
- Job: 1 ca mô phỏng tình huống tư vấn xe/chính sách được sales hoàn thành và nhận báo cáo đánh giá hợp lệ.
- Volume: 1,000 ca thử / tháng; Containment rate: 82% → 820 ca hoàn thành; 180 ca escalate sang người.
- Model: Claude Haiku 4.5 ($1.00 / $5.00 per 1M token; Cache write $1.25, Cache read $0.10).
- Token/lượt: 3,000 cache + 1,000 fresh input, 300 output × 6 lượt → $0.02025/job.
- Infra: $0.005/job; Retry: 8% → $0.00162/job; HITL Biến thể B: QA 5% ($15/tháng) + Escalation 180 ca ($162/tháng).
- Tổng chi phí: $203.87/tháng → Cost/Job (mẫu số 820): **$0.2486/job**.
- Giá đề xuất: $0.99/job → Gross Margin: **74.89%**.

### Phản biện từ AI (Đóng vai Ruthless CFO & Skeptical Infra Engineer)
1. **Missing Costs / Chi phí có nguy cơ thiếu:**
   - *Phản biện:* Vector DB và Embedding cho chính sách xe VinFast có tăng theo thời gian không? Khi VinFast ra mắt thêm dòng xe (VF3, VF6, VF7) và cập nhật chính sách pin hàng tháng, kích thước vector store và chi phí re-indexing sẽ tăng.
   - *Quyết định:* **Accept (Chấp nhận).** Giữ chi phí Infra ở mức $0.005/ca (đủ cover serverless vector DB Pinecone/Qdrant + text-embedding-3-small ở quy mô 1,000 - 10,000 ca/tháng).
2. **Denominator Check / Kiểm tra mẫu số:**
   - *Phản biện:* Xác nhận mẫu số là 820 ca hoàn thành (completed jobs), không phải 1,000 ca thử. Nếu chia cho 1,000 ca, Cost/Job sẽ bị làm đẹp giả tạo thành $0.2039 (-18%).
   - *Quyết định:* **Accept.** Giữ nguyên mẫu số B11 = 820.
3. **HITL Reality / Ai chịu chi phí escalation:**
   - *Phản biện:* Nếu chọn Biến thể B, 180 ca escalate tốn $162/tháng, chiếm tới 79.5% tổng chi phí. Nếu containment tụt xuống 60%, chi phí HITL sẽ tăng vọt lên $360, đẩy Cost/Job lên $0.67 và GM xuống 32.3%.
   - *Quyết định:* **Accept.** Ghi rõ trong Decision Note và One-Pager: Mô hình phụ thuộc sống còn vào containment rate (ngưỡng breakeven là 72.68%). Cần cơ chế đặt trần escalation tối đa 15-18% bằng cách yêu cầu sales xem lại tài liệu gợi ý trước khi gửi yêu cầu can thiệp tới quản lý.
4. **Token Math & Batch Pricing:**
   - *Phản biện:* Prompt caching tiết kiệm 38.64% là thực tế với system prompt cố định. Không thể dùng Batch API vì đây là tương tác realtime giữa sales và khách ảo.
   - *Quyết định:* **Accept.** Đặt Batch API = 0.

---

## 2. Prompt 2 — Channel Reality Check (§4.7.3)

### Ngữ cảnh đưa vào phản biện (Tab 4)
- ARPU: $300/tháng/đại lý (tương đương ~300 ca hoàn thành) · Gross Margin: 74.89%.
- Phân khúc: SMB (Đại lý / Showroom ô tô).
- Ngân sách CAC cho phép (Payback 12 tháng): $300 × 74.89% × 12 = **$2,695.92/khách**.
- Kênh đề xuất: Partner-Led (VinFast Dealer Network & Nền tảng LMS Base.vn).
- Pain Moment: 20h00 – 21h30 tối, sales mới ôn luyện kịch bản tư vấn xe điện trước ngày thi sát hạch trên VinFast Sales Portal / LMS.

### Phản biện từ AI (Đóng vai Skeptical GTM Advisor)
1. **Direct Sales Viability / Tính khả thi của Sales-Led:**
   - *Phản biện:* Với ACV chỉ $3,600/năm ($300/tháng), quota $250k/năm đòi hỏi 1 AE phải chốt 69.4 deal/năm ≈ 0.28 deal/ngày làm việc. Dù số học tưởng như có thể (dưới 1 deal/ngày), nhưng chu kỳ bán hàng B2B cho đại lý ô tô (qua Giám đốc đại lý và thẩm định phần mềm) thường kéo dài 4-8 tuần.
   - *Đối chiếu:* Benchmark ICONIQ 2026 cho thấy Cost per Opportunity cho SMB là $6,300; với win rate 25%, CAC thực tế của Direct Sales lên tới **$25,200/khách** — gấp **9.35 lần** ngân sách CAC ($2,695.92). Direct Sales chắc chắn lỗ nặng.
   - *Quyết định:* **Accept.** Khẳng định bằng chứng toán học chứng minh Direct Sales hoàn toàn bất khả thi. Chốt 100% tập trung vào Partner-Led.
2. **Partner Incentive Risk / Động lực của đối tác:**
   - *Phản biện:* Tại sao Ban quản lý đại lý VinFast hoặc nền tảng LMS Base.vn lại chịu hợp tác và phân phối giải pháp cho bạn? Nếu bạn không có cam kết rõ ràng, kênh này chỉ là ảo tưởng.
   - *Quyết định:* **Accept & Điều chỉnh.** Ghi rõ cơ chế hợp tác win-win trong One-Pager:
     - Đối với Đại lý VinFast: Giải quyết nỗi đau thiếu người kèm cặp sales mới, chuẩn hóa kiến thức tư vấn xe điện, giảm tỷ lệ trượt sát hạch và giảm rủi ro mất khách.
     - Đối với nền tảng LMS / Đối tác: Được chia sẻ 20-30% doanh thu subscription định kỳ (Rev-share) và làm phong phú hệ sinh thái tính năng đào tạo AI thực chiến mà không tốn công phát triển từ đầu.
3. **Pain Moment Specificity:**
   - *Phản biện:* Pain moment 3 phần đã đủ cụ thể (20h tối + ôn kịch bản tư vấn xe trước sát hạch + app VinFast Sales Portal/LMS). Cần đảm bảo điểm nhúng là widget/SDK trực tiếp trong app, không bắt sales chuyển tab.
   - *Quyết định:* **Accept.** Điểm nhúng được chốt là Tab "Luyện tập tình huống" trên cổng thông tin sales nội bộ.

---

## 3. Tổng kết quyết định của học viên

- **Đã phê duyệt:**
  1. Giữ nguyên mô hình Cost/Job đầy đủ 5 thành phần, Biến thể B có tính escalation vào COGS để bảo đảm GM trung thực ở mức **74.89%** (không ảo tưởng > 85%).
  2. Chọn Value Metric **Outcome** ($0.99/ca hoàn thành) nhờ Attribution 9/10 và Autonomy 9/10.
  3. Chọn duy nhất 1 kênh GTM: **Partner-Led** với bằng chứng chênh lệch CAC thực tế gấp 9.35 lần ngân sách CAC.
  4. Xác lập kế hoạch 90 ngày theo 3 giai đoạn: Học (Tháng 1) → Đòn bẩy (Tháng 2-3) → Mở rộng (Tháng 4+).

