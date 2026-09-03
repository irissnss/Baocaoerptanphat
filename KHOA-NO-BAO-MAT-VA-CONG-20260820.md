# KHOÁ NỢ GẤP — BẢO MẬT & CỔNG TRƯỚC COMMIT

> **Loại:** KHOÁ NỢ + TOOLING · **Ngày:** 20/08/2026 · **Owner:** TanPhatERP
> **Actor:** Agent IDE · **Nhánh:** `gov/2026-08-18-rules-ui-standard-upgrade` · **Gốc phiên:** `<mã-nguồn-riêng>` · **HEAD:** `<mã-nguồn-riêng>`
> **Doc Version:** giữ nguyên **2.5** · **Parity 5 file:** `<mã-nguồn-riêng>…` **KHÔNG ĐỔI** (không đụng luật)
> **Căn cứ:** Owner chốt 4/4 lúc 21:59 20/08 — *"ok 4 quyết định theo khuyến nghị anh chốt"*

---

## 0) TÓM TẮT — 4 ĐIỀU CẦN BIẾT

1. **Hai trong bốn việc đã được phiên khác làm xong trước, và làm tốt hơn cách đề bài nêu.** `DEBT-013` và `DEBT-005` đều đã đóng. Tôi **xác minh độc lập** thay vì làm lại.
2. **Đề bài bảo thêm allowlist cho 2 file email — tôi KHÔNG làm, và đó là chủ đích.** Phiên R7 đã đổi hẳn 72 địa chỉ sang tên miền dành riêng `@example.invalid`. Thêm miễn trừ lúc này sẽ **làm cổng mù với PII MỚI** ở chính 2 file đó — đổi một lỗ hổng đã bịt lấy một lỗ hổng mới.
3. **Việc thật của phiên: nối cổng vào pre-commit (`DEBT-031`).** Sửa ở **script nguồn được git theo dõi**, thêm chứ không thay, kiểm ngược 3/3 ca. Hook đã chạy thật trên chính các commit của phiên này.
4. **Đính chính số liệu của chính tôi ở phiên trước.** Tôi từng ghi "3 luật khai AUTO-trước-commit". Rà lại 11 dòng `ENFORCEMENT` thì **chỉ §G7.5 khai đích danh "trước commit"**. Hai luật kia chỉ khai `AUTO` chung — không sai. Nhưng rà cũng lộ một khai AUTO **thật sự không có thi hành** → `DEBT-032`.

---

## 1) N1 — `DEBT-013` (email drap)

### Đề bài yêu cầu vs thực tế

| Đề bài | Thực tế đo được | Tôi làm gì |
|---|---|---|
| Thêm 2 file vào allowlist cổng `pii-scan` | Phiên **R7** (`<mã-nguồn-riêng>`, 19:41 20/08) đã **đổi hẳn 72 địa chỉ** sang `@example.invalid` — RFC 6761, tên miền không ai sở hữu được. `DEBT-013` **đã ĐÃ XỬ LÝ** | **KHÔNG thêm allowlist** |

**Bằng chứng xác minh (đếm lại từ đầu):**

| File | Tổng email | Tên miền **THẬT** còn lại |
|---|---|---|
| `docs/golive/P01-IMPLEMENTATION-REPORT-V0217.md` | 16 | **0** |
| `scripts/seed-golive-p01.sql` | 56 | **0** |

Toàn bộ 72 địa chỉ nay là `@example.invalid`.

### Vì sao KHÔNG thêm allowlist

Allowlist theo file = **tắt cảm biến cho file đó vĩnh viễn**. Hai file này vẫn đang được git theo dõi và vẫn có thể nhận PII thật trong tương lai. R7 đã chọn cách đúng hơn: **khử dữ liệu**, không **che cảm biến**. Thêm miễn trừ lúc này chỉ làm yếu đi thứ đang hoạt động tốt.

### Việc tôi thật sự sửa — một thông báo sai bản chất

Cổng in **chung một lý do** cho mọi loại bỏ qua:

> `[email] ×16 — vô hại: dữ liệu mẫu (số tuần tự/lặp)`

Email `@example.invalid` **không phải** "số tuần tự/lặp" — nó được loại vì **tên miền dành riêng**. Thông báo sai khiến phiên sau đọc log rồi kết luận nhầm về lý do file được bỏ qua. Đã sửa:

> `[email] ×16 — vô hại: placeholder trên tên miền DÀNH RIÊNG (RFC 2606/6761) — không định danh người thật`

### Kiểm ngược 2 chiều (N1.3)

| Ca | Kỳ vọng | Kết quả |
|---|---|---|
| Kho hiện tại | 0 vi phạm | ✅ **PASS** — (A) 0 · (B) 0 |
| Gieo 16 email **tên miền thật** vào file tạm | FAIL | ✅ **FAIL đúng** — bắt `scripts/_tmp_pii_probe.md [email] ×16 (ngưỡng 15)` |

File tạm đã xoá.

---

## 2) N2 — `DEBT-005` (đổi khoá) xử lý bằng bằng chứng

**`DEBT-005` đã được phiên trước đóng:** `KHÔNG CÒN HỢP LỆ — OWNER_ACCEPTED 20/08/2026` — Owner chốt chấp nhận rủi ro rò rỉ trước 15/08, xác nhận mật khẩu hiện tại an toàn.

Tôi vẫn chạy quét độc lập để **đối chứng chứ không tin suông** (đọc giá trị từ sổ bí mật + `.env*`, **không in giá trị nào**):

| Kết quả quét cây làm việc | |
|---|---|
| Số giá trị khoá đọc được | 5 |
| File **HOẠT ĐỘNG** còn tham chiếu giá trị khoá | **0** |
| File `docs/` nhắc tới | 2 — và đó là **ĐƯỜNG DẪN tới tệp khoá SSH**, không phải giá trị khoá |

→ Đúng nhánh *"không nơi hoạt động nào dùng"* của N2.3 → **không cần checklist đổi khoá**, không mở lại nợ. Phán quyết của phiên trước được bằng chứng độc lập ủng hộ.

---

## 3) N3 — `DEBT-031`: nối cổng vào pre-commit *(việc chính)*

### Sửa ở đâu

| | |
|---|---|
| **Nơi sửa** | `scripts/pre-commit-hook.sh` — **được git theo dõi**, nên máy khác clone chạy `npm run hooks:install` là có |
| Cài | `npm run hooks:install` → `.git/hooks/pre-commit` |
| Xác nhận | `npm run hooks:verify` → *"hook da cai TRUNG KHOP nguon chuan"* |

### Nối cổng nào — và vì sao chỉ hai

| Cổng | Vào hook? | Lý do |
|---|---|---|
| `test:secret-scan` | ✅ | Chặn rủi ro **không đảo ngược được** — bí mật commit rồi đẩy lên remote thì gỡ không sạch |
| `test:pii-scan` | ✅ | Như trên, với dữ liệu cá nhân |
| `clause-count` · `standard-clause-count` · `ref-exists` · `skills-registry` | ❌ | Cổng nặng, chạy theo work package qua `npm run test:gov-gates` — nhét vào hook chỉ làm commit chậm mà không chặn thêm rủi ro mất mát |

**Đo thật:** `secret-scan` 1630 ms + `pii-scan` 910 ms ≈ **2,5 giây/commit**.

### THÊM, không THAY

Toàn bộ logic `version.ts` giữ nguyên, chỉ chuyển xuống **PHẦN 2** của script. Cổng chạy **trước**, rồi rơi xuống logic cũ.

### Kiểm ngược 3/3 ca (N3.3)

| Ca | Kỳ vọng | Kết quả thật |
|---|---|---|
| Gieo mật khẩu giả + `git add` + commit | FAIL, in đúng tên cổng | ✅ `TU CHOI COMMIT — cong 'test:secret-scan' BAO LOI (GOV-SECRET-IN-CODE-001)`, nêu đúng `file:dòng`, **không in giá trị**, `EXIT=1`, commit không lọt |
| Commit tài liệu sạch | PASS | ✅ hai cổng PASS, commit qua |
| Commit đụng `version.ts` sai bậc | Logic cũ chạy đúng như trước | ✅ hai cổng PASS **rồi** `verify-release` chặn: *"version phai tang DUNG +1 bac… HEAD = V1.00.350, staged = V1.00.999"* |

File tạm đã xoá; `version.ts` đã khôi phục nguyên trạng.

> Hook không chỉ được kiểm bằng ca dựng — nó **đã chạy thật** trên chính hai commit của phiên này, in `test:secret-scan: PASS` / `test:pii-scan: PASS`.

---

## 4) 🔎 RÀ 11 KHAI `AUTO` — ĐÍNH CHÍNH + PHÁT HIỆN MỚI

### Đính chính số liệu của chính tôi

Phiên trước tôi ghi `DEBT-031` là *"3 luật khai AUTO-trước-commit"*. Rà lại toàn bộ 16 dòng `ENFORCEMENT`: **chỉ một** luật khai đích danh *"trước commit"*.

| Luật | Khai | Đúng/Sai |
|---|---|---|
| `GOV-SECRET-IN-CODE-001` §G7.5 | `AUTO — cổng quét **trước commit**` | Trước phiên này **sai**; nay **đã có thi hành thật** |
| `GOV-PII-HANDLING-001` §G7.13 | `AUTO (quét kho) + MANUAL` | **Không sai** — không hứa "trước commit". Nay được nối vào hook là **mạnh hơn mức khai** |
| `GOV-EDIT-PRESERVE-001` §G7.0 | `AUTO (cổng đếm) + MANUAL` | **Không sai** — có `test:clause-count` + cổng phụ |

### Phát hiện mới → `DEBT-032`

Rà 11 dòng khai `AUTO`: **10 dòng có cổng thật**. Một dòng không:

> `GOV-ITERATION-LIMIT-001` §G7.3 — `ENFORCEMENT: AUTO (đếm trường) + MANUAL`

`grep -rl "lần_lặp\|bác_vì" scripts/` → **0**. Không script nào đếm hai trường đó. Cùng loại vấn đề với `DEBT-031`, trái `GOV-GATE-REAL-INPUT-001` §G7.7.

*(§F1b khai `"AUTO **nếu có** cổng đếm mục sổ theo phiên"` — có chữ **nếu có**, nên không phải khai sai.)*

**Không tự sửa luật** — phiên này cấm sửa luật. Ghi `DEBT-032`, hai lối ra để phiên luật kế tiếp chọn: (a) viết cổng đếm, hoặc (b) khai lại `MANUAL`.

---

## 5) N4 — Đẩy kho mã riêng tư (Q-PUSH)

| | |
|---|---|
| Nhánh | `gov/2026-08-18-rules-ui-standard-upgrade` |
| Commit chưa đẩy trước khi push | **4** |
| Kết quả | `<mã-nguồn-riêng>..<mã-nguồn-riêng>` → `origin` · **EXIT=0** |
| Force-push / viết lại history | **KHÔNG** |

---

## 6) N5 — Ghi sổ

### Sổ Yêu Cầu Owner — 4 mục mới

| Mục | Quyết định | Kết quả |
|---|---|---|
| **#90** | Q-EMAIL | `DEBT-013` — xác minh đã đóng đúng cách, không thêm allowlist |
| **#91** | Q-KEY | `DEBT-005` — đối chứng bằng bằng chứng, 0 file hoạt động dùng khoá |
| **#92** | Q-HOOK | `DEBT-031` **ĐÃ XỬ LÝ** — nối cổng, kiểm ngược 3/3 |
| **#93** | Q-PUSH | Đã đẩy `<mã-nguồn-riêng>..<mã-nguồn-riêng>` |

Ghi bằng **quyền mặc định** của `GOV-SESSION-DECISION-001` §F1b mục 3 — không hỏi lại Owner câu nào.

### Sổ nợ — 31 → 32

| Mã | Trạng thái |
|---|---|
| `DEBT-013` | ✅ ĐÃ XỬ LÝ (R7) — xác minh lại, giữ nguyên |
| `DEBT-005` | ✅ KHÔNG CÒN HỢP LỆ — OWNER_ACCEPTED — đối chứng, giữ nguyên |
| `DEBT-031` | ✅ **ĐÃ XỬ LÝ 20/08** (phiên này) |
| `DEBT-032` | 🆕 MỞ — §G7.3 khai AUTO nhưng không có cổng đếm trường |

---

## 7) KẾT QUẢ 8 CỔNG

| # | Cổng | Kết quả | Số đo |
|---|---|---|---|
| **T1** | Parity 5 file | ✅ **PASS** | `<mã-nguồn-riêng>…` **KHÔNG ĐỔI** → chứng minh không đụng luật |
| **T2** | `clause-count` | ✅ **PASS** | **388**, không đổi; ĐK1a ✅ ĐK1b ✅ ĐK2 ✅ parity ✅ |
| **T3** | `ref-exists` | ✅ **PASS** | 53 đạt / **0 hỏng** |
| **T4** | `skills-registry` | ✅ **PASS** | 4/4 điều kiện |
| **T5** | `secret-scan` | ✅ **PASS** | 0 theo mẫu + 0 theo giá trị |
| **T6** | `pii-scan` | ✅ **PASS** | 0 vi phạm **và** email tên miền thật gieo vào vẫn FAIL |
| **T7** | pre-commit hook | ✅ **PASS** | **3/3 ca** kiểm ngược đúng; hook đã chạy thật trên commit của phiên |
| **T8** | Khối báo cáo kết thúc | ✅ **PASS** | 11/11 · trường 4 = ĐÃ GHI kèm mã mục |

**8/8 PASS.**

---

## 8) BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - N1 (Q-EMAIL): xác minh DEBT-013 đã đóng đúng cách bởi phiên R7 (72 địa chỉ →
     @example.invalid, 0 tên miền thật còn lại). KHÔNG thêm allowlist như đề bài nêu
     — thêm sẽ làm cổng mù với PII MỚI ở chính 2 file đó. Chỉ vá thông báo sai bản
     chất của cổng ("số tuần tự/lặp" → "tên miền dành riêng RFC 2606/6761").
     Kiểm ngược 2 chiều đạt.
   - N2 (Q-KEY): quét độc lập đối chứng DEBT-005 — 0 file HOẠT ĐỘNG nào còn tham
     chiếu 3 giá trị khoá (2 file docs/ chỉ nhắc ĐƯỜNG DẪN tệp khoá SSH). Không cần
     checklist đổi khoá. KHÔNG in giá trị nào. KHÔNG hỏi lại Owner.
   - N3 (Q-HOOK): nối test:secret-scan + test:pii-scan vào scripts/pre-commit-hook.sh
     (nơi ĐƯỢC GIT THEO DÕI), cài bằng hooks:install, hooks:verify xác nhận trùng khớp.
     THÊM chứ không thay — logic version.ts giữ nguyên. Kiểm ngược 3/3 ca đạt.
     DEBT-031 → ĐÃ XỬ LÝ.
   - N4 (Q-PUSH): đẩy kho mã riêng tư <mã-nguồn-riêng>..<mã-nguồn-riêng> (4 commit), EXIT=0,
     không force-push, không viết lại history.
   - N5: ghi Sổ Yêu Cầu Owner mục #90–#93; cập nhật sổ nợ; rà 11 dòng khai AUTO
     → phát hiện DEBT-032 (§G7.3 khai AUTO nhưng không có cổng đếm trường).
   - Đính chính số liệu của chính tôi ở phiên trước: chỉ 1 luật (§G7.5) khai đích
     danh "trước commit", không phải 3 như tôi đã ghi trong DEBT-031.

2. PHẠM VI
   ĐỤNG    : scripts/pre-commit-hook.sh · .git/hooks/pre-commit (qua hooks:install) ·
             scripts/tests/pii-scan-gate.test.mjs (chỉ thông báo) ·
             docs/OWNER-REQUEST-LEDGER.md (#90–#93) ·
             .governance/registry/tech-debt.md (DEBT-031 đóng, DEBT-032 mới)
   KHÔNG ĐỤNG: 5 file luật? KHÔNG (parity <mã-nguồn-riêng> không đổi) · src/? KHÔNG ·
             DB/schema? KHÔNG · deploy? KHÔNG · SSOT nội dung? KHÔNG · Notion? KHÔNG ·
             nạp dữ liệu? KHÔNG · nội dung 2 file email? KHÔNG · git history? KHÔNG ·
             force-push? KHÔNG

3. BẰNG CHỨNG
   sha256sum CLAUDE.md → <mã-nguồn-riêng> (bằng mốc đầu phiên)              → FILE_PROVEN
   đếm email 2 file → 16 và 56, tên miền THẬT = 0/0                 → FILE_PROVEN
   test:pii-scan → 0 vi phạm; gieo email tên miền thật → FAIL ×16   → RUNTIME_PROVEN
   quét 5 giá trị khoá trên cây làm việc → 0 file HOẠT ĐỘNG         → FILE_PROVEN
   npm run hooks:verify → "hook da cai TRUNG KHOP nguon chuan"      → RUNTIME_PROVEN
   hook ca 1: gieo mật khẩu → TU CHOI COMMIT, nêu đúng cổng, EXIT=1 → RUNTIME_PROVEN
   hook ca 2: commit tài liệu sạch → 2 cổng PASS, commit qua        → RUNTIME_PROVEN
   hook ca 3: version.ts sai bậc → cổng PASS rồi logic cũ chặn      → RUNTIME_PROVEN
   đo thời gian: secret-scan 1630ms + pii-scan 910ms ≈ 2,5s         → RUNTIME_PROVEN
   git push → <mã-nguồn-riêng>..<mã-nguồn-riêng>, EXIT=0                             → RUNTIME_PROVEN
   grep -rl "lần_lặp|bác_vì" scripts/ → 0                          → CODE_PROVEN
   grep "^ENFORCEMENT:" → 16 dòng, 11 khai AUTO, 1 khai "trước commit" → FILE_PROVEN
   T2–T6 → PASS (388 · 53/0 · skills 4/4 · secret 0/0 · pii 0)      → RUNTIME_PROVEN
   ⚠️ CHƯA có UI_PROVEN / DB_PROVEN — phiên không đụng giao diện và không đụng CSDL.

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #90, #91, #92, #93
   Ghi bằng quyền mặc định GOV-SESSION-DECISION-001 §F1b mục 3.

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng>
       · file KHOA-NO-BAO-MAT-VA-CONG-20260820.md
   Không nêu giá trị nhạy cảm, không nêu địa chỉ máy chủ.

6. CÒN SÓT / CHƯA LÀM
   - DEBT-032 (§G7.3 khai AUTO không có cổng) — mới ghi, phiên luật kế tiếp.
   - DEBT-016 — phiên trước để trạng thái "ĐANG XỬ LÝ — CẦN OWNER XÁC NHẬN"
     (mật khẩu đăng nhập ứng dụng, KHÁC DEBT-005 là khoá hạ tầng). Quyết định Q-KEY
     hôm nay nêu đích danh DEBT-005, KHÔNG nêu DEBT-016 → tôi KHÔNG tự suy rộng.
     Đây là câu hỏi phiên TRƯỚC đã đặt, không phải câu mới của phiên này.
   - DEBT-021 (mã băm bcrypt trong file seed, hạn 27/08) — ngoài 5 việc phiên này.
   - DEBT-001/002/003 · 004/009 · 010 · 012 · 023/030 · 016/017 · 025–029 — nợ mềm.
   - docs/AUDIT-FK-NGUOI-VA-CUSTOMER-OWNERSHIP-2026-08-20.md vẫn chưa được git theo
     dõi (phiên khác) — tôi KHÔNG đụng.
   ⚠️ Đã rà lại thật; mọi mục đều có mã DEBT-xxx trừ mục cuối (việc của phiên khác).

7. ĐANG CHỜ OWNER
   - (trống) — phiên này KHÔNG đẻ thêm câu hỏi nào cho Owner. Bốn quyết định đã chốt
     và đã thi hành xong; DEBT-032 mới ghi có sẵn hai lối ra để phiên luật tự chọn.
     Mục DEBT-016 nêu ở trường 6 là câu hỏi tồn từ phiên TRƯỚC, đã nằm trong sổ.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner đi làm module — nợ gấp đã khoá hết, phần còn lại là nợ mềm không chặn.

9. CHƯA XÁC MINH ĐƯỢC
   - 14 tài khoản thử trong file seed có đang sống trên máy vận hành không —
     không truy cập máy vận hành trong phiên này (DEBT-021). Ai: phiên bảo mật.
   - Máy khác clone kho có chạy `npm run hooks:install` hay không — hook git KHÔNG
     tự lan truyền; tôi chỉ bảo đảm được nguồn chuẩn + lệnh cài đã có sẵn. Ai: Owner.
   - Phiên audit FK (DEBT-025..029) có định làm tiếp không. Ai: Owner.

10. TRẠNG THÁI CHUNG
   [x] PASS — 8/8 cổng đạt. Bốn quyết định Owner đã thi hành xong và có bằng chứng
       RUNTIME_PROVEN. DEBT-031 đóng bằng thi hành thật (không phải bằng khai lại nhãn).
       Parity luật KHÔNG ĐỔI, chứng minh phiên này không đụng luật đúng như ràng buộc.
   [ ] PROVISIONAL   [ ] BLOCKED

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: KHÔNG.
   Dù không nén, mọi tham chiếu đều ĐỌC TRỰC TIẾP TỪ ĐĨA trong phiên này:
     · scripts/tests/pii-scan-gate.test.mjs — đọc phần lọc + ALLOW trước khi sửa;
       đọc commit <mã-nguồn-riêng> để hiểu phiên R7 đã làm gì
     · scripts/pre-commit-hook.sh + .git/hooks/pre-commit + scripts/install-hooks.mjs
       — đọc toàn phần trước khi nối cổng
     · .governance/registry/tech-debt.md — đọc DEBT-005/013/016/021/031 đầy đủ
     · docs/OWNER-REQUEST-LEDGER.md — đọc cấu trúc + mục #89 trước khi chèn #90–#93
     · SO-BI-MAT-NOI-BO.md — chỉ đọc để đối chứng, KHÔNG in giá trị nào
     · CLAUDE.md — grep 16 dòng ENFORCEMENT (rà khai AUTO), KHÔNG sửa
   KHÔNG đọc docs/UI-STANDARD.md — phiên không đụng giao diện nên
   GOV-READ-STANDARD-001 không kích hoạt.
═══════════════════════════════════════════
```
