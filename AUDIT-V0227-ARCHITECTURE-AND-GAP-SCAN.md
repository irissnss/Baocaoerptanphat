# 🔍 AUDIT-V0227 — Architecture Confirm + Gap Scan

> **Ngày:** 05/07/2026
> **Plan ID:** AUDIT-20260705-ARCH-CONFIRM-AND-GAP-SCAN
> **Mode:** READ-ONLY AUDIT — Không sửa code nghiệp vụ
> **Version local:** V0.229 (version.ts MINOR=229)
> **Latest commit (code repo):** `7a937e4` (worktree checkout)

---

## R1 — Architecture Decision Record (ADR)

### Xác nhận kiến trúc thật

| Quyết định | Xác nhận |
|-----------|---------|
| **NestJS** | ❌ KHÔNG dùng — 0 reference trong `package.json`, 0 `@nestjs/*` imports |
| **Prisma** | ❌ KHÔNG dùng — 0 reference trong `package.json`. 2 file `.prisma` tìm thấy chỉ trong `template/metronic-v9.4.6/` (template tham khảo, không phải code dự án) |
| **Mobile App** | ❌ KHÔNG có — không có React Native, Flutter, Expo, hay bất kỳ mobile framework nào |
| **ORM** | ❌ KHÔNG dùng ORM — dùng `mysql2 ^3.14.0` raw queries qua `src/lib/db.ts` |
| **Framework** | ✅ Next.js 16.1.6 (Server Actions + Server Components) |
| **Real-time** | ✅ SSE (Server-Sent Events) |
| **DB** | ✅ MySQL 8.4.3 (Laragon local, VPS production) |

### Dependencies (package.json evidence)

**Production deps (18 packages):**
- Framework: `next ^16.1.6`, `react ^19.2.4`, `react-dom ^19.2.4`
- DB: `mysql2 ^3.14.0` (raw SQL, no ORM)
- Auth: `bcryptjs ^3.0.3`
- UI: Radix UI (12 packages), `lucide-react`, `cmdk`, `sonner`
- CSS: `tailwindcss ^4.2.1`, `tailwind-merge`, `tw-animate-css`
- Form: `react-hook-form ^7.71.1`, `@hookform/resolvers ^5.2.2`, `zod ^4.3.6`
- Misc: `dotenv ^16.4.7`, `read-excel-file ^7.0.1`, `next-themes ^0.4.6`

**Dev deps (8 packages):**
- TypeScript, ESLint, postcss, tsx, archiver

### NestJS/Prisma grep results

```
grep "@nestjs|prisma" package.json → No results found
grep "*.prisma" files → 2 files ONLY in template/metronic-v9.4.6/ (not project code)
```

### ADR Record
> **ADR-20260705:** Architecture pivot confirmed — No NestJS, no Prisma, no mobile app.
> Stack: Next.js 16 (Server Actions + Server Components) + mysql2 raw SQL + SSE.
> Decision by: Owner. Confirmed: 05/07/2026.

---

## R2 — Audit `/mf/doi-chieu` (Đối Chiếu Công Nợ)

### Current implementation

| File | Lines | Content |
|------|-------|---------|
| `src/app/mf/doi-chieu/page.tsx` | 31 | **UI PLACEHOLDER ONLY** |

**Code evidence (full):**
```tsx
// Line 24:
<p className="text-muted-foreground">Chức năng đang được phát triển...</p>
```

- **Actions.ts:** Không có
- **Store:** Không có
- **DB tables:** Không có bảng `cong_no_doi_chieu` hay tương đương

### So sánh với V3.44 Draft (Option C — `cong_no_doi_chieu`)

| Tiêu chí | `/mf/doi-chieu` (hiện có) | V3.44 `cong_no_doi_chieu` (đề xuất) |
|----------|---------------------------|--------------------------------------|
| Code thật | PLACEHOLDER (0 logic) | Chưa code (draft spec) |
| DB table | Không | Đề xuất tạo mới |
| Chức năng | "Đối chiếu công nợ theo kỳ" (label) | Nghiệm thu + đối chiếu công nợ |

### Đề xuất (Owner quyết định)

> **(a) Hoàn toàn trùng scope — nên dùng lại route `/mf/doi-chieu` có sẵn.**
> Route đã tồn tại, tên chức năng khớp, chỉ là placeholder. V3.44 nên implement trực tiếp vào route này thay vì tạo route mới.

---

## R3 — Audit `dm_nhan_vien` References

### Grep results (toàn repo, bao gồm docs + scripts)

| File | Dòng | Nội dung | Loại |
|------|------|---------|------|
| `scripts/phase5-db-evidence-full.ts` | 140, 191, 193 | SELECT sample data (diagnostic script) | Legacy script |
| `docs/.../M4 2 - Routing...` | 91 | FK comment trong spec doc | Notion-exported doc |
| `docs/.../M4 3 - Thực Thi...` | 58, 108, 158, 159 | FK comment trong spec doc | Notion-exported doc |
| `docs/.../M4 1 - Lệnh SX...` | 54 | FK comment trong spec doc | Notion-exported doc |

### Kết luận

```
grep "dm_nhan_vien" src/ → 0 results (KHÔNG có trong code thật)
grep "dm_nhan_vien" src/app/m8/ → 0 results (M8 Task KHÔNG tham chiếu)
```

**✅ KHÔNG phải bug hoạt động.** `dm_nhan_vien` chỉ còn trong:
1. Legacy diagnostic script (`scripts/phase5-db-evidence-full.ts`) — not runtime
2. Notion-exported documentation (FK comments) — reference docs only
3. Code thật (`src/`) đã dùng `hr_employee_nhanvien` (đúng SSOT Prefix Rule #5)

---

## R4 — Audit MC (Marketing/Content) Scope

### Code evidence

| File | Size | Content |
|------|------|---------|
| `mc-client.tsx` | 31KB, 760 dòng | **100% Contract Management** |
| `actions.ts` | 7KB | CRUD: hopDong, hopDongChiTiet, mauHopDong |
| `page.tsx` | 789B | Server Component loads contract data |

### Tính năng thật đã code

| Feature | Evidence |
|---------|---------|
| Mẫu hợp đồng (McMauHopDong) | `createMauHopDongAction`, `updateMauHopDongAction`, `getMauHopDongList` |
| Hợp đồng (McHopDong) | `createHopDongAction`, `updateHopDongAction`, `getHopDongList` |
| Chi tiết hợp đồng (McHopDongChiTiet) | `createHopDongChiTietAction`, `updateHopDongChiTietAction`, `deleteHopDongChiTietAction` |
| Workflow trạng thái | `transitionHopDongAction` |
| Loại hợp đồng | `ban_hang, mua_hang, dich_vu, lao_dong, khac` |

### Content Marketing features found

```
grep "content.*marketing|blog|campaign|seo|social|post|article" mc-client.tsx → 0 results
```

**✅ MC = Contract Management ONLY.** Mô tả "content marketing" trong README là **dự kiến chưa code** — cần sửa README cho chính xác.

---

## R5 — Version Drift Check

### Local dev

| Item | Value |
|------|-------|
| `version.ts` MINOR | 229 → **V0.229** |
| Latest commit | `7a937e4` (worktree checkout) |
| Latest feature commit | `c0c2e22` (Vietnamese mojibake fix) |
| Latest tagged commit | `2cc1cdd` (V0.225E) |

### Report repo (`Baocaoerptanphat`)

| Item | Value |
|------|-------|
| Latest commit | `b44f383` (V0.226B Final guard) |
| README version | V0.226B |

### Drift analysis

| Source | Version shown |
|--------|--------------|
| `version.ts` (local) | V0.229 |
| README (report repo) | V0.226B |
| Last git tag (code repo) | V0.225E |
| Batch 1 CEO apply (this session) | V0.227 (label) |

**⚠️ DRIFT DETECTED:**
- `version.ts` = V0.229 (MINOR=229) nhưng README/report = V0.226B
- V0.227, V0.228, V0.229 chưa được sync vào CHANGELOG/README
- Git tags chỉ đến V0.225E — chưa tag V0.226/V0.227/V0.228/V0.229

---

## Summary & Recommendations

| # | Finding | Severity | Action |
|---|---------|----------|--------|
| R1 | No NestJS/Prisma/Mobile — confirmed | ℹ️ Info | Log ADR ✅ |
| R2 | `/mf/doi-chieu` = placeholder, trùng scope V3.44 | ⚠️ Medium | Owner quyết định merge/keep |
| R3 | `dm_nhan_vien` only in docs/scripts, not in runtime code | ℹ️ Info | No action needed |
| R4 | MC = Contract Management only, no content marketing | ⚠️ Low | Update README mô tả MC |
| R5 | Version drift: code=V0.229 vs report=V0.226B | ⚠️ Medium | Sync changelog + tags |
