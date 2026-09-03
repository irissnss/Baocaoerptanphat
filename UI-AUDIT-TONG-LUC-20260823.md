# AUDIT TỔNG LỰC — TOÀN BỘ YÊU CẦU UI & CHUẨN GIAO DIỆN

> **Ngày:** 23/08/2026 · **Loại:** AUDIT READ-ONLY (0 dòng mã ứng dụng bị sửa)
> **Actor:** Agent IDE · **Lớp:** Local / Code / Git — CHỈ ĐỌC
> **Căn cứ:** Owner chốt 06:15 23/08 — *"giao diện không chuẩn thì code-fix tới lui 4–5 lần mỗi chỗ; phải CHUẨN CHỈNH TRƯỚC"*
> **Cách làm:** 13 tác nhân đọc song song theo nguồn (10 đào nguồn + 3 kiểm chứng đối chứng) · 336 lượt gọi công cụ · 2.304.224 token · 0 lỗi
> **Bàn giao:** Owner đọc → TanPhatAI đối chiếu 2 tầng → Owner duyệt BẢNG 3 → TanPhatAI khoá vào Notion

---

## 0) MỐC ĐO — VÀ MỘT CẢNH BÁO VỀ CHÍNH MỐC ĐO

| Mục | Giá trị |
|---|---|
| Nhánh | `main` |
| HEAD lúc **bắt đầu** audit | `5348619` — 23/08/2026 06:22:22 |
| HEAD lúc **kết thúc** audit | `<mã-nguồn-riêng>` — 23/08/2026 06:37:23 (`feat(m3): DOT C - hoan thien don hang`) |
| Thời điểm đo cuối | 23/08/2026 06:49:35 +07:00 |
| Cây làm việc | **SẠCH** (`git status --porcelain` → 0 dòng) ở cả hai mốc |

⚠️ **Kho đã dịch chuyển 1 commit NGAY TRONG lúc audit chạy.** Đã kiểm: `<mã-nguồn-riêng>` chỉ chạm 4 file M3 (`bao-gia/actions.ts`, `bao-gia-client.tsx`, `don-hang/actions.ts`, `don-hang-client.tsx`), **KHÔNG** chạm `docs/UI-STANDARD.md`, **KHÔNG** chạm registry, **KHÔNG** chạm `.cursor/skills/`.
→ Đã **đo lại toàn bộ số liệu mã tại `<mã-nguồn-riêng>`**: cả 5 chỉ số không đổi. Mọi kết luận dưới đây đúng cho cả hai mốc.

**Cách đọc bằng chứng:** mọi con số đều kèm lệnh đo. Lớp bằng chứng cao nhất đạt được trong phiên này là `FILE_PROVEN` / `CODE_PROVEN`. **KHÔNG có `UI_PROVEN`** — phiên read-only, không chạy ứng dụng, không chụp ảnh.

---

## A) ĐÍNH CHÍNH LỚN — HAI CHUỖI BẰNG CHỨNG CHỌI NHAU ĐÃ ĐƯỢC PHÂN XỬ

Đây là phát hiện quan trọng nhất của phiên. Bối cảnh prompt nêu hai chuỗi mâu thuẫn: **19/08 báo đã vá xong** vs **23/08 sáng khảo sát đo là chưa vá**. Đã truy nguyên dứt điểm.

### A1. Bản khảo sát sáng 23/08 đã đo trên MỘT BẢN LỖI THỜI

| Mốc | Ngày | Số dòng `docs/UI-STANDARD.md` |
|---|---|---|
| `<mã-nguồn-riêng>` | 11/08/2026 23:28 | 112 |
| `4230148` | 17/08/2026 21:02 | 176 |
| `<mã-nguồn-riêng>` | 18/08/2026 10:54 | **177** ← **bản mà khảo sát sáng đã đọc** |
| `<mã-nguồn-riêng>` | 18/08/2026 23:47 | **371** ← lượt vá D1–D4 |
| `<mã-nguồn-riêng>` | 22/08/2026 11:45 | 371 — *Merge branch `gov/2026-08-18-rules-ui-standard-upgrade`* |
| `<mã-nguồn-riêng>` (HEAD) | 23/08/2026 06:37 | **371** |

*Lệnh: `for c in …; do git show $c:docs/UI-STANDARD.md | wc -l; done`*

**Nguyên nhân gốc:** lượt vá `<mã-nguồn-riêng>` được làm trên **nhánh riêng** `gov/2026-08-18-rules-ui-standard-upgrade` và **chỉ vào `main` ngày 22/08 lúc 11:45** qua commit hợp nhất `<mã-nguồn-riêng>`. Bản khảo sát đo bản 177 dòng — tức bản **trước** khi nhánh được hợp nhất.

### A2. Ba kiểm chứng độc lập — kết quả

| # | Câu hỏi | Kết luận | Độ chắc chắn |
|---|---|---|---|
| **KC1** | Vá pill có nằm trong `main` không? | **CÓ.** `git merge-base --is-ancestor <mã-nguồn-riêng> HEAD` → CÓ. `git diff --stat <mã-nguồn-riêng> HEAD -- docs/UI-STANDARD.md` → **TRỐNG** ⇒ file không đổi một dòng nào từ 18/08 23:47 tới nay | CHẮC CHẮN |
| **KC2** | `docs/METRONIC_UI_RESEARCH_PROTOCOL.md` — chưa từng tồn tại / đã bị xoá / ở đường dẫn khác? | **CHƯA TỪNG TỒN TẠI.** Quét toàn bộ kho đối tượng git (456 commit / 7.591 đối tượng) → chỉ 3 kết quả chứa chữ "metronic", không có file này. `find` toàn ổ D: và `C:\Users\tantm` → 0 kết quả (đã xác minh cả hai đường dẫn đọc được, nên "rỗng" là kết quả thật) | CHẮC CHẮN |
| **KC3** | 11 kỹ năng đã thật sự được gộp chưa? | **NỬA ĐÚNG NỬA SAI** — xem §A4 | CHẮC CHẮN |

### A3. Bảy điểm mà khảo sát sáng 23/08 nay **KHÔNG CÒN ĐÚNG**

| Khảo sát sáng nói | Trạng thái thật trên `main` hôm nay | Bằng chứng |
|---|---|---|
| "Pill vẫn chọi §1/§6/§15" | ❌ **SAI** — cả **7 vị trí** đều `rounded-md` | dòng 41 · 46 · 95 · 101 · 169 · 173 · 176 |
| "SSOT không có mục định dạng ngày" | ❌ **SAI** — có **§17.1**, chốt `DD/MM/YYYY` | `docs/UI-STANDARD.md:186–199` |
| "SSOT không có mục định dạng số" | ❌ **SAI** — có **§17.2**, chốt `1.234.567,89` | `:201–214` |
| "Không nguồn nào quy định loading/lỗi/không-quyền" | ❌ **SAI** — có **§18**, đủ 4 trạng thái | `:216–226` |
| "16 kỹ năng không có nhãn hiệu lực" | ⚠️ **MỘT NỬA** — 11 kỹ năng đã gán nhãn ở registry; **7 kỹ năng UI vẫn KHÔNG có nhãn** | `.governance/registry/ui-standard-sources.md` hàng S8 |
| "Metronic là tham chiếu gãy còn hiệu lực" | ❌ **SAI** — đã gán **HISTORICAL** ở **3 nơi độc lập** | `ARCHIVE:162` · `legacy-rules-status.md:36` · `ui-standard-sources.md:24` |
| "Không có bảng phân xử nguồn" | ❌ **SAI** — có registry gán nhãn **10/10 nguồn** | `.governance/registry/ui-standard-sources.md` (51 dòng) |

> **Bài học rút ra (đề nghị nâng thành luật — xem §E):** khảo sát sáng làm đúng quy trình, đọc đúng file, nhưng **không kiểm mốc git trước khi đo**. Một lệnh `git log -1 -- <file>` đối chiếu ngày sửa cuối với ngày commit hợp nhất là đủ chặn. Đây chính là dạng lỗi mà `GOV-RELOAD-AFTER-COMPACT-001` nhắm tới nhưng chưa phủ: **luật hiện chỉ bắt đọc lại sau khi phiên bị nén, chưa bắt kiểm mốc kho trước khi kết luận.**

### A4. Nhưng claim 19/08 cũng **KHÔNG đúng hoàn toàn** — 4 lỗ chưa khai

Claim: *"đã gộp 11 kỹ năng vào SSOT"*. Kiểm chứng đọc toàn văn 16 SKILL.md + 371/371 dòng SSOT:

- **Số lượng:** đúng — 11 kỹ năng đều có dòng tham chiếu thật ở §20.1.
- **Phạm vi:** **thiếu** — grep tên trong SSOT: **9 có / 7 không**. Bảy kỹ năng UI (**1.293 dòng**) không được §20.1 nhắc tên: `master-list-data-table` (432) · `mobile-responsive-ui-patterns` (294) · `inline-filter-bar-layout` (197) · `transactional-page-redesign` (134) · `premium-module-page-redesign` (115) · `header-inline-badge` (65) · `sticky-gradient-sheet-header` (56).
- **Nội dung:** 2/16 gộp đủ cốt lõi · 6/16 gộp phần lớn · 4/16 gộp một phần nhỏ.
- **⚠️ 4 xung đột MỚI do chính việc gộp sinh ra, KHÔNG được khai ở bảng phân xử §20.7:**

| # | Kỹ năng gốc nói | SSOT viết thành | Vấn đề |
|---|---|---|---|
| N1 | `icon-style-guideline`: avatar trong thẻ lưới **được phép** dùng emoji (ngoại lệ duy nhất) | §20.5: *"Emoji trong UI — **cấm tuyệt đối**, kể cả trong dữ liệu"* | Đảo ngược ngoại lệ Owner đã duyệt 12/03/2026 mà không khai |
| N2 | `status-color-mapping` §7: **M4 Production = teal** | §2: **M4 = purple→pink** | Hai màu module khác nhau, chưa phân xử |
| N3 | `ui-components-usage` §7: Stats Card `rounded-2xl` | §1: *"**CẤM** `rounded-xl`/`rounded-2xl` trên card"* | Xung đột trực tiếp, chưa khai |
| N4 | `module-color-palette:29`: **M9 Portal = slate→gray** | grep `"M9"` trong SSOT → **0 kết quả** | Mất hẳn một dòng khi gộp |

- **⚠️ Một nguồn Owner-duyệt bị bỏ sót khỏi registry:** `mobile-responsive-ui-patterns/SKILL.md:17` ghi nguyên văn **"Approved by Owner: 13/03/2026"** và tự xưng SSOT — nhưng `grep` trong `ui-standard-sources.md` → **0 kết quả**. Theo quy tắc 5 của chính registry (*"Nguồn không có nhãn = không có hiệu lực"*), một nguồn **Owner đã duyệt** đang ở trạng thái vô hiệu lực về mặt thủ tục.

---

## B) SỐ ĐO TRẠNG THÁI THẬT (R1 – R5)

### B1. Nợ mã — đo tại `<mã-nguồn-riêng>`, đối chiếu khảo sát sáng

| Chỉ số | Lệnh đo | Khảo sát 23/08 | Đo lại `<mã-nguồn-riêng>` | Kết luận |
|---|---|---|---|---|
| `mx-auto` trong file client | `grep -rn "mx-auto" src/app --include=*client*.tsx` | 23 lần / 16 file | **23 / 16** | ✅ KHỚP |
| Trang dùng `PageHeader` | `grep -rln "PageHeader" src/app --include=*.tsx` | 15 / 77 | **15 / 77** | ✅ KHỚP |
| File có gradient 3 chặng chuẩn | `grep -rln "ea580c 0%, #f97316 30%, #fb923c"` | 15 | **15** | ✅ KHỚP |
| File có lưới `[minmax(0,1fr)_380px]` | `grep -rln "minmax(0,1fr)_380px"` | 1 | **1** | ✅ KHỚP |
| `rounded-xl` / `rounded-2xl` trong `src/app` | `grep -rn "rounded-xl\|rounded-2xl" src/app` | 0 | **0** | ✅ KHỚP |

→ **Số liệu mã của khảo sát sáng hoàn toàn chính xác.** Chỉ phần đánh giá **tài liệu** là lỗi thời.

### B2. Nợ mã mới đo được trong phiên này

| Chỉ số | Lệnh | Kết quả | Đối chiếu chuẩn |
|---|---|---|---|
| `text-3xl` trong `src/app` | `grep -rn "text-3xl" src/app --include=*.tsx` | **22 lần / 20 file** | §5 dòng 87 ghi **"CẤM text-3xl"** |
| Trang dùng `StandardListPageHeader` (chứa `text-3xl`) | `grep -rln "StandardListPageHeader" src/app` | **4 file** | cùng vi phạm §5 |
| `confirm()` gốc trình duyệt | `grep -rn "\bconfirm(" src/app --include=*.tsx` | **1** — `src/app/mf/ke-toan/ke-toan-client.tsx:54` | §18 ghi **"CẤM `alert()`/`confirm()`"** |
| `Loader2` / `animate-spin` / `Skeleton` | `grep -rn … src/app src/components` | **63 / 45 / 2** | §18 chốt `Loader2` là chuẩn — số liệu khớp lập luận của §18 |
| `PageHeader` ở **4 trang mẫu** | `grep -c "PageHeader"` từng file + cả `page.tsx` | **0 / 0 / 0 / 0** | §5 tiêu đề ghi **"HEADER TRANG (dùng `PageHeader`)"** |
| `Loader2`/`animate-spin` ở **4 trang mẫu** | `grep -c` từng file | **0 / 0 / 0 / 0** | §18 ghi trạng thái đang tải là **bắt buộc** |

### B3. Bộ luật — kiểm sức khoẻ

| Mục | Kết quả | Bằng chứng |
|---|---|---|
| Đồng bộ 5 bản luật (`GOV-FIVE-REPLICA-SYNC-001`) | ✅ **PASS** | `git hash-object` × 5 → cùng `<mã-nguồn-riêng>…`; `sha256sum` × 5 → cùng `<mã-nguồn-riêng>…`; 92.538 byte / 2.075 dòng mỗi bản |
| 18 đường dẫn tham chiếu bắt buộc | ✅ **18/18 tồn tại thật** | `ls` từng đường dẫn |
| Tiền đề của S8 (*Claude Code không đọc được `.cursor/skills/`*) | ✅ **Vẫn đúng hôm nay** | `ls -a .claude` → chỉ có `settings.local.json` |
| Sổ nợ kỹ thuật | 87 dòng nợ / **82 mã duy nhất** | **⚠️ 5 mã bị cấp trùng: `DEBT-030` `DEBT-031` `DEBT-032` `DEBT-066` `DEBT-067`** |
| Sổ Yêu Cầu Owner | 130 mục đánh số, cao nhất **#125**, 387 dòng | `grep -c "^| [0-9]"` |

### B4. R5 — bốn điểm khảo sát sáng bỏ dở

| Hạng mục | SSOT quy định gì | Thực tế 4 trang mẫu |
|---|---|---|
| **Sắp xếp cột** | ✅ **CÓ** — §8 dòng 116: nút sort `inline-flex items-center gap-1 hover:text-orange-100` + `ArrowUp/Down/UpDown h-3 w-3` | ✅ **4/4 đều có** (`ArrowUpDown`: 8 · 6 · 4 · 3 lần) |
| **Hành động trên dòng** | ❌ **KHÔNG CÓ QUY ĐỊNH** — grep "Thao Tác" trong SSOT chỉ ra §20.2 (bảng toggle) và §20.7 (nút hero), không có dòng nào quy định cột thao tác | **4/4 đều KHÔNG có cột thao tác.** `san-pham-client.tsx:1506` ghi nguyên văn: *"Thao Tác - ẨN (actions ở Detail Panel header)"* → **đây là quy ước thực tế, chưa được viết thành chuẩn** |
| **Responsive** | ⚠️ **CÓ MỘT PHẦN** — §14: bắt buộc `min-w-0`, `dvh`, `min-w-150`→`lg:min-w-215`, ẩn cột `hidden sm:table-cell`. **Nhưng KHÔNG có bảng liệt kê giá trị breakpoint** (sm/md/lg/xl là bao nhiêu px) | Chưa kiểm được bằng mắt (không chạy app) |
| **Đang tải / rỗng / lỗi / không quyền** | ✅ **CÓ** — §18 đủ 4 trạng thái, có bằng chứng code cho từng cái | ⚠️ **Đang tải: 0/4 trang mẫu có.** Lỗi: 0/4 có khung lỗi (cả 4 dùng `toast`). Rỗng: có. Không quyền: chặn ở tầng `page.tsx` |
| **Toggle dòng (§20.2 — bắt buộc)** | ✅ §20.2 | ✅ **4/4 đều có**: `san-pham:724` · `khach-hang:264` · `nhan-su:350` · `kho-thanh-pham:220` |

---

## BẢNG 1 — KHO YÊU CẦU UI CỦA OWNER

> Đã thu **537 yêu cầu** từ 10 nhóm nguồn. Bảng dưới trình **các yêu cầu có thẩm quyền cao nhất** (Sổ Owner + luật + sổ nợ) và **các yêu cầu chuẩn hoá cốt lõi**. Toàn bộ 537 mục thô kèm trích nguyên văn + `file:dòng` nằm trong dữ liệu phiên, cấp theo yêu cầu.
>
> Phân bố 537 mục theo hạng mục: `khác` 153 · `form` 61 · `panel` 40 · `màu` 36 · `icon` 24 · `header bảng` 22 · `pill` 21 · `bo góc` 20 · `hành động trên dòng` 19 · `khoảng đệm` 19 · `ngày/số` 18 · `empty-loading-error` 18 · `tìm kiếm` 17 · `responsive` 16 · `cỡ chữ` 15 · `title case` 13 · `phân trang` 10 · `vùng cuộn` 8 · `sắp xếp` 7.

### 1A. Yêu cầu Owner ra trực tiếp — nguyên tắc thường trực

| # | Nguồn | Ngày | Nội dung (1 câu) | Trạng thái hôm nay | Bằng chứng |
|---|---|---|---|---|---|
| 1 | OIL #13 | 10/08 | Chỉ cải tiến kỹ thuật/mượt mà responsive, **KHÔNG đổi mạnh giao diện** | ♾️ **ĐANG CÓ HIỆU LỰC** (nguyên tắc thường trực) | `OWNER-REQUEST-LEDGER.md:28` |
| 2 | OIL #14 | 10/08 | `/m5/kho-thanh-pham` là **CHUẨN tối ưu không gian** | ✅ ĐÃ ÁP DỤNG — SSOT §0 gọi là GOLD | `:40` · `UI-STANDARD.md:6` |
| 3 | OIL #15 | 10/08 | Owner không rành kỹ thuật → **dựng demo HTML có link** để Owner xem trước khi code | ⚠️ **CHƯA ÁP DỤNG có hệ thống** — không có cổng nào bắt | `:41` |
| 4 | OIL #71 | 18/08 | Bộ chuẩn UI theo **4 trang mẫu**; panel chi tiết phải **GIÀU**, không "sơ sài" | ✅ ĐÃ ÁP DỤNG cho `/m5/giao-hang` | `:259` |
| 5 | OIL #73 | 18/08 | Cần **1 tài liệu chuẩn UI chi tiết**; "thiếu bo góc nhẹ ở tất cả các điểm" | ✅ ĐÃ ÁP DỤNG — `docs/UI-STANDARD.md` 371 dòng, 21 mục | `:277` |
| 6 | OIL #75 | 18/08 | *"góc bo giống như hình dùm — **1 tiêu chuẩn 1**"* (pill header) | ✅ ĐÃ ÁP DỤNG — 7/7 vị trí trong SSOT nay thống nhất `rounded-md` | `:273` · SSOT 41/46/95/101/169/173/176 |
| 7 | OIL #72 | 18/08 | Toolbar tìm/lọc phải **nằm chung 1 card với bảng**; form phải có **header màu + section** | ✅ ĐÃ ÁP DỤNG — SSOT §7, §11 | `:262` |
| 8 | OIL #69/#70 | 18/08 | Dùng **toàn bề ngang**, không `max-w`/`mx-auto`; siết lề về mức shell | ⚠️ **ĐANG BỊ VI PHẠM** — còn **23 chỗ `mx-auto` / 16 file** | `:255,257` · số đo B1 |
| 9 | OIL #78 | 18/08 | **Đổi hướng: hoàn thiện từng chức năng module, đụng đâu xử lý đó** (dừng sweep UI) | ♾️ **ĐANG CÓ HIỆU LỰC** | `:267` |
| 10 | Sổ nợ (Owner chỉ đạo 22/08) | 22/08 | **KHÔNG đầu tư** trang hồ sơ người dùng · chuông thông báo · chế độ sáng-tối lúc này — dồn lực go-live | ⏸️ **HOÃN CÓ CHỦ ĐÍCH** | `tech-debt.md:82,83,84` (DEBT-063/064/065) |
| 11 | OIL #10 | 10/08 | Bo góc chip thống kê trang Khách Hàng **bớt tròn** ("đang tròn quá xấu") | ✅ ĐÃ ÁP DỤNG | `:21` · `khach-hang-client.tsx:379` |
| 12 | OIL #11 | 10/08 | Thêm **icon theo chủ đề** cho từng dòng khách hàng | ✅ ĐÃ ÁP DỤNG | `:21` |
| 13 | OIL #27/#28 | 11/08 | Board Thiết kế 6 cột phải **đa thiết bị**; gộp `/m8`+`/m8/tasks` vì "mở từng lớp rườm rà" | ✅ ĐÃ ÁP DỤNG | `:82,83` |
| 14 | OIL #59 | 16/08 | **Tinh giảm thao tác là tiêu chí duyệt** (3 bước → 1) | ♾️ **NGUYÊN TẮC THƯỜNG TRỰC** | `:222` |
| 15 | OIL #63 | 17/08 | Màn đơn phải hiện **số đặt / đã giao / chênh lệch** | ✅ ĐÃ ÁP DỤNG | `:240` |

### 1B. Chuẩn kỹ thuật cốt lõi — trạng thái thi hành

| # | Hạng mục | Chuẩn | Trạng thái | Bằng chứng |
|---|---|---|---|---|
| 16 | Bo góc pill thống kê header | `rounded-md` | ✅ SSOT nhất quán 7/7 vị trí | SSOT 41·46·95·101·169·173·176 |
| 17 | Cấm `rounded-xl`/`rounded-2xl` trên card | 0 lần | ✅ **ĐÃ ĐẠT** trong `src/app` | grep = 0 |
| 18 | Cấm `text-3xl` ở tiêu đề trang | 0 lần | ❌ **ĐANG BỊ VI PHẠM — 22 lần / 20 file** | grep |
| 19 | Không `max-w`/`mx-auto` trên shell trang | 0 lần | ❌ **ĐANG BỊ VI PHẠM — 23 chỗ / 16 file** | grep |
| 20 | Dùng `PageHeader` | mọi trang | ❌ **15/77 trang** — và **0/4 trang mẫu** | grep |
| 21 | Gradient đầu bảng 3 chặng | mọi bảng | ⚠️ **15 file** | grep |
| 22 | Lưới panel `[minmax(0,1fr)_380px]` | mọi trang list/detail | ❌ **1 file duy nhất** (`kho-thanh-pham`) | grep |
| 23 | Toggle dòng (§20.2) | bắt buộc | ✅ **4/4 trang mẫu có** | 724·264·350·220 |
| 24 | Trạng thái đang tải (§18) | bắt buộc | ❌ **0/4 trang mẫu có** | grep `Loader2` = 0 |
| 25 | Cấm `alert()`/`confirm()` (§18) | 0 lần | ❌ **1 chỗ** — `mf/ke-toan/ke-toan-client.tsx:54` | grep |
| 26 | Định dạng ngày `DD/MM/YYYY` (§17.1) | mọi nơi | ❌ **16 chỗ sai** (nợ đã khai `DEBT-007`) | SSOT §17.1 · `tech-debt.md:22` |
| 27 | Title Auto Case (§13) | mọi nhãn | ⚠️ **18/53 file client** | grep |
| 28 | Bộ hành vi form G2 (§11) | mọi form | ✅ **31 file** | grep `useFormDirtyTracker` |
| 29 | Icon `lucide-react` (§3) | mọi icon | ⚠️ **KeenIcon còn ở 9 file shell** (`DEBT-062`, `DEBT-081`) | `tech-debt.md:81,102` |
| 30 | Metronic HISTORICAL (Q3) | không phụ thuộc | ⚠️ **Mã còn phụ thuộc 12 file**, gồm chuỗi *"Powered by Metronic"* tại `sidebar.tsx:417` | `DEBT-081` |

### 1C. NHÓM RANH GIỚI — chưa phân loại được, cần Owner xác nhận

Các mục Sổ Owner có thể dính UI nhưng câu gốc không nói về giao diện: **#16** (bộ thông tin liên hệ — là dữ liệu, nhưng chỉ tồn tại để hiển thị) · **#22** (`can_thiet_ke` — hàm ý có ô bật/tắt trên form nhưng câu gốc chỉ nói cột CSDL) · **#34** (quy ước đánh số phiên bản — hiện ở chân trang) · **#54** (phép thử quản trị, kết quả "0 dòng mã") · **#81** (cấm cắt câm — chưa rõ có phải cảnh báo trên màn hình không) · **#96** (tách quyền duyệt giá — nói về QUYỀN, hệ quả UI có thật) · **#101** (gỡ 2 trang giả khỏi menu).

### 1D. NOT_CHECKED — khai rõ

| Mục | Lý do |
|---|---|
| Mọi hành vi hiển thị thật (`UI_PROVEN`) | Phiên READ-ONLY, không chạy ứng dụng, không chụp ảnh |
| Điểm gãy responsive thật ở 3 kích thước | Như trên — chỉ đọc được lớp class |
| Hình thức hiển thị của `toast` | Không đọc file component toast trong phiên này |
| Quy đổi HSL ↔ hex cho các cặp token không có chú thích | Ràng buộc read-only, không chạy được trình quy đổi màu |
| Nhánh UI giả còn sót trong 2 file client lớn nhất | Nợ đã khai `DEBT-035` — chưa đọc toàn văn 3.400 + 4.689 dòng |

---

## BẢNG 2 — MÂU THUẪN

> **224 điểm mâu thuẫn** thu được. Dưới đây là **các cặp có sức chặn thật**. Cột "đề xuất hoà giải" **chỉ là đề xuất** — Agent KHÔNG tự chọn bên thắng.

### 2A. Mâu thuẫn NẶNG — SSOT chọi chính 4 trang mẫu mà nó khai là nguồn gốc

| # | Hạng mục | Phía A — SSOT | Phía B — mã 4 trang mẫu | Đề xuất hoà giải |
|---|---|---|---|---|
| **M1** | Header trang | §5 tiêu đề: *"HEADER TRANG (**dùng `PageHeader`**)"*, icon badge `h-10 w-10 rounded-md` — `page-header.tsx:48` đúng vậy | **0/4 trang mẫu dùng `PageHeader`** (kể cả `page.tsx`). GOLD `kho-thanh-pham:241` tự dựng header, icon `rounded-lg` | Hoặc (a) sửa 4 trang mẫu về `PageHeader`, hoặc (b) hạ §5 xuống "khuyến nghị" và ghi rõ mẫu tự dựng cũng hợp lệ. **Không thể để nguyên** — 100% trang mẫu trái chuẩn |
| **M2** | Lưới panel chi tiết | §10: `xl:grid-cols-[minmax(0,1fr)_380px]`, panel **380px cố định** | `kho-thanh-pham:273` đúng · `san-pham:1248` `lg:grid-cols-3` · `khach-hang:404` `lg:grid-cols-3` (luôn bật) · `nhan-su` dùng **Sheet trượt** | Chốt 1 trong 3 kiểu, hoặc cho phép 2 kiểu kèm điều kiện dùng |
| **M3** | Nút hành động chính | §12: `<Button>`, `rounded-md`, `h-8 sm:h-9` | GOLD `kho-thanh-pham:249`: `<button>` thô, `rounded-lg px-3.5 py-2` | Chốt một bên |
| **M4** | Trạng thái đang tải | §18: **bắt buộc** `Loader2` + `animate-spin` | **0/4 trang mẫu có** (grep = 0 cả 4) | Hoặc bổ sung vào 4 trang mẫu, hoặc hạ §18 xuống "khi thao tác > 300ms" |
| **M5** | Tiêu đề trang | §5: **"CẤM `text-3xl`"** | `text-3xl` còn **22 lần / 20 file**, và `StandardListPageHeader` (dùng ở 4 trang) hard-code `text-3xl` | Dọn mã, hoặc khai là nợ có hạn đóng |

### 2B. Mâu thuẫn NẶNG — họ component `standard-*` chưa từng được đưa vào bảng phân xử

> **Phát hiện mới của phiên này.** Tồn tại một họ component mang tên *"standard"* trong `src/components/ux/` và `src/components/foundation/` **chọi SSOT có hệ thống**, và **chưa nguồn nào gán nhãn**.

| # | Hạng mục | Phía A — component | Phía B — SSOT | Ghi chú |
|---|---|---|---|---|
| **M6** | Nền đầu bảng | `standard-data-table.tsx:134` — `<th>` **tự đặt nền xám** `--tp-table-header-bg` (#f9f9f9), chữ xám; **không có gradient trên `<tr>`** | §2 + §8: gradient 3 chặng trên `<tr>`, `<th>` **không** set nền, chữ **trắng** | §20.7 mục 8 đã phân xử SSOT thắng — nhưng component vẫn nguyên |
| **M7** | Vùng cuộn | `standard-data-table.tsx:124` — `max-h-[60vh]` (đơn vị `vh`) | §8: `max-h-[max(240px,calc(100dvh-280px))]`; §20.7 mục 3 chốt `dvh` thắng `vh` | Như trên |
| **M8** | Bo góc khung bảng | `standard-data-table.tsx:123` — `rounded-lg` (8px) | §8: `rounded-md` (6px) + `bg-white shadow-sm` | — |
| **M9** | Trạng thái đang tải của bảng | `standard-data-table.tsx:158` — khung xương `animate-pulse`, 5 dòng giả | §18: `Loader2` là chuẩn, *"`Skeleton` chỉ 2 chỗ → **KHÔNG phải chuẩn**"* | — |
| **M10** | Vị trí thanh lọc | `standard-filter-toolbar.tsx:66` — thẻ **riêng ngoài bảng**; `foundation/filter-bar.tsx:12` cũng là khung lọc độc lập | §7: thanh lọc **nằm TRONG card bảng** (con đầu, `border-b`); §20.7 mục 10: Owner yêu cầu đích danh (Sổ #72) | — |
| **M11** | Tiêu đề trang | `standard-list-page-header.tsx:27` — `text-3xl` chữ chuyển sắc trong suốt | §5: **"CẤM text-3xl"** | Đang dùng ở **4 trang** |
| **M12** | Thẻ số liệu | `foundation/stat-card.tsx` — icon 32px, số 24px, `shadow-none`, không thanh accent | §6 Variant B: icon `h-10 w-10 rounded-lg`, số `text-xl tabular-nums`, có thanh accent gradient | Đang dùng ở **4 trang** |

> ⚠️ **`standard-data-table.tsx` (11.370 byte) hiện KHÔNG được trang nào dùng** (`grep -rln "StandardDataTable" src/app` → **0**). Nhưng nó **mang tên "standard"** — người/agent sau rất dễ nhặt lên dùng và dựng ra một trang trái chuẩn toàn diện. Đây là **bẫy đặt sẵn**.

### 2C. Mâu thuẫn NỘI BỘ trong chính SSOT

| # | Hạng mục | Vị trí A | Vị trí B | Ghi chú |
|---|---|---|---|---|
| **M13** | Vòng lấy nét ô nhập | §7 dòng 108 + §11 dòng 145: `focus:ring-[3px] ring-orange-400/10` | §12 dòng 153: `ring-2 ring-orange-500/10` | Mã (`input.tsx:14`) khớp §12, lệch §7/§11 |
| **M14** | Icon bọc vòng tròn màu | §20.5 dòng 299: *"Icon lucide đặt **trực tiếp** trong badge/pill, **KHÔNG** bọc thêm vòng tròn màu"* | §1 dòng 44 + §10 dòng 134: thẻ mục có `circle h-5.5 w-5.5 rounded-full` **bọc icon** | Ranh giới ngữ cảnh không rõ (badge/pill vs đầu thẻ mục) |
| **M15** | Số bản ghi mặc định | §9: *"Default 10 (M1) / 25 (kho)"* | `master-list-page-template` checklist: `perPage=25` | Lệch chưa khai ở §20.7 |

### 2D. Mâu thuẫn TÀI LIỆU ↔ MÃ (ngoài 4 trang mẫu)

| # | Hạng mục | Phía A — mã | Phía B — SSOT |
|---|---|---|---|
| **M16** | Token màu brand | Hai hệ token song song không trỏ sang nhau: `design-tokens.css:11` hex `#F97316` ↔ `globals.css:20,22` HSL `21.7 89.1% 53.9%`; chính `globals.css:55` chú thích cụm HSL của `#f97316` là `24.6 94.4% 53.1%` — **hai bộ ba số khác nhau** | §2: một bảng màu hex duy nhất |
| **M17** | Token chiều cao | `design-tokens.css:48–55` khai `--tp-input-h-*`, `--tp-btn-h-*` — nhưng `--tp-input-h-*` **0 lần dùng**, `--tp-btn-h-*` **1 lần** | §12 chép theo component, không theo token |
| **M18** | Bo góc `Card` | `ui/card.tsx:12` — `rounded-lg` | §1: card = `rounded-md` |
| **M19** | Biến thể Button | `ui/button.tsx:11-27` — **6 biến thể, 4 cỡ** (có `secondary`, `link`, `lg h-11 px-8`) | §12 chỉ liệt kê **4 biến thể, 3 cỡ** — thiếu 3 |
| **M20** | Bề rộng Sheet | `ui/sheet.tsx:34,43` — mặc định `p-6`, `sm:max-w-sm` (384px) | §11: `w-full p-0`, `sm:max-w-120` (480px) |
| **M21** | Viền FormSection | `form-section.tsx:92` — `border-[#e1e7ef]` | §2: border `#E2E8F0` |
| **M22** | Chú thích `PremiumSection` | `premium-section.tsx:19` chú thích ghi *"bo góc 2XL"*, dòng 35 thực thi `rounded-md` | §1 cấm `rounded-2xl` — **mã đúng, chú thích sai** |

### 2E. Mâu thuẫn về HIỆU LỰC NGUỒN

| # | Vấn đề | Bằng chứng A | Bằng chứng B | Đề xuất |
|---|---|---|---|---|
| **M23** | 7 kỹ năng UI (1.293 dòng) **không có nhãn** ở registry | `grep` 7 tên trong `ui-standard-sources.md` → **0/7** | Registry quy tắc 5: *"Nguồn không có nhãn = không có hiệu lực"* | Bổ sung 7 hàng vào registry |
| **M24** | Một nguồn **Owner đã duyệt** không có nhãn | `mobile-responsive-ui-patterns/SKILL.md:17`: *"Approved by Owner: 13/03/2026"*, tự xưng SSOT | Không có trong registry | Owner xác nhận còn hiệu lực hay hạ nhãn |
| **M25** | 7/8 kỹ năng **tự xưng "SSOT"** ngay trong thân file | `master-list-page-template:6` · `icon-style-guideline:6` · `ui-typography:7` · `status-color-mapping:15` … | Nhãn hạ hiệu lực **chỉ tồn tại BÊN NGOÀI file** (registry + SSOT §20) | Ai mở file kỹ năng ra đọc sẽ thấy nó tự xưng SSOT mà không thấy nhãn |
| **M26** | 4 xung đột do việc gộp sinh ra, **chưa khai** ở §20.7 | N1–N4 ở §A4 | §20.7 khai 11 xung đột | Bổ sung 4 hàng vào §20.7 |
| **M27** | Metronic: luật đã hạ nhãn nhưng **mã còn phụ thuộc** | Q3 18/08 → HISTORICAL ở 3 nơi | `DEBT-081`: 12 file còn phụ thuộc, gồm *"Powered by Metronic"* hiển thị ở `sidebar.tsx:417` | Đã ghi sổ, chờ phiên dọn sau go-live |
| **M28** | 5 mã sổ nợ bị **cấp trùng** | `DEBT-030` `DEBT-031` `DEBT-032` `DEBT-066` `DEBT-067` | 87 dòng / 82 mã duy nhất | Đánh lại số hoặc thêm hậu tố |

### 2F. Mâu thuẫn giữa YÊU CẦU OWNER với nhau qua thời gian

| # | Sớm hơn | Muộn hơn | Trạng thái |
|---|---|---|---|
| **M29** | OIL #71/#73/#77 (18/08): đang đồng bộ UI theo chuẩn, #77 khai nợ *"panel in-grid + hero giàu + header icon-badge cho nhóm chứng từ"* | **OIL #78 (18/08, muộn hơn): "chuyển sang hoàn thiện từng chức năng module, đụng chỗ nào xử lý tới đó"** | ⚠️ **Quyết định muộn hơn đang chặn việc quét UI diện rộng** |
| **M30** | OIL #13 (10/08): *"chỉ cải tiến kỹ thuật/mượt, **không đổi mạnh giao diện**"* | Việc sửa M1/M2 (đổi `PageHeader`, đổi lưới panel) **là đổi mạnh** | Cần Owner cho phép đích danh từng điểm |
| **M31** | Chuẩn chung: dùng `lucide-react` | Sổ nợ 22/08: Owner chỉ đạo **KHÔNG đầu tư** dọn icon lúc này (`DEBT-065`) | Nhất quán — nợ có chủ đích, không phải vi phạm |

---

## BẢNG 3 — BẢNG GIÁ TRỊ CHUẨN ĐỀ XUẤT (Owner chốt 1 lần)

> Ô **✅ ĐÃ RÕ** = SSOT đã chốt, có bằng chứng, không xung đột → đề nghị Owner **duyệt nguyên trạng**.
> Ô **🔴 CHỜ OWNER** = có xung đột thật → kèm tối đa 3 phương án, ưu/nhược.

### 3A. Các ô ĐÃ RÕ — đề nghị duyệt nguyên trạng (28 ô)

| Hạng mục | Giá trị chuẩn | Nguồn | Cách kiểm |
|---|---|---|---|
| Thang bo góc | `rounded-md`=6px · `rounded-lg`=8px · `rounded-full`=tròn. **CẤM** `rounded-xl`/`rounded-2xl` trên card | §1:25 | `grep -rn "rounded-xl\|rounded-2xl" src/app` → phải = 0 |
| Card/khung bảng/panel/hero/stat card | `rounded-md` + `overflow-hidden` | §1:29,30,36 | grep class |
| Hero panel · header form | `rounded-t-md` | §1:31 | grep |
| `th` đầu/cuối bảng | `rounded-tl-md` / `rounded-tr-md` | §1:35 | grep + ảnh |
| **Pill thống kê header** | `rounded-md px-2 sm:px-3 py-0.5 sm:py-1 font-medium` · nền `bg-{c}-50/80` · viền `border-{c}-200` · số `font-bold` · icon `text-{c}-500` | §1:41 · §6:95 · nguồn chân lý `khach-hang-client.tsx:379` | grep + ảnh |
| Badge trạng thái **trong hàng bảng** | `rounded-full` | §1:42 | grep |
| Ô nhập / tìm kiếm / select lọc | `h-9 rounded-lg` | §1:38 · §12:153 | grep |
| Avatar / icon-circle | `rounded-full` | §1:44 | grep |
| Màu brand | `#F97316` · hover `#EA580C` · active `#C2410C` · surface `#FFF7ED` | §2:54 | grep hex |
| Gradient đầu bảng | `linear-gradient(135deg, #ea580c 0%, #f97316 30%, #fb923c 100%)` đặt trên `<tr>`; `<th>` **không** set nền | §2:55 · §8:116 | grep chuỗi hex |
| Màu trạng thái | chờ/nháp=amber · đang chạy=blue · xong/duyệt=emerald · huỷ/lỗi=rose · nghỉ=slate | §2:56 | đọc mã |
| Icon theo trạng thái | nháp `FileText` · đang xử lý `Send` · hoàn thành `CheckCircle` · huỷ `XCircle` · chờ `Clock` · khác `AlertCircle` | §20.4 | đọc mã |
| Hover/chọn dòng | hover `bg-[#fff7ed]`; chọn thêm `boxShadow: inset 3px 0 0 #f97316` | §2:59 | grep |
| Cỡ chữ ô bảng | `text-[12.5px]`, `td px-4 py-2.5` | §8:115,117 | grep |
| Đầu bảng | `text-[11px] font-bold text-white uppercase tracking-[0.04em]`, `sticky top-0 z-30 h-10 px-4` | §8:116 | grep |
| Thư viện icon | `lucide-react` (nét) — không dùng bộ khác | §3 | grep |
| Cỡ icon | nút/ô/nhãn phụ `h-4 w-4` · trạng thái lớn `h-5 w-5` · trong vòng tròn thẻ mục `h-3 w-3` · nút tròn hero `h-3.5 w-3.5` | §3 | grep |
| Vùng cuộn bảng | **1 lớp** `max-h-[max(240px,calc(100dvh-280px))] overflow-auto`; đơn vị `dvh` không `vh`; **cấm lồng 2 lớp** | §8:114 · §20.7 #3 | grep |
| Bề rộng bảng | `min-w-150` → `lg:min-w-215`; `min-w-0` bắt buộc trên child grid chứa bảng | §14:159,162 | grep |
| Thanh lọc | **nằm TRONG card bảng**, con đầu, `border-b border-gray-100 px-3 sm:px-4 py-2.5` | §7:104 · §20.7 #10 | ảnh |
| Ô tìm kiếm | `relative flex-1 min-w-45 sm:max-w-[320px]` + icon `Search h-4 w-4 text-gray-400` + input `pl-9` | §7:108 | grep |
| Tìm kiếm bỏ dấu | `toLowerCase()` + `normalize("NFD")` + xoá dấu `[\u0300-\u036f]` + `trim()` | Archive §Search Normalization | đọc mã |
| Phân trang | chân trong card, `border-t`; trái "Hiển thị {n}/{total} bản ghi"; giữa nút số `w-7 h-7 rounded-md text-[11px]`, đang chọn nền `#f97316`; phải ô chọn 10/25/50 | §9:122–126 | ảnh |
| **Ngày hiển thị** | **`DD/MM/YYYY`**; ngày+giờ `DD/MM/YYYY HH:mm`; ô nhập giữ ISO `YYYY-MM-DD`; **cấm** render thẳng đối tượng `Date` ra JSX | §17.1 | grep + ảnh |
| **Tiền & số** | `1.234.567,89` — chấm ngăn nghìn, phẩy thập phân; dùng `toLocaleString("vi-VN")`; cột số `text-right font-mono tabular-nums`; cột Tổng Tiền `font-bold text-orange-600` | §17.2 | ảnh |
| **Trạng thái rỗng** | `py-12 text-center text-sm text-muted-foreground` + icon mờ + **một câu gợi hành động** | §8:118 · §18 | ảnh |
| **Không có quyền** | điều hướng sang `/403`, **không tự vẽ lại**; ẩn nút/cột theo quyền thay vì hiện rồi báo lỗi | §18 · `403/page.tsx` | ảnh |
| **Toggle dòng** | bấm lại đúng dòng đang chọn → **ĐÓNG** panel | §20.2 | thao tác + ảnh |
| Chọn danh mục (combobox) | hiện **TÊN**, gửi đi **ID**; ≥10 lựa chọn phải có ô tìm kiếm bỏ dấu; nguồn từ server, **cấm dữ liệu giả** | §19 | đọc mã |
| Chữ hiển thị | Title Auto Case qua `toVietnameseTitleCase()`; **không** áp cho mã/ID/email/SĐT; **không** mutate dữ liệu gốc | §13 | grep + ảnh |
| Hành vi form (G2) | `useFormDirtyTracker` + `useSafeClose` + `ConfirmExitDialog` + `ConfirmActionDialog variant="delete"`; chặn đóng ngoài/Esc khi dirty | §11 | grep |
| Form Sheet | `w-full p-0 flex flex-col sm:max-w-120`; header chuyển sắc sticky `rounded-t-md`; chia `FormSection colorTheme`; chân sticky Hủy+Lưu | §11 | ảnh |
| Panel chi tiết — nội dung | hero name-on-top + 3 nút tròn `h-8 w-8` (Sửa/Xóa/Đóng) + badges; ≥2 thẻ mục màu; stat box; chân audit | §10 | ảnh panel MỞ |
| Audit trên dòng | icon `History` cạnh mã, bấm phải `e.stopPropagation()`, mở Dialog "Thông Tin Audit" read-only 4 trường | §20.6 | thao tác |
| Cấm | `alert()` · `confirm()` gốc trình duyệt · emoji trong UI · chấm tròn đặc (trừ chỉ báo trạng thái) | §18 · §20.5 | grep |

### 3B. Các ô 🔴 CHỜ OWNER — 9 quyết định

| # | Hạng mục | Phương án | Ưu | Nhược |
|---|---|---|---|---|
| **Q1** | **Header trang: `PageHeader` hay tự dựng?** (M1) | **(a)** Bắt buộc `PageHeader`, sửa 4 trang mẫu | 1 chuẩn 1, đúng §5 | Đổi mạnh 4 trang mẫu — chọi OIL #13 |
| | | **(b)** Hạ §5 xuống "khuyến nghị", công nhận cả header tự dựng | Không đụng mã đang chạy | Vẫn "mỗi nơi mỗi kiểu" — đúng thứ Owner muốn hết |
| | | **(c)** Giữ §5 làm chuẩn cho **trang mới**, trang cũ ghi nợ có hạn đóng | Cân bằng, không chặn go-live | Nợ kéo dài |
| **Q2** | **Lưới panel chi tiết** (M2) | **(a)** `[minmax(0,1fr)_380px]` cho mọi trang (theo GOLD) | Panel không phình ở màn lớn | Sửa 3/4 trang mẫu |
| | | **(b)** Cho phép 2 kiểu: 380px cho danh mục, Sheet trượt cho hồ sơ nhiều trường | Hợp thực tế `nhan-su` | Phải viết rõ điều kiện dùng |
| | | **(c)** Giữ nguyên, chỉ áp cho trang mới | Rẻ nhất | Không giải quyết được "so với mẫu" |
| **Q3** | **Trạng thái đang tải** (M4) | **(a)** Bắt buộc `Loader2` mọi trang list | Đúng §18 | 0/4 trang mẫu đang có → việc lớn |
| | | **(b)** Chỉ bắt buộc khi thao tác > 300ms; danh sách tải server-side thì miễn | Sát kiến trúc Server Components hiện tại | Cần định nghĩa rõ ranh giới |
| **Q4** | **Họ component `standard-*`** (M6–M12) | **(a)** Sửa cả họ về đúng SSOT | Hết bẫy | Chạm 5 file dùng chung, rủi ro hồi quy |
| | | **(b)** Gán nhãn SUPERSEDED + đổi tên thành `legacy-*`, cấm dùng mới | Rẻ, chặn bẫy ngay | Mã cũ vẫn lệch |
| | | **(c)** Xoá `standard-data-table.tsx` (0 nơi dùng), 4 cái còn lại gán nhãn | Gọn nhất cho cái mồ côi | Cần Owner cho phép xoá |
| **Q5** | **`text-3xl`** (M5, 22 lần/20 file) | **(a)** Dọn hết về `text-xl sm:text-[22px]` | Đúng §5 | 20 file |
| | | **(b)** Ghi nợ có hạn đóng, dọn sau go-live | Không chặn go-live | Chuẩn tiếp tục bị vi phạm |
| **Q6** | **`mx-auto`** (23 chỗ/16 file) | **(a)** Dọn hết | Đúng §4, đúng OIL #69/#70 | 16 file |
| | | **(b)** Dọn trang có bảng trước, trang form sau | Ưu tiên chỗ Owner nhìn thấy | Làm 2 đợt |
| **Q7** | **Vòng lấy nét ô nhập** (M13) | **(a)** `ring-2 ring-orange-500/10` (theo mã thật) | Không đụng mã | Sửa §7 + §11 |
| | | **(b)** `ring-[3px] ring-orange-400/10` (theo §7/§11) | Theo tài liệu | Sửa `input.tsx` |
| **Q8** | **Icon bọc vòng tròn màu** (M14) | **(a)** Cấm chỉ trong badge/pill; đầu thẻ mục **được phép** — làm rõ ranh giới trong §20.5 | Khớp mã GOLD đang chạy | Phải sửa câu chữ |
| | | **(b)** Cấm tuyệt đối, sửa §10 bỏ vòng tròn | Nhất quán tuyệt đối | Đổi mạnh panel — chọi OIL #13/#71 |
| **Q9** | **Emoji trong avatar thẻ lưới** (N1) | **(a)** Giữ ngoại lệ Owner duyệt 12/03/2026 | Tôn trọng quyết định cũ | §20.5 phải sửa lại |
| | | **(b)** Cấm tuyệt đối như §20.5 hiện ghi | Đơn giản | Đảo ngược quyết định Owner mà chưa khai |

### 3C. Ô CÒN TRỐNG — chuẩn chưa từng tồn tại, cần Owner cấp mới

| # | Hạng mục | Thực tế đang chạy | Đề xuất |
|---|---|---|---|
| **T1** | **Hành động trên dòng** | 4/4 trang mẫu **không** có cột thao tác; `san-pham:1506` ghi rõ *"Thao Tác - ẨN (actions ở Detail Panel header)"* | Viết thành chuẩn: **không có cột thao tác; mọi hành động nằm ở hero panel chi tiết**. Ngoại lệ (nếu có) phải khai |
| **T2** | **Bảng giá trị breakpoint** | §14 dùng `sm`/`lg`/`xl` nhưng **không nơi nào liệt kê giá trị px** | Chốt bảng: `sm`=640 · `md`=768 · `lg`=1024 · `xl`=1280 (mặc định Tailwind) và ghi vào §14 |
| **T3** | **Trạng thái lỗi** | 4/4 trang mẫu dùng `toast`, **không** trang nào có khung lỗi như §18 mô tả | Chốt: `toast` cho lỗi thao tác · khung đỏ §18 cho lỗi tải trang. Hoặc bỏ khung đỏ khỏi §18 |
| **T4** | **Quy trình demo HTML trước khi code** | OIL #15 yêu cầu, **không cổng nào bắt** | Thêm vào bộ tiêu chí nghiệm thu (BẢNG 4 mục N0) cho việc "đổi mạnh giao diện" |

---

## BẢNG 4 — BỘ TIÊU CHÍ NGHIỆM THU UI TÁI DÙNG

> **Mục tiêu:** sau khi Owner duyệt BẢNG 3, mọi việc UI về sau **chỉ áp bảng này** — hết vòng sửa 4–5 lần.
> **Cách dùng:** sao bảng ra file `docs/reports/UI-CHECKLIST-<màn>-<ngày>.md` **TRƯỚC khi sửa dòng mã đầu tiên** (`GOV-ACCEPTANCE-FIRST-001`), đánh dấu từng dòng sau mỗi màn.
> **XONG = đạt hết mọi dòng áp dụng.** Không phải "trông ổn rồi".

### N0 — CỔNG TRƯỚC KHI VIẾT MÃ (4 dòng, làm trước tiên)

| # | Tiêu chí | Cách kiểm | Bằng chứng cần có |
|---|---|---|---|
| N0.1 | **Kiểm mốc kho trước khi đo** — ghi nhánh · HEAD · ngày sửa cuối của `docs/UI-STANDARD.md` · cây sạch hay bẩn | `git rev-parse HEAD` · `git log -1 --date=iso -- docs/UI-STANDARD.md` · `git status --porcelain` | 4 dòng đầu ra thật *(dòng này sinh ra từ chính lỗi §A1 của phiên 23/08)* |
| N0.2 | **Đọc TOÀN PHẦN SSOT** — khai tên file + dòng đầu–cuối | `wc -l docs/UI-STANDARD.md` rồi đọc hết | "Đã đọc `docs/UI-STANDARD.md` 1–371/371" |
| N0.3 | **Tra 2 sổ trước khi kết luận lệch** | đọc `docs/OWNER-REQUEST-LEDGER.md` + `.governance/registry/tech-debt.md` | Mã OIL/DEBT liên quan, hoặc "không có" |
| N0.4 | **Chốt bảng tiêu chí với Owner**, ghi ra file | — | Đường dẫn file + dấu duyệt Owner |

### N1 — BO GÓC (8 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm | Bằng chứng |
|---|---|---|---|---|
| N1.1 | Khung [toolbar+bảng+phân trang] | `rounded-md` + `overflow-hidden` | grep | ảnh |
| N1.2 | Khung panel chi tiết | `rounded-md` + `overflow-hidden` | grep | ảnh panel MỞ |
| N1.3 | Hero panel · header form | `rounded-t-md` | grep | ảnh |
| N1.4 | `th` đầu / cuối | `rounded-tl-md` / `rounded-tr-md` | grep | ảnh (thanh cam bo góc trên) |
| N1.5 | Pill thống kê header | `rounded-md` | grep | ảnh cạnh `/m1/khach-hang` |
| N1.6 | Badge trong hàng bảng | `rounded-full` | grep | ảnh |
| N1.7 | Ô nhập / tìm / select lọc | `rounded-lg` | grep | ảnh |
| N1.8 | Không `rounded-xl`/`rounded-2xl` trên card | 0 lần | `grep -c` | đầu ra = 0 |

### N2 — MÀU (5 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N2.1 | Màu brand | `#F97316` / hover `#EA580C` | grep hex |
| N2.2 | Gradient đầu bảng trên `<tr>`, `<th>` không set nền | chuỗi 3 chặng §2 | grep + ảnh |
| N2.3 | Màu trạng thái đúng nhóm | amber/blue/emerald/rose/slate | đọc mã + ảnh |
| N2.4 | Hover/chọn dòng | `bg-[#fff7ed]` + `inset 3px 0 0 #f97316` | grep + ảnh |
| N2.5 | Màu module đúng bảng §2 | theo §2 | đọc mã |

### N3 — CHỮ & KHOẢNG ĐỆM (6 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N3.1 | Tiêu đề trang | `text-xl font-bold sm:text-[22px]` — **cấm `text-3xl`/`h-14`/bọc card** | grep |
| N3.2 | Chữ ô bảng | `text-[12.5px]`, `td px-4 py-2.5` | grep |
| N3.3 | Đầu bảng | `text-[11px] font-bold text-white uppercase tracking-[0.04em]` | grep |
| N3.4 | Không double-pad — trang **không** tự thêm `px-*` ở wrapper ngoài | 0 lần | đọc mã |
| N3.5 | Không `max-w`/`mx-auto` trên shell trang | 0 lần | grep |
| N3.6 | Title Auto Case cho mọi nhãn/tiêu đề/tên (trừ mã/ID/email/SĐT) | `toVietnameseTitleCase()` | grep + ảnh |

### N4 — CẤU TRÚC TRANG (7 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N4.1 | Header trang | 🔴 theo **Q1** | grep + ảnh |
| N4.2 | Pill thống kê ở **hàng riêng** dưới tiêu đề (`mt-1.5`) | §5 | ảnh |
| N4.3 | Thanh lọc **trong** card bảng, con đầu, `border-b` | §7 | ảnh |
| N4.4 | Phân trang là chân **trong** card, `border-t` | §9 | ảnh |
| N4.5 | Vùng cuộn **1 lớp**, đơn vị `dvh` | §8 | grep |
| N4.6 | Lưới panel chi tiết | 🔴 theo **Q2** | grep + ảnh |
| N4.7 | Panel **GIÀU**: hero + 3 nút tròn + badges + ≥2 thẻ mục màu + stat box + chân audit | §10 · OIL #71 | ảnh panel MỞ |

### N5 — BIỂU MẪU (5 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N5.1 | Sheet `w-full p-0 flex flex-col sm:max-w-120` | §11 | grep |
| N5.2 | Header chuyển sắc sticky `rounded-t-md` | §11 | ảnh form MỞ |
| N5.3 | Chia mục bằng `FormSection colorTheme` | §11 | grep |
| N5.4 | Chân sticky Hủy + Lưu | §11 | ảnh |
| N5.5 | Bộ G2 đầy đủ + chặn đóng khi dirty | §11 | grep + thao tác thử |

### N6 — TRẠNG THÁI & TƯƠNG TÁC (7 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N6.1 | **Rỗng** — `py-12 text-center` + icon + câu gợi hành động | §18 | ảnh |
| N6.2 | **Đang tải** | 🔴 theo **Q3** | ảnh |
| N6.3 | **Lỗi** | 🔴 theo **T3** | ảnh |
| N6.4 | **Không quyền** — sang `/403`, ẩn nút/cột theo quyền | §18 | ảnh |
| N6.5 | **Toggle dòng** — bấm lại dòng đang chọn thì đóng panel | §20.2 | thao tác + ảnh |
| N6.6 | **Sắp xếp cột** — nút sort + `ArrowUp/Down/UpDown h-3 w-3` | §8 | thao tác |
| N6.7 | **Không** `alert()` / `confirm()` gốc | 0 lần | `grep -c` |

### N7 — ĐỊNH DẠNG DỮ LIỆU (4 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N7.1 | Ngày | `DD/MM/YYYY` (audit: `DD/MM/YYYY HH:mm`) | ảnh |
| N7.2 | Tiền & số | `1.234.567,89`; cột số `text-right font-mono tabular-nums` | ảnh |
| N7.3 | Chọn danh mục: hiện **TÊN**, gửi **ID**, ≥10 mục có tìm kiếm bỏ dấu | §19 | thao tác |
| N7.4 | Ô tìm kiếm bỏ dấu (NFD + xoá dấu) | Archive | thao tác |

### N8 — ĐA THIẾT BỊ (4 dòng)

| # | Tiêu chí | Giá trị chuẩn | Cách kiểm |
|---|---|---|---|
| N8.1 | `min-w-0` trên child grid chứa bảng | §14 | grep |
| N8.2 | `min-w-150` → `lg:min-w-215`; ẩn cột `hidden sm:table-cell`/`lg:table-cell` | §14 | grep |
| N8.3 | Panel xếp xuống dưới trên điện thoại (không drawer riêng) | §14 | ảnh |
| N8.4 | Bảng giá trị breakpoint | 🔴 theo **T2** | — |

### N9 — BẰNG CHỨNG & ĐÓNG VIỆC (5 dòng)

| # | Tiêu chí | Bằng chứng bắt buộc |
|---|---|---|
| N9.1 | Ảnh **3 kích thước**: 1920 · ~1024 · ~390 | 3 ảnh × mỗi trạng thái |
| N9.2 | Ảnh **cả trạng thái MỞ**: list + panel MỞ + form MỞ | ≥3 ảnh |
| N9.3 | **Đặt cạnh trang mẫu** cùng kích thước + bảng đối chiếu từng mục | bảng đối chiếu; nếu 0 lệch thì ghi **"0 điểm lệch"**, cấm ghi "trông ổn" |
| N9.4 | Ảnh lưu `docs/anh-kiem-thu/<nhóm>/` (đã gitignore — **cấm đẩy lên kho công khai**) | đường dẫn |
| N9.5 | `tsc` 0 lỗi · `npm run build` exit 0 · bộ test nền không hồi quy | log lệnh |

> ⚠️ **Chụp ảnh KHÔNG phải so sánh hồi quy.** Dự án **chưa có** ảnh nền + so pixel + ngưỡng tự động (§20.8 mục 7) — **cấm khai là đã có**.

**Tổng: 55 tiêu chí** (4 cổng trước + 46 tiêu chí kỹ thuật + 5 bằng chứng), trong đó **5 dòng phụ thuộc quyết định Owner** ở BẢNG 3 (N4.1 · N4.6 · N6.2 · N6.3 · N8.4).

---

## C) BA ĐIỂM CHẶN CẦN OWNER TRẢ LỜI TRƯỚC KHI CÓ THỂ BẮT ĐẦU BẤT KỲ VIỆC UI NÀO

| # | Điểm chặn | Vì sao chặn |
|---|---|---|
| **C1** | **OIL #78 (18/08) đang có hiệu lực: "hoàn thiện từng chức năng module, đụng chỗ nào xử lý tới đó" — tức DỪNG quét UI diện rộng.** Cộng với chỉ đạo 22/08 *"KHÔNG đầu tư lúc này, dồn lực go-live"* (`DEBT-063/064/065`) | Việc chuẩn hoá UI diện rộng đi ngược hai quyết định gần nhất. Owner muốn: **(a)** đảo #78 và quét UI, **(b)** giữ #78 — chỉ áp BẢNG 4 khi đụng module đang làm chức năng, hay **(c)** khác? |
| **C2** | **OIL #13: "không đổi mạnh giao diện"** | Q1 (đổi `PageHeader`) và Q2 (đổi lưới panel) **là đổi mạnh**. Cần Owner cho phép đích danh từng điểm |
| **C3** | **9 ô 🔴 CHỜ OWNER ở BẢNG 3B** | 5 dòng của BẢNG 4 không có giá trị chuẩn cho tới khi Owner chốt |

> **Đề xuất đường đi rẻ nhất, không chọi quyết định nào:** Owner chỉ duyệt **BẢNG 3A (28 ô đã rõ)** + trả lời **Q1–Q9**, rồi **KHÔNG quét UI ngay**. BẢNG 4 nằm sẵn; mỗi khi đụng một module vì lý do **chức năng** (đúng OIL #78), áp BẢNG 4 cho đúng màn đó. Như vậy chuẩn được chốt một lần mà không mở đợt sửa diện rộng trước go-live.

---

## D) BỐN VIỆC SỬA TÀI LIỆU ĐÃ GHI NHẬN — KHÔNG THỰC HIỆN (phiên READ-ONLY)

| # | Việc | Nơi | Vì sao cần |
|---|---|---|---|
| D1 | Bổ sung **7 hàng** cho 7 kỹ năng UI chưa có nhãn | `.governance/registry/ui-standard-sources.md` | Quy tắc 5 của chính registry (M23) |
| D2 | Bổ sung **4 hàng xung đột N1–N4** vào bảng phân xử | `docs/UI-STANDARD.md` §20.7 | Việc gộp sinh xung đột mới chưa khai (M26) |
| D3 | Xử lý **5 mã sổ nợ cấp trùng** | `.governance/registry/tech-debt.md` | `DEBT-030/031/032/066/067` (M28) |
| D4 | Gán nhãn cho **họ component `standard-*`** | registry + mã | Bẫy đặt sẵn (M6–M12, Q4) |

---

## E) MỘT ĐỀ XUẤT LUẬT MỚI — SINH RA TỪ CHÍNH LỖI CỦA PHIÊN NÀY

Lỗi §A1 (khảo sát đo bản 177 dòng trong khi `main` là 371 dòng) **không luật nào hiện hành chặn được**:
- `GOV-READ-STANDARD-001` bắt đọc **toàn phần** — khảo sát đã đọc toàn phần, nhưng toàn phần của **bản cũ**.
- `GOV-RELOAD-AFTER-COMPACT-001` bắt đọc lại **sau khi phiên bị nén** — khảo sát khai *"KHÔNG bị nén"*, nên luật không kích.

**Đề xuất (Owner quyết, tôi không tự ban hành):**

```
RULE_ID:     GOV-MEASURE-BASELINE-001 (đề xuất)
LEVEL:       MUST
TRIGGER:     Trước khi kết luận trạng thái của BẤT KỲ file chuẩn/sổ/registry nào
REQUIREMENT: Ghi nhánh · HEAD · ngày sửa cuối của chính file đó · cây sạch/bẩn,
             TRƯỚC khi trích số liệu. Nếu ngày sửa cuối của file CŨ HƠN commit
             hợp nhất gần nhất chạm nhánh → phải kiểm `git log --all -- <file>`
             xem có bản mới trên nhánh khác chưa hợp nhất không.
FORBIDDEN:   Kết luận "tài liệu còn thiếu/còn mâu thuẫn" mà không nêu HEAD đã đo
EVIDENCE:    4 dòng đầu ra thật (branch · HEAD · git log -1 -- <file> · status)
FAILURE:     DEGRADE_TO_PROVISIONAL
```

---

## ═══════════ BÁO CÁO KẾT THÚC ═══════════

**1. ĐÃ LÀM**
- Ghi mốc đo đầu (`main` @ `5348619` @ 06:22:55, sạch) và phát hiện kho dịch chuyển sang `<mã-nguồn-riêng>` giữa phiên → **đo lại toàn bộ số liệu mã tại mốc mới**, xác nhận 5/5 chỉ số không đổi.
- Chạy **13 tác nhân đọc song song theo nguồn** (10 đào nguồn + 3 kiểm chứng đối chứng), 336 lượt gọi công cụ, 2.304.224 token, **0 lỗi, 0 tác nhân trả rỗng**.
- Thu **537 yêu cầu UI** và **224 điểm mâu thuẫn** kèm trích nguyên văn + `file:dòng`.
- **Phân xử dứt điểm hai chuỗi bằng chứng chọi nhau**: khảo sát sáng 23/08 đo trên bản `docs/UI-STANDARD.md` **177 dòng** (`<mã-nguồn-riêng>`, 18/08 10:54), trong khi `main` đang là **371 dòng** — lượt vá `<mã-nguồn-riêng>` nằm trên nhánh `gov/2026-08-18-rules-ui-standard-upgrade`, chỉ vào `main` ngày **22/08 lúc 11:45** qua `<mã-nguồn-riêng>`. **7 kết luận của khảo sát sáng nay không còn đúng.**
- Truy vết `docs/METRONIC_UI_RESEARCH_PROTOCOL.md` bằng 4 lớp độc lập → **CHƯA TỪNG TỒN TẠI**; nhãn HISTORICAL đã có ở **3 nơi**.
- Kiểm claim 19/08 "gộp 11 kỹ năng" → **nửa đúng nửa sai**: 9/16 kỹ năng có tên trong SSOT, **7 kỹ năng (1.293 dòng) không**; phát hiện **4 xung đột mới do việc gộp sinh ra mà chưa khai**, và **1 nguồn Owner-duyệt (`mobile-responsive-ui-patterns`, 13/03/2026) không có trong registry**.
- Phát hiện **họ component `standard-*`** (5 file) chọi SSOT có hệ thống ở 7 điểm, chưa nguồn nào gán nhãn; trong đó `standard-data-table.tsx` **0 nơi dùng** nhưng mang tên "standard" = bẫy đặt sẵn.
- Đo thêm 6 chỉ số nợ mã mới: `text-3xl` **22 lần/20 file** (SSOT cấm) · `confirm()` 1 chỗ · `PageHeader` **0/4 trang mẫu** · `Loader2` **0/4 trang mẫu** · 5 mã sổ nợ cấp trùng · parity 5 file luật **PASS**.
- Hoàn tất R5 (4 điểm khảo sát bỏ dở): sắp xếp cột **có chuẩn, 4/4 trang có**; hành động trên dòng **không có chuẩn**, 4/4 trang không có cột thao tác; responsive có chuẩn nhưng **thiếu bảng breakpoint**; 4 trạng thái hiển thị **đã có §18**.
- Soạn **4 bảng đầu ra**: kho yêu cầu · 31 điểm mâu thuẫn có sức chặn · bảng giá trị chuẩn (28 ô đã rõ + 9 ô chờ Owner + 4 ô cần cấp mới) · **bộ 55 tiêu chí nghiệm thu tái dùng**.

**2. PHẠM VI**
- **ĐỤNG**: đúng **1 file**, trên **kho công khai** `Baocaoerptanphat` — `UI-AUDIT-TONG-LUC-20260823.md`.
- **KHÔNG ĐỤNG**: `src/` KHÔNG · `docs/` KHÔNG · `.cursor/` KHÔNG · `.governance/` KHÔNG · 5 file luật KHÔNG · `migrations/` KHÔNG · kỹ năng KHÔNG · `docs/UI-STANDARD.md` **KHÔNG** (dù phát hiện 3 điểm cần sửa — đã ghi ở §D) · DB KHÔNG ghi · deploy KHÔNG · version KHÔNG bump · commit vào kho mã **KHÔNG**.
- Kho mã sạch trước và sau: `git status --porcelain` → 0 dòng.

**3. BẰNG CHỨNG**
- `git rev-parse HEAD` · `git log --follow --date=iso -20 -- docs/UI-STANDARD.md` · `git merge-base --is-ancestor` · `git show <sha>:docs/UI-STANDARD.md | wc -l` cho 7 mốc → **FILE_PROVEN** (chuỗi thời gian của SSOT).
- `git rev-list --all --objects | grep -i METRONIC` trên **456 commit / 7.591 đối tượng** + `find` toàn ổ D: và `C:\Users\tantm` (đã xác minh cả hai mount đọc được) → **FILE_PROVEN** (file chưa từng tồn tại).
- `git hash-object` × 5 → `<mã-nguồn-riêng>…`; `sha256sum` × 5 → `<mã-nguồn-riêng>…` → **FILE_PROVEN** (parity 5 bản luật PASS).
- `grep -rn`/`grep -rln`/`grep -c` trên `src/app`, `src/components`, `.cursor/skills`, `.governance/registry` → **CODE_PROVEN** (mọi số đo nợ mã, kèm lệnh trong bảng B1/B2).
- Đọc toàn phần: `docs/UI-STANDARD.md` 371/371 · `CLAUDE.md` 2075/2075 · `ARCHIVE-LEGACY-RULESET.md` 2785/2785 · `OWNER-REQUEST-LEDGER.md` 387/387 · `tech-debt.md` 136/136 · 16 SKILL.md · 4 trang mẫu · 7 doc mẫu · token + component nền → **FILE_PROVEN**.
- ⚠️ **KHÔNG có `UI_PROVEN` và KHÔNG có `RUNTIME_PROVEN`** — phiên read-only, không chạy ứng dụng, không chụp ảnh. Mọi kết luận về "hiển thị thật" đều **chưa được kiểm bằng mắt**.

**4. GHI SỔ YÊU CẦU OWNER**
- **CHƯA** — lý do: phiên này `FORBIDDEN_ACTIONS` **cấm sửa mọi file trong kho mã**, mà `docs/OWNER-REQUEST-LEDGER.md` nằm trong kho mã. Ghi sổ là hành động **mặc định được phép** theo `GOV-SESSION-DECISION-001` §F1b mục 3, nhưng ở đây bị **lệnh phiên của Owner** chặn — đây là xung đột giữa hai chỉ thị, tôi chọn tuân lệnh phiên hẹp hơn và **khai rõ** thay vì tự quyết.
- ⛔ **Cần ghi ngay ở phiên sau** (đã soạn sẵn nội dung): (a) Owner chốt 06:15 23/08 về nguyên tắc "chuẩn chỉnh trước"; (b) kết quả phân xử hai chuỗi bằng chứng §A; (c) 9 câu hỏi Q1–Q9 và 3 điểm chặn C1–C3.

**5. PUSH BÁO CÁO CÔNG KHAI**
- **ĐÃ PUSH** — kho `Baocaoerptanphat` · nhánh `main` · file `UI-AUDIT-TONG-LUC-20260823.md` · commit **`<mã-nguồn-riêng>`** (`<mã-nguồn-riêng>`).
- *(Bản này là lượt đẩy thứ hai, chỉ điền mã commit thật vào trường 5 — nội dung audit không đổi.)*

**6. CÒN SÓT / CHƯA LÀM**
- Chưa đọc toàn văn 2 file client lớn nhất: `m3/bao-gia/bao-gia-client.tsx` (~3.400 dòng) và `m3/tinh-gia-admin/tinh-gia-admin-client.tsx` (~4.689 dòng) — nợ cũ `DEBT-035`, có thể còn nhánh UI giả chưa lộ.
- Chưa chụp ảnh màn hình nào → **0 tiêu chí nào đạt lớp `UI_PROVEN`**.
- Chưa quy đổi HSL ↔ hex cho các cặp token không kèm chú thích (`--warning`, `--success`, `--error`, `--info`, `--text-*`, `--border-*`) → **chưa kết luận được** chúng khớp hay lệch.
- Chưa đọc component `toast` → chưa mô tả được hình thức hiển thị của trạng thái lỗi đang chạy.
- 537 yêu cầu thô mới trình dạng **tổng hợp + phân bố**; bản liệt kê đầy đủ từng dòng kèm trích nguyên văn chưa đưa vào file này (giữ trong dữ liệu phiên, cấp khi Owner yêu cầu).
- Chưa truy được vì sao `src/hooks/use-overlay-guard.ts` và `use-close-confirm.ts` được tài liệu G1/G2 nhắc mà không tồn tại (không có dấu vết tạo/xoá trong lịch sử đã tra).
- 4 việc sửa tài liệu D1–D4 **chỉ ghi nhận, không thực hiện** (đúng cổng dừng R15).

**7. ĐANG CHỜ OWNER**
- **C1** — giữ hay đảo OIL #78 (dừng sweep UI) → **chặn toàn bộ** việc UI diện rộng.
- **C2** — OIL #13 "không đổi mạnh giao diện" → chặn Q1, Q2, Q8b.
- **C3 / Q1–Q9** — 9 quyết định ở BẢNG 3B → chặn 5 dòng của BẢNG 4 (N4.1 · N4.6 · N6.2 · N6.3 · N8.4).
- **T1–T4** — 4 chuẩn chưa từng tồn tại, cần Owner cấp mới.
- **D1–D4** — 4 việc sửa tài liệu, cần Owner cho phép ghi vào kho mã.
- **E** — có ban hành `GOV-MEASURE-BASELINE-001` không.
- Nếu Owner chưa trả lời: **không sửa được dòng mã UI nào**, và BẢNG 4 chỉ dùng được 50/55 dòng.

**8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC**
Owner trả lời **C1** (giữ hay đảo OIL #78). Đây là câu chặn gốc — 8 câu còn lại chỉ có nghĩa nếu C1 cho phép động vào UI. Nếu Owner giữ #78, đường đi rẻ nhất đã đề xuất ở cuối §C: duyệt BẢNG 3A + trả lời Q1–Q9, **không mở đợt quét UI nào trước go-live**.

**9. CHƯA XÁC MINH ĐƯỢC**
- **Giao diện thật trên màn hình** — vì không chạy ứng dụng, không chụp ảnh. Ai xác minh được: Agent IDE (chạy `npm run go` + `scripts/shot-route.mjs`) hoặc Owner xem trực tiếp.
- **Gradient đầu bảng có bị che trong thực tế không** — chỉ đọc được class, chưa nhìn thấy. Ai: như trên.
- **Điểm gãy responsive thật ở 3 kích thước** — chưa chụp. Ai: như trên.
- **Ý định gốc của "METRONIC UI MANDATORY PROTOCOL"** — file tham chiếu chưa từng tồn tại trong kho, không truy được nguồn. Ai: Owner, hoặc Agent Notion (có thể còn bản trên Notion).
- **`mobile-responsive-ui-patterns` (Owner duyệt 13/03/2026) còn hiệu lực không** — chỉ Owner quyết được.
- **Trang mẫu nào "đúng" khi 4 trang lệch nhau** (Q1, Q2) — chỉ Owner quyết được.
- **Hai hệ token màu (hex ở `design-tokens.css` ↔ HSL ở `globals.css`) có thật sự lệch không** — chỉ khẳng định được ở các cặp có chú thích hex; phần còn lại cần công cụ quy đổi màu.

**10. TRẠNG THÁI CHUNG**
- **PROVISIONAL** — thiếu: (a) lớp bằng chứng `UI_PROVEN` cho mọi tiêu chí hiển thị; (b) 9 quyết định Owner ở BẢNG 3B; (c) trả lời điểm chặn C1 về phạm vi.
- Điều kiện lên **PASS**: Owner trả lời C1–C3 + Q1–Q9 → BẢNG 3 đầy đủ giá trị chuẩn → BẢNG 4 dùng được trọn 55 dòng → chạy một lượt chụp ảnh 3 kích thước cho 4 trang mẫu để nâng các tiêu chí hiển thị lên `UI_PROVEN`.
- Riêng phần **audit đọc-hiểu** (R0–R13): **đã hoàn tất, đủ bằng chứng text-first**, 13/13 tác nhân trả kết quả, 0 lỗi.

**11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU**
- Phiên có bị nén ngữ cảnh không: **KHÔNG**.
- **Nhưng đã xảy ra một dạng lệch nguy hiểm hơn nén: dữ liệu tham chiếu cũ từ lượt trước trong cùng phiên.** Bản khảo sát Bước 0–5 đo `docs/UI-STANDARD.md` = 177 dòng; tại HEAD của phiên này file là 371 dòng. Đã **đọc lại toàn phần từ đĩa** thay vì tin số liệu lượt trước — chính nhờ vậy mới phát hiện §A.
- Tài liệu tham chiếu **đã đọc lại từ đĩa trong phiên này**: `docs/UI-STANDARD.md` (371/371) · `CLAUDE.md` (2075/2075) · `.governance/ARCHIVE-LEGACY-RULESET.md` (2785/2785) · `.governance/registry/ui-standard-sources.md` (51/51) · `.governance/registry/legacy-rules-status.md` · `.governance/registry/tech-debt.md` (136/136) · `docs/OWNER-REQUEST-LEDGER.md` (387/387) · 16 file `SKILL.md` · 7 doc mẫu/G1/G2/Metronic · `design-tokens.css` · `globals.css` · `components/foundation/*` · `components/ux/*` · `components/ui/*` · 4 trang mẫu · các báo cáo UI trên kho công khai.
- **Cảnh báo cho phiên sau:** kho đã dịch chuyển 1 commit ngay trong phiên này. Trước khi dùng bất kỳ số liệu nào trong báo cáo này, **chạy lại N0.1** (kiểm mốc kho) — nếu HEAD đã khác `<mã-nguồn-riêng>`, phải đo lại phần số liệu mã.

═══════════════════════════════════════════
