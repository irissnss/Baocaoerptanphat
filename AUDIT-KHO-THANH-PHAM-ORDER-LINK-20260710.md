# 🔍 AUDIT — kho_thanh_pham Order Link + Giao Hàng Filter

> **Plan ID:** PLAN-20260710-kho-thanh-pham-order-filter  
> **Date:** 10/07/2026  
> **Mode:** READ-ONLY AUDIT  

---

## R1 — Schema thật kho_thanh_pham (14 columns)

| Column | Type | Notes |
|--------|------|-------|
| id | varchar(50) PK | |
| ma_thanh_pham | varchar(50) | Product code |
| ten_thanh_pham | varchar(200) | Product name |
| dvt | varchar(20) | Unit of measure |
| so_luong_ton | decimal(15,2) | Stock quantity |
| so_luong_san_sang | decimal(15,2) | Ready-to-ship qty |
| **lien_ket_lsx** | **varchar(50)** | **Link to lenh_san_xuat.ma_lsx** |
| vi_tri_kho | varchar(50) | Warehouse location |
| ngay_nhap | date | Receipt date |
| ghi_chu | text | |
| ngay_tao, nguoi_tao, ngay_sua, nguoi_sua | audit | |

**Indexes:** idx_ma_tp, idx_ten_tp, **idx_lien_ket_lsx**, idx_vi_tri  
**FK constraints:** KHÔNG CÓ FK nào — `lien_ket_lsx` chỉ là text, không enforce

### Data thật (1 record):
```
id=KTP-1770607077038 | ma_thanh_pham=TP-001 | ten="Hop Carton 5 Lop"
dvt=Cai | ton=500 | san_sang=350 | lien_ket_lsx=LSX-2026-001 | vi_tri=A1-03
```

### Link chain: kho_thanh_pham → đơn hàng
```
kho_thanh_pham.lien_ket_lsx → lenh_san_xuat.ma_lsx
lenh_san_xuat.don_hang_item_id → don_hang_item.id
don_hang_item.don_hang_id → don_hang.id
```
→ **CÓ THỂ trace ngược từ kho_thanh_pham → đơn hàng qua 3 JOIN**

### ❌ KHÔNG CÓ cột `ma_don_hang` trực tiếp trong kho_thanh_pham

---

## R2 — Code Audit

### Ai ghi vào kho_thanh_pham?
- **m5-store.ts:1163** — `INSERT INTO kho_thanh_pham` (CRUD từ route `/m5/kho-thanh-pham`)
- **M4 (src/app/m4/)** — `grep kho_thanh_pham` → **0 results** — M4 KHÔNG tự động ghi vào

→ **Gap:** Theo quy trình (NV sản xuất cập nhật → tự động vào kho), M4 cần trigger INSERT vào `kho_thanh_pham` khi LSX complete. Hiện tại phải nhập thủ công qua `/m5/kho-thanh-pham`.

### Route `/m5/giao-hang` đọc từ kho_thanh_pham?
- `grep kho_thanh_pham src/app/m5/giao-hang/` → **0 results**
- Route giao hàng KHÔNG filter sản phẩm từ kho thành phẩm
- UI hiện tại: Nhập tay items cho phiếu giao hàng, không lookup từ kho

---

## R3 — Đề xuất (KHÔNG tự code)

### Option A: Thêm cột `ma_don_hang` vào kho_thanh_pham (RECOMMENDED)
**Ưu điểm:**
- Query trực tiếp: `WHERE ma_don_hang = ?` — O(1) lookup, không cần 3 JOIN
- Filter giao hàng nhanh: chỉ show sản phẩm thuộc đơn hàng đang giao
- Đơn giản cho UI: dropdown chọn đơn hàng → auto-filter kho

**Nhược điểm:**
- Denormalize (data duplicate từ LSX chain)
- Cần backfill cho data cũ (1 record hiện tại)
- Cần trigger khi LSX complete: extract `ma_don_hang` từ chain

**Migration draft:**
```sql
ALTER TABLE kho_thanh_pham 
  ADD COLUMN ma_don_hang VARCHAR(50) NULL COMMENT 'Ref don_hang.ma_don_hang (denormalized)' 
  AFTER lien_ket_lsx;
CREATE INDEX idx_ma_don_hang ON kho_thanh_pham(ma_don_hang);
```

### Option B: Dùng JOIN chain hiện có (no schema change)
**Ưu điểm:**
- Không thay đổi schema
- Data luôn consistent (derive từ FK chain)

**Nhược điểm:**
- Query phức tạp: 3-level JOIN (kho_thanh_pham → lenh_san_xuat → don_hang_item → don_hang)
- `lien_ket_lsx` không có FK constraint → có thể orphan
- Performance kém khi data lớn

### Option C: Thêm cả 2 cột `ma_don_hang` + `so_lenh_sx` (full denormalize)
- `so_lenh_sx` = alias cho `lien_ket_lsx` nhưng với FK constraint
- `ma_don_hang` = denormalized từ chain
- Linh hoạt nhất nhưng nhiều cột nhất

### Recommendation: **Option A** — thêm `ma_don_hang` + sửa `/m5/giao-hang`:
1. ALTER TABLE thêm cột
2. M4 trigger: khi LSX complete → INSERT kho_thanh_pham **kèm ma_don_hang**
3. `/m5/giao-hang` tạo phiếu: dropdown chọn đơn hàng → filter kho_thanh_pham WHERE ma_don_hang = ?
4. Chỉ hiện sản phẩm có `so_luong_san_sang > 0` của đơn hàng đó

### Coordinator Decisions Needed:
1. **Option A hay B hay C?**
2. **M4 auto-trigger hay vẫn nhập tay kho?**
3. **Backfill data cũ** (1 record LSX-2026-001) — có cần link đơn hàng không?

---

*Không chứa source code, credentials, business logic chi tiết.*
