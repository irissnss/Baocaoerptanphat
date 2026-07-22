# 📊 Báo Cáo ERP Tân Phát — Tổng Quan & Changelog

> **Repo công khai** để Notion và các công cụ AI có thể đọc, hỗ trợ Owner quản lý dự án ERP Tân Phát.
>
> ⚠️ **CHỈ chứa báo cáo** — KHÔNG chứa source code, schema, credentials, hay dữ liệu nhạy cảm.

---

## 📌 Thông Tin Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Tên dự án** | ERP Tân Phát (Tân Phát Packaging) |
| **Version hiện tại** | `V0.285` |
| **Tổng cập nhật** | 285+ lần |
| **Ngày bắt đầu** | 18/01/2026 |
| **Cập nhật lần cuối** | 22/07/2026 (PL4 Phase 1 — HR-Org-RBAC) |
| **Tech Stack** | Next.js 16.1.6 · React 19.2.4 · Tailwind 4.2.1 · TypeScript 5.9.3 · MySQL |
| **Architecture** | Server Actions + Server Components + SSE |
| **UI Framework** | Metronic (Demo 1 backbone) |
| **Tổng modules** | 11 modules (M0–M9, MC, MF) |
| **Tổng bảng DB** | 98 bảng (đã kiểm đếm live 22/07/2026) |
| **Trạng thái** | Production đã go-live (chi tiết hạ tầng không công khai) |

---

## 📋 Trạng Thái Modules (cập nhật 22/07/2026)

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
| MC | Quản Lý Hợp Đồng | 🔨 In Dev | Quản lý hợp đồng (mẫu, chi tiết, workflow trạng thái) | 1 client module |
| MF | Tài Chính | 🔨 In Dev | Phiếu thu/chi, công nợ, ngân hàng, đối chiếu, nghiệm thu | 7 sub-routes |

### Tổng kết trạng thái:
- **✅ Ready (production):** 2 modules (M0, M1)
- **🔨 In Dev (đang phát triển):** 7 modules (M3, M4, M6, M7, M8, MC, MF)
- **📋 Planned/Phase 1:** 2 modules (M5, M9)

> 📂 Xem chi tiết tiến độ từng module tại [MODULE-PROGRESS.md](MODULE-PROGRESS.md)
>
> 🏆 **LATEST (V0.285 · 22/07/2026):** PL4 Phase 1 — Nền tảng Nhân sự & Phân quyền (HR-Org-RBAC): liên kết Nhân viên ↔ Tài khoản ↔ Vai trò theo mô hình phân tách trách nhiệm; nghỉ việc thay cho xóa cứng. Chi tiết ở mục Changelog bên dưới.
>
> 🗂️ *Các mục 🏆/🔐/✅ bên dưới là báo cáo LỊCH SỬ (V0.218–V0.227) — lưu tham khảo, không phải bản mới nhất.*
>
> 🔐 [REAL-USER-PILOT-V0227-BATCH1-CEO.md](REAL-USER-PILOT-V0227-BATCH1-CEO.md) — Batch 1 CEO applied + smoke test
>
> ✅ [REAL-USER-PILOT-V0227-BATCH0-VERIFY.md](REAL-USER-PILOT-V0227-BATCH0-VERIFY.md) — Batch 0 admin verify PASS
>
> 🛡️ [REAL-USER-PILOT-V0226B-FINAL-GUARD.md](REAL-USER-PILOT-V0226B-FINAL-GUARD.md) — Final guard: batch-scoped rollback
>
> 🛡️ [REAL-USER-PILOT-V0226A-APPLY-HARDENING.md](REAL-USER-PILOT-V0226A-APPLY-HARDENING.md) — Apply plan hardened: rollback safe, password flow safe
>
> 📦 [REAL-USER-PILOT-V0226-APPLY-PLAN.md](REAL-USER-PILOT-V0226-APPLY-PLAN.md) — V0.226 Safe Apply Plan (5 batches, hardened V0.226B)
>
> 📋 [REAL-USER-PILOT-UNIQUE-EMPLOYEE-MASTER-V0225E.md](REAL-USER-PILOT-UNIQUE-EMPLOYEE-MASTER-V0225E.md) — 10 unique employees, shared email blocked
>
> 🔒 [RBAC-V0223-EARLY-STAFF-PILOT-GATE.md](RBAC-V0223-EARLY-STAFF-PILOT-GATE.md) — Early Staff Pilot Gate: 135/303 guarded, 10/42 files full
>
> 🔐 [RBAC-P1-GAP-CLOSEOUT-20260627.md](RBAC-P1-GAP-CLOSEOUT-20260627.md) — P1 Gap Closeout: MF/M6/M7 guarded
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

### V0.281–V0.285 (22/07/2026) — PL4 Phase 1: Nền Tảng Nhân Sự & Phân Quyền (HR-Org-RBAC)
- **[M1 Nhân sự]** Hoàn thiện quản lý nhân sự để phân quyền: liên kết Nhân viên ↔ Tài khoản đăng nhập ↔ Vai trò, theo mô hình **phân tách trách nhiệm** (HR quản hồ sơ + tài khoản; Bảo mật M0 quản vai trò + mật khẩu + kích hoạt).
- **[M0 Bảo mật]** Kích hoạt tài khoản có kiểm soát (chỉ Admin, bắt buộc đặt mật khẩu trước khi kích hoạt); giữ nguyên ranh giới gán vai trò admin-only + bảo vệ admin cuối.
- **[M1 Nhân sự]** Vòng đời **nghỉ việc thay cho xóa cứng** (giữ lịch sử nhân sự); offboarding: khóa tài khoản + thu hồi phiên đăng nhập (Admin).
- **[M0/M1]** Phân quyền **theo từng chức năng** (Phòng ban / Vị trí / Nhân sự riêng) thay vì gộp module; ghi vết người thao tác từ phiên đăng nhập (bỏ hardcode).
- **[UI]** Trang Nhân sự thêm khu "Tài Khoản Đăng Nhập" (liên kết/tạo/hủy, vai trò chỉ-xem) + nút "Chuyển Nghỉ Việc".
- **[Scope]** Logic + Data (seed quyền, không đổi cấu trúc DB) + UI · nhánh local `fix/pl4-hr-rbac-linkage`, **chưa deploy/merge** · kiểm thử typecheck & lint sạch, kiểm thử cơ chế đạt.

### V0.271–V0.280 (21–22/07/2026) — Đồng Bộ UI 4 Trang + Chuẩn Hóa Tailwind v4
- **[M0/M1/M5]** Thống nhất giao diện Phòng Ban / Vị Trí / Nhân Sự / Kho Thành Phẩm về cùng 1 chuẩn (detail panel, sắc độ, khoảng cách, bo góc).
- **[Toàn hệ]** Quét chuẩn hóa class Tailwind v4 canonical (CSS tương đương 100%, không đổi giao diện) + nâng cấp skill hỗ trợ triển khai.
- **[Scope]** UI + Tooling.

### V0.250 (18/07/2026) — Demo UI Visual Audit v1.1.0
- **[M1]** Button text đầy đủ 'Thêm Vị Trí' (không viết tắt), sizing chuẩn Metronic h-34px
- **[M1]** Table header border-b-2 + font-semibold (Metronic v9.5)
- **[M1]** Badge cấp bậc color-coded: 8 cấp bậc → 8 màu phân biệt
- **[M1]** Action icons hover feedback (text color cam/đỏ)
- **[M1]** Null/empty → 'chưa gán' italic thay vì '-'
- **[M1]** Table card wrapper thêm shadow
- **[Infra]** Charset utf8mb4 đã confirm OK ở tầng kết nối DB
- **[Scope]** UI only — 7/12 visual bugs fixed (5 đã OK từ v1.0.0)

### V0.249 (18/07/2026) — Demo UI Standardization Phase 1
- **[Shared]** Tạo hệ thống design tokens CSS (Metronic v9.5 adapted + TanPhat brand colors)
- **[Shared]** Tạo auto-case utility (autoCase, displayName, displayStatus, displayLabel) cho Vietnamese Title Case chuẩn
- **[Shared]** Tạo status-styles utility (centralized 16-status badge system, 6 nhóm màu)
- **[M1]** Fix responsive bug P0: FormRow grid luôn 2 cột → 1 cột mobile, 2 cột desktop
- **[M1]** Upgrade StandardDataTable: thêm pagination (ellipsis), responsive columns, loading skeleton, empty state, row click
- **[M1]** Refactor trang Vị Trí Công Việc dùng shared components + design tokens + autoCase
- **[Scope]** UI only — không thay đổi business logic, API, database

### V0.248 (18/07/2026) — Control Audit + Worktree Preserve Checkpoint
- **[Audit]** Kiểm toán kỹ thuật read-only toàn hệ thống: xác minh baseline, đối chiếu Notion ↔ Source ↔ DB metadata
- **[RBAC]** Xác nhận RBAC guards đã gắn trên 200+ Server Actions (M0/M1/M3/M5/M8/MC/MF)
- **[Security]** Phát hiện 5 findings ưu tiên cao (transaction safety, persistence, audit trail) — chưa fix, chờ Owner duyệt
- **[Git]** Bảo toàn 37 file uncommitted thành 4 checkpoint commits, push backup lên origin
- **[Git]** Xác minh Draft PR #1 vẫn Open trên GitHub, chưa merge
- **[Git]** Sync main local = origin/main (V0.239)
- **[Scope]** Read-only audit + checkpoint preserve — Không thay đổi logic code, schema, Notion, hay deploy

### V0.242 (18/07/2026) — Kho Thành Phẩm Premium UI V2.1
- **[M5/KTP]** Redesign giao diện Kho Thành Phẩm theo premium style, header cam gradient matching M1 Khách Hàng
- **[M5/KTP]** Stat cards với colored top-border accent, table header orange gradient, detail panel với progress bar
- **[Scope]** UI only — Không thay đổi schema/logic/RBAC

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
- **Hạ tầng:** Production đã go-live (chi tiết máy chủ / domain / cấu hình **KHÔNG công khai** — theo Rule 14)
- **Git flow**: Local → GitHub (private) → Production deploy (quy trình có kiểm soát)

---

## 📊 Thống Kê Tổng Hợp (cập nhật 22/07/2026)

| Metric | Giá trị |
|--------|---------|
| **Tổng version updates** | 285+ |
| **Thời gian phát triển** | 18/01/2026 → 22/07/2026 (~6 tháng) |
| **Modules hoạt động** | M0, M1, M3, M4, M6, M7, M8, MC, MF (9/11) |
| **Modules planned / skeleton** | M5, M9 (2/11) |
| **Tổng bảng DB** | 98 bảng (kiểm đếm live 22/07/2026) |
| **Skills hỗ trợ** | 60+ |
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

## 📋 Ghi Chú Trạng Thái (22/07/2026)

- **Mới nhất:** PL4 Phase 1 (HR-Org-RBAC) hoàn tất ở nhánh local — **chưa deploy/merge**, đang chờ Owner review.
- **Kiểm thử:** typecheck & lint sạch; kiểm thử cơ chế đạt; runtime phân quyền theo vai trò thật còn chờ (thiếu test identities).
- **Môi trường dev:** MySQL local (Laragon) cần bật khi phát triển.

---

> **Cập nhật lần cuối:** 22/07/2026 — V0.285 (PL4 Phase 1: HR-Org-RBAC)
