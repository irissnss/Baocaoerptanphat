# CHUỖI LUẬT → SKILL → GIAO DIỆN — CHỖ ĐỨT, SỐ ĐO, KẾ HOẠCH

> **Loại:** ĐO — ĐỐI CHIẾU — LẬP KẾ HOẠCH (**CHỈ ĐỌC, không sửa file mã/luật/SSOT nào**)
> **Ngày:** 23/08/2026 ~13:00 · **Owner:** TanPhatERP · **Actor:** Agent IDE (phiên "Luật Cho Dự Án TanPhat")
> **HEAD:** `<mã-nguồn-riêng>` · nhánh `main` · parity 5 file luật `<mã-nguồn-riêng>…`
> **Căn cứ:** Sổ Yêu Cầu Owner **mục #131** (23/08 12:30)
> **Cách đo:** 15 tác nhân con đọc song song, chỉ-đọc · 2.536.084 token · 462 lượt gọi công cụ · 0 lỗi

---

## 0) TRẢ LỜI CÂU HỎI CỦA OWNER

> *"Theo em có nên đưa quy chuẩn Ui và phiên này luôn không, chuẩn chỉnh từ luật → skill → UI, sau này code fix sẽ đỡ tốn thời gian, token, nhanh chóng hơn?"*

**CÓ — và lý do không phải "cho gọn gàng". Chuỗi đang đứt ở chỗ khiến việc tuân thủ luật DẪN TỚI code sai.**

Owner mô tả hiện tượng: *"hèn gì anh thấy nó cứ trở ngại và vướng vấp miết"*. Báo cáo này chứng minh hiện tượng đó **có nguyên nhân cơ học đo được**, không phải cảm giác.

---

## 1) BA MẮT XÍCH GÃY — đo bằng lệnh, không suy đoán

### 1.1 Skill không biết SSOT tồn tại

| Đại lượng | Số | Lệnh đo |
|---|---|---|
| Skill trong `.cursor/skills/` | **128** | `ls .cursor/skills \| wc -l` |
| Skill có nội dung giao diện | **57** | `grep -rl "className\|rounded-\|Tailwind\|Sheet\|DataTable" .cursor/skills/*/SKILL.md` |
| Đã gộp vào SSOT (đợt `D3` 18/08) | **11** | bảng `docs/UI-STANDARD.md` §20.1 |
| **Chưa gộp** | **49** | phép trừ |
| **Skill trỏ ngược về `docs/UI-STANDARD.md`** | **0** | `grep -rl "UI-STANDARD" .cursor/skills/` → 2 kết quả, **cả 2 không phải skill giao diện** (`documentation-sync`, `public-report-sanitize-gate`) |

> ⚠️ **Khai chênh lệch số liệu (bắt buộc theo `GOV-EVIDENCE-STRENGTH-001`):** tác nhân thiết kế đếm được **54**, bộ dò của phiên chính đếm **57**. Chênh do **tiêu chí dò khác nhau**, chưa hợp nhất. Con số dùng để lập kế hoạch phải là con số đo lại bằng **một** bộ dò duy nhất — đã đưa thành dòng nghiệm thu `A2`.

### 1.2 🔴 NẶNG NHẤT — hai sổ đăng ký chọi nhau, và LUẬT ĐANG TRỎ VÀO CÁI SAI

Ngày 23/08 lúc 11:13, Owner duyệt `D-D1`: hạ nhãn 4 kỹ năng xuống **SUPERSEDED — chọi trực tiếp SSOT** (hàng **S13–S16** của `.governance/registry/ui-standard-sources.md`).

| Kỹ năng | `ui-standard-sources.md` (Owner duyệt hôm nay) | `.governance/registry/skills.yml` |
|---|---|---|
| `master-list-data-table` | 🔴 SUPERSEDED — chọi 3 điểm | 🟢 **`health_label: "HEALTHY"`** |
| `header-inline-badge` | 🔴 SUPERSEDED — `text-3xl` | `UNKNOWN-PURPOSE` |
| `inline-filter-bar-layout` | 🔴 SUPERSEDED — chọi §7 | `UNKNOWN-PURPOSE` |
| `transactional-page-redesign` | 🔴 SUPERSEDED — chọi §8.1 | `UNKNOWN-PURPOSE` |

`grep -c "SUPERSEDED" .governance/registry/skills.yml` → **0**.

**Vì sao đây là mắt xích nguy hiểm nhất:** luật `GOV-SKILL-RESOLUTION-001` §L2 bắt tra **`skills.yml`** khi chọn kỹ năng — **không phải** `ui-standard-sources.md`. Chuỗi hành vi của một phiên **làm đúng luật từng bước**:

```
cần dựng bảng danh sách
  → §L2 bắt tra skills.yml
  → tìm thấy master-list-data-table, nhãn HEALTHY
  → nạp
  → được dạy: rounded-2xl        (SSOT §1 CẤM)
              max-h-[60vh]       (SSOT §8 dùng dvh)
              <th> nền đặc       (chọi §2/§8)
  → Owner bác
```

**Càng tuân thủ luật càng code sai.** Đây chính là cơ chế của "vướng vấp miết".

### 1.3 Bộ 57 tiêu chí nghiệm thu chưa được nối vào luật

```
grep -c "UI-ACCEPTANCE-CHECKLIST" CLAUDE.md .cursorrules AGENTS.md
  → 0 · 0 · 0
```

`GOV-ACCEPTANCE-FIRST-001` §G7.1 `REFERENCE` vẫn chỉ trỏ `.governance/procedures/acceptance-template.md` (mẫu rỗng 92 dòng). Bộ **57 tiêu chí** Owner duyệt sáng nay (`docs/UI-ACCEPTANCE-CHECKLIST.md`, 173 dòng) **không có đường nào dẫn tới từ luật**.

---

## 2) ĐỐI CHIẾU 49 SKILL CHƯA GỘP — KẾT QUẢ

Cách làm: 7 lô × 7 kỹ năng, mỗi lô một tác nhân đọc **toàn phần** SSOT + 7 `SKILL.md`. Lô nào tìm ra mâu thuẫn thì sinh tiếp **tác nhân phán xử** mở `src/` đối chứng.

### 2.1 Phân loại 49 kỹ năng

| Rổ | Số | Nghĩa |
|---|---|---|
| **MÂU THUẪN** | **35** | dạy giá trị khác SSOT cho cùng một thứ |
| **BỔ SUNG** | **8** | có quy tắc đúng mà SSOT **chưa phủ** |
| **LỖI THỜI** | **1** | `demo-before-implement` — cách làm cụ thể đã lạc hậu |
| **KHÔNG LIÊN QUAN UI** | **5** | thực chất là quy trình/logic, không đặt giá trị giao diện |

### 2.2 Phán xử 114 điểm chọi bằng **mã thật**

| Kết quả | Số | Nghĩa |
|---|---|---|
| **SSOT ĐÚNG** | **107 (94%)** | kỹ năng sai → phải hạ nhãn |
| SSOT sai *(theo tác nhân)* | 4 | ⚠️ **phiên chính kiểm lại: 3/4 SAI** — xem §3 |
| CẢ HAI SAI | 1 | cỡ icon nút Đóng panel |
| Không kết luận được | 2 | không tìm thấy mã tương ứng |

Mức ưu tiên: **GẤP 49** · THƯỜNG 60 · THẤP 5. Điểm chọi mức **CAO: 35**.

### 2.3 Chủ đề chọi lặp lại nhiều nhất

`Emoji trong giao diện` (6 lượt, 3 dạng) · `Bảng trên điện thoại có cuộn ngang không` (2) · `Chiều cao + bo góc nút chọn` (2) · `Cột Thao Tác` · `Bo góc panel` · `Màu gradient hero` · `Ký hiệu đơn vị tiền`.

### 2.4 Tám kỹ năng **BỔ SUNG** — đây là phần "đỡ tốn token" thật

| Kỹ năng | SSOT hiện phủ | Khoảng trống |
|---|---|---|
| `implement-wizard-step` | **KHÔNG CÓ** — `grep "wizard"` SSOT → **0** | Toàn bộ đặc tả wizard |
| `tailwind-v4-canonical-classes` | KHÔNG CÓ | Bảng quy đổi v3→v4 (dự án dùng `tailwindcss ^4.2.1`) |
| `title-auto-case` | §13 — **đúng 1 dòng** | Danh mục nơi-áp/nơi-cấm, 3 mẫu, 4 phản mẫu |
| `autocomplete-input-component` | §19 chỉ phủ combobox danh mục | Ô nhập biểu thức/công thức có gợi ý |
| `portal-safe-dropdown-overlay` | — | Overlay dùng Portal, không bị `overflow-hidden` cắt |
| `popover-scroll-fix` | — | Chặn cuộn bị overlay cha nuốt |
| `optimistic-ui-updates` | §18 | Phản hồi sau create/update/delete + hoàn tác |
| `flex-layout-expand-fix` | §14 mới có `min-w-0` ngang | `flex-1` + `min-h-0` + `shrink-0` dọc |

---

## 3) ⚠️ PHIÊN CHÍNH TỰ KIỂM NGƯỢC TÁC NHÂN CON — 3/4 PHÁN QUYẾT "SSOT SAI" LÀ SAI

Luật `GOV-EVIDENCE-STRENGTH-001` cấm nhận kết luận của công cụ làm bằng chứng trực tiếp. Bốn ca gắn nhãn *"SSOT ghi sai"* là loại kết luận **dẫn tới sửa SSOT**, nên phiên chính mở mã kiểm lại từng ca.

| Ca | Tác nhân kết luận | Phiên chính kiểm lại | Đúng/Sai |
|---|---|---|---|
| `searchable-dropdown` — nút chọn `h-9 rounded-lg` | SSOT sai | Dòng `:130 h-9` của kỹ năng là **ô tìm kiếm BÊN TRONG popover**, không phải nút chọn. Nút chọn ở `:102-111` dùng `<Button>` không set cỡ → thừa kế `h-10 rounded-md`, **y hệt mã thật**. Không có xung đột — **SSOT chỉ THIẾU định nghĩa** nút combobox trong biểu mẫu | ❌ **SAI** |
| `searchable-multiselect-popover` — như trên | SSOT sai | Cùng nguyên nhân | ❌ **SAI** |
| `test-execution-ui` — cột thao tác trên dòng | SSOT sai | **Phát hiện đúng, kết luận sai.** `D-T1` là **quyết định quy phạm của Owner** (duyệt 11:13 hôm nay), mã lệch quyết định mới = **NỢ**, không phải "SSOT sai" — đúng điều `GOV-SESSION-DECISION-001` §F1b mục 5 bắt phải tra sổ trước khi kết luận. **NHƯNG nó lộ ra một lỗi thật rất nặng → §4** | ⚠️ **NỬA ĐÚNG** |
| `mobile-responsive-ui-patterns` — vị trí icon lịch sử | SSOT sai | Mã có chú thích nguyên văn *"Mã KH column hidden per Owner request"* — là quyết định Owner nằm trong mã, SSOT không phủ. **SSOT THIẾU**, không sai | ⚠️ **NỬA ĐÚNG** |

> **Kết luận quan trọng: 0/4 ca là "SSOT ghi sai GIÁ TRỊ CHUẨN".** Giá trị chuẩn của SSOT đứng vững. Chỗ hụt là **thiếu định nghĩa**, và **một dòng căn cứ sai sự thật** (§4).
>
> **Bài học ghi sổ:** tác nhân phán xử `grep "h-9"` thấy khớp là kết luận, **không kiểm dòng đó thuộc phần tử nào**. Đây đúng loại lỗi mà chính báo cáo 18/08 đã gọi tên là *"đọc lỗ khoá"* — nay tái diễn ở tầng tác nhân con. Đề xuất `V5`.

---

## 4) 🔴 PHÁT HIỆN NẶNG — DÒNG "CĂN CỨ 4/4" CỦA §8.1 SAI SỰ THẬT

`docs/UI-STANDARD.md` §8.1 (Owner duyệt **11:13 hôm nay**, `D-T1`) ghi:

> *"**Căn cứ:** đây là quy ước đã chạy thật ở **4/4 trang nền tảng**… ba trang còn lại cũng **không có cột thao tác**."*

**Đo lại 4 trang nền tảng:**

| Trang nền tảng | Nút Sửa/Xoá trên dòng |
|---|---|
| `m1/san-pham` | 0 |
| `m1/khach-hang` | 0 |
| `m1/nhan-su` | 0 |
| **`m5/kho-thanh-pham`** | 🔴 **2** — `Pencil` "Sửa" + `Trash2` "Xóa" |

Bằng chứng: `src/app/m5/kho-thanh-pham/kho-thanh-pham-client.tsx:321` — `<th className={...rounded-tr-md w-16}></th>` (**cột KHÔNG có nhãn**) và `:367-371` — hai nút trên **từng dòng**.

**Con số đúng là 3/4, không phải 4/4.**

**Vì sao lọt:** cột đó có tiêu đề **rỗng**. Mọi phép dò kiểu `grep "Thao tác"` đều không thấy — kể cả phép dò đã dùng để viết dòng căn cứ. Ngoài ra còn **7 trang khác** có cột "Thao tác" **có nhãn**: `m0/form-mau` · `m0/form-phat-hanh` · `m1/khach-hang/wizard` · `m3/bao-gia/item` · `m3/bao-gia/option` · `m3/crm/nhat-ky` · `m3/crm/task`.

**Điều này KHÔNG bác quyết định `D-T1`** — đó là lựa chọn quy phạm của Owner, vẫn đứng. Nhưng:

1. **Dòng căn cứ phải sửa** cho đúng sự thật (`GOV-EDIT-PRESERVE-001`: giữ nguyên văn dòng cũ ở §21).
2. **`m5/kho-thanh-pham` + 7 trang kia = NỢ MỚI** đối với `D-T1`, phải vào sổ nợ.
3. **`kho-thanh-pham` là 1 trong 4 TRANG MẪU** — phiên sau chép mẫu từ nó sẽ chép luôn cột thao tác, tái tạo vi phạm.

---

## 5) KẾ HOẠCH — 6 BƯỚC, KHÔNG QUÉT DIỆN RỘNG

**Ràng buộc bám suốt:** Sổ **#78** (CẤM quét UI diện rộng) · Sổ **#13** (không đổi mạnh giao diện) · `GOV-EDIT-PRESERVE-001` (cấm xoá/ghi đè im lặng) · Owner làm một mình → ít lệnh.

### V1 🔴 — Nối nhãn vào đúng nơi luật bắt tra *(gốc của "vướng vấp")*

`scripts/tests/skills-registry-build.mjs` đọc thêm `ui-standard-sources.md` + dữ liệu đối chiếu, sinh **3 trường mới** cho mỗi entry `skills.yml`:

```yaml
ui_scope:     UI | KHONG_UI
ssot_verdict: SSOT_THANG | BO_SUNG | LOI_THOI | TRUNG_KHOP | CHUA_DOI_CHIEU
ssot_muc:     "§1 §8 §2"      # đụng kỹ năng này thì đọc mục nào
```

- **KHÔNG sửa một file kỹ năng nào** — kỹ năng là lưu trữ, `D3` đã chốt.
- **Không xoá gì** — chỉ thêm trường.

### V2 — Cổng máy chặn tái diễn

Thêm `npm run test:ui-skill-conflict`, nối vào `test:gov-gates`. Đọc **đầu vào thật**: `skills.yml` + `ui-standard-sources.md` + `docs/UI-STANDARD.md` (thoả `GOV-GATE-REAL-INPUT-001`).

**FAIL khi:** (a) nhãn hai registry lệch nhau · (b) kỹ năng `ui_scope: UI` mà `ssot_verdict: CHUA_DOI_CHIEU` · (c) kỹ năng chứa chuỗi SSOT cấm (`rounded-2xl` · `text-3xl` · `max-h-[*vh]`) mà không gắn `SSOT_THANG` · (d) kỹ năng mới thêm vào kho mà chưa có nhãn.

**Kiểm ngược bắt buộc:** gieo một nhãn lệch → cổng **phải FAIL**. Không FAIL = cổng giả, huỷ làm lại.

### V3 — Nối bộ 57 tiêu chí vào luật

Thêm (**không thay**) một dòng `REFERENCE` vào `GOV-ACCEPTANCE-FIRST-001` §G7.1 + một dòng ở §V: *việc giao diện → `docs/UI-ACCEPTANCE-CHECKLIST.md`*. Sửa **cả 5 file luật một lượt**, verify parity SHA-256. `ref-exists-gate` canh từ đó.

### V4 — Xử lý 49 kỹ năng theo 4 rổ

| Rổ | Số | Việc | Xoá gì? |
|---|---|---|---|
| MÂU THUẪN | 35 | Gắn `SSOT_THANG` + liệt kê điểm chọi | **Không xoá** |
| BỔ SUNG | 8 | Gộp quy tắc vào SSOT (§ mới), gắn `MERGED` | **Không xoá** |
| LỖI THỜI | 1 | Nhãn `HISTORICAL` | **Không xoá** |
| KHÔNG LIÊN QUAN | 5 | `ui_scope: KHONG_UI` | **Không xoá** |

### V5 — Vá lỗi "đọc lỗ khoá" ở tầng tác nhân con

Ca §3 cho thấy tác nhân con `grep` trúng chuỗi là kết luận, không kiểm chuỗi đó thuộc phần tử nào. Đề xuất: mọi phán quyết *"tài liệu chuẩn ghi sai"* phải kèm **ngữ cảnh ±10 dòng** của cả hai phía, và **phiên chính bắt buộc kiểm lại** trước khi sửa SSOT.

### V6 — Vá §8.1 và ghi nợ *(từ §4)*

Sửa dòng căn cứ `4/4 → 3/4` (giữ nguyên văn dòng cũ ở §21) · ghi nợ `m5/kho-thanh-pham` + 7 trang · **cảnh báo trong §8.1** rằng trang mẫu `kho-thanh-pham` đang lệch chính điều này.

---

## 6) BẢNG TIÊU CHÍ NGHIỆM THU — soạn sẵn theo `GOV-ACCEPTANCE-FIRST-001`

| # | Tiêu chí | Cách đo | Lớp bằng chứng |
|---|---|---|---|
| A1 | `skills.yml` có đủ 3 trường mới cho **128/128** entry | `grep -c "ssot_verdict:"` = 128 | FILE_PROVEN |
| A2 | Số kỹ năng `ui_scope: UI` **thống nhất một bộ dò** (giải chênh 54↔57) | in ra danh sách + số, đối chiếu tay | FILE_PROVEN |
| A3 | 4 kỹ năng S13–S16 mang `ssot_verdict: SSOT_THANG` | `grep` từng slug | FILE_PROVEN |
| A4 | Kỹ năng `ui_scope: UI` còn `CHUA_DOI_CHIEU` = **0** | cổng in số | FILE_PROVEN |
| A5 | Cổng `test:ui-skill-conflict` **PASS** trên cây sạch | chạy lệnh | RUNTIME_PROVEN |
| A6 | **Kiểm ngược:** gieo 1 nhãn lệch → cổng **FAIL** đúng chỗ; khôi phục → PASS lại | chạy 3 lần | RUNTIME_PROVEN |
| A7 | 5 file luật **byte-identical** sau khi thêm dòng §G7.1 + §V | so 5 SHA-256 | FILE_PROVEN |
| A8 | `ref-exists-gate` PASS với tham chiếu mới | chạy lệnh | RUNTIME_PROVEN |
| A9 | Cổng đếm điều khoản: **SAU ≥ TRƯỚC** cả 2 điều kiện | `test:clause-count` + `test:standard-clause-count` | RUNTIME_PROVEN |
| A10 | §8.1 ghi **3/4**, dòng cũ **4/4** còn nguyên văn ở §21 | đọc 2 chỗ | FILE_PROVEN |
| A11 | Nợ `kho-thanh-pham` + 7 trang có mã DEBT, có `hạn_đóng` | đọc sổ nợ | FILE_PROVEN |
| A12 | **0 file `.cursor/skills/` bị sửa** | `git status --porcelain .cursor/skills` rỗng | FILE_PROVEN |
| A13 | **0 file `src/` bị sửa** trong đợt V1–V3 | `git diff --stat src/` rỗng | FILE_PROVEN |
| A14 | Đo **THẬT** token đọc SSOT trước/sau (thay ước lượng §7) | ghi số công cụ báo về | RUNTIME_PROVEN |

---

## 7) TIẾT KIỆM ĐƯỢC GÌ — kèm khai rõ phần nào là ƯỚC

**Đo thật trong phiên này:** đọc `docs/UI-STANDARD.md` dòng 1–344 (76% file) tốn **27.978 token** (công cụ báo về).

| Loại việc | Nay | Sau | Chênh |
|---|---|---|---|
| Sửa cục bộ (bo góc · lề · 1 cột · 1 nhãn) | ~36.900 | ~4.800 | **−87%** |
| Dựng lại một khối (panel · form · toolbar) | ~36.900 | ~14.000 | **−62%** |
| Dựng màn MỚI / đổi mạnh giao diện | ~36.900 | ~36.900 | **0 — cố ý không nới** |
| Xét dùng 1 kỹ năng | 6.000–14.000 **và vẫn không biết nó chọi gì** | ~150 | **−98%, và BIẾT** |

> ⚠️ **Khai trung thực:** con số **27.978 là số thật**; các con số còn lại là **ngoại suy tuyến tính**, phải đo lại ở dòng nghiệm thu `A14`. **KHÔNG hứa** "hết bị Owner bác" — cơ chế này chỉ đóng **một** nguyên nhân: *chép trúng giá trị sai từ một kỹ năng không ai gắn nhãn*. Các nguyên nhân khác do bộ nghiệm thu và §20.8 (chụp ảnh đối chiếu) lo.

---

## 8) CẦN OWNER QUYẾT

| # | Việc | Đề xuất của Agent |
|---|---|---|
| **1** | Cho làm **V1+V2** (nối nhãn + cổng máy) không? | **Nên làm trước tiên** — đây là gốc của "vướng vấp miết" |
| **2** | Cho sửa **5 file luật** ở V3 không? | Nên — 2 dòng thêm, không thay dòng nào |
| **3** | **8 kỹ năng BỔ SUNG** — gộp hết vào SSOT, hay chọn lọc? | Đề nghị gộp `implement-wizard-step` + `tailwind-v4-canonical-classes` + `title-auto-case` trước (3 khoảng trống lớn nhất) |
| **4** | §8.1 sai "4/4" — sửa ngay hay gộp vào đợt sau? | **Sửa ngay** — `kho-thanh-pham` là trang mẫu, để lâu là phiên sau chép lại vi phạm |
| **5** | `GOV-READ-STANDARD-001` §G7.2 bắt đọc SSOT **toàn phần** mỗi lần. SSOT nay **453 dòng**, chính §G7.2 có sẵn ngoại lệ cho file **>500 dòng**. Áp ngoại lệ sớm hay chờ vượt 500? | Agent **không tự quyết** — đây là nới lỏng một điều MUST |

---

## 9) PHẦN KHÔNG TỰ QUYẾT / CHƯA LÀM

- **Chưa vá bất cứ thứ gì.** Phiên này chỉ-đọc theo đúng câu hỏi của Owner ("nên hay không").
- **`OPEN-N2`** (màu module M4 teal ↔ purple→pink) vẫn chờ Owner — phiên này **không đụng**.
- **K1–K6** của báo cáo trước (lỗ hổng `git ls-files -z`, file dữ liệu khách hàng) **vẫn nguyên**, chưa xử lý.
- Chênh **54 ↔ 57** kỹ năng UI chưa hợp nhất bộ dò.

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Đo 3 mắt xích gãy của chuỗi Luật -> Skill -> Giao dien bang lenh
   - Doi chieu 49 ky nang UI chua gop voi SSOT + ma that (15 tac nhan, 2.53M token)
   - Tu kiem nguoc tac nhan con: 3/4 phan quyet "SSOT sai" la SAI
   - Phat hien dong "Can cu 4/4 trang nen tang" cua §8.1 SAI SU THAT (dung: 3/4)
   - Ghi So Yeu Cau Owner muc #131
   - Soan ke hoach V1-V6 + bang 14 tieu chi nghiem thu

2. PHẠM VI
   ĐỤNG      : docs/OWNER-REQUEST-LEDGER.md (muc #131) · bao cao nay
   KHÔNG ĐỤNG: 5 file luat (parity <mã-nguồn-riêng> KHONG doi) · docs/UI-STANDARD.md
               · .cursor/skills (0 file) · src (0 file) · skills.yml
               · DB · deploy · cong kiem

3. BẰNG CHỨNG
   grep -rl "UI-STANDARD" .cursor/skills/ -> 2 ket qua, ca 2 khong phai skill UI -> FILE_PROVEN
   grep -c "SUPERSEDED" skills.yml -> 0 | master-list-data-table -> health_label HEALTHY -> FILE_PROVEN
   grep -c "UI-ACCEPTANCE-CHECKLIST" CLAUDE.md .cursorrules AGENTS.md -> 0 0 0 -> FILE_PROVEN
   kho-thanh-pham-client.tsx:321 cot th rong + :367-371 Pencil/Trash tren dong -> CODE_PROVEN
   4 trang nen tang, dem nut sua/xoa tren dong -> 0 0 0 2 -> CODE_PROVEN
   workflow 15 tac nhan: 49 skill, 114 diem choi, SSOT dung 107/114 -> CODE_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #131

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng> · file CHUOI-LUAT-SKILL-UI-20260823.md

6. CÒN SÓT / CHƯA LÀM
   - V1..V6 CHUA LAM — cho Owner duyet (phien nay chi-doc theo dung cau hoi)
   - Chenh 54 <-> 57 so ky nang UI chua hop nhat bo do
   - OPEN-N2 (mau module M4) van cho Owner
   - K1..K6 bao cao truoc (lo hong -z, file du lieu khach hang) van nguyen

7. ĐANG CHỜ OWNER
   - 5 cau o muc 8. Chan: khong tra loi thi V1-V6 khong bat dau duoc
   - Rieng cau 4 (muc 8.1 sai 4/4) de lau thi phien sau chep lai vi pham tu trang mau

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Lam V1: them 3 truong ui_scope/ssot_verdict/ssot_muc vao skills.yml qua script sinh.

9. CHƯA XÁC MINH ĐƯỢC
   - Con so tiet kiem token sau khi co co che — moi la ngoai suy tuyen tinh,
     phai do that o dong nghiem thu A14
   - Co che nay giam duoc bao nhieu luot Owner bac — khong do duoc trong phien
   - Noi dung khung chat phien "Chuan Hoa UI Webapp" — phien khac, chi doc duoc
     san pham ghi ra dia. Ai xac minh: Owner mo tab do

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — thieu quyet dinh Owner cho 5 cau o muc 8.
       Dieu kien len PASS: Owner tra loi, agent thi hanh V1-V6, dat 14/14 dong nghiem thu

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phien co bi nen: CO
   Da doc lai sau nen: docs/UI-STANDARD.md · docs/UI-ACCEPTANCE-CHECKLIST.md
   · .governance/registry/ui-standard-sources.md · .governance/registry/skills.yml
   · .governance/registry/tech-debt.md · docs/OWNER-REQUEST-LEDGER.md
   · CLAUDE.md §G7.1 §G7.2 §L2 §L6 §V · scripts/pre-commit-hook.sh
   · src/app/m5/kho-thanh-pham/kho-thanh-pham-client.tsx
═══════════════════════════════════════════
```
