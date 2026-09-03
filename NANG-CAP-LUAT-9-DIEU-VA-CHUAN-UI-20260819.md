# NÂNG CẤP QUẢN TRỊ — 9 LUẬT MỚI + VÁ DỮ LIỆU CHUẨN GIAO DIỆN

> **Loại:** MUTATION LỚN (governance + chuẩn UI + bảo mật + cổng kiểm) · **Ngày:** 18–19/08/2026
> **Actor:** Agent IDE · **Lớp được phép:** Local / Code / Git · **Đã thi hành:** Master Prompt **v2** (v1 VOID)
> **Nhánh:** `gov/2026-08-18-rules-ui-standard-upgrade` · **Gốc:** `<mã-nguồn-riêng>` · **HEAD:** `<mã-nguồn-riêng>`
> **Căn cứ:** đề xuất Owner duyệt 18/08/2026 "CHỈ THÊM, KHÔNG XOÁ" + quyết định Q2/Q3/Q4 (Q1 chưa trả lời)
> **KHÔNG deploy · KHÔNG đổi schema/DB · KHÔNG ghi Notion · KHÔNG đẩy kho mã riêng tư**

---

## 0) TÓM TẮT — 4 ĐIỀU QUAN TRỌNG NHẤT

1. **9 luật mới đã nạp INLINE vào cả 5 file quản trị** (§G7), schema đầy đủ, parity byte-identical `<mã-nguồn-riêng>`. Cổng đếm: **334 → 383** điều khoản, không điều nào mất.
2. **Đặc tả pill đã vá HẾT 4 vị trí trong MỘT lượt.** Quét toàn file tìm ra **vị trí thứ 4 nằm ở §16** (đề bài chỉ biết 3). Tất cả dòng cũ giữ nguyên văn ở mục Lịch sử — kiểm máy: **9/9 dòng bị thay đều còn**.
3. **Thông tin nhạy cảm: đầu vào nêu 3 script → thực tế 12 vị trí / 11 file.** Gấp **4 lần**. Trong đó có một file `.sql` mà cổng bản đầu **bỏ qua toàn bộ theo đuôi file**, và một báo cáo **tự tuyên bố "0 credential thật"** nhưng chính dòng đó trích credential làm ví dụ.
4. **Kiểm ngược cứu cả hai cổng.** Cổng quét bí mật bản đầu **bỏ lọt 4/4** dạng thực tế → đã vá → nay bắt 4/4. Cổng báo cáo kết thúc bản đầu chỉ chạy 3 chuỗi mẫu viết cứng → nay đọc đầu ra thật, kiểm hai chiều PASS/FAIL. **Nếu không kiểm ngược thì cả hai vẫn báo PASS trong khi rỗng.**

---

## 1) BẢNG ĐỊNH VỊ (Step 1) — TÌM ĐỦ, KHÔNG BỊA ĐƯỜNG DẪN

| Mục tiêu | Đường dẫn thật | Bằng chứng định vị |
|---|---|---|
| 5 file quản trị | `.cursorrules` · `.antigravityrules` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md` | `sha256sum` × 5 → **1 hash duy nhất** `<mã-nguồn-riêng>…` trước sửa; 1492 dòng mỗi file |
| **SSOT chuẩn giao diện (Q2)** | **`docs/UI-STANDARD.md`** | (a) dòng 6 khai "Rút TỪ CODE THẬT"; (b) **5/5 file mã nó trỏ tới TỒN TẠI THẬT**; (c) brand `#f97316` khớp `src/app/globals.css:55,282,319,342` |
| Vị trí đặc tả pill | `docs/UI-STANDARD.md` §1(41) §6(95) §15(166,170) **§16(173)** | `grep -n -i "pill"` + `grep -n "rounded-full"` toàn file → **5 vị trí nói về pill header, 4 sai** |
| Cổng báo cáo giả | `scripts/tests/completion-report-gate.test.mjs` | Không có `process.argv`; 3 chuỗi mẫu viết cứng tại dòng **80 · 109 · 135** |
| File giao thức bắt buộc **KHÔNG tồn tại** | tham chiếu ở `.governance/ARCHIVE-LEGACY-RULESET.md:163` | `ls` → No such file · `find . -name "METRONIC_UI_RESEARCH_PROTOCOL*"` → **0 kết quả** |
| 11 kỹ năng UI | `.cursor/skills/{…11 thư mục…}/SKILL.md` | `ls -d` → **11/11**. Và `find .claude -type f` → **chỉ `settings.local.json`** ⇒ Claude Code không đọc được |
| 10 nguồn chuẩn UI | S1–S10 (bảng §4) | 7 file/khối trong repo + 11 kỹ năng + 2 file kế thừa — đều `ls` xác nhận |
| File chứa giá trị nhạy cảm | **11 file** (xem §6) | Quét theo GIÁ TRỊ toàn kho + `git log -S` toàn nhánh |

**NOT_FOUND:** không mục nào. Không đường dẫn nào được đoán.

---

## 2) 9 LUẬT MỚI — ĐÃ NẠP INLINE (§G7)

| Mã | Tiêu đề ngắn | LEVEL | ENFORCEMENT | REVIEW | Cổng thi hành |
|---|---|---|---|---|---|
| `GOV-EDIT-PRESERVE-001` | Sửa bảo toàn — cấm ghi đè im lặng | MUST | AUTO + MANUAL | 90 ngày | `npm run test:clause-count` |
| `GOV-ACCEPTANCE-FIRST-001` | Chốt tiêu chí nghiệm thu trước khi bắt đầu | MUST | MANUAL + AUTO | 90 ngày | `test:ref-exists-gate` (kiểm file mẫu tồn tại) |
| `GOV-READ-STANDARD-001` | Đọc chuẩn giao diện TOÀN PHẦN | MUST | MANUAL | 90 ngày | — (khai trong báo cáo) |
| `GOV-ITERATION-LIMIT-001` | Dừng CÁCH LÀM CŨ ở lần bác thứ ba | MUST | AUTO + MANUAL | 180 ngày | trường `lần_lặp` trong sổ |
| `GOV-FAILURE-RECORD-001` | Ghi trung thực thất bại (sổ + bản tin) | MUST | MANUAL | 90 ngày | — |
| `GOV-SECRET-IN-CODE-001` | Cấm bí mật trong mọi file git theo dõi | MUST NOT | AUTO | 90 ngày | `npm run test:secret-scan` |
| `GOV-REF-EXISTS-001` | Tham chiếu bắt buộc phải tồn tại thật | MUST | AUTO | 180 ngày | `npm run test:ref-exists-gate` |
| `GOV-GATE-REAL-INPUT-001` | Cổng AUTO phải đọc đầu ra thật | MUST | MANUAL | 180 ngày | rà từng cổng |
| `GOV-RELOAD-AFTER-COMPACT-001` | Đọc lại tham chiếu sau khi phiên bị nén | MUST | MANUAL | 180 ngày | — |

- **SCOPE = `RIÊNG — ERP`** cho cả 9 · **STATUS = `ACTIVE` (Owner duyệt 18/08/2026)** cho cả 9 · **REVISION 1** cho cả 9.
- **REVIEW đúng R13:** 90 ngày cho 5 luật (`EDIT-PRESERVE`, L1, L2, L4, L5) · 180 ngày cho 4 luật (L3, L6, L7, L8). Kiểm máy: `grep -c "REVIEW:      90 ngày"` = **5**, `"180 ngày"` = **4**.
- **L2 REFERENCE đã ghi đường dẫn SSOT THẬT** = `docs/UI-STANDARD.md`, kèm 3 bằng chứng định vị ngay trong luật. **Owner vui lòng xác nhận đường dẫn này.**

### ⚠️ 3 SAI LỆCH SO VỚI "VERBATIM" — KHAI RÕ, KHÔNG SỬA IM LẶNG

R13 đòi schema đầy đủ (có `REFERENCE`, có `FORBIDDEN`), nhưng bản văn verbatim của 3 luật thiếu trường. Tôi **giữ nguyên toàn bộ chữ đã cho** và **bổ sung trường còn thiếu**, ghi chú ngay trong luật:

| Luật | Trường bản gốc thiếu | Tôi đã thêm |
|---|---|---|
| `GOV-REF-EXISTS-001` | `REFERENCE` | Nêu ca gốc (file Metronic không tồn tại) + tên cổng |
| `GOV-GATE-REAL-INPUT-001` | `REFERENCE` | Nêu ca gốc (cổng PASS 7/7 trên chuỗi mẫu) |
| `GOV-RELOAD-AFTER-COMPACT-001` | `FORBIDDEN` | "Làm tiếp bằng trí nhớ từ trước nén · khai đã đọc lại mà không nêu tên tài liệu" — đánh dấu `[bổ sung schema theo R13]` |

→ Nếu Owner muốn đúng verbatim tuyệt đối, ba trường này gỡ được trong 1 phút. Tôi chọn ưu tiên R13 vì cổng PASS/FAIL đòi "schema complete".

---

## 3) VÁ ĐẶC TẢ PILL (D1) — 4/4 VỊ TRÍ, MỘT LƯỢT

### Đề bài biết 3 vị trí; quét toàn file ra **5 vị trí nói về pill header, 4 sai**

| Vị trí | §1 nói | Vị trí này nói | Trạng thái |
|---|---|---|---|
| dòng 41 (§1) | `rounded-md` | `rounded-md` | ✅ đã đúng từ V1.00.347 |
| dòng 46 (§1 tổng kết) | `rounded-md` | `rounded-md` | ✅ đã đúng |
| dòng 95 (§6 Variant A) | `rounded-md` | **`rounded-full`** | 🔧 **ĐÃ VÁ** |
| dòng 166 (§15 mục 2) | `rounded-md` | **`rounded-full`** | 🔧 **ĐÃ VÁ** |
| dòng 170 (§15 mục 6) | `rounded-md` | **`rounded-full`** | 🔧 **ĐÃ VÁ** |
| **dòng 173 (§16)** | `rounded-md` | **"→ về `rounded-md`/`rounded-full`"** — coi pill ĐÚNG của trang mẫu là "nợ phải đổi" | 🔧 **ĐÃ VÁ** ← **vị trí thứ 4, tìm bằng quét** |

### Vá cả ĐẶC TẢ, không chỉ bo góc

§6 Variant A còn lệch §1 ở **4 điểm khác** mà đề bài không nêu — đều đã đồng bộ theo code thật `src/app/m1/khach-hang/khach-hang-client.tsx:379`:

| Thuộc tính | §6 (cũ) | §1 = code thật (mới) |
|---|---|---|
| bo góc | `rounded-full` | `rounded-md` |
| padding | `px-2.5 py-1` | `px-2 sm:px-3 py-0.5 sm:py-1` |
| độ đậm chữ | `font-semibold` | `font-medium` |
| nền | `bg-{c}-50` | `bg-{c}-50/80` |
| màu icon | *(không nêu)* | `text-{c}-500` · số bọc `font-bold` |

> Nếu chỉ sửa bo góc thì lượt sau vẫn lệch padding/độ đậm → lại một vòng bác nữa. Đây đúng là lỗi mà V1.00.347 đã mắc ở quy mô nhỏ hơn.

### Bằng chứng T4

| Chuỗi sai cũ | Ở thân file | Ở mục Lịch sử §21 |
|---|---|---|
| `gap-1 rounded-full px-2.5 py-1 text-[11px] font-semibold` | **0** | 1 |
| `hàng pill \`rounded-full\` riêng dưới title` | **0** | 1 |
| `pill/avatar \`rounded-full\`` | **0** | 1 |
| `header-pill \`rounded-md\` → về \`rounded-md\`/\`rounded-full\`` | **0** | 1 |

Các dòng `rounded-full` **còn lại** trong file thuộc **badge trong hàng bảng · badge hero · avatar/icon-circle · nút tròn · badge đếm** — đúng theo §1, **cố ý giữ**, đã ghi rõ trong mục Lịch sử.

---

## 4) PHÂN XỬ 10 NGUỒN CHUẨN GIAO DIỆN (D2) — GỘP TRƯỚC, HẠ NHÃN SAU

`.governance/registry/ui-standard-sources.md` (mới) — **10/10 nguồn đều có nhãn:**

| # | Nguồn | Nhãn |
|---|---|---|
| S1 | `docs/UI-STANDARD.md` | 🟢 **SSOT** |
| S2 | Archive §11 UI & FORMAT RULES | 🟡 MERGED (tiền) + 🔴 SUPERSEDED (ngày) |
| S3 | **Nền tảng UI trả phí (Metronic)** | ⚪ **HISTORICAL** — quyết định **Q3** |
| S4 | Archive Master List DataTable | 🔴 SUPERSEDED |
| S5 | Archive Detail Panel | 🔴 SUPERSEDED |
| S6 | Archive UI Typography + Title Auto Case | 🟡 MERGED |
| S7 | `docs/ke-thua-antigravity/TANPHAT_UI_STANDARD.md` | 🔴 SUPERSEDED |
| S8 | **11 kỹ năng `.cursor/skills/`** | 🟡 **MERGED 11/11** |
| S9 | GLOBAL UX STANDARD G1 + G2-ROLLOUT | 🟡 MERGED |
| S10 | `docs/UI-GROUPED-TREE-VIEW.md` | ⚪ HISTORICAL |

**Nhãn cũng được gắn TẠI CHỖ trong archive** (chỉ THÊM banner, `2777 → 2786` dòng — không xoá dòng nào) + **5 hàng addenda UI** thêm vào `legacy-rules-status.md`, vá đúng lỗ hổng "bảng phân xử chỉ phủ mã `GOV-*`".

### D3 — gộp 11 kỹ năng: **11 xung đột đã phân xử, SSOT + code THẮNG**

Không gộp mù. `docs/UI-STANDARD.md` §20.7 ghi rõ 11 xung đột, mỗi cái một lý do. Ba cái đáng chú ý:

| Xung đột | Kỹ năng/nguồn nói | SSOT nói | Ghi chú |
|---|---|---|---|
| pill header | `master-list-page-template` §2: **`rounded-full`** | `rounded-md` | **Đây là vị trí thứ 6** của đúng đặc tả sai đã gây lượt bác #75 — nằm trong file mà Claude Code không đọc được |
| khung bảng | `master-list-page-template` §4: **`rounded-xl`** (12px) | `rounded-md` (6px) | SSOT dòng 25 **CẤM** `rounded-xl` trên card |
| hero panel | `detail-panel-layout` §1: mã ở trên, tên ở dưới | tên trên cùng | Chính kỹ năng đó ở §1.1 đã ghi name-on-top là **RECOMMENDED (Owner-approved)** — §1 là bản cũ hơn của cùng file |

**Thư mục `.cursor/skills/` GIỮ NGUYÊN VẸN làm lưu trữ** — không xoá, không sửa file kỹ năng nào.

### D4 — 4 hạng mục trước đây **không nguồn nào phủ**

| Mục mới | Nội dung | Neo vào code thật |
|---|---|---|
| **§17** | Định dạng **ngày** `DD/MM/YYYY` + **tiền** `1.234.567,89` | `formatDueDate()` `src/lib/due-state.ts:234–243` · `toLocaleString("vi-VN")` |
| **§18** | Trạng thái **đang tải · rỗng · lỗi · không có quyền** | `Loader2` (61 chỗ) là chuẩn, `Skeleton` (2 chỗ) không phải · trang `src/app/403/page.tsx:1–38` |
| **§19** | **Combobox chọn theo TÊN + tìm kiếm** (≥10 lựa chọn), value gửi đi là `id`, cấm mock | trước đây chỉ có trong kỹ năng Cursor-only |
| **§20** | Gộp 11 kỹ năng + bảng phân xử | — |

> ⚠️ **Ghi trung thực, không làm đẹp số liệu:** định dạng ngày trong code **CHƯA nhất quán** — `toLocaleDateString("vi-VN")` trơn **16 chỗ** (ra `D/M/YYYY`), biến thể đúng **2 chỗ**, biến thể `year:"2-digit"` **2 chỗ** (sai chuẩn). Đã ghi thành **nợ khai rõ** trong §17.1 thay vì tuyên bố đã chuẩn.

---

## 5) HAI CỔNG SUÝT LÀ CỔNG GIẢ — CỨU ĐƯỢC NHỜ KIỂM NGƯỢC

### 5.1 Cổng khối báo cáo kết thúc (R8)

| | Trước | Sau |
|---|---|---|
| Đầu vào | **3 chuỗi mẫu viết cứng** trong chính file kiểm (dòng 80 · 109 · 135) | Đường dẫn file · `stdin` · `REPORT_FILE` |
| Kết quả | **PASS 7/7 ở MỌI phiên**, kể cả phiên quên hẳn khối báo cáo | PASS/FAIL theo nội dung thật |
| Lệnh | `npm run test:completion-report-gate` | `npm run test:report-gate -- <đường-dẫn>` (lệnh cũ → `--selftest`) |
| Giá trị thi hành | **0** | thật |

**T5 kiểm hai chiều trên file thật:**

| Phép thử | Kỳ vọng | Thực tế |
|---|---|---|
| Báo cáo thật ĐỦ 11 trường + commit thật + mục sổ thật | PASS | ✅ `1/1 PASS`, exit **0** |
| Báo cáo thật THIẾU trường | FAIL | ✅ `0/1 PASS`, exit **1** — chỉ đúng: thiếu trường 7, 8, 11 · trường 5 "ĐÃ PUSH" không có commit · mục #9999 không có trong sổ |

Còn một lỗi tôi tự tạo rồi tự bắt: hàm trích khối cắt ngay tại dải `═` của **dòng tiêu đề** nên báo cáo hợp lệ bị chấm FAIL. Đã sửa (trích từ dòng SAU tiêu đề).

### 5.2 Cổng quét bí mật — bản đầu **bỏ lọt 4/4**

| Dạng thực tế | Bản đầu | Sau khi vá |
|---|---|---|
| `process.env.TEST_PASSWORD ?? "…"` | ❌ lọt | ✅ bắt |
| `process.env.DB_PASSWORD \|\| "…"` | ❌ lọt | ✅ bắt |
| `PASSWORD = process.env.X ?? "…"` | ❌ lọt | ✅ bắt |
| `bootstrapPassword = process.env.Y \|\| "…"` | ❌ lọt | ✅ bắt |

**Hai nguyên nhân gốc:** (1) `\bPASSWORD` không khớp `TEST_PASSWORD` vì `_` là ký tự từ; (2) không cho phép chuỗi dự phòng `= process.env.X ?? "lit"` giữa tên biến và giá trị.

**Kiểm hồi quy quyết định:** phục hồi **4 file bản trước khi gỡ** rồi quét → cổng bắt **đúng 4/4 tại đúng dòng 14 · 16 · 23 · 35**, khớp y bảng định vị Step 1.

**Thêm chế độ quét theo GIÁ TRỊ** — đọc giá trị thật từ nơi được phép giữ (sổ bí mật + `.env*` không commit) rồi tìm đúng chuỗi đó ở mọi file git theo dõi. Quét-theo-mẫu **không bao giờ** bắt được giá trị nằm trong văn xuôi, bảng tài liệu hay chú thích SQL — và đó chính là chỗ đợt trước để sót. Nếu thiếu nguồn giá trị, cổng **báo FAIL kèm cảnh báo**, không im lặng PASS.

---

## 6) THÔNG TIN NHẠY CẢM (R12/D5) — 3 SCRIPT → **12 VỊ TRÍ / 11 FILE**

Bản ghi đầy đủ (không chứa giá trị): `.governance/registry/secret-exposure-status.md`

| Giá trị | Số vị trí git theo dõi | Đã gỡ |
|---|---|---|
| **A** (13 ký tự) — mật khẩu đăng nhập dùng chung | 4 (3 script chụp ảnh/kiểm thử + **1 script migration đầu vào không nêu**) | ✅ 4/4 |
| **B** (12 ký tự) — mật khẩu seed quản trị | 7 (6 tài liệu + **1 chú thích `.sql`**) | ✅ 7/7 |
| **C** (11 ký tự) — mật khẩu CSDL máy nội bộ | 1 | ✅ 1/1 |
| | **12** | **✅ 12/12** |

**Ba điều đáng báo động:**

1. **File `.sql` bị cổng bỏ qua theo đuôi file** — `.sql` nằm trong danh sách loại trừ nhị phân. Một mật khẩu thật nằm trong chú thích SQL và **không cổng nào thấy**. Đã bỏ `.sql` khỏi danh sách loại trừ.
2. **Một báo cáo tự tuyên bố "archive 0 credential thật"** — nhưng chính dòng đó trích **hai** credential làm ví dụ lệnh tìm.
3. **Giá trị A đã lên remote từ commit ĐẦU TIÊN của kho, ngày 03/03/2026** (mã commit chỉ nêu ở bản nội bộ); `git log -S` thấy ở **9 commit** trên mọi nhánh, từng nằm cả trong ảnh chụp 5 file quản trị.

**Cách gỡ:** đọc từ biến môi trường, **không giá trị mặc định**, thiếu biến thì **báo lỗi to + `exit 1`**. Đã kiểm: chạy không có biến → in hướng dẫn tiếng Việt + `EXIT=1`.

**8 nhóm loại trừ đều có LÝ DO** (sổ bí mật gitignore · `.env*` · 3 script placeholder tường minh · đường dẫn khoá SSH ≠ giá trị khoá · 10 ảnh chụp chỉ chứa placeholder — xác minh bằng nhóm băm · chú thích nêu tên biến · 21 dương tính giả).

### ⚠️ Một chỗ tôi gỡ QUÁ CẨN THẬN — khai rõ

Một tài liệu triển khai (vị trí chính xác ở bản nội bộ): ban đầu tôi xếp là bí mật thật (22 ký tự). Đối chiếu sau cho thấy đó là **placeholder** (cùng chuỗi với `.env.deploy.example`). Thay đổi vẫn giữ vì câu mới rõ hơn, nhưng **không phải** một vụ phơi nhiễm và **không tính** vào 12 vị trí.

### 🔴 KHUYẾN NGHỊ ĐỔI KHOÁ + HOÃN VIẾT LẠI HISTORY

| Việc | Trạng thái |
|---|---|
| Gỡ khỏi cây làm việc | ✅ **XONG 12/12** |
| **Đổi khoá giá trị A và B** | 🔴 **KHUYẾN NGHỊ LÀM NGAY** — đã tới remote thì coi như đã phơi nhiễm, bất kể kho công khai hay riêng tư |
| Đổi khoá giá trị C | 🟡 Nên làm (CSDL máy nội bộ trong Docker) |
| **Viết lại git history** | ⏸️ **HOÃN** — chờ Owner trả lời **Q1** |

---

## 7) TRACE PACK

### 7.1 File thay đổi — 29 file, `+2788 / −58`

| Nhóm | File |
|---|---|
| 5 file quản trị | `.cursorrules` · `.antigravityrules` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md` (mỗi file `+315`) |
| Chuẩn UI (SSOT) | `docs/UI-STANDARD.md` (`+208`) |
| Registry / procedure **mới** | `ui-standard-sources.md` · `secret-exposure-status.md` · `procedures/acceptance-template.md` · `clause-count-baseline.json` |
| Registry / archive cập nhật | `legacy-rules-status.md` (`+18`) · `ARCHIVE-LEGACY-RULESET.md` (`+9`, chỉ thêm banner nhãn) |
| Cổng **mới** | `clause-count-gate.test.mjs` · `ref-exists-gate.test.mjs` · `secret-scan-gate.test.mjs` |
| Cổng sửa | `completion-report-gate.test.mjs` (`+145`) · `package.json` (6 lệnh mới) |
| Gỡ bí mật | 4 script/test + 7 tài liệu/sql |

### 7.2 Bảng băm — parity 5/5

| Mốc | SHA-256 (5 file giống hệt) | Số dòng |
|---|---|---|
| Trước (`<mã-nguồn-riêng>`) | `6aec8539fd7d8dce7dfd0fdb894c8a001659e36e0f4fa0bdbbd7bad92389ecd7` | 1492 |
| Sau (`<mã-nguồn-riêng>`) | `05a8ff93eb961aa6c8aa09bd1a11948e4d888c0837e3936a1e39e1114599d062` | 1797 |

`npm run check:governance` → **5/5 OK** (byte-identical 61987 byte mỗi file · nhóm an toàn inline 6 dấu × 5 file · 5 file tham chiếu tồn tại · archive bảo toàn).

### 7.3 Đếm điều khoản — HAI ĐIỀU KIỆN

| | Trước | Sau |
|---|---|---|
| Mã luật duy nhất | 26 | **35** (+9) |
| Heading | 308 | **348** (+40) |
| **Tổng** | **334** | **383** |

- **[ĐK 1]** `383 ≥ 334` → **PASS**
- **[ĐK 2]** mỗi điều rời file gốc có ĐÚNG 1 con trỏ → **PASS** (0 điều rời gốc)
- **[PHỤ]** 5 replica byte-identical → **PASS**
- 9 mã mới đúng bằng 9 luật đã nạp, không thừa không thiếu.

### 7.4 Không mất thông tin — kiểm bằng máy

9 dòng bị thay ở tầng luật/chuẩn (1 dòng `ENFORCEMENT` · 1 dòng chỉ mục §V · 7 dòng đặc tả pill). Đối chiếu tự động với mục Lịch sử → **9/9 còn nguyên văn**. Không có xoá nội dung ở bất kỳ file nào.

### 7.5 Bảng nghiệm thu của chính work package này

| # | Tiêu chí | Cách đo | Ngưỡng | Kết quả | TT |
|---|---|---|---|---|---|
| 1 | 9 luật nạp đủ vào 5 file | `grep -c "^RULE_ID:     <mã>$"` × 9 | mỗi mã = 1 | 9/9 | ✅ |
| 2 | Schema đầy đủ theo R13 | grep `SCOPE` · `STATUS` · `REVIEW` | SCOPE=RIÊNG-ERP 9/9 · REVIEW 5×90 + 4×180 | khớp | ✅ |
| 3 | Parity 5 file | `sha256sum` | 1 hash duy nhất | 1 | ✅ |
| 4 | Đếm điều khoản 2 điều kiện | `test:clause-count` | cả 2 PASS | cả 2 PASS | ✅ |
| 5 | Đặc tả pill | `grep` 4 chuỗi cũ | 0 ở thân file | 0/0/0/0 | ✅ |
| 6 | Không mất dòng cũ | đối chiếu Lịch sử | 9/9 | 9/9 | ✅ |
| 7 | Tham chiếu bắt buộc tồn tại | `test:ref-exists-gate` | 0 hỏng | 27 đạt / 0 hỏng | ✅ |
| 8 | Bí mật trong file git theo dõi | `test:secret-scan` (2 chế độ) | 0 vi phạm | 0 mẫu / 0 giá trị | ✅ |
| 9 | Cổng bắt được bí mật thật | cắm 4 dạng + phục hồi 4 file cũ | bắt hết | 4/4 và 4/4 | ✅ |
| 10 | Cổng báo cáo đọc đầu ra thật | `test:report-gate` 2 chiều | PASS(a) FAIL(b) | exit 0 / exit 1 | ✅ |
| 11 | Biên dịch | `tsc --noEmit` · `npm run build` | 0 lỗi | tsc 0 · build 0 | ✅ |
| 12 | Nhãn 10 nguồn UI | đếm hàng registry | 10/10 | 10/10 | ✅ |
| 13 | Gộp 11 kỹ năng, gốc còn nguyên | `ls -d` + `git status` | 11/11 · 0 sửa | 11/11 · 0 | ✅ |
| 14 | Không deploy / không đổi schema | `git diff --stat` | 0 file DB/deploy | 0 | ✅ |

---

## 8) KẾT QUẢ TEST PLAN

| Mã | Nội dung | Kết quả |
|---|---|---|
| **T1** | Parity `sha256` × 5 giống nhau, sau Step 5 và ở trạng thái cuối | ✅ **PASS** — 1 hash `<mã-nguồn-riêng>` |
| **T2** | Cổng đếm: **cả hai** điều kiện | ✅ **PASS** — 383 ≥ 334; 0 điều rời gốc |
| **T3** | Mọi tham chiếu bắt buộc tồn tại (kể cả file mẫu; Metronic đã hạ nhãn) | ✅ **PASS** — 27 đạt / 0 hỏng |
| **T4** | `grep` đặc tả pill cũ → 0 ngoài mục Lịch sử | ✅ **PASS** — 0/0/0/0 |
| **T5** | Cổng báo cáo trên file THẬT: (a) đủ → PASS, (b) thiếu → FAIL | ✅ **PASS** — exit 0 / exit 1 |
| **T6** | 0 bí mật trong file git theo dõi; script thiếu biến → lỗi to, exit ≠ 0 | ✅ **PASS** — 0 vi phạm; `EXIT=1` |
| **T7** | `tsc` + `build` xanh (có sửa TS) | ✅ **PASS** — tsc 0 · build 0 |
| **T8** | Danh sách vị trí = số file đã xử lý (+ lý do cho phần loại trừ) | ✅ **PASS** — 12 vị trí định vị / **12 đã gỡ**; 8 nhóm loại trừ đều có lý do |

---

## 9) ATTRIBUTION LEDGER

| Hành động | Lớp | Ai làm | Bằng chứng |
|---|---|---|---|
| `MODIFIED_LOCAL` | 5 file quản trị (§G7, §V, §W, §G5) | Agent IDE | commit `<mã-nguồn-riêng>`, `<mã-nguồn-riêng>` · sha256 `<mã-nguồn-riêng>` |
| `MODIFIED_LOCAL` | `docs/UI-STANDARD.md` §17–§21 | Agent IDE | commit `<mã-nguồn-riêng>` |
| `MODIFIED_LOCAL` | registry + procedures (4 file mới, 2 cập nhật) | Agent IDE | `<mã-nguồn-riêng>`, `<mã-nguồn-riêng>` |
| `MODIFIED_LOCAL` | 3 cổng mới + 1 cổng sửa + `package.json` | Agent IDE | `<mã-nguồn-riêng>` |
| `MODIFIED_LOCAL` | Gỡ bí mật 11 file | Agent IDE | `<mã-nguồn-riêng>` |
| `VERIFIED` | parity · đếm điều khoản · tham chiếu · quét bí mật (2 chế độ) · cổng báo cáo 2 chiều · tsc · build | Agent IDE | đầu ra lệnh ở §7, §8 |
| `OBSERVED` | Metronic protocol không tồn tại · `.claude/` không có kỹ năng · `.sql` bị cổng bỏ qua · giá trị A trên remote từ 03/03/2026 | Agent IDE | §1, §6 |
| `OWNER_APPROVED` | 9 luật · Q2 · Q3 · Q4 | Owner 18/08/2026 | STATUS trong từng luật |
| `DEFERRED` | Viết lại git history · đổi khoá | — | chờ **Q1** |
| `BLOCKED` | Đẩy kho mã riêng tư | — | cổng Owner ở §11 |
| **KHÔNG** `MODIFIED_NOTION` | — | — | Agent IDE **không** đụng Notion lượt này |

## 10) CROSS-LAYER MATRIX

| Lớp | Đã đổi? | Bằng chứng | Ghi chú |
|---|---|---|---|
| **Luật (5 file)** | ✅ CÓ | sha256 `<mã-nguồn-riêng>`, parity 5/5 | +9 luật INLINE, không xoá điều nào |
| **Chuẩn UI (SSOT)** | ✅ CÓ | `docs/UI-STANDARD.md` §0–§21 | +4 mục D4, +§20 gộp, vá 4 vị trí pill |
| **Registry / Archive** | ✅ CÓ | 4 file mới, 2 cập nhật | archive **chỉ thêm** banner nhãn |
| **Mã nguồn (`src/`)** | ❌ KHÔNG | `git diff --stat` không có `src/` | Không đụng logic ứng dụng |
| **Script / cổng kiểm** | ✅ CÓ | 3 mới, 2 sửa | Không đổi hành vi nghiệp vụ |
| **Tài liệu (docs)** | ✅ CÓ | 7 file gỡ bí mật + SSOT | Chỉ redact, giữ nguyên nội dung khác |
| **CSDL / schema** | ❌ KHÔNG | không migration, không DDL | — |
| **Deploy / VPS / vận hành** | ❌ KHÔNG | không chạy lệnh deploy | — |
| **Version ERP** | ❌ KHÔNG bump | `src/lib/version.ts` không đổi | Đúng `GOV-VERSION-RELEASE-001`: đây là governance/docs, không phải release |
| **Notion** | ❌ KHÔNG | — | Chuyển tiếp cho TanPhatAI (§12) |
| **Kho báo cáo công khai** | ✅ CÓ | commit ở trường 5 §13 | Đã qua cổng công-bố-an-toàn |
| **Kho mã riêng tư (`origin`)** | ⏸️ **CHƯA ĐẨY** | `git status` sạch, chưa `push` | **Chờ Owner duyệt** |

---

## 11) 🚪 CỔNG OWNER

### Câu hỏi 1 — ĐẨY KHO MÃ RIÊNG TƯ?

Nhánh `gov/2026-08-18-rules-ui-standard-upgrade` có **4 commit sạch**, mọi cổng PASS, **chưa đẩy**.

> **Đẩy kho mã riêng tư? (y / n)**
> `y` → tôi `git push -u origin gov/2026-08-18-rules-ui-standard-upgrade` (không gộp vào `main` nếu Owner chưa nói)
> `n` → giữ nguyên local, chờ Owner rà thêm

### Câu hỏi 2 — **Q1 nhắc lại (đang chặn)**

> **Kho mã `origin` là CÔNG KHAI hay RIÊNG TƯ?**
>
> - **CÔNG KHAI** → phơi nhiễm nghiêm trọng: **đổi khoá A + B ngay**, cân nhắc **viết lại git history** (9 commit, từ 03/03/2026).
> - **RIÊNG TƯ** → mức thấp hơn: **vẫn nên đổi khoá A + B**, và **có thể KHÔNG cần** viết lại history (viết lại sẽ làm hỏng mọi bản clone hiện có).
>
> Tôi **không** tự quyết việc này. Trong lúc chờ: giá trị đã gỡ hết khỏi cây làm việc, history **chưa** bị đụng.

### Ba việc nhỏ cần Owner xác nhận

1. **Đường dẫn SSOT** đã ghi vào `GOV-READ-STANDARD-001` là `docs/UI-STANDARD.md` — đúng ý Owner chưa?
2. **3 trường schema tôi thêm** (§2) — giữ, hay gỡ để đúng verbatim tuyệt đối?
3. **Ghi Sổ Yêu Cầu Owner**: lượt này là CHỈ-ĐỌC-SỔ (không tự ghi thêm mục). Cho phép ghi mục mới không?

---

## 12) HANDOFF

**→ TanPhatAI (Agent Notion):** sau khi Owner rà báo cáo này, cập nhật tài liệu bên Notion. Đọc Control Truth hiện hành trước. **Không** claim thay đổi code. Agent IDE **không** đụng Notion.

Điểm cần đồng bộ sang Notion: 9 luật mới (mã + LEVEL + REVIEW) · SSOT giao diện = `docs/UI-STANDARD.md` · nhãn 10 nguồn (Metronic = HISTORICAL) · yêu cầu chốt tiêu chí nghiệm thu trước khi làm · trạng thái phơi nhiễm + khuyến nghị đổi khoá.

---

## 13) BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Nạp 9 luật mới INLINE vào cả 5 file quản trị (§G7), schema đầy đủ theo R13.
   - Ghi đường dẫn SSOT THẬT (docs/UI-STANDARD.md) vào GOV-READ-STANDARD-001 kèm 3 bằng chứng.
   - Tạo .governance/procedures/acceptance-template.md (điều kiện nạp của L1).
   - Vá đặc tả pill HẾT 4 vị trí trong MỘT lượt; tìm ra vị trí thứ 4 (§16) bằng quét toàn file;
     vá cả padding/độ đậm/nền/màu icon theo code thật, không chỉ bo góc.
   - Gán nhãn 10/10 nguồn chuẩn UI (registry mới) + gắn nhãn TẠI CHỖ trong archive
     + 5 hàng addenda UI vào bảng phân xử cũ (trước chỉ phủ mã GOV-*).
   - Gộp 11 kỹ năng UI vào SSOT (§20) kèm phân xử 11 xung đột; thư mục kỹ năng gốc GIỮ NGUYÊN.
   - Thêm 4 mục D4: định dạng ngày/tiền · 4 trạng thái hiển thị · combobox chọn theo TÊN.
   - Sửa cổng khối báo cáo kết thúc để đọc ĐẦU RA THẬT; lệnh cũ chuyển sang --selftest.
   - Viết 3 cổng mới: đếm điều khoản 2 điều kiện · tham chiếu tồn tại · quét bí mật 2 chế độ.
   - KIỂM NGƯỢC cả 2 cổng: quét bí mật bản đầu bỏ lọt 4/4 -> vá -> bắt 4/4;
     phục hồi 4 file bản cũ -> bắt đúng 4/4 tại dòng 14/16/23/35.
   - Gỡ 12 vị trí nhạy cảm / 11 file (đầu vào nêu 3) -> biến môi trường, lỗi to, exit != 0.
   - Lập .governance/registry/secret-exposure-status.md (không chứa giá trị).

2. PHẠM VI
   ĐỤNG    : 29 file (+2788/-58) — 5 file quản trị · docs/UI-STANDARD.md · 4 registry/procedure mới
             · 2 registry cập nhật · archive (chỉ thêm banner) · 3 cổng mới · 2 cổng sửa
             · package.json · 11 file gỡ bí mật
   KHÔNG ĐỤNG: src/? KHÔNG · DB/schema? KHÔNG · deploy/VPS? KHÔNG · version ERP? KHÔNG bump
             · Notion? KHÔNG · .cursor/skills/? KHÔNG (giữ nguyên lưu trữ)
             · git history? KHÔNG viết lại (hoãn chờ Q1) · kho mã riêng tư? CHƯA đẩy

3. BẰNG CHỨNG
   sha256sum x5 -> 1 hash <mã-nguồn-riêng> (trước: <mã-nguồn-riêng>)                        -> FILE_PROVEN
   npm run check:governance -> 5/5 OK, 61987 byte mỗi file                  -> RUNTIME_PROVEN
   npm run test:clause-count -> 334 -> 383; ĐK1 PASS, ĐK2 PASS, parity PASS -> RUNTIME_PROVEN
   npm run test:ref-exists-gate -> 27 đạt / 0 hỏng                          -> RUNTIME_PROVEN
   npm run test:secret-scan -> 0 vi phạm (mẫu) + 0 vi phạm (giá trị)        -> RUNTIME_PROVEN
   test:report-gate <file đủ> -> exit 0 ; <file thiếu> -> exit 1            -> RUNTIME_PROVEN
   cắm 4 dạng bí mật -> bắt 4/4 ; phục hồi 4 file cũ -> bắt 4/4 đúng dòng   -> RUNTIME_PROVEN
   chạy script thiếu biến -> in lỗi tiếng Việt + EXIT=1                     -> RUNTIME_PROVEN
   npx tsc --noEmit -> 0 lỗi ; npm run build -> exit 0                      -> CODE_PROVEN
   grep 4 chuỗi pill cũ -> 0 ở thân file, 1 ở Lịch sử mỗi chuỗi             -> FILE_PROVEN
   đối chiếu 9 dòng bị thay với mục Lịch sử -> 9/9 còn nguyên văn           -> FILE_PROVEN
   git log -S <giá trị A> --all -> 9 commit, sớm nhất 03/03/2026 (mã: bản nội bộ) -> FILE_PROVEN
   ⚠️ CHƯA có UI_PROVEN — lượt này không sửa giao diện đang chạy, chỉ sửa TÀI LIỆU chuẩn.
      Không trang nào cần chụp ảnh. Việc áp chuẩn mới vào trang thật là gói việc SAU,
      và khi đó GOV-ACCEPTANCE-FIRST-001 buộc phải có dòng UI_PROVEN.

4. GHI SỔ YÊU CẦU OWNER
   [ ] ĐÃ GHI
   [x] CHƯA — lý do: prompt cho phép sửa file luật + chuẩn UI + cổng, KHÔNG nêu Sổ Yêu Cầu Owner
       trong ALLOWED_ACTIONS. Không tự mở rộng phạm vi. ĐỀ NGHỊ Owner cho phép ghi mục mới,
       nội dung: "Owner duyệt 9 luật + Q2/Q3/Q4 (18/08); Agent IDE nạp §G7, vá 4 vị trí pill,
       gán nhãn 10 nguồn UI, gộp 11 kỹ năng, sửa cổng giả, gỡ 12 vị trí nhạy cảm;
       Q1 chưa trả lời -> hoãn viết lại history; chờ Owner duyệt đẩy kho mã."
       (Lưu ý: một phiên khác đã ghi mục #81/#82 vào sổ trong lúc lượt này chạy — xem trường 9.)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng>
       · file NANG-CAP-LUAT-9-DIEU-VA-CHUAN-UI-20260819.md
   Cổng công-bố-an-toàn: bản công khai KHÔNG nêu file:dòng của bí mật (git history còn giá trị,
   chưa viết lại) · KHÔNG nêu địa chỉ máy chủ · KHÔNG nêu giá trị nào.
   Bản nội bộ này giữ đầy đủ vị trí.

6. CÒN SÓT / CHƯA LÀM
   - Đổi khoá giá trị A, B, C: CHƯA làm (là việc trên hệ thống vận hành, ngoài lớp được phép).
   - Viết lại git history: CHƯA — hoãn theo yêu cầu, chờ Q1.
   - Chưa ghi Sổ Yêu Cầu Owner (xem trường 4).
   - Chưa đẩy kho mã riêng tư (cổng Owner).
   - S9/S10 chỉ gán nhãn + nêu chỗ đã gộp; CHƯA đối chiếu từng hạng mục chi tiết như S1–S8.
   - Định dạng ngày trong code vẫn chưa nhất quán (16 chỗ + 2 chỗ sai chuẩn) — đã ghi thành
     nợ khai rõ ở SSOT §17.1, CHƯA sửa code (ngoài phạm vi gói việc này).
   - Địa chỉ IP máy chủ vận hành xuất hiện trong vài tài liệu được git theo dõi — PHÁT HIỆN MỚI,
     ngoài phạm vi D5 (không phải credential), CHƯA xử lý, cần Owner quyết.
   ⚠️ Đã rà lại thật — danh sách đích danh.

7. ĐANG CHỜ OWNER
   - Đẩy kho mã riêng tư? y/n -> chặn việc đưa 9 luật lên remote.
   - Q1 kho công khai hay riêng tư? -> chặn quyết định viết lại git history + mức khẩn đổi khoá.
   - Xác nhận đường dẫn SSOT = docs/UI-STANDARD.md -> chặn việc coi L2 là chốt.
   - Giữ hay gỡ 3 trường schema tôi thêm (§2)?
   - Cho phép ghi Sổ Yêu Cầu Owner? -> chặn trường 4.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner trả lời Q1 (kho công khai hay riêng tư). Đây là câu chặn nặng nhất: nó quyết định
   mức khẩn của việc đổi khoá và có viết lại git history hay không.

9. CHƯA XÁC MINH ĐƯỢC
   - Kho origin công khai hay riêng tư — lệnh git local không cho biết. Ai: Owner.
   - Giá trị A/B/C có bị ai khai thác chưa — không có log truy cập. Ai: Owner (nhà cung cấp VPS/GitHub).
   - Nội dung 3 nguồn chuẩn UI ngoài repo (Notion) — ngoài lớp được phép. Ai: Owner / Agent Notion.
   - Một phiên khác commit <mã-nguồn-riêng> (18/08 23:46) lên CÙNG nhánh này giữa lúc lượt này chạy
     (chỉ đụng OWNER-REQUEST-LEDGER.md + PLAN-IMPORT-APPSHEET). Đã kiểm: không đụng file nào
     của lượt này, parity 5 file vẫn nguyên. Nhưng vì sao có 2 phiên ghi cùng nhánh thì
     tôi KHÔNG xác minh được. Ai: Owner.

10. TRẠNG THÁI CHUNG
   [ ] PASS
   [x] PROVISIONAL — thiếu: Q1 + 4 quyết định Owner ở trường 7 + đổi khoá (ngoài lớp)
       + ghi Sổ Yêu Cầu Owner.
       Điều kiện lên PASS: Owner trả lời Q1 và 4 câu còn lại; đổi khoá A+B; cho phép ghi sổ.
       Toàn bộ phần THI HÀNH trong lớp Local/Code/Git đã xong và có bằng chứng RUNTIME_PROVEN
       (14/14 dòng bảng nghiệm thu ĐẠT, T1–T8 PASS) — không phụ thuộc các mục đang thiếu.
   [ ] BLOCKED

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: KHÔNG (phiên này chưa gặp mốc nén nào).
   Dù không nén, MỌI tham chiếu đều ĐỌC TRỰC TIẾP TỪ ĐĨA trong lượt này, không dùng trí nhớ:
     · docs/UI-STANDARD.md — đọc TOÀN PHẦN trước và sau khi sửa (đúng GOV-READ-STANDARD-001,
       chính điều mà ca thất bại đã vi phạm 12 lần bằng cách đọc lỗ khoá)
     · CLAUDE.md — §G5/G6 (576–660), §K (866–886), §V (1480–1492), §P2 (1320–1345), toàn §G7 sau khi nạp
     · .governance/ARCHIVE-LEGACY-RULESET.md — 150–185, 1993–2050, 2076–2110, 2683
     · .governance/registry/legacy-rules-status.md — toàn phần
     · .governance/registry/{ui-standard-sources,secret-exposure-status}.md — toàn phần (tự viết)
     · .governance/procedures/acceptance-template.md — toàn phần (tự viết)
     · 11 file .cursor/skills/*/SKILL.md — cấu trúc mục + nội dung các mục cần gộp
     · scripts/tests/completion-report-gate.test.mjs — toàn phần trước khi sửa
     · src/app/m1/khach-hang/khach-hang-client.tsx:376–392 · src/lib/due-state.ts:234–243
       · src/app/403/page.tsx:1–38 (neo D4 vào code thật)
     · SO-BI-MAT-NOI-BO.md — cấu trúc (để cổng quét theo giá trị), KHÔNG trích giá trị
   Không có đối tượng nào phải tham chiếu mà chưa đọc lại.
═══════════════════════════════════════════
```
