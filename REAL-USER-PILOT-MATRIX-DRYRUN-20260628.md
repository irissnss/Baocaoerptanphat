# 📋 Real User Pilot Matrix — Dry-Run Report V0.224

> **Ngày:** 28/06/2026
> **Plan ID:** PLAN-20260628-V0224-REAL-STAFF-PILOT-MATRIX
> **Version:** V0.224
> **Status:** DRY-RUN COMPLETE — NO REAL USERS CREATED

---

## I. Summary

| Metric | Count |
|--------|-------|
| Total candidates reviewed | 10 |
| Pilot candidates (proposed) | 7 |
| Blocked (needs role confirmation) | 1 |
| Deferred (not in pilot scope) | 1 |
| No login required (SX/CN) | 1 |
| Missing emails (needs Owner input) | 8/8 |
| Duplicate email detected | 0 |
| Shared email detected | 0 |
| Validation errors | 0 |
| Validation warnings | 7 |

---

## II. Candidate Distribution by Role

| Role | Count | Packs |
|------|-------|-------|
| ADMIN (Owner) | 1 | Full bypass |
| CEO | 1 | Monitor/approve |
| KE_TOAN | 1 | Công nợ + Thu chi + Giao hàng |
| SALES | 2 | Own-customers only |
| THIET_KE | 2–3 | LSX/PDI draft only |
| DEFERRED | 1 | Marketing — MC/M8 chưa mở |
| NO_LOGIN | many | SX/CN — employee record only |

---

## III. Permission Pack Summary

| Pack | Status | Assigned To |
|------|--------|-------------|
| KE_TOAN_CONG_NO | ✅ Active | 1 accounting candidate |
| KE_TOAN_THU_CHI | ✅ Active | 1 accounting candidate |
| KE_TOAN_GIAO_HANG | ✅ Active | 1 accounting candidate |
| KE_TOAN_NHAN_SU | ⏳ Deferred | — |
| KE_TOAN_TINH_LUONG | ⏳ Deferred | — |
| KE_TOAN_KHO_CHUNG_TU | ⏳ Deferred | — |
| SALE_ADMIN | ❓ Proposed | Owner quyết |

---

## IV. Sensitive Permission Audit

| Permission | ADMIN | CEO | KE_TOAN | SALES | THIET_KE |
|-----------|-------|-----|---------|-------|----------|
| cost/profit/margin | ✅ | ✅ | ❌ | ❌ | ❌ |
| HR access | ✅ | ✅ (view) | ❌ | ❌ | ❌ |
| Payroll access | ✅ | ✅ (view) | ❌ | ❌ | ❌ |
| Transfer customer | ✅ | ✅ | ❌ | ❌ | ❌ |
| View all customers | ✅ | ✅ | ✅ (for debt) | ❌ | ❌ |
| Delete permanently | ✅ | ❌ | ❌ | ❌ | ❌ |

> ✅ No sensitive permission leak detected in validator.

---

## V. Risk Flags

| Risk | Count | Status |
|------|-------|--------|
| Missing email (needs Owner input) | 8 | ⚠️ BLOCKING |
| Name duplication in DMKH | 3 | ✅ RESOLVED (different person confirmed) |
| Role needs confirmation | 1 | ⚠️ BLOCKING |
| Duplicate/shared email | 0 | ✅ CLEAR |
| Sensitive permission leak | 0 | ✅ CLEAR |
| HR/payroll leak | 0 | ✅ CLEAR |
| Old password usage | 0 | ✅ CLEAR |

---

## VI. User Creation Batch Plan (NOT EXECUTED)

| Batch | Candidates | Purpose | Rollback |
|-------|-----------|---------|----------|
| 1 | Admin + CEO | Core leadership | Disable accounts |
| 2 | Kế toán | Finance verification | Disable + remove packs |
| 3 | Sales (1–2) | Ownership filter test | Disable accounts |
| 4 | Thiết kế (2–3) | LSX/PDI draft test | Disable accounts |

Each batch:
- Password generated at runtime (≥12 chars)
- `must_change_password = true`
- No old Google Drive passwords
- Rollback = disable account, not delete

---

## VII. RBAC Coverage Context (V0.223)

| Metric | Value |
|--------|-------|
| Functions guarded | 135/303 (44.6%) |
| Files fully guarded | 10/42 |
| M0 Security | 8/8 ✅ (double-lock) |
| M1 KH | 12/12 ✅ |
| M3 BG/DH | 21/21 ✅ |
| M4 LSX/PDI | 37/37 ✅ |
| MF Finance | 13/13 ✅ |
| M6 HR | 28/28 ✅ (protection lock) |
| M7 Payroll | 16/16 ✅ (protection lock) |

---

## VIII. Owner Decisions Required

> ⬜ = Chưa trả lời — Owner cần confirm

| # | Question | Status |
|---|----------|--------|
| 1 | Email chính thức CEO (`@intanphat.com`?) | ⬜ |
| 2 | Email chính thức kế toán | ⬜ |
| 3 | Email riêng cho từng Sale (cá nhân hay công ty?) | ⬜ |
| 4 | Email riêng cho từng Thiết kế | ⬜ |
| 5 | Trương Tiến Cường: THIET_KE hay role khác? Tạo vòng đầu? | ⬜ |
| 6 | Trọng Vĩ: defer Marketing đúng không? | ⬜ |
| 7 | KE_TOAN cần SALE_ADMIN pack không? | ⬜ |
| 8 | KE_TOAN cần xem cost/profit không? | ⬜ |
| 9 | Sales có được xem công nợ KH mình? | ⬜ |
| 10 | Admin account: tạo mới hay reuse test account? | ⬜ |

---

## IX. Security Compliance

| Check | Result |
|-------|--------|
| No real users created | ✅ |
| No old passwords used | ✅ |
| No email/phone/CCCD published | ✅ |
| No salary data published | ✅ |
| No source code published | ✅ |
| No schema published | ✅ |
| Validator dry-run passed | ✅ (0 errors) |
| TypeScript build passed | ✅ |
| Kill switch default = false | ✅ |

---

## X. Final Status

> **✅ READY FOR OWNER MATRIX REVIEW — NO REAL USERS CREATED**
>
> Next step: Owner điền email + approve từng candidate → Agent tạo user có kiểm soát (batch-by-batch, rollback sẵn sàng).
