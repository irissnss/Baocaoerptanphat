# 🔍 AUDIT M5 — Kho Hàng (Warehouse) Discovery

> **Plan ID:** PLAN-20260706-m5-warehouse-audit  
> **Mode:** READ-ONLY AUDIT  
> **Date:** 09/07/2026  

---

## CONFLICT RESOLUTION

| Source | Says |
|--------|------|
| Notion "Module M5" | "Complete V3.43" |
| AUDIT-V0227 | "Skeleton — Planned" |
| **Code thật (verified)** | **FULL CRUD implementation — 10 routes, 1905-line store, 12 DB tables** |

**Kết luận:** AUDIT-V0227 **SAI** — M5 đã được build đầy đủ CRUD, không phải skeleton. Notion "Complete" gần đúng hơn nhưng vẫn thiếu cross-module integration (R3).

---

## R1 — DB Tables: 12/14 bảng tồn tại

| # | Table | Rows | Cols | PK | Status |
|---|-------|------|------|----|--------|
| 1 | dm_nha_cung_cap | 1 | 20 | ma_ncc (varchar) | ✅ Data |
| 2 | mua_hang | 0 | 22 | so_po (varchar) | ✅ Empty |
| 3 | mua_hang_item | 0 | 18 | id (varchar) | ✅ Empty |
| 4 | phieu_nhap | 0 | 15 | so_phieu_nhap (varchar) | ✅ Empty |
| 5 | phieu_nhap_item | 0 | 14 | id (varchar) | ✅ Empty |
| 6 | phieu_xuat_kho | 0 | 16 | so_phieu (varchar) | ✅ Empty |
| 7 | phieu_xuat_kho_item | 0 | 14 | id (varchar) | ✅ Empty |
| 8 | phieu_giao_hang | 1 | 18 | so_phieu (varchar) | ✅ Data |
| 9 | phieu_giao_hang_item | 2 | 14 | id (varchar) | ✅ Data |
| 10 | kho_giao_dich | 0 | 17 | id (varchar) | ✅ Empty |
| 11 | kho_thanh_pham | 1 | 14 | id (varchar) | ✅ Data |
| 12 | kiem_ke_kho | 0 | 18 | so_phieu_kiem_ke (varchar) | ✅ Empty |

**Bảng KHÔNG có trong DB nhưng có trong store:**
- `material_supplier` — referenced in m5-store.ts but **table does not exist in DB** (queries sẽ fail)
- `material_price` — referenced in m5-store.ts but **table does not exist in DB** (queries sẽ fail)

**Key FK relationships in schema:**
- mua_hang.ma_ncc → dm_nha_cung_cap.ma_ncc (FK enforced)
- mua_hang.lien_ket_lenh_sx → lenh_san_xuat.ma_lsx (COMMENT only, no FK)
- mua_hang_item.so_po → mua_hang.so_po (FK enforced, ON DELETE CASCADE)
- phieu_giao_hang_item.so_phieu → phieu_giao_hang.so_phieu (FK enforced, ON DELETE CASCADE)
- phieu_xuat_kho_item.so_phieu → phieu_xuat_kho.so_phieu (FK enforced, ON DELETE CASCADE)

---

## R2 — Code Routes: 10 sub-routes + overview

| Route | Client Lines | Actions Lines | Functions | Has CRUD? |
|-------|-------------|--------------|-----------|-----------|
| /m5 (overview) | 248 | — | — | Overview page |
| /m5/nha-cung-cap | 1,046 | 73 (5 fn) | list/get/create/update/delete | ✅ Full |
| /m5/mua-hang | 1,027 | 139 (10 fn) | CRUD + items CRUD | ✅ Full |
| /m5/phieu-nhap | 1,034 | 119 (9 fn) | CRUD + items CRUD | ✅ Full |
| /m5/phieu-xuat-kho | 1,065 | 97 (8 fn) | CRUD + items CRUD | ✅ Full |
| /m5/giao-hang | 1,048 | 130 (9 fn) | CRUD + items CRUD | ✅ Full |
| /m5/kho-thanh-pham | 638 | 73 (5 fn) | CRUD | ✅ Full |
| /m5/kho-giao-dich | 567 | 21 (3 fn) | list/stats/filtered | ⚠️ Read-only |
| /m5/kiem-ke-kho | 625 | 36 (4 fn) | list/create/update/delete | ✅ Full |
| /m5/ncc-vat-tu | 771 | 66 (5 fn) | CRUD | ✅ Full |
| /m5/gia-vat-tu | 737 | 66 (5 fn) | CRUD | ✅ Full |

**Store:** `src/lib/m5-store.ts` — 1,905 lines, 65+ exported async functions  
**Types:** `src/types/m5.ts` — 324 lines, 14 interfaces  
**Menu:** All 10 routes registered in `menu-registry.ts` with status='ready'

**Tổng: ~9,550 lines code UI + ~820 lines actions + 1,905 lines store + 324 lines types = ~12,600 lines**

---

## R3 — Cross-Module Integration

### M4 (Sản xuất) → M5:
- DB: `mua_hang.lien_ket_lenh_sx` VARCHAR(50) COMMENT 'Ref lenh_san_xuat.ma_lsx' — **column exists** but NO FK constraint
- Code: `grep lenh_san_xuat src/app/m5/` → **0 results** — UI KHÔNG reference M4
- Code: `grep lenh_san_xuat src/lib/m5-store.ts` → **0 results** — store KHÔNG reference M4
- **Verdict:** Schema hỗ trợ nhưng code chưa implement integration. Là placeholder column.

### MF (Tài chính) → M5:
- Code: `grep cong_no src/app/m5/` → **0 results**
- Code: `grep cong_no src/lib/m5-store.ts` → **0 results**
- **Verdict:** "PO completed → tạo công nợ phải trả" và "Giao hàng delivered → tạo công nợ phải thu" — **CHƯA implement**. Chỉ là design intent trên Notion.

### RBAC guards:
- `grep requireActionPermission` across M5 actions → **0 guards**
- **Verdict:** M5 actions **KHÔNG CÓ RBAC** — bất kỳ user nào login đều CRUD được.

---

## R4 — Form Mẫu Integration

### dm_form_mau M5-related records:
```
id=7  | FORM_PHIEU_GIAO_HANG          | phieu_giao_hang | default=1 | active
id=14 | FORM_PHIEU_GIAO_HANG_LEGACY_1 | phieu_giao_hang | default=0 | inactive
id=5  | FORM_PHIEU_NHAP_LEGACY_1      | phieu_nhap      | default=0 | inactive
id=12 | FORM_PHIEU_NHAP_LEGACY_2      | phieu_nhap      | default=0 | draft
id=19 | FORM_PHIEU_NHAP               | phieu_nhap      | default=1 | active
id=6  | FORM_PHIEU_XUAT_LEGACY_1      | phieu_xuat      | default=0 | draft
id=13 | FORM_PHIEU_XUAT               | phieu_xuat      | default=1 | active
id=20 | FORM_PHIEU_XUAT_LEGACY_2      | phieu_xuat      | default=0 | inactive
```

### Code usage:
- `grep FORM_PHIEU_NHAP src/` → **0 results** — Form mẫu CHƯA được gọi
- `grep FORM_PHIEU_XUAT src/` → **0 results** — Form mẫu CHƯA được gọi
- `grep FORM_PHIEU_GIAO src/` → **0 results** (trừ version.ts changelog)
- `grep printPhieu|handlePrint src/app/m5/` → **0 results**

**Verdict:** 8 form mẫu records tồn tại trong DB nhưng **KHÔNG CÓ** code nào trong M5 gọi tới chúng. In phiếu chưa implement.

---

## R5 — Build Plan Proposal (KHÔNG tự thực thi)

### Gap Summary

| Gap ID | Mô tả | Priority | Risk |
|--------|--------|----------|------|
| G1 | material_supplier + material_price tables MISSING → store queries sẽ crash | CRITICAL | Data loss/error |
| G2 | RBAC guards = 0 trên 63 server actions | HIGH | Security hole |
| G3 | M4 integration (LSX → PO) chưa implement | MEDIUM | Business logic gap |
| G4 | MF integration (PO/Giao → Công nợ) chưa implement | MEDIUM | Business logic gap |
| G5 | Print phiếu (Nhập/Xuất/Giao) chưa implement | MEDIUM | UX gap |
| G6 | 8/12 bảng EMPTY — chưa có data thật hoặc mock | LOW | Testing gap |
| G7 | UI quality chưa audit (Metronic compliance) | LOW | UI consistency |

### Proposed Build Order

**Phase 1 — Critical Fix (trước khi user dùng M5):**
1. **G1:** CREATE TABLE `material_supplier` + `material_price` — hoặc remove store functions nếu bảng chưa thiết kế
2. **G2:** Add RBAC guards vào 63 server actions (pattern từ M3/MF V0.221)

**Phase 2 — Integration:**
3. **G3:** Mua hàng ↔ LSX link (populate `lien_ket_lenh_sx`, dropdown LSX)
4. **G4:** PO approved → auto-create công nợ phải trả; Giao hàng delivered → auto-create công nợ phải thu (pattern tương tự MF phieu_thu/phieu_chi → chung_tu_ke_toan)

**Phase 3 — Print + Polish:**
5. **G5:** Print templates cho Phiếu Nhập/Xuất/Giao (premium style như Nghiệm Thu V0.237)
6. **G7:** UI audit theo Metronic protocol

### Coordinator Decisions Needed
- G1: Bảng `material_supplier` và `material_price` — design từ Notion hay chưa? Cần `SHOW CREATE TABLE` reference hay thiết kế mới?
- G4: MF integration auto-create — Owner muốn trigger khi nào? (PO approved? PO completed? Giao hàng delivered?)
- G6: Mock data hay chờ data thật từ production?

---

*Báo cáo chỉ chứa audit kết quả. Không chứa source code chi tiết, credentials.*
