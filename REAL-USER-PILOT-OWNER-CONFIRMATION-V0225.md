# 📋 Real User Pilot — Owner Confirmation V0.225

> **Ngày:** 28/06/2026
> **Plan ID:** PLAN-20260628-V0225-OWNER-MATRIX-CONFIRMATION
> **Version:** V0.225
> **Status:** OWNER CONFIRMATION PREPARED — NO REAL USERS CREATED

---

## I. Summary

| Metric | Value |
|--------|-------|
| Version | V0.225 |
| Pilot candidates (active) | 7 |
| Blocked (needs role confirm) | 1 |
| Deferred | 1 |
| No-login | 1 (SX/CN group) |
| Emails filled | 0/7 (Owner not yet responded) |
| Scripts prepared | ✅ Creation + Rollback (both locked) |
| Real users created | ❌ ZERO |
| Passwords used | ❌ ZERO |
| Sensitive data published | ❌ ZERO |

---

## II. What Was Prepared

### Files Created

| File | Purpose | Location |
|------|---------|----------|
| Owner Confirmation Matrix | Candidate approval form | Private (local) |
| Safe Creation Script | User creation with kill switch | ERP repo |
| Rollback Script | Disable accounts + remove packs | ERP repo |
| Dry-Run Validator | Permission validation | ERP repo (V0.224) |

### Kill Switch Status

| Switch | Value | Effect |
|--------|-------|--------|
| `OWNER_APPROVED_REAL_USER_CREATION` | `false` (default) | Script runs dry-run only |
| `--apply` CLI flag | Not passed | Extra safety gate |
| Both required | N/A | Only then real creation happens |

---

## III. Role Distribution (Pilot)

| Role | Count | Key Permissions |
|------|-------|----------------|
| ADMIN | 1 | Full bypass |
| CEO | 1 | View all + approve + cost/profit |
| KE_TOAN | 1 | Công nợ + Thu chi + Giao hàng |
| SALES | 2 | Own-customers only |
| THIET_KE | 2 | LSX/PDI draft only |
| BLOCKED | 1 | Role needs confirmation |
| DEFERRED | 1 | Marketing — MC/M8 scope |

---

## IV. Sensitive Permission Enforcement

| Permission | ADMIN | CEO | Others |
|-----------|-------|-----|--------|
| cost/profit/margin | ✅ | ✅ | ❌ BLOCKED |
| HR access | ✅ | ✅ (view) | ❌ BLOCKED |
| Payroll access | ✅ | ✅ (view) | ❌ BLOCKED |
| Transfer customer | ✅ | ✅ | ❌ BLOCKED |
| Permanent delete | ✅ | ❌ | ❌ BLOCKED |

> ✅ Validator confirms: 0 sensitive permission leaks

---

## V. Batch Plan

| Batch | Candidates | Trigger |
|-------|-----------|---------|
| 0 | Admin (verify existing) | Owner email |
| 1 | CEO | Owner email |
| 2 | Kế toán tổng hợp | Owner email + SALE_ADMIN decision |
| 3 | Sales ×2 | Owner emails |
| 4 | Thiết kế ×2 | Owner emails |
| 5 | Optional (blocked candidate) | Owner role confirmation |

Each batch: rollback = disable account, not delete.

---

## VI. Security Test Plan (Post-Approval)

| # | Test | Expected |
|---|------|----------|
| 1 | Admin login | Full access |
| 2 | CEO login | View all + cost/profit + approve |
| 3 | KE_TOAN: view all KH (for debt) | ✅ |
| 4 | KE_TOAN: manage debt/payment/delivery | ✅ per pack |
| 5 | KE_TOAN: view cost/profit | ❌ DENIED |
| 6 | KE_TOAN: transfer customer | ❌ DENIED |
| 7 | KE_TOAN: HR/payroll | ❌ DENIED |
| 8 | Sales: only own customers | ✅ |
| 9 | Sales: view other sale's customers | ❌ DENIED |
| 10 | Sales: cost/profit | ❌ DENIED |
| 11 | Thiết kế: view all customers | ❌ DENIED |
| 12 | Thiết kế: create/edit LSX/PDI draft | ✅ |
| 13 | Thiết kế: approve production | ❌ DENIED |
| 14 | Thiết kế: cost/profit | ❌ DENIED |
| 15 | Export: no cost/profit leak | ✅ field masking |
| 16 | Direct action: no RBAC bypass | ✅ |

---

## VII. RBAC Coverage (V0.223)

| Module | Status | Coverage |
|--------|--------|----------|
| M0 Security | ✅ FULL | 8/8 (double-lock) |
| M1 KH | ✅ FULL | 12/12 |
| M3 BG/DH | ✅ FULL | 21/21 |
| M4 LSX/PDI | ✅ FULL | 37/37 |
| MF Finance | ✅ FULL | 13/13 |
| M6 HR | ✅ LOCKED | 28/28 |
| M7 Payroll | ✅ LOCKED | 16/16 |
| **Total** | | **135/303 (44.6%)** |

---

## VIII. Owner Decisions Required

| # | Question | Status |
|---|----------|--------|
| 1–6 | Login emails for 7 candidates | ⬜ PENDING |
| 7 | Trương Tiến Cường role + vòng đầu? | ⬜ PENDING |
| 8 | Trọng Vĩ defer? | ⬜ PENDING |
| 9 | KE_TOAN SALE_ADMIN pack? | ⬜ PENDING |
| 10 | Sales xem công nợ KH mình? | ⬜ PENDING |
| 11 | Admin reuse or create new? | ⬜ PENDING |
| 12 | ONLY ADMIN/CEO see cost/profit? | ⬜ PENDING |
| 13 | HR/payroll closed? | ⬜ PENDING |
| 14 | Batch order confirmed? | ⬜ PENDING |

---

## IX. Compliance Checklist

| Check | Result |
|-------|--------|
| No real users created | ✅ |
| No old passwords used | ✅ |
| No email/phone/CCCD published | ✅ |
| No salary/token/secret published | ✅ |
| Scripts locked by default | ✅ |
| Rollback prepared | ✅ |
| Validator passed | ✅ |
| TypeScript build passed | ✅ |
| Changelog updated | ✅ |

---

## X. Final Status

> **✅ READY FOR OWNER APPROVAL BEFORE REAL USER CREATION**
>
> **Next:** Owner điền email + approve matrix → Agent tạo user batch-by-batch với rollback sẵn sàng.
>
> **No real users created. No passwords generated. No sensitive data published.**
