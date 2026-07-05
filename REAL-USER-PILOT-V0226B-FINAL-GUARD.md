# 📋 V0.226B Final Guard Before Owner-Approved Apply

> **Ngày:** 05/07/2026
> **Version:** V0.226B
> **Status:** FINAL GUARD PASSED · BATCH-SCOPED ROLLBACK · ADMIN VERIFY-ONLY · NO REAL USERS CREATED

---

## I. Hardening Changes (V0.226A → V0.226B)

| Item | V0.226A | V0.226B | Status |
|------|---------|---------|--------|
| Rollback scope | Per-user (blanket) | **Batch-scoped** (manifest-tracked) | ✅ Fixed |
| Rollback manifest | None | **Private runtime manifest** per batch | ✅ Added |
| R3 role mapping | "created by batch" (vague) | **Chỉ manifest IDs** (exact) | ✅ Fixed |
| Pre-existing records | Could be affected | **Explicitly excluded** | ✅ Fixed |
| Batch 0 | "REUSE — no create" | **VERIFY-ONLY — read + report only** | ✅ Fixed |
| Batch 0 constraints | Implicit | **6 explicit ❌ rules** (no reset/revoke/disable/remove/dup/DML) | ✅ Added |
| Shared email bypass | Blocked | Blocked + **no fake email workaround** | ✅ Confirmed |

---

## II. Batch 0 — Admin/Dev Verify-Only Contract

| Rule | Detail |
|------|--------|
| Query type | **SELECT only** (read + report) |
| Verify account | Check `user_account` WHERE email = tan***@intanphat.com |
| Verify role | Check role mapping = ADMIN |
| Verify login identity | Confirm default login form |
| ❌ Reset password | KHÔNG |
| ❌ Revoke session | KHÔNG |
| ❌ Disable account | KHÔNG |
| ❌ Remove role mapping | KHÔNG |
| ❌ Create duplicate | KHÔNG |
| ❌ INSERT/UPDATE/DELETE | KHÔNG |
| Output | PASS/FAIL report only |

---

## III. Batch 1 CEO — Apply Contract

| Step | Detail |
|------|--------|
| 1 | Owner approve |
| 2 | DB snapshot backup |
| 3 | DRY_RUN → verify log |
| 4 | `--apply` with `OWNER_APPROVED_REAL_USER_CREATION=true` |
| 5 | Rollback manifest created |
| 6 | `must_change_password = true` |
| 7 | Temp password → private terminal only |
| 8 | Smoke test CEO (6 items) |
| 9 | PASS → proceed to Batch 2 |
| 10 | FAIL → rollback (batch-scoped) → STOP |

---

## IV. Batch 2 Sales — Apply Contract

| Step | Detail |
|------|--------|
| Pre | Batch 1 must PASS |
| 1–7 | Same as Batch 1 |
| 8 | Smoke test Sales (6 items) |

### Sales Smoke Test

| # | Test | Expected |
|---|------|----------|
| S1 | Login được | ✅ |
| S2 | Chỉ thấy KH của mình/được phân bổ | ✅ |
| S3 | Không thấy giá vốn/lợi nhuận/margin | ✅ |
| S4 | Không transfer customer | ✅ |
| S5 | Không delete permanently | ✅ |
| S6 | must_change_password prompt | ✅ |

---

## V. Blocked Users — No Workaround

| Rule | Detail |
|------|--------|
| Shared email | ⛔ Blocked cho user login cá nhân |
| Fake email | ⛔ KHÔNG được dùng email giả để lách |
| Employee-only record | ✅ OK nếu schema cho phép (hr_employee_nhanvien only, no user_account) |
| Unblock path | Owner cung cấp unique email → chuyển sang ready batch |

---

## VI. Rollback Manifest Detail

```
MANIFEST (private, runtime, không commit):
{
  batch_id: "B1-CEO-20260705",
  timestamp_start: "...",
  timestamp_end: "...",
  records_created: {
    user_account: [{ id: X, email: "li***@..." }],
    user_role_mapping: [{ id: Y, user_id: X, role: "CEO" }]
  },
  rollback_applied: false
}
```

> Manifest chỉ tồn tại runtime. Không commit. Không public.
> Rollback chỉ tác động records có trong manifest.

---

## VII. Compliance Checklist

| Check | Result |
|-------|--------|
| Rollback = batch-scoped | ✅ |
| Rollback manifest = private runtime | ✅ |
| Pre-existing records excluded from rollback | ✅ |
| Batch 0 = verify-only (no DML) | ✅ |
| Admin password not reset | ✅ |
| Admin not duplicated | ✅ |
| Shared email blocked (no fake email) | ✅ |
| Password flow: no file/log/report/commit/public | ✅ |
| Batch order: sequential, fail → STOP | ✅ |
| Smoke test required per batch | ✅ |
| No real users created | ✅ |
| No production deploy | ✅ |
| No sensitive data published | ✅ |

---

## VIII. Final Status

> **✅ READY FOR V0.227 BATCH 0 + BATCH 1 OWNER-APPROVED APPLY**
>
> **BATCH-SCOPED ROLLBACK VERIFIED · ADMIN VERIFY-ONLY · NO REAL USERS CREATED**
