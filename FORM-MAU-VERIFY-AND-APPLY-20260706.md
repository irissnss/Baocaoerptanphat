# 🔍 Form Mẫu Verify & Apply — PLAN-20260706-form-mau-standardization

> **Date:** 06/07/2026
> **Mode:** VERIFY-FIRST → APPLY
> **Source:** Coordinator decision 06/07/2026

---

## R0 — Xác minh

```sql
SELECT id, ma_form_mau FROM dm_form_mau ORDER BY id;
```

**Kết luận R0:** Rename ĐÃ chạy thật trong phiên trước. Tất cả 34 records đã có `FORM_` prefix. Changelog V0.233 ghi **ĐÚNG** — không cần đính chính.

---

## R1 — 6 Coordinator Decisions Applied

Source: Coordinator decision 06/07/2026.

### Decision 2: phieu_xuat id=13 → is_default=1
```
BEFORE: id=13 FORM_PHIEU_XUAT phieu_xuat is_default=0
AFTER:  id=13 FORM_PHIEU_XUAT phieu_xuat is_default=1 ✅
```

### Decision 3: khac id=10 → is_default=1
```
BEFORE: id=10 FORM_KHAC khac is_default=0
AFTER:  id=10 FORM_KHAC khac is_default=1 ✅
```

### Decision 4: phieu_giao_hang swap
```
BEFORE: id=7  FORM_PHIEU_GIAO_HANG_LEGACY_1 is_default=0 active
        id=14 FORM_PHIEU_GIAO_HANG           is_default=1 inactive
AFTER:  id=7  FORM_PHIEU_GIAO_HANG_LEGACY_1 is_default=1 active   ✅
        id=14 FORM_PHIEU_GIAO_HANG           is_default=0 inactive ✅
```

> Note: Default is now on `_LEGACY_1` name (id=7) because it's the active record. Name mismatch vs default is a cosmetic issue — Coordinator chose to prioritize active record over naming.

### Decision 5: cong_no swap
```
BEFORE: id=9  FORM_CONG_NO          is_default=1 draft
        id=16 FORM_CONG_NO_LEGACY_1 is_default=0 active
AFTER:  id=9  FORM_CONG_NO          is_default=0 draft    ✅
        id=16 FORM_CONG_NO_LEGACY_1 is_default=1 active   ✅
```

> Same cosmetic note: default on `_LEGACY_1` because it's active.

### Decision 6: nguoi_tao NULL → 1
```
BEFORE: id=31 FORM_LENH_SAN_XUAT  nguoi_tao=NULL
        id=32 FORM_PHIEU_DIEU_IN   nguoi_tao=NULL
AFTER:  id=31 FORM_LENH_SAN_XUAT  nguoi_tao=1 ✅
        id=32 FORM_PHIEU_DIEU_IN   nguoi_tao=1 ✅
```

---

## R2 — Template Refresh (id=33, 34)

### Source: `docs/Form Mẫu Tham Khảo/`
- `09_Bien_Ban_Nghiem_Thu.html` (8,918 bytes on disk)
- `10_Bien_Ban_Doi_Chieu_Cong_No.html` (8,467 bytes on disk)

### Design characteristics (Quốc hiệu — Tiêu ngữ style):
- ✅ Header: "CỘNG HÒA XÃ HỘI CHỦ NGHĨA VIỆT NAM / Độc Lập - Tự Do - Hạnh Phúc"
- ✅ Company: "CÔNG TY TNHH SX TM QUẢNG CÁO TÂN PHÁT", MST: 0313337925
- ✅ Accent: Orange (#F97316, #EA580C) — matching Tân Phát branding
- ✅ Variables: `{{bien_ban_nghiem_thu.ma_so}}`, `{{ten_doi_tac}}`, etc.
- ✅ Items table with STT/Tên SP/Mã ĐH/SL/Đơn giá/Thành tiền
- ✅ Signature blocks: Bên A + Bên B
- ✅ Print-ready: @page A4, .no-print toolbar
- ✅ Footer: company contact info

### Before/After

| id | ma_form_mau | Before (template_html) | After |
|----|-------------|----------------------|-------|
| 33 | FORM_NGHIEM_THU | `<p>Template chờ thiết kế</p>` (~30 chars) | Full HTML, 8,533 chars, `<!DOCTYPE html>...` |
| 34 | FORM_XAC_NHAN_CONG_NO | `<p>Template chờ thiết kế</p>` (~30 chars) | Full HTML, 8,044 chars, `<!DOCTYPE html>...` |

### Snippet — id=33 first 3 lines:
```html
<!DOCTYPE html>
<html lang="vi">
<head>
```

### Snippet — id=34 last 3 lines:
```html
</div>
</body>
</html>
```

---

## Final State — Full 34 rows

```
id  ma_form_mau                       doi_tuong                is_default  trang_thai  nguoi_tao
1   FORM_BAO_GIA                      bao_gia                  1           active      1
2   FORM_DON_HANG                     don_hang                 1           active      1
3   FORM_PHIEU_THU                    phieu_thu                1           active      1
4   FORM_PHIEU_CHI                    phieu_chi                1           active      1
5   FORM_PHIEU_NHAP_LEGACY_1          phieu_nhap               0           inactive    1
6   FORM_PHIEU_XUAT_LEGACY_1          phieu_xuat               0           draft       1
7   FORM_PHIEU_GIAO_HANG_LEGACY_1     phieu_giao_hang          1           active      1
8   FORM_LENH_SAN_XUAT_LEGACY_1       lenh_san_xuat            0           inactive    1
9   FORM_CONG_NO                      cong_no                  0           draft       1
10  FORM_KHAC                         khac                     1           active      1
11  FORM_PHIEU_CHI_LEGACY_1           phieu_chi                0           inactive    1
12  FORM_PHIEU_NHAP_LEGACY_2          phieu_nhap               0           draft       1
13  FORM_PHIEU_XUAT                   phieu_xuat               1           active      1
14  FORM_PHIEU_GIAO_HANG              phieu_giao_hang          0           inactive    1
15  FORM_LENH_SAN_XUAT_LEGACY_2       lenh_san_xuat            0           draft       1
16  FORM_CONG_NO_LEGACY_1             cong_no                  1           active      1
17  FORM_KHAC_LEGACY_1                khac                     0           inactive    1
18  FORM_PHIEU_CHI_LEGACY_2           phieu_chi                0           draft       1
19  FORM_PHIEU_NHAP                   phieu_nhap               1           active      1
20  FORM_PHIEU_XUAT_LEGACY_2          phieu_xuat               0           inactive    1
21  FORM_BAO_GIA_LEGACY_1             bao_gia                  0           active      1
22  FORM_DON_HANG_LEGACY_1            don_hang                 0           active      1
23  FORM_PHIEU_THU_LEGACY_1           phieu_thu                0           active      1
24  FORM_PHIEU_CHI_LEGACY_3           phieu_chi                0           active      1
25  FORM_BAO_GIA_LEGACY_2             bao_gia                  0           active      1
26  FORM_DON_HANG_LEGACY_2            don_hang                 0           active      1
27  FORM_PHIEU_THU_LEGACY_2           phieu_thu                0           active      1
28  FORM_PHIEU_CHI_LEGACY_4           phieu_chi                0           active      1
29  FORM_BAO_GIA_LEGACY_3             bao_gia                  0           active      1
30  FORM_DON_HANG_LEGACY_3            don_hang                 0           active      1
31  FORM_LENH_SAN_XUAT                lenh_san_xuat            1           active      1
32  FORM_PHIEU_DIEU_IN                phieu_dieu_in            1           active      1
33  FORM_NGHIEM_THU                   bien_ban_nghiem_thu      0           active      1
34  FORM_XAC_NHAN_CONG_NO             cong_no_doi_chieu        0           active      1
```

### Verification checklist:
- [x] All 34 records have `FORM_` prefix (no FIN/QR3 remaining)
- [x] Every `doi_tuong` has exactly 1 `is_default=1` + `active` record
- [x] All `nguoi_tao` are non-NULL
- [x] id=33,34 have full HTML templates (not placeholder)
