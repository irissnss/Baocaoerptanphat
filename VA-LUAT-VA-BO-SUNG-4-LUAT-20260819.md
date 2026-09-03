# VÁ LUẬT + BỔ SUNG 4 LUẬT MỚI — Doc Version 2.1 → 2.2

> **Loại:** CHỈ XỬ LÝ LUẬT · **Ngày:** 19/08/2026 · **Owner:** TanPhatERP
> **Actor:** Agent IDE · **Lớp được phép:** Local / Code / Git — chỉ file luật + file tham chiếu mới
> **Nhánh:** `gov/2026-08-18-rules-ui-standard-upgrade` · **Gốc phiên:** `<mã-nguồn-riêng>` · **HEAD:** `<mã-nguồn-riêng>`
> **KHÔNG:** deploy · DB/schema · `src/` · nội dung SSOT `docs/UI-STANDARD.md` · Notion · đẩy kho mã riêng tư · nạp dữ liệu thật · kiểm giao diện thực tế
> **Nguyên tắc tách bạch phiên:** mọi việc ngoài luật → **ghi sổ nợ**, không làm trong phiên này.

---

## 0) TÓM TẮT — 4 ĐIỀU ĐÁNG CHÚ Ý NHẤT

1. **Luật mới bắt được lỗi thật ngay lần chạy đầu.** `GOV-PII-HANDLING-001` (L11) vừa ban hành đã phát hiện **2 file được git theo dõi chứa email nhân viên công ty mật độ cao** — một báo cáo golive (**14 địa chỉ**) và một script seed CSDL (**56 địa chỉ**). Đây là **phơi nhiễm PII thật**, đã đẩy lên remote. → **T9 FAIL** (trung thực), ghi `DEBT-013`, chuyển phiên bảo mật.
2. **Kiểm ngược lại cứu thêm 2 cổng.** Cổng `ref-exists` chỉ bắt đường dẫn **có backtick** → bỏ lọt hoàn toàn tham chiếu viết trần; vá xong phủ tăng **27 → 42** (+55%). Cổng `pii-scan` bắt nhầm 2 chỗ (regex `backup/` khớp vào tên thư mục, và SĐT demo tuần tự) → vá xong còn đúng 2 vi phạm thật.
3. **12 điểm vá thực tế đụng 17 vị trí.** Quét cùng chủ đề theo `GOV-EDIT-PRESERVE-001` lộ thêm 5 vị trí đề bài không nêu: tiêu đề §G7 còn ghi "CHÍN LUẬT", câu mở, câu SCOPE ("cả 9 luật"), cụm "cùng một yêu cầu" ở L4, và dòng Doc Version. Sửa hết trong **một lượt**.
4. **Không điều nào bị xoá.** 391 điều khoản (trước 334). 17 dòng cũ giữ **nguyên văn** ở §W, mỗi dòng một hàng con riêng.

---

## 1) BẢNG QUÉT CÙNG CHỦ ĐỀ (Step 0 — `GOV-EDIT-PRESERVE-001` yêu cầu 2)

| Chủ đề quét | Vị trí tìm thấy | Xử lý |
|---|---|---|
| "cùng một yêu cầu" | L3 `FORBIDDEN` · **L4 `REQUIREMENT` mục 3** | H1 định nghĩa ở L3; **L4 thêm dòng trỏ về đúng định nghĩa đó** (đề bài không nêu) |
| "cách làm" | L3 mục 4 · L3 `FORBIDDEN` · §L1 (skill, khác chủ đề) | H2 định nghĩa ở L3; §L1 **cố ý giữ** — nói về skill, không liên quan |
| `WARN` | duy nhất L7 `FAILURE` | H3 |
| "nhạy cảm" | L5 (2 chỗ) · Q1 · §J1 · **§J1b (danh mục gốc)** | H4 trỏ L5 → §J1b; §J1/§J1b **cố ý giữ nguyên** |
| "toàn phần" | L2 (3 chỗ) · **§V dòng UI** | H5/H6 sửa L2; §V đã đúng ("đọc TOÀN PHẦN, bắt buộc") → **cố ý giữ** |
| `EVIDENCE: Dòng khai` | L2 · **L8** | H6 sửa L2; L8 là chủ đề khác (nén phiên) → **cố ý giữ** |
| Q1 | duy nhất §G7.9 | H7 |
| "sổ" | L0 · L3 · L4 (4 chỗ) · L5 | X1 nối L3↔L4 về cùng một sổ |
| "template / mẫu bảng" | L1 `REFERENCE` · §V | X2 |
| "nén" | G5 (5 chỗ) · L8 (5 chỗ) | X3 khai rõ G5 trường 11 = cổng kiểm của L8 |
| "nợ" · "nạp dữ liệu" · "PII" | **0 kết quả** | Hoàn toàn mới, không xung đột |
| **Số đếm luật** *(đề bài không nêu)* | tiêu đề §G7 · lời mở · câu SCOPE · Doc Version | **Phát hiện thêm** — sửa cả 4 |

---

## 2) 12 ĐIỂM VÁ → 17 VỊ TRÍ

| Mã | Luật | Trường | Nội dung vá |
|---|---|---|---|
| **H1** | L3 §G7.3 | `REQUIREMENT` mục 5 | Định nghĩa "cùng một yêu cầu" = cùng Business Intent + cùng đối tượng đầu ra; **Owner phân định, không phải Agent** |
| **H2** | L3 §G7.3 | `REQUIREMENT` mục 6 | "Cách làm mới" phải khác **bản chất**; đổi tham số/số dòng/vị trí = KHÔNG phải cách mới |
| **H3** | L7 §G7.7 | `FAILURE` | `WARN` → **`BLOCK_ALL`** cho cổng đó tới khi (a) nhận đầu vào thật, hoặc (b) khai lại MANUAL + Owner duyệt rủi ro |
| **H4** | L5 §G7.5 | `REFERENCE` | "Nhạy cảm" = danh mục §J1b; §J1b mở rộng → L5 **tự áp dụng**, không phải sửa luật. Thêm: PII do L11 phủ |
| **H5** | L2 §G7.2 | `REQUIREMENT` | File SSOT > 500 dòng: đọc toàn phần **ít nhất một lần/phiên**; lần sau đọc phần liên quan + khai lần đầu; **cấm** dùng "đã đọc lần trước" khi phiên đã nén |
| **H6** | L2 §G7.2 | `EVIDENCE` | Phải ghi **TÊN FILE SSOT**, không nói "chuẩn UI"; tên trong EVIDENCE = tên trong REFERENCE, lệch → `BLOCK_MUTATION` |
| **H7** | §G7.9 | ô Q1 | `CHƯA TRẢ LỜI` → **`RIÊNG TƯ`** (Owner chốt 19/08). Gỡ khỏi cây làm việc: XONG 12/12. Viết lại history: **KHÔNG** (hỏng clone). Đổi khoá A+B: **CẦN LÀM NGAY** |
| **H8** | L1 §G7.1 | `REQUIREMENT` mục 4 | Bằng chứng phải **kiểm được bởi người ngoài phiên**; ảnh chỉ lưu máy phát triển = KHÔNG kiểm được |
| **X1** | L3 §G7.3 | `REFERENCE` | `lần_lặp` + `bác_vì` ghi vào **cùng** Sổ Yêu Cầu Owner với L4 — một dòng, đủ cả ba |
| **X2** | L1 §G7.1 | `REFERENCE` | Thứ tự: tạo template → nạp luật → chạy cổng L6. Ngược thứ tự thì L6 FAIL là **hành vi đúng** |
| **X3** | L8 §G7.8 | `REFERENCE` | G5 trường 11 là **CỔNG KIỂM** thi hành L8 — luật + cổng, không phải hai luật trùng |
| **E** | L1 §G7.1 | `TRIGGER` | Thêm "nạp dữ liệu hàng loạt · di trú dữ liệu" |
| *(L0)* | L4 §G7.4 | `REFERENCE` | **Thêm:** cụm "cùng một yêu cầu" dùng đúng định nghĩa L3 mục 5 — một định nghĩa, hai luật dùng chung |
| *(L0)* | §G7 | tiêu đề · lời mở · câu SCOPE | "CHÍN LUẬT" → "MƯỜI BA LUẬT"; "cả 9 luật" → "cả 13 luật"; "8 luật còn lại" → "12 luật còn lại" |
| *(L0)* | header | Doc Version | `2.1` → `2.2` |
| *(L0)* | §V | chỉ mục | +2 dòng: sổ nợ · checklist nạp dữ liệu |

---

## 3) 4 LUẬT MỚI — §G7.10 → §G7.13

| Mã | Luật | LEVEL | FAILURE | ENFORCEMENT | Cổng |
|---|---|---|---|---|---|
| **L12** | `GOV-TECH-DEBT-LEDGER-001` | MUST | BLOCK_ALL | MANUAL | sổ `.governance/registry/tech-debt.md` |
| **L9** | `GOV-IMPORT-RECONCILE-001` | MUST | BLOCK_ALL | AUTO + MANUAL | `npm run test:import-reconcile -- <bảng>` |
| **L10** | `GOV-IMPORT-ERROR-THRESHOLD-001` | MUST | BLOCK_MUTATION | AUTO | `npm run test:import-threshold -- <nhật-ký-lô>` |
| **L11** | `GOV-PII-HANDLING-001` | MUST | BLOCK_ALL | AUTO + MANUAL | `npm run test:pii-scan` |

- Cả 4: `SCOPE = RIÊNG — ERP` · `REVISION 1` · `STATUS = ACTIVE (Owner duyệt 19/08/2026)` · `REVIEW = 90 ngày`.
- **Một sai lệch verbatim, khai rõ:** bản văn L10 thiếu trường `REFERENCE`. R13 đòi schema đủ 14 trường → tôi **thêm** `REFERENCE`, đánh dấu "*Bổ sung schema theo R13 (bản gốc không có trường này)*" ngay trong luật. Gỡ lại được nếu Owner muốn verbatim tuyệt đối.
- **Ghi chú đánh số:** §G7.10–G7.13 đặt **trước** §G7.9 theo đúng chỉ định. Bảng quyết định Owner thành mục đóng khối. Số đọc không liên tục → ghi `DEBT-012`, chờ Owner có muốn đánh số lại không.

---

## 4) ⚠️ LUẬT MỚI BẮT ĐƯỢC LỖI THẬT NGAY LẦN ĐẦU

`npm run test:pii-scan` chạy lần đầu: **4 phát hiện** → phân loại → **2 dương tính giả + 2 THẬT**.

| Phát hiện | Kết luận | Xử lý |
|---|---|---|
| Một file kỹ năng có chuỗi `backup/` trong tên thư mục | **Dương tính giả** — regex chưa neo vào đầu đoạn đường dẫn | Vá cổng: `(^\|\/)backups?\/` |
| Một trang demo có 5 SĐT | **Dương tính giả** — số tuần tự `0901234567`, `0912345678`… là dữ liệu mẫu | Vá cổng: thêm bộ lọc dãy tăng/giảm liên tiếp |
| **Một báo cáo golive: 14 email `@` tên miền công ty** | 🔴 **PII THẬT** | `DEBT-013` — phiên bảo mật |
| **Một script seed CSDL: 56 email `@` tên miền công ty** | 🔴 **PII THẬT** | `DEBT-013` — phiên bảo mật |

> **Không nêu đường dẫn chính xác trong báo cáo này** — chạy `npm run test:pii-scan` là ra ngay, và giá trị vẫn còn trong git history (chưa viết lại theo Q1). Vị trí đã ghi trong sổ nợ ở kho mã.
>
> Hai file này **ngoài phạm vi phiên luật** (§0: việc ngoài luật → ghi sổ nợ). Tôi **không** tự sửa. Nhưng phải nói thẳng: đây là dữ liệu cá nhân của nhân sự thật, đã đẩy lên remote, và **gỡ khỏi cây làm việc không xoá khỏi history** — cùng loại quyết định như Q1.

---

## 5) HAI CỔNG ĐƯỢC VÁ NHỜ KIỂM NGƯỢC

### 5.1 `ref-exists` — bỏ lọt hoàn toàn tham chiếu viết trần

Thêm 3 file vào danh sách quét mà **số tham chiếu không đổi (27)** → dấu hiệu cổng đang bỏ lọt. Nguyên nhân: regex chỉ bắt đường dẫn **có backtick**, trong khi luật mới viết `Mẫu bảng + checklist: .governance/procedures/import-checklist.md` — **không backtick**.

| | Trước | Sau |
|---|---|---|
| Tham chiếu được kiểm | 27 | **42** (+55%) |
| Trích dẫn ca hỏng nhận diện đúng | 0 | 5 |
| Tham chiếu hỏng thật | 0 | 0 |

Cổng cũng học được cách phân biệt **tham chiếu còn sống** với **trích dẫn một ca hỏng** (L6 nêu chính file Metronic không tồn tại làm lý do ban hành luật) — xét cửa sổ ±2 dòng tìm dấu hiệu phủ định rõ ràng. Không nới thành kẽ hở.

### 5.2 `pii-scan` — 2 dương tính giả (xem §4)

---

## 6) SỔ NỢ KỸ THUẬT — 13 NỢ

`.governance/registry/tech-debt.md` (mới, theo L12). 10 nợ theo chỉ định Owner + 3 nợ phát sinh trong chính phiên này:

| Mã | Tóm tắt | Phiên nên xử lý |
|---|---|---|
| DEBT-001 · 002 | SSOT chưa rõ dải màu "3 sắc/1 sắc" và màu dải đầu trang | Phiên UI |
| DEBT-003 | Bằng chứng ảnh không có trước/sau — H8 đã vá luật, cần kiểm thực tế | Phiên UI |
| DEBT-004 | Rà báo cáo phiên trước xem có nợ chưa ghi sổ | Phiên rà soát |
| DEBT-005 | **Đổi khoá A+B** — Owner chốt RIÊNG TƯ, không viết lại history | **Phiên bảo mật** |
| DEBT-006 | Địa chỉ máy chủ trong file git theo dõi | Phiên bảo mật |
| DEBT-007 | 16 vị trí định dạng ngày không chuẩn | Phiên code |
| DEBT-008 | Chưa ghi Sổ Yêu Cầu Owner cho phiên luật 19/08 | Phiên kế tiếp |
| DEBT-009 | S9/S10 mới gán nhãn, chưa đối chiếu chi tiết | Phiên rà soát |
| DEBT-010 | Đồng bộ 13 luật + nhãn nguồn sang Notion | Phiên Notion |
| **DEBT-011** | Cổng L9/L10 mới kiểm **cơ chế**, chưa chạy trên đợt nạp thật | Phiên nạp dữ liệu đầu tiên |
| **DEBT-012** | Đánh số §G7 không liên tục (G7.10–13 trước G7.9) | Phiên luật kế tiếp |
| **DEBT-013** | 🔴 **Phơi nhiễm PII: 2 file, 70 email nhân viên** | **Phiên bảo mật — ưu tiên cao** |

---

## 7) KẾT QUẢ 10 CỔNG

| # | Cổng | Kết quả | Số đo |
|---|---|---|---|
| **T1** | Parity 5 file | ✅ **PASS** | 1 hash `<mã-nguồn-riêng>`, 78634 byte mỗi file |
| **T2** | Cổng đếm L0 (hai điều kiện) | ✅ **PASS** | 334 → **391**; ĐK1 ✅ ĐK2 ✅ parity ✅; đúng 13 mã luật |
| **T3** | `ref-exists` (L6) | ✅ **PASS** | 42 đạt / 0 hỏng / 5 trích dẫn ca hỏng |
| **T4** | `gate-real-input` (L7) | ✅ **PASS** | 8/8 cổng đọc đầu vào thật (file kho hoặc tham số/stdin); không cổng nào chỉ chạy chuỗi mẫu |
| **T5** | `secret-scan` (L5) | ✅ **PASS** | 0 vi phạm theo MẪU + 0 theo GIÁ TRỊ |
| **T6** | Schema đầy đủ | ✅ **PASS** | **13/13 luật × 14/14 trường**; SCOPE=RIÊNG—ERP 13/13; STATUS=ACTIVE 13/13 |
| **T7** | `import-reconcile` (L9) | ✅ **PASS** | 3 chiều: bảng đúng→exit 0 · bảng sai→exit 1 (chỉ đúng dòng lệch 18) · không đầu vào→exit 1 |
| **T8** | `import-threshold` (L10) | ✅ **PASS** | 3 chiều: dưới ngưỡng→exit 0 · vượt rồi chạy tiếp→exit 1 · không khai ngưỡng→exit 1 |
| **T9** | `pii-scan` (L11) | ❌ **FAIL** | **2 vi phạm THẬT** (§4) — ngoài phạm vi phiên luật, ghi `DEBT-013` |
| **T10** | Khối báo cáo kết thúc | ✅ **PASS** | 11/11 trường, cổng đọc đầu ra thật |

**9/10 PASS · 1 FAIL.** Theo quy tắc "bất kỳ FAIL → BLOCK_ALL, báo Owner": **báo Owner** — chi tiết ở §8.

---

## 8) 🚪 TRẠNG THÁI & CỔNG OWNER

### T9 FAIL — nên hiểu thế nào

Đây **không phải** lỗi của việc vá luật. Toàn bộ phần **LUẬT** (T1–T8, T10) đạt. T9 FAIL vì **luật mới vừa ban hành đã làm đúng việc của nó**: soi ra một phơi nhiễm PII có sẵn từ trước, mà trước đây không cổng nào thấy.

Theo §0 của chỉ đạo phiên (việc ngoài luật → ghi sổ nợ, không làm trong phiên này), tôi **không tự gỡ** 2 file đó. Nhưng cũng **không hạ ngưỡng cổng để nó PASS** — đó đúng là kiểu "cổng giả" mà `GOV-GATE-REAL-INPUT-001` cấm.

### Cần Owner quyết

1. **PII (DEBT-013) — ưu tiên cao.** Cho phép mở phiên bảo mật gỡ 70 email khỏi 2 file? Và: history có viết lại không (giống Q1, mặc định **không**)?
2. **Đổi khoá A+B (DEBT-005).** Q1 đã chốt RIÊNG TƯ + không viết lại history, nhưng khuyến nghị đổi khoá **vẫn còn hiệu lực** — giá trị đã tới remote.
3. **Đẩy kho mã riêng tư?** Nhánh có **6 commit** chưa đẩy (4 của phiên trước + 1 của phiên khác + 1 của phiên này).
4. **Cho phép ghi Sổ Yêu Cầu Owner** (DEBT-008)? Hai phiên liên tiếp chưa ghi được.
5. **Đánh số §G7** (DEBT-012): giữ nguyên G7.10–13 trước G7.9, hay đánh số lại cho liên tục?

---

## 9) BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Vá 12 điểm (H1–H8, X1–X3, E) trong 9 luật đã ban hành — thực tế đụng 17 vị trí,
     5 vị trí do QUÉT CÙNG CHỦ ĐỀ phát hiện thêm (đề bài không nêu).
   - Thêm 4 luật mới §G7.10–G7.13: GOV-TECH-DEBT-LEDGER-001 · GOV-IMPORT-RECONCILE-001
     · GOV-IMPORT-ERROR-THRESHOLD-001 · GOV-PII-HANDLING-001 (STATUS ACTIVE, Owner duyệt 19/08).
   - Cập nhật Q1 = RIÊNG TƯ; sửa TRIGGER L1 thêm "nạp dữ liệu hàng loạt · di trú dữ liệu".
   - Tạo .governance/registry/tech-debt.md — ghi 13 nợ (10 theo chỉ định + 3 phát sinh).
   - Tạo .governance/procedures/import-checklist.md (điều kiện nạp của §V + L9/L10/L11).
   - Viết 3 cổng mới: import-reconcile · import-threshold · pii-scan.
   - VÁ 2 CỔNG sau khi kiểm ngược: ref-exists bỏ lọt đường dẫn viết trần (27→42 tham chiếu);
     pii-scan 2 dương tính giả (regex backup/ + SĐT demo tuần tự).
   - Nhân bản byte-identical 5 file; Doc Version 2.1 → 2.2; ghi 18 hàng §W (1 chính + 17 con).

2. PHẠM VI
   ĐỤNG    : 5 file quản trị (§G7 · §V · §W · header) · .governance/registry/tech-debt.md
             · .governance/procedures/import-checklist.md · 3 cổng mới · ref-exists-gate
             · package.json (3 lệnh mới + gộp pii-scan vào bộ cổng)
   KHÔNG ĐỤNG: src/? KHÔNG · DB/schema/migration? KHÔNG · deploy/release? KHÔNG
             · nội dung SSOT docs/UI-STANDARD.md? KHÔNG · Notion? KHÔNG
             · kho mã ERP riêng tư? CHƯA đẩy · nạp dữ liệu thật? KHÔNG
             · giao diện thực tế? KHÔNG kiểm · git history? KHÔNG viết lại

3. BẰNG CHỨNG
   sha256sum x5 → 1 hash <mã-nguồn-riêng>, 78634 byte mỗi file             → FILE_PROVEN
   npm run check:governance → 5/5 OK                               → RUNTIME_PROVEN
   npm run test:clause-count → 334 → 391; ĐK1+ĐK2+parity PASS      → RUNTIME_PROVEN
   npm run test:ref-exists-gate → 42 đạt / 0 hỏng / 5 trích dẫn    → RUNTIME_PROVEN
   npm run test:secret-scan → 0 (mẫu) + 0 (giá trị)                → RUNTIME_PROVEN
   npm run test:pii-scan → 2 vi phạm THẬT (sau khi loại 2 giả)     → RUNTIME_PROVEN
   test:import-reconcile 3 chiều → exit 0 / 1 / 1                  → RUNTIME_PROVEN
   test:import-threshold 3 chiều → exit 0 / 1 / 1                  → RUNTIME_PROVEN
   kiểm schema 13 luật → 13/13 × 14/14 trường, SCOPE+STATUS đúng   → FILE_PROVEN
   rà 8 cổng → 8/8 đọc đầu vào thật, 0 cổng chỉ chạy chuỗi mẫu     → CODE_PROVEN
   ⚠️ CHƯA có UI_PROVEN / DB_PROVEN — phiên này KHÔNG đụng giao diện và KHÔNG đụng CSDL
      nên không có gì để chụp ảnh hay truy vấn. Đúng phạm vi đã khai.

4. GHI SỔ YÊU CẦU OWNER
   [ ] ĐÃ GHI
   [x] CHƯA — lý do: mục 4 "CHO PHÉP" của chỉ đạo phiên KHÔNG nêu Sổ Yêu Cầu Owner;
       không tự mở rộng phạm vi. → đã ghi DEBT-008 vào sổ nợ (L12).
       Nội dung đề nghị ghi: "Owner duyệt 4 luật mới (L9–L12) + 12 điểm vá + Q1 = RIÊNG TƯ
       (19/08). Agent IDE nạp §G7.10–G7.13, vá 17 vị trí, tạo sổ nợ + checklist nạp,
       viết 3 cổng. T9 FAIL: phát hiện phơi nhiễm PII 2 file → DEBT-013."

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng>
       · file VA-LUAT-VA-BO-SUNG-4-LUAT-20260819.md
   Kho này Owner xác nhận RIÊNG TƯ. Vẫn giữ kỷ luật công-bố-an-toàn: KHÔNG nêu đường dẫn
   chính xác của 2 file chứa PII (giá trị còn trong git history), KHÔNG nêu giá trị nào,
   KHÔNG nêu địa chỉ máy chủ. Vị trí đầy đủ tra bằng `npm run test:pii-scan`.

6. CÒN SÓT / CHƯA LÀM
   - T9 FAIL: 2 file chứa PII chưa gỡ — NGOÀI phạm vi phiên luật → DEBT-013.
   - Đổi khoá A+B chưa làm (việc trên hệ thống vận hành, ngoài lớp được phép) → DEBT-005.
   - Địa chỉ máy chủ trong file git theo dõi chưa xử lý → DEBT-006.
   - Chưa ghi Sổ Yêu Cầu Owner → DEBT-008.
   - Cổng L9/L10 chưa chạy trên đợt nạp thật (chưa có đợt nào) → DEBT-011.
   - Đánh số §G7 không liên tục → DEBT-012.
   - Chưa đẩy kho mã riêng tư (chờ Owner).
   - Chưa rà báo cáo các phiên TRƯỚC xem còn nợ nào chưa vào sổ → DEBT-004.
   ⚠️ Đã rà lại thật. Mọi mục trên đều có mã DEBT-xxx trong .governance/registry/tech-debt.md
      theo GOV-TECH-DEBT-LEDGER-001.

7. ĐANG CHỜ OWNER
   - Cho phép mở phiên bảo mật gỡ PII (DEBT-013)? → chặn việc xử lý phơi nhiễm PII.
   - Đổi khoá A+B (DEBT-005)? → chặn khép lại rủi ro credential.
   - Đẩy kho mã riêng tư? y/n → chặn việc đưa 13 luật lên remote (6 commit chờ).
   - Cho phép ghi Sổ Yêu Cầu Owner? → chặn trường 4 (hai phiên liên tiếp).
   - Đánh số §G7 giữ nguyên hay đánh lại (DEBT-012)?

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner quyết DEBT-013 (phơi nhiễm PII 70 email nhân viên) — đây là rủi ro cao nhất
   phiên này phát hiện, và là việc duy nhất không chờ được.

9. CHƯA XÁC MINH ĐƯỢC
   - 70 email trong 2 file đó có bị ai lấy chưa — không có log truy cập. Ai: Owner (GitHub).
   - Các phiên TRƯỚC còn nợ nào chưa vào sổ — chưa rà (DEBT-004). Ai: phiên rà soát.
   - Cổng L9/L10 có đúng với dữ liệu nạp thật không — chưa có đợt nạp nào (DEBT-011).
     Ai: phiên nạp dữ liệu đầu tiên.
   - Một file lạ `docs/SKILL_UPGRADE_PLAN_20260819.md` đang chưa được theo dõi trong kho,
     không phải của phiên này; tôi KHÔNG đụng vào. Vì sao có thì tôi không xác minh được.
     Ai: Owner.

10. TRẠNG THÁI CHUNG
   [ ] PASS
   [x] PROVISIONAL — thiếu: T9 còn 2 vi phạm PII thật (ngoài phạm vi phiên, đã ghi DEBT-013)
       + 5 quyết định Owner ở trường 7 + ghi Sổ Yêu Cầu Owner.
       Điều kiện lên PASS: xử lý DEBT-013 ở phiên bảo mật (test:pii-scan → 0 vi phạm)
       và Owner trả lời 5 câu.
       Phần LUẬT của phiên này đã xong và có bằng chứng: T1–T8 và T10 PASS,
       13/13 luật đủ 14/14 trường, parity 5/5, không điều nào bị xoá (391 ≥ 334).
   [ ] BLOCKED

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: KHÔNG.
   Dù không nén, mọi tham chiếu đều ĐỌC TRỰC TIẾP TỪ ĐĨA trong phiên này:
     · CLAUDE.md — quét toàn file theo 13 chủ đề (Step 0), đọc §G7 (dòng 672–957 trước sửa),
       §J1b, §V, §W; không dùng trí nhớ từ phiên trước
     · .governance/registry/tech-debt.md · .governance/procedures/import-checklist.md
       (tự viết trong phiên, đọc lại sau khi ghi)
     · scripts/tests/ref-exists-gate.test.mjs · secret-scan-gate.test.mjs
       (đọc toàn phần trước khi vá)
   KHÔNG đọc docs/UI-STANDARD.md trong phiên này — phiên chỉ xử lý LUẬT, không đụng
   nội dung SSOT, nên GOV-READ-STANDARD-001 không kích hoạt (TRIGGER là "đụng giao diện").
═══════════════════════════════════════════
```
