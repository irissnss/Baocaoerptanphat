# 📋 Implementation Report — V0.230: Nghiệm Thu & Đối Chiếu Công Nợ (V3.44)

> **Plan ID:** PLAN-20260705-nghiem-thu-doi-chieu-cong-no (PL5 close-out)
> **Version:** V0.230 (version.ts MINOR=230, verified 06/07/2026)
> **Date:** 06/07/2026
> **Decision:** Option C — Hybrid (Owner approved 05/07/2026 23:03 Asia/Ho_Chi_Minh)
> **Notion spec:** "V3.44 Draft — Nghiệm thu & Đối chiếu công nợ (Option C)" (page d7c1c080)

---

## 1. DB EVIDENCE (SHOW CREATE TABLE — paste nguyên văn)

### bien_ban_nghiem_thu

```sql
CREATE TABLE `bien_ban_nghiem_thu` (
  `ma_nghiem_thu` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ten_nghiem_thu` varchar(200) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT 'Ten hien thi, VD: Dot 1 - Thang 3/2026',
  `ma_khach_hang` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `tu_ngay` date NOT NULL,
  `den_ngay` date NOT NULL,
  `no_dau_ky` decimal(18,2) DEFAULT '0.00',
  `tong_truoc_vat` decimal(18,2) DEFAULT '0.00' COMMENT 'Tong gia tri truoc VAT',
  `tien_vat` decimal(18,2) DEFAULT '0.00' COMMENT 'Tien thue GTGT',
  `tong_phat_sinh` decimal(18,2) DEFAULT '0.00' COMMENT 'Tong sau VAT = tong_truoc_vat + tien_vat',
  `da_thanh_toan` decimal(18,2) DEFAULT '0.00',
  `con_phai_thanh_toan` decimal(18,2) DEFAULT '0.00',
  `danh_sach_so_phieu_gh` json DEFAULT NULL,
  `trang_thai` enum('draft','da_ky','cancelled') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'draft',
  `nguoi_lap` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ngay_lap` datetime NOT NULL,
  `nguoi_ky` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `ngay_ky` datetime DEFAULT NULL,
  `file_da_ky` json DEFAULT NULL COMMENT 'File scan bien ban da ky (JSON array URL)',
  `ghi_chu` text COLLATE utf8mb4_unicode_ci,
  `ngay_tao` datetime NOT NULL,
  `nguoi_tao` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ngay_sua` datetime DEFAULT NULL,
  `nguoi_sua` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`ma_nghiem_thu`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='Bien ban nghiem thu khach hang (V3.44)'
```

**Column count:** 22 columns. **PK:** `ma_nghiem_thu` (VARCHAR 50).

### cong_no_doi_chieu

```sql
CREATE TABLE `cong_no_doi_chieu` (
  `ma_doi_chieu` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ma_khach_hang` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ngay_chot` date NOT NULL,
  `tong_no_duoi_30d` decimal(18,2) DEFAULT '0.00',
  `tong_no_31_60d` decimal(18,2) DEFAULT '0.00',
  `tong_no_tren_60d` decimal(18,2) DEFAULT '0.00',
  `tong_du_no` decimal(18,2) DEFAULT '0.00',
  `trang_thai` enum('draft','da_gui','da_xac_nhan','tu_choi','qua_han_phan_hoi','cancelled') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'draft',
  `ngay_gui` datetime DEFAULT NULL,
  `han_phan_hoi` datetime DEFAULT NULL,
  `so_ngay_phan_hoi` int DEFAULT '3',
  `quy_tac_im_lang_chap_nhan` tinyint(1) DEFAULT '1',
  `file_da_ky` json DEFAULT NULL COMMENT 'File scan bien ban da ky/phan hoi (JSON array URL)',
  `nguoi_lap` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ghi_chu` text COLLATE utf8mb4_unicode_ci,
  `ngay_tao` datetime NOT NULL,
  `nguoi_tao` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ngay_sua` datetime DEFAULT NULL,
  `nguoi_sua` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`ma_doi_chieu`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='Doi chieu va xac nhan cong no khach hang (V3.44)'
```

**Column count:** 19 columns. **PK:** `ma_doi_chieu` (VARCHAR 50).

### dm_form_mau seed

```
id  ma_form_mau             ten_form_mau                                     doi_tuong               trang_thai
33  FORM_NGHIEM_THU         Biên bản nghiệm thu                             bien_ban_nghiem_thu     active
34  FORM_XAC_NHAN_CONG_NO   Biên bản đối chiếu và xác nhận công nợ          cong_no_doi_chieu       active
```

**Adaptation vs Notion INSERT:** Notion spec dùng `ma_form`, `ten_form`, `template_file_url`, `is_active`, `nguoi_tao='system'` — tất cả sai schema thật. Đã adapt: `ma_form_mau`, `ten_form_mau`, `template_html`, `trang_thai='active'`, `nguoi_tao=1 (INT)`.

---

## 2. SSOT DECISIONS

### Snapshot immutability

| Table | Status | Immutable after | Fields locked |
|-------|--------|-----------------|---------------|
| `bien_ban_nghiem_thu` | `da_ky` | `ngay_ky` set | `tong_truoc_vat`, `tien_vat`, `tong_phat_sinh`, `no_dau_ky`, `danh_sach_so_phieu_gh`, `nguoi_ky`, `ngay_ky` — snapshot frozen. `da_thanh_toan` + `con_phai_thanh_toan` are **derived** and may update from phieu_thu. |
| `cong_no_doi_chieu` | `da_xac_nhan` | workflow terminal | All fields frozen. No further transitions allowed. |

### Derived fields

| Field | Formula | Source |
|-------|---------|--------|
| `da_thanh_toan` | `SUM(phieu_thu.so_tien)` WHERE `lien_ket_type='bien_ban_nghiem_thu' AND lien_ket_id=ma_nghiem_thu AND trang_thai='approved'` | `phieu_thu` table |
| `con_phai_thanh_toan` | `tong_phat_sinh + no_dau_ky - da_thanh_toan` | Derived from header + phieu_thu sum |
| `tong_phat_sinh` | `tong_truoc_vat + tien_vat` | Set at creation time from PGH items |

### Aging bucket computation (đối chiếu)

```
duoi_30d:  DATEDIFF(ngay_chot, han_thanh_toan) <= 30
31_60d:    31 <= diff <= 60
tren_60d:  diff > 60
```

Source: `cong_no.han_thanh_toan` vs `ngay_chot`. Null `han_thanh_toan` → classified as `duoi_30d`.

### Architecture decisions

- `chung_tu_ke_toan` table does NOT exist in DB → Đối chiếu reads from `cong_no` + `phieu_thu` + `don_hang` instead (Owner approved Q2).
- `dm_form_mau` schema adapted to match real DB (Owner approved Q1).

---

## 3. IMPLEMENTATION / CHANGES (repo paths)

### R4: `/mf/doi-chieu` — Đối chiếu công nợ (replace placeholder)

| File | Action | Description |
|------|--------|-------------|
| `src/lib/mf-doi-chieu-store.ts` | NEW (220 lines) | CRUD, aging buckets, workflow state machine. Raw SQL via `query()`. |
| `src/app/mf/doi-chieu/actions.ts` | NEW (68 lines) | 6 Server Actions: list, get, create, update, transition, computeAging. RBAC guarded. |
| `src/app/mf/doi-chieu/page.tsx` | MODIFIED (24 lines, was 31-line placeholder) | Server Component loads from `listDoiChieu()`. |
| `src/app/mf/doi-chieu/doi-chieu-client.tsx` | NEW (188 lines) | Client UI: table + 4 stat cards + create dialog + workflow buttons. |

### R5: `/mf/nghiem-thu` — Biên bản nghiệm thu (new route)

| File | Action | Description |
|------|--------|-------------|
| `src/lib/mf-nghiem-thu-store.ts` | NEW (301 lines) | CRUD, PGH item aggregation, nợ đầu kỳ, workflow, `updateThanhToanNghiemThu()`. |
| `src/app/mf/nghiem-thu/actions.ts` | NEW (62 lines) | 6 Server Actions: list, get, create, update, transition, preview. |
| `src/app/mf/nghiem-thu/page.tsx` | NEW (24 lines) | Server Component. |
| `src/app/mf/nghiem-thu/nghiem-thu-client.tsx` | NEW (195 lines) | Client UI: table + stat cards + create dialog. |

### R6: Wire phieu_thu → bien_ban_nghiem_thu

| File | Action | Description |
|------|--------|-------------|
| `src/lib/mf-store.ts` | MODIFIED (+11 lines, at line 756) | In `approvePersistedPhieuThu()`: checks `lien_ket_type === 'bien_ban_nghiem_thu'` → calls `updateThanhToanNghiemThu()`. |

### R7: Version bump

| File | Action | Description |
|------|--------|-------------|
| `src/lib/version.ts` | MODIFIED (line 26) | MINOR: 229 → 230 |

### Migration script

| File | Action | Description |
|------|--------|-------------|
| `scripts/migrations/v344-nghiem-thu-doi-chieu.ts` | NEW | Idempotent migration: `CREATE TABLE IF NOT EXISTS` + `INSERT IGNORE`. |

---

## 4. TRACE PACK

### Files changed (complete list — 11 files)

```
NEW  src/lib/mf-doi-chieu-store.ts           (220 lines, 7563 bytes)
NEW  src/lib/mf-nghiem-thu-store.ts          (301 lines, 9938 bytes)
NEW  src/app/mf/doi-chieu/actions.ts         (68 lines, 2008 bytes)
NEW  src/app/mf/doi-chieu/doi-chieu-client.tsx (188 lines, 9472 bytes)
MOD  src/app/mf/doi-chieu/page.tsx           (24 lines, 646 bytes — was 31-line placeholder)
NEW  src/app/mf/nghiem-thu/actions.ts        (62 lines, 1964 bytes)
NEW  src/app/mf/nghiem-thu/page.tsx          (24 lines, 608 bytes)
NEW  src/app/mf/nghiem-thu/nghiem-thu-client.tsx (195 lines, 9611 bytes)
MOD  src/lib/mf-store.ts                     (+11 lines at line 756)
MOD  src/lib/version.ts                      (MINOR 229→230)
NEW  scripts/migrations/v344-nghiem-thu-doi-chieu.ts (idempotent)
```

### Key snippet — `updateThanhToanNghiemThu` (mf-nghiem-thu-store.ts:277-300)

```typescript
export async function updateThanhToanNghiemThu(maNghiemThu: string): Promise<void> {
  const rows = await query<Array<{ total: number }>>(
    `SELECT IFNULL(SUM(so_tien), 0) AS total
     FROM phieu_thu
     WHERE lien_ket_type = 'bien_ban_nghiem_thu'
       AND lien_ket_id = ?
       AND trang_thai = 'approved'`,
    [maNghiemThu]
  )
  const daTT = Number(rows[0]?.total ?? 0)
  const nt = await getNghiemThu(maNghiemThu)
  if (!nt) return
  const conPhai = nt.tong_phat_sinh + nt.no_dau_ky - daTT
  await query(
    `UPDATE bien_ban_nghiem_thu SET da_thanh_toan = ?, con_phai_thanh_toan = ? WHERE ma_nghiem_thu = ?`,
    [daTT, conPhai, maNghiemThu]
  )
}
```

### Key snippet — R6 wiring (mf-store.ts:756-764)

```typescript
  // R6: Wire phieu_thu → bien_ban_nghiem_thu (V3.44)
  if (existing.lien_ket_type === "bien_ban_nghiem_thu" && existing.lien_ket_id) {
    try {
      const { updateThanhToanNghiemThu } = await import("@/lib/mf-nghiem-thu-store")
      await updateThanhToanNghiemThu(existing.lien_ket_id)
    } catch {
      // Silent fail: nghiem-thu module may not be loaded
    }
  }
```

### Key snippet — aging buckets (mf-doi-chieu-store.ts:64-90)

```typescript
export async function computeAgingBuckets(maKhachHang: string, ngayChot: string) {
  const rows = await query<...>(
    `SELECT so_tien_con_lai, han_thanh_toan
     FROM cong_no
     WHERE id_khach_hang = (SELECT id FROM dm_khach_hang WHERE ma_khach_hang = ? LIMIT 1)
       AND trang_thai IN ('chua_thu','cho_thu','qua_han')
       AND so_tien_con_lai > 0`,
    [maKhachHang]
  )
  let duoi30 = 0, t31_60 = 0, tren60 = 0
  const chot = new Date(ngayChot)
  for (const r of rows) {
    const amt = Number(r.so_tien_con_lai) || 0
    if (!r.han_thanh_toan) { duoi30 += amt; continue }
    const days = Math.floor((chot.getTime() - new Date(r.han_thanh_toan).getTime()) / 86400000)
    if (days <= 30) duoi30 += amt
    else if (days <= 60) t31_60 += amt
    else tren60 += amt
  }
  return { tong_no_duoi_30d: duoi30, tong_no_31_60d: t31_60, tong_no_tren_60d: tren60, tong_du_no: duoi30+t31_60+tren60 }
}
```

### SQL executed in this session

```sql
-- R1
CREATE TABLE bien_ban_nghiem_thu (...) -- 22 columns, SUCCESS

-- R2
CREATE TABLE cong_no_doi_chieu (...) -- 19 columns, SUCCESS

-- R3
INSERT INTO dm_form_mau (...) VALUES ('FORM_NGHIEM_THU',...), ('FORM_XAC_NHAN_CONG_NO',...) -- id=33,34, SUCCESS
```

### TSC compile

```
npx tsc --noEmit → 0 errors
```

---

## 5. PASS/FAIL + TEST CASES

### Test 1: Tạo `bien_ban_nghiem_thu` draft từ phieu_giao_hang

| | Detail |
|---|--------|
| **Expected** | Insert row vào `bien_ban_nghiem_thu` với `trang_thai='draft'`, aggregate items từ PGH trong kỳ/KH, tính `tong_truoc_vat` + `tien_vat` + `tong_phat_sinh` |
| **Actual** | ⚠️ NOT PROVEN — Code logic đúng (verified by code review + TSC compile), nhưng chưa có smoke test trên running app. Cần `npm run dev` → UI test tạo draft thật. |
| **Status** | ⚠️ PARTIAL (code ready, runtime test pending Owner) |

### Test 2: `da_ky` → khóa snapshot

| | Detail |
|---|--------|
| **Expected** | `transitionNghiemThu(ma, 'da_ky')` → set `nguoi_ky`, `ngay_ky`, `trang_thai='da_ky'`. Sau đó `updateNghiemThu()` reject vì `trang_thai !== 'draft'`. |
| **Actual** | ⚠️ NOT PROVEN — Code review confirms: `updateNghiemThu()` returns null if `existing.trang_thai !== 'draft'` (line ~228). `transitionNghiemThu()` sets `nguoi_ky`/`ngay_ky` khi target=`da_ky` (line ~261). Logic đúng nhưng chưa test runtime. |
| **Status** | ⚠️ PARTIAL (logic verified by code review) |

### Test 3: Phiếu thu liên kết → `da_thanh_toan` cập nhật

| | Detail |
|---|--------|
| **Expected** | Approve `phieu_thu` với `lien_ket_type='bien_ban_nghiem_thu'` → `updateThanhToanNghiemThu()` chạy → `da_thanh_toan` = SUM approved, `con_phai_thanh_toan` = `tong_phat_sinh + no_dau_ky - da_thanh_toan` |
| **Actual** | ⚠️ NOT PROVEN — Code wiring confirmed (mf-store.ts:756-764 + mf-nghiem-thu-store.ts:277-300). SQL queries đúng. Chưa test E2E vì cần phiếu thu linked thật. |
| **Status** | ⚠️ PARTIAL (code wiring verified) |

### Test 4: `cong_no_doi_chieu` draft → `da_gui` → aging buckets

| | Detail |
|---|--------|
| **Expected** | Create draft → `createDoiChieu()` auto-computes aging buckets từ `cong_no`. Transition `da_gui` → auto-set `ngay_gui` + `han_phan_hoi`. |
| **Actual** | ⚠️ NOT PROVEN — Code review: `createDoiChieu()` calls `computeAgingBuckets()` (line ~135). `transitionDoiChieu()` with `da_gui` sets `ngay_gui=NOW()` + `han_phan_hoi=DATE_ADD(NOW(), INTERVAL N DAY)` (line ~190). Logic đúng, chưa test runtime. |
| **Status** | ⚠️ PARTIAL (code logic verified) |

### Summary

| R# | Nội dung | Status | Evidence type |
|----|---------|--------|---------------|
| R1 | Migration `bien_ban_nghiem_thu` | ✅ PASS | `SHOW CREATE TABLE` — nguyên văn |
| R2 | Migration `cong_no_doi_chieu` | ✅ PASS | `SHOW CREATE TABLE` — nguyên văn |
| R3 | Seed `dm_form_mau` | ✅ PASS | `SELECT` result — id=33,34 |
| R4 | `/mf/doi-chieu` implement | ⚠️ PARTIAL | Code exists, TSC=0 errors, no runtime test |
| R5 | `/mf/nghiem-thu` NEW | ⚠️ PARTIAL | Code exists, TSC=0 errors, no runtime test |
| R6 | Phiếu thu linkage | ⚠️ PARTIAL | Code wiring verified, no E2E test |
| R7 | Version sync | ✅ PASS | `version.ts` MINOR=230, changelog 4 entries added |
| R8 | Report update | ✅ PASS | This file + MODULE-PROGRESS.md updated |

> **Lưu ý trung thực:** R4, R5, R6 ghi ⚠️ PARTIAL vì có code + compile thành công nhưng **chưa chạy runtime smoke test** (`npm run dev` + browser). Owner cần test UI thật trước khi coi là ✅ PASS hoàn toàn.
