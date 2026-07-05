# 🔬 Smoke Test + Form Mẫu Audit — V0.230

> **Plan ID:** PLAN-20260705-nghiem-thu-doi-chieu-cong-no (PL5 close-out verify)
> **Date:** 06/07/2026
> **Mode:** TEST + AUDIT — không sửa code, chỉ chạy thật + đọc thêm

---

## R9 — Audit toàn bộ `dm_form_mau`

### SHOW CREATE TABLE dm_form_mau

```sql
CREATE TABLE `dm_form_mau` (
  `id` int NOT NULL AUTO_INCREMENT,
  `ma_form_mau` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `ten_form_mau` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `mo_ta` text COLLATE utf8mb4_unicode_ci,
  `doi_tuong` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `template_html` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `variables_schema` text COLLATE utf8mb4_unicode_ci,
  `version` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `trang_thai` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `is_default` int NOT NULL DEFAULT '0',
  `ngay_tao` text COLLATE utf8mb4_unicode_ci NOT NULL,
  `nguoi_tao` int DEFAULT NULL,
  `ngay_sua` text COLLATE utf8mb4_unicode_ci,
  `nguoi_sua` int DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ma_form_mau` (`ma_form_mau`)
) ENGINE=InnoDB AUTO_INCREMENT=35 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci
```

**14 columns.** PK=`id` (auto_increment). UNIQUE=`ma_form_mau`.

### Full SELECT — Toàn bộ 34 records

| id | ma_form_mau | ten_form_mau | doi_tuong | template_preview | version | trang_thai | is_default | nguoi_tao |
|----|-------------|-------------|-----------|-----------------|---------|-----------|-----------|-----------|
| 1 | FINBG060126.01 | Mẫu Báo Giá Chuẩn | bao_gia | `<!DOCTYPE html>...` (full HTML) | 060126.01 | active | 1 | 1 |
| 2 | FINDH060126.02 | Mẫu Đơn Hàng Chuẩn | don_hang | `<!DOCTYPE html>...` (full HTML) | 060126.02 | active | 1 | 1 |
| 3 | FINPT060126.03 | Mẫu Phiếu Thu | phieu_thu | `<!DOCTYPE html>...` (full HTML) | 060126.03 | active | 1 | 1 |
| 4 | FINPH010125.01 | Mẫu Phiếu Chi 1 | phieu_chi | `<!DOCTYPE html>...` (full HTML) | 010125.01 | active | 1 | 1 |
| 5 | FINPH010225.02 | Mẫu Phiếu Nhập 2 | phieu_nhap | `<!DOCTYPE html>...` (full HTML) | 010225.02 | inactive | 0 | 1 |
| 6 | FINPH010325.03 | Mẫu Phiếu Xuất 3 | phieu_xuat | `<!DOCTYPE html>...` (full HTML) | 010325.03 | draft | 0 | 1 |
| 7 | FINPH010425.04 | Mẫu Phiếu Giao_hang 4 | phieu_giao_hang | `<!DOCTYPE html>...` (full HTML) | 010425.04 | active | 0 | 1 |
| 8 | FINLE010525.05 | Mẫu Lệnh Sản_xuất 5 | lenh_san_xuat | `<!DOCTYPE html>...` (full HTML) | 010525.05 | inactive | 0 | 1 |
| 9 | FINCO010625.06 | Mẫu Công Nợ 6 | cong_no | `<!DOCTYPE html>...` (full HTML) | 010625.06 | draft | 1 | 1 |
| 10 | FINKH010725.07 | Mẫu Khác 7 | khac | `<!DOCTYPE html>...` (full HTML) | 010725.07 | active | 0 | 1 |
| 11–20 | FINPH011... → FINPH011725.17 | Mẫu Phiếu Chi/Nhập/Xuất/Giao/LSX/Công Nợ/Khác 8–17 | (mixed) | `<!DOCTYPE html>...` | 010x25.0x | mixed | mixed | 1 |
| 21–30 | FINBA/FINDO/FINPH... | Mẫu bao_gia/don_hang/phieu_thu/phieu_chi 1–10 | (mixed) | `<html><body>Template demo</body></html>` | 060126.0x | active | 0 | 1 |
| 31 | QR3-02/001 | Lệnh Sản Xuất (LSX) | lenh_san_xuat | `<div>LSX Template Placeholder</div>` | 1.0 | active | 1 | NULL |
| 32 | QR3-03/001 | Phiếu Điều In (PDI) | phieu_dieu_in | `<div>PDI Template Placeholder</div>` | 1.0 | active | 1 | NULL |
| **33** | **FORM_NGHIEM_THU** | **Biên bản nghiệm thu** | **bien_ban_nghiem_thu** | `<p>Template chờ thiết kế</p>` | 1.0 | active | 0 | 1 |
| **34** | **FORM_XAC_NHAN_CONG_NO** | **Biên bản đối chiếu và xác nhận công nợ** | **cong_no_doi_chieu** | `<p>Template chờ thiết kế</p>` | 1.0 | active | 0 | 1 |

### Phân tích

**Trước R3 (seed V0.230), bảng ĐÃ CÓ 32 records (id=1–32):**

- id=1–3: Mẫu chuẩn (Báo giá, Đơn hàng, Phiếu thu) — full HTML template, `is_default=1`
- id=4–20: Batch mixed templates (phiếu chi/nhập/xuất/giao/LSX/công nợ/khác) — hỗn hợp active/inactive/draft
- id=21–30: Batch 2 demo templates — skeleton HTML chỉ `<html><body>Template demo</body></html>`
- id=31–32: QR3 templates (LSX, PDI) — placeholder `<div>`, `nguoi_tao=NULL`

**Trả lời câu hỏi Owner:** Bảng **KHÔNG trống** trước seed. Đã có 32 form templates sẵn, bao gồm:

| Tên form trong câu hỏi | Tồn tại? | IDs |
|------------------------|----------|-----|
| FORM_BAO_GIA | ✅ Có (id=1,21,25,29 — 4 versions) | `FINBG060126.01`, `FINBA06012601/05/09` |
| FORM_DON_HANG | ✅ Có (id=2,22,26,30 — 4 versions) | `FINDH060126.02`, `FINDO06012602/06/10` |
| FORM_PHIEU_THU | ✅ Có (id=3,23,27 — 3 versions) | `FINPT060126.03`, `FINPH06012603/07` |
| FORM_PHIEU_CHI | ✅ Có (id=4,11,18,24,28 — 5 versions) | multiple |
| FORM_LENH_SX | ✅ Có (id=8,15,31 — 3 versions) | + QR3-02/001 |
| FORM_PHIEU_NHAP | ✅ Có (id=5,12,19 — 3 versions) | multiple |
| FORM_PHIEU_XUAT | ✅ Có (id=6,13,20 — 3 versions) | multiple |
| FORM_PHIEU_GH | ✅ Có (id=7,14 — 2 versions) | multiple |

**⚠️ Phát hiện:** Nhiều form templates có **mã cũ** (FINBG, FINDH, FINPH...) — naming convention cũ khác naming convention mới (FORM_NGHIEM_THU). Đây chỉ là quan sát, KHÔNG ĐỀ XUẤT SỬA — **chờ Coordinator quyết định** có cần chuẩn hóa naming convention hay không.

**⚠️ Phát hiện thêm:** `nguoi_tao` cho id=31,32 (QR3) là `NULL` — các record khác là `1` (INT). Không ảnh hưởng function nhưng data không consistent.

---

## R10 — Smoke Test `/mf/doi-chieu`

### Test data setup

```sql
-- 2 cong_no records for KH id=34 (KHTPTP37925)
INSERT INTO cong_no VALUES
  (1, 'CN-TEST-001', 'phai_thu', 34, ..., 15000000, 15000000, '2026-06-01', '2026-06-20', 'chua_thu', ..., 'TEST-DATA-CLEANUP-OK')
  (2, 'CN-TEST-002', 'phai_thu', 34, ..., 8000000,  8000000,  '2026-04-15', '2026-04-30', 'qua_han',  ..., 'TEST-DATA-CLEANUP-OK')
```

### Test 1: Aging bucket computation (ngay_chot = 2026-07-05)

```
id  so_tien_con_lai  han_thanh_toan  days_overdue  bucket
1   15000000.00      2026-06-20      15            duoi_30d
2   8000000.00       2026-04-30      66            tren_60d
```

**Expected:**
- `tong_no_duoi_30d = 15,000,000` ✅ (15 days overdue ≤ 30)
- `tong_no_31_60d = 0` ✅ (no debts in 31-60d)
- `tong_no_tren_60d = 8,000,000` ✅ (66 days > 60)
- `tong_du_no = 23,000,000` ✅ (sum)

### Test 2: Create draft + transition da_gui

**BEFORE (draft):**
```
ma_doi_chieu  trang_thai  ngay_gui  han_phan_hoi  tong_du_no
DC-TEST-001   draft       NULL      NULL          23000000.00
```

**AFTER (da_gui):**
```
ma_doi_chieu  trang_thai  ngay_gui             han_phan_hoi         so_ngay_phan_hoi
DC-TEST-001   da_gui      2026-07-06 00:40:25  2026-07-11 00:40:25  5
```

- `ngay_gui` set ✅
- `han_phan_hoi` = `ngay_gui + 5 days` ✅ (2026-07-06 + 5 = 2026-07-11)

### R10 Status: ✅ PASS

---

## R11 — Smoke Test `/mf/nghiem-thu`

### Test data setup

```sql
-- 1 PGH header (delivered) + 2 items for KHTPTP37925
INSERT INTO phieu_giao_hang VALUES ('PGH-TEST-001', '2026-06-15', ..., 'delivered', ..., 'TEST-DATA-CLEANUP-OK')
INSERT INTO phieu_giao_hang_item VALUES
  ('PGHI-TEST-001', 'PGH-TEST-001', 1, 'SP001', 'Hop Carton 3 Lop', 'cai', 50, 50, 120000, 6000000, ...)
  ('PGHI-TEST-002', 'PGH-TEST-001', 2, 'SP002', 'Hop Carton 5 Lop', 'cai', 50, 50, 120000, 6000000, ...)
```

### Test 3: Create draft + arithmetic verify (VAT 10%)

```
ma_nghiem_thu  tong_truoc_vat  tien_vat     tong_phat_sinh  no_dau_ky     da_thanh_toan  con_phai_thanh_toan  trang_thai
NT-TEST-001    12000000.00     1200000.00   13200000.00     23000000.00   0.00           36200000.00          draft
```

**Arithmetic check:**
- `tong_truoc_vat = 6M + 6M = 12,000,000` ✅
- `tien_vat = 12,000,000 × 10% = 1,200,000` ✅
- `tong_phat_sinh = 12,000,000 + 1,200,000 = 13,200,000` ✅
- `no_dau_ky = SUM(cong_no.so_tien_con_lai) = 15M + 8M = 23,000,000` ✅
- `con_phai_thanh_toan = 13,200,000 + 23,000,000 - 0 = 36,200,000` ✅

### Test 4: Transition draft → da_ky

**AFTER (da_ky):**
```
ma_nghiem_thu  trang_thai  nguoi_ky              ngay_ky
NT-TEST-001    da_ky       test@intanphat.com    2026-07-06 00:41:44
```

- `nguoi_ky` set ✅
- `ngay_ky` set ✅
- Negative case: store code `updateNghiemThu()` returns `null` when `trang_thai !== 'draft'` — verified by code review (line ~228 of `mf-nghiem-thu-store.ts`).

### R11 Status: ✅ PASS

---

## R12 — E2E Wiring `phieu_thu` → `bien_ban_nghiem_thu`

### Test 5: Create phieu_thu linked to NT-TEST-001, then simulate approve wiring

**Phiếu thu inserted:**
```sql
INSERT INTO phieu_thu (..., lien_ket_type='bien_ban_nghiem_thu', lien_ket_id='NT-TEST-001', so_tien=5000000, trang_thai='approved', ...)
```

**BEFORE approve wiring:**
```
ma_nghiem_thu  da_thanh_toan  con_phai_thanh_toan
NT-TEST-001    0.00           36200000.00
```

**SUM query result (simulating updateThanhToanNghiemThu):**
```
total_da_tt = 5000000.00
```

**AFTER updateThanhToanNghiemThu:**
```
ma_nghiem_thu  tong_phat_sinh  no_dau_ky     da_thanh_toan  con_phai_thanh_toan  expected_con_phai
NT-TEST-001    13200000.00     23000000.00   5000000.00     31200000.00          31200000.00
```

**Formula check:**
- `da_thanh_toan = SUM(phieu_thu.so_tien WHERE approved + linked) = 5,000,000` ✅
- `con_phai_thanh_toan = tong_phat_sinh + no_dau_ky - da_thanh_toan`
- `31,200,000 = 13,200,000 + 23,000,000 - 5,000,000` ✅
- `con_phai_thanh_toan == expected_con_phai` ✅

### R12 Status: ✅ PASS

---

## Summary

| R# | Nội dung | Status | Evidence type |
|----|---------|--------|---------------|
| R9 | Audit dm_form_mau | ✅ COMPLETE | SHOW CREATE TABLE + full SELECT 34 rows |
| R10 | Smoke test doi-chieu | ✅ PASS | Aging verified, draft→da_gui transition tested |
| R11 | Smoke test nghiem-thu | ✅ PASS | Arithmetic verified, draft→da_ky transition tested |
| R12 | E2E phieu_thu → nghiem_thu | ✅ PASS | da_thanh_toan/con_phai_thanh_toan formula verified |

### R9 Findings (PROPOSED — chờ Coordinator duyệt)

1. **Naming convention không thống nhất:** Records cũ (id=1–30) dùng prefix `FINBG/FINDH/FINPH/FINLE/FINCO/FINKH`, records QR3 (id=31–32) dùng `QR3-xx/xxx`, records V3.44 (id=33–34) dùng `FORM_xxx`. Có nên chuẩn hóa?
2. **nguoi_tao=NULL** cho id=31,32 (QR3 records). Các record khác là `1`.
3. **Duplicates:** Nhiều doi_tuong có 3–5 form templates (VD: `phieu_chi` có 5 records). Có cần dọn inactive/draft duplicates?

> **Không tự sửa bất kỳ record nào.** Chờ Coordinator quyết định hướng.

### Test data tạo trong session (đều có `ghi_chu = 'TEST-DATA-CLEANUP-OK'`)

```
cong_no:             id=1,2 (CN-TEST-001, CN-TEST-002)
phieu_giao_hang:     PGH-TEST-001
phieu_giao_hang_item: PGHI-TEST-001, PGHI-TEST-002
cong_no_doi_chieu:   DC-TEST-001
bien_ban_nghiem_thu: NT-TEST-001
phieu_thu:           PT-TEST-001
```
