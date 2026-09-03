# CHỐT CHUẨN GIAO DIỆN — GHI 15 QUYẾT ĐỊNH OWNER VÀO TÀI LIỆU

> **Ngày:** 23/08/2026 · **Loại:** DOC-ONLY 100% — **0 dòng mã ứng dụng bị sửa**
> **Actor:** Agent IDE · **Lớp:** `docs/` · `.governance/registry/` — chỉ tài liệu
> **Căn cứ:** Owner duyệt **11:13 23/08/2026**, quyết định `D-C1` → `D-D2` (15 quyết định), sau audit tổng lực `UI-AUDIT-TONG-LUC-20260823.md` (commit `<mã-nguồn-riêng>`)
> **Bàn giao:** Owner đọc → TanPhatAI đối chiếu 2 tầng

---

## 0) MỐC ĐO (cổng N0.1 — bài học sáng 23/08)

| Mục | Lúc bắt đầu (P0) | Lúc kết thúc |
|---|---|---|
| Nhánh | `main` | `main` |
| HEAD | `<mã-nguồn-riêng>` | `<mã-nguồn-riêng>` → **`<mã-nguồn-riêng>`** → **`<mã-nguồn-riêng>`** |
| `git log -1 -- docs/UI-STANDARD.md` | `<mã-nguồn-riêng>` · 18/08/2026 23:47:58 | `<mã-nguồn-riêng>` · 23/08/2026 |
| `git status --porcelain` | `?? scripts/tam/` (1 file untracked, **0 file tracked bị sửa**) | có việc của **phiên khác** — xem §5 |
| Số dòng `docs/UI-STANDARD.md` | 371 | **462** |
| Thời điểm | 06:49 | 11:56 |

**Quyết định tại cổng P0:** cây có 1 file untracked (`scripts/tam/d0-do.mjs`) nhưng **`git status --porcelain --untracked-files=no` → rỗng** ⇒ **0 file tracked bị sửa** ⇒ mốc đo vững, tiếp tục. File đó không do tôi tạo và không được đưa vào commit nào (dùng `git add` từng đường dẫn cụ thể, **không dùng `git add -A`**).

---

## 1) ĐÃ GHI GÌ VÀO TÀI LIỆU

### 1A. `docs/UI-STANDARD.md` — Doc `2.0` → `3.0` (371 → 462 dòng)

| Mục | Quyết định | Nội dung ghi vào |
|---|---|---|
| **header** | `D-C1` | Khối phạm vi áp dụng: giữ Sổ **#78**, **KHÔNG quét UI diện rộng**; trang cũ lệch = **nợ đã biết**, không phải lỗi mới. Thêm dòng trỏ tới bộ nghiệm thu |
| **§2** | `D-D2` / **N4** | **Khôi phục dòng màu `M9 slate→gray`** đã bị mất khi gộp 18/08. Đánh dấu **M4 = `OPEN-N2`** chờ Owner |
| **§5** | `D-Q1(c)` | Khối ranh giới: trang **MỚI** bắt buộc `PageHeader`; trang hiện hữu = **nợ `DEBT-083`**, vá khi đụng module |
| **§7** · **§11** | `D-Q7` | Vòng lấy nét ô nhập chốt **theo mã đang chạy**: `ring-2 ring-orange-500/10` (trước ghi `ring-[3px] ring-orange-400/10`) |
| **§8.1** *(mục MỚI)* | `D-T1` | **KHÔNG có cột thao tác trên dòng bảng.** Mọi hành động ở hero panel chi tiết. Ngoại lệ phải khai rõ kèm lý do nghiệp vụ |
| **§10** | `D-Q2(b)` | Bảng **2 kiểu panel có điều kiện**: kiểu A lưới `[minmax(0,1fr)_380px]` cho trang danh mục; kiểu B `Sheet` trượt cho hồ sơ nhiều trường. Cả hai vẫn phải giữ **nội dung panel GIÀU** |
| **§14** | `D-T2` | Bảng điểm gãy: `sm`=640 · `md`=768 · `lg`=1024 · `xl`=1280 |
| **§18** | `D-Q3(b)` · `D-T3` | Đang tải: **bắt buộc khi thao tác >300ms**, danh sách server-side **được miễn**. Lỗi **tách hai loại**: thao tác → `toast`; tải trang → khung đỏ + nút "Thử lại" |
| **§20.5** | `D-Q8(a)` · `D-Q9(a)` | Ranh giới vòng tròn bọc icon: **cấm trong badge/pill**, **được phép ở đầu thẻ mục**. **Giữ ngoại lệ emoji** ở avatar thẻ lưới |
| **§20.7-B** *(MỚI)* | `D-D2` | 4 xung đột **N1–N4** do chính việc gộp 18/08 sinh ra |
| **§20.7-C** *(MỚI)* | `D-Q4` | Họ component `standard-*` = **SUPERSEDED, ⛔ CẤM DÙNG MỚI**, kèm bảng 7 điểm chọi SSOT |
| **§21** | — | **9 hàng lịch sử**, mỗi hàng có lý do + mã quyết định + **nguyên văn dòng cũ** |

### 1B. `.governance/registry/ui-standard-sources.md` — `1.0` → `2.0`

Thêm mục **"BỔ SUNG 23/08/2026"** với **8 hàng S11–S18**. Nay **18/18 nguồn chuẩn giao diện đều có nhãn**.

| Nhãn | Nguồn |
|---|---|
| 🟢 **ACTIVE** | **S12** `mobile-responsive-ui-patterns` — nguồn **Owner đã duyệt 13/03/2026** mà trước nay không có nhãn (`D-Q10`) |
| 🔴 **SUPERSEDED** | **S11** họ `standard-*` (5 file, `D-Q4`) · **S13** `master-list-data-table` · **S14** `header-inline-badge` · **S15** `inline-filter-bar-layout` · **S16** `transactional-page-redesign` |
| 🟡 **REFERENCE** | **S17** `sticky-gradient-sheet-header` · **S18** `premium-module-page-redesign` |

**Cách phân loại 7 kỹ năng theo `D-D1`** — mặc định REFERENCE, chỉ gán SUPERSEDED khi có **quy tắc chọi trực tiếp SSOT**, kèm trích dòng chứng minh:

| Kỹ năng | Trích dòng chứng minh | Chọi mục nào |
|---|---|---|
| `master-list-data-table` | `:323` `rounded-2xl` · `:25`/`:340` `max-h-[60vh]` · `:47` *"nền đặc/opaque cho `<th>` sticky"* | §1 (cấm `rounded-2xl`) · §8 (`dvh`) · §2/§8 (`<th>` không set nền) |
| `header-inline-badge` | `:16` và `:30` `<h1 className="text-3xl font-bold">` | §5 (**CẤM `text-3xl`**) |
| `inline-filter-bar-layout` | `:23` `<Card className="p-4 border border-primary/20 bg-primary/5">` — thanh lọc là Card riêng **ngoài** bảng | §7 (thanh lọc **trong** card bảng) |
| `transactional-page-redesign` | `:4` và `:15` khai bố cục gồm *"row actions"* | **§8.1** mới (`D-T1`: không có cột thao tác) |
| `sticky-gradient-sheet-header` | `:23` `z-50` · `:24` `shadow-md` vs §11 `z-10`, không đổ bóng | **lệch nhỏ**, không phải quy tắc ngược → REFERENCE |
| `premium-module-page-redesign` | grep `rounded-*`/`text-*`/`gradient` → **0 kết quả**; là quy trình, không đặc tả | không chọi → REFERENCE |
| `mobile-responsive-ui-patterns` | grep `rounded-xl`/`rounded-2xl`/`text-3xl`/`vh` → **0/0/0/0** | không chọi → **ACTIVE** |

### 1C. `.governance/registry/tech-debt.md` — thêm **DEBT-083 → DEBT-088**

Mã cấp từ **083** (mã lớn nhất trước đó là **082**). **Không đụng 5 mã đang trùng** (`DEBT-030/031/032/066/067`) theo lệnh phiên — `DEBT-082` vẫn MỞ chờ Owner.

| Mã | Nội dung | Số đo |
|---|---|---|
| `DEBT-083` | `PageHeader` chưa phủ hết | **15/77** trang · **0/4** trang nền tảng |
| `DEBT-084` | `text-3xl` (SSOT §5 cấm) | **22 lần / 20 file**, cộng 4 trang gián tiếp qua `standard-list-page-header.tsx:27` |
| `DEBT-085` | Bó bề ngang trang | **3 file** — xem đính chính ở §3 |
| `DEBT-086` | Thiếu chỉ báo đang tải cho **thao tác** ở 4 trang nền tảng | `Loader2` = **0/4** |
| `DEBT-087` | `confirm()` gốc trình duyệt | **1 chỗ** — `mf/ke-toan/ke-toan-client.tsx:54` (thao tác **khoá sổ kế toán**, không đảo được) |
| `DEBT-088` | Họ `standard-*` đã SUPERSEDED nhưng còn được dùng | **6 lượt dùng**; `standard-data-table.tsx` **0 nơi dùng** (mồ côi) |

Cả 6 đều ghi rõ **"dọn khi đụng module, theo Sổ #78"** — không mở đợt quét diện rộng.

### 1D. `docs/OWNER-REQUEST-LEDGER.md` — thêm mục **#126 · #127 · #128**

- **#126** — Owner chốt 06:15 23/08: nguyên tắc **"chuẩn chỉnh trước"** cho giao diện (cắt vòng code-fix 4–5 lần).
- **#127** — **phán xử hai chuỗi bằng chứng** (xem §2 dưới).
- **#128** — Owner duyệt 11:13 23/08: toàn bộ 15 quyết định `D-C1` → `D-D2`, kèm điểm `OPEN-N2`.

### 1E. `docs/UI-ACCEPTANCE-CHECKLIST.md` — **MỚI**

Bộ nghiệm thu tái dùng, **57 tiêu chí**, dùng trọn **57/57**. Năm dòng từng chờ quyết định nay đã điền: **N4.1** (`D-Q1(c)`) · **N4.6** (`D-Q2(b)`) · **N6.2** (`D-Q3(b)`) · **N6.3** (`D-T3`) · **N8.4** (`D-T2`).

---

## 2) NHẮC LẠI PHÁN XỬ HAI CHUỖI BẰNG CHỨNG (ghi vào Sổ mục #127)

Bản 19/08 báo *"đã vá xong"*, khảo sát sáng 23/08 đo *"chưa vá"*. **Cả hai đều nói thật, ở hai thời điểm khác nhau:**

- Khảo sát sáng đo trên bản `docs/UI-STANDARD.md` **177 dòng** (`<mã-nguồn-riêng>`, 18/08 10:54) — bản **lỗi thời**.
- Lượt vá `<mã-nguồn-riêng>` (18/08 23:47) nằm trên **nhánh riêng** `gov/2026-08-18-rules-ui-standard-upgrade`, chỉ vào `main` ngày **22/08 lúc 11:45** qua commit hợp nhất `<mã-nguồn-riêng>`.
- Bản trong `main` khi khảo sát chạy là **371 dòng**; `git diff <mã-nguồn-riêng> HEAD -- docs/UI-STANDARD.md` → **TRỐNG**.

⇒ **Bài học đã đưa thành cổng `N0.1`:** trước khi kết luận một tài liệu còn thiếu / còn mâu thuẫn, **phải kiểm mốc kho** (`git log -1 -- <file>`); nếu ngày sửa cuối cũ hơn commit hợp nhất gần nhất thì phải tra `git log --all -- <file>`.

---

## 3) BA ĐÍNH CHÍNH DO CHÍNH LƯỢT NÀY TỰ PHÁT HIỆN

> Ghi ra vì cả ba đều là **số liệu sai của chính tôi**, và nếu để nguyên sẽ khiến phiên sau làm việc thừa.

### 3.1 Bộ nghiệm thu là **57** tiêu chí, không phải 55

Lệnh Owner ghi "55 tiêu chí" **và** yêu cầu thêm cổng `N0.5` (`D-T4`). Chuẩn mới `D-T1` cũng cần một dòng nghiệm thu riêng → đặt ở `N7.5`. `55 + 2 = 57`. Đếm máy xác nhận: `grep -cE "^\| \*?\*?N[0-9]\.[0-9]"` → **57**. Tôi ghi số đếm thật thay vì ép về 55.

### 3.2 `DEBT-085` — `mx-auto` vi phạm thật chỉ **3 file**, không phải "23 chỗ / 16 file"

Báo cáo audit ghi *"23 chỗ / 16 file"* — đó là **`grep "mx-auto"` thô**. Phân loại thủ công **23/23 dòng** tại commit `<mã-nguồn-riêng>`:

| Loại | Số | Ví dụ |
|---|---|---|
| ❌ **Vi phạm thật** — bó bề ngang trang | **3** | `m5/gia-vat-tu:227` · `m5/mua-hang:335` · `m5/ncc-vat-tu:211` — cả ba là `<div className="mx-auto max-w-300 space-y-6 p-6">`, **vi phạm 3 điều cùng lúc**: `mx-auto` + `max-w` + `p-6` (double-pad chồng gutter shell) |
| ✅ **Hợp lệ** — căn giữa icon/phần tử nhỏ | **20** | `<Package className="h-8 w-8 mx-auto mb-2" />` … |

Nếu dùng số 23, phiên sau sẽ đi "sửa" 20 chỗ vốn đúng. Đã ghi đính chính vào chính dòng `DEBT-085`. Cả 3 file vi phạm đều là **trang có bảng** — khớp đúng thứ tự ưu tiên Owner chốt ở `D-Q6`.

### 3.3 Thiếu sót `GOV-EDIT-PRESERVE-001`, đã tự bắt và vá

Sau commit `<mã-nguồn-riêng>`, bước tự kiểm phát hiện **tiêu đề §10 cũ** (`## 10) PANEL CHI TIẾT (in-grid, KHÔNG drawer overlay)`) và **dòng `- Grid:`** bị thay mà **chưa lưu nguyên văn** xuống §21 — vi phạm yêu cầu 1 của luật. Đã vá bằng commit `<mã-nguồn-riêng>`: thêm 2 hàng §21, mỗi hàng kèm lý do + mã quyết định + nguyên văn dòng cũ. **Kiểm lại: 11/11 dòng bị thay đều có nguyên văn trong §21.**

---

## 4) MỘT ĐIỂM 🔴 OPEN — CHỜ OWNER QUYẾT

### `OPEN-N2` — màu module **M4** có hai giá trị

| Nguồn | Giá trị | Ghi chú |
|---|---|---|
| `.cursor/skills/status-color-mapping/SKILL.md:226` | **`teal`** | bảng *"Module Header Colors"* |
| `.cursor/skills/module-color-palette/SKILL.md:24` | **`Purple` → `pink`** | bảng dải màu module |
| `src/components/foundation/page-header.tsx:15` | **`purple→pink`** | **mã đang chạy**, tự khai SSOT là `module-color-palette` |
| `docs/UI-STANDARD.md` §2 | **`purple→pink`** | SSOT |

**Agent KHÔNG tự chọn** — đúng lệnh `D-D2`. Đã ghi `OPEN-N2` vào SSOT §2 và §20.7-B.

⚠️ **Nhận xét trình Owner:** hai bảng có thể **đo hai thứ khác nhau** — `module-color-palette` là **dải màu gradient của module**, còn `status-color-mapping` §7 là **màu nhấn đầu trang theo loại trang**. Nếu đúng vậy thì đây **không phải xung đột**, mà là **thiếu định nghĩa hai khái niệm** — và cách xử lý sẽ là *đặt tên cho hai khái niệm* chứ không phải *chọn một bên*.

---

## 5) ⚠️ TÌNH HUỐNG NGOÀI DỰ KIẾN — MỘT PHIÊN KHÁC ĐANG LÀM VIỆC CÙNG LÚC

Tại P0 (06:49) cây làm việc chỉ có 1 file untracked. Đến lúc tự kiểm (11:53) xuất hiện thêm:

```
 M package.json
 M src/app/m0/security/actions.ts
 M src/app/m0/security/page.tsx
 M src/app/m0/security/security-client.tsx
 M src/lib/security-store.ts
?? .governance/acceptance/20260823-dot-D-ma-tran-tick-quyen-hanh-dong.md
?? scripts/tests/d-ma-tran-quyen-hanh-dong.test.ts
?? scripts/tests/d-chup-anh-ma-tran-quyen.ts
?? src/lib/security/action-registry.ts
```

Dấu thời gian 11:38 / 11:49 / 11:52 — đây là **ĐỢT D (ma trận tick quyền hành động)**, đúng module mà `D-C1` chỉ định làm đầu tiên.

**Cách tôi xử lý:**

1. **Không chạm một file nào của họ.** Dùng `git add` **từng đường dẫn cụ thể** cho đúng 5 file tài liệu, **không dùng `git add -A`**. Kiểm sau khi stage: `git diff --cached --name-only | grep -E "^src/|^package.json|^scripts/|^migrations/"` → **0 kết quả**.
2. **`.governance/acceptance/` KHÔNG được stage** — thư mục này nằm ngoài lớp được phép của tôi (`docs/` · `.governance/registry/` · `docs/reports/`), và là việc của họ.
3. **Một chồng lấn không tránh được, khai rõ:** họ cũng ghi vào `.governance/registry/tech-debt.md` (thêm `DEBT-089`, `DEBT-090`). Git đóng gói theo **trạng thái file**, không theo tác giả ⇒ commit `<mã-nguồn-riêng>` của tôi **có mang theo 2 dòng nợ đó**. Đã kiểm `git diff --numstat` → **9 thêm / 0 xoá**, **không dòng nào bị mất**.
4. **`DEBT-089` do họ ghi là về chính việc của tôi** — họ ghi lúc `UI-STANDARD.md` §0 đã trỏ tới `docs/UI-ACCEPTANCE-CHECKLIST.md` mà file **chưa tồn tại** (khoảnh khắc giữa lúc tôi sửa SSOT và lúc tôi tạo checklist). **File nay đã tồn tại** ⇒ `DEBT-089` **đã được giải quyết ngay trong lượt này**. Tôi **không sửa dòng nợ của phiên khác** — trình Owner để họ hoặc Owner đóng.

**Rủi ro còn lại cần Owner biết:** hai phiên cùng ghi vào một sổ nợ và cùng cấp mã tăng dần **không có cơ chế khoá** — đây đúng là cơ chế đã sinh ra 5 mã trùng (`DEBT-030/031/032/066/067`, `DEBT-082`). Lần này may mắn không trùng (tôi 083–088, họ 089–090), nhưng **là may, không phải do cơ chế**.

---

## 6) ĐIỂM GHI NHẬN — KHÔNG SỬA (đúng cổng dừng)

| # | Phát hiện | Vì sao không sửa |
|---|---|---|
| 1 | `standard-data-table.tsx` (11.370 byte) **0 nơi dùng**, mang tên *"standard"* nhưng chọi SSOT 5 điểm | Xoá/đổi tên là **việc CODE** — lệnh phiên cấm chạm `src/`. Đã gán nhãn + cấm dùng mới ở registry S11 và SSOT §20.7-C |
| 2 | `.governance/registry/tech-debt.md` dòng **70–71** có lỗi cấu trúc bảng (thiếu ô, thiếu dấu `\|`) | Lỗi **có sẵn** — kiểm `git diff --numstat` xác nhận tôi **thêm 9 / xoá 0**. Ngoài phạm vi lượt này |
| 3 | 5 mã `DEBT` đang trùng | Lệnh phiên **cấm đánh lại** — để phiên quản trị (`DEBT-082` vẫn MỞ) |
| 4 | Phiên ĐỢT D vừa thêm một `mx-auto` mới (`Loader2 className="mx-auto ..."`) | **Không phải vi phạm** — là căn giữa icon, đúng loại "hợp lệ" ở §3.2. Ghi ra để Owner không hiểu nhầm khi thấy số đếm nhảy |

---

## 7) TỰ KIỂM (P5) — KẾT QUẢ

| Kiểm | Kết quả |
|---|---|
| 5 file governance **không đổi** | ✅ hash trước = sau = `<mã-nguồn-riêng>` (cả 5 file, 1 hash duy nhất) |
| File đã chạm **100% trong `docs/` hoặc `.governance/registry/`** | ✅ đúng 5 file |
| **0 file `src/`** trong commit | ✅ `git diff --cached --name-only \| grep "^src/"` → 0 |
| 13 chuỗi quyết định mới tồn tại trong SSOT | ✅ `D-Q1(c)` `D-Q2(b)` `D-Q3(b)` `D-Q7` `D-Q8(a)` `D-Q9(a)` `D-T1` `D-T2` `D-T3` `20.7-B` `20.7-C` `OPEN-N2` `M9 slate→gray` — đều ≥1 |
| Tham chiếu bắt buộc tồn tại thật (`GOV-REF-EXISTS-001`) | ✅ `docs/UI-ACCEPTANCE-CHECKLIST.md` đã tồn tại, tên khớp dòng trỏ ở §0 |
| Bảo toàn nội dung (`GOV-EDIT-PRESERVE-001`) | ✅ **11/11** dòng bị thay đều có nguyên văn trong §21 *(sau khi vá bằng `<mã-nguồn-riêng>` — xem §3.3)* |
| Đếm tiêu chí nghiệm thu | ✅ **57**, khớp số ghi trong file |
| Pre-commit | ✅ **XANH** cả 2 lần: `test:secret-scan` PASS · `test:pii-scan` PASS · `test:script-parse` PASS |

---

## ═══════════ BÁO CÁO KẾT THÚC ═══════════

**1. ĐÃ LÀM**

- Ghi **15 quyết định Owner** (`D-C1` → `D-D2`) vào tài liệu, chạm đúng **5 file, 100% tài liệu, 0 file `src/`**.
- `docs/UI-STANDARD.md` lên Doc **`3.0`** (371 → 462 dòng): sửa/bổ sung **§2 §5 §7 §8.1 §10 §11 §14 §18 §20.5 §20.7-B §20.7-C §21** + 2 dòng trỏ ở đầu file. Thêm **§8.1** (không có cột thao tác trên dòng) và **§20.7-C** (cấm dùng mới họ `standard-*`) là hai mục hoàn toàn mới.
- Khôi phục dòng màu **M9 slate→gray** bị mất khi gộp 18/08 — đã kiểm giá trị đối lập theo yêu cầu `D-D2`: **không có**; hai nguồn khớp nhau (`module-color-palette:29` + `page-header.tsx:17` là mã đang chạy).
- `.governance/registry/ui-standard-sources.md` lên **`2.0`**: thêm **S11–S18**, nay **18/18 nguồn có nhãn**. Mỗi hàng SUPERSEDED đều kèm **trích dòng chứng minh** điểm chọi SSOT.
- `.governance/registry/tech-debt.md`: thêm **DEBT-083 → 088**, cấp từ 083 (mã lớn nhất trước đó 082), **không đụng 5 mã đang trùng**.
- `docs/OWNER-REQUEST-LEDGER.md`: thêm mục **#126 · #127 · #128**.
- Tạo mới `docs/UI-ACCEPTANCE-CHECKLIST.md` — **57 tiêu chí**, dùng trọn 57/57.
- **Tự phát hiện và sửa 3 sai số của chính mình**: số tiêu chí là 57 chứ không phải 55 · `DEBT-085` chỉ 3 file vi phạm chứ không phải 23 chỗ · thiếu lưu nguyên văn 2 dòng cũ của §10 (vá bằng commit riêng `<mã-nguồn-riêng>`).

**2. PHẠM VI**

- **ĐỤNG (5 file):** `docs/UI-STANDARD.md` · `docs/UI-ACCEPTANCE-CHECKLIST.md` *(mới)* · `docs/OWNER-REQUEST-LEDGER.md` · `.governance/registry/ui-standard-sources.md` · `.governance/registry/tech-debt.md`.
- **KHÔNG ĐỤNG:** `src/` **KHÔNG** (kể cả xoá/đổi tên `standard-data-table.tsx`) · `migrations/` KHÔNG · DB KHÔNG · deploy KHÔNG · version KHÔNG bump · **5 file governance KHÔNG** (hash trước = sau) · `.governance/acceptance/` KHÔNG (ngoài lớp được phép, là việc phiên khác) · 5 mã DEBT đang trùng KHÔNG đụng.

**3. BẰNG CHỨNG**

- `git hash-object` × 5 file governance, trước và sau → **cùng `<mã-nguồn-riêng>`** → `FILE_PROVEN`.
- `git diff --cached --name-only` → đúng 5 file; `grep -E "^src/|^package.json|^scripts/|^migrations/"` → **0 kết quả** → `FILE_PROVEN`.
- `git diff --numstat .governance/registry/tech-debt.md` → **9 thêm / 0 xoá** (6 của tôi + 3 của phiên khác, **không mất dòng nào**) → `FILE_PROVEN`.
- `git grep -n "mx-auto" <mã-nguồn-riêng> -- 'src/app/**client*.tsx'` → 23 dòng, **phân loại thủ công 23/23**: 3 vi phạm thật + 20 hợp lệ → `CODE_PROVEN`.
- `grep -cE "^\| \*?\*?N[0-9]\.[0-9]" docs/UI-ACCEPTANCE-CHECKLIST.md` → **57** → `FILE_PROVEN`.
- Pre-commit chạy thật 2 lần: `test:secret-scan` PASS · `test:pii-scan` PASS · `test:script-parse` PASS → `RUNTIME_PROVEN` cho phần cổng quản trị.
- ⚠️ **KHÔNG có `UI_PROVEN`** — lượt này không chạm mã, không chạy ứng dụng, không chụp ảnh. Mọi số đo giao diện là **đọc mã**, tính đến commit `<mã-nguồn-riêng>`.

**4. GHI SỔ YÊU CẦU OWNER**

- ✅ **ĐÃ GHI — mục #126 · #127 · #128** trong `docs/OWNER-REQUEST-LEDGER.md`, commit `<mã-nguồn-riêng>`.
- *(Đây là khoản nợ của phiên audit sáng nay: lượt đó bị lệnh phiên cấm sửa file trong kho mã nên trường 4 phải ghi "CHƯA". Nay đã ghi đủ cả ba mục còn nợ.)*

**5. PUSH BÁO CÁO CÔNG KHAI**

- ✅ **ĐÃ PUSH** — kho `Baocaoerptanphat` · nhánh `main` · file `UI-DOC-CHOT-CHUAN-20260823.md` · commit **`<mã-nguồn-riêng>`** (`<mã-nguồn-riêng>`).
- *(Bản này là lượt đẩy thứ hai, chỉ điền mã commit thật vào trường 5 — nội dung báo cáo không đổi.)*
- **Commit trong kho mã (docs-only):** `<mã-nguồn-riêng>` (chính) và `<mã-nguồn-riêng>` (vá bảo toàn §10). Cả hai đã push, `local = origin/main`.

**6. CÒN SÓT / CHƯA LÀM**

- `standard-data-table.tsx` và 4 component `standard-*`/`foundation` khác **vẫn nằm nguyên trong `src/`** — mới gán nhãn + cấm dùng mới, **chưa xoá/đổi tên** (việc CODE, `DEBT-088`).
- **6 nợ mới `DEBT-083`…`088` chưa dọn dòng mã nào** — đúng chủ đích, theo `D-C1` + Sổ #78.
- `DEBT-089` (do phiên khác ghi) **đã được giải quyết trên thực tế** nhưng **tôi không sửa dòng nợ của họ** — cần họ hoặc Owner đóng.
- Lỗi cấu trúc bảng ở `tech-debt.md` dòng **70–71** (có sẵn) — chưa vá, ngoài phạm vi.
- 5 mã `DEBT` trùng — chưa xử lý, lệnh phiên cấm.
- **Chưa có tiêu chí nào của bộ 57 được nghiệm thu thực tế** — bộ mới lập, chưa áp cho màn nào.
- Chưa đối chiếu chéo chi tiết S9/S10 như S1–S8 (`DEBT-009` vẫn MỞ).

**7. ĐANG CHỜ OWNER**

- **`OPEN-N2`** — màu module **M4** (`teal` vs `purple→pink`). Chặn: dòng **N2.5** của bộ nghiệm thu khi làm màn **M4**. Đề nghị Owner cân nhắc hướng "hai khái niệm khác nhau" ở §4 trước khi chọn một bên.
- **`DEBT-089`** — xác nhận đóng (file checklist nay đã tồn tại). Chặn: không chặn gì, chỉ để sổ nợ khỏi mang một dòng sai.
- **`DEBT-082`** (5 mã trùng) — vẫn chờ Owner quyết cách xử lý; rủi ro tái diễn còn nguyên vì hai phiên vẫn cùng ghi một sổ không có khoá.
- Có cho phép phiên sau **xoá `standard-data-table.tsx`** (0 nơi dùng) không.

**8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC**

Áp `docs/UI-ACCEPTANCE-CHECKLIST.md` cho **màn ma trận quyền** — đúng module mà `D-C1` chỉ định làm đầu tiên, và **phiên khác đang làm chính màn đó ngay lúc này**. Việc cụ thể: sao bộ 57 tiêu chí ra `docs/reports/UI-CHECKLIST-ma-tran-quyen-20260823.md`, chốt với Owner, rồi đối chiếu.

**9. CHƯA XÁC MINH ĐƯỢC**

- **Giao diện thật trên màn hình** — lượt này không chạy ứng dụng, không chụp ảnh. Ai xác minh: Agent IDE (chạy app + chụp 3 kích thước) hoặc Owner xem trực tiếp.
- **Bộ 57 tiêu chí có dùng được trơn tru trong thực tế không** — chưa áp cho màn nào. Ai: phiên kế tiếp khi làm màn ma trận quyền.
- **Ranh giới "đổi mạnh giao diện"** ở cổng N0.5 — tôi tự đề xuất định nghĩa (đổi bố cục · kiểu panel · cấu trúc header · bảng màu). Chỉ Owner xác nhận được là đúng ý.
- **`OPEN-N2` có thật là xung đột hay là hai khái niệm** — cần Owner hoặc người biết lịch sử hai bảng đó.
- **Việc của phiên ĐỢT D có xung đột gì với chuẩn vừa chốt không** — tôi chỉ đọc trạng thái file, **không đọc nội dung việc của họ** để tránh nhận định sai về công việc đang dở.

**10. TRẠNG THÁI CHUNG**

- ✅ **PASS** cho phạm vi được giao (P0 → P6): 15 quyết định đã vào tài liệu, 5 file commit sạch, 0 file `src/`, 5 file governance không đổi, pre-commit xanh, báo cáo đã push kèm mã commit.
- Còn **1 điểm `OPEN`** (`OPEN-N2`) — đúng cổng dừng, đã trình Owner thay vì tự chọn.
- Bộ nghiệm thu dùng được **57/57 dòng** ngay từ bây giờ.

**11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU**

- Phiên có bị nén ngữ cảnh không: **KHÔNG**.
- **Nhưng kho đã dịch chuyển trong lúc làm** — một phiên khác sửa 5 file `src/` và ghi thêm 2 dòng vào cùng sổ nợ tôi đang ghi. Đã xử lý bằng `git add` từng đường dẫn và kiểm lại trạng thái trước khi commit (§5).
- **Tài liệu tham chiếu đã đọc lại TỪ ĐĨA trong lượt này** (không dùng trí nhớ từ lượt audit trước): `docs/UI-STANDARD.md` (đọc lại từng đoạn §2 §5 §7 §8 §10 §11 §14 §18 §20.5 §20.7 §21 trước mỗi lần sửa) · `.governance/registry/ui-standard-sources.md` · `.governance/registry/tech-debt.md` · `docs/OWNER-REQUEST-LEDGER.md` · 7 file `SKILL.md` của các kỹ năng cần phân loại nhãn · `src/components/foundation/page-header.tsx` (chỉ đọc, để đối chứng `OPEN-N2` và M9).
- **Cảnh báo cho phiên sau:** chạy lại **N0.1** trước khi dùng bất kỳ số liệu nào trong báo cáo này. Kho đang có việc dở của phiên khác; HEAD lúc tôi kết thúc là **`<mã-nguồn-riêng>`**.

═══════════════════════════════════════════
