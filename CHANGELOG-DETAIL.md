# 📜 Changelog Chi Tiết — V0.231 → V0.100

> Trích xuất 100% từ version.ts (SSOT). Mỗi entry ghi rõ category, scope, chi tiết thay đổi.

---

## V0.231 (06/07/2026) — V3.44 Option A: Chứng Từ Kế Toán + Khóa Sổ

| Field | Value |
|-------|-------|
| **Category** | Feature / Schema / Finance |
| **Scope** | MF module — 1 new table (26 cols) + 1 new store + 1 new route + approve wiring + khóa sổ guards |

- [Schema] Tạo `chung_tu_ke_toan` (26 columns): chứng từ kế toán tổng hợp, 14 loại, khóa sổ flag
- [Logic] Auto-create `chung_tu_ke_toan` khi approve phiếu thu/chi (`trang_thai='draft'`, kế toán duyệt tay)
- [Logic] Khóa sổ cuối tháng: `khoaSoThang(thang, nam)` → set `khoa_so=1` cho approved records
- [Guard] 4 khóa sổ guards: update/cancel phiếu thu + update/cancel phiếu chi — chặn nếu chứng từ liên kết đã khóa sổ
- [Route] `/mf/ke-toan` — trang quản lý chứng từ kế toán + button khóa sổ
- [Version] Bump V0.230 → V0.231
- [Audit] Đóng gap F1/F3/F4 từ AUDIT-CHUNG-TU-KE-TOAN-20260706

---

## V0.230 (06/07/2026) — V3.44: Nghiệm Thu & Đối Chiếu Công Nợ

| Field | Value |
|-------|-------|
| **Category** | Feature / Schema / Finance |
| **Scope** | MF module — 2 new tables + 2 new routes + phieu_thu linkage |

- [Schema] Tạo `bien_ban_nghiem_thu` (V3.44 Option C): 22 columns, workflow draft→da_ky→cancelled
- [Schema] Tạo `cong_no_doi_chieu` (V3.44 Option C): 19 columns, workflow draft→da_gui→da_xac_nhan/tu_choi/qua_han
- [Schema] Seed `dm_form_mau`: FORM_NGHIEM_THU + FORM_XAC_NHAN_CONG_NO
- [Feature] Route `/mf/doi-chieu`: thay thế placeholder, implement CRUD + aging buckets + workflow
- [Feature] Route `/mf/nghiem-thu` (NEW): CRUD biên bản nghiệm thu từ PGH, compute items, VAT
- [Feature] Wire `phieu_thu` → `bien_ban_nghiem_thu`: approve phiếu thu liên kết tự động cập nhật `da_thanh_toan`
- [Store] `mf-doi-chieu-store.ts` + `mf-nghiem-thu-store.ts` (raw SQL, mysql2)
- [DB] Tổng bảng: 90 → 92

---

## V0.229 (05/07/2026) — Architecture Audit + Version Sync

| Field | Value |
|-------|-------|
| **Category** | Audit / Documentation |
| **Scope** | Read-only audit, no code changes |

- [Audit] R1: Confirmed no NestJS, no Prisma, no mobile app — ADR-20260705 logged
- [Audit] R2: `/mf/doi-chieu` confirmed as placeholder (31 lines, 0 logic)
- [Audit] R3: `dm_nhan_vien` confirmed dead (only in legacy docs/scripts)
- [Audit] R4: MC = Contract Management only (not content marketing)
- [Audit] R5: Version drift detected (code V0.229 vs report V0.226B)
- [Report] `AUDIT-V0227-ARCHITECTURE-AND-GAP-SCAN.md` created
- [Report] `GOVERNANCE-LOG.md` updated with ADR entry
- [Report] `README.md` MC description corrected

---

## V0.228 (05/07/2026) — Batch 1 CEO Apply + gia_von Fix

| Field | Value |
|-------|-------|
| **Category** | Security / User Management |
| **Scope** | Batch 1 real user creation + bug fix |

- [User] CEO `lienntk@intanphat.com` created (Batch 1 of Real User Pilot)
- [Fix] `maskSensitiveFields` — resolved session email correctly (was defaulting to null)
- [Fix] CEO role `la_admin=1` in `dm_vai_tro` — `gia_von` now visible to CEO
- [Report] `REAL-USER-PILOT-V0227-BATCH1-CEO.md` created

---

## V0.227 (05/07/2026) — Batch 0 Admin Verify + Real User Pilot Start

| Field | Value |
|-------|-------|
| **Category** | Security / Process |
| **Scope** | Batch 0 verify-only + Batch 1 preparation |

- [Verify] Admin `tan***@intanphat.com` verified (SELECT only, 6 ❌ rules enforced)
- [Process] V0.226B guard rules confirmed: batch-scoped rollback, manifest, shared email blocked
- [Report] `REAL-USER-PILOT-V0227-BATCH0-VERIFY.md` created

---

## V0.226B (05/07/2026) — Final Guard Before Owner-Approved Apply

| Field | Value |
|-------|-------|
| **Category** | Security / Process |
| **Scope** | Plan only — no code changes, no real users |

- [Security] Rollback siết thành **batch-scoped** với private runtime manifest
- [Security] Rollback manifest ghi chính xác: record IDs, batch_id, timestamps
- [Security] Pre-existing records explicitly excluded from rollback
- [Security] R3 role mapping: chỉ remove manifest IDs (không blanket DELETE)
- [Process] Batch 0 siết thành **verify-only**: SELECT only, 6 explicit ❌ rules
- [Process] Batch 0: không reset password, revoke, disable, remove mapping, duplicate, DML
- [Process] Shared email: blocked + no fake email workaround
- [Process] Employee-only record OK nếu schema cho phép (no user login)
- [Report] V0.226B final guard report created
- [Report] V0.226 apply plan updated with batch-scoped rollback
- [Safety] No real users created
- [Safety] No production deploy
- [Safety] No sensitive data published

---

## V0.226A (02/07/2026) — Apply Plan Hardened

| Field | Value |
|-------|-------|
| **Category** | Security / Process |
| **Scope** | Plan only — no code changes, no real users |

- [Security] Rollback strategy changed: DELETE-first → **disable-first** (5-step R1→R5)
- [Security] Hard delete requires 4 conditions + Owner explicit approval
- [Security] Password flow hardened: no file/log/report/commit/public
- [Security] Password delivery: private terminal + Owner kênh bảo mật
- [Process] Batch order: sequential mandatory, fail → STOP all
- [Process] Smoke test checklist: CEO (6 items) + Sales (6 items) + Admin (3 items)
- [Process] Batch 0 verify admin/dev → Batch 1 CEO → Batch 2 Sales → Batch 3/4/5 chờ Owner
- [Report] V0.226A hardening report created
- [Report] V0.226 apply plan updated with hardened rules
- [Identity] Admin tan***@intanphat.com reuse — no password reset, no duplicate
- [Identity] Shared email still blocked (4 candidates)
- [Safety] No real users created
- [Safety] No production deploy
- [Safety] No sensitive data published

---

## V0.225E (28/06/2026) — Menu Label Normalized + Final Cleanup

| Field | Value |
|-------|-------|
| **Category** | UX / Report |
| **Scope** | UI only |

- [UX] Menu label normalized: "Phân quyền & bảo mật" (sentence case, 5 sources synced)
- [UX] Old label "Bảo Mật Ứng Dụng" fully removed from codebase
- [UX] Vietnamese encoding fix: MENU_CODE_LABEL_MAP fallback for garbled DB text
- [UX] Role name encoding fix: safeRoleName with ROLE_LABEL_FALLBACK
- [UX] autoCase applied via toVietnameseTitleCase
- [UX] Search/filter added to role assignment, account status, audit log
- [Report] Wording fix: E02/E09 → "Ready — verify/apply in V0.226"
- [Report] E06 → "Existing user — reuse, do not create duplicate"
- [Identity] Admin tan***@intanphat.com verified, no password reset
- [Identity] Shared email still blocked for personal login (4 candidates)
- [Route] `/m0/security` unchanged
- [Safety] No real users created
- [Safety] No sensitive data published

---

## V0.225D (28/06/2026) — Existing Admin Verified + Permission Menu UX Cleaned

| Field | Value |
|-------|-------|
| **Category** | Auth/Security + UX |
| **Scope** | Logic / UI |

- [Identity] Trần Minh Tân corrected: tan***@intanphat.com (existing admin, NOT duplicate)
- [Identity] Admin login verified in login form default
- [UX] Menu renamed: "Bảo Mật Ứng Dụng" → "Phân Quyền & Bảo Mật" (5 files)
- [UX] Description updated: reflects permission + security scope
- [UX] Route `/m0/security` unchanged (no breaking links)
- [Report] Public-safe summary table with 10 candidates

---

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — Identity Management |
| **Scope** | Data / Logic |

- [Identity] 13 source rows normalized → 10 unique employee candidates
- [Identity] 3 rows dedupe-merged (Lê Thụy Ngọc Hân KD1/KD3 aliases)
- [Security] 5 shared email groups detected. 2 groups (5 people) BLOCKED for user login
- [Security] 1 candidate BLOCKED_INCOMPLETE_NAME (missing full name)
- [Schema] Audit: `hr_employee_nhanvien.user_id` nullable → employee CAN exist without user login
- [Schema] Audit: `user_account.email` used as login identity (unique lookup)
- [Script] `prepare-real-users-v0225c.ts`: dry-run + employee-only + rollback modes
- [Report] Public-safe: no full emails, no phones, no passwords

---

## V0.225 (28/06/2026) — Owner Matrix Confirmation + Safe Scripts

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — Pilot Readiness |
| **Scope** | Data / DevOps |

- [Pilot] Owner confirmation matrix: 10 candidates, 14 decision questions
- [Script] Safe creation script with dual kill switch (ENV + CLI)
- [Script] Rollback template (disable + remove packs)
- [Security] No real users created. Scripts locked by default.

---

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — Pilot Readiness |
| **Scope** | Data / DevOps |

- [Pilot] Tạo private employee matrix dry-run cho 10 candidates
- [Pilot] Phân loại: 7 pilot-ready, 1 blocked, 1 deferred, 1 no-login
- [Pilot] Validator script: 10 checks, 0 errors, 7 warnings (missing emails)
- [Security] Private matrix gitignored — NOT tracked by git
- [Security] No real users created. No passwords used.
- [Report] Public-safe report pushed to GitHub

---

## V0.223 (27/06/2026) — Early Staff Pilot Gate

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — RBAC |
| **Scope** | Logic |

- [M0] Security: 8/8 functions double-locked (requireActionPermission + requireAdminContext)
- [M4] PDI: 17/17 functions — replaced dummy checkPermission with real RBAC
- [M1] KH: 12/12 functions guarded
- [M3] BG: 16/16 functions guarded
- [M3] DH: 5/5 functions guarded
- [M4] LSX: 20/20 functions guarded
- [Coverage] 135/303 functions (44.6%), 10/42 files fully guarded

---

## V0.222 (27/06/2026) — RBAC P1 Gap Closeout

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — RBAC |
| **Scope** | Logic |

- [MF] Finance: 13 functions guarded (cong-no/phieu-thu/phieu-chi)
- [M6] HR: 28/28 protection lock
- [M7] Payroll: 16/16 protection lock
- [THIET_KE] Scope correction: can_view_all_customers → 0

---

## V0.221 (27/06/2026) — RBAC Deep Audit

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — RBAC |
| **Scope** | Audit |

- [Audit] Full RBAC deep scan: 5/42 files guarded, 37 gaps identified
- [Audit] Priority matrix created for gap closeout
- [Report] RBAC-DEEP-AUDIT-20260627.md

---

## V0.218 (15/06/2026) — P01 Foundation + Internal Mini-Pilot

| Field | Value |
|-------|-------|
| **Category** | Auth/Security — Foundation |
| **Scope** | Logic / UI / Data |

- [Foundation] 29 RBAC guards in Server Actions
- [Foundation] SecurityContext, RBAC tables, permission packs
- [Foundation] Smoke test 15/15 PASS
- [Flexible] Permission pack system for accounting roles
- [Report] Multiple golive reports published

---

## V0.217 (14/06/2026) — Audit & Hardening Module Khách Hàng — Phase A Critical Fixes

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục — Khách Hàng |
| **Scope** | Logic / Data Safety |

- [Audit] Audit chuyên sâu toàn bộ module Khách Hàng (M1.1) — phát hiện 12 vấn đề (3 Critical, 2 High, 4 Medium, 3 Low)
- [Critical Fix] Thêm FK Guard cho DELETE: kiểm tra bao_gia, don_hang, phieu_thu, cong_no, phieu_chi trước khi xóa — chặn xóa hoàn toàn nếu có liên kết, hiển thị lỗi rõ ràng
- [Critical Fix] Wrap tất cả bundle operations trong MySQL Transaction (BEGIN/COMMIT/ROLLBACK) — createCustomerBundle, updateCustomerBundle, deleteCustomer giờ atomic
- [Fix] Thay alert() placeholder trên nút Lịch sử bằng Audit Dialog UI chuyên nghiệp
- [Refactor] Thêm helper connQuery() cho transaction queries, export checkCustomerReferences() + CustomerReferenceCheck interface
- [Verify] TypeScript tsc --noEmit PASS — zero errors, backward compatible
- [Files] m1-1-store.ts (+200/-170), actions.ts (+3/-2), khach-hang-client.tsx (+1/-4)

---

## V0.216 (08/05/2026) — Add rule 14: PUBLIC REPORT SYNC + 5-way governance sync

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Data/Schema |

- [Governance] Them section 14) PUBLIC REPORT SYNC vao AGENTS.md: bat buoc cap nhat bao cao len repo public https://github.com/irissnss/Baocaoerptanphat sau moi lan code/fix/audit/deploy.
- [Security] Chot danh sach NGHIEM CAM public: source code, DB schema, API endpoints, credentials, business logic chi tiet, du lieu that, server info, governance files.
- [Security] Chot danh sach DUOC PHEP public: version/changelog, module progress, thong ke tong hop, tech stack tong quan.
- [Governance] 5-WAY SYNC: .cursorrules, .antigravityrules, AGENTS.md, CLAUDE.md, GEMINI.md — SHA256 verified MATCH.
- [Repo] Tao repo public https://github.com/irissnss/Baocaoerptanphat voi README.md chua changelog V0.00 den V0.215.
- [Files] AGENTS.md, .cursorrules, .antigravityrules, CLAUDE.md, GEMINI.md, version.ts.

---

## V0.215 (04/05/2026)

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Maintenance |

- Code update: docs/bg-login.png, docs/demo-v6.css, docs/login-demo.html, docs/ui-demo-v6.html, next-env.d.ts, +8 more

---

## V0.214 (04/05/2026)

| Field | Value |
|-------|-------|
| **Category** | General |
| **Scope** | UI only |

- UI mobile footer compact + module grid responsive + hover effects

---

## V0.213 (30/04/2026)

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Maintenance |

- Code update: public/login-illustration.png, src/app/auth/login/login-client.tsx, src/app/auth/login/page.tsx

---

## V0.212 (30/04/2026) — Login UI Finalization: Full-illustration + Floating Card

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | UI only |

- [Auth/Login] Layout 2 phần: Top bar (logo + "Tân Phát ERP" + heading + subtitle) + Card login floating bên phải trên nền full ảnh minh họa.
- [Auth/Login] Desktop: Logo + branding text góc trên trái, card login bên phải sát top, ảnh illustration full viewport object-cover.
- [Auth/Login] Mobile: Logo compact + subtitle trên cùng, card login ngay dưới (không cần scroll), padding/gap/font responsive (px-5 py-6, h-11, gap-4).
- [Auth/Login] Card: bg-white/95 backdrop-blur, rounded-2xl mobile / rounded-[20px] desktop, orange gradient top accent.
- [UI] Login form responsive: input h-11/h-12, heading text-xl/text-2xl, gap-4/gap-5 theo breakpoint.
- [Files] page.tsx, login-client.tsx.
- [Scope] UI-only. Không thay đổi auth logic/API/schema.

---

## V0.211 (27/04/2026)

| Field | Value |
|-------|-------|
| **Category** | General |
| **Scope** | Maintenance |

- Test: Verify auto version bump system

---

## V0.210 (2026-04-17) — Local dev: default Webpack (Turbopack panic os error 21 on Windows)

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | DevOps |

- [Dev] `npm run dev` / `dev:3000` dung `next dev --webpack` de tranh Turbopack FATAL `The device is not ready. (os error 21)` tren Windows.
- [Dev] Them `npm run dev:turbo` (`next dev --turbopack`) cho ai muon thu lai Turbopack.
- [Files] package.json, version.ts, WORK_LOG.md.

---

## V0.209 (2026-03-29) — M1 Vi Tri UI consistency fix to match Metronic system design

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [M1/VT] Thay DataTableSimple + StandardListPageHeader bang inline Metronic-style table voi orange gradient header (rounded-tl/tr-xl) matching /m0/phong-ban va /m1/nhan-su.
- [M1/VT] Them 3 stats pills (Tong/Co PB/Chung) voi Lucide icons va Metronic styling dong bo voi phong-ban reference.
- [M1/VT] Inline filter bar (search + sort dropdown) nam trong table card, sortable columns (ma_vi_tri, ten_vi_tri, cap_bac), pagination voi per-page selector.
- [M1/VT] Action column Pencil/Trash2 icons, audit history icon, cap_bac badges.
- [M1/VT] Sheet form gradient orange header, sticky footer, section grouping dong bo voi phong-ban pattern.
- [M1/VT] Tich hop normalizeText (search khong dau) va toVietnameseTitleCase (Title Auto Case) theo SSOT.
- [Scope] Chi UI/UX consistency. Khong thay doi schema/action/logic/RBAC/route/ownership.
- [Files] vi-tri-client.tsx (1 file). 18-point forensic diff completed vs phong-ban reference.

---

## V0.208 (2026-03-28) — System-wide mobile viewport hardening

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Bug fix |

- [Auth/ChangePassword] Fix SAME ROOT CAUSE as login keyboard bug: min-h-screen + flex items-center justify-center + 3 password fields → form bi ep co khi virtual keyboard bat tren mobile. Fix: min-h-dvh + top-aligned mobile (pt-12) + overflow-y-auto + lg:items-center desktop.
- [Error/403/GlobalError] Standardize min-h-screen → min-h-dvh toan bo standalone pages de dam bao consistent dynamic viewport behavior. Pages: error.tsx, global-error.tsx, 403/page.tsx.
- [Forensic] System-wide scan 11 files co min-h-screen pattern. main-layout.tsx va M3/M5 pages INTENTIONALLY NOT CHANGED (inside shell, natural scroll, no centering bug).
- [Verify] 14 test cases: Login (3 viewports + keyboard sim), ChangePassword (2 viewports + keyboard sim), Desktop regression check. All PASS.
- [Files] change-password/page.tsx, error.tsx, global-error.tsx, 403/page.tsx, version.ts.

---

## V0.207 (2026-03-28) — CRITICAL: Mobile login keyboard layout fix

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Bug fix |

- [Auth/Login] Root cause: flex items-center + min-h-screen khien form bi ep giua viewport khi virtual keyboard bat. Khi keyboard chiem 50%+ viewport height, submit button va password field bi day ra ngoai viewable area.
- [Fix] Chuyen mobile layout tu vertical-center sang top-aligned (pt-12 + overflow-y-auto). Giu lg:items-center cho desktop. Dung min-h-dvh thay min-h-screen cho dynamic viewport handling.
- [Layout] Them interactiveWidget: resizes-content va viewportFit: cover vao viewport config (layout.tsx). Dam bao iOS/Android xu ly keyboard resize dung cach + ho tro notch devices.
- [Verify] Test 10 cases tren 3 viewports (iPhone 375x812, Android 360x640, Desktop 1280x800) voi keyboard simulation (375x406, 375x350, 360x320). Tat ca PASS.
- [Files] page.tsx (login), layout.tsx (viewport config).

---

## V0.206 (2026-03-28) — Forensic Stabilization V5: Critical async bug fixes

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | Data/Schema |

- [M3/BG] Fix bao-gia-client.tsx: loai bo import truc tiep workflow-service (DB-backed) tu client component.
  - Tao validateBaoGiaTransitionAction server action moi trong actions.ts.
  - Refactor validateWorkflowTransition va executeTransition sang server actions, fix Promise-truthy bypass trong validation.
- [M0/QT] Fix quy-trinh-client.tsx: isTerminalState (async DB call) dung trong .filter() dong bo gay moi state deu la terminal.
  - Thay the bang local computation tu chuyen_doi_cho_phep data da co san tren DmQuyTrinh object.
- [Scope] Chi fix async boundary violations. Khong thay doi schema/contract/logic.
- [Files] bao-gia-client.tsx, actions.ts, quy-trinh-client.tsx.

---

## V0.205 (2026-03-28) — Forensic Stabilization V4: dm_quy_trinh migration prep

| Field | Value |
|-------|-------|
| **Category** | Database |
| **Scope** | Data/Schema |

*Không có chi tiết bổ sung.*

---

## V0.204 (2026-03-19) — M1 Nhan Su UI redesign to Metronic/phong-ban pattern

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [M1/NS] Thay DataTableSimple bang inline table voi orange gradient header (rounded-tl/tr-xl) matching /m0/phong-ban.
- [M1/NS] Them 4 stats pills (Tong/Chinh Thuc/Thu Viec/Nghi) voi Lucide icons va Metronic styling.
- [M1/NS] Inline filter bar (search + status dropdown), sortable columns (ma_nv, ho_ten, trang_thai), pagination voi per-page selector.
- [M1/NS] Action column Pencil/Trash2 icons, responsive column visibility (tablet/mobile).
- [Files] nhan-su-client.tsx (1 file, 348 insertions, 170 deletions). Khong thay doi schema/action/logic/RBAC.

---

## V0.203 (2026-03-14) — San Pham & Khach Hang UX fixes

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [M1/SP] Default perPage chuyen tu 25 xuong 10 de trang tai nhanh hon tren mobile va desktop.
- [M1/KH] Detail panel giu trang thai sau khi edit qua wizard: wizard redirect ve `/m1/khach-hang?detail=MA_KH`, client useSearchParams + useEffect tu dong mo lai panel.
- [M1/SP] Dong bo styling nut List/Grid toggle giua San Pham va Khach Hang: container `border rounded-lg`, buttons `w-9 h-9`, active `bg-orange-50 text-orange-600`.
- [Files] san-pham-client.tsx, khach-hang-client.tsx, wizard-client.tsx.

---

## V0.202 (2026-03-11) — Extend Metronic addendum into module overview docs

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Logic |

- [Module Docs] Bo sung `UI Backbone Addendum (V3.43)` vao trang tong quan module local `ERP Modules - Tổng Quan & Hướng Dẫn` de task UI o tang module docs cung thay Metronic gate.
- [Notion MCP] Cap nhat trang `ERP Modules - Tổng Quan & Hướng Dẫn` tren Notion voi addendum Metronic va them buoc doc `METRONIC_UI_RESEARCH_PROTOCOL.md` trong workflow AI/Cursor.
- [Scope Control] Co y chi bo sung addendum/gate Metronic, khong mo rong sang viec audit lai toan bo noi dung lich su `86 bang / 10 modules` cua trang overview cu.

---

## V0.201 (2026-03-11) — Sync Metronic UI governance into local and Notion SSOT

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [Local SSOT] Bo sung ghi nhan Metronic vao local `FullSpec`, `rules`, va `changelog` (ca ban goc va ban mirror trong `docs/🏭 ERP TanPhat - FIX/`) de khong bi lech voi governance core.
- [Notion MCP] Cap nhat 3 trang SSOT tren Notion: `ERP_TanPhat_FullSpec.md`, `ERP_TanPhat_FullSpec.rules.md`, va `ERP_TanPhat_FullSpec.changelog.md` voi UI backbone, UI checklist gate, va changelog `V3.43.6`.
- [Consistency] Chot ro Metronic la nen tang UI chinh thuc, `Demo 1/8/13` co vai tro ro rang, va task UI phai khai bao Metronic sources, base pattern, reuse/adapt/custom, responsive strategy, va impact scope.

---

## V0.200 (2026-03-11) — Align M4/M5/M9 overview metadata with owner decisions

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [M5] Chuan hoa metadata theo SSOT V3.43: bo sung du 8 nhom chuc nang (14 bang) trong `app-navigation-metadata`, refactor `/m5` overview thanh consumer metadata chung.
- [M4] Chuyen `/m4` overview bo hardcode groups, doc tu shared selector (`getModuleGroupCards("M4")`) + readiness display de dong bo voi shared navigation metadata.
- [M9] Khoa naming V3.43 tren placeholder metadata/note: `customer_portal_*` + `dm_auto_pricing_formula` (shared), dong bo hien thi theo readiness mapping.

---

## V0.199 (2026-03-11) — Refine Metronic UI mandatory protocol wording across governance files

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [Governance] Cap nhat wording cua section Metronic UI trong `.cursorrules`, `.antigravityrules`, `AGENTS.md`, `CLAUDE.md`, va `GEMINI.md` theo ban protocol moi.
- [Read Gate] Khoa ro hon viec bat buoc doc `docs/METRONIC_UI_RESEARCH_PROTOCOL.md`, `ERP_TanPhat_FullSpec.md`, va module/spec lien quan truc tiep cho moi task UI.
- [Output Gate] Chot output UI phai neu ro Metronic sources da review, base page/demo/pattern/component, reuse/adapt/custom, responsive strategy, va impact scope.

---

## V0.198 (2026-03-11) — Add mandatory Metronic UI rule to governance core

| Field | Value |
|-------|-------|
| **Category** | M8 Công Việc |
| **Scope** | UI only |

- [Governance] Bo sung `Metronic UI Rule (Mandatory)` vao `.cursorrules`, `.antigravityrules`, `AGENTS.md`, `CLAUDE.md`, va `GEMINI.md`.
- [Execution Gate] Bat buoc doc `docs/METRONIC_UI_RESEARCH_PROTOCOL.md`, `ERP_TanPhat_FullSpec.md`, va review nguon Metronic chinh thuc truoc moi task UI.
- [Design Lock] Chot `Demo 1` la backbone mac dinh, `Demo 8` cho KPI/analytics, `Demo 13` cho dense admin/backoffice, va buoc phai khai bao reuse/adapt/custom + responsive strategy truoc khi code UI.

---

## V0.197 (2026-03-11) — Add Metronic UI research and implementation protocol

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [Governance] Bo sung `docs/METRONIC_UI_RESEARCH_PROTOCOL.md` de chot protocol nghien cuu, mapping demo, reuse/adapt/custom policy, va quality gate cho moi task UI.
- [UI Strategy] Khoa `Demo 1` lam backbone mac dinh, `Demo 8` cho KPI/analytics, va `Demo 13` cho dense admin/backoffice theo dinh huong toan ERP.
- [Execution Gate] Them checklist bat buoc truoc khi code UI: review Metronic sources, chon pattern base, xac dinh responsive strategy, va giu SSOT ERP lam quyet dinh toi cao.

---

## V0.196 (2026-03-11) — Extract M3 highlight cards into shared metadata selectors

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [SSOT] Bo sung metadata selector cho `M3` highlight cards (`getModuleHighlightCards`, `getModuleHighlightToneClasses`) de gom `Luong Chinh` + `Key Feature` ve 1 nguon.
- [Consumer] `src/app/m3/page.tsx` bo hardcode highlight block va render tu selector chung, giam duplicate text/class trong overview.
- [Consistency] Giu nguyen menu route contract va chi chuan hoa layer hien thi metadata cho `M3` overview.

---

## V0.195 (2026-03-11) — Move M0 feature cards and M3 workflow lanes to shared metadata selectors

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | UI only |

- [SSOT] Bo sung bo selector metadata chung cho `M0` feature cards (`getModuleFeatureCards`, `getFeatureToneClasses`) va cho `M3` workflow lanes (`getModuleWorkflowLanes`, `getWorkflowLaneClasses`, `getWorkflowStepClasses`).
- [Consumer] `src/app/m0/page.tsx` bo hardcode `featureCards`/`toneClassMap`, chuyen sang doc selector tu `app-navigation-metadata` de dong bo va mo rong de.
- [Consumer] `src/app/m3/page.tsx` bo hardcode 2 workflow cards, render tu metadata lane chung de giam duplicate UI text/class.
- [Build Gate] Sua `src/app/mf/page.tsx` de guard `group.href` nullable truoc khi render `<Link>`, loai blocker TypeScript khi build production.

---

## V0.194 (2026-03-10) — Extract M1/M3/MF group cards into shared selectors

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [SSOT] Bo sung `getModuleGroupCards()` trong `app-navigation-metadata` va dua toan bo config group card `M1/M3/MF` ve 1 nguon chung.
- [Consumer] `M1`, `M3`, `MF` page bo khai bao group arrays local, chuyen sang doc tu selector chung de giam duplicate config va drift statusLabel.
- [Consistency] Group badges dung readiness display mapping chung (`getReadinessDisplay`) thay vi text hardcode rieng theo tung page.

---

## V0.193 (2026-03-10) — Roll out shared metadata selectors to remaining overview pages

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | General |

- [Overview] `M0`, `M1`, `M3`, `MF` doi phan header readiness/status sang doc tu `app-navigation-metadata` de khong con moi page tu dat trang thai rieng.
- [Consistency] Dong bo title/table/progress hiển thị theo selector chung, xoa hardcode de lech reality nhu `M3 In Development` va so lieu tong bang co dinh.
- [Contract Safety] Giu nguyen Menu Lite contract (route, menu key, ownership, RBAC), chi chuan hoa lop render metadata cho cac trang tong quan.

---

## V0.192 (2026-03-10) — Preserve isolated B2/B3 skills in separate inventory doc

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Governance |

- [Preservation] Tach 5 skill trong cum `B2+B3` sang `docs/GOVERNANCE-SKILL-INVENTORY.md` de khong mat thong tin da duoc bo sung truoc do.
- [Safety] Giu governance core sach, khong dua lai block duplicate vao `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.cursorrules`, hoac `.antigravityrules`.
- [Reuse Rule] Ghi ro quy tac neu can phuc hoi sau nay: re-add tung item mot, test Agent sau moi lan, va uu tien luu reference ngoai governance core.

---

## V0.191 (2026-03-10) — Restore governance standard after freeze root cause isolation

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [Root Cause Kept Fixed] Giu nguyen viec loai bo 2 block duplicate 5-skill references khoi governance core, vi day la nguyen nhan da duoc xac nhan gay treo Agent.
- [Standard Restored] Dua ten file tro lai `GEMINI.md`, dong bo lai `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.cursorrules`, va `.antigravityrules`, dong thoi khoi phuc cac phan `B1` va `B4` khong phai nguon loi.
- [Reference Cleanup] Cap nhat lai skills, docs, va verification scripts de dung ten `GEMINI.md`, khong con drift `MODEL_GUIDE.md` trong governance active.

---

## V0.190 (2026-03-10) — Remove duplicate 5-skill blocks from AGENTS and CLAUDE after confirmed freeze

| Field | Value |
|-------|-------|
| **Category** | UI/Shell |
| **Scope** | General |

- [Confirmed Cause] Sau khi them lai `B2+B3` thi Agent treo tro lai, xac nhan 2 block duplicate skill reference la trigger gay freeze trong `AGENTS.md` va `CLAUDE.md`.
- [Cleanup] Go lai 2 block duplicate gom 5 skill `css-var-density-tuning`, `foundation-component-rollout`, `theme-cleanup-single-lock`, `shell-surface-alignment`, va `sidebar-route-grouping`.
- [Stability] Giu `B1` va `B4` tiep tuc o trang thai rollback an toan de uu tien workspace on dinh, tranh nhap lai noi dung da co bang chung gay treo.

---

## V0.189 (2026-03-10) — Re-add suspicious skill blocks B2 and B3 only for freeze isolation

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Maintenance |

- [Isolation] Them lai duy nhat 2 cum 5 skill moi (`B2` va `B3`) vao `AGENTS.md` va `CLAUDE.md`, trong khi van giu `B1` va `B4` o trang thai rollback.
- [Goal] Xac dinh nhanh xem nhom 5 skill references moi co phai thu pham chinh gay Agent freeze hay khong, truoc khi dong vao cac cum rename/addendum khac.
- [Method] Neu user test lai va treo tro lai, co the khoa ngay pham vi loi vao `B2/B3` ma khong can nghi sang `B1` hoac `B4`.

---

## V0.188 (2026-03-10) — Roll back recent AGENTS and CLAUDE sync blocks for content isolation

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [Diagnosis] Roll back 4 cum noi dung nghi van tren `AGENTS.md` va `CLAUDE.md`: 1 dong skill mapping, 2 cum 5 bullet skill moi, va cum rename/addendum drift de test lai Agent freeze theo block.
- [Scope] Giu nguyen `MODEL_GUIDE.md` va cac thay doi tooling/debug khac; chi rollback 2 file dang duoc anh xac nhan la xuat hien cung luc voi symptom treo.
- [Next Step] Sau khi user test lai, se re-add tung cum `B1/B2/B3/B4` de khoa chinh xac block noi dung gay treo neu symptom bien mat.

---

## V0.187 (2026-03-10) — Disable folder-open auto task that kept recreating Agent terminal state

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [Root Cause] Phat hien `.vscode/tasks.json` dang auto-run `npm run dev:withdb` bang `runOn: folderOpen`, khien terminal startup path va task state tai tao moi lan mo workspace.
- [Stability] Bo `runOn: folderOpen`, chuyen task labels sang ASCII, va xoa lai workspace task/terminal state de ngan Antigravity ke thua buffer co nguy co invalid UTF-8.
- [Debug] Giam them mot duong tai tao treo Agent ma khong can user chu dong chay dev command.

---

## V0.186 (2026-03-10) — Sanitize local dev startup output to prevent terminal mojibake in Agent context

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [Debug] Loai bo emoji va chuoi Unicode de gay mojibake trong `dev-withdb`, `ensure-mysql-3306`, `dev-clean`, `dev-clear`, va `test-mysql-connection`.
- [Evidence] Kiem lai `ensure-mysql-3306.ps1` va `npm run test:mysql` deu ra output ASCII sach, khong con chuoi loi UTF-8 tren terminal startup path.
- [Risk Reduction] Giam kha nang Agent auto-attach terminal buffer bi marshal fail voi `invalid UTF-8` khi workspace mo lai.

---

## V0.185 (2026-03-10) — Replace GEMINI.md with MODEL_GUIDE.md to avoid Antigravity filename freeze

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Governance |

- [Investigation] Xac nhan Antigravity van treo ngay ca khi `GEMINI.md` chi con noi dung ASCII toi gian, nen loi nghieng ve filename-level thay vi content-level.
- [Governance] Doi ten file active tu `GEMINI.md` sang `MODEL_GUIDE.md` va cap nhat rule, skill, docs, va verification scripts dang phu thuoc ten cu.
- [Safety] Giu nguyen noi dung governance 3-file sync (`AGENTS.md`, `CLAUDE.md`, `MODEL_GUIDE.md`) de khong mat SSOT trong khi loai bo trigger treo cua Antigravity.

---

## V0.184 (2026-03-10) — Restore full governance sync between AGENTS, CLAUDE, and GEMINI

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Data/Schema |

- [Governance] Dong bo `AGENTS.md` ve cung noi dung day du nhu `CLAUDE.md` va `GEMINI.md`, bo sung cac skill references moi dang co hieu luc trong repo.
- [Consistency] Chot lai mapping skill FK lookup merged (`ref-options-from-server`) va 2 cum skill additions de tranh drift giua cac file huong dan AI.
- [Investigation] Khoa 3 file governance ve mot mat bang noi dung de phuc vu viec doi chieu issue Agent loading theo workspace nay.

---

## V0.181 (2026-03-09) — Add hard local-safety gate for skills, rules, and backup assets

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Governance |

- [Governance] Cam agent de xuat push `.cursor/skills/**`, `.cursor/rules/**`, backup local, transcript, va research artifacts len `origin/main` neu Owner chua yeu cau ro.
- [Safety] Cam tu y xoa/move/overwrite skill local, rule local, backup local, docs local trong cac thao tac "cleanup"; neu chua chac local-only hay final-safe thi phai dung va hoi.
- [Rules] Them rule always-apply `local-skill-safety.mdc` va dong bo wording sang bo governance + deploy checklist de agent nao cung doc cung mot gate.

---

## V0.180 (2026-03-09) — Reorganize overlapping versioning skills around one primary workflow

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Governance |

- [Skills] Reposition `versioning-change-history`, `version-bump-on-feature`, va `update-work-log` thanh supporting-reference, khong con tranh trigger voi `versioning-auto-log`.
- [Governance] Chot `versioning-auto-log` la primary workflow cho bump version, changelog, va `WORK_LOG.md`; cac skill con lai chi con vai tro field-map, bump-decision, va concurrency-safe logging.
- [Docs] Them `status` va `superseded_by` trong cum skill overlap de agent nhin vao la biet skill nao la main, skill nao la support.

---

## V0.179 (2026-03-09) — Strengthen skill governance and skill mining workflow

| Field | Value |
|-------|-------|
| **Category** | Governance |
| **Scope** | Data/Schema |

- [Skills] Them `skill-mining-governance` de mine pattern lap, chong duplicate skill, va chot keep/supplement/merge/reorganize/candidate truoc khi tao skill moi.
- [Skills] Nang cap `versioning-auto-log`, `schema-migration-safe`, `documentation-sync` voi gate manh hon cho version/history, data safety, va governance sync.
- [Skills/UI] Reorganize `premium-module-page-redesign` va `transactional-page-redesign` de tach ro shell overview voi transactional workspace, giam overlap trigger.
- [Governance] Bo sung reference `skill-mining-governance` vao bo rule 5 chieu de agent chu dong audit skill library thay vi tao skill roi rac.

---

## V0.178 (2026-03-09) — Fix grouped HR routes to avoid opening M1 sidebar section

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | Bug fix |

- [Sidebar] Them `visual owner` cho cac route da gom trong cum `Nhan Su HR` de `/m1/nhan-su`, `/m1/vi-tri`, `/m0/phong-ban`, `/m6`, `/m7` chi active/expand tai 1 cum sidebar duy nhat.
- [UX] Ngan `Danh Muc / M1` tu bung ra khi dang dung route nhan su da duoc group, giu dung muc tieu `tranh duplicate visual entry point`.
- [Contract] Khong doi `dm_menu`, khong doi route, khong doi ownership `M0/M1/M6/M7`; chi sua UI state cho khop SSOT `Compact HR Sidebar Grouping`.

---

## V0.177 (2026-03-09) — Replace bulky HR shortcut layer with compact sidebar grouping

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | General |

- [Navigation] Gom `Phong Ban`, `Nhan Su`, `Vi Tri`, `Van Hanh HR`, `Tien Luong` vao 1 cum `Nhan Su HR` trong sidebar va an cac diem vao duplicate o `M0/M1/M7`.
- [UI] Xoa shortcut layer khoi `sidebar.tsx`, `app/page.tsx`, `app/m0/page.tsx`, `app/m1/page.tsx`; doi `m6` thanh nhom dieu huong `Nhan Su HR` va giu `Tien Luong` la label cho payroll.
- [Cleanup/Docs] Xoa `src/lib/hr-navigation-flow.ts`, `src/components/foundation/hr-flow-shortcut-grid.tsx`, dong bo docs/changelog theo huong group sidebar gon.

---

## V0.176 (2026-03-09) — Add flow-first HR navigation shortcuts without changing module ownership

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | General |

- [Navigation] Them `src/lib/hr-navigation-flow.ts` lam SSOT presentation cho luong `To Chuc -> Ho So Nhan Su -> Van Hanh HR -> Payroll`, giu ownership du lieu tai `M0/M1/M6/M7`.
- [UI] `sidebar.tsx`, `app/page.tsx`, `app/m0/page.tsx`, `app/m1/page.tsx` them shortcut layer de gom `Phong Ban`, `Nhan Su`, `Vi Tri`, `M6`, `M7` theo nghiep vu ma khong doi route, menu key, hay RBAC contract.
- [Docs] Dong bo overview + docs module M0/M1/M6/M7 de chot lai: flow-first chi la navigation/UX, khong phai re-parent module.

---

## V0.175 (2026-03-09) — Dark navy sidebar + compact MISA-style layout + remove dark mode

| Field | Value |
|-------|-------|
| **Category** | UI/Shell |
| **Scope** | UI only |

- [UI] Xoa dark mode hoan toan: chi giu 1 theme light duy nhat; xoa toggle theme tren topbar; xoa toan bo `.dark` CSS block va `dark:` Tailwind classes.
- [UI] Sidebar dark navy co dinh: CSS vars `--sidebar-bg/border/text/*` dat gia tri dark navy trong `:root`, sidebar luon nen xanh dam + text/icon trang.
- [UI] Slim collapsed sidebar: width giam tu 4.5rem → 3.5rem, item padding p-2.5 → p-2, tao sidebar strip mong giong MISA.
- [UI] Tighten layout spacing: header height, canvas padding, card gaps, topbar padding, icon sizes deu giam 25-35% theo compact density MISA MeinVoice.
- [Files] globals.css, app-shell-provider.tsx, sidebar.tsx, topbar.tsx, main-layout.tsx, page.tsx, layout.tsx.

---

## V0.174 (2026-03-09) — Clarify M0 owns current auth while M9 inherits later

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Data/Schema |

- [Contract] Chot lai wording: `M0` la noi trien khai auth/session hien tai cho toan he thong; `M9` chi la huong ke thua ky thuat multi-session trong tuong lai.
- [Docs] Cap nhat FullSpec/rules/changelog/M0 docs va migration comments de xoa cach dien dat de hieu nham `M9` la current auth owner.
- [Notion] Se sync lai cac page SSOT de wording tren workspace giong local docs, khong lech nghia ownership nua.

---

## V0.173 (2026-03-09) — Standardize internal auth sessions in M0 with future M9 inheritance

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Data/Schema |

- [Auth] Them `src/lib/security-session-store.ts` de dua session SSOT sang bang `user_session`, ho tro multi-session, inactivity timeout, revoke current/all sessions, va legacy backfill tu `user_account.refresh_token`.
- [Schema/Verify] Them migration `20260309_m0_internal_user_session.sql`, script `smoke-m0-security.ts`, cap nhat deploy scripts/package verify de gate M0 security duoc chay cung migrate/build.
- [Docs] Dong bo local FullSpec/M0/deploy docs theo contract moi: M0 = 18 bang, ERP = 90 bang, session noi bo duoc chuan hoa tai M0 va chuan bi san de M9 ke thua ve sau; da sync tiep len cac Notion pages SSOT.

---

## V0.172 (2026-03-09) — Show explicit warning when session has expired

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | DevOps |

- [Auth UX] `security-guard.ts` va `use-security-context.ts` gan `reason=session-expired` khi redirect ve login do session cookie con nhung context khong con hop le.
- [Login] `src/app/auth/login/page.tsx` + `login-client.tsx` them banner canh bao co icon `AlertTriangle` de user nhan biet ro ly do bi day ve trang dang nhap.
- [Verify] `npm run build` pass va browser snapshot local xac nhan banner `Phien dang nhap khong con hieu luc` hien thi dung tren login page.

---

## V0.171 (2026-03-09) — Fail closed when stale sessions would otherwise show partial menus

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | DevOps |

- [Auth] `use-security-context.ts` duoc doi sang shared provider trong `MainLayout` de gom 1 lan tai security context cho toan bo shell va redirect ve `/auth/login` khi `/api/security/me` tra `401`.
- [Session] `src/app/api/security/me/route.ts` chu dong xoa cookie `erp_session` stale khi session khong resolve duoc, tranh trang thai cookie con nhung context null.
- [Guard] `src/lib/security-guard.ts`, `src/app/page.tsx`, va `src/app/dashboard/layout.tsx` duoc harden theo fail-closed de `/` va `/dashboard` khong con render shell thieu menu khi session invalid.
- [Shell] `topbar.tsx` + `sidebar.tsx` doi sang loading state ro rang, khong con de lai UI "Unknown User" / menu partial gay hieu nham.
- [Verify] `npm run build` pass; `curl.exe` smoke test xac nhan `/`, `/dashboard` stale cookie => `307 /auth/login?...` va `/api/security/me` => `401` kem xoa cookie.

---

## V0.170 (2026-03-08) — Align M3 table styling with demo + font consistency

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [M3] `bao-gia-client.tsx` table headers them sort icon ⇅ tren 5 cot (Khach Hang, Ma BG, Gia Tri, Trang Thai, Ngay Tao) giong demo Metronic.
- [M3] Avatar doi tu flat bg + rounded-full sang gradient bg-gradient-to-br + rounded-lg + white text giong demo.
- [M3] Ma bao gia doi tu text-foreground (dam) sang text-muted-foreground (nhat) + font-medium thay vi font-semibold.
- [M3] Font body table cells chuan hoa tu text-sm (14px) sang text-[13px] dong bo voi demo 13px.
- [Verify] `npm run build` pass.

---

## V0.169 (2026-03-08) — Unify border-radius + remove redundant table header

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [Foundation] `filter-bar.tsx` rounded-2xl → rounded-xl de dong nhat voi PageHeader, StatCard, SectionPanel.
- [M3] `bao-gia-client.tsx` filter buttons + search input rounded-xl → rounded-lg (vua phai cho elements nho). Bo summary bar "7 bao gia" du thua trong table card.
- [Verify] `npm run build` pass.

---

## V0.168 (2026-03-08) — Metronic data table implementation for M3/Báo Giá

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [M3] `bao-gia-client.tsx` table section duoc thay the bang Metronic-style data table voi: customer avatar initials, monospace code, dot-style status badges (6 mau), VN currency format, DD/MM/YY date, action dropdown menu (Xem/Sửa/In), empty state, va pagination footer.
- [UI] Card wrapper + filter summary bar + clean uppercase gray headers theo dung Metronic demo13 pattern.
- [Verify] `npm run build` pass va JSX structure da duoc fix sau khi apply edit. Business logic (handleViewDetail, handleEdit, handlePrintBaoGia) giu nguyen.

---

## V0.167 (2026-03-08) — Reset shell surfaces and recompose M0 into shell-native layout

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | UI only |

- [Shell] `globals.css`, `main-layout.tsx`, `topbar.tsx`, `sidebar.tsx`, `page-canvas.tsx` duoc chinh lai de sidebar/topbar/content dung chung mot surface language, khong con cam giac den-gat ben trai va trang-gat ben phai.
- [Home] `src/app/page.tsx` bo lop tint cam o `PageCanvas` de trang chu dung nen neutral cua shell thay vi tu tach mau rieng.
- [M0] `src/app/m0/page.tsx` duoc bo toan bo hero/page wrapper cu va dung lai bang `PageHeader`, `StatCard`, `SectionPanel` de module he thong nam dung trong backbone thay vi tu dung mot landing page rieng.
- [Verify] `npm run build` pass va `ReadLints` khong co loi moi sau shell reset phase A.

---

## V0.166 (2026-03-08) — Tighten homepage card rhythm and reduce overview clutter

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [Home] `src/app/page.tsx` duoc lam gon lai theo nhịp Metronic hon: module cards dong chieu cao, footer metadata/progress thong nhat, va bo bot text lap de grid khong con so le roi.
- [Foundation] `src/components/foundation/section-panel.tsx` va `stat-card.tsx` duoc ha border/shadow mac dinh de overview block bt day, de content shell thong hon.
- [Verify] `npm run build` pass va `ReadLints` khong co loi moi sau homepage visual refinement.

---

## V0.165 (2026-03-08) — Realign shell chrome closer to Metronic backbone

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [Shell] `MainLayout` bo outer content card lon de workspace bt doi thong thoang hon, gan hon voi shell frame cua template thay vi kieu hop-trong-hop.
- [Sidebar] `src/components/layout/sidebar.tsx` duoc lam phang lai theo nav grammar gon hon: bo item dang card, bo dong module code, doi sang dark shell surface va them hover-peek khi collapsed tren desktop.
- [Topbar] `src/components/layout/topbar.tsx` duoc giam do day: bo version khoi header chinh, utility buttons gon hon, va `Shell Settings` doi tu `Sheet` dai sang `Popover` utility panel nhe hon.
- [Foundation] `PageHeader` va `FilterBar` duoc ha border/shadow de page lists khong bi day block nhu truoc.
- [Verify] `npm run build` pass va `ReadLints` khong co loi moi sau shell realignment phase 1.

---

## V0.164 (2026-03-08) — Rename shell presets to real operating roles

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | DevOps |

- [Shell] Doi shell presets tu ten ky thuat (`Balanced`, `Focus`, `Overview`) sang preset theo vai tro dung that: `Owner Review`, `Operator`, `Focus Entry`.
- [Compatibility] Bo sung mapping tu preset cu sang preset moi trong local persistence de may dang dung khong bi mat settings sau khi nang cap.
- [UI] `Shell Settings Panel` va deploy notes da duoc cap nhat wording moi de owner de hieu va chon theo ngu canh van hanh thay vi theo ten mau chung chung.
- [Verify] `npm run build` pass va `ReadLints` khong co loi moi sau khi doi preset labels.

---

## V0.163 (2026-03-08) — Migrate Next 16 middleware convention to proxy

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Data/Schema |

- [Platform] Doi `src/middleware.ts` sang `src/proxy.ts` va doi export `middleware()` thanh `proxy()` theo convention moi cua Next 16.
- [Cleanup] Giu nguyen logic gate session va `config.matcher`, chi lam sach naming/convention de giam canh bao framework va de repo di dung huong stack moi.
- [Verify] `npm run build` pass sau migration va warning `middleware -> proxy` khong con xuat hien trong build output.

---

## V0.162 (2026-03-08) — Add shell presets and overview density controls

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [Shell] Mo rong `AppShellProvider` voi `preset` (`Balanced`, `Focus`, `Overview`, `Custom`) va `overviewDensity`, dong thoi phat CSS variables cho foundation layer va cac overview cards.
- [UI] `Shell Settings Panel` da co them khu `Shell Presets` va `Overview Density`, giup doi nhịp shell nhanh theo muc dich lam viec thay vi chi bat/tat tung toggle rieng le.
- [Foundation] `PageCanvas`, `PageHeader`, `StatCard`, `SectionPanel` va cac overview grids chinh o `/`, `M1`, `M4`, `MF` da doc settings moi de thay doi spacing/card density that.
- [Verify] `npm run build` pass va `ReadLints` khong co loi moi sau rollout shell presets.

---

## V0.161 (2026-03-08) — Add live shell settings panel with local persistence

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | DevOps |

- [Shell] Mo rong `AppShellProvider` de quan ly `density`, `showTopbarSearch`, `showSidebarFooter`, va ghi nho local settings cung trang thai `sidebarCollapsed`.
- [UI] Them `src/components/layout/shell-settings-panel.tsx` va nut mo panel tren `Topbar`, bien backbone thanh mot cong cu tuy chinh shell that ngay trong ERP.
- [UX] `MainLayout` va `Sidebar` da doc settings chung de doi spacing compact/comfortable, an hien search topbar, va an hien footer sidebar ma khong can sua code tay.
- [Verify] `npm run build` pass va `ReadLints` khong phat sinh loi moi sau khi them shell settings panel.

---

## V0.160 (2026-03-08) — Roll compact backbone style to M1 M4 MF and suppress extension hydration noise

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | UI only |

- [UI] Rollout foundation page structure sang `M1`, `M4`, `MF`: doi hero + stat + section cards sang `PageCanvas`, `PageHeader`, `StatCard`, `SectionPanel` de cac module overview di cung mot nhịp compact hon.
- [Shell] `src/app/layout.tsx`: them `suppressHydrationWarning` tren `body` de giam canh bao local khi browser extension chen them attributes truoc React hydrate.
- [Verify] `npm run build` pass sau rollout, xac nhan homepage + M1 + M4 + MF da cung nam trong backbone compact moi.

---

## V0.159 (2026-03-08) — Compact homepage shell and document Metronic backbone deploy basis

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [UI] Giam do day cua foundation layer: `PageCanvas`, `PageHeader`, `SectionPanel`, `StatCard` deu duoc compact lai de giam khoi block va tra lai nhieu khong gian lam viec hon.
- [Home] Tinh gon `src/app/page.tsx`: bo stat card `Version` bi lap, chuyen version thanh chip summary nho, thu gon action buttons, doi `Modules ERP` thanh `Modules`, va giam do nang cua module cards.
- [Shell] Sidebar bo dong `ERP Workspace` du thua de giam nhiễu; them `docs/METRONIC-BACKBONE-DEPLOY-NOTES.md` de ghi ro backbone da tich hop gi, chua co gi, va can verify gi truoc deploy.

---

## V0.158 (2026-03-08) — Move shell state into a shared provider backbone

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | DevOps |

- [Foundation] Tao `AppShellProvider` trong `src/components/layout/` de gom state `sidebarCollapsed`, `mobileSidebarOpen`, va CSS variables cua shell thanh mot nguon dung chung.
- [Shell] Refactor `MainLayout`, `Topbar`, `Sidebar` sang context-based shell: bo truyen props dieu khien layout theo tung cap component, giam do rach giao dien khi rollout them module.
- [Verify] `npm run build` tiep tuc pass sau khi doi shell sang provider backbone; Topbar/Sidebar/Main content da cung dung mot state layer that.

---

## V0.157 (2026-03-08) — Strengthen app shell backbone and remove noisy local shell artifacts

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | General |

- [Foundation] Noi `ThemeProvider` + `TooltipProvider` vao `src/app/layout.tsx` de shell goc bat dau dung provider/config pattern gan hon voi Metronic starter, thay vi chi co UI bridge o tung page.
- [Shell] Don `Topbar`, `Sidebar`, va `MainLayout`: bo nut collapse du thua tren topbar, mo rong search area de dung khong gian lien mach hon, va nen/spacing cua shell gon hon.
- [Dev] Tat `Next.js dev indicator` trong `next.config.ts`; overlay `Route / Builder / Route Info / Preferences` anh chup o local khong phai UI template va se khong con chen vao sidebar khi dev.

---

## V0.156 (2026-03-08) — Roll out responsive foundation page structure to home and M3

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [Foundation] Them `PageCanvas` va `SectionPanel` de khoa nhịp page, spacing, section shell, va responsive structure thanh mot lop dung chung.
- [UI] Refactor `src/app/page.tsx` va `src/app/m3/page.tsx` sang foundation page structure moi: header/action/summary theo `PageHeader`, section wrap theo `SectionPanel`, va content canvas khong con dung cac layout rời rạc tung man.
- [Verify] `npm run build` pass sau rollout, giup `Trang chu`, `M3`, `Bao Gia`, va shell cung di chung mot xương layout.

---

## V0.155 (2026-03-08) — Extract reusable foundation components from Bao Gia pilot

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [Foundation] Them `PageHeader`, `StatCard`, `FilterBar` trong `src/components/foundation/` de gom shell-level UI thanh lop dung chung thay vi hardcode tung man.
- [Pilot] Refactor `m3/bao-gia/bao-gia-client.tsx` sang foundation components cho header, stats, va filter bar; detail panel/workflow van giu nguyen.
- [Verify] `npm run build` tiep tuc pass sau khi tach component dung chung, chot xong phase vendor foundation + pilot Bao Gia.

---

## V0.154 (2026-03-08) — Remove old Bao Gia demo and start live M3 foundation pilot

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [Cleanup] Xoa route demo cu `m3/bao-gia/model-demo`, bo dev entry demo trong `m3/page.tsx`, va don cac label runtime con mang tinh chat demo/placeholder.
- [UI] Cap nhat `Topbar`, `Sidebar`, va homepage wording sang trang thai foundation that (`Foundation Active`), bo placeholder search `UI only`, va cap nhat stack text len `Next 16 + React 19 + Tailwind 4`.
- [Pilot] `m3/bao-gia`: dua header, stats, filter bar, va list wrapper sang foundation shell moi trong khi giu nguyen detail panel/workflow quen tay.

---

## V0.153 (2026-03-08) — Next 16 foundation gate pass and shell bridge

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [Platform] Chot upgrade gate cho `Tailwind 4 + React 19 + Next 16`: doi sang ESLint flat config, sua config build/lint moi, va loai `template/` khoi TypeScript scope de vendor source khong pha app runtime.
- [Shell] Refactor `MainLayout`, `Topbar`, `Sidebar` sang foundation bridge theo huong Metronic: shell dung CSS variables, sidebar mobile/desktop tach rieng, giu nguyen man business children ben trong.
- [Verify] `npm run build` pass tren stack moi sau khi fix type compatibility (`useRef` React 19, Tailwind config typing, tsconfig exclude template).

---

## V0.152 (2026-03-08) — Demo V2 bao gia giu detail panel hien tai

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI Demo] Lam lai route `m3/bao-gia/model-demo` theo huong gan man that: dung `MainLayout` hien tai, bo look glass dam, va giu trai nghiem detail panel quen tay.
- [UX] Demo V2 tap trung vao chuan hoa spacing, filter, table, va responsive; khong de xuat thay doi manh detail workflow nhu ban demo dau.

---

## V0.151 (2026-03-08) — Demo chuan hoa layout chung va model Bao Gia

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | General |

- [UI Demo] Them route `m3/bao-gia/model-demo` de preview layout shell moi + model trang Bao Gia dung data that, khong anh huong flow production hien tai.
- [Navigation] Bo sung dev entry tu trang tong quan `M3` de mo nhanh demo, phuc vu owner review truoc khi rollout code that.

---

## V0.150 (2026-03-07) — Remove noisy final deploy header warning

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | DevOps |

- [DevOps] `scripts/deploy-vps-via-git.ps1`: bo so sanh `[ -n ... ]` o remote shell, doi sang chuoi `curl | awk || curl | awk` de lay `Cache-Control` header gon va on dinh hon.
- [Ops] Hoan thien buoc verify domain/origin cuoi deploy ma khong con warning bash gay nhieu.

---

## V0.149 (2026-03-07) — Fix PowerShell escaping in final deploy verification

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [DevOps] `scripts/deploy-vps-via-git.ps1`: doi `originHeaderCmd` sang here-string de PowerShell khong thuc thi nham `$(...)`/pipe ngay tren may local.
- [Ops] Buoc verify header cuoi deploy co the chay het sau khi app da online, thay vi fail do escape shell.

---

## V0.148 (2026-03-07) — Harden deploy header verification after successful runtime switch

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [DevOps] `scripts/deploy-vps-via-git.ps1`: sua buoc doi chieu header domain/origin de khong crash khi output rong va tu fallback `127.0.1.1:3000` -> `127.0.0.1:3000`.
- [Ops] Sau khi runtime standalone + PM2 online, script deploy khong con fail gia o buoc verify cuoi.

---

## V0.147 (2026-03-07) — Unblock release build from legacy lint debt

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Bug fix |

- [DevOps] Dong bo `next.config.ts` va `next.config.mjs`: bat `eslint.ignoreDuringBuilds = true` de build production khong bi chan boi cac loi lint cu ngoai pham vi bundle release hien tai.
- [Deploy] Giu `npm run lint` lam quality gate rieng, con luong GitHub -> VPS tiep tuc build/deploy duoc cho cac fix cap module.

---

## V0.146 (2026-03-07) — Sync governance addendum across 5 rule files

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Governance |

- [Governance] Sync block `Cursor Cloud specific instructions` vao `.cursorrules` va `.antigravityrules` de khop voi addendum trong `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`.
- [Ops] `check:governance` khong con fail vi addendum lech giua rule files va agent files.

---

## V0.145 (2026-03-07) — Sync governance files sau thay doi AGENTS

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | Governance |

- [Governance] Sync block `Cursor Cloud specific instructions` tu `AGENTS.md` sang `CLAUDE.md` va `GEMINI.md` de khoi phuc trang thai governance dong bo.
- [Ops] `npm run check:governance` khong con fail vi lech 3 file agent rules, mo duong cho release doctor va deploy preflight.

---

## V0.144 (2026-03-07) — Release doctor khong chet som vi dev server

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | DevOps |

- [DevOps] `scripts/release-final.ps1`: them `doctor mode` de canh bao process dang dung port 3000 thay vi throw ngay, giup scan tiep blocker release that.
- [Ops] Van giu release that an toan: neu khong phai doctor mode thi script van chan deploy khi dev server dang chay, tru khi dung `-ForceStopDev`.

---

## V0.143 (2026-03-07) — VPS deploy chay standalone runtime that

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Governance |

- [DevOps] Chot script deploy GitHub -> VPS va zip deploy cung activate `scripts/vps-activate-standalone.sh` de PM2 chay `node .standalone-run/server.js`.
- [Docs] Dong bo CODE-AND-DB-FINAL, DEPLOY-VPS-AND-SYNC, RUNBOOK, VPS-FIRST-TIME-SETUP, DB-PUSH-TO-VPS-AND-SYNC, deploy skill/checklist theo runtime standalone that.
- [Ops] Loai bo wording cu `pm2 restart`/`next start` o cac tai lieu deploy de GitHub va VPS cung theo mot chuan van hanh duy nhat.

---

## V0.142 (2026-03-04) — MF smoke automation and topbar logo cleanup

| Field | Value |
|-------|-------|
| **Category** | Auth/Security |
| **Scope** | Data/Schema |

- [MF] Them `npm run verify:mf` de smoke schema/store/runtime cho `phieu_thu`, `cong_no`, va `/api/mf/stats` ma khong phu thuoc browser session.
- [UI] Sua topbar logo Tan Phat theo `next/image fill` container co dinh de tranh warning resize trong smoke/browser console.
- [Verify] `npm run verify:mf` va `npm run build` deu pass sau khi them smoke automation.

---

## V0.141 (2026-03-04) — MF persisted receipts and receivables hardening

| Field | Value |
|-------|-------|
| **Category** | M7 Tiền Lương |
| **Scope** | Data/Schema |

- [MF] Chuyen `phieu_thu` sang DB that + actor audit, bo client mock options, dong bo workflow duyet/thu tien voi persisted path.
- [MF] Chuyen `cong_no` page/actions/stats sang DB that; bo mapping lech giua schema live va UI bang id/loai_chung_tu_nguon generic + audit emails.
- [Data] Migration additive mo rong `phieu_thu` va `cong_no` de giu lien ket, doi tuong, audit, va chi muc phuc vu van hanh dai han.
- [Verify] `npm run migrate:m7`, `npm run build`, `npm run verify:m7` deu pass sau hardening.

---

## V0.140 (2026-03-04) — M7 production hardening with persisted payroll vouchers

| Field | Value |
|-------|-------|
| **Category** | M7 Tiền Lương |
| **Scope** | Governance |

- [M7] Freeze payroll contract theo Notion MCP + DB thuc te: normalize legacy `dang_lam_viec` -> `chinh_thuc`, chot TNCN luy tien toi da 35%.
- [M7] Them payroll readiness gate: chan tinh luong/tao phieu chi khi thieu luong co ban, cham cong locked, hoac bank info.
- [M7] Loai bo actor placeholder `current-user`; server actions lay actor dang dang nhap va tu dong ghi audit.
- [MF] Mo rong `phieu_chi` de luu linkage/audit payroll that, tao phieu chi persisted cho tung payroll item va doi soat trang thai chi luong tu chung tu `paid`.
- [Docs] Dong bo governance 5-way sync uu tien Notion MCP, bat buoc version + changelog cho M7 hardening.

---

## V0.139 (2026-03-04) — One-command sync final (GitHub -> VPS -> health-check)

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Bug fix |

- [DevOps] Them `scripts/sync-final.ps1`: tu dong pull --rebase, push main, deploy VPS, check HTTPS domain va port 3000 tren VPS.
- [NPM] Them script `npm run sync:final` de van hanh 1 lenh duy nhat sau moi lan code/fix.
- [Docs] Cap nhat runbook voi huong dan one-command full sync va dieu kien an toan (main + clean working tree).

---

## V0.138 (2026-03-04) — Fix deploy VPS fail do tailwind.config.js placeholder

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Bug fix |

- [Fix] Xoa `tailwind.config.js` bi loi (`export default __CONFIG__`) gay crash build tren VPS.
- [Config] Dung duy nhat `tailwind.config.ts` lam nguon cau hinh Tailwind de tranh conflict js/ts.
- [Ops] Re-deploy theo workflow GitHub -> VPS de dong bo code final.

---

## V0.137 (2026-03-04) — Runbook thao tac deploy/db sync theo [LOCAL]/[VPS]/[AAPANEL]

| Field | Value |
|-------|-------|
| **Category** | M1 Danh Mục |
| **Scope** | Governance |

- [Docs] Them RUNBOOK-DEPLOY-DB-SYNC-OPS.md: checklist thao tac copy/paste theo vi tri con tro, quy trinh deploy code hang ngay, sync DB VPS<->Local, va canh bao PowerShell phai dung curl.exe.
- [Docs] CODE-AND-DB-FINAL.md, DEPLOY-VPS-AND-SYNC.md: them link den runbook moi de vao dung tai lieu thao tac.
- [Ops] Ghi nhan su co thuc te va cach xu ly: thieu bang dm_nhom_universal tren VPS => can sync Local->VPS + backup truoc import + deploy lai.

---

## V0.131 (2026-02-27) — Rà soát dung lượng repo + archive/backup

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | DevOps |

- [Repo] .gitignore: thêm BACKUPS/, backup/, backup-* (pattern), backup-root-*, BACKUP_* (pattern), backup-vps-to-local.sql, .agent/, .backup/, .cursor/, .claude/ để repo không chứa backup/agent.
- [DevOps] create-deploy-zip.mjs: exclude thêm backup-root-*, backup-pre-*, BACKUP_*, backup-vps-to-local.sql, .agent, .backup, .claude khi tạo zip deploy.
- [Docs] docs/REPO-SIZE-AND-ARCHIVE.md: rà soát dung lượng (BACKUPS ~3.3 GB, backup-root-* ~163 MB, docs ~628 MB); hướng dẫn archive/backup ra ngoài repo; checklist duy trì repo nhẹ.

---

## V0.136 (2026-02-27) — Script dùng đúng thông số GitHub + VPS (irissnss/tanphat-erp, /www/wwwroot/tanphat-erp, DB tanphat-erp)

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Governance |

- [Config] .env.deploy.example: VPS_PATH=/www/wwwroot/tanphat-erp, VPS_DB_USER/NAME=tanphat-erp, thêm GITHUB_REPO; comment thông số chuẩn.
- [DevOps] deploy-and-setup-vps.ps1, deploy-to-vps.mjs: default VPS_PATH /www/wwwroot/tanphat-erp; .env.local trên VPS: DB_USER/DB_NAME=tanphat-erp.
- [DevOps] vps-app-only.sh, vps-pull-and-restart.sh: path /www/wwwroot/tanphat-erp, DB_NAME/DB_USER=tanphat-erp; REPO_URL mẫu git@github.com:irissnss/tanphat-erp.git.
- [DevOps] migrate-vps-to-local.ts: default VPS_DB_USER/VPS_DB_NAME=tanphat-erp.
- [Docs] DEPLOY-VPS-AND-SYNC.md: bảng thông số chuẩn (GitHub, VPS path, proxy, MySQL) để script tham chiếu.

---

## V0.135 (2026-02-27) — Dọn dự án sạch gọn, chuẩn bị push lên GitHub (dưới 2 GB)

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | DevOps |

- [Repo] .gitignore sửa đúng chuẩn (chỉ pattern; thêm backup-*, .restore-snapshot-backup, .cursor, .agent, desktop.ini, pattern smart-offset-master).
- [DevOps] cleanup-repo-disk.ps1: chuyển thêm .restore-snapshot-backup và mọi thư mục backup-* / backup-root-* ra archive.
- [DevOps] clean-project-for-github.ps1: script tổng hợp — chạy cleanup, đo .git, in hướng dẫn push (nếu .git ≥ 1.5 GB gợi ý lịch sử sạch); npm run clean:for-github.
- [Docs] REPO-SIZE-AND-ARCHIVE.md, GIT-PUSH-UNDER-2GB.md: bước 0 chạy clean:for-github; REPO-SIZE cập nhật mô tả .gitignore và cleanup.

---

## V0.134 (2026-02-27) — Workflow Local → GitHub → VPS (có lịch sử truy vết) + tự động hóa

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Governance |

- [DevOps] Workflow khuyến nghị: đưa code lên GitHub rồi từ GitHub đưa về VPS; một lệnh từ máy anh: npm run deploy:vps:git (SSH + pull + build + pm2 restart).
- [DevOps] scripts/vps-pull-and-restart.sh: chạy trên VPS — git fetch + checkout -B main origin/main, npm ci, npm run build, pm2 restart.
- [DevOps] scripts/deploy-vps-via-git.ps1: đọc .env.deploy, SSH vào VPS chạy vps-pull-and-restart.sh; package.json thêm script deploy:vps:git.
- [Docs] CODE-AND-DB-FINAL.md, DEPLOY-VPS-AND-SYNC.md: workflow GitHub-first; chuẩn bị lần đầu (VPS cần clone/convert sang git); runbook deploy:vps:git.

---

## V0.130 (2026-03-03) — Fix deploy treo ở bước nén + preflight + VPS_PATH + smoke check

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | Bug fix |

- [DevOps] create-deploy-zip.mjs: Promise stream finish/error trước finalize(); archive.on("error"); log tiến độ mỗi 500 entry; exclude mạnh (.cursor, .env, .turbo, ...).
- [DevOps] deploy-and-setup-vps.ps1: preflight VPS_HOST/VPS_USER/VPS_KEY (kiểm tra file key tồn tại), cảnh báo TANPHAT_DB_PASSWORD; remote dùng VPS_PATH (VPS_PARENT/VPS_LEAF); smoke check pm2 status + curl 127.0.0.1:3000.
- [Docs] DEPLOY-VPS-AND-SYNC.md: runbook lần đầu vs lần sau; checklist khi 502.

---

## V0.129 (2026-02-27) — Deploy VPS một lệnh (full setup từ máy anh)

| Field | Value |
|-------|-------|
| **Category** | DevOps |
| **Scope** | DevOps |

- [DevOps] deploy-and-setup-vps.ps1: một lệnh từ Windows (npm run deploy:vps:full) — nén code, scp lên VPS, cài Node 20 + PM2, tạo .env.local, build, pm2 start.
- [DevOps] create-deploy-zip.mjs: script chỉ tạo zip để dùng trong deploy full.

---

## V0.128 (2026-02-27) — Audit Webapp Hoàn Thiện (plan): metadata M5/M8, G2 form, backup dọn, alert→toast

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | General |

- [Data] module-metadata: thêm m5Metadata, m8Metadata và đăng ký vào allModuleMetadata; trang chủ hiển thị đúng tiến độ M5/M8.
- [UX] G2: M5 nha-cung-cap (useFormDirtyTracker, useSafeClose, ConfirmExitDialog, markDirty trên form); M4 lenh-san-xuat Sheet tạo LSX (G2 tương tự).
- [UX] Title Case: DialogTitle xác nhận xóa trong nha-cung-cap dùng toVietnameseTitleCase.
- [Cleanup] Xóa file backup/legacy: vat-tu-client.tsx.backup, .backup.20260125, .clean; danh-muc-labels.ts.backup.20260125.
- [UX] Print: lsx-print-template.ts, pdi-print-template.ts thay alert() bằng toast.error() khi popup bị chặn.

---

## V0.127 (2026-02-09) — Trang chủ: tiến độ & số bảng lấy từ module-metadata (SSOT)

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | General |

- [UI] page.tsx: buildModules() dùng allModuleMetadata; số bảng = sum groups[].tables.length, tiến độ = completionPercentage, status Ready/In Development/Planned; thêm badge Ready (xanh), chip Ready trong header; fallback M5,M6,M7,M8,M9 khi chưa có metadata.

---

## V0.126 (2026-02-09) — Tên module tiếng Việt: M0→Hệ Thống, M1→Danh Mục, M3→CRM & Bán Hàng, M4→Sản Xuất, M6→Nhân Sự, M7→Tiền Lương, M8→Công Việc & Quy Trình, M9→Cổng Thông Tin

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | General |

- [UI] menu-registry + trang chủ + sidebar (tất cả module hiển thị chỉ entry.label, bỏ dòng nhỏ) + trang tổng quan từng module + module-metadata title.

---

## V0.125 (2026-02-09) — M5 Kho Hàng: tên hiển thị "Kho Hàng" tương tự MF

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | General |

- [UI] Menu/Sidebar/Trang chủ/Trang M5: đổi "Warehouse" → "Kho Hàng"; sidebar hiển thị tên chính "Kho Hàng", bỏ dòng nhỏ cho M5; header /m5 là "Kho Hàng", badge "Phase 1 - Skeleton", "Luồng Nghiệp Vụ Kho Hàng".

---

## V0.124 (2026-02-09) — MF Tài Chính: sublink sidebar + tên hiển thị "Tài Chính"

| Field | Value |
|-------|-------|
| **Category** | MF Tài Chính |
| **Scope** | General |

- [UI] Sidebar: thêm sublink MF (Phiếu Thu, Phiếu Chi, Công Nợ, Ngân Hàng, Đối Chiếu); hiển thị tên chính "Tài Chính" thay vì "MF", bỏ dòng nhỏ cho MF.
- [UI] Trang /mf: title header "Tài Chính" thay "MF"; subtitle "Tiến độ module Tài Chính".

---

## V0.123 (2026-02-09) — MF: Trang tổng quan Module MF (Finance) đồng style M0/M1

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | UI only |

- [UI] Redesign /mf: premium header gradient emerald/teal, 4 stats cards (AR, AP, Thu Tháng, Chi Tháng), progress section, 4 nhóm chức năng (Phiếu Thu, Phiếu Chi, Công Nợ, Ngân Hàng) với color-coded cards + DEV ENTRY, section Blockers.

---

## V0.122 (2026-02-08) — M5: Trang tổng quan Module M5 (Warehouse)

| Field | Value |
|-------|-------|
| **Category** | M5 Kho Hàng |
| **Scope** | UI only |

- [UI] Thay ModulePlaceholder bằng trang tổng quan: header gradient amber/orange, 2 card (Luồng NCC→Mua Hàng→Giao Hàng, Key Feature Kho & Xuất), 3 nhóm chức năng (Nhà Cung Cấp, Mua Hàng, Giao Hàng) với nút Mở Quản Lý, section Luồng Nghiệp Vụ.

---

## V0.121 (2026-02-08) — Fix M0 Universal Vietnamese labels encoding

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | Bug fix |

- [DB] Re-backfill 103 rows with correct UTF-8 Vietnamese labels (12/12 categories OK)
- [CODE] danh-muc-labels.ts: +3 hardcode entries, +getUniqueDanhMucWithLabels(), +getDbLabelForDanhMuc()
- [CODE] dm-nhom-universal-client.tsx: 7 locations now pass DB label to getDanhMucLabel()
- [FIX] Root cause: previous backfill used wrong charset; PowerShell pipe encoding corruption

---

## V0.120 (2026-02-08) — M3 CRM: header lớn gộp chung, searchbox khách, bỏ lọc nhật ký theo khách

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | General |

- [CRM] Một header lớn: Title + ô Khách Hàng (SearchableSelect) + Tạo Việc + Thêm Nhật Ký + Mở M8; search nhật ký hiện khi tab Nhật Ký
- [CRM] Tab Nhật Ký: hiển thị tất cả nhật ký (nhatKyData), bỏ lọc theo khách và gợi ý; badge = tổng số nhật ký
- [CRM] Tab Việc Cần Làm: CrmTaskPanel hideHeader; CTA Tạo Việc/Mở M8 đưa lên header; ref openCreateDialog từ header
- [UI] Thay Select khách bằng SearchableSelect (tìm mã/tên, normalize không dấu)

---

## V0.119 (2026-02-08) — M3 CRM: khách hàng dùng chung cho cả 2 tab (lọc nhật ký theo KH)

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | General |

- [CRM] Nhật ký tab: lọc theo khách hàng đang chọn (nhatKyFilteredByCustomer); badge đếm theo bộ lọc
- [CRM] Gợi ý ngắn: "Đang hiển thị nhật ký theo khách hàng đã chọn" / "Chọn khách hàng phía trên để lọc..."

---

## V0.118 (2026-02-08) — M3 CRM: tách Việc Cần Làm / Nhật Ký Chăm Sóc thành 2 tab

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | Logic |

- [UI] CRM trang chính dùng Tabs: "Việc Cần Làm (M8)" | "Nhật Ký Chăm Sóc" (badge đếm từng tab)
- [UI] Bỏ layout 2 section chồng + divider "▼ Đã làm ▼"; Legacy "Lịch Sử CSKH" nằm trong tab Nhật Ký
- [SCOPE] Chỉ UI layout; logic M8 embed, Customer selector, Sheet không đổi

---

## V0.117 (2026-02-08) — M3 Đơn hàng: card cam đậm + xanh lá phân biệt rõ

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI] Card Tổng quan: cam đậm (from-orange-100 to-orange-200, border-l-orange-600). Card Công nợ: xanh lá (from-green-50 to-emerald-200, border-l-green-600).

---

## V0.117 (2026-02-08) — M4: LSX + PDI status workflows

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | UI only |

- [FEAT] updateLSXStatus with guarded transitions (trang_thai, trang_thai_in, trang_thai_sx)
- [FEAT] getAvailableTransitions helper for LSX UI buttons
- [FEAT] updatePDIStatus with guarded transitions (draft → da_dieu_in → hen_lich_in → da_in_hoan_tat)
- [FEAT] getAvailablePDITransitions helper for PDI list inline buttons
- [UI] Workflow action card on LSX detail page
- [UI] Action column with inline buttons on PDI list page

---

## V0.116 (2026-02-08) — M4 PDI: Fix mojibake trong actions.ts

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | Bug fix |

- [FIX] 50+ dòng mojibake → UTF-8 đúng: error messages, validation, comments
- [SCOPE] Encoding-only fix, zero logic changes
- [VERIFY] grep "Ã¡|Ã©|Ã³" → 0 results; tsc --noEmit m4/ → 0 errors

---

## V0.115 (2026-02-08) — M3 Đơn hàng: card tổng quan radian màu nổi bật

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI] Hai card đổi sang gradient (from-orange-50 to-amber-100 / from-amber-50 to-yellow-100), viền trái đậm (border-l-orange-500, border-l-amber-500), viền + shadow-lg.

---

## V0.114 (2026-02-08) — M3 Đơn hàng: card tổng quan style giống Báo giá

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI] Hai card (Tổng quan, Công nợ) đổi sang layout giống Báo giá: nhỏ gọn (py-3 px-4, icon 8x8), tối đa 2 dòng, bỏ Card wrapper, dùng div + border-l-4 + rounded-xl.

---

## V0.113 (2026-02-08) — M3 Báo giá: card nhỏ gọn, nội dung 2 dòng

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI] Năm card stats thu nhỏ (py-3 px-4, icon 8x8), nội dung tối đa 2 dòng: dòng 1 = icon + "Label · count", dòng 2 = tổng tiền. Bỏ decorative circles.

---

## V0.112 (2026-02-08) — M3 Card: bo góc nhẹ + radian màu sắc

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI] Đơn hàng: card tổng quan rounded-3xl → rounded-xl (bo góc nhẹ), thêm border-l màu (primary/amber) + nền nhạt (primary/5, amber-50/50)
- [UI] Báo giá: 5 card stats rounded-2xl → rounded-xl, thêm border-l màu theo từng trạng thái (amber/blue/emerald/rose); card Tổng dark thêm border slate
- Filter bar Đơn hàng + Báo giá: rounded-2xl → rounded-xl đồng bộ

---

## V0.111 (2026-02-08) — M3 Đơn Hàng: radian card tổng quan nổi bật

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | UI only |

- [UI] Hai card tổng quan (Tổng quan, Công nợ còn lại) đổi rounded-2xl → rounded-3xl để góc bo rõ, nổi bật (ghi chú ảnh)
- Phạm vi: UI only. Logic/Data/Integration: không đổi. Không ảnh hưởng dữ liệu thật.

---

## V0.110 (2026-02-08) — M4 Phase 1 E2E Verification + Scan

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | Bug fix |

- [E2E] Merge test: LSXTP0802260003 (SL=200) from 0001+0002 — old LSX cancelled
- [E2E] Split test: LSXTP0802260004 (SL=80) from 0003 — proportional source items (40+40 vs 60+60)
- [SCAN] All 11 M4 files scanned: no more `ten_cong_ty` SQL errors
- [SCAN] phieu-dieu-in/actions.ts: 50+ mojibake lines in user-facing error messages (Phase 2 fix needed)
- [PLAN] Phase 2/3 roadmap created: PDI mojibake fix, status workflow, RBAC, production dashboard

---

## V0.109 (2026-02-08) — M3 Đơn Hàng: style & section tham khảo Báo giá

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | General |

- Detail: header gradient (amber/orange), Thao tác nhanh grid 3 cột (Sửa, Xác nhận, Bắt đầu SX, ...), sections Sản Phẩm / Tài Chính / Thông Tin (rounded-xl, header gradient)
- Form Sheet: header gradient, section Thông Tin Chung (amber), section Sản Phẩm (purple), Lưu ý IMMUTABLE
- Trang: header premium + nút Tạo Đơn Hàng gradient; bảng header gradient; row selected border-l-orange-500; cột Khách hàng hiển thị tên + mã

---

## V0.108 (2026-02-08) — M8 UI: trực quan tổng quát

| Field | Value |
|-------|-------|
| **Category** | M8 Công Việc |
| **Scope** | General |

- [M8 Home] Gradient hero Indigo (palette M8) + Quick Stats (Quá hạn / Hôm nay / Sắp đến / Tổng task)
- [M8 Tasks] Sticky table header + max-h-[60vh] scroll body (master-list-data-table)
- [M8 Tasks] "Clear all" → "Xóa bộ lọc"; Trạng thái/Ưu tiên hiển thị tiếng Việt (Chờ xử lý, Đang làm, Khẩn cấp, Cao...)
- [M8 Tasks] Link "← M8" trong header Task Hub; Detail drawer dùng statusLabels/priorityLabels

---

## V0.107 (2026-02-08) — P4: Gộp/Tách Lệnh SX

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | General |

- [M4] mergeLSX(lsx_ids): gộp ≥2 LSX cùng don_hang_item_id + draft → LSX mới, LSX cũ → cancelled
- [M4] splitLSX(lsx_id, so_luong_tach): tách LSX draft thành 2 LSX theo tỷ lệ items
- [M4] Nút "Gộp LSX" trên trang danh sách (checkbox chọn LSX draft cùng don_hang_item)
- [M4] Nút "Tách LSX" trên trang chi tiết (Dialog nhập số lượng tách)

---

## V0.106 (2026-02-08) — P3: Tạo Phiếu Điều In từ LSX

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | Data/Schema |

- [DB] Migration: lenh_san_xuat_id, lenh_san_xuat_item_id vào m4_phieu_dieu_in
- [M4] createPhieuDieuInFromLSXItem: tạo phiếu từ LSX item (vat_tu ∈ nhom_giay_in)
- [M4] Nút "Phiếu Điều In" trên LSX detail cho item có is_giay_in
- [M4] getLSXWithItems: thêm is_giay_in (EXISTS nhom_giay_in)

---

## V0.105 (2026-02-08) — P2: M4 Overview thay placeholder

| Field | Value |
|-------|-------|
| **Category** | M4 Sản Xuất |
| **Scope** | General |

- [M4] Replaced ModulePlaceholder with premium overview: stats, Dev Entry links
- [M4] 2 groups: Lệnh Sản Xuất, Phiếu Điều In
- [META] Added m4Metadata to module-metadata.ts

---

## V0.104 (2026-02-08) — P1: Nút Tạo Lệnh SX từ Đơn hàng

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | General |

- [M4] Added "Tạo Lệnh SX" button on LSX list page + Sheet chọn don_hang_item
- [M3] Added "Tạo Lệnh SX" button per item in Đơn hàng detail panel (confirmed/in_production/waiting_material)
- [M4] listDonHangItemsForLSX, createLSXFromDonHangItem already existed; wired to UI

---

## V0.103 (2026-02-07) — PLAN-20260206-db-driven-labels COMPLETE

| Field | Value |
|-------|-------|
| **Category** | Governance |
| **Scope** | Governance |

- [DB] Added ten_danh_muc_hien_thi column to dm_nhom_universal
- [DB] Backfilled 106 records with 13 Vietnamese labels (0 NULL)
- [LOGIC] getDanhMucLabel() 3-tier fallback: DB → Hardcode → Auto-generate
- [UI] Added "Tên Hiển Thị Danh Mục" input field in form
- [CLEANUP] Deleted legacy mock data file for dm_nhom_universal (100% DB-driven)
- [CLEANUP] Deprecated sync functions in dm-nhom-helpers.ts
- [FILES] Modified: dm-nhom-universal.ts, m0-dm-nhom-universal-store.ts, danh-muc-labels.ts, dm-nhom-universal-client.tsx, page.tsx, dm-nhom-helpers.ts (deprecated stubs)
- [VERIFIED] 106 records, 13 categories, 0 NULL labels

---

## V0.102 (2026-02-07) — PLAN-20260202-M3-M8-INTEGRATION P1/P2

| Field | Value |
|-------|-------|
| **Category** | M3 CRM & Bán Hàng |
| **Scope** | Data/Schema |

- [P1] Added 4 indexes for dashboard query optimization:
  - bao_gia_option.idx_is_selected - Dashboard selected options filter
  - don_hang.idx_con_lai - Debt queries (con_lai > 0)
  - bao_gia.idx_khach_trang_thai - Customer + status composite
  - bao_gia.idx_trang_thai - Status filtering
- [P2] Dropped deprecated table cskh_task (0 rows, no FK deps)
- [FIX] Removed 3 runtime code references to cskh_task:
  - api/m3/stats/route.ts - Removed COUNT query
  - api/m3/stats/stream/route.ts - Removed from tables array
  - lib/m3-store.ts - listCskhTask now returns empty array
- [VERIFIED] EXPLAIN shows indexes in possible_keys

---

## V3.33.1 (2026-01-18)

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | Bug fix |

- Dashboard Premium Redesign
- Fixed button type warnings across M0, M1, M3

---

## V3.33.0 (2026-01-18)

| Field | Value |
|-------|-------|
| **Category** | M0 Hệ Thống |
| **Scope** | General |

- Initial UI Skeleton
- M0, M1 modules layout

---

