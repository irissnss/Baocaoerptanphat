# 🔍 Audit Report — `chung_tu_ke_toan` Table

> **Plan ID:** PLAN-20260706-chung-tu-ke-toan-audit
> **Date:** 06/07/2026
> **Mode:** READ-ONLY AUDIT — không tạo bảng, không sửa code

---

## R1 — Xác nhận tồn tại hay không

### SQL kiểm tra

```sql
SHOW TABLES LIKE '%chung_tu%';   -- 0 results
SHOW TABLES LIKE '%ke_toan%';    -- 0 results
SHOW TABLES LIKE '%accounting%'; -- 0 results
SHOW TABLES LIKE '%journal%';    -- 0 results
SHOW TABLES LIKE '%voucher%';    -- 0 results
SHOW TABLES LIKE '%ledger%';     -- 0 results
SHOW TABLES LIKE '%entry%';      -- 0 results
SHOW TABLES LIKE '%khoa_so%';    -- 0 results
SHOW TABLES LIKE '%lich_su_cong_no%'; -- 1 result (lich_su_cong_no — but this is debt history, not accounting voucher)
```

### Kết luận R1

**`chung_tu_ke_toan` KHÔNG TỒN TẠI** trong DB `tanphat_erp_dev`. Không có bảng nào khác thay thế vai trò này.

Bảng liên quan gần nhất là `lich_su_cong_no` — nhưng đây chỉ là lịch sử thay đổi công nợ, không phải chứng từ kế toán.

---

## R2 — Grep code: ai tham chiếu `chung_tu_ke_toan`?

### Kết quả grep toàn bộ `src/`

#### 2.1 SQL statements tham chiếu bảng `chung_tu_ke_toan`

```
grep -rn "(INSERT INTO|SELECT.*FROM|UPDATE|DELETE FROM).*chung_tu_ke_toan" src/
→ 0 results
```

**Không có bất kỳ SQL statement nào** INSERT/SELECT/UPDATE/DELETE vào bảng `chung_tu_ke_toan`. **Không có runtime crash risk.**

#### 2.2 String literal `chung_tu_ke_toan` (3 hits — tất cả là comments)

```
src/types/mf.ts:5       — JSDoc comment: "giao_dich_ngan_hang, chung_tu_ke_toan, doi_chieu_cong_no"
src/lib/mock-mf.ts:5    — JSDoc comment: identical
src/lib/mf-store.ts:5   — JSDoc comment: identical
```

#### 2.3 TypeScript interface `ChungTuKeToan` (defined but dead)

```
src/types/mf.ts:162     — export interface ChungTuKeToan { ... } (12 fields — NOT 23 as in Notion spec)
src/lib/mock-mf.ts:14   — import ChungTuKeToan
src/lib/mock-mf.ts:207  — export const mockChungTuKeToan: ChungTuKeToan[] = []  ← EMPTY ARRAY, never populated
```

**No file imports or uses `mockChungTuKeToan` at runtime.** The interface exists as a type stub only.

#### 2.4 Related fields in code (NOT the missing table)

These are **DIFFERENT** from `chung_tu_ke_toan` table:

| Field | Context | Used in | Purpose |
|-------|---------|---------|---------|
| `chung_tu_dinh_kem` | Column on `phieu_thu` and `phieu_chi` tables | CRUD in mf-store.ts | Stores attachment reference (text), NOT a FK to `chung_tu_ke_toan` |
| `id_chung_tu_nguon` | Column on `cong_no` table | Query/INSERT in mf-store.ts | FK to source document (don_hang, nhap_hang, etc.), NOT to `chung_tu_ke_toan` |
| `loai_chung_tu_nguon` | Column on `cong_no` table | Query/INSERT in mf-store.ts | Source document type discriminator |

These fields are **working correctly** with existing tables. They do not depend on `chung_tu_ke_toan`.

#### 2.5 Critical workflow check: "Phiếu thu approved → tạo chung_tu_ke_toan"

**Notion spec (MF-1, line 269):**
```
5. Approve phiếu thu
6. System:
   - Update cong_no_khach_hang (giảm nợ)          ← IMPLEMENTED (mf-store.ts:766-777, collectPersistedPhieuThu)
   - Update tai_khoan_ngan_hang (tăng số dư)       ← NOT IMPLEMENTED (no bank balance table exists either)
   - Tạo chung_tu_ke_toan                          ← NOT IMPLEMENTED (table doesn't exist)
```

**Actual code (`approvePersistedPhieuThu`, mf-store.ts:742-766):**
- Sets `trang_thai = 'approved'`, `nguoi_duyet`, `ngay_duyet` ✅
- Does NOT create any `chung_tu_ke_toan` record ✅ (no crash, just missing feature)
- R6 wiring to `bien_ban_nghiem_thu` was added in V0.230

**Actual code (`collectPersistedPhieuThu`, mf-store.ts:769-787):**
- Updates `cong_no.so_tien_con_lai` and derives new status ✅
- Does NOT create any `chung_tu_ke_toan` record ✅ (no crash)
- Does NOT update `tai_khoan_ngan_hang` (that table structure unknown/unaudited)

### R2 conclusion

**No runtime crash risk.** The `chung_tu_ke_toan` table is referenced in:
1. Comments (3 files) — harmless
2. TypeScript interface (dead code, empty mock array, never imported at runtime)
3. Notion spec/docs (design intent, not yet implemented)

The approve workflow in Notion specifies 3 side effects, of which only 1 is implemented (cong_no update). The other 2 (bank balance, accounting voucher) are **not implemented** — but they are **silent gaps**, not crash bugs.

---

## R3 — Khóa sổ cuối tháng

### Notion spec (MF-2, lines 320-330)

```
Workflow 3: Khóa sổ cuối tháng
1. Review tất cả phiếu thu/chi trong tháng
2. Approve tất cả chứng từ kế toán
3. Verify số dư công nợ
4. Đối soát bank statement
5. Khóa sổ: UPDATE chung_tu_ke_toan SET khoa_so = TRUE, ngay_khoa_so = NOW()
   WHERE thang_ke_toan = X AND nam_ke_toan = Y AND trang_thai = 'approved'
6. Sau khi khóa → IMMUTABLE
```

### Code grep

```
grep -rni "khoa.so|lock.*period|close.*month|period.*close" src/
→ 0 results

grep -rni "immutable|frozen|locked|lock" src/lib/mf-store.ts
→ 0 results
```

### R3 conclusion

**Khóa sổ cuối tháng CHƯA ĐƯỢC IMPLEMENT.** Không có:
- Bảng `chung_tu_ke_toan` để chứa `khoa_so` flag
- Code nào kiểm tra `khoa_so` trước khi cho phép sửa phiếu thu/chi cũ
- API/route/button nào để trigger khóa sổ
- Bất kỳ cơ chế thay thế nào

Hiện tại phiếu thu/chi **có thể bị sửa/hủy bất kỳ lúc nào** (chỉ guard bằng `trang_thai !== 'draft'` cho approve/collect, không guard theo tháng kế toán).

---

## Tổng kết

### Phát hiện chính

| # | Phát hiện | Mức độ | Giải thích |
|---|----------|--------|-----------|
| F1 | `chung_tu_ke_toan` table KHÔNG tồn tại | 🔴 GAP vs SSOT | Notion MF spec ghi rõ là 1/8 bảng lõi (23 cột). DB thật = 0. |
| F2 | TypeScript interface có nhưng dead | 🟡 INFO | `ChungTuKeToan` in `mf.ts` (12 fields, vs Notion 23 fields). Mock empty array. Never used at runtime. |
| F3 | Approve workflow thiếu 2/3 side effects | 🟠 DESIGN GAP | Approve phieu_thu chỉ update cong_no. Thiếu: tạo chung_tu_ke_toan + update bank balance. |
| F4 | Khóa sổ cuối tháng = 0% implemented | 🔴 GAP vs SSOT | Notion ghi workflow 6 bước. Code = 0 references. Phiếu thu/chi có thể sửa không giới hạn thời gian. |
| F5 | `tai_khoan_ngan_hang` balance update cũng thiếu | 🟡 RELATED | Approve phieu_thu không update bank balance (chưa audit `tai_khoan_ngan_hang` sâu). |

### Đánh giá tác động thực tế

- **Runtime:** Không crash. Không bug. Code hoạt động bình thường với dữ liệu hiện tại.
- **Data integrity:** Phiếu thu/chi thật **KHÔNG bị chặn** bởi khóa sổ → rủi ro: user có thể vô tình sửa/hủy phiếu của tháng đã đóng.
- **Accounting audit trail:** Không có bảng ghi nhận chứng từ kế toán → thiếu báo cáo tài chính kiểu sổ cái.

### Quyết định cần Coordinator/Owner

> ⚠️ **PROPOSED — chờ Coordinator duyệt.** Không tự hành động.

**Option A — Tạo bảng `chung_tu_ke_toan` (23 cột, theo Notion spec):**
- Pro: Đồng bộ SSOT, enable khóa sổ, audit trail kế toán
- Con: Cần thêm code auto-create khi approve phiếu thu/chi, migration data cũ

**Option B — Postpone, ghi nhận gap:**
- Pro: Không phá logic hiện tại, tập trung vào MF features khác trước
- Con: SSOT drift tiếp tục, khóa sổ vẫn không có

**Option C — Remove từ SSOT (nếu chưa cần):**
- Pro: Dọn dẹp spec vs reality
- Con: Mất audit trail kế toán khi cần scale

> Coordinator/Owner cần chọn 1 trong 3 options trước khi agent hành động.
