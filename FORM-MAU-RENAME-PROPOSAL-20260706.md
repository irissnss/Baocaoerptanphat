# 🏷️ Form Mẫu Rename Proposal — PLAN-20260706-form-mau-standardization

> **Date:** 06/07/2026
> **Mode:** Phase A — Scan + Propose (không sửa DB)

---

## R1: Dependency Scan Result

```
grep -rn "FINBG|FINDH|FINPT|FINPH|FINLE|FINCO|FINKH|QR3-02|QR3-03" src/
→ 0 results
```

**✅ R1 PASS — 0 hardcode references.** All `ma_form_mau` usage is dynamic:
- `m4/lenh-san-xuat/actions.ts`: queries by `doi_tuong` parameter
- `m0/form-mau/`: CRUD by variable, no hardcoded codes
- `m0/form-phat-hanh/`: search/filter by variable
- `form-in-helpers.ts`: lookup by variable

**→ Safe to rename without code changes.**

---

## R2: Full Mapping (34 records)

### Legend
- **🟢 KEEP** = already standard `FORM_` prefix, no change needed
- **🔵 RENAME** = primary/default template, rename to `FORM_<DOI_TUONG>`
- **🟡 LEGACY** = duplicate, rename to `FORM_<DOI_TUONG>_LEGACY_<n>` (PROPOSED — chờ Coordinator)
- **🔴 REVIEW** = needs special decision

---

### doi_tuong = `bao_gia` (4 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 1 | FINBG060126.01 | ✅ 1 | active | Full HTML (<!DOCTYPE) | **🔵 FORM_BAO_GIA** | safe |
| 21 | FINBA06012601 | 0 | active | Demo skeleton | 🟡 FORM_BAO_GIA_LEGACY_1 | safe |
| 25 | FINBA06012605 | 0 | active | Demo skeleton | 🟡 FORM_BAO_GIA_LEGACY_2 | safe |
| 29 | FINBA06012609 | 0 | active | Demo skeleton | 🟡 FORM_BAO_GIA_LEGACY_3 | safe |

### doi_tuong = `don_hang` (4 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 2 | FINDH060126.02 | ✅ 1 | active | Full HTML | **🔵 FORM_DON_HANG** | safe |
| 22 | FINDO06012602 | 0 | active | Demo skeleton | 🟡 FORM_DON_HANG_LEGACY_1 | safe |
| 26 | FINDO06012606 | 0 | active | Demo skeleton | 🟡 FORM_DON_HANG_LEGACY_2 | safe |
| 30 | FINDO06012610 | 0 | active | Demo skeleton | 🟡 FORM_DON_HANG_LEGACY_3 | safe |

### doi_tuong = `phieu_thu` (3 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 3 | FINPT060126.03 | ✅ 1 | active | Full HTML | **🔵 FORM_PHIEU_THU** | safe |
| 23 | FINPH06012603 | 0 | active | Demo skeleton | 🟡 FORM_PHIEU_THU_LEGACY_1 | safe |
| 27 | FINPH06012607 | 0 | active | Demo skeleton | 🟡 FORM_PHIEU_THU_LEGACY_2 | safe |

### doi_tuong = `phieu_chi` (5 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 4 | FINPH010125.01 | ✅ 1 | active | Full HTML | **🔵 FORM_PHIEU_CHI** | safe |
| 11 | FINPH010825.08 | 0 | inactive | Full HTML | 🟡 FORM_PHIEU_CHI_LEGACY_1 | safe |
| 18 | FINPH011525.15 | 0 | draft | Full HTML | 🟡 FORM_PHIEU_CHI_LEGACY_2 | safe |
| 24 | FINPH06012604 | 0 | active | Demo skeleton | 🟡 FORM_PHIEU_CHI_LEGACY_3 | safe |
| 28 | FINPH06012608 | 0 | active | Demo skeleton | 🟡 FORM_PHIEU_CHI_LEGACY_4 | safe |

### doi_tuong = `phieu_nhap` (3 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 19 | FINPH011625.16 | ✅ 1 | active | Full HTML | **🔵 FORM_PHIEU_NHAP** | safe |
| 5 | FINPH010225.02 | 0 | inactive | Full HTML | 🟡 FORM_PHIEU_NHAP_LEGACY_1 | safe |
| 12 | FINPH010925.09 | 0 | draft | Full HTML | 🟡 FORM_PHIEU_NHAP_LEGACY_2 | safe |

### doi_tuong = `phieu_xuat` (3 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 13 | FINPH011025.10 | 0 | active | Full HTML | **🔵 FORM_PHIEU_XUAT** | safe |
| 6 | FINPH010325.03 | 0 | draft | Full HTML | 🟡 FORM_PHIEU_XUAT_LEGACY_1 | safe |
| 20 | FINPH011725.17 | 0 | inactive | Full HTML | 🟡 FORM_PHIEU_XUAT_LEGACY_2 | safe |

> ⚠️ **Note:** `phieu_xuat` has **NO `is_default=1` record.** id=13 is active + full HTML → best candidate. **PROPOSED: set `is_default=1` for id=13 cùng lúc rename?** Chờ Coordinator.

### doi_tuong = `phieu_giao_hang` (2 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 14 | FINPH011125.11 | ✅ 1 | **inactive** | Full HTML | **🔵 FORM_PHIEU_GIAO_HANG** | safe |
| 7 | FINPH010425.04 | 0 | active | Full HTML | 🟡 FORM_PHIEU_GIAO_HANG_LEGACY_1 | safe |

> ⚠️ **Note:** Default record (id=14) is **`inactive`** while non-default (id=7) is **`active`**. **PROPOSED: swap default to id=7 (active) hoặc activate id=14?** Chờ Coordinator.

### doi_tuong = `lenh_san_xuat` (3 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 31 | QR3-02/001 | ✅ 1 | active | Placeholder | **🔴 FORM_LENH_SAN_XUAT** | 🔴 nguoi_tao=NULL |
| 8 | FINLE010525.05 | 0 | inactive | Full HTML | 🟡 FORM_LENH_SAN_XUAT_LEGACY_1 | safe |
| 15 | FINLE011225.12 | 0 | draft | Full HTML | 🟡 FORM_LENH_SAN_XUAT_LEGACY_2 | safe |

> 🔴 **QUESTION:** id=31 có `nguoi_tao=NULL`. Fix luôn `nguoi_tao=1` trong cùng UPDATE, hay chờ?

### doi_tuong = `phieu_dieu_in` (1 record)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 32 | QR3-03/001 | ✅ 1 | active | Placeholder | **🔴 FORM_PHIEU_DIEU_IN** | 🔴 nguoi_tao=NULL |

> 🔴 **QUESTION:** Same — fix `nguoi_tao=NULL` → `1`?

### doi_tuong = `cong_no` (2 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 9 | FINCO010625.06 | ✅ 1 | **draft** | Full HTML | **🔵 FORM_CONG_NO** | safe |
| 16 | FINCO011325.13 | 0 | active | Full HTML | 🟡 FORM_CONG_NO_LEGACY_1 | safe |

> ⚠️ **Note:** Default record (id=9) is `draft`. Non-default (id=16) is `active`. Cùng vấn đề như phieu_giao_hang.

### doi_tuong = `khac` (2 records)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 10 | FINKH010725.07 | 0 | active | Full HTML | **🔵 FORM_KHAC** | safe |
| 17 | FINKH011425.14 | 0 | inactive | Full HTML | 🟡 FORM_KHAC_LEGACY_1 | safe |

> ⚠️ **Note:** `khac` has **NO `is_default=1` record.** id=10 is active → best candidate.

### doi_tuong = `bien_ban_nghiem_thu` (1 record)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 33 | FORM_NGHIEM_THU | 0 | active | Placeholder | **🟢 KEEP** (already standard) | safe |

### doi_tuong = `cong_no_doi_chieu` (1 record)

| id | ma_form_mau (cũ) | is_default | trang_thai | template | Đề xuất mới | Risk |
|----|------------------|-----------|-----------|----------|-------------|------|
| 34 | FORM_XAC_NHAN_CONG_NO | 0 | active | Placeholder | **🟢 KEEP** | safe |

---

## Summary

| Action | Count | Records |
|--------|-------|---------|
| 🟢 KEEP (already standard) | 2 | id=33, 34 |
| 🔵 RENAME to primary | 10 | id=1,2,3,4,7 or 14,9 or 16,10,13,19,31,32 |
| 🟡 RENAME to LEGACY | 20 | remaining duplicates |
| 🔴 NEEDS DECISION | 2 | id=31,32 (nguoi_tao=NULL) |

### Open questions for Coordinator:

1. **LEGACY naming:** Use `FORM_<DOI_TUONG>_LEGACY_<n>` or keep old names as-is?
2. **phieu_xuat (id=13):** No default → set `is_default=1`?
3. **khac (id=10):** No default → set `is_default=1`?
4. **phieu_giao_hang (id=14 vs 7):** Default is inactive, non-default is active → swap?
5. **cong_no (id=9 vs 16):** Default is draft, non-default is active → swap?
6. **id=31,32:** Fix `nguoi_tao=NULL` → `1` in same UPDATE?
