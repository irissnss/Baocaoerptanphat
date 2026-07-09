# 🔨 BUILD — kho_thanh_pham Order Filter (V0.239)

> **Plan ID:** PLAN-20260710-kho-thanh-pham-order-filter  
> **Date:** 10/07/2026  
> **Version:** V0.238 → V0.239  
> **Coordinator Decision:** Option A + M4 auto-trigger + Backfill

---

## R1 — Migration (ma_don_hang column)

```sql
ALTER TABLE kho_thanh_pham ADD COLUMN ma_don_hang VARCHAR(50) NULL AFTER lien_ket_lsx;
CREATE INDEX idx_ma_don_hang ON kho_thanh_pham(ma_don_hang);
```

**Evidence:** SHOW CREATE TABLE xác nhận cột + index tồn tại ✅

---

## R2 — TypeScript Sync + Backfill

### Files sửa:
- `src/types/m5.ts` — Thêm `ma_don_hang: string | null` vào KhoThanhPham interface
- `src/lib/m5-store.ts` — Thêm vào Row type, mapper, create, update, + `listKhoThanhPhamByDonHang()`
- `src/lib/m3-store.ts` — Thêm `listDonHangForFilter()` (lightweight dropdown data)

### Backfill:
- Record cũ (KTP-1770607077038, lien_ket_lsx=LSX-2026-001): LSX-2026-001 **không tồn tại** trong bảng lenh_san_xuat → không thể trace → ma_don_hang vẫn NULL (expected)

---

## R3 — M4 Auto-trigger (LSX done → kho_thanh_pham)

### Logic:
Khi `updateLSXStatus(trang_thai → "done")`:
1. Fetch LSX details (ma_lsx, ten_san_pham, don_vi_tinh, so_luong, don_hang_item_id)
2. Trace chain: don_hang_item_id → don_hang_item.id_don_hang → don_hang.ma_don_hang
3. INSERT kho_thanh_pham với ma_don_hang derived

### Safety:
- **Best-effort**: nếu insert kho fail → console.error, KHÔNG fail status transition
- **NULL safe**: don_hang_item_id=NULL → ma_don_hang=NULL (graceful)
- **KHÔNG sửa logic M4 hiện có** — chỉ thêm hook sau UPDATE thành công

---

## R4 — Giao Hàng Order Filter

### UI Flow:
1. Khi tạo phiếu giao hàng item → hiện dropdown "Chọn Đơn Hàng"
2. Chọn đơn hàng → load sản phẩm từ kho_thanh_pham WHERE ma_don_hang=? AND so_luong_san_sang > 0
3. Click sản phẩm → auto-fill (mã, tên, đvt, SL đơn hàng)
4. Vẫn cho nhập tay nếu không chọn từ kho

### Files sửa:
- `src/app/m5/giao-hang/actions.ts` — 2 actions mới
- `src/app/m5/giao-hang/page.tsx` — Pass donHangList prop
- `src/app/m5/giao-hang/giao-hang-client.tsx` — Order filter dropdown + kho product list

---

## Evidence

| Check | Result |
|-------|--------|
| Migration | ✅ cột + index tồn tại |
| TypeScript | ⏳ compile check |
| Version | V0.239 ✅ |
| Backfill | N/A (LSX test data không tồn tại) |

*Không chứa source code, credentials, business logic chi tiết.*
