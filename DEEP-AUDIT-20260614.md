# 🔬 Deep Audit Report — Go-Live Stage 1

> **Plan ID:** PLAN-20260614-GOLIVE-STAGE1-DEEP-AUDIT
> **Ngày audit:** 14/06/2026
> **Trạng thái:** PLAN-ONLY — Chờ Owner xác nhận

---

## Quick Summary

| Metric | Giá trị |
|--------|---------|
| **Version hiện tại** | V0.216 (code) / V0.217 (changelog) |
| **Total Findings** | 19 (4 BLOCKER, 5 DANGER, 4 RISK, 3 NOTE, 3 PASS) |
| **Scope audit** | M0 RBAC, M1 Nhân sự, M1.1 Khách hàng, M3 Báo giá/Đơn hàng/Thiết kế |
| **Files inspected** | 14 files |
| **Tables referenced** | 14 tables |
| **Kết luận** | ❌ CHƯA sẵn sàng Go-Live — còn 6 BLOCKER bắt buộc |

---

## System Map — Employee / User / Role / Permission

| Component | Table | Status |
|-----------|-------|--------|
| Nhân viên | `hr_employee_nhanvien` (41 cột) | ✅ Có |
| User Account | `user_account` | ✅ Có — bcrypt, lockout, session |
| NV ↔ User link | `user_id` FK (nullable) | ⚠️ Chưa liên kết cho NV thật |
| Vai trò | `dm_vai_tro` | ⚠️ Chưa có SALE/THIET_KE |
| Gán vai trò | `user_role_mapping` | ✅ Có |
| Menu permission | `role_menu_permission` (5 flags) | ✅ Có |
| Sidebar filtering | `canViewMenuKey()` | ✅ Hoạt động |
| Page-level guard | ❌ | 🔴 KHÔNG CÓ |
| Action-level permission | ❌ | 🔴 KHÔNG CÓ |
| Middleware | ❌ | 🔴 KHÔNG CÓ |

---

## Role Matrix Proposal

| Menu | ADMIN | SALE | THIET_KE |
|------|-------|------|----------|
| M0 (System) | Full | ❌ Ẩn | ❌ Ẩn |
| M1 KH | Full | View+Create+Update | View only |
| M1 SP | Full | View only | View only |
| M1 NV | Full | ❌ Ẩn | ❌ Ẩn |
| M3 BG | Full | View+Create+Update | View only |
| M3 ĐH | Full | View+Create+Update | View only |
| M3 TK | Full | View only | View+Create+Update |
| M3 CRM | Full | View+Create+Update | ❌ Ẩn |
| MF | Full | ❌ Ẩn | ❌ Ẩn |

> **Lưu ý:** DELETE chỉ dành cho ADMIN

---

## Findings Table

| # | Status | Item | Impact |
|---|--------|------|--------|
| F1 | 🚫 BLOCKER | Page-level RBAC guard KHÔNG CÓ | NV gõ URL trực tiếp truy cập trang admin |
| F2 | 🚫 BLOCKER | Action-level permission KHÔNG CÓ | Ai login cũng CRUD được |
| F3 | 🚫 BLOCKER | Vai trò SALE/THIET_KE chưa tồn tại | Không thể phân quyền |
| F4 | 🚫 BLOCKER | NV ↔ User chưa liên kết | NV không có tài khoản |
| F5 | 🧨 DANGER | saveBaoGiaBundle() thiếu transaction | Fail giữa chừng = mất data |
| F6 | 🧨 DANGER | deleteBaoGia() không FK guard | Xóa BG có ĐH → orphan |
| F7 | 🧨 DANGER | deleteNhanVien() không FK guard | Xóa NV link user → orphan |
| F8 | 🧨 DANGER | nguoi_tao hardcode = 1 toàn M3 | Không track ai tạo thật |
| F9 | 🧨 DANGER | Version mismatch code vs changelog | Lệch version |
| F10 | 🟡 RISK | Middleware.ts không tồn tại | Không route protection |
| F11 | 🟡 RISK | BG auto-promote draft→sent | Vô tình promote status |
| F12 | 🟡 RISK | BG workflow thiếu reject action | Không thể từ chối BG |
| F13 | 🟡 RISK | Thiết kế chưa audit sâu | Có thể gặp lỗi |
| F14 | ⚠️ NOTE | NV list fallback mock data | OK cho dev |
| F15 | ⚠️ NOTE | Vị trí list fallback mock data | OK cho dev |
| F16 | ⚠️ NOTE | Security page guard chưa redirect | Cần verify |
| F17 | ✅ PASS | M1.1 KH FK Guard | Đã fix V0.217 |
| F18 | ✅ PASS | M1.1 KH DB Transaction | Đã fix V0.217 |
| F19 | ✅ PASS | M3 createBaoGia + createDonHang Transaction | OK |

---

## PASS / LOCKED Items

- ✅ Login/Logout/Password (bcrypt + lockout) — LOCKED
- ✅ Session management (multi-device, auto-expire) — LOCKED
- ✅ RBAC tables schema — LOCKED
- ✅ Sidebar menu filtering — PASS
- ✅ Security Admin UI — PASS
- ✅ M1.1 KH FK Guard + Transaction (V0.217) — PASS
- ✅ M3 create BaoGia/DonHang Transaction — PASS
- ✅ M3 → M8 cross-module safe pattern — PASS
- ✅ Architecture: Server Actions + SSE — LOCKED

---

## Sequenced Fix Plan

| Phase | Mục tiêu | Effort | Prerequisite |
|-------|----------|--------|-------------|
| 0 | Runtime/version proof (bật DB, verify) | 15 phút | MySQL Laragon |
| 1 | Map NV/User/Role hiện tại (read-only) | 30 phút | Phase 0 |
| 2 | Tạo vai trò SALE/THIET_KE + gán permission | 30 phút | Phase 1 |
| 3 | Page-level RBAC guard (chặn URL trực tiếp) | 2 giờ | Phase 2 |
| 4 | Action-level permission (CRUD theo quyền) | 2 giờ | Phase 3 |
| 5 | M3 data safety (transaction + FK guard) | 1 giờ | Any |
| 6 | Tạo user account cho NV + link | 30 phút/NV | Phase 2 |
| 7 | Smoke test end-to-end (8 test cases) | 1 giờ | All |

**Thứ tự khuyến nghị:** 0→1→2→5→3→4→6→7 (fix data safety trước)
**Tổng effort:** ~7-8 giờ

---

## Câu Hỏi Cần Owner Quyết Định

| # | Câu hỏi |
|---|---------|
| Q1 | Danh sách NV cần tạo tài khoản? (tên + email) |
| Q2 | Ma trận quyền có đúng ý? (SALE/THIET_KE/ADMIN) |
| Q3 | Thứ tự fix: Option A (an toàn) hay Option B (nhanh)? |
| Q4 | nguoi_tao hardcode = 1: fix ngay hay sau Go-Live? |

---

## Kết Luận

> ❌ **CHƯA sẵn sàng Go-Live** — còn 4 BLOCKER bắt buộc + 2 data safety critical.
>
> Cần Owner xác nhận Q1-Q4 trước khi implement.

---

*Báo cáo tạo bởi ERP TanPhat AI Agent — Audit 14/06/2026*
