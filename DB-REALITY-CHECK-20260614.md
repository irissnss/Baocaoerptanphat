# 🔬 DB Reality Check — Go-Live P0 Conflict Audit

> **Plan ID:** PLAN-20260614-GOLIVE-FOUNDATION-P0
> **Ngày:** 14/06/2026 23:30
> **MySQL:** 8.4.3 Laragon LOCAL — Database: tanphat_erp_dev

---

## Quick Summary

| Metric | Giá trị |
|--------|---------|
| **MySQL version** | 8.4.3 |
| **Database** | tanphat_erp_dev |
| **User accounts** | 1 (Admin) |
| **Employees** | 30 records (ALL chưa link user) |
| **Roles defined** | 6 (ADMIN, CEO, HR, KE_TOAN, **SALES**, USER) |
| **Roles with permissions** | 2 (ADMIN full, USER basic) |
| **SALES permissions** | 🔴 0 — Tồn tại nhưng CHƯA gán quyền |
| **THIET_KE role** | ❌ Chưa tạo |
| **Action permission table** | ✅ Có nhưng RỖNG |
| **Permission audit log** | ✅ Có (4-layer: menu/data/field/action) |

---

## 🔴 Conflict Phát Hiện

### C1: Role Name Mismatch
- **GitHub report nói:** "Chưa có SALE/THIET_KE"
- **DB thực tế:** Đã có **SALES** (không phải SALE) — tạo từ 05/03/2026
- **Quyết định:** Dùng **SALES** làm canonical (không tạo SALE trùng)

### C2: SALES Role Rỗng
- SALES tồn tại trong `dm_vai_tro` nhưng **0 records** trong `role_menu_permission`
- Nghĩa là: dù gán role SALES cho user, user đó **không thấy menu nào**
- **Action cần:** Seed permissions cho SALES

### C3: Employee-User Disconnect
- 30 nhân viên trong DB — **ALL `user_id = NULL`**
- Chỉ 1 user account tồn tại (Admin)
- **Không NV nào có tài khoản đăng nhập**

---

## Kết Quả Đối Chiếu

| Area | Report trước | DB thực tế | Khớp? |
|------|-------------|------------|-------|
| Employee-User link field | user_id | ✅ user_id (FK verified) | ✅ |
| Role SALE | "Chưa có" | SALES đã có (tên khác) | 🔴 Conflict |
| Role THIET_KE | "Chưa có" | ✅ Đúng — chưa có | ✅ |
| RBAC 4-layer | Chưa verify | ✅ menu + action + data + field + log | ✅ Tốt hơn kỳ vọng |
| Page guard | Không có | ❌ Đúng — không có | ✅ |
| Action guard code | Không có | ❌ Đúng — table có nhưng code chưa dùng | ✅ |

---

## Plan Tiếp Theo

| Phase | Nội dung | Status |
|-------|----------|--------|
| 0 | DB Reality Check + Conflict Audit | ✅ DONE |
| 1 | Employee/User/Role Map | ✅ DONE |
| 2 | Role Canonical Decision | ✅ DONE — dùng SALES, tạo THIET_KE |
| 3 | Seed permissions + tạo 14 test users | ⏳ Chờ Owner approve |
| 4 | Page-level RBAC guard | ⏳ Chờ Phase 3 |
| 5 | Action-level permission | ⏳ Chờ Phase 4 |
| 6 | KH smoke test theo role | ⏳ Chờ Phase 5 |
| 7 | M3 readiness audit | ⏳ Chờ Phase 6 |
| 8 | Version/changelog/report | ⏳ Cuối cùng |

---

## Câu Hỏi Còn Lại

| # | Câu hỏi | Ảnh hưởng |
|---|---------|-----------|
| Q2 | Dùng **SALES** (đã có trong DB) thay vì tạo SALE mới? | Phase 3 |
| Q3 | Em đề xuất fix `nguoi_tao = 1` ngay trong Phase 5 — OK? | Phase 5 |
| Q4 | Guard qua layout.tsx (đề xuất) — OK? | Phase 4 |

---

*Báo cáo tạo bởi ERP TanPhat AI Agent — 14/06/2026 23:30*
