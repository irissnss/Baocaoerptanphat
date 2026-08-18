# 📚 THAM CHIẾU DB APPSHEET "TÂN PHÁT PACKAGING" → NỀN TẢNG CHO ERP

> **Nguồn:** `docs/File DB Của Appsheet/Tan Phat Packaging.xlsx` — DB vận hành THẬT của Tân Phát, chạy bằng AppSheet nhiều năm qua (Owner cung cấp 18/08/2026).
> **Mục đích:** khai thác cấu trúc + quy ước nghiệp vụ đã kiểm chứng thực tế để làm nền cho DB ERP (khách hàng, NCC, nhân viên, sản phẩm, vật tư, luồng đơn hàng…).
> **Phạm vi tài liệu:** CHỈ **cấu trúc / schema / danh mục cấu hình / thống kê số lượng** — đã **PUBLIC-SAFE**. KHÔNG chứa dữ liệu cá nhân (tên/SĐT/địa chỉ KH), số tiền, công nợ, credential. Xem mục 11.
> **Cách rút:** đọc trực tiếp workbook bằng `openpyxl` (script ở `scratchpad`, không commit). Số dòng là xấp xỉ tại thời điểm đọc.

---

## 1) TỔNG QUAN

- **32 sheet**, mô hình ERP đầy đủ: Danh mục → Báo giá → Đơn hàng → Sản xuất → Giao hàng → Nghiệm thu → Công nợ → Thu chi.
- **Quy mô dữ liệu thật** (điểm mạnh để kế thừa + phải tính khi migrate):

| Nhóm | Sheet (dòng ~) |
|---|---|
| **Danh mục nền** | DMKH **1.803** · DMSP **6.761** · DMVT **313** · LienHe **1.765** · NhanVien 18 · CDSX 289 · DMTB 2 · Thiết kế 2 |
| **Báo giá** | BaoGia **4.102** · CTBaoGia **13.205** |
| **Đơn hàng** | DonHang **1.783** · CTDonHang **4.206** |
| **Giao hàng** | GiaoHang **1.046** · CTGiaoHang **2.234** |
| **Nghiệm thu / Hóa đơn** | NghiemThu **498** · HoaDon 5 |
| **Mua hàng** | MuaHang **195** |
| **Tài chính** | ThuChi **1.800** · CongNo 6 · RP_CongNo 33 · TaiKhoan 6 |
| **Sản xuất** | LSX 3 (74 cột) · NhatKySX 3 · KhoThanhPham 3 · KHSX 2 · KHVT 2 · SuCo 2 · QuyTrinh 28 |
| **Hệ thống** | Menu 29 · CaiDat **305** · Form LSX 76 · ReportKHSX 5 |

> **Đọc nhanh:** phần bán hàng (báo giá/đơn/giao/nghiệm thu) và danh mục đã **rất dày** (chục nghìn dòng); phần **sản xuất chi tiết (LSX/NhatKySX/KhoThanhPham) gần như trống** (2–3 dòng) → module sản xuất trên AppSheet mới/ít dùng, ERP có cơ hội làm tốt hơn.

---

## 2) BẢN ĐỒ THỰC THỂ & LUỒNG NGHIỆP VỤ

```
DMKH (KH + NCC) ─┬─> BaoGia ──> DonHang ──> LSX (sản xuất) ──> KhoThanhPham
                 │      │           │            │
DMSP ────────────┘   CTBaoGia   CTDonHang    NhatKySX / KHVT / KHSX
                                     │
DMVT ──> MuaHang (nhập vật tư)       ├─> GiaoHang ──> CTGiaoHang
                                     ├─> NghiemThu ──> HoaDon
                                     └─> CongNo <── ThuChi <── TaiKhoan
```

**Luồng chuẩn (khớp sheet `QuyTrinh` — 6 nhóm quy trình):**
1. **Quản lý báo giá** (4 bước) → 2. **Quản lý đơn hàng** (6 bước) → 3. **Quản lý sản xuất** (6 bước) → 5. **Quản lý giao hàng** (4 bước) → **Nghiệm thu** → 6. **Thanh toán công nợ** (4 bước). Song song: 4. **Quản lý Thu chi** (3 bước).

---

## 3) DANH MỤC NỀN (MASTER) — chi tiết + ánh xạ ERP

### 3.1 `DMKH` — Khách hàng **VÀ** Nhà cung cấp (GỘP 1 bảng) — 1.803 dòng, 28 cột
> ⭐ **Bài học quan trọng:** AppSheet **gộp KH + NCC + Info vào 1 bảng**, phân biệt bằng cột **`Hạng mục`**: `Khách hàng` (1.692) · `Nhà cung cấp` (109) · `Info` (1). Cột `Nhóm` phân loại tiếp (Cá nhân 1.526 · Công ty 166 · và các nhóm NCC: Giấy, Keo Dán, Nhà In, Màng, Decal…).
>
> ERP hiện **tách** `nha-cung-cap` và `khach-hang` (M5/M1) → khi migrate cần **route theo `Hạng mục`**.

Cột: `Mã KH | Hạng mục | Nhóm | Tên rút gọn | Tên đầy đủ | Địa chỉ | Mã số thuế | Người đại diện | Số điện thoại | Website | Fanpage | Số tài khoản | Tên chủ tài khoản | Ngân hàng | Người liên hệ | Di động | Email | Địa chỉ Giao Hàng | Logo | Giá trị ĐH | Giá trị GH | Đã thanh toán | Còn nợ | …`

**Ánh xạ ERP:** `Mã KH`→mã; `Tên rút gọn/đầy đủ`→tên; `Mã số thuế/Người đại diện/SĐT/Email/Địa chỉ`→hồ sơ; `Nhóm`→loại KH/NCC; `Giá trị ĐH/GH/Đã thanh toán/Còn nợ`→**tổng hợp công nợ (đừng migrate như dữ liệu gốc — nên tính lại)**.

### 3.2 `DMSP` — Sản phẩm — 6.761 dòng, 17 cột
Cột: `Mã SP | Khách hàng | Tên khách hàng | Loại hàng | Nhóm sản phẩm | Tên sản phẩm | Quy cách | Chất liệu | Quy trình sản xuất | Hình ảnh | Đvt | Giá vốn | Giá bán | Hiện tồn | …`
> ⭐ Sản phẩm **gắn theo khách hàng** (`Mã SP` có `Khách hàng`) — SP đặt riêng từng KH (đặc thù bao bì). `Quy trình sản xuất` là **free-text** (2.414 giá trị khác nhau!) → ERP nên chuẩn hoá thành chuỗi công đoạn (`CDSX`).

**Taxonomy sản phẩm (rút từ dữ liệu thật):**
- **Loại hàng** (7): Offset (4.813) · Gia Công Sau In (1.137) · Không in (586) · Flexo (191) · Digital (20) · Thủ Công Mỹ Nghệ · Kéo Lụa.
- **Nhóm sản phẩm** (chuẩn ~24, thực tế 39 do trùng hoa/thường): Túi Giấy · Hộp Mềm · Hộp Carton · Tem Nhãn · Hộp Sóng Bồi · Hộp Cứng · Thùng Carton · Thẻ Tag · Tờ Rơi · Khay Sản Phẩm · Bao Thư · Catalogue · Name Card · Folder · Brochure · Lịch · Đế Lót Ly · Túi Vải · Hộp Gỗ · Ly Giấy …
- **Đvt** (13): Hộp (2.804) · Cái (1.614) · Túi (968) · Tờ · Nhãn · Thùng · Bộ · Cuốn · Tấm · Cuộn · Bao · Chuyến · Cây.

### 3.3 `DMVT` — Vật tư — 313 dòng, 14 cột ⭐ (đã có MÔ HÌNH TỒN)
Cột: `Mã vật tư | Chất liệu | Định lượng | Tên vật tư | Quy cách | Đvt | **Hiện tồn | Tồn đầu | Nhập | Xuất | Tồn cuối** | Ghi chú | …`
> ⭐⭐ **Xác nhận quyết định tồn vật tư của Owner:** AppSheet **CÓ** theo dõi tồn vật tư kiểu số dư (`Tồn đầu + Nhập − Xuất = Tồn cuối`). Đây chính là mô hình **sổ kho** ERP vừa áp cho phiếu nhập/xuất (V1.00.350). → Migrate `Tồn đầu` thành số dư đầu kỳ + ghi nhận Nhập/Xuất qua sổ.
> **Loại vật tư** (từ `CaiDat`): ~200 loại giấy/vật liệu theo định lượng gsm (Ivory/Couche/Duplex/Kraft/Bristol/Chipboard/Decal/Carton nhiều lớp/Mút/Gỗ…). Là **từ điển chất liệu giấy in** rất chi tiết — nên bê nguyên vào danh mục vật tư ERP.

### 3.4 `NhanVien` — 18 dòng, 35 cột (kèm phân quyền + KPI)
Cột chính: `Mã NV | Họ và tên | Phòng ban | Chức vụ | Bộ phận | SĐT | Email | Password | Phân quyền | Quyền xem/thêm/sửa/xóa | Kiểm tra | Phê duyệt | …` + nhóm cột báo cáo/KPI kinh doanh.
> ⚠️ Có cột **`Password`** (plaintext) → **KHÔNG migrate mật khẩu**; ERP tự hash + cấp lại.
> **Cấu trúc tổ chức (public-safe):** Phòng ban (7): Kinh Doanh · Thiết Kế · Sản Xuất · Ban Giám Đốc · Kế Toán · Giao Hàng · Quản Trị Hệ Thống. Chức vụ: Giám đốc · TP Kinh Doanh · Team Leader · Kế Toán Trưởng · Admin. Phân quyền: User (13) · Admin (4).

### 3.5 `LienHe` — 1.765 dòng — liên hệ theo `Mã KH` (nhiều người nhận hàng/1 KH). ERP: bảng liên hệ con của khách hàng.

### 3.6 `CDSX` — Công đoạn sản xuất — 289 dòng (định mức + thời gian + khuôn + bù hao)
> **24 công đoạn** thật: Phụ Kiện Thành Phẩm · Bồi · In Offset · Thành Phẩm · Cán Màng · Dán · Cán Màng Trước In · Hiệu Ứng · Ép Kim · Bế · Hộp Cứng · Tráng Phủ · Phay · In Flexo · Bấm Kim · Đóng Cuốn · Cắt Thành Phẩm · Giao Hàng · Hộp Gỗ · Đóng Gói · Cắt Giấy · QC In · Chiết Quang · Kiểm Phẩm. → **Xương sống định tuyến sản xuất** cho ERP (thay cho free-text `Quy trình sản xuất`).

### 3.7 `Thiết kế` (khuôn/mold) · `DMTB` (thiết bị) — danh mục hỗ trợ, dữ liệu còn ít.

---

## 4) CHỨNG TỪ GIAO DỊCH

| Sheet | Dòng | Vai trò | Cột khoá đáng chú ý |
|---|---|---|---|
| `BaoGia` / `CTBaoGia` | 4.102 / 13.205 | Báo giá + chi tiết | `Số BG`, `Mã KH`, `VAT`, `Trạng thái`, `Số ĐH` (liên kết lên đơn) |
| `DonHang` / `CTDonHang` | 1.783 / 4.206 | Đơn hàng + chi tiết | `Số ĐH`, `Đặt cọc/Đã thanh toán/Còn lại`, `SL Thành phẩm/đã giao/còn lại`, `Mã Công nợ`, `Lệnh SX` |
| `GiaoHang` / `CTGiaoHang` | 1.046 / 2.234 | Giao hàng + chi tiết | `Số phiếu GH`, `Số ĐH`, `Phương tiện giao`, `SL ĐH/đã giao/còn lại` |
| `NghiemThu` | 498 | Nghiệm thu (chốt số tính tiền) | `SL Đặt/Giao/Tính tiền`, `VAT`, `Tổng tiền + VAT` |
| `MuaHang` | 195 | Mua vật tư (theo LSX + NCC) | `Số phiếu`, `Lệnh SX`, `Nhà cung cấp`, `Mã vật tư`, `Chất lượng` |
| `ThuChi` | 1.800 | Sổ thu/chi/nạp quỹ | `Hạng mục` (Thu/Chi/Nạp), `Phân loại`, `Tồn đầu/Tồn cuối`, `Tài khoản` |
| `CongNo` / `HoaDon` / `TaiKhoan` | 6 / 5 / 6 | Công nợ kỳ · Hóa đơn · Tài khoản/quỹ | công nợ theo kỳ (Nợ đầu/cuối kỳ) |

> **Liên kết chính:** `Số ĐH` xuyên suốt Báo giá→Đơn→Giao→Nghiệm thu; `Mã Công nợ` nối CTDonHang↔CongNo; `Lệnh SX` nối Đơn↔Sản xuất↔Mua hàng.

**Quy ước bút toán Thu Chi (từ `CaiDat`) — public-safe:**
- **Thu:** Thu Cọc Đơn Hàng · Thu Tiền Đơn Hàng · Thu Công Nợ · Bán Hàng · Phế Liệu · In Gia Công · Gia Công Thành Phẩm · Thu Phí Thiết Kế · Thu Khác.
- **Chi:** Mua Vật Tư SX · Mua Nguyên Vật Liệu SX · Chi Phí Vận Chuyển · Chi Lương · Sửa Chữa/ Mua Máy Móc TB · Văn Phòng Phẩm · Xăng Dầu · Công Đoàn · Quản Lý Cty · Mua Ứng Dụng Số · Chi Phí Khác.
- **Nạp:** Nạp Quỹ.

---

## 5) BẢNG TRẠNG THÁI (STATUS FLOWS) — rút từ dữ liệu thật, đánh số sẵn

| Thực thể | Chuỗi trạng thái |
|---|---|
| **Đơn hàng** | `1. ĐH Chờ Duyệt` → `2. ĐH Đang SX` → `3. ĐH đã SX hoàn tất` → `4. ĐH đã giao` → `5. ĐH hoàn thành` · (`6. ĐH đã hủy`) |
| **Giao hàng** | `1. Chờ Soạn Hàng` → `2. Đã Soạn Hàng` → `3. Đang Giao Hàng` → `4. Giao Hàng Hoàn Tất` |
| **Thu chi** | `1. Chờ phê duyệt` → `2. Đã phê duyệt` → `3. Hoàn tất` |
| **Công nợ** | `1. Chưa xác nhận` → `2. Đã gửi công nợ` · tình trạng: `Quá hạn >60 ngày`… |
| **Nghiệm thu** | Chờ Duyệt Nội Bộ → Chờ Khách Xác Nhận → Đã Xuất Hoá Đơn → Hoàn Tất |

> ⭐ Quy ước **đánh số thứ tự trạng thái** (`1.`, `2.`…) rất rõ — ERP nên giữ để sắp xếp/hiển thị nhất quán.

**Cấu hình khác (public-safe):** Loại khách hàng: Khách lẻ · VIP · Facebook · Zalo · Google Ads · Hotline. Loại đơn: Sản xuất · Mẫu · Bù hàng. Mức ưu tiên: Thông thường · Gấp. Hạn thanh toán: 0/15/30/45/60 ngày. Phương tiện giao: Shipper · Xe tải · Gửi chành.

---

## 6) NHẬN ĐỊNH & KHUYẾN NGHỊ CHO ERP

**Khai thác được ngay (dữ liệu nền để seed ERP — KHÔNG mock):**
1. **Danh mục vật tư** (313) + **từ điển chất liệu giấy** (~200 loại theo gsm) → seed `dm_vat_tu` / material.
2. **Taxonomy sản phẩm** (Loại hàng × Nhóm SP × Đvt) → chuẩn cho danh mục SP.
3. **24 công đoạn sản xuất** (`CDSX` có định mức/bù hao/thời gian) → định tuyến sản xuất.
4. **Bộ trạng thái + quy trình 6 nhóm** → chuẩn hoá state machine các module.
5. **Bút toán Thu/Chi** (danh mục Thu/Chi/Nạp) → module tài chính.
6. **Cấu trúc tổ chức** (7 phòng ban, chức vụ) → HR.

**Bài học kiến trúc:**
- **KH và NCC chung 1 danh bạ** (phân bằng `Hạng mục`). ERP tách 2 bảng — cần map khi migrate; cân nhắc dùng chung "đối tác" nếu muốn giống thực tế.
- **Vật tư đã có mô hình tồn nhập-xuất-tồn** → khớp 100% quyết định "sổ kho" của Owner (V1.00.350).
- **SP gắn theo khách hàng** (bao bì đặt riêng) — không phải catalog dùng chung.
- Liên kết bằng **mã chứng từ** (`Số ĐH`, `Lệnh SX`, `Mã Công nợ`) xuyên suốt.

**Vấn đề chất lượng dữ liệu (phải xử lý khi migrate):**
- **Trùng danh mục do hoa/thường/khoảng trắng**: `Hộp Mềm` vs `Hộp mềm`, `Tem Nhãn` vs `Tem nhãn`, `Tờ Rơi` vs `Tờ rơi`, `Cuộn` vs `Cuồn` → cần **chuẩn hoá (normalize)** trước khi nạp.
- **`Quy trình sản xuất` free-text 2.414 biến thể** → phải map về chuỗi công đoạn `CDSX`.
- Một số danh mục lẫn dữ liệu rác/1 lần (câu mô tả dài lọt vào "Nhóm sản phẩm").
- Cột tổng hợp công nợ trong `DMKH`/`DonHang` là **giá trị chốt sẵn** → ERP nên **tính lại từ chứng từ**, không tin số cũ.

---

## 7) ÁNH XẠ SHEET APPSHEET → MODULE/BẢNG ERP

| Sheet AppSheet | Module ERP | Bảng ERP hiện có (gần đúng) |
|---|---|---|
| DMKH (Hạng mục=Khách hàng) | M1 Danh mục | `khach_hang` |
| DMKH (Hạng mục=Nhà cung cấp) | M5 Kho | `nha_cung_cap` |
| DMSP | M1 | `san_pham` / `dm_san_pham` |
| DMVT | M5 | `dm_vat_tu` / `material_item` |
| LienHe | M1 | liên hệ khách hàng |
| NhanVien | M-HR | `user_account` + nhân sự |
| CDSX | M4 Sản xuất | công đoạn |
| BaoGia/CTBaoGia | CRM báo giá | (cần dựng) |
| DonHang/CTDonHang | CRM/Bán hàng | `don_hang` + chi tiết |
| GiaoHang/CTGiaoHang | M5 | `phieu_giao_hang` + item ✅ đã có |
| NghiemThu | M5/Tài chính | `bien_ban_nghiem_thu` |
| MuaHang | M5 | `mua_hang` + item ✅ đã có |
| ThuChi | MF Tài chính | `phieu_thu` / `phieu_chi` |
| CongNo | MF | `cong_no` + `lich_su_cong_no` |
| TaiKhoan | MF | `tai_khoan_ngan_hang` |
| LSX/NhatKySX/KhoThanhPham | M4/M5 | `lenh_san_xuat`, `kho_thanh_pham` ✅ |

---

## 8) DỮ LIỆU NHẠY CẢM — ĐÃ LOẠI KHỎI TÀI LIỆU CÔNG KHAI

Tài liệu này **KHÔNG** chứa (theo `GOV-PUBLIC-SAFE-001` + `GOV-SECRET-IN-LAW-001`):
- **Credential:** Telegram Bot Token / App ID / Group ID (có trong sheet `CaiDat`) → *[đã gỡ — tra file gốc/sổ bí mật nội bộ]*.
- **Tài chính:** số dư quỹ đầu kỳ, doanh thu, công nợ, số tiền cụ thể.
- **PII:** tên/SĐT/địa chỉ/email khách hàng, nhà cung cấp, nhân viên; mật khẩu nhân viên (plaintext trong `NhanVien.Password`).

> File gốc `.xlsx` là dữ liệu nội bộ — **KHÔNG commit lên repo công khai**. Chỉ tài liệu tham chiếu cấu trúc này (đã sanitize) được đẩy.

---

## 9) BƯỚC KHAI THÁC KẾ TIẾP (đề xuất)

1. Chuẩn hoá + seed **danh mục vật tư** (313) và **từ điển chất liệu giấy** vào ERP.
2. Chuẩn hoá **taxonomy sản phẩm** (Loại hàng/Nhóm SP/Đvt) → dropdown ERP.
3. Nạp **24 công đoạn** `CDSX` (định mức/bù hao) làm định tuyến sản xuất.
4. Thống nhất **bộ trạng thái** theo quy ước đánh số của AppSheet.
5. Lập kế hoạch **migrate KH/NCC** (route theo `Hạng mục`) — sau khi Owner chốt phạm vi.

> Mỗi bước migrate dữ liệu thật **cần Owner duyệt** (phạm vi + chuẩn hoá) trước khi nạp — tránh nhân bản lỗi cũ.
