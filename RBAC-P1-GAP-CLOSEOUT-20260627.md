# 🔒 RBAC P1 Gap Closeout Report — V0.221

> **Date:** 27/06/2026
> **Plan ID:** PLAN-20260627-RBAC-P1-GAP-CLOSEOUT
> **Version:** V0.220 → V0.221

---

## Summary

Server action-level RBAC guards implemented for **57 additional functions** across the 3 most sensitive modules: MF Finance, M6 HR, M7 Payroll.

### Before (V0.220)
- **5/42** action files guarded (M1 KH, M3 BG, M3 DH, M4 LSX, M4 PDI)
- **37 files** had ZERO server action guards

### After (V0.221)
- **10/42** action files guarded
- **57 new function guards** added
- **0 TypeScript errors** — build passes

---

## Changes Detail

### MF Finance — 3 files, 13 functions

| File | Functions | Guard Type |
|------|-----------|------------|
| mf/cong-no/actions.ts | getCongNoList, getCongNoById | `mf.view` + `can_view_customer_debt` |
| mf/cong-no/actions.ts | createCongNoAction | `mf.create` + `can_manage_customer_debt` |
| mf/phieu-thu/actions.ts | create, update, approve, collect, cancel | `mf.create/update` + `can_manage_payment` |
| mf/phieu-chi/actions.ts | create, update, approve, pay, cancel | `mf.create/update` + `can_manage_payment` |

### M6 HR — 1 file, 28 functions (Protection Lock)

ALL functions guarded with `requireActionPermission("m6", action)`:
- View: getNhanVienOptions, getCaLamViecList, getChamCongRawList, getChamCongTinhList, getNghiPhepList, getNghiPhepDetail, getOvertimeList, getOvertimeDetail
- Create: createCaLamViecAction, createChamCongRawAction, importChamCongRawAction, createChamCongTinhAction, createNghiPhepAction, createOvertimeAction
- Update: updateCaLamViecAction, toggleCaLamViecStatusAction, updateChamCongTinhAction, bulkUpdateChamCongTinhAction, lockChamCongThangAction, approveNghiPhepAction, rejectNghiPhepAction, bulkApproveNghiPhepAction, bulkRejectNghiPhepAction, submitNghiPhepAction, approveOvertimeAction, rejectOvertimeAction, bulkApproveOvertimeAction, bulkRejectOvertimeAction

> ⚠️ KE_TOAN_NHAN_SU pack NOT enabled

### M7 Payroll — 1 file, 16 functions (Protection Lock)

ALL functions guarded with `requireActionPermission("m7", action)`:
- View: getBangLuongList, getBangLuongDetail, getBangLuongItemList, getLuongPhuCapList, getNhanVienOptions, getPayrollReadinessAction
- Create: createBangLuongAction, createLuongPhuCapAction
- Update: calculatePayrollAction, transitionBangLuongAction, approvePhuCapAction, rejectPhuCapAction, recalcHeaderTotalsAction, recalcBangLuongItemAction, updateItemPaymentStateAction, createPhieuChiFromPayrollAction

> ⚠️ KE_TOAN_TINH_LUONG pack NOT enabled

### THIET_KE Scope Correction

- DB: `can_view_all_customers` → `0` (context-only access)
- THIET_KE users see only customers linked to their assigned work

---

## Remaining (P2/P3)

| Priority | Files | Module |
|----------|-------|--------|
| P2 | 13 files | M5 Kho, M3 CRM, M3 Design |
| P3 | 19 files | M1, M0, M8, MC, Admin |
| Deferred | KE_TOAN_NHAN_SU, KE_TOAN_TINH_LUONG, KE_TOAN_KHO_CHUNG_TU packs |

---

## Verification

- ✅ TypeScript build (`tsc --noEmit`): **PASS**
- ✅ DB query confirmed THIET_KE.can_view_all_customers = 0
- ✅ No breaking changes to existing UI
- ❌ No production deploy (per Owner decision)
