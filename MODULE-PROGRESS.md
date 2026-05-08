# 📦 Module Progress — ERP Tân Phát

> Chi tiết chức năng đã hoàn thành, đang phát triển, và planned cho từng module.

---

## M0 — Hệ Thống (✅ Ready)

**Mô tả:** Quản lý danh mục chung, phòng ban, chức vụ, quy trình, auth/session nội bộ.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Danh mục chung (Universal) | ✅ Done | V0.05+ | Vietnamese labels, encoding fix V0.121 |
| Quản lý phòng ban | ✅ Done | V0.10+ | Metronic-style table, orange gradient header |
| Quản lý chức vụ | ✅ Done | V0.10+ | DB-backed |
| Quy trình (Workflow Engine) | ✅ Done | V0.205+ | dm_quy_trinh migration, async fix V0.206 |
| Auth/Session nội bộ | ✅ Done | V0.171+ | Fail-closed, expired warning V0.172, M0 owns auth (V0.174) |
| Dashboard tổng quan | ✅ Done | V3.33.1 | Premium redesign, module progress cards |

### Ghi chú:
- M0 là SSOT cho auth/session. M9 (Cổng Thông Tin) sẽ kế thừa auth pattern từ M0.
- Sidebar grouping Nhân Sự (M6) nằm trong M0 shell (V0.176–V0.178).

---

## M1 — Danh Mục (✅ Ready)

**Mô tả:** Khách hàng, sản phẩm, vật tư, nhân sự, vị trí, bảng giá công đoạn.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Khách hàng (dm_khach_hang) | ✅ Done | V0.30 | **MAJOR** migrate in-memory → MySQL. Wizard multi-step, detail panel |
| Sản phẩm (dm_san_pham) | ✅ Done | V0.39+ | Table redesign V2, List/Grid toggle, HARD ENFORCEMENT V0.21 |
| Vật tư (dm_vat_tu) | ✅ Done | V0.20+ | DB-backed, normalizeTenVatTu |
| Nhân sự (dm_nhan_su) | ✅ Done | V0.204 | Metronic UI redesign, 4 stats pills |
| Vị trí (dm_vi_tri) | ✅ Done | V0.209 | Metronic UI consistency, 18-point forensic diff |
| Bảng giá công đoạn (M1.4) | ✅ Done | V0.95–V0.99 | TierBuilder, UOM SSOT validation, 2-key resolve |
| SearchableSelect (không dấu) | ✅ Done | V0.33+ | normalizeText SSOT pattern |
| Title Auto Case | ✅ Done | V0.209+ | toVietnameseTitleCase, formatVietHoaDauChu |

### Schema highlights:
- **dm_khach_hang:** wizard 4 steps, sub-tables kh_dia_chi + kh_lien_he
- **dm_san_pham:** SSOT chain enforcement, combo from dm_blueprint only
- **Bảng giá:** 2-key resolve (nhom_cong_nghe_id, nhom_san_pham_id), markup% from dm_auto_pricing_formula

---

## M3 — CRM & Bán Hàng (🔨 In Dev)

**Mô tả:** Báo giá, đơn hàng, CRM, tính giá tự động/thủ công.

### Chức năng đã hoàn thành:
| Chức năng | Status | Version | Ghi chú |
|-----------|--------|---------|---------|
| Báo giá (Quotation) | ✅ Done | V0.154+ | Foundation pilot, Metronic table V0.168, workflow V0.206 |
| Đơn hàng (Order) | ✅ Done | V0.109+ | Card tổng quan, radian styling, status workflow |
| CRM / Nhật ký | ✅ Done | V0.118–V0.120 | Tách Việc Cần Làm / Nhật Ký 2 tab, lọc theo KH, searchbox |
| Tính giá thủ công | ✅ Done | V0.50–V0.60 | SSOT enforcement, HARD BLOCK missing cach_tinh |
| Tính giá tự động | ✅ Done | V0.95–V0.99 | Tier pricing, Blueprint combo gate |
| Pricing Admin UI | ✅ Done | V0.60–V0.77 | Blueprint/Formula/Addon/Param dialogs, Việt hóa 100% |

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
| M4 Overview | ✅ Done | V0.105 | Thay placeholder, shared metadata |
| Phase 1 E2E | ✅ Done | V0.110 | End-to-end verification |

---

## M5 — Kho Hàng (📋 Phase 1 Skeleton)

**Mô tả:** Nhà cung cấp, mua hàng, nhập/xuất kho, giao hàng.

| Chức năng | Status | Version |
|-----------|--------|---------|
| Overview page | ✅ Done | V0.122 |
| Skeleton layout | ✅ Done | V0.128 |
| Core features | 📋 Planned | — |

---

## M6 — Nhân Sự (📋 Planned)

**Mô tả:** Vận hành HR, chấm công, nghỉ phép.

| Chức năng | Status | Ghi chú |
|-----------|--------|---------|
| HR sidebar grouping | ✅ Done | V0.176–V0.178 |
| Core features | 📋 Planned | Sẽ kế thừa dm_nhan_su từ M1 |

---

## M7 — Tiền Lương (🔨 In Dev)

**Mô tả:** Payroll, BHXH, TNCN lũy tiến, phiếu chi lương.

| Chức năng | Status | Version |
|-----------|--------|---------|
| Payroll engine | ✅ Done | V0.140 |
| Phiếu chi lương | ✅ Done | V0.140 |
| BHXH/TNCN | 🔨 In Dev | — |

---

## M8 — Công Việc & Quy Trình (🔨 In Dev)

**Mô tả:** Task management, tích hợp CRM.

| Chức năng | Status | Version |
|-----------|--------|---------|
| Task list UI | ✅ Done | V0.108 |
| M3-M8 Integration | ✅ Done | V0.102+ |

---

## M9 — Cổng Thông Tin (📋 Planned)

**Mô tả:** Customer portal, multi-session.

| Chức năng | Status | Ghi chú |
|-----------|--------|---------|
| Auth kế thừa M0 | 📋 Planned | V0.174 clarified ownership |
| Customer portal | 📋 Planned | — |

---

## MF — Tài Chính (🔨 In Dev)

**Mô tả:** Phiếu thu/chi, công nợ, ngân hàng.

| Chức năng | Status | Version |
|-----------|--------|---------|
| Overview page | ✅ Done | V0.123 |
| Phiếu thu/chi | ✅ Done | V0.141 |
| Công nợ KH/NCC | ✅ Done | V0.141 |
| Smoke test automation | ✅ Done | V0.142 |
| Core accounting | 🔨 In Dev | — |
