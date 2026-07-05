# 📋 Implementation Report — V0.230: Nghiệm Thu & Đối Chiếu Công Nợ (V3.44)

> **Plan ID:** PLAN-20260705-nghiem-thu-doi-chieu-cong-no
> **Version:** V0.230
> **Date:** 06/07/2026
> **Decision:** Option C — Hybrid (Owner approved 05/07/2026)

---

## R1 — Migration: `bien_ban_nghiem_thu` ✅ PASS

**Evidence:** `SHOW CREATE TABLE bien_ban_nghiem_thu` → 22 columns, PK `ma_nghiem_thu`, ENUM workflow `draft/da_ky/cancelled`

Key columns: `ten_nghiem_thu`, `tong_truoc_vat`, `tien_vat`, `tong_phat_sinh`, `file_da_ky` (JSON), `danh_sach_so_phieu_gh` (JSON)

---

## R2 — Migration: `cong_no_doi_chieu` ✅ PASS

**Evidence:** `SHOW CREATE TABLE cong_no_doi_chieu` → 19 columns, PK `ma_doi_chieu`, ENUM workflow `draft/da_gui/da_xac_nhan/tu_choi/qua_han_phan_hoi/cancelled`

Key columns: `tong_no_duoi_30d`, `tong_no_31_60d`, `tong_no_tren_60d`, `file_da_ky` (JSON), `quy_tac_im_lang_chap_nhan` (BOOLEAN)

---

## R3 — Seed `dm_form_mau` ✅ PASS

**Evidence:** 2 rows inserted (id=33, 34):
- `FORM_NGHIEM_THU` → `bien_ban_nghiem_thu`
- `FORM_XAC_NHAN_CONG_NO` → `cong_no_doi_chieu`

**Adaptation:** Notion INSERT adapted to match real schema (ma_form_mau, template_html, nguoi_tao=INT)

---

## R4 — Route `/mf/doi-chieu` ✅ IMPLEMENTED

**Changes:**
- `src/lib/mf-doi-chieu-store.ts` (NEW) — CRUD, aging buckets, workflow state machine
- `src/app/mf/doi-chieu/actions.ts` (NEW) — Server Actions with RBAC
- `src/app/mf/doi-chieu/page.tsx` (MODIFIED) — replaced 31-line placeholder
- `src/app/mf/doi-chieu/doi-chieu-client.tsx` (NEW) — Client UI with table, stats, dialog

**Features:**
- Aging buckets computed from `cong_no.han_thanh_toan` via DATEDIFF
- Workflow: draft → da_gui (auto sets ngay_gui + han_phan_hoi) → da_xac_nhan/tu_choi/qua_han
- Create dialog with khách hàng, ngày chốt, số ngày phản hồi

---

## R5 — Route `/mf/nghiem-thu` ✅ IMPLEMENTED

**Changes:**
- `src/lib/mf-nghiem-thu-store.ts` (NEW) — CRUD, PGH item aggregation, workflow
- `src/app/mf/nghiem-thu/actions.ts` (NEW) — Server Actions with RBAC
- `src/app/mf/nghiem-thu/page.tsx` (NEW) — Server Component
- `src/app/mf/nghiem-thu/nghiem-thu-client.tsx` (NEW) — Client UI

**Features:**
- Auto-aggregates items from `phieu_giao_hang_item` by product within date range
- Computes `tong_truoc_vat`, `tien_vat`, `tong_phat_sinh` with configurable VAT %
- Gets `no_dau_ky` from current `cong_no` balance
- Formula: `con_phai_thanh_toan = tong_phat_sinh + no_dau_ky - da_thanh_toan`
- Workflow: draft → da_ky (locks snapshot) / cancelled

---

## R6 — Phiếu Thu Linkage ✅ IMPLEMENTED

**Change:** `src/lib/mf-store.ts` line ~755 — added link check in `approvePersistedPhieuThu()`

When `phieu_thu.lien_ket_type = 'bien_ban_nghiem_thu'` and approved:
→ Calls `updateThanhToanNghiemThu()` to recalculate `da_thanh_toan` + `con_phai_thanh_toan`

---

## R7 — Version Sync ✅ DONE

- `version.ts`: MINOR bumped 229 → 230
- `CHANGELOG-DETAIL.md`: entries added for V0.227, V0.228, V0.229, V0.230
- TSC compile: ✅ 0 errors

---

## R8 — Report Updates ✅ DONE

- `README.md`: version V0.230, MF description updated, table count 92
- `CHANGELOG-DETAIL.md`: 4 new entries
- This report file created

---

## Build Verification

| Check | Result |
|-------|--------|
| TSC compile (`npx tsc --noEmit`) | ✅ 0 errors |
| `SHOW CREATE TABLE bien_ban_nghiem_thu` | ✅ 22 columns |
| `SHOW CREATE TABLE cong_no_doi_chieu` | ✅ 19 columns |
| `SELECT dm_form_mau` 2 new rows | ✅ id=33,34 |
| `/mf/doi-chieu` no longer placeholder | ✅ Real implementation |
| `/mf/nghiem-thu` route exists | ✅ New route |

---

## Files Changed

| Action | File |
|--------|------|
| NEW | `src/lib/mf-doi-chieu-store.ts` |
| NEW | `src/lib/mf-nghiem-thu-store.ts` |
| NEW | `src/app/mf/doi-chieu/actions.ts` |
| NEW | `src/app/mf/doi-chieu/doi-chieu-client.tsx` |
| MODIFIED | `src/app/mf/doi-chieu/page.tsx` (placeholder → real) |
| NEW | `src/app/mf/nghiem-thu/page.tsx` |
| NEW | `src/app/mf/nghiem-thu/actions.ts` |
| NEW | `src/app/mf/nghiem-thu/nghiem-thu-client.tsx` |
| MODIFIED | `src/lib/mf-store.ts` (R6 linkage) |
| MODIFIED | `src/lib/version.ts` (229→230) |
| NEW | `scripts/migrations/v344-nghiem-thu-doi-chieu.ts` |
