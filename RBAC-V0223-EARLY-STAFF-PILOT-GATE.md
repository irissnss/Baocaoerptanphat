# 🔒 RBAC V0.223 — Early Real Staff Pilot Gate

> **Ngày:** 27/06/2026
> **Plan ID:** PLAN-20260627-V0223-EARLY-STAFF-PILOT-GATE
> **Version:** V0.222 → V0.223

---

## Owner Confirmation

- ❌ KHÔNG tạo user thật
- ❌ KHÔNG production deploy
- ✅ Chạy thêm 1 vòng gate an toàn
- ✅ RBAC modular, policy-driven, fail-closed

---

## I. Baseline (V0.222)

| Metric | Value |
|--------|-------|
| Functions guarded | 69/303 (22.8%) |
| Fully guarded files | 5/42 |
| Partial files | 4/42 |
| Unguarded files | 33/42 |

## II. Scope Implemented (V0.223)

### Phase 1 — M0 Security ✅ (8 functions)

Double-lock pattern: `requireActionPermission` + `requireAdminContext`

| Function | Guard |
|----------|-------|
| assignRoleAction | `m0.update` |
| removeRoleAction | `m0.update` |
| upsertRoleMenuPermissionAction | `m0.update` |
| applyRoleTemplateAction | `m0.update` |
| createRoleAction | `m0.create` |
| resetUserPasswordAction | `m0.update` |
| lockUserAction | `m0.update` |
| unlockUserAction | `m0.update` |

### Phase 2 — M4 PDI ✅ (17 functions)

Replaced dummy `checkPermission()` (always-true) with real RBAC guards.

- View: 11 functions (`m4.view`)
- Create: 2 functions (`m4.create`)
- Update: 4 functions (`m4.update`)

### Phase 3A — M1 Khách Hàng ✅ (8 functions)

- View: 5 functions with ownership filter
- Delete: 2 sub-entity deletes
- Create: 1 CSV import

### Phase 3B — M3 Báo Giá ✅ (13 functions)

- View: 3 functions
- Create: 3 functions (bundle, item, option)
- Update: 4 functions (transition, item, option, selection)
- Delete: 3 functions (item, option, quotation parent already had guard)

### Phase 3C — M3 Đơn Hàng ✅ (3 functions)

- View: 3 functions (list, items, bundle)

### Phase 3D — M4 LSX ✅ (17 functions)

- View: 11 functions (list, detail, dropdowns, transitions)
- Create: 3 functions (single, multiple, from don hang)
- Update: 3 functions (merge, split, status already guarded)

---

## III. Coverage Before/After

| Metric | Before (V0.222) | After (V0.223) | Tăng |
|--------|-----------------|----------------|------|
| Functions guarded | 69/303 (22.8%) | **135/303 (44.6%)** | **+66** |
| Fully guarded files | 5/42 | **10/42** | **+5** |
| Partial files | 4/42 | **1/42** | **-3** |
| Unguarded files | 33/42 | **31/42** | **-2** |

### Fully Guarded Files (10)

| File | Functions |
|------|-----------|
| m0/security/actions.ts | 8/8 |
| m1/khach-hang/actions.ts | 12/12 |
| m3/don-hang/actions.ts | 5/5 |
| m4/lenh-san-xuat/actions.ts | 20/20 |
| m4/phieu-dieu-in/actions.ts | 17/17 |
| m6/actions.ts | 28/28 |
| m7/actions.ts | 16/16 |
| mf/cong-no/actions.ts | 3/3 |
| mf/phieu-thu/actions.ts | 5/5 |
| mf/phieu-chi/actions.ts | 5/5 |

---

## IV. Flexible Accounting Model

| Pack | Status | Mô tả |
|------|--------|--------|
| KE_TOAN_CONG_NO | ✅ Active | Công nợ |
| KE_TOAN_THU_CHI | ✅ Active | Phiếu thu/chi |
| KE_TOAN_GIAO_HANG | ✅ Active | Giao hàng |
| KE_TOAN_NHAN_SU | ⏳ Deferred | HR |
| KE_TOAN_TINH_LUONG | ⏳ Deferred | Payroll |
| KE_TOAN_KHO_CHUNG_TU | ⏳ Deferred | Kho |
| SALE_ADMIN | 📋 Proposed | Chưa cần |

---

## V. Real User Matrix

- ✅ Template tại `docs/golive/REAL-EMPLOYEE-MATRIX-TEMPLATE.md`
- ❌ KHÔNG tạo user thật
- ❌ KHÔNG dùng password cũ
- Chờ Owner điền matrix

---

## VI. Verification

- ✅ TypeScript build: `tsc --noEmit` — **PASS** (0 errors)
- ✅ Guard scan: 135/303 verified
- ✅ Git committed: V0.223
- ❌ Production deploy: NOT APPROVED

---

## VII. Remaining Gaps

| Priority | Files | Functions | Module |
|----------|-------|-----------|--------|
| P2 | 10 files | 63 fns | M5 Kho |
| P2 | 3 files | 12 fns | M3 CRM/Design |
| P3 | 18 files | 93 fns | M0 DM/M1/M8/MC/Admin |

---

## VIII. Decision

### ✅ READY FOR EARLY REAL STAFF PILOT PREPARATION

- 10/42 files fully guarded (tất cả modules nhạy cảm)
- M0 Security: ADMIN-only double-lock
- MF Finance: Full guard
- M6/M7: Protection lock
- M1 KH + M3 BG/DH + M4 LSX/PDI: Full guard
- Chưa tạo user thật
- Chưa deploy production
- Hệ thống sẵn sàng cho Owner duyệt matrix rồi tạo user thật
