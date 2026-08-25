# KẾ HOẠCH HỢP NHẤT 10 PHIÊN CLAUDE CODE — 25/08/2026 *(bản công khai đã lọc)*

> **Bản này đã lọc theo `GOV-PUBLIC-SAFE-001`.** So với bản nội bộ `docs/reports/KE-HOACH-HOP-NHAT-10-PHIEN-20260825.md` (kho riêng tư), bản công khai đã **bỏ**: nguyên văn lời Owner · đường dẫn tệp dữ liệu khách hàng · nội dung nhạy cảm. **Giữ nguyên**: kết luận · số đo · mã nợ · mã commit · tên cổng/module · bảng quyết định · phần *"điều chưa chứng minh được"*.
>
> **Mã mục sổ Owner:** `#153` · **Lớp bằng chứng:** `FILE_PROVEN` + `CODE_PROVEN`
>
> **Quan hệ với kế hoạch đang có:** **KHÔNG** tạo kế hoạch cạnh tranh (`GOV-ONE-PLAN-OF-RECORD-001` §E3). Gói quyết định `#152` (6 nhóm A–F) **vẫn là PLAN OF RECORD**. Bản này bọc ngoài nó: thêm lớp điều phối liên phiên + đính chính **đúng 1 tiền đề đã lỗi thời** trong nhóm A.

---

## 0. TÓM TẮT

**Ba điều Owner hỏi — đều đã ổn:**

| Câu hỏi | Sự thật đo được lúc 14:45 ngày 25/08 |
|---|---|
| Đã push hết chưa? | **RỒI, cả hai kho.** Kho mã `a7db8a7` · kho công khai `ea5f7ec` · cả hai `0 ahead / 0 behind` · cây làm việc sạch |
| Tương tác trực tiếp đã thành bảng chưa? | **RỒI.** Sổ Yêu Cầu Owner — **152 mục** (nay 153), không trùng mã, cổng `test:ledger-dup-id` PASS 5/5 |
| Có cần thêm luật vào bộ 5 tệp quản trị? | **KHÔNG.** §F1b (20/08) + §F1c (25/08) đã phủ đúng. Doc `2.8`, 5 tệp byte-identical (`d100fae2`), 16/16 cổng PASS |

**Vấn đề thật KHÔNG nằm ở luật, mà ở ĐIỀU PHỐI:**

10 phiên chạy song song. Lúc 14:10–14:14 cùng một câu hỏi được gửi cho 9 phiên. Trong 30 phút sau đó kho mã đi qua **4 mốc** (`d9e823a` → `27fda56` → `93605dd` → `a7db8a7`), kho công khai đi qua **6 mốc**. Kết quả: **mỗi phiên trả lời đúng phần của mình, không phiên nào đúng cho toàn cục.** Không phiên nào báo sai.

---

## 1. BẢN ĐỒ 10 PHIÊN

Nguồn: đọc trực tiếp tệp nhật ký phiên.

| # | Phiên | Chủ đề gốc | Bắt đầu | Commit | Push | Trạng thái cuối |
|---|---|---|---|---:|---:|---|
| 1 | `0f63d1` | Phiên gốc lớn nhất (20.311 lượt) | 09/08 | 211 | 140 | Báo 3 việc còn sót |
| 2 | `cdc70c` | Audit công cụ AI | 30/07 | 40 | 28 | Đã push gói quyết định 6 nhóm (`#152`) |
| 3 | `5d9e2c` | Điều tra + soát luật — nguồn sinh §G7 | 18/08 | 41 | 26 | Đính chính ngữ cảnh cũ 2 ngày |
| 4 | `ccc761` | UI không lặp vòng | 18/08 | 6 | 6 | Tự khai lỗi ghi trùng, đã gỡ |
| 5 | `44ea19` | Pha C — đóng sổ Pha B | 23/08 | 18 | 11 | Ban hành §F1c · ghi bù 3 mục |
| 6 | `07f81b` | Đợt 4 — cổng duyệt giá M3 | 21/08 | 7 | 7 | Vá điểm mù cổng enum |
| 7 | `5d184f` | Audit FK cột người | 20/08 | 1 | 1 | Push 3 báo cáo rút gọn |
| 8 | `ad4081` | CSDL / môi trường local | 09/08 | 7 | 4 | Chờ Owner 2 việc |
| 9 | `885cf7` | Governance | 23/08 | 1 | 1 | **BLOCKED — tự dừng, không push** |
| 10 | `b70460` | Phiên hợp nhất (bản này) | 25/08 | 1 | 1 | Đã lập kế hoạch |

**Ba hành vi đúng cần ghi nhận:** `885cf7` tự dừng và từ chối commit khi phát hiện phiên khác đang ghi · `ccc761` tự khai lỗi ghi trùng của chính mình và gỡ ra · `5d9e2c` tự đính chính ngữ cảnh lỗi thời 2 ngày.

---

## 2. BA MÂU THUẪN ĐO ĐƯỢC

### 2.1 Gói `#152` chứa một tiền đề đã lỗi thời — `DEBT-104`

Nhóm A bước 1 đề xuất *"vá cổng `pii-scan` trước — thêm cờ `-z`"*. **Việc này đã hoàn tất 23/08/2026 lúc 14:36**, commit `1f5e379`.

Nguyên nhân đo sai: phiên đó đếm chuỗi `-z` **trong tệp cổng** và ra `0`. Nhưng bản vá đã được rút ra tệp dùng chung `scripts/tests/lib/tracked-files.mjs`, nơi **4 cổng** cùng gọi: `pii-scan` · `secret-scan` · `path-audit` · `script-parse`.

**Bằng chứng phản chứng:** cổng in ra được cả **7 đường dẫn có dấu tiếng Việt và emoji**. Một cổng còn mù không thể làm vậy.

> **Rủi ro:** duyệt A1 nguyên văn ⇒ một phiên đi "vá" cổng đã vá, có thể làm hỏng ngược bản vá đang tốt. **Bước 1 phải gạch bỏ; 4 bước còn lại vẫn đúng nguyên.**

**Phần cảnh báo còn lại vẫn ĐÚNG:** tệp dữ liệu khách hàng/nhà cung cấp **vẫn bị git theo dõi** (`DEBT-066-B`). Cổng nay xanh vì vi phạm đã được ghi nhận là **nợ đã biết**, **không phải vì đã dọn**.

### 2.2 Chín phiên báo chín trạng thái kho — cả chín đều đúng lúc đo

| Phiên báo | HEAD kho mã | HEAD kho công khai |
|---|---|---|
| `ccc761` · `44ea19` | `d9e823a` | `f222599` / `e842f53` |
| `07f81b` | `27fda56` + `93605dd` | `671e401` |
| `cdc70c` | `a7db8a7` | `1188dcb` |
| `5d184f` | (không đụng) | `ea5f7ec` |
| **Chốt 14:45** | **`a7db8a7`** | **`ea5f7ec`** |

### 2.3 Sổ Owner vỡ cột bảng — `DEBT-103`

Header bảng chính khai **3 cột**. Trong 39 hàng dữ liệu:

| Số ô | Số hàng | Hệ quả |
|---:|---:|---|
| 3 (đúng) | 19 | — |
| 2 | 1 | thiếu 1 trường |
| **4** | **17** | 1 trường bị nuốt |
| **9** | **2** | **6 trường bị nuốt** |

Cộng thêm **22 dòng trống** nằm giữa các hàng, cắt mạch bảng Markdown.

Trong các ô bị nuốt có `decision_state` và **`notion_sync_state`** — đúng hai trường mà `GOV-NOTION-HANDOFF-001` §F1c dựng ra cho Agent Notion đọc. **Đây là cơ chế vật lý của việc "Agent Notion bỡ ngỡ".** Chưa có cổng nào kiểm hình dạng bảng.

---

## 3. CHẨN ĐOÁN GỐC

**Không thiếu luật** — đối chiếu từng điều:

| Điều cần | Luật đã có | Ban hành |
|---|---|---|
| Code được đi trước tài liệu | `GOV-SESSION-DECISION-001` §F1b mục 4 | 20/08 |
| Ghi sổ là quyền mặc định | §F1b mục 3 | 20/08 |
| Sổ là kênh vận chuyển về Notion | `GOV-NOTION-HANDOFF-001` §F1c mục 1 | 25/08 |
| Mỗi chỉ thị = một mục | §F1c mục 2 | 25/08 |
| `notion_sync_state` có bảng 5 giá trị | §F1c mục 3 + §F3 trục 6 | 25/08 |
| Bắt buộc có gói bàn giao | §F1c mục 4 | 25/08 |
| Ghi bù giữ mốc thật | §F1c mục 5 | 25/08 |

**Gốc thật — ba lỗ điều phối:**

| Lỗ | Biểu hiện đo được | Nợ |
|---|---|---|
| **L1** Không có cơ chế cấp số | Hai phiên cùng đọc *"số lớn nhất là #147"* rồi cùng ghi `#148`. Cổng bắt **SAU KHI** ghi, không ngăn **TRƯỚC KHI** ghi | `DEBT-102` |
| **L2** Không có mốc sự thật chung | 9 phiên đo 9 lúc ⇒ 9 câu trả lời khác nhau | vá ở Pha 0 |
| **L3** Không có hàng đợi câu hỏi | 7 phiên giữ 13 câu hỏi, rải 7 cửa sổ chat | vá ở §5 |

> §E3 `GOV-ONE-PLAN-OF-RECORD-001` **đã cấm** kế hoạch song song, nhưng không có **cơ chế** thi hành xuyên phiên. Luật đúng, thi hành trống.

---

## 4. KẾ HOẠCH 4 PHA

### PHA 0 — ĐÓNG BĂNG & CHỐT SỰ THẬT

1. Owner chỉ định **MỘT phiên điều phối**; 9 phiên còn lại chuyển **CHỈ ĐỌC** cho tới khi Pha 1 xong.
2. **Chốt mốc sự thật 25/08 14:45** — nguồn duy nhất, mọi phiên đọc lại từ đây:

| Hạng mục | Giá trị chốt |
|---|---|
| Kho mã | `a7db8a7` · sạch · `0/0` |
| Kho công khai | `ea5f7ec` · sạch · `0/0` · 91 tệp |
| Sổ Yêu Cầu Owner | 152 mục · mã kế tiếp `#153` |
| Sổ nợ kỹ thuật | 108 mục · mã kế tiếp `DEBT-103` |
| 5 tệp luật | byte-identical `d100fae2` · Doc `2.8` |
| Cổng quản trị | **16/16 PASS · EXIT=0** |
| Phiên bản | `V1.00.355` |
| Điều khoản luật | 399 *(mốc cổng đang 386 — cần đóng lại)* |
| Nhánh cũ | 3 nhánh, cả 3 có **0 commit lệch `main`** |

3. Gộp 13 câu hỏi của 7 phiên thành 1 bảng (§5).

### PHA 1 — VÁ CƠ CHẾ, CHẶN TÁI DIỄN

| Việc | Nợ | Cách làm | Bằng chứng bắt buộc |
|---|---|---|---|
| **1.1** Cấp số nguyên tử cho sổ | `DEBT-102` | Lệnh `oil:new`: đọc lại từ đĩa → chụp `mtime` → lấy mã kế tiếp từ cổng → ghi → **so lại `mtime`, đổi thì HUỶ** | Kiểm ngược: 2 tiến trình ghi cùng lúc → đúng 1 thành công |
| **1.2** Vá hàng vỡ cột | `DEBT-103` | Đưa mọi hàng về đúng số cột; trường bị nuốt chuyển thành khối chú thích dưới bảng, **giữ nguyên văn** (`GOV-EDIT-PRESERVE-001`) | Cổng đếm ô: mọi hàng = số cột header |
| **1.3** Cổng chống vỡ cột | — | `test:ledger-shape` — FAIL khi số ô khác header | Kiểm ngược: thêm ô thừa → đỏ; gỡ → xanh |
| **1.4** Đóng lại mốc cổng đếm điều khoản | — | 386 → 399 | `test:clause-count` PASS mốc mới |
| **1.5** Xoá 3 nhánh cũ | — | Đã đo: 0 commit lệch, 0 tệp khác biệt | `git rev-list --count main..<nhánh>` = 0 |

### PHA 2 — 6 NHÓM QUYẾT ĐỊNH

Giữ nguyên cấu trúc A–F và thứ tự **A → C → B → E → D → F** của gói `#152`. **Đúng một sửa đổi:** gạch bỏ bước A1.1 (đã xong 23/08). Nâng `DEBT-103` từ nhóm D lên **Pha 1**, vì nó đang âm thầm giấu chính trường mà Notion đọc.

### PHA 3 — VIỆC SẢN PHẨM ĐANG TREO

Phần bị bỏ quên trong lúc 10 phiên bận rà sổ — **OIL `#141`** (24/08), trạng thái **ĐANG LÀM**, điều tra bị dừng:

| Việc | Trạng thái | Chặn bởi |
|---|---|---|
| **1.** Đơn hàng: huỷ chỉ admin/CEO · đóng đơn theo trạng thái · theo dõi tới khi thu xong tiền | Quyết định đã khoá, thi hành chưa xong | — |
| **2.** Đề xuất quyền Trưởng phòng Sản xuất | Chưa có | `DEBT-100` — CSDL không có quy trình sản xuất |
| **3.** Quyền xem giá vốn — hợp nhất code cũ | Chưa hợp nhất | — |
| **4.** `/m0/security` → giao diện dạng tab kiểu `/m6`, `/m7`, có từng bước | Chưa làm | Cần bộ 57 tiêu chí nghiệm thu UI (§G7.1) |
| **5.** `/bieu-mau` → đưa vào `/m1` hoặc `/mf` | Chưa làm | — |
| **6.** `/m0/quy-trinh` có công dụng gì | Chưa làm | — |
| **7.** Đợt 6 → deploy | Chưa tới | Phụ thuộc 1–6 |

**Thứ tự đề xuất:** `6` → `2` → `1`+`3` → `4` → `5` → `7`.

> Việc `4` là việc **giao diện** — `GOV-ACCEPTANCE-FIRST-001` §G7.1 **bắt buộc** chốt bảng 57 tiêu chí với Owner **TRƯỚC khi sửa dòng mã đầu tiên**. Đã có 9 lượt bị bác hồi 17–18/08 vì bỏ bước này.

### PHA 4 — BÀN GIAO NOTION

**Đo được:** chỉ **19/152 mục (12,5%)** có trường `notion_sync_state`; **133 mục** chưa có.

| Loại mục | Giá trị đề xuất |
|---|---|
| Thuần kỹ thuật nội bộ (cổng, test, refactor, dọn tệp) | `NOT_APPLICABLE` |
| Có quyết định nghiệp vụ · policy · phân quyền · workflow · cấu trúc dữ liệu | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| Đã có gói bàn giao đủ trường | `HANDOFF_PACKAGED` |
| — | ~~`SYNCED_TO_NOTION`~~ — **Agent IDE không tự ghi**, chỉ TanPhatAI xác nhận (§F1c mục 3) |

**Cách làm:** MỘT phiên duy nhất, ghi bù theo lô, **giữ mốc thật**, gắn nhãn GHI BÙ. Không rải cho nhiều phiên — đó chính là cách sinh ra `DEBT-102`.

---

## 5. BẢNG QUYẾT ĐỊNH GỘP — 13 CÂU HỎI TỪ 7 PHIÊN

| Mã | Phiên chờ | Câu hỏi | Đề xuất |
|---|---|---|---|
| **Q-P0** | — | Phiên nào làm **điều phối**? | `cdc70c` — đang giữ gói `#152` |
| **Q-A1** | `cdc70c` | Nhóm A: A1 hay A2? | **A1, GẠCH BƯỚC 1** (đã xong 23/08) |
| **Q-A2** | `cdc70c` | 5 tệp biểu mẫu — giữ hay gỡ? | **GIỮ** + khai vào bản đồ thư mục. **Nhưng** 2 tệp tên gợi ý dữ liệu vận hành thật ⇒ **mở kiểm nội dung trước** |
| **Q-A3** | `cdc70c` | Viết lại lịch sử git? | **KHÔNG** — hỏng mọi bản sao, và 10 phiên đang chạy chung cây làm việc. Thay bằng gỡ khỏi HEAD + xác nhận kho ở chế độ riêng tư |
| **Q-A4** | `cdc70c` | `DEBT-016` trùng việc `DEBT-051`? | **Cần Owner xác nhận** — Agent không tự đóng nợ bảo mật |
| **Q-B1** | `ccc761` | Quy ước ghi bù cho 133 mục cũ? | Theo bảng Pha 4; một phiên làm, theo lô |
| **Q-B2** | `885cf7` · `0f63d1` | Chọn A / B / C? | **A cho cả hai** — chờ Pha 1, rồi một phiên gộp |
| **Q-C1** | `07f81b` | Xoá 3 nhánh cũ? | **XOÁ** — đã đo 0 commit lệch, 0 tệp khác biệt |
| **Q-C2** | `07f81b` | Cần cơ chế cấp số nguyên tử? | **CÓ** — Pha 1 việc 1.1 |
| **Q-C3** | `07f81b` | Siết cổng bắt buộc khai `notion_sync_state`? | **CÓ, nhưng SAU `DEBT-103`** — siết trước sẽ đỏ oan ở đúng 2 mục đang bị nuốt trường |
| **Q-D1** | `5d9e2c` · `44ea19` | Đóng lại mốc cổng đếm 386 → 399? | **CÓ** — 1 lệnh |
| **Q-D2** | `ad4081` | Tạo bản sổ Owner công khai cho Notion tra thẳng? | **CÓ** — đúng tinh thần §F1c, lọc theo `GOV-PUBLIC-SAFE-001` |
| **Q-E1** | `5d9e2c` | Dòng *"Graphify trước"* trong danh mục kỹ năng — đảo hay giữ quyết định 30/07? | **GIỮ** — kỹ năng đó `content_status = DORMANT`, cấm tự kích hoạt (§G7.15) |

---

## 6. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC — CẤM NHẬN NHẦM THÀNH SỰ THẬT

| # | Điều chưa chứng minh | Vì sao | Ai xác minh được |
|---|---|---|---|
| 1 | 10 phiên kia còn chạy hay đã đóng | Không đọc được trạng thái sống của phiên khác | **Owner** |
| 2 | Kho mã đang công khai hay riêng tư | Chưa gọi API; ảnh hưởng mức gấp của Q-A3 | Owner / phiên được cấp quyền |
| 3 | 2 tệp bảng tính có PII thật hay chỉ biểu mẫu trống | Chưa mở nội dung tệp | Owner / phiên được cho phép |
| 4 | Công thức khổ trải nào đúng (`DEBT-043`) | Quyết định **nghiệp vụ**, Agent không tự quyết | **Chỉ Owner** |
| 5 | Trạng thái runtime máy vận hành | Phiên này không đo production | Phiên có quyền đo |
| 6 | Đợt nạp 22/08 có thật sự lỗi 0% | Lấy từ báo cáo phiên khác, chưa tự đo lại | Phiên chạy lại đối chứng |

---

## 7. ĐIỀU AGENT KHÔNG LÀM

Không sửa `src/` · không đụng CSDL · không deploy · không bump version · không viết lại lịch sử git · không tự vá `DEBT-102`/`DEBT-103` *(là thi hành, chờ Owner chốt Q-P0 để đúng một phiên làm)* · không ghi đè việc phiên khác · không tự ghi `SYNCED_TO_NOTION` · không tự đóng `DEBT-016`.

**Cơ chế chống `DEBT-102` đã tự áp dụng khi ghi sổ:** chụp `mtime` → soạn → so lại `mtime` → mới ghi. Kết quả commit: **+306 dòng, 0 dòng bị xoá.**

---

*Agent IDE (Claude Code · phiên `b70460`) · 25/08/2026 · `GOV-COMPLETION-REPORT-001` · `GOV-NOTION-HANDOFF-001` · `GOV-SESSION-DECISION-001` · `GOV-ONE-PLAN-OF-RECORD-001` · `GOV-PUBLIC-SAFE-001`*
