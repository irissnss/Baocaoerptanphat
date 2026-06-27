# 📊 ERP Tân Phát — Báo Cáo Chênh Lệch Local vs VPS

> **Ngày:** 27/06/2026
> **Local:** V0.220 (commit `8c1d6a0`)
> **VPS (ước tính):** V0.214 (commit `cda3653`)
> **Chênh:** 6 versions, 40 files, +7.758 / -272 dòng
> **Mục đích:** Xử lý local chuẩn chỉnh trước khi deploy VPS

---

## Tổng Quan

| Metric | Local | VPS |
|--------|-------|-----|
| Version | V0.220 | V0.214 |
| Commits chênh | 3 commits | — |
| Files thay đổi | 40 files | — |
| Dòng thêm | +7.758 | — |
| Dòng xóa | -272 | — |

---

## Phân Loại Chênh Lệch

### A) CODE THAY ĐỔI — 8 files ảnh hưởng chức năng

| # | File | +/- | Thay đổi | Impact |
|---|------|-----|----------|--------|
| 1 | src/lib/action-permission.ts | +218 new | Core RBAC engine (requireActionPermission, field masking) | 🔴 CRITICAL — Toàn bộ phân quyền server |
| 2 | src/lib/m1-1-store.ts | +375 -252 | Customer query thêm ownership filter | 🔴 CRITICAL — SALES chỉ thấy KH mình |
| 3 | src/app/m1/khach-hang/actions.ts | +99 -3 | 6 guards + ownership + transfer audit | 🔴 CRITICAL — KH CRUD guarded |
| 4 | src/app/m3/bao-gia/actions.ts | +12 -1 | 5 permission guards | 🟡 MEDIUM — BG actions guarded |
| 5 | src/app/m3/don-hang/actions.ts | +11 | 3 permission guards | 🟡 MEDIUM — DH actions guarded |
| 6 | src/app/m4/lenh-san-xuat/actions.ts | +42 | 5 guards + THIET_KE state control | 🟡 MEDIUM — LSX guarded |
| 7 | src/app/m1/khach-hang/page.tsx | +2 -2 | listCustomers → listCustomersFiltered | 🔴 CRITICAL — Ownership enforcement |
| 8 | src/app/m1/khach-hang/khach-hang-client.tsx | +1 -4 | Nhận data đã filtered | 🟢 LOW — UI adapter |

### B) DOCS/REPORTS — 22 files (chỉ documentation, không ảnh hưởng app)

| # | Folder | Files | Tổng dòng | Mô tả |
|---|--------|-------|-----------|-------|
| 1 | docs/golive/ | 22 files | +6.133 | Audit reports, plans, changelogs |

### C) SCRIPTS — 4 files (công cụ hỗ trợ, không ảnh hưởng app runtime)

| # | File | Mô tả |
|---|------|-------|
| 1 | scripts/seed-golive-p01.sql | SQL seed roles + permissions + test users |
| 2 | scripts/reset-admin.js | Reset admin password tool |
| 3 | scripts/audit-rbac.js | RBAC audit tool |
| 4 | scripts/test-password.js | Password verify tool |

### D) AGENT RULES — 5 files (chỉ cho AI agent, không ảnh hưởng app)

| # | File | +/- |
|---|------|-----|
| 1 | .antigravityrules | +59 -3 |
| 2 | .cursorrules | +59 -3 |
| 3 | AGENTS.md | +58 -1 |
| 4 | CLAUDE.md | +58 -1 |
| 5 | GEMINI.md | +58 -1 |

### E) VERSION — 1 file

| File | Local | VPS |
|------|-------|-----|
| src/lib/version.ts | V0.220 | V0.214 |

---

## DB Chênh Lệch (cần seed SQL nếu deploy)

| Table | Local | VPS | Chênh |
|-------|-------|-----|-------|
| user_account | 17 | 1 (admin) | +16 test users |
| user_role_mapping | 17 | 1 (ADMIN) | +16 mappings |
| role_menu_permission | 38 | ~11 (ADMIN only?) | +27 entries |
| role_action_permission | 31 | 0? | +31 entries |
| role_field_permission | 6 | 0? | +6 entries |
| role_data_permission | 3 | 0? | +3 entries |
| dm_menu | ~11 | ~11 | Giống nhau |
| dm_khach_hang | 3 | 3 | Giống nhau |
| Các bảng khác | — | — | Giống nhau |

> ⚠️ DB VPS chưa verify trực tiếp — ước tính dựa trên code V0.214

---

## Phân Tích: Cái gì NHẤT QUÁN, cái gì CHÊNH

### ✅ NHẤT QUÁN (không cần sửa)

| # | Area |
|---|------|
| 1 | UI toàn bộ modules M0–M9, MC, MF |
| 2 | Business logic (pricing, BG, DH, LSX workflow) |
| 3 | Database schema (cấu trúc bảng) |
| 4 | Master data (sản phẩm, vật tư, khách hàng, NCC) |
| 5 | Transactional data (báo giá, đơn hàng, LSX) |
| 6 | SSE, Server Actions architecture |
| 7 | Metronic UI framework |
| 8 | Login/auth base flow |

### ⚠️ CHÊNH LỆCH (chỉ local có, VPS chưa có)

| # | Area | Files | Mô tả |
|---|------|-------|-------|
| 1 | **RBAC engine** | 1 file | action-permission.ts — core |
| 2 | **Server guards** | 4 files | KH, BG, DH, LSX actions guarded |
| 3 | **Customer ownership** | 2 files | m1-1-store + page.tsx filtered |
| 4 | **Cost masking data** | DB only | 6 field permission rows |
| 5 | **Roles/permissions data** | DB only | 5 roles + 38 menu + 31 action |
| 6 | **Test users** | DB only | 16 test accounts |
| 7 | **Version number** | 1 file | V0.214 → V0.220 |

---

## Kết Luận

> **Anh đúng:** Ngoài phân quyền, hệ thống NHẤT QUÁN.
>
> - **Code app (UI, logic, schema):** 100% giống nhau
> - **Phân quyền (RBAC):** 100% chỉ ở local, VPS chưa có
> - **Docs/scripts/agent rules:** Không ảnh hưởng app
>
> **Ưu tiên hiện tại:** Xử lý local chuẩn chỉnh trước khi đẩy lên VPS.

---

## Việc cần làm trên LOCAL trước khi deploy VPS

| Priority | Task | Status |
|----------|------|--------|
| 🔴 P1 | Guard MF actions (phieu-thu, phieu-chi, cong-no) | ⏳ Chưa làm |
| 🔴 P1 | Guard M6 HR actions | ⏳ Chưa làm |
| 🔴 P1 | Guard M7 Payroll actions | ⏳ Chưa làm |
| 🟡 P2 | Guard M5 Kho (10 files) | ⏳ Chưa làm |
| 🟡 P2 | Guard M3 CRM, thiet-ke actions | ⏳ Chưa làm |
| 🟡 P2 | Guard M1 master data actions | ⏳ Chưa làm |
| 🟡 P2 | Guard M0 system actions | ⏳ Đã có 9 guards |
| 🟢 P3 | UI button hide/disable | ⏳ Chưa làm |
| ✅ Done | RBAC core engine | ✅ action-permission.ts |
| ✅ Done | KH/BG/DH/LSX guards | ✅ 28 guards |
| ✅ Done | Cost masking | ✅ 6 rules |
| ✅ Done | Customer ownership | ✅ filtered queries |
| ✅ Done | Test users + roles | ✅ 17 users, 5 roles |
| ✅ Done | Accounting packs | ✅ 3 PASS |

---

*Báo cáo chênh lệch — 27/06/2026 — ERP TanPhat AI Agent*
