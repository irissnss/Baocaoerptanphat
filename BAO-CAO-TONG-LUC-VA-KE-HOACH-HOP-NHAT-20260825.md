# BÁO CÁO TỔNG LỰC HỢP NHẤT + KẾ HOẠCH TỔNG LỰC — 25/08/2026 *(bản công khai đã lọc)*

> **Bản này đã lọc theo `GOV-PUBLIC-SAFE-001`.** So với bản nội bộ (kho riêng tư), bản công khai đã **bỏ**: nguyên văn lời Owner · đường dẫn tệp dữ liệu khách hàng · giá trị nhạy cảm. **Giữ nguyên**: kết luận · số đo · mã nợ · mã commit · tên cổng/module/màn hình · toàn bộ kế hoạch · bảng quyết định · phần *"điều chưa chứng minh được"* · phần tự đính chính.
>
> **Mã mục sổ Owner:** `#154` · **Lớp bằng chứng:** `FILE_PROVEN` + `CODE_PROVEN` + `RUNTIME_OBSERVED` *(local)*
>
> **Đây là tài liệu HỢP NHẤT DUY NHẤT** — gom trọn 10 phiên Claude Code · 8 báo cáo kết thúc · gói quyết định 6 nhóm (`#152`) · kế hoạch liên phiên (`#153`) · 110 dòng nợ kỹ thuật · việc sản phẩm đang treo (`#141`). Các tài liệu đó **KHÔNG bị thay thế** — bản này là **lớp điều phối trên cùng**.

---

# PHẦN I — TÓM TẮT ĐIỀU HÀNH

## I.1 Ba câu hỏi của chủ dự án — trả lời dứt điểm

| Câu hỏi | Trả lời | Bằng chứng |
|---|---|---|
| Đã push hết chưa? | **RỒI — cả hai kho, không còn gì** | Kho mã `<mã-nguồn-riêng>` · kho công khai `<mã-nguồn-riêng>` · cả hai `0 ahead / 0 behind` · cây làm việc sạch |
| Tương tác trực tiếp đã thành bảng chưa? | **RỒI — 154 mục, không trùng mã** | `test:ledger-dup-id` PASS 5/5 · mã kế tiếp an toàn `#155` |
| Có cần thêm luật vào bộ 5 tệp quản trị? | **KHÔNG — luật đã đủ và đúng** | §F1b (20/08) + §F1c (25/08) phủ đúng · Doc `2.8` · 5 tệp byte-identical `<mã-nguồn-riêng>` · **16/16 cổng PASS** |

## I.2 Kết luận cốt lõi — một câu

> **Bộ luật không thiếu gì. Thứ thiếu là CƠ CHẾ ĐIỀU PHỐI giữa các phiên chạy song song, và VIỆC SẢN PHẨM đang bị bỏ quên trong lúc 10 phiên bận rà sổ sách.**

## I.3 Bốn việc gấp nhất

| # | Việc | Vì sao gấp | Chặng |
|---|---|---|---|
| **1** | **Chốt 1 phiên điều phối** | 10 phiên cùng ghi 2 cuốn sổ chung, không có cơ chế cấp số | **Chặng 0** |
| **2** | **Gỡ tệp dữ liệu khách hàng khỏi git** | Tệp chứa **2.368 số điện thoại · 22 email · cột số tài khoản ngân hàng**, đã lên remote. Cổng xanh vì đã ghi *nợ đã biết*, **KHÔNG phải vì đã dọn** | **Chặng 2** |
| **3** | **Chốt công thức khổ trải (`DEBT-043`)** | **Hai công thức KHÁC NHAU cùng nằm trong mã** ⇒ một trong hai đang tính sai tiền | **Chặng 3** |
| **4** | **Nối lại việc sản phẩm `#141`** | 6 việc + 3 quyết định nghiệp vụ giao 24/08 vẫn **ĐANG LÀM**, điều tra bị dừng | **Chặng 5** |

## I.4 Tin tốt

- Hai kho **sạch tuyệt đối**, không mất commit nào.
- **16/16 cổng quản trị PASS**, `EXIT=0` — trước và sau mọi lần ghi.
- **Môi trường local ĐANG CHẠY** *(đo 15:05: CSDL cổng *(cổng — đã che)* UP · app `3000` UP)* ⇒ **không có điểm chặn kỹ thuật** cho việc code.
- Cổng quét dữ liệu cá nhân **đã hết mù** từ 23/08 — báo cáo nói *"chưa vá"* là đo nhầm chỗ.
- Ba phiên hành xử **đúng luật** trong tình huống khó: một phiên tự dừng không commit · một phiên tự khai lỗi ghi trùng · một phiên tự đính chính ngữ cảnh cũ.

---

# PHẦN II — HIỆN TRẠNG TOÀN CỤC *(đo 25/08/2026 ~15:05)*

## II.1 Hai kho mã

| Kho | HEAD | Chưa push | Chưa kéo về | Cây làm việc |
|---|---|---:|---:|---|
| Kho mã *(riêng tư)* | `<mã-nguồn-riêng>` | **0** | **0** | sạch |
| Kho báo cáo *(công khai)* | `<mã-nguồn-riêng>` | **0** | **0** | sạch · **91 tệp** |

**Nhánh:** `main` + 3 nhánh cũ — **cả 3 có `0` commit lệch `main` và `0` tệp khác biệt** ⇒ xoá an toàn.

## II.2 Bộ luật quản trị

| Mục | Giá trị |
|---|---|
| Doc version | **2.8** |
| 5 tệp replica | `CLAUDE.md` · `AGENTS.md` · `GEMINI.md` · `.cursorrules` · `.antigravityrules` |
| Parity SHA-256 | **`<mã-nguồn-riêng>…` × 5/5 — byte-identical** |
| Điều khoản | **399** *(38 mã luật + 361 heading)* · **mốc cổng đang 386** ⇒ cần đóng lại |
| 4 luật mới chưa đóng mốc | `GOV-NOTION-HANDOFF-001` · `GOV-SECRET-LOCATION-001` · `GOV-SESSION-DECISION-001` · `GOV-SKILL-CONTENT-STATUS-001` |

## II.3 Hai cuốn sổ

| Sổ | Số mục | Mã kế tiếp | Trùng mã |
|---|---:|---|---|
| Sổ Yêu Cầu Owner | **154** | `#155` | **0** |
| Sổ nợ kỹ thuật | **110** | `DEBT-105` | **0** *(6 mã đã cấp lại bằng hậu tố `-B`)* |

**Trường `notion_sync_state`:** khai ở **21/154 mục (14%)** — `OWNER_APPROVED_PENDING_NOTION_SYNC` 15 · `HANDOFF_PACKAGED` 5 · `NOT_APPLICABLE` 2. **133 mục chưa khai.**

## II.4 Cổng tự kiểm

**62 cổng** khai trong `package.json`. Chuỗi quản trị = **16 cổng · 16/16 PASS · `EXIT=0`**:

`clause-count` · `standard-clause-count` · `ref-exists-gate` (57/57) · `skills-registry` · `registry-history-filter` (8/8) · `quality-gate-contract` (10/10) · `skill-content-status` · `notion-sync-state` · `context7-registry` · `ledger-dup-id` (5/5) · `script-parse` · `secret-scan` · `pii-scan` · `completion-report-gate` · `ui-skill-conflict` · `version-policy` (37/37)

## II.5 Phiên bản & môi trường — **BA LỚP bằng chứng**

| Lớp | Giá trị | Bằng chứng |
|---|---|---|
| `CODE_CURRENT` | **V1.00.355** | `src/lib/version.ts` |
| `DEPLOYMENT_RECORDED` | **V1.00.355** | hồ sơ phát hành trong registry |
| `RUNTIME_OBSERVED` *(máy vận hành)* | ⛔ **UNVERIFIED** | chưa được cấp kênh đo |
| `LOCAL_RUNTIME_OBSERVED` | ✅ **UP** *(đo 15:05)* | CSDL *(cổng — đã che)* UP · app `3000` UP |

> ⚠️ **Đính chính registry:** dòng cũ ghi local `BLOCKED_SAFE_START` *(đo sáng nay)*. Phép đo 15:05 cho **UP**. Đã ghi khối đo mới, **giữ nguyên văn dòng cũ** theo `GOV-EDIT-PRESERVE-001`. **Local nay `POLICY_MATCH`; máy vận hành vẫn `UNVERIFIED` ⇒ phán quyết toàn cục CHƯA kết luận được.**

**Số bảng CSDL:** **101** theo chủ dự án chốt (sổ `#144`).

## II.6 Quy mô hệ thống

| Mục | Số đo |
|---|---:|
| Màn hình (`page.tsx`) | **79** |
| Tệp Server Action | **46** |
| Kỹ năng trong registry | **128** |
| Cổng tự kiểm | **62** |
| Báo cáo công khai đã đẩy | **91** |

---

# PHẦN III — BẢN ĐỒ 10 PHIÊN

Chín phiên nhận **cùng một tin nhắn** lúc **14:10–14:14**.

| # | Phiên | Chủ đề gốc | Từ | Lượt | Commit | Push | Trạng thái cuối | Đang giữ gì |
|---|---|---|---|---:|---:|---:|---|---|
| 1 | `0f63d1` | Phiên gốc lớn nhất | 09/08 | 20.311 | 211 | 140 | Báo 3 việc còn sót | Điều tra `#141` |
| 2 | `cdc70c` | Audit công cụ AI | 30/07 | 3.191 | 40 | 28 | Đã push gói `#152` | **PLAN OF RECORD** |
| 3 | `5d9e2c` | Điều tra + soát luật *(sinh §G7)* | 18/08 | 4.129 | 41 | 26 | Đính chính ngữ cảnh cũ 2 ngày | Mốc cổng đếm điều khoản |
| 4 | `ccc761` | UI không lặp vòng | 18/08 | 1.068 | 6 | 6 | Tự khai lỗi ghi trùng, đã gỡ | Quy ước ghi bù enum |
| 5 | `44ea19` | Pha C — đóng sổ Pha B | 23/08 | 1.931 | 18 | 11 | Ban hành §F1c · ghi bù 3 mục | — |
| 6 | `07f81b` | Đợt 4 — cổng duyệt giá M3 | 21/08 | 1.184 | 7 | 7 | Vá điểm mù cổng enum | 3 câu hỏi kỹ thuật |
| 7 | `5d184f` | Audit FK cột người | 20/08 | 1.016 | 1 | 1 | Push 3 báo cáo rút gọn | Gói bàn giao đợt 1–5 |
| 8 | `ad4081` | CSDL / môi trường local | 09/08 | 1.442 | 7 | 4 | Chờ 2 việc | Bản sổ công khai cho Notion |
| 9 | `885cf7` | Governance | 23/08 | 354 | 1 | 1 | 🛑 **BLOCKED — tự dừng, không push** | **Audit 3 công cụ** |
| 10 | `b70460` | Hợp nhất *(bản này)* | 25/08 | — | 3 | 3 | Đã lập kế hoạch tổng lực | — |

**Tổng: 9 phiên hoạt động đã tạo 332 commit và 224 lần push** trên cùng một cây làm việc.

---

# PHẦN IV — BỐN MÂU THUẪN ĐO ĐƯỢC

## IV.1 🔴 Gói quyết định `#152` dựa trên tiền đề đã lỗi thời — `DEBT-104`

**Nhóm A · bước 1** đề xuất *"vá cổng quét dữ liệu cá nhân trước — thêm cờ `-z`"*.

**Sự thật:** đã hoàn tất **23/08/2026 lúc 14:36**, commit `<mã-nguồn-riêng>`.

**Vì sao đo sai:** phiên đó đếm chuỗi `-z` **trong tệp cổng** → ra `0`. Nhưng bản vá đã được **rút ra tệp dùng chung** `scripts/tests/lib/tracked-files.mjs` (dòng 38), nơi **4 cổng** cùng gọi: `pii-scan` · `secret-scan` · `path-audit` · `script-parse`.

**Bằng chứng phản chứng:** cổng **in ra được cả 7 đường dẫn có dấu tiếng Việt và emoji**. Cổng còn mù không thể làm vậy.

> ⚠️ **Rủi ro nếu duyệt A1 nguyên văn:** một phiên sẽ đi "vá" cổng đã vá — có thể **làm hỏng ngược bản vá đang tốt**. **Bước 1 phải gạch bỏ. Bốn bước còn lại vẫn đúng nguyên.**
>
> ✅ **Phần cảnh báo còn lại vẫn ĐÚNG và VẪN GẤP:** tệp dữ liệu khách hàng/nhà cung cấp **vẫn bị git theo dõi và đã lên remote**.

## IV.2 🟡 Chín phiên báo chín trạng thái kho — cả chín đều đúng lúc đo

| Phiên báo | HEAD kho mã | HEAD kho công khai |
|---|---|---|
| `ccc761` · `44ea19` | `<mã-nguồn-riêng>` | `<mã-nguồn-riêng>` / `<mã-nguồn-riêng>` |
| `07f81b` | `<mã-nguồn-riêng>` + `<mã-nguồn-riêng>` | `<mã-nguồn-riêng>` |
| `cdc70c` | `<mã-nguồn-riêng>` | `<mã-nguồn-riêng>` |
| `5d184f` | *(không đụng)* | `<mã-nguồn-riêng>` |
| **Chốt 15:05** | **`<mã-nguồn-riêng>`** | **`<mã-nguồn-riêng>`** |

Trong **30 phút**, kho mã đi qua **4 mốc**, kho công khai đi qua **6 mốc**. Không phiên nào báo sai.

## IV.3 🔴 Hai cuốn sổ vỡ cột bảng — `DEBT-103`

**Sổ Yêu Cầu Owner** — header bảng chính khai **3 cột**. Trong 39 hàng dữ liệu:

| Số ô | Số hàng | Hệ quả |
|---:|---:|---|
| **3** *(đúng)* | 19 | — |
| 2 | 1 | thiếu 1 trường |
| **4** | **17** | 1 trường bị nuốt |
| **9** | **2** | **6 trường bị nuốt** |

Cộng **22 dòng trống** nằm giữa các hàng, cắt mạch bảng Markdown.

Trong các ô bị nuốt có `decision_state` và **`notion_sync_state`** — **đúng hai trường mà `GOV-NOTION-HANDOFF-001` §F1c dựng ra cho Agent Notion đọc**.

⇒ **Đây là cơ chế VẬT LÝ của việc "Agent Notion bỡ ngỡ và phản bác".** Luật đúng, trường có, nhưng **bảng nuốt mất trường trước khi Notion kịp đọc**.

**Sổ nợ kỹ thuật cũng dính:** `DEBT-035` có **11 ô/8**.

> Một phiên phát hiện **2 hàng** nhưng **chưa ghi sổ**. Đo lại ra **20 hàng**. **Chưa có cổng nào kiểm hình dạng bảng.**

## IV.4 🟢 Registry ghi local "DOWN", đo lại là "UP"

Registry *(ghi sáng 25/08)* ghi `LOCAL_RUNTIME_OBSERVED = ⛔ BLOCKED_SAFE_START`. Đo lại **15:05 cùng ngày**: CSDL *(cổng — đã che)* UP · app `3000` UP.

⇒ **Môi trường local KHÔNG còn là điểm chặn.** Đã ghi khối đo mới, **giữ nguyên văn dòng cũ**.

---

# PHẦN V — CHẨN ĐOÁN GỐC

## V.1 Không phải thiếu luật — đối chiếu từng điều

| Điều cần | Luật đã ban hành | Ngày | Vị trí |
|---|---|---|---|
| Code được đi trước tài liệu, lệch **không phải là sai** | `GOV-SESSION-DECISION-001` mục 4 | 20/08 | §F1b |
| Ghi sổ là **quyền mặc định**, không phải xin phép | `GOV-SESSION-DECISION-001` mục 3 | 20/08 | §F1b |
| Phải **đọc sổ trước** khi kết luận "code lệch tài liệu = lỗi" | `GOV-SESSION-DECISION-001` mục 5 | 20/08 | §F1b |
| Sổ là **kênh vận chuyển** về Notion, không cạnh tranh Notion | `GOV-NOTION-HANDOFF-001` mục 1 | 25/08 | §F1c |
| **Mỗi chỉ thị = một mục sổ**, kể cả quyết định hoãn | `GOV-NOTION-HANDOFF-001` mục 2 | 25/08 | §F1c |
| `notion_sync_state` có **bảng 5 giá trị** | `GOV-NOTION-HANDOFF-001` mục 3 + trục 6 | 25/08 | §F1c + §F3 |
| **Bắt buộc có gói bàn giao** | `GOV-NOTION-HANDOFF-001` mục 4 | 25/08 | §F1c |
| **Ghi bù giữ mốc thật** | `GOV-NOTION-HANDOFF-001` mục 5 | 25/08 | §F1c |
| Một workstream **một Plan of Record** | `GOV-ONE-PLAN-OF-RECORD-001` | — | §E3 |

**⇒ Bộ luật phủ 9/9. Không cần thêm điều nào.**

## V.2 Gốc thật — bốn lỗ THI HÀNH

| Lỗ | Biểu hiện đo được | Nợ | Chặng vá |
|---|---|---|---|
| **L1 — Không có cơ chế cấp số** | Hai phiên cùng đọc *"mã lớn nhất #147"* rồi cùng ghi `#148` → 4 mã trùng. Cổng bắt **SAU KHI** ghi, không ngăn **TRƯỚC KHI** ghi | `DEBT-102` | **Chặng 1** |
| **L2 — Không có mốc sự thật chung** | 9 phiên đo 9 thời điểm ⇒ 9 câu trả lời khác nhau; một phiên viết gói quyết định dựa trên tiền đề đã cũ 2 ngày | `DEBT-104` | **Chặng 0** |
| **L3 — Không có hàng đợi câu hỏi** | **7 phiên** giữ **13 câu hỏi**, rải rác 7 cửa sổ chat | — | **Chặng 0** |
| **L4 — Không có cổng kiểm hình dạng sổ** | 20 hàng vỡ cột **im lặng nuốt** đúng trường mà Notion đọc | `DEBT-103` | **Chặng 1** |

> §E3 **đã cấm** kế hoạch song song, nhưng **không có cơ chế nào thi hành xuyên phiên**. Luật đúng — thi hành trống. Đúng dạng lỗi mà `GOV-GATE-REAL-INPUT-001` §G7.7 cảnh báo: *khai có, thi hành bằng 0*.

---

# PHẦN VI — KIỂM KÊ NỢ KỸ THUẬT

## VI.1 Phân bố 110 dòng nợ

| Trạng thái | Số dòng |
|---|---:|
| **MỞ** | **61** |
| **MỞ — CHỜ OWNER** | **21** 🔴 |
| ĐÃ XỬ LÝ | 16 |
| HOÃN CÓ CHỦ ĐÍCH | 4 |
| KHÔNG CÒN HỢP LỆ | 3 |
| ĐANG XỬ LÝ | 2 |
| TẠM BỎ | 1 |
| *(hình dạng hàng lỗi)* | 2 |

## VI.2 Hai mươi mốt nợ CHỜ QUYẾT ĐỊNH — gom theo nhóm

### 🔴 NHÓM A — AN TOÀN DỮ LIỆU *(gấp nhất)*

| Nợ | Nội dung | Trạng thái |
|---|---|---|
| `DEBT-066-B` | Tệp dữ liệu KH/NCC **2.368 SĐT · 22 email · cột số tài khoản ngân hàng** còn bị git theo dõi, **đã lên remote** | MỞ · **quá hạn 24/08** |
| `DEBT-091` | **5 tệp bảng tính** tên tiếng Việt bị git theo dõi; 2 tệp nghi là **dữ liệu vận hành thật** | MỞ · hạn 26/08 |
| `DEBT-092` | Bản xuất tài liệu chứa **34 địa chỉ email** bị git theo dõi | MỞ · hạn 26/08 |
| `DEBT-016` | Mật khẩu đăng nhập quản trị — sổ vẫn ghi *"chưa làm"* | ĐANG XỬ LÝ — **CẦN XÁC NHẬN** |
| `DEBT-076` | `DEBT-016` mâu thuẫn với `DEBT-051` đã đóng 23/08 | MỞ — CHỜ QUYẾT |
| `DEBT-067-B` | *(nguyên nhân gốc — cổng mù)* | **ĐÃ VÁ 23/08**, dòng sổ chưa đóng |

> ⚠️ **Cổng quét nay XANH nhưng dữ liệu VẪN TRONG GIT.** Xanh vì 7 tệp đã được ghi vào registry *nợ đã biết* — registry đó **tự khai KHÔNG phải miễn trừ**, có 3 điều kiện tự-FAIL chống biến thành tấm bịt mắt. **Nhưng ghi nợ không phải là dọn nợ.**

### 🟠 NHÓM B — TÍNH GIÁ *(rủi ro TIỀN)*

| Nợ | Nội dung |
|---|---|
| `DEBT-043` | 🔴 **HAI CÔNG THỨC KHỔ TRẢI KHÁC NHAU cùng trong mã** — hai quy ước trục L/W/H khác nhau, cả hai **viết cứng**, trái thiết kế tài liệu ⇒ **một trong hai đang tính sai tiền** |
| `DEBT-042` | GĐ2 bị chặn vì **thiếu DỮ LIỆU SETTING**, không phải thiếu mã: bảng blueprint chỉ **1 dòng** demo, không chứa công thức; bảng giá công đoạn chỉ **10 giá / 85 công đoạn** |
| `DEBT-044` | Bộ kiểm thử tính giá **dựng blueprint GIẢ** ở 3 route; cột `intentional_fail` có thật trong CSDL nhưng **0 dòng mã nào đọc** |

### 🟠 NHÓM C — NẠP DỮ LIỆU

| Nợ | Nội dung |
|---|---|
| `DEBT-069` | Đợt nạp production 22/08 chạy **khi ngưỡng tỉ lệ lỗi CHƯA được chốt** — §G7.12 ghi *"chưa chốt = BLOCK_ALL"*. Lỗi 0% là **may**, không phải cổng hoạt động |
| `DEBT-060` | **gần như toàn bộ khách hàng** đang gán tạm cho **một người** |
| `DEBT-061` | Bảng nhà cung cấp **không có cột người phụ trách** |
| `DEBT-036` · `DEBT-037` · `DEBT-077` | Hạn ghi *"trước khi nạp NCC"* nhưng NCC **đã nạp xong** ⇒ hạn vô nghĩa |

### 🟡 NHÓM D — TÀI LIỆU

`DEBT-039` 🔴 **lệch độ rộng cột hai đầu quan hệ KH ↔ sản phẩm** *(`RUNTIME_PROVEN`, có thể cắt cụt dữ liệu khi ghi)* · `DEBT-029` hai lớp tài liệu chồng nhau không gắn nhãn hiệu lực · `DEBT-038` chưa đối chiếu phần Data Model Notes.

### 🟡 NHÓM E — SỔ SÁCH & LUẬT

`DEBT-102` không cơ chế cấp số nguyên tử · `DEBT-103` hai sổ vỡ cột *(mới)* · `DEBT-104` gói `#152` tiền đề lỗi thời *(mới)* · `DEBT-074` 2 trạng thái luật chưa định nghĩa *(đã vá luật, dòng sổ chưa đóng)* · `DEBT-082`/`048`/`031` mã nợ cấp trùng.

### ⚪ NHÓM F — NHỎ

`DEBT-047` dropdown Sale sót nhân viên chưa có tài khoản · `DEBT-049` 2 stash cũ · `DEBT-050` assertion lỗi thời · `DEBT-090` nút áp mẫu quyền còn viết cứng mã vai trò · `DEBT-026` còn `?? 1` ở 5 chỗ.

## VI.3 Bốn nợ HOÃN CÓ CHỦ ĐÍCH — không cần quyết lại

`DEBT-063` trang người dùng tự quản hồ sơ · `DEBT-064` chuông thông báo · `DEBT-065` chế độ sáng/tối + dọn icon · `DEBT-095` 5 kỹ năng có quy tắc đúng mà chuẩn UI chưa phủ. **Tất cả = BACKLOG SAU GO-LIVE.**

---

# PHẦN VII — VIỆC SẢN PHẨM ĐANG TREO

> **Phần bị bỏ quên trong lúc 10 phiên bận rà sổ sách — và là thứ chủ dự án thật sự cần.**

## VII.1 OIL `#141` — giao 24/08, trạng thái **ĐANG LÀM**

| # | Việc | Trạng thái | Chặn bởi |
|---|---|---|---|
| **1** | **Đơn hàng:** huỷ chỉ **admin/CEO** · đóng đơn **ở trạng thái nào mới được** · đơn **theo dõi tới khi thu xong tiền** mới kết thúc | Quyết định đã khoá, **thi hành chưa xong** | — |
| **2** | **Đề xuất quyền cho Trưởng phòng Sản xuất** | Chưa có đề xuất | `DEBT-100` — CSDL **không có quy trình sản xuất** ⇒ vai trò nhận **0 quyền chuyển trạng thái** |
| **3** | **Quyền xem giá vốn** — hợp nhất code cũ | Chưa hợp nhất | — |
| **4** | `/m0/security` **quá rối** → giao diện **dạng tab giống `/m6`, `/m7`**, có **từng bước rõ ràng** | Chưa làm | Bắt buộc chốt **57 tiêu chí** trước |
| **5** | `/bieu-mau` → đưa vào `/m1` hoặc `/mf` | Chưa làm | — |
| **6** | `/m0/quy-trinh` **có công dụng gì** — phân tích sâu, xử lý nếu liên quan phân quyền | Chưa làm | — |
| **7** | Xong 1–6 → **Đợt 6** → **deploy** | Chưa tới | Phụ thuộc 1–6 |

## VII.2 Ràng buộc bắt buộc cho việc **4** *(giao diện)*

`GOV-ACCEPTANCE-FIRST-001` §G7.1:

1. **TRƯỚC khi sửa dòng mã đầu tiên:** sao bộ **57 tiêu chí** `docs/UI-ACCEPTANCE-CHECKLIST.md` ra `docs/reports/UI-CHECKLIST-m0-security-<ngày>.md` và **chốt với chủ dự án**.
2. Bắt buộc có dòng ở **lớp bằng chứng nơi lỗi lộ ra** ⇒ `UI_PROVEN`, ảnh trước/sau **cạnh trang mẫu**.
3. Đọc **TOÀN PHẦN** `docs/UI-STANDARD.md` (`GOV-READ-STANDARD-001` §G7.2) — **cấm đọc lỗ khoá**.
4. Bám 4 trang mẫu: `/m1/san-pham` · `/m1/khach-hang` · `/m1/nhan-su` · `/m5/kho-thanh-pham`.

> ⚠️ Ca 17–18/08 đo được **12 gói việc · 9 lượt bị bác liên tiếp** vì bỏ đúng những bước trên. `GOV-ITERATION-LIMIT-001` §G7.3: **bị bác lần thứ ba ⇒ DỪNG cách làm cũ**.

---

# PHẦN VIII — KẾ HOẠCH TỔNG LỰC *(một plan duy nhất)*

## VIII.1 Nguyên tắc điều phối

1. **MỘT phiên điều phối** giữ Plan of Record. Các phiên khác **chỉ đọc** cho tới khi Chặng 1 xong.
2. **Không chặng nào bắt đầu khi chặng trước chưa có bằng chứng đóng.**
3. Mỗi chặng có **cổng ra đo được** — không dùng chữ định tính.
4. Gói `#152` (6 nhóm A–F) **KHÔNG bị thay thế** — được **hấp thụ** vào Chặng 2–6, giữ nguyên thứ tự ưu tiên.

## VIII.2 Sơ đồ phụ thuộc

```
CHẶNG 0  ĐÓNG BĂNG & CHỐT SỰ THẬT          [cần trả lời §IX]
   │
   ▼
CHẶNG 1  VÁ HẠ TẦNG SỔ SÁCH                 [1 phiên · nửa ngày]
   │      DEBT-102 · DEBT-103 · cổng shape · mốc 386→399 · xoá 3 nhánh
   │
   ├──────────────┬──────────────┬───────────────┐
   ▼              ▼              ▼               ▼
CHẶNG 2        CHẶNG 3        CHẶNG 4        (song song được
AN TOÀN        TIỀN &         DỮ LIỆU &       sau khi Chặng 1
DỮ LIỆU        TÍNH GIÁ       TÀI LIỆU        đóng)
DEBT-066-B     DEBT-043       DEBT-069/060
DEBT-091/092   DEBT-042       DEBT-061/036/037
DEBT-016/076   DEBT-044       DEBT-039/029/038
   │              │              │
   └──────────────┴──────────────┘
                  ▼
            CHẶNG 5  SẢN PHẨM — OIL #141 → Đợt 6 → DEPLOY
                  │
                  ▼
            CHẶNG 6  DỌN & BÀN GIAO NOTION
```

## VIII.3 CHẶNG 0 — ĐÓNG BĂNG & CHỐT SỰ THẬT

**~30 phút · cần 1 câu trả lời (Q-P0)**

| Bước | Việc | Cổng ra |
|---|---|---|
| 0.1 | Chỉ định **1 phiên điều phối**; 9 phiên còn lại **CHỈ ĐỌC** — cấm ghi sổ, ghi nợ, commit | Nêu đích danh mã phiên |
| 0.2 | Phiên điều phối **dán mốc sự thật §II** vào đầu mọi lượt sau đó | Mốc có trong báo cáo kế tiếp |
| 0.3 | Trả lời **bảng 13 quyết định §IX** một lượt | 13/13 dòng có câu trả lời |

## VIII.4 CHẶNG 1 — VÁ HẠ TẦNG SỔ SÁCH *(chặn mọi chặng sau)*

| # | Việc | Nợ | Cách làm | Cổng ra bắt buộc |
|---|---|---|---|---|
| **1.1** | **Cấp số nguyên tử cho hai sổ** | `DEBT-102` | Lệnh `oil:new` / `debt:new`: đọc lại từ đĩa → chụp `mtime` → lấy mã kế tiếp từ chính cổng → ghi → **so lại `mtime`; đổi ⇒ HUỶ, làm lại** | **Kiểm ngược:** 2 tiến trình ghi cùng lúc → đúng **1 thành công, 1 bị từ chối** |
| **1.2** | **Vá hàng vỡ cột** | `DEBT-103` | Đưa mọi hàng về đúng số cột *(sổ Owner 20 hàng · sổ nợ `DEBT-035`)*. Trường bị nuốt → **khối chú thích dưới bảng**, **giữ nguyên văn** | Tổng điều khoản SAU ≥ TRƯỚC · mỗi mục rời chỗ có **đúng 1** con trỏ |
| **1.3** | **Cổng kiểm hình dạng bảng** | — | `test:ledger-shape` — FAIL khi số ô ≠ số cột header, **kiểm CẢ HAI sổ** | **Kiểm ngược:** thêm ô thừa → đỏ; gỡ → xanh |
| **1.4** | **Siết cổng khai `notion_sync_state`** | — | Bắt buộc khai cho mọi mục có **quyết định MỚI**. **Chỉ bật SAU 1.2** | Đỏ đúng mục thiếu, xanh khi đủ |
| **1.5** | **Đóng lại mốc cổng đếm điều khoản** | — | 386 → **399** | `test:clause-count` PASS mốc mới |
| **1.6** | **Xoá 3 nhánh cũ** | `DEBT-049` *(phần)* | Đã đo: **0 commit lệch, 0 tệp khác biệt** | `git rev-list --count main..<nhánh>` = 0 × 3 |
| **1.7** | **Đóng 3 dòng nợ đã hết hiệu lực** | `DEBT-067-B` · `DEBT-074` · `DEBT-104` | Ba nợ **đã được vá thật** nhưng dòng sổ chưa đóng | Mỗi dòng ghi commit chứng minh |

**Cổng ra Chặng 1:** `test:gov-gates` PASS + `ledger-shape` PASS + **kiểm ngược 2/2**.

## VIII.5 CHẶNG 2 — AN TOÀN DỮ LIỆU 🔴

| # | Việc | Nợ | Ghi chú |
|---|---|---|---|
| ~~2.0~~ | ~~Vá cổng thêm `-z`~~ | `DEBT-067-B` | ❌ **GẠCH BỎ — đã xong 23/08 commit `<mã-nguồn-riêng>`** |
| **2.1** | **Gỡ tệp dữ liệu KH/NCC khỏi git** | `DEBT-066-B` | `git rm --cached` *(giữ tệp trên máy)* + `.gitignore` + **xác nhận bằng `git check-ignore -q`** |
| **2.2** | **Mở kiểm nội dung 2 tệp nghi dữ liệu thật** | `DEBT-091` | Có PII ⇒ gỡ như 2.1. Không có ⇒ giữ + khai vào bản đồ thư mục |
| **2.3** | **Quyết 3 tệp biểu mẫu còn lại** | `DEBT-091` | Đề xuất **GIỮ** + khai vào bản đồ thư mục |
| **2.4** | **Bản xuất 34 email** | `DEBT-092` | Khử bằng bảng ánh xạ `nv01..nvNN@example.invalid` — **giữ cấu trúc + số lượng** |
| **2.5** | **`DEBT-016` mật khẩu quản trị** | `DEBT-016` · `DEBT-076` | Chỉ đóng bằng đối chiếu mật mã thật — **KHÔNG bằng lời khai, KHÔNG bằng so chuỗi băm** *(agent đã đóng sai 2 lần trước đây)* |
| **2.6** | **Viết lại lịch sử git?** | — | ⛔ **Đề xuất KHÔNG** — xem Q-A3 |

**Cổng ra:** cổng quét PASS **với bảng nợ-đã-biết rút ngắn thật sự** *(không phải thêm dòng để làm xanh)* + `git ls-files` không còn tệp dữ liệu KH + kiểm ngược.

## VIII.6 CHẶNG 3 — TIỀN & TÍNH GIÁ 🟠

Thứ tự **bắt buộc**: `DEBT-043` → `DEBT-042` → `DEBT-044`.

| # | Việc | Vì sao thứ tự này |
|---|---|---|
| **3.1** | Chủ dự án **chỉ rõ công thức khổ trải đúng** | Hai công thức khác nhau ⇒ một cái đang tính sai tiền. **Agent KHÔNG tự quyết nghiệp vụ** (§D1) |
| **3.2** | **Hợp nhất về một nguồn** — đưa công thức ra bảng blueprint thay vì viết cứng 2 nơi | Đúng thiết kế tài liệu |
| **3.3** | **Nạp dữ liệu setting thật** — blueprint · bảng giá công đoạn | Không có dữ liệu thì GĐ2 vẫn chặn dù mã đúng |
| **3.4** | **Sửa bộ kiểm thử đọc dữ liệu thật**; nối cột `intentional_fail` | Test giả không chứng minh được gì |

**Cổng ra:** một công thức duy nhất trong mã · test đọc dữ liệu thật · **đối chiếu ít nhất 3 đơn hàng thật** cho ra đúng số tiền được xác nhận *(`DB_PROVEN`)*.

## VIII.7 CHẶNG 4 — DỮ LIỆU & TÀI LIỆU 🟠

| # | Việc | Nợ |
|---|---|---|
| **4.1** | **`DEBT-039` làm TRƯỚC** — lệch độ rộng cột | có thể **cắt cụt dữ liệu khi ghi** |
| **4.2** | **Hồi tố hợp thức hoá đợt nạp 22/08** — ghi ngoại lệ có lý do. **KHÔNG hoàn tác** *(lỗi 0%, dữ liệu đúng)* | `DEBT-069` |
| **4.3** | **Chốt ngưỡng lỗi cho đợt sau** — đề xuất **2%** | `DEBT-069` |
| **4.4** | **Phân bổ người phụ trách cho gần như toàn bộ khách hàng** — chỉ chủ dự án làm được | `DEBT-060` |
| **4.5** | **Thêm cột người phụ trách cho nhà cung cấp** — migration nhỏ, nullable | `DEBT-061` |
| **4.6** | **Hạ hạn `DEBT-036/037`** xuống *"trước đợt nạp kế tiếp"* | `DEBT-077` |
| **4.7** | **Gắn nhãn hiệu lực** cho hai lớp tài liệu chồng nhau | `DEBT-029` · `DEBT-038` |

## VIII.8 CHẶNG 5 — SẢN PHẨM 🟢

| Thứ tự | Việc `#141` | Vì sao đặt ở đây |
|---|---|---|
| **1** | **`6`** — `/m0/quy-trinh` có công dụng gì | Rẻ nhất, **có thể liên quan cơ chế phân quyền** ⇒ biết trước thì việc sau đỡ làm lại |
| **2** | **`2`** — quyền Trưởng phòng Sản xuất | Mở khoá `DEBT-100`. Cần thiết kế **quy trình sản xuất** trong CSDL trước |
| **3** | **`1` + `3`** — nghiệp vụ đơn hàng + quyền giá vốn | Cùng lớp nghiệp vụ + phân quyền ⇒ đỡ đụng lại `actions.ts` hai lần |
| **4** | **`4`** — giao diện `/m0/security` dạng tab | **Tốn nhất.** Bắt buộc chốt 57 tiêu chí + đọc toàn phần chuẩn UI **TRƯỚC** |
| **5** | **`5`** — `/bieu-mau` → `/m1` hoặc `/mf` | Phụ thuộc kết quả việc `4` về cấu trúc menu |
| **6** | **`7`** — Đợt 6 → **deploy** | Chỉ sau khi 1–5 có bằng chứng đóng |

**Ràng buộc deploy** *(chốt sổ `#144` mục f)* — **cấm báo "xong" khi mới sửa local**:

```
so sánh local ↔ GitHub ↔ máy vận hành → bảo toàn khác biệt → test → commit
→ push → backup → deploy → smoke → so lại → cập nhật registry
→ báo cáo công khai → ghi sổ → bàn giao Notion
```

## VIII.9 CHẶNG 6 — DỌN & BÀN GIAO NOTION ⚪

| # | Việc | Nợ |
|---|---|---|
| **6.1** | **Ghi bù `notion_sync_state` cho 133 mục** — **MỘT phiên, theo lô, giữ mốc thật** | — |
| **6.2** | **Tạo bản sổ công khai đã lọc** để Agent Notion tra thẳng | — |
| **6.3** | **Viết gói bàn giao cho các đợt chưa có** | — |
| **6.4** | **Đóng mã nợ trùng** — giữ hậu tố `-B`, **KHÔNG đánh số lại** *(phá tham chiếu chéo)* | `DEBT-082` · `048` · `031` |
| **6.5** | **Gom việc nhỏ một lượt** | `DEBT-047` · `049` · `050` · `090` · `026` |
| **6.6** | **Báo cáo audit 3 công cụ** *(graphify · speckit · context7)* — **chưa ai phủ**, đo được `grep` = 0·0·0 trong gói `#152` | — |

**Quy ước ghi bù `notion_sync_state`** *(cần chốt)*:

| Loại mục | Giá trị |
|---|---|
| Thuần kỹ thuật nội bộ — cổng, test, refactor, dọn tệp | `NOT_APPLICABLE` |
| Có quyết định nghiệp vụ · policy · phân quyền · workflow · cấu trúc dữ liệu | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| Đã có gói bàn giao đủ 6 trường | `HANDOFF_PACKAGED` |
| Notion và quyết định mới nói **khác nhau** về cùng một điều | `SYNC_CONFLICT` → **DỪNG, báo Owner** |
| — | ~~`SYNCED_TO_NOTION`~~ 🚫 **Agent IDE KHÔNG tự ghi** — chỉ TanPhatAI xác nhận |

## VIII.10 Bảng tổng

| Chặng | Nội dung | Phiên đề xuất | Cần Owner | Chặn chặng sau |
|---|---|---|---|---|
| **0** | Đóng băng & chốt sự thật | Owner + điều phối | ✅ **Q-P0** | ✅ chặn tất cả |
| **1** | Vá hạ tầng sổ sách | phiên điều phối | — | ✅ chặn 2–6 |
| **2** | An toàn dữ liệu 🔴 | 1 phiên chuyên | ✅ Q-A1..A4 | — |
| **3** | Tiền & tính giá 🟠 | 1 phiên chuyên | ✅ công thức | chặn GĐ2 |
| **4** | Dữ liệu & tài liệu 🟠 | 1 phiên | ✅ ngưỡng + phân bổ | — |
| **5** | Sản phẩm `#141` → deploy 🟢 | 1–2 phiên | ✅ 57 tiêu chí | — |
| **6** | Dọn & bàn giao Notion ⚪ | 1 phiên | ✅ Q-B1 · Q-D2 | — |

---

# PHẦN IX — BẢNG QUYẾT ĐỊNH DUY NHẤT

> **13 câu hỏi từ 7 phiên, gom về một chỗ. Trả lời một lượt — mọi phiên gỡ chặn.**

| Mã | Phiên chờ | Câu hỏi | Đề xuất | Chặng |
|---|---|---|---|---|
| **Q-P0** | *(bản này)* | Phiên nào làm **điều phối**? | **`cdc70c`** — đang giữ gói `#152` | 0 |
| **Q-A1** | `cdc70c` | Nhóm A: **A1** hay **A2**? | **A1, GẠCH BƯỚC 1** *(đã xong 23/08)* | 2 |
| **Q-A2** | `cdc70c` | 5 tệp biểu mẫu — giữ hay gỡ? | **GIỮ 3 tệp** + khai vào bản đồ thư mục. **MỞ KIỂM 2 tệp nghi dữ liệu thật TRƯỚC** | 2 |
| **Q-A3** | `cdc70c` | **Viết lại lịch sử git**? | ⛔ **KHÔNG.** (a) hỏng mọi bản sao · (b) **10 phiên đang chạy chung cây làm việc** · (c) tiền lệ 19/08 đã chốt *"không viết lại"*. Thay bằng **gỡ khỏi HEAD** + **xác nhận kho ở chế độ riêng tư** | 2 |
| **Q-A4** | `cdc70c` | `DEBT-016` cùng việc `DEBT-051`? | **Cần xác nhận** — agent **không tự đóng nợ bảo mật** *(đã đóng sai 2 lần trước đây)* | 2 |
| **Q-B1** | `ccc761` | Quy ước ghi bù cho **133 mục** cũ? | **Theo bảng §VIII.9**, một phiên làm | 6 |
| **Q-B2** | `885cf7` · `0f63d1` | Chọn **A / B / C**? | **A cho cả hai** — chờ Chặng 1, rồi **một phiên** gộp | 6 |
| **Q-C1** | `07f81b` | Xoá 3 nhánh cũ? | ✅ **XOÁ** — đã đo **0 commit lệch, 0 tệp khác biệt** | 1 |
| **Q-C2** | `07f81b` | Cần **cơ chế cấp số nguyên tử**? | ✅ **CÓ** — Chặng 1 việc 1.1 | 1 |
| **Q-C3** | `07f81b` | Siết cổng khai `notion_sync_state`? | ✅ **CÓ, nhưng SAU `DEBT-103`** — siết trước sẽ **đỏ oan** | 1 |
| **Q-D1** | `5d9e2c` · `44ea19` | Đóng mốc cổng đếm 386 → 399? | ✅ **CÓ** — 1 lệnh | 1 |
| **Q-D2** | `ad4081` | Tạo **bản sổ công khai** cho Notion? | ✅ **CÓ** — đúng tinh thần §F1c *"kênh vận chuyển"* | 6 |
| **Q-E1** | `5d9e2c` *(chờ từ 23/08)* | Dòng *"Graphify trước"* — đảo hay giữ quyết định 30/07? | **GIỮ.** Kỹ năng đó `content_status = DORMANT`, **cấm tự kích hoạt** (§G7.15) | 6 |

**Ngoài 13 dòng trên — 1 quyết định NGHIỆP VỤ chỉ chủ dự án làm được:**

| Mã | Câu hỏi | Vì sao |
|---|---|---|
| **Q-TIỀN** | **Công thức khổ trải nào đúng?** | Đây là **nghiệp vụ in ấn**, không phải kỹ thuật. §D1 cấm agent tự hoàn thiện business rule. **Đang chặn GĐ2 tính giá** |

---

# PHẦN X — ĐIỀU CHƯA CHỨNG MINH ĐƯỢC

> §F1c mục 4 **bắt buộc** nêu phần này, để Notion không nhận nhầm thành sự thật hiện hành rồi phản bác về sau.

| # | Điều chưa chứng minh | Vì sao | Ai xác minh được |
|---|---|---|---|
| 1 | **10 phiên kia còn chạy hay đã đóng** | Không đọc được trạng thái sống của phiên khác | **Owner** — xem cửa sổ chat còn lại |
| 2 | **Kho mã đang công khai hay riêng tư** | Chưa gọi API. **Ảnh hưởng mức gấp của Q-A3** | Owner / phiên được cấp quyền |
| 3 | **2 tệp bảng tính** có PII thật hay chỉ biểu mẫu trống | Chưa mở nội dung | Owner / phiên được cho phép |
| 4 | **Công thức khổ trải nào đúng** | Quyết định **nghiệp vụ** | **Chỉ Owner** |
| 5 | **Trạng thái runtime máy vận hành** | Không có kênh đo được cấp ⇒ `UNVERIFIED` | Phiên có quyền đo |
| 6 | **Đợt nạp 22/08 có thật lỗi 0%** | Lấy từ báo cáo phiên khác, chưa tự đo lại | Phiên chạy lại đối chứng |
| 7 | **`#141` việc `1` đã thi hành tới đâu** | Sổ ghi *"ĐANG LÀM"* nhưng phiên giữ việc đã dừng; chưa đo mã nguồn | Phiên tiếp quản Chặng 5 |

---

# PHẦN XI — TỰ ĐÍNH CHÍNH

> `GOV-FAILURE-RECORD-001` §G7.4 — ghi nhận trung thực, không làm mềm.

## XI.1 Agent mắc đúng lỗi vừa tố cáo

Khi ghi `DEBT-103` *(nợ về vỡ cột bảng)*, **chính dòng nợ đó ra 12 ô thay vì 8** — vì mô tả chứa ký tự ngăn cột chưa thoát. Đo được bằng script đếm ô ngay sau khi ghi.

**Đã xử lý:** viết lại dòng không dùng ký tự ngăn cột trong mô tả · **kiểm số ô TRƯỚC khi ghi** *(tự chặn nếu sai)* · ghi rõ sự việc vào chính nội dung `DEBT-103`, **không xoá, không giấu**.

**Bài học đưa vào kế hoạch:** cổng `test:ledger-shape` ở Chặng 1 việc **1.3** phải kiểm **CẢ HAI sổ**. Nếu cổng đó đã tồn tại, lỗi này bị bắt ngay lúc ghi.

## XI.2 Registry ghi local DOWN — đo lại là UP

Dòng registry do phiên khác ghi sáng nay là **đúng lúc họ đo**. Phép đo 15:05 cho kết quả khác. **Không phải ai sai** — đây là **đúng cùng cơ chế L2** *(không có mốc sự thật chung)* mà báo cáo này đang chẩn đoán. Đã ghi khối đo mới, **giữ nguyên văn dòng cũ**.

---

*Agent IDE (Claude Code · phiên `b70460`) · 25/08/2026 · `GOV-COMPLETION-REPORT-001` · `GOV-NOTION-HANDOFF-001` · `GOV-SESSION-DECISION-001` · `GOV-ONE-PLAN-OF-RECORD-001` · `GOV-EDIT-PRESERVE-001` · `GOV-FAILURE-RECORD-001` · `GOV-PUBLIC-SAFE-001`*
