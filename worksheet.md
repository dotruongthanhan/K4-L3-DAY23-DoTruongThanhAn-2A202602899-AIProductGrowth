# Worksheet — SteelLearn (AI SOP Tutor Agent)

Họ tên: Đỗ Trương Thành Ân · MSSV: 2A202602899 · Ngày làm: 09/10/2026

---

## Trạm 1 — Loại mô hình

**Câu chốt loại:** Chúng tôi là **B2B** (B2B Industrial Tech / Enterprise SaaS) vì tiền đến từ Ban Giám đốc & Phòng Kỹ thuật - An toàn Lao động (EHS) của các nhà máy luyện kim cán thép (doanh nghiệp B2B), người dùng thật là công nhân mới vào ca và quản đốc phân xưởng nội bộ của chính doanh nghiệp đó, và chúng tôi giải quyết bài toán giảm thiểu tai nạn lao động, số hóa quy trình chuẩn và rút ngắn 80% thời gian kèm cặp onboarding trực tiếp tại hiện trường xưởng.

**Bảng đèn §3 của B2B** (đánh giá hiện trạng đo lường của dự án SteelLearn):

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| **Time-to-first-value (TTFV)** ⭐ | ✅ | Đo được ngay từ bảng `worker_lesson_progress` trong CSDL PostgreSQL: thời gian từ ngày bàn giao tài khoản đến ca học đầu tiên có công nhân đạt điểm quiz an toàn ≥ 80. |
| **Pipeline coverage** | ✅ | Đo được trong CRM quản lý bán hàng (HubSpot/Notion): Tổng giá trị cơ hội B2B đã qua bước khảo sát hiện trường xưởng ÷ Chỉ tiêu doanh thu quý (Target 540 triệu VNĐ/quý ~ 3 nhà máy). |
| **% deal chết ở khâu security/procurement** | ✅ | Đo được từ biên bản thương thảo hợp đồng: tỷ lệ deal dừng do vướng quy định an toàn dữ liệu công nghệ sản xuất, bí mật quy trình SOP hoặc yêu cầu triển khai On-Premise/Private Cloud. |
| **POC → paid conversion rate** | 🔧 | Đo được trong 2 tuần: tỷ lệ các nhà máy chạy thử nghiệm Pilot 1 phân xưởng chuyển đổi thành hợp đồng thương mại thường niên chính thức (ACV trả tiền). Benchmark ngành B2B AI: **~50%** theo [ICONIQ State of Go-to-Market 2026](https://www.iconiq.com/growth/reports/state-of-go-to-market-2026) (kiểm tra 27/08/2026). |
| **Sales cycle (tuần)** | ✅ | Đo được trong CRM: số tuần tính từ buổi demo đầu tiên với Quản đốc phân xưởng đến ngày ký kết hợp đồng có chữ ký Giám đốc nhà máy. Benchmark ngành B2B: **~19 tuần** theo [ICONIQ State of Go-to-Market 2026](https://www.iconiq.com/growth/reports/state-of-go-to-market-2026) (kiểm tra 27/08/2026). |
| **Usage depth trong tài khoản** | ✅ | Đo được trực tiếp từ database query: % công nhân trong ca có ít nhất 1 phiên học hỏi đáp AI Tutor và hoàn thành tối thiểu 1 bài quiz kiểm tra trong 7 ngày qua. |
| **Chi phí triển khai ÷ ACV** | ✅ | Đo được từ bảng chấm công nhân sự: (Giờ công kỹ sư số hóa Markdown SOP + khảo sát xưởng thực tế + server setup) ÷ Giá trị hợp đồng năm (ACV). |
| **Tập trung doanh thu** | ✅ | Đo được từ báo cáo tài chính: Doanh thu của nhà máy lớn nhất ÷ Tổng doanh thu toàn hệ thống. |
| **Net Revenue Retention (NRR)** | 🔧 | Cần theo dõi sau 6–12 tháng: tỷ lệ mở rộng hợp đồng từ 1 phân xưởng ban đầu sang các phân xưởng khác trong cùng tập đoàn thép. Benchmark trung vị SaaS: **101%** theo [Benchmarkit 2025 SaaS Performance Metrics](https://www.benchmarkit.ai/2025benchmarks) (kiểm tra 27/08/2026). |
| **Gross Margin** | ✅ | Đo được hàng tháng: (Doanh thu phần mềm - Chi phí API OpenAI GPT-4o-mini - Chi phí máy chủ Render/Supabase) ÷ Doanh thu. Benchmark AI-native: **45% (2025) → 53% (2026P)** theo [ICONIQ State of AI 2026](https://www.iconiq.com/growth/reports/state-of-ai-2026) (công bố 07/2026). |

---

## Trạm 2 — Thẻ đèn

**North Star:** **Time-to-first-value (TTFV)** (Số ngày từ lúc ký hợp đồng/cấp tài khoản đến ca học đầu tiên có công nhân pass bài sát hạch an toàn SOP ≥ 80 điểm) — **Hiện tại: 21 ngày — Mục tiêu: < 10 ngày**.

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | **L** | **Time-to-first-value (TTFV)** | Đếm: số ngày từ lúc ký hợp đồng/bàn giao tài khoản xưởng đến khi công nhân đầu tiên hoàn thành 1 SOP và đạt điểm quiz ≥ 80.<br>· **Không đếm**: ngày bàn giao kỹ thuật thuần túy chưa có công nhân học thật, các tài khoản test thử của Admin. | `Ngày công nhân đầu tiên pass quiz SOP - Ngày ký biên bản bàn giao tài khoản` | Mỗi khách hàng mới · Deployment Lead | Báo trước cho: Tỷ lệ chuyển đổi POC → Paid và NRR năm đầu |
| 2 | **L** | **% deal dừng ở khâu IT Security / Data Privacy** | Đếm: các cơ hội B2B bị đóng băng hoặc từ chối do lo ngại an toàn thông tin, bảo mật bí quyết công nghệ SOP hoặc yêu cầu lưu trữ dữ liệu nội bộ.<br>· **Không đếm**: deal dừng do cắt giảm ngân sách hoặc thay đổi nhân sự. | `(Số deal dừng vì bảo mật ÷ Tổng số deal đóng/thất bại trong kỳ) × 100%` | Hàng tháng · B2B Sales Lead | Báo trước cho: Độ dài chu kỳ bán hàng (Sales cycle) và Win rate |
| 3 | **O** | **Tỷ lệ hoàn thành sát hạch an toàn tuần đầu (D7 Safety Qualification)** | Đếm: % công nhân mới đạt chuẩn an toàn lao động (hoàn thành tối thiểu 3 bài học SOP cốt lõi và đạt quiz ≥ 80) trong vòng 7 ngày đầu vào xưởng.<br>· **Không đếm**: công nhân cũ học lại, bài quiz làm thử ngắt quãng dưới 3 câu hỏi. | `(Số công nhân mới đạt chuẩn SOP D7 ÷ Tổng công nhân mới tiếp nhận trong tuần) × 100%` | Hàng tuần · Customer Success & Quản đốc ca | Báo trước cho: Usage depth trong tài khoản và Năng lực vận hành an toàn thực tế của nhà máy |
| 4 | **O** | **Usage depth trong tài khoản (% Active Workers)** | Đếm: % công nhân được cấp tài khoản có hoạt động học tập thực tế (có tối thiểu 1 phiên chat AI Tutor ≥ 3 lượt hỏi đáp hoặc nộp 1 bài quiz) trong 7 ngày qua.<br>· **Không đếm**: chỉ đăng nhập điểm danh rồi thoát, quản đốc xem màn hình thống kê. | `(Số công nhân có hoạt động học trong tuần ÷ Tổng số tài khoản công nhân được cấp) × 100%` | Hàng tuần · Hệ thống trích xuất tự động | Báo trước cho: Tỷ lệ gia hạn hợp đồng (Gross Retention Rate - GRR) |
| 5 | **O** | **Chi phí Inference AI / Ca đào tạo SOP hoàn thành (AI Cost per Finished SOP Job)** ⭐ *(Đèn chi phí AI)* | Đếm: tổng chi phí token LLM (OpenAI GPT-4o-mini qua RAG và sinh quiz) tiêu tốn cho mỗi lượt công nhân học và hoàn thành bài kiểm tra SOP.<br>· **Không đếm**: chi phí server hosting Render/Supabase cố định, chi phí test nội bộ của dev. | `Tổng chi phí token API LLM trong tháng ÷ Tổng số ca học SOP có làm quiz hoàn thành` | Hàng tuần · Tech Lead / FinOps | Báo trước cho: Biên lợi nhuận gộp (Gross Margin) của sản phẩm |
| 6 | **O** | **Chi phí triển khai & số hóa SOP ÷ ACV (Deployment Cost / ACV)** | Đếm: toàn bộ chi phí nhân sự kỹ thuật đi nhà máy khảo sát, số hóa tài liệu SOP thành Markdown, gắn video và đào tạo quản đốc chia cho giá trị hợp đồng năm.<br>· **Không đếm**: chi phí R&D phát triển tính năng chung của nền tảng. | `(Giờ công kỹ sư triển khai × Đơn giá giờ + Chi phí đi lại khảo sát xưởng) ÷ ACV` | Mỗi khách hàng mới · Operations Lead | Báo trước cho: Khả năng mở rộng quy mô (Scalability) và Biên lợi nhuận ròng |
| 7 | **G** | **Gross Margin (Biên lợi nhuận gộp)** | Đếm: tỷ lệ % doanh thu còn lại sau khi trừ giá vốn trực tiếp (COGS: chi phí API AI token + hạ tầng Cloud Render/Supabase DB & Storage + hỗ trợ kỹ thuật trực tiếp).<br>· **Không đếm**: chi phí marketing, sales, R&D tính năng mới. | `(Doanh thu phần mềm - COGS) ÷ Doanh thu phần mềm × 100%` | Hàng quý · Giám đốc Tài chính / Founder | Báo trước cho: Định giá công ty, Runway và Điểm hòa vốn (Break-even) |

**Đèn chi phí AI là đèn số:** **5** (Chi phí Inference AI / Ca đào tạo SOP hoàn thành).

---

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn [BM]/[MH]/[TB] | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | **Time-to-first-value (TTFV)** | < 14 ngày | 14–30 ngày | > 30 ngày | **[TB]** Đo lường nội bộ | Chu kỳ thử việc của công nhân nhà máy là 30 ngày; nếu sau 14 ngày công nhân chưa tự tin thao tác máy thì nhà máy sẽ quay lại cách kèm cặp thủ công và bỏ rơi phần mềm. |
| 2 | **% deal dừng ở khâu IT Security / Data Privacy** | < 10% | 10–20% | > 20% | **[BM]** [Báo cáo AI Việt Nam 2025 — Viện CNTT ĐHQGHN / Bộ KH&CN](https://mst.gov.vn/cac-nganh-nao-o-viet-nam-dang-tang-toc-ung-dung-ai-197251122182057807.htm) (công bố 24/11/2025) | Lo ngại rò rỉ bí quyết công nghệ luyện kim là rào cản số 1 của doanh nghiệp sản xuất; nếu >20% deal dừng ở đây thì sản phẩm chưa chứng minh được an toàn dữ liệu nội bộ. |
| 3 | **Tỷ lệ hoàn thành sát hạch an toàn tuần đầu (D7)** | ≥ 80% | 60–80% | < 60% | **[TB]** Quy chuẩn an toàn lao động phân xưởng thép | Công nhân không đạt chuẩn an toàn 80% trong tuần đầu sẽ có nguy cơ cao thao tác sai gây sự cố dừng lò cán hoặc tai nạn lao động nghiêm trọng. |
| 4 | **Usage depth trong tài khoản (% Active Workers)** | ≥ 65% | 40–65% | < 40% | **[BM]** [Benchmarkit 2025 SaaS Performance Metrics](https://www.benchmarkit.ai/2025benchmarks) (kiểm tra 27/08/2026) | Ca sản xuất có tính kỷ luật cao; nếu dưới 40% công nhân vào học thì chứng tỏ Quản đốc chưa đưa phần mềm vào quy chế vận hành ca bắt buộc. |
| 5 | **Chi phí Inference AI / Ca đào tạo hoàn thành** ⭐ | < 1.200 đ | 1.200 – 1.800 đ | > 1.800 đ | **[MH]** Suy ngược từ mô hình tài chính Tokenomics & COGS trần 24% | Với giá bán 75.000 đ/công nhân/tháng học 10 ca SOP, chi phí token tối đa cho phép là 1.800 đ/ca để giữ tỷ trọng chi phí AI < 24% cấu trúc giá vốn (đúng chuẩn ICONIQ 2026). |
| 6 | **Chi phí triển khai & số hóa SOP ÷ ACV** | < 15% | 15–25% | > 25% | **[MH]** Suy ngược từ ACV 180 triệu VNĐ và định mức giờ công kỹ sư | Nếu chi phí số hóa tài liệu tại chỗ vượt quá 25% ACV (>45 triệu VNĐ), công ty sẽ rơi vào bẫy công ty dịch vụ gia công tài liệu thay vì sản phẩm phần mềm có thể nhân rộng. |
| 7 | **Gross Margin (Biên lợi nhuận gộp)** | ≥ 65% | 55–65% | < 55% | **[BM]** [ICONIQ State of AI 2026](https://www.iconiq.com/growth/reports/state-of-ai-2026) (công bố 07/2026) | Báo cáo ICONIQ 2026 chỉ ra trung vị Gross Margin AI-native là 53% (2026P); duy trì mức ≥65% đảm bảo thặng dư để tái đầu tư vào R&D mô hình RAG chuyên sâu. |

---

### Phụ lục [MH] — phép tính (≥2)

#### **[MH] 1 — Chi phí Inference AI / Ca đào tạo SOP hoàn thành (Cost per Completed SOP Job)**

```
Đầu vào tài chính:
- Giá gói thuê bao phân bổ theo công nhân: 75.000 VNĐ / công nhân / tháng (trong gói hợp đồng nhà máy 180.000.000 VNĐ / năm cho phân xưởng 200 công nhân).
- Trung bình 1 công nhân học, ôn luyện và làm bài kiểm tra: 10 ca SOP / tháng.
  → Doanh thu phân bổ cho 1 ca đào tạo hoàn chỉnh = 75.000 ÷ 10 = 7.500 VNĐ / ca.
- Theo báo cáo ICONIQ State of AI 2026 (07/2026), chi phí inference AI chiếm trung bình 20% - 23% cấu trúc chi phí của công ty AI phần mềm. Để bảo vệ biên lợi nhuận gộp mục tiêu ≥ 65%, tỷ lệ chi phí token trực tiếp không được vượt quá 24% doanh thu phân bổ mỗi ca.
  → Chi phí trần AI tối đa cho phép = 7.500 VNĐ × 24% = 1.800 VNĐ / ca.

Phép tính kỹ thuật (Tokenomics mô hình GPT-4o-mini):
- Đơn giá OpenAI API (tỷ giá 25.000 VNĐ / USD):
  • Input: $0.15 / 1M tokens = 0,00375 VNĐ / token (3,75 VNĐ / 1.000 tokens).
  • Output: $0.60 / 1M tokens = 0,015 VNĐ / token (15 VNĐ / 1.000 tokens).
- Trong 1 ca học SOP tiêu chuẩn của công nhân:
  1. Hỏi đáp AI Tutor: Công nhân hỏi trung bình 6 câu hỏi kỹ thuật. Mỗi lượt nạp context Markdown SOP + System Prompt (~2.500 tokens input) và nhận phản hồi (~250 tokens output).
     Tổng 6 lượt: 15.000 input tokens + 1.500 output tokens.
     Chi phí = (15 × 3,75) + (1,5 × 15) = 56,25 + 22,5 = 78,75 VNĐ.
  2. Tạo Quiz ôn tập tự động: 1 lần gọi tool create_quiz sinh 3-5 câu trắc nghiệm. Nạp toàn văn SOP (~4.000 tokens input) và nhận cấu trúc JSON câu hỏi (~600 tokens output).
     Chi phí = (4 × 3,75) + (0,6 × 15) = 15 + 9 = 24 VNĐ.
  → Chi phí mô hình trung bình lý thuyết cho 1 ca hoàn thành = 78,75 + 24 = 102,75 VNĐ.
- Hệ số đuôi nặng (p95 Power User): Công nhân ca đêm hỏi liên tục, thử thách guardrails, câu hỏi dài hoặc lỗi kết nối phải retry gấp 10 lần mức trung bình:
  Chi phí p95 = 102,75 × 10 = 1.027,5 VNĐ / ca.

Kết quả thiết lập ngưỡng:
→ 🟢 < 1.200 VNĐ (Biên gộp AI đạt > 84%, an toàn tuyệt đối).
→ 🟡 1.200 – 1.800 VNĐ (Chạm vùng cảnh báo, cần bật Semantic Cache câu hỏi thường gặp).
→ 🔴 > 1.800 VNĐ (Vượt trần 24% doanh thu phân bổ, ăn mòn biên lợi nhuận gộp sản phẩm).
```

---

#### **[MH] 2 — Chi phí triển khai & số hóa SOP ÷ ACV (Deployment Cost / ACV)**

```
Đầu vào tài chính:
- Giá trị hợp đồng thường niên trung bình (ACV) cho 1 phân xưởng thép (200 công nhân): 180.000.000 VNĐ / năm.
- Phí thiết lập và chuẩn hóa dữ liệu ban đầu thu từ khách hàng (Setup fee): 25.000.000 VNĐ.
- Theo nguyên lý B2B của HANDBOOK §3.2, chi phí triển khai trực tiếp cho 1 khách hàng mới không được vượt quá 25% ACV (tối đa 45.000.000 VNĐ), nếu vượt quá tỷ lệ này thì công ty đang vận hành như một công ty dịch vụ gia công (Agency/System Integrator) không thể scale.

Phép tính định mức nhân sự kỹ thuật triển khai tại nhà máy:
- Lương chuyên viên kỹ thuật giải pháp triển khai (Field Engineer): 20.000.000 VNĐ / tháng (~100.000 VNĐ / giờ làm việc).
- Định mức thời gian triển khai 1 phân xưởng (15 quy trình SOP chuẩn + gắn video thao tác):
  1. Khảo sát hiện trường xưởng và phỏng vấn thợ bậc cao: 40 giờ công = 4.000.000 VNĐ.
  2. Số hóa tài liệu văn bản thành Markdown, chạy script standardize_data.py và kiểm thử prompt guardrails: 60 giờ công = 6.000.000 VNĐ.
  3. Cài đặt hệ thống, đào tạo Quản đốc và hướng dẫn tổ chức thi sát hạch thử nghiệm: 30 giờ công = 3.000.000 VNĐ.
  4. Chi phí công tác phí (đi lại, lưu trú tại khu công nghiệp nhà máy): 12.000.000 VNĐ.
  → Tổng chi phí triển khai chuẩn 1 nhà máy = 4tr + 6tr + 3tr + 12tr = 25.000.000 VNĐ.
- Tỷ lệ triển khai thực tế chuẩn = 25.000.000 ÷ 180.000.000 = 13,89% ACV.
- Trường hợp nhà máy có tài liệu phức tạp phát sinh ngoài định mức lên đến 150 giờ công và chi phí đạt 45.000.000 VNĐ → Tỷ lệ chạm ngưỡng 25% ACV.

Kết quả thiết lập ngưỡng:
→ 🟢 < 15% ACV (Quy trình số hóa SOP tự động hóa tốt, vận hành chuẩn mô hình phần mềm có thể nhân rộng).
→ 🟡 15% – 25% ACV (Cần kiểm soát chặt giờ công tại nhà máy, chuẩn hóa bộ template mẫu).
→ 🔴 > 25% ACV (Chi phí nhân công triển khai ăn mòn doanh thu năm đầu, rơi vào bẫy "dịch vụ đội lốt sản phẩm").
```

---

## Trạm 4 — 5 luật quyết định

1. **NẾU** Time-to-first-value (TTFV) > 30 ngày **TRÊN** 2 hợp đồng nhà máy gần nhất **VÀ** mỗi nhà máy có quy mô ≥ 50 công nhân **THÌ** đóng băng toàn bộ hoạt động ký mới hợp đồng với các phân xưởng mới trong 3 tuần, chuyển 100% kỹ sư giải pháp xuống xưởng rút gọn phạm vi pilot về đúng 1 dây chuyền cán thô duy nhất và bàn giao tài liệu số hóa trong 48 giờ **KHÔNG THÌ** không được tuyển thêm nhân viên kinh doanh để lấp đầy pipeline khi khâu triển khai đang tắc nghẽn. ⏹

2. **NẾU** tỷ lệ deal dừng ở khâu IT Security / Data Privacy > 20% **TRONG** 1 quý liên tiếp **VÀ** có ít nhất 5 cuộc đàm phán hợp đồng đang diễn ra **THÌ** dừng toàn bộ chiến dịch tiếp cận khách hàng mới 2 tuần, phát hành ngay gói triển khai Private On-Premise / Hybrid VPC kèm bản cam kết pháp lý không lưu trữ dữ liệu SOP trên cloud công cộng (Evidence Pack v1) **KHÔNG THÌ** không được giảm giá hợp đồng để thuyết phục khách hàng bỏ qua khâu kiểm duyệt an toàn thông tin. ⏹

3. **NẾU** Chi phí Inference AI trung bình vượt 1.800 VNĐ / ca đào tạo **TRONG** 2 tuần liên tiếp **VÀ** tổng số lượt học trong tuần đạt ≥ 500 ca **THÌ** kích hoạt ngay cơ chế Semantic Cache cho 100 câu hỏi SOP phổ biến nhất và cắt giảm cửa sổ lịch sử chat từ 6 lượt xuống tối đa 3 lượt đối với các câu hỏi tra cứu thông số kỹ thuật **KHÔNG THÌ** không được tự ý nâng giá gói cước seat của nhà máy giữa chu kỳ hợp đồng để bù đắp sự lãng phí token.

4. **NẾU** Chi phí triển khai vượt 25% ACV **TRÊN** 3 hợp đồng liên tiếp **THÌ** bắt buộc áp dụng phí thiết lập (Setup Fee) tối thiểu 30 triệu VNĐ vào báo giá hoặc chuyển giao bộ công cụ tự tải tài liệu (Self-serve SOP Upload via Admin Portal) cho phòng đào tạo của nhà máy tự thực hiện **KHÔNG THÌ** không được chấp nhận các yêu cầu tùy biến giao diện hoặc quay dựng video riêng lẻ mà không tính thêm phụ phí dịch vụ.

5. **NẾU** Usage depth của một nhà máy giảm dưới 40% **TRONG** 3 tuần liên tiếp sau khi đã go-live thành công **THÌ** cử trực tiếp 1 chuyên viên Customer Success đến gặp Quản đốc phân xưởng để tích hợp kết quả điểm thi quiz SOP vào tiêu chí chấm điểm KPI chuyên cần của ca làm việc **KHÔNG THÌ** không được chào bán thêm module đào tạo mới hay mở rộng sang phân xưởng thứ hai cho tài khoản đó cho đến khi tỷ lệ sử dụng hiện tại vượt 65%.
