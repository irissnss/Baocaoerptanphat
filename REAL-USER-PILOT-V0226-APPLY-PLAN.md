# 📋 V0.226 Safe User Apply Plan (Hardened V0.226A)

> **Ngày lập:** 28/06/2026 | **Hardened:** 02/07/2026
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
| Rollback strategy | **Disable-first** (không hard delete mặc định) |

---

## II. Batch Plan (Tuần Tự — Batch N fail → KHÔNG chạy Batch N+1)

### Batch 0 — Verify/Reuse Existing Admin/Dev

| # | Candidate | Role | Email (masked) | Action |
|---|-----------|------|---------------|--------|
| E06 | Trần Minh Tân | ADMIN/DEV | tan***@intanphat.com | **REUSE** — no create, no password reset |

**Quy trình:** Verify tồn tại → verify role mapping → log audit → PASS/FAIL.
**Nếu FAIL:** DỪNG toàn bộ. Báo Owner.

---

### Batch 1 — CEO (Ready) — Chỉ sau Batch 0 PASS

| # | Candidate | Role | Email (masked) | Action |
|---|-----------|------|---------------|--------|
| E02 | Nguyễn Thị Kim Liên | CEO | li***@intanphat.com | Create user + assign CEO role |

**Quy trình:**
1. DRY_RUN → log output → verify
2. Owner approve → --apply
3. **Smoke test CEO** (xem bảng bên dưới)
4. Nếu PASS → tiếp Batch 2. Nếu FAIL → rollback + DỪNG.

---

### Batch 2 — Sales (Ready) — Chỉ sau Batch 1 PASS

| # | Candidate | Role | Email (masked) | Action |
|---|-----------|------|---------------|--------|
| E09 | Phạm Thị Phong Nhã | SALES | nh***@... (unique) | Create user + assign SALES role |

**Quy trình:** Tương tự Batch 1. Smoke test Sales.

---

### Batch 3 — Kế Toán (Chờ Confirm) — Chỉ sau Owner confirm email type

| # | Candidate | Role | Blocker |
|---|-----------|------|---------|
| E01 | Nguyễn Thị Bạch Kim | KE_TOAN | Accounting email type chưa confirm |

---

### Batch 4 — Sales Dedupe (Chờ Confirm) — Chỉ sau Owner confirm KD aliases

| # | Candidate | Role | Blocker |
|---|-----------|------|---------|
| E05 | Lê Thụy Ngọc Hân | SALES | KD aliases chưa confirm = 1 person |

---

### Batch 5 — Blocked (Shared Email / Incomplete)

| # | Candidate | Role | Required |
|---|-----------|------|----------|
| E03 | Nguyễn Thị Hoài Phương | THIET_KE | Unique email |
| E04 | Nguyễn Thị Như Ngọc | THIET_KE | Unique email |
| E07 | Nguyễn Công Trận | USER | Unique email |
| E08 | Phong | THIET_KE | Full name + unique email |
| E10 | Trương Tiến Cường | THIET_KE | Unique email |

---

## III. Script Safety Rules (Hardened V0.226A)

| Rule | Implementation |
|------|---------------|
| Default DRY_RUN | Script chỉ log, không INSERT |
| Apply gate | `OWNER_APPROVED_REAL_USER_CREATION=true` + `--apply` flag |
| Backup | DB snapshot trước mỗi batch apply |
| No duplicate | Check `user_account.email` trước INSERT |
| No shared email | Reject nếu email thuộc shared group |

### Password Flow (Hardened)

| Rule | Implementation |
|------|---------------|
| `must_change_password` | `= true` cho tất cả user mới |
| Không ghi file/log | Password không ghi vào file, log, hoặc report |
| Không commit | Password không commit vào git |
| Không public | Không hiện trong public report |
| Runtime only | Temp password sinh runtime, hiện 1 lần trên private terminal |
| Delivery | Owner nhận password riêng qua kênh bảo mật (trực tiếp/chat riêng) |

### Rollback Strategy (Hardened — Disable-First)

| Bước | Action | Điều kiện |
|------|--------|-----------|
| R1 | **Disable account** (`trang_thai = 'disabled'`) | Mặc định |
| R2 | **Revoke sessions** (xóa session/cookie nếu có) | Mặc định |
| R3 | **Remove role mappings** created by batch | Mặc định |
| R4 | **Mark audit** (ghi lý do rollback) | Mặc định |
| R5 | **Hard DELETE** | CHỈ KHI: chưa login + chưa session/audit + mới tạo trong batch + Owner approve |

> ⚠️ **KHÔNG hard delete mặc định.** Disable-first để bảo toàn audit trail.

---

## IV. Smoke Test Checklist (Bắt buộc sau mỗi batch)

### CEO Smoke Test

| # | Test | Expected |
|---|------|----------|
| C1 | Login được | ✅ 200 + redirect to dashboard |
| C2 | Dashboard/menu hiện đúng | ✅ Thấy tất cả modules |
| C3 | Role hiển thị CEO | ✅ Badge/context = CEO |
| C4 | Xem cost/profit/margin | ✅ Được phép (nếu rule cho phép) |
| C5 | Session ổn định | ✅ Không bị expire ngay |
| C6 | must_change_password | ✅ Prompt đổi MK lần đầu |

### Sales Smoke Test

| # | Test | Expected |
|---|------|----------|
| S1 | Login được | ✅ 200 + redirect |
| S2 | Chỉ thấy KH được phân bổ | ✅ Filtered by user |
| S3 | Không thấy giá vốn/lợi nhuận/margin | ✅ Hidden |
| S4 | Không transfer customer | ✅ Blocked |
| S5 | Không delete permanently | ✅ Blocked |
| S6 | must_change_password | ✅ Prompt đổi MK lần đầu |

### Admin/Dev Verification

| # | Test | Expected |
|---|------|----------|
| A1 | tan***@intanphat.com không bị reset password | ✅ Password giữ nguyên |
| A2 | Không tạo duplicate admin | ✅ Chỉ 1 account |
| A3 | Role mapping đúng | ✅ ADMIN role intact |

---

## V. Permission Guards (V0.226)

| Guard | Scope |
|-------|-------|
| No cost/profit view | Non-ADMIN/CEO users |
| No transfer customer | KE_TOAN / SALES / THIET_KE |
| No delete permanently | Non-ADMIN users |
| No salary/payroll view | Non-ADMIN/CEO/HR users |
| No user management | Non-ADMIN users |

---

## VI. Owner Decisions Required

| # | Decision | Impact |
|---|----------|--------|
| D1 | Approve Batch 1 (CEO) apply? | Tạo user CEO thật |
| D2 | Approve Batch 2 (Sales) apply? | Tạo user Sales thật |
| D3 | Confirm Bạch Kim accounting email type? | Unblock Batch 3 |
| D4 | Confirm KD aliases = 1 person? | Unblock Batch 4 |
| D5 | Provide unique emails cho 5 blocked users? | Unblock Batch 5 |

---

## VII. V0.226A Hardening Changes (vs V0.226)

| Item | V0.226 (trước) | V0.226A (sau) |
|------|---------------|---------------|
| Rollback | DELETE by email | **Disable-first** (R1→R5) |
| Password delivery | "1 lần trên terminal" | **Private terminal + Owner kênh bảo mật** |
| Password log | "không log" | **Explicit: không file/log/report/commit/public** |
| Batch order | Parallel ok | **Tuần tự bắt buộc, fail → DỪNG** |
| Smoke test | Không có | **Checklist 6 items/role** |
| Hard delete | Mặc định | **Chỉ khi 4 điều kiện thỏa + Owner approve** |

---

## VIII. Final Status

> **📋 PLAN HARDENED — NO REAL USERS CREATED**
>
> Rollback: disable-first · Password: private-only · Batch: sequential + smoke test
>
> **READY FOR OWNER-APPROVED BATCH 0/1 APPLY**
