# 🔬 P0.1 Discovery Audit — DB Reality + Code Reality + Conflict Correction

> **Plan ID:** PLAN-20260614-GOLIVE-FOUNDATION-P0.1
> **Ngày:** 15/06/2026 00:10
> **MySQL:** 8.4.3 — DB: tanphat_erp_dev (82 tables)

---

## 🔴 Sửa Lại Báo Cáo Trước (Conflict Corrections)

| Item | Báo cáo trước nói | Thực tế | Sửa lại |
|------|-------------------|---------|---------|
| **Page-level guard** | "🔴 KHÔNG CÓ" | ✅ **ĐÃ CÓ** — tất cả 12 module layout.tsx đều có guard | Báo cáo trước SAI |
| **Role SALE** | "Chưa có" | ✅ **SALES** đã có trong DB | Tên khác: SALES (không phải SALE) |
| **Middleware cần thiết** | "Cần tạo" | ❌ KHÔNG CẦN — layout guard đã đủ | Không cần middleware |

---

## Quick Summary

| Metric | Giá trị |
|--------|---------|
| **DB Tables** | 82 tables |
| **User accounts** | 1 (Admin) |
| **Employees** | 30 (ALL chưa link user) |
| **Roles defined** | 6 (ADMIN, CEO, HR, KE_TOAN, SALES, USER) |
| **THIET_KE role** | ❌ Chưa tạo |
| **Permission tables** | 4-layer ready (menu/action/data/field + audit log) |
| **Action permissions** | 🔴 Table tồn tại nhưng RỖNG |
| **Data permissions** | 🔴 Table tồn tại nhưng RỖNG |
| **Field permissions** | 🔴 Table tồn tại nhưng RỖNG |
| **Page-level guard** | ✅ **ĐÃ CÓ** (12/12 modules) |
| **Action-level guard** | 🔴 Code CHƯA enforce |
| **Customer ownership field** | ✅ `sale_phu_trach` (INT) đã có |
| **Sensitive fields (giá vốn)** | 5 fields trong 5 tables |

---

## Phát Hiện Quan Trọng

### ✅ Hệ thống tốt hơn kỳ vọng:
1. **Page guard đã có** — mọi module đều chặn URL trực tiếp
2. **4-layer permission schema** — sẵn sàng cho enterprise RBAC
3. **Customer ownership field** — `sale_phu_trach` đã có trong DB
4. **Permission audit log** — table `permission_log` sẵn sàng

### 🔴 Cần implement:
1. SALES chưa có menu permissions (0 records)
2. THIET_KE role chưa tồn tại
3. Action/Data/Field permission tables RỖNG
4. Server Actions không check quyền
5. 30 NV chưa có tài khoản đăng nhập
6. Customer ownership chưa filter theo SALE
7. Giá vốn chưa mask cho SALE/THIET_KE/KE_TOAN

---

## Sensitive Fields Cần Bảo Vệ

| Table | Field | Ai được xem |
|-------|-------|-------------|
| bao_gia_option | gia_von | ADMIN, CEO |
| don_hang_item | gia_von | ADMIN, CEO |
| material_item | gia_von_trung_binh | ADMIN, CEO |
| dm_profit_margin_rule | loi_nhuan_percent | ADMIN, CEO |
| pricing_quote_history | gia_von | ADMIN, CEO |

---

## LSX/PDI State Machine

| Entity | States | THIET_KE được sửa | Bị chặn |
|--------|--------|-------------------|---------|
| LSX | draft → in_progress → done/cancelled | draft only | Các trạng thái khác |
| PDI | draft → da_dieu_in → hen_lich_in → da_in_hoan_tat/huy | draft only | Sau điều in |

---

## Implementation Plan (9 Steps, ~8 giờ)

| # | Step | Effort | Status |
|---|------|--------|--------|
| 1 | Seed THIET_KE role + SALES/CEO/KE_TOAN permissions | 30p | ⏳ |
| 2 | Seed action permissions (ownership, cost, production) | 30p | ⏳ |
| 3 | Seed field permissions (giá vốn masking) | 15p | ⏳ |
| 4 | Tạo 14 test users + link employees | 45p | ⏳ |
| 5 | Tạo action-permission helper | 1h | ⏳ |
| 6 | Wrap Server Actions với permission checks | 2h | ⏳ |
| 7 | Implement customer ownership filter | 1.5h | ⏳ |
| 8 | Implement cost/profit field masking | 1h | ⏳ |
| 9 | Version bump + changelog + report | 30p | ⏳ |

---

## Kết Luận

> 🚫 **NOT READY** cho Go-Live — nhưng hệ thống **TỐT HƠN KỲ VỌNG**.
> 
> Page guard ✅ đã có. Schema 4-layer ✅ sẵn sàng.
> Chỉ cần seed data + implement action guard + ownership filter + cost masking.
> 
> Chờ Owner xác nhận plan trước khi implement.

---

*Báo cáo tạo bởi ERP TanPhat AI Agent — 15/06/2026 00:10*
