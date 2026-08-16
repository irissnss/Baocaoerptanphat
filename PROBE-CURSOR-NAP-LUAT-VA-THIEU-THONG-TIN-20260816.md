# Báo cáo probe — Cursor nạp luật nào + khi thiếu thông tin phải làm gì

> **Tầng thông tin:** LOCAL (file luật trên đĩa + luật Cursor tiêm vào phiên này)  
> **Loại:** Báo cáo công khai / đối chiếu luật — **CHỈ TÀI LIỆU**, không đụng mã nguồn ERP, không đụng CSDL, không triển khai, không đổi số phiên bản.  
> **Ngày:** 16/08/2026  
> **Phiên:** Owner hỏi 2 câu probe + yêu cầu báo cáo đầy đủ + đẩy GitHub report + chữ ký Agent Cursor.

---

## Chữ ký Agent (để Owner nhận đúng người viết)

```
══════════════════════════════════════
  CHỮ KÝ AGENT — KHÔNG GIẢ MẠO
══════════════════════════════════════
  Vai trò     : Agent IDE (Cursor)
  Công cụ     : Cursor
  Model       : Cursor Grok 4.6
  Lane        : execution / IDE
  Actor       : Agent IDE — KHÔNG phải Agent Notion, KHÔNG phải Coordinator
  Ngày giờ    : 16/08/2026 (phiên tối)
  Phạm vi     : trả lời 2 câu probe + ghi sổ + đẩy báo cáo công khai
══════════════════════════════════════
```

Nếu thấy chữ ký **Agent IDE (Cursor) / Cursor Grok 4.6** trên báo cáo này → đó là bản của phiên Cursor này.

---

## 🎯 Yêu cầu Owner phiên này (nguyên văn ý định)

1. Bạn đang tự nạp file luật nào của repo này — nêu tên file?
2. Khi thiếu thông tin bạn phải làm gì?
3. Xem và trả lời đầy đủ chi tiết.
4. Nếu được, đẩy lên GitHub report.
5. Để lại chữ ký là Agent Cursor để Owner nhận ra.

**Phân loại đề xuất (IDE):** `CLARIFICATION` / probe xác nhận nạp luật — **không** đổi luật, **không** đổi mã.

---

## Câu 1 — File luật Cursor đang tự nạp (tên file)

### Trả lời ngắn (đúng luật hiện hành)

- **Canonical không phải một file “thắng” bốn file còn lại.** Canonical là bộ luật logic `TANPHAT_AGENT_RULESET`.
- **Năm file là 5 bản sao ngang hàng, giống hệt từng byte:**
  - `.cursorrules`
  - `.antigravityrules`
  - `AGENTS.md`
  - `CLAUDE.md`
  - `GEMINI.md`
- **File entry Cursor tự nạp cho phiên này:** `.cursorrules`
- **Luật:** `GOV-FIVE-REPLICA-SYNC-001` — Agent đọc **một replica + kiểm parity**; không đọc lặp cả 5 bản giống nhau vào ngữ cảnh.

### Bằng chứng phiên này (không giả định)

**A. Cursor tiêm vào phiên (quan sát trực tiếp trong context):**

| File Cursor tiêm vào phiên này | Vai trò | Có phải 1 trong 5 replica? |
|---|---|---|
| `.cursorrules` | **Entry / workspace rule chính** — bộ luật VNext đầy đủ | Có |
| `CLAUDE.md` | Cursor tiêm thêm (cùng nội dung replica) | Có — trùng nội dung |
| `AGENTS.md` | Cursor tiêm thêm (cùng nội dung replica) | Có — trùng nội dung |
| `.cursor/rules/deploy-schema-compatibility.mdc` | Luật bổ trợ deploy/schema | **Không** — chỉ bổ trợ |

**B. Không được Cursor tiêm vào phiên này (em chỉ đo parity trên đĩa, không đọc lại nội dung):**

- `.antigravityrules`
- `GEMINI.md`

**C. Parity trên đĩa (đo thật 16/08/2026, phiên này):**

| File | SHA-256 | Số dòng (đếm dòng có nội dung) | Kết luận |
|---|---|---|---|
| `.cursorrules` | `c009a4d7378dfa8f53ee45329e3c95a976054a4f165d1394eac1dd6f3185b431` | 1128 | Khớp 5 file |
| `.antigravityrules` | `c009a4d7378dfa8f53ee45329e3c95a976054a4f165d1394eac1dd6f3185b431` | 1128 | Khớp 5 file |
| `AGENTS.md` | `c009a4d7378dfa8f53ee45329e3c95a976054a4f165d1394eac1dd6f3185b431` | 1128 | Khớp 5 file |
| `CLAUDE.md` | `c009a4d7378dfa8f53ee45329e3c95a976054a4f165d1394eac1dd6f3185b431` | 1128 | Khớp 5 file |
| `GEMINI.md` | `c009a4d7378dfa8f53ee45329e3c95a976054a4f165d1394eac1dd6f3185b431` | 1128 | Khớp 5 file |

**`RULESET_PARITY = PASS`** — một mã băm duy nhất. Khớp sổ Owner mục #51 (`c009a4d7…`).

### Cách hiểu đúng (tránh nhầm)

1. **Không** nói “`.cursorrules` là file chủ, bốn file kia phụ thuộc.” Mô hình cũ đó đã **SUPERSEDED** (16/08/2026).
2. **Có** nói: Cursor **tự nạp `.cursorrules`** làm entry; nội dung = cùng một bộ luật với 4 replica còn lại.
3. File `.cursor/rules/*.mdc` **chỉ bổ trợ** — không thay 5 replica, không phải “file luật thứ 6.”
4. Sổ Owner #52 đã ghi: Cursor **có đọc** `.cursorrules` (bằng chứng empirical #49). Phiên này **xác nhận lại bằng quan sát tiêm context + đo hash đĩa**.

---

## Câu 2 — Khi thiếu thông tin, phải làm gì?

### Trả lời theo luật hiện hành (VNext) — `GOV-NO-ASSUMPTION-001` (§D1)

**Thiếu thông tin không còn mặc định = “hỏi ngay rồi dừng toàn bộ.”**  
Cách cũ “thiếu info → hỏi và DỪNG mọi tình huống” đã bị **bảng phân xử luật lịch sử** thay bằng quy trình dưới đây.

### Trình tự bắt buộc (5 bước, theo thứ tự)

1. **Kiểm dữ liệu Owner đã cung cấp** trong câu hỏi / phiên.
2. **Kiểm file / mã nguồn / CSDL / SSOT** mà Agent **được quyền đọc**.
3. **Kiểm Quyết định Owner / Sổ Yêu Cầu Owner** (`docs/OWNER-REQUEST-LEDGER.md`).
4. **Kiểm công cụ / tài liệu phù hợp** (registry công cụ, runbook, skill đúng trigger).
5. **Nếu vẫn thiếu:**
   - **`PROVISIONAL`** — được phân tích **chỉ-đọc** nếu còn làm được an toàn. Phải ghi: thiếu gì · claim nào còn hợp lệ · claim nào chưa · điều kiện lên PASS.
   - **`BLOCKED`** — **cấm mutation** nếu thông tin thiếu làm thay kết quả.
   - **Hỏi Owner đúng một câu tối thiểu** khi không tự resolve được.

### Cấm khi thiếu thông tin

- Đoán schema / tự bịa cột-bảng.
- Bịa đường dẫn.
- Tự hoàn thiện business rule.
- Lấy mốc lịch sử / checkpoint cũ coi như **Current Truth**.

### Ba trạng thái cổng (phải ghi rõ)

| Cổng | Nghĩa | Được làm gì |
|---|---|---|
| `PASS` | Đủ quyền + đủ bằng chứng | Mutation trong phạm vi đã khai |
| `PROVISIONAL` | Chưa đủ Current Truth nhưng đọc vẫn an toàn | Chỉ phân tích / báo cáo; không sửa phần thiếu |
| `BLOCKED` | Thiếu điều kiện thiết yếu / xung đột / không được phép | Dừng mutation; báo Owner |

### Áp vào chính phiên này (minh họa)

- Owner hỏi 2 câu + yêu cầu báo cáo + đẩy GitHub report → **đủ** để trả lời và ghi báo cáo.
- Không thiếu schema / không thiếu quyết định nghiệp vụ → **không BLOCKED**.
- Mutation chỉ ở repo báo cáo công khai + sổ Owner → **PASS** cho phạm vi tài liệu.

---

## Đối chiếu với lần đo trước (không mâu thuẫn)

| Mốc | Việc | Kết luận |
|---|---|---|
| OIL #49 | Owner hỏi Cursor 3 câu an toàn | Cursor trả lời đúng 3/3 |
| OIL #52 | Đo Cursor có đọc `.cursorrules` không | VERIFIED — có đọc |
| **Phiên này** | Hỏi tên file đang nạp + quy trình thiếu info | **`.cursorrules`** (entry) = 1 replica của `TANPHAT_AGENT_RULESET`; thiếu info → discover → PROVISIONAL / BLOCKED → hỏi 1 câu |

Hai luật khép phiên (`GOV-COMPLETION-REPORT-001`, `GOV-DONE-DEFINITION-001`) nằm **inline trong cùng 5 file** → Cursor nạp `.cursorrules` là nạp luôn 2 luật đó. **Không tạo file thứ 6.**

---

## Phạm vi phiên này

**ĐỤNG**
- Repo báo cáo công khai: file báo cáo này · `README.md` (thêm mục changelog) · `GOVERNANCE-LOG.md` (ghi nhận probe)
- Repo mã nguồn (nội bộ): `docs/OWNER-REQUEST-LEDGER.md` mục #53

**KHÔNG ĐỤNG**
- `src/` · migration · CSDL · deploy · số phiên bản ERP · 5 file luật (chỉ đọc + hash)

---

## Yêu cầu Owner phiên này — để Agent Notion đối chiếu (không hỏi lại nếu cùng scope)

1. Probe Cursor: file luật tự nạp = **`.cursorrules`** (entry), canonical = **`TANPHAT_AGENT_RULESET`** (5 replica byte-identical).
2. Probe Cursor: thiếu thông tin = **discover trước** → `PROVISIONAL` (đọc) / `BLOCKED` (mutation) → hỏi **một câu tối thiểu**. Không đoán schema/path/business rule.
3. Chữ ký phiên này: **Agent IDE (Cursor) / Cursor Grok 4.6**.
4. Báo cáo này là bridge công khai; **không** tự nhận đã sửa Notion.

---

## Chữ ký đóng báo cáo

**Agent IDE (Cursor)** · model **Cursor Grok 4.6** · 16/08/2026  
Đã trả lời 2 câu probe theo luật đang chạy trên đĩa (parity PASS, sha256 `c009a4d7…`).
