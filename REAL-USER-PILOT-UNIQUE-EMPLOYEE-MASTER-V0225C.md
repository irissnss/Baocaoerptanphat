# 📋 Unique Employee Master — V0.225C Report

> **Ngày:** 28/06/2026
> **Version:** V0.225C
> **Status:** UNIQUE EMPLOYEE MASTER COMPLETE — NO REAL USERS CREATED
> **Apply requires:** Owner approval + `--apply` + backup

---

## I. Source Audit

| Metric | Value |
|--------|-------|
| Source rows (Owner provided) | 13 |
| Unique employees identified | 10 |
| Rows merged (dedupe) | 3 |
| Shared email groups detected | 5 |
| Shared email groups blocking login | 2 (Group A: 2 people, Group B: 3 people) |

---

## II. Identity Status Distribution

| Status | Count | Action |
|--------|-------|--------|
| READY for user creation prep | 2 | Can create user + employee after email filled |
| Verify existing admin | 1 | Check DB for existing admin before create |
| Needs email type confirm | 1 | Owner confirms if dept email or personal |
| Needs dedupe confirm | 1 | Owner confirms aliases = same person |
| BLOCKED shared email | 4 | Employee only. No user login until unique email |
| BLOCKED incomplete name | 1 | Cannot create anything until full name |
| **Total unique** | **10** | |

---

## III. Role Distribution

| Role | Count | Cost/Profit | All Customers | Transfer |
|------|-------|-------------|---------------|----------|
| ADMIN | 1 | ✅ | ✅ | ✅ |
| CEO | 1 | ✅ | ✅ | ✅ |
| KE_TOAN | 1 | ❌ | ✅ (debt) | ❌ |
| SALES | 2 | ❌ | ❌ (own only) | ❌ |
| THIET_KE | 3–4 | ❌ | ❌ | ❌ |
| USER/KY_THUAT | 1 | ❌ | ❌ | ❌ |

---

## IV. Shared Email Blocking

| Group | People | Status |
|-------|--------|--------|
| A | 2 people (TK + TK) | ⛔ BLOCKED for user login |
| B | 3 people (TK + KT + TK) | ⛔ BLOCKED for user login |
| C | 1 person (3 aliases merged) | ✅ OK — same person |
| D | 1 person (unclear type) | ⚠️ Needs confirm |
| E | 1 person (company + personal) | ✅ OK — company email as login |

---

## V. Employee vs User Login Separation

| Capability | Count |
|-----------|-------|
| CAN create employee record | 9/10 |
| CAN create user login | 2/10 |
| Employee-only (no login) | 7/10 |
| Cannot create anything | 1/10 (incomplete name) |

> **Schema confirms:** `hr_employee_nhanvien.user_id` is nullable.
> Employees CAN exist without user login.

---

## VI. Permission Packs Prepared

| Pack | Status | For |
|------|--------|-----|
| KE_TOAN_CONG_NO | ✅ Exists | Accounting debt |
| KE_TOAN_THU_CHI | ✅ Exists | Payment in/out |
| KE_TOAN_GIAO_HANG | ✅ Exists | Delivery |
| SALES_OWN_CUSTOMERS | ✅ Defined | Own customers |
| THIET_KE_LSX_DRAFT | ✅ Defined | LSX draft |
| THIET_KE_PDI_DRAFT | ✅ Defined | PDI draft |
| CEO_MONITOR | ✅ Defined | CEO monitoring |
| CEO_APPROVAL | ✅ Defined | CEO approval |

---

## VII. Scripts Prepared

| Script | Mode | Status |
|--------|------|--------|
| `prepare-real-users-v0225c.ts` | `--dry-run` | ✅ PASSED (0 errors, 2 warnings) |
| `prepare-real-users-v0225c.ts` | `--rollback` | ✅ PASSED (template only) |
| `prepare-real-users-v0225c.ts` | `--apply --employee-only` | 🔒 Locked (needs ENV + Owner) |
| `prepare-real-users-v0225c.ts` | `--apply --with-user-accounts` | 🔒 Locked |

---

## VIII. Security Compliance

| Check | Result |
|-------|--------|
| No real users created | ✅ |
| No real employees created | ✅ |
| No passwords generated/committed | ✅ |
| No full emails published | ✅ (masked) |
| No phone numbers published | ✅ |
| No CCCD/salary published | ✅ |
| No source code published | ✅ |
| Shared email blocked for user login | ✅ |
| TypeScript build passed | ✅ |
| Dry-run validation passed | ✅ |

---

## IX. Owner Decisions for V0.226

| # | Decision | Status |
|---|----------|--------|
| 1 | Accounting email: personal or company? | ⬜ |
| 2 | KD1/KD3 = same person? | ⬜ |
| 3 | "Phong" full name? | ⬜ |
| 4 | Admin: reuse test account? | ⬜ |
| 5 | Blocked candidates: provide unique emails? | ⬜ |
| 6 | Kỹ Thuật (Ng. Công Trận) role? | ⬜ |
| 7 | KE_TOAN needs SALE_ADMIN pack? | ⬜ |

---

## X. Final Status

> **✅ READY FOR OWNER UNIQUE EMAIL CLEANUP + SAFE USER APPLY PREP**
>
> **SHARED EMAIL BLOCKED FOR USER LOGIN**
>
> Next: Owner provides unique email identities → V0.226 safe apply with backup.
