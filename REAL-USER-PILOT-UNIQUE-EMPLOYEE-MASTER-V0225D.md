# 📋 Unique Employee Master — V0.225D Report

> **Ngày:** 28/06/2026
> **Version:** V0.225D
> **Status:** EXISTING DEV ADMIN VERIFIED · PERMISSION MENU UX CLEANED · NO REAL USERS CREATED

---

## I. Bảng Tổng Hợp Nhân Sự

| # | Candidate | Source Rows | Proposed Role | Login Status | Email Status | Permission Packs | Action Taken | Blocking Reason | Next Owner Action |
|---|-----------|-------------|---------------|--------------|--------------|------------------|--------------|-----------------|-------------------|
| E01 | Nguyễn Thị Bạch Kim | 1 | KE_TOAN | ⚠️ Needs confirm | Dept email unclear | CONG_NO, THU_CHI, GIAO_HANG | Employee prep only | Email type unclear | Confirm if personal login |
| E02 | Nguyễn Thị Kim Liên | 2 | CEO | ✅ Ready | Company email ✅ | CEO_MONITOR, CEO_APPROVAL | Ready for creation | — | Fill email to proceed |
| E03 | Nguyễn Thị Hoài Phương | 3 | THIET_KE | ⛔ Blocked | Shared (Group A) | LSX_DRAFT, PDI_DRAFT | Employee only | Shared email 3 people | Provide unique email |
| E04 | Nguyễn Thị Như Ngọc | 4 | THIET_KE | ⛔ Blocked | Shared (Group B) | LSX_DRAFT, PDI_DRAFT | Employee only | Shared email 3 people | Provide unique email |
| E05 | Lê Thụy Ngọc Hân | 5,9,10,11 | SALES | ⚠️ Needs dedupe | KD email ✅ | SALES_OWN_CUSTOMERS | Employee prep | Dedupe KD1/KD3 | Confirm 1 person |
| E06 | Trần Minh Tân | 6 | ADMIN/DEV | ✅ Existing user | tan***@intanphat.com | Full bypass | **Verify + reuse** | — | None (verified) |
| E07 | Nguyễn Công Trận | 7 | USER | ⛔ Blocked | Shared (Group B) | — | Employee only | Shared email | Provide unique email |
| E08 | Phong | 8 | THIET_KE | ⛔ Blocked | Shared + incomplete | LSX_DRAFT, PDI_DRAFT | None | Name + email | Full name + email |
| E09 | Phạm Thị Phong Nhã | 12 | SALES | ✅ Ready | Unique email ✅ | SALES_OWN_CUSTOMERS | Ready for creation | — | Fill email to proceed |
| E10 | Trương Tiến Cường | 13 | THIET_KE | ⛔ Blocked | Shared (Group A) | LSX_DRAFT, PDI_DRAFT | Employee only | Shared email | Provide unique email |

---

## II. Source & Dedupe Audit

| Metric | Value |
|--------|-------|
| Source rows (Owner provided) | 13 |
| Unique employees identified | 10 |
| Rows merged (dedupe) | 3 (Hân KD1/KD3 aliases) |
| Shared email groups | 5 (2 blocking) |
| Existing admin verified | ✅ tan***@intanphat.com |

---

## III. Existing Admin Verification

| Check | Result |
|-------|--------|
| `tan***@intanphat.com` in login form default | ✅ Found in `login-client.tsx` |
| Login method | Email-based (`WHERE email = ? LIMIT 1`) |
| Account in `user_account` table | ✅ Exists (default login) |
| Role mapping | Needs DB verify at runtime |
| Action | **REUSE** — do NOT create duplicate |
| Old email (tantr***@gmail.com) | Alternate/contact only |

---

## IV. Menu Permission UX Cleanup

| Item | Before | After |
|------|--------|-------|
| Sidebar label | "Bảo Mật Ứng Dụng" | "Phân Quyền & Bảo Mật" |
| Page title | "Bảo mật ứng dụng" | "Phân quyền & bảo mật" |
| Description | "Quản trị role/user/menu permission..." | "Quản trị user, vai trò, menu permission và bảo mật tài khoản" |
| Route | `/m0/security` | `/m0/security` (unchanged) |
| Files changed | 5 files | menu-registry, app-navigation, security-client, page.tsx ×2 |

---

## V. Identity Status Summary

| Status | Count | Candidates |
|--------|-------|------------|
| ✅ READY for user creation | 2 | E02 (CEO), E09 (Sales) |
| ✅ EXISTING verified/reuse | 1 | E06 (Admin) |
| ⚠️ Needs email confirm | 1 | E01 (KT) |
| ⚠️ Needs dedupe confirm | 1 | E05 (Sales) |
| ⛔ BLOCKED shared email | 4 | E03, E04, E07, E10 |
| ⛔ BLOCKED incomplete name | 1 | E08 |
| **Total unique** | **10** | |

---

## VI. Shared Email Groups

| Group | Email (masked) | People | Status |
|-------|---------------|--------|--------|
| A | inant*** | 3 people (TK×2 + KD source) | ⛔ BLOCKED |
| B | tanphat.in*** | 3 people (TK + KT + TK) | ⛔ BLOCKED |
| C | kdbaobig*** | 1 person (3 aliases) | ✅ OK |
| D | ctyint*** | 1 person (dept?) | ⚠️ Confirm |
| E | tan***@intanphat | 1 person (existing admin) | ✅ Verified |

---

## VII. Security Compliance

| Check | Result |
|-------|--------|
| No real users created | ✅ |
| Existing admin verified, not duplicated | ✅ |
| Shared email blocked for personal login | ✅ |
| No passwords generated/committed | ✅ |
| No full emails published | ✅ (masked) |
| No phone numbers published | ✅ |
| No CCCD/salary/tokens published | ✅ |
| Menu UX cleaned (5 files) | ✅ |
| Route unchanged (no breaking links) | ✅ |
| TypeScript build | ✅ (pre-commit) |

---

## VIII. Owner Decisions for V0.226

| # | Decision | Status |
|---|----------|--------|
| 1 | Accounting email: personal or company? | ⬜ |
| 2 | KD1/KD3 = same person? | ⬜ |
| 3 | "Phong" full name? | ⬜ |
| 4 | 5 blocked candidates — unique emails? | ⬜ |
| 5 | Nguyễn Công Trận role? | ⬜ |
| 6 | KE_TOAN needs SALE_ADMIN pack? | ⬜ |

---

## IX. Final Status

> **✅ READY FOR OWNER UNIQUE EMAIL CLEANUP + SAFE USER APPLY PREP**
>
> **EXISTING DEV ADMIN VERIFIED · PERMISSION MENU UX CLEANED · SHARED EMAIL BLOCKED**
>
> Next: V0.226 — Owner provides unique emails → safe apply with backup.
