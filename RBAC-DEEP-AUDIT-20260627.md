# 📊 ERP Tân Phát — RBAC Deep Audit V0.219 (27/06/2026)

> **Version:** V0.219
> **Decision:** A*) INTERNAL MINI PILOT — TEST USERS ONLY
> **⛔ NOT READY FOR PRODUCTION GO-LIVE**

## System Overview

| Metric | Count |
|--------|-------|
| User accounts | 17 (all test) |
| Active roles | 5 + 2 system |
| Menu permissions | 38 |
| Action permissions | 31 |
| Field masking | 6 (gia_von hidden for SALES/TK/KT) |
| Accounting packs | 3 PASS + 3 DEFERRED |

## Security Coverage

### Page Guards: 12/12 ✅
All module layouts have security guards.

### Server Action Guards: 5/42 files guarded ⚠️

| Status | Files | Modules |
|--------|-------|---------|
| ✅ Guarded | 5 | KH, Báo giá, Đơn hàng, LSX, Security |
| ⚠️ Unguarded | 37 | M0 system, M1 master data, M3 CRM, M4 PDI, M5 Kho (10), M6 HR, M7 Payroll, M8, MC, MF Finance (3) |

### Critical Gaps

| Priority | Gap | Impact |
|----------|-----|--------|
| 🔴 P1 | MF Finance (thu/chi/công nợ) unguarded | Finance data exposed |
| 🔴 P1 | M6 HR actions unguarded | Employee data exposed |
| 🔴 P1 | M7 Payroll actions unguarded | Payroll data exposed |
| 🟡 P2 | M5 Kho (10 files) unguarded | Warehouse data exposed |
| 🟡 P2 | M1/M3 master data unguarded | Master data editable |

> **Note:** Page layout guards block URL access for all modules. Gaps are in server action calls only. Acceptable for local test pilot, NOT for production.

## Permission Matrix Summary

| Capability | ADMIN | CEO | SALES | THIET_KE | KE_TOAN |
|-----------|-------|-----|-------|----------|---------|
| View all KH | ✅ | ✅ | ❌ own only | ✅ | ✅ |
| View cost/profit | ✅ | ✅ | ❌ | ❌ | ❌ |
| Transfer KH | ✅ | ✅ | ❌ | ❌ | ❌ |
| Create LSX/PDI | ✅ | ❌ | ❌ | ✅ draft | ❌ |
| Approve production | ✅ | ✅ | ❌ | ❌ | ❌ |
| Manage debt | ✅ | — | — | — | ✅ |
| Manage payments | ✅ | — | — | — | ✅ |

## Accounting Packs

| Pack | Status |
|------|--------|
| Công Nợ | ✅ PASS |
| Thu Chi | ✅ PASS |
| Giao Hàng | ✅ PASS |
| Nhân Sự | ⏳ DEFERRED (M6 audit) |
| Tính Lương | ⏳ DEFERRED (M7 audit) |
| Kho Chứng Từ | ⏳ DEFERRED (M5 audit) |

## Recommendations

1. **P1:** Guard MF/M6/M7 action files (Finance, HR, Payroll)
2. **P2:** Guard M5 Kho + M3 CRM action files
3. **P2:** Guard M1 master data action files
4. **P3:** UI button hide/disable per permission
5. **P3:** Production deployment preparation

## Data Summary

- 3 khách hàng, 7 báo giá, 6 đơn hàng, 6 LSX
- 30 employees (14 linked to test users)
- Customer ownership: 3 KH → SALES_A

---

*Deep Audit — 27/06/2026 — ERP TanPhat AI Agent*
