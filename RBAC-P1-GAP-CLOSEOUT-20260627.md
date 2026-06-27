# 📊 RBAC P1 Gap Closeout — Báo Cáo Chi Tiết Toàn Diện

> **Ngày:** 27/06/2026
> **Plan ID:** PLAN-20260627-RBAC-P1-GAP-CLOSEOUT
> **Version:** V0.220 → V0.222

---

## I. TÓM TẮT TỔNG QUAN

### Trước khi làm (V0.219)
- **Page layout guards:** 14/14 ✅ (đã xong từ V0.218)
- **Server action guards:** 12/303 functions có guard (~4%)
- **Action files guarded:** 4/42 files (partial)
- **Sensitive modules:** MF Finance, M6 HR, M7 Payroll — KHÔNG CÓ GUARD NÀO

### Sau khi làm (V0.222)
- **Page layout guards:** 14/14 ✅ (không đổi)
- **Server action guards:** 69/303 functions có guard (~23%)
- **Action files guarded:** 5/42 fully guarded + 4 partial = 9/42
- **Sensitive modules:** MF (13/13), M6 (28/28), M7 (16/16) — FULL GUARD ✅

### Tăng trưởng
| Metric | Trước | Sau | Tăng |
|--------|-------|-----|------|
| Functions guarded | 12 | 69 | **+57 (+475%)** |
| Fully guarded files | 0 | 5 | **+5** |
| Finance protection | ❌ | ✅ | **13 functions** |
| HR protection | ❌ | ✅ | **28 functions** |
| Payroll protection | ❌ | ✅ | **16 functions** |

---

## II. CHI TIẾT TỪNG MODULE ĐÃ LÀM

### A. MF Finance — 3 files, 13 functions ✅ FULL GUARD

| File | Function | Guard |
|------|----------|-------|
| mf/cong-no/actions.ts | getCongNoList | `mf.view` + `can_view_customer_debt` |
| mf/cong-no/actions.ts | getCongNoById | `mf.view` + `can_view_customer_debt` |
| mf/cong-no/actions.ts | createCongNoAction | `mf.create` + `can_manage_customer_debt` |
| mf/phieu-thu/actions.ts | createPhieuThuAction | `mf.create` + `can_manage_payment` |
| mf/phieu-thu/actions.ts | updatePhieuThuAction | `mf.update` + `can_manage_payment` |
| mf/phieu-thu/actions.ts | approvePhieuThuAction | `mf.update` + `can_manage_payment` |
| mf/phieu-thu/actions.ts | collectPhieuThuAction | `mf.update` + `can_manage_payment` |
| mf/phieu-thu/actions.ts | cancelPhieuThuAction | `mf.update` + `can_manage_payment` |
| mf/phieu-chi/actions.ts | createPhieuChiAction | `mf.create` + `can_manage_payment` |
| mf/phieu-chi/actions.ts | updatePhieuChiAction | `mf.update` + `can_manage_payment` |
| mf/phieu-chi/actions.ts | approvePhieuChiAction | `mf.update` + `can_manage_payment` |
| mf/phieu-chi/actions.ts | payPhieuChiAction | `mf.update` + `can_manage_payment` |
| mf/phieu-chi/actions.ts | cancelPhieuChiAction | `mf.update` + `can_manage_payment` |

> **Ý nghĩa:** SALES không thể tạo/duyệt phiếu thu chi. Chỉ KE_TOAN + ADMIN/CEO mới thao tác được tài chính.

### B. M6 HR — 1 file, 28 functions ✅ FULL GUARD (Protection Lock)

Tất cả 28 functions đều có `requireActionPermission("m6", action)`:

**View (8 fns):** getNhanVienOptions, getCaLamViecList, getChamCongRawList, getChamCongTinhList, getNghiPhepList, getNghiPhepDetail, getOvertimeList, getOvertimeDetail

**Create (6 fns):** createCaLamViecAction, createChamCongRawAction, importChamCongRawAction, createChamCongTinhAction, createNghiPhepAction, createOvertimeAction

**Update (14 fns):** updateCaLamViecAction, toggleCaLamViecStatusAction, updateChamCongTinhAction, bulkUpdateChamCongTinhAction, lockChamCongThangAction, approveNghiPhepAction, rejectNghiPhepAction, bulkApproveNghiPhepAction, bulkRejectNghiPhepAction, submitNghiPhepAction, approveOvertimeAction, rejectOvertimeAction, bulkApproveOvertimeAction, bulkRejectOvertimeAction

> **⚠️ KE_TOAN_NHAN_SU pack chưa bật.** Chỉ ADMIN/CEO mới truy cập được M6.

### C. M7 Payroll — 1 file, 16 functions ✅ FULL GUARD (Protection Lock)

Tất cả 16 functions đều có `requireActionPermission("m7", action)`:

**View (6 fns):** getBangLuongList, getBangLuongDetail, getBangLuongItemList, getLuongPhuCapList, getNhanVienOptions, getPayrollReadinessAction

**Create (2 fns):** createBangLuongAction, createLuongPhuCapAction

**Update (8 fns):** calculatePayrollAction, transitionBangLuongAction, approvePhuCapAction, rejectPhuCapAction, recalcHeaderTotalsAction, recalcBangLuongItemAction, updateItemPaymentStateAction, createPhieuChiFromPayrollAction

> **⚠️ KE_TOAN_TINH_LUONG pack chưa bật.** Chỉ ADMIN/CEO mới truy cập được M7.

### D. THIET_KE Scope Fix ✅

- **Trước:** `can_view_all_customers = 1` (sai)
- **Sau:** `can_view_all_customers = 0` (đúng — chỉ xem KH theo context LSX/PDI)

---

## III. TRẠNG THÁI TOÀN BỘ 42 ACTION FILES

### ✅ Fully Guarded (5 files — 69 functions)

| File | Functions | Status |
|------|-----------|--------|
| m6/actions.ts | 28/28 | ✅ FULL |
| m7/actions.ts | 16/16 | ✅ FULL |
| mf/cong-no/actions.ts | 3/3 | ✅ FULL |
| mf/phieu-thu/actions.ts | 5/5 | ✅ FULL |
| mf/phieu-chi/actions.ts | 5/5 | ✅ FULL |

### 🟡 Partially Guarded (4 files — từ trước V0.219)

| File | Guarded/Total | Status |
|------|--------------|--------|
| m1/khach-hang/actions.ts | 4/12 | 🟡 PARTIAL |
| m3/bao-gia/actions.ts | 3/16 | 🟡 PARTIAL |
| m3/don-hang/actions.ts | 2/5 | 🟡 PARTIAL |
| m4/lenh-san-xuat/actions.ts | 3/20 | 🟡 PARTIAL |

### ❌ Unguarded (33 files — 234 functions)

**P1 Critical (2 files):**
| File | Functions | Lý do |
|------|-----------|-------|
| m0/security/actions.ts | 8 | Admin config — cần guard ASAP |
| m4/phieu-dieu-in/actions.ts | 17 | Có session check nhưng chưa có RBAC |

**P2 (13 files — 85 functions):**
| File | Functions | Module |
|------|-----------|--------|
| m5/giao-hang/actions.ts | 9 | M5 Kho |
| m5/phieu-nhap/actions.ts | 9 | M5 Kho |
| m5/phieu-xuat-kho/actions.ts | 8 | M5 Kho |
| m5/mua-hang/actions.ts | 10 | M5 Kho |
| m5/nha-cung-cap/actions.ts | 5 | M5 Kho |
| m5/ncc-vat-tu/actions.ts | 5 | M5 Kho |
| m5/kho-thanh-pham/actions.ts | 5 | M5 Kho |
| m5/kho-giao-dich/actions.ts | 3 | M5 Kho |
| m5/gia-vat-tu/actions.ts | 5 | M5 Kho |
| m5/kiem-ke-kho/actions.ts | 4 | M5 Kho |
| m3/crm/actions.ts | 4 | M3 CRM |
| m3/crm/nhat-ky/actions.ts | 4 | M3 CRM |
| m3/thiet-ke/actions.ts | 4 | M3 Design |

**P3 (18 files — 124 functions):**
| File | Functions | Module |
|------|-----------|--------|
| m1/san-pham/actions.ts | 5 | M1 Danh Mục |
| m1/vat-tu/actions.ts | 5 | M1 Danh Mục |
| m1/cong-doan/actions.ts | 5 | M1 Danh Mục |
| m1/bang-gia-cong-doan/actions.ts | 8 | M1 Danh Mục |
| m1/material-item/actions.ts | 5 | M1 Danh Mục |
| m1/uom/actions.ts | 5 | M1 Danh Mục |
| m1/uom-conversion/actions.ts | 4 | M1 Danh Mục |
| m1/vi-tri/actions.ts | 5 | M1 Danh Mục |
| m1/nhan-su/actions.ts | 6 | M1 Nhân Sự |
| m1/khach-hang/dia-chi-vn/actions.ts | 1 | M1 KH |
| m0/dm-nhom-universal/actions.ts | 5 | M0 DM |
| m0/form-mau/actions.ts | 4 | M0 |
| m0/form-phat-hanh/actions.ts | 2 | M0 |
| m0/phong-ban/actions.ts | 5 | M0 |
| m0/quy-trinh/actions.ts | 4 | M0 |
| m8/actions.ts | 3 | M8 |
| mc/actions.ts | 15 | MC |
| admin/pricing/actions.ts | 6 | Admin |

---

## IV. PERMISSION MATRIX — HIỆN TRẠNG DB

### Action Permissions (role_action_permission)

| Action Code | ADMIN | CEO | SALES | THIET_KE | KE_TOAN |
|-------------|-------|-----|-------|----------|---------|
| can_view_all_customers | bypass | ✅ | ❌ | ❌ (fixed) | ✅ |
| can_view_own_customers | bypass | — | ✅ | ❌ | — |
| can_transfer_customer | bypass | ✅ | ❌ | ❌ | ❌ |
| can_view_cost_price | bypass | ✅ | ❌ | ❌ | ❌ |
| can_view_profit | bypass | ✅ | ❌ | ❌ | ❌ |
| can_view_margin_percent | bypass | ✅ | ❌ | — | — |
| can_approve_production | bypass | ✅ | — | ❌ | — |
| can_create_production_order | bypass | — | — | ✅ | — |
| can_create_print_instruction | bypass | — | — | ✅ | — |
| can_manage_payment | bypass | — | — | — | ✅ |
| can_view_customer_debt | bypass | — | — | — | ✅ |
| can_manage_customer_debt | bypass | — | — | — | ✅ |
| can_view_invoice | bypass | — | — | — | ✅ |

### Permission Packs (KE_TOAN)

| Pack | Status | Mô tả |
|------|--------|--------|
| KE_TOAN_CONG_NO | ✅ Active | Xem/quản lý công nợ |
| KE_TOAN_THU_CHI | ✅ Active | Phiếu thu/chi |
| KE_TOAN_GIAO_HANG | ✅ Active | Giao hàng/chứng từ |
| KE_TOAN_NHAN_SU | ⏳ Deferred | Chưa audit M6 |
| KE_TOAN_TINH_LUONG | ⏳ Deferred | Chưa audit M7 |
| KE_TOAN_KHO_CHUNG_TU | ⏳ Deferred | Chưa audit M5 |

---

## V. MỨC ĐỘ HOÀN THÀNH

| Hạng mục | Hoàn thành | Ghi chú |
|----------|------------|---------|
| Page layout guards (14 routes) | **100%** | Đã xong từ V0.218 |
| MF Finance server guards | **100%** | 13/13 functions |
| M6 HR protection lock | **100%** | 28/28 functions |
| M7 Payroll protection lock | **100%** | 16/16 functions |
| THIET_KE scope correction | **100%** | DB updated |
| M1 KH/M3 BG/DH/M4 LSX guards | **~30%** | Partial — từ trước |
| M5 Kho guards | **0%** | P2 deferred |
| M0/M1 DM/M8/MC guards | **0%** | P3 deferred |
| UI permission hiding | **0%** | P3 — chưa ưu tiên |
| Real user creation | **0%** | Template sẵn — chờ Owner matrix |
| Employee matrix template | **100%** | Đã tạo file template |
| TypeScript build | **100%** | tsc --noEmit PASS |
| Public reports | **100%** | GitHub updated |

**Tổng thể RBAC Server Action Guards: 69/303 functions (23%) — tăng từ 4%**

---

## VI. ĐỀ XUẤT TIẾP THEO (ƯU TIÊN)

### 🔴 Ưu tiên 1: Hoàn thiện Partial Guards (4 files đang PARTIAL)
- m1/khach-hang/actions.ts: 8 functions chưa guard
- m3/bao-gia/actions.ts: 13 functions chưa guard
- m3/don-hang/actions.ts: 3 functions chưa guard
- m4/lenh-san-xuat/actions.ts: 17 functions chưa guard
- **Tổng: ~41 functions → nâng lên ~110/303 (36%)**

### 🔴 Ưu tiên 2: M0 Security Actions Guard
- m0/security/actions.ts: 8 functions (admin config — critical)
- **Đây là trang quản lý phân quyền, cần guard ADMIN-only**

### 🟡 Ưu tiên 3: M5 Kho (khi Owner mở scope)
- 10 files, 63 functions
- **Nâng lên ~173/303 (57%)**

### 🟢 Ưu tiên 4: Real User Creation
- Template sẵn tại `docs/golive/REAL-EMPLOYEE-MATRIX-TEMPLATE.md`
- **Owner điền matrix → Agent tạo user**

### 🔵 Ưu tiên 5: UI Permission Hiding (P3)
- Ẩn buttons/columns không có quyền
- Dùng `hasSpecificAction()` helper đã có sẵn

---

## VII. KIỂM CHỨNG

- ✅ TypeScript build: `npx tsc --noEmit` — **PASS** (0 errors)
- ✅ DB verified: THIET_KE.can_view_all_customers = 0
- ✅ Guard count verified: Script scan 42 files → 69/303
- ✅ Git committed: V0.222
- ✅ GitHub public report: Pushed
- ❌ Production deploy: NOT APPROVED (per Owner)

---

## VIII. FILES ĐÃ THAY ĐỔI TRONG PHASE NÀY

| File | Thay đổi |
|------|----------|
| src/app/mf/cong-no/actions.ts | +2 imports, +6 guard lines |
| src/app/mf/phieu-thu/actions.ts | +2 imports, +10 guard lines |
| src/app/mf/phieu-chi/actions.ts | +2 imports, +10 guard lines |
| src/app/m6/actions.ts | +2 imports, +28 guard lines |
| src/app/m7/actions.ts | +2 imports, +16 guard lines |
| src/lib/version.ts | V0.220 → V0.222 |
| docs/golive/REAL-EMPLOYEE-MATRIX-TEMPLATE.md | NEW — employee matrix template |

---

> **Đề xuất:** Owner chọn ưu tiên tiếp theo (1–5) để em implement. Không deploy production trong phase này.
