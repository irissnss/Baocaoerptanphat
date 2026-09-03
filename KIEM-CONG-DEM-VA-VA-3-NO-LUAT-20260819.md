# KIỂM CỔNG ĐẾM + VÁ 3 NỢ LUẬT — Doc Version 2.2 → 2.3

> **Loại:** CHỈ XỬ LÝ LUẬT · **Ngày:** 19/08/2026 (phiên #2) · **Owner:** TanPhatERP
> **Actor:** Agent IDE · **Nhánh:** `gov/2026-08-18-rules-ui-standard-upgrade` · **Gốc phiên:** `<mã-nguồn-riêng>` · **HEAD:** `<mã-nguồn-riêng>`
> **KHÔNG:** deploy · DB/schema · `src/` · nội dung SSOT · Notion · đẩy kho mã riêng tư · nạp dữ liệu thật · gỡ PII
> **Tách bạch phiên:** việc ngoài luật → ghi sổ nợ.

---

## 0) KẾT LUẬN NGẮN

**Cổng đếm `GOV-EDIT-PRESERVE-001` HỞ — 3 lỗ, cả 3 đều chứng minh được bằng thí nghiệm.** Trước phiên này, **xoá trọn §1 của SSOT** và **xoá trọn một luật có mã GOV** đều được cổng cho **PASS**. Đã vá cả 3 + thêm cổng phụ. Kiểm ngược lại: cả hai ca đều FAIL đúng.

---

## 1) PHASE 1 — KIỂM CỔNG ĐẾM

### 1a — Cổng đếm bằng mẫu gì

| Hạng mục | Kết quả |
|---|---|
| **File script** | `scripts/tests/clause-count-gate.test.mjs` (`package.json` → `scripts["test:clause-count"]`) |
| **Mẫu đếm (bản cũ)** | `RULE_RE = /\bGOV-[A-Z0-9-]+-\d{3}\b/g` (mã luật, đếm duy nhất) **+** `HEAD_RE = /^#{1,3}\s+\S/` (heading `#`/`##`/`###`) |
| **Có đếm mục không mang mã `GOV-*`?** | **CÓ** — `docs/UI-STANDARD.md` nằm trong `LINKED_SET`, mọi `## 1)`, `## 2)`… đều được đếm là heading |
| **Cách so sánh** | Chỉ so **TỔNG** trên toàn tập liên kết với mốc chuẩn |

### 1b — Phân loại: 🔴 **HỞ**

Tưởng là "✅ ĐỦ" vì có đếm section. Nhưng **cách so sánh** mới là chỗ hỏng. Ba lỗ:

| # | Lỗ | Bằng chứng đo được |
|---|---|---|
| **1** | **Chỉ so TỔNG** → mọi "điểm dư" thành chỗ xoá tự do | Tổng hiện tại **391** vs mốc chuẩn **334** = **57 điểm dư** |
| **2** | **Đếm LẦN NHẮC, không phải ĐỊNH NGHĨA** | `GOV-PII-HANDLING-001` xuất hiện **5 lần** (1 định nghĩa + 4 nhắc chéo). Xoá khối định nghĩa → còn 4 nhắc → số "mã duy nhất" **không đổi** |
| **3** | **Mốc chuẩn cũ hơn 13 luật mới** | Mốc stamp từ `<mã-nguồn-riêng>` (18/08), `ruleList` có 26 mã, **không có** mã nào trong 13 luật mới → ĐK2 (con trỏ) không thể phát hiện chúng biến mất |

### 1c — Kiểm ngược (trước khi vá)

| Thí nghiệm | Kỳ vọng | **Kết quả THẬT** |
|---|---|---|
| Xoá **trọn §1** (mục bo góc, 1977 ký tự) của `docs/UI-STANDARD.md` | phải FAIL | ❌ **PASS** — `390 >= 334`, không điều kiện nào kêu |
| Xoá **trọn luật `GOV-PII-HANDLING-001`** ở cả 5 file (giữ parity) | phải FAIL | ❌ **PASS** — tổng vẫn 390, số mã **không đổi**, ĐK2 im lặng |

> §1 là **mục tranh cãi nhiều nhất dự án** (9 lượt Owner bác về bo góc). Trước phiên này, xoá sạch nó không cổng nào kêu.

---

## 2) PHASE 2 — VÁ

### 2a — Cổng phụ mới `test:standard-clause-count`

`scripts/tests/standard-clause-count-gate.test.mjs` — khác cổng chính ở hai điểm:

| | Cổng chính (cũ) | Cổng phụ (mới) |
|---|---|---|
| Phạm vi so | Gộp TỔNG toàn tập | **TỪNG FILE** — không có điểm dư để bù trừ |
| Đơn vị so | Số lượng | **TÊN MỤC** — mọi mục trong mốc phải CÒN |

**Kiểm ngược cổng phụ:**

| Thí nghiệm | ĐK A (số lượng) | ĐK B (tên mục) | Kết quả |
|---|---|---|---|
| Xoá §1 | FAIL (22→21) | FAIL — nêu đích danh `§1 BO GÓC (radius)` | ✅ **FAIL đúng** |
| Xoá §1 **rồi thêm §99** cho đủ số lượng | PASS (22 = 22) | **FAIL** — vẫn mất `§1 BO GÓC` | ✅ **FAIL đúng** |

> Ca thứ hai là điểm mấu chốt: **chỉ đếm số lượng thì gian lận được** — bớt một, thêm một, tổng không đổi. Cổng phụ so theo tên mục nên bắt được.

### 2b — Vá 3 lỗ của cổng chính

| Lỗ | Cách vá |
|---|---|
| 1 | Thêm **ĐK 1b — TỪNG FILE không giảm** (định nghĩa · lần nhắc · heading), song song với ĐK 1a (tổng) |
| 2 | `RULE_DEF_RE = /^(?:RULE_ID:\s+\|\*\*Rule ID:\*\*\s+`?)(GOV-…)/gm` → đếm **nơi định nghĩa**. **Giữ thêm** thước cũ (lần nhắc) vì nó bảo vệ file chỉ *liệt kê* mã (bảng phân xử legacy: xoá bớt hàng → định nghĩa không đổi nhưng nhắc giảm → vẫn bị bắt) |
| 3 | Dựng lại mốc chuẩn từ `<mã-nguồn-riêng>` **bằng đúng thước mới** (so cùng một thước), rồi `--accept` về trạng thái hiện tại. Mốc cũ giữ trong `history` |

**⚠️ Một sai lầm giữa chừng, ghi lại cho trung thực:** lần đầu tôi đổi thước đếm nhưng vẫn so với mốc đo bằng **thước cũ** → cổng báo FAIL giả (ARCHIVE `13→6`, legacy `15→0`) trong khi **không có gì bị xoá**. Đó là so khập khiễng, không phải mất nội dung. Đã dựng lại mốc cùng thước rồi mới so.

**Kiểm ngược sau khi vá:**

| Thí nghiệm | ĐK 1a | ĐK 1b | ĐK 2 | Kết quả |
|---|---|---|---|---|
| Xoá §1 của SSOT | FAIL `385 >= 386` | FAIL `docs/UI-STANDARD.md: heading 35 → 34` | PASS | ✅ **FAIL đúng** |
| Xoá trọn `GOV-PII-HANDLING-001` | FAIL `384 >= 386` | FAIL `CLAUDE.md: ĐỊNH NGHĨA luật 31 → 30` | **FAIL** | ✅ **FAIL đúng cả 3** |

### 2c — L0 EVIDENCE trỏ tới cổng phụ

Thêm vào `GOV-EDIT-PRESERVE-001` §G7.0 dòng `EVIDENCE`, kèm lý do vì sao cần cổng phụ (cổng chính đếm gộp nên điểm dư che được việc xoá).

---

## 3) PHASE 3 — VÁ 3 NỢ LUẬT

### C — Cảnh báo lỗi thời cho bản công bố (6/6 file)

Đã thêm khối cảnh báo vào **cả 6 file** trong `LUAT-QUAN-TRI-20260819/`.

**Hai vấn đề phát sinh, đã xử lý:**

1. **Hash tự tham chiếu.** Bản đầu tôi ghi mã băm của *chính file snapshot* — nhưng thêm dòng cảnh báo làm đổi mã đó, người đọc không bao giờ kiểm khớp được. → Đổi sang ghi **mã băm BẢN GỐC trong kho riêng tư**, và nói rõ cách kiểm là hash **bản gốc**, không phải file này.
2. **Snapshot đang công bố LUẬT CŨ.** `CLAUDE.md` trong bản công bố là **Doc Version 2.1** (`<mã-nguồn-riêng>`) trong khi bản sống đã là **2.3**. Nếu chỉ dán cảnh báo mà không làm mới, ta sẽ công bố luật cũ kèm mã băm của luật mới — **gây hiểu nhầm là còn hạn**. → Đã **làm mới snapshot lên 2.3** rồi mới ghi mã.

**Đối chiếu nội dung sau khi làm mới** (bỏ qua kiểu xuống dòng + khối cảnh báo):

| File snapshot | Khớp bản gốc? |
|---|---|
| `CLAUDE.md` | ✅ 1592/1592 dòng |
| `UI-STANDARD.md` | ✅ 293/293 dòng |
| `ui-standard-sources.md` | ✅ 38/38 dòng |
| `acceptance-template.md` | ✅ 64/64 dòng |

> Hai file `README.md` và `secret-exposure-status-CONG-KHAI.md` **chỉ tồn tại ở bản công bố** (không có bản gốc để đối chiếu) → cảnh báo ghi cách kiểm khác: so ngày chụp với bản công bố mới nhất.
>
> Cũng ghi rõ trong cảnh báo: file trong kho báo cáo có thể bị đổi kiểu xuống dòng khi tải về nên mã băm của chính nó sẽ khác bản gốc — thêm một lý do phải hash bản gốc.

### D — Trường `hạn_đóng` vào L12

Sửa **2 vị trí cùng chủ đề trong một lượt** (`GOV-EDIT-PRESERVE-001` yêu cầu 2):

| Vị trí | Sửa |
|---|---|
| `GOV-TECH-DEBT-LEDGER-001` §G7.10 `REQUIREMENT` mục 2 | Thêm `hạn_đóng` (ngày — NULL nếu không cấp bách) vào danh sách trường bắt buộc |
| `.governance/registry/tech-debt.md` — **hàng tiêu đề bảng** | Thêm cột `hạn_đóng` + giải thích ý nghĩa `NULL` và cách xử lý khi quá hạn |

### E + F — Sổ nợ: 13 → 15 nợ

| Mã | Nội dung | hạn_đóng | Trạng thái |
|---|---|---|---|
| DEBT-005 | Đổi khoá A+B | **26/08/2026** | MỞ |
| DEBT-013 | Phơi nhiễm PII (70 email nhân viên, 2 file) | **26/08/2026** | MỞ |
| DEBT-014 | Bản công bố thiếu cảnh báo lỗi thời — đã vá phiên này; cần rà bản công bố cũ hơn | NULL | MỞ |
| DEBT-015 | 3 lỗ cổng đếm | NULL | **ĐÃ XỬ LÝ** 19/08/2026 |

12 nợ còn lại: `hạn_đóng = NULL`.

---

## 4) KẾT QUẢ 8 CỔNG

| # | Cổng | Kết quả | Số đo |
|---|---|---|---|
| **T1** | Parity 5 file | ✅ **PASS** | 1 hash `<mã-nguồn-riêng>`, 81771 byte mỗi file |
| **T2** | Cổng đếm L0 | ✅ **PASS** | 386 ≥ 386 · ĐK1a ✅ ĐK1b ✅ ĐK2 ✅ parity ✅ |
| **T3** | Cổng phụ chuẩn (mới) | ✅ **PASS** | 22 mục · 35 heading; ĐK A ✅ ĐK B ✅ |
| **T4** | `ref-exists` (L6) | ✅ **PASS** | 42 đạt / 0 hỏng / 5 trích dẫn ca hỏng |
| **T5** | `gate-real-input` (L7) | ✅ **PASS** | **8/8 cổng** đọc đầu vào thật; `secret-scan` 0 vi phạm |
| **T6** | Schema đầy đủ | ✅ **PASS** | **13/13 luật × 14/14 trường** + SCOPE + STATUS |
| **T7** | `pii-scan` (L11) | ❌ **FAIL** | Đúng **2 vi phạm CŨ** (DEBT-013) — **KHÔNG có vi phạm mới** |
| **T8** | Khối báo cáo kết thúc | ✅ **PASS** | 11/11 trường, cổng đọc đầu ra thật |

**7/8 PASS · 1 FAIL đã có nợ ghi sổ.** Theo lưu ý của chỉ đạo phiên: T7 FAIL vì 2 vi phạm cũ → **PROVISIONAL**, không phải lỗi mới.

---

## 5) BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Kiểm cổng đếm L0: đếm bằng mã GOV-* (mọi lần NHẮC) + heading #/##/###; CÓ đếm mục
     không mang mã GOV, nhưng chỉ so TỔNG → kết luận 🔴 HỞ, 3 lỗ.
   - Kiểm ngược TRƯỚC khi vá: xoá trọn §1 của SSOT → PASS (sai); xoá trọn luật
     GOV-PII-HANDLING-001 → PASS (sai).
   - Vá cả 3 lỗ cổng chính: đếm ĐỊNH NGHĨA (giữ thêm thước NHẮC) · thêm ĐK 1b theo
     TỪNG FILE · dựng lại mốc chuẩn bằng cùng thước rồi --accept.
   - Tạo cổng phụ test:standard-clause-count: theo TỪNG FILE + theo TÊN MỤC.
   - Kiểm ngược SAU khi vá: cả 2 ca xoá đều FAIL đúng; cổng phụ bắt được cả trò
     "bớt một thêm một" (xoá §1 rồi thêm §99).
   - Thêm cảnh báo lỗi thời vào 6/6 file bản công bố; sửa lỗi hash tự tham chiếu;
     làm mới snapshot CLAUDE.md 2.1 → 2.3 (đang công bố luật cũ).
   - Thêm trường hạn_đóng vào L12 §G7.10 VÀ vào cột bảng sổ nợ (2 vị trí, 1 lượt).
   - Gắn hạn 26/08/2026 cho DEBT-005 + DEBT-013; ghi DEBT-014 + DEBT-015.
   - L0 EVIDENCE trỏ tới cổng phụ; §V thêm 1 dòng; §W thêm 1 hàng chính + 4 hàng con;
     Doc Version 2.2 → 2.3; nhân bản byte-identical 5 file.

2. PHẠM VI
   ĐỤNG    : 5 file quản trị (§G7.0 · §G7.10 · §V · §W · header) ·
             .governance/registry/tech-debt.md · 2 file mốc chuẩn ·
             scripts/tests/clause-count-gate.test.mjs · standard-clause-count-gate.test.mjs (mới) ·
             package.json · 6 file snapshot trong LUAT-QUAN-TRI-20260819/ (kho báo cáo)
   KHÔNG ĐỤNG: src/? KHÔNG · DB/schema? KHÔNG · deploy? KHÔNG · nội dung SSOT? KHÔNG
             (chỉ sửa TẠM khi kiểm ngược rồi khôi phục, hash về đúng <mã-nguồn-riêng>) ·
             Notion? KHÔNG · kho mã riêng tư? CHƯA đẩy · PII? KHÔNG gỡ (việc phiên bảo mật)

3. BẰNG CHỨNG
   sha256sum x5 → 1 hash <mã-nguồn-riêng>, 81771 byte                        → FILE_PROVEN
   check:governance → 5/5 OK                                         → RUNTIME_PROVEN
   KIỂM NGƯỢC TRƯỚC VÁ: xoá §1 SSOT → "390 >= 334 PASS" (cổng bỏ lọt) → RUNTIME_PROVEN
   KIỂM NGƯỢC TRƯỚC VÁ: xoá GOV-PII-HANDLING-001 → PASS (cổng bỏ lọt) → RUNTIME_PROVEN
   grep GOV-PII-HANDLING-001 CLAUDE.md → 5 lần (1 định nghĩa + 4 nhắc) → FILE_PROVEN
   mốc chuẩn cũ: 26 mã, không chứa mã nào của 13 luật mới            → FILE_PROVEN
   KIỂM NGƯỢC SAU VÁ: xoá §1 → FAIL (ĐK1a + ĐK1b nêu đúng file)      → RUNTIME_PROVEN
   KIỂM NGƯỢC SAU VÁ: xoá luật → FAIL cả ĐK1a + ĐK1b + ĐK2           → RUNTIME_PROVEN
   Cổng phụ: xoá §1 → FAIL; xoá §1 + thêm §99 → ĐK B vẫn FAIL        → RUNTIME_PROVEN
   test:clause-count → 386 ≥ 386, cả 3 điều kiện PASS                → RUNTIME_PROVEN
   test:standard-clause-count → 22 mục · 35 heading, ĐK A + B PASS   → RUNTIME_PROVEN
   test:ref-exists-gate → 42 đạt / 0 hỏng                            → RUNTIME_PROVEN
   test:secret-scan → 0 (mẫu) + 0 (giá trị)                          → RUNTIME_PROVEN
   test:pii-scan → đúng 2 vi phạm CŨ, 0 vi phạm mới                  → RUNTIME_PROVEN
   rà 8 cổng → 8/8 đọc đầu vào thật                                  → CODE_PROVEN
   schema 13 luật → 13/13 × 14/14 trường                             → FILE_PROVEN
   đối chiếu 4 file snapshot ↔ bản gốc → khớp 100% số dòng nội dung  → FILE_PROVEN
   khôi phục SSOT sau kiểm ngược → sha <mã-nguồn-riêng> (đúng bản trước)     → FILE_PROVEN
   ⚠️ CHƯA có UI_PROVEN / DB_PROVEN — phiên chỉ xử lý LUẬT, không đụng giao diện
      và không đụng CSDL nên không có gì để chụp ảnh hay truy vấn.

4. GHI SỔ YÊU CẦU OWNER
   [ ] ĐÃ GHI
   [x] CHƯA — lý do: mục 4 "CHO PHÉP" của chỉ đạo phiên không nêu Sổ Yêu Cầu Owner;
       không tự mở rộng phạm vi. → DEBT-008 đã có sẵn trong sổ nợ (phiên thứ 3 liên tiếp).

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng>
       · file KIEM-CONG-DEM-VA-VA-3-NO-LUAT-20260819.md
   Kèm cập nhật 6 file trong LUAT-QUAN-TRI-20260819/ (cảnh báo lỗi thời + làm mới CLAUDE.md).
   Giữ kỷ luật: KHÔNG nêu đường dẫn chính xác file chứa PII, KHÔNG nêu giá trị nào.

6. CÒN SÓT / CHƯA LÀM
   - T7 FAIL: 2 file chứa PII chưa gỡ — ngoài phạm vi phiên → DEBT-013 (hạn 26/08).
   - Đổi khoá A+B chưa làm → DEBT-005 (hạn 26/08).
   - Chưa ghi Sổ Yêu Cầu Owner → DEBT-008.
   - Chưa rà các bản công bố CŨ hơn xem có thiếu cảnh báo lỗi thời không → DEBT-014.
   - Cổng L9/L10 chưa chạy trên đợt nạp thật → DEBT-011.
   - Đánh số §G7 vẫn không liên tục → DEBT-012.
   - Địa chỉ máy chủ trong file git theo dõi → DEBT-006.
   - Chưa đẩy kho mã riêng tư (10 commit chờ Owner).
   - Cổng phụ mới chỉ đặt mốc cho docs/UI-STANDARD.md; các file chuẩn khác chưa có mốc.
   ⚠️ Đã rà lại thật; mọi mục đều có mã DEBT-xxx trong sổ nợ, trừ mục cuối (mới phát
      sinh trong phiên này) — xem trường 9.

7. ĐANG CHỜ OWNER
   - DEBT-013 (PII) + DEBT-005 (đổi khoá): hạn 26/08/2026 → cần Owner mở phiên bảo mật.
   - Đẩy kho mã riêng tư? y/n → chặn đưa luật 2.3 + 2 cổng mới lên remote.
   - Cho phép ghi Sổ Yêu Cầu Owner? → chặn trường 4 (phiên thứ 3 liên tiếp).
   - Đánh số §G7 giữ nguyên hay đánh lại (DEBT-012)?
   - Có muốn đặt mốc cổng phụ cho các file chuẩn khác ngoài SSOT giao diện không?

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner mở phiên bảo mật xử lý DEBT-013 + DEBT-005 — cả hai có hạn 26/08/2026,
   là hai nợ duy nhất trong sổ có hạn cứng.

9. CHƯA XÁC MINH ĐƯỢC
   - Cổng phụ mới chỉ có mốc cho docs/UI-STANDARD.md. Các file chuẩn khác (nếu Owner
     coi là file chuẩn) chưa được bảo vệ — chưa biết Owner muốn phủ tới đâu. Ai: Owner.
   - Bản công bố CŨ hơn LUAT-QUAN-TRI-20260819 có tồn tại không — chưa rà kho báo cáo
     toàn bộ (DEBT-014). Ai: phiên rà soát.
   - 70 email trong 2 file có bị lấy chưa — không có log truy cập. Ai: Owner (GitHub).
   - File docs/SKILL_UPGRADE_PLAN_20260819.md vẫn chưa được git theo dõi, không phải
     của phiên này; tôi KHÔNG đụng. Ai: Owner.

10. TRẠNG THÁI CHUNG
   [ ] PASS
   [x] PROVISIONAL — thiếu: T7 còn 2 vi phạm PII CŨ (DEBT-013, hạn 26/08) + 5 quyết định
       Owner ở trường 7 + ghi Sổ Yêu Cầu Owner.
       Điều kiện lên PASS: phiên bảo mật đóng DEBT-013 (test:pii-scan → 0) và Owner
       trả lời 5 câu.
       Phần LUẬT của phiên này đã xong: T1–T6 và T8 PASS; 3 lỗ cổng đếm đã vá và
       ĐÃ KIỂM NGƯỢC lại; cổng phụ hoạt động đúng cả với trò bớt-một-thêm-một;
       parity 5/5; không điều nào bị xoá.
   [ ] BLOCKED

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: KHÔNG.
   Dù không nén, mọi tham chiếu đều ĐỌC TRỰC TIẾP TỪ ĐĨA trong phiên này:
     · scripts/tests/clause-count-gate.test.mjs — ĐỌC TOÀN PHẦN trước khi vá (đây là
       nhiệm vụ trung tâm của phiên: Step 1a yêu cầu đọc toàn phần)
     · CLAUDE.md — quét 7 chủ đề (Step 0); đọc §G7.0, §G7.10, §V, §W trước khi sửa
     · .governance/registry/tech-debt.md — toàn phần trước khi thêm cột
     · .governance/registry/clause-count-baseline.json — toàn phần (để chẩn lỗ #3)
     · package.json (mục scripts) · 6 file trong LUAT-QUAN-TRI-20260819/
   KHÔNG đọc toàn phần docs/UI-STANDARD.md trong phiên này: phiên chỉ xử lý LUẬT,
   KHÔNG sửa nội dung SSOT (chỉ sửa TẠM để kiểm ngược rồi khôi phục nguyên trạng,
   hash trở lại đúng <mã-nguồn-riêng>) → GOV-READ-STANDARD-001 không kích hoạt vì TRIGGER
   của nó là "đụng giao diện", không phải "đụng file".
═══════════════════════════════════════════
```
