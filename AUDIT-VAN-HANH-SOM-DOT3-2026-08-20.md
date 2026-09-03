> ## ⚠️ BẢN RÚT GỌN CÔNG KHAI
>
> Đây là **bản công khai đã lược** của báo cáo *Audit vận hành sớm đợt 3 — 6 nhóm tính năng ưu tiên*.
> Bản đầy đủ nằm ở kho mã riêng tư.
>
> **Đã lược (Owner duyệt 21/08/2026, theo `GOV-PUBLIC-SAFE-001` §J1):**
>
> | Mã | Nội dung bị lược | Vì sao |
> |---|---|---|
> | `⟨SEC-01⟩` | Tên cụ thể của các route API ghi chưa có kiểm quyền | Hệ thống **đang chạy và chưa vá** — công bố tên route = đưa danh sách điểm vào cho người ngoài |
> | `⟨SEC-02⟩` | Đường dẫn màn quản trị chưa có cổng quyền | như trên |
> | `:▮` | **Số dòng** trong mọi tham chiếu mã nguồn | Giữ tên file để tra được, bỏ toạ độ chính xác |
>
> **KHÔNG lược:** kết luận · số đo · bảng diff schema · danh sách blocker · các mục cần Owner quyết · phần "không xác minh được".

---

# AUDIT VẬN HÀNH SỚM — ĐỢT 3 · 6 NHÓM TÍNH NĂNG ƯU TIÊN

**Ngày:** 20/08/2026 · **Loại:** `EVIDENCE` + `STATE` · **Chế độ:** READ-ONLY
**Tổ chức:** multi-agent song song (5 agent) + luồng chính đọc production
**Phạm vi:** 25 bảng / 6 nhóm — mọi bảng khác = TẠM NỢ

> **KHÔNG ĐỤNG:** code (`src/` 0 file) · DDL · DML · migration · commit · push · deploy.
> Trên production **chỉ `SELECT` / `SHOW` / `information_schema`**. Không tạo user, không cấp quyền, 0 lệnh ghi.
> Đường hầm SSH **đã đóng**, file bí mật tạm **đã xoá**. Mọi đề xuất sửa là **phase riêng**, chờ Owner GO.

> ⚠️ **NGUYÊN TẮC BẰNG CHỨNG CỦA ĐỢT NÀY (theo cảnh báo Owner):** dữ liệu trong DB hiện tại — ngoài tài khoản admin/dev — là **draft/demo/mockup, KHÔNG có giá trị**. Báo cáo này **KHÔNG dùng dữ liệu hiện có làm bằng chứng đúng/sai** ở bất kỳ đâu. Mọi kết luận dựa trên **schema** (`SHOW CREATE TABLE` / `information_schema`) và **code** (`file:line`).

---

## 1. T1 — SCHEMA TRUTH & DIFF

### 1.1 Nhãn bằng chứng

| Mục | Kết quả |
|---|---|
| Nguồn đo | **PRODUCTION** (`tanphat-erp` @ `⟨SEC-03 tên máy chủ⟩`, MariaDB `10.11.10-MariaDB-log`) qua đường hầm SSH read-only |
| 25/25 bảng trong phạm vi tồn tại trên production | ✅ |
| 25/25 bảng tồn tại trên dev | ✅ |
| **Diff prod ↔ dev trong phạm vi (cột + FK)** | **0 điểm khác** |
| Nhãn | **`RUNTIME_PROVEN`** cho toàn bộ §1 và §2 |

> Đợt 2 đã chứng minh toàn schema prod↔dev chỉ lệch 4 cột cụm `mua_hang` — **cụm đó KHÔNG thuộc phạm vi đợt 3**. Trong 25 bảng đợt 3, hai môi trường **giống hệt nhau tuyệt đối**, nên mọi số liệu dưới đây đúng cho cả hai.

### 1.2 Diff docs ↔ live — tổng quan

Quét mọi khối `CREATE TABLE` trong `docs/` (loại `docs/_snapshot*`), đối chiếu từng cột với production:

| Chỉ số | Số |
|---|---|
| Bảng trong phạm vi **có** `CREATE TABLE` trong docs | **19 / 25** |
| Bảng **KHÔNG có** `CREATE TABLE` trong docs | **6 / 25** *(xem phân mức ngay dưới)* |
| Cột khớp docs ↔ live | **289** |
| Cột **docs khai mà live KHÔNG có** | **87** |
| Cột **live có mà docs KHÔNG khai** | **36** |

**🔴 Sáu bảng KHÔNG có `CREATE TABLE` trong tài liệu — toàn bộ thuộc nhóm 6 (Tính giá).** Nhưng phải phân biệt hai mức, kẻo nói quá:

| Mức | Bảng | Docs có gì |
|---|---|---|
| **Có danh sách cột, KHÔNG có đặc tả** | `dm_auto_pricing_formula` · `dm_blueprint` · `dm_pricing_addon` | Chỉ một dòng liệt kê tên cột trong khối *"DB hiện tại (dump 26/01/2026)"* của tài liệu **M3.2 Báo giá** — **không có kiểu dữ liệu, không nullable, không khoá, không FK, không mô tả ý nghĩa** |
| **KHÔNG có gì cả** | `dm_pricing_test_case` · `dm_param_registry` · `dm_addon_type` | **Không một danh sách cột nào** trong toàn bộ `docs/`. Tên bảng chỉ xuất hiện rải rác trong báo cáo/changelog (5–8 file), không phải tài liệu đặc tả |

> Đây là **lỗ tài liệu lớn nhất của phạm vi đợt 3**: nhóm tính giá — nhóm phức tạp nhất về nghiệp vụ — có **6/8 bảng không được đặc tả**. Tổng 85 cột, trong đó **41 cột (3 bảng) không có đến một dòng liệt kê tên**.

### 1.3 Diff docs ↔ live — từng bảng

| Bảng | docs khai | live có | docs-thừa | live-thừa | Mức lệch |
|---|---|---|---|---|---|
| `dm_khach_hang` | 23 | 22 | 5 | 4 | 🟠 |
| `kh_lien_he` | 15 | 14 | 4 | 3 | 🟠 |
| `kh_dia_chi` | 18 | 14 | **10** | 6 | 🔴 |
| `dm_dia_chi_vn` | 14 | 13 | 2 | 1 | 🟡 |
| `dm_nha_cung_cap` | 20 | 20 | **0** | **0** | ✅ **KHỚP TUYỆT ĐỐI** |
| `hr_employee_nhanvien` | 41 | 41 | **0** | **0** | ✅ **KHỚP TUYỆT ĐỐI** |
| `hr_vi_tri` | 18 | 11 | **9** | 2 | 🔴 |
| `dm_san_pham` | 16 | 17 | 1 | 2 | 🟡 |
| `dm_vat_tu` | 27 | 19 | **12** | 4 | 🔴 |
| `material_item` | 25 | 24 | 4 | 3 | 🟠 |
| `dm_nhom_universal` | 29 | 17 | **13** | 1 | 🔴 |
| `bao_gia` | 20 | 15 | 5 | 0 | 🟠 |
| `bao_gia_item` | 13 | 9 | 4 | 0 | 🟠 |
| `bao_gia_option` | 13 | 13 | **0** | **0** | ✅ **KHỚP TUYỆT ĐỐI** |
| `don_hang` | 20 | 20 | **0** | **0** | ✅ **KHỚP TUYỆT ĐỐI** |
| `don_hang_item` | 15 | 16 | 0 | 1 | 🟡 |
| `thiet_ke_yeu_cau` | 23 | 13 | **10** | 0 | 🔴 |
| `dm_cong_doan` | 12 | 10 | 3 | 1 | 🟠 |
| `dm_bang_gia_cong_doan` | 14 | 17 | 5 | **8** | 🔴 |
| `dm_auto_pricing_formula` · `dm_blueprint` · `dm_pricing_addon` | *(chỉ liệt kê tên cột)* | 44 | — | — | 🔴 **không có đặc tả** |
| `dm_pricing_test_case` · `dm_param_registry` · `dm_addon_type` | **0** | 41 | — | — | 🔴 **không có gì** |

**Bốn bảng khớp tuyệt đối:** `dm_nha_cung_cap`, `hr_employee_nhanvien`, `bao_gia_option`, `don_hang`. Riêng `hr_employee_nhanvien` (41 cột) và `dm_nha_cung_cap` (20 cột) khớp 100% là **tin tốt cho việc nạp dữ liệu thật đang chạy** — tài liệu của hai danh mục đó đáng tin.

### 1.4 Chi tiết từng điểm lệch

**Nhóm 1 — Khách hàng**

```
[dm_khach_hang] DOCS khai mà LIVE KHÔNG CÓ (5):
    dia_chi_chinh · tinh_thanh · quoc_gia · dieu_kien_thanh_toan · thoi_gian_cong_no_ngay
[dm_khach_hang] LIVE CÓ mà DOCS KHÔNG KHAI (4):
    fax · chuc_vu_dai_dien_id · dieu_kien_thanh_toan_id · so_ngay_cong_no
```
> Đọc được ý nghĩa: docs mô tả **thiết kế cũ** (địa chỉ nhúng thẳng vào bảng khách, điều kiện thanh toán là chữ tự do). Live đã **tách địa chỉ ra `kh_dia_chi`** và đổi sang **khoá ngoại** (`dieu_kien_thanh_toan_id`, `chuc_vu_dai_dien_id`). Cặp `thoi_gian_cong_no_ngay` ↔ `so_ngay_cong_no` là **đổi tên cùng nghĩa**.

```
[kh_lien_he] DOCS khai mà LIVE KHÔNG CÓ (4): khach_hang_id · zalo · la_lien_he_chinh · vai_tro
[kh_lien_he] LIVE CÓ mà DOCS KHÔNG KHAI (3): ma_khach_hang · is_lien_he_chinh · ngay_sinh
```
> Hai điểm đáng chú ý: (a) docs nói khoá ngoại là **`khach_hang_id`** (số), live dùng **`ma_khach_hang`** (chuỗi) — FK thật trỏ `dm_khach_hang.ma_khach_hang`; (b) `la_lien_he_chinh` ↔ `is_lien_he_chinh` là **cùng cột, khác tiền tố ngôn ngữ**. Cột `zalo` và `vai_tro` docs hứa mà **live không có**.

```
[kh_dia_chi] DOCS khai mà LIVE KHÔNG CÓ (10):
    khach_hang_id · ten_dia_chi · dia_chi_day_du · phuong_xa · quan_huyen · tinh_thanh ·
    quoc_gia · nguoi_nhan · dien_thoai_nguoi_nhan · la_dia_chi_mac_dinh
[kh_dia_chi] LIVE CÓ mà DOCS KHÔNG KHAI (6):
    ma_khach_hang · dia_chi_chi_tiet · tinh_thanh_id · phuong_xa_id ·
    is_dia_chi_mac_dinh · nguoi_phu_trach_ids
```
> 🔴 **Lệch nặng nhất nhóm 1.** Docs mô tả địa chỉ dạng **chữ tự do** (`tinh_thanh`, `quan_huyen`, `phuong_xa` là text); live đã chuyển sang **khoá ngoại vào `dm_dia_chi_vn`** (`tinh_thanh_id`, `phuong_xa_id`). **Không còn cấp `quan_huyen`** trong live — chỉ 2 cấp tỉnh/phường. Docs cũng hứa `nguoi_nhan` + `dien_thoai_nguoi_nhan` mà live không có.

**Nhóm 2 — NCC + nhân viên**

`dm_nha_cung_cap` và `hr_employee_nhanvien`: **khớp 100%**, không có điểm lệch nào.

```
[hr_vi_tri] DOCS khai mà LIVE KHÔNG CÓ (9):
    ten_tieng_anh · cap_bac_nv · trach_nhiem · yeu_cau_cong_viec · luong_toi_thieu ·
    luong_toi_da · so_luong_ke_hoach · so_luong_hien_tai · ghi_chu
[hr_vi_tri] LIVE CÓ mà DOCS KHÔNG KHAI (2): nhom_vi_tri_id · thu_tu
```
> Docs mô tả `hr_vi_tri` như một **bảng mô tả công việc đầy đủ** (khung lương, biên chế kế hoạch/thực tế, trách nhiệm). Live chỉ là **danh mục vị trí gọn** 11 cột. Chênh lệch 9 cột — đây là **spec chưa cài**, không phải lỗi.

**Nhóm 3 — Sản phẩm & vật tư**

```
[dm_vat_tu] DOCS khai mà LIVE KHÔNG CÓ (12):
    ten_vat_tu · do_dai_cm · do_rong_cm · kho_giay · chieu_dai_mm · chieu_rong_mm ·
    don_vi_tinh · gia_theo_ram · ton_kho_toi_thieu · mo_ta · hinh_anh · is_active
[dm_vat_tu] LIVE CÓ mà DOCS KHÔNG KHAI (4):
    ten_vat_tu_ngan · gia_tham_chieu_tinh · gia_tham_chieu_dong_30d · gia_tham_chieu_updated_at
```
> ⚠️ Đáng chú ý: docs khai `ten_vat_tu` nhưng **live không có cột đó** — live chỉ có `ten_vat_tu_ngan`. Các cột kích thước (`chieu_dai_mm`, `kho_giay`…) docs đặt ở `dm_vat_tu`, live đặt ở **`material_item`**. Cụm `gia_tham_chieu_*` là **tính năng live có mà docs chưa ghi**.

```
[material_item] DOCS khai mà LIVE KHÔNG CÓ (4): ma_vat_tu_master · dinh_luong_gsm · do_day_mm · kich_thuoc
[material_item] LIVE CÓ mà DOCS KHÔNG KHAI (3): uom_mua_mac_dinh_id · kho_dai_mm_default · kho_rong_mm_default
[dm_san_pham] DOCS khai mà LIVE KHÔNG CÓ (1): ma_nhom_san_pham
[dm_san_pham] LIVE CÓ mà DOCS KHÔNG KHAI (2): nhom_cong_nghe_id · dm_vat_tu_id
[dm_nhom_universal] DOCS khai mà LIVE KHÔNG CÓ (13):
    id_nhom · id_nhom_cha · thu_tu_hien_thi · is_active · entity_type · category_code ·
    category_name · prefix · category_id · node_code · node_name · node_abbr · parent_node_id
[dm_nhom_universal] LIVE CÓ mà DOCS KHÔNG KHAI (1): ten_danh_muc_hien_thi
```
> 🔴 `dm_nhom_universal` lệch 13 cột — nhưng đọc kỹ thì đây là **ba thế hệ thiết kế chồng nhau trong docs**: thế hệ 1 (`id_nhom`, `id_nhom_cha`), thế hệ 2 (`category_code`, `node_code`, `entity_type`, `prefix`), thế hệ 3 = live (`ma_nhom`, `nhom_cha_id`, `danh_muc_nhom`). Docs giữ cả ba mà **không gắn nhãn hiệu lực**.

**Nhóm 4 — Báo giá**

```
[bao_gia] DOCS khai mà LIVE KHÔNG CÓ (5): so_bao_gia · ma_khach_hang · ngay_bg · han_hieu_luc · nguoi_lap
[bao_gia_item] DOCS khai mà LIVE KHÔNG CÓ (4): so_bao_gia · ma_san_pham · mo_ta_chi_tiet · thu_tu
```
> Đây là **đúng ca đã nêu ở đợt 1**: docs giữ khối "Schema intent" của thiết kế cũ (PK = `so_bao_gia VARCHAR`, có `nguoi_lap`) song song với khối "DB hiện tại" khớp live (PK = `id INT`, có `id_nhan_vien_phu_trach`). **`bao_gia_option` khớp 100%.**

**Nhóm 5 — Đơn hàng**

```
[don_hang] khớp tuyệt đối 20/20
[don_hang_item] LIVE CÓ mà DOCS KHÔNG KHAI (1): can_thiet_ke
[thiet_ke_yeu_cau] DOCS khai mà LIVE KHÔNG CÓ (10):
    so_don_hang · mo_ta_yeu_cau · file_tham_khao · nguoi_yeu_cau · nguoi_thiet_ke ·
    ngay_yeu_cau · ngay_hoan_thanh_du_kien · ngay_hoan_thanh_thuc_te · file_thiet_ke · ghi_chu
```
> 🔴 `thiet_ke_yeu_cau`: docs khai 23 cột, live chỉ 13 — **thiếu 10 cột, 0 cột thừa**. Toàn bộ phần "nội dung yêu cầu thiết kế" (mô tả, file tham khảo, file thiết kế, người yêu cầu, người thiết kế, các mốc ngày) **chưa được cài**. Bảng live hiện chỉ là **khung liên kết** (`id_don_hang`, `id_bao_gia`, `id_khach_hang`) chứ chưa mang được nội dung nghiệp vụ mà docs mô tả.

**Nhóm 6 — Tính giá**

```
[dm_cong_doan] DOCS khai mà LIVE KHÔNG CÓ (3): ma_nhom_cong_doan · ten_tieng_anh · thu_tu_trong_nhom
[dm_cong_doan] LIVE CÓ mà DOCS KHÔNG KHAI (1): nhom_cong_nghe_id
[dm_bang_gia_cong_doan] DOCS khai mà LIVE KHÔNG CÓ (5): ma_cong_doan · ten_cong_doan · don_gia · don_vi_lo · don_gia_lo
[dm_bang_gia_cong_doan] LIVE CÓ mà DOCS KHÔNG KHAI (8):
    cong_doan_id · ten_cong_doan_snapshot · gia_theo_don_vi_tinh · don_vi_tron_goi ·
    gia_tron_goi · ngay_sua · nguoi_sua · nhom_cong_nghe_id
```
> `dm_bang_gia_cong_doan` là bảng **duy nhất trong phạm vi có live-thừa nhiều hơn docs-thừa** (8 vs 5) — nghĩa là **code đã đi trước tài liệu** ở đây: cơ chế giá trọn gói (`don_vi_tron_goi`, `gia_tron_goi`) và snapshot tên công đoạn là tính năng thật chưa được ghi.

---

---

## 2. T2 — QUY ƯỚC 5 NHÓM CỘT

**Luật Owner:** mọi bảng phải có đủ **(1) khoá chính · (2) khoá phụ · (3) mã nghiệp vụ · (4) ngày tạo · (5) người tạo**.

**Cách chấm (khai rõ để Owner kiểm lại được):**
- **PK** — có `PRIMARY KEY`.
- **FK** — có ít nhất một ràng buộc `FOREIGN KEY` thật trong DB.
- **Mã nghiệp vụ** — cột định danh nghiệp vụ **của chính bản ghi đó** và **có UNIQUE**. Đã **loại trừ** thủ công các cột chỉ trùng dạng chữ: `so_luong*` (số lượng, không phải mã), `ma_khach_hang` khi nó là **FK sang bảng khác** (không phải mã của chính bảng), `ma_so_thue`/`so_cmnd_cccd` (số định danh pháp lý, không phải mã bản ghi).
- **Ngày tạo / Người tạo** — có cột `ngay_tao`/`nguoi_tao` (hoặc biến thể `created_at`/`created_by`).

### 2.1 Bảng vi phạm — 17/25 bảng

| # | Bảng | PK | FK | Mã nghiệp vụ | ngày_tạo | người_tạo | **VI PHẠM** |
|---|---|---|---|---|---|---|---|
| 1 | `dm_khach_hang` | `id` | **0** ❌ | `ma_khach_hang` ✅ | ✅ | ✅ | **THIẾU FK** |
| 2 | `kh_lien_he` | `id` | 2 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 3 | `kh_dia_chi` | `id` | 3 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 4 | `dm_nha_cung_cap` | `ma_ncc` | **0** ❌ | `ma_ncc` ✅ (là PK) | ✅ | ✅ | **THIẾU FK** |
| 5 | `dm_san_pham` | `id` | **0** ❌ | `ma_san_pham` ✅ | ✅ | ✅ | **THIẾU FK** |
| 6 | `dm_nhom_universal` | `id` | 1 ✅ | `ma_nhom` ✅ (unique theo cặp `danh_muc_nhom,ma_nhom`) | ⚠️ `created_at` | ⚠️ `created_by` | **TÊN LỆCH CHUẨN** (EN) |
| 7 | `bao_gia_item` | `id` | 2 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 8 | `bao_gia_option` | `id` | 2 ✅ | **KHÔNG** ❌ | **KHÔNG** ❌ | **KHÔNG** ❌ | 🔴 **THIẾU MÃ + NGÀY_TẠO + NGƯỜI_TẠO** |
| 9 | `don_hang_item` | `id` | 2 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 10 | `thiet_ke_yeu_cau` | `id` | 3 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 11 | `dm_bang_gia_cong_doan` | `id` | 1 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 12 | `dm_auto_pricing_formula` | `id` | 1 ✅ | `ma_pricing_formula` **KHÔNG UNIQUE** ❌ | ✅ | ✅ | **MÃ KHÔNG UNIQUE** |
| 13 | `dm_blueprint` | `id` | 3 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 14 | `dm_pricing_addon` | `id` | 3 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 15 | `dm_pricing_test_case` | `id` | 1 ✅ | **KHÔNG** ❌ | ✅ | ✅ | **THIẾU MÃ** |
| 16 | `dm_param_registry` | `id` | **0** ❌ | `param_key` ✅ unique nhưng **tên không theo chuẩn `ma_*`** ⚠️ | ✅ | ✅ | **THIẾU FK** |
| 17 | `dm_addon_type` | `id` | **0** ❌ | `ma_loai_addon` ✅ | ✅ | ✅ | **THIẾU FK** |

### 2.2 Tám bảng ĐẠT đủ 5 nhóm

`dm_dia_chi_vn` · `hr_employee_nhanvien` · `hr_vi_tri` · `dm_vat_tu` · `material_item` · `bao_gia` · `don_hang` · `dm_cong_doan`

### 2.3 Tổng hợp vi phạm theo loại

| Loại vi phạm | Số bảng | Danh sách |
|---|---|---|
| **Thiếu mã nghiệp vụ** | **9** | `kh_lien_he` · `kh_dia_chi` · `bao_gia_item` · `bao_gia_option` · `don_hang_item` · `thiet_ke_yeu_cau` · `dm_bang_gia_cong_doan` · `dm_blueprint` · `dm_pricing_addon` · `dm_pricing_test_case` *(10 dòng — `bao_gia_option` tính cả ở nhóm dưới)* |
| **Thiếu FK** | **5** | `dm_khach_hang` · `dm_nha_cung_cap` · `dm_san_pham` · `dm_param_registry` · `dm_addon_type` |
| **Thiếu ngày_tạo + người_tạo** | **1** | `bao_gia_option` |
| **Mã có nhưng KHÔNG UNIQUE** | **1** | `dm_auto_pricing_formula` (`ma_pricing_formula`) |
| **Tên cột audit lệch chuẩn** | **1** | `dm_nhom_universal` (`created_at`/`created_by` thay vì `ngay_tao`/`nguoi_tao`) |

### 2.4 Ba vi phạm đáng lo nhất

**🔴 V1 — `bao_gia_option` thiếu CẢ ngày_tạo LẪN người_tạo.**
Đây là bảng **chứa giá bán và giá vốn** (`don_gia`, `gia_von`, `gia_ban`, `thanh_tien`). Không có vết ai tạo, tạo lúc nào ⇒ **một phương án giá bị sửa thì không truy được ai sửa**. Trong 25 bảng phạm vi, đây là bảng **duy nhất** thiếu cả hai cột audit — và lại đúng là bảng nhạy cảm nhất về tiền.

**🔴 V2 — `dm_khach_hang` không có một khoá ngoại nào.**
Bảng gốc của toàn bộ nghiệp vụ bán hàng. Bốn cột đáng lẽ là FK đều **thả nổi**: `sale_phu_trach` (→ `hr_employee_nhanvien.id`, chính là X-1) · `nhom_khach_hang_id` (→ `dm_nhom_universal.id`) · `chuc_vu_dai_dien_id` · `dieu_kien_thanh_toan_id`. Trong khi đó hai bảng con `kh_lien_he`/`kh_dia_chi` **lại có** FK trỏ ngược về nó — nên quan hệ cha-con được bảo vệ, còn **quan hệ của chính bảng cha ra ngoài thì không**.

**🟠 V3 — `dm_auto_pricing_formula.ma_pricing_formula` không UNIQUE.**
UNIQUE duy nhất của bảng là cặp `(nhom_cong_nghe_id, nhom_san_pham_id)`. Nghĩa là **hai công thức khác nhau có thể mang cùng một mã** — mã mất tác dụng định danh. Cùng kiểu này: `dm_blueprint` cũng chỉ unique theo `(nhom_cong_nghe_id, nhom_san_pham_id)` và **không có cột mã nào**.

### 2.5 Ghi chú kỹ thuật đáng lưu ý (không phải vi phạm)

- **`dm_nha_cung_cap` dùng `ma_ncc varchar(20)` làm khoá chính**, không có cột `id` số. Đây là bảng duy nhất trong 25 bảng dùng khoá tự nhiên làm PK. Hệ quả: đổi mã NCC = đổi khoá chính.
- **`kh_lien_he` và `kh_dia_chi` trỏ FK về `dm_khach_hang.ma_khach_hang` (chuỗi)**, không phải `.id` (số) — khác với `bao_gia`/`don_hang`/`thiet_ke_yeu_cau` đều trỏ `.id`. Trong cùng một cụm nghiệp vụ khách hàng đang tồn tại **hai kiểu tham chiếu**.
- **`dm_param_registry` và `dm_addon_type` không có FK nào** — hợp lý nếu chúng là bảng cấu hình độc lập, nhưng `dm_addon_type` có tên gợi ý quan hệ với `dm_pricing_addon` mà **không có ràng buộc nào nối hai bảng**.


---

---

## 3. T3 — FEATURE COMPLETENESS & BLOCKER GO-LIVE

### 3.1 Bề mặt hiện có — 6 nhóm

| Nhóm | Trang chính | Trang phụ | Server action | Route API |
|---|---|---|---|---|
| **1. Khách hàng** | `/m1/khach-hang` · `/m1/khach-hang/wizard` · `/m1/khach-hang/dia-chi-vn` | `/lien-he` · `/dia-chi` = **PLACEHOLDER** | 13 export | **KHÔNG CÓ** (thư mục `api/m1/khach-hang/**` chỉ có `desktop.ini`) |
| **2. NCC + nhân viên** | `/m5/nha-cung-cap` · `/m1/nhan-su` · `/m1/vi-tri` | — | 5 + 13 + 5 | **KHÔNG CÓ** |
| **3. Sản phẩm** | `/m1/san-pham` · `/m1/vat-tu` · `/m1/material-item` · `/m0/dm-nhom-universal` | — | 5 mỗi trang | `api/vat-tu/` chỉ có `desktop.ini` |
| **4. Báo giá** | `/m3/bao-gia` | `/item` · `/option` | 16 export | `api/m3/stats` (chỉ đếm) |
| **5. Đơn hàng** | `/m3/don-hang` · `/m3/thiet-ke` | `/don-hang/item` (read-only có chủ đích) | 6 + 9 | **KHÔNG CÓ** |
| **6. Tính giá** | `/m3/tinh-gia-manual` · `/m3/tinh-gia-admin` · `/m1/cong-doan` · `/m1/bang-gia-cong-doan` | `⟨SEC-02⟩` (**cửa hậu ẩn**) · `/m3/tinh-gia-production` (đã redirect) | 2 + 0 + 5 + 8 + 6 | **18 route** |

### 3.2 CRUD — chỗ nào thiếu

| Thực thể | Tạo | Đọc | Sửa | Xoá | Ghi chú |
|---|---|---|---|---|---|
| `dm_khach_hang` + `kh_lien_he` + `kh_dia_chi` | ✅ | ✅ | ✅ | ✅ | Đầy đủ (qua wizard bundle) |
| `dm_dia_chi_vn` | ❌ | ✅ | ❌ | ❌ | **Chỉ đọc** — chỉ import cả file JSON |
| `dm_nha_cung_cap` | ✅ | ✅ | ✅ | ✅ | Đầy đủ |
| `hr_employee_nhanvien` | ✅ | ✅ | ✅ | ⛔ **chặn có chủ đích** | Xoá luôn trả `success:false`; thay bằng "nghỉ việc" |
| `hr_vi_tri` · `dm_san_pham` · `dm_vat_tu` · `material_item` · `dm_nhom_universal` | ✅ | ✅ | ✅ | ✅ | Đầy đủ |
| `bao_gia` | ✅ | ✅ | ✅ | 🔴 **KHÔNG CÓ UI** | `deleteBaoGiaAction` tồn tại, **0 caller** |
| `bao_gia_item` · `bao_gia_option` | ✅ gián tiếp | ✅ | ✅ gián tiếp | ✅ gián tiếp | **6 action lẻ đều 0 caller** — chỉ sửa được qua bundle |
| `don_hang` | ✅ (chỉ từ báo giá) | ✅ | 🔴 **chỉ trạng thái** | 🔴 **KHÔNG TỒN TẠI** | Store **không có** `updateDonHang`/`deleteDonHang` |
| `don_hang_item` | ✅ tự động | ✅ | 🔴 chỉ cờ `can_thiet_ke` | ❌ | |
| `thiet_ke_yeu_cau` | ✅ | ✅ | 🔴 **có action, 0 UI** | 🔴 **có action, 0 UI** | |
| `dm_addon_type` | ❌ | ✅ | ❌ | ❌ | **Chỉ đọc, không có UI quản trị** |
| `pricing_quote_history` | ⛔ **chặn** | ✅ | ❌ | ❌ | POST trả 409 |

**Tổng cộng ~20 server action tồn tại nhưng không UI nào gọi** — nhiều nhất ở nhóm 4 (9 action) và nhóm 5 (8 action).

### 3.3 🔴 BLOCKER CHẶN HẲN — 12 điểm quan trọng nhất

**B1+B2 — Hai trang UI GIẢ đang nằm trong menu.**
`/m3/bao-gia/item` và `/m3/bao-gia/option`: nút **Lưu / Xoá / Chọn phương án chỉ hiện toast xanh, KHÔNG ghi DB**.
- `bao-gia/item/bao-gia-item-client.tsx:▮` — `handleSave` chỉ `success("Đã cập nhật báo giá item")`
- `:▮` — `confirmDelete` chỉ toast, không xoá
- `:▮` — `handleOptionSelect` **mutate thẳng object trong mảng** rồi báo "Đã chọn option"
- `bao-gia/option/bao-gia-option-client.tsx:▮`, `:▮` — y hệt
- Cả hai file **không import một server action nào**
- **Hai trang này có link trong menu**: `app-navigation-metadata.ts:▮` và `:▮`

> Người dùng thật sẽ sửa giá, thấy báo "Đã cập nhật", F5 là **mất trắng**. Về hậu quả, cái này **tệ hơn mock data** — vì nó hiển thị dữ liệu thật rồi giả vờ ghi.
> **Bằng chứng đây là lỗi đã biết ở nơi khác:** `don-hang/item/don-hang-item-client.tsx:▮` ghi *"Trước đây trang này có form Lưu/Xóa GIẢ chỉ toast, không ghi DB — đã gỡ bỏ."* ⇒ cùng lỗi đó **vẫn còn nguyên** ở 2 trang báo giá.

**B3 — Không xoá được báo giá.** `deleteBaoGiaAction` (`m3/bao-gia/actions.ts:▮`) 0 caller. Báo giá nhập nhầm nằm lại vĩnh viễn.

**B4 — Sửa báo giá làm mất định danh mọi dòng.** `saveBaoGiaBundle` **DELETE toàn bộ rồi INSERT lại** (`m3-store.ts:▮`) ⇒ mọi `bao_gia_item.id`/`bao_gia_option.id` **đổi sau mỗi lần lưu**. Tham chiếu ngoài (đơn hàng đã tạo, lịch sử chọn option) trỏ vào id không còn tồn tại. Cộng với việc **không có transaction** (T5 §5.5) ⇒ lỗi giữa chừng là mất sạch.

**B6 — Không sửa, không xoá được đơn hàng.** `src/lib/m3-store.ts` **không hề có** hàm `updateDonHang` hay `deleteDonHang` — chỉ có `updateDonHangStatus` (`:▮`). Nhập sai một đơn = **kẹt vĩnh viễn**, chỉ đổi được trạng thái.

**B7 — `ngay_giao_du_kien` bị gán cứng = ngày tạo đơn.** `m3-store.ts:▮` truyền `todayStr` cho **cả** `ngay_dat_hang` **lẫn** `ngay_giao_du_kien`. Cột NOT NULL, **không có UI nào sửa**.
> ⇒ **Mọi đơn hàng thật đều có hạn giao là chính ngày tạo đơn.** Sai nghiệp vụ hoàn toàn.

**B8 — `dia_chi_giao_hang` không bao giờ được ghi.** Cột tồn tại nhưng INSERT (`m3-store.ts:▮`) **không có cột này**, và không có action update ⇒ đơn hàng **không có địa chỉ giao**.

**B9 — Board Thiết kế không đổi được trạng thái.** `updateThietKeYeuCauAction` (`thiet-ke/actions.ts:▮`) là hàm **duy nhất** đổi `trang_thai` nhưng **0 caller**. Yêu cầu thiết kế tạo ra ở `"cho_xu_ly"` và **kẹt vĩnh viễn ở cột "Mới"**. Cũng không xoá được (`:▮`, 0 caller).

**B12+B13+B14 — Tính giá thủ công: không lưu được, giá thì hardcode, engine đúng thì chết.** (Chi tiết ở §6.)

**B15 — Toàn bộ API tính giá không có guard.** (Chi tiết ở §6.)

**🔴 B16 — `⟨SEC-02⟩` viết SQL vào 3 cột KHÔNG TỒN TẠI — em đã xác minh trên schema production:**

```
Truy vấn: SELECT COUNT(*) FROM information_schema.COLUMNS
          WHERE TABLE_NAME='dm_bang_gia_cong_doan'
            AND COLUMN_NAME IN ('is_active','created_at','updated_at')
Kết quả:  0
```

Code `admin/pricing/actions.ts` dùng cả ba: `:▮` `WHERE is_active = TRUE` · `:▮` `INSERT … is_active, created_at` · `:▮` `SET is_active = FALSE, updated_at = NOW()`.
Cột thật của bảng: `id, cong_doan_id, ten_cong_doan_snapshot, don_vi, gia_theo_don_vi_tinh, don_vi_tron_goi, gia_tron_goi, cach_tinh, nguon_cap_nhat, ap_dung_tu_ngay, ap_dung_den_ngay, ghi_chu, ngay_tao, nguoi_tao, ngay_sua, nguoi_sua, nhom_cong_nghe_id`.
⇒ **Route này ném `Unknown column` ngay lần bấm đầu tiên.** Nhãn: **`RUNTIME_PROVEN`** (đo trực tiếp, không suy từ dump).

**B18+B19 — Dropdown "Sale phụ trách" sẽ RỖNG trên production, và id lẫn hai domain.**
`m1/nhan-su/actions.ts:▮`: `list.filter(nv => nv.user_id != null).map(nv => ({ id: nv.user_id!, … }))`.
Kết hợp với NP-2 đợt 2 (**0/30 nhân viên production có `user_id`**) ⇒ `listSaleStaffOptionsForCustomer` trả **mảng rỗng** ⇒ màn Khách hàng (`m1/khach-hang/page.tsx:▮`) và Wizard (`wizard/page.tsx:▮`) **không có ai để chọn làm Sale phụ trách**.
Nặng hơn: hàm trả `id = nv.user_id` (**`user_account.id`**), trong khi ownership guard so bằng `ctx.empId` (**`hr_employee_nhanvien.id`**) — `khach-hang/actions.ts:▮`, `:▮`. **Hai domain khác nhau.** Nếu hai id không trùng nhau, khách tạo ra bị gán sai người phụ trách ⇒ **sale không thấy khách của chính mình**.
> Đây là **X-1/X-3 hiện hình ở một chỗ cụ thể, có thể sửa được ngay** — và là blocker dây chuyền: nhóm 2 chặn nhóm 1.

**🔴 B20 — `dm_san_pham.ma_khach_hang` NOT NULL, không FK — và em phát hiện thêm một lỗi lệch kiểu:**

Đo trực tiếp production: `dm_san_pham.ma_khach_hang` = **`varchar(20)` NOT NULL**, bảng có **0 FK**.
Nhưng `dm_khach_hang.ma_khach_hang` = **`varchar(255)`**.

> **🔴 PHÁT HIỆN MỚI (không agent nào nêu): hai đầu của cùng một quan hệ lệch độ rộng 20 vs 255.**
> `generateMaKhachHang` sinh mã cho khách **cá nhân** theo dạng `KHTP` + **tên đầy đủ không dấu, bỏ khoảng trắng** (`khach-hang-helpers.ts:▮`) — dễ dàng vượt 20 ký tự. Khi đó khách **tạo được** (cột 255) nhưng **không gắn sản phẩm cho khách đó được** (cột 20) — hoặc bị cắt câm, tuỳ `sql_mode`.
> Vì không có FK nên DB **không phát hiện** được sự lệch này. Đây là rủi ro trực tiếp cho việc nạp dữ liệu thật đang chạy.

Ngoài ra `ma_khach_hang` NOT NULL nghĩa là **mọi sản phẩm bắt buộc gắn đúng một khách** — không tạo được sản phẩm mẫu/dùng chung; và vì không có FK nên **sinh ra sản phẩm mồ côi** (chính code đã thừa nhận: `m1/san-pham/page.tsx:▮`, `OWNER_DATA_IDENTIFICATION_PENDING`).

### 3.4 🟠 Vướng nhưng dùng được — chọn lọc

| # | Nhóm | Vấn đề | Bằng chứng |
|---|---|---|---|
| C1 | 1 | Hai trang placeholder **vẫn có link trong menu mobile** | `khach-hang/lien-he/page.tsx:▮`, `dia-chi/page.tsx:▮`; menu `mobile-actions-config.ts:▮, :▮` |
| C2 | 1 | `dm_dia_chi_vn` chỉ đọc — sai một tỉnh phải import lại cả file JSON | `dia-chi-vn/actions.ts:▮` |
| C6 | 2 | **Luồng cụt 3 tầng**: xoá NV bị chặn cứng → phải "nghỉ việc" → nghỉ việc bị chặn nếu còn phụ trách khách → bàn giao cần `can_transfer_customer` mà **quyền này chưa nạp production** | `nhan-su/actions.ts:▮`, `:▮`; `khach-hang/actions.ts:▮` |
| C10 | 3 | **`healCorruptLabelsAction` GHI DB ngay khi render trang** — một lần `GET` gây ghi dữ liệu, dựa trên map hardcode | `m0/dm-nhom-universal/page.tsx:▮`; `actions.ts:▮, :▮, :▮` |
| C11 | 3 | Xoá nhóm universal không kiểm ràng buộc → lỗi FK thô (4 bảng trỏ vào) | `actions.ts:▮` |
| C13/C16 | 4,5 | **Audit hardcode `1`**: `bao_gia_item.nguoi_tao` (`m3-store.ts:▮`) · `bao_gia.nguoi_sua` (`:▮`) · `don_hang.nguoi_tao` (`:▮`) · `don_hang_item.nguoi_tao` (`:▮`) | trái chuẩn X-3 |
| C17 | 5 | **RBAC stub luôn trả `true`** — `checkThietKePermission()` trả `hasPermission:true, userId:1` kèm 2 `TODO`. Hiện được che bởi guard thật ở `:▮`, nhưng là bẫy cho hàm tương lai | `thiet-ke/actions.ts:▮` |
| C15/C23 | 4,6 | **Hai màn dùng HAI nhóm đơn vị tính khác nhau** — xác minh production: màn báo giá lọc cứng `nhom_don_vi_tinh_id === 17` (`bao-gia/page.tsx:▮`) = nhóm **`NDT0002`**; màn bảng giá công đoạn resolve theo `ma_nhom = 'NDT0003'` (`bang-gia-cong-doan/page.tsx:▮`) = **id 18**. Cả hai nhóm đều tồn tại nên không rỗng, nhưng **là hai tập đơn vị khác nhau** | đo trực tiếp `dm_nhom_universal` |
| C19 | 6 | **Hai màn quản trị bảng giá song song, khác schema** — `/m1/bang-gia-cong-doan` (đúng cột, có guard) vs `⟨SEC-02⟩` (sai cột, không guard page). Không rõ đâu là SSOT | |

### 3.5 Mock data — trả lời trực tiếp nỗi lo của Owner

| Nhóm | Kết luận |
|---|---|
| **1, 2, 3, 5** | ✅ **SẠCH** — không có mock trên đường chạy thật |
| **4** | ✅ sạch mock data — **nhưng có 2 trang UI GIẢ** (B1, B2), hậu quả nặng hơn mock |
| **6** | 🔴 **NHÓM DUY NHẤT CÒN MOCK/HARDCODE SỐNG** — 7 điểm (xem §6) |

Ba file `src/lib/mock-m1-1.ts`, `mock-m1-2.ts`, `mock-m3.ts` **đều export mảng rỗng**, và importer duy nhất là `src/lib/dual-key-resolver.ts:▮` — mà **file này không được import ở đâu cả** ⇒ dead code, vô hại.
Ngoài phạm vi 6 nhóm (ghi nhận, không audit): `mock-mf.ts` (**213 dòng, CÓ dữ liệu thật**) được `src/lib/mf-store.ts:▮` import — thuộc module Tài chính; `mock-m0-form-in.ts` → `form-in-helpers.ts:▮`.

### 3.6 Không xác minh được (T3)

Agent T3 dùng `backup-vso-to-local.sql` làm nguồn DDL (dump trong repo) chứ không truy vấn DB. **Em đã đối chiếu lại 3 claim nặng nhất bằng production thật** (B16, B20, C15/C23 — đều xác nhận đúng). Các claim DDL còn lại của T3 giữ nhãn `FILE_PROVEN`, không phải `RUNTIME_PROVEN`.
Hai file lớn `bao-gia-client.tsx` (3.400 dòng) và `tinh-gia-admin-client.tsx` (4.689 dòng) **chỉ được grep có mục tiêu, không đọc toàn văn** — có thể còn nhánh UI giả nhỏ chưa lộ.

---

---

## 4. T4 — IMPORT READINESS (NV / KH / NCC) 🔥 **PHẦN KHẨN**

> Owner **đang nạp dữ liệu thật ở phiên khác**. Mục này viết để dùng ngay.

### 4.0 KẾT LUẬN NGẮN — đọc trước khi nạp

| Danh mục | Có công cụ nạp hàng loạt? | Sinh mã tự động? | Chống trùng? | `nguoi_tao` đúng chuẩn X-3? |
|---|---|---|---|---|
| **Nhân viên** | 🔴 **KHÔNG CÓ** | ✅ **Tốt nhất hệ** — counter + `FOR UPDATE` | 🟠 chỉ dựa UNIQUE của DB | ✅ **Đúng, fail-closed** |
| **Khách hàng** | 🟠 **Có code, KHÔNG có UI gọi** | ✅ có (`KHTP…`) nhưng **không lock** | 🟠 bulk không chuẩn hoá MST | ✅ đường bulk đúng · 🔴 đường wizard hardcode `1` |
| **NCC** | 🔴 **KHÔNG CÓ** | 🔴 **KHÔNG CÓ — gõ tay** | 🔴 **chỉ mã (do người gõ)** | 🔴 **`"system"`, kiểu `varchar`** |

**🔴 Rủi ro số 1: không có đường nạp hàng loạt cho NV và NCC.** `importCSVAction` của khách hàng tồn tại (`m1/khach-hang/actions.ts:▮`) nhưng **grep toàn repo không có component nào gọi** — không có nút "Nhập CSV/Excel" trong `khach-hang-client.tsx`. ⇒ Nếu nạp tay qua UI thì **150+ thao tác cho 30 nhân viên** và mọi rủi ro dưới đây đều hiện thực hoá.

### 4.1 Bốn cái bẫy nguy hiểm nhất — cần chặn TRƯỚC khi nạp

**🔴 B1 — Địa chỉ và liên hệ biến mất âm thầm.**
Trong `bulkInsertKhachHang` có **ba khối `try { INSERT } catch { }` RỖNG**: `m1-1-store.ts:▮` (liên hệ) · `:▮` (địa chỉ chính) · `:▮` (địa chỉ giao hàng). Lỗi ghi → nuốt, **`inserted++` vẫn tăng**. Owner sẽ thấy "nạp thành công 500 khách" trong khi hàng trăm địa chỉ/liên hệ **không hề được ghi**.
Cộng thêm: biến `skipped` khai ở `:▮`, trả về ở `:▮`, **không bao giờ được tăng** — báo cáo import sai.

**🔴 B2 — `kh_dia_chi.tinh_thanh_id` bị hardcode `= 2`.**
`m1-1-store.ts:▮` và `:▮` ghi chú *"TP.HCM mặc định"*. Không có bằng chứng nào trong code rằng id=2 là TP.HCM trong thế hệ `dm_dia_chi_vn` hiện tại. Cột này **NOT NULL + FK** (xác minh ở §2) ⇒ nếu id=2 không tồn tại thì **FK violation**, nếu tồn tại nhưng là tỉnh khác thì **toàn bộ khách gán sai tỉnh**.
Nặng hơn: **không có bất kỳ code parse tỉnh/thành nào.** Grep `parseTinh|matchTinh|aliasTinh|normalizeTinh` toàn `src/` = **0 hit**. Cũng **chưa có sentinel "Chưa xác định"**.

**🔴 B3 — NCC không có generator mã, và cột thì rất hẹp.**
Xác nhận dứt khoát: **không có hàm sinh `ma_ncc` nào**; không có key `NCC` trong `system_code_counter`; không có prefix `NCCTP` ở đâu trong `src/`. Người dùng **tự gõ**, chỉ được `.toUpperCase()` (`nha-cung-cap-client.tsx:▮`).
Đồng thời NCC là bảng có **cột hẹp nhất trong 3 danh mục** (số đo lấy từ production ở §2):

| Cột | Kiểu | Rủi ro thật |
|---|---|---|
| `so_dien_thoai` | **`varchar(15)`** | 🔴 `"09xxxxxxxx - 028xxxxxxx"` = 23 ký tự ⇒ **vỡ** |
| `ma_ncc` | `varchar(20)` **PK** | 🔴 `NCC-GIAY-AN-BINH-01` đã 19 ký tự |
| `ma_so_thue` | `varchar(20)` | 🟠 MST 13 số + gạch = 15; MST nước ngoài dễ vượt |
| `ten_viet_tat` · `tinh_thanh` · `quoc_gia` | `varchar(50)` | 🟠 |
| `email` | `varchar(100)` | 🟡 |
| `ten_ncc` | `varchar(200)` **NOT NULL** | 🟠 tên công ty VN đầy đủ dễ chạm |

**Không có kiểm độ dài ở bất kỳ đâu** — 0 `maxLength` trong client, 0 `substring` trong store. Hành vi phụ thuộc `sql_mode` của server: strict ⇒ chặn hẳn (đỡ hơn); non-strict ⇒ **cắt câm**.
⚠️ `src/lib/db.ts:▮` **không set `sql_mode`** ⇒ **chưa biết server đang ở chế độ nào** (xem §4.5).

**🔴 B4 — Hai cột ENUM của NCC sẽ từ chối giá trị tiếng Việt.**
`loai_ncc` và `trang_thai` là `enum`. Code chỉ đặt mặc định khi giá trị **falsy** (`m5-store.ts:▮`, `:▮`) — giá trị lạ được **truyền thẳng**. Nạp `"Vật tư"` / `"Đang hoạt động"` từ AppSheet ⇒ MySQL từ chối (strict) hoặc ghi chuỗi rỗng.

### 4.2 Bảng cột — schema vs code, ba danh mục

**Cột NOT NULL không có default (bắt buộc phải có giá trị khi INSERT)** — đo trực tiếp trên production:

| Bảng | Số cột bắt buộc | Danh sách |
|---|---|---|
| `hr_employee_nhanvien` | **9** | `ma_nv` · `ho_ten` · `dien_thoai` · **`vi_tri_id`** · **`phong_ban_id`** · `ngay_vao_lam` · `loai_hop_dong` · `trang_thai` · `ngay_tao` |
| `dm_khach_hang` | 5 | `ma_khach_hang` · `ten_khach_hang` · `loai_khach_hang` · `ngay_tao` · `ngay_sua` |
| `kh_lien_he` | 4 | `ma_khach_hang` · `ho_ten` · `ngay_tao` · `ngay_sua` |
| `kh_dia_chi` | 5 | `ma_khach_hang` · `dia_chi_chi_tiet` · **`tinh_thanh_id`** · `ngay_tao` · `ngay_sua` |
| `dm_nha_cung_cap` | 4 | `ma_ncc` · `ten_ncc` · `ngay_tao` · **`nguoi_tao`** |
| `hr_vi_tri` | 3 | `ma_vi_tri` · `ten_vi_tri` · `ngay_tao` |
| `dm_dia_chi_vn` | 6 | `ma_hanh_chinh` · `ten_don_vi_hanh_chinh` · `ten_day_du` · `cap_hanh_chinh` · `level` · `ngay_tao` |

**Hai FK NOT NULL chặn cứng việc nạp nhân viên:** `vi_tri_id` → `hr_vi_tri` và `phong_ban_id` → `dm_phong_ban`. Code còn thêm **ràng buộc chéo**: nếu vị trí có `phong_ban_id` thì phòng ban của NV **phải trùng** (`m1-3-store.ts:▮`).
⇒ **Bắt buộc seed `dm_phong_ban` và `hr_vi_tri` TRƯỚC khi nạp nhân viên.**

**Chuỗi rỗng lọt qua NOT NULL** — code truyền `|| ''` thay vì chặn: `kh_lien_he.ho_ten` (`m1-1-store.ts:▮`), `kh_dia_chi.dia_chi_chi_tiet` (`:▮`). Tạo ra bản ghi "ma" không có nội dung.

### 4.3 Sinh mã — ba mức chất lượng rất khác nhau

| Danh mục | Cơ chế | Đánh giá |
|---|---|---|
| **Nhân viên** | `allocateGlobalStt(conn,"HR_EMPLOYEE_NHANVIEN")` (`code-counter.ts:▮`) dùng **`SELECT … FOR UPDATE` trong cùng transaction với INSERT** (`m1-3-store.ts:▮, 282, 341`) → `NVTP0001` | ✅ **Chuẩn nhất hệ thống.** An toàn khi chạy song song |
| **Khách hàng** | `generateMaKhachHang` (`khach-hang-helpers.ts:▮`) — chống trùng bằng mảng `existingCodes` **in-memory**, không lock | 🟠 An toàn trong 1 lô; **không an toàn** nếu 2 lô song song hoặc có người tạo tay cùng lúc |
| **NCC** | **KHÔNG CÓ** — gõ tay | 🔴 |

**🟠 Bẫy riêng của mã khách hàng:** doanh nghiệp không có MST → phần số = `'00000'` và viết tắt chỉ **2 chữ cái** (`khach-hang-helpers.ts:▮, 162-164`). Ví dụ mọi khách "… Tân Phát" không MST đều ra `KHTPTP00000`, rồi `…01`, `…02`… Hậu tố chỉ 2 chữ số ⇒ **bản ghi thứ 100 phá format** (nối tiếp thành `100`). Với vài trăm khách thật thiếu MST, đây là rủi ro thật.

### 4.4 Dedup — mức chặt giảm dần

| Trường | Ràng buộc DB | Code | Kết luận |
|---|---|---|---|
| `dm_khach_hang.ma_khach_hang` | UNIQUE | SELECT rồi throw (`:▮`) — có TOCTOU | cứng (DB chặn cuối) |
| `dm_khach_hang.ma_so_thue` | **UNIQUE** | tạo lẻ: chuẩn hoá `\D` + throw (`:▮`) · **bulk: KHÔNG kiểm, KHÔNG chuẩn hoá** (`:▮`) | 🟠 `"03xxxxxxxx"` vs `"03xx-xxx-xxx"` **lọt lưới thành 2 dòng** |
| MST rỗng nhiều dòng | UNIQUE cho phép nhiều NULL | tạo lẻ map `'' → null` ✅ · bulk `row.ma_so_thue \|\| null` — **`" "` là truthy** ⇒ ghi khoảng trắng | 🟠 dòng thứ hai có `" "` **vỡ UNIQUE** |
| `hr_employee_nhanvien.so_cmnd_cccd` | **UNIQUE** | 🔴 **không kiểm gì trong code**; UI map `"" → null` nhưng **không `.trim()`** (`nhan-su-client.tsx:▮`) | 🟠 `"  "` vỡ UNIQUE; lỗi trả ra là message MySQL thô |
| `dm_nha_cung_cap.ma_ncc` | **PRIMARY KEY** | SELECT + throw (`m5-store.ts:▮`) | cứng |
| `dm_nha_cung_cap.ma_so_thue` | 🔴 **KHÔNG UNIQUE** | 🔴 **không kiểm gì** | 🔴 **Nạp 2 lần = nhân đôi toàn bộ NCC, không ai báo** |
| SĐT / email (mọi danh mục) | không unique | chỉ cảnh báo client cho KH (`wizard-client.tsx:▮`) | mềm — đúng chủ ý |

### 4.5 `nguoi_tao` / `ngay_tao` — đối chiếu chuẩn X-3 (`user_account.id`)

| Điểm ghi | file:line | Đúng X-3? |
|---|---|---|
| NV — action tạo | `m1/nhan-su/actions.ts:▮` `getCurrentUserId()`, **null thì từ chối tạo** | ✅ **Chuẩn nhất, fail-closed** |
| KH — action tạo lẻ | `m1/khach-hang/actions.ts:▮` | ✅ |
| KH — store bảng cha | `m1-1-store.ts:▮` `… > 0 ? … : 1` | 🟠 còn fallback `1` |
| KH — `kh_lien_he` | `m1-1-store.ts:▮` `now, 1, now, 1` | 🔴 **hardcode 1** |
| KH — `kh_dia_chi` | `m1-1-store.ts:▮` `now, 1, now, 1` | 🔴 **hardcode 1** |
| KH — bulk import | `m1-1-store.ts:▮` fail-closed, `:▮` dùng `actorId` | ✅ **tốt nhất trong 3 danh mục** |
| **NCC** | `m5-store.ts:▮, 153, 195` — `data.nguoi_tao \|\| "system"`; action **không hề gọi `getCurrentUserId()`** | 🔴 **VI PHẠM HOÀN TOÀN** |

**🔴 NCC vi phạm ở mức schema, không chỉ code:** cột `nguoi_tao` là **`varchar(100)` NOT NULL** — **không thể lưu `user_account.id` (số) mà không đổi schema**. Sửa sau khi đã nạp = phải backfill 100% bản ghi. **Nên quyết trước khi nạp.**

**🟡 Lệch múi giờ:** mọi nơi ghi thời gian bằng chuỗi từ `new Date().toISOString()` (**UTC**) trong khi phiên DB đã ghim `+07:00` (`src/lib/db.ts:▮`) ⇒ **lệch 7 giờ** so với `NOW()` của DB. Ảnh hưởng `m1-1-store.ts:▮,984` · `m1-3-store.ts:▮` · `m5-store.ts:▮`.

### 4.6 Bề mặt liên kết NHÂN VIÊN ↔ TÀI KHOẢN — ✅ **TIN TỐT: UI ĐÃ CÓ ĐẦY ĐỦ**

> Đây là điểm em dự đoán sai và cần đính chính: NP-2 của đợt 2 nêu 0/30 nhân viên có `user_id`, và em lo bề mặt liên kết chưa có. **Kiểm chứng: bề mặt đã có đủ cả action lẫn UI.**

**Server action — đủ 5 chức năng, gác đúng 3 quyền đã seed production:**

| Chức năng | file:line | Quyền |
|---|---|---|
| Liệt kê tài khoản chưa link | `m1/nhan-su/actions.ts:▮` | — |
| Xem tài khoản đã link | `:▮` | — |
| **Tạo tài khoản inactive + link** | `:▮` | `HR_CREATE_USER_ACCOUNT` + `HR_LINK_USER_ACCOUNT` |
| **Link tài khoản có sẵn** | `:▮` | `HR_LINK_USER_ACCOUNT` |
| **Unlink** | `:▮` | `HR_UNLINK_USER_ACCOUNT` |

**UI — có thật, trong khung chi tiết nhân viên** (`nhan-su-client.tsx`): panel "Tài Khoản Đăng Nhập" `:▮` · nút "Liên kết tài khoản" `:▮` · "Tạo tài khoản" `:▮` · "Hủy liên kết" `:▮` · dialog `:▮` · cảnh báo lệch email `:▮` · badge trạng thái `:▮`.

**Ràng buộc 1↔1 được tôn trọng ở đường link:** transaction + `SELECT … FOR UPDATE` (`m1-3-store.ts:▮`), chặn NV đã có tài khoản (`:▮`), chặn tài khoản đã thuộc NV khác (`:▮`).

**🟠 Nhưng đường TẠO nhân viên thì bỏ qua toàn bộ guard đó:** `createNhanVien` nhận thẳng `input.user_id ?? null` từ client (`m1-3-store.ts:▮` ← `nhan-su-client.tsx:▮`) — **vòng qua cả 3 quyền `HR_*` và cả logic 1↔1**. Chỉ còn `UNIQUE KEY user_id` của DB chặn, bằng lỗi thô.
⇒ **Khuyến nghị cho việc nạp: luôn để `user_id = NULL` khi tạo nhân viên, link sau bằng đúng đường `linkEmployeeAccount`.**

**🔴 Không có tạo tài khoản hàng loạt.** Chỉ một đường, mỗi lần một người: mở chi tiết NV → "Tạo tài khoản" → nhập email → tạo inactive → link. Sau đó **ADMIN phải vào `/m0/security`** đặt mật khẩu + kích hoạt + gán vai trò cho từng người (tách quyền có chủ đích, ghi rõ ở `actions.ts:▮`).
⇒ **30 nhân viên × ~5 thao tác ≈ 150 thao tác thủ công.** Không phải blocker kỹ thuật, nhưng là gánh nặng vận hành cần biết trước.

**🟡 Bẫy độ dài email:** `hr_employee_nhanvien.email` kiểu `text`, còn `user_account.email` là `varchar(100) UNIQUE`. Email > 100 ký tự ⇒ **tạo được nhân viên nhưng không tạo được tài khoản**.

### 4.7 Đối chiếu Plan import AppSheet (v4) ↔ code thật

`docs/PLAN-IMPORT-APPSHEET-DANHMUC-NEN.md` — **v4, trạng thái `PLAN-ONLY`**, "Duyệt v4 = cổng cuối trước Pha B".

**Plan đã chốt đúng:** mã NV giữ `NVTP{STT}` qua counter · mã KH/NCC đổi sang timestamp `KH{YYMMDDHHmmss}` · re-code toàn bộ bản ghi cũ · `dm_dia_chi_vn` 34 tỉnh 2 tầng · sentinel "Chưa xác định" `level=0` · **"CẤM cắt câm (báo vượt độ dài 15/20)"** — plan đã nhận diện đúng rủi ro `varchar(15)/(20)` của NCC.

**Điểm plan giả định mà CODE CHƯA CÓ:**

| Plan giả định | Thực tế | Mức |
|---|---|---|
| `src/lib/ma-timestamp.ts` — `generateMaTimestamp("KH"/"NCC")` | **Chưa tồn tại** (plan tự ghi *"đề xuất, chưa tạo"*). Grep = 0 hit | 🔴 |
| Thay call-site KH tại `m1-1-store.ts:▮` | Xác minh **plan trỏ đúng dòng**; code vẫn dùng `generateMaKhachHang` | 🔴 chưa làm |
| Thêm sinh mã NCC tại `m5-store.ts:▮` | Xác minh **`:▮` đúng là chỗ validate `ma_ncc`**; chưa có generator | 🔴 chưa làm |
| Parse tỉnh 2 tầng + alias 63→34 | **0 dòng code parse nào tồn tại** | 🔴 |
| Pipeline import NV → KH → NCC | **Không có importer cho NV và NCC**; KH có code nhưng không UI. **Plan không nói sẽ viết importer bằng gì** (script? action? SQL?) | 🔴 **lỗ hổng lớn nhất của plan** |

**Ba điểm plan KHÔNG hề đề cập — đề nghị bổ sung vào v5:**
1. 🔴 Ba `catch {}` rỗng nuốt lỗi liên hệ + địa chỉ (`m1-1-store.ts:▮, 1091, 1118`)
2. 🔴 Hardcode `tinh_thanh_id = 2` (`:▮`, `:▮`) — plan bàn về sentinel nhưng **không nhắc hai dòng hardcode này**
3. 🟠 `nguoi_tao = "system"` của NCC (kiểu `varchar`, không lưu được `user_account.id`) và `trang_thai` nhân viên coerce âm thầm

### 4.8 🟠 Coerce âm thầm — bẫy làm sai dữ liệu mà không báo lỗi

`hr_employee_nhanvien.trang_thai`: `normalizeNhanVienTrangThai` (`m1-3-store.ts:▮`) chỉ nhận **9 key không dấu snake_case** (`:▮`); **mọi giá trị lạ → `"thu_viec"`**. Dữ liệu AppSheet ghi `"Đang làm việc"` / `"Chính thức"` (có dấu) **không nằm trong map** ⇒ **30 nhân viên chính thức có thể bị ghi thành thử việc, không một dòng cảnh báo**.

Tương tự `dm_khach_hang.loai_khach_hang`: mọi giá trị lạ → `'doanh_nghiep'` (`m1-1-store.ts:▮`).

⇒ **Sau khi nạp phải đối chiếu `COUNT(*) GROUP BY trang_thai`** với số liệu nguồn, không tin con số "inserted".

### 4.9 Không xác minh được (T4)

1. **`sql_mode` của production** — `src/lib/db.ts:▮` không set. Quyết định "cắt câm hay báo lỗi" khi vượt `varchar` **phụ thuộc hoàn toàn** cấu hình server. Đây là câu hỏi cần trả lời **trước khi nạp NCC**.
2. **`dm_dia_chi_vn` id = 2 là gì** — không có bằng chứng code/schema nào cho biết id=2 tồn tại hay có phải TP.HCM.
3. **Số dòng và độ dài tối đa của dữ liệu AppSheet thật** — file nguồn nằm ở `docs/File DB Của Appsheet/` và `docs/Danh Mục Improt/`, chưa mở trong phiên này.
4. **Giá trị hiện tại của `system_code_counter.HR_EMPLOYEE_NHANVIEN`** — dump ghi 20, plan ghi 30.

---

---

## 5. T5 — BÁO GIÁ → ĐƠN HÀNG + RULE Q5

### 5.1 Rule Q5 — "báo giá phải ĐÃ DUYỆT GIÁ mới ra đơn": **CÓ CHẶN, chặn đúng chỗ**

Gate nằm ở **tầng store**, nên mọi caller đều phải đi qua — `src/lib/m3-store.ts:▮`:

```ts
if (baoGia.trang_thai !== "approved_for_order") {
  throw new Error("Chỉ có thể tạo đơn hàng từ báo giá đã duyệt tạo đơn (approved_for_order)")
}
```

**Không có đường vòng trong ứng dụng** — đã chứng minh:
- `INSERT INTO don_hang` chỉ tồn tại **đúng một chỗ** trong toàn `src/`: `m3-store.ts:▮`, nằm **sau** gate trong cùng hàm.
- `createDonHangFromBaoGia` chỉ được 2 file action import.
- API route M3 chỉ có `api/m3/stats/route.ts` — chỉ `SELECT COUNT(*)`, không ghi.
- Không có import CSV cho `don_hang`.

**Ba lớp gate phụ, đều nhất quán:** client chặn trước (`bao-gia-client.tsx:▮`) · dropdown chọn báo giá đã lọc sẵn `WHERE trang_thai='approved_for_order'` (`m3-store.ts:▮`) · **có test hồi quy trên DB thật** (`scripts/tests/h3-bao-gia-don-gate.test.ts` — 5 trạng thái chưa duyệt đều throw).

> ✅ **Đây là điểm sáng nhất của toàn bộ đợt 3.** Q5 được thi hành nghiêm túc, có test khoá hành vi.

### 5.2 Nhưng cổng DUYỆT thì hở — 5 điểm

Gate Q5 chặn "chưa duyệt thì không ra đơn". Vấn đề nằm ở chỗ **việc duyệt quá dễ và duyệt xong vẫn sửa được**:

| # | Vấn đề | Bằng chứng | Hệ quả |
|---|---|---|---|
| **Q5-a** | `updateBaoGiaAction` nhận `Partial<BaoGia>` → client **ghi thẳng `trang_thai`**, không qua workflow | `bao-gia/actions.ts:▮` → `m3-store.ts:▮` | Bất kỳ ai có `m3:update` gọi `updateBaoGiaAction(id,{trang_thai:'approved_for_order'})` là **tự duyệt giá cho chính mình** |
| **Q5-b** | **Không có mã quyền riêng cho hành động duyệt** — chỉ cần `m3:update` | `bao-gia/actions.ts:▮` là toàn bộ cổng quyền. Grep `requireSpecificAction` toàn repo: **0 hit trong `m3/bao-gia` và `m3/don-hang`** | Sale tự làm báo giá → tự duyệt giá của mình. Không tách được vai trò lập ≠ duyệt |
| **Q5-c** | Kiểm quyền theo vai trò trong workflow **đã bị comment out** | `workflow-service.ts:▮`: `// if (user_role && !isRoleAllowed(...))`. Tham số `user_role` **không bao giờ được truyền** từ M3 | Cơ chế phân vai trò trên chuyển trạng thái tồn tại nhưng đang tắt |
| **Q5-d** | **Duyệt xong vẫn sửa giá được**, không cần duyệt lại | `saveBaoGiaBundleAction` (`actions.ts:▮`) và `updateBaoGiaOptionAction` (`:▮`) **không kiểm `trang_thai`**. Store `saveBaoGiaBundle` giữ nguyên trạng thái (`m3-store.ts:▮`) | Duyệt giá 100tr → sửa thành 80tr → tạo đơn, **trạng thái vẫn là "đã duyệt"** |
| **Q5-e** | Điều kiện "mọi item phải có option được chọn" **chỉ kiểm ở client** | `bao-gia-client.tsx:▮`; server **không kiểm lại** | Gọi thẳng server action là bỏ qua được |

**Bổ sung:** `checkDonHangMutable` (`m3-store.ts:▮`) được định nghĩa nhưng **0 nơi gọi** — code chết dễ gây hiểu nhầm "đã có bảo vệ". Và `scripts/approve-baogia.ts:▮` ghi thẳng `UPDATE bao_gia SET trang_thai='approved_for_order' WHERE id=1` — script test còn sót, nên gỡ trước go-live.

### 5.3 Không có vết chuyển trạng thái nào cho M3

`transitionState` (`workflow-service.ts:▮`) chỉ validate rồi gọi `executePostActions` — mà cả ba hành động hậu kỳ (`handleCreateTask`, `handleNotify`, `handleSendEmail`, `:▮`) **đều là no-op** (`void title; void description;…`).

Grep `audit_log` toàn `src/`: chỉ **một** nơi thực sự ghi nghiệp vụ là `m1/khach-hang/actions.ts:▮` (chuyển sale phụ trách khách). **M3 = 0 dòng audit.** Dấu vết duy nhất còn lại là `bao_gia.nguoi_sua` — mà cột này bị **hardcode `1`** (`m3-store.ts:▮`), nên vô nghĩa.

> ⇒ **Duyệt giá — hành động có hệ quả tài chính trực tiếp — hiện không để lại bất kỳ dấu vết nào.**

### 5.4 Flow A / Flow B — hai action trùng lặp, cả hai đều đang chạy

Hai action **cùng tên** `createDonHangFromBaoGiaAction`, cùng input, cùng gọi một store, cùng cổng quyền — nhưng **side-effect khác nhau**:

| | `m3/don-hang/actions.ts:▮` | `m3/bao-gia/actions.ts:▮` |
|---|---|---|
| `revalidatePath` × 2 | ✅ | ❌ |
| `publishSse` | ❌ | ✅ |
| Ai gọi | **Flow B** — `don-hang-client.tsx:▮` | **Flow A** — `bao-gia-client.tsx:▮` và `:▮` |

⇒ Tuỳ người dùng đi đường nào: hoặc **cache Next không được làm mới**, hoặc **các tab khác không nhận realtime**. Đây là duplicate thật, cần gộp.

**Flow A** (từ màn Báo giá) có 2 nhánh:
- **A-1** "Duyệt & tạo đơn ngay" (`bao-gia-client.tsx:▮`) → popup chọn option → tạo đơn.
  ⚠️ `:▮` gọi `await handleApprove(baoGia)` nhưng hàm này **không trả giá trị** (`:▮`) — nếu duyệt thất bại, **popup vẫn mở như thường**; chỉ đến khi bấm Xác nhận mới bị store ném lỗi Q5. Gate vẫn giữ, nhưng trải nghiệm sai.
- **A-2** sheet 2 bước (`:▮`) — **cho chọn tập con item**.

**Flow B** (từ màn Đơn hàng, `don-hang-client.tsx:▮`): dropdown báo giá đã lọc → cùng popup như A-1.

**Khác biệt hành vi giữa hai flow:** `SelectOptionForOrderDialog` (dùng ở A-1 và B) **không cho chọn tập con item** — nút xác nhận bị khoá đến khi *mọi* item đều có option, rồi trả về **toàn bộ** item. Trong khi sheet A-2 **cho chọn tập con**. ⇒ Cùng một báo giá, đi hai đường ra **hai kết quả khác nhau**.

**Đơn tay (không cần báo giá): KHÔNG TẠO ĐƯỢC — đúng chủ đích.** `don-hang-client.tsx:▮` ghi rõ: *"TẠO ĐƠN: chỉ từ báo giá đã duyệt — đã BỎ ô nhập tay giả + nút Lưu giả (Owner 10/08/2026)"*.

### 5.5 `bao_gia_option` — cơ chế phương án giá

`bao_gia_item` = thông tin sản phẩm, **không mang số lượng/giá** (`types/m3.ts:▮`, các cột giá đánh dấu `DEPRECATED: moved to bao_gia_option`). `bao_gia_option` = **các phương án giá theo thang số lượng** của một item.

Quy tắc **1 option được chọn / 1 item** được cưỡng chế ở **4 chỗ** (`m3-store.ts:▮`, `:▮`, `:▮`+`:▮`, `:▮`).

Khi ra đơn, `m3-store.ts:▮` lấy option `is_selected` của từng item; snapshot sang `don_hang_item` (`:▮`): số lượng/đơn giá/giá vốn/thành tiền lấy từ **option**, tên/quy cách/id sản phẩm lấy từ **item**.

**🔴 Hai chỗ `tong_tien` không nhất quán:**

1. **`updateBaoGiaOptionSelection` (`m3-store.ts:▮`) KHÔNG gọi `updateBaoGiaTongTien`** — mà đây **chính là hàm popup tạo đơn gọi ngay trước khi tạo đơn**. Người dùng đổi option trong popup → DB đổi `is_selected` → nhưng `bao_gia.tong_tien` **giữ giá trị cũ**. Đơn hàng thì tính lại đúng (`:▮`). ⇒ **`don_hang.tong_tien` có thể ≠ `bao_gia.tong_tien`** dù chọn đủ item — **báo giá in ra và đơn hàng lệch số tiền**.
2. `updateBaoGiaAction` cho client ghi `tong_tien` tuỳ ý (`actions.ts:▮` → `m3-store.ts:▮`), ghi đè giá trị dẫn xuất.

**Bổ sung:** `saveBaoGiaBundle` (`m3-store.ts:▮`) **DELETE rồi INSERT lại item/option mà KHÔNG có transaction** — khác `createBaoGiaBundle` có đủ `START TRANSACTION`/`COMMIT`/`ROLLBACK` (`:▮`, `:▮`). Lỗi giữa chừng ⇒ **mất sạch item/option của báo giá**.

### 5.6 ⚠️ ĐÍNH CHÍNH NP-9 CỦA ĐỢT 2

Đợt 2 nêu `don_hang.da_thanh_toan` là "bẫy nghiệp vụ: tên đọc như cờ đúng/sai nhưng chứa số tiền", và cảnh báo nguy cơ có chỗ dùng nó như boolean.

**Kiểm chứng đợt 3: KHÔNG có chỗ nào dùng như boolean.** Đã soi cả 24 điểm khớp chuỗi `da_thanh_toan` trong `src/`. Ba nơi đọc đều dùng **số học** đúng, kể cả có guard chia 0 (`don-hang-client.tsx:▮`). `bien_ban_nghiem_thu.da_thanh_toan` cũng dùng đúng và **có** hàm cập nhật thật (`mf-nghiem-thu-store.ts:▮`).

⇒ **Cảnh báo NP-9 dạng "dùng số như cờ" KHÔNG được xác nhận.** Nguy cơ vẫn có thật về mặt đặt tên (đề xuất đổi tên ở N5 đợt 2 vẫn giữ nguyên giá trị), nhưng hiện chưa gây lỗi.

**🔴 Nhưng phát hiện một lỗi nghiệp vụ THẬT, nặng hơn, ở đúng cột đó:**

`don_hang.da_thanh_toan` **vĩnh viễn = 0** và `con_lai` **vĩnh viễn = tong_tien**, vì **không tồn tại bất kỳ đường UPDATE nào** sau khi INSERT. Grep `UPDATE don_hang` toàn `src/` chỉ ra 2 chỗ, đều không đụng hai cột này. Hệ quả trên màn hình:

- Thanh tiến độ thanh toán (`don-hang-client.tsx:▮`) **luôn 0%**
- Thẻ "Còn lại" (`:▮`) **luôn bằng tổng tiền đơn**
- Tổng "còn lại" toàn danh sách (`:▮`) **= tổng doanh số** ⇒ **con số tài chính hiển thị trên màn Đơn hàng đang sai**

> Đây là blocker go-live thật của nhóm Đơn hàng: hoặc nối với luồng thanh toán MF, hoặc **ẩn các số này đi** cho tới khi nối được.

### 5.7 Sơ đồ luồng — đánh dấu nơi có gate và nơi không

```
[Tạo báo giá]  createBaoGiaBundleAction  actions.ts:▮
      ✔ quyền m3:create   ✔ transaction (m3-store.ts:▮)
      │ trang_thai = 'draft'
      ▼
[Nhập option giá]  createBaoGiaOptionAction :310 · updateBaoGiaOptionAction :360
      ✔ quyền     ✘ KHÔNG gate trạng thái báo giá     ✔ tự tính lại tong_tien (:▮)
      │ auto draft→sent (m3-store.ts:▮ — ✘ không qua workflow)
      ▼
[sent] ──► [accepted]   transitionBaoGiaStatusAction  actions.ts:▮
      ✔ validateTransition        ✘ KHÔNG gate vai trò (workflow-service.ts:▮ comment out)
      ✘ KHÔNG audit / lịch sử
      ⟂ ĐƯỜNG TẮT: updateBaoGiaAction(id,{trang_thai}) actions.ts:▮ → m3-store.ts:▮
        ghi thẳng cột — ✘ KHÔNG QUA WORKFLOW  ⇒ TỰ DUYỆT ĐƯỢC
      ▼
[approved_for_order] ← "ĐÃ DUYỆT GIÁ"
      ✔ client kiểm mọi item có option (bao-gia-client.tsx:▮)
      ✘ server KHÔNG kiểm lại
      ✘ SAU KHI DUYỆT VẪN SỬA GIÁ ĐƯỢC, không cần duyệt lại
      │
      ├─ FLOW A-1 :697  → popup (✘ không chọn tập con) → actions.ts:▮ [SSE ✔ / revalidate ✘]
      │     └─ updateBaoGiaOptionSelection ✘ KHÔNG cập nhật bao_gia.tong_tien (:▮)
      ├─ FLOW A-2 :1424 sheet 2 bước (✔ chọn tập con)  → actions.ts:▮
      └─ FLOW B   :229  ✔ dropdown đã lọc → don-hang/actions.ts:▮ [revalidate ✔ / SSE ✘]
      ▼
createDonHangFromBaoGia   m3-store.ts:▮
      ★★ GATE Q5 — TẦNG STORE, mọi caller đều qua — :1113-1115 ★★
      ✔ gate "phải có option is_selected" :1132   ✔ tính lại tong_tien :1136   ✔ transaction :1140
      ✔ id_nhan_vien_phu_trach kế thừa từ báo giá :1155
      ✘ nguoi_tao hardcode = 1  :1164, :▮
      ▼
[don_hang draft] ──► confirmed ──►…   transitionDonHangStatusAction  don-hang/actions.ts:▮
      ✔ quyền + workflow      ✘ KHÔNG audit
      ✘ da_thanh_toan / con_lai KHÔNG BAO GIỜ được cập nhật
      ✘ checkDonHangMutable (m3-store.ts:▮) — 0 nơi gọi
```

### 5.8 Không xác minh được (T5)

1. **Cấu hình workflow thật của `bao_gia`/`don_hang`** — `validateTransition` đọc bảng `dm_quy_trinh` từ **DB**, agent không truy vấn DB. Nên **chưa xác minh được** transition nào thực sự được phép (VD `draft → approved_for_order` có bị cấm không). Các chuỗi trong `types/m3.ts:▮` và `app-navigation-metadata.ts:▮` chỉ là **comment**, không phải cấu hình.
2. **Có tồn tại quyền `m3:approve` trong bảng `role_action_permission` không** — ở tầng code chắc chắn **không có mã quyền nào được kiểm**; còn trong DB thì chưa tra.
3. **Trigger / stored procedure DB có ghi `don_hang` không** — không kiểm tra được.

---

---

## 6. T6 — TÍNH GIÁ

### 6.1 Câu hỏi trọng tâm: **động cơ nào đang chạy?**

Repo có **4 file tự xưng là engine tính giá**. Kết luận dứt khoát:

| Engine | Trạng thái | Bằng chứng |
|---|---|---|
| **Viết thẳng trong React** — `tinh-gia-manual-client.tsx:▮` (`useMemo`) | ✅ **ĐANG CHẠY** — đây là engine của `/m3/tinh-gia-manual`, chạy **trong trình duyệt** | khổ trải `:▮` · bình bài `:▮` · giá vốn+markup+làm tròn `:▮` |
| `src/lib/pricing-engine-new.ts:▮` `runPricingTest` | ✅ **ĐANG CHẠY** — nhưng **chỉ** ở tab Kiểm Thử của admin | caller `/api/tinh-gia/⟨SEC-01⟩`/route.ts:▮` ← `tinh-gia-admin-client.tsx:▮, :▮` |
| `src/lib/offset-pricing-engine.ts` | ✅ chạy **gián tiếp** qua engine trên | import duy nhất bởi `pricing-engine-new.ts:▮` |
| `src/lib/pricing-engine.ts` (299 dòng) | 🔴 **CHẾT 100%** | grep `from "@/lib/pricing-engine"` = **0 kết quả** |
| `src/lib/unified-pricing-adapter.ts` (378 dòng) | 🔴 **CHẾT 100%** | grep = **0 importer** |
| `computeManualPricingAction` (`tinh-gia-manual/actions.ts:▮`) + `manual-pricing-core.ts` + `process-cost-core.ts` | 🔴 **CHẾT** | grep chỉ ra **2 comment**, 0 lời gọi |

> **Hai route sống chạy hai engine hoàn toàn khác nhau, KHÔNG chia sẻ công thức.**

### 6.2 🔴 Bốn blocker nặng nhất của nhóm tính giá

**B12 — Tính giá thủ công KHÔNG LƯU ĐƯỢC.** `tinh-gia-manual-client.tsx:▮` — nút Lưu **chỉ hiện toast lỗi** *"Chưa thể lưu: đây là GIÁ ƯỚC TÍNH CŨ…"*. API cũng chặn: `api/tinh-gia/quotes/route.ts:▮` trả 409 `LEGACY_ESTIMATE_WRITE_BLOCKED`. Trang treo banner đỏ *"không được dùng để phát hành báo giá cho khách"* (`:▮`).
⇒ **Nhóm 6 hiện KHÔNG dùng được cho vận hành thật.**

**B13 — Giá người dùng nhìn thấy đến từ hằng số trong React, không phải DB:**

| Hằng số | file:line | Vào giá thế nào |
|---|---|---|
| **Giấy 500đ/tờ** | `tinh-gia-manual-client.tsx:▮` | cộng thẳng vào `giaVon` (`:▮`) |
| **Kẽm `<đơn giá kẽm — đã gỡ theo GOV-PUBLIC-SAFE-001>`**, lượt in 80đ/150đ | `:▮` | `tienKem` (`:▮`) + `tienLuotIn` (`:▮`) |
| 2 khổ giấy 65×86, 79×109 | `:▮` | quyết định số tờ ⇒ nhân lên hai dòng trên |
| Markup mặc định **25%** | `:▮` | fallback khi không resolve được công thức (`:▮`) |
| Blueprint dự phòng hardcode | `:▮` | áp dụng **im lặng** khi API rỗng (`:▮`) |
| Tham số giả `area_m2: 0`, `machine_type:"SMALL"`, `plate_count: 4` kèm `// TODO` | `:▮` | ⇒ chi phí công đoạn Universal **sai có hệ thống** |

Vi phạm trực tiếp nguyên tắc "NO HARDCODE PRICING". Chỉ được che bằng banner cảnh báo, **không phải sửa gốc**.

**B14 — Engine "đúng chuẩn" đã viết xong nhưng CHẾT.** `computeManualPricingAction` là bản có đủ: giá nạp từ DB, `can_view_cost_price` shaping, fail-closed khi thiếu `cach_tinh`. Client **chưa bao giờ gọi nó**.
> Chính test của repo thừa nhận điều này: `scripts/tests/m1-manual-pricing-parity.test.ts:▮` in ra *"client CHƯA nối server action và vẫn tự tính giá — chờ Owner chốt (OWNER_DECISION_REQUIRED)"* và **cố ý không đánh FAIL** để khỏi chặn suite.
> ⇒ **Toàn bộ lớp bảo vệ giá đã được xây xong nhưng không có hiệu lực.**

**🔴 B15 — 14/15 route API tính giá KHÔNG có bất kỳ kiểm quyền nào, và `/m3/tinh-gia-admin` cũng vậy.**

| Bề mặt | Guard |
|---|---|
| `/m3/tinh-gia-manual` page | ✅ `requireSpecificAction("can_use_manual_pricing")` |
| `/api/tinh-gia/quotes` | ✅ (và POST đã chặn ghi) |
| **`/m3/tinh-gia-admin` page** | 🔴 **KHÔNG** — chỉ `requireMenuView("m3")` từ layout |
| **14 route API còn lại** | 🔴 **KHÔNG CÓ GUARD NÀO** |

Trong đó có **đường GHI**: `formulas/route.ts:▮, :▮` (ghi đè/xoá công thức) · `blueprints/route.ts:▮, :▮` · `addons/route.ts:▮, :▮` · `params/route.ts:▮, :▮` · `bindings/route.ts:▮, :▮` · `test-cases/route.ts:▮, :▮` · `settings/route.ts:▮` (còn để nguyên `// TODO: Add proper auth check for admin role` ở `:▮`).
**Repo không có `middleware.ts`** ⇒ không có lớp chặn chung bù lại.

> ⇒ Bất kỳ ai gửi được HTTP request tới máy chủ đều **sửa/xoá được công thức giá, blueprint, addon, tham số**. Đây là blocker nghiêm trọng nhất của nhóm 6.

### 6.3 Thêm ba vấn đề về chất lượng bảo chứng

**B4' — Hai engine dùng HAI công thức khổ trải khác nhau:**
- Manual: `(W+H)*2+21` × `L+W+36` (`tinh-gia-manual-client.tsx:▮`)
- Engine test: `(L+W+20)*2` × `H+W+20` (`pricing-engine-new.ts:▮`)

⇒ **Test "PASS" ở tab Kiểm Thử không chứng minh được gì cho màn Manual.**

**B5' — Bộ kiểm thử chạy trên blueprint GIẢ và spec GIẢ.** Cả 3 route chạy test đều **bịa** `id:"mock-blueprint", blueprint_json:{}` thay vì nạp `dm_blueprint` thật (`run-test/route.ts:▮` · `run-all/route.ts:▮` · `tinh-gia-admin/test-cases/[id]/run/route.ts:▮`), và áp spec mặc định `{L:200,W:150,H:50}`, `so_luong:1000` khi thiếu input.
⇒ Kết quả PASS **không có giá trị bảo chứng**.

**B6' — `intentional_fail` chỉ có ở DB, chưa có ở code.**
Em đã **xác minh trên schema thật**: `dm_pricing_test_case` **CÓ đủ hai cột** `intentional_fail tinyint(1)` và `fail_reason varchar(200)`.
> ⇒ Điều này **giải toả nghi ngờ của T6** rằng migration `sql/migration_v060_intentional_fail.sql` có thể chưa chạy (`docs/config-ssot-map.md:461-462` ghi "Pending approval") — **migration đã chạy thật rồi.**

Nhưng: grep `intentional_fail` trong `src/` = **0 kết quả**. `run-all/route.ts:▮` không SELECT cột này, `:▮` chỉ có `passed++`/`failed++`, **không có nhánh "expected fail"**.
⇒ Test cố ý sai bị đếm là **FAIL thật**, làm nhiễu cổng chất lượng. **Cơ chế tồn tại ở tầng DB nhưng chưa được cài ở tầng ứng dụng.**

### 6.4 🟠 Tàn dư Auto Pricing / Auto Quote (PL4 cấm)

| Loại | Điểm | Đánh giá |
|---|---|---|
| **(a) Route/link người dùng bấm được** | Link "Demo V3.1" → `/m3/tinh-gia-v2` (`app-navigation-metadata.ts:▮`, render tại `m3/page.tsx:▮`) — **route không tồn tại ⇒ 404** | 🟠 link chết hiển thị cho người dùng |
| | `/m3/tinh-gia-production` — còn tồn tại nhưng đã `redirect("/m3")` (`page.tsx:▮`) | 🟡 không lộ công cụ, nhưng là route thứ 3 |
| **(b) Hàm còn được gọi** | **KHÔNG CÓ** — `runProductionPricing` (`unified-pricing-adapter.ts:▮`), `writeBackToOption` (`:▮`), `buildPricingInputFromOption` (`:▮`) đều **0 caller** | ✅ hiện chưa nguy hiểm |
| **(c) Tên bảng `dm_auto_pricing_formula`** | ~50 hit — **HỢP LỆ**, bảng thật, không tính tàn dư | ✅ |
| **(d) Comment/tên biến** | `// See tinh-gia-auto route for active implementation` (`tinh-gia-manual-client.tsx:▮`) — **trỏ tới route đã xoá**, gây hiểu nhầm | 🟡 |

> ⚠️ **Cảnh báo:** code auto-quote ghi ngược `bao_gia_option` **vẫn nằm nguyên trong `src/`**. Hiện 0 importer nên chưa chạy, **nhưng chỉ cần một dòng `import` là sống lại**. Chỉ báo cáo — không xoá, không đề xuất đổi quyết định Owner.

### 6.5 Đối chiếu docs "Smart Offset Master" ↔ live

**Docs khớp code (7 điểm):** quy tắc làm tròn (<100K→500đ, 100K–1M→1.000đ, >1M→5.000đ) · `don_gia` derive từ `gia_ban` đã làm tròn · markup SSOT = `dm_auto_pricing_formula.markup_percent` resolve 2-key · blueprint ref FK · process pricing SSOT ở `cach_tinh` JSON · addon là config+mapping không chứa giá · admin UI table-first 6 tab.

**Docs có — code chưa/đã chết (5 điểm đáng chú ý):**

| Docs nói | Thực tế |
|---|---|
| Routes chính thức `/m3/tinh-gia` + `/m3/tinh-gia-v2` | **Cả hai KHÔNG tồn tại** — docs lệch với quyết định PL4 |
| "1 runtime engine dùng chung Manual ↔ Production" | **Sai** — 2 engine tách rời (§6.1) |
| "Markup% **không hardcode** trong client" | **Vi phạm** — `tinh-gia-manual-client.tsx:▮` có default 25% |
| Cột giá lô tên `gia_lo`, `don_vi_lo` | Live dùng **`gia_tron_goi`, `don_vi_tron_goi`** — em đã xác minh trên schema thật |
| Combo thiếu blueprint phải "FAIL rõ" | Code **im lặng fallback** về hằng số (`:▮`) |

**Code có — docs không nói (4 điểm):** giá giấy 500đ hardcode · giá kẽm `<đơn giá kẽm — đã gỡ theo GOV-PUBLIC-SAFE-001>` hardcode · khổ giấy hardcode · `FORBIDDEN_PRICING_KEYS` validator (`forbidden-pricing-keys.ts:▮`).

### 6.6 Hai khoảng trống của T6 — em đã đóng bằng schema thật

| T6 khai chưa xác minh | Em đo được |
|---|---|
| *"`intentional_fail`/`fail_reason` có thực sự tồn tại trên DB không — migration ghi Pending approval"* | ✅ **CÓ ĐỦ CẢ HAI CỘT** trong `dm_pricing_test_case`. Migration đã chạy |
| *"Bảng `dm_profit_margin_rule` có tồn tại không"* | ✅ **CÓ TỒN TẠI**. Nhưng đúng như T6 nói: **0 code app nào đọc/ghi**, UI ghi "Chưa tích hợp — pending (Phase 2)" ⇒ **bảng mồ côi ở tầng ứng dụng** |

### 6.7 Không xác minh được (T6)

1. **Nội dung `dm_auto_pricing_formula.formula_json` thật** — nhánh `offset_print`/`lamination`/`foil_stamping` (`pricing-engine-new.ts:▮`) **có thể chưa bao giờ chạy** nếu DB không có rule nào dùng mode đó. Không kiểm được bằng schema (là dữ liệu, mà dữ liệu hiện tại là draft).
2. **Hành vi runtime thực tế của 2 route** — không build, không chạy app.
3. **Doc Smart Offset 566 dòng** — đọc theo outline + 4 vùng trọng yếu; các mục `:▮` chưa đối chiếu chi tiết.
4. **Spec kỹ thuật bên thứ ba** `docs/Tính Giá Offset/Code Tính Giá Của Netprint/…` — chưa đọc.

---

---

## 7. T7 — OWNERSHIP COVERAGE 6 NHÓM

### 7.1 Bảng tổng kết coverage

| Nhóm | Mức phủ | Lý do |
|---|---|---|
| **1. Khách hàng** | ✅ **ĐẦY ĐỦ** (1 dấu hỏi) | Mọi đường list/detail/direct-URL/create/update/delete/import đều fail-closed theo `sale_phu_trach`. Chỉ `transferCustomerSaleAction` (`m1/khach-hang/actions.ts:▮`) **không kiểm khách nguồn** — chỉ cần `can_transfer_customer` là chuyển được **bất kỳ** khách. Có thể là chủ ý (quyền chuyển giao = toàn cục) — **cần Owner xác nhận** |
| **2. NCC + nhân viên** | ✅ **KHÔNG CÓ — và đúng thiết kế** | Grep `sale_phu_trach` toàn `src/`: **0 hit ngoài `dm_khach_hang`**. Ba bảng này là danh mục dùng chung, gác bằng RBAC module |
| **3. Sản phẩm & quy cách** | 🟠 **MỘT PHẦN** | Đường ĐỌC/CHỌN `dm_san_pham` kế thừa ownership đầy đủ; **cả ba đường GHI không kiểm gì** (xem 7.3) |
| **4. Báo giá** | 🔴 **KHÔNG CÓ** | 17 action + 3 nguồn SSR đều không scope |
| **5. Đơn hàng + thiết kế** | 🔴 **KHÔNG CÓ** | Như nhóm 4, cộng board thiết kế |
| **6. Tính giá** | 🔴 **KHÔNG CÓ** (chưa cần cho khách, nhưng **thiếu RBAC nghiêm trọng**) | Xem 7.5 |

### 7.2 Hàm scope dùng chung — ai gọi, ai không

| Hàm | Số nơi gọi trong `src/` |
|---|---|
| `getCurrentEmployeeId` | 17 |
| `canViewAllCustomers` | 10 |
| `listCustomersScoped` | 6 |
| `filterOwnedCustomers` | 6 |
| `resolveCustomerScope` | 3 |
| `canAccessCustomer` | 3 |
| `listOwnedCustomerCodes` | **1** |

**🔴 Tám file trong phạm vi có 0 hit cho CẢ BẢY hàm scope:**
`m3/bao-gia/actions.ts` (17 action) · `m3/don-hang/actions.ts` (6 action) · `m3/thiet-ke/actions.ts` (9 action) · 3 trang `item`/`option` độc lập · `m3/tinh-gia-manual/actions.ts` · `m3/tinh-gia-admin/page.tsx`.

> **Điểm cốt lõi:** `src/lib/m3-store.ts` (1.700+ dòng, mọi truy vấn báo giá/đơn hàng/thiết kế) **không import gì từ `src/lib/security/`**. Mọi hàm `list*` là `SELECT * FROM … ORDER BY id DESC` **không tham số scope** (`:▮`, `:▮`, `:▮`, `:▮`).

### 7.3 Nhóm 3 — kế thừa ownership CÓ ở đường đọc, KHÔNG ở đường ghi

**Đường đọc/chọn — cơ chế thật, fail-closed:**
`filterSelectableForNewTransaction` (`customer-scope.ts:▮`) là hàm thuần, giữ bản ghi khi `ma_khach_hang` nằm trong tập mã khách nhìn thấy được. Tập đó sinh từ `listOwnedCustomerCodes()` → `listCustomersScoped()` → `resolveCustomerScope()` + `filterOwnedCustomers()`. Áp ở 2 nơi: `m1/san-pham/actions.ts:▮` và `m3/bao-gia/page.tsx:▮`.
Chưa link nhân viên ⇒ tập mã rỗng ⇒ **0 sản phẩm** (fail-closed đúng).

**🔴 Đường ghi — không kiểm gì:**

| Thao tác | file:line | Hậu quả |
|---|---|---|
| Create sản phẩm | `m1/san-pham/actions.ts:▮` | Sale tạo sản phẩm gắn `ma_khach_hang` của **khách người khác** |
| Update sản phẩm | `:▮` | Sale **sửa/đổi `ma_khach_hang`** của sản phẩm bất kỳ — chỉ cần biết `id` |
| Delete sản phẩm | `:▮` | Xoá sản phẩm của khách người khác (chỉ chặn theo tham chiếu) |

### 7.4 🔴 Nhóm 4 và 5 — không có scope, kèm rò giá vốn

Ngoài việc `listBaoGia()`/`listDonHang()` không lọc (đã chốt ở đợt 2), agent tìm thêm:

**IDOR đọc/ghi trên mọi action:** `getBaoGiaBundleAction` (`:▮`), `saveBaoGiaBundleAction` (`:▮`), `updateBaoGiaAction` (`:▮`), `deleteBaoGiaAction` (`:▮`), `transitionBaoGiaStatusAction` (`:▮`), item/option CRUD (`:▮`), `updateBaoGiaOptionSelectionAction` (`:▮`) — **không action nào kiểm khách hàng của bản ghi**. Biết `id` là thao tác được.

**🔴 Rò `gia_von` (giá vốn) — 6 đường:**

| Đường | file:line | Có mask? |
|---|---|---|
| `getBaoGiaBundleAction` | `bao-gia/actions.ts:▮` | ✅ **CÓ** mask |
| `getDonHangBundleAction` | `don-hang/actions.ts:▮` | ✅ **CÓ** mask |
| SSR `optionHistoryList` (20 option gần nhất, **mọi khách**) | `bao-gia/page.tsx:▮` → prop `:▮` | 🔴 **KHÔNG** |
| SSR `listDonHangItems()` | `don-hang/page.tsx:▮` → prop `:▮` | 🔴 **KHÔNG** |
| `listDonHangItemsAction` | `don-hang/actions.ts:▮` | 🔴 **KHÔNG** (cùng file với đường có mask — bị bỏ sót) |
| 3 trang độc lập `/m3/bao-gia/item`, `/m3/bao-gia/option`, `/m3/don-hang/item` | `item/page.tsx:▮`, `option/page.tsx:▮`, `don-hang/item/page.tsx:▮` | 🔴 **KHÔNG** — dump toàn bộ, chỉ có `requireMenuView("m3")` |

`maskSensitiveFields` (`action-permission.ts:▮`) **chỉ được gọi ở đúng 2 nơi** trong toàn repo.

**Board thiết kế** (`m3/thiet-ke/page.tsx:▮,24,25`) đọc `listThietKeYeuCau` + `listDonHang` + `listBaoGia` **không scope**; chỉ danh sách khách được scope (`:▮`).

**🟡 SSE M3 vẫn kèm `id`** — `publishSse("m3:bao-gia",{id})` (`bao-gia/actions.ts:▮,116,137,154`) và `publishSse("m3:don-hang",{id})` (`:▮`), trong khi M1 **đã vá** thành `{type:"changed"}` không kèm định danh (`m1/khach-hang/actions.ts:▮`). Lộ định danh/tồn tại bản ghi cho mọi phiên.

### 7.5 🔴 Nhóm 6 — phát hiện nghiêm trọng nhất về phân quyền

| Bề mặt | Guard | Đánh giá |
|---|---|---|
| `/m3/tinh-gia-manual` page | `requireSpecificAction("can_use_manual_pricing")` (`page.tsx:▮`) | ✅ |
| `computeManualPricingAction` | `requireSpecificAction` + cost gate `can_view_cost_price` (`actions.ts:▮, :▮`) | ✅ |
| `/m3/tinh-gia-admin` page | 🔴 **KHÔNG có guard riêng** — chỉ `requireMenuView("m3")` từ layout | Bất kỳ ai xem được M3 đều **mở được màn cấu hình giá** (bảng giá công đoạn, công thức, markup) |
| `/api/tinh-gia/quotes` GET | `requireSpecificAction("can_use_manual_pricing")` (`route.ts:▮`) nhưng **không mask `gia_von`/`gia_ban`** | 🟠 |
| **13 route `/api/tinh-gia/*` còn lại** | 🔴 **KHÔNG CÓ GUARD NÀO** — grep `requireSpecificAction\|requireActionPermission\|requireMenuView\|getSecurityContext` = **0 hit** | Xem dưới |

**🔴 Mười ba route API cấu hình giá không có bất kỳ kiểm quyền nào:**
**⟨SEC-01⟩ — 12 route, tên cụ thể đã lược khỏi bản công khai.**
Trong đó có **đường GHI**: `addons/route.ts:▮, :▮` · `blueprints/route.ts:▮, :▮` · `formulas/route.ts:▮, :▮` · `settings/route.ts:▮`.
**Repo không có `middleware.ts`** ⇒ **không có lớp chặn chung nào bù lại.**

**Hai hàm bảo vệ là code chết:** `assertCanManagePricingSettings` (`tinh-gia-manual/actions.ts:▮`) và `listManualPricingProductsAction` (`:▮`) — **0 nơi gọi**.

**⚠️ Đính chính một giả định:** comment `tinh-gia-manual/actions.ts:▮` ghi *"Selector sản phẩm cho màn tính giá — DÙNG LẠI action đã scoped"*. **Kiểm chứng: không đúng trên thực tế.** Grep `listManualPricingProductsAction` toàn `src/` chỉ ra **đúng 1 hit là chính dòng khai báo**. Màn tính giá thủ công hiện **không chọn khách, không chọn sản phẩm** — lookup báo giá đã bị vô hiệu (`tinh-gia-manual-client.tsx:▮`: `// REMOVED: selectedBaoGia lookup - feature disabled`). ⇒ Comment mô tả **ý định**, không phải cơ chế đang chạy. **Khi bật lại tính năng chọn báo giá/khách, ownership sẽ KHÔNG tự có.**

### 7.6 ✅ Không có chỗ nào so chéo domain sai

Đã grep các mẫu `nguoi_tao === …`, `empId.*nguoi_tao`, toàn bộ 70+ hit của `sale_phu_trach`, và mọi call site của `getCurrentEmployeeId()`/`getCurrentUserId()`.

**Không tìm thấy chỗ nào so chéo.** Backdoor cũ **đã được gỡ và có ghi chú tử tế** — `m5/giao-hang/actions.ts:▮`: *"GỠ backdoor `kh.nguoi_tao === empId` (so lệch namespace: `nguoi_tao` = `user_account.id` / `empId` = `hr_employee_nhanvien.id`) → fail-open"*.

`getCurrentUserId()` chỉ dùng để **ghi** cột audit, không dùng để so quyền. Đây là điểm kỷ luật tốt của codebase.

**🟠 Nhưng còn fallback `?? 1` cho actor audit ở M3:** `m3/thiet-ke/actions.ts:▮, 143, 182, 396` · `m3/crm/actions.ts:▮, 334` · `m3/crm/nhat-ky/actions.ts:▮` · `m3-store.ts:▮` · `m3/don-hang/actions.ts:▮`. Mẫu này đã bị cấm ở M1 nhưng vẫn còn ở M3.

### 7.7 Bổ sung từ T1/T2 — đóng một khoảng trống của T7

T7 khai chưa xác minh được *"`bao_gia`/`don_hang` có cột nhân viên phụ trách riêng không"*. **§1–§2 của báo cáo này đã trả lời bằng đo trực tiếp production:** cả `bao_gia` và `don_hang` **đều có `id_nhan_vien_phu_trach int(11) NOT NULL, KHÔNG có FK**. Cột tồn tại, nhưng như T5 chứng minh, nó **không được dùng để lọc phạm vi ở bất kỳ đâu**.

### 7.8 Không xác minh được (T7)

1. **`canViewField` được cấu hình ra sao** — `maskSensitiveFields` phụ thuộc bảng phân quyền field-level trong DB. **§6 của đợt 2 đã đo: `role_field_permission` = 0 dòng ở CẢ production lẫn dev.** ⇒ Suy ra mask hiện đang **no-op**, nhưng cần Owner xác nhận ý định.
2. **Vai trò nào được seed `can_view_all_customers` / `can_transfer_customer`** — đợt 2 đã đo: **production chỉ có 3 quyền HR**, không có quyền nào trong số này. Nghĩa là trên máy vận hành, các cổng quyền đó hiện **luôn trả false** cho mọi vai trò không phải admin.
3. **Runtime behaviour** — phân tích tĩnh, không chạy app.
4. **Thư mục ngoài `src/`** chưa quét cho các hàm scope.

---

---

## 8. CẦN OWNER QUYẾT — gom hết một chỗ

> Xếp theo mức chặn go-live. Mỗi mục kèm **khuyến nghị của em** — Owner quyết, em không tự làm.

### 8.1 🔴 NHÓM CHẶN GO-LIVE — phải quyết trước khi mở cho người dùng thật

| # | Việc cần quyết | Bối cảnh | Khuyến nghị |
|---|---|---|---|
| **Q1** | **Nạp ma trận phân quyền + liên kết 30 nhân viên ↔ tài khoản lên production?** | NP-1 + NP-2 đợt 2. Hệ quả dây chuyền vừa đo được ở đợt 3: dropdown "Sale phụ trách" sẽ **rỗng** (B18) ⇒ **không tạo được khách hàng có người phụ trách** ⇒ chặn luôn nhóm 1 | **LÀM TRƯỚC TIÊN.** Bề mặt UI liên kết **đã có sẵn đầy đủ** (§4.6) nên chỉ là việc vận hành, không cần code |
| **Q2** | **`listSaleStaffProjection` trả `user_account.id` hay `hr_employee_nhanvien.id`?** | `nhan-su/actions.ts:▮` trả `user_id`, nhưng ownership guard so bằng `empId` — **hai domain khác nhau** (B19). Đây là X-1/X-3 hiện hình ở một chỗ cụ thể | Theo quyết định đã chốt (ownership = `hr_employee_nhanvien.id`) thì **phải trả `nv.id`**. Sửa 1 dòng, nhưng phải rà chỗ đọc |
| **Q3** | **Hai trang UI GIẢ `/m3/bao-gia/item` và `/m3/bao-gia/option` — gỡ khỏi menu hay nối vào DB?** | Nút Lưu/Xoá chỉ toast, không ghi (B1, B2). Đang có link trong menu | **Gỡ khỏi menu ngay** (rẻ, chặn được thiệt hại), nối DB sau. Đã có tiền lệ: trang `/m3/don-hang/item` từng bị lỗi y hệt và **đã gỡ** |
| **Q4** | **Khoá 14 route API tính giá + `/m3/tinh-gia-admin` lại?** | Không guard nào; ai gửi được HTTP request đều sửa/xoá được công thức giá (B15). Không có `middleware.ts` | **Chặn trước go-live.** Đây là bề mặt nguy hiểm nhất toàn báo cáo |
| **Q5** | **`⟨SEC-02⟩` — sửa hay gỡ?** | Viết SQL vào 3 cột **không tồn tại** ⇒ lỗi ngay lần bấm đầu (B16, `RUNTIME_PROVEN`). Page không guard, không nằm trong menu = **cửa hậu ẩn** | **Gỡ hoặc vô hiệu.** Đã có `/m1/bang-gia-cong-doan` làm đúng — không cần hai màn song song |
| **Q6** | **Đơn hàng: `ngay_giao_du_kien` = ngày tạo đơn, `dia_chi_giao_hang` không bao giờ ghi — chấp nhận đi vận hành không?** | B7, B8. Không có UI sửa, không có `updateDonHang` (B6) | **Không nên go-live nhóm Đơn hàng** cho tới khi có đường sửa. Hoặc chấp nhận và **ẩn hai trường này đi** |
| **Q7** | **Số tiền tài chính trên màn Đơn hàng đang sai — ẩn hay nối?** | `da_thanh_toan` luôn = 0, `con_lai` luôn = tổng tiền, không có đường UPDATE nào (§5.6). Thanh tiến độ luôn 0%, tổng "còn lại" = tổng doanh số | **Ẩn tạm** cho tới khi nối luồng thanh toán MF. Hiển thị số sai còn tệ hơn không hiển thị |
| **Q8** | **Cổng duyệt giá: có thêm quyền riêng `can_approve_bao_gia` không?** | Hiện chỉ cần `m3:update` ⇒ **sale tự làm báo giá, tự duyệt giá của mình** (Q5-b). Kiểm vai trò trong workflow **đã bị comment out** (Q5-c) | **Nên tách** lập ≠ duyệt. Đây là kiểm soát nội bộ cơ bản với thứ có hệ quả tài chính |
| **Q9** | **`updateBaoGiaAction` nhận `Partial<BaoGia>` nguyên khối — đóng lại?** | Client ghi thẳng được `trang_thai`, `tong_tien`, `nguoi_tao` (Q5-a, NP-6). **Tự duyệt được** dù không qua workflow | **Đóng bằng allow-list trường.** Rẻ, chặn được cả 3 lỗ cùng lúc |

### 8.2 🔥 NHÓM KHẨN CHO VIỆC NẠP DỮ LIỆU ĐANG CHẠY

| # | Việc cần quyết | Bối cảnh | Khuyến nghị |
|---|---|---|---|
| **Q10** | **Nạp bằng công cụ gì?** | **Không có importer cho NV và NCC**; KH có code nhưng **không có UI** (§4.0) | **Viết script `scripts/import-*.ts`** thay vì nạp tay. Nạp tay 3 danh mục = mọi rủi ro §4.1 đều hiện thực hoá |
| **Q11** | **Ba `catch {}` rỗng nuốt lỗi — vá trước khi nạp?** | `m1-1-store.ts:▮, 1091, 1118`. Địa chỉ/liên hệ lỗi → **biến mất âm thầm**, `inserted` vẫn tăng (B1 §4.1) | **Phải vá trước.** Nếu không, không có cách nào biết đã mất bao nhiêu dữ liệu |
| **Q12** | **`tinh_thanh_id` hardcode `= 2` — thay bằng gì?** | `m1-1-store.ts:▮, :▮`. Không có parser tỉnh, không có sentinel (B2 §4.1) | Chạy migration sentinel **trước**, thay `2` bằng id sentinel, rồi viết parser + bảng ánh xạ 63→34 tỉnh |
| **Q13** | **`dm_nha_cung_cap.nguoi_tao` là `varchar(100)` chứa `"system"` — đổi kiểu trước khi nạp?** | Không lưu được `user_account.id` theo chuẩn X-3 nếu không đổi schema (§4.5) | **Quyết trước khi nạp.** Sửa sau = backfill 100% bản ghi |
| **Q14** | **`sql_mode` của production là gì?** | Quyết định NCC bị **cắt câm** hay **báo lỗi** khi vượt `varchar(15)/(20)`. `src/lib/db.ts` không set (§4.9) | Chạy `SELECT @@sql_mode` trước khi nạp NCC. Nếu non-strict ⇒ **bắt buộc** kiểm độ dài ở tầng import |
| **Q15** | **`dm_san_pham.ma_khach_hang varchar(20)` vs `dm_khach_hang.ma_khach_hang varchar(255)` — nới hay đổi quy tắc sinh mã?** | **Phát hiện mới của đợt này** (B20). Mã khách cá nhân dễ vượt 20 ký tự ⇒ tạo được khách nhưng **không gắn được sản phẩm** | Plan v4 đã chốt đổi sang `KH{YYMMDDHHmmss}` = **15 ký tự** — nếu làm đúng plan thì **tự khỏi**. Cần xác nhận |
| **Q16** | **`trang_thai` nhân viên coerce mọi giá trị lạ → `"thu_viec"` — chuẩn hoá nguồn trước?** | 30 nhân viên chính thức có thể bị ghi thành thử việc, **không một dòng cảnh báo** (§4.8) | Chuẩn hoá ở nguồn + **đối chiếu `COUNT(*) GROUP BY trang_thai`** sau khi nạp |
| **Q17** | **Bổ sung 3 điểm còn thiếu vào Plan import v5?** | Plan v4 **không đề cập**: 3 `catch{}` rỗng · hardcode `tinh_thanh_id=2` · `nguoi_tao="system"` của NCC (§4.7) | Nên bổ sung trước khi duyệt Pha B |

### 8.3 🟠 NHÓM TÀI LIỆU & CHUẨN HOÁ

| # | Việc cần quyết | Bối cảnh | Khuyến nghị |
|---|---|---|---|
| **Q18** | **Viết đặc tả cho 6 bảng nhóm tính giá?** | 6/8 bảng **không có `CREATE TABLE`** trong docs; 3 bảng (41 cột) **không có đến một dòng liệt kê tên** (§1.2) | Nhóm phức tạp nhất mà không có tài liệu — nên bù trước khi ai đó phải sửa nó |
| **Q19** | **17/25 bảng vi phạm quy ước 5 nhóm cột — sửa những cái nào?** | §2.3. Nặng nhất: `bao_gia_option` thiếu **cả** `ngay_tao` **lẫn** `nguoi_tao` (bảng chứa giá vốn/giá bán) | **Ưu tiên `bao_gia_option`** — bảng nhạy cảm tiền mà không truy được ai sửa |
| **Q20** | **`dm_khach_hang` không có FK nào — thêm không?** | 4 cột đáng lẽ là FK đều thả nổi, gồm `sale_phu_trach` (§2.4) | Theo quy ước 3 nhãn đã chốt (FK-DB / CODE-LINK / PROPOSED): tối thiểu **ghi rõ nhãn** cho từng cột; thêm FK thật là phase riêng |
| **Q21** | **`kh_lien_he`/`kh_dia_chi` trỏ FK về `ma_khach_hang` (chuỗi), các bảng khác trỏ `.id` (số) — thống nhất?** | Hai kiểu tham chiếu trong cùng cụm nghiệp vụ (§2.5) | Chi phí đổi cao; **khuyến nghị giữ nguyên**, chỉ ghi vào tài liệu |
| **Q22** | **Hai màn dùng hai nhóm đơn vị tính khác nhau — cố ý hay nhầm?** | Báo giá dùng nhóm id 17 (`NDT0002`), bảng giá công đoạn dùng `NDT0003` (id 18) — xác minh trên production (C15/C23) | Cần Owner xác nhận đây là hai tập đơn vị khác nhau **có chủ đích** hay là lỗi |
| **Q23** | **`intentional_fail` — cài ở tầng ứng dụng hay bỏ cột?** | Hai cột **đã có thật trong DB** (em xác minh), nhưng `src/` **0 dòng đọc** ⇒ test cố ý sai bị đếm FAIL thật (B6' §6.3) | Hoặc cài nhánh "expected fail", hoặc bỏ cột. Để lửng làm nhiễu cổng chất lượng |
| **Q24** | **Code auto-quote chết (`unified-pricing-adapter.ts`) — xoá hẳn hay giữ?** | 378 dòng, 0 importer, nhưng chứa đúng logic PL4 cấm. **Một dòng `import` là sống lại** (§6.4) | Owner đã cấm Auto Pricing ⇒ **nên xoá** để không tái phát. Em chỉ báo cáo, không tự xoá |
| **Q25** | **Cho phép commit/push báo cáo này?** | Prompt khai READ-ONLY, cấm commit/push | — |

### 8.4 Ba câu hỏi cần Owner trả lời bằng kiến thức nghiệp vụ (không tra được bằng code)

| # | Câu hỏi |
|---|---|
| **Q26** | `transferCustomerSaleAction` **không kiểm khách nguồn** — người có `can_transfer_customer` chuyển được **bất kỳ** khách, kể cả khách không phụ trách. Đây là **chủ đích** (quyền chuyển giao = toàn cục) hay là lỗ? |
| **Q27** | `listSaleStaffOptionsForCustomer` cấp bằng quyền `m1_khach_hang:view` ⇒ **mọi người xem được khách đều thấy toàn bộ danh sách sale** (id + tên). Chấp nhận được không? |
| **Q28** | `saveBaoGiaBundle` **tự động chọn option đầu tiên** khi người dùng không chọn gì (`m3-store.ts:▮`) — quyết định giá ngầm định. Giữ hay bắt buộc chọn tay? |

---

---

## 9. PHÁT HIỆN NGOÀI PHẠM VI

> Ghi riêng theo yêu cầu, **không trộn** vào T1–T7.

**NP3-1 — `healCorruptLabelsAction` GHI DB khi render trang.** `m0/dm-nhom-universal/page.tsx:▮` gọi hàm này ngay trong đường render; hàm **ghi DB** để "chữa nhãn hỏng" dựa trên map hardcode (`actions.ts:▮, :▮`). Một thao tác `GET` gây ghi dữ liệu — side-effect ngoài ý muốn, và là mẫu nguy hiểm nếu bị nhân bản.

**NP3-2 — `mock-mf.ts` (213 dòng, CÓ dữ liệu thật) đang được import ở đường chạy.** `src/lib/mf-store.ts:▮`. Thuộc module Tài chính — **ngoài phạm vi 6 nhóm**, nhưng đây là **mock data sống duy nhất còn lại** trong repo mà em thấy. Owner rất sợ mock data nên ghi nhận để xử lý ở đợt sau.

**NP3-3 — `src/lib/m3-pricing-store.ts:▮` chứa DDL viết bằng cú pháp SQLite** (`INTEGER PRIMARY KEY AUTOINCREMENT`, `TEXT`) trong một hệ chạy **MySQL/MariaDB**. Hằng số không được dùng ở đâu, nhưng là **tài liệu schema sai lệch** nằm ngay trong code.

**NP3-4 — Bốn route API orphan vẫn mở, không guard.** `/api/tinh-gia/⟨SEC-01⟩`/route.ts` · `api/tinh-gia-admin/test-cases/route.ts` · `[id]/route.ts` · `[id]/run/route.ts` — **0 UI gọi**. Bề mặt tấn công thừa.

**NP3-5 — `/api/tinh-gia/⟨SEC-01⟩` bỏ qua `nhom_cong_nghe_id` khi khớp bảng giá.** `route.ts:▮` SELECT toàn bộ `dm_bang_gia_cong_doan` không lọc, rồi chỉ giữ **dòng đầu tiên** theo `cong_doan_id` ⇒ **có thể lấy nhầm giá của công nghệ khác**. Bản đúng (có JOIN theo `nhom_cong_nghe_id` + dedup) nằm ở `tinh-gia-manual/actions.ts:▮` — nhưng đó là **bản đã chết**.

**NP3-6 — SSE của M3 vẫn kèm `id` bản ghi**, trong khi M1 đã vá thành `{type:"changed"}` không kèm định danh. `bao-gia/actions.ts:▮, 116, 137, 154` và `:▮`. Lộ định danh/tồn tại bản ghi cho mọi phiên đang mở.

**NP3-7 — Kết quả kiểm thử giá không được lưu ở đâu.** `run-test/route.ts:▮`, `run-all/route.ts:▮` chỉ trả JSON; refresh trang là mất. **Không có bằng chứng kiểm thử nào để trình go-live.**

**NP3-8 — Lệch múi giờ có hệ thống.** Mọi nơi ghi thời gian bằng chuỗi từ `new Date().toISOString()` (**UTC**) trong khi phiên DB đã ghim `+07:00` (`src/lib/db.ts:▮`) ⇒ **lệch 7 giờ** so với `NOW()`. Ảnh hưởng cả 3 danh mục đang nạp.

---

---

## 10. PHỤ LỤC — CÁCH LÀM

**Tổ chức:** 5 agent chạy song song (T3 · T4 · T5 · T6 · T7) + luồng chính làm T1/T2 và đọc production.

**Trên production (qua đường hầm SSH, chỉ đọc):** `SHOW CREATE TABLE` · `information_schema.COLUMNS / KEY_COLUMN_USAGE / REFERENTIAL_CONSTRAINTS / STATISTICS / TABLES`. **0 lệnh ghi.** Đường hầm **đã đóng**, file bí mật tạm **đã xoá**.

**Đối chiếu chéo:** em đã dùng schema production để **kiểm lại 5 claim** mà các agent khai là "chưa xác minh được" hoặc suy từ file dump — cả 5 đều cho kết quả dứt khoát:

| Claim của agent | Nguồn agent dùng | Em đo lại | Kết quả |
|---|---|---|---|
| `⟨SEC-02⟩` viết vào cột không tồn tại (B16) | dump `.sql` | `information_schema` production | ✅ **XÁC NHẬN** — 0/3 cột tồn tại |
| `dm_san_pham.ma_khach_hang` NOT NULL, không FK (B20) | dump | production | ✅ **XÁC NHẬN** + phát hiện thêm **lệch kiểu 20 vs 255** |
| `intentional_fail` có thể chưa chạy migration | `docs/config-ssot-map.md` ghi "Pending" | production | ✅ **ĐÃ CHẠY** — cả 2 cột tồn tại |
| `dm_profit_margin_rule` có tồn tại không | không rõ | production | ✅ **CÓ** — nhưng 0 code app dùng |
| Nhóm UOM id 17 / `NDT0003` (C15/C23) | code | production | ✅ **Hai nhóm KHÁC NHAU**: id 17 = `NDT0002`, `NDT0003` = id 18 |

---

════════════ BÁO CÁO KẾT THÚC ════════════

**1. ĐÃ LÀM**
- **T1:** `SHOW CREATE TABLE` + `information_schema` cho **25/25 bảng** trên production; so chữ ký prod↔dev trong phạm vi → **0 điểm khác**; quét toàn bộ `CREATE TABLE` trong `docs/` → diff 289 khớp / 87 docs-thừa / 36 live-thừa cho 19 bảng, và xác định **6 bảng không có đặc tả** (chia 2 mức chính xác).
- **T2:** chấm quy ước 5 nhóm cột cho từng bảng, **loại thủ công các cột chỉ trùng dạng chữ** (`so_luong*`, `ma_khach_hang` khi là FK, `ma_so_thue`) → **17/25 bảng vi phạm**, phân 5 loại.
- **T3:** bề mặt + CRUD + blocker cho 6 nhóm; phát hiện **~20 server action không UI nào gọi** và **2 trang UI GIẢ trong menu**.
- **T4:** truy vết 3 danh mục nạp dữ liệu — đường tạo/import, cột bắt buộc, sinh mã, dedup, `nguoi_tao`, điểm vỡ; **xác nhận bề mặt liên kết NV↔tài khoản ĐÃ CÓ ĐỦ**; đối chiếu Plan v4 ↔ code.
- **T5:** Q5 **có chặn, chặn đúng tầng store, có test khoá**; nhưng cổng **duyệt** hở 5 điểm; 2 action tạo đơn trùng lặp; **đính chính NP-9**.
- **T6:** xác định dứt khoát **động cơ nào đang chạy**; 4 blocker nặng; đối chiếu docs Smart Offset.
- **T7:** bản đồ ownership 6 nhóm; **8 file có 0 hit cho cả 7 hàm scope**; 6 đường rò `gia_von`; xác nhận **không có chỗ nào so chéo domain**.
- **Đối chiếu chéo 5 claim** của agent bằng schema production (bảng ở §10).

**2. PHẠM VI**
- **ĐỤNG:** `docs/AUDIT-VAN-HANH-SOM-DOT3-2026-08-20.md` (tạo mới) · `docs/OWNER-REQUEST-LEDGER.md` (thêm 1 mục) · `.governance/registry/tech-debt.md` (thêm nợ).
- **KHÔNG ĐỤNG:** `src/` (0 file) · `migrations/` (0) · `sql/` (0) · DDL (0 lệnh) · DML (0 lệnh) · production (**0 lệnh ghi**) · version (không bump) · commit/push/deploy (không).

**3. BẰNG CHỨNG**
- 25/25 bảng đo trên production; diff prod↔dev trong phạm vi = **0** → **`RUNTIME_PROVEN`** cho §1–§2
- `dm_bang_gia_cong_doan` **0/3** cột `is_active`/`created_at`/`updated_at` → **`RUNTIME_PROVEN`** (B16)
- `dm_san_pham.ma_khach_hang varchar(20) NOT NULL`, 0 FK, vs `dm_khach_hang.ma_khach_hang varchar(255)` → **`RUNTIME_PROVEN`** (B20 + phát hiện mới)
- `dm_pricing_test_case` **CÓ** `intentional_fail` + `fail_reason` → **`RUNTIME_PROVEN`**
- `dm_nhom_universal` id 17 = `NDT0002`, `NDT0003` = id 18 → **`RUNTIME_PROVEN`**
- Gate Q5 tại `m3-store.ts:▮`, `INSERT INTO don_hang` **chỉ 1 chỗ** trong `src/` → **`CODE_PROVEN`**
- 14/15 route `/api/tinh-gia/*` không guard; không có `middleware.ts` → **`CODE_PROVEN`**
- 2 trang UI giả chỉ toast, không import action nào → **`CODE_PROVEN`**

**4. GHI SỔ YÊU CẦU OWNER**
- [x] **ĐÃ GHI** — mục **#95**

**5. PUSH BÁO CÁO CÔNG KHAI**
- [x] **CHƯA PUSH** — prompt khai READ-ONLY, cấm commit/push/deploy. Báo cáo ở cây làm việc local, chưa commit.

**6. CÒN SÓT / CHƯA LÀM**
- Chưa đọc toàn văn `bao-gia-client.tsx` (3.400 dòng) và `tinh-gia-admin-client.tsx` (4.689 dòng) — chỉ grep có mục tiêu → `DEBT-035`
- Chưa đọc file nguồn AppSheet thật (`docs/File DB Của Appsheet/`, `docs/Danh Mục Improt/`) để đo độ dài/số dòng thực tế → `DEBT-036`
- Chưa chạy `SELECT @@sql_mode` trên production (cần cho quyết định Q14) → `DEBT-037`
- Chưa đối chiếu chi tiết doc Smart Offset phần `:▮` và spec bên thứ ba → `DEBT-038`
- Chưa audit 74 bảng ngoài phạm vi (TẠM NỢ theo chỉ định Owner)

**7. ĐANG CHỜ OWNER**
28 mục ở **§8**, gom thành 4 nhóm: **9 mục chặn go-live** (Q1–Q9) · **8 mục khẩn cho việc nạp dữ liệu** (Q10–Q17) · **7 mục tài liệu/chuẩn hoá** (Q18–Q24) · **3 câu hỏi nghiệp vụ** (Q26–Q28) + cho phép commit (Q25).

**8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC**
Trả lời **Q1** (nạp ma trận phân quyền + liên kết 30 nhân viên ↔ tài khoản lên production). Đây là nút thắt dây chuyền: chưa làm thì dropdown "Sale phụ trách" **rỗng** ⇒ không tạo được khách có người phụ trách ⇒ nhóm 1 không vận hành được, và mọi công trình ownership ở M1 vẫn chưa có tác dụng trên máy thật. Bề mặt UI đã có sẵn nên **không cần code**.

**9. CHƯA XÁC MINH ĐƯỢC**
- **`sql_mode` production** — quyết định NCC bị cắt câm hay báo lỗi khi vượt `varchar`. *Ai xác minh:* chạy `SELECT @@sql_mode` (chưa chạy trong phiên này).
- **`dm_dia_chi_vn` id = 2 là tỉnh nào** — bulk import hardcode giá trị này; là **dữ liệu**, mà dữ liệu hiện tại Owner đã khai là draft nên không dùng làm bằng chứng.
- **Nội dung `formula_json` thật** — nhánh `offset_print`/`lamination`/`foil_stamping` có thể chưa bao giờ chạy. Là dữ liệu, không kết luận được.
- **Hành vi runtime** — toàn bộ báo cáo là phân tích tĩnh + đo schema; không build, không chạy app.
- **`user_account.id` có trùng `hr_employee_nhanvien.id` không** (liên quan B19) — cần dữ liệu thật, mà dữ liệu hiện là draft.

**10. TRẠNG THÁI CHUNG**
- [ ] PASS
- [x] **PROVISIONAL** — thiếu: 28 quyết định Owner ở §8. **Điều kiện lên PASS:** Owner trả lời, tối thiểu nhóm 8.1 và 8.2.
- [ ] BLOCKED

> **7/7 nhiệm vụ T1–T7 hoàn tất đầy đủ kèm bằng chứng**, cộng mục phát hiện ngoài phạm vi. Trạng thái `PROVISIONAL` là do **quyết định nghiệp vụ còn treo**, không phải nhiệm vụ nào bỏ dở.

**11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU**
- Phiên có bị nén ngữ cảnh không: **KHÔNG**
- Tham chiếu đã đọc trong phiên: `docs/AUDIT-TONG-LUC-DOT2-2026-08-20.md` (kết quả đợt 2) · `docs/OWNER-REQUEST-LEDGER.md` · `.governance/registry/tech-debt.md` · `migrations/20260731_a2p2_mua_hang_draft_nullable.sql`. Năm agent con đọc thêm: `docs/PLAN-IMPORT-APPSHEET-DANHMUC-NEN.md` (v4), `docs/🏭 ERP TanPhat - FIX/**`, `docs/📐 Smart Offset Master…`, `src/lib/m3-store.ts`, `src/lib/m5-store.ts`, `src/lib/m1-1-store.ts`, `src/lib/m1-3-store.ts`, `src/lib/security/*`, `src/app/m1/**`, `src/app/m3/**`, `src/app/m5/**`, `src/app/api/tinh-gia/**`.
- **KHÔNG đụng UI** ⇒ `GOV-READ-STANDARD-001` không kích hoạt (phiên không sửa chữ hiển thị, lớp trình bày, màu, cột bảng, biểu tượng, component hay bố cục nào).

═══════════════════════════════════════════
