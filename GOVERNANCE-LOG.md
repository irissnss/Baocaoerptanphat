# 🔒 Governance Log — ERP Tân Phát

> Lịch sử thay đổi governance rules, skills, và sync events.

---

## Governance Files (5-WAY SYNC)

| File | Vai trò |
|------|---------|
| `AGENTS.md` | Master rules — 14 sections |
| `CLAUDE.md` | Sync 100% với AGENTS.md |
| `GEMINI.md` | Sync 100% với AGENTS.md |
| `.cursorrules` | Sync 100% với AGENTS.md |
| `.antigravityrules` | Sync 100% với AGENTS.md |

**Quy tắc:** Khi update 1 file → tự động sync sang 4 files còn lại. Verify bằng SHA256 hash.

---

## Governance Rules (14 sections trong AGENTS.md)

| # | Tên | Mô tả |
|---|-----|-------|
| 0 | CORE PRINCIPLES | SSOT, Notion MCP first, version+changelog bắt buộc, verify 100% |
| 1 | LANGUAGE | Tiếng Việt mặc định, ghi rõ nguồn, text-first |
| 2 | READ-FIRST ORDER | .cursorrules → AGENTS.md → SKILL.md → Notion → FullSpec |
| 3 | THINK BEFORE DO | Phân tích trước, hỏi khi mơ hồ |
| 4 | DATA SAFETY | Không xóa data gốc, không chia sẻ ngoài workspace |
| 5 | SCOPE CONTROL | Anti-scope-creep, CẤM refactor/drop/rename tự ý |
| 6 | PLAN LABELING | Plan ID, IN/OUT scope, LOCKED, Plan Ledger |
| 7 | 2-PHASE COMMIT | Phase 1 Plan-only → Phase 2 Implement |
| 8 | OUTPUT FORMAT | 10 sections bắt buộc mỗi report |
| 9 | EVIDENCE RULES | DB/UI/Code evidence, text-first |
| 10 | UNREADABLE POLICY | UNREADABLE → yêu cầu ảnh zoom/OCR |
| 11 | UI & FORMAT RULES | Title Case, DD/MM/YY, VN number format |
| 12 | PRICING GUARDS | MM-only, Combo Gate, 2-key, markup% SSOT |
| 13 | COMPLETION GATE | Plan Ledger + Proof + Test tối thiểu |
| 14 | PUBLIC REPORT SYNC | Báo cáo sau mỗi code/fix/audit/deploy |

---

## Skills Inventory (60+ skills)

### Categories:
| Category | Số lượng | Ví dụ |
|----------|----------|-------|
| UI/UX Patterns | 20+ | premium-table-styling, detail-panel-layout, status-color-mapping |
| Module Scaffolding | 5+ | scaffold-module, transactional-page-redesign |
| Data/Schema | 10+ | schema-migration-safe, fk-safe-delete-guard, bundle-transaction-pattern |
| Search/Filter | 5+ | search-normalization, inline-filter-bar-layout |
| Governance | 10+ | versioning-change-history, skill-mining-governance, text-first-report |
| DevOps/Debug | 5+ | debug-systematic, windows-next-cache-stability |
| Form/Input | 5+ | form-field-validation, autocomplete-input-component |

---

## Sync Events Log

| Ngày | Version | Event | Verify |
|------|---------|-------|--------|
| 08/05/2026 | V0.216 | Add rule 14 PUBLIC REPORT SYNC + create public repo | SHA256 MATCH ✅ |
| 04/03/2026 | V0.146 | Sync governance addendum across 5 files | MD5 verified |
| 04/03/2026 | V0.145 | Sync governance files sau AGENTS change | Verified |
| 10/03/2026 | V0.184 | Restore full governance sync sau freeze investigation | Verified |
| 27/01/2026 | V0.26 | 5-WAY SYNC governance files | MD5 hash verified |
| 27/01/2026 | V0.25 | Synced 2 new skills to all 5 files | Verified |
| 26/01/2026 | V0.16 | Establish 3-layer version tracking + 5-way sync rule | Initial setup |

---

## Architecture Decisions

| Quyết định | Mô tả | Version | Locked |
|-----------|-------|---------|--------|
| Server Actions + Server Components | Không tách FE/BE theo REST API | V0.05+ | 🔒 LOCKED |
| SSE (Server-Sent Events) | Cho real-time features | V0.05+ | 🔒 LOCKED |
| Metronic Demo 1 | UI backbone mặc định | V0.197+ | 🔒 LOCKED |
| Next.js 16 + React 19 | Framework chính | V0.153+ | 🔒 LOCKED |
| MySQL | DB engine | V0.00 | 🔒 LOCKED |
| Tailwind 4 | CSS framework | V0.05+ | 🔒 LOCKED |
| No mock data | DB SSOT duy nhất, mock disabled V0.18 | V0.18+ | 🔒 LOCKED |
| 5-way governance sync | 5 files phải identical | V0.16+ | 🔒 LOCKED |

---

## Security Boundaries (Rule 14)

### ĐƯỢC PHÉP public:
- ✅ Version number + changelog entries
- ✅ Module progress (tên + trạng thái)
- ✅ Tech stack tổng quan
- ✅ Thống kê tổng hợp

### NGHIÊM CẤM public:
- ❌ Source code
- ❌ Database schema (CREATE TABLE, columns, FK)
- ❌ API endpoints
- ❌ Credentials (.env, SSH keys)
- ❌ Business logic chi tiết (pricing formulas, workflow rules)
- ❌ Dữ liệu thật (khách hàng, đơn hàng, tài chính)
- ❌ Server/VPS info
- ❌ Governance files (.cursorrules, AGENTS.md...)
