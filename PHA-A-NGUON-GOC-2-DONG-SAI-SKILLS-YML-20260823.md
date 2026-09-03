# PHA A — ĐIỀU TRA NGUỒN GỐC 2 DÒNG SAI TRONG `skills.yml`

> **Loại:** ĐIỀU TRA — **CHỈ ĐỌC TUYỆT ĐỐI**. Không sửa file nào ngoài chính báo cáo này. Không chạy script ghi. Không commit. Không push. Không ghi sổ.
> **Ngày:** 23/08/2026 22:20 · **Owner:** TanPhatERP · **Actor:** Agent IDE (Claude Code)
> **Kết luận một dòng:** hai dòng sai có **HAI NGUYÊN NHÂN GỐC KHÁC NHAU**, phải vá ở **hai tầng khác nhau** — và **một tiền đề của quyết định Owner 30/07 đã không còn đúng**.

---

## A0 — MỐC ĐO TRƯỚC KHI ĐỌC

| Mục | Đầu ra thật |
|---|---|
| Lệnh | `git rev-parse --abbrev-ref HEAD` · `git log -1` · `git log -1 --date=iso -- <file>` · `wc -l` · `grep -c ''` · `sha256sum` |
| Nhánh | `main` |
| HEAD | **`<mã-nguồn-riêng>`** · `2026-08-23 21:45:49 +0700` · *"feat(ui): DOT 4 — ma tran quyen menu dang TAB + CAY…"* |
| Cây làm việc | **16 file đang thay đổi** (do phiên khác — phiên này KHÔNG đụng) |
| `skills.yml` sửa cuối | `<mã-nguồn-riêng>` · `2026-08-23 14:56:58` |
| `skills.yml` số dòng | `wc -l` = **4738** · `grep -c ''` = **4738** (khớp) |
| `skills.yml` kích thước | 197.643 byte · `sha256` `<mã-nguồn-riêng>…` |
| `skills.yml` trạng thái git | `git status --porcelain` → **0** (sạch, đúng bản đã commit) |

> ✅ **HEAD hiện tại CHÍNH LÀ `<mã-nguồn-riêng>`** — cùng mốc mà báo cáo audit đã đo. Không cần đo lại chéo.
> ✅ `<mã-nguồn-riêng>` là **tổ tiên** của HEAD (`git merge-base --is-ancestor` → CÓ).

**Xác nhận 2 dòng còn nguyên vị trí** — đo lại bằng `grep -n`, không tin số dòng chép lại:

```
grep -n "alwaysApply"      .governance/registry/skills.yml  → 795
grep -n "Graphify trước"   .governance/registry/skills.yml  → 3642
```

---

## A1 — TRẠNG THÁI HIỆN TẠI CỦA `skills.yml`

### A1.1 Đếm entry và trường (lệnh đếm thật)

| Trường | Số | Lệnh |
|---|---|---|
| entry | **128** | `grep -c '^  - slug:'` |
| `ui_scope:` | **128** | `grep -c '^    ui_scope:'` |
| `ssot_verdict:` | **128** | `grep -c '^    ssot_verdict:'` |
| `ssot_muc:` | **128** | `grep -c '^    ssot_muc:'` |
| `ssot_diem_choi:` | **128** | `grep -c '^    ssot_diem_choi:'` |
| `ssot_chuoi_cam:` | **128** | `grep -c '^    ssot_chuoi_cam:'` |

### A1.2 Phiên Gộp (V2) đã chạy chưa? — **RỒI**

Ba bằng chứng độc lập cùng chỉ một hướng:

1. `git log -- skills.yml` → commit gần nhất **`<mã-nguồn-riêng>`** (`23/08 14:56`) = chính commit V2.
2. Trong file: `meta.generated_at: "2026-08-23"` · `meta.generator: "scripts/tests/skills-registry-build.mjs"` (dòng 24–25).
3. `mtime` file = `2026-08-23 14:51:57` — trước commit vài phút, khớp trình tự *sinh → commit*.

⇒ **V2 đã chạy đầy đủ, 128/128 entry.** Hai dòng sai **không phải** do V2 bỏ sót entry.

### A1.3 Ngữ cảnh ±10 dòng — CẢ HAI PHÍA

**Dòng 795** (entry `dependency-relationship-scan`):

```
 785 ssot_verdict: "KHONG_UI"
 786 ssot_muc: "KHONG_CO"
 787 ssot_diem_choi: 0
 788 ssot_chuoi_cam: []
 789 ssot_ghi_chu: "Toàn bộ 391 dòng là quy trình quét phụ thuộc… không đặt bất kỳ giá trị giao diện nào…"
 790 tracked: true
 791 frontmatter_opens_line_1: true
 792 version: "1.2.0"          ← ⚠️ ĐÁNG CHÚ Ý
 793 bytes: 16363
 794 previous_result: "USAGE_EVIDENCE work_log=5 docs=27 git_log=1"
 795 trap_seen_before: "Verify: graph.json tồn tại; rule graphify.mdc alwaysApply; AGENTS.md cấm API routes/fetch."
 796 symptom:
 797   - "Trigger khi:"
 …
```

> 🔴 **Điểm mấu chốt trong ngữ cảnh:** dòng 792 ghi `version: "1.2.0"`. Nhưng dòng 795 lại chép nội dung của **`v1.1.0`**. **Một entry mang số hiệu v1.2.0 nhưng cảnh báo lấy từ v1.1.0.**

**Dòng 3642** (entry `speckit-erp-ssot-adapter`):

```
3640 - slug: "speckit-erp-ssot-adapter"
3641   name: "speckit-erp-ssot-adapter"
3642   description: "…feature-sized specs, Graphify trước, đồng bộ docs↔code, Owner gate bắt buộc."
3643   domain: "speckit"
3644   operation: "APPLY"
3645   failure_family: "NEEDS_OWNER_INPUT"
3646   health_label: "HEALTHY"
3647   ui_scope: "KHONG_UI"
…
3653   tracked: true              ← ⚠️ ĐÁNG CHÚ Ý (xem A6.3)
3655   version: "1.0.0"
```

---

## A2 — NGUỒN GỐC 2 DÒNG SAI *(câu hỏi trọng tâm)*

### A2.1 Script sinh registry — đọc gì, ghi gì

`scripts/tests/skills-registry-build.mjs` — **535 dòng**.

| Câu hỏi | Trả lời | Bằng chứng |
|---|---|---|
| Đọc từ đâu? | `.cursor/skills/<slug>/SKILL.md` | `:26-28` `ROOT` / `SKILLS_DIR` / `OUT` |
| Ghi vào đâu? | `.governance/registry/skills.yml` | `:28` `const OUT = …skills.yml` |
| **Ghi đè hay thêm?** | 🔴 **GHI ĐÈ TOÀN BỘ** | `:532` `fs.writeFileSync(OUT, header + bodyYaml + "\n", "utf8")` |
| Chạy tự động? | ❌ **KHÔNG** — thủ công | `grep -c 'skills-registry-build' package.json` → **0**; không có trong `pre-commit-hook.sh` |
| Có chế độ chỉ-đọc? | Có — `--check` | `:526` `if (process.argv.includes("--check"))` |

### A2.2 🔴 DÒNG 795 — **SCRIPT SINH**, và **LỖI NẰM Ở SCRIPT**

**Chuỗi truy vết đầy đủ:**

```
skills.yml:795  trap_seen_before
   ↑ sinh bởi  skills-registry-build.mjs:315-319
   ↑ nguồn     .cursor/skills/dependency-relationship-scan/SKILL.md:388
```

Nguyên văn logic sinh (`:315-319`):

```js
// trap_seen_before — chỉ lấy dòng có thật
let trap = "NONE_RECORDED"
for (const l of lines) {
    if (TRAP_LINE.test(l) && !NEG_LINE.test(l)) { const c = cleanLine(l); if (c.length > 12) { trap = c.slice(0, 220); break } }
}
```

`TRAP_LINE` (`:242`) khớp: `bẫy|trap|pitfall|gotcha|CẤM|NEVER|AVOID|KHÔNG ĐƯỢC|MUST NOT|sai lầm|lưu ý`.

**Đo trên file nguồn — kết quả quyết định:**

| Đo | Kết quả |
|---|---|
| Tổng số dòng `SKILL.md` | **391** |
| Số dòng khớp **bất kỳ** từ khoá TRAP | **ĐÚNG 1** — dòng **388** (do chữ *"cấm"* trong `AGENTS.md cấm API routes/fetch`) |
| Dòng 388 nằm trong mục nào? | `### v1.1.0 (2026-07-21)` — mục bắt đầu ở dòng **384**, mục kế `### v1.0.0` ở dòng **390** |
| Mục cha? | `## Changelog` — dòng **376** |
| Số lần script nhắc "changelog" | **1** — và đó là ở `DOMAIN_RULES` để phân loại lĩnh vực, **KHÔNG** phải để bỏ qua mục |

> ### 🔴 NGUYÊN NHÂN GỐC — SCRIPT KHÔNG BIẾT ĐÂU LÀ LỊCH SỬ
>
> Script quét **toàn bộ 391 dòng, không phân biệt mục**, lấy dòng khớp **đầu tiên**, chép **nguyên văn**.
> Dòng duy nhất khớp lại nằm trong **mục Changelog**, thuộc bản **`v1.1.0`** — chính bản đã bị **`v1.2.0` (30/07) thay thế**.
>
> **Kết quả: một dòng LỊCH SỬ bị nâng cấp thành một cảnh báo ĐANG SỐNG trong registry.**
>
> ✅ File `SKILL.md` **KHÔNG SAI** — changelog ghi đúng việc v1.1.0 đã làm.
> ✅ Báo cáo chốt 30/07 (`CHOT:133`) đã xếp dòng 379–382 là **HISTORICAL — giữ**.
> 🔴 Cái sai là **script không tôn trọng nhãn HISTORICAL đó**.

**Không phải gõ tay** — `git blame -L795,795` → `<mã-nguồn-riêng>` (20/08), chính commit sinh registry lần đầu. Không có commit nào sửa tay dòng này về sau.

### A2.3 🔴 DÒNG 3642 — **SCRIPT SINH**, nhưng **LỖI NẰM Ở NỘI DUNG NGUỒN**

```
skills.yml:3642  description
   ↑ sinh bởi  skills-registry-build.mjs:364  →  description: fm.description || NEEDS
   ↑ nguồn     .cursor/skills/speckit-erp-ssot-adapter/SKILL.md:4  (frontmatter description:)
```

Khác hẳn ca 795: đây **không** phải dòng lịch sử bị hoist. Đây là **mô tả tự khai ĐANG SỐNG** của chính kỹ năng đó, và nó **thật sự vẫn ghi "Graphify trước"**.

`grep "graphify"` trên `SKILL.md` → **5 vị trí**: dòng `4` · `33` · `39` · `41` · `107`. Đây là một **cụm nội tại trong file**, không phải một dòng lẻ.

**Về `health_label: "HEALTHY"` (dòng 3646)** — đọc `:343-350`:

```js
const hasName = !!fm.name
const hasDesc = !!fm.description
const hasWhen = symptom[0] !== NEEDS && !symptom[0].startsWith("(rút từ description)")
const hasRef  = /(CLAUDE\.md|\.governance\/|docs\/|src\/|scripts\/|§)/.test(raw)
…
else health = "HEALTHY"
```

Đo trên file nguồn: có `name` ✅ · có `description` ✅ · có `triggers` ✅ · có tham chiếu (2 lần) ✅ → **4/4 điều kiện đạt**.

> ### ⚖️ PHÁN ĐỊNH VỀ `health_label`
>
> `HEALTHY` **ĐÚNG theo đúng định nghĩa của chính nó** — nó đo **sức khoẻ CẤU TRÚC** (đủ frontmatter, có trigger, có tham chiếu), **KHÔNG** đo **nội dung còn hiệu lực hay không**.
>
> 🔴 Vấn đề là **CÁI TÊN GÂY HIỂU NHẦM**: người tra registry đọc `HEALTHY` sẽ tưởng *"kỹ năng này còn dùng tốt"*, trong khi thực tế nó **DORMANT + DE-REFERENCED** từ 30/07.
>
> ⇒ Đây **không phải lỗi giá trị**, mà là **lỗ hổng thiết kế registry**: thiếu một trục đo *"nội dung còn hiệu lực?"*. Đúng cùng loại vấn đề mà đợt V2 đã gặp và đã xử bằng cách **tách trục** (`ssot_verdict` giữ nguyên, thêm `ssot_chuoi_cam` cảnh báo riêng).

**Không phải gõ tay** — `git blame -L3642,3642` → cũng `<mã-nguồn-riêng>`.

### A2.4 Cụm lỗi có lan rộng không? — **KHÔNG, trong luật sống**

Quét `.governance/**` + `docs/**` + 5 file luật:

| Nơi xuất hiện | Số | Phân loại |
|---|---|---|
| `.governance/registry/skills.yml` | **2** | 🔴 **LỖI ĐANG SỐNG** (chính 2 dòng này) |
| `.governance/ARCHIVE-LEGACY-RULESET.md:835` | 1 | ✅ ĐÚNG — ghi rõ `alwaysApply: false`, ADVISORY |
| `docs/BAO-CAO-…-INDEPENDENT-REVIEW-2026-07-30.md` | 6 | ✅ HISTORICAL — hồ sơ phát hiện lỗi |
| `docs/BAO-CAO-CHOT-…-2026-07-30.md` | 6 | ✅ HISTORICAL — hồ sơ khắc phục |
| `docs/_snapshot-2026-08-16-…/AGENTS.md` | 1 | ✅ ẢNH CHỤP — theo định nghĩa là bản đông cứng |
| **5 file luật** (`CLAUDE.md` · `.cursorrules` · `.antigravityrules` · `AGENTS.md` · `GEMINI.md`) | **0** | ✅ SẠCH |
| `.cursor/skills/dependency-relationship-scan/SKILL.md:34` | 1 | ✅ ĐÚNG — *"Read/Grep/Glob trực tiếp LUÔN được phép… Không cần chạy Graphify trước"* |

> ✅ **KHÔNG kích điều kiện dừng** *"phát hiện cụm lỗi lan rộng"*. Mọi nơi khác đều là lưu trữ / hồ sơ / ảnh chụp — đúng bản chất, phải giữ. **Chỉ 2 dòng trong `skills.yml` là lỗi đang sống.**

---

## A3 — BẢN ĐỒ QUY TRÌNH SINH REGISTRY

`grep -rl "skills.yml" scripts/ .governance/ package.json` → **4 file script + 2 file dữ liệu**.

| Script | Đọc / Ghi | Chạy khi nào | Đầu vào | Ghi đè? |
|---|---|---|---|---|
| `scripts/tests/skills-registry-build.mjs` | 🔴 **GHI** | **Thủ công** — không có trong `package.json` (đếm: 0), không trong `pre-commit` | `.cursor/skills/*/SKILL.md` + `ui-skill-verdict.json` + git log/corpus | 🔴 **GHI ĐÈ TOÀN BỘ** (`:532`) |
| `scripts/tests/skills-registry-gate.test.mjs` | Đọc | `npm run test:skills-registry` (trong `test:gov-gates`) | `skills.yml` + thư mục skill | — |
| `scripts/tests/ui-skill-conflict-gate.test.mjs` | Đọc | `npm run test:ui-skill-conflict` (trong `test:gov-gates`) | `skills.yml` + `ui-standard-sources.md` + `UI-STANDARD.md` + `package.json` + `pre-commit-hook.sh` | — |
| `scripts/tests/ref-exists-gate.test.mjs` | Đọc | `npm run test:ref-exists-gate` | 5 file luật + registry | — |

### A3.3 🔴 Sửa tay lên `skills.yml` có bị ghi đè không? — **CÓ, MẤT SẠCH**

Căn cứ đọc mã, không suy đoán: `:532` gọi `fs.writeFileSync(OUT, header + bodyYaml + "\n", "utf8")` — dựng lại **toàn bộ** nội dung từ 128 entry, **không đọc bản cũ, không merge**.

⇒ **Mọi chỉnh sửa tay trên `skills.yml` sẽ biến mất ở lần chạy script kế tiếp.** Đây là căn cứ quyết định tầng vá ở A7.

⚠️ Ghi nhận thêm: `skills-registry-gate.test.mjs:134` có kiểm **"trôi nội dung — registry đã bị sửa tay kể từ lần sinh?"** nhưng ghi rõ **"INFO, không FAIL"**. Nghĩa là nếu ai sửa tay, cổng **có thấy** nhưng **không chặn**.

---

## A4 — `GOV-EDIT-PRESERVE-001` NGUYÊN VĂN

### A4.1 Vị trí — **cùng dòng 741 ở cả 5 file**

| File | Dòng |
|---|---|
| `CLAUDE.md` · `.cursorrules` · `.antigravityrules` · `AGENTS.md` · `GEMINI.md` | **741** |

### A4.2 Nguyên văn phần quyết định cách sửa

```
REQUIREMENT:
  1. Dòng cũ sai → dòng mới thay ở vị trí cũ; dòng cũ chuyển xuống mục
     "Lịch sử sửa đổi" CÙNG FILE, HOẶC dời sang ARCHIVE kèm ĐÚNG MỘT dòng
     con trỏ (mã · vị trí mới · ngày · lý do). Hai cách đều hợp lệ.
  2. Trước khi sửa: quét toàn file tìm MỌI vị trí nói về cùng đối tượng.
     Sửa hết trong cùng một lượt, hoặc nêu rõ chỗ nào cố ý giữ và vì sao.
  3. Thêm điều mới → khai rõ có ghi đè điều nào không. Có → điều cũ gắn
     SUPERSEDED, giữ nguyên văn.
FORBIDDEN:   Ghi đè im lặng · xoá không con trỏ · sửa một chỗ bỏ chỗ khác
FAILURE:     BLOCK_ALL
```

> ⚠️ **Điểm khó áp cho ca này:** `skills.yml` là **file SINH RA TỰ ĐỘNG**, **không có mục "Lịch sử sửa đổi"**, và bị ghi đè toàn bộ mỗi lần chạy. Cả hai cách của yêu cầu 1 đều **không áp thẳng được**. Xem A7 để biết cách xử lý đúng tinh thần luật.

### A4.3 Parity 5 file — **ĐẠT**

```
9251b1870fa2f5549b2bf328f0a25c6db535fd718245342be4e6326e6288d0a8  CLAUDE.md
9251b1870fa2f5549b2bf328f0a25c6db535fd718245342be4e6326e6288d0a8  .cursorrules
9251b1870fa2f5549b2bf328f0a25c6db535fd718245342be4e6326e6288d0a8  .antigravityrules
9251b1870fa2f5549b2bf328f0a25c6db535fd718245342be4e6326e6288d0a8  AGENTS.md
9251b1870fa2f5549b2bf328f0a25c6db535fd718245342be4e6326e6288d0a8  GEMINI.md
```
→ **1 mã duy nhất.**

---

## A5 — CỔNG NÀO CANH 2 DÒNG NÀY? — **KHÔNG CÓ CỔNG NÀO**

### A5.1 `test:ui-skill-conflict` — 6 điều kiện, **không điều kiện nào chạm tới**

| ĐK | Canh gì | Bắt được 2 dòng này? |
|---|---|---|
| a | Nhãn `SUPERSEDED` ở `ui-standard-sources.md` ↔ `ssot_verdict` trong `skills.yml` | ❌ — cả 2 kỹ năng đều `KHONG_UI`, không có mặt ở `ui-standard-sources.md` |
| b | `ui_scope: UI` mà `ssot_verdict: CHUA_DOI_CHIEU` | ❌ — cả 2 là `KHONG_UI` |
| c | Kỹ năng chứa chuỗi SSOT **giao diện** cấm (`rounded-2xl` · `text-3xl` · `max-h-[*vh]`) | ❌ — không liên quan giao diện |
| d | Kỹ năng trên đĩa thiếu entry / entry thiếu nhãn | ❌ — cả 2 có entry đủ nhãn |
| e | Cổng gọi `git ls-files` thiếu cờ `-z` | ❌ — không liên quan |
| f | 3 cổng pre-commit còn được khai + còn được gọi | ❌ — không liên quan |

### A5.2 Có cổng nào canh "mandate công cụ đã chết" không? — **KHÔNG**

| Phép đo | Kết quả |
|---|---|
| `grep -rn "graphify\|Graphify" scripts/` | **0 kết quả** |
| `grep -rc "alwaysApply" scripts/` | **0 file** |

✅ **Xác nhận lại kết luận của audit là ĐÚNG:** *"bệnh công cụ chưa có cổng nào"*. Không một cổng nào trong repo kiểm chứng rằng giá trị trong registry còn khớp với sự thật tại nguồn (`.cursor/rules/*.mdc`, quyết định Owner).

---

## A6 — ĐỐI CHIẾU LỊCH SỬ QUYẾT ĐỊNH *(đọc sổ TRƯỚC khi kết luận — §F1b mục 5)*

### A6.1 Hai sổ chính — **KHÔNG CÓ DÒNG NÀO**

| Sổ | Số dòng nhắc `Graphify` / `alwaysApply` / 2 kỹ năng này |
|---|---|
| `docs/OWNER-REQUEST-LEDGER.md` | **0** |
| `.governance/registry/tech-debt.md` | **0** |

⇒ Không có nợ nào đang ghi nhận 2 dòng này. Theo `GOV-TECH-DEBT-LEDGER-001` §G7.10 mục 3, đây là **lỗi MỚI phát hiện**, không phải nợ đã biết.

### A6.2 Nguồn quyết định duy nhất: `docs/BAO-CAO-CHOT-AI-TOOLING-GOVERNANCE-2026-07-30.md`

| Đối tượng | Dòng | Trạng thái CHÍNH THỨC 30/07 |
|---|---|---|
| `dependency-relationship-scan` (dòng 53, 370) | `CHOT:132` | ✅ **CURRENT — ĐÚNG** (`alwaysApply: false`, ADVISORY) |
| `dependency-relationship-scan` (dòng 379–382, Changelog) | `CHOT:133` | ✅ **HISTORICAL — giữ** |
| `speckit-erp-ssot-adapter` (dòng 4, 24, 33, 39, 41, 44, 53, 99, 107) | `CHOT:138` | ⚠️ **CURRENT nhưng ĐÃ DE-REFERENCE + DORMANT** · **"KHÔNG sửa" theo D3/§VI** |

Nguyên văn phần lý do của `CHOT:138`:

> *"Tham chiếu governance tới skill này đã được thay bằng `erp-change-control` (mục 7) ⇒ skill không còn được governance trỏ tới; đồng thời nó **bị `.gitignore:78` loại khỏi Git** nên là tài sản **local-only**"*

Và `CHOT:141`:

> *"Adapter cũ còn ngôn ngữ Graphify-first nhưng đã **dormant + de-referenced + untracked**, và **Owner đã chốt không sửa nó**."*

### A6.3 🔴 PHÁT HIỆN QUAN TRỌNG NHẤT — **MỘT TIỀN ĐỀ ĐÃ KHÔNG CÒN ĐÚNG**

Quyết định *"KHÔNG sửa"* của Owner ngày 30/07 dựa trên **ba tiền đề**: `dormant` · `de-referenced` · **`untracked` (local-only, bị `.gitignore` loại)**.

Đo lại tiền đề thứ ba **hôm nay**:

| Phép đo | Đầu ra thật |
|---|---|
| `git check-ignore -v .cursor/skills/speckit-erp-ssot-adapter/SKILL.md` | **KHÔNG bị chặn** |
| `git ls-files -z .cursor/skills/speckit-erp-ssot-adapter` | **1 file — ĐANG BỊ THEO DÕI** |
| `.gitignore` dòng 76–80 | Nội dung là `dm_dia_chi_vn.sql` · `docs/Lịch Sử Trò Chuyện/` · `docs/Tính Giá Offset/` — **không có dòng nào loại kỹ năng này** |
| `git log --diff-filter=A -- <file>` | **`<mã-nguồn-riêng>` · 2026-08-20 19:18** · *"R3/S9+S7: bao ve 2 skill tu viet…"* |
| Số commit trước 31/07 | **0** |

> ### ⚖️ PHÁN ĐỊNH
>
> Ngày 30/07 tiền đề *"untracked, local-only"* **ĐÚNG** (0 commit trước 31/07).
> Ngày 20/08, commit `<mã-nguồn-riêng>` **đưa kỹ năng này vào Git** — **21 ngày SAU** quyết định.
>
> ⇒ Quyết định *"KHÔNG sửa vì nó chỉ là tài sản local"* nay đứng trên **một tiền đề đã đổi**.
> Kỹ năng này giờ **được git theo dõi**, **đã lên remote**, và **có mặt trong registry với nhãn `HEALTHY`** — tức là **mọi phiên tra registry đều thấy nó**.
>
> 🔴 Đây **KHÔNG phải** căn cứ để agent tự sửa. Theo `GOV-OWNER-DECISION-PORTABILITY-001` §F2, quyết định phải **Owner Gate lại** khi *"evidence mới làm thay bản chất"* — và đây đúng là ca đó. **Trình Owner, không tự quyết.**

---

## A7 — BẢNG KÊ PHÁT HIỆN + ĐỀ XUẤT HƯỚNG VÁ *(CHỈ ĐỀ XUẤT)*

### Dòng 1

| Mục | Nội dung |
|---|---|
| **#** | 1 |
| **Vị trí** | `.governance/registry/skills.yml:795` — trường `trap_seen_before` của `dependency-relationship-scan` |
| **Nội dung sai hiện tại** | `"Verify: graph.json tồn tại; rule graphify.mdc alwaysApply; AGENTS.md cấm API routes/fetch."` |
| **Nội dung đúng theo bằng chứng** | Không có dòng bẫy nào **đang sống** trong file ⇒ giá trị đúng là **`NONE_RECORDED`** (chính giá trị mặc định của script, `:316`). Sự thật tại nguồn: `.cursor/rules/graphify.mdc:3` = `alwaysApply: false` |
| **Nguồn gốc** | 🔴 **SCRIPT SINH** — `skills-registry-build.mjs:315-319`. **KHÔNG** phải gõ tay (`git blame` → `<mã-nguồn-riêng>`, commit sinh registry) |
| **Nguyên nhân gốc** | Script **không phân biệt mục**; dòng khớp DUY NHẤT trong 391 dòng nằm ở `## Changelog` › `### v1.1.0` — bản **đã bị v1.2.0 thay thế**. Một dòng LỊCH SỬ bị nâng thành cảnh báo ĐANG SỐNG |
| **Tầng phải vá** | 🔴 **SCRIPT** — bắt buộc. Sửa `skills.yml` bằng tay là **vô nghĩa**: `:532` ghi đè toàn bộ ở lần chạy kế |
| **Rủi ro nếu vá tầng này** | Đổi `TRAP_LINE` / thêm nhận biết mục sẽ **đổi giá trị `trap_seen_before` của nhiều entry khác**, không riêng entry này. **Bắt buộc so trước/sau toàn bộ 128 entry** và khai rõ entry nào đổi |
| **Giữ lịch sử theo §G7.0** | `skills.yml` **không có** mục "Lịch sử sửa đổi" và bị sinh lại hoàn toàn ⇒ **không áp được cách 1**. Cách đúng: ghi vào **khối chú thích của chính script** (nơi bền vững, không bị ghi đè) + một dòng ở sổ nợ, kèm **nguyên văn giá trị cũ** |

### Dòng 2

| Mục | Nội dung |
|---|---|
| **#** | 2 |
| **Vị trí** | `.governance/registry/skills.yml:3642` (`description`) + `:3646` (`health_label`) của `speckit-erp-ssot-adapter` |
| **Nội dung sai hiện tại** | `description` chứa `"Graphify trước"` · `health_label: "HEALTHY"` |
| **Nội dung đúng theo bằng chứng** | `description` **phản ánh trung thực** nguồn `SKILL.md:4` — **script không sai**. `health_label: HEALTHY` **đúng theo định nghĩa cấu trúc** (4/4 điều kiện `:343-350`). Cái **thiếu** là một trục đo *"nội dung còn hiệu lực?"* — trạng thái thật là **DORMANT + DE-REFERENCED** (`CHOT:138`) |
| **Nguồn gốc** | **SCRIPT SINH**, nhưng **lỗi ở NỘI DUNG NGUỒN + THIẾT KẾ REGISTRY**, không phải ở logic script |
| **Tầng phải vá** | ⚠️ **CẢ HAI + CẦN OWNER**: (a) `SKILL.md` — nhưng Owner đã chốt *"KHÔNG sửa"* 30/07, **và tiền đề đã đổi** (A6.3) ⇒ **phải hỏi lại**; (b) **REGISTRY** — thêm trục `hieu_luc_noi_dung` để `HEALTHY` không còn gây hiểu nhầm |
| **Rủi ro nếu vá tầng này** | Sửa `SKILL.md` khi chưa hỏi lại = **đi ngược quyết định Owner đang hiệu lực** (`GOV-OWNER-AUTHORITY-001`). Thêm trục mới vào registry = đổi lược đồ, phải cập nhật cổng `skills-registry` và `ui-skill-conflict` |
| **Giữ lịch sử theo §G7.0** | Nếu Owner cho sửa `SKILL.md`: file **CÓ** mục `## Changelog` ⇒ **áp được cách 1** (thêm entry `v1.0.1`, giữ nguyên văn dòng cũ). Đây là điểm khác căn bản so với dòng 1 |

### Đề xuất hướng vá — trình Owner duyệt

| Ưu tiên | Việc | Vì sao xếp trước |
|---|---|---|
| **1** | **Vá SCRIPT** cho dòng 795: dạy script bỏ qua mục `## Changelog` khi dò `trap_seen_before` | Sai rõ ràng, không cần Owner quyết nội dung, và **là loại lỗi có thể đang ảnh hưởng nhiều entry khác** — phải đo trước/sau cả 128 |
| **2** | **Hỏi Owner về `speckit-erp-ssot-adapter`** — tiền đề *"untracked, local-only"* của quyết định 30/07 **đã không còn đúng** từ 20/08 | Không hỏi mà sửa = trái quyết định đang hiệu lực. Không hỏi mà bỏ qua = để nhãn `HEALTHY` tiếp tục gây hiểu nhầm |
| **3** | **Thêm trục `hieu_luc_noi_dung`** vào registry + cổng canh | Vá gốc loại bệnh, không chỉ 2 dòng. Cùng cách đã dùng ở V2 (`ssot_chuoi_cam`) |
| **4** | **Cổng mới canh "mandate công cụ đã chết"** — đối chiếu giá trị registry với sự thật tại `.cursor/rules/*.mdc` | Hiện **0 cổng** làm việc này (A5.2) |

> ⚠️ **Cảnh báo bắt buộc trước khi làm ưu tiên 1:** thay đổi `TRAP_LINE` sẽ ảnh hưởng **toàn bộ 128 entry**. Phải chạy `--check` (chế độ không ghi) trước, so bảng trước/sau, và khai rõ mọi entry đổi giá trị. Phiên này **KHÔNG chạy** kể cả `--check` — đúng lệnh cấm.

---

## A8 — CÂU HỎI DUY NHẤT TRÌNH OWNER

> **`speckit-erp-ssot-adapter`** — ngày 30/07 anh chốt *"KHÔNG sửa"* vì nó **dormant + de-referenced + untracked (local-only)**.
> Nhưng ngày **20/08** commit `<mã-nguồn-riêng>` đã **đưa nó vào Git**, nay nó **được theo dõi, đã lên remote, và hiện trong registry với nhãn `HEALTHY`**.
> **Tiền đề thứ ba đã đổi. Anh giữ nguyên quyết định "không sửa", hay mở lại?**

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - A0 do moc that: HEAD <mã-nguồn-riêng> (TRUNG mocc bao cao audit da do), skills.yml
     4738 dong, sach, sha256 <mã-nguồn-riêng>
   - A1 xac nhan V2 DA chay: 128/128 entry du 5 truong; 2 dong sai con nguyen
   - A2 truy nguon 2 dong: CA HAI do SCRIPT SINH, khong dong nao go tay
     (git blame -> <mã-nguồn-riêng>, commit sinh registry lan dau)
   - A2 tim ra HAI nguyen nhan goc KHAC NHAU (chi tiet o truong 3)
   - A2.4 quet cum loi: chi 2 dong trong skills.yml la loi dang song;
     14 lan xuat hien khac deu la luu tru/ho so/anh chup — DUNG ban chat
   - A3 ban do 4 script; xac dinh script GHI DE TOAN BO, chay THU CONG
   - A4 trich nguyen van GOV-EDIT-PRESERVE-001 + parity 5 file (1 ma duy nhat)
   - A5 kiem 6 dieu kien cong ui-skill-conflict: KHONG dieu kien nao cham toi
   - A6 tra 2 so (deu 0 dong) + doi chieu quyet dinh chot 30/07
   - A6.3 phat hien MOT TIEN DE CUA QUYET DINH OWNER DA KHONG CON DUNG
   - A7 bang ke 2 dong + de xuat 4 uu tien va

2. PHẠM VI
   ĐỤNG      : DUNG 1 file — docs/reports/PHA-A-NGUON-GOC-2-DONG-SAI-SKILLS-YML-20260823.md
   KHÔNG ĐỤNG: skills.yml · SKILL.md bat ky · 5 file luat · scripts/ · src/ ·
               so Owner · so no · DB · deploy
   KHÔNG CHẠY: skills-registry-build.mjs (ke ca --check) · graphify · bat ky
               script nao co the ghi
   KHÔNG      : commit · push

3. BẰNG CHỨNG
   git rev-parse HEAD -> <mã-nguồn-riêng> = dung moc bao cao audit -> FILE_PROVEN
   wc -l = grep -c '' = 4738 -> FILE_PROVEN
   grep -c '^  - slug:' = 128 ; 5 truong x 128 -> FILE_PROVEN
   grep -n "alwaysApply" -> 795 ; grep -n "Graphify truoc" -> 3642 -> FILE_PROVEN
   SKILL.md dependency-relationship-scan: 391 dong, DUNG 1 dong khop tu khoa
     TRAP la dong 388, nam trong "## Changelog" (376) > "### v1.1.0" (384) -> CODE_PROVEN
   skills-registry-build.mjs:532 fs.writeFileSync(OUT, ...) -> GHI DE -> CODE_PROVEN
   grep -c 'skills-registry-build' package.json -> 0 (chay thu cong) -> FILE_PROVEN
   grep -rn "graphify" scripts/ -> 0 ket qua -> CODE_PROVEN
   git check-ignore speckit-erp-ssot-adapter -> KHONG chan ;
     git ls-files -> 1 file DANG THEO DOI ;
     git log --diff-filter=A -> <mã-nguồn-riêng> (20/08), 0 commit truoc 31/07 -> CODE_PROVEN
   sha256 x5 file luat -> 1 ma duy nhat <mã-nguồn-riêng> -> FILE_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [ ] ĐÃ GHI
   [x] CHƯA — ly do: phien nay CAM ghi so (FORBIDDEN_ACTIONS neu ro
       "CAM ghi so Owner/so no — chi bao ra bao cao"). Noi dung can ghi
       da neu day du o A6.3 + A8 de phien sau ghi.

5. PUSH BÁO CÁO CÔNG KHAI
   [ ] ĐÃ PUSH
   [x] CHƯA PUSH — ly do: phien nay CAM commit/push. Ban tom tat public-safe
       da soan san (muc rieng ben duoi), CHI push khi Owner duyet.

6. CÒN SÓT / CHƯA LÀM
   - CHUA va gi — dung pham vi PHA A (dieu tra, khong sua)
   - CHUA do anh huong cua viec doi TRAP_LINE len 127 entry con lai
     (can chay --check, ma phien nay bi cam chay)
   - CHUA kiem lieu con truong NAO KHAC ngoai trap_seen_before cung bi
     hoist tu muc Changelog (vd symptom/negative_trigger) — ngoai A0-A8

7. ĐANG CHỜ OWNER
   - Cau duy nhat o muc A8: giu hay mo lai quyet dinh 30/07 ve
     speckit-erp-ssot-adapter, khi tien de "untracked" da doi tu 20/08
   - Duyet 1 trong 4 uu tien va o A7
   - Chan: khong tra loi thi khong duoc dong toi SKILL.md do (trai quyet
     dinh dang hieu luc), va nhan HEALTHY tiep tuc gay hieu nham

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Chay `node scripts/tests/skills-registry-build.mjs --check` (che do KHONG
   ghi) de do xem sua TRAP_LINE se lam doi gia tri cua bao nhieu entry.

9. CHƯA XÁC MINH ĐƯỢC
   - Vi sao commit <mã-nguồn-riêng> (20/08) dua speckit-erp-ssot-adapter vao Git trong
     khi chot 30/07 xep no la local-only. Doc thong diep commit khong du ket
     luan la co y hay vo tinh. Ai xac minh: Owner, hoac phien da chay <mã-nguồn-riêng>
   - Cac truong khac ngoai trap_seen_before co bi hoist tu Changelog khong —
     can chay --check moi do duoc, phien nay bi cam

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — dieu tra A0-A8 hoan tat, moi khang dinh co lenh do kem
       dau ra that. Thieu: quyet dinh Owner cho A8 + phep do --check o truong 9.
       Dieu kien len PASS: Owner tra loi cau A8 va duyet huong va.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phien co bi nen: KHONG (phien nay bat dau moi, khong bi nen)
   Da doc trong phien (deu doc TRUC TIEP tu dia, khong dung tri nho):
   .governance/registry/skills.yml (4738 dong — doc vung 785-806, 3632-3656,
   3653-3656, meta 24-25) · scripts/tests/skills-registry-build.mjs (535 dong)
   · scripts/tests/ui-skill-conflict-gate.test.mjs · scripts/tests/
   skills-registry-gate.test.mjs · .cursor/skills/dependency-relationship-scan/
   SKILL.md (391 dong) · .cursor/skills/speckit-erp-ssot-adapter/SKILL.md
   · .cursor/rules/graphify.mdc · CLAUDE.md §G7.0 (dong 741-771) · .gitignore
   · docs/BAO-CAO-CHOT-AI-TOOLING-GOVERNANCE-2026-07-30.md (dong 98-141)
   · docs/BAO-CAO-AI-TOOLING-GOVERNANCE-INDEPENDENT-REVIEW-2026-07-30.md
   · docs/OWNER-REQUEST-LEDGER.md · .governance/registry/tech-debt.md
   · package.json
═══════════════════════════════════════════
```

---

## PHỤ LỤC — BẢN TÓM TẮT PUBLIC-SAFE *(soạn sẵn, CHỈ push khi Owner duyệt)*

> Theo `GOV-PUBLIC-SAFE-001` §J1: được nêu tên file · tên trường · tên registry · số lượng kỹ thuật · mã commit công khai. Không có credential / PII / dữ liệu tiền / định danh hạ tầng trong nội dung dưới đây.

### Điều tra nguồn gốc 2 dòng sai trong danh mục kỹ năng — 23/08/2026

Danh mục kỹ năng `.governance/registry/skills.yml` (**128 mục**) có 2 dòng ghi sai so với sự thật tại nguồn. Điều tra chỉ-đọc tại mốc `<mã-nguồn-riêng>` kết luận:

**Cả 2 dòng đều do script sinh ra, không dòng nào gõ tay** — nhưng **hai nguyên nhân gốc khác hẳn nhau**:

| Dòng | Nguyên nhân | Tầng phải vá |
|---|---|---|
| `:795` | Script **không phân biệt mục**: file nguồn 391 dòng chỉ có **đúng 1 dòng** khớp từ khoá cảnh báo, và dòng đó nằm trong **mục Changelog của một bản đã bị thay thế**. Một dòng lịch sử bị nâng thành cảnh báo đang sống | **Script** — sửa tay danh mục là vô nghĩa vì script **ghi đè toàn bộ** mỗi lần chạy |
| `:3642` | Script chép trung thực mô tả tự khai của kỹ năng. Nhãn sức khoẻ ghi "HEALTHY" **đúng theo định nghĩa cấu trúc** nhưng **không đo nội dung còn hiệu lực** | **Nội dung nguồn + thiết kế danh mục** — cần Owner quyết |

**Ba phát hiện kèm theo:**

1. **Không cổng nào canh loại lỗi này.** `grep` trên toàn bộ thư mục cổng trả về **0 kết quả** cho tên công cụ liên quan.
2. **Cụm lỗi KHÔNG lan rộng.** 14 lần xuất hiện khác đều nằm ở lưu trữ / hồ sơ / ảnh chụp — đúng bản chất, phải giữ. **5 file luật sạch hoàn toàn.**
3. 🔴 **Một tiền đề của quyết định 30/07 đã không còn đúng.** Quyết định *"không sửa"* dựa trên việc kỹ năng đó là tài sản local, không nằm trong kho mã. Đo lại: nó **đã vào kho mã ngày 20/08**, tức **21 ngày sau** quyết định. Theo luật về tính chuyển tiếp của quyết định Owner, đây là ca **phải hỏi lại**, không được tự suy rộng.

**Phiên này không sửa gì.** Đã lập bảng kê 2 dòng kèm 4 hướng vá xếp theo ưu tiên, trình Owner duyệt.
