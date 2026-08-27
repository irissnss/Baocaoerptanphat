# WIREFRAME CUỐI — GOM ĐIỀU HƯỚNG TÀI CHÍNH + TRUNG TÂM PHÂN QUYỀN

**Ngày:** 27/08/2026
**Trạng thái:** ⏳ **CHỜ OWNER DUYỆT** — **chưa đổi lược đồ · chưa DDL · chưa code**
**Thay cho:** `WIREFRAME-DA-SUA-M0-20260827.md` (giữ nguyên tại chỗ làm lịch sử)

---

## 0. TÔI ĐÃ BỎ Ý CỦA OWNER — NAY NHẬN LẠI

Bản trước tôi viết *«**KHÔNG gom**. Giữ ba mục cấp cao nhất»*, với lý do gom sẽ tạo trùng lặp.
Owner bác đúng: **ý gom điều hướng không được bỏ**. Trùng lặp là thứ phải *tránh khi làm*,
không phải lý do để không làm.

Bản này **gom**, và nêu rõ gom thế nào mà **không** đụng cơ sở dữ liệu, **không** nhân bản
bản ghi menu, **không** khiến bất kỳ liên kết nào hiện hai lần.

---

## 1. GOM Ở ĐÂU — VÀ KHÔNG ĐỘNG VÀO CÁI GÌ

| Thứ | Có đổi không |
|---|---|
| Bản ghi `dm_menu` | ❌ **không đụng một dòng nào** |
| `dm_menu.duong_dan` (route) | ❌ không đổi — `/bieu-mau` · `/mc` · `/mf` giữ nguyên |
| Khoá quyền | ❌ không đổi — `form_mau` · `mc` · `mf` · 7 khoá `mf_*` giữ nguyên |
| Module sở hữu | ❌ không đổi — Biểu Mẫu vẫn thuộc module của nó, Hợp Đồng vẫn thuộc MC |
| **Cây hiển thị thanh bên** | ✅ **chỉ đổi ở đây** — một tệp cấu hình giao diện |

Nói gọn: **gom là việc của lớp hiển thị**, không phải việc của dữ liệu.
Cùng cơ chế mà nhóm **Nhân Sự** trong dự án đã dùng từ trước — không phát minh gì mới.

---

## 2. HÌNH DUNG — NGƯỜI CÓ ĐỦ QUYỀN (ví dụ quản trị)

```
┌────────────────────────────────────┐
│  ▾ Tài Chính                       │ ← ĐẦU NGĂN, không bấm được
│      Tổng Quan            → /mf    │ ← liên kết thật tới trang tổng
│      Phiếu Thu            → /mf/phieu-thu
│      Phiếu Chi            → /mf/phieu-chi
│      Công Nợ              → /mf/cong-no
│      Ngân Hàng            → /mf/ngan-hang
│      Đối Chiếu            → /mf/doi-chieu
│      Nghiệm Thu           → /mf/nghiem-thu
│      Kế Toán              → /mf/ke-toan
│      ─────────────────────────────
│      Hợp Đồng             → /mc    │ ← giữ nguyên khoá `mc`, route `/mc`
│      Biểu Mẫu / Mẫu In    → /bieu-mau
│          Bản Phát Hành    → /bieu-mau/phat-hanh
└────────────────────────────────────┘
```

**Ba điều làm cho không có liên kết nào hiện hai lần:**

1. `Biểu Mẫu` và `Hợp Đồng` **rời khỏi cấp cao nhất** khi vào ngăn — chúng chỉ ở **một** chỗ.
2. Trang tổng `/mf` **không** nằm ở đầu ngăn nữa mà thành **một mục con riêng** ("Tổng Quan").
   Đầu ngăn chỉ còn là chữ để mở/đóng. ⇒ đúng một liên kết tới `/mf`.
3. Đầu ngăn **không** dựng thẻ liên kết (đưa `mf` vào nhóm đầu-ngăn-thuần), nên nó không thể
   là liên kết thứ hai tới cùng đích.

> Đây chính là điều Owner khoá ở mục F.3: *«Tổng quan /mf là child/capability riêng»*.

---

## 3. HÌNH DUNG — BỐN CA THEO QUYỀN (dựa trên dữ liệu THẬT đang có)

### 3.1 CEO — có 7 màn Tài chính, **không** có quyền trang tổng

```
┌────────────────────────────────────┐
│  ▾ Tài Chính                       │ ← chữ thường, chỉ mở/đóng
│      Phiếu Thu · Phiếu Chi · Công Nợ · Ngân Hàng
│      Đối Chiếu · Nghiệm Thu · Kế Toán
│      Hợp Đồng                      │
│      Biểu Mẫu / Mẫu In             │
└────────────────────────────────────┘
      (KHÔNG có mục "Tổng Quan")
```

Đây **không phải giả định** — đo trên dữ liệu thật ngày 27/08/2026: CEO có `can_view=1` cho cả
7 khoá `mf_*` và `MENU_MC`, `MENU_FORM_MAU`, nhưng `MENU_MF` là **0**.
Ảnh chụp trình duyệt đã chứng minh CEO **không** thấy `/mf`.

### 3.2 Chỉ có **một** màn Tài chính (ví dụ vai trò Sales, thật)

```
┌────────────────────────────────────┐
│  ▾ Tài Chính                       │
│      Công Nợ                       │ ← đúng một
└────────────────────────────────────┘
```

### 3.3 Chỉ có **Hợp Đồng** — đúng ca Owner nêu ở F.4

```
┌────────────────────────────────────┐
│  ▾ Tài Chính                       │ ← thấy ngăn (vì có một mục con)
│      Hợp Đồng             → /mc    │ ← thấy, bấm được
└────────────────────────────────────┘
```
- ✅ thấy ngăn Tài Chính
- ✅ thấy Hợp Đồng
- ❌ **không** mở được `/mf` — không có mục Tổng Quan, đầu ngăn không phải liên kết
- ❌ **không** thấy màn tài chính nào — bảy mục con đều ẩn

⚠️ Điểm cần Owner biết rõ: người này **nhìn thấy chữ "Tài Chính"**. Đó là hệ quả trực tiếp của
việc gom. Nếu Owner **không** muốn người chỉ có Hợp Đồng nhìn thấy chữ "Tài Chính", thì phải
chọn cách khác — nêu ở mục 5.

### 3.4 Tài khoản chờ cấp phát

```
┌────────────────────────────────────┐
│  (không mục nghiệp vụ nào)         │
└────────────────────────────────────┘
      Tài khoản đang chờ phân quyền
```

---

## 4. TIÊU CHÍ NGHIỆM THU — ĐO ĐƯỢC, TRƯỚC KHI VIẾT DÒNG MÃ ĐẦU TIÊN

| # | Tiêu chí | Cách đo |
|---|---|---|
| 1 | Mỗi liên kết hiện **đúng một lần** trong toàn cây | cổng: đếm `href` trùng, phải = 0 |
| 2 | Không bản ghi `dm_menu` nào bị thêm/sửa/xoá | so ảnh chụp bảng trước/sau, phải giống hệt |
| 3 | Ba route giữ nguyên | cổng khẳng định `/bieu-mau` · `/mc` · `/mf` |
| 4 | Ba khoá quyền giữ nguyên | cổng khẳng định `form_mau` · `mc` · `mf` |
| 5 | Người chỉ có Hợp Đồng: thấy `/mc`, **0** liên kết `/mf*` | ảnh chụp trình duyệt |
| 6 | CEO: 7 liên kết `/mf/*`, **0** liên kết `/mf` | ảnh chụp trình duyệt |
| 7 | Quản trị: đủ 7 + `/mf` + `/mc` + `/bieu-mau` | ảnh chụp trình duyệt |
| 8 | Tài khoản chờ cấp phát: 0 mục nghiệp vụ | ảnh chụp trình duyệt |
| 9 | Đầu ngăn **không** dựng thẻ liên kết | cổng đọc mã sống |

Tiêu chí 5–8 đã có sẵn kịch bản chạy được: `npm run test:anh-thanh-ben`.

---

## 5. MỘT VIỆC CẦN OWNER QUYẾT — TÔI KHÔNG TỰ CHỌN

Gom xong thì **người chỉ có Hợp Đồng vẫn nhìn thấy chữ "Tài Chính"**, vì Hợp Đồng nằm trong ngăn đó.

| Cách | Được | Mất |
|---|---|---|
| **A. Gom như mục 2** (đúng ý Owner) | điều hướng gọn, ba mục về một chỗ | người chỉ có Hợp Đồng thấy chữ "Tài Chính" |
| **B. Gom, nhưng đổi tên ngăn** thành nhãn rộng hơn (ví dụ *"Tài Chính & Hồ Sơ"*) | vẫn gom, chữ không hứa nội dung tài chính | phải chốt một nhãn mới |
| **C. Không gom** | không có hiện tượng trên | bỏ ý Owner — **tôi không đề xuất** |

**Tôi làm theo A** trừ khi Owner nói khác. Nêu ra vì đây là thứ Owner sẽ nhìn thấy đầu tiên
khi mở máy, và tôi không muốn Owner bất ngờ.

---

## 6. TRUNG TÂM PHÂN QUYỀN — LUỒNG DẪN TỪNG BƯỚC (Owner F.5–F.6)

```
Tài khoản  →  Vai trò  →  Màn hình  →  Hành động / Dữ liệu / Trường  →  Xem lại & Áp dụng
   ①            ②            ③                   ④                            ⑤
```

### Bước ① — Tài khoản
```
┌───────────────────────────────────────────────────────────────────────┐
│ Tài khoản: (chọn người)                    [ Tìm… ]                   │
│                                                                       │
│  ● đang hoạt động   ○ chờ phân quyền   ⚠ đã khoá                      │
│                                                                       │
│  Vai trò đang có:  [CEO ✕]  [SALES ✕]        [ + Thêm vai trò ]        │
│  Quyền hiệu lực:   43 màn · 6 hành động · 4 trường nhạy cảm           │
│                                                → [ Xem chi tiết ]     │
└───────────────────────────────────────────────────────────────────────┘
```

### Bước ② — Vai trò: **mẫu sẵn** hoặc **vai trò riêng cho người này**
```
┌───────────────────────────────────────────────────────────────────────┐
│  ○ Dùng MẪU SẴN:  [ Kế Toán ▾ ]    ⚠ đang dùng chung cho 2 tài khoản  │
│                                       sửa mẫu này là đổi cho CẢ HAI    │
│                                                                       │
│  ● Tạo VAI TRÒ RIÊNG cho người này (nhân bản từ mẫu rồi chỉnh)        │
│      Tên: Kế Toán — Chi nhánh 2                                       │
│      → không ảnh hưởng ai khác                                        │
└───────────────────────────────────────────────────────────────────────┘
```
> Không có kho quyền riêng theo từng người. Muốn khác mẫu thì **tạo vai trò riêng** —
> vẫn đúng một cơ chế, không sinh nguồn quyền song song.

### Bước ③ — Màn hình
```
│   Menu                        xem  thêm  sửa  xoá  xuất   nguồn cấp   │
│   ▾ Tài Chính                                                         │
│       Công Nợ                  ☑    ☐    ☐    ☐    ☐    ← CEO         │
│       Phiếu Thu                ☑    ☑    ☐    ☐    ☐    ← CEO         │
│     Hợp Đồng                   ☑    ☐    ☐    ☐    ☐    ← vai trò này │
│                                                                       │
│   ☑ = vai trò NÀY cấp     ☐ = vai trò NÀY không cấp                   │
│   ☐ KHÔNG phải "cấm" — vai trò khác của cùng người vẫn có thể cấp.    │
│   Cột "nguồn cấp" cho biết quyền đang tới TỪ ĐÂU.                     │
```

### Bước ④ — Hành động · Dữ liệu · Trường
```
│  [ Hành động ]  [ Phạm vi dữ liệu ]  [ Trường nhạy cảm ]              │
│   Duyệt báo giá            ☑   ← CEO                                  │
│   Xem giá vốn              ☑   ← CEO      (chỉ Quản trị + CEO)        │
│   Huỷ đơn hàng             ☐                                          │
│                                                                       │
│  Các tầng ghép bằng VÀ: tài khoản VÀ màn VÀ hành động VÀ dữ liệu      │
│  VÀ trường. Thiếu một tầng là không làm được.                         │
```

### Bước ⑤ — Xem lại & Áp dụng
```
┌───────────────────────────────────────────────────────────────────────┐
│  KHÁC BIỆT SẼ ÁP DỤNG                    ảnh hưởng 2 tài khoản        │
│    + thêm   Công Nợ (xem)                                             │
│    + thêm   Phiếu Thu (xem, thêm)                                     │
│    − BỚT    Ngân Hàng (xem)      ← người dùng sẽ MẤT quyền này        │
│                                                                       │
│  ⚠ Không có cảnh báo quản trị cuối cùng cho thay đổi này.             │
│                                                                       │
│  Ghi nhật ký: có · Hoàn tác: được, trong 30 ngày                      │
│                                    [ Huỷ ]   [ Áp dụng thay đổi ]     │
└───────────────────────────────────────────────────────────────────────┘
```

**Đây là bước thay thế cho nút "Áp mẫu quyền nhanh" đang tạm ngưng.** Nút cũ ghi đè toàn bộ
mà không cho thấy dòng **"− BỚT"** nào — đó chính là lý do nó bị ngưng.

### Lối vào khôi phục (Owner F.6)
```
┌───────────────────────────────────────────────────────────────────────┐
│  🛟 KHÔI PHỤC QUẢN TRỊ                                                │
│     Đang có 3 quản trị còn khôi phục được hệ thống.                   │
│     Hệ thống KHÔNG cho phép hạ xuống 0 — mọi thao tác dẫn tới đó      │
│     sẽ bị chặn kèm lý do.                                             │
│                                    [ Cấp quyền quản trị cho… ]        │
└───────────────────────────────────────────────────────────────────────┘
```
Phần chặn ở máy chủ **đã làm xong và đã kiểm** trong lượt này (`DEBT-126`).
Phần **hiển thị** ở đây thì chưa — chờ Owner duyệt wireframe.

---

## 7. MƯỜI THÀNH PHẦN OWNER YÊU CẦU — ĐỐI CHIẾU

| Owner F.6 | Có trong wireframe | Đã code chưa |
|---|---|---|
| mẫu vai trò sẵn | bước ② | có sẵn từ trước |
| vai trò riêng cho từng người | bước ② | **chưa** |
| xem trước quyền hiệu lực | bước ① + ⑤ | **chưa** |
| nguồn cấp quyền | bước ③ ④ (cột "nguồn cấp") | **chưa** |
| số tài khoản bị ảnh hưởng | bước ② + ⑤ | **chưa** |
| khác biệt trước/sau | bước ⑤ | **chưa** |
| cảnh báo quản trị cuối | bước ⑤ + mục khôi phục | **máy chủ ĐÃ chặn**; hiển thị chưa |
| lối vào khôi phục | mục khôi phục | **chưa** |
| nhật ký | bước ⑤ | một phần (3/10 hành động chưa ghi nhật ký) |
| hoàn tác | bước ⑤ | **chưa** |

**Không code phần nào của Trung Tâm Phân Quyền trong lượt này** — đúng khoá Owner F.8.

---

## 8. ẢNH CHỤP THẬT ĐÃ CÓ

`docs/anh-kiem-thu/m0-closeout-thanh-ben-20260827/` — 6 ảnh 1920×1080, chụp bằng trình duyệt
thật, đăng nhập thật, mỗi vai trò một ảnh, kèm ảnh nút áp mẫu quyền đang mờ.
Thư mục đã bị git bỏ qua (`GOV-SECRET-LOCATION-001`), tra trên máy Owner.

> Đây là ảnh của **hiện trạng sau khi vá**, chưa phải ảnh của bản gom điều hướng —
> bản gom chưa được code vì đang chờ Owner duyệt.
