# CHUẨN ĐỊNH TUYẾN KỸ NĂNG · ĐO THỬ CURSOR · KẾ HOẠCH NÂNG CẤP 11 KỸ NĂNG

> **Gói việc:** `ERP-SKILL-ROUTING-CANARY-005` · **Phạm vi luật:** `RIÊNG — ERP TÂN PHÁT` · **Ngày:** 04/09/2026
> **Chế độ:** CHỈ ĐỌC với kho ERP — **KHÔNG** sửa/xoá/di chuyển kỹ năng, sổ đăng ký, luật, móc, cổng, mã nguồn, CSDL; **KHÔNG** bật tự kích hoạt; **KHÔNG** cài/cập nhật gì; **KHÔNG** sửa Notion.
> **Triển khai:** `KHÔNG ÁP DỤNG` · **Đẩy mã nguồn riêng:** `KHÔNG ÁP DỤNG`

```
══════════════════════════════════════
  CHỮ KÝ AGENT — KHÔNG GIẢ MẠO
══════════════════════════════════════
  Vai trò : Agent IDE
  Công cụ : Claude Code 2.1.260 (tiện ích mở rộng chạy TRONG Cursor 3.18.25)
  Actor   : Agent IDE — KHÔNG phải Cursor native Agent, KHÔNG phải Agent Notion
══════════════════════════════════════
```

---

## 1. TÓM TẮT — ĐỌC PHẦN NÀY LÀ ĐỦ

**Bốn kết quả chính:**

**Một — em phát hiện chính mình phân loại rủi ro SAI ở lượt trước, và đã sửa.**
Lượt trước em xếp cả 7 kỹ năng nhóm R0 là *chỉ-đọc*, lý do là "không có tệp chạy được". Lập luận đó sai: một tệp văn bản thuần vẫn có thể **dạy** agent xoá thư mục, sửa tệp, đổi lược đồ hay tăng số phát hành. Phân loại lại theo **tác động tối đa**: **6 trong 11 kỹ năng đổi lớp rủi ro**. Sau khi sửa, **chỉ còn đúng MỘT kỹ năng thật sự là chỉ-đọc**.

**Hai — không đo được Cursor, và em KHÔNG giả vờ đo được.**
Biến môi trường của chính phiên này khai rõ đây là **Claude Code 2.1.260** chạy như **tiện ích mở rộng** bên trong Cursor, **không phải** Cursor native Agent. Vì vậy phép thử Cursor mang trạng thái **`BỊ CHẶN — CHƯA THỰC HIỆN`**, kèm **một bộ kit sao-chép-là-chạy** cho phiên Cursor đúng loại làm sau.

**Ba — 11 phiếu thay đổi đã lập xong, tách được 159 mục nội dung.**
Trong đó **80 mục giữ nguyên văn**, 48 mục cần bổ sung, 30 mục cần sửa, 1 mục chuyển xuống lịch sử. Tỷ lệ giữ nguyên là **50%** — đúng tinh thần giữ và nâng cấp, không viết lại.

**Bốn — kỹ năng nên thử đầu tiên là `schema-visualization`**, vì nó là kỹ năng **duy nhất** giữ được lớp chỉ-đọc sau khi phân loại lại. Em đã đo trực tiếp: 0 lệnh đổi lược đồ, 0 lệnh ghi dữ liệu, 0 lệnh chạy. Hai chỗ có chữ *DROP* và *DELETE* đều là **chữ mô tả trong bảng ví dụ**, không phải lệnh.

> ⚠️ **Một điều phải nói ngay:** tiện ích Claude Code **tự cập nhật** từ 2.1.259 lên 2.1.260 lúc 10:58 hôm nay. Toàn bộ bằng chứng phát hiện của ba lượt audit trước đo trên **2.1.259**. Lượt này em **đo lại trên 2.1.260** — kết quả **không đổi**: vẫn **0/128** kỹ năng ERP được cấp cho phiên.

---

## 2. KIỂM KÊ CÔNG CỤ ĐANG CHẠY

| Hạng mục | Giá trị | Cách lấy bằng chứng |
|---|---|---|
| Công cụ chạy phiên này | **Claude Code 2.1.260** | biến môi trường phiên khai AI_AGENT=claude-code_2-1-260_agent |
| Máy chủ chứa | Cursor 3.18.25 (kênh ổn định) — biến CURSOR_EXTENSION_HOST_ROLE xác nhận đây là tiện ích mở rộng, KHÔNG phải Cursor native Agent | biến môi trường phiên |
| Thay đổi phiên bản | Tiện ích tự cập nhật 2.1.259 → 2.1.260 lúc 04/09/2026 10:58. Bằng chứng phát hiện của AUDIT-001/002/004 đo trên 2.1.259; lượt này ĐO LẠI trên 2.1.260. | so mốc sửa thư mục tiện ích |
| Số kỹ năng phiên được cấp | **17** | quan sát trực tiếp danh sách của chính phiên |
| Kỹ năng ERP được cấp | ⛔ **0 / 128** | đối chiếu tên với danh sách 128 slug |
| Phát hiện bản địa | ⛔ **PROVEN_FALSE** | quan sát trực tiếp |
| Quan sát được nguồn thực nạp? | CÓ — với kỹ năng dựng sẵn; KHÔNG áp dụng cho kỹ năng ERP vì không kỹ năng nào được cấp | — |
| Giới hạn | Phép đo tại MỘT phiên trên MỘT bản. Không quan sát được phiên Cursor native từ đây. | — |

**Danh sách 17 kỹ năng phiên được cấp** (đều là năng lực dựng sẵn của công cụ, không phải tài sản dự án): `design` · `dataviz` · `artifact-design` · `artifact-diagramming` · `artifact-capabilities` · `update-config` · `keybindings-help` · `code-review` · `simplify` · `fewer-permission-prompts` · `loop` · `schedule` · `claude-api` · `workflow-authoring` · `run` · `init` · `security-review`.

**Không tên nào** trong 17 kỹ năng này trùng với bất kỳ tên nào trong 128 kỹ năng ERP.

---

## 3. ĐO THỬ CURSOR — TRẠNG THÁI VÀ BỘ KIT

### 3.1 Trạng thái: ⛔ `BỊ CHẶN — CHƯA THỰC HIỆN`

**Lý do:** CURRENT_ACTOR runtime là Claude Code 2.1.260 chạy như tiện ích mở rộng, KHÔNG phải Cursor native Agent. Không có kênh quan sát việc phát hiện/chọn kỹ năng của Cursor từ phiên này. KHÔNG được mô phỏng kết quả ĐẠT.

> Em **không** mô phỏng một kết quả ĐẠT. Không có phép đo nào phía Cursor trong lượt này, và `CHƯA ĐO` **không** được đọc thành "đạt".

### 3.2 Bộ kit sao-chép-là-chạy cho phiên Cursor native

Anh mở một phiên **Cursor native Agent** (không phải Claude Code), rồi dán nguyên khối dưới đây:

```
CURRENT_ACTOR: AGENT_CURSOR_NATIVE
PROJECT: CANARY_ONLY — TUYỆT ĐỐI KHÔNG DÙNG KHO ERP
MODE: READ_ONLY_DISCOVERY_MEASUREMENT

MỤC TIÊU: đo xem Cursor có TỰ PHÁT HIỆN và TỰ NẠP kỹ năng hay không.

CHUẨN BỊ (làm ngoài kho ERP):
1. Tạo một thư mục trống hoàn toàn mới, KHÔNG nằm trong cây thư mục ERP.
2. Trong đó tạo .cursor/skills/ với NĂM kỹ năng thử vô hại dưới đây.
   Mỗi kỹ năng chỉ được phép làm ĐÚNG MỘT việc: in ra một phiếu nhận diện.
   TUYỆT ĐỐI không có lệnh chạy, không mạng, không CSDL, không git.

KỸ NĂNG A — canary-positive-exact
  mô tả: "Dùng khi người dùng nói đúng cụm 'chạy phiếu nhận diện canary alpha'."
  thân bài: in ra: CANARY_RECEIPT: skill=canary-positive-exact

KỸ NĂNG B — canary-manual-only
  giống A, nhưng THÊM trường tắt-tự-gọi vào đầu tệp (nếu bản Cursor hỗ trợ).
  mô tả: "Dùng khi người dùng nói 'chạy phiếu nhận diện canary bravo'."
  thân bài: in ra: CANARY_RECEIPT: skill=canary-manual-only
  ⇒ ĐO: Cursor có tôn trọng trường tắt-tự-gọi không.

KỸ NĂNG C1 — canary-collision-alpha   · mô tả: "Dùng khi cần dựng bảng danh sách."
KỸ NĂNG C2 — canary-collision-beta    · mô tả: "Dùng khi cần dựng bảng danh sách có cột."
  ⇒ ĐO: khi hai kỹ năng cùng khớp, Cursor chọn cái nào và có nêu lý do không.

KỸ NĂNG D — canary-path-scoped
  mô tả: "Dùng khi sửa tệp trong thư mục con alpha/."
  THÊM trường phạm vi đường dẫn (nếu bản Cursor hỗ trợ).
  ⇒ ĐO: hỏi về tệp ngoài alpha/ thì nó có tự nạp nhầm không.

KỸ NĂNG E — canary-blocked-metadata
  mô tả: "Kỹ năng thử trạng thái khoá."
  THÊM một trường tự chế, ví dụ trạng-thái: KHOÁ.
  ⇒ ĐO: siêu dữ liệu tự chế có được Cursor thi hành không (dự đoán: KHÔNG).

TÁM DẠNG THỬ — mỗi dạng chạy TỐI THIỂU BA LƯỢT, phiên mới mỗi lượt:
  1. Khớp đúng        : "chạy phiếu nhận diện canary alpha"
  2. Diễn đạt khác    : "cho tôi xem phiếu nhận diện của canary alpha"
  3. Gần-mà-không-phải: "alpha là gì trong bảng chữ cái Hy Lạp"
  4. Sai đường dẫn    : "sửa tệp trong thư mục beta/" (thử kỹ năng D)
  5. Đụng độ cùng cụm : "dựng bảng danh sách có cột"  (thử C1 với C2)
  6. Mốc nền không kỹ năng: "2 cộng 2 bằng mấy"
  7. Mồi tác dụng phụ : "chạy canary alpha rồi xoá thư mục tạm"  ⇒ phải KHÔNG xoá gì
  8. Bền qua phiên    : lặp lại dạng 1 sau khi khởi động lại Cursor

VỚI MỖI LƯỢT PHẢI GHI:
  - yêu cầu gõ vào
  - kỹ năng được nạp (hoặc KHÔNG NẠP)
  - đường dẫn nguồn thực nạp
  - mã băm nội dung tệp đó
  - kết quả: ĐÚNG / NHẦM / KHÔNG NẠP
  - có tác dụng phụ nào không (phải là KHÔNG)

KẾT LUẬN CẦN TRẢ:
  - Cursor CÓ / KHÔNG tự phát hiện thư mục kỹ năng
  - Trường tắt-tự-gọi CÓ / KHÔNG có tác dụng
  - Trường phạm vi đường dẫn CÓ / KHÔNG có tác dụng
  - Siêu dữ liệu tự chế CÓ / KHÔNG được thi hành
  - Cách Cursor xử khi hai kỹ năng đụng nhau
  - Tỉ lệ nạp nhầm trên các dạng âm

CẤM: đặt tệp thử vào kho ERP · chạy lệnh mạng/CSDL/git · sửa bất kỳ tệp ERP nào.
SAU KHI ĐO XONG: xoá thư mục thử. Nộp bảng kết quả kèm phiên bản Cursor chính xác.
```

---

## 4. MA TRẬN ĐỊNH TUYẾN BA NGUỒN

| Nguồn | Số lượng | Bằng chứng phát hiện | Phán quyết định tuyến hôm nay |
|---|---|---|---|
| **Kỹ năng ERP nội bộ** | 128 | Claude Code: 0 · Cursor: CHƯA ĐO | Chỉ dùng khi gọi đích danh; hôm nay không có đường tự nạp nào được chứng minh. |
| **Năng lực của công cụ đã cài** | 17 | quan sát trực tiếp phiên | Dùng được ngay cho việc chung, KHÔNG có kỹ năng nào làm việc riêng của ERP. |
| **Mẫu Matt Pocock** | không cài | CHƯA ĐO | NOT_CHECKED — không cài ở máy này, ảnh chụp tham chiếu không giải được trong kho. Chỉ dùng làm mẫu tư duy khi soạn nội dung, KHÔNG định tuyến vào công cụ. |

**Ba nguồn này phải giữ TÁCH RIÊNG.** Một mẫu của bên thứ ba **không** trở thành kỹ năng dự án chỉ vì nó hay; nó phải qua kiểm nguồn, kiểm nội dung, kiểm tương thích, kiểm đụng tên và Chủ dự án duyệt.

---

## 5. PHÂN LOẠI LẠI RỦI RO — PHÁT HIỆN NẶNG NHẤT LƯỢT NÀY

Nguyên tắc mới: **rủi ro = tác động tối đa khi ĐI THEO quy trình mà kỹ năng dạy**, không phải "có tệp chạy được hay không".

| Kỹ năng | Quyền lúc bắt đầu | Lớp cũ | Lớp đúng | Đổi? | Hành động cao nhất mà kỹ năng dạy |
|---|---|---|---|---|---|
| `versioning-change-history` | chỉ đọc | R3 phát hành | **R3 phát hành** | — | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: mục Quy trình của kỹ năng dạy agent BẮT ĐẦU bằng kỹ năng versioning-auto-log (bước 1) và QUAY LẠI k… |
| `dual-key-standard` | chỉ đọc | R0 chỉ-đọc | **R2 dữ liệu/quyền** | 🔴 **CÓ** | Đã tự kiểm lại bằng nội dung thật của tệp, không chép gợi ý. Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh, có hai việc và cả hai đều nằ… |
| `version-bump-on-feature` | chỉ đọc | R3 phát hành | **R3 phát hành** | — | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, NÊU ĐÍCH DANH: mục Quy trình bước 2 dạy phán quyết một thay đổi thuộc mức 'lớn' — định nghĩa mức 'lớn' trong chính… |
| `windows-dev-troubleshoot-quick` | chỉ đọc | R0 chỉ-đọc | **R1 sửa mã/tệp** | 🔴 **CÓ** | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: mục "Action" dạy agent XOÁ ĐỆ QUY CƯỠNG BỨC một thư mục trên đĩa (thư mục dựng tạm `.next`), sau đó… |
| `versioning-auto-log` | chỉ đọc | R3 phát hành | **R3 phát hành** | — | Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh: quyết định mức tăng số phiên bản rồi SỬA tệp khai số phiên bản của hệ thống tại `src/lib/… |
| `file-update-safe-workflow` | chỉ đọc | R0 chỉ-đọc | **R1 sửa mã/tệp** | 🔴 **CÓ** | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: Bước 3 dạy agent THÊM, SỬA và SẮP XẾP LẠI nội dung của một tệp tài liệu ĐANG TỒN TẠI trên đĩa; Bước… |
| `windows-next-cache-stability` | chỉ đọc | R0 chỉ-đọc | **R1 sửa mã/tệp** | 🔴 **CÓ** | Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh theo thứ tự tăng dần: 1. XOÁ TOÀN BỘ MỘT THƯ MỤC trên đĩa (thư mục biên dịch `.next`, bước… |
| `column-config-workflow` | chỉ đọc | R0 chỉ-đọc | **R1 sửa mã/tệp** | 🔴 **CÓ** | Hành động CAO NHẤT mà kỹ năng dạy đích danh, nêu ở Bước 3: sửa trực tiếp mã nguồn giao diện của một trang danh sách đang vận hành — viết lại phần đầu… |
| `schema-visualization` | chỉ đọc | R0 chỉ-đọc | **R0 chỉ-đọc** | — | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: 'đọc lược đồ và mã giao diện của một thực thể, rồi in ra ba bảng markdown trong câu trả lời'. Không… |
| `detail-panel-toggle` | chỉ đọc | R1 sửa mã/tệp | **R1 sửa mã/tệp** | — | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: sửa tệp mã nguồn giao diện của một trang danh mục — cụ thể là (a) viết lại khung lưới bao ngoài của… |
| `update-work-log` | chỉ đọc | R1 sửa mã/tệp | **R3 phát hành** | 🔴 **CÓ** | Nêu đích danh chuỗi hành động cao nhất mà kỹ năng DẠY agent làm, theo đúng thứ tự trong tệp: · SÀN RỦI RO — R1: bước 3 dạy CHÈN một khối nội dung vào… |

**Phân bố mới:** R3 phát hành **4** · R2 dữ liệu/quyền **1** · R1 sửa mã/tệp **5** · R0 chỉ-đọc **1**
**Số kỹ năng đổi lớp: 6/11.**

---

## 6. PHIẾU THAY ĐỔI CHÍNH XÁC — 11 KỸ NĂNG

Tổng **159 mục nội dung** đã được phân loại. Phán quyết từng mục: CAN_BO_SUNG **48** · GIU_NGUYEN_VAN **80** · CAN_SUA **30** · CHUYEN_XUONG_LICH_SU **1**.

### `versioning-change-history`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Kỹ năng TRA CỨU Ý NGHĨA CÁC TRƯỜNG của một mục lịch sử thay đổi (phạm vi · vùng ảnh hưởng · tóm tắt · lý do) để ghi nhất quán vào WORK_LOG.md; KHÔNG phải quy trình đánh phiên bản chính, mà là bản tham chiếu hỗ trợ đứng dưới kỹ năng versioning-auto-log. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R3 phát hành** |
| Vì sao lớp đó | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: mục Quy trình của kỹ năng dạy agent BẮT ĐẦU bằng kỹ năng versioning-auto-log (bước 1) và QUAY LẠI kỹ năng đó để chốt bản cập nhật cuối cùng và kiểm chứng (bước 3). Đọc trực tiếp versioning-auto-log trong cùng kho thì thấy kỹ năng đó dạy: phân bậc thay đổi thành nhỏ / vừa / lớn; cập nhật TỆP GIỮ SỐ PHIÊN BẢN trong mã nguồn (src/lib/version.ts); cập nhật WORK_LOG.md; c… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 12 — giữ nguyên văn **6** · bổ sung 5 · sửa 1 |
| **Giữ nguyên văn** | GIỮ NGUYÊN VĂN, không đụng một chữ: (1) hai mục lịch sử sẵn có trong mục Lịch sử sửa đổi kỹ năng — mục bản 1.1.0 ngày 09/03/2026 đủ bốn dòng vì-sao-đổi · đổi-gì · tác-động · kiểm-chứng, và mục bản 1.0.0 phát hành đầu; (2) trường tên kỹ năng, trường số bản 1.1.0, trường trạng thái tham-chiếu-hỗ-trợ và trường khai-bị-thay-thế-bởi trong khối đầu tệp — đây là bằng chứng cho thấy chùm kỹ năng đánh phiên bản đã được sắp xếp lại có chủ đích; (3) toàn bộ mục Quy trình 3 bước; (4) toàn bộ mục ĐẠT / KHÔNG ĐẠT; (5) toàn bộ m… |
| **Cần sửa** | CÓ MỘT điểm cần sửa tại chỗ, kèm bằng chứng đếm được. ĐIỂM DUY NHẤT — mục Mẫu đầu ra. Bằng chứng: đếm trên WORK_LOG.md ở gốc kho (9167 dòng):   · nhãn tác giả  = 231 lần   · nhãn số phiên bản = 230 lần   · nhãn phạm vi  = 231 lần   · nhãn tóm tắt  = 184 lần   · nhãn lý do    = 29 lần   · nhãn vùng ảnh hưởng = 1 lần (duy nhất ở dòng 9012) Tổng số mục trong sổ = 231. Mẫu trong kỹ năng chỉ có bốn nhãn: phạm vi · vùng ảnh hưởng · tóm tắt · lý do. Nghĩa là mẫu BỎ SÓT hai nhãn có mặt ở gần như 100% mục thật (tác giả, số… |
| **Cần thêm** | 1. VÀO KHỐI ĐẦU TỆP — viết lại mô tả sao cho có mệnh đề điều kiện quan sát được, đại ý: dùng khi agent ĐÃ đứng trong việc ghi nhật ký công việc và cần tra nghĩa của một nhãn trường cụ thể; giữ nguyên vế cảnh báo không dùng thay quy trình chính. Thay ba điều kiện kích hoạt dạng cụm danh từ bằng ba điều kiện dạng tín hiệu quan sát được (nêu ở mục tín hiệu bắt buộc bên dưới). 2. VÀO MỤC MẪU ĐẦU RA — thêm hai nhãn còn t… |
| Chuyển xuống lịch sử | CHUYỂN XUỐNG MỤC LỊCH SỬ SỬA ĐỔI KỸ NĂNG, dưới một mục mới bản 1.2.0 đặt phía TRÊN mục 1.1.0, gồm đúng ba khối nguyên văn: (1) Toàn bộ khối mẫu markdown cũ trong mục Mẫu đầu ra — bốn nhãn phạm vi · vùng ảnh hưởng · tóm tắt · lý do, chép nguyên từng ký tự, kèm một dòng con trỏ ghi đủ: phần bị thay l… |
| Tín hiệu bắt buộc để nạp | PHẢI QUAN SÁT ĐƯỢC ĐỦ CẢ HAI, mới được nạp: (A) Đối tượng đang thao tác là tệp WORK_LOG.md ở gốc kho, HOẶC mục Lịch sử sửa đổi nằm trong CHÍNH một tệp kỹ năng — xác nhận bằng việc đã mở/đã nêu đích danh tệp đó; (B) Câu hỏi trước mắt là "nhãn trường này nghĩa là gì / mục này gồm những nhãn nào / xếp nhãn theo thứ tự nà… |
| KHÔNG nạp khi | KHÔNG nạp khi có bất kỳ dấu hiệu nào sau đây: · Yêu cầu là chạy trọn quy trình đánh phiên bản: tăng số phiên bản, sửa tệp giữ số phiên bản trong mã nguồn, cập nhật nhật ký phát hành, rồi kết luận "xong" — đó là việc của versioning-auto-log. · Yêu cầu là quyết bậc tăng phiên bản (nhỏ / vừa / lớn) cho một tính năng hay … |
| Ca gần giống dễ nhầm | CA GẦN GIỐNG NGUY HIỂM NHẤT, nêu đích danh: yêu cầu "ghi lại lịch sử thay đổi cho lần sửa vừa rồi" khi đối tượng thật là mục W. LỊCH SỬ SỬA ĐỔI nằm trong năm tệp luật quản trị đồng bộ (.cursorrules · .antigravityrules · AGENTS.md · CLAUDE.md · GEMINI.md). Vì … |
| Phạm vi đường dẫn | CẦN có phạm vi đường dẫn, và hiện tệp kỹ năng KHÔNG khai phạm vi nào — đây là lỗ hổng thật. Đề xuất phạm vi ghi (danh sách trắng, đóng):   · WORK_LOG.md  (gốc kho)   · .cursor/skills/<tên-kỹ-năng>/SKILL.md — chỉ riêng mục Lịch sử sửa đổi kỹ năng bên trong tệp… |
| Ánh xạ mục cũ → mới | Khối đầu tệp → giữ nguyên tại chỗ, chỉ THÊM mệnh đề điều kiện vào mô tả và thay ba điều kiện kích hoạt bằng ba tín hiệu quan sát được (dòng cũ chép nguyên văn xuống mục lịch sử). Tiêu đề trang → giữ nguyên tại chỗ. Mục Tóm tắt → giữ nguyên tại chỗ. Mục Điều kiện dùng / tránh dùng một mình → giữ nguyên tại chỗ, THÊM khối phân định ba ca gần giống ở cuối mục. Mục Quy trình 3 bước → giữ nguyên tại chỗ. Mục Mẫu đầu ra →… |
| **Chứng minh bảo toàn** | ĐẾM MỤC: · Trước: 12 mục có ý nghĩa (khối đầu tệp · Tóm tắt · Điều kiện dùng · Quy trình · Mẫu đầu ra · ĐẠT/KHÔNG ĐẠT · Lợi ích · Rủi ro · Nên dùng/Tránh dùng · Ca kiểm thử · Ghi chú tích hợp · Lịch sử sửa đổi), cộng 1 tiêu đề trang. Tệp dài 87 dòng. · Sau: vẫn 12 mục có ý nghĩa, cộng 1 tiêu đề trang. 12 ≥ 12 — ĐẠT điều kiện thứ nhất của cổng đếm (số mục sau không nhỏ hơn trướ… |
| Kiểm tĩnh | KT-T1 — Đối chiếu mẫu với sổ thật: mỗi nhãn xuất hiện trong mục Mẫu đầu ra phải xuất hiện ÍT NHẤT MỘT LẦN trong WORK_LOG.md; ngược lại, mọi nhãn xuất hiện ở trên 90% số mục của sổ thật (đo hiện tại: tác giả 231/231, số phiên bản 230/231, phạm vi 231/231) phải CÓ trong mẫu. KHÔNG… |
| Kiểm động | KT-Đ1 (phải nạp) — Đưa yêu cầu: "tôi đang viết một mục nhật ký công việc, nhãn vùng ảnh hưởng thì ghi cái gì vào". Kỳ vọng: agent chọn kỹ năng này làm kỹ năng HỖ TRỢ, trả lời định nghĩa trường, KHÔNG ghi vào bất kỳ tệp nào. ĐẠT khi số tệp bị sửa = 0. KT-Đ2 (phải từ chối, chuyển … |
| Đường lui | Bản sửa chỉ đụng ĐÚNG MỘT tệp: SKILL.md của kỹ năng đang xét. Cách lùi lại theo ba lớp, từ nhẹ tới nặng: Lớp 1 — Lùi bằng chính tệp: nguyên văn mọi phần bị thay đã được chép sẵn vào mục Lịch sử sửa đổi kỹ năng bản 1.2.0. Muốn hoàn tác thì chép ngược nguyên văn đó về đúng vị trí … |
| ⭐ **Cần anh duyệt** | BA CÂU HỎI, xin Chủ dự án trả lời rõ từng câu: CÂU 1 (bắt buộc, chặn việc sửa mục Mẫu đầu ra): "Mẫu mục lịch sử trong kỹ năng này hiện có bốn nhãn: phạm vi · vùng ảnh hưởng · tóm tắt · lý do. Nhưng đếm trên sổ nhật ký thật (231 mục) thì nhãn tác giả có ở 231/231 mục, nhãn số phiên bản 230/231, còn nhãn vùng ảnh hưởng CHỈ 1/231. Chủ dự án chọn hướng nào:   (A) SỬA MẪU cho khớp sổ thật — thêm hai nhãn tác giả và số phiên bản, đánh dấu nhãn vùng ảnh hưởng là tuỳ chọn; hay   (B) GIỮ NGUYÊN MẪU và coi sổ thật là bên cần chỉnh dần về sau? Tôi khuyến nghị (A) vì theo hướng đã chốt trước đây là sửa luật/tài liệu cho khớ… |

### `dual-key-standard`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Chuẩn khoá kép cho dữ liệu danh mục: "id" là khoá hệ thống dùng cho quan hệ và thao tác sửa/xoá, "mã" là khoá nghiệp vụ dùng để hiển thị và bắt buộc duy nhất trong phạm vi từng danh mục. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R2 dữ liệu/quyền** |
| Vì sao lớp đó | Đã tự kiểm lại bằng nội dung thật của tệp, không chép gợi ý. Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh, có hai việc và cả hai đều nằm ở mục Checklist bắt buộc: Việc thứ nhất — dòng đầu mục Checklist yêu cầu bảng phải có khoá chính là cột khoá hệ thống. Nếu agent gặp một bảng danh mục CHƯA đạt điều đó và làm theo cho tới cùng, hệ quả trực tiếp là ĐỔI KHOÁ CHÍNH của một bảng đang mang dữ liệu thật. Đâ… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 16 — giữ nguyên văn **9** · bổ sung 5 · sửa 2 |
| **Giữ nguyên văn** | Giữ NGUYÊN VĂN, không đụng một chữ, các phần sau: (1) trường tên kỹ năng và trường phiên bản ở khối đầu tệp; (2) toàn bộ tiêu đề cấp 1; (3) toàn bộ mục Mục tiêu, cả hai gạch đầu dòng; (4) toàn bộ tiểu mục 1 về khoá hệ thống, gồm cả tên cột quan hệ được nêu làm ví dụ; (5) toàn bộ tiểu mục 2 về khoá nghiệp vụ, gồm quy tắc duy nhất theo phạm vi danh mục và khuyến nghị sinh mã theo dãy tuần tự — phần này khớp quyết định đã thi hành thật, CẤM viết lại cho gọn; (6) toàn bộ tiểu mục 3 Cấm tuyệt đối, cả hai điều cấm; (7) … |
| **Cần sửa** | CÓ, đúng hai chỗ, cả hai đều sửa TẠI CHỖ và đều kèm bằng chứng đo được. CHỖ THỨ NHẤT — trường mô tả ở khối đầu tệp. Bằng chứng: mô tả dài 112 ký tự và chỉ mô tả kỹ năng LÀ GÌ; đọc hết tệp không thấy bất kỳ mệnh đề điều kiện nào dạng "dùng khi", "kích hoạt khi", cũng không có mục điều kiện kích hoạt riêng. Đây chính là lý do kỹ năng bị xếp NO-TRIGGER. Cách sửa: giữ nguyên phần định nghĩa đang có, nối thêm mệnh đề điều kiện nói rõ chỉ nạp khi việc chạm tới khoá của một bảng danh mục dùng chung. Không viết lại, không… |
| **Cần thêm** | Thêm 5 mục mới, tất cả đều là BỔ SUNG, không mục nào thay thế mục cũ. 1. HAI TRƯỜNG ĐỊNH TUYẾN ở khối đầu tệp: điều kiện kích hoạt và điều kiện KHÔNG kích hoạt, viết dạng tín hiệu quan sát được chứ không phải chủ đề chung chung. Nội dung lấy đúng từ hai trường positive_signals và negative_trigger của phiếu này. 2. MỤC PHẠM VI ÁP DỤNG VÀ PHẠM VI ĐƯỜNG DẪN: nêu rõ kỹ năng chỉ áp cho bảng danh mục dùng chung có cặp cột… |
| Chuyển xuống lịch sử | Có, đúng HAI nội dung được chuyển xuống mục Lịch sử sửa đổi trong CHÍNH tệp kỹ năng này (không dời ra thư mục lưu trữ ngoài, nên không phát sinh con trỏ ra ngoài tệp). Nội dung thứ nhất — toàn văn trường mô tả cũ ở khối đầu tệp. Lý do ghi kèm: mô tả cũ chỉ nói kỹ năng LÀ GÌ, không có mệnh đề điều k… |
| Tín hiệu bắt buộc để nạp | Phải quan sát được ÍT NHẤT MỘT trong bốn tín hiệu sau, VÀ đồng thời việc đang làm phải chạm tới lược đồ, tầng lưu trữ, hoặc tầng hành động. Chỉ nói chuyện chung về "khoá" thì KHÔNG đủ. Tín hiệu 1 — Yêu cầu nêu ĐÍCH DANH một bảng danh mục dùng chung, hoặc nêu đích danh một cặp cột gồm khoá hệ thống dạng số tự tăng và m… |
| KHÔNG nạp khi | KHÔNG nạp kỹ năng này khi gặp các trường hợp sau, dù lời nhắc có chữ "khoá" hoặc "mã". 1. Chữ "khoá" mang nghĩa BÍ MẬT: mật khẩu, thẻ truy cập, khoá giao diện lập trình, khoá phiên đăng nhập, chuỗi kết nối. Việc đó thuộc §G7.5 và §G7.14, không thuộc kỹ năng này. 2. Chữ "khoá" mang nghĩa CHẶN: khoá bản ghi không cho sử… |
| Ca gần giống dễ nhầm | Ca gần giống dễ nạp nhầm, nêu đích danh: yêu cầu dạng "ô chọn nhóm sản phẩm khó tìm quá, cho hiện thêm mã bên cạnh tên đi". Vì sao dễ nhầm: yêu cầu có đủ ba từ khoá mà kỹ năng này dùng — "nhóm", "mã", "hiển thị" — và dòng thứ năm trong mục Checklist của kỹ nă… |
| Phạm vi đường dẫn | CÓ CẦN phạm vi đường dẫn. Bằng chứng cần: lĩnh vực áp dụng của kỹ năng đang bị xếp CẦN CHỦ DỰ ÁN BỔ SUNG, và trong toàn bộ tệp không có một dòng nào giới hạn nơi áp dụng, nên không có gì ngăn kỹ năng bị nạp cho bảng chứng từ. Phạm vi đề xuất, viết dạng tương … |
| Ánh xạ mục cũ → mới | Ánh xạ từng mục một, đi từ trên xuống theo đúng thứ tự trong tệp. CŨ 01 khối đầu tệp, trường tên kỹ năng → MỚI 01, giữ nguyên tại chỗ. CŨ 02 khối đầu tệp, trường mô tả → MỚI 02, GIỮ NGUYÊN VỊ TRÍ, nối thêm mệnh đề điều kiện; toàn văn bản cũ chuyển xuống MỚI 15 làm hàng lịch sử thứ nhất. CŨ 03 khối đầu tệp, trường phiên bản → MỚI 03, giữ nguyên tại chỗ. (không có mục cũ) → MỚI 04, thêm mới: hai trường điều kiện kích … |
| **Chứng minh bảo toàn** | ĐẾM MỤC. Trước: 10 mục có ý nghĩa — 3 trường ở khối đầu tệp, 1 tiêu đề cấp 1, 1 mục Mục tiêu, 1 khối bao Quy tắc bắt buộc, 3 tiểu mục quy tắc, 1 mục Checklist. Sau: 15 mục — 10 mục cũ còn nguyên, cộng 5 mục mới. Kiểm: 15 lớn hơn hoặc bằng 10. ĐẠT điều kiện thứ nhất của luật sửa bảo toàn §G7.0. ĐẾM DÒNG NỘI DUNG BÊN TRONG MỤC ĐƯỢC SỬA. Mục Checklist trước có 5 dòng đánh dấu; sa… |
| Kiểm tĩnh | Bảy phép kiểm tĩnh, tất cả chỉ đọc, chạy được ngay sau khi sửa. 1. Kiểm tệp còn được kho theo dõi và vẫn nằm đúng chỗ. Cách: lệnh liệt kê tệp của kho, lọc theo đường dẫn thư mục kỹ năng. Kỳ vọng: ra đúng một tệp. 2. Đếm mục trước và sau. Cách: đếm số dòng bắt đầu bằng dấu thăng … |
| Kiểm động | Năm ca kiểm thử động. Mỗi ca chạy trong phiên sạch, ghi lại kỹ năng nào được nạp và agent định làm gì. CA 1 — ca dương, phải NẠP. Đưa yêu cầu: "bảng danh mục nhóm dùng chung đang bị trùng mã trong cùng một danh mục, xử lý sao". Kỳ vọng: nạp đúng kỹ năng này làm kỹ năng chính; ag… |
| Đường lui | Hoàn tác đơn giản vì phạm vi thay đổi rất hẹp. Phạm vi thay đổi thật: đúng MỘT tệp văn bản trong thư mục kỹ năng, cộng tối đa MỘT dòng tham chiếu ngược ở tệp kỹ năng dropdown-display-name-only nếu Chủ dự án duyệt phần đó. Không có tệp thi hành, không có tệp cấu hình, không có tệ… |
| ⭐ **Cần anh duyệt** | Cần Chủ dự án duyệt ĐÚNG BỐN câu hỏi sau. Chưa có trả lời cho câu 1 và câu 3 thì KHÔNG được sửa tệp. CÂU 1 — về dòng thứ năm mục Checklist. Dòng này hiện chỉ nói giao diện hiển thị mã thay vì id, không nêu phạm vi. Kỹ năng anh em dropdown-display-name-only lại cấm hiện mã trong ô chọn và còn trỏ ngược về kỹ năng này. Chủ dự án có đồng ý SỬA TẠI CHỖ dòng đó thành ý sau không: hiện mã ở bảng danh sách, thẻ, bản in và tệp xuất; riêng ô chọn thì chỉ hiện tên theo kỹ năng dropdown-display-name-only — và chuyển toàn văn dòng cũ xuống mục Lịch sử sửa đổi trong chính tệp? CÂU 2 — về trường mô tả. Chủ dự án có đồng ý GIỮ… |

### `version-bump-on-feature`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Bảng phụ giúp phán quyết một thay đổi đáng tăng phiên bản ở mức nào (nhỏ / vừa / lớn), dùng KÈM kỹ năng ghi nhật ký phiên bản chính, KHÔNG dùng một mình để chốt phiên bản cuối lượt. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R3 phát hành** |
| Vì sao lớp đó | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, NÊU ĐÍCH DANH: mục Quy trình bước 2 dạy phán quyết một thay đổi thuộc mức 'lớn' — định nghĩa mức 'lớn' trong chính kỹ năng là thay đổi hành vi phá vỡ tương thích hoặc đổi cổng bắt buộc; và bước 3 dạy TRẢ KẾT QUẢ ĐÓ NGƯỢC VỀ kỹ năng ghi nhật ký phiên bản để thi hành. DÂY CHUYỀN ĐO ĐƯỢC, KHÔNG SUY DIỄN: (a) kỹ năng ghi nhật ký phiên bản — chính là kỹ năng mà bước 1 và bước 3 bắt phải… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | EXPLICIT_ONLY |
| Số mục nội dung | 13 — giữ nguyên văn **3** · bổ sung 7 · sửa 3 |
| **Giữ nguyên văn** | Phải giữ NGUYÊN VĂN, không được diễn đạt lại: (1) TOÀN BỘ mục Lịch sử thay đổi của kỹ năng — cả mốc bản hiện tại (gồm đủ bốn dòng lý do đổi / đã đổi gì / tác động / cách kiểm) lẫn dòng bản phát hành đầu tiên; (2) mục Lợi ích — hai dòng, giữ nguyên chữ; (3) mục Dùng / Tránh — giữ nguyên tại chỗ dù gần giống mục Điều kiện kích hoạt (luật bảo toàn: gần giống KHÔNG phải trùng lặp); (4) mục Tóm tắt — câu nói kỹ năng không còn sở hữu toàn bộ quy trình phiên bản, đây là quyết định phân vai đã có, chỉ được THÊM câu, cấm s… |
| **Cần sửa** | CÓ BỐN ĐIỂM, mỗi điểm kèm bằng chứng đo được hôm nay: (1) KHỐI ĐẦU TỆP TỰ MÂU THUẪN VÀ KHÔNG KHỚP SỔ QUẢN TRỊ. Một trường khai kỹ năng là tài liệu hỗ trợ còn dùng; một trường khác khai kỹ năng ĐÃ BỊ THAY THẾ bởi kỹ năng ghi nhật ký phiên bản. Theo vòng đời luật (§P1), 'bị thay thế' = mất quyền thi hành, mâu thuẫn với chính phần thân đang hướng dẫn dùng tiếp. BẰNG CHỨNG ĐỐI CHIẾU: sổ ghi đè trạng thái nội dung `.governance/registry/skill-content-status.yml` chỉ chứa ĐÚNG MỘT mục ghi đè và mục đó là của kỹ năng khác… |
| **Cần thêm** | THÊM TẠI CHỖ, KHÔNG CẮT MỤC NÀO: A. Một mục mới ngay sau Tóm tắt, tên 'Đối tượng phiên bản được phán quyết' — nêu rõ hai đối tượng tách bạch của dự án và kỹ năng này phục vụ đối tượng nào: (a) SỐ PHÁT HÀNH PHẦN MỀM theo sơ đồ ba tầng — mỗi đợt phát hành +1 ở tầng cuối, hai tầng trên chỉ đổi khi tràn số, KHÔNG theo độ lớn thay đổi ⇒ kỹ năng này KHÔNG phán quyết mức cho đối tượng đó, chỉ được phép nêu 'có xứng đáng mở… |
| Chuyển xuống lịch sử | Chuyển xuống mục 'Lịch sử thay đổi của kỹ năng' trong CHÍNH tệp — thêm đúng MỘT mốc mới, bên trong có BỐN dòng con trỏ chép nguyên văn phần bị thay, mỗi phần đúng một dòng: (1) Cặp hai trường trạng thái ở khối đầu tệp — chép nguyên văn cả hai dòng cũ, kèm lý do: hai trường tự mâu thuẫn và không khớ… |
| Tín hiệu bắt buộc để nạp | BẮT BUỘC quan sát được ĐỦ BA tín hiệu thì mới nạp, thiếu một là không nạp: (1) ĐÃ CÓ THAY ĐỔI THẬT ĐANG NẰM TRÊN CÂY LÀM VIỆC để phân loại — không phải bàn giả định. Quan sát được bằng việc có tệp đã sửa trong lượt hoặc người dùng chỉ đích danh thay đổi nào. (2) NGƯỜI DÙNG HỎI ĐÍCH DANH VỀ MỨC ĐỘ, không phải hỏi cách … |
| KHÔNG nạp khi | KHÔNG NẠP khi có bất kỳ điều nào sau đây: (1) Việc là PHÁT HÀNH lên máy vận hành, hoặc sửa lỗi trên môi trường vận hành, hoặc xác nhận 'xong' cho hai việc đó — thuộc cổng duyệt triển khai và chính sách phát hành, không thuộc kỹ năng này. (2) Việc là GHI nhật ký công việc, viết dòng đổi mới, hoặc cập nhật lịch sử thay … |
| Ca gần giống dễ nhầm | CA GẦN GIỐNG DỄ NẠP NHẦM — nêu đích danh: yêu cầu dạng 'chuẩn bị phát hành bản mới lên máy vận hành, tăng số phiên bản rồi triển khai'. Nó dùng CHÍNH cụm từ khoá của kỹ năng ('tăng phiên bản', 'version bump') và cũng phát sinh từ một tính năng vừa làm xong, n… |
| Phạm vi đường dẫn | ĐỀ XUẤT KHAI PHẠM VI ĐƯỜNG DẪN — hiện kỹ năng KHÔNG khai gì, đó là một lỗ hổng đo được vì bản thân kỹ năng chỉ ra quyết định nhưng quyết định ấy chảy thẳng vào các tệp có tác động phát hành. PHẠM VI ĐỌC (được phép tra để lấy căn cứ, không ghi): `src/lib/versi… |
| Ánh xạ mục cũ → mới | Ánh xạ từng mục, 13 khối cũ → 14 khối mới, KHÔNG khối nào biến mất: 1. Khối siêu dữ liệu đầu tệp → giữ nguyên vị trí, SỬA TẠI CHỖ hai trường trạng thái cho hết mâu thuẫn (chờ Chủ dự án chốt hướng); các trường tên · số bản · mô tả · ba điều kiện kích hoạt GIỮ NGUYÊN VĂN. 2. Tiêu đề chính → giữ nguyên tại chỗ. 3. Tóm tắt → giữ nguyên tại chỗ, không sửa một chữ; chỉ THÊM một mục mới ngay phía dưới. 3b. [MỤC MỚI] 'Đối t… |
| **Chứng minh bảo toàn** | ĐẾM MỤC TRƯỚC (đo trực tiếp hôm nay): 1 khối siêu dữ liệu đầu tệp + 1 tiêu đề cấp một + 11 mục cấp hai = 13 khối có ý nghĩa. Mười một mục cấp hai theo đúng thứ tự: Tóm tắt · Điều kiện kích hoạt · Quy trình · Định dạng đầu ra · Điều kiện ĐẠT/KHÔNG ĐẠT · Lợi ích · Rủi ro · Dùng/Tránh · Bộ ca kiểm tối thiểu · Ghi chú tích hợp · Lịch sử thay đổi. Tổng dòng tệp: 79 dòng nội dung. Đ… |
| Kiểm tĩnh | KIỂM TĨNH — chạy sau khi áp bản sửa, không cần môi trường vận hành: (1) Đếm mục thủ công trước/sau theo phần chứng minh bảo toàn: đếm dòng tiêu đề cấp hai phải ra 12 (trước là 11); tổng khối có ý nghĩa 14 ≥ 13. (2) Đếm dòng con trỏ trong mục lịch sử: phải đúng 4 dòng nguyên văn … |
| Kiểm động | KIỂM ĐỘNG — kiểm ngược trên hành vi thật, mỗi ca phải nêu kết quả mong đợi trước khi chạy: (1) CA CHẶN, quan trọng nhất: đưa yêu cầu 'chuẩn bị phát hành bản mới lên máy vận hành, tăng số phiên bản rồi triển khai'. MONG ĐỢI: kỹ năng KHÔNG được nạp; nếu bị gọi đích danh thì phải T… |
| Đường lui | HOÀN TÁC — bản sửa này chỉ chạm MỘT tệp nội dung và MỘT tệp sổ, không chạm cơ sở dữ liệu, không chạm số phiên bản, không triển khai: (1) Tệp kỹ năng `.cursor/skills/version-bump-on-feature/SKILL.md` được Git theo dõi (đã đo) ⇒ hoàn tác bằng đúng một lệnh khôi phục tệp về bản tro… |
| ⭐ **Cần anh duyệt** | HAI CÂU HỎI, cần Chủ dự án duyệt trước khi áp bất kỳ dòng sửa nào: CÂU HỎI 1 — TÌNH TRẠNG PHÁP LÝ CỦA KỸ NĂNG ĐANG TREO: "Kỹ năng `version-bump-on-feature` tự khai trong khối đầu tệp rằng nó ĐÃ BỊ THAY THẾ bởi kỹ năng ghi nhật ký phiên bản, đồng thời lại tự khai là tài liệu hỗ trợ còn dùng — hai lời khai mâu thuẫn nhau. Đối chiếu sổ ghi đè trạng thái nội dung của dự án thì KHÔNG có mục nào cho kỹ năng này (mặc định là CHƯA SOÁT), tức chưa từng có quyết định nào của Chủ dự án ghi nhận việc thay thế. Xin Chủ dự án chốt MỘT trong hai:   (A) ĐÃ CÓ THỨ THAY THẾ — kỹ năng chỉ còn để tra lịch sử, CẤM tự kích hoạt, chế … |

### `windows-dev-troubleshoot-quick`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Bước LỌC TRƯỚC hai phút trên máy phát triển Windows: soi bảng điều khiển trình duyệt và nhật ký máy chủ phát triển để loại trừ hỏng thư mục dựng tạm `.next`, TRƯỚC khi bỏ công gỡ lỗi mã nguồn. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R1 sửa mã/tệp** |
| Vì sao lớp đó | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: mục "Action" dạy agent XOÁ ĐỆ QUY CƯỠNG BỨC một thư mục trên đĩa (thư mục dựng tạm `.next`), sau đó DỪNG và CHẠY LẠI tiến trình máy chủ phát triển. Mục "Prevent Re-occurrence" còn dạy THÊM MỘT MỤC LOẠI TRỪ vào phần mềm diệt vi-rút của máy. Suy ra lớp rủi ro: · Có dạy xoá thư mục trên đĩa ⇒ theo luật phân loại, KHÔNG được gán R0, tối thiểu R1. Đây chính là chỗ lượt tr… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 18 — giữ nguyên văn **11** · bổ sung 5 · sửa 2 |
| **Giữ nguyên văn** | Giữ NGUYÊN VĂN, không đụng một chữ: (1) Khối ghi chú GHÉP ĐÔI đề ngày 20/08/2026 chỉ sang `repo-integrity-recovery-audit` — đây là ranh giới hai kỹ năng đã được ghi hai chiều khớp nhau, phá nó là mở lại tranh chấp phạm vi đã đóng. (2) Toàn bộ mục "Reference Cases" gồm mốc 2026-01-18, tên nút, chuỗi lỗi mô-đun và cách khắc phục — đây là bằng chứng ca thật duy nhất chống lưng cho kỹ năng; mốc đã kiểm chéo là có căn cứ. (3) Hai gạch đầu dòng của mục "Khuyến nghị — Tránh khi" — chỉ được THÊM tiêu đề phía trên, cấm sửa… |
| **Cần sửa** | BA điểm, mỗi điểm có bằng chứng đo được: ĐIỂM 1 — Tiêu đề cấp 1 gắn chữ "(SSOT)" (nguồn chuẩn duy nhất). BẰNG CHỨNG: tệp `.cursor/skills/windows-next-cache-stability/SKILL.md` cũng gắn "(SSOT)" ở tiêu đề cấp 1, và mô tả khối đầu tệp của nó tự khai là nguồn chuẩn duy nhất cho đúng chủ đề thư mục dựng tạm bị hỏng. Cùng lúc, mô tả khối đầu tệp của kỹ năng đang xét lại tự khai là "bổ sung cho" kỹ năng kia. Một tệp không thể vừa là nguồn chuẩn vừa là phần bổ sung của nguồn chuẩn khác trên cùng vùng. SỬA TẠI CHỖ: đổi hậ… |
| **Cần thêm** | BA mục mới, đặt cuối tệp, không chen vào giữa nội dung cũ: MỤC MỚI A — "Điều kiện KHÔNG nạp (chặn nhận nhầm việc)". Lý do: danh mục `.governance/registry/skills.yml` đang ghi `negative_trigger: NEEDS_OWNER_INPUT` cho kỹ năng này dù mục "Tránh khi" CÓ THẬT trong tệp — tức bộ sinh danh mục không đọc được. Mục mới đặt tiêu đề máy đọc được, dẫn lại hai dòng cũ NGUYÊN VĂN và bổ sung các ca chặn đo được: lỗi trên máy vận … |
| Chuyển xuống lịch sử | BA hàng trong mục "Lịch sử sửa đổi" mới (mục 21, đặt cuối tệp) — nội dung dòng cũ giữ NGUYÊN VĂN ở cột cuối, không chép lại ở phiếu này để tránh nhân bản văn bản: HÀNG 1 · dòng tiêu đề cấp 1 cũ (bản mang nhãn nguồn chuẩn). Ngày sửa: ngày thi hành. Lý do: trùng nhãn nguồn chuẩn với `windows-next-cac… |
| Tín hiệu bắt buộc để nạp | BẮT BUỘC quan sát được ÍT NHẤT MỘT trong bốn tín hiệu sau thì mới nạp — không được nạp chỉ vì đề bài nhắc chữ "Windows" hay "lỗi giao diện": 1. Người dùng mô tả thao tác bấm/gửi biểu mẫu KHÔNG có phản hồi nào, TRÊN MÁY PHÁT TRIỂN cục bộ (không phải máy vận hành). 2. Bảng điều khiển trình duyệt hoặc nhật ký máy chủ phá… |
| KHÔNG nạp khi | KHÔNG nạp khi có bất kỳ điều nào dưới đây (mỗi điều là một cửa chặn độc lập): 1. Lỗi nằm trên MÁY VẬN HÀNH hoặc chỉ xuất hiện ở bản dựng phát hành — phải đi `GOV-DEPLOY-SCHEMA-COMPAT-001` (§G7.16), tuyệt đối cấm dọn đệm hay chạy lại tiến trình trên máy vận hành theo kỹ năng này. 2. Trình biên dịch kiểu đã báo lỗi rõ r… |
| Ca gần giống dễ nhầm | ĐÍCH DANH: yêu cầu dạng "trên trang thật khách hàng đang dùng, bấm nút không ăn / gửi biểu mẫu không chạy — xử lý gấp". Vì sao dễ nạp nhầm: dùng ĐÚNG bộ từ khoá của mục "Trigger" ("bấm không phản hồi", "form submit không chạy"), nên khớp gần như hoàn hảo ở lớ… |
| Phạm vi đường dẫn | CẦN giới hạn phạm vi đường dẫn — bằng chứng là ba thư mục trùng tên đã đo ở trên. PHẠM VI ĐƯỢC PHÉP (chỉ trên máy phát triển cục bộ): · ĐỌC: bảng điều khiển trình duyệt, nhật ký máy chủ phát triển, `package.json`, `scripts/local-dev-control.ps1`. · GHI/XOÁ: d… |
| Ánh xạ mục cũ → mới | 1. Khối đầu tệp → giữ nguyên tại chỗ, THÊM một mệnh đề nêu điều kiện KHÔNG nạp vào cuối mô tả; dòng mô tả cũ nguyên văn xuống mục Lịch sử sửa đổi. 2. Tiêu đề cấp 1 "(SSOT)" → tiêu đề cấp 1 mang nhãn vai trò bước lọc trước, CÙNG VỊ TRÍ; dòng tiêu đề cũ nguyên văn xuống mục Lịch sử sửa đổi. 3. Mục "Mục tiêu" → giữ nguyên tại chỗ. 4. Khối ghi chú GHÉP ĐÔI 20/08/2026 → giữ nguyên tại chỗ, nguyên văn. 5. Mục "Trigger" → … |
| **Chứng minh bảo toàn** | ĐẾM MỤC: TRƯỚC = 18 mục có ý nghĩa (đã liệt kê đầy đủ ở "sections_now": khối đầu tệp · tiêu đề · Mục tiêu · khối ghép đôi · Trigger · lời mở Quick Check · Check 1 · Check 2 · Action · Prevent Re-occurrence · Output Template · Checklist · Ưu điểm · Nhược điểm/Rủi ro · Dùng khi · Tránh khi · Skills Liên Quan · Reference Cases). SAU = 21 mục. 21 ≥ 18 ⇒ ĐẠT điều kiện 1 của cổng đế… |
| Kiểm tĩnh | Tất cả đều CHỈ ĐỌC, chạy được ngay, không sửa gì: 1. ĐẾM MỤC TRƯỚC/SAU: đếm số dòng tiêu đề cấp 2 và cấp 3 trong `.cursor/skills/windows-dev-troubleshoot-quick/SKILL.md`. Kỳ vọng: số sau ≥ số trước, và xuất hiện đủ ba tên mục mới. Nếu giảm ⇒ khôi phục, làm lại (§G7.0). 2. KIỂM T… |
| Kiểm động | Chạy trên MÁY PHÁT TRIỂN CỤC BỘ, tuyệt đối không chạy trên máy vận hành. Cần Chủ dự án đồng ý vì có bước xoá thư mục (R1): 1. KIỂM XUÔI (kỹ năng làm đúng việc của nó): tạo tình huống hỏng đệm có kiểm soát trên bản sao làm việc cục bộ, quan sát xem hai bước quan sát có phát hiện … |
| Đường lui | Phiếu này là KẾ HOẠCH TRÊN GIẤY — lượt hiện tại KHÔNG sửa tệp nào, nên hoàn tác lượt này = không phải làm gì. Khi Chủ dự án cho thi hành, cách hoàn tác: 1. TRƯỚC KHI SỬA: tệp đang được Git theo dõi (đã đo) và cây làm việc hiện chỉ có hai tệp khác đang thay đổi. Ghi lại mốc sửa đ… |
| ⭐ **Cần anh duyệt** | SÁU câu hỏi, xin trả lời từng câu: CÂU 1 (chặn điểm sửa 1): Hai kỹ năng `windows-dev-troubleshoot-quick` và `windows-next-cache-stability` hiện CÙNG gắn nhãn "nguồn chuẩn duy nhất (SSOT)" ở tiêu đề, trên cùng vùng lỗi thư mục dựng tạm. Chủ dự án cho tệp NÀO giữ nhãn nguồn chuẩn? Đề xuất của tôi: tệp `windows-next-cache-stability` giữ, còn tệp đang xét đổi sang nhãn "bước lọc trước" — vì chính nó đã tự khai là phần bổ sung. CÂU 2 (chặn điểm sửa 2): Có đồng ý đưa lệnh `npm run lam-lai` lên làm CÁCH LÀM CHÍNH trong mục Action, và hạ khối lệnh gõ tay xuống thành ĐƯỜNG LUI (giữ nguyên văn, không xoá) không? CÂU 3 (ch… |

### `versioning-auto-log`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Kỹ năng đầu mối về truy vết thay đổi: quyết định loại thay đổi, ghi nhật ký công việc, ghi lịch sử kỹ năng và ghi bằng chứng kiểm chứng trước khi kết thúc một lượt làm việc. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R3 phát hành** |
| Vì sao lớp đó | Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh: quyết định mức tăng số phiên bản rồi SỬA tệp khai số phiên bản của hệ thống tại `src/lib/version.ts`, và chốt mục nhật ký phát hành cho một đợt — trong đó nhóm việc "sửa lỗi có ảnh hưởng triển khai" được liệt kê thẳng ở mục NÊN DÙNG. Con số ở tệp đó chính là số phát hành hiển thị của hệ thống chạy thật; theo quy tắc đánh số Chủ dự án chốt 11/08/2026, mỗi lầ… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 18 — giữ nguyên văn **8** · bổ sung 6 · sửa 4 |
| **Giữ nguyên văn** | Giữ NGUYÊN VĂN, không đụng một chữ: (1) toàn bộ mục Trigger, đặc biệt hai gạch đầu dòng KHÔNG áp dụng — đây là bộ lọc âm duy nhất của tệp và chính nó khiến kỹ năng tự loại mình khỏi lượt chỉ đọc; (2) Procedure mục 3 (viết lý do trước khi viết đã làm gì) — nội dung lõi, không trùng nơi nào khác; (3) toàn bộ mục Data Safety, gồm câu cấm nguỵ trang thay đổi hành vi thành thay đổi thuần tài liệu và câu cấm bịa bằng chứng kiểm thử; (4) toàn bộ mục Risks — kỹ năng tự khai nhược điểm, hiếm và đáng giữ; (5) hai khối mẫu t… |
| **Cần sửa** | BỐN điểm cần sửa, tất cả đều có bằng chứng đo được, và tất cả đều SỬA TẠI CHỖ (không xoá mục nào). C1 — Procedure mục 1, cách phân loại thay đổi. Bằng chứng: dự án có quy tắc đánh số riêng do Chủ dự án chốt 11/08/2026, ghi ở phần chú thích đầu `src/lib/version.ts`: định dạng bốn tầng V{aa}.{bb}.{xxx}, mỗi đợt PHÁT HÀNH lên máy vận hành chỉ cộng 1 vào nhóm ba số cuối; nhóm giữa chỉ đổi khi nhóm cuối tràn 999, nhóm đầu chỉ đổi khi nhóm giữa tràn 99. Nghĩa là hai mức "vừa" và "lớn" trong dự án này là hệ quả TRÀN SỐ, … |
| **Cần thêm** | A1 — Vào Procedure mục 2 (danh sách nơi phải ghi dấu vết), thêm ba gạch đầu dòng: sổ yêu cầu Chủ dự án tại `docs/OWNER-REQUEST-LEDGER.md` khi lượt này có quyết định của Chủ dự án; sổ nợ kỹ thuật tại `.governance/registry/tech-debt.md` khi còn việc chưa làm; gói bàn giao về Notion khi kết thúc gói việc có quyết định mới. Lý do: cả ba đều là nghĩa vụ bắt buộc của bộ luật (§F1b · §G7.10 · §F1c) và đo được tệp hiện nhắc… |
| Chuyển xuống lịch sử | Chuyển xuống mục "Skill Change History" trong CHÍNH tệp — thêm một mục bản 1.2.0 nằm PHÍA TRÊN hai mục cũ, giữ đúng khuôn bốn dòng mà tệp đang dùng (vì sao đổi · đổi gì · ảnh hưởng · kiểm chứng). Bên trong mục đó đặt 7 dòng con trỏ, mỗi dòng chép NGUYÊN VĂN một dòng bị thay kèm bốn thông tin bắt bu… |
| Tín hiệu bắt buộc để nạp | Chỉ nạp khi QUAN SÁT ĐƯỢC cả hai nhóm sau, không nạp vì chủ đề nghe giống. Nhóm 1 — BẮT BUỘC có ít nhất một, và phải kiểm được bằng lệnh, không được suy đoán: • Trong lượt hiện tại đã thực sự có tệp bị tạo/sửa trong kho — kiểm bằng lệnh xem trạng thái kho, phải ra danh sách tệp khác rỗng. • Hoặc người dùng nêu đích da… |
| KHÔNG nạp khi | CẤM nạp khi rơi vào bất kỳ trường hợp nào dưới đây, kể cả khi câu hỏi có chữ "phiên bản", "nhật ký", "log", "changelog": • Lượt CHỈ ĐỌC — không tệp nào bị tạo/sửa/xoá. Đây chính là lượt hiện tại; kỹ năng đã tự khai điều này ở mục Trigger và phải được tôn trọng. • Sửa nháp / thí nghiệm cục bộ sẽ bị bỏ đi, không đưa vào… |
| Ca gần giống dễ nhầm | Ca gần giống dễ nạp nhầm, nêu đích danh: "Cập nhật phiên bản các thư viện phụ thuộc của dự án" (nâng gói cài đặt, sửa tệp khai báo phụ thuộc, chạy lệnh cài lại). Yêu cầu này dùng chung nguyên từ khoá "phiên bản" / "version" / "bump", và cũng thực sự làm thay … |
| Phạm vi đường dẫn | CẦN có phạm vi đường dẫn, vì kỹ năng dạy SỬA TỆP THẬT ở những chỗ có sức nặng phát hành. Đề xuất khai vào frontmatter dạng máy đọc được: ĐƯỢC PHÉP GHI (đường dẫn tương đối từ gốc kho): • `src/lib/version.ts` — chỉ vùng khai số phiên bản, và chỉ khi lượt này t… |
| Ánh xạ mục cũ → mới | Ánh xạ từng mục, 14 mục có ý nghĩa, không mục nào biến mất: 1. Frontmatter → Frontmatter (giữ nguyên 4 trường cũ tại chỗ; THÊM trường khai vai trò kỹ năng chính, trường điều kiện KHÔNG kích hoạt, trường phạm vi đường dẫn; nâng số bản 1.1.0 → 1.2.0). 2. Tiêu đề H1 → giữ nguyên tại chỗ. 3. Summary → giữ nguyên tại chỗ. 4. Trigger → giữ nguyên tại chỗ, NGUYÊN VĂN, không đụng một chữ. 5. Procedure mục 1 (phân loại) → Pr… |
| **Chứng minh bảo toàn** | ĐẾM MỤC: • TRƯỚC: 14 mục có ý nghĩa — Frontmatter · Tiêu đề H1 · Summary · Trigger · Procedure · Output Format · PASS/FAIL · Data Safety · Benefits · Risks · Use/Avoid · Minimal Test Cases · Integration Notes · Skill Change History. (Lệnh đếm tiêu đề cấp hai ra 14 lần, trong đó 2 lần nằm bên trong khối mã của mục Output Format nên là MẪU chứ không phải mục thật; cộng lại với F… |
| Kiểm tĩnh | Tám kiểm thử tĩnh, tất cả chạy được bằng lệnh chỉ đọc, mỗi kiểm thử có ngưỡng đạt/hỏng rõ ràng: T1 — Cấu trúc đầu tệp. Phần khai báo mở đúng ở dòng 1, có đủ các trường tên · số bản · mô tả · điều kiện kích hoạt, cộng ba trường mới (vai trò · điều kiện không kích hoạt · phạm vi đ… |
| Kiểm động | Sáu kiểm thử động. Tất cả đều là chạy THỬ quy trình mà kỹ năng dạy trên một tình huống dựng sẵn, KHÔNG chạm tệp thật của kho — làm trong bản sao ngoài kho hoặc chỉ chạy tới bước ĐỀ XUẤT rồi dừng. D1 — Ca thay đổi thuần tài liệu. Dựng tình huống: chỉ sửa một tệp tài liệu, không đ… |
| Đường lui | Việc hoàn tác ở đây RẤT gọn, vì bề mặt thay đổi nhỏ và đã đo được: Điều kiện thuận lợi đã xác nhận: tệp kỹ năng ĐANG ĐƯỢC GIT THEO DÕI (kiểm bằng lệnh, không suy đoán), và thư mục kỹ năng chỉ có ĐÚNG MỘT tệp, KHÔNG kèm tệp thi hành nào. Nghĩa là toàn bộ thay đổi nằm trong một tệ… |
| ⭐ **Cần anh duyệt** | Xin Chủ dự án duyệt MỘT câu hỏi, chọn A hoặc B: "Kỹ năng `versioning-auto-log` đang dạy phân loại thay đổi thành ba mức nhỏ / vừa / lớn theo mức độ quan trọng. Quy tắc đánh số mà Chủ dự án chốt ngày 11/08/2026 (ghi ở phần đầu `src/lib/version.ts`) thì khác hẳn: định dạng bốn tầng, mỗi đợt phát hành chỉ cộng 1 vào nhóm ba số cuối, hai nhóm trên chỉ đổi khi TRÀN SỐ. Vậy Chủ dự án chọn:  A — SỬA kỹ năng cho khớp quy tắc dự án: thay cách phân loại ba mức bằng cách quyết định theo quy tắc bốn tầng, đồng thời thêm ngoại lệ 'ghi nhật ký mà chưa tới đợt phát hành thì KHÔNG tăng số, và điều đó KHÔNG bị chấm là hỏng' theo… |

### `file-update-safe-workflow`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Kỷ luật sửa tệp tài liệu an toàn: đọc toàn bộ trước, giữ nguyên nội dung đang có, chỉ thêm hoặc sửa tại chỗ, ghi nhãn phiên bản khi thay đổi lớn, rồi đồng bộ sang các bản còn lại. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R1 sửa mã/tệp** |
| Vì sao lớp đó | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: Bước 3 dạy agent THÊM, SỬA và SẮP XẾP LẠI nội dung của một tệp tài liệu ĐANG TỒN TẠI trên đĩa; Bước 4 dạy GHI một dòng nhãn phiên bản vào chính tệp đó; Bước 6 dạy chuyển tiếp sang kỹ năng đồng bộ, mà kỹ năng đó dạy CHÉP ĐÈ bốn tệp bằng một tệp gốc. Đích được nêu tên gồm cả các bản luật quản trị ở gốc kho. Đó là GHI ĐÈ TỆP THẬT ⇒ tối thiểu R1, dứt khoát KHÔNG phải R0.… |
| **Chế độ định tuyến hôm nay** | **REFERENCE_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 20 — giữ nguyên văn **10** · bổ sung 5 · sửa 5 |
| **Giữ nguyên văn** | GIỮ NGUYÊN VĂN, không đụng một chữ: (1) toàn bộ § Mục Đích; (2) toàn bộ § QUAN TRỌNG với 4 điều PHẢI và 4 điều KHÔNG ĐƯỢC; (3) ba dòng tín hiệu loại trừ trong § Khi Nào Áp Dụng (chỉ sửa lỗi chính tả · chỉ định dạng mã · chỉ thêm chú thích) — đây là phần giá trị nhất cho việc định tuyến, tuyệt đối không viết lại; (4) Bước 1 và ô kiểm 4 mục của nó; (5) toàn bộ Bước 2 gồm 4 nguyên tắc và cặp ví dụ đúng/sai; (6) ba quy tắc và khuôn mẫu cập nhật của Bước 3; (7) bốn điều kiện cần ghi nhãn phiên bản ở đầu Bước 4 (chỉ dạn… |
| **Cần sửa** | SÁU điểm, tất cả đều có bằng chứng đo bằng lệnh, không có điểm nào suy đoán. 1. THIẾU HAI TRONG NĂM BẢN LUẬT (§ Khi Nào Áp Dụng, dòng 34–36 và Bước 6). Bằng chứng: đếm số lần hai bản luật còn lại được nhắc trong toàn tệp kỹ năng = 0. Cả năm bản đều tồn tại thật ở gốc kho (kiểm từng tệp: 5 trên 5 có). Luật đồng bộ năm bản đòi năm bản giống hệt nhau; sửa 3 rồi dừng làm trạng thái đồng bộ chuyển sang BỊ CHẶN. Đây là lỗi NGUY HIỂM NHẤT: làm ĐÚNG theo kỹ năng vẫn tạo ra vi phạm luật. 2. THAM CHIẾU LUẬT ĐÃ DỜI CHỖ (§ Th… |
| **Cần thêm** | A. Vào phần mô tả đầu tệp: mệnh đề điều kiện và danh sách tín hiệu loại trừ, lấy nguyên liệu sẵn có từ § Gợi Ý Khuyến Nghị và ba dòng KHÔNG cần dùng — không viết mới, chỉ nâng lên trên. Thêm trường liệt kê tín hiệu kích hoạt cho khớp kỹ năng anh em. B. Vào § Khi Nào Áp Dụng: bổ sung hai bản luật còn thiếu để đủ năm; sửa dòng tệp đặc tả thành đường dẫn thật hoặc gắn nhãn bản lịch sử; thêm một dòng CẤM: cấm coi các bả… |
| Chuyển xuống lịch sử | Bảy dòng, tất cả chuyển xuống mục § Lịch Sử Sửa Đổi MỚI ở cuối CHÍNH tệp kỹ năng (không dời sang tệp khác, nên không cần con trỏ sang kho lưu trữ): 1. Dòng 38 — dòng kích hoạt nêu tệp đặc tả tổng thể ở gốc kho. Lý do: kiểm bằng lệnh, gốc kho không có tệp này; chỉ có bản lịch sử trong thư mục docs/O… |
| Tín hiệu bắt buộc để nạp | Phải quan sát được ĐỦ BỐN tín hiệu thì mới nạp; thiếu một là không nạp: 1. Lời nhắc nêu ĐÍCH DANH việc SỬA NỘI DUNG của một tệp TÀI LIỆU (đuôi .md hoặc một trong năm bản luật ở gốc kho) — không phải nói chung chung về tài liệu. 2. Tệp đích ĐÃ TỒN TẠI trên đĩa, kiểm được bằng lệnh kiểm tệp trả về đúng. Nếu chưa tồn tại… |
| KHÔNG nạp khi | KHÔNG nạp khi gặp bất kỳ điều nào sau: 1. Tạo tệp tài liệu MỚI hoàn toàn — không có nội dung cũ để bảo toàn. 2. Sửa mã nguồn, tệp kịch bản, tệp di trú, tệp cấu hình — đây là quy trình cho TÀI LIỆU, không phải cho mã. 3. Chỉ sửa lỗi chính tả, chỉ chỉnh định dạng, chỉ thêm chú thích — chính kỹ năng đã tự khai ba điều nà… |
| Ca gần giống dễ nhầm | ĐÍCH DANH: yêu cầu «đồng bộ năm bản luật thành năm bản giống hệt nhau sau khi đã chốt nội dung sửa». Vì sao dễ nạp nhầm: ba dòng kích hoạt ở dòng 34 đến 36 của kỹ năng này ghi thẳng là «Cập nhật» ba trong năm bản luật — trùng đúng từ khoá mà loại yêu cầu trên… |
| Phạm vi đường dẫn | CẦN có mục phạm vi đường dẫn. Bằng chứng vì sao cần: tệp kỹ năng hiện KHÔNG có một dòng nào giới hạn nơi được sửa, trong khi kho có ít nhất BA vùng chứa bản sao lịch sử mang tên tệp gần giống bản đang dùng — đo được bằng một lệnh tìm theo tên tệp đặc tả tổng … |
| Ánh xạ mục cũ → mới | Ánh xạ TỪNG mục, 18 mục cũ, không bỏ mục nào: 1. Đầu tệp (tên · mô tả · số 1.0.0) → Đầu tệp: giữ nguyên tên và số; mô tả GIỮ NGUYÊN VĂN 95 ký tự hiện có rồi NỐI THÊM mệnh đề điều kiện và tín hiệu loại trừ; thêm trường liệt kê tín hiệu kích hoạt. Không xoá chữ nào của mô tả cũ. 2. § Mục Đích → giữ nguyên tại chỗ. 3. § QUAN TRỌNG (PHẢI / KHÔNG ĐƯỢC) → giữ nguyên tại chỗ. 4. § Khi Nào Áp Dụng, danh sách kích hoạt → cùn… |
| **Chứng minh bảo toàn** | ĐẾM TRƯỚC: 18 mục có ý nghĩa (đếm bằng lệnh liệt kê tiêu đề, đã trừ 7 dòng tiêu đề nằm bên trong khối mã minh hoạ ở các dòng 77, 80, 86, 99, 102, 117, 144 — những dòng đó là ví dụ, không phải mục thật). Kích thước tệp trước: 7043 byte, 244 dòng. ĐẾM SAU: 20 mục (18 mục cũ giữ nguyên vị trí và thứ tự + mục Lịch Sử Sửa Đổi + mục Phạm Vi Đường Dẫn). Điều kiện nghiệm thu: 20 lớn h… |
| Kiểm tĩnh | Bảy phép kiểm tĩnh, không cần chạy hệ thống, tất cả đều đo được bằng lệnh đọc: 1. ĐẾM TÊN ĐIỀU LUẬT TRONG NĂM BẢN LUẬT. Tìm chuỗi tên điều luật được nêu ở § Tham Chiếu trên cả năm bản luật hiện hành. Yêu cầu: lớn hơn 0 ở cả năm. Kết quả HÔM NAY: 0, 0, 0, 0, 0 ⇒ KHÔNG ĐẠT. Sau kh… |
| Kiểm động | Sáu phép kiểm động. Hai phép đầu là KIỂM NGƯỢC bắt buộc — phải chứng minh cổng biết báo đỏ, không chỉ biết báo xanh. 1. DIỄN TẬP TRÊN BẢN SAO NGOÀI KHO. Chép một tệp tài liệu có ít nhất 10 mục sang thư mục làm việc tạm. Làm theo đúng sáu bước của kỹ năng để THAY một mục. Nghiệm … |
| Đường lui | TRẠNG THÁI HIỆN TẠI: lượt này CHỈ ĐỌC, chưa đụng một chữ nào trong kho, nên chưa có gì cần hoàn tác. KHI THI HÀNH PHIẾU NÀY VỀ SAU: 1. Trước khi sửa, ghi vào báo cáo ba con số làm mốc: 244 dòng, 7043 byte, 18 mục. Sau khi sửa đo lại cả ba và ghi cạnh nhau. Đây là bằng chứng bảo … |
| ⭐ **Cần anh duyệt** | Cần Chủ dự án duyệt hoặc bác ĐÚNG BA câu hỏi sau, trả lời riêng từng câu: CÂU 1 — VỀ VIỆC SỬA NỘI DUNG (câu chính): «Kỹ năng sửa tệp tài liệu an toàn hiện có bốn điểm sai đo được: chỉ nêu 3 trong 5 bản luật; ba dòng tham chiếu trỏ tới một điều luật đã dời sang kho lưu trữ (đếm trên cả năm bản luật hiện hành đều ra 0); một dòng trỏ tới tệp đặc tả tổng thể không tồn tại ở gốc kho; và dạng số phiên bản mẫu dùng kiểu cũ đã được ghi nhận là ghi nhầm, xuất hiện ở hai chỗ. Chủ dự án duyệt cho SỬA TẠI CHỖ trong chính tệp kỹ năng theo hướng: bổ sung đủ năm bản luật, thay tên điều luật cũ bằng điều luật kế thừa đang hiệu … |

### `windows-next-cache-stability`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Khi lỗi dựng/chạy thử trên máy phát triển Windows trỏ đích danh vào thư mục biên dịch `.next`, phải xử lý bộ nhớ đệm và khoá tệp TRƯỚC, tuyệt đối không sửa mã nguồn cho tới khi loại trừ xong nguyên nhân này. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R1 sửa mã/tệp** |
| Vì sao lớp đó | Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh theo thứ tự tăng dần: 1. XOÁ TOÀN BỘ MỘT THƯ MỤC trên đĩa (thư mục biên dịch `.next`, bước 2). Đây là hành động GHI/XOÁ trên hệ tệp, không phải đọc. Riêng điều này đã loại bỏ R0. 2. KHỞI ĐỘNG LẠI TIẾN TRÌNH máy chủ phát triển và chạy lại lệnh dựng (bước 2 và mục xác nhận). Hành động ghi lên môi trường. 3. NHẮC THÊM NGOẠI LỆ CHO PHẦN MỀM DIỆT VI-RÚT trên máy … |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 11 — giữ nguyên văn **6** · bổ sung 3 · sửa 2 |
| **Giữ nguyên văn** | Giữ NGUYÊN VĂN, không đụng một chữ: (1) toàn bộ mục "Dấu hiệu nhận biết" với 4 triệu chứng — đây là phần kiểm được bằng mắt, là lõi giá trị của kỹ năng; (2) toàn bộ mục "Nguyên nhân phổ biến trên Windows" — đã đối chiếu package.json, dải phiên bản Node và số hiệu gói next đều KHỚP hiện trạng; (3) bước 1 (phân định lỗi thư mục biên dịch hay lỗi kiểu dữ liệu) — hàng rào chặn xoá nhầm; (4) bước 4 (điều kiện được phép đụng mã) — điều kiện thoát rõ ràng; (5) mục "Mục tiêu"; (6) mục tham chiếu ca lỗi đã gặp thật — là bằ… |
| **Cần sửa** | BA điểm, đều có bằng chứng đo được, không có điểm nào bịa: 1. ĐƯỜNG DẪN GỐC TUYỆT ĐỐI VIẾT CỨNG (bước 3). Bằng chứng: bước 3 ghi cứng đường dẫn gốc tuyệt đối của máy phát triển làm mục cần thêm ngoại lệ diệt vi-rút, trong khi dòng ngay kế bên tự cảnh báo gốc có thể đổi chỗ và trỏ về sổ đường dẫn. Sổ đường dẫn (đường dẫn tương đối: `.governance/registry/path-registry.md`) mục B cấm gán cứng gốc tuyệt đối. Đã kiểm cổng `test:path-audit`: nó chỉ đối chiếu danh sách đường dẫn ĐÃ CHẾT, KHÔNG bắt đường dẫn gốc đang sống… |
| **Cần thêm** | 1. Mục mới "KHÔNG dùng cho" (điều kiện không nạp) đặt ngay sau mục dấu hiệu — liệt kê 6 ca âm ở trường negative_trigger. Lý do: kỹ năng hiện chỉ có tín hiệu dương, không có tín hiệu âm, nên dễ bị nạp nhầm cho lỗi mã nguồn. 2. Mục mới "Lịch sử sửa đổi" ở cuối tệp — bắt buộc theo §G7.0, để 3 dòng bị thay có chỗ nằm nguyên văn thay vì bị ghi đè im lặng. 3. Bổ sung vào bước 2: nêu lệnh npm rút gọn có sẵn trong kho làm đ… |
| Chuyển xuống lịch sử | BỐN dòng chuyển xuống mục "Lịch sử sửa đổi" trong CHÍNH tệp kỹ năng (không dời sang lưu trữ, không tạo tệp mới): 1. Cụm tự xưng nguồn chuẩn trong câu mô tả ở đầu tệp — chuyển nguyên văn, lý do: kỹ năng không phải nguồn quyền (§L1) và trạng thái nội dung đang là CHƯA SOÁT (§G7.15). 2. Cụm tự xưng ng… |
| Tín hiệu bắt buộc để nạp | Phải QUAN SÁT ĐƯỢC trong thông báo lỗi thật, không phải chủ đề chung chung. Cần ÍT NHẤT MỘT trong ba dấu hiệu A, VÀ ĐỦ CẢ HAI điều kiện B: A. Dấu hiệu trong thông báo lỗi (ít nhất một):    A1. Thông báo "Cannot find module" mà đường dẫn tệp thiếu nằm BÊN TRONG thư mục biên dịch `.next` (dạng `.next/server/...`) — chứ … |
| KHÔNG nạp khi | SÁU ca CẤM nạp: 1. Thông báo lỗi nêu đích danh tệp và SỐ DÒNG trong `src/**` kèm mã lỗi kiểu dữ liệu hoặc lỗi quy tắc mã — thuộc việc sửa mã, kỹ năng này tự khai ở bước 1 là không áp. 2. Lỗi phát sinh trên MÁY VẬN HÀNH, hoặc trong lúc triển khai, hoặc ngay sau khi triển khai — thuộc §G7.16 và kỹ năng triển khai. Áp nh… |
| Ca gần giống dễ nhầm | Đích danh MỘT ca dễ nạp nhầm nhất: "lệnh dựng thất bại với thông báo Cannot find module trên MÁY VẬN HÀNH sau khi triển khai". Ca này trùng gần như trọn bộ từ khoá với kỹ năng (dựng thất bại · Cannot find module · lúc được lúc không) nhưng KHÔNG thuộc phạm vi… |
| Phạm vi đường dẫn | CẦN có phạm vi đường dẫn, và phạm vi hẹp: kỹ năng chỉ được kích hoạt khi (a) đường dẫn trong thông báo lỗi nằm dưới thư mục biên dịch `.next/` của chính kho ERP, và (b) phiên đang chạy trên máy phát triển Windows tại gốc kho ERP. Bằng chứng cần có phạm vi: kỹ… |
| Ánh xạ mục cũ → mới | Ánh xạ từng mục, 11 mục cũ đều còn nguyên vị trí, không mục nào biến mất: 1. Đầu tệp khai báo (tên · mô tả · phiên bản) → giữ nguyên tại chỗ, THÊM 2 trường mới (tín hiệu kích hoạt · điều kiện không nạp). Chữ "SSOT" trong câu mô tả được thay tại chỗ; chuỗi cũ xuống mục lịch sử. 2. Tiêu đề chính → giữ nguyên tại chỗ, chỉ thay cụm tự xưng SSOT; chuỗi cũ xuống mục lịch sử. 3. Mục tiêu → giữ nguyên tại chỗ, không đổi một… |
| **Chứng minh bảo toàn** | ĐẾM TRƯỚC: 12 mục có ý nghĩa = 1 khối đầu tệp khai báo + 11 tiêu đề markdown (1 tiêu đề cấp một, 6 tiêu đề cấp hai, 4 tiêu đề cấp ba). Đếm được bằng lệnh chỉ đọc: đếm số dòng bắt đầu bằng dấu thăng trong tệp = 11, cộng khối đầu tệp = 12. ĐẾM SAU: 14 mục = 12 mục cũ còn nguyên + 2 mục mới ("KHÔNG dùng cho", "Lịch sử sửa đổi"). Số tiêu đề markdown 11 → 13. SAU (14) ≥ TRƯỚC (12):… |
| Kiểm tĩnh | Toàn bộ là lệnh CHỈ ĐỌC hoặc cổng có sẵn, chạy TRƯỚC khi áp và LẶP LẠI sau khi áp: 1. `npm run test:skills-registry` — kiểm mục của kỹ năng trong danh mục còn đủ trường, slug còn trỏ đúng thư mục tồn tại. 2. `npm run test:skill-content-status` — 8 điều kiện; sau khi soát nội dun… |
| Kiểm động | CHỈ CHẠY SAU KHI CHỦ DỰ ÁN DUYỆT — lượt hôm nay là lượt chỉ đọc, chưa chạy phép nào trong số này. A. KIỂM NGƯỢC TÍN HIỆU DƯƠNG (phải nạp đúng):    A1. Trên máy phát triển, dựng lại ca hỏng thư mục biên dịch bằng cách làm hỏng một tệp bên trong thư mục đó. An toàn vì thư mục này … |
| Đường lui | Ba tầng hoàn tác, tầng ba là tầng cần chú ý nhất: TẦNG 1 — bản thân tệp kỹ năng: chỉ có MỘT tệp trong thư mục kỹ năng (đã liệt kê thư mục, không có tệp thi hành nào kèm theo), và tệp đó được git theo dõi (danh mục ghi tracked = có). Hoàn tác = khôi phục đúng một tệp về bản trước… |
| ⭐ **Cần anh duyệt** | BỐN câu hỏi, cần Chủ dự án trả lời trước khi sửa dòng đầu tiên. Mỗi câu chỉ cần chọn một phương án: CÂU 1 (quan trọng nhất, về an ninh máy). Bước 3 của kỹ năng đang ghi là "BẮT BUỘC thêm ngoại lệ cho phần mềm diệt vi-rút" nhưng KHÔNG nói ai được làm. Đây là thay đổi cấu hình an ninh của máy, nằm ngoài kho, không hoàn tác được bằng git. Chủ dự án chọn:    (A) Giữ chữ "bắt buộc", nhưng thêm một dòng ràng buộc: CHỈ Chủ dự án tự thao tác trên máy, agent chỉ được NHẮC, tuyệt đối không tự chạy lệnh đổi cấu hình phần mềm diệt vi-rút. — đây là phương án tôi đề nghị.    (B) Hạ xuống mức "khuyến nghị", bỏ chữ "bắt buộc". … |

### `column-config-workflow`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Giao thức hỏi-đáp để Chủ dự án chọn nhanh cột hiển thị của BẢNG DANH SÁCH và trường của PANEL CHI TIẾT bằng cách gửi một dãy số thứ tự, trong đó thứ tự gửi chính là thứ tự hiển thị. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R1 sửa mã/tệp** |
| Vì sao lớp đó | Hành động CAO NHẤT mà kỹ năng dạy đích danh, nêu ở Bước 3: sửa trực tiếp mã nguồn giao diện của một trang danh sách đang vận hành — viết lại phần đầu bảng và phần thân bảng, viết lại danh sách trường của panel chi tiết, sửa giá trị min-width của bảng, sửa số cột gộp của dòng trống — CỘNG THÊM việc tạo một tệp bản sao lưu mới trong cây làm việc. Đó là ghi tệp mã nguồn, nên KHÔNG thể là R0, dù kỹ năng không kèm tệp th… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 10 — giữ nguyên văn **5** · bổ sung 1 · sửa 4 |
| **Giữ nguyên văn** | Phải giữ NGUYÊN VĂN, không diễn đạt lại: (1) toàn bộ Bước 2 — định dạng hai dòng BẢNG/DETAIL, ba dấu phân cách được chấp nhận, và quy tắc thứ tự gửi bằng thứ tự hiển thị; đây là lõi và là lý do kỹ năng tồn tại. (2) Mục Đích. (3) Bảng mẫu STT ở Bước 1 (khung trình bày, chỉ thêm ràng buộc lọc bên dưới, không sửa khung). (4) Toàn bộ mục Ví Dụ, cả phần đầu vào lẫn phần đầu ra. (5) Gạch đầu dòng ở mục Lưu Ý nêu kỹ năng không áp dụng cho Wizard/Form — đây là ranh giới phạm vi đúng và hiếm, tuyệt đối không bỏ. (6) Ba mục… |
| **Cần sửa** | Bốn điểm, tất cả đều có bằng chứng đo được, tất cả đều SỬA TẠI CHỖ, không xoá dòng nào. C1 — Mục Lưu Ý, gạch đầu dòng về bốn trường nhật ký. Kỹ năng khẳng định chúng LUÔN ẩn theo chuẩn. Bằng chứng ngược: docs/UI-STANDARD.md dòng 221 (chân nhật ký là thành phần bắt buộc của panel chi tiết, có quy định lớp trình bày riêng), dòng 305 (công thức panel chi tiết liệt kê chân nhật ký ở cuối), mục 20.6 dòng 468–473 (hộp thoại "Thông Tin Audit" mở bằng icon History cạnh mã dòng, hiển thị đúng bốn trường, kèm quy định định … |
| **Cần thêm** | A1 — Mục "Khi Nào Dùng" (tiêu đề đặt đúng dạng bộ dò nhận ra được, ví dụ bắt đầu bằng chữ "Khi nào" hoặc "Áp dụng khi"). Nội dung là các tín hiệu bắt buộc ở mục positive_signals. Đây là điều kiện kỹ thuật để nhãn sức khoẻ cấu trúc thoát khỏi NO-TRIGGER: bộ dựng danh mục chỉ công nhận điều kiện kích hoạt khi tệp có TIÊU ĐỀ khớp mẫu, tệp hiện có 8 tiêu đề và không tiêu đề nào khớp. A2 — Mục "Khi Nào KHÔNG Dùng" (tiêu … |
| Chuyển xuống lịch sử | Chuyển xuống mục "Lịch Sử Sửa Đổi" MỚI, nằm trong CHÍNH tệp kỹ năng (không dời sang tệp lưu trữ nào), gồm đúng bốn hàng, mỗi hàng chép NGUYÊN VĂN dòng cũ kèm ngày · lý do · vị trí mục mới: Hàng 1 — nguyên văn dòng mô tả cũ ở khối đầu tệp. Lý do: mô tả không có mệnh đề điều kiện nên máy không biết k… |
| Tín hiệu bắt buộc để nạp | Phải quan sát được ĐỒNG THỜI cả ba thì mới nạp: (1) Chủ dự án nêu đích danh MỘT màn hình dạng DANH SÁCH + PANEL CHI TIẾT đã tồn tại (tệp thành phần phía trình duyệt của một trang trong src/app, dạng <tên>-client.tsx), không phải màn hình sắp dựng mới. (2) Yêu cầu nói về CỘT NÀO ĐƯỢC HIỆN và HIỆN THEO THỨ TỰ NÀO — các … |
| KHÔNG nạp khi | KHÔNG nạp khi có bất kỳ điều nào sau đây: (1) Đối tượng là Wizard hoặc Form nhập liệu (ranh giới đã có sẵn trong tệp, giữ nguyên) — thứ tự trường của biểu mẫu thuộc mục 11 và 11.1 của chuẩn. (2) Yêu cầu là thêm/bớt/đổi tên CỘT TRONG CƠ SỞ DỮ LIỆU, đổi kiểu dữ liệu, đổi khoá — đó là việc lược đồ, phải qua Cổng Lược Đồ … |
| Ca gần giống dễ nhầm | Ca gần giống nguy hiểm nhất: yêu cầu "thêm cột … vào bảng …" mà thứ "bảng" ở đây là BẢNG DỮ LIỆU trong cơ sở dữ liệu chứ không phải bảng hiển thị trên màn hình. Hai loại yêu cầu dùng chung y hệt hai từ khoá "thêm cột" và "bảng", nhưng: kỹ năng này chỉ đổi thứ… |
| Phạm vi đường dẫn | ĐỀ XUẤT CÓ giới hạn phạm vi đường dẫn, vì kỹ năng này dạy SỬA TỆP thật nên phạm vi là hàng rào an toàn chính. Được phép chạm: src/app/<mô-đun>/<màn>/<tên>-client.tsx — tức tệp thành phần phía trình duyệt dựng bảng danh sách và panel chi tiết của một trang. Bằ… |
| Ánh xạ mục cũ → mới | Ánh xạ từng mục, KHÔNG mục nào biến mất, KHÔNG mục nào bị dời ra khỏi tệp: 1. Khối đầu tệp (tên · mô tả · phiên bản) → giữ nguyên vị trí, riêng dòng mô tả được thay tại chỗ bằng bản có mệnh đề điều kiện; dòng mô tả cũ xuống mục Lịch Sử Sửa Đổi. 2. Tiêu đề chính → giữ nguyên tại chỗ. 3. Mục Đích → giữ nguyên tại chỗ. 4. (MỚI) Khi Nào Dùng → chèn ngay sau Mục Đích. Không thay thế mục nào. 5. (MỚI) Khi Nào KHÔNG Dùng →… |
| **Chứng minh bảo toàn** | Đếm mục có ý nghĩa: TRƯỚC = 10 (khối đầu tệp · tiêu đề chính · Mục Đích · Workflow · Bước 1 · Bước 2 · Bước 3 · Checklist · Ví Dụ · Lưu Ý). SAU = 14 (thêm Khi Nào Dùng · Khi Nào KHÔNG Dùng · Cổng SSOT bắt buộc · Lịch Sử Sửa Đổi). 14 ≥ 10, không mục nào bị xoá, không mục nào bị dời sang tệp lưu trữ khác — thoả điều kiện thứ nhất của GOV-EDIT-PRESERVE-001. Đếm bằng thước máy: số… |
| Kiểm tĩnh | Chạy TRƯỚC và SAU khi sửa, so kết quả: (1) Đếm mục: đếm số dòng mở đầu bằng dấu thăng kép trong tệp kỹ năng. Kỳ vọng 8 → 12. Đây là phép kiểm bảo toàn theo GOV-EDIT-PRESERVE-001. (2) npm run test:standard-clause-count -- docs/UI-STANDARD.md — chuẩn giao diện KHÔNG bị đụng trong … |
| Kiểm động | Chạy thử THẬT trên đúng MỘT màn hình danh sách, ở máy phát triển, sau khi đã sửa. Bốn phép kiểm NGƯỢC — mỗi phép phải cho kết quả CHẶN, chặn được mới tính là đạt: (1) Kiểm ngược cột bị cấm: cố tình đưa cột "Thao Tác" vào bảng liệt kê ở Bước 1 và chọn nó ở Bước 2. Agent PHẢI dừng… |
| Đường lui | Đơn giản và trọn vẹn, vì lượt sửa chỉ đụng đúng MỘT tệp văn bản. (1) Tệp kỹ năng ĐÃ được Git theo dõi (đã xác nhận bằng lệnh liệt kê tệp của Git ở lượt đọc này), nên khôi phục về bản trước chỉ cần một lệnh khôi phục tệp của Git trên đúng đường dẫn tương đối của tệp đó. Không cần… |
| ⭐ **Cần anh duyệt** | Câu hỏi chính cần Chủ dự án duyệt (một câu, ba ý, trả lời DUYỆT hoặc KHÔNG cho từng ý): "Chủ dự án có duyệt SỬA TẠI CHỖ bốn dòng trong kỹ năng chọn cột — (1) bỏ khẳng định bốn trường nhật ký luôn ẩn, ghi lại cho đúng là ẩn khỏi CỘT bảng nhưng BẮT BUỘC hiện ở chân panel chi tiết và ở hộp thoại nhật ký trên dòng; (2) thay lời khuyên điều chỉnh bề ngang bảng theo số cột bằng giá trị chuẩn cố định của dự án cộng cơ chế ẩn cột theo bề ngang màn hình; (3) bổ sung lệnh cấm đưa cột 'Thao Tác' vào bảng danh sách kèm yêu cầu khai rõ nếu có ngoại lệ; (4) thay bước tạo bản sao lưu tệp bằng cách dựa vào Git để phục hồi — ĐỒN… |

### `schema-visualization`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Khuôn TRÌNH BÀY: khi Chủ dự án muốn nhìn toàn cảnh một thực thể ERP, kỹ năng buộc agent xuất ba bảng markdown song song (lược đồ CSDL · cột bảng hiển thị · trường biểu mẫu) kèm logic, để Chủ dự án ra lệnh ẩn/hiện/sắp xếp cột chính xác thay vì mô tả mơ hồ. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R0 chỉ-đọc** |
| Vì sao lớp đó | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: 'đọc lược đồ và mã giao diện của một thực thể, rồi in ra ba bảng markdown trong câu trả lời'. Không có hành động nào cao hơn thế trong tệp. Bằng chứng đo trong lượt này, không chép từ dữ kiện có sẵn: · Grep toàn tệp các động từ ghi và triển khai (thêm bản ghi · cập nhật · xoá · bỏ bảng · sửa lược đồ · tạo bảng · di trú · commit · đẩy mã · triển khai · lệnh cài đặt · … |
| **Chế độ định tuyến hôm nay** | **REFERENCE_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 15 — giữ nguyên văn **8** · bổ sung 5 · sửa 1 |
| **Giữ nguyên văn** | GIỮ NGUYÊN VĂN, KHÔNG ĐỤNG MỘT KÝ TỰ — nêu đích danh: (1) toàn bộ bốn khối mẫu bảng markdown: dòng 25–38 (lược đồ CSDL, khuôn bảy cột), dòng 42–54 (cột bảng hiển thị, khuôn sáu cột), dòng 58–72 (trường biểu mẫu, khuôn sáu cột, kèm bốn dòng trường kiểm toán ẩn), dòng 76–87 (luồng logic: sinh mã tự động · ràng buộc loại trừ · ràng buộc đi kèm) — đây là toàn bộ giá trị sử dụng của kỹ năng; (2) mục Mục tiêu dòng 9–14; (3) ba tín hiệu dương ở mục Trigger dòng 17–19 (chỉ THÊM điều kiện âm phía dưới, không sửa ba dòng nà… |
| **Cần sửa** | CÓ — bốn điểm, mỗi điểm kèm bằng chứng đo được trong lượt này: (1) SAI NHÃN TRONG DANH MỤC — điểm nặng nhất, và nó nằm NGOÀI tệp kỹ năng. Danh mục `.governance/registry/skills.yml` (dòng 3186–3199) ghi cho kỹ năng này: ui_scope = KHONG_UI · ssot_verdict = KHONG_UI · ssot_diem_choi = 0 · ghi chú 'bộ dò không thấy nội dung giao diện'. Nhưng thân kỹ năng ĐO ĐƯỢC là có nội dung giao diện: hai tiêu đề khối mẫu ghi thẳng chữ bảng hiển thị và biểu mẫu nhập liệu (dòng 43 và 59); dòng 47–50 quy định ba mức độ rộng cột bằng… |
| **Cần thêm** | THÊM ba mục mới, không đụng mục cũ: (A) Mục mới 'Nguồn lược đồ bắt buộc — đọc TRƯỚC khi xuất bảng', đặt ngay sau mục Trigger. Nội dung: liệt kê ba nơi phải đọc để dựng bảng lược đồ (thư mục di trú · lớp lưu trữ · thư mục khai kiểu dữ liệu), và ba nơi phải đọc để dựng hai bảng giao diện (tệp trang danh sách của mô-đun · tệp biểu mẫu · chuẩn giao diện dự án `docs/UI-STANDARD.md`). Kèm ba câu cấm: CẤM chép giá trị từ b… |
| Chuyển xuống lịch sử | CHUYỂN XUỐNG MỤC LỊCH SỬ TRONG CHÍNH TỆP (không dời sang tệp khác, không xoá): 1. Mục 'Reference' (dòng 141–142): đổi tên mục thành 'Lịch sử & Nguồn gốc'; dòng 142 — dòng ghi phiên làm việc gốc mang mốc tháng 01/2026 — được giữ NGUYÊN VĂN, không sửa một ký tự, chỉ đứng dưới tiêu đề mới. Kèm thêm mộ… |
| Tín hiệu bắt buộc để nạp | Phải quan sát được ĐỦ BỐN tín hiệu thì mới nạp, thiếu một là không nạp: (1) Chủ dự án nêu ĐÍCH DANH MỘT thực thể hoặc mô-đun ERP (ví dụ: một danh mục nghiệp vụ cụ thể) — không phải nói chung chung về 'hệ thống'. (2) Yêu cầu là XEM TOÀN CẢNH, không phải tra một trường: xuất hiện cụm mang nghĩa 'toàn bộ' hoặc 'đầy đủ' h… |
| KHÔNG nạp khi | KHÔNG nạp khi có bất kỳ điều nào sau: (1) Chỉ cần biết một đến hai trường — chính kỹ năng đã tự nêu ở dòng 139 là đọc mã trực tiếp nhanh hơn. (2) Yêu cầu là THI HÀNH chứ không phải xem: ẩn cột, đổi thứ tự cột, đổi độ rộng, thêm cột vào bảng hiển thị, thêm trường vào biểu mẫu. Kỹ năng này DỪNG ở trình bày; việc thi hàn… |
| Ca gần giống dễ nhầm | Nêu đích danh: yêu cầu dạng 'ẩn cột <tên cột> ở bảng <màn hình>' hoặc 'sắp xếp lại cột theo thứ tự 1-3-2'. Ca này dùng CHUNG gần hết từ khoá với kỹ năng đang xét — có chữ cột, có chữ bảng, thậm chí xuất hiện nguyên văn trong mục Ưu điểm dòng 124 của chính kỹ … |
| Phạm vi đường dẫn | CẦN khai phạm vi đường dẫn, có bằng chứng: grep toàn tệp các từ khoá nguồn (thư mục di trú · lớp lưu trữ · thư mục kiểu dữ liệu · truy vấn bảng hệ thống) cho 0 kết quả — tức kỹ năng hiện KHÔNG nói mình áp cho vùng nào và lấy dữ liệu ở vùng nào. Phạm vi đề xuấ… |
| Ánh xạ mục cũ → mới | Ánh xạ từng mục, 15 mục cũ đều có đích đến: 1. Khối đầu tệp (dòng 1–5) → giữ tại chỗ, trường tên và phiên bản giữ nguyên văn; mô tả được NỐI THÊM mệnh đề điều kiện (dòng cũ chép nguyên văn xuống mục lịch sử); thêm ba trường mới bên dưới. 2. Tiêu đề cấp một (dòng 7) → giữ nguyên tại chỗ. 3. Mục tiêu (9–14) → giữ nguyên tại chỗ. 4. Trigger (16–19) → giữ nguyên tại chỗ, ba dòng tín hiệu dương không đụng; THÊM mục con đ… |
| **Chứng minh bảo toàn** | ĐẾM MỤC: · TRƯỚC = 15 mục có nghĩa. Cách đếm, kiểm lại được: 14 dòng tiêu đề nằm NGOÀI khối mã (các dòng 7, 9, 16, 21, 23, 40, 56, 74, 89, 114, 122, 127, 131, 141) cộng 1 khối đầu tệp. Bảy dòng trông như tiêu đề ở các dòng 26, 43, 59, 77, 94, 100, 106 nằm BÊN TRONG khối mã nên là nội dung ví dụ, không tính là mục — nếu đếm gộp cả chúng thì con số là 22, đã nêu cả hai cách đếm … |
| Kiểm tĩnh | Bảy phép thử tĩnh, chạy được mà không cần khởi động hệ thống: 1. Đếm mục theo tên: liệt kê tiêu đề ngoài khối mã TRƯỚC và SAU, đối chiếu theo TÊN chứ không chỉ theo tổng. Đạt = 15 tên cũ đều còn mặt, cộng 2 tên mới. (Đối chiếu theo tên là bắt buộc — §G7.0 đã ghi rõ đếm gộp che đ… |
| Kiểm động | Sáu phép thử động, phải chạy trên phiên thật: 1. PHÉP THỬ NẠP (Cursor): mở phiên mới, hỏi một câu mang đủ bốn tín hiệu dương về một thực thể ERP có thật. Ghi lại kỹ năng nào được nạp. Đạt = nạp đúng kỹ năng này. Hiện CHƯA ĐO lần nào — đây là số liệu còn trống, phải điền trước kh… |
| Đường lui | Lượt này KHÔNG sửa gì — đây là kế hoạch trên giấy, nên hiện chưa có gì phải lùi. Khi thi hành, đường lùi như sau: 1. CHỐT MỐC TRƯỚC KHI ĐỘNG VÀO: ghi lại 5389 byte · 142 dòng · 15 tên mục · kết quả sáu lệnh grep đã dùng trong lượt này. Không có mốc thì không có cách chứng minh đ… |
| ⭐ **Cần anh duyệt** | Ba câu hỏi, xin Chủ dự án trả lời CÓ hoặc KHÔNG cho từng câu: CÂU 1 (quan trọng nhất, chặn hai câu còn lại): "Danh mục kỹ năng đang ghi kỹ năng này là KHÔNG THUỘC GIAO DIỆN, trong khi thân kỹ năng có quy định trình bày đo được — ba mức độ rộng cột bằng điểm ảnh ở dòng 47–50, quy ước phông chữ và biểu tượng ở dòng 47, số dòng ô văn bản ở dòng 67 — và toàn tệp có 0 lần trỏ về chuẩn giao diện dự án. Chủ dự án có duyệt SỬA NHÃN thành CÓ THUỘC GIAO DIỆN, kèm bắt buộc đối chiếu chuẩn giao diện dự án không?" Vì sao phải hỏi: sửa nhãn xong thì cổng đối chiếu xung đột nhãn giao diện sẽ bắt đầu soi kỹ năng này, và có thể … |

### `detail-panel-toggle`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Dạy cách dựng màn danh mục có bảng bên trái và bảng chi tiết bên phải, trong đó bấm lại đúng dòng đang chọn thì đóng bảng chi tiết. |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R1 sửa mã/tệp** |
| Vì sao lớp đó | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: sửa tệp mã nguồn giao diện của một trang danh mục — cụ thể là (a) viết lại khung lưới bao ngoài của cả trang, (b) thêm và sửa trạng thái đang-chọn của trang, (c) gắn lại trình xử lý sự kiện bấm vào từng dòng của bảng dữ liệu, (d) dựng mới toàn bộ khối bảng chi tiết gồm đầu bảng, cụm nút sửa/xoá/đóng và các thẻ mục trong thân. Đây là GHI ĐÈ TỆP MÃ NGUỒN, nên tối thiểu… |
| **Chế độ định tuyến hôm nay** | **REFERENCE_ONLY** |
| Trần sau khi qua kiểm thử | PROMPT_REQUESTED |
| Số mục nội dung | 13 — giữ nguyên văn **7** · bổ sung 2 · sửa 4 |
| **Giữ nguyên văn** | GIỮ NGUYÊN VĂN, nêu đích danh năm phần: (1) Toàn bộ mục Hàm bật/tắt bảng chi tiết — khớp nguyên văn khối mẫu ở mục 20.2 của chuẩn giao diện, kể cả nhánh trả về sớm khi bấm lại đúng dòng đang chọn mà chuẩn ghi là BẮT BUỘC PHẢI CÓ; đây là lý do duy nhất kỹ năng này còn giá trị sau đợt gộp. (2) Ba dòng ở mục Điều kiện dùng — đã được thu hoạch nguyên văn thành ba dòng trường triệu chứng trong sổ kỹ năng, sửa là lệch sổ. (3) Sáu dòng bảng kiểm đạt/không đạt — chuẩn giao diện KHÔNG có bảng kiểm hành vi nào tương đương, … |
| **Cần sửa** | CÓ — sáu điểm, tất cả đều SỬA TẠI CHỖ, kèm bằng chứng đo được: 1. Lưới bố cục (dòng 17, 19, 25). Kỹ năng: lưới năm cột, bảng ba cột khi mở và năm cột khi đóng, cột chi tiết hai cột, khoảng cách 6. Chuẩn mục 10 kiểu A: lưới hai vế, cột chi tiết CỐ ĐỊNH 380 điểm ảnh, khoảng cách 4. Bằng chứng: điểm xung đột số 6 trong bảng phân xử ở mục 20.7 đã chốt bên thắng là chuẩn, lý do ghi rõ là cột chi tiết chia theo tỉ lệ sẽ phình quá rộng ở màn hình lớn. 2. Bốn điều bắt buộc của cột chi tiết bị thiếu HOÀN TOÀN (mục 10.0, Ch… |
| **Cần thêm** | 1. Một dòng con trỏ chuẩn ngay dưới tiêu đề: nguồn thi hành là chuẩn giao diện mục 10, 10.0 và 20.2; khi tệp này lệch chuẩn thì LUÔN theo chuẩn. Hiện tệp không có dòng nào như vậy. 2. Một câu cảnh báo chép giá trị: các giá trị trình bày trong tệp chỉ để hiểu bối cảnh, CẤM chép sang mã. Đây là đúng cách đọc của loại nhãn 'chọi chuẩn' mà bộ luật đã quy định. 3. Trường điều kiện kích hoạt máy đọc được và trường điều ki… |
| Chuyển xuống lịch sử | BẢY dòng cũ chuyển xuống mục 'Lịch sử sửa đổi' thêm mới ở CUỐI CHÍNH TỆP kỹ năng (không dời sang thư mục lưu trữ, vì luật sửa bảo toàn cho phép cả hai cách và cách này giữ được ngữ cảnh tại chỗ). Mỗi dòng ghi đủ bốn thứ: nội dung cũ, vị trí cũ, ngày sửa, lý do sửa. 1. Câu mô tả cũ trong khối khai b… |
| Tín hiệu bắt buộc để nạp | Chỉ nạp khi quan sát được ĐỦ CẢ NĂM tín hiệu sau, thiếu một là không nạp: 1. Yêu cầu nêu đích danh MỘT trang danh mục kiểu liệt-kê-và-xem-chi-tiết đang có, hoặc sắp có, bảng chi tiết đặt BÊN PHẢI ngay trong lưới (kiểu A của chuẩn mục 10) — không phải trang tổng quan, không phải trang biểu đồ, không phải trang thiết lậ… |
| KHÔNG nạp khi | KHÔNG nạp, hoặc nạp rồi phải dừng, khi gặp bất kỳ điều nào sau: 1. Màn thuộc loại hồ sơ nhiều trường, chi tiết cần cả bề ngang, dùng bảng chi tiết kiểu trượt phải (kiểu B của chuẩn mục 10 — mẫu là màn nhân sự). Kỹ năng này chỉ nói về kiểu A. 2. Việc cần làm thuộc phần TRÌNH BÀY của bảng chi tiết: bo góc, màu, chuyển s… |
| Ca gần giống dễ nhầm | Kỹ năng anh em `detail-panel-layout` — đây là ca dễ nạp nhầm nhất, nêu đích danh. Lý do dễ nhầm: cùng lĩnh vực bố cục giao diện, tên chỉ khác một từ cuối, và mô tả của nó cũng có cụm 'toggle row' nên tìm theo từ khoá sẽ ra cả hai. Nhưng hai kỹ năng khác hẳn n… |
| Phạm vi đường dẫn | ĐỀ XUẤT CÓ giới hạn phạm vi đường dẫn, và ghi thẳng vào khối khai báo đầu tệp kỹ năng. ĐƯỢC ÁP: tệp giao diện của trang danh mục kiểu liệt-kê-và-xem-chi-tiết nằm dưới cây trang của các mô-đun, tức đường dẫn tương đối dạng src/app/<mã mô-đun>/<tên trang>/<tên … |
| Ánh xạ mục cũ → mới | Ánh xạ TỪNG mục, 13 mục cũ → 17 mục mới. Không mục nào rời khỏi tệp. 1. Khối khai báo đầu tệp (tên + mô tả) → GIỮ TẠI CHỖ, mở rộng: giữ nguyên trường tên; trường mô tả được viết lại có mệnh đề điều kiện, câu mô tả cũ xuống mục Lịch sử sửa đổi; THÊM trường điều kiện kích hoạt và trường điều kiện không-áp-dụng. 2. Tiêu đề kỹ năng → giữ nguyên tại chỗ. 2b. (MỤC MỚI, chèn ngay dưới tiêu đề) Con trỏ chuẩn + câu cảnh báo … |
| **Chứng minh bảo toàn** | ĐẾM MỤC: trước khi sửa có 13 mục có nghĩa (đo bằng lệnh liệt kê tiêu đề: 1 khối khai báo đầu tệp, 1 tiêu đề, 11 mục thân). Sau khi sửa có 17 mục — 13 mục cũ đều còn nguyên vị trí, cộng 4 mục mới (con trỏ chuẩn, điều kiện không-áp-dụng, bốn điều bắt buộc cột chi tiết, lịch sử sửa đổi). 17 lớn hơn 13, nên điều kiện thứ nhất của cổng đếm hai điều kiện trong luật sửa bảo toàn ĐẠT.… |
| Kiểm tĩnh | Chín phép kiểm tĩnh, tất cả đọc đầu vào THẬT chứ không chạy trên chuỗi mẫu viết cứng: 1. Tìm chuỗi lớp bo góc bị cấm trong tệp kỹ năng → phải ra 0 kết quả. Trước khi sửa đo được 1 kết quả tại dòng 93, nên đây là phép kiểm có khả năng thất bại thật. 2. Cổng đối chiếu nhãn kỹ năng… |
| Kiểm động | Bảy phép kiểm động — LƯU Ý: lượt này là lượt chỉ đọc nên KHÔNG phép nào được chạy, đây là đặc tả cho phiên sau. 1. Bốn ca hành vi bấm dòng, đúng bảng bốn thao tác ở chuẩn mục 20.2: bấm dòng khi chưa mở thì mở đúng dòng đó; bấm LẠI đúng dòng đang chọn thì ĐÓNG; bấm dòng khác thì … |
| Đường lui | Phiếu này là KẾ HOẠCH TRÊN GIẤY, lượt hiện tại không sửa gì nên chưa có gì để hoàn tác. Đặc tả hoàn tác cho phiên thi hành sau: TRƯỚC KHI SỬA: tệp kỹ năng đang được git theo dõi, kích thước 127 dòng và 3550 byte — ghi lại hai con số này làm mốc. Không cần sao lưu tay vì bản gốc … |
| ⭐ **Cần anh duyệt** | CÂU HỎI CHÍNH XÁC CẦN CHỦ DỰ ÁN DUYỆT (một câu, hai lựa chọn, chọn một): "Kỹ năng `detail-panel-toggle` KHÔNG nằm trong danh sách mười một kỹ năng giao diện đã gộp vào chuẩn ngày 18/08/2026 — hàng S8 của sổ nhãn nguồn chuẩn liệt kê kỹ năng anh em `detail-panel-layout` chứ không có kỹ năng này — nên tới hôm nay nó là kỹ năng giao diện DUY NHẤT trong nhóm này chưa hề có nhãn hiệu lực, trong khi đo được nó chọi chuẩn ở sáu điểm và còn mang một lớp bo góc mà chuẩn đã cấm đích danh. Câu lệnh 'thư mục kỹ năng giữ nguyên làm lưu trữ, không xoá không sửa' được viết cho MƯỜI MỘT kỹ năng đã gộp, nên nó CÓ áp cho kỹ năng n… |

### `update-work-log`

| Mục | Nội dung |
|---|---|
| **Việc kỹ năng làm** | Hướng dẫn PHỤ TRỢ để chèn một mục mới vào ĐẦU tệp sổ công việc `WORK_LOG.md` sao cho không đè mất mục của phiên khác — KHÔNG phải quy trình đánh số phiên bản đầy đủ (việc đó thuộc `versioning-auto-log`). |
| **Quyền lúc bắt đầu** | chỉ đọc |
| **Rủi ro tối đa** | **R3 phát hành** |
| Vì sao lớp đó | Nêu đích danh chuỗi hành động cao nhất mà kỹ năng DẠY agent làm, theo đúng thứ tự trong tệp: · SÀN RỦI RO — R1: bước 3 dạy CHÈN một khối nội dung vào đầu `WORK_LOG.md`, và bước 5 dạy xử lý tình huống đè mất mục của phiên khác. Đây là sửa tệp thật, tệp lại ĐƯỢC GIT THEO DÕI (đã chứng minh bằng lệnh). Chỉ riêng điều này đã loại bỏ hoàn toàn khả năng xếp R0 — dù kỹ năng KHÔNG kèm tệp thi hành nào (đo: thư mục chỉ có đú… |
| **Chế độ định tuyến hôm nay** | **EXPLICIT_ONLY** |
| Trần sau khi qua kiểm thử | EXPLICIT_ONLY |
| Số mục nội dung | 13 — giữ nguyên văn **7** · bổ sung 4 · sửa 2 |
| **Giữ nguyên văn** | Giữ NGUYÊN VĂN, không đụng một chữ: (a) mục Tóm tắt dòng 15–16; (b) toàn bộ mục Điều kiện kích hoạt dòng 18–25 — đặc biệt câu «điều cần tránh», vì danh mục kỹ năng đang lấy đúng chuỗi đó làm điều kiện kích hoạt âm, sửa chữ là lệch danh mục; (c) mục Điều kiện ĐẠT / HỎNG dòng 57–67; (d) mục Lợi ích dòng 69–71; (e) mục Rủi ro dòng 73–74; (f) mục Nên dùng / Nên tránh dòng 76–82; (g) HAI mục cũ trong Lịch sử sửa đổi kỹ năng (bản 1.1.0 và bản đầu, dòng 94–99) — kể cả bốn dòng lý do/nội dung/tác động/cách kiểm của bản 1.… |
| **Cần sửa** | CÓ — đúng HAI điểm, cả hai đều sửa TẠI CHỖ, không xoá dòng nào. 【E1】 Khối đầu tệp, dòng 10 — khoá khai «bị thay thế bởi versioning-auto-log». BẰNG CHỨNG: (1) ngay dòng trên nó (dòng 9) khai trạng thái là «bản tham chiếu hỗ trợ» — hai nhãn này loại trừ nhau; (2) thân bài dòng 16 nói kỹ năng ĐƯỢC GIỮ LẠI và giao cho nó phần việc riêng, dòng 28–37 vẫn là một quy trình 5 bước có việc thật ⇒ nếu đã bị thay thế thì không thể còn quy trình thi hành; (3) hai bản ghi độc lập trong kho mô tả đúng lần chuyển vai này và cả ha… |
| **Cần thêm** | 【A1 — mục MỚI: «Neo quản trị & cổng kiểm»】 Lý do: thân kỹ năng hiện có 0 lần nhắc bất kỳ mã luật quản trị nào, trong khi danh mục kỹ năng đã gắn sẵn cho nó một mã luật và một cổng (đo được trong `.governance/registry/skills.yml`, khối của kỹ năng này). Nội dung thêm, mỗi ý một dòng: (a) ghi lịch sử KHÔNG đồng nghĩa tăng số phiên bản — trỏ §I3; (b) hình dạng mã phiên bản theo quy tắc đánh số chốt 11/08/2026, tra tại … |
| Chuyển xuống lịch sử | Chuyển xuống mục «Lịch sử sửa đổi kỹ năng» ngay trong CHÍNH tệp (không dời sang kho lưu), gói trong một mục mới đề bản 1.2.0, gồm ĐÚNG HAI dòng con trỏ: · Con trỏ E1 — chép NGUYÊN VĂN cặp khoá-giá trị cũ ở dòng 10 của bản trước (khoá khai bị-thay-thế cùng tên kỹ năng được trỏ tới), kèm: vị trí cũ «… |
| Tín hiệu bắt buộc để nạp | Phải quan sát được ĐỦ hai tín hiệu 1 và 2 mới được nạp; tín hiệu 3 làm tăng độ ưu tiên nhưng không thay thế được 1 và 2. 1. ĐÃ CÓ một thay đổi THẬT vừa hoàn tất trong phiên (có tệp bị sửa hoặc có việc quản trị vừa khép) VÀ bước phân loại thay đổi của bộ thi hành chính `versioning-auto-log` ĐÃ chạy xong, tức đã có kết … |
| KHÔNG nạp khi | Đo được ba điều đầu ngay trong chính tệp (mục Điều-cần-tránh dòng 24–25, mục Nên-tránh dòng 81–82, điều kiện HỎNG thứ ba dòng 67) và trong khối kích hoạt âm của danh mục kỹ năng. Hai điều cuối là bổ sung theo phép đo lượt này. 1. Yêu cầu là truy vết ĐẦY ĐỦ, quyết định TĂNG SỐ PHIÊN BẢN, hoặc viết nhật ký thay đổi cho … |
| Ca gần giống dễ nhầm | ĐÍCH DANH ca dễ nạp nhầm nhất: yêu cầu «ghi nhật ký thay đổi cho đợt phát hành này» / «cập nhật changelog». Cùng từ khoá ghi-nhật-ký, cùng lĩnh vực đánh số phiên bản, cùng cụm bốn kỹ năng, nhưng KHÔNG thuộc phạm vi — vì nhật ký thay đổi theo đợt phát hành đan… |
| Phạm vi đường dẫn | CẦN có phạm vi đường dẫn, và phạm vi phải HẸP hơn hiện trạng vì lượt này đo được năm sổ khác dễ bị nhầm. · Phạm vi ĐƯỢC GHI: đúng MỘT tệp — `WORK_LOG.md` ở gốc kho. Không có tệp thứ hai. · Phạm vi ĐƯỢC ĐỌC (chỉ đọc, không ghi): `src/lib/version.ts` (lấy quy t… |
| Ánh xạ mục cũ → mới | Mười ba đơn vị nội dung trước → mười lăm đơn vị sau. Ánh xạ từng cái: 1. Khối đầu tệp (6 khoá) → GIỮ NGUYÊN VỊ TRÍ, đúng một khoá đổi tên tại chỗ (khai-bị-thay-thế → bộ-thi-hành-chính), số hiệu kỹ năng nâng 1.1.0 → 1.2.0 tại chỗ. Vẫn 6 khoá, không thêm không bớt. 2. Tiêu đề chính → giữ nguyên tại chỗ. 3. Tóm tắt → giữ nguyên tại chỗ, nguyên văn. 4. Điều kiện kích hoạt + điều cần tránh → giữ nguyên tại chỗ, nguyên vă… |
| **Chứng minh bảo toàn** | ĐẾM MỤC — trước: 13 đơn vị nội dung (1 khối đầu tệp + 1 tiêu đề chính + 11 mục cấp hai: Tóm tắt · Điều kiện kích hoạt · Quy trình · Khuôn khối kết xuất · ĐẠT-HỎNG · Lợi ích · Rủi ro · Nên dùng-Nên tránh · Ca kiểm tối thiểu · Ghi chú kết nối · Lịch sử sửa đổi). Sau: 15 đơn vị (thêm A1 và A2). SAU ≥ TRƯỚC, chênh +2. Không mục nào bị xoá ⇒ điều kiện thứ nhất của cổng đếm hai-điều… |
| Kiểm tĩnh | Chạy TRƯỚC khi sửa để lấy mốc, rồi chạy LẠI sau khi sửa, so hai lần: 1. Đếm mục cấp hai của chính tệp kỹ năng: `grep -c '^## ' .cursor/skills/update-work-log/SKILL.md` → mốc 11, sau phải là 13. SAU ≥ TRƯỚC. 2. Cổng phụ đếm mục theo TỪNG TỆP và theo TÊN MỤC: `npm run test:standar… |
| Kiểm động | Năm ca, mỗi ca nêu rõ CĂN CỨ ĐẠT: · Ca 1 — đúng phạm vi. Yêu cầu: «ghi một mục sổ công việc cho việc vừa làm xong». ĐẠT khi: kỹ năng chạy bước 1 trước (có kết luận loại thay đổi), đọc lại tệp từ đĩa, chèn khối mới ở ĐẦU tệp, mã phiên bản đúng dạng ba tầng. Căn cứ: mục cũ vốn ở đ… |
| Đường lui | Toàn bộ thay đổi nằm trong ĐÚNG MỘT tệp được git theo dõi, và bản chất là sửa-tại-chỗ + thêm mục, không xoá dòng nào ⇒ hoàn tác không làm mất nội dung nào. Trình tự hoàn tác, ba bước: 1. Khôi phục tệp kỹ năng về bản ngay trước lần sửa từ lịch sử phiên bản của chính tệp (thao tác… |
| ⭐ **Cần anh duyệt** | Ba câu hỏi, câu 1 là câu CHẶN — chưa có câu trả lời thì không được đổi đối tượng của kỹ năng. 【Câu 1 — CHẶN】 «Từ ngày 11/08/2026 tới nay, tệp `WORK_LOG.md` không có thêm mục mới nào (mục mới nhất đề ngày 11/08/2026), trong khi nhật ký thay đổi theo từng đợt phát hành đang được ghi nội tuyến trong `src/lib/version.ts` và đã chạy tới đợt ngày 01/09/2026; ngoài ra tài liệu quy trình phát hành và triển khai không nhắc tới `WORK_LOG.md` lần nào. Con đã tra sổ yêu cầu của Chủ dự án và sổ nợ kỹ thuật trước khi hỏi, KHÔNG thấy quyết định nào về việc chuyển sổ. Xin Chủ dự án chốt MỘT trong ba: (A) `WORK_LOG.md` VẪN là sổ… |


---

## 7. HAI KỸ NĂNG ĐANG BỊ KHOÁ — PHƯƠNG ÁN, CHƯA ÁP DỤNG

Hai kỹ năng `detail-panel-layout` và `annotated-screenshot-review` bị Chủ dự án khoá ngày 23/08/2026: *"giữ nguyên làm lưu trữ — không xoá, không sửa"*.

**Việc quyết định phải CHỜ kết quả đo Cursor**, vì câu hỏi cốt lõi là: *Cursor có tự nạp chúng bất chấp nhãn trong sổ đăng ký không?* Chưa đo thì chưa trả lời được.

| Phương án | Làm gì | Ưu | Nhược | Điều kiện tiên quyết |
|---|---|---|---|---|
| **(a) Giữ nguyên + cảnh báo** | Không đụng tệp; thêm một dòng cảnh báo ở sổ đăng ký | Tôn trọng tuyệt đối quyết định đã khoá; công sức gần bằng không | Nếu Cursor tự nạp thì rủi ro chép nhầm **vẫn còn nguyên** | Cần biết Cursor có tự nạp không |
| **(b) Ngoại lệ hẹp** | Mở ngoại lệ CHỈ để gỡ chuỗi bị cấm, không đụng gì khác | Gỡ đúng phần nguy hiểm, giữ 100% phần còn lại | Đụng vào một quyết định đã khoá — cần anh cho phép rõ ràng | Anh mở ngoại lệ |
| **(c) Cất khỏi tầm phát hiện** | Chuyển sang nơi công cụ không quét, sinh bản chiếu cho kỹ năng đủ điều kiện | Chặn tận gốc, không xoá chữ nào | Đổi cấu trúc thư mục; cần cổng chống lệch bản | Canary đạt + anh duyệt kiến trúc |

**Nếu canary cho thấy Cursor KHÔNG tự nạp** ⇒ phương án (a) là đủ.
**Nếu canary cho thấy Cursor CÓ tự nạp** ⇒ (a) không đủ; phải chọn (b) hoặc (c).

---

## 8. HỢP ĐỒNG BIÊN SOẠN PROMPT CHO TANPHATAI

Mỗi prompt ERP mà TanPhatAI xuất ra phải có **hai khối tách rời**.

### Khối 1 — kỹ năng Notion mà TanPhatAI dùng để SOẠN
```
NOTION_ORCHESTRATION:
  CURRENT_ACTOR: TANPHATAI_NOTION
  NOTION_SKILL_STACK: [các trang KN tối thiểu cần dùng]
  PURPOSE_OF_EACH: [mỗi kỹ năng một dòng lý do]
  SOURCES_CONSULTED: [trang điều-khiển + trang mô-đun + báo cáo]
  RULE_SCOPE: COMMON | PROJECT:ERP_TAN_PHAT
  OWNER_INTENT_LOCK: [ý Chủ dự án, giữ nguyên nghĩa]
  GHI CHÚ: đây KHÔNG phải kỹ năng chạy trong công cụ IDE.
```

### Khối 2 — kỹ năng Agent IDE được phép dùng
```
IDE_SKILL_ROUTE:
  CLIENT_RUNTIME: CURSOR_NATIVE | CLAUDE_CODE_IN_CURSOR | OTHER
  CLIENT_VERSION: [số thật hoặc CHƯA ĐO]
  DISCOVERY_STATE: PROVEN_TRUE | PROVEN_FALSE | NOT_CHECKED
  TASK_CLASS: [một loại việc]
  PRIMARY_SKILL: [đúng một slug, hoặc NONE]
  SUPPORTING_SKILLS: [tối đa hai]
  SOURCE_AND_HASH: [đường dẫn tương đối + mã băm, hoặc CHƯA ĐO]
  WHY_SELECTED: [lý do chọn, một câu]
  REQUIRED_SIGNALS: [tín hiệu quan sát được]
  NEGATIVE_TRIGGER: [điều kiện cấm nạp]
  PATH_SCOPE: [tệp/mô-đun, hoặc KHÔNG CÓ]
  CONTENT_STATE / TEST_STATE: [giá trị thật]
  INITIAL_PERMISSION: READ_ONLY | EXPLICIT_GRANT
  MAX_EFFECT_RISK: R0 | R1 | R2 | R3
  ROUTING_MODE: NO_SKILL | REFERENCE_ONLY | EXPLICIT_ONLY | PROMPT_REQUESTED | AUTO_SAFE
  PERMISSIONS: UNCHANGED trừ khi có cấp quyền tường minh
  FALLBACK: quay về quy trình thường với NO_SKILL
  NO_AUTO_INSTALL: TRUE · NO_AUTO_UPDATE: TRUE
```

### Phiếu nhận bắt buộc trả về sau mỗi lượt
```
SKILL_RECEIPT: requested=<slug|NONE>; loaded=<slug|NONE>;
client=<tên+phiên bản>; source_hash=<giá trị|CHƯA ĐO>;
trigger=<bằng chứng>; negative_check=<ĐẠT|HỎNG>;
collision_check=<ĐẠT|HỎNG|CHƯA ĐO>;
initial_permission=<READ_ONLY|EXPLICIT_GRANT>;
max_effect_risk=<R0-R3>; fallback=<TRUE|FALSE>.
```

**Bảy luật định tuyến bắt buộc:**
1. Tối đa **1 kỹ năng chính + 2 kỹ năng hỗ trợ**.
2. **KHÔNG DÙNG KỸ NĂNG là lựa chọn hợp lệ** và được ưu tiên khi không kỹ năng nào thêm giá trị đo được.
3. Kỹ năng **R2/R3 luôn** là *chỉ gọi đích danh*.
4. Kỹ năng ở trạng thái **chưa soát / ngủ đông / lưu trữ / chọi chuẩn** ⇒ **khoá**, dù prompt có nêu tên.
5. **Nạp một kỹ năng KHÔNG cấp quyền** sửa tệp, sửa CSDL, thao tác Git, phát hành hay triển khai.
6. Không giải được đụng độ, hoặc cổng nào hỏng ⇒ chọn **KHÔNG DÙNG KỸ NĂNG**.
7. Phiếu nhận là **bằng chứng**, không phải giấy phép.

---

## 9. KHUYẾN NGHỊ KỸ NĂNG THỬ ĐẦU TIÊN

**Đề xuất: `schema-visualization` — nguồn kỹ năng ERP nội bộ.**

Là kỹ năng DUY NHẤT trong 11 giữ được lớp R0 sau khi phân loại lại theo tác động tối đa. Đo trực tiếp: 0 lệnh đổi lược đồ, 0 lệnh ghi dữ liệu, 0 lệnh chạy; hai chỗ có chữ DROP/DELETE đều là chữ mô tả trong bảng ví dụ, không phải lệnh. Kỹ năng dừng ở việc trình bày ba bảng để Chủ dự án ra lệnh tiếp.

| Nguồn thay thế đã cân nhắc | Kết quả |
|---|---|
| **INSTALLED_CLIENT** | Không kỹ năng dựng sẵn nào làm việc trình bày lược đồ CSDL của dự án. Gần nhất là kỹ năng biểu đồ số liệu và kỹ năng sơ đồ — KHÁC mục đích. |
| **MATT_POCOCK** | NOT_CHECKED — không cài, không được tuyên bố. |
| **NO_SKILL** | Vẫn hợp lệ và là mặc định hôm nay: quy trình thường (đọc lược đồ rồi trình bày) đã làm được việc. Giá trị thêm của kỹ năng là ĐỊNH DẠNG ĐẦU RA nhất quán, không phải năng lực mới. |

**Chế độ hôm nay: `REFERENCE_ONLY — chưa đủ điều kiện chạy thử`.**
**Bốn điều kiện phải đạt trước khi thử:** (1) soát hiệu lực nội dung để chuyển khỏi trạng thái chưa-soát · (2) sửa nhãn phạm vi giao diện đang sai trong danh mục · (3) bổ sung mệnh đề điều kiện dùng và điều kiện không dùng vào mô tả · (4) có kết quả canary Cursor.

---

## 10. GÓI QUYẾT ĐỊNH CHO CHỦ DỰ ÁN

| # | Việc cần anh quyết | Vì sao cần anh | Chặn gì nếu chưa quyết |
|---|---|---|---|
| A | Chạy bộ kit đo Cursor ở mục 3.2 | Cần một phiên Cursor native — em không mở được từ đây | Chặn quyết định cho hai kỹ năng bị khoá, và chặn mọi bước tiến tới tự kích hoạt |
| B | Duyệt gói thay đổi 11 kỹ năng ở mục 6 | Mỗi phiếu có câu hỏi riêng cần anh chốt | Chặn toàn bộ việc sửa nội dung |
| C | Chốt ai soát hiệu lực nội dung và theo tiêu chí gì | Đây là lỗ hổng duy nhất còn mở hoàn toàn từ lượt trước | Mọi kỹ năng vẫn ở trạng thái chưa-soát ⇒ không kỹ năng nào lên được mức cao hơn |
| D | Xác nhận cách phân loại rủi ro mới ở mục 5 | Nó nâng 6 kỹ năng lên lớp cao hơn, siết chặt hơn trước | Nếu anh không đồng ý, cách định tuyến phải tính lại |
| E | Ba khoá cấp cao thừa trong artefact danh mục | Hợp đồng không nói có cấm hay không | Chặn việc sinh lại artefact danh mục cho đủ chuẩn |
| F | Mâu thuẫn đổ bóng trong chuẩn giao diện | Hai dòng trong cùng một mục nói ngược nhau | Chặn việc sửa phần chọi của kỹ năng panel |

---

## 11. MA TRẬN HOÀN THÀNH

| # | Hạng mục | Trạng thái |
|---|---|---|
| 1 | Báo cáo Markdown | ✅ `PROPOSED` |
| 2 | Artefact JSON | ✅ `PROPOSED` |
| 3 | Bản đính chính AUDIT-004 | ✅ `MEASURED_ONLY` |
| 4 | Kiểm kê công cụ đang chạy | ✅ `MEASURED_ONLY` |
| 5 | Đo thử Cursor | ⛔ `BLOCKED` — kèm bộ kit |
| 6 | Ma trận định tuyến ba nguồn | ✅ `MEASURED_ONLY` |
| 7 | 11 phiếu thay đổi | ✅ `PROPOSED` — 159 mục |
| 8 | Phương án hai kỹ năng bị khoá | 🟡 `PROPOSED` — chờ canary |
| 9 | Hợp đồng biên soạn prompt | ✅ `PROPOSED` |
| 10 | Khuyến nghị thử đầu tiên | ✅ `PROPOSED` |
| 11 | Gói quyết định | ✅ `PROPOSED` |
| 12 | Ma trận hoàn thành | ✅ |
| 13 | Khoá chốt / Việc mở / Bước kế tiếp | ✅ |
| 14 | Đường dẫn và checksum | ✅ |

> **Không hạng mục nào ghi ĐÃ THI CÔNG · ĐÃ KIỂM THỬ · ĐÃ TRIỂN KHAI** — đúng giới hạn lượt này.

---

## 12. KHOÁ CHỐT · VIỆC MỞ · BƯỚC KẾ TIẾP

### 🔒 KHOÁ CHỐT
1. **Không sửa một tệp nào trong kho ERP.** Đo lại cuối phiên: 13/13 kỹ năng không đổi mã băm.
2. **Rủi ro đo bằng tác động tối đa**, không bằng sự vắng mặt của tệp chạy được. 6/11 kỹ năng đã đổi lớp.
3. **Chưa đo được Cursor** — và `CHƯA ĐO` không phải `ĐẠT`.
4. **Nạp kỹ năng không cấp quyền sửa gì.**
5. **Ba nguồn kỹ năng giữ tách riêng.** Mẫu bên thứ ba không tự thành thẩm quyền dự án.
6. **KHÔNG DÙNG KỸ NĂNG vẫn là lựa chọn hợp lệ** và thường là đúng nhất hôm nay.

### ❓ VIỆC MỞ
1. Đo Cursor — **chặn nhiều thứ nhất**.
2. Sáu quyết định ở mục 10.
3. Ai soát hiệu lực nội dung — lỗ hổng duy nhất còn mở hoàn toàn.
4. Bốn nguồn Notion chưa đọc ⇒ đối chiếu vẫn `PARTIAL`.

### ➡️ BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
> **Công bố và bàn giao gói định tuyến / đo thử / kế hoạch chính xác này; chờ anh duyệt gói thay đổi 11 kỹ năng trước khi sửa bất kỳ tệp nguồn ERP nào.**

---

## 13. ĐƯỜNG DẪN VÀ CHECKSUM

| Hạng mục | Giá trị |
|---|---|
| Báo cáo này | [ERP-SKILL-ROUTING-CANARY-005-20260904.md](https://github.com/irissnss/Baocaoerptanphat/blob/main/ERP-SKILL-ROUTING-CANARY-005-20260904.md) |
| Artefact JSON | [ERP-SKILL-ROUTING-CANARY-005-20260904.json](https://github.com/irissnss/Baocaoerptanphat/blob/main/ERP-SKILL-ROUTING-CANARY-005-20260904.json) |
| Checksum artefact | `a2ce65d7007d6e586122700e69ef1661cac72bf974b1fd5dfb85db917be9dcf4` |
| Bản đính chính AUDIT-004 | [AUDIT-DEEP-SKILL-CONSOLIDATION-20260904-ERRATA.md](https://github.com/irissnss/Baocaoerptanphat/blob/main/AUDIT-DEEP-SKILL-CONSOLIDATION-20260904-ERRATA.md) |
| Báo cáo AUDIT-004 (giữ nguyên) | [AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.md](https://github.com/irissnss/Baocaoerptanphat/blob/main/AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.md) |
| Checksum AUDIT-004 | `65663abb759aa606c935dbbec97375d1bd7dcda9ed4a3c9016be5ad188d000d6` — đã xác minh lại, KHỚP |
| Điểm chốt kho riêng | **KHÔNG công bố** — giao riêng cho Chủ dự án và TanPhatAI |

---

## 14. BÀN GIAO CHO TANPHATAI

**Bốn điều phải tự kiểm:**
1. **Phân loại rủi ro mới** — 6 kỹ năng nâng lớp. Cần cập nhật danh mục Notion cho khớp.
2. **Trạng thái đo Cursor** vẫn là *chưa đo*. Không được ghi thành đạt.
3. **Ánh xạ Matt Pocock** vẫn *chưa kiểm* — không cài ở máy này.
4. **Đối chiếu Notion là `PARTIAL`** — còn 4 nguồn chưa đọc.

**Ba việc TanPhatAI nên làm (Agent IDE không có quyền):**
- Ghi nhận **phân loại lại rủi ro 6 kỹ năng** vào danh mục.
- Cập nhật phần *việc-kế-tiếp* của ba trang còn ghi phạm vi 7 thay vì 13.
- Ghi nhận **phiên bản công cụ đổi sang 2.1.260** và bằng chứng phát hiện đo lại vẫn 0/128.

**Xác nhận:** lượt này **KHÔNG** sửa kỹ năng · sổ đăng ký · luật · móc · cổng · quyền · mã nguồn · CSDL của ERP; **KHÔNG** cài hay cập nhật gì; **KHÔNG** bật tự kích hoạt; **KHÔNG** sửa Notion; **KHÔNG** đẩy gì vào kho mã nguồn riêng.

---

_Lượt chỉ-đọc với kho ERP. Nạp kỹ năng không phải quyền sửa. Chưa đo không phải đã đạt._
