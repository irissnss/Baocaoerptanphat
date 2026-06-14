# 📋 P0.1 Final Report — V0.218

> **Ngày:** 15/06/2026
> **Version:** V0.218
> **Type:** FINAL CONSISTENCY + IMPLEMENTATION

## Summary

| Metric | Before | After |
|--------|--------|-------|
| Roles | 6 | **7** (+THIET_KE) |
| Menu perms | 12 | **38** |
| Action perms | 0 | **31** |
| Data perms | 0 | **3** (accounting packs) |
| Field perms | 0 | **6** (cost masking) |
| Server guards | 0 | **20** (4 action files) |

## Corrections

- Page guard: ĐÃ CÓ 12/12 (báo cáo cũ sai)
- Role canonical: SALES (not SALE)
- Version split: V0.217 = KH hardening, V0.218 = P0.1 RBAC

## Actions Guarded (20 total)

- **M1.1 KH:** ownership filter, CRUD guards, transfer guard (5)
- **M3 BG:** cost masking, CRUD guards (5)
- **M3 DH:** cost masking, create, transition (4)
- **M4 LSX:** create, status transition, print form + THIET_KE state guards (6)

## Key Features

- Customer ownership: SALES only see own customers
- Cost masking: gia_von hidden for SALES/THIET_KE/KE_TOAN
- THIET_KE: create/edit LSX draft only, no approve, no cost view
- Accounting packs: CONG_NO, THU_CHI, GIAO_HANG seeded

## Test Matrix: 13/14 code-verified PASS

## Decision: **B) PARTIAL READY — INTERNAL TEST ONLY**

Needs: browser smoke test, transfer log wiring, employee data review.

---

*15/06/2026 — ERP TanPhat AI Agent*
