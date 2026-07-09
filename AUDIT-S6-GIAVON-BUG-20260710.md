# 🔍 AUDIT S6 — CEO Không Xem Được Giá Vốn/Lợi Nhuận

> **Plan ID:** PLAN-20260709-s6-giavon-bug-audit  
> **Date:** 10/07/2026  
> **Mode:** READ-ONLY AUDIT  

---

## R1 — Nơi hiển thị giá vốn/lợi nhuận

| # | Location | Table.Field | UI Component |
|---|----------|-------------|-------------|
| 1 | `/m3/bao-gia` | bao_gia_option.gia_von | bao-gia-client.tsx:3093 — "Giá vốn:" |
| 2 | `/m3/bao-gia` | bao_gia_option.gia_von | bao-gia-client.tsx:4039 — Form edit field |
| 3 | `/m3/don-hang` | don_hang_item.gia_von | Masked server-side (actions.ts:41) |
| 4 | `/m3/tinh-gia-manual` | Manual calc output | tinh-gia-manual-client.tsx:1535 — "💰 Giá Vốn" |
| 5 | `/m3/tinh-gia-admin` | dm_profit_margin_rule | tinh-gia-admin-client.tsx:2012 — "Biên Lợi Nhuận" |
| 6 | `/m1/material-item` | material_item.gia_von_trung_binh | material-item-client.tsx:722 — "Giá Vốn TB" |

---

## R2 — Cơ chế bảo mật giá vốn

### Server-side Masking Flow:
1. `bao-gia/actions.ts:49` gọi `maskSensitiveFields(options, "bao_gia_option", ["gia_von"])`
2. `don-hang/actions.ts:41` gọi `maskSensitiveFields(items, "don_hang_item", ["gia_von"])`
3. `maskSensitiveFields` → `canViewField(email, table, field)`
4. `canViewField` (action-permission.ts:116-142):
   - **Line 125: `if (ctx.isAdmin) return true`** → Admin/CEO bypass, thấy hết
   - **Line 141: `if (!rows?.[0]) return true`** → Không có rule = cho xem
   - **Line 142:** Chỉ mask khi `can_view=0 AND mask_value=1`

### role_field_permission (DB thật):
```
KE_TOAN  | bao_gia_option | gia_von | can_view=0, mask=1
KE_TOAN  | don_hang_item  | gia_von | can_view=0, mask=1
SALES    | bao_gia_option | gia_von | can_view=0, mask=1
SALES    | don_hang_item  | gia_von | can_view=0, mask=1
THIET_KE | bao_gia_option | gia_von | can_view=0, mask=1
THIET_KE | don_hang_item  | gia_von | can_view=0, mask=1
```
**KHÔNG CÓ rule nào cho CEO** → default = cho xem (line 141).

### CEO Role Config:
```
user_role_mapping: lienntk@intanphat.com → CEO
dm_vai_tro: CEO → la_admin=1
security-store.ts:343: isAdmin = roles.some(r => r.la_admin === 1) → TRUE for CEO
```

---

## R3 — Root Cause Analysis

### KẾT LUẬN: Code logic ĐÚNG — CEO **PHẢI** xem được giá vốn

**Logic chain:**
1. CEO login → `resolveSessionEmail()` → `lienntk@intanphat.com`
2. `getSecurityContext()` → roles=[CEO] → `la_admin=1` → `isAdmin=true`
3. `canViewField()` → `ctx.isAdmin=true` → **return true (bypass all checks)**
4. `maskSensitiveFields()` → **không mask gì** → `gia_von` giữ nguyên
5. UI render `option.gia_von` → **hiển thị số**

### Possible Root Causes (nếu CEO vẫn không thấy trên VPS):

| # | Hypothesis | Verification |
|---|-----------|--------------|
| H1 | **Session cookie stale** — CEO login bằng session cũ trước khi role được gán (05/07) | Logout + login lại sẽ fix |
| H2 | **VPS chưa deploy code mới** — version trên VPS không có `la_admin=1` check | Check VPS `dm_vai_tro` table |
| H3 | **Browser cache** — old SSR HTML cached | Hard refresh (Ctrl+Shift+R) |
| H4 | **VPS `dm_vai_tro` khác local** — CEO role trên VPS chưa có `la_admin=1` | `SELECT la_admin FROM dm_vai_tro WHERE ma_vai_tro='CEO'` trên VPS |

### Đề xuất Fix (đơn giản):
1. **Trước tiên:** Yêu cầu CEO logout + login lại + hard refresh
2. **Nếu vẫn lỗi:** Check VPS DB `dm_vai_tro` có `la_admin=1` cho CEO không
3. **Nếu VPS thiếu:** `UPDATE dm_vai_tro SET la_admin=1 WHERE ma_vai_tro='CEO'`

---

## S1-S6 Status Update

| # | Test | Status |
|---|------|--------|
| S1 | Login | ⏳ Chưa test thật |
| S2 | Change PW | ⏳ Chưa test thật |
| S3 | Re-login | ⏳ Chưa test thật |
| S4 | Menu CEO | ⏳ Chưa test thật |
| S5 | View all customers | ⏳ Chưa test thật |
| S6 | **Giá vốn/lợi nhuận** | **Code logic OK — cần verify session/VPS** |

*Không chứa source code chi tiết, credentials.*
