# 🚀 Go-Live Foundation P0 — Implementation Plan (Public)

> **Plan ID:** PLAN-20260614-GOLIVE-FOUNDATION-P0
> **Mục tiêu:** Cho NV Kinh doanh & Thiết kế vận hành KH + Báo Giá + Đơn Hàng
> **Cập nhật:** 14/06/2026

---

## Tổng Quan Tình Hình Hiện Tại

| Item | Status |
|------|--------|
| Version code | V0.216 (chưa bump lên 217) |
| V0.217 changes | ⚠️ Tồn tại nhưng CHƯA COMMIT (FK guard, transaction, audit dialog) |
| MySQL DB | 🔴 OFFLINE (cần bật Laragon) |
| 403 page | ✅ Có |
| RBAC model code | ✅ Đầy đủ (5 flags: view/create/update/delete/export) |
| Sidebar filtering | ✅ Hoạt động |
| Page-level guard | 🔴 KHÔNG CÓ |
| Action-level permission | 🔴 KHÔNG CÓ |
| Middleware.ts | ❌ Không tồn tại |
| Role SALE/THIET_KE | 🔴 Chưa tạo |

---

## Test Users Đề Xuất (14 users)

### 10 SALE Users

| # | Họ tên | Email | Role | Phòng ban |
|---|--------|-------|------|-----------|
| S1 | Nguyễn Văn Minh | minh.nv@intanphat.com | SALE | Kinh Doanh |
| S2 | Trần Thị Hương | huong.tt@intanphat.com | SALE | Kinh Doanh |
| S3 | Lê Hoàng Nam | nam.lh@intanphat.com | SALE | Kinh Doanh |
| S4 | Phạm Thanh Tùng | tung.pt@intanphat.com | SALE | Kinh Doanh |
| S5 | Võ Thị Mai | mai.vt@intanphat.com | SALE | Kinh Doanh |
| S6 | Đặng Quốc Bảo | bao.dq@intanphat.com | SALE | Kinh Doanh |
| S7 | Hoàng Minh Tuấn | tuan.hm@intanphat.com | SALE | Kinh Doanh |
| S8 | Bùi Thị Lan | lan.bt@intanphat.com | SALE | Kinh Doanh |
| S9 | Ngô Đức Thắng | thang.nd@intanphat.com | SALE | Kinh Doanh |
| S10 | Lý Thanh Hà | ha.lt@intanphat.com | SALE | Kinh Doanh |

### 4 THIET_KE Users

| # | Họ tên | Email | Role | Phòng ban |
|---|--------|-------|------|-----------|
| T1 | Phan Văn Đức | duc.pv@intanphat.com | THIET_KE | Thiết Kế |
| T2 | Nguyễn Thị Ngọc | ngoc.nt@intanphat.com | THIET_KE | Thiết Kế |
| T3 | Trương Minh Khôi | khoi.tm@intanphat.com | THIET_KE | Thiết Kế |
| T4 | Lê Thị Phương | phuong.lt@intanphat.com | THIET_KE | Thiết Kế |

> **Mật khẩu mặc định:** `TanPhat@2026` (bắt buộc đổi lần đầu)
> Đây là test data, có thể sửa/xóa/edit tùy ý.

---

## Câu Hỏi Cần Quyết Định (Q2, Q3, Q4)

### Q2 — Role Matrix (Ma Trận Quyền)

**Vấn đề:** Mỗi role (ADMIN, SALE, THIET_KE) được phép làm gì trong hệ thống?

**Đề xuất hiện tại:**

| Hành động | ADMIN | SALE | THIET_KE |
|-----------|-------|------|----------|
| **Xem** KH/BG/ĐH | ✅ | ✅ | ✅ |
| **Tạo** KH | ✅ | ✅ | ❌ |
| **Sửa** KH | ✅ | ✅ | ❌ |
| **Xóa** KH/BG/ĐH | ✅ | ❌ | ❌ |
| **Tạo** Báo Giá | ✅ | ✅ | ❌ |
| **Sửa** Báo Giá | ✅ | ✅ | ❌ |
| **Tạo** Đơn Hàng | ✅ | ✅ | ❌ |
| **Sửa** Đơn Hàng | ✅ | ✅ | ❌ |
| **Tạo** Yêu cầu TK | ✅ | ❌ | ✅ |
| **Sửa** Thiết Kế | ✅ | ❌ | ✅ |
| Vào System/Security | ✅ | ❌ | ❌ |
| Vào Tài Chính | ✅ | ❌ | ❌ |
| Vào Nhân Sự/Lương | ✅ | ❌ | ❌ |

**Điểm cần Owner xác nhận:**
1. SALE có nên tạo Yêu cầu Thiết Kế không? (đề xuất: ❌ để tách rõ trách nhiệm)
2. THIET_KE có nên xem Đơn Hàng không? (đề xuất: ✅ view-only để biết context)
3. Chỉ ADMIN được xóa? (đề xuất: ✅ an toàn nhất)

### Q3 — nguoi_tao Hardcode = 1

**Vấn đề:** Hiện tại toàn bộ M3 (Báo Giá, Đơn Hàng) hardcode `nguoi_tao = 1` (tức Owner). Nghĩa là khi SALE tạo báo giá, hệ thống ghi "người tạo = Owner" thay vì "người tạo = NV Sale đó".

**2 lựa chọn:**
- **A) Fix ngay trong P0:** Truyền user session thật vào store functions → biết chính xác ai tạo cái gì → audit trail tốt hơn
- **B) Fix sau Go-Live:** Chấp nhận tạm `nguoi_tao = 1`, sau khi vận hành ổn mới sửa

**Đề xuất:** Option A — fix ngay, vì hệ thống cần biết "ai tạo báo giá này" để phân quyền sửa/xóa sau này.

**Impact:** ~1 giờ code, không ảnh hưởng UI, chỉ thay đổi giá trị ghi vào DB.

### Q4 — Guard Approach (Page-Level RBAC)

**Vấn đề:** Khi user gõ URL trực tiếp (vd: SALE gõ `/m0/security`), hệ thống cần chặn. Có 2 cách:

**Option A — Layout Guard (Server Component)** ⭐ Đề xuất
- Thêm kiểm tra quyền trong `layout.tsx` của mỗi module (M0, M1, M3...)
- Phù hợp architecture hiện tại (Server Actions + Server Components)
- Không cần tạo thêm file mới ngoài helper
- Chạy server-side, an toàn

**Option B — Middleware (Edge Runtime)**
- Tạo file `src/middleware.ts` chạy trước mọi request
- Nhanh hơn nhưng Edge Runtime có hạn chế (không gọi DB trực tiếp)
- Cần workaround phức tạp hơn

**Đề xuất:** Option A — layout guard, đơn giản + phù hợp kiến trúc hiện tại.

---

## Phân Tích 5 GAP Đã Phát Hiện

| GAP | Mô tả | Status hiện tại | Fix Plan |
|-----|-------|-----------------|----------|
| G1 | Page-level RBAC guard | 🔴 Không có | Phase 4: Thêm guard vào layout.tsx |
| G2 | NV ↔ User chưa liên kết | 🔴 user_id nullable | Phase 3: Tạo user + link |
| G3 | Role SALE/THIET_KE chưa có | 🔴 Chưa tạo | Phase 3: Seed script |
| G4 | M3 chưa audit | 🟡 Partial | Phase 7: Audit readiness |
| G5 | Action permission chưa enforce | 🔴 Không có | Phase 5: Wrapper Server Actions |

---

## Thứ Tự Thực Hiện (8 Phases)

```
Phase 0 ✅ Version/Runtime proof — DONE
Phase 1 ⏳ Employee/User/Role Map — Chờ MySQL
Phase 2 ✅ Role Matrix — Đề xuất xong, chờ confirm
Phase 3 ⏳ Role Setup + User Create — Chờ MySQL + confirm
Phase 4 ⏳ Page-Level RBAC Guard — Chờ Phase 3
Phase 5 ⏳ Action-Level Permission — Chờ Phase 4
Phase 6 ⏳ KH Smoke Test — Chờ Phase 5
Phase 7 ⏳ M3 Readiness Audit — Chờ Phase 6
Phase 8 ⏳ Version/Changelog/Report — Cuối cùng
```

**Tổng effort ước tính:** 7-8 giờ

---

## BLOCKERS Hiện Tại

| # | Blocker | Action cần |
|---|---------|-----------|
| B0A | V0.217 chưa commit | Commit source code |
| B0B | MySQL OFFLINE | Bật Laragon MySQL |

---

## Kết Luận Hiện Tại

> 🚫 **NOT READY** — Còn thiếu page guard, action guard, roles, employee-user link.
>
> Chờ Owner bật MySQL + xác nhận Q2/Q3/Q4 để bắt đầu implement.

---

*Báo cáo tạo bởi ERP TanPhat AI Agent — 14/06/2026*
