# 📋 V0.226 Safe User Apply Plan

> **Ngày lập:** 28/06/2026
> **Trạng thái:** PLAN ONLY — CHƯA TẠO USER THẬT
> **Điều kiện apply:** Owner set `OWNER_APPROVED_REAL_USER_CREATION=true` + chạy `--apply` + backup/rollback point

---

## I. Tổng Quan

| Thông tin | Chi tiết |
|-----------|----------|
| Tổng unique employees | 10 |
| Ready apply ngay | 2 (Batch 1 + 2) |
| Existing reuse | 1 (Batch 0) |
| Chờ Owner confirm | 2 (Batch 3 + 4) |
| Blocked | 5 (Batch 5) |
| Script mode | DRY_RUN mặc định |

---

## II. Batch Plan

### Batch 0 — Verify/Reuse Existing Admin/Dev

| # | Candidate | Role | Email (masked) | Action |
|---|-----------|------|---------------|--------|
| E06 | Trần Minh Tân | ADMIN/DEV | tan***@intanphat.com | **REUSE** — no create, no password reset |

**Điều kiện:** Tự động — chỉ verify tồn tại + role mapping.

---

### Batch 1 — CEO (Ready)

| # | Candidate | Role | Email (masked) | Action |
|---|-----------|------|---------------|--------|
| E02 | Nguyễn Thị Kim Liên | CEO | li***@intanphat.com | Create user + assign CEO role |

**Điều kiện:** Owner approve → DRY_RUN → verify → --apply.
**Post-apply:** `must_change_password = true`, tạo mật khẩu tạm (không log/commit).

---

### Batch 2 — Sales (Ready)

| # | Candidate | Role | Email (masked) | Action |
|---|-----------|------|---------------|--------|
| E09 | Phạm Thị Phong Nhã | SALES | nh***@... (unique) | Create user + assign SALES role |

**Điều kiện:** Owner approve → DRY_RUN → verify → --apply.
**Post-apply:** `must_change_password = true`, tạo mật khẩu tạm (không log/commit).

---

### Batch 3 — Kế Toán (Chờ Confirm)

| # | Candidate | Role | Email (masked) | Blocker |
|---|-----------|------|---------------|---------|
| E01 | Nguyễn Thị Bạch Kim | KE_TOAN | ??? | Accounting email type chưa confirm |

**Điều kiện:** Owner confirm email type (dept vs personal) → thêm vào apply queue.

---

### Batch 4 — Sales Dedupe (Chờ Confirm)

| # | Candidate | Role | Email (masked) | Blocker |
|---|-----------|------|---------------|---------|
| E05 | Lê Thụy Ngọc Hân | SALES | kd***@... | KD aliases chưa confirm 1 person |

**Điều kiện:** Owner confirm KD1 = KD3 = same person → thêm vào apply queue.

---

### Batch 5 — Blocked (Shared Email / Incomplete)

| # | Candidate | Role | Blocker | Required |
|---|-----------|------|---------|----------|
| E03 | Nguyễn Thị Hoài Phương | THIET_KE | Shared email (Group A) | Unique email |
| E04 | Nguyễn Thị Như Ngọc | THIET_KE | Shared email (Group B) | Unique email |
| E07 | Nguyễn Công Trận | USER | Shared email (Group B) | Unique email |
| E08 | Phong | THIET_KE | Shared + incomplete | Full name + unique email |
| E10 | Trương Tiến Cường | THIET_KE | Shared email (Group A) | Unique email |

**Điều kiện:** Owner cung cấp unique email cho từng người → chuyển sang ready batch.

---

## III. Script Safety Rules

| Rule | Implementation |
|------|---------------|
| Default DRY_RUN | Script chỉ log, không INSERT |
| Apply gate | `OWNER_APPROVED_REAL_USER_CREATION=true` + `--apply` flag |
| Backup | DB snapshot trước apply |
| Rollback | Scripted DELETE by email if needed |
| `must_change_password` | `= true` cho tất cả user mới |
| No password log/commit | Password sinh tạm, chỉ hiện 1 lần trên terminal |
| No duplicate admin/dev | Check `user_account.email` trước INSERT |
| No shared email login | Reject nếu email thuộc shared group |

---

## IV. Permission Guards (V0.226)

| Guard | Scope |
|-------|-------|
| No cost/profit view | Non-ADMIN/CEO users |
| No transfer customer | KE_TOAN / SALES / THIET_KE |
| No delete permanently | Non-ADMIN users |
| No salary/payroll view | Non-ADMIN/CEO/HR users |
| No user management | Non-ADMIN users |

---

## V. Owner Decisions Required

| # | Decision | Impact |
|---|----------|--------|
| D1 | Approve Batch 1 (CEO) apply? | Tạo user CEO thật |
| D2 | Approve Batch 2 (Sales) apply? | Tạo user Sales thật |
| D3 | Confirm Bạch Kim accounting email type? | Unblock Batch 3 |
| D4 | Confirm KD aliases = 1 person? | Unblock Batch 4 |
| D5 | Provide unique emails cho 5 blocked users? | Unblock Batch 5 |

---

## VI. Final Status

> **📋 PLAN PREPARED — NO REAL USERS CREATED**
>
> Awaiting Owner approval to proceed with Batch 0 → 1 → 2 → (3,4,5 khi unblocked).
>
> **READY FOR V0.226 OWNER-APPROVED SAFE APPLY**
