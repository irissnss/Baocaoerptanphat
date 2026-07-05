# 📋 V0.227 Batch 1 — CEO Apply Report

> **Ngày:** 05/07/2026
> **Batch:** 1 — CEO (Nguyễn Thị Kim Liên)
> **Status:** ✅ APPLIED — Awaiting Smoke Test

---

## Pre-Conditions

| Check | Result |
|-------|--------|
| Batch 0 PASS | ✅ |
| DB backup (user_account_backup_v0227) | ✅ 17 rows |
| DB backup (user_role_mapping_backup_v0227) | ✅ 17 rows |
| DB backup (auth_audit_log_backup_v0227) | ✅ |
| OWNER_APPROVED_REAL_USER_CREATION=true | ✅ |
| --apply flag | ✅ |

## Apply Results

| Step | Action | Result |
|------|--------|--------|
| Pre-check | No duplicate email | ✅ 0 existing |
| Pre-check | Role CEO exists | ✅ |
| Pre-check | Backup verified | ✅ |
| INSERT | user_account id=18 | ✅ active, must_change_password=1 |
| INSERT | user_role_mapping CEO | ✅ |
| Audit | auth_audit_log BATCH_CREATE_USER | ✅ |

## Manifest Summary (Runtime Only)

| Field | Value |
|-------|-------|
| batch_id | B1-CEO-20260705 |
| Records: user_account | 1 (id=18, email=li***@intanphat.com) |
| Records: user_role_mapping | 1 (CEO) |
| rollback_applied | false |

## Password Safety

| Check | Status |
|-------|--------|
| Password in file | ❌ NOT present |
| Password in git | ❌ NOT committed |
| Password in report | ❌ NOT included |
| Password in log | ❌ NOT logged |
| must_change_password | ✅ = true |
| Delivery method | Private terminal (1 time display) |

## Smoke Test Checklist (Pending Owner)

| # | Test | Expected | Actual | Status |
|---|------|----------|--------|--------|
| S1 | Login với temp password | ✅ 200 + redirect | — | ⏳ Pending |
| S2 | Forced change password | ✅ Prompt đổi PW | — | ⏳ Pending |
| S3 | Đổi PW → login lại OK | ✅ Success | — | ⏳ Pending |
| S4 | Menu đúng role CEO | ✅ Thấy all modules | — | ⏳ Pending |
| S5 | Không giới hạn customer | ✅ View all | — | ⏳ Pending |
| S6 | Thấy giá vốn/lợi nhuận | ✅ CEO allowed | — | ⏳ Pending |

## Rollback Plan (nếu cần)

```sql
-- R1: Disable account
UPDATE user_account SET trang_thai = 'inactive' WHERE id = 18;
-- R3: Remove role mapping
DELETE FROM user_role_mapping WHERE user_email = 'lienntk@intanphat.com' AND ma_vai_tro = 'CEO';
-- R5 (only if Owner approves): Hard delete
-- DELETE FROM user_account WHERE id = 18;
```

## Final Status

**✅ BATCH 1 APPLIED — CEO ACCOUNT CREATED — AWAITING SMOKE TEST BY OWNER**
