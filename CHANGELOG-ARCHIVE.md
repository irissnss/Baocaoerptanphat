# 📜 Changelog Archive — V0.99 → V0.00

> Lịch sử cũ hơn, trích xuất 100% từ ersion.ts.

---

## V0.99 (2026-02-01) — PLAN-20260201-tier-pricing-complete Phase 3A+3B

**Category:** M3 CRM & Bán Hàng

- [PL5] Duplicate Dialog Fix: Removed "Tạo Giá Mới" workaround
  - Dialog now shows only 2 buttons: "Hủy" / "Sửa Giá Hiện Tại"
  - Removed date picker for future pricing (prevents duplicate creation)
  - Clicking "Sửa Giá Hiện Tại" navigates to edit mode of existing record
- [PL7] TierBuilder UX Expansion:
  - Full-width layout (w-full, table-fixed)
  - Tier badges: "Tier 1", "Tier 2" in styled badges
  - Improved column sizing (min-w-[100px] to min-w-[200px])
  - Better input field sizing (h-10, font-mono)
  - Row hover effects + visual separation
- Files: bang-gia-cong-doan-client.tsx, TierBuilder.tsx

---

## V0.98 (2026-01-31) — Phase 2 Tier Pricing

**Category:** M3 CRM & Bán Hàng

- [PL1-PL7] Tier pricing implementation complete
  - TierBuilder component
  - Smart filter UI
  - Duplicate validation
  - Radio toggle simple/tier

---

## V0.97 (2026-01-31)

**Category:** M1 Danh Mục

- [FIX] M1.4 Bảng Giá Công Đoạn - Toast Fix (T2+T3 PASS):
  - form noValidate: bỏ native HTML validation, dùng handleSubmit validation
  - Removed required attrs từ cong_doan_id, don_vi, gia_theo_don_vi_tinh
  - markDirty() khi auto-clear Công đoạn
  - All 8 tests PASS: T1-T8

---

## V0.96 (2026-01-31)

**Category:** M1 Danh Mục

- [FIX] M1.4 Bảng Giá Công Đoạn - Option C Extended:
  - UOM Invalid Fix: Backend validate bằng ma_uom thay vì ten_uom
  - Mismatch Handling: Đổi Nhóm CN -> auto-clear Công đoạn + warning message
  - Controlled State: cong_doan_id giờ là controlled component
  - Test Results: T1/T4/T5/T6/T7 ALL PASS

---

## V0.95 (2026-01-31)

**Category:** M1 Danh Mục

- [FIX] M1.4 Bảng Giá Công Đoạn - Option C Implementation:
  - Root cause: UI Option 1 gating bị kẹt do default "all" + toast silent
  - Fix: Remove disabled condition cho cong_doan_id field
  - Fix: Nhóm CN = optional filter (không bắt buộc)
  - Fix: Thêm ToastContainer để hiển thị validation errors
  - LOCKED: Backend Option 2 derive nhom_cong_nghe_id (không đổi)

---

## V0.77 (2026-01-30)

**Category:** M3 CRM & Bán Hàng

- [UI] Addon + Param Dialog Redesign V0.77:
  - Addon Dialog: Gradient header, 3 section cards (Thông Tin, Ánh Xạ, Tham Số)
  - Param Dialog: Gradient header, 4-5 section cards (Định Danh, Kiểu Dữ Liệu, Giới Hạn, Tùy Chọn, Trạng Thái)
  - 100% Vietnamese localization - removed all bilingual text
  - Footer: compact layout with "Xem JSON", "Hủy", "Tạo Mới"/"Cập Nhật" buttons
  - NO logic/field/ID changes - UI only

---

## V0.76 (2026-01-30)

**Category:** M3 CRM & Bán Hàng

- [UI] P0 PIXEL-LOCK to Demo HTML:
  - Tabs: Gradient active styling + border-b-2 indicator
  - Search: Added Search icon, width w-56
  - Buttons: Changed from green to blue (bg-blue-600)
  - Layout: Glass container wrapper with p-3 padding
  - All 6 tabs standardized: Blueprints, Addons, Formulas, Params, Bindings, Tests
  - NO logic/field/ID changes - UI only

---

## V0.75 (2026-01-30)

**Category:** M3 CRM & Bán Hàng

- [UI] Pricing Admin Pagination V0.75:
  - Page size options added
  - Prev/Next navigation
  - Row counter display

---

## V0.73 (2026-01-30)

**Category:** M3 CRM & Bán Hàng

- [UI] Pricing Admin Pagination V0.73:
  - Client-side pagination for all 4 tabs (Blueprints, Addons, Formulas, Params)
  - Page size options: 25 / 50 / 100 (default: 50)
  - Prev/Next navigation buttons
  - Page indicator: "1 / 3" format
  - Row counter: "Hiển thị 1-50 / 127 dòng"
  - Auto-reset to page 1 after create/update
  - NO logic/field/ID changes - UI only

---

## V0.72 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Formula Dialog Redesign V0.72:
  - Việt hóa 100%: Header, 6 Tabs, Settings labels, Footer buttons
  - Flow diagram compact (Quy Trình: 1→5)
  - Width: max-w-7xl, Height: h-[90vh]
  - Labels Việt: Mã, Nhóm Công Nghệ, Tỷ Lệ Lợi Nhuận
  - Mappings/Equations headers Vietnamese only
  - NO logic/field/ID changes - UI only

---

## V0.71 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Addon + Param Dialog Redesign V0.71:
  - R1: Việt hóa 100% (bỏ song ngữ)
  - Addon Dialog: max-w-5xl, 3 tabs (Thông Tin/Ánh Xạ/Tham Số)
  - Param Dialog: max-w-4xl, compact header
  - All labels, buttons, table headers Vietnamese only
  - NO logic/field/ID changes - UI only

---

## V0.70 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Tinh Gia Admin V0.70 Localization:
  - R1: 4 section headers compact (py-3, text-base, gap-1)
  - R1: Button text '+ Tạo Mới' (not 'Create New')
  - R1: Descriptions VN ngắn gọn (not EN paragraphs)
  - R2: 39 table columns Việt hóa (ID→Mã, Config→Cấu Hình, Actions→Thao Tác...)
  - Tabs: Blueprint, Addon, Formula, Params

---

## V0.69 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Dialog: Thêm Tham Số button demo-like styling
  - R1: Header Hủy button bg-white/10 border-white/50 (visible)
  - R2: Header py-2, icon w-9h-9, title text-lg
  - R3: Flow diagram super compact w-5h-5, text-xs, gap-1
  - R4: Cards p-2.5 gap-2, badge w-5h-5, gap-1.5
  - R5: Footer py-2
  - R6: Dialog 95vh
  - R7: Subtitle gộp 1 dòng
  - NO logic/field/ID changes - UI only

---

## V0.67 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Dialog V2.3: Layout Optimization
  - R1: Header compact py-4 → py-3
  - R2: Footer compact py-4 → py-3
  - R3: Cards compact p-4 → p-3, gap-4 → gap-3
  - R4: Dialog width max-w-6xl → max-w-7xl
  - R5: Button visibility bg-slate-700/50 + contrast
  - R6: Flow diagram pb-4 → pb-2
  - NO logic/field/ID changes - UI only

---

## V0.66 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Dialog V2.2: Full Demo Match
  - R4: Subtitle opacity text-blue-100/80 và /60
  - R6: Labels revert to text-slate-400
  - R9: "+ Thêm Tham Số" button visible (border-slate-600 text-slate-300)
  - R9: Table header bg-slate-700/50, text-slate-300 font-medium
  - R9: Table cells text-slate-200/400, badge bg-slate-600
  - R12: Footer Hủy button border-slate-600 text-slate-300
  - NO logic/field/ID changes - UI only

---

## V0.65 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Dialog V2.1: Typography fixes for readability
  - R1: Header Lưu button → shadow-md
  - R2: Header Hủy button → text-white + font-medium (was text-white/80)
  - R3: Footer Hủy button → text-slate-200 + font-medium (was text-slate-300)
  - R5: All labels → text-slate-300 (was text-slate-400)
  - R6: Header subtitles → text-blue-100/200 (was blue-100/60-80)
  - NO logic/field/ID changes - UI only

---

## V0.64 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Dialog V2.0: Full Demo HTML Style
  - Gradient header blue (bg-gradient-to-r from-blue-600 to-blue-700)
  - 4-step flow diagram: Thông Tin → Liên Kết → Chi Tiết → Xem Trước
  - 2-column section cards (Thông Tin Cơ Bản + Liên Kết & Trạng Thái)
  - Section 3: Chi Tiết Kỹ Thuật (BlueprintEditorTabs)
  - Section 4: Preview box amber (Xem Trước Kết Quả)
  - Footer: Gradient save button + Eye icon for JSON
  - Max width: 5xl → 6xl
  - NO logic/field/ID changes - UI only

---

## V0.63 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Dialog V1.4: Compact header + footer scroll fix
  - Header: p-5 → px-4 py-3, icon w-6 → w-5, text-xl → text-lg
  - Body: overflow-auto → overflow-y-auto + min-h-0, p-6 → p-4
  - Footer: p-4 → px-4 py-3 (fit small windows)
  - NO logic/field/ID changes - UI only

---

## V0.62 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] Blueprint Form: Việt hóa 100% - Loại bỏ song ngữ
  - blueprint-editor-tabs.tsx: Tab labels, table headers, descriptions
  - tinh-gia-admin-client.tsx: Dialog header + footer buttons
  - Tabs: "Tham Số" "Bố Cục" "Bình Bản" "Xem Trước" "JSON"
  - All buttons: "Lưu" "Hủy" "Tạo Mới" "Cập Nhật"

---

## V0.61 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [UI] pricing-rule-popup.tsx: V3 Final Redesign (UI ONLY - NO LOGIC CHANGE)
  - Dialog width: 650px → 1024px (max-w-5xl)
  - Header: Gradient orange-to-amber
  - Layout: 2-column → 3-column (Đơn Giá, Giá Trọn Gói, Quy Tắc)
  - Flow Diagram: Vertical → Horizontal compact
  - Cards: Premium gradient backgrounds + icons
  - JSON section: Collapsible panel
  - Footer: Gradient save button

---

## V0.60 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [P0] /m3/tinh-gia-manual: HARD BLOCK when cach_tinh missing
  - No fallback to unit price - calculation blocked completely
  - Red banner shows offending processes (ten, ma, db_id)
  - Preview shows "BLOCKED" instead of price

---

## V0.59 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [SSOT FIX] /m3/tinh-gia-manual: Remove hardcoded threshold 1000
  - getThreshold() now returns null when cach_tinh missing
  - Tracks missingCachTinhRules for UI warning
  - Shows amber warning when process has gia_tron_goi but no cach_tinh

---

## V0.58 (2026-01-29)

**Category:** M1 Danh Mục

- [P0 FIX] M1.4 /m1/bang-gia-cong-doan: Radix Select.Item empty value crash
  - pricing-rule-popup.tsx:310-323: value="" → value="__NONE__"
  - Sentinel __NONE__ maps to undefined in state
  - Verified: page loads, dropdowns work, save-reload OK

---

## V0.57 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] B1 Hard Gate: BLOCK SAVE when SSOT addon types not loaded
  - addonSsotLoaded check in tinh-gia-admin-client.tsx
  - Save buttons disabled when fallback
  - Red warning message: "Không thể lưu..."
- [A] Schema proofs: dm_addon_type (11 cols), dm_bang_gia_cong_doan.cach_tinh (TEXT)
- [C] cach_tinh: Verified NO BUG, PricingRulePopup builder exists
- [D] UOM: Reconfirmed group 18 filter

---

## V0.56 (2026-01-29)

**Category:** M1 Danh Mục

- [M3] A2: Implemented fallback SAVE policy (Allow + Mark NotProven)
  - console.warn when using DEFAULT_ADDON_TYPES fallback
  - UI warning already exists in tinh-gia-admin-client.tsx:2965
- [M1.4] Verified: cach_tinh parse/serialize working correctly
- [M1.4] Verified: UOM validation already implemented (group 18 filter)
- [DOCS] Close-out V0.55+V0.56 with runtime DB proof for dm_addon_type

---

## V0.55 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] SSOT loai_addon: Created dm_addon_type table with 8 seed types
- [M3] AddonConfigBuilder integrated into tinh-gia-admin
  - Server query SSOT options in page.tsx
  - Builder receives props from parent
  - Raw JSON view is secondary (View/Export only)
- [M3] Fallback behavior: Show warning if SSOT options not loaded

---

## V0.54 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] SCHEMA FIX - nhom_san_pham_id type mismatch:
  - DB PROOF: SHOW CREATE TABLE via Laragon MySQL
  - dm_blueprint.nhom_san_pham_id: int NOT NULL
  - dm_pricing_addon.nhom_san_pham_id: int NULL
  - dm_auto_pricing_formula.nhom_san_pham_id: int NULL
  - FIX: Updated TypeScript types string → number
  - FIX: Updated form state defaults "" → 0
  - Files: tinh-gia-admin-client.tsx, pricing-config.ts

---

## V0.53 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] HARD ENFORCEMENT SSOT Compliance:
  - DELETE: /m3/test-ui-builders route (violation)
  - REFACTOR: rule-builder.tsx accepts basisOptions from SSOT
  - REFACTOR: addon-config-builder.tsx accepts addonTypeOptions, congDoanOptions
  - REFACTOR: formula-builder.tsx accepts processRuleOptions, bangGiaOptions
  - NEW types: BasisOption, AddonTypeOption, CongDoanOption, ProcessRuleOption, BangGiaOption
  - All builders show warning when using default fallback options

---

## V0.52 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] Phase 3-4 UI Builders Complete:
  - NEW: formula-builder.tsx (process rules, mappings table, custom expressions)
  - NEW: addon-config-builder.tsx (addon types, FK dropdown, config fields)
  - NEW: test-ui-builders page for component showcase
  - Browser test: All components verified working

---

## V0.51 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] Phase 2 RuleBuilder for cach_tinh:
  - NEW: rule-builder.tsx (3 pricing modes, bilingual labels, visual preview)
  - Modes: per_unit, flat_fee, tier_switch
  - JSON output panel with copy button

---

## V0.50 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] Phase 1 Manual UX Enhancement (Bilingual Labels):
  - NEW: bilingual-field.tsx (BilingualFieldLabel, FieldRow, FieldTable, PRICING_LABELS)
  - MODIFY: tinh-gia-manual Preview table with 3-column layout
  - MODIFY: Card headers with VI — EN format (5 headers updated)

---

## V0.48 (2026-01-29)

**Category:** M3 CRM & Bán Hàng

- [M3] UI Builders for JSON - Phase 1 & 2 Complete:
  - NEW: ViewJsonPanel - Readonly JSON viewer with copy, collapse, modal
  - NEW: BilingualLabel - VI/EN label format with tooltip
  - NEW: InputContractTag - Source indicator (user_input/derived/system)
  - NEW: PRICING_GLOSSARY - Bilingual terminology (200+ terms)
  - NEW: ParamRegistryBuilder - Param cards with type-specific inputs
  - NEW: BindingEditor - Binding table with ConditionBuilder integration
  - NEW: TypedInputForm - Form generator from field definitions
  - MODIFY: pricing/index.ts - Export all new components

---

## V0.47 (2026-01-28)

**Category:** M3 CRM & Bán Hàng

- [SEED] Created Demo Flow Carton V1 seed script
- 5 demo processes (DEMO_IN_OFFSET_4C, CAN_MANG_MO, BE_HINH, DAN_THANH_PHAM, DONG_GOI)
- 5 pricing rules with cach_tinh JSON (tiered, per_unit, flat_fee)
- Formula FORMULA_DEMO_CARTON_V1 (markup=20%, rounding=ceil, step=1000)
- Blueprint BP_DEMO_CARTON_V1
- 5 test cases (3 PASS, 2 FAIL - intentional wrong expected values)
- Script: scripts/seed-demo-flow-carton-v1.ts

---

## V0.46 (2026-01-28)

**Category:** Database

- [BUGFIX] Fixed NaN display in Test Vectors diff column
- [BUGFIX] Fixed Actual Total showing "—" incorrectly
- Updated formatDiff() to handle null/NaN safely
- Updated diff interface to allow null values
- API now returns null diff when expected values are null

---

## V0.45 (2026-01-28)

**Category:** M3 CRM & Bán Hàng

- [API] Complete Test Vectors API endpoints:
  - GET /api/tinh-gia-admin/test-cases?formula_id=...
  - PUT /api/tinh-gia-admin/test-cases/[id] (with Forbidden Keys validation)
  - DELETE /api/tinh-gia-admin/test-cases/[id]
  - POST /api/tinh-gia-admin/test-cases/[id]/run (PASS/FAIL/ERROR + diff)
- [Validation] Server-side Forbidden Keys: markup, price, cost, profit, amount, gia...

---

## V0.44 (2026-01-28)

**Category:** M3 CRM & Bán Hàng

- [M3] Complete Test Vectors UI implementation per spec:
  - Created TestCaseForm.tsx with Forbidden Keys validation + autocomplete
  - Created TestResultAlert.tsx (PASS/FAIL/ERROR variants + Diff table)
  - Created RunAllSummary component for aggregated results
  - Updated TestVectorsTab.tsx with Form dialog, Edit button, Diff columns
  - Added run-all API endpoint at /api/tinh-gia/run-test/run-all
  - Added server-side Forbidden Keys validation in test-cases POST route
  - Wired formulaId, formulaName, onRefreshTestCases props through FormulaDialogV13

---

## V0.43 (2026-01-28)

**Category:** M3 CRM & Bán Hàng

- [M3] Implemented Test Vectors UI in FormulaDialogV13:
  - Created TestVectorsTab.tsx component (300+ lines)
  - Added 6th tab "Tests (Kiểm Thử)" to FormulaDialogV13
  - UI: Header with Run All/Add Test buttons, Stats bar, Table with PASS/FAIL status
  - Expandable detail rows showing Input JSON + Expected vs Actual comparison
  - Added 4 test case functions: addTestCase, runSingleTestCase, runAllTestsCases, deleteTestCase
  - Wired props: testCases, testResults, isRunningTest, isRunningAll, callbacks

---

## V0.42 (2026-01-28)

**Category:** Database

- [Admin UI] Phase 2 Audit completed with schema evidence

---

## V0.41 (2026-01-28)

**Category:** M3 CRM & Bán Hàng

- [DB] dm_blueprint.nhom_cong_nghe_id: NULL → NOT NULL (2-key architecture)
- [DB] Added FK fk_blueprint_nhom_cong_nghe → dm_nhom_universal
- [DB] Added UNIQUE uk_blueprint_2key (nhom_cong_nghe_id, nhom_san_pham_id)
- [Breaking] Blueprint creation now REQUIRES nhom_cong_nghe_id

---

## V0.40 (2026-01-28)

**Category:** M3 CRM & Bán Hàng

- [DB] Fixed nhom_san_pham_id type mismatch (VARCHAR → INT) in dm_blueprint, dm_pricing_addon
- [DB] Added 4 Foreign Key constraints for integrity
- [Breaking] Code using these tables must be aware of strict FKs

---

## V0.39 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.2] Sản Phẩm table redesign theo demo V2:
  - Ẩn cột "Thao Tác" (actions ở Detail Panel)
  - Thêm cột "Nhóm" với badge màu theo nhóm
  - Audit icon hover (giống Khách Hàng/Vật Tư)
  - Compact styling: padding giảm, font size nhỏ hơn
  - Product avatar màu pastel (demo-matching)
  - Table card đơn giản (bỏ glassmorphism)
  - Footer đơn giản (bỏ sort badge)

---

## V0.38 (2026-01-27)

**Category:** General

- [R1] Khách Hàng Table: Ẩn cột "Thao Tác"
- [R2] Khách Hàng Table: Cột "Loại KHG" min-w-[180px]
- [R3] Khách Hàng Table: Cột "Tên Khách Hàng" min-w-[450px]
- [R4] Khách Hàng Table: Icon Audit chuyển sang Mã KH + hover effect (giống Vật Tư)

---

## V0.37 (2026-01-27)

**Category:** M1 Danh Mục

- [R1] Fix wizard preview ĐKTT: hiển thị label thay vì ID (lookup từ options)
- [R2] Fix detail panel ĐKTT: hiển thị label thay vì "Chưa có" (lookup từ dieu_kien_thanh_toan_id)
- [R2] Fix detail panel Chức Vụ Đại Diện: lookup từ chuc_vu_dai_dien_id
- [M1.1] Added chucVuDaiDienOptions to khach-hang page/client

---

## V0.36 (2026-01-27)

**Category:** M1 Danh Mục

- [DB] DROP COLUMN loai_dia_chi FROM kh_dia_chi (legacy removed)
- [DB] DROP COLUMN chuc_vu FROM kh_lien_he (legacy removed)
- [DB] DROP COLUMN chuc_vu_dai_dien FROM dm_khach_hang (legacy removed)
- [DB] DROP COLUMN dieu_kien_thanh_toan FROM dm_khach_hang (legacy removed)
- [M1.1] All types, store, UI updated to use SSOT ID fields only

---

## V0.35 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.1 UI] chuc_vu_dai_dien dropdown now saves ID (+ legacy text lookup)
- [M1.1 UI] dieu_kien_thanh_toan dropdown now saves ID (+ legacy text lookup)
- [M1.1] State init from ID fields, payload includes both legacy + SSOT

---

## V0.34 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.1 DB] ADD COLUMN chuc_vu_dai_dien_id to dm_khach_hang (SSOT)
- [M1.1 DB] ADD COLUMN dieu_kien_thanh_toan_id to dm_khach_hang (SSOT)
- [M1.1] Types + Store updated with 2 new ID fields
- [M1.1] INSERT/UPDATE queries include new ID columns

---

## V0.33 (2026-01-27)

**Category:** M1 Danh Mục

- [Components] NEW: SearchableSelect with diacritics-insensitive search
- [M1.1] R6: Tỉnh/Phường dropdowns now support có dấu/không dấu search
- [M1.1] Example: "Ho Chi Minh" or "hồ chí minh" both find HCM

---

## V0.32 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.1 DB] ADD COLUMN chuc_vu_khg_id to kh_lien_he (SSOT ref dm_nhom_universal)
- [M1.1] 6 UI fixes: loai_dia_chi + chuc_vu now display ten_nhom from SSOT
- [M1.1] Fix: wizard default address uses first nhomDiaChiOptions
- [M1.1] Fix: contact table/preview lookup chucVuDaiDienOptions
- [M1.1] Fix: detail panel address badge uses nhom_dia_chi_id lookup

---

## V0.31 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.1 DB] ADD COLUMN nhom_dia_chi_id to kh_dia_chi (SSOT ref dm_nhom_universal)
- [M1.1] Step 3 Address: load types from dm_nhom_universal (no hardcode)
- [M1.1] Fix title "Địa Chỉ Giao Hàng" → "Địa Chỉ Khách Hàng"
- [M1.1] Fix column header "Loại" → "Loại Địa Chỉ"
- [M1.1] Hide postal code field in address modal

---

## V0.30 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.1 MAJOR] Migrated Khách Hàng from in-memory store to MySQL database
- [M1.1] Rewrote m1-1-store.ts: all CRUD functions now use mysql2 query()
- [M1.1] Updated actions.ts with async/await for all store function calls
- [M1.1] Fixed page.tsx to await listCustomers()

---

## V0.29 (2026-01-27)

**Category:** DevOps

- [UI] ComboboxCreate: Add onTouchMove handler for touch scroll support in nested portals
- [UI] vat-tu-client: Use DB props for nhomVatTuOptions instead of mock helper
- [UI] combobox-create.tsx: Remove ID display from dropdown options (only show label)

---

## V0.28 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.4] UOM SSOT: don_vi/don_vi_tron_goi dropdown từ dm_uom (nhom_don_vi_tinh_id=18, NDT0003)
- [M1.4] Backend UOM validation: reject nếu don_vi không thuộc nhóm Công Đoạn
- [M1.4] Regression test: 10/10 snapshot MATCH

---

## V0.27 (2026-01-27)

**Category:** M1 Danh Mục

- [M1.4 UI] R1: Ẩn cột ID trên list table
- [M1.4 UI] R2: Thêm cột Nhóm CN (read-only badge)
- [M1.4 UI] R3: Form refactored với 4 premium sections (gradient headers)
- [M1.4 UI] R4.1: Backend must-match guard verified (đã có sẵn)

---

## V0.26 (2026-01-27)

**Category:** Governance

- [5-WAY SYNC] Full sync: .cursorrules = .antigravityrules, AGENTS.md = CLAUDE.md = GEMINI.md
- [VERIFIED] All 5 files now have identical content (verified by MD5 hash)

---

## V0.25 (2026-01-27)

**Category:** Governance

- [5-WAY SYNC] Synced 2 new skills (mysql-schema-extraction, text-first-report) to all 5 files
- [RULES] Updated: AGENTS.md, GEMINI.md, CLAUDE.md, .antigravityrules, .cursorrules

---

## V0.24 (2026-01-27)

**Category:** DevOps

- [SKILLS] Created mysql-schema-extraction skill - Extract full schema from MySQL
- [SKILLS] Created text-first-report skill - SSOT report format with 4 sections + Open Questions
- [DOCS] Updated database-ops skill (v1.2.0) - Added Extract Schema section
- [RULES] Added skill references to AGENTS.md and GEMINI.md trigger list

---

## V0.23 (2026-01-27)

**Category:** Auth/Security

- [SKILLS] Mining & Authoring: Created 4 new skills (phased-migration, feature-flag, ssot-docs, contract-preservation)
- [RULES] 5-Way Sync: Updated .cursorrules, .antigravityrules, AGENTS.md, CLAUDE.md, GEMINI.md
- [DOCS] Updated database-ops (v1.1.0) and hard-enforcement-review skills

---

## V0.22 (2026-01-26)

**Category:** M3 CRM & Bán Hàng

- [R4+R5] combo-gate-helpers.ts - Combo Gate SSOT validation
- [R4] getActiveNhomCongNgheIds(), getValidNhomSanPhamIds(), validateComboGate()
- [DB] sanitize-kich-thuoc.ts - Normalized 5 records " x " → "x"
- [R6] loadBlueprintWithFormula() - Load blueprint + formula together

---

## V0.21 (2026-01-26)

**Category:** M3 CRM & Bán Hàng

- [HARD ENFORCEMENT] Production input from dm_san_pham SSOT chain
- [R2] parseKichThuocMM() - MM-ONLY, no cm conversion, no decimals
- [R3+R4] Add dm_san_pham.nhom_cong_nghe_id + dm_vat_tu_id
- [R5] No placeholder blueprint/formula - FAIL if missing
- Migration SQL: sql/migration_dm_san_pham_pricing_fk.sql

---

## V0.20 (2026-01-26)

**Category:** M1 Danh Mục

- [R1] Migrate m1-4-bang-gia-store.ts to DB-backed (MySQL)
- [R1] Remove in-memory store and mock data init
- [R2] Chuẩn hóa success messages với entity name
- Fix CRUD persistence cho /m1/bang-gia-cong-doan

---

## V0.19 (2026-01-26)

**Category:** M1 Danh Mục

- Fix delete dialog button text tại /m1/bang-gia-cong-doan
- Đổi 'Thoát' → 'Xóa', 'Ở Lại' → 'Hủy'
- Title Case: 'Xác nhận xóa' → 'Xác Nhận Xóa'

---

## V0.18 (2026-01-26)

**Category:** M0 Hệ Thống

- [MAJOR] Disable ALL mock/demo data (M0/M1/M3)
- DB local là SSOT duy nhất cho M0/M1
- M3 giữ in-memory mode (empty, no demo)
- Remove demoSanPham, SPTPDEMO từ san-pham-client.tsx
- Empty mock arrays: mock-m1-1.ts, mock-m1-2.ts, mock-m3.ts, mock-m0-form-in.ts
- Fix TS errors: m3-store.ts (gia_ban), san-pham-client.tsx (Phase 0B fields)

---

## V0.17 (2026-01-25)

**Category:** General

- Created watch-version.ps1 file watcher for auto version bump on file save
- Installed git pre-commit hook successfully
- Full automation setup complete

---

## V0.16 (2026-01-25)

**Category:** DevOps

- Implement 3-layer version tracking: skill + git hook + agent rules
- Fix bug ten_vat_tu_day_du hiển thị "ID: 50" thay vì tên nhóm
- Thay getGroupLabelById() bằng nhomVatTuLabelById[] (DB props)
- Đổi column header "Tên Vật Tư" → "Tên Vật Tư Đầy Đủ"
- Implement 5-way sync rule (.cursorrules ↔ .antigravityrules ↔ AGENTS/CLAUDE/GEMINI.md)
- Widen filter dropdown từ 180px → 250px

---

## V0.15 (2026-01-20)

**Category:** Governance

- Created 2 new skills: field-name-consistency-check, optimistic-ui-updates
- Updated 2 existing skills: form-field-validation (Context-Aware Validation), debug-systematic (Console Error - Field Undefined)
- Updated references in .cursorrules, AGENTS.md, CLAUDE.md

---

## V0.15 (2026-01-20)

**Category:** M3 CRM & Bán Hàng

- Demo Module Tinh Gia Offset: route /m3/tinh-gia, dropdown 20 nhom SP, wizard 4 buoc
- Tao component checkbox.tsx, cai @radix-ui/react-checkbox

---

## V0.14 (2026-01-20)

**Category:** Governance

- Created 3 new skills: hard-enforcement-review, ui-section-reorder, db-label-to-human-text
- Updated references in .cursorrules, AGENTS.md, CLAUDE.md

---

## V0.13 (2026-01-20)

**Category:** M1 Danh Mục

- M3 Bao Gia: Sap xep lai sections trong form (San Pham -> Thoi Gian -> Ghi Chu)

---

## V0.12 (2026-01-20)

**Category:** M0 Hệ Thống

- M0 Dashboard: Fix title cards dm_nhom_universal → "Nhóm Danh Mục", dm_quy_trinh → "Quy Trình Workflow"

---

## V0.11 (2026-01-20)

**Category:** M3 CRM & Bán Hàng

- M3 Bao Gia: Sap xep lai Detail Panel, layout 2 cot, icon Eye popover, fix search khong dau

---

## V0.08 (2026-01-19)

**Category:** Governance

- Added 5 new skills (versioning-change-history, debug-systematic, mock-data-generator, form-field-validation, refactor-extract-component) and updated 3 existing skills

---

## V0.06 (2026-01-19)

**Category:** Governance

- Added rule 12: Versioning and Change History (MANDATORY) to .cursorrules, AGENTS.md, CLAUDE.md

---

## V0.05 (2026-01-19)

**Category:** General

- Test bump version script

---

