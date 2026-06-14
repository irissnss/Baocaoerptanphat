# 🚀 Kế Hoạch Go-Live — Phân Quyền NV Kinh Doanh & Thiết Kế

> **Plan ID:** PLAN-20260614-GOLIVESTAGE1
> **Ngày lập:** 14/06/2026
> **Trạng thái:** Chờ Owner xác nhận

---

## 1. Mục Đích

Phân quyền cho nhân viên **Kinh doanh (Sale)** và **Thiết kế (Design)** vận hành trước 3 chức năng:
- ✅ Tạo & quản lý **Khách Hàng** (M1.1)
- ✅ Quản lý **Báo Giá** (M3)
- ✅ Quản lý **Đơn Hàng** (M3)

---

## 2. Hiện Trạng Hệ Thống

### ✅ Đã Sẵn Sàng

| # | Component | Trạng thái |
|---|-----------|------------|
| 1 | Hệ thống đăng nhập (Login/Logout/Password) | ✅ Production-ready — bcrypt, lockout, session |
| 2 | Bảng phân quyền RBAC (vai trò, gán quyền, menu permission) | ✅ Đầy đủ 4 bảng |
| 3 | Session quản lý (multi-device, auto-expire, concurrent limit) | ✅ Production-ready |
| 4 | Sidebar filter theo quyền (ẩn menu không có quyền) | ✅ Hoạt động |
| 5 | UI quản trị Security (tạo user, gán role, set permission) | ✅ Có sẵn |
| 6 | Module Khách Hàng (M1.1) — CRUD, Wizard, Import CSV | ✅ Đã audit Phase A |
| 7 | Bảng Nhân sự — liên kết NV ↔ User Account | ✅ Có FK `user_id` |
| 8 | SSE Real-time refresh | ✅ Hoạt động |

### Audit Khách Hàng (M1.1) — Đã Hoàn Thành V0.217

| Fix | Trước | Sau |
|-----|-------|-----|
| FK Guard | Xóa KH có Báo Giá → mất liên kết | ✅ Chặn hoàn toàn, liệt kê bảng liên quan |
| DB Transaction | INSERT/DELETE tuần tự → partial data | ✅ Atomic BEGIN/COMMIT/ROLLBACK |
| Alert placeholder | Nút Lịch sử dùng `alert()` thô | ✅ Audit Dialog chuyên nghiệp |

---

## 3. GAP Analysis — 5 Vấn Đề Cần Xử Lý

| # | GAP | Mức độ | Mô tả |
|---|-----|--------|-------|
| **G1** | Không có page-level RBAC guard | 🔴 CRITICAL | Sidebar ẩn menu đúng, nhưng user có thể gõ URL trực tiếp để truy cập trang không có quyền |
| **G2** | Nhân viên ↔ User chưa liên kết chặt | 🟠 HIGH | Tạo NV mới không tự tạo tài khoản đăng nhập |
| **G3** | Chưa có vai trò SALE / THIET_KE | 🟠 HIGH | Cần tạo vai trò + gán quyền menu cụ thể |
| **G4** | M3 Báo Giá/Đơn Hàng chưa audit | 🟡 MEDIUM | Cần audit tương tự KH: FK guard, transaction |
| **G5** | Action-level permission chưa enforce | 🟡 MEDIUM | `can_create`, `can_update`, `can_delete` chưa kiểm tra trong Server Actions |

---

## 4. Kế Hoạch 5 Phase

### Phase 1: Setup Data (~30 phút)
- Tạo vai trò **SALE** (NV Kinh doanh)
- Tạo vai trò **THIET_KE** (NV Thiết kế)
- Gán quyền menu cho từng vai trò
- Tạo tài khoản đăng nhập cho NV
- Liên kết NV ↔ User Account

### Phase 2: Page-Level Guard (~2 giờ)
- Tạo helper kiểm tra quyền truy cập trang
- Thêm guard vào tất cả page (redirect 403 nếu không quyền)
- Tạo trang 403 "Không có quyền truy cập"
- (Optional) Tạo middleware chặn route

### Phase 3: Action-Level Permission (~2 giờ)
- Tạo helper kiểm tra quyền CRUD
- Wrap tất cả Server Actions với permission check
- Ẩn/disable nút Tạo/Sửa/Xóa nếu không có quyền

### Phase 4: Audit M3 Báo Giá/Đơn Hàng (~3 giờ)
- Audit M3 store: FK guard, transaction, data safety
- Fix các vấn đề critical
- Test CRUD flow: Tạo BG → Tạo ĐH → Verify

### Phase 5: Go-Live (~1 giờ)
- Deploy lên VPS
- Tạo tài khoản thật cho NV
- Hướng dẫn NV đăng nhập + đổi mật khẩu
- Test flow thực tế

---

## 5. Ma Trận Quyền Đề Xuất

| Vai trò | M0 (System) | M1 (KH, SP, NV) | M3 (BG, ĐH) | MF (Tài chính) | Khác |
|---------|-------------|------------------|--------------|-----------------|------|
| **ADMIN** | Full | Full | Full | Full | Full |
| **SALE** | ❌ | View + Create + Update | View + Create + Update | ❌ | ❌ |
| **THIET_KE** | ❌ | View only | View only | ❌ | ❌ |

> **Lưu ý:** SALE không có quyền Delete (xóa KH, xóa BG) — chỉ Admin mới được xóa.

---

## 6. Test Plan

| # | Test Case | Expected Result |
|---|-----------|-----------------|
| T1 | Admin login → thấy tất cả menu | ✅ Full access |
| T2 | Sale login → chỉ thấy M1 + M3 | ✅ Sidebar filtered |
| T3 | Sale gõ URL `/m0/security` trực tiếp | ✅ Redirect → 403 |
| T4 | Sale tạo KH mới qua Wizard | ✅ Thành công |
| T5 | Sale xóa KH | ❌ Bị chặn (không có can_delete) |
| T6 | Thiết kế login → M1 + M3 (view only) | ✅ Không thấy nút Tạo/Sửa/Xóa |
| T7 | Thiết kế gõ URL wizard tạo KH | ✅ Redirect → 403 |
| T8 | Sale tạo Báo Giá | ✅ Thành công |
| T9 | Sale xóa Báo Giá | ❌ Bị chặn |

---

## 7. Câu Hỏi Cần Owner Quyết Định

| # | Câu hỏi | Ảnh hưởng |
|---|---------|-----------|
| Q1 | Số lượng NV cần tạo tài khoản? Danh sách tên + email? | Phase 1 |
| Q2 | Quyền THIET_KE chính xác? (View only hay cần tạo/sửa?) | Phase 1 |
| Q3 | Thứ tự ưu tiên: Phân quyền trước hay Audit M3 trước? | Phase 2-4 |
| Q4 | Email NV dùng loại gì? (công ty hay cá nhân) | Phase 5 |

---

## 8. Timeline Dự Kiến

```
Phase 1 (30p) → Phase 2 (2h) → Phase 3 (2h) → Phase 4 (3h) → Phase 5 (1h)
                                                                    ↓
                                                              🎯 GO-LIVE
                                                            (~8.5 giờ tổng)
```

---

## 9. Rủi Ro & Mitigation

| Rủi ro | Xác suất | Mitigation |
|--------|----------|------------|
| NV quên mật khẩu ngày đầu | Cao | Admin reset qua UI Security |
| NV truy cập URL không quyền | Cao (hiện tại) | Phase 2 fix page guard |
| Data Báo Giá bị partial (no transaction) | Trung bình | Phase 4 audit + fix |
| NV xóa nhầm KH quan trọng | Thấp (đã có FK guard) | V0.217 đã chặn |

---

*Báo cáo này được tạo tự động bởi ERP TanPhat AI Agent — V0.217 (14/06/2026)*
