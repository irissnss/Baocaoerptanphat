# 🎨 UI STANDARD — ERP Tân Phát (CHUẨN DUY NHẤT, áp đại trà)

> **Mục đích:** hết "mỗi nơi mỗi kiểu / mò mẫm". Mọi task UI mở file này ra tra là làm được ngay — nhanh + nhất quán.
> **Owner chốt:** 4 trang **NỀN TẢNG** (chưa hoàn hảo nhưng nhất quán/đồng bộ nhất tới nay — bám theo để đỡ tốn thời gian):
> `/m1/san-pham` · `/m1/khach-hang` · `/m1/nhan-su` · **`/m5/kho-thanh-pham`** (GOLD — layout list/detail).
> **Rút TỪ CODE THẬT** (không bịa). Khi sửa UI: **tái dùng component + class chuẩn dưới đây, KHÔNG tự chế lại.**
> **Bắt buộc:** trước khi claim "đã đạt chuẩn" → CHỤP ẢNH đặt cạnh trang mẫu (cả list + panel MỞ + form MỞ).

---

## 0) 3 COMPONENT CHUẨN — DÙNG LẠI, ĐỪNG TỰ CHẾ

| Việc | Component chuẩn | File |
|---|---|---|
| Header trang | `PageHeader` | `src/components/foundation/page-header.tsx` |
| Section + field trong FORM | `FormSection` · `FormRow` · `FormFieldWrapper` · `FormPreview` | `src/components/ux/form-section.tsx` |
| List / bảng / detail panel | Theo mẫu | `src/app/m5/kho-thanh-pham/kho-thanh-pham-client.tsx` |

> Base UI: `src/components/ui/{button,input,badge,card,sheet,select}.tsx`. Token: `src/lib/design-tokens.css` + `src/app/globals.css` (`--radius: 0.5rem = 8px`, `--tp-brand: #F97316`, `--tp-radius-md: 6px`).

---

## 1) BO GÓC (radius) — bảng TỪNG PHẦN TỬ ⭐ (Owner nhắc kỹ: "bo góc nhẹ ở TẤT CẢ")

Thang: `rounded-sm/rounded`=4px · **`rounded-md`=6px** · `rounded-lg`=8px · `rounded-[10px]`=10px · `rounded-full`=tròn. **CẤM `rounded-xl`/`rounded-2xl` trên card.**

| Phần tử | Radius CHỐT |
|---|---|
| Card ngoài / khung bảng / khung toolbar+bảng+phân trang | **`rounded-md`** + `overflow-hidden` (để bo cả header bảng bên trong) |
| Panel chi tiết (khung ngoài) | **`rounded-md`** + `overflow-hidden` |
| Hero header (panel/form) | **`rounded-t-md`** (bo 2 góc trên) |
| Header trang — icon badge | **`rounded-md`** |
| Thẻ mục (section card) trong panel | **`rounded-md`** (⚠ khach-hang đang `rounded-[10px]` = nợ, đụng thì về md) |
| Section card trong FORM (`FormSection`) | `rounded-[10px]` (giữ nguyên component) |
| Header bảng — `th` đầu/cuối | `rounded-tl-md` / `rounded-tr-md` |
| Stat card | **`rounded-md`** |
| Stat box nhỏ trong panel | **`rounded-lg`** |
| Ô input / search / select filter | **`rounded-lg`** |
| Select trong form (native) | `rounded-md` (đồng bộ chiều cao với Input) |
| Nút (Button) / nút số phân trang | **`rounded-md`** |
| **Pill THỐNG KÊ ở HEADER** (Tổng/Doanh Nghiệp/… — dưới tiêu đề trang) | **`rounded-md`** ⚠ (chuẩn `/m1/khach-hang`: `rounded-md px-2 sm:px-3 py-0.5 sm:py-1 font-medium` · nền `bg-{c}-50/80` · viền `border-{c}-200` · số `font-bold` · icon `text-{c}-500`) — **KHÔNG dùng `rounded-full` cho pill header** |
| Badge trạng thái / loại **TRONG HÀNG bảng** (Doanh Nghiệp/Cá Nhân…) | **`rounded-full`** (chuẩn `/m1/khach-hang` td badge) |
| Tag mini ("CHÍNH"/"MẶC ĐỊNH") | `rounded` (4px) |
| Avatar / icon-circle (hero action, section circle) | **`rounded-full`** |

> **"Bo góc nhẹ" chuẩn cho mọi KHUNG/CARD/PANEL/HEADER/STAT/BẢNG + PILL THỐNG KÊ HEADER = `rounded-md` (6px)** + `overflow-hidden` để bo luôn phần tử con (header bảng, hero). Input/select-filter `rounded-lg`; avatar/icon-circle + badge-trong-hàng `rounded-full`.

---

## 2) MÀU SẮC (hex thật)

| Vai trò | Giá trị |
|---|---|
| **Brand (cam)** | `#F97316` (`primary`/orange-500) · hover `#EA580C` · active `#C2410C` · surface `#FFF7ED`/`#FFEDD5` · brand-text `#9A3412` |
| **Gradient header bảng (đặt trên `<tr>`)** | `linear-gradient(135deg, #ea580c 0%, #f97316 30%, #fb923c 100%)` — `<th>` **KHÔNG** set bg (để lộ gradient) |
| **Status** | chờ/draft = **amber** · đang chạy/xác nhận = **blue** · xong/đã giao/duyệt = **emerald** · huỷ/lỗi = **rose** · mặc định/nghỉ = **gray/slate** |
| **Text** | heading `#0F172A` (slate-900) · body `#334155` (slate-700) · muted `#64748B` (slate-500) · disabled `#94A3B8` |
| **Cấu trúc** | border `#E2E8F0` (slate-200) · viền nhạt trong card `#F3F4F6` (gray-100) · app bg `#F9F9F9` · card bg `#FFFFFF` |
| **Row hover/selected (M1 cam)** | hover `bg-[#fff7ed]`; selected `bg-[#fff7ed]` + `boxShadow: inset 3px 0 0 #f97316` |
| **Màu module (from→to)** | M0 orange→amber · M1 blue→indigo · M3 emerald→teal · M4 purple→pink · **M5 amber→yellow** · M6 cyan→sky · M7 pink→rose · M8 indigo→violet · MF red→rose |

### 6 THEME MÀU cho SECTION CARD trong PANEL (inline `style`)

| Theme | Header gradient (to right) | border-bottom | Circle bg | Title text |
|---|---|---|---|---|
| Orange | `#fff7ed → rgba(255,237,213,.5)` | `#fed7aa` | `#f97316` | `#9a3412` |
| Blue | `#eff6ff → rgba(219,234,254,.5)` | `#bfdbfe` | `#3b82f6` | `#1e40af` |
| Green | `#ecfdf5 → rgba(209,250,229,.5)` | `#a7f3d0` | `#10b981` | `#065f46` |
| Purple | `#faf5ff → rgba(243,232,255,.5)` | `#e9d5ff` | `#a855f7` | `#6b21a8` |
| Rose | `#fff1f2 → rgba(255,228,230,.5)` | `#fecdd3` | `#f43f5e` | `#9f1239` |
| Amber | `#fffbeb → rgba(254,243,199,.5)` | `#fde68a` | `#f59e0b` | `#92400e` |
| Indigo | `#eef2ff → rgba(224,231,255,.5)` | `#c7d2fe` | `#6366f1` | `#3730a3` |

---

## 3) ICON
- Thư viện **`lucide-react`** (line). KHÔNG emoji trong UI/data.
- Kích thước: nút/cell/label phụ `h-4 w-4` · status/icon lớn trong cell `h-5 w-5` · icon-badge header `h-5 w-5` (badge `h-10 w-10`) · icon trong circle section `h-3 w-3` · nút tròn hero `h-3.5 w-3.5`.

## 4) KHUNG TRANG (page shell)
- **KHÔNG `max-w` / `mx-auto`** — full width. Shell `main-layout` ĐÃ có gutter `lg:px-4` + `max-w-430` → **CẤM double-pad** (trang tự thêm `px-*` = cộng dồn lề, lỗi hay gặp).
- Wrapper ngoài: `space-y-2.5/4 pt-1/3` (KHÔNG px ngang) — bảng sát mép shell 16px.
- Header trang: dùng `PageHeader` (`px-0 sm:px-6 pt-3 pb-2.5`) hoặc header phẳng `px-1`.

## 5) HEADER TRANG (dùng `PageHeader`)
- Icon badge **`h-10 w-10 rounded-md`** + gradient module + `shadow-md` (hoặc pastel `bg-{c}-100 text-{c}-600` cho nhẹ), icon `h-5 w-5`.
- Title **`text-xl font-bold text-gray-900 sm:text-[22px]`** (≈ md:text-2xl). **CẤM text-3xl/h-14/bọc card.**
- Subtitle `text-sm text-muted-foreground` (ẩn mobile `hidden sm:block`).
- **Pill thống kê ở HÀNG RIÊNG ngay dưới title** (`mt-1.5`), không chen cùng hàng title.
- Nút hành động chính: `<Button className="h-8 px-3 text-[12px] sm:h-9 sm:px-4 sm:text-sm">` + `<Plus className="mr-1.5 h-3.5 w-3.5 sm:mr-2 sm:h-4 sm:w-4"/>`.

## 6) STAT / PILL thống kê
**Variant A — Header PILL (list gọn) — ⚠ KHỚP §1, RÚT TỪ `khach-hang-client.tsx:379`:**
```
inline-flex items-center gap-1 sm:gap-1.5 rounded-md px-2 sm:px-3 py-0.5 sm:py-1 font-medium
text-{c}-700 bg-{c}-50/80 border border-{c}-200 whitespace-nowrap
  + icon h-3 w-3 sm:h-3.5 sm:w-3.5 text-{c}-500
  + số bọc <span className="font-bold">
```
Hàng pill riêng `mt-1.5 flex items-center gap-1.5 sm:gap-3 text-[11px] sm:text-[13px]`. Màu: tổng=blue, đang=amber/orange, xong=emerald.
> ⛔ **`rounded-md`, KHÔNG `rounded-full`** — xem §1 dòng "Pill THỐNG KÊ ở HEADER". Bo tròn hẳn chỉ dùng cho **badge TRONG HÀNG bảng** (§1) và avatar/icon-circle.
**Variant B — Stat CARD (trang có KPI, mẫu kho):** `rounded-md border bg-white p-4` + thanh accent `absolute inset-x-0 top-0 h-0.75` (gradient) + icon box `h-10 w-10 rounded-lg bg-{c}-50` + value `text-xl font-bold tabular-nums`. Grid `grid-cols-2 gap-3 lg:grid-cols-4`.

## 7) TOOLBAR tìm/lọc — NẰM TRONG CARD BẢNG (child đầu, `border-b`)
```
flex flex-wrap items-center gap-2 border-b border-gray-100 px-3 sm:px-4 py-2.5
```
- Search: `relative flex-1 min-w-45 sm:max-w-[320px]` + `<Search className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-gray-400"/>` + input `h-9 rounded-lg pl-9 focus:ring-[3px] focus:ring-orange-400/10`.
- Select filter `h-9 rounded-lg`. Reset (khi có filter) `h-9 w-9 rounded-lg`. View toggle List/Grid `flex border rounded-lg` (nút `w-9 h-9`, active `bg-orange-50 text-orange-600`), cụm `ml-auto`.
- Đếm kết quả `ml-auto text-[12px] text-muted-foreground`.

## 8) BẢNG (table)
- Khung: **[toolbar + bảng + phân trang] trong 1 CARD** `overflow-hidden rounded-md border bg-white shadow-sm`.
- Vùng cuộn 1 lớp: `max-h-[max(240px,calc(100dvh-280px))] overflow-auto` (trang có stat card: `-360px`; giao-hang list gọn: `-235px`). **CẤM lồng 2 lớp cuộn x/y.**
- `table w-full min-w-150 lg:min-w-215 text-[12.5px]`.
- `thead tr` gradient 3-stop (§2). `th`: `sticky top-0 z-30 h-10 px-4 text-left text-[11px] font-bold text-white uppercase tracking-[0.04em] whitespace-nowrap` (đầu/cuối thêm `rounded-tl-md`/`rounded-tr-md`). Nút sort: `inline-flex items-center gap-1 hover:text-orange-100` + `ArrowUp/Down/UpDown h-3 w-3`.
- `tr`: `border-b border-gray-100 cursor-pointer transition-colors` + hover/selected (§2). `td`: `px-4 py-2.5 align-middle text-[12.5px] text-gray-700`; mã `font-mono font-semibold`; số `text-right font-mono tabular-nums`.
- Empty: `py-12 text-center text-sm text-muted-foreground` + icon.

## 9) PHÂN TRANG (footer trong card, `border-t`)
```
flex flex-wrap items-center justify-between gap-3 border-t border-gray-100 px-4 sm:px-5 py-2.5
```
- Trái `text-[12px] text-gray-500`: "Hiển thị {n} / {total} bản ghi".
- Giữa `flex items-center gap-0.5`: prev/next + số trang `w-7 h-7 rounded-md text-[11px]`; active `text-white` + `style={{background:'#f97316'}}`; inactive `border bg-white text-gray-500`.
- Phải: "Hiển thị `<select className="h-7 rounded px-1.5 text-[11px]">`10/25/50`</select>` / trang". Default 10 (M1) / 25 (kho).

## 10) PANEL CHI TIẾT (in-grid, KHÔNG drawer overlay)
- Grid: `grid gap-4 ${sel ? "xl:grid-cols-[minmax(0,1fr)_380px]" : ""}` (detail **380px cố định**, list `minmax(0,1fr)`; mobile stack xuống dưới). Khung panel `overflow-hidden rounded-md border bg-white shadow-lg flex flex-col lg:sticky lg:top-4`.
- **Hero "name-on-top"**: `bg-linear-to-r from-orange-400 to-orange-500 px-4 sm:px-5 py-3.5 sm:py-4 rounded-t-md`. Hàng 1: `<h2 className="flex-1 min-w-0 text-lg font-bold text-white leading-snug wrap-break-word">` + **cụm nút tròn** phải. Hàng 2 (`mt-2 flex flex-wrap gap-1.5`): badges.
  - Nút tròn (`h-8 w-8 rounded-full flex items-center justify-center transition`): Sửa `bg-white/90 text-orange-600 hover:bg-white shadow-sm`; Xóa `bg-red-500/80 text-white hover:bg-red-600 shadow-sm`; Đóng `bg-white/20 text-white hover:bg-white/30`. Icon `h-3.5 w-3.5`.
  - Badge hero: `inline-flex items-center gap-1 bg-white/20 text-white text-[11px] font-medium px-2 py-0.5 rounded-full`; mã thêm `font-mono`.
- Thân `p-3/4 space-y-2.5/3`:
  - **Section card màu**: `overflow-hidden rounded-md border border-gray-200`; header `flex items-center gap-2 px-3 py-2` + `style={{background:<gradient §2>, borderBottom:'1px solid <hex>'}}`; circle `h-5.5 w-5.5 rounded-full` (bg hex) + icon `h-3 w-3`; title `text-[12.5px] font-bold` (color hex); body `p-3`.
  - Có đếm/collapsible: header là `<button w-full justify-between>` + badge đếm `min-w-4.5 h-4.5 rounded-full text-[10px] font-bold text-white` + `ChevronDown h-3.5 w-3.5`.
  - **Stat box** (Số Lượng/Giá Trị): `grid grid-cols-2 gap-3`; ô `rounded-lg border border-{c}-200 bg-{c}-50 p-3 text-center`; số `text-xl font-bold tabular-nums text-{c}-700`; label `text-[11px] text-{c}-500`.
  - Info rows: `dl grid grid-cols-2 gap-x-4 gap-y-2.5 text-sm`; `dt text-[11px] text-slate-400`; `dd font-medium text-slate-700`.
  - Sub-card (dòng con): `flex items-center justify-between p-3 bg-gray-50 rounded-lg border border-gray-100` + nút sửa/xóa tròn nhỏ `h-7 w-7 rounded-full bg-{c}-50 border`.
  - Audit footer: `border-t border-gray-100 pt-2.5 space-y-0.5 text-[11px] text-slate-400`.

## 11) FORM (Sheet trượt phải)
- `SheetContent`: `w-full p-0 flex flex-col sm:max-w-120` (480px) hoặc `sm:max-w-2xl`.
- **Header chuyển sắc sticky**: `sticky top-0 z-10 flex items-center gap-2.5 rounded-t-md bg-linear-to-r from-orange-500 to-orange-600 px-5 py-4 text-white` + icon box `h-9/10 w-9/10 rounded-md bg-white/20` + `SheetTitle text-white font-bold` + subtitle `text-[12px] text-orange-100`.
- **Chia SECTION màu**: dùng `<FormSection colorTheme="orange|blue|green|purple|rose" icon title>` (card `rounded-[10px]`, header `border-l-[3px] from-{c}-50`, icon-circle `h-5.5 w-5.5`, title `text-[12.5px] font-bold`, body `space-y-3.5 px-4 py-3.5`). Field ngắn ghép `<FormRow>` (`grid md:grid-cols-2 gap-4`).
- Field: `<FormFieldWrapper label required hint error>`; label `text-[11.5px] font-semibold text-gray-700`; Input `h-9 rounded-lg` focus `ring-[3px] ring-orange-400/10`.
- **Footer sticky**: `sticky bottom-0 border-t bg-gray-50 px-5 py-3 flex justify-end gap-2` — Hủy (`outline`) + Submit (primary/gradient `h-9`).
- **Cảnh báo trước submit** (nếu có): `flex items-center gap-2 bg-orange-50 border border-orange-200 rounded-lg p-3` + `AlertTriangle h-4 w-4 text-orange-600`.
- **G2 bắt buộc**: `useFormDirtyTracker` + `useSafeClose` + `ConfirmExitDialog` + `ConfirmActionDialog variant="delete"`; chặn `onInteractOutside/onEscapeKeyDown` khi dirty.

## 12) BUTTON / BADGE / INPUT
- **Button** (`button.tsx`): base `rounded-md text-sm font-medium`; variant default/outline/ghost/destructive; size `sm h-9 px-3` · `default h-10 px-4` · `icon h-10 w-10`. Header dùng `h-8 sm:h-9`.
- **Badge**: `rounded-full border px-2.5 py-0.5 text-xs font-semibold`. Trạng thái inline: `inline-flex items-center gap-1 rounded-full px-2.5 py-1 text-[10.5px] font-semibold text-{c}-700 bg-{c}-50 border border-{c}-200`. Chip mã: `rounded-md bg-blue-50 px-2 py-0.5 text-[11px] font-semibold text-blue-700 ring-1 ring-inset ring-blue-600/20`.
- **Input** (`input.tsx`): `h-9 rounded-lg border-[#e2e8f0] bg-white px-3 text-[12.5px]` focus `ring-2 ring-orange-500/10 border-orange-500`; placeholder `#94a3b8`.

## 13) TYPOGRAPHY & TITLE CASE
- Font **Inter**. **`toVietnameseTitleCase()`** (`src/lib/text-helpers.ts`) cho MỌI label/heading/table/tên — KHÔNG áp cho mã/ID/email/SĐT.

## 14) RESPONSIVE
- **`min-w-0`** bắt buộc trên child grid chứa bảng (thiếu → cột nở theo bảng, `overflow-hidden` cắt mất, không cuộn ngang được mobile).
- `dvh`: bảng `calc(100dvh-280px)`; panel `-120px`.
- Grid chính: `grid gap-4 ${sel ? "xl:grid-cols-[minmax(0,1fr)_380px]" : ""}`; panel **stack xuống dưới** mobile (không drawer riêng).
- min-w bảng theo breakpoint: `min-w-150` mobile → `lg:min-w-215`. Ẩn cột: `hidden sm:table-cell`/`lg:table-cell`.
- Co giãn: title `text-xl sm:text-[22px]`; pill/nút `h-8 sm:h-9`. Viết tắt mobile: `<span className="hidden sm:inline">Doanh Nghiệp</span><span className="sm:hidden">DN</span>`.

---

## 15) CHEAT-SHEET — dựng nhanh 1 màn hình
1. **Shell**: wrapper `space-y-2.5` (KHÔNG px — shell lo lề). CẤM `max-w`/`mx-auto`.
2. **Header**: `PageHeader` (icon `rounded-md`, title `text-xl sm:text-[22px]`) + hàng pill thống kê **`rounded-md`** riêng dưới title (§1 · §6).
3. **List**: 1 CARD `overflow-hidden rounded-md border` = [toolbar `border-b` (search `max-w-320` `rounded-lg`) → bảng gradient 3-stop `th uppercase` → phân trang footer `border-t`].
4. **Detail**: grid `xl:grid-cols-[minmax(0,1fr)_380px]`; panel `rounded-md overflow-hidden` = [hero name-on-top `rounded-t-md` + nút tròn + badges] + [section card màu §10] + [stat box] + [audit footer].
5. **Form**: Sheet `p-0` + header gradient sticky `rounded-t-md` + `FormSection colorTheme` + `FormRow`/`FormFieldWrapper` + footer sticky.
6. **Bo góc**: card/panel/header/stat/bảng + **pill thống kê header** = `rounded-md` + `overflow-hidden`; input/select `rounded-lg`; **badge trong hàng bảng** + avatar/icon-circle = `rounded-full`. **Chụp ảnh cạnh mẫu trước khi claim.**

## 16) NỢ KỸ THUẬT ĐÃ BIẾT (đổi khi đụng, không sửa hàng loạt vô cớ)
- `khach-hang` section card `rounded-[10px]` → về `rounded-md`. **Header-pill của `khach-hang` ĐÃ ĐÚNG (`rounded-md`) — là CHUẨN, KHÔNG phải nợ, CẤM đổi sang `rounded-full`.**
- `kho`/`nhan-su` `th` phủ `bg-(--tp-brand)` che gradient → bỏ bg trên th.
- `san-pham` hero `from-orange-600 to-orange-400` + form footer không sticky + dùng `PremiumSection` thay `FormSection` → hợp nhất.
- Header cũ `text-3xl`/`h-14` (M3 thiết-kế, don-hang) → về `PageHeader`.
- Native select form `h-10 rounded-md` lệch Input `h-9 rounded-lg` → đồng bộ.

---

## 17) ĐỊNH DẠNG NGÀY · TIỀN · SỐ (D4 — bổ sung 18/08/2026, trước đây KHÔNG nguồn nào trong SSOT phủ)

### 17.1 NGÀY — chuẩn `DD/MM/YYYY`

| Việc | Chuẩn | Helper / bằng chứng code |
|---|---|---|
| Ngày hiển thị (bảng · panel · form · in/xuất) | **`DD/MM/YYYY`** (ngày & tháng 2 chữ số, năm 4 chữ số) | `formatDueDate()` — `src/lib/due-state.ts:234–243` (pad `getDate`/`getMonth`, `getFullYear`) |
| Ngày + giờ (audit, lịch sử) | **`DD/MM/YYYY HH:mm`** | mục 20.6 (gộp từ kỹ năng `audit-ui`) |
| Ngày trong ô nhập | ISO `YYYY-MM-DD` (giá trị `<input type="date">`), **hiển thị** vẫn `DD/MM/YYYY` | — |

- **CẤM** render trực tiếp đối tượng `Date` ra JSX → gây lệch múi giờ server/client (Hydration Error). Luôn đổi sang chuỗi trước. *(gộp từ kỹ năng `ui-components-usage` §1)*
- ⚠️ **NỢ ĐÃ BIẾT — code hiện KHÔNG nhất quán, đụng đâu sửa đó về `DD/MM/YYYY`:** `toLocaleDateString("vi-VN")` trơn (**16 chỗ**) cho ra `D/M/YYYY` không pad; biến thể `{day:"2-digit",month:"2-digit",year:"numeric"}` (**2 chỗ**) đúng chuẩn; biến thể `year:"2-digit"` (**2 chỗ**) cho ra `DD/MM/YY` — **sai chuẩn**.
- **`DD/MM/YY` (2 chữ số năm) là SUPERSEDED** — xem 20.7 (phân xử với `.governance/ARCHIVE-LEGACY-RULESET.md:153`). Owner đã chốt "ngày đầy đủ" (Sổ Yêu Cầu Owner #65, #67).

### 17.2 TIỀN & SỐ — chuẩn VN `1.234.567,89`

| Việc | Chuẩn |
|---|---|
| Phân cách hàng nghìn | dấu **chấm** `.` |
| Phân cách thập phân | dấu **phẩy** `,` |
| Cách làm | `Number(x).toLocaleString("vi-VN")` — KHÔNG tự cắt chuỗi |
| Ô nhập | cho nhập `,` hoặc `.` linh hoạt, **normalize về chuẩn trên khi render** |
| Cột số trong bảng | `text-right font-mono tabular-nums` (§8) |
| Cột **Tổng Tiền** — nhấn mạnh | `text-right` + `font-bold text-orange-600` *(gộp từ `premium-table-styling` §3)* |
| Đơn vị tiền | ghi `đ` sau số, cách một khoảng trắng |

> Chuẩn này **KHỚP** `.governance/ARCHIVE-LEGACY-RULESET.md:154–157` và khớp code (`toLocaleString("vi-VN")`) → không có xung đột.

---

## 18) TRẠNG THÁI HIỂN THỊ: ĐANG TẢI · RỖNG · LỖI · KHÔNG CÓ QUYỀN (D4 — bổ sung 18/08/2026, trước đây KHÔNG nguồn nào phủ)

Bốn trạng thái này **bắt buộc có** trên mọi trang list/detail. Thiếu một trạng thái = trang chưa xong.

| Trạng thái | Chuẩn hiển thị | Bằng chứng code |
|---|---|---|
| **ĐANG TẢI** | `Loader2` (lucide) + `animate-spin`, `h-4 w-4` trong nút / `h-6 w-6` giữa vùng nội dung; nút đang gửi thì `disabled` + đổi nhãn ("Đang lưu…") | mẫu dùng nhiều nhất trong dự án: `Loader2` **61 chỗ** · `animate-spin` **44 chỗ**. `Skeleton` chỉ 2 chỗ → **KHÔNG phải chuẩn**, đụng đâu đổi về `Loader2` |
| **RỖNG** (không có dữ liệu) | `py-12 text-center text-sm text-muted-foreground` + icon lucide mờ phía trên + một câu gợi hành động ("Chưa có phiếu nào — bấm *Tạo mới* để bắt đầu") | §8 (đã có, nay bổ sung câu gợi hành động) |
| **LỖI** (tải/ghi thất bại) | Khung `flex items-center gap-2 rounded-lg border border-red-200 bg-red-50 p-3` + `AlertTriangle h-4 w-4 text-red-600` + câu lỗi **tiếng Việt, nói được việc gì thất bại** + nút "Thử lại". **CẤM** `alert()`/`confirm()` — dùng toast/dialog theo G2 | mẫu cảnh báo ở §11; quy tắc cấm `alert()` gộp từ kỹ năng `audit-ui` |
| **KHÔNG CÓ QUYỀN** | Điều hướng sang trang `/403`, **không** tự vẽ lại: khung `rounded-md border border-slate-200 bg-white p-8` + badge `h-12 w-12 rounded-md bg-red-100` + `ShieldX h-6 w-6 text-red-600` + tiêu đề `text-2xl font-bold text-slate-900` + 2 nút điều hướng | `src/app/403/page.tsx:1–38` |

> **Ẩn theo quyền ≠ báo lỗi.** Nút/cột người dùng không có quyền → **ẩn hẳn** (không hiện rồi báo lỗi khi bấm). Chỉ dùng trang `/403` khi cả trang bị chặn.

---

## 19) COMBOBOX / SELECT — CHỌN THEO **TÊN** + CÓ TÌM KIẾM (D4 — bổ sung 18/08/2026, trước đây chỉ có trong kỹ năng `.cursor/skills/` mà Claude Code không đọc được)

| Quy tắc | Nội dung |
|---|---|
| **Hiển thị theo TÊN** | Ô chọn danh mục **chỉ hiện TÊN** cho người dùng. **CẤM** hiện mã/ID trong nhãn (`KH001 - Công Ty A` → chỉ `Công Ty A`) |
| **Giá trị gửi đi là ID** | Nhãn hiện tên, nhưng `value` gửi về server là **`id`**, KHÔNG phải tên (khớp `GOV-SCHEMA-NO-INVENT-001`: FK tham chiếu bằng id) |
| **Có tìm kiếm khi danh sách dài** | ≥ **10 lựa chọn** → bắt buộc có ô tìm kiếm gõ-để-lọc, lọc **không phân biệt dấu và hoa/thường** |
| **Nguồn lựa chọn** | Lấy từ server (danh mục thật). **CẤM dữ liệu giả / mock** trong danh sách chọn |
| **Kích thước** | `h-9 rounded-lg` — **đồng bộ với `Input`** (§12). Native select trong form: `rounded-md` + cùng chiều cao Input (§1) |
| **Z-index trong Sheet/Dialog** | `<SelectContent position="popper" sideOffset={5}>`; nếu bị che → kiểm `z-index` Portal hoặc `container={document.body}` *(gộp từ `ui-components-usage` §3)* |
| **Rỗng / đang tải** | Danh sách rỗng → "Chưa có dữ liệu"; đang tải → `Loader2` trong ô (§18) |

---

## 20) GỘP TỪ 11 KỸ NĂNG `.cursor/skills/` (D3 — 18/08/2026)

> **Vì sao phải gộp:** 11 kỹ năng UI nằm trong `.cursor/skills/`, mỗi cái tự xưng "SSOT". Công cụ thi hành ca 17–18/08 là Claude Code — nó **KHÔNG đọc** `.cursor/skills/` (thư mục `.claude/` chỉ có `settings.local.json`). Nội dung đúng nay gộp vào đây; **thư mục kỹ năng gốc GIỮ NGUYÊN làm lưu trữ, không xoá, không sửa**.

### 20.1 BẢNG ĐỐI CHIẾU KỸ NĂNG → MỤC ĐÃ GỘP

| # | Kỹ năng (`.cursor/skills/…/SKILL.md`) | Nội dung đã có sẵn ở | Nội dung MỚI gộp vào | Xung đột? |
|---|---|---|---|---|
| 1 | `master-list-page-template` | §4 §5 §7 §8 §9 §10 | — | ⚠️ **3 xung đột** → 20.7 |
| 2 | `detail-panel-layout` | §10 | **20.2** (tương tác toggle dòng) | ⚠️ 1 xung đột → 20.7 |
| 3 | `premium-table-styling` | §8 §14 | **17.2** (cột Tổng Tiền) · **20.3** (ô đầu dòng theo màu trạng thái) | — |
| 4 | `module-color-palette` | §2 (map màu module) | — | ⚠️ 1 xung đột → 20.7 |
| 5 | `ui-typography` | §13 §8 | — | — |
| 6 | `status-color-mapping` | §2 (nhóm màu trạng thái) | **20.4** (icon theo trạng thái) | — |
| 7 | `icon-style-guideline` | §3 | **20.5** (phong cách bị cấm + bảng icon) | — |
| 8 | `ui-components-usage` | §11 §12 | **17.1** (cấm render Date) · **19** (z-index select) · **18** (alert box) | — |
| 9 | `audit-ui` | — | **20.6** (mẫu audit trên dòng) | — |
| 10 | `screenshot-verification` | §0 (câu "chụp ảnh cạnh mẫu") | **20.8** (quy trình chụp & đối chiếu) | — |
| 11 | `annotated-screenshot-review` | — | **20.9** (đọc ảnh Owner khoanh đỏ) | — |

### 20.2 TƯƠNG TÁC TOGGLE DÒNG — BẮT BUỘC *(từ `detail-panel-layout` §2)*

| Thao tác | Kết quả |
|---|---|
| Bấm dòng (panel chưa mở) | Mở panel chi tiết của dòng đó |
| **Bấm lại đúng dòng đang chọn** | ❗ **ĐÓNG** panel (toggle) — bắt buộc có |
| Bấm dòng khác | Chuyển sang chi tiết dòng mới |
| Bấm nút `X` trên hero | Đóng panel |

```tsx
const handleViewDetail = (item) => {
  if (selected?.id === item.id) { setSelected(null); return }   // toggle off — BẮT BUỘC
  setSelected(item)
}
```

### 20.3 Ô ĐẦU DÒNG THEO MÀU TRẠNG THÁI *(từ `premium-table-styling` §1)*

Cột định danh đầu tiên = `flex items-center gap-3` gồm: hộp icon **`h-10 w-10 rounded-md`** (⚠ kỹ năng gốc ghi `rounded-xl` — **SAI**, xem 20.7) nền theo màu trạng thái (§2) + mã `font-mono font-semibold` + icon `History` cho audit (20.6).

### 20.4 ICON THEO TRẠNG THÁI *(từ `status-color-mapping` §5)*

| Trạng thái | Màu (§2) | Icon lucide |
|---|---|---|
| nháp / draft | amber | `FileText` |
| đang gửi / đang xử lý / đang sản xuất | blue | `Send` |
| đã duyệt / chấp nhận / hoàn thành / đã giao | emerald | `CheckCircle` |
| huỷ / từ chối / hết hạn / lỗi | rose | `XCircle` |
| chờ / đợi | amber | `Clock` |
| khác (mặc định) | gray/slate | `AlertCircle` |

### 20.5 PHONG CÁCH ICON BỊ CẤM *(từ `icon-style-guideline`)*

- ❌ **Emoji trong UI** (`🏢` `👤` `📄` `📊`) — cấm tuyệt đối, kể cả trong dữ liệu.
- ❌ **Emoji bọc trong vòng tròn màu** — cấm.
- ❌ **Chấm tròn đặc** (`h-2 w-2 rounded-full bg-…`) — cấm, **trừ** chỉ báo trạng thái.
- ✅ Icon lucide **đặt trực tiếp** trong badge/pill, **KHÔNG** bọc thêm vòng tròn màu.

| Ý nghĩa | Icon | Ý nghĩa | Icon |
|---|---|---|---|
| Tổng / Tất cả | `Users` | Địa chỉ | `MapPin` |
| Doanh Nghiệp | `Building2` | Website | `Globe` |
| Cá Nhân | `User` | Sửa | `Pencil` |
| Điện thoại | `Phone` | Xoá | `Trash2` |
| Email | `Mail` | Thêm mới | `Plus` |
| Tìm kiếm | `Search` | Danh sách / Lưới | `List` / `LayoutGrid` |

### 20.6 MẪU AUDIT TRÊN DÒNG *(từ `audit-ui`)*

- Điểm vào: icon **`History`** (lucide) đặt **cạnh mã** của dòng.
- Bấm: **bắt buộc `e.stopPropagation()`** — không được kích hoạt chọn dòng.
- Mở `Dialog` tiêu đề **"Thông Tin Audit"**, read-only: **Ngày Tạo · Người Tạo** và **Ngày Sửa · Người Sửa**.
- Ngày trong audit: **`DD/MM/YYYY HH:mm`** (§17.1). **CẤM `alert()`/`confirm()`** — dùng dialog/toast theo G2.
- Mẫu tham chiếu: `src/app/m0/dm-nhom-universal/dm-nhom-universal-client.tsx`.

### 20.7 ⚖️ PHÂN XỬ XUNG ĐỘT — **SSOT + CODE ĐANG CHẠY THẮNG**

| # | Nguồn | Nguồn nói gì | SSOT/code nói gì | Bên thắng | Lý do |
|---|---|---|---|---|---|
| 1 | `master-list-page-template` §2 (dòng 57–70) | pill thống kê header = **`rounded-full`** | §1 · §6 = **`rounded-md`** | **SSOT** | Đây là **vị trí thứ 6** của đúng đặc tả sai đã gây lượt Owner bác #75. Code mẫu `khach-hang-client.tsx:379` dùng `rounded-md` |
| 2 | `master-list-page-template` §4 (dòng 129) | khung bảng **`rounded-xl`** (12px) | §1 dòng 25: **CẤM `rounded-xl`/`rounded-2xl` trên card**; khung bảng = `rounded-md` | **SSOT** | Owner chốt "bo góc nhẹ" = 6px (Sổ #73) |
| 3 | `master-list-page-template` §4 (dòng 130) | `max-h-[calc(100vh-280px)]` | §8: `max-h-[max(240px,calc(100dvh-280px))]` | **SSOT** | `dvh` xử lý đúng thanh địa chỉ trên điện thoại |
| 4 | `master-list-page-template` §4 + `module-color-palette` | **M5 = Purple** `#7e22ce→#a855f7→#c084fc` | §2: M5 = **amber→yellow**; và đầu bảng M5 dùng **gradient cam 3 chặng** (V1.00.348/349, 8 trang) | **SSOT** | Đã áp cam cho toàn bộ 10 trang M5, có ảnh kiểm thử |
| 5 | `detail-panel-layout` §1 (dòng 25–72) | hero `from-{c}-600 to-{c}-500`, **mã ở trên**, tên ở dưới | §10: `from-orange-400 to-orange-500`, **tên trên cùng** (name-on-top), mã xuống hàng badge | **SSOT** | Chính kỹ năng đó ở §1.1 đã ghi biến thể name-on-top là **RECOMMENDED (Owner-approved)** — §1 là bản cũ hơn của cùng kỹ năng |
| 6 | `master-list-page-template` §1 | grid `lg:grid-cols-3` (bảng 2/3 + panel 1/3) | §10: `xl:grid-cols-[minmax(0,1fr)_380px]` (panel **380px cố định**) | **SSOT** | Panel tỉ lệ 1/3 phình quá rộng ở màn lớn |
| 7 | `.governance/ARCHIVE-LEGACY-RULESET.md:153` | ngày = **`DD/MM/YY`** | §17.1: **`DD/MM/YYYY`** | **SSOT** | Owner chốt "ngày đầy đủ" (Sổ #65, #67); helper `formatDueDate()` trả năm 4 chữ số |
| 8 | `.governance/ARCHIVE-LEGACY-RULESET.md:2021` | `<th>` sticky dùng nền **opaque** (`bg-muted`/`bg-primary/10`) | §2 dòng 55 + §8 dòng 113: `<th>` **KHÔNG** set nền, để lộ gradient 3 chặng | **SSOT** | Gradient cam đầu bảng đã áp 10/10 trang M5 + 4 trang nền tảng |
| 9 | `.governance/ARCHIVE-LEGACY-RULESET.md:2036–2038` | nút thao tác **bên trái**, tên **bên dưới** hàng nút | §10: tên **trên cùng**, cụm nút tròn **bên phải** | **SSOT** | Owner nghiệm thu "ổn rồi đó" (Sổ #76) trên bản name-on-top |
| 10 | `.governance/ARCHIVE-LEGACY-RULESET.md:2011` | thanh lọc đặt ở **khu riêng phía trên** bảng | §7: thanh lọc **nằm TRONG card bảng** (con đầu, `border-b`) | **SSOT** | Owner yêu cầu đích danh (Sổ #72 điểm 3) |
| 11 | `docs/ke-thua-antigravity/TANPHAT_UI_STANDARD.md:26,28,29,34` | card 12px · lề trang 30px · dòng bảng 52px · lưới 8px | §1 `rounded-md` 6px · §4 lề = shell 16px · §8 `py-2.5` · §7 `gap-2`/`px-3` | **SSOT** | Bản 27/04/2026 theo nền tảng trả phí, dự án đã không đi hướng đó (xem Q3) |

> **Xung đột #1 và #2 KHÔNG được sửa trong file kỹ năng gốc** — thư mục `.cursor/skills/` giữ nguyên làm lưu trữ theo `GOV-EDIT-PRESERVE-001`. Bảng này là **con trỏ phân xử**: khi hai bên lệch, **luôn theo SSOT**.

### 20.8 QUY TRÌNH CHỤP & ĐỐI CHIẾU ẢNH *(từ `screenshot-verification`)*

1. Mở đúng route cần kiểm (đăng nhập trước nếu route yêu cầu).
2. Chụp ở **3 kích thước**: 1920 (desktop) · ~1024 (tablet) · ~390 (điện thoại).
3. Chụp **cả trạng thái MỞ**: list + panel chi tiết MỞ + form MỞ. Chụp riêng từng vùng khi cần so chi tiết.
4. **Đặt cạnh trang mẫu** cùng kích thước → lập bảng đối chiếu từng mục (bo góc · lề · mật độ dòng · vị trí pill · màu đầu bảng).
5. Nêu **từng điểm lệch**; nếu 0 điểm lệch thì nói rõ "0 điểm lệch", không nói "trông ổn".
6. Lưu vào `docs/anh-kiem-thu/<nhóm>/` (đã gitignore — **ảnh có thể chứa dữ liệu thật, KHÔNG đẩy lên kho công khai**).
7. ⚠️ **Chụp ảnh KHÔNG phải so sánh hồi quy.** Hiện dự án **chưa có** ảnh nền + so pixel + ngưỡng tự động → bước "visual regression" **chưa thực hiện được**, cấm khai là đã có.

### 20.9 ĐỌC ẢNH OWNER KHOANH ĐỎ *(từ `annotated-screenshot-review`)*

- **Đọc HẾT** mọi ảnh Owner gửi — cấm đọc sơ 1–2 ảnh rồi làm.
- Mọi khoanh vùng (vuông/tròn/oval đỏ) + mũi tên + chú thích → **tách thành danh sách việc rõ ràng**, đánh số, đối chiếu lại sau khi sửa.
- **Sửa đúng vùng Owner chỉ** — không tự suy diễn, không "làm lan" sang chỗ khác.
- Trả lời: liệt kê từng điểm khoanh → đã xử lý thế nào → bằng chứng ảnh nào.

---

## 21) LỊCH SỬ SỬA ĐỔI (`GOV-EDIT-PRESERVE-001` — dòng cũ KHÔNG mất)

> **Doc Version:** `2.0` (18/08/2026). Bản trước: `1.1` (18/08/2026 10:52, vá §1) · `1.0` (17/08/2026 21:02, viết lại 16 mục) · bản gốc 11/08/2026.

| Ngày | Vị trí | Lý do | Người duyệt | Dòng cũ (nguyên văn) — đã được thay bởi |
|---|---|---|---|---|
| 18/08/2026 | §6 Variant A — Header PILL | Đặc tả **pill thống kê ở HEADER** tự mâu thuẫn trong cùng file: §1 ghi `rounded-md` (đúng, khớp code mẫu) còn §6/§15/§16 vẫn ghi `rounded-full` (sai). Sai này đã gây lượt Owner bác #75 (V1.00.345 → V1.00.347). V1.00.347 chỉ vá §1, để sót 4 vị trí → nay vá HẾT trong MỘT lượt theo `GOV-EDIT-PRESERVE-001` yêu cầu 2 | Owner duyệt 18/08/2026 | `**Variant A — Header PILL (list gọn):**<br>```<br>inline-flex items-center gap-1 rounded-full px-2.5 py-1 text-[11px] font-semibold sm:text-[13px]<br>text-{c}-700 bg-{c}-50 border border-{c}-200   + icon h-3.5 w-3.5<br>```<br>Hàng pill riêng `mt-1.5 flex flex-wrap items-center gap-1.5`. Màu: tổng=blue/orange, đang=amber, xong=emerald.` |
| 18/08/2026 | §15 mục 2 — Header | Đặc tả **pill thống kê ở HEADER** tự mâu thuẫn trong cùng file: §1 ghi `rounded-md` (đúng, khớp code mẫu) còn §6/§15/§16 vẫn ghi `rounded-full` (sai). Sai này đã gây lượt Owner bác #75 (V1.00.345 → V1.00.347). V1.00.347 chỉ vá §1, để sót 4 vị trí → nay vá HẾT trong MỘT lượt theo `GOV-EDIT-PRESERVE-001` yêu cầu 2 | Owner duyệt 18/08/2026 | `2. **Header**: `PageHeader` (icon `rounded-md`, title `text-xl sm:text-[22px]`) + hàng pill `rounded-full` riêng dưới title.` |
| 18/08/2026 | §15 mục 6 — Bo góc | Đặc tả **pill thống kê ở HEADER** tự mâu thuẫn trong cùng file: §1 ghi `rounded-md` (đúng, khớp code mẫu) còn §6/§15/§16 vẫn ghi `rounded-full` (sai). Sai này đã gây lượt Owner bác #75 (V1.00.345 → V1.00.347). V1.00.347 chỉ vá §1, để sót 4 vị trí → nay vá HẾT trong MỘT lượt theo `GOV-EDIT-PRESERVE-001` yêu cầu 2 | Owner duyệt 18/08/2026 | `6. **Bo góc**: card/panel/header/stat/bảng = `rounded-md` + `overflow-hidden`; input/select `rounded-lg`; pill/avatar `rounded-full`. **Chụp ảnh cạnh mẫu trước khi claim.**` |
| 18/08/2026 | §16 Nợ kỹ thuật — dòng khach-hang | Đặc tả **pill thống kê ở HEADER** tự mâu thuẫn trong cùng file: §1 ghi `rounded-md` (đúng, khớp code mẫu) còn §6/§15/§16 vẫn ghi `rounded-full` (sai). Sai này đã gây lượt Owner bác #75 (V1.00.345 → V1.00.347). V1.00.347 chỉ vá §1, để sót 4 vị trí → nay vá HẾT trong MỘT lượt theo `GOV-EDIT-PRESERVE-001` yêu cầu 2 | Owner duyệt 18/08/2026 | `- `khach-hang` section card `rounded-[10px]` + header-pill `rounded-md` → về `rounded-md`/`rounded-full`.` |
| 18/08/2026 10:52 | §1 bảng bo góc | Owner: "góc bo giống như hình dùm 1 tiêu chuẩn 1" — pill header phải `rounded-md` như `/m1/khach-hang` | Owner | `\| Pill / badge / chip trạng thái \| **rounded-full** \|` (bản 11/08 §6) — đã thay bởi dòng "Pill THỐNG KÊ ở HEADER = rounded-md" |

| 18/08/2026 | §17 §18 §19 §20 (thêm mới) + §17→§21 (đổi số mục Lịch sử) | **D4**: bổ sung 4 hạng mục trước đây KHÔNG nguồn nào phủ (định dạng ngày · định dạng tiền · trạng thái đang tải/rỗng/lỗi/không-có-quyền · combobox chọn theo TÊN + tìm kiếm). **D3**: gộp nội dung 11 kỹ năng `.cursor/skills/` vào SSOT (Claude Code không đọc được thư mục đó) — thư mục kỹ năng gốc GIỮ NGUYÊN làm lưu trữ. Kèm §20.7 phân xử **11 xung đột**, SSOT + code đang chạy THẮNG | Owner duyệt 18/08/2026 (Q2) | Không dòng nào bị xoá. Mục "LỊCH SỬ SỬA ĐỔI" đổi số từ `## 17)` sang `## 21)` để nhường số cho 4 mục mới — nội dung giữ nguyên |

**Nguồn chân lý của đặc tả pill:** `src/app/m1/khach-hang/khach-hang-client.tsx:379`, `:383`, `:387` (code đang chạy). Mọi mục nói về pill header trong file này phải khớp đúng ba dòng đó.

**Quét cùng chủ đề (GOV-EDIT-PRESERVE-001 yêu cầu 2)** — đã rà TOÀN FILE bằng `grep -n -i "pill"` và `grep -n "rounded-full"`, tìm được **5 vị trí** nói về pill header: §1 (đúng từ 18/08 10:52) · §1 câu tổng kết (đúng) · §6 Variant A · §15 mục 2 · §15 mục 6 · §16. **4 vị trí sai đã vá hết trong lượt này.** Các dòng `rounded-full` còn lại trong file thuộc **badge trong hàng bảng · badge hero · avatar/icon-circle · nút tròn · badge đếm** — ĐÚNG theo §1, **cố ý giữ nguyên**.
