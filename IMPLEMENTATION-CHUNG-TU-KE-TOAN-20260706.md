# 📋 Implementation Report — V0.231: `chung_tu_ke_toan` (V3.44 Option A)

> **Plan ID:** PLAN-20260706-chung-tu-ke-toan-audit (continued from audit → build)
> **Version:** V0.231
> **Date:** 06/07/2026

---

## 1. DB EVIDENCE

### SHOW CREATE TABLE chung_tu_ke_toan

```sql
CREATE TABLE `chung_tu_ke_toan` (
  `id` int NOT NULL AUTO_INCREMENT,
  `so_chung_tu` varchar(255) NOT NULL,
  `ngay_chung_tu` datetime NOT NULL,
  `loai_chung_tu` varchar(50) NOT NULL COMMENT '14 loai: phieu_thu, phieu_chi, ...',
  `phieu_thu_id` int DEFAULT NULL COMMENT 'FK phieu_thu.id',
  `phieu_chi_id` int DEFAULT NULL COMMENT 'FK phieu_chi.id',
  `so_tien` decimal(15,2) NOT NULL,
  `noi_dung` text,
  `thang_ke_toan` int NOT NULL COMMENT '1-12',
  `nam_ke_toan` int NOT NULL,
  `trang_thai` text NOT NULL COMMENT 'draft, approved, cancelled',
  `khoa_so` tinyint(1) NOT NULL DEFAULT '0',
  `ngay_khoa_so` datetime DEFAULT NULL,
  `nguoi_lap` varchar(100) DEFAULT NULL,
  `nguoi_duyet` varchar(100) DEFAULT NULL,
  `ngay_duyet` datetime DEFAULT NULL,
  `ghi_chu` text,
  `file_dinh_kem` text COMMENT 'JSON array URLs',
  `ma_lien_ket` varchar(50) DEFAULT NULL,
  `loai_lien_ket` varchar(50) DEFAULT NULL,
  `ngay_tao` text NOT NULL,
  `nguoi_tao` int DEFAULT NULL,
  `nguoi_tao_email` varchar(100) DEFAULT NULL,
  `ngay_sua` text,
  `nguoi_sua` int DEFAULT NULL,
  `nguoi_sua_email` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `so_chung_tu` (`so_chung_tu`),
  KEY `idx_ctkt_phieu_thu` (`phieu_thu_id`),
  KEY `idx_ctkt_phieu_chi` (`phieu_chi_id`),
  KEY `idx_ctkt_thang_nam` (`thang_ke_toan`,`nam_ke_toan`),
  KEY `idx_ctkt_khoa_so` (`khoa_so`)
) ENGINE=InnoDB
```

**26 columns.** PK=`id` AUTO_INCREMENT. UNIQUE=`so_chung_tu`.

---

## 2. SSOT DECISIONS

| Decision | Reasoning |
|----------|-----------|
| 26 columns (not 23 Notion) | Real convention requires `nguoi_tao_email`, `nguoi_sua`, `nguoi_sua_email` pairs |
| `trang_thai` = TEXT (not ENUM) | Matches phieu_thu/phieu_chi convention |
| `ngay_tao` = TEXT (not DATETIME) | Matches phieu_thu/phieu_chi convention |
| Auto-create = `trang_thai='draft'` | Spec: "kế toán duyệt tay" — NOT auto-approved |
| Guard uses dynamic `import()` | Same pattern as R6 nghiệm thu — if module not loaded, silent pass |
| Khóa sổ only on `trang_thai='approved'` | Spec: "approve tất cả chứng từ → khóa sổ" |

---

## 3. IMPLEMENTATION / CHANGES

| File | Action | Lines | Description |
|------|--------|-------|-------------|
| `src/lib/mf-chung-tu-store.ts` | NEW | 218 | Store: CRUD, auto-create from PT/PC, khóa sổ, guard checks |
| `src/lib/mf-store.ts` | MODIFIED | +42 lines | R3: 2 auto-create blocks + R4: 4 khóa sổ guards |
| `src/app/mf/ke-toan/actions.ts` | NEW | 22 | 3 Server Actions |
| `src/app/mf/ke-toan/page.tsx` | NEW | 12 | Server Component |
| `src/app/mf/ke-toan/ke-toan-client.tsx` | NEW | 152 | Client: table + khóa sổ button |
| `src/lib/version.ts` | MODIFIED | 1 line | MINOR: 230→231 |
| `scripts/migrations/v344-chung-tu-ke-toan.sql` | NEW | 36 | CREATE TABLE IF NOT EXISTS |

---

## 4. TRACE PACK

### Smoke test results

**R3 — Auto-create chung_tu from phieu_thu approve:**
```
id  so_chung_tu            loai_chung_tu  phieu_thu_id  so_tien      trang_thai  khoa_so  thang  nam
1   CT-THU-260706010915    phieu_thu      2             3000000.00   draft       0        7      2026
```
✅ Created with `trang_thai='draft'`, correct FK, correct amounts.

**R4 — Khóa sổ + guard:**
```
id  so_chung_tu            trang_thai  khoa_so  ngay_khoa_so
1   CT-THU-260706010915    approved    1        2026-07-06 01:09:15
```
```
is_locked = 1
```
✅ `khoa_so=1` after khóa sổ. Guard query returns `is_locked=1` → update/cancel would be blocked.

### TSC compile
```
npx tsc --noEmit → 0 errors
```

---

## 5. PASS/FAIL + TEST CASES

| Test | Expected | Actual | Status |
|------|----------|--------|--------|
| R2: CREATE TABLE | 26 columns, PK+UNIQUE+4 indexes | Verified SHOW CREATE TABLE | ✅ PASS |
| R3: Approve PT → chung_tu draft | INSERT record with `loai_chung_tu='phieu_thu'`, `trang_thai='draft'` | `CT-THU-260706010915` created | ✅ PASS |
| R4: Khóa sổ | `UPDATE khoa_so=1` for approved records | khoa_so=1 confirmed | ✅ PASS |
| R4: Guard blocks update | `isPhieuThuKhoaSo()` returns true | `is_locked=1` | ✅ PASS |
| R5: Route `/mf/ke-toan` | Page + client component exist | 3 files created, TSC pass | ✅ PASS |
| TSC compile | 0 errors | 0 errors | ✅ PASS |
| R3: Approve PC → chung_tu draft | Same for phieu_chi | ⚠️ PARTIAL — code exists, not smoke tested | ⚠️ |
| R5: UI visual test | Browser test | ⚠️ PARTIAL — needs `npm run dev` | ⚠️ |

### Test data (all `ghi_chu='TEST-DATA-CLEANUP-OK'`)
```
phieu_thu:          PT-CTKT-TEST (id=2)
chung_tu_ke_toan:   CT-THU-260706010915 (id=1)
```
