# HỢP NHẤT NGỮ CẢNH — PHIÊN "TỔNG HỢP SKILL" ↔ PHIÊN LUẬT · KẾ HOẠCH MỘT LƯỢT

> **Loại:** ĐỌC — ĐỐI CHIẾU — LẬP KẾ HOẠCH (**PLAN-ONLY, không vá gì**)
> **Ngày:** 23/08/2026 12:0x · **Owner:** TanPhatERP · **Actor:** Agent IDE (phiên "Luật Cho Dự Án TanPhat")
> **HEAD:** `<mã-nguồn-riêng>` · nhánh `main` · parity 5 file luật `<mã-nguồn-riêng>…` (1 mã duy nhất)
> **Căn cứ:** Sổ Yêu Cầu Owner **mục #130** (23/08)

---

## 0) TRẢ LỜI THẲNG CÂU HỎI CỦA ANH

**"Tổng Hợp Skill" là một TAB / phiên Claude Code khác**, không phải mục trong tài liệu — ảnh anh gửi xác nhận: 5 tab đang mở là *Code Fix Tổng Thể · Chuẩn Hóa UI Webapp · Luật Cho Dự Án TanPhat E… · **Tổng Hợp Skill** · Xử Lý Nhất Quán Tài Liệu…*

| Câu hỏi | Trả lời |
|---|---|
| Em đọc được khung chat của tab đó không? | **KHÔNG.** Mỗi tab là một phiên riêng, em không thấy được nội dung hội thoại của phiên khác |
| Vậy em đọc được gì? | **Toàn bộ thứ phiên đó ghi ra đĩa** — và đó mới là thứ có giá trị: `docs/reports/AUDIT-DO-RONG-LUAT-20260823.md` (479 dòng) · `DEBT-066…090` trong sổ nợ · mục sổ Owner **#125** · các commit |
| Đủ để "nhất quán liền mạch" chưa? | **Đủ** — vì luật của dự án đã bắt mọi phiên phải ghi ra sổ + báo cáo. Đó chính là cơ chế chống rời rạc mà mình dựng suốt tuần qua, và lần này nó chạy đúng |

---

## 1) ⚠️ TRƯỚC HẾT — EM PHẢI TỰ NHẬN HAI LỖI

### 1.1 Phiên này chạy trên ngữ cảnh **cũ 3 ngày**

| | Em tưởng | Thực tế lúc đọc lại |
|---|---|---|
| Ngày | 20/08 | **23/08 12:09** |
| Nhánh | `gov/2026-08-18-rules-ui-standard-upgrade` | **`main`** |
| HEAD | `<mã-nguồn-riêng>` | **`<mã-nguồn-riêng>`** |
| Mã nợ lớn nhất | DEBT-032 | **DEBT-090** |
| Mục sổ Owner lớn nhất | #93 | **#129** |

### 1.2 Hậu quả thật: em đã cấp **trùng mã nợ** — chính là `DEBT-082`

`DEBT-082` (do phiên "Tổng Hợp Skill" phát hiện) ghi: *"Sổ nợ có 3 mã bị CẤP TRÙNG — `DEBT-030` (dòng 45 và 55), `DEBT-031` (46 và 56), `DEBT-032`…"*

**Đó là do em.** Phiên 20/08 của em cấp DEBT-030/031, phiên 20/08 sau cấp DEBT-032 — trong khi một phiên khác đã dùng các mã đó. Em làm đúng quy tắc *"đọc dải mã trước khi cấp"*, nhưng đọc trên **bản sổ đã cũ**.

→ Bài học không phải "đọc sổ trước" (em có đọc) mà là **"đọc sổ ở đúng nhánh/đúng thời điểm trước"**. Đề xuất `K5` bên dưới bịt chỗ này.

---

## 2) 🔴 PHÁT HIỆN NGHIÊM TRỌNG NHẤT — VÀ NÓ RỘNG HƠN BÁO CÁO KIA NÊU

Phiên "Tổng Hợp Skill" tìm ra: cổng `pii-scan` **mù với mọi đường dẫn có dấu tiếng Việt**, vì gọi `git ls-files` **thiếu cờ `-z`**. Em **tự kiểm chứng lại** và xác nhận đúng:

```
git ls-files       → "docs/<thư-mục-có-dấu-đã-che>/<tên-file-đã-che>.csv"
git ls-files -z    → docs/<thư-mục-có-dấu-đã-che>/<tên-file-đã-che>.csv
```

Git tự **trích dẫn kiểu C** mọi đường dẫn có byte non-ASCII. Chuỗi cổng nhận được kết thúc bằng `csv"` chứ không phải `csv`, nên mẫu `BULK_EXT` (`\.csv$`) và `SOURCE_DIRS` (`<mẫu-thư-mục-đã-che>`) **không bao giờ khớp**.

### Nhưng phạm vi lớn hơn: **4 cổng**, không phải 1

| Cổng | Dòng | Nối vào pre-commit? |
|---|---|---|
| `scripts/tests/pii-scan-gate.test.mjs` | `:84` | ✅ **CÓ** |
| `scripts/tests/secret-scan-gate.test.mjs` | `:123` | ✅ **CÓ** |
| `scripts/tests/path-audit.test.mjs` | `:67` | ❌ |
| `scripts/tests/script-parse-gate.test.mjs` | `:18` | ❌ |

→ **`secret-scan` cũng mù y hệt** — báo cáo kia chưa nêu. Nghĩa là **cả hai cổng đang gác pre-commit đều không nhìn thấy** bất kỳ file nào có dấu tiếng Việt trong tên hoặc thư mục cha. Trong một dự án Việt Nam, đó là một lớp file rất lớn.

### Đây là lỗi của em

Em viết cả hai cổng đó (18–20/08). Em **đã lường đúng rủi ro** — có sẵn mẫu `csv` và thư mục `<mẫu-thư-mục-đã-che>` trong `SOURCE_DIRS` — nhưng thất bại ở khâu **lấy danh sách file**. Em cũng đã "kiểm ngược" hai cổng bằng cách gieo file thử, nhưng **file thử của em đặt tên ASCII** (`_tmp_pii_probe.md`), nên phép kiểm ngược đi qua đúng cái lỗ này mà không chạm vào nó.

> **Bài học rút ra, đáng ghi thành luật:** kiểm ngược chỉ chứng minh được đúng cái ca mình nghĩ ra. Ca thử phải **giống dữ liệu thật** — dự án tiếng Việt thì ca thử phải có dấu tiếng Việt. Xem đề xuất `K4`.

### Hậu quả đang tồn tại

File `docs/<thư-mục-có-dấu-đã-che>/<tên-file-đã-che>.csv` — **hàng nghìn dòng khách hàng**, 28 cột, gồm **số tài khoản ngân hàng · tên chủ tài khoản · ngân hàng · mã số thuế · số điện thoại · địa chỉ · giá trị đơn hàng** — **đang được git theo dõi và đã lên remote**. Em xác nhận lại bằng `git ls-files -z`: file **có** trong danh sách theo dõi.

---

## 3) BỨC TRANH HỢP NHẤT — 5 LUỒNG ĐANG CHẠY SONG SONG

| Tab / luồng | Việc | Trạng thái |
|---|---|---|
| **Tổng Hợp Skill** | Audit độ rỗng của luật | 🔴 **BLOCKED** — chờ Owner 5 câu |
| **Luật Cho Dự Án TanPhat** (phiên này) | Ban hành + vá luật, cổng kiểm | Đã ban hành 14 luật, 13 cổng |
| **Chuẩn Hóa UI Webapp** | SSOT giao diện | Nợ UI: DEBT-083…088 |
| **Code Fix Tổng Thể** | Go-live chuỗi bán hàng | Mục sổ #129, ĐỢT A–D |
| **Xử Lý Nhất Quán Tài Liệu** | Đồng bộ tài liệu | — |

**Số liệu sổ nợ hiện tại:** **95 nợ** · đã đóng **14** · **còn mở 81**.

### Vì sao "rời rạc" như anh lo — và cơ chế nào đang giữ nó lại

Năm phiên chạy song song trên **cùng một nhánh `main`**, cùng ghi vào **hai file sổ dùng chung**. Đó là lý do có:
- **DEBT-082** — trùng mã nợ (em gây ra)
- Phiên "Tổng Hợp Skill" báo: *"3 file của em bị cuốn vào commit `5348619` của phiên khác — nội dung nguyên vẹn và đã push, nhưng thông điệp commit em soạn đã mất"*

**Nhưng cơ chế chống rời rạc đang hoạt động:** chính vì luật bắt mọi phiên ghi sổ + xuất báo cáo, mà hôm nay em ngồi ở tab này vẫn dựng lại được đầy đủ việc của tab kia. Thứ còn thiếu chỉ là **chống va chạm khi ghi đồng thời** — xem `K5`.

---

## 4) 🔴 HAI VIỆC GẤP PHIÊN KIA ĐANG CHỜ ANH

Trích nguyên trạng, kèm đối chiếu của em:

| # | Câu hỏi | Đề xuất của phiên kia | Em đối chiếu thêm |
|---|---|---|---|
| **1** | File CSV hàng nghìn dòng khách hàng (số tài khoản ngân hàng) đang trong git, đã lên remote — xử lý sao? | Gỡ khỏi cây làm việc + `.gitignore` **ngay**. Viết lại lịch sử **không suy rộng** tiền lệ Q1/D3 — lần đó là bí mật nội bộ, lần này là **dữ liệu của khách hàng**. Cần Owner quyết riêng | **Em đồng ý và nhấn mạnh:** Q1 (19/08) chốt "kho riêng tư → không viết lại history" cho **credential nội bộ**. Dữ liệu khách hàng là **loại khác** — không được suy rộng quyết định cũ sang. Đây đúng là chỗ `GOV-SESSION-DECISION-001` §F1b mục 5 bắt phải tra sổ trước khi kết luận |
| **2** | Vá cổng `pii-scan` (thêm `-z`) — làm ngay hay chờ? | Làm ngay phiên sau. Sửa 1 dòng, nhưng **bắt buộc kèm kiểm ngược** | **Em bổ sung: phải vá 4 cổng, không phải 1** (§2). Và ca kiểm ngược **bắt buộc dùng tên file có dấu tiếng Việt** — nếu không sẽ lặp lại đúng lỗ hổng cũ |

---

## 5) KẾ HOẠCH MỘT LƯỢT — XẾP THEO *RỦI RO KHÔNG ĐẢO NGƯỢC ĐƯỢC*

Nguyên tắc xếp: việc mà **để lâu thêm một ngày là mất thêm** đứng trước; việc dọn dẹp đứng sau.

### 🔴 K1 — Vá 4 cổng `git ls-files -z` *(1 dòng/cổng · ~15 phút)*

Sửa `pii-scan` · `secret-scan` · `path-audit` · `script-parse-gate`.

**Điều kiện nghiệm thu bắt buộc (`GOV-ACCEPTANCE-FIRST-001`):**
- Trước vá: chạy cổng → phải **KHÔNG** thấy file CSV kia.
- Sau vá: chạy cổng → phải **THẤY** và **FAIL**.
- Ca kiểm ngược mới: gieo 1 file tên **có dấu tiếng Việt** chứa PII → cổng phải FAIL. Xoá file thử.
- Chạy `npm run test:gov-gates` toàn bộ để xem còn cổng nào mù nữa không.

> Làm K1 **trước** K2, vì sau khi vá, cổng mới đủ tin cậy để xác nhận K2 đã sạch.

### 🔴 K2 — Xử lý file dữ liệu khách hàng *(cần anh quyết trước)*

| Bước | Nội dung | Cần anh? |
|---|---|---|
| a | `git rm --cached` + thêm `.gitignore` + giữ file trên máy | Chỉ cần anh gật |
| b | Rà xem còn file dữ liệu khách hàng nào khác đang bị theo dõi (dùng cổng đã vá ở K1) | Không |
| c | **Viết lại git history?** | ⚠️ **CHỈ ANH QUYẾT** — đây là dữ liệu khách hàng, không suy rộng từ Q1 |
| d | Nếu (c) = không → ghi nhận phơi nhiễm vào `secret-exposure-status.md` + nêu rõ đã lên remote | Không |

### 🟠 K3 — Ba câu còn lại của phiên "Tổng Hợp Skill"

| Câu | Đề xuất | Em có ý kiến thêm |
|---|---|---|
| 3 · Đợt nạp production chạy khi chưa chốt ngưỡng — hồi tố sao? | Ghi ngoại lệ có lý do + chốt ngưỡng **trước** đợt sau. Không hoàn tác | Đồng ý — hoàn tác dữ liệu đã nạp rủi ro hơn nhiều so với lợi ích |
| 4 · 5 biểu mẫu chưa từng dùng: cấp nơi lưu hay hạ xuống khuyến nghị? | **Chọn một, đừng để nguyên** — luật MUST mà 0 lần thi hành làm mòn hiệu lực cả bộ luật | Đồng ý mạnh. `DEBT-080` cho thấy thư mục `.governance/acceptance/` mà luật chỉ định **còn chưa tồn tại** |
| 5 · Sổ nợ dùng 2 trạng thái luật không định nghĩa — sửa sổ hay sửa luật? | **Sửa luật** — 2 trạng thái đó phản ánh vận hành đúng | Đồng ý (`DEBT-074`) |

### 🟠 K4 — Luật mới: **ca kiểm ngược phải giống dữ liệu thật**

Bài học §2: em kiểm ngược 2 cổng bằng file tên ASCII nên đi ngay qua lỗ hổng. Đề xuất bổ sung vào `GOV-GATE-REAL-INPUT-001` (§G7.7):

> *Ca kiểm ngược phải mang **đặc tính của dữ liệu thật trong dự án** — dự án tiếng Việt thì ít nhất một ca phải có dấu tiếng Việt trong tên file/thư mục. Kiểm ngược bằng ca "sạch" chỉ chứng minh cổng chạy, không chứng minh cổng phủ.*

### 🟡 K5 — Chống va chạm giữa các phiên song song *(gốc của `DEBT-082`)*

Năm tab cùng ghi 2 file sổ trên cùng nhánh. Ba lựa chọn, xếp theo công sức:

| | Cách | Ưu | Nhược |
|---|---|---|---|
| **a** | Trước khi cấp mã mới: bắt buộc `git pull` + đọc lại dải mã | Rẻ nhất, 1 dòng luật | Vẫn kẹt nếu 2 phiên cấp cùng lúc |
| **b** | Mã nợ mang tiền tố phiên (`DEBT-UI-001`, `DEBT-LAW-001`) | Không bao giờ trùng | Phải đổi sổ đang có 95 dòng |
| **c** | Cổng kiểm trùng mã, chạy trong pre-commit | Bắt được ngay, tự động | Cần viết cổng mới |

**Em nghiêng về (a) + (c)**: (a) chặn phần lớn, (c) là lưới an toàn. Không đổi 95 dòng sổ đang có.

### 🟡 K6 — Dọn nợ tồn *(sau khi K1–K5 xong)*

81 nợ mở. Gom nhóm để anh xử một lượt thay vì từng cái:

| Nhóm | Mã nợ | Gợi ý |
|---|---|---|
| Trùng mã / trạng thái sổ | `DEBT-074` `DEBT-082` | Xử cùng K3 câu 5 + K5 |
| Luật trỏ sai số / trỏ hụt | `DEBT-073` (125 vs 128 skill) · `DEBT-089` (cổng không quét trong SSOT) · `DEBT-078` (không cổng nào kiểm số liệu bản công bố) | Một phiên luật gom hết |
| Chờ Owner quá hạn | `DEBT-076` `DEBT-077` (hạn 26/08) · `DEBT-090` | Cần anh trả lời, đang chặn |
| Giao diện | `DEBT-081` `DEBT-083`…`DEBT-088` | Phiên "Chuẩn Hóa UI Webapp" |
| Hạ tầng luật chưa có ruột | `DEBT-079` `DEBT-080` | Gom với K3 câu 4 |

---

## 6) VIỆC EM ĐÃ LÀM TRONG PHIÊN NÀY *(không vá gì)*

| Việc | Kết quả |
|---|---|
| Định vị "Tổng Hợp Skill" | Là tab/phiên khác — đã xác nhận bằng ảnh anh gửi |
| Đọc sản phẩm phiên đó | `AUDIT-DO-RONG-LUAT-20260823.md` 479 dòng · `DEBT-066…090` · mục sổ #125 |
| Đọc thư viện skill hai bên | Notion "📚 Kỹ năng AI" KN-V2.7 (**31 kỹ năng**) ↔ local `.cursor/skills` (**128 skill**, `skills.yml` 128 entry) |
| Tự kiểm chứng lỗi gốc | ✅ Xác nhận, **và mở rộng: 4 cổng chứ không phải 1** |
| Ghi sổ Owner | ✅ **mục #130** |
| Tự nhận lỗi | ✅ `DEBT-082` là do phiên này gây ra |

### Một thứ đáng giá tìm được khi đọc hai thư viện skill

Notion có kỹ năng **KN21 "🎨 Thiết kế Giao diện và Hệ thống Thiết kế ERP"**. Trong đó:
- **Output** bắt buộc có: *"… **Acceptance checklist**"*
- **Quy trình** bước 6: *"Prototype → usability review → implementation spec → **visual regression**"*
- **Quy trình** bước 4: *"… feedback/state, **empty/error/no-permission**"*

Đây **chính xác** là 3 thứ mà bản tự soát ngày 18/08 kết luận là **KHÔNG TỒN TẠI** và đánh dấu `UNVERIFIED` (U1/U2/U3), vì lúc đó nằm ngoài lớp được phép đọc.

→ **Nay xác nhận được:** yêu cầu ấy **có tồn tại** — nhưng nằm ở thư viện Notion dành cho **TanPhatAI**, trong khi bên thi hành giao diện là **Agent IDE** (đọc `.cursor/skills`). Hai thư viện **không có bảng ánh xạ nào** với nhau.

**Đó là lời giải cuối cùng cho ca thất bại 12 lượt giao diện 17–18/08:** yêu cầu "phải có acceptance checklist" đã tồn tại từ trước, chỉ là **nằm ở thư viện mà bên thi hành không đọc được**. Trùng khớp hoàn toàn với nguyên nhân gốc #2 đã ghi trong bản tự soát: *"chuẩn có tồn tại nhưng không bị bắt buộc đọc"*.

→ Đề xuất gộp vào K6: **lập bảng ánh xạ 31 kỹ năng Notion ↔ 128 skill local**, ít nhất cho nhóm giao diện và nhóm quản trị.

---

## 7) BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Trả lời câu hỏi của Owner: "Tổng Hợp Skill" là TAB/phiên Claude Code khác, không
     phải mục tài liệu. Không đọc được khung chat phiên khác, nhưng đọc được TOÀN BỘ
     sản phẩm phiên đó ghi ra đĩa.
   - Đọc AUDIT-DO-RONG-LUAT-20260823.md (479 dòng) + DEBT-066…090 + mục sổ #125.
   - Đọc hai thư viện skill: Notion "Kỹ năng AI" KN-V2.7 (31) ↔ local .cursor/skills (128).
   - TỰ KIỂM CHỨNG lỗi git ls-files thiếu -z → xác nhận đúng, VÀ mở rộng: 4 cổng bị
     ảnh hưởng chứ không phải 1 (pii-scan · secret-scan · path-audit · script-parse-gate),
     trong đó 2 cổng đầu đang gác pre-commit.
   - Xác nhận file CSV hàng nghìn dòng khách hàng ĐANG được git theo dõi (git ls-files -z).
   - Nối được manh mối cuối của ca thất bại giao diện: KN21 trên Notion đã bắt buộc
     "Acceptance checklist" + "visual regression" từ trước — nhưng nằm ở thư viện mà
     bên thi hành không đọc được. Đóng U1/U2/U3 của bản tự soát 18/08.
   - Lập kế hoạch K1–K6 xếp theo rủi ro không đảo ngược được.
   - Ghi Sổ Yêu Cầu Owner mục #130.

2. PHẠM VI
   ĐỤNG    : docs/OWNER-REQUEST-LEDGER.md (mục #130) ·
             docs/reports/HOP-NHAT-TONG-HOP-SKILL-VA-PHIEN-LUAT-20260823.md (mới)
   KHÔNG ĐỤNG: 5 file luật? KHÔNG (parity <mã-nguồn-riêng> không đổi) · cổng kiểm? KHÔNG vá ·
             file CSV khách hàng? KHÔNG đụng · src/? KHÔNG · DB? KHÔNG · deploy? KHÔNG ·
             SSOT nội dung? KHÔNG · Notion? CHỈ ĐỌC · sổ nợ? KHÔNG sửa dòng nào

3. BẰNG CHỨNG
   git ls-files | grep csv → chuỗi trích dẫn kiểu C "docs/<thư-mục-có-dấu-đã-che>…"  → CODE_PROVEN
   git ls-files -z | tr → đường dẫn thật, file CSV CÓ trong danh sách theo dõi     → CODE_PROVEN
   grep 'execSync("git ls-files' scripts/tests/*.mjs → 4 cổng, không cổng nào có -z → CODE_PROVEN
   git log/HEAD → <mã-nguồn-riêng>, nhánh main, ngày hệ thống 23/08/2026 12:09              → FILE_PROVEN
   sổ nợ → 95 nợ, đóng 14, mở 81, mã lớn nhất DEBT-090                             → FILE_PROVEN
   sổ Owner → mục lớn nhất #129 trước khi ghi, nay #130                            → FILE_PROVEN
   sha256sum CLAUDE.md → <mã-nguồn-riêng> (không đổi)                                      → FILE_PROVEN
   Notion KN21 → Output có "Acceptance checklist", bước 6 có "visual regression"    → DOC_PROVEN
   skills.yml → 128 entry / 128 thư mục; Notion KN-V2.7 → 31 kỹ năng               → FILE_PROVEN
   ⚠️ CHƯA có UI_PROVEN / DB_PROVEN / RUNTIME_PROVEN — phiên PLAN-ONLY, không chạy
      ứng dụng, không vá cổng nên không có đầu ra thi hành để dẫn.

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #130
   Ghi bằng quyền mặc định GOV-SESSION-DECISION-001 §F1b mục 3.

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng> (bản đầu HỎNG do lỗi shell) → **sửa ở commit <mã-nguồn-riêng>**
       · file HOP-NHAT-TONG-HOP-SKILL-VA-PHIEN-LUAT-20260823.md
   Công-bố-an-toàn: KHÔNG nêu tên file/thư mục/cột của dữ liệu khách hàng trong bản
   công khai, KHÔNG nêu giá trị nào, KHÔNG nêu mã commit chứa dữ liệu nhạy cảm.

6. CÒN SÓT / CHƯA LÀM
   - K1 vá 4 cổng thiếu -z — CHƯA làm (Owner yêu cầu tự xử lý một lượt) → DEBT-066…082
     đã phủ phần pii-scan; phần secret-scan/path-audit/script-parse-gate là PHÁT HIỆN
     MỚI của phiên này, chưa có mã nợ riêng.
   - K2 file CSV khách hàng — CHƯA đụng, chờ Owner quyết (câu 1 của phiên kia).
   - K3 ba câu còn lại — chờ Owner.
   - K4 luật "ca kiểm ngược phải giống dữ liệu thật" — mới đề xuất, chưa ban hành.
   - K5 chống va chạm phiên song song — mới đề xuất; gốc của DEBT-082 do em gây ra.
   - K6 bảng ánh xạ 31 kỹ năng Notion ↔ 128 skill local — chưa lập.
   - 81 nợ mở, trong đó DEBT-076/077 hạn 26/08 đang chờ Owner.
   ⚠️ Đã rà lại thật. Ba mục K1(phần mở rộng)/K4/K5/K6 chưa có mã DEBT vì phiên này
      KHÔNG được phép sửa sổ nợ — đề nghị Owner cho ghi ở phiên xử lý.

7. ĐANG CHỜ OWNER
   - Câu 1 (GẤP): file CSV hàng nghìn dòng khách hàng — gỡ khỏi git? có viết lại history không?
     → Không suy rộng từ Q1: lần đó là bí mật nội bộ, lần này là dữ liệu khách hàng.
   - Câu 2 (GẤP): vá cổng — em bổ sung: phải vá 4 cổng, và ca kiểm ngược phải có dấu
     tiếng Việt.
   - Câu 3/4/5 của phiên "Tổng Hợp Skill" (hồi tố ngưỡng nạp · 5 biểu mẫu · 2 trạng thái sổ).
   - K4 K5 K6: đồng ý hướng nào để em thi hành ở phiên sau.
   - DEBT-076 + DEBT-077 hạn 26/08 — còn 3 ngày.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Anh quyết câu 1 + câu 2 (hai việc gấp). Em đề nghị thứ tự K1 trước K2: vá cổng xong
   thì mới có công cụ tin cậy để xác nhận K2 đã sạch.

9. CHƯA XÁC MINH ĐƯỢC
   - Còn bao nhiêu file có dấu tiếng Việt khác đang mang dữ liệu nhạy cảm — chưa quét
     được vì cổng đang mù; phải vá K1 trước mới đếm đúng. Ai: phiên xử lý sau K1.
   - Nội dung hội thoại của 4 tab còn lại — không đọc được khung chat phiên khác;
     chỉ dựng lại được qua sổ + báo cáo. Ai: Owner, hoặc chính các phiên đó.
   - File CSV đã bị ai tải về từ remote chưa — không có log truy cập. Ai: Owner (GitHub).
   - 3 file của phiên "Tổng Hợp Skill" bị cuốn vào commit 5348619 của phiên khác —
     nội dung còn nguyên nhưng thông điệp commit đã mất; không khôi phục được. Ai: —

10. TRẠNG THÁI CHUNG
   [ ] PASS
   [x] PROVISIONAL — phần ĐỌC + ĐỐI CHIẾU + LẬP KẾ HOẠCH đã xong và có bằng chứng
       CODE_PROVEN/FILE_PROVEN. Nhưng phiên là PLAN-ONLY: chưa vá gì, và hai việc gấp
       (K1 K2) đang chờ Owner. Điều kiện lên PASS: Owner quyết câu 1 + câu 2, K1 vá
       xong và kiểm ngược bằng ca có dấu tiếng Việt đạt.
   [ ] BLOCKED

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: KHÔNG — nhưng phiên chạy trên NGỮ CẢNH CŨ 3 NGÀY,
   hậu quả tương đương một lần nén: cấp trùng mã nợ (DEBT-082). Đã đọc lại từ đĩa
   TRƯỚC khi kết luận, đúng tinh thần GOV-RELOAD-AFTER-COMPACT-001.
   Tài liệu đã đọc lại trực tiếp từ đĩa trong phiên này:
     · docs/reports/AUDIT-DO-RONG-LUAT-20260823.md (cấu trúc + §0 S1/S2 nguyên văn)
     · .governance/registry/tech-debt.md (toàn bộ 95 dòng, thống kê mở/đóng)
     · docs/OWNER-REQUEST-LEDGER.md (dải mục, mục #129 trước khi chèn #130)
     · scripts/tests/{pii-scan,secret-scan,path-audit,script-parse}-gate — dòng gọi git ls-files
     · .governance/registry/skills.yml (128 entry, 10 trường)
     · docs/SKILL_UPGRADE_PLAN_20260819.md (§0 tóm tắt + §6 decision gate 10 phiếu)
     · Notion (CHỈ ĐỌC): "📚 Kỹ năng AI" KN-V2.7 · "🎨 Thiết kế Giao diện và Hệ thống
       Thiết kế ERP" KN21 · "🧭 Bàn giao ngữ cảnh 21/08"
     · git log / git ls-files / git ls-files -z (đối chứng lỗi trích dẫn)
═══════════════════════════════════════════
```
