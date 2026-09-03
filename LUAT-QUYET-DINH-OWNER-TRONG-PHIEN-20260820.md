# LUẬT QUYẾT ĐỊNH OWNER TRONG PHIÊN — `GOV-SESSION-DECISION-001` (§F1b)

> **Loại:** CHỈ XỬ LÝ LUẬT · **Ngày:** 20/08/2026 · **Owner:** TanPhatERP
> **Actor:** Agent IDE · **Nhánh:** `gov/2026-08-18-rules-ui-standard-upgrade` · **Gốc phiên:** `<mã-nguồn-riêng>` · **HEAD:** `<mã-nguồn-riêng>`
> **Doc Version:** `2.4` → `2.5` · **Parity 5 file:** `<mã-nguồn-riêng>…`
> **KHÔNG:** deploy · DB/schema · `src/` · nội dung SSOT · Notion · đẩy kho mã riêng tư · nạp dữ liệu thật · gỡ PII/đổi khoá

---

## 0) BỐN ĐIỀU ĐÁNG CHÚ Ý

1. **Luật tự chứng minh ngay trong phiên ban hành.** `GOV-SESSION-DECISION-001` mục 3 nói *ghi sổ là hành động mặc định được phép*. Tôi dùng đúng quyền đó để ghi **mục #89** vào Sổ Yêu Cầu Owner — **không chờ prompt cấp quyền**. Mục #89 chính là bằng chứng luật đã thi hành, và nó **đóng `DEBT-008` bằng cách vá GỐC**, không phải vá triệu chứng.
2. **Hai mã nợ mà đề bài chỉ định đã bị dùng.** `DEBT-016`/`DEBT-017` nay thuộc về phiên khác. Quan trọng hơn: nội dung mà đề bài định ghi thành `DEBT-016` (*97/128 skill thiếu `negative_trigger`*) **đã có sẵn là `DEBT-023`** — nên tôi **không ghi trùng**, chỉ trỏ tới nó. Hai nợ thật sự mới được cấp `DEBT-030`/`DEBT-031`.
3. **Phát hiện mới ngoài kịch bản — `DEBT-031`.** Trong lúc kiểm xem "cổng trước commit" có thật không, đo được: `.git/hooks/pre-commit` **chỉ kiểm `src/lib/version.ts`**; commit không đụng file đó thì hook `exit 0` ngay. Nghĩa là `GOV-SECRET-IN-CODE-001` (§G7.5) khai `ENFORCEMENT: AUTO — cổng quét trước commit` nhưng **thực tế không cổng quản trị nào chạy khi commit**. Đây đúng loại vấn đề `GOV-GATE-REAL-INPUT-001` sinh ra để chặn.
4. **Có phiên khác đang làm dở trên cùng nhánh.** Hai file sổ mang thêm `DEBT-025…029` + OIL `#88` của một phiên audit FK. Tôi **giữ nguyên vẹn**, chỉ commit kèm để không mất, và nói rõ không phải việc của phiên này.

---

## 1) QUÉT CÙNG CHỦ ĐỀ (Step 0 — `GOV-EDIT-PRESERVE-001` yêu cầu 2)

| Chủ đề | Vị trí tìm thấy | Xử lý |
|---|---|---|
| "Sổ Yêu Cầu Owner" | §G6 · §G7.3 `REFERENCE` · §H3b | **Cố ý giữ** — cả ba nhất quán với luật mới |
| `OWNER-REQUEST-LEDGER` | header · §F1 · §G7.3 · §G7.4 | §F1 là nơi chèn §F1b ngay sau; ba chỗ còn lại **giữ nguyên** |
| `IDE_LEAD` | §F3 · §F4 · §S nguyên tắc 13 | **Giữ nguyên** — luật mới dựa vào chúng, không sửa |
| `SYNC_OVERDUE` | §F3 · §Q bảng legacy | **Giữ nguyên** — luật mới trỏ tới §F3 |
| "ghi sổ" | §G7.10 (2 chỗ) | **Giữ nguyên** — cùng hướng, đã tham chiếu chéo trong `REFERENCE` của luật mới |

**Kết luận quét:** luật mới **không đè lên** điều nào. `GOV-OWNER-REQUEST-LEDGER-001` (§F1) giữ nguyên — F1 định nghĩa **SỔ**, F1b định nghĩa **THỜI ĐIỂM ghi · QUYỀN mặc định ghi · NGHĨA VỤ đọc trước khi nhận định**.

---

## 2) BA SỬA — MỘT LƯỢT

| # | Vị trí | Nội dung |
|---|---|---|
| **A** | **§F1b** (mục MỚI, giữa §F1 và §F2) | Toàn bộ `GOV-SESSION-DECISION-001`, 14/14 trường, `SCOPE = RIÊNG — ERP`, `STATUS = ACTIVE (Owner duyệt 20/08/2026)`, `REVIEW = 90 ngày` |
| **C** | §G5 khối báo cáo kết thúc — trường 4 | Thêm: *ghi sổ là hành động MẶC ĐỊNH ĐƯỢC PHÉP — trường này KHÔNG được ghi "CHƯA" với lý do "prompt không nêu / không có trong phạm vi"* |
| **B** | §V chỉ mục | Thêm dòng: **trước khi nhận định code ↔ tài liệu lệch nhau** → tra Sổ Yêu Cầu Owner + sổ nợ; lệch khớp quyết định đã ghi = `SYNC_OVERDUE`, KHÔNG phải lỗi |

Kèm: Doc Version `2.4` → `2.5`; §W thêm **1 hàng chính + 4 hàng con** (mỗi sửa một hàng, dòng cũ nguyên văn).

### Ba khe hở mà luật này bịt

| # | Khe hở | Hệ quả thật | Điều bịt |
|---|---|---|---|
| 1 | Không luật bắt ĐỌC sổ trước khi kết luận "code lệch tài liệu = sai" | Phiên sau có thể **sửa ngược code đúng** | mục 5 + `FORBIDDEN` + dòng §V |
| 2 | §F1 không quy định THỜI ĐIỂM ghi → dồn cuối phiên | Phiên nén/gián đoạn → **mất quyết định** trong khi code đã đi trước | mục 1 + mục 2 |
| 3 | Ghi sổ bị coi là việc cần prompt cấp quyền | `DEBT-008`: **3 phiên liên tiếp** không ghi được sổ | mục 3 + §G5 trường 4 |

---

## 3) GHI SỔ — DÙNG NGAY LUẬT VỪA BAN HÀNH

### Sổ Yêu Cầu Owner — mục **#89**

Ghi bằng **quyền mặc định** của §F1b mục 3, không chờ cấp quyền. Nguyên văn Owner được trích đầy đủ; `timestamp` 20/08/2026 20:18; `type` DECISION; `decision_state` OWNER_CONFIRMED; `notion_sync_state` SYNC_DUE (→ `DEBT-010`).

> Mục #89 **vừa là hồ sơ quyết định, vừa là bằng chứng luật đã thi hành** — nếu tôi vẫn ghi "CHƯA — prompt không nêu" như 3 phiên trước thì luật mới coi như chết ngay khi ban hành.

### Sổ nợ kỹ thuật — 29 → 31 nợ, đóng 1

| Mã | Nội dung | Trạng thái |
|---|---|---|
| **DEBT-008** | Sổ Yêu Cầu Owner không ghi được | ✅ **ĐÃ XỬ LÝ 20/08/2026** — vá GỐC bằng §F1b mục 3; bằng chứng: mục #89 |
| **DEBT-030** | `skills.yml` là STATE biến động nhưng chưa có quy ước chạy lại bộ sinh khi thêm/xoá/đổi skill. Cổng `test:skills-registry` **có thật** và nằm trong `test:gov-gates`, nhưng **chỉ chạy khi ai đó gõ lệnh** | MỞ |
| **DEBT-031** | 🔴 Luật khai `ENFORCEMENT: AUTO — cổng quét trước commit` nhưng **không cổng quản trị nào chạy khi commit** (xem §4) | MỞ |

### ⚠️ Một chỉ định của đề bài KHÔNG thi hành được như viết — và vì sao

Đề bài yêu cầu ghi `DEBT-016` + `DEBT-017`. Thực tế:

| Đề bài | Thực tế | Xử lý |
|---|---|---|
| `DEBT-016` = 97/128 skill thiếu `negative_trigger` | Mã **DEBT-016 đã bị dùng** (đổi mật khẩu R2) **và** nội dung đó **đã có sẵn là `DEBT-023`** | **Không ghi trùng** — trỏ tới `DEBT-023` |
| `DEBT-017` = quy ước chạy lại bộ sinh skills.yml | Mã **DEBT-017 đã bị dùng** (script nhận mật khẩu qua tham số dòng lệnh). Nội dung thì **chưa ai ghi** | Cấp mã mới **`DEBT-030`** |

Quy tắc của chính sổ nợ: *"Mã không tái sử dụng. `DEBT-xxx` cấp tăng dần, kể cả khi dòng cũ đã đóng."* Ghi đè `DEBT-016`/`017` sẽ **xoá mất hai nợ đang MỞ** của phiên khác.

---

## 4) 🔴 PHÁT HIỆN NGOÀI KỊCH BẢN — `DEBT-031`

Trong lúc kiểm xem `DEBT-017` có thật là khoảng trống không, tôi đọc `.git/hooks/pre-commit`:

```sh
STAGED_VERSION=$(git diff --cached --name-only | grep -c '^src/lib/version\.ts$')
if [ "$STAGED_VERSION" -eq 0 ]; then
  # Khong dung version.ts -> commit thuong hoac chi tai lieu. Cho qua.
  exit 0
fi
```

`grep -c 'gov-gates\|skills-registry' .git/hooks/pre-commit` → **0**.

| Luật | Khai gì | Thực tế |
|---|---|---|
| `GOV-SECRET-IN-CODE-001` §G7.5 | `ENFORCEMENT: AUTO — cổng quét trước commit` | ❌ Không chạy khi commit |
| `GOV-PII-HANDLING-001` §G7.13 | `ENFORCEMENT: AUTO (quét kho) + MANUAL` | ❌ Không chạy khi commit |
| `GOV-EDIT-PRESERVE-001` §G7.0 | `ENFORCEMENT: AUTO (cổng đếm) + MANUAL` | ❌ Không chạy khi commit |

Các cổng **tồn tại thật và chạy đúng** khi gõ lệnh — nhưng chữ "trước commit" trong luật **không có gì thi hành**. Đây chính xác là loại vấn đề `GOV-GATE-REAL-INPUT-001` (§G7.7) sinh ra để chặn: khai AUTO mà không có thi hành.

**Không tự sửa trong phiên này** — nối hook là việc tooling, ngoài phạm vi "chỉ xử lý luật". Hai lối ra, Owner chọn:
- **(a)** nối `test:gov-gates` vào `scripts/pre-commit-hook.sh` → luật đúng như đang khai; hoặc
- **(b)** khai lại `ENFORCEMENT = MANUAL` + Owner duyệt chấp nhận rủi ro (đúng lối thoát mà §G7.7 `FAILURE` cho phép).

---

## 5) KẾT QUẢ 7 CỔNG

| # | Cổng | Kết quả | Số đo |
|---|---|---|---|
| **T1** | Parity 5 file | ✅ **PASS** | 1 hash `<mã-nguồn-riêng>`, 92538 byte mỗi file; `check:governance` 5/5 OK |
| **T2** | Cổng đếm L0 | ✅ **PASS** | 386 → **388**; ĐK1a ✅ ĐK1b ✅ ĐK2 ✅ parity ✅; **đúng 1 mã luật mới** |
| **T3** | Cổng phụ chuẩn | ✅ **PASS** | 22 mục · 35 heading; ĐK A ✅ ĐK B ✅ |
| **T4** | `ref-exists` (L6) | ✅ **PASS** | **53 đạt** / 0 hỏng / 5 trích dẫn ca hỏng *(tăng từ 42 — luật mới thêm tham chiếu, đều tồn tại thật)* |
| **T5** | Schema 14 trường | ✅ **PASS** | **14/14 luật × 14/14 trường** + SCOPE + STATUS |
| **T6** | `skills-registry` | ✅ **PASS** | 4/4 điều kiện (a·b·c·d) |
| **T7** | Khối báo cáo kết thúc | ✅ **PASS** | 11/11 trường; **trường 4 = ĐÃ GHI — mục #89** |

**7/7 PASS.**

> `test:pii-scan` vẫn báo **2 vi phạm CŨ** (`DEBT-013`, hạn 26/08/2026) — **không phải lỗi phiên này**, và phiên này không được phép gỡ PII.

---

## 6) BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Ban hành §F1b `GOV-SESSION-DECISION-001` (ACTIVE, Owner duyệt 20/08, mục #89):
     ghi sổ NGAY · ghi sổ là quyền MẶC ĐỊNH · code được đi trước tài liệu
     (lệch = SYNC_OVERDUE) · PHẢI đọc sổ trước khi kết luận "code lệch = sai".
   - §G5 trường 4: cấm ghi "CHƯA" với lý do "prompt không nêu".
   - §V: thêm dòng tra sổ trước khi nhận định code ↔ tài liệu lệch nhau.
   - Doc Version 2.4 → 2.5; §W 1 hàng chính + 4 hàng con; nhân bản 5 file byte-identical.
   - GHI SỔ YÊU CẦU OWNER MỤC #89 bằng quyền mặc định của chính luật vừa ban hành.
   - Đóng DEBT-008 (vá GỐC, không vá triệu chứng); thêm DEBT-030 + DEBT-031.
   - Phát hiện ngoài kịch bản: pre-commit hook KHÔNG chạy cổng quản trị nào → DEBT-031.

2. PHẠM VI
   ĐỤNG    : 5 file quản trị (§F1b mới · §G5 trường 4 · §V · §W · header) ·
             docs/OWNER-REQUEST-LEDGER.md (mục #89) ·
             .governance/registry/tech-debt.md (đóng DEBT-008, thêm 030/031)
   KHÔNG ĐỤNG: src/? KHÔNG · DB/schema? KHÔNG · deploy? KHÔNG · nội dung SSOT? KHÔNG
             · Notion? KHÔNG · kho mã riêng tư? CHƯA đẩy · PII/đổi khoá? KHÔNG
             · .git/hooks? KHÔNG sửa (chỉ ĐỌC để chẩn DEBT-031)

3. BẰNG CHỨNG
   sha256sum x5 → 1 hash <mã-nguồn-riêng>, 92538 byte                        → FILE_PROVEN
   npm run check:governance → 5/5 OK                                 → RUNTIME_PROVEN
   npm run test:clause-count → 386 → 388; ĐK1a+1b+2+parity PASS;
     đúng 1 mã luật mới GOV-SESSION-DECISION-001                      → RUNTIME_PROVEN
   npm run test:standard-clause-count → 22 mục · 35 heading, A+B PASS → RUNTIME_PROVEN
   npm run test:ref-exists-gate → 53 đạt / 0 hỏng (tăng từ 42)        → RUNTIME_PROVEN
   npm run test:skills-registry → 4/4 điều kiện PASS                  → RUNTIME_PROVEN
   kiểm schema → 14/14 luật × 14/14 trường + SCOPE + STATUS           → FILE_PROVEN
   grep -c 'gov-gates|skills-registry' .git/hooks/pre-commit → 0      → CODE_PROVEN
   đọc .git/hooks/pre-commit → commit thường `exit 0` ngay            → CODE_PROVEN
   grep DEBT trong sổ → 31 nợ; DEBT-008 = ĐÃ XỬ LÝ; 030/031 đủ 8 cột  → FILE_PROVEN
   grep '^| 89 |' docs/OWNER-REQUEST-LEDGER.md → mục #89 tồn tại      → FILE_PROVEN
   ⚠️ CHƯA có UI_PROVEN / DB_PROVEN — phiên chỉ xử lý LUẬT, không đụng giao diện
      và không đụng CSDL nên không có gì để chụp ảnh hay truy vấn.

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #89
   [ ] CHƯA
   Ghi bằng quyền mặc định của GOV-SESSION-DECISION-001 §F1b mục 3 — chính luật
   vừa ban hành trong phiên này. Đây là lần đầu trường 4 ghi "ĐÃ GHI" sau 3 phiên
   liên tiếp ghi "CHƯA" (DEBT-008, nay ĐÃ XỬ LÝ).

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng>
       · file LUAT-QUYET-DINH-OWNER-TRONG-PHIEN-20260820.md
   Kỷ luật công-bố-an-toàn: KHÔNG nêu đường dẫn chính xác file chứa PII, KHÔNG nêu
   giá trị nhạy cảm, KHÔNG nêu địa chỉ máy chủ.

6. CÒN SÓT / CHƯA LÀM
   - DEBT-031 (pre-commit không chạy cổng nào) — CHƯA sửa, ngoài phạm vi phiên luật.
   - DEBT-030 (quy ước chạy lại bộ sinh skills.yml) — CHƯA làm, phiên skill.
   - DEBT-013 (PII 70 email) + DEBT-005 (đổi khoá) — hạn 26/08/2026, phiên bảo mật.
   - DEBT-023 (97/128 skill thiếu negative_trigger) — vẫn MỞ, không ghi trùng.
   - DEBT-010 (đồng bộ Notion) — mục #89 đang SYNC_DUE.
   - Chưa đẩy kho mã riêng tư (13 commit chờ Owner).
   - Chưa rà xem còn luật nào khác khai AUTO-trước-commit ngoài 3 luật đã liệt kê ở §4.
   ⚠️ Đã rà lại thật; mọi mục đều có mã DEBT-xxx, trừ mục cuối — xem trường 9.

7. ĐANG CHỜ OWNER
   - DEBT-031: chọn (a) nối test:gov-gates vào pre-commit, hay (b) khai lại
     ENFORCEMENT = MANUAL + duyệt chấp nhận rủi ro → chặn việc khép §G7.5/§G7.13.
   - DEBT-013 + DEBT-005: hạn 26/08/2026 (còn 6 ngày) → cần mở phiên bảo mật.
   - Đẩy kho mã riêng tư? y/n → chặn đưa luật 2.5 lên remote.
   - Xác nhận cách xử lý mã nợ: tôi KHÔNG dùng DEBT-016/017 như prompt nêu (đã bị
     dùng) mà cấp DEBT-030/031 — Owner đồng ý cách này chứ?

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner mở phiên bảo mật xử lý DEBT-013 + DEBT-005 — hạn 26/08/2026, còn 6 ngày,
   là hai nợ duy nhất trong sổ có hạn cứng.

9. CHƯA XÁC MINH ĐƯỢC
   - Còn luật nào khác khai AUTO-trước-commit ngoài 3 luật ở §4 — mới rà §G7,
     chưa rà toàn bộ 14 luật + phần A–V. Ai: phiên luật kế tiếp.
   - Phiên audit FK (DEBT-025..029, OIL #88) có định commit riêng không — tôi commit
     kèm để không mất, nhưng không biết ý định của phiên đó. Ai: Owner.
   - docs/AUDIT-FK-NGUOI-VA-CUSTOMER-OWNERSHIP-2026-08-20.md vẫn chưa được git theo
     dõi, không phải của phiên này; tôi KHÔNG đụng. Ai: Owner.

10. TRẠNG THÁI CHUNG
   [x] PASS — 7/7 cổng đạt; luật mới đã ban hành VÀ đã tự thi hành trong chính phiên
       (mục #89); DEBT-008 đóng bằng cách vá gốc; parity 5/5; không điều nào bị xoá
       (388 ≥ 386, đúng 1 mã luật mới).
       Các nợ còn MỞ đều đã ghi sổ có mã và không chặn phần LUẬT của phiên này.
   [ ] PROVISIONAL   [ ] BLOCKED

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: KHÔNG.
   Dù không nén, mọi tham chiếu đều ĐỌC TRỰC TIẾP TỪ ĐĨA trong phiên này:
     · CLAUDE.md — quét 5 chủ đề (Step 0); đọc §F1 (391–420), §F2, §F3, §F4,
       §G5 trường 4, §V, §W trước khi sửa
     · docs/OWNER-REQUEST-LEDGER.md — đọc cấu trúc + mục #88 trước khi chèn #89
     · .governance/registry/tech-debt.md — đọc TOÀN BỘ dải mã (DEBT-001…029) +
       đọc đầy đủ DEBT-022/023/024 để KHÔNG ghi trùng
     · .git/hooks/pre-commit — đọc toàn phần (chẩn DEBT-031)
     · package.json (mục scripts)
   KHÔNG đọc docs/UI-STANDARD.md — phiên chỉ xử lý LUẬT, không đụng giao diện nên
   GOV-READ-STANDARD-001 không kích hoạt (TRIGGER là "đụng giao diện").
═══════════════════════════════════════════
```
