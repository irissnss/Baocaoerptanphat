# 📊 Báo Cáo ERP Tân Phát — Tổng Quan & Changelog

> **Repo công khai** để Notion và các công cụ AI có thể đọc, hỗ trợ Owner quản lý dự án ERP Tân Phát.
>
> ⚠️ **CHỈ chứa báo cáo** — KHÔNG chứa source code, schema, credentials, hay dữ liệu nhạy cảm.

---

## 📌 Thông Tin Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Tên dự án** | ERP Tân Phát (Tân Phát Packaging) |
| **Version hiện tại** | `V0.223` |
| **Tổng cập nhật** | 223+ lần |
| **Ngày bắt đầu** | 18/01/2026 |
| **Cập nhật lần cuối** | 27/06/2026 (Early Staff Pilot Gate — 10/42 files fully guarded, 44.6%) |
| **Tech Stack** | Next.js 16.1.6 · React 19.2.4 · Tailwind 4.2.1 · TypeScript 5.9.3 · MySQL |
| **Architecture** | Server Actions + Server Components + SSE |
| **UI Framework** | Metronic (Demo 1 backbone) |
| **Tổng modules** | 11 modules (M0–M9, MC, MF) |
| **Tổng bảng DB** | 90+ bảng |
| **Tổng DB migrations** | 50 files |
| **Domain production** | `erp.intanphat.com` (HTTPS + nginx + HSTS) |

---

## 📋 Trạng Thái Modules (Audit 14/06/2026)

| Module | Tên | Status | Mô tả chức năng | Sub-routes |
|--------|-----|--------|------------------|------------|
| M0 | Hệ Thống | ✅ Ready | Danh mục chung, phòng ban, quy trình, auth/session, RBAC | 6 sub-routes |
| M1 | Danh Mục | ✅ Ready | Khách hàng, sản phẩm, vật tư, nhân sự, vị trí, bảng giá công đoạn | 10 sub-routes |
| M3 | CRM & Bán Hàng | 🔨 In Dev | Báo giá, đơn hàng, CRM, tính giá thủ công/tự động, pricing admin | 7 sub-routes |
| M4 | Sản Xuất | 🔨 In Dev | Lệnh sản xuất, phiếu điều in, gộp/tách LSX | 2 sub-routes |
| M5 | Kho Hàng | 📋 Phase 1 | NCC, mua hàng, nhập/xuất kho, kiểm kê, giao hàng | 10 sub-routes (skeleton) |
| M6 | Vận Hành HR | 🔨 In Dev | Chấm công, nghỉ phép, biến động nhân sự | 1 client module |
| M7 | Tiền Lương | 🔨 In Dev | Payroll, BHXH, TNCN lũy tiến, phiếu chi lương | 1 client module |
| M8 | Công Việc | 🔨 In Dev | Task management, tích hợp CRM M3-M8 | 1 sub-route (tasks) |
| M9 | Cổng Thông Tin | 📋 Planned | Customer portal, multi-session (kế thừa auth M0) | Placeholder |
| MC | Marketing/Content | 🔨 In Dev | Quản lý hợp đồng, content marketing | 1 client module |
| MF | Tài Chính | 🔨 In Dev | Phiếu thu/chi, công nợ, ngân hàng, đối chiếu | 5 sub-routes |

### Tổng kết trạng thái:
- **✅ Ready (production):** 2 modules (M0, M1)
- **🔨 In Dev (đang phát triển):** 7 modules (M3, M4, M6, M7, M8, MC, MF)
- **📋 Planned/Phase 1:** 2 modules (M5, M9)

> 📂 Xem chi tiết tiến độ từng module tại [MODULE-PROGRESS.md](MODULE-PROGRESS.md)
>
> 🏆 **LATEST:** [RBAC-V0223-EARLY-STAFF-PILOT-GATE.md](RBAC-V0223-EARLY-STAFF-PILOT-GATE.md) — Early Staff Pilot Gate: 135/303 guarded, 10/42 files full
>
> 🔒 [RBAC-P1-GAP-CLOSEOUT-20260627.md](RBAC-P1-GAP-CLOSEOUT-20260627.md) — P1 Gap Closeout: 57 functions guarded (MF/M6/M7)
>
> 📋 [P01-RECONCILIATION-CLOSEOUT-V0218.md](P01-RECONCILIATION-CLOSEOUT-V0218.md) — Timeline + Employee Safety + Final Decision
>
> ✅ [P01-FINAL-SMOKE-TEST-V0218.md](P01-FINAL-SMOKE-TEST-V0218.md) — 15/15 PASS Browser + API + DB
>
> 🔧 [P01-FLEXIBLE-FOUNDATION-V0218.md](P01-FLEXIBLE-FOUNDATION-V0218.md) — 29 Guards, Architecture
>
> 🔒 [P01-SAFETY-VERIFICATION-V0218.md](P01-SAFETY-VERIFICATION-V0218.md) — Safety Report
>
> 📋 [GOLIVE-PLAN.md](GOLIVE-PLAN.md) — Kế hoạch Go-Live tổng quan

---

### 🚀 Go-Live P0.1 — A*) READY FOR INTERNAL MINI PILOT (15/06/2026)

**Mục tiêu:** Phân quyền RBAC cho NV Kinh doanh & Thiết kế

| Phase | Nội dung | Status |
|-------|----------|--------|
| P0.1-1 | Roles + Menu + Action permissions | ✅ Done (7 roles, 38 menu, 31 action) |
| P0.1-2 | Page guards (14/14) | ✅ Verified |
| P0.1-3 | Customer ownership + cost masking | ✅ Done + Browser verified |
| P0.1-4 | LSX/PDI state guards (THIET_KE) | ✅ Done |
| P0.1-5 | Accounting packs (KE_TOAN) | ✅ Done (3 packs) |
| P0.1-6 | Smoke test 15/15 | ✅ PASS |
| P0.1-7 | Production deploy | ❌ NOT APPROVED |

> **Decision: A\*) INTERNAL MINI PILOT — TEST USERS ONLY**
> Page guard: 14/14 ✅ (không missing). Role canonical: SALES. THIET_KE đã tạo.
> Employee data: SAFE (original was dummy). Test users: 17, local only.

---

## 🏗️ Kiến Trúc Hệ Thống

### Stack chính:
| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | Next.js | 16.1.6 |
| UI Library | React | 19.2.4 |
| CSS | Tailwind CSS | 4.2.1 |
| Language | TypeScript | 5.9.3 |
| Database | MySQL | 8.x |
| Form | React Hook Form + Zod 4 | Latest |
| UI Components | Radix UI + Shadcn | Multiple |
| Icons | Lucide React | 0.462.0 |
| Template | Metronic (Demo 1) | Licensed |

### Architecture patterns:
- **Server Actions + Server Components** (không tách FE/BE theo REST)
- **SSE** (Server-Sent Events) cho real-time
- **Foundation Components**: PageCanvas, PageHeader, StatCard, FilterBar, SectionPanel
- **Shell System**: AppShellProvider với presets (Owner Review, Operator, Focus Entry)
- **Auth**: Fail-closed session, expired warning, cookie-based

### Deployment:
- **VPS**: 103.x.x.x (Ubuntu + nginx + PM2 + standalone runtime)
- **Domain**: `erp.intanphat.com` (HTTPS + HSTS)
- **Git flow**: Local → GitHub (private) → VPS deploy via scripts
- **CI/CD Scripts**: deploy-vps-via-git.ps1, sync-final.ps1, release-final.ps1

---

## 📊 Thống Kê Tổng Hợp (Audit 14/06/2026)

| Metric | Giá trị |
|--------|---------|
| **Tổng version updates** | 216+ |
| **Thời gian phát triển** | 18/01/2026 → 14/06/2026 (~5 tháng) |
| **Modules hoạt động** | M0, M1, M3, M4, M6, M7, M8, MC, MF (9/11) |
| **Modules planned** | M5, M9 (2/11) |
| **App routes** | 19 top-level directories trong `/src/app` |
| **Library files (src/lib)** | 75 files (71 .ts + 4 subdirs) |
| **Components** | 14 component groups (10 subdirs + 4 standalone) |
| **DB migrations** | 50 files |
| **Skills tạo** | 60+ |
| **Governance files** | 5 files (5-way sync, SHA256 verified) |
| **Deploy iterations** | 20+ |
| **npm scripts** | 53 scripts (dev, build, deploy, verify, migrate, check) |
| **Node.js** | v24.14.1 (engines: >=20 <25) |

---

## 📂 Cấu Trúc Báo Cáo

| File | Nội dung |
|------|----------|
| **README.md** | Tổng quan dự án, modules, kiến trúc, thống kê (file này) |
| **[CHANGELOG-DETAIL.md](CHANGELOG-DETAIL.md)** | Changelog chi tiết V0.216 → V0.100 (gần đây) |
| **[CHANGELOG-ARCHIVE.md](CHANGELOG-ARCHIVE.md)** | Changelog archive V0.99 → V0.00 (cũ hơn) |
| **[MODULE-PROGRESS.md](MODULE-PROGRESS.md)** | Tiến độ chi tiết từng module + chức năng |
| **[GOVERNANCE-LOG.md](GOVERNANCE-LOG.md)** | Lịch sử governance, skills, architecture decisions |

---

## 🔗 Liên Kết

- **Main repo (private):** `irissnss/erptanphat`
- **Báo cáo này (public):** [`irissnss/Baocaoerptanphat`](https://github.com/irissnss/Baocaoerptanphat)
- **Notion workspace:** Synced qua Notion MCP

---

## 📋 Audit Notes (14/06/2026)

### VPS Health Check:
- ✅ `erp.intanphat.com` → HTTP 307 → `/auth/login` (healthy)
- ✅ nginx + HSTS enabled
- ✅ Login page render đầy đủ (RSC payload, branding, form)
- ⚠️ VPS đang chạy V0.215, local có V0.216 (governance-only change, chưa deploy)

### Pending:
- 6 files governance chưa commit (V0.216 = chỉ thêm rule 14 PUBLIC REPORT SYNC)
- Local MySQL (Laragon) cần bật khi dev

---

> **Cập nhật lần cuối:** 14/06/2026 — Audit toàn hệ thống V0.216
