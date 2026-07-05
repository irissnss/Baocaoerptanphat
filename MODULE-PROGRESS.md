# 📦 Module Progress — ERP Tân Phát

> Chi tiết chức năng đã hoàn thành, đang phát triển, và planned cho từng module.
> 
> **Cập nhật:** 14/06/2026 — Audit toàn hệ thống

---

## M0 — Hệ Thống (✅ Ready)

**Mô tả:** Quản lý danh mục chung, phòng ban, chức vụ, quy trình, auth/session nội bộ, RBAC.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Danh mục chung (Universal) | ✅ Done | V0.05+ | Vietnamese labels, encoding fix V0.121 |
| Quản lý phòng ban | ✅ Done | V0.10+ | Metronic-style table, orange gradient header |
| Quản lý chức vụ | ✅ Done | V0.10+ | DB-backed |
| Quy trình (Workflow Engine) | ✅ Done | V0.205+ | dm_quy_trinh migration, async fix V0.206 |
| Auth/Session nội bộ | ✅ Done | V0.171+ | Fail-closed, expired warning V0.172, M0 owns auth (V0.174) |
| Security RBAC | ✅ Done | V0.173+ | User session, multi-session, inactivity timeout |
| Dashboard tổng quan | ✅ Done | V3.33.1 | Premium redesign, module progress cards |
| Form mẫu | ✅ Done | — | Form template system |
| Form phát hành | ✅ Done | — | Document issuance forms |

### Sub-routes:
- `/m0/dm-nhom-universal` — Danh mục nhóm universal
- `/m0/form-mau` — Quản lý form mẫu
- `/m0/form-phat-hanh` — Form phát hành
- `/m0/phong-ban` — Quản lý phòng ban
- `/m0/quy-trinh` — Workflow engine
- `/m0/security` — RBAC & Security settings

### Ghi chú:
- M0 là SSOT cho auth/session. M9 (Cổng Thông Tin) sẽ kế thừa auth pattern từ M0.
- Sidebar grouping Nhân Sự (M6) nằm trong M0 shell (V0.176–V0.178).
- Security session store với multi-session, revoke, legacy backfill (V0.173).

---

## M1 — Danh Mục (✅ Ready)

**Mô tả:** Khách hàng, sản phẩm, vật tư, nhân sự, vị trí, bảng giá công đoạn, đơn vị tính.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Khách hàng (dm_khach_hang) | ✅ Done | V0.30 | **MAJOR** migrate in-memory → MySQL. Wizard multi-step, detail panel |
| Sản phẩm (dm_san_pham) | ✅ Done | V0.39+ | Table redesign V2, List/Grid toggle, HARD ENFORCEMENT V0.21 |
| Vật tư (dm_vat_tu) | ✅ Done | V0.20+ | DB-backed, normalizeTenVatTu |
| Nhân sự (dm_nhan_su) | ✅ Done | V0.204 | Metronic UI redesign, 4 stats pills |
| Vị trí (dm_vi_tri) | ✅ Done | V0.209 | Metronic UI consistency, 18-point forensic diff |
| Bảng giá công đoạn (M1.4) | ✅ Done | V0.95–V0.99 | TierBuilder, UOM SSOT validation, 2-key resolve |
| Công đoạn | ✅ Done | — | Process/stage management |
| Material Item | ✅ Done | — | Bill of materials items |
| UOM (Đơn vị tính) | ✅ Done | — | Unit of measure management |
| UOM Conversion | ✅ Done | — | Unit conversion rules |
| SearchableSelect (không dấu) | ✅ Done | V0.33+ | normalizeText SSOT pattern |
| Title Auto Case | ✅ Done | V0.209+ | toVietnameseTitleCase, formatVietHoaDauChu |

### Sub-routes:
- `/m1/khach-hang` — Khách hàng (wizard 4 steps, sub-tables)
- `/m1/san-pham` — Sản phẩm (SSOT chain, combo from blueprint)
- `/m1/vat-tu` — Vật tư
- `/m1/nhan-su` — Nhân sự (Metronic redesign V0.204)
- `/m1/vi-tri` — Vị trí (Metronic consistency V0.209)
- `/m1/cong-doan` — Công đoạn sản xuất
- `/m1/material-item` — Danh mục vật liệu
- `/m1/bang-gia-cong-doan` — Bảng giá công đoạn
- `/m1/uom` — Đơn vị tính
- `/m1/uom-conversion` — Quy đổi đơn vị

---

## M3 — CRM & Bán Hàng (🔨 In Dev)

**Mô tả:** Báo giá, đơn hàng, CRM, tính giá tự động/thủ công, pricing admin.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Báo giá (Quotation) | ✅ Done | V0.154+ | Foundation pilot, Metronic table V0.168, workflow V0.206 |
| Đơn hàng (Order) | ✅ Done | V0.109+ | Card tổng quan, radian styling, status workflow |
| CRM / Nhật ký | ✅ Done | V0.118–V0.120 | Tách Việc Cần Làm / Nhật Ký 2 tab, lọc theo KH, searchbox |
| Tính giá thủ công | ✅ Done | V0.50–V0.60 | SSOT enforcement, HARD BLOCK missing cach_tinh |
| Tính giá tự động | ✅ Done | V0.95–V0.99 | Tier pricing, Blueprint combo gate |
| Pricing Admin UI | ✅ Done | V0.60–V0.77 | Blueprint/Formula/Addon/Param dialogs, Việt hóa 100% |
| Thiết kế | ✅ Done | — | Design/specification module |

### Sub-routes:
- `/m3/bao-gia` — Quản lý báo giá
- `/m3/don-hang` — Quản lý đơn hàng
- `/m3/crm` — CRM & nhật ký bán hàng
- `/m3/tinh-gia-manual` — Tính giá thủ công
- `/m3/tinh-gia-admin` — Pricing admin (Blueprint, Formula, Addon)
- `/m3/tinh-gia-production` — Tính giá sản xuất
- `/m3/thiet-ke` — Thiết kế sản phẩm

### Đang phát triển:
- Đơn hàng → LSX auto-generate flow
- Print document (Báo giá / Đơn hàng)
- Advanced pricing scenarios

---

## M4 — Sản Xuất (🔨 In Dev)

**Mô tả:** Lệnh sản xuất, phiếu điều in, gộp/tách lệnh.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Lệnh sản xuất (LSX) | ✅ Done | V0.104+ | Tạo từ Đơn hàng, status workflow |
| Phiếu điều in (PDI) | ✅ Done | V0.106+ | Tạo từ LSX, workflow actions |
| Gộp/Tách Lệnh SX | ✅ Done | V0.107 | Merge/split operations |
| M4 Overview | ✅ Done | V0.105 | Shared metadata selectors |
| Phase 1 E2E | ✅ Done | V0.110 | End-to-end verification |

### Sub-routes:
- `/m4/lenh-san-xuat` — Quản lý lệnh sản xuất
- `/m4/phieu-dieu-in` — Phiếu điều in

---

## M5 — Kho Hàng (📋 Phase 1 Skeleton)

**Mô tả:** Nhà cung cấp, mua hàng, nhập/xuất kho, kiểm kê, giao hàng.

| Chức năng | Status | Ghi chú |
|-----------|--------|---------|
| Overview page | ✅ Done | V0.122, metadata selectors V0.200 |
| Skeleton layout | ✅ Done | V0.128 |
| Core features | 📋 Planned | Chờ implement |

### Sub-routes (skeleton đã tạo):
- `/m5/nha-cung-cap` — Nhà cung cấp
- `/m5/ncc-vat-tu` — NCC vật tư
- `/m5/gia-vat-tu` — Giá vật tư
- `/m5/mua-hang` — Mua hàng
- `/m5/phieu-nhap` — Phiếu nhập kho
- `/m5/phieu-xuat-kho` — Phiếu xuất kho
- `/m5/kho-thanh-pham` — Kho thành phẩm
- `/m5/kho-giao-dich` — Kho giao dịch
- `/m5/kiem-ke-kho` — Kiểm kê kho
- `/m5/giao-hang` — Giao hàng

---

## M6 — Vận Hành HR (🔨 In Dev)

**Mô tả:** Chấm công, nghỉ phép, biến động nhân sự, vận hành HR.

| Chức năng | Status | Ghi chú |
|-----------|--------|---------|
| HR sidebar grouping | ✅ Done | V0.176–V0.178 (compact grouping) |
| M6 client module | 🔨 In Dev | 81KB client component, 21KB actions |
| UI redesign 3-layer | ✅ Done | Module Identity, Workspace Tabs, Operational Content |
| Core HR operations | 🔨 In Dev | Kế thừa dm_nhan_su từ M1 |

### Cấu trúc:
- `m6-client.tsx` (81KB) — Client component chính
- `actions.ts` (21KB) — Server actions

---

## M7 — Tiền Lương (🔨 In Dev)

**Mô tả:** Payroll, BHXH, TNCN lũy tiến, phiếu chi lương.

| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Payroll engine | ✅ Done | V0.140 | Production hardening, readiness gate |
| Phiếu chi lương | ✅ Done | V0.140 | Persisted payroll vouchers, actor audit |
| TNCN lũy tiến | ✅ Done | V0.140 | Tối đa 35%, freeze contract |
| BHXH | 🔨 In Dev | — | |
| Smoke test | ✅ Done | V0.142 | npm run verify:m7 |
| E2E workflow | ✅ Done | — | npm run verify:m7:e2e |

### Cấu trúc:
- `m7-client.tsx` (25KB) — Client component
- `actions.ts` (7KB) — Server actions

---

## M8 — Công Việc & Quy Trình (🔨 In Dev)

**Mô tả:** Task management, tích hợp CRM.

| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Task list UI | ✅ Done | V0.108 | Task management |
| M3-M8 Integration | ✅ Done | V0.102+ | CRM ↔ Task linking |
| Task sub-route | ✅ Done | — | /m8/tasks |

### Sub-routes:
- `/m8/tasks` — Quản lý công việc

---

## M9 — Cổng Thông Tin (📋 Planned)

**Mô tả:** Customer portal, multi-session.

| Chức năng | Status | Ghi chú |
|-----------|--------|---------|
| Auth kế thừa M0 | 📋 Planned | V0.174 clarified ownership |
| Customer portal | 📋 Planned | Placeholder page only |

### Cấu trúc:
- `page.tsx` (1KB) — Placeholder overview

---

## MC — Marketing/Content (🔨 In Dev)

**Mô tả:** Quản lý hợp đồng, content marketing.

| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Contract management | 🔨 In Dev | V0.305+ | mc_contract_management migration |
| MC client module | 🔨 In Dev | — | 31KB client component |

### Cấu trúc:
- `mc-client.tsx` (31KB) — Client component
- `actions.ts` (7KB) — Server actions

---

## MF — Tài Chính (🔨 In Dev)

**Mô tả:** Phiếu thu/chi, công nợ, ngân hàng, đối chiếu, nghiệm thu.

| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Overview page | ✅ Done | V0.123 | |
| Phiếu thu (Receipt) | ✅ Done | V0.141 | DB-backed, actor audit |
| Phiếu chi (Payment) | ✅ Done | V0.141 | Persisted, payroll linkage |
| Công nợ KH/NCC | ✅ Done | V0.141 | DB-backed, đối tượng/audit |
| Smoke test automation | ✅ Done | V0.142 | npm run verify:mf |
| Ngân hàng | 🔨 In Dev | — | |
| Đối chiếu công nợ | ✅ Done | V0.230 | V3.44 Option C, aging buckets, workflow draft→da_gui→da_xac_nhan |
| Nghiệm thu | ✅ Done | V0.230 | V3.44 Option C, PGH aggregation, VAT, phieu_thu linkage |

### Sub-routes:
- `/mf/phieu-thu` — Phiếu thu
- `/mf/phieu-chi` — Phiếu chi
- `/mf/cong-no` — Công nợ
- `/mf/ngan-hang` — Ngân hàng
- `/mf/doi-chieu` — Đối chiếu công nợ (V3.44)
- `/mf/nghiem-thu` — Biên bản nghiệm thu (V3.44, NEW)

---

## Tổng Kết Codebase (Audit 14/06/2026)

| Metric | Giá trị |
|--------|---------|
| **Tổng lib files** | 75 files (stores, helpers, engines, schemas) |
| **Store files** | 12 files (m0-m1-m3-m5-m6-m7-m8-mc-mf stores) |
| **Pricing engines** | 5 files (manual, offset, new, formula resolver, unified adapter) |
| **Foundation components** | PageCanvas, PageHeader, StatCard, FilterBar, SectionPanel |
| **Layout components** | MainLayout, Sidebar, Topbar, AppShellProvider, ShellSettingsPanel |
| **UI component groups** | ui, ux, generic, keenicons, wizard, workflow, pricing, m3 |
| **API routes** | /api/security/me, /api/mf/stats, + module-specific |
| **SSE endpoint** | /sse — Server-Sent Events |
