# 🔍 BUILD G1 — material_supplier + material_price Tables

> **Plan ID:** PLAN-20260709-g1-material-tables  
> **Date:** 10/07/2026  
> **Mode:** READ-ONLY AUDIT (phát hiện bảng ĐÃ TỒN TẠI)  

---

## R1 — So sánh Local vs Audit M5

### KẾT QUẢ: CẢ 2 BẢNG ĐÃ TỒN TẠI TRÊN LOCAL!

**Audit M5 trước đó (AUDIT-M5-WAREHOUSE-20260706.md) ghi SAI:** "material_supplier + material_price tables MISSING → store queries sẽ crash"

**Thực tế:**
```
SHOW TABLES LIKE 'material_supplier'; → 1 result ✅
SHOW TABLES LIKE 'material_price';    → 1 result ✅
```

| Table | Exists | Rows | Columns | Schema |
|-------|--------|------|---------|--------|
| material_supplier | ✅ YES | 0 | 17 | Full schema with FK to dm_nha_cung_cap |
| material_price | ✅ YES | 0 | 16 | Full schema with FK to dm_nha_cung_cap + mua_hang |

### ĐÃ ĐÍNH CHÍNH G1 từ "CRITICAL" thành "RESOLVED — bảng đã tồn tại"

---

## Schema (Verified from DB)

### material_supplier (17 columns):
| Column | Type | Notes |
|--------|------|-------|
| id | varchar(50) PK | |
| material_id | varchar(50) | Ref material_item |
| ma_ncc | varchar(20) | FK → dm_nha_cung_cap |
| ma_vat_tu_cua_ncc | varchar(50) | NCC's own material code |
| ten_vat_tu_cua_ncc | varchar(200) | NCC's own name |
| la_ncc_uu_tien | tinyint(1) | Preferred supplier flag |
| thu_tu_uu_tien | int | Priority order |
| gia_tham_khao | decimal(15,2) | Reference price |
| don_vi_tinh_cua_ncc | varchar(20) | NCC's UoM |
| so_luong_toi_thieu | int | MOQ |
| thoi_gian_giao_hang | int | Lead time (days) |
| ghi_chu | text | |
| trang_thai | enum(active,inactive) | |
| ngay_tao, nguoi_tao, ngay_sua, nguoi_sua | audit | |

**UK:** (material_id, ma_ncc) — UNIQUE  
**FK:** ma_ncc → dm_nha_cung_cap ON DELETE CASCADE

### material_price (16 columns):
| Column | Type | Notes |
|--------|------|-------|
| id | varchar(50) PK | |
| material_id | varchar(50) | Ref material_item |
| ma_ncc | varchar(20) | FK → dm_nha_cung_cap |
| don_gia | decimal(15,2) | Unit price |
| don_vi_tinh | varchar(20) | |
| ngay_bat_dau | date | Price start date |
| ngay_ket_thuc | date | NULL = current |
| so_luong_toi_thieu | int | Min qty for this price |
| dieu_kien_ap_dung | text | Conditions |
| lien_ket_po | varchar(50) | FK → mua_hang.so_po |
| la_gia_hien_hanh | tinyint(1) | Current price flag |
| ghi_chu | text | |
| ngay_tao, nguoi_tao, ngay_sua, nguoi_sua | audit | |

**FK:** ma_ncc → dm_nha_cung_cap ON DELETE CASCADE  
**FK:** lien_ket_po → mua_hang ON DELETE SET NULL

---

## R2 — Không cần tạo mới

**Bảng đã tồn tại → KHÔNG cần migration.** G1 từ Audit M5 được đóng.

**Remaining concern:** Cả 2 bảng đều empty (0 rows) — cần mock data hoặc data thật để test CRUD.

*Không chứa credentials, business logic chi tiết.*
