# 📜 Changelog Chi Tiết — ERP Tân Phát

> Ghi nhận đầy đủ mọi thay đổi theo từng version.
> Mỗi entry ghi rõ: **Module ảnh hưởng · Scope · Files · Lý do · Kết quả verify.**

---

## V0.216 (08/05/2026) — Governance: Public Report Sync Rule

| Field | Chi tiết |
|-------|----------|
| **Tiêu đề** | Thêm rule 14: PUBLIC REPORT SYNC |
| **Module** | Governance (toàn hệ thống) |
| **Scope** | Governance only — không thay đổi code/logic/schema |
| **Lý do** | Owner yêu cầu tạo repo public để Notion đọc được changelog, hỗ trợ làm việc |

**Thay đổi chi tiết:**
- Thêm section `14) PUBLIC REPORT SYNC` vào AGENTS.md — bắt buộc cập nhật báo cáo lên repo public `https://github.com/irissnss/Baocaoerptanphat` sau mỗi lần code/fix/audit/deploy
- Chốt danh sách **NGHIÊM CẤM public**: source code, DB schema, API endpoints, credentials, business logic chi tiết, dữ liệu thật, server info, governance files
- Chốt danh sách **ĐƯỢC PHÉP public**: version/changelog, module progress, thống kê tổng hợp, tech stack tổng quan
- 5-WAY SYNC đồng bộ 5 governance files — SHA256 verified MATCH
- Tạo repo public với README.md chứa changelog V0.00 → V0.215

**Files thay đổi:** AGENTS.md, .cursorrules, .antigravityrules, CLAUDE.md, GEMINI.md, version.ts

**Verify:** SHA256 hash 5 files = `47133D36...CA975` (identical)

---

## V0.215 (04/05/2026) — Code update batch

| Field | Chi tiết |
|-------|----------|
| **Module** | Docs, Auth |
| **Scope** | UI + Assets |
| **Files** | bg-login.png, demo-v6.css, login-demo.html, ui-demo-v6.html, next-env.d.ts, +8 more |

---

## V0.214 (04/05/2026) — Mobile responsive improvements

| Field | Chi tiết |
|-------|----------|
| **Module** | Layout (toàn hệ thống) |
| **Scope** | UI only |

**Thay đổi chi tiết:**
- Mobile footer compact hơn
- Module grid responsive (2 cột mobile, 4 cột desktop)
- Hover effects nổi bật hơn cho cards

---

## V0.213 (30/04/2026) — Login assets update

| Field | Chi tiết |
|-------|----------|
| **Module** | Auth/Login |
| **Scope** | UI + Assets |
| **Files** | login-illustration.png, login-client.tsx, page.tsx |

---

## V0.212 (30/04/2026) — Login UI Finalization: Full-illustration + Floating Card

| Field | Chi tiết |
|-------|----------|
| **Module** | Auth/Login |
| **Scope** | UI only — không thay đổi auth logic/API/schema |
| **Files** | page.tsx, login-client.tsx |

**Thay đổi chi tiết:**
- Layout 2 phần: Top bar (logo + "Tân Phát ERP" + heading + subtitle) + Card login floating bên phải trên nền full ảnh minh họa
- **Desktop:** Logo + branding text góc trên trái, card login bên phải sát top, ảnh illustration full viewport object-cover
- **Mobile:** Logo compact + subtitle trên cùng, card login ngay dưới (không cần scroll), responsive padding/gap/font (px-5 py-6, h-11, gap-4)
- **Card:** bg-white/95 backdrop-blur, rounded-2xl mobile / rounded-[20px] desktop, orange gradient top accent
- Login form responsive: input h-11/h-12, heading text-xl/text-2xl, gap-4/gap-5 theo breakpoint

---

## V0.211 (27/04/2026) — Verify auto version bump

| Field | Chi tiết |
|-------|----------|
| **Module** | DevOps |
| **Scope** | Test/Verify |
| **Kết quả** | Auto version bump system hoạt động đúng |

---

## V0.210 (17/04/2026) — Local dev: default Webpack

| Field | Chi tiết |
|-------|----------|
| **Module** | DevOps |
| **Scope** | Dev tooling |
| **Files** | package.json, version.ts, WORK_LOG.md |
| **Lý do** | Turbopack FATAL `os error 21` trên Windows |

**Thay đổi chi tiết:**
- `npm run dev` / `dev:3000` dùng `next dev --webpack` để tránh Turbopack FATAL
- Thêm `npm run dev:turbo` cho ai muốn thử lại Turbopack

---

## V0.209 (29/03/2026) — M1 Vị Trí UI consistency fix

| Field | Chi tiết |
|-------|----------|
| **Module** | M1/Vị Trí |
| **Scope** | UI/UX consistency — không thay đổi schema/action/logic/RBAC/route |
| **Files** | vi-tri-client.tsx (1 file) |
| **Reference** | 18-point forensic diff vs phong-ban pattern |

**Thay đổi chi tiết:**
- Thay DataTableSimple + StandardListPageHeader bằng inline Metronic-style table với orange gradient header
- Thêm 3 stats pills (Tổng/Có PB/Chung) với Lucide icons và Metronic styling
- Inline filter bar (search + sort dropdown), sortable columns (mã vị trí, tên vị trí, cấp bậc), pagination
- Action column Pencil/Trash2 icons, audit history icon, cấp bậc badges
- Sheet form gradient orange header, sticky footer, section grouping đồng bộ với phòng ban pattern
- Tích hợp normalizeText (search không dấu) và toVietnameseTitleCase (Title Auto Case) theo SSOT

---

## V0.208 (28/03/2026) — System-wide mobile viewport hardening

| Field | Chi tiết |
|-------|----------|
| **Module** | Auth, Error pages (toàn hệ thống) |
| **Scope** | UI fix — mobile viewport |
| **Files** | change-password/page.tsx, error.tsx, global-error.tsx, 403/page.tsx, version.ts |
| **Verify** | 14 test cases: Login (3 viewports + keyboard sim), ChangePassword (2 viewports + keyboard sim), Desktop regression — All PASS |

**Thay đổi chi tiết:**
- **Auth/ChangePassword:** Fix root cause giống login — min-h-screen + flex items-center + virtual keyboard → form bị ép co
- **Fix:** min-h-dvh + top-aligned mobile (pt-12) + overflow-y-auto + lg:items-center desktop
- **Error/403/GlobalError:** Standardize min-h-screen → min-h-dvh toàn bộ standalone pages
- **Forensic:** System-wide scan 11 files có min-h-screen pattern. main-layout.tsx và M3/M5 pages INTENTIONALLY NOT CHANGED

---

## V0.207 (28/03/2026) — CRITICAL: Mobile login keyboard layout fix

| Field | Chi tiết |
|-------|----------|
| **Module** | Auth/Login |
| **Scope** | UI fix — CRITICAL mobile bug |
| **Files** | page.tsx (login), layout.tsx (viewport config) |
| **Verify** | 10 test cases trên 3 viewports (iPhone 375x812, Android 360x640, Desktop 1280x800) + keyboard simulation — All PASS |

**Root cause:** flex items-center + min-h-screen khiến form bị ép giữa viewport khi virtual keyboard bật. Khi keyboard chiếm 50%+ viewport height, submit button và password field bị đẩy ra ngoài viewable area.

**Fix:**
- Chuyển mobile layout từ vertical-center sang top-aligned (pt-12 + overflow-y-auto)
- Giữ lg:items-center cho desktop
- Dùng min-h-dvh thay min-h-screen cho dynamic viewport handling
- Thêm interactiveWidget: resizes-content và viewportFit: cover vào viewport config (layout.tsx)

---

## V0.206 (28/03/2026) — Forensic Stabilization V5: Critical async bug fixes

| Field | Chi tiết |
|-------|----------|
| **Module** | M3/Báo Giá, M0/Quy Trình |
| **Scope** | Logic fix — async boundary violations |
| **Files** | bao-gia-client.tsx, actions.ts, quy-trinh-client.tsx |

**Thay đổi chi tiết:**
- **M3/BG:** Fix import trực tiếp workflow-service (DB-backed) từ client component → tạo validateBaoGiaTransitionAction server action mới
- **M0/QT:** Fix isTerminalState (async DB call) dùng trong .filter() đồng bộ → thay bằng local computation từ chuyen_doi_cho_phep data đã có sẵn

---

## V0.205 (28/03/2026) — Forensic Stabilization V4: dm_quy_trinh migration prep

| Field | Chi tiết |
|-------|----------|
| **Module** | M0 |
| **Scope** | Data migration preparation |

---

## V0.204 (19/03/2026) — M1 Nhân Sự UI redesign to Metronic pattern

| Field | Chi tiết |
|-------|----------|
| **Module** | M1/Nhân Sự |
| **Scope** | UI redesign — không thay đổi schema/action/logic/RBAC |
| **Files** | nhan-su-client.tsx (1 file, 348 insertions, 170 deletions) |

**Thay đổi chi tiết:**
- Thay DataTableSimple bằng inline table với orange gradient header (rounded-tl/tr-xl) matching /m0/phong-ban
- 4 stats pills (Tổng/Chính Thức/Thử Việc/Nghỉ) với Lucide icons và Metronic styling
- Inline filter bar (search + status dropdown), sortable columns (mã NV, họ tên, trạng thái), pagination với per-page selector
- Action column Pencil/Trash2 icons, responsive column visibility (tablet/mobile)

---

## V0.203 (14/03/2026) — Sản Phẩm & Khách Hàng UX fixes

| Field | Chi tiết |
|-------|----------|
| **Module** | M1/Sản Phẩm, M1/Khách Hàng |
| **Scope** | UX improvement |
| **Files** | san-pham-client.tsx, khach-hang-client.tsx, wizard-client.tsx |

**Thay đổi chi tiết:**
- **M1/SP:** Default perPage chuyển từ 25 xuống 10 để trang tải nhanh hơn
- **M1/KH:** Detail panel giữ trạng thái sau khi edit qua wizard: redirect về `/m1/khach-hang?detail=MA_KH`, useSearchParams + useEffect tự động mở lại panel
- **M1/SP:** Đồng bộ styling nút List/Grid toggle giữa Sản Phẩm và Khách Hàng

---

## V0.202–V0.190 (10–11/03/2026) — Metronic Backbone & Governance

> Giai đoạn tích hợp Metronic UI backbone và chuẩn hóa governance toàn hệ thống.

| Version | Tiêu đề | Scope |
|---------|---------|-------|
| V0.202 | Extend Metronic addendum into module overview docs | Docs |
| V0.201 | Sync Metronic UI governance into local + Notion SSOT | Governance |
| V0.200 | Align M4/M5/M9 overview metadata với owner decisions | Data/UI |
| V0.199 | Refine Metronic UI mandatory protocol wording | Governance |
| V0.198 | Add mandatory Metronic UI rule to governance core | Governance |
| V0.197 | Add Metronic UI research and implementation protocol | Docs |
| V0.196 | Extract M3 highlight cards into shared metadata selectors | UI refactor |
| V0.195 | Move M0/M3 feature cards to shared metadata selectors | UI refactor |
| V0.194 | Extract M1/M3/MF group cards into shared selectors | UI refactor |
| V0.193 | Roll out shared metadata selectors to overview pages | UI refactor |
| V0.192 | Preserve isolated B2/B3 skills in separate inventory doc | Governance |
| V0.191 | Restore governance standard after freeze root cause | Governance |
| V0.190 | Remove duplicate 5-skill blocks after confirmed freeze | Governance |

---

## V0.189–V0.185 (10/03/2026) — Agent Freeze Investigation

> Giai đoạn điều tra và fix Agent freeze khi đọc governance files.

| Version | Tiêu đề | Root cause |
|---------|---------|------------|
| V0.189 | Re-add suspicious skill blocks B2/B3 for isolation | Test isolation |
| V0.188 | Roll back recent AGENTS and CLAUDE sync blocks | Content rollback |
| V0.187 | Disable folder-open auto task (recreating terminal state) | .vscode/tasks.json |
| V0.186 | Sanitize local dev startup output (prevent mojibake) | UTF-8 encoding |
| V0.185 | Replace GEMINI.md with MODEL_GUIDE.md | Filename-level freeze |

**Kết luận:** 2 block duplicate 5-skill references là trigger gây Agent freeze.

---

## V0.184–V0.175 (09/03/2026) — Shell & Sidebar Redesign

| Version | Tiêu đề | Module |
|---------|---------|--------|
| V0.184 | Restore full governance sync (AGENTS, CLAUDE, GEMINI) | Governance |
| V0.181 | Add hard local-safety gate for skills/rules/backup | Governance |
| V0.180 | Reorganize overlapping versioning skills | Skills |
| V0.179 | Strengthen skill governance and skill mining workflow | Skills |
| V0.178 | Fix grouped HR routes — avoid opening M1 sidebar section | M1/M6 sidebar |
| V0.177 | Compact sidebar grouping cho Nhân Sự HR | Navigation |
| V0.176 | Flow-first HR navigation shortcuts | Navigation |
| V0.175 | Dark navy sidebar + compact MISA-style + remove dark mode | Shell UI |

**Kết quả V0.175:** Xóa dark mode hoàn toàn, sidebar dark navy cố định, slim collapsed width 3.5rem, layout spacing giảm 25–35%.

---

## V0.174–V0.170 (08–09/03/2026) — Auth & M3 Table Styling

| Version | Tiêu đề | Module |
|---------|---------|--------|
| V0.174 | Clarify M0 owns current auth, M9 inherits later | Auth contract |
| V0.173 | Standardize internal auth sessions in M0 | Auth/Session |
| V0.172 | Show explicit warning when session expired | Auth UX |
| V0.171 | Fail closed when stale sessions show partial menus | Auth security |
| V0.170 | Align M3 table styling with Metronic demo | M3 UI |

---

## V0.169–V0.153 (08/03/2026) — Metronic Foundation Rollout

> Giai đoạn lớn: tích hợp Metronic template, tạo foundation components, rollout toàn hệ thống.

| Version | Tiêu đề | Impact |
|---------|---------|--------|
| V0.169 | Unify border-radius + remove redundant table header | M3 |
| V0.168 | Metronic data table implementation cho M3/Báo Giá | M3 |
| V0.167 | Reset shell surfaces, recompose M0 | Shell + M0 |
| V0.166 | Tighten homepage card rhythm | Home |
| V0.165 | Realign shell chrome closer to Metronic backbone | Shell |
| V0.164 | Rename shell presets → real operating roles | Shell |
| V0.163 | Migrate Next 16 middleware → proxy convention | Platform |
| V0.162 | Add shell presets + overview density controls | Shell |
| V0.161 | Add live shell settings panel with local persistence | Shell |
| V0.160 | Roll compact backbone to M1/M4/MF | M1/M4/MF |
| V0.159 | Compact homepage shell + document Metronic backbone | Home + Docs |
| V0.158 | Move shell state into shared provider (AppShellProvider) | Shell |
| V0.157 | Strengthen app shell backbone + remove dev indicator | Shell |
| V0.156 | Roll out responsive foundation to home + M3 | Home + M3 |
| V0.155 | Extract reusable foundation components (PageHeader, StatCard, FilterBar) | Foundation |
| V0.154 | Remove old demo, start live M3 foundation pilot | M3 |
| V0.153 | Next 16 foundation gate pass + shell bridge | Platform |

---

## V0.152–V0.140 (04–08/03/2026) — Deploy Pipeline & Finance Hardening

| Version | Tiêu đề | Scope |
|---------|---------|-------|
| V0.152 | Demo V2 báo giá | M3 UI |
| V0.151 | Demo chuẩn hóa layout | M3 UI |
| V0.150 | Remove noisy deploy header warning | DevOps |
| V0.149 | Fix PowerShell escaping in deploy | DevOps |
| V0.148 | Harden deploy header verification | DevOps |
| V0.147 | Unblock release build from lint debt | DevOps |
| V0.146–V0.144 | Governance syncs + release doctor | Governance |
| V0.143 | VPS deploy standalone runtime | DevOps |
| V0.142 | MF smoke automation | MF + DevOps |
| V0.141 | MF persisted receipts + receivables | MF Data |
| V0.140 | M7 production hardening + payroll vouchers | M7 + MF Data |

---

## V0.139–V0.100 (07/02–03/03/2026) — Deploy, Features & Modules

> Xem file [MODULE-PROGRESS.md](MODULE-PROGRESS.md) để biết chi tiết chức năng từng module.

| Giai đoạn | Versions | Nội dung chính |
|-----------|----------|---------------|
| Deploy Pipeline | V0.139–V0.129 | One-command sync, GitHub→VPS workflow, runbook |
| Feature Dev | V0.128–V0.120 | Module metadata SSOT, tên VN, MF/M5 overview |
| M3 & M4 | V0.119–V0.100 | CRM tabs, M4 LSX/PDI workflows, gộp/tách LSX, M8 UI |

---

## V0.99–V0.50 (29/01–01/02/2026) — Pricing System

| Giai đoạn | Versions | Nội dung chính |
|-----------|----------|---------------|
| Tier Pricing | V0.99–V0.95 | TierBuilder, smart filter, duplicate validation, UOM fix |
| Pricing Admin UI | V0.77–V0.60 | Dialog redesign (Blueprint/Formula/Addon/Param), Việt hóa 100%, pagination |
| Pricing Engine | V0.59–V0.50 | SSOT enforcement, dm_addon_type, hardcoded threshold removal |

---

## V0.48–V0.20 (26–29/01/2026) — UI Builders, Schema & SSOT

| Giai đoạn | Versions | Nội dung chính |
|-----------|----------|---------------|
| UI Builders | V0.48–V0.42 | ViewJsonPanel, BilingualLabel, ParamRegistryBuilder, Test Vectors UI |
| Schema fixes | V0.41–V0.40 | Blueprint 2-key architecture, FK constraints |
| M1 SSOT Migration | V0.39–V0.30 | Sản phẩm redesign, Khách Hàng wizard, migrate in-memory → MySQL |
| DB & Governance | V0.29–V0.20 | UOM SSOT, 5-way sync, skills creation, Combo Gate, HARD ENFORCEMENT |

---

## V0.19–V0.05 (19–26/01/2026) — Foundation

| Version | Nội dung |
|---------|----------|
| V0.19 | Fix delete dialog button text |
| V0.18 | **MAJOR:** Disable ALL mock/demo data — DB SSOT duy nhất |
| V0.17 | Auto version bump watcher + git pre-commit hook |
| V0.16 | 3-layer version tracking + 5-way sync rule |
| V0.15 | Demo Module Tính Giá Offset + skills creation |
| V0.14 | Created 3 new skills |
| V0.13 | M3 Báo Giá: sắp xếp lại sections |
| V0.12 | M0 Dashboard: fix title cards |
| V0.11 | M3 Báo Giá: Detail Panel, layout 2 cột |
| V0.08 | Added 5 new skills |
| V0.06 | Added rule 12: Versioning (MANDATORY) |
| V0.05 | Test bump version script |

---

## V3.33.x (18/01/2026) — Khởi Tạo

| Version | Nội dung |
|---------|----------|
| V3.33.1 | Dashboard Premium Redesign + fix button type warnings |
| V3.33.0 | **Initial UI Skeleton** — M0, M1 modules layout |
