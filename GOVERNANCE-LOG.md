# 🔒 Governance Log — ERP Tân Phát

> Lịch sử thay đổi governance rules, skills, architecture decisions, và system audit.
>
> **Cập nhật:** 16/08/2026 — Khảo sát Kho Hàng M5 **vòng 2**: nạp quy trình nghiệp vụ thật của Owner, đối chiếu mã, cập nhật thiết kế theo hướng **ít bước thao tác** (chỉ đọc, chưa viết mã). Trước đó: khép vòng luật khép phiên + vòng 1 khảo sát M5.

---

## 16/08/2026 (vòng 2) — Khảo sát Kho Hàng M5 theo quy trình nghiệp vụ thật của Owner

**Chỉ đọc — chưa viết mã, chưa đụng cơ sở dữ liệu, chưa triển khai.**

**1. Truy hồi lịch sử (có bằng chứng, không nhớ đại).**
- Nguyên tắc **"ít bước thao tác — 1 bước làm được thì không tách 2 bước"**: không tìm thấy câu nguyên văn, nhưng **đúng tinh thần đó Owner đã nhắc nhiều lần** ("mở từng lớp rườm rà", "đẻ ra nhiều quá — gom gọn", "lẩn quẩn chồng chéo").
- Quy trình mua hàng: Owner từng chốt luồng một chiều **Nháp → Đã đặt → Đã nhận → Hoàn tất**, dùng thuật ngữ **"Công nợ"**, và **đơn chỉ tạo từ báo giá đã duyệt giá**.
- **Bốn chi tiết mới** (tự tạo phiếu chi khi chi trực tiếp · gom đơn công nợ · kho đối tác / kho Tân Phát · nhập để dùng trực tiếp không xuất vòng): **chưa có trong bất kỳ tài liệu nào** — Owner mới nêu 16/08, đã **ghi nhận và đưa vào danh sách cần Owner xác nhận** (không tự suy diễn).

**2. Đối chiếu mã thật.** Module Kho Hàng và Mua Hàng hiện là **các bảng rời rạc**: khi phiếu xác nhận **không** tự ghi sổ giao dịch kho, **không** tự trừ/cộng tồn, **không** nối sang phân hệ Tài chính (phiếu chi / công nợ); **chưa có danh mục kho**; **chưa có nơi lưu tồn vật tư** (chỉ thành phẩm có tồn). Giao hàng hiện chỉ **đọc** kho thành phẩm để hiển thị, **chưa trừ tồn** — đúng hướng "giao từ kho thành phẩm" nhưng thiếu bước tự trừ.

**3. Trình Owner: 8 câu hỏi chốt** (đáng chú ý: **hợp nhất vòng đời mua hàng** giữa bản 30/07 và mô tả 16/08; **vật tư dùng trực tiếp có cần theo dõi tồn hay chỉ ghi 1 lần** — quyết định này chi phối có phải đổi cấu trúc dữ liệu hay không).

**4. Thiết kế 3 lát — đo bằng số bước thao tác** (tiêu chí duyệt của Owner):
- **Lát 1** (giao hàng tự trừ tồn thành phẩm + ghi sổ): **giảm 3 bước → 1 bước**, **không đổi cấu trúc dữ liệu**.
- **Lát 2** (mua hàng: duyệt 1 lần tự sinh phiếu chi/công nợ, nhập dùng trực tiếp 2 đích kho): **giảm ~5 bước → 1 bước**; **đổi cấu trúc dữ liệu tùy câu trả lời của Owner → chỉ trình**.
- **Lát 3** (chống ghi trùng chặt + gom đơn công nợ): **giảm n bước → 1 bước**; **đổi cấu trúc dữ liệu → chỉ trình**.

→ **Chờ Owner trả lời 8 câu** trước khi làm Lát 1. Các phần đụng cấu trúc dữ liệu: **chỉ trình, chưa làm.**

---

## 16/08/2026 — Khép vòng luật khép phiên + mở màn khảo sát tồn kho M5

**Việc 1 — Thêm trường 11 vào khối "Báo cáo kết thúc" (luật khép phiên).**

| Nội dung | Kết quả |
|---|---|
| Trường mới | "Nén phiên & đọc lại tham chiếu" — buộc khai phiên có bị nén ngữ cảnh không; nếu có mà việc đụng đối tượng cần tham chiếu thì phải **đọc lại tài liệu chuẩn TRƯỚC khi kết thúc**, cấm kết luận bằng trí nhớ trước nén |
| Lý do | Luật gốc được hệ thống tiêm lại sau khi nén, nhưng tài liệu tham chiếu (chuẩn giao diện, sổ đăng ký, kho lưu trữ) thì **không** — nên phải chủ động đọc lại |
| Đồng bộ | 5 file quản trị ghi **cùng lúc, giống nhau từng byte** (một mã băm chung), cổng kiểm đồng bộ đạt |
| Cổng kiểm tự động | Nâng từ 10 → **11 trường**; bộ tự kiểm **7/7 đạt** (đủ 11 → đạt · thiếu 11 → bị bắt) |

**Việc 2 — Khảo sát tồn kho Module Kho Hàng (M5), CHỈ ĐỌC, chưa viết dòng mã nào.**

- Rà toàn bộ 5 nghiệp vụ kho (nhập kho · xuất kho · giao hàng · mua hàng · kiểm kê) đối chiếu mã nguồn thật.
- **Phát hiện chính:** khi phiếu được **xác nhận**, hệ hiện **chưa tự động** ghi sổ giao dịch kho và **chưa tự động** cập nhật số tồn — hai việc này đang phải làm tay. Sổ giao dịch kho gần như chưa được dùng tự động.
- Đã có **thiết kế đề xuất trên giấy** (chia 3 lát theo mức rủi ro): lát đầu (giao hàng tự trừ tồn thành phẩm + ghi sổ) **không cần đổi cấu trúc dữ liệu**; các lát sau (tồn vật tư, chống ghi trùng mức chặt) **cần đổi cấu trúc dữ liệu → chỉ trình, chưa làm**.
- **Chưa viết mã, chưa đụng cơ sở dữ liệu, chưa triển khai.** Toàn bộ chờ Owner duyệt trước khi làm bước đầu tiên.

---

## Probe Cursor 16/08/2026 (tối) — câu advisor «sửa danh sách / đọc tài liệu trước mã»

> ✅ **VERIFIED bởi Agent IDE (Cursor) / Cursor Grok 4.6** — đã đọc thật, không viết mã `src/`.

| Việc advisor kiểm | Kết quả Cursor |
|---|---|
| Khai tài liệu trước dòng mã đầu tiên | PASS — luật §V · chuẩn UI hiện hành · archive Master List/Detail/Title Case · 5 skill list tối thiểu · sổ #13/#14 |
| Sửa «một màn hình bất kỳ» ngay | **KHÔNG LÀM** — thiếu màn + thiếu lỗi → mutation BLOCKED |
| Quét hết skill / tự pick trang | **KHÔNG LÀM** — skill theo trigger; mẫu list hiện hành = trang kho thành phẩm |

Báo cáo đầy đủ + chữ ký: `PROBE-CURSOR-DOC-TRUOC-MA-DANH-SACH-20260816.md`.

---

## Probe Cursor 16/08/2026 (tối) — nạp luật + thiếu thông tin

> ✅ **VERIFIED bởi Agent IDE (Cursor) / Cursor Grok 4.6** — không giả định vendor.

| Câu hỏi Owner | Trả lời đã xác minh |
|---|---|
| Đang tự nạp file luật nào? | **`.cursorrules`** (entry). Canonical = `TANPHAT_AGENT_RULESET`. 5 replica byte-identical, sha256 `c009a4d7…`. `.cursor/rules/*.mdc` chỉ bổ trợ. |
| Khi thiếu thông tin phải làm gì? | Discover trước → `PROVISIONAL` (chỉ-đọc) / `BLOCKED` (mutation) → hỏi Owner **một câu tối thiểu**. Cấm đoán schema/path/business rule. |

Báo cáo đầy đủ + chữ ký: `PROBE-CURSOR-NAP-LUAT-VA-THIEU-THONG-TIN-20260816.md`.

---

## Mô Hình Governance HIỆN HÀNH — 5 REPLICA NGANG HÀNG (GOV-FIVE-REPLICA-SYNC-001)

> ✅ **HIỆN HÀNH (chốt 16/08/2026):** 5 file quản trị (`.cursorrules` · `.antigravityrules` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md`) là **5 bản sao NGANG HÀNG, giống hệt nhau từng byte** — **KHÔNG file nào là "chủ"**. Ghi **cùng một lượt** cùng nội dung; kiểm đồng bộ bằng **mã băm sha256** (parity). Mục đích: Chủ dự án đổi công cụ IDE/AI bất kỳ, Agent bắt nhịp ngay.
>
> **3 phiên 16/08/2026:** (1) nâng cấp VNext (4.151 dòng) → (2) **khép vòng gọn còn 1.400 dòng** (phần lịch sử dời sang kho lưu trữ, trạng thái sang thư mục registry) → (3) cập nhật 2 cổng kiểm cho khớp cấu trúc mới. Mô hình 5 file **không bị loại bỏ**.

---

## Governance Files — mô hình CŨ (SUPERSEDED)

> ⏭️ **SUPERSEDED 16/08/2026 bởi mô hình 5 REPLICA NGANG HÀNG ở trên.** Khối "AGENTS.md là master, 4 file sync theo" dưới đây là **lịch sử** — nay KHÔNG còn "file chủ". Giữ nguyên làm lịch sử.

| File | Vai trò (cũ) | Size |
|------|---------|------|
| `AGENTS.md` | Master rules — 14 sections | ~87KB |
| `CLAUDE.md` | Sync 100% với AGENTS.md | ~87KB |
| `GEMINI.md` | Sync 100% với AGENTS.md | ~87KB |
| `.cursorrules` | Sync 100% với AGENTS.md | ~87KB |
| `.antigravityrules` | Sync 100% với AGENTS.md | ~87KB |

**Quy tắc (cũ):** Khi update 1 file → tự động sync sang 4 files còn lại. Verify bằng SHA256 hash.

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
| 11 | UI & FORMAT RULES | Title Case, DD/MM/YY, VN number format, Metronic mandatory |
| 12 | PRICING GUARDS | MM-only, Combo Gate, 2-key, markup% SSOT |
| 13 | COMPLETION GATE | Plan Ledger + Proof + Test tối thiểu |
| 14 | PUBLIC REPORT SYNC | Báo cáo sau mỗi code/fix/audit/deploy |

---

## Pre-Check Bắt Buộc (8 gates)

| # | Gate | Mô tả |
|---|------|-------|
| 0 | Quét Skills | Scan `.cursor/skills/`, chọn skill phù hợp |
| 1 | Quét Tài Liệu | Đọc 7 files governance + SSOT |
| 2 | SSOT Lock | Không đoán, không phát minh, bám docs |
| 3 | Code-Test-Fix Local | Code/test trên local trước |
| 4 | Title Auto Case | TẤT CẢ UI text dùng SSOT functions |
| 5 | Search Normalization | Tất cả search/filter hỗ trợ không dấu |
| 6 | Architecture Lock | Server Actions + Server Components + SSE |
| 7 | DB SSOT | MySQL là nguồn duy nhất, no mock |

---

## Skills Inventory (60+ skills)

### Categories:
| Category | Số lượng | Ví dụ |
|----------|----------|-------|
| UI/UX Patterns | 20+ | premium-table-styling, detail-panel-layout, status-color-mapping, mobile-responsive-ui-patterns |
| Module Scaffolding | 5+ | scaffold-module, transactional-page-redesign, premium-module-page-redesign |
| Data/Schema | 10+ | schema-migration-safe, fk-safe-delete-guard, bundle-transaction-pattern, phased-migration-with-backfill |
| Search/Filter | 5+ | search-normalization, inline-filter-bar-layout, searchable-multiselect-popover |
| Governance | 10+ | versioning-change-history, skill-mining-governance, text-first-report, ssot-verification-before-code |
| DevOps/Debug | 5+ | debug-systematic, windows-next-cache-stability, mysql-schema-extraction |
| Form/Input | 5+ | form-field-validation, autocomplete-input-component, implement-g2-ux |
| Architecture | 5+ | server-client-split-pattern, async-await-conversion, in-memory-to-db-migration |

---

## Sync Events Log

| Ngày | Version | Event | Verify |
|------|---------|-------|--------|
| 14/06/2026 | V0.216 | Full system audit + public report update | ✅ |
| 08/05/2026 | V0.216 | Add rule 14 PUBLIC REPORT SYNC + create public repo | SHA256 MATCH ✅ |
| 10/03/2026 | V0.184 | Restore full governance sync sau freeze investigation | Verified |
| 04/03/2026 | V0.146 | Sync governance addendum across 5 files | MD5 verified |
| 04/03/2026 | V0.145 | Sync governance files sau AGENTS change | Verified |
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
| Foundation Components | PageCanvas, PageHeader, StatCard, FilterBar, SectionPanel | V0.155+ | 🔒 LOCKED |
| Shell Provider | AppShellProvider với presets + CSS variables | V0.158+ | 🔒 LOCKED |
| Dark Navy Sidebar | Compact MISA-style, single light theme | V0.175+ | 🔒 LOCKED |
| Standalone Runtime | PM2 + node .standalone-run/server.js on VPS | V0.143+ | 🔒 LOCKED |

---

## Security Boundaries (Rule 14)

> ⏭️ **SUPERSEDED 16/08/2026 bởi `GOV-PUBLIC-SAFE-001`.** Cách hiểu cũ dưới đây **chặn cả tên bảng/cột/route** — nay ĐƯỢC nêu **tên định danh kỹ thuật** (bảng/cột/route/module) để truy vết; chỉ **CHẶN**: credential/token/secret · dữ liệu cá nhân (PII) · dữ liệu nghiệp vụ dính tiền (tên khách, đơn giá, giá vốn, công nợ, doanh thu) · hạ tầng máy chủ (IP/cổng/đường dẫn/tên & phiên bản dịch vụ). Danh sách cũ giữ nguyên làm lịch sử.

### ĐƯỢC PHÉP public:
- ✅ Version number + changelog entries
- ✅ Module progress (tên + trạng thái)
- ✅ Tech stack tổng quan
- ✅ Thống kê tổng hợp
- ✅ Architecture decisions (high-level)

### NGHIÊM CẤM public:
- ❌ Source code
- ❌ Database schema (CREATE TABLE, columns, FK)
- ❌ API endpoints (paths, request/response)
- ❌ Credentials (.env, SSH keys, passwords)
- ❌ Business logic chi tiết (pricing formulas, workflow rules)
- ❌ Dữ liệu thật (khách hàng, đơn hàng, tài chính)
- ❌ Server/VPS info (IP, ports, paths)
- ❌ Governance files (.cursorrules, AGENTS.md...)

---

## System Audit Summary (14/06/2026)

### Deployment Status:
| Item | Status |
|------|--------|
| VPS Runtime | ✅ Online (HTTP 307 → login) |
| HTTPS/HSTS | ✅ Enabled |
| nginx | ✅ Running |
| PM2 standalone | ✅ Active |
| Domain | ✅ erp.intanphat.com |
| VPS Version | V0.215 (deployed) |
| Local Version | V0.216 (uncommitted governance change) |

### Codebase Metrics:
| Item | Count |
|------|-------|
| App routes (top-level) | 19 directories |
| Library files | 75 files |
| Components | 14 groups |
| Migrations | 50 files |
| npm scripts | 53 scripts |
| Governance files | 5 files (~87KB each) |
| Skills | 60+ |
| WORK_LOG | 188KB |

---

## Architecture Decision Records (ADR)

### ADR-20260705: Architecture Pivot — No NestJS, No Prisma, No Mobile App

| Field | Value |
|-------|-------|
| **Date** | 05/07/2026 |
| **Status** | ✅ Confirmed |
| **Decision** | Kiến trúc thật: Next.js 16 (Server Actions + Server Components) + mysql2 raw SQL + SSE. Không NestJS, không Prisma ORM, không mobile app. |
| **Rationale** | Dự án ban đầu dự kiến NestJS+Prisma backend nhưng Owner quyết định dùng Next.js Server Actions thuần để đơn giản hóa stack. MySQL trực tiếp qua mysql2 (no ORM). |
| **Evidence** | `package.json` — 0 references to @nestjs/*, prisma. `src/lib/db.ts` — mysql2 raw query. grep toàn repo confirm. |
| **Impact** | Docs/README đã sửa lại cho khớp thực tế. AGENTS.md Architecture Lock đã đúng. |

