# 📊 Báo Cáo ERP Tân Phát — Changelog & Version History

> **Repo công khai** để Notion và các công cụ AI có thể đọc lịch sử phát triển dự án ERP Tân Phát.
>
> **Version hiện tại:** `V0.215` | **Tổng cập nhật:** 215+ lần | **Ngày bắt đầu:** 18/01/2026

---

## 🏗️ Tổng Quan Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Tên dự án** | ERP Tân Phát |
| **Tech Stack** | Next.js 16 + React 19 + Tailwind 4 + MySQL |
| **Architecture** | Server Actions + Server Components + SSE |
| **UI Framework** | Metronic (Demo 1 backbone) |
| **Số modules** | 10 modules (M0–M9, MF) |
| **Số bảng DB** | 90+ bảng |
| **Version format** | V{MAJOR}.{MINOR} (MINOR 0–99 → reset MAJOR+1) |

---

## 📋 Modules

| Module | Tên | Trạng thái | Mô tả |
|--------|-----|------------|-------|
| M0 | Hệ Thống | ✅ Ready | Quản lý danh mục chung, phòng ban, quy trình |
| M1 | Danh Mục | ✅ Ready | Khách hàng, sản phẩm, vật tư, bảng giá |
| M3 | CRM & Bán Hàng | 🔨 In Dev | Báo giá, đơn hàng, CRM, tính giá |
| M4 | Sản Xuất | 🔨 In Dev | Lệnh sản xuất, phiếu điều in |
| M5 | Kho Hàng | 📋 Planned | Nhà cung cấp, mua hàng, giao hàng |
| M6 | Nhân Sự | 📋 Planned | Vận hành HR |
| M7 | Tiền Lương | 🔨 In Dev | Payroll, BHXH, TNCN |
| M8 | Công Việc & Quy Trình | 🔨 In Dev | Task management |
| M9 | Cổng Thông Tin | 📋 Planned | Customer portal |
| MF | Tài Chính | 🔨 In Dev | Phiếu thu/chi, công nợ |

---

## 📜 Changelog Chi Tiết

### V0.215 (04/05/2026)
- Code update: docs/bg-login.png, docs/demo-v6.css, docs/login-demo.html, docs/ui-demo-v6.html, next-env.d.ts, +8 more

### V0.214 (04/05/2026)
- UI mobile footer compact + module grid responsive + hover effects

### V0.213 (30/04/2026)
- Code update: public/login-illustration.png, src/app/auth/login/login-client.tsx, src/app/auth/login/page.tsx

### V0.212 (30/04/2026) — Login UI Finalization: Full-illustration + Floating Card
- **[Auth/Login]** Layout 2 phần: Top bar (logo + "Tân Phát ERP" + heading + subtitle) + Card login floating bên phải trên nền full ảnh minh họa
- **[Auth/Login]** Desktop: Logo + branding text góc trên trái, card login bên phải sát top, ảnh illustration full viewport
- **[Auth/Login]** Mobile: Logo compact + subtitle trên cùng, card login ngay dưới, responsive padding/gap/font
- **[Auth/Login]** Card: bg-white/95 backdrop-blur, rounded-2xl mobile / rounded-[20px] desktop, orange gradient top accent
- **[Scope]** UI-only. Không thay đổi auth logic/API/schema

### V0.211 (27/04/2026)
- Test: Verify auto version bump system

### V0.210 (17/04/2026) — Local dev: default Webpack
- **[Dev]** `npm run dev` dùng `next dev --webpack` để tránh Turbopack FATAL `os error 21` trên Windows
- **[Dev]** Thêm `npm run dev:turbo` cho Turbopack

### V0.209 (29/03/2026) — M1 Vị Trí UI consistency fix
- **[M1/VT]** Thay DataTableSimple bằng inline Metronic-style table với orange gradient header
- **[M1/VT]** Thêm 3 stats pills (Tổng/Có PB/Chung) với Lucide icons
- **[M1/VT]** Inline filter bar, sortable columns, pagination
- **[M1/VT]** Sheet form gradient orange header, sticky footer

### V0.208 (28/03/2026) — System-wide mobile viewport hardening
- **[Auth/ChangePassword]** Fix min-h-screen + flex items-center khi virtual keyboard bật
- **[Error/403/GlobalError]** Standardize min-h-screen → min-h-dvh toàn bộ standalone pages
- **[Verify]** 14 test cases: Login, ChangePassword, Desktop regression — All PASS

### V0.207 (28/03/2026) — CRITICAL: Mobile login keyboard layout fix
- **[Auth/Login]** Root cause: flex items-center + min-h-screen khiến form bị ép khi virtual keyboard bật
- **[Fix]** Mobile layout top-aligned (pt-12 + overflow-y-auto), desktop giữ lg:items-center
- **[Layout]** Thêm interactiveWidget: resizes-content và viewportFit: cover
- **[Verify]** 10 test cases trên 3 viewports — All PASS

### V0.206 (28/03/2026) — Forensic Stabilization V5: Critical async bug fixes
- **[M3/BG]** Fix import trực tiếp workflow-service từ client component
- **[M0/QT]** Fix isTerminalState async trong .filter() đồng bộ
- **[Scope]** Chỉ fix async boundary violations

### V0.205 (28/03/2026) — Forensic Stabilization V4: dm_quy_trinh migration prep

### V0.204 (19/03/2026) — M1 Nhân Sự UI redesign to Metronic pattern
- **[M1/NS]** Thay DataTableSimple bằng inline table với orange gradient header
- **[M1/NS]** 4 stats pills (Tổng/Chính Thức/Thử Việc/Nghỉ)
- **[M1/NS]** Inline filter bar, sortable columns, pagination

### V0.203 (14/03/2026) — Sản Phẩm & Khách Hàng UX fixes
- **[M1/SP]** Default perPage 25 → 10
- **[M1/KH]** Detail panel giữ trạng thái sau edit qua wizard
- **[M1/SP]** Đồng bộ styling nút List/Grid toggle

### V0.202–V0.190 (10–11/03/2026) — Metronic Backbone & Governance
- **[V0.202]** Extend Metronic addendum into module overview docs
- **[V0.201]** Sync Metronic UI governance into local and Notion SSOT
- **[V0.200]** Align M4/M5/M9 overview metadata
- **[V0.199]** Refine Metronic UI mandatory protocol wording
- **[V0.198]** Add mandatory Metronic UI rule to governance core
- **[V0.197]** Add Metronic UI research and implementation protocol
- **[V0.196]** Extract M3 highlight cards into shared metadata
- **[V0.195]** Move M0/M3 feature cards to shared metadata selectors
- **[V0.194]** Extract M1/M3/MF group cards into shared selectors
- **[V0.193]** Roll out shared metadata selectors to overview pages
- **[V0.192]** Preserve isolated B2/B3 skills in separate inventory doc
- **[V0.191]** Restore governance standard after freeze root cause isolation
- **[V0.190]** Remove duplicate 5-skill blocks after confirmed freeze

### V0.189–V0.185 (10/03/2026) — Agent Freeze Investigation
- **[V0.189]** Re-add suspicious skill blocks B2/B3 for freeze isolation
- **[V0.188]** Roll back recent AGENTS and CLAUDE sync blocks
- **[V0.187]** Disable folder-open auto task
- **[V0.186]** Sanitize local dev startup output (prevent terminal mojibake)
- **[V0.185]** Replace GEMINI.md with MODEL_GUIDE.md to avoid filename freeze

### V0.184–V0.180 (09–10/03/2026) — Governance & Skills
- **[V0.184]** Restore full governance sync (AGENTS, CLAUDE, GEMINI)
- **[V0.181]** Add hard local-safety gate for skills/rules/backup
- **[V0.180]** Reorganize overlapping versioning skills

### V0.179–V0.175 (09/03/2026) — Shell & Sidebar Redesign
- **[V0.179]** Strengthen skill governance and skill mining workflow
- **[V0.178]** Fix grouped HR routes sidebar
- **[V0.177]** Compact sidebar grouping cho Nhân Sự HR
- **[V0.176]** Flow-first HR navigation shortcuts
- **[V0.175]** Dark navy sidebar + compact MISA-style layout + remove dark mode

### V0.174–V0.170 (08–09/03/2026) — Auth & M3 Table Styling
- **[V0.174]** Clarify M0 owns current auth, M9 inherits later
- **[V0.173]** Standardize internal auth sessions in M0
- **[V0.172]** Show explicit warning when session expired
- **[V0.171]** Fail closed when stale sessions
- **[V0.170]** Align M3 table styling with Metronic demo

### V0.169–V0.155 (08/03/2026) — Metronic Foundation Rollout
- **[V0.169]** Unify border-radius + remove redundant table header
- **[V0.168]** Metronic data table implementation for M3/Báo Giá
- **[V0.167]** Reset shell surfaces, recompose M0
- **[V0.166]** Tighten homepage card rhythm
- **[V0.165]** Realign shell chrome closer to Metronic backbone
- **[V0.164]** Rename shell presets to real operating roles
- **[V0.163]** Migrate Next 16 middleware convention to proxy
- **[V0.162]** Add shell presets and overview density controls
- **[V0.161]** Add live shell settings panel with local persistence
- **[V0.160]** Roll compact backbone to M1/M4/MF
- **[V0.159]** Compact homepage shell, document Metronic backbone
- **[V0.158]** Move shell state into shared provider backbone
- **[V0.157]** Strengthen app shell backbone
- **[V0.156]** Roll out responsive foundation to home and M3
- **[V0.155]** Extract reusable foundation components from Báo Giá

### V0.154–V0.150 (07–08/03/2026) — Next 16 Foundation
- **[V0.154]** Remove old Báo Giá demo, start live M3 foundation
- **[V0.153]** Next 16 foundation gate pass and shell bridge
- **[V0.152]** Demo V2 báo giá giữ detail panel hiện tại
- **[V0.151]** Demo chuẩn hóa layout chung và model Báo Giá
- **[V0.150]** Remove noisy final deploy header warning

### V0.149–V0.142 (04–07/03/2026) — Deploy & DevOps
- **[V0.149]** Fix PowerShell escaping in deploy verification
- **[V0.148]** Harden deploy header verification
- **[V0.147]** Unblock release build from legacy lint debt
- **[V0.146]** Sync governance addendum across 5 rule files
- **[V0.145]** Sync governance files sau thay đổi AGENTS
- **[V0.144]** Release doctor không chết sớm vì dev server
- **[V0.143]** VPS deploy chạy standalone runtime thật
- **[V0.142]** MF smoke automation and topbar logo cleanup

### V0.141–V0.140 (04/03/2026) — MF & M7 Hardening
- **[V0.141]** MF persisted receipts and receivables hardening
- **[V0.140]** M7 production hardening with persisted payroll vouchers

### V0.139–V0.130 (27/02–03/03/2026) — Deploy Pipeline
- **[V0.139]** One-command sync final (GitHub → VPS → health-check)
- **[V0.138]** Fix deploy VPS fail do tailwind.config.js placeholder
- **[V0.137]** Runbook thao tác deploy/db sync
- **[V0.136]** Script dùng đúng thông số GitHub + VPS
- **[V0.135]** Dọn dự án sạch gọn, chuẩn bị push lên GitHub
- **[V0.134]** Workflow Local → GitHub → VPS
- **[V0.131]** Rà soát dung lượng repo + archive/backup
- **[V0.130]** Fix deploy treo ở bước nén + preflight

### V0.129–V0.120 (08–27/02/2026) — Feature Development
- **[V0.129]** Deploy VPS một lệnh (full setup)
- **[V0.128]** Audit Webapp Hoàn Thiện: metadata M5/M8, G2 form, backup dọn
- **[V0.127]** Trang chủ: tiến độ lấy từ module-metadata (SSOT)
- **[V0.126]** Tên module tiếng Việt toàn bộ
- **[V0.125]** M5 Kho Hàng: tên hiển thị
- **[V0.124]** MF Tài Chính: sublink sidebar
- **[V0.123]** MF: Trang tổng quan Module MF
- **[V0.122]** M5: Trang tổng quan Module M5
- **[V0.121]** Fix M0 Universal Vietnamese labels encoding
- **[V0.120]** M3 CRM: header gộp, searchbox khách

### V0.119–V0.110 (08/02/2026) — M3 & M4 Features
- **[V0.119]** M3 CRM: lọc nhật ký theo khách hàng
- **[V0.118]** M3 CRM: tách Việc Cần Làm / Nhật Ký thành 2 tab
- **[V0.117]** M3 Đơn hàng: card cam đậm + M4 LSX/PDI status workflows
- **[V0.116]** M4 PDI: Fix mojibake trong actions.ts
- **[V0.115]** M3 Đơn hàng: card tổng quan nổi bật
- **[V0.114]** M3 Đơn hàng: card style giống Báo giá
- **[V0.113]** M3 Báo giá: card nhỏ gọn, 2 dòng
- **[V0.112]** M3 Card: bo góc nhẹ + radian màu
- **[V0.111]** M3 Đơn Hàng: radian card tổng quan
- **[V0.110]** M4 Phase 1 E2E Verification

### V0.109–V0.100 (07–08/02/2026) — M3 & M4 Core
- **[V0.109]** M3 Đơn Hàng: style & section tham khảo Báo giá
- **[V0.108]** M8 UI: trực quan tổng quát
- **[V0.107]** M4: Gộp/Tách Lệnh SX
- **[V0.106]** M4: Tạo Phiếu Điều In từ LSX
- **[V0.105]** M4 Overview thay placeholder
- **[V0.104]** M4: Nút Tạo Lệnh SX từ Đơn hàng
- **[V0.103]** PLAN db-driven-labels COMPLETE
- **[V0.102]** M3-M8 Integration P1/P2

### V0.99–V0.90 (31/01–01/02/2026) — Pricing System
- **[V0.99]** Tier pricing Phase 3A+3B complete
- **[V0.98]** Phase 2 Tier Pricing (TierBuilder, Smart filter, Duplicate validation)
- **[V0.97]** M1.4 Bảng Giá Công Đoạn — Toast Fix
- **[V0.96]** M1.4 Bảng Giá — UOM Invalid Fix + Mismatch Handling
- **[V0.95]** M1.4 Bảng Giá — Option C Implementation

### V0.77–V0.60 (29–30/01/2026) — Pricing Admin UI
- **[V0.77]** Addon + Param Dialog Redesign (Vietnamese 100%)
- **[V0.76]** P0 PIXEL-LOCK to Demo HTML
- **[V0.75]** Pricing Admin Pagination
- **[V0.73]** Client-side pagination all 4 tabs
- **[V0.72]** Formula Dialog Redesign (Việt hóa 100%)
- **[V0.71]** Addon + Param Dialog V0.71
- **[V0.70]** Tinh Gia Admin Localization
- **[V0.69]** Blueprint Dialog demo-like styling
- **[V0.67]** Blueprint Dialog Layout Optimization
- **[V0.66]** Blueprint Dialog V2.2: Full Demo Match
- **[V0.65]** Blueprint Dialog Typography fixes
- **[V0.64]** Blueprint Dialog V2.0: Full Demo HTML Style
- **[V0.63]** Blueprint Dialog V1.4: Compact header + footer
- **[V0.62]** Blueprint Form: Việt hóa 100%
- **[V0.61]** Pricing Rule Popup V3 Final Redesign
- **[V0.60]** /m3/tinh-gia-manual: HARD BLOCK when cach_tinh missing

### V0.59–V0.50 (29/01/2026) — Pricing Engine
- **[V0.59]** Remove hardcoded threshold 1000
- **[V0.58]** Radix Select.Item empty value crash fix
- **[V0.57]** BLOCK SAVE when SSOT addon types not loaded
- **[V0.56]** Fallback SAVE policy (Allow + Mark NotProven)
- **[V0.55]** SSOT loai_addon: dm_addon_type table + 8 seed types
- **[V0.54]** nhom_san_pham_id type mismatch fix (string → number)
- **[V0.53]** HARD ENFORCEMENT SSOT Compliance
- **[V0.52]** Phase 3-4 UI Builders Complete
- **[V0.51]** Phase 2 RuleBuilder for cach_tinh
- **[V0.50]** Phase 1 Manual UX Enhancement (Bilingual Labels)

### V0.48–V0.40 (27–29/01/2026) — UI Builders & Schema
- **[V0.48]** UI Builders Phase 1 & 2 (ViewJsonPanel, BilingualLabel, ParamRegistryBuilder...)
- **[V0.47]** Demo Flow Carton V1 seed script
- **[V0.46]** Fix NaN display in Test Vectors
- **[V0.45]** Complete Test Vectors API endpoints
- **[V0.44]** Test Vectors UI implementation
- **[V0.43]** Test Vectors UI in FormulaDialog
- **[V0.42]** Phase 2 Audit completed
- **[V0.41]** dm_blueprint.nhom_cong_nghe_id: NULL → NOT NULL (2-key)
- **[V0.40]** Fix nhom_san_pham_id type mismatch + 4 FKs

### V0.39–V0.30 (27/01/2026) — M1 SSOT Migration
- **[V0.39]** M1.2 Sản Phẩm table redesign theo demo V2
- **[V0.38]** Khách Hàng Table UI fixes
- **[V0.37]** Fix wizard preview hiển thị label thay vì ID
- **[V0.36]** DROP COLUMN legacy fields từ kh_dia_chi, kh_lien_he, dm_khach_hang
- **[V0.35]** chuc_vu_dai_dien + dieu_kien_thanh_toan dropdown saves ID
- **[V0.34]** ADD COLUMN chuc_vu_dai_dien_id + dieu_kien_thanh_toan_id
- **[V0.33]** SearchableSelect với diacritics-insensitive search
- **[V0.32]** ADD COLUMN chuc_vu_khg_id to kh_lien_he
- **[V0.31]** ADD COLUMN nhom_dia_chi_id to kh_dia_chi
- **[V0.30]** **MAJOR:** Migrated Khách Hàng from in-memory to MySQL

### V0.29–V0.20 (26–27/01/2026) — DB Migration & SSOT
- **[V0.29]** ComboboxCreate touch scroll + DB props for nhomVatTuOptions
- **[V0.28]** M1.4 UOM SSOT validation (nhom_don_vi_tinh_id=18)
- **[V0.27]** M1.4 UI: ẩn cột ID, thêm cột Nhóm CN, 4 premium sections
- **[V0.26]** 5-WAY SYNC governance files (verified MD5 hash)
- **[V0.25]** Synced 2 new skills to all 5 governance files
- **[V0.24]** Created mysql-schema-extraction + text-first-report skills
- **[V0.23]** Created 4 new skills (phased-migration, feature-flag, ssot-docs, contract-preservation)
- **[V0.22]** Combo Gate SSOT validation + sanitize kích thước
- **[V0.21]** HARD ENFORCEMENT: Production input from dm_san_pham SSOT chain
- **[V0.20]** Migrate m1-4-bang-gia-store.ts to DB-backed (MySQL)

### V0.19–V0.05 (19–26/01/2026) — Foundation
- **[V0.19]** Fix delete dialog button text
- **[V0.18]** **MAJOR:** Disable ALL mock/demo data (M0/M1/M3)
- **[V0.17]** Auto version bump watcher + git pre-commit hook
- **[V0.16]** 3-layer version tracking + 5-way sync rule
- **[V0.15]** Demo Module Tính Giá Offset + Skills creation
- **[V0.14]** Created 3 new skills (hard-enforcement, ui-section-reorder, db-label-to-human-text)
- **[V0.13]** M3 Báo Giá: sắp xếp lại sections
- **[V0.12]** M0 Dashboard: Fix title cards
- **[V0.11]** M3 Báo Giá: sắp xếp Detail Panel, layout 2 cột
- **[V0.08]** Added 5 new skills + updated 3 existing
- **[V0.06]** Added rule 12: Versioning (MANDATORY)
- **[V0.05]** Test bump version script

### V3.33.1–V3.33.0 (18/01/2026) — Initial
- **[V3.33.1]** Dashboard Premium Redesign + Fix button type warnings
- **[V3.33.0]** Initial UI Skeleton — M0, M1 modules layout

---

## 📊 Thống Kê Tổng Hợp

| Metric | Giá trị |
|--------|---------|
| **Tổng version updates** | 215+ |
| **Thời gian phát triển** | 18/01/2026 → hiện tại |
| **Modules hoạt động** | M0, M1, M3, M4, M7, M8, MF |
| **Modules planned** | M5, M6, M9 |
| **DB migrations** | 50+ |
| **Skills tạo** | 60+ |
| **Governance syncs** | 30+ |
| **Deploy iterations** | 20+ |

---

## 🔗 Liên Kết

- **Main repo (private):** `irissnss/tanphat-erp`
- **Báo cáo này (public):** `irissnss/Baocaoerptanphat`
- **Notion workspace:** Synced qua Notion MCP

---

> **Cập nhật lần cuối:** 08/05/2026 — V0.215
>
> File này được tự động trích xuất từ `src/lib/version.ts` của dự án ERP Tân Phát.
