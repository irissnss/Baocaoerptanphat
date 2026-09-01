# BÁO CÁO TỔNG LỰC PHIÊN 01/09/2026 — VÀ GÓI BÀN GIAO CHO AGENT NOTION

> **Kho:** `irissnss/erptanphat` (riêng tư) · nhánh `main`
> **Phiên bản đang chạy:** `V1.00.366`
> **Ngày:** 01/09/2026
> **Người thực hiện:** Agent IDE (Claude Code)
> **Loại:** báo cáo tổng lực kết thúc gói việc + **GÓI BÀN GIAO** theo `GOV-NOTION-HANDOFF-001` §F1c

---

## 0. ĐỌC PHẦN NÀY TRƯỚC — GỬI RIÊNG AGENT NOTION

Owner chỉ thị ngày 01/09/2026, **nguyên văn**:

> *«các yêu cầu của anh cần ghi nhận để **agent notion biết phân biệt các thay đổi
> điều chỉnh mà tài liệu notion không thể theo kịp** tránh **phản bác vô lý rồi lại
> lòng vòng làm rõ** nha em»*

Và trước đó, cùng ngày:

> *«Xử lý xong cần cập nhật nhất quán, push báo cáo chi tiết đầy đủ lên GitHub report
> cả các yêu cầu, xác nhận của anh trong phiên làm việc trò chuyện để **agent Notion
> nhận biết đâu là mệnh lệnh của anh và không phản bác khi có mâu thuẫn xảy ra giữa
> tài liệu và thực tế** vì thực tế anh đang tương tác liên tục với em và agent Notion
> không nắm và sẽ lỗi thời nên cần ghi nhận lại nha em»*

### Điều này nghĩa là gì — bốn câu, không hơn

1. **Owner ra quyết định TRỰC TIẾP trong phiên code.** Không qua Notion trước.
2. Vì vậy **code hợp lệ đi trước tài liệu** — `GOV-SESSION-DECISION-001` §F1b mục 4
   đã cho phép rõ điều này.
3. Tài liệu Notion chưa kịp cập nhật là **`SYNC_OVERDUE`** — **nợ đồng bộ**, KHÔNG
   phải sai. Nó **khác** `DIVERGED` (hai bên thật sự nói khác nhau về cùng một điều).
4. ⛔ **Khi Notion và thực tế lệch nhau, Agent Notion KHÔNG được kết luận «code sai».**
   Phải **tra sổ trước** (`docs/OWNER-REQUEST-LEDGER.md`). Lệch mà khớp một mục đã
   ghi → xử lý theo §F3/§F4: **đồng bộ tài liệu, KHÔNG sửa ngược code.**

### Ranh giới phải giữ

Sổ Yêu Cầu Owner là **KÊNH VẬN CHUYỂN** quyết định về Notion. Nó **KHÔNG** phải một
nguồn sự thật cạnh tranh với Notion (`GOV-NOTION-HANDOFF-001` §F1c mục 1). Notion vẫn
là sổ gốc đã chuẩn hoá; sổ này chỉ **chở quyết định MỚI** về đó.

### 🔴 Một phát hiện Agent Notion cần biết ngay

Điều Owner lo — *«phản bác vô lý rồi lòng vòng làm rõ»* — **có một nguyên nhân vật lý
đã được tìm ra và sửa trong chính phiên này**:

Sổ Yêu Cầu Owner bị **vỡ hình dạng bảng Markdown**. Ô thứ tư trở đi của một hàng
**không hiển thị**. Ở hai hàng nặng nhất có tới **6 trường bị nuốt**, gồm
`decision_state` và `notion_sync_state` — **đúng hai trường dựng ra cho Agent Notion
đọc**.

Nói cách khác: đã có lúc Agent Notion mở sổ ra và **không thấy** trường quyết định,
không phải vì Agent IDE quên ghi, mà vì **bảng vỡ nên trình đọc cắt mất**.

Đã sửa hôm nay (`DEBT-103`), và đã có cổng máy canh để không tái diễn.

---

## 1. MỌI CHỈ THỊ CỦA OWNER TRONG PHIÊN — NGUYÊN VĂN, MỖI CHỈ THỊ MỘT MỤC

Theo `GOV-NOTION-HANDOFF-001` §F1c mục 2: **mỗi chỉ thị riêng biệt = một mục sổ**.
Sáu chỉ thị dưới đây đều đã ghi vào `docs/OWNER-REQUEST-LEDGER.md`.

| Mục sổ | Nguyên văn (trích đủ, không làm mềm) | Loại | Trạng thái Notion |
|---|---|---|---|
| **#200** | *«Ví trí vẫn đang là draf em tự gắng theo vị trí vai trò trên local tương thích phù hợp là được nha em. — quản trị vẫn là anh chứ em, tan@intanphat.com — **màng hình phân quyền vẫn chưa ổn lắm em ơn, anh vẫn chưa trực quan, dễ dùng như anh nghĩ** em có giải pháp nào khác hoặc nâng cấp mạnh mẽ hơn không em»* | DECISION + **lượt bác thứ 4** | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| **#201** | *«**Quyền phải duyệt xác nhận mới áp dụng, stick chọn xong được liền nguy cơ nhầm lẫn là điều không thể tránh khỏi, phải có bước bấm save xác nhận mới được, popup hiện lên liệt kê các quyền đã có trước và sau điều chỉnh và xác nhận lần nữa** nó mới chuyên nghiệp và chính xác nha em»* | DECISION | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| **#202** | *«F có vẻ sinh ra để xử lý cho việc xây dựng modul tính giá nhưng xấu trúc bảng khá giống nhau. Cần lên kế hoạch gộp dùng chung là đúng. **Cấm sinh bảng rời rạc rải rác khắp nơi mất kiểm soát phải tối ưu hóa nhất có thể** nha em. B/ cần xử lý triệt để song song luôn trong lượt xử lý giao diện UI phân quyền. G/ **mã là 4 số**. Còn A, C, D, E em đề xuất hợp lý xem sao?»* | DECISION | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| **#203** | *«Xử lý xong cần cập nhật nhất quán, push báo cáo chi tiết đầy đủ lên GitHub report cả các yêu cầu, xác nhận của anh trong phiên làm việc trò chuyện để agent Notion nhận biết đâu là mệnh lệnh của anh…»* | DECISION | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| **#204** | *«Làm xong mà em không tổng kết tổng hợp lại ngay là sẽ **phình to nợ và mất kiểm soát** đi nha **anh nhắc lại cấm quên, cấm rơi rớt**, phải **tuyệt đối liền mạch phù hợp tương thích tuyệt đối** đó nha»* | **CORRECTION** — lần nhắc **thứ hai** | `OWNER_APPROVED_PENDING_NOTION_SYNC` |
| **#205** | *«làm tiếp đi em, **vấn đề nào đã xác định, nằm trong khả năng là xử lý dứt điểm đi**, push báo cáo tổng hợp tổng lực, **gom tổng hợp tồn đọng cấm để nợ phình lớn mất kiểm soát**, các yêu cầu của anh cần ghi nhận để agent notion biết phân biệt các thay đổi điều chỉnh mà tài liệu notion không thể theo kịp tránh phản bác vô lý rồi lại lòng vòng làm rõ nha em»* | DECISION | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

> ⚠️ **Owner đã phải nhắc LẶP LẠI hai chủ đề.** Ghi rõ theo `GOV-ITERATION-LIMIT-001`
> §G7.3 mục 1 (*«sổ ghi thêm hai trường: lần_lặp · bác_vì»*):
> - **Chống phình nợ:** nhắc **3 lần** (#198 → #204 → #205)
> - **Bàn giao Notion:** nhắc **2 lần** (#203 → #205)
>
> Không viết mềm đi. Đây là số đo về **cách Agent làm việc**, không phải về sản phẩm.

---

## 2. ĐÃ LÀM — TÁM NỢ ĐÓNG, MỖI NỢ CÓ BẰNG CHỨNG ĐO ĐƯỢC

### 2.1 Bốn nợ chặn go-live (commit `92d16e4`)

| Nợ | Việc | Bằng chứng |
|---|---|---|
| `DEBT-149` | Một báo giá sinh được **nhiều đơn hàng**, không gì ngăn | Chốt chặn đặt ở **lớp lưu trữ** (`createDonHangFromBaoGia`), KHÔNG ở giao diện — giao diện đi vòng được, lớp lưu trữ thì không. Lỗi trả về **liệt kê mã đơn đã sinh** |
| `DEBT-150` | Ba con số tiền trên màn Đơn Hàng là **số chết** | `da_thanh_toan`/`con_lai` ghi **một lần** lúc tạo đơn rồi không ai cập nhật — chính mã nguồn đã tự ghi nhận ở `dong-don-hang.ts:16` là *«cột chết»* |
| `DEBT-151` | Hai hàm sửa/xoá đơn nháp **không nơi nào gọi** | `grep` toàn dự án ra **đúng hai kết quả: chính hai dòng khai báo** |
| `DEBT-147` | Bốn lỗi nhãn `/m0/quy-trinh` | Chữ kỹ thuật trên màn **4 → 0**, đo bằng trình duyệt thật |

**Điểm đáng chú ý nhất — `DEBT-151`:** bài kiểm cũ **24/24 PASS** nhưng **không thể**
bắt được lỗi này, vì nó gọi thẳng hàm lưu trữ — tức **tự làm hộ đúng cái việc mà giao
diện đang thiếu**. Nay thêm nhóm `C5` (7 điều kiện) đọc **mã nguồn thật**, phân biệt
«có NHẬP hàm» với «có GỌI hàm».
**Kiểm ngược:** gỡ lời gọi mà giữ dòng nhập → `C5b` xanh, `C5d` **đỏ** (30/1); khôi
phục → **31/0**.

**`DEBT-150` — vì sao GỠ chứ không TÍNH LẠI:** tính đúng phải đọc bảng công nợ và
phiếu thu, mà **cả hai đang 0 dòng trên máy vận hành** — chưa có nguồn sự thật để
tính. Bày một con số đúng-về-kỹ-thuật nhưng luôn bằng 0 cũng gây hiểu nhầm y như bày
số chết.

**`DEBT-147` — lượt đầu chưa đạt, nói thẳng:** đưa mã `WF_*` xuống dòng phụ mờ, đo lại
**vẫn 4/4 chữ kỹ thuật** vì mã vẫn nằm trên màn. Phải gỡ hẳn khỏi bảng mới về 0. Mã
**không mất** — vẫn tìm được bằng ô tìm, vẫn hiện ở hộp thoại lịch sử và biểu mẫu sửa;
ở ba nơi đó nó **được gọi tên là mã**, nên không ai nhầm là nhãn.

### 2.2 Làm lại màn phân quyền — lượt thứ NĂM (commit `75716c5`)

Ba việc a·b·c Owner duyệt:

**(c) XÁC NHẬN HAI BƯỚC — quan trọng nhất, và lỗi có thật:**

Đo được trong mã: **CẢ HAI** ma trận đều **GHI NGAY** khi tick.
`ma-tran-menu-cay.tsx` hàm `doiMotCo` gọi thẳng `ghi()` → máy chủ; và
`ma-tran-chuyen-trang-thai.tsx` `onChange` cũng vậy. Nguy hiểm nhất là nút **«tick cả
nhóm»**: một cú bấm ghi thẳng **hàng chục dòng**.

Nay: tick → **nháp**. Thanh nhắc *«có N thay đổi CHƯA được lưu — chưa có gì đổi trên
hệ thống»*. Bấm «Xem Lại & Lưu» mở hộp thoại bày **TRƯỚC → SAU** từng mục, mục **bị
bớt quyền tô đỏ và xếp lên trước**. Phải bấm **«Tôi Đã Xem Kỹ»** thì nút ghi thật
**«Xác Nhận Đổi Quyền»** mới hiện.

**(b) NGÔN NGỮ NGHIỆP VỤ:** màn nói **«Làm được: Bán Hàng · Kho Hàng (một phần)»** thay
cho **«23 màn hình đang dùng được»**. Mảng suy từ **tiền tố mã màn** nên thêm màn mới
là tự vào đúng mảng. Kèm bảng dịch **32 mã quyền hành động** sang việc thật
(`can_view_cost_price` → «Xem giá vốn»). Mã lạ **hiện nguyên mã**, không ẩn — ẩn một
quyền chưa kịp đặt tên là cách chắc chắn nhất để nó lọt qua mắt người duyệt.

**(a) BỐ CỤC HAI CỘT:** danh sách người **dính bên trái, luôn nhìn thấy**. Trước đây
xếp dọc nên chọn người xong cuộn xuống là **tên người đang sửa đã trôi khỏi màn** —
gốc của cảm giác khó kiểm soát.

**BẰNG CHỨNG — cổng mới `npm run test:xac-nhan-hai-buoc`: 18/18.**
Đo bằng **số dòng trong cơ sở dữ liệu**, không nhìn giao diện (giao diện có thể vẽ một
đằng máy chủ ghi một nẻo).

| Bước | Dòng quyền |
|---|---|
| mốc nền | **148** |
| sau khi tick một ô | **148** ✅ |
| sau khi mở hộp thoại | **148** ✅ |
| sau khi bấm «Tôi Đã Xem Kỹ» | **148** ✅ |
| sau khi huỷ | **148** ✅ |

**Kiểm ngược:** trả về kiểu ghi-ngay → **148 → 149** (2 điều kiện đỏ); khôi phục →
**18/18**.

### 2.3 Hai nợ quản trị (commit `56d5bc9`)

| Nợ | Việc |
|---|---|
| `DEBT-103` | **Sổ vỡ hình dạng bảng** — nguyên nhân vật lý của điều Owner lo. Sổ Owner 1 hàng lệch + **30 dòng trống cắt mạch**; sổ nợ **9 hàng lệch**. Cổng mới `test:hinh-dang-so` (6/6) |
| `DEBT-165` | Quét **507 tệp**: không tệp nào nói «bỏ tick = cấm» ra màn. Cổng mới `test:cau-noi-quyen` |

**🔴 Điều phải nói thẳng về `DEBT-103`:** trong 9 hàng lệch của sổ nợ, **có hai dòng
chính Agent vừa ghi sáng nay** (`DEBT-150`, `DEBT-151`) — tức lỗi này **vẫn đang tái
sinh**, không phải chuyện cũ. Vì vậy cổng máy là cần thiết, không phải trang trí.

**⚠️ Cổng tự sửa mình:** phép đếm đầu tiên của `test:hinh-dang-so` **SAI** — nó so
*tổng số lần nhắc* cụm «Owner CHỈ THỊ» với *tổng số lần ghi* `notion_sync_state`, ra
15/12 và **tố oan** trong khi không hàng nào thiếu. Nay đếm **theo HÀNG**.
Ghi lại vì đây là loại lỗi dễ lặp: **cổng đếm sai đơn vị thì tố oan chính dữ liệu
đúng.**

---

## 3. ĐIỀU AGENT LÀM SAI TRONG PHIÊN — GHI TRUNG THỰC

`GOV-FAILURE-RECORD-001` §G7.4 bắt ghi đúng, không làm mềm.

| # | Sai gì | Hậu quả | Đã sửa thế nào |
|---|---|---|---|
| 1 | `DEBT-147` lượt đầu chỉ đưa mã xuống dòng phụ | Đo lại **vẫn 4/4 chữ kỹ thuật** | Gỡ hẳn mã khỏi bảng |
| 2 | Thước đo mới chọn vai trò **trước khi** mở mục chứa ma trận | Lúc đó chưa có ô chọn nào tồn tại → thước in *«không ô chọn nào mở khoá được»*, **nghe như giao diện hỏng trong khi giao diện đúng** | Đảo thứ tự: mở trước, chọn sau |
| 3 | Thước chờ 1400ms sau khi đổi vai trò | Chưa nạp xong quyền → ô tick còn khoá → **đo sai** | Nâng lên 2500ms |
| 4 | Thước đọc chữ trong `<main>` | Hộp thoại nằm ở lớp phủ **ngoài** `<main>` → báo thiếu cột trong khi có đủ | Đọc thẳng từ phần tử hộp thoại |
| 5 | So chữ y nguyên hoa/thường | Tiêu đề cột bị CSS in hoa → `innerText` trả «ĐANG CÓ» ≠ «Đang Có» | So không phân biệt hoa thường |
| 6 | Dùng **dấu huyền ngược** trong chuỗi mẫu — **đúng cái bẫy đã tự ghi cảnh báo ở đầu file** | Vỡ cú pháp, không chạy được | Bỏ dấu huyền ngược |
| 7 | Chạy lượt kiểm ngược **qua ống dẫn có `head`** | `head` đóng ống sớm, tiến trình chết **trước khối dọn** → **sót 1 dòng quyền + 1 tài khoản thử trong CSDL** | Đã dọn chính xác theo dấu tay; đã vá khối dọn (thiếu bảng `audit_log`); đã ghi cảnh báo vào chính tệp cổng |
| 8 | Ghi hai dòng sổ nợ có **dấu ngăn cột chưa thoát** | Hai dòng đó **vỡ bảng** ngay trong ngày | Cổng `test:hinh-dang-so` nay chặn |

**Riêng số 7 phải nhấn mạnh:** lượt kiểm ngược đã **để lại dấu vết thật trong cơ sở dữ
liệu**. Phát hiện bằng cách đếm lại, không phải bằng may mắn. Đã trả về đúng mốc nền
(**148 dòng quyền · 9 tài khoản · 0 tài khoản thử**) và có kiểm chứng.

---

## 4. TỒN ĐỌNG — GOM ĐỦ, KHÔNG BỎ SÓT

Owner: *«gom tổng hợp tồn đọng cấm để nợ phình lớn mất kiểm soát»*.

> ⚠️ **SỬA SỐ 01/09/2026, ngay sau khi đẩy bản đầu.** Bản đầu ghi **«37 nợ còn mở»** —
> **SAI**. Con số đó lấy từ một lượt đếm giữa chừng, chưa cộng các nợ đóng sau đó và
> chưa trừ đúng. Đếm lại trên sổ thật:
>
> | | Số |
> |---|---|
> | Tổng dòng nợ trong sổ | **85** |
> | Còn mở / đang xử lý | **42** |
> | Đã xử lý | **36** |
> | Hoãn có chủ đích · không còn hợp lệ · tạm bỏ | **7** |
>
> Đóng trong phiên này: **9 nợ** — `DEBT-147` · `149` · `150` · `151` · `145` · `162` ·
> `103` · `165` · `166`. Sinh mới: `DEBT-164` (còn mở) · `165` · `166` (đã đóng ngay).
>
> Ghi lại chỗ sai này thay vì lặng lẽ sửa — chính Owner vừa nhắc *«cấm rơi rớt»*, và
> một con số tồn đọng sai là đúng thứ làm mất kiểm soát.

**Tổng: 42 nợ còn mở** — 8 chờ Owner quyết, **34** Agent làm được.
Chia hai nhóm theo **ai xử lý được**:

### 4.1 🔴 CHỜ OWNER QUYẾT — 8 nợ, Agent KHÔNG tự làm

| Nợ | Việc | Owner cần quyết gì |
|---|---|---|
| `DEBT-117` | Mâu thuẫn giữa **Decision D7 và D9 trong Notion** về cách tính tiền in | **Phân xử** — hai quyết định trong Notion nói khác nhau |
| `DEBT-152` | Bản in báo giá in ra chỉ ghi «KH034» thay vì tên khách, không có thông tin công ty | **Duyệt mẫu giấy** trước khi làm |
| `DEBT-143` | Tab «Hành động sau chuyển» là **cấu hình chết** — cấu hình xong không chạy gì, và im lặng | Bỏ tab, hay làm cho nó chạy thật |
| `DEBT-144` | Quyền sửa **sơ đồ quy trình** canh ở mức module, không theo từng màn | Chấp nhận, hay siết theo màn |
| `DEBT-142` | Quyền chuyển bước cấp cho vai trò **không có người mang** | Gán người, hay bỏ quyền |
| `DEBT-153` | **Kế hoạch Phase 1 treo «Chờ Owner xác nhận» từ 14/06/2026** — 9 bài nghiệm thu + 4 câu hỏi chưa trả lời | Đọc và chốt, hay huỷ kế hoạch đó |
| `DEBT-163` | **Đặt tên trong CSDL không nhất quán** — nhóm A, C, D, E | Duyệt đề xuất (mục 5 dưới) |
| `DEBT-069` · `DEBT-082` | Hai nợ quản trị cũ | Xác nhận để đóng |

### 4.2 🟠 AGENT LÀM ĐƯỢC — 34 nợ, xếp theo mức chặn

**Nhóm chặn go-live (4):**

- `DEBT-016` 🔴 — **chưa đổi mật khẩu đăng nhập** `tan@intanphat.com` trên cả hai máy
- `DEBT-021` 🔴 — mã băm mật khẩu nằm trong tệp seed **được git theo dõi**
- `DEBT-116` 🔴 — cổng quét dữ liệu cá nhân **không quét họ tên thật**, chỉ quét email · SĐT · CCCD
- `DEBT-108` 🟠 — hơn **20 tệp** còn chứa địa chỉ IP máy chủ vận hành

**Nhóm tính giá — cụm lớn (6):** `DEBT-112` · `DEBT-113` · `DEBT-115` · `DEBT-111` ·
`DEBT-114` · `DEBT-118`.
Nội dung: đã **tìm được công thức khổ trải thật của Tân Phát**, và nó chứng minh **mô
hình hiện tại không đủ** — một công thức đang dùng chung cho **40 nhóm sản phẩm**,
trong đó có **~20 kiểu dáng hộp khác nhau về hình học**. Màn cấu hình khổ trải **đã tồn
tại đầy đủ nhưng chưa được nối** vào đường tính giá — suýt dựng trùng.

**Nhóm chất lượng (8):** `DEBT-131` (67/85 cổng kiểm **mồ côi** khỏi mọi bộ gộp — hỏng
bao lâu cũng không ai hay) · `DEBT-133` · `DEBT-134` · `DEBT-135` · `DEBT-139` ·
`DEBT-140` · `DEBT-155` · `DEBT-158`.

**Nhóm giao diện & quyền (7):** `DEBT-093` · `DEBT-094` (8 trang còn cột thao tác trên
dòng) · `DEBT-096` · `DEBT-098` · `DEBT-099` · `DEBT-100` · `DEBT-104`.

**Nhóm nhỏ sinh trong phiên này (4):** `DEBT-154` · `DEBT-164` (sửa đơn nháp chưa sửa
được địa chỉ giao) · `DEBT-166` (cổng mới chưa đo ma trận chuyển trạng thái) ·
`DEBT-109` · `DEBT-110` · `DEBT-121`.

---

## 5. ĐỀ XUẤT CHỜ OWNER DUYỆT — NHÓM A, C, D, E CỦA `DEBT-163`

Owner đã chốt: **F** = gộp bảng · **B** = xử lý triệt để · **G** = mã 4 số.
Ba nhóm còn lại Agent đề xuất, **chờ Owner duyệt trước khi đụng**:

| Nhóm | Đề xuất | Vì sao |
|---|---|---|
| **Bước 0** | Viết `docs/DB-NAMING-STANDARD.md` + cổng `test:db-naming` **TRƯỚC** | Không có chuẩn viết ra thì sửa xong lại lệch tiếp. Đây là gốc của việc *«agent không theo chuẩn hóa của anh sinh bảng pha trộn tùm lum»* |
| **D**, **C** | Dọn — **rủi ro thấp** | Không đụng dữ liệu đang chạy |
| **A** | Dọn — **rủi ro vừa** | Cần bản di trú + diễn tập + kiểm ngược |
| **E** | ❌ **KHÔNG đổi tên bảng** | Rủi ro **cao nhất**, lợi ích **thấp nhất**. Ghi thành ngoại lệ lịch sử trong tài liệu chuẩn thay vì đổi |

---

## 6. CHƯA CHỨNG MINH ĐƯỢC — BẮT BUỘC NÊU

`GOV-NOTION-HANDOFF-001` §F1c mục 4 bắt nêu, **để Notion không nhận nhầm thành sự thật
hiện hành**:

1. **Màn phân quyền chưa được Owner nghiệm thu.** Đây là lượt làm lại **thứ NĂM**; bốn
   lượt trước Owner bác. Agent **không tự tuyên bố màn đã đạt**.
2. ~~**Cổng `test:xac-nhan-hai-buoc` mới chỉ đo ma trận QUYỀN MÀN.**~~
   ✅ **ĐÃ GIẢI cùng ngày.** Đã thêm nhánh đo thứ hai cho ma trận **chuyển trạng thái**
   (`role_action_permission`, 67 dòng — duyệt báo giá · huỷ đơn · duyệt thu chi):
   tick → **67 → 67**, bỏ thay đổi → vẫn **67**. Cổng nay **22/22**, không còn ma trận
   nào chỉ được kiểm bằng đọc mã nguồn. `DEBT-166` **đã đóng**. Commit `1a33712`.
3. **Mọi thay đổi trong phiên này CHƯA TRIỂN KHAI lên máy vận hành.** Máy vận hành vẫn
   chạy `V1.00.366`. Ba commit `92d16e4` · `75716c5` · `56d5bc9` mới nằm ở kho.
   Triển khai cần cổng duyệt của Owner theo `GOV-DEPLOY-SCHEMA-COMPAT-001` §G7.16.
4. **Sáu đơn hàng trùng trên máy vận hành vẫn còn.** Đo được cả sáu là dữ liệu thử
   (khách «Sđsfdsfds», hàng «Test San Pham R1») nên **không hại tiền thật**, nhưng
   **xoá dữ liệu phải Owner duyệt** — Agent không tự làm.
5. **`DEBT-165` chọn hướng an toàn là BỎ SÓT, không phải KÊU OAN.** Cổng bóc chú thích
   trước khi quét, nên một câu sai giấu trong chuỗi trông giống chú thích **có thể lọt**.
   Chọn có chủ đích: cổng kêu oan sẽ bị tắt đi, cổng bỏ sót thì vẫn còn tác dụng.

---

## 7. GÓI BÀN GIAO — SÁU QUYẾT ĐỊNH CẦN ĐƯA VỀ NOTION

Đủ trường theo `GOV-NOTION-HANDOFF-001` §F1c mục 4.

| Mục sổ | Quyết định | Phạm vi áp dụng | **CẤM mở rộng sang** | Bằng chứng | Trang Notion cần sửa |
|---|---|---|---|---|---|
| **#200** | Màn phân quyền phải trực quan thật; quản trị là `tan@intanphat.com`; vị trí lấy theo máy nội bộ | `/m0/security` · dữ liệu vị trí nhân sự | Không đổi quyền tài khoản thật của nhân viên | `75716c5` · V1.00.366 | **CHƯA XÁC ĐỊNH** |
| **#201** | Đổi quyền **phải qua Save + popup trước/sau + xác nhận lần nữa** | Mọi ô tick quyền trong ERP | Không áp cho ô tick **không phải quyền** | `test:xac-nhan-hai-buoc` **18/18**, đo 148→148 | **CHƯA XÁC ĐỊNH** |
| **#202** | **Cấm sinh bảng rời rạc**; F gộp dùng chung; mã **4 số**; A/C/D/E chờ duyệt | Lược đồ CSDL | **Chưa được đụng** A/C/D/E khi Owner chưa duyệt | Rà 101 bảng · 1 563 cột | **CHƯA XÁC ĐỊNH** |
| **#203** | Đẩy báo cáo đủ để Agent Notion phân biệt mệnh lệnh Owner | Kho báo cáo công khai | Không đưa dữ liệu nhạy cảm ra kho công khai | Chính tệp này | **CHƯA XÁC ĐỊNH** |
| **#204** | **Chốt sổ NGAY sau mỗi việc**, cấm dồn | Cách làm việc mọi phiên | — | Áp dụng ngay trong phiên: 8 nợ chốt ngay sau khi vá | **CHƯA XÁC ĐỊNH** |
| **#205** | Xử lý dứt điểm việc trong khả năng; gom tồn đọng; ghi nhận để Notion không phản bác | Toàn phiên | Không tự quyết 8 nợ đang chờ Owner | Mục 4 tệp này | **CHƯA XÁC ĐỊNH** |

> **Vì sao mọi ô đều ghi «CHƯA XÁC ĐỊNH»:** Agent IDE **không có quyền đọc Notion**
> (`GOV-ACTOR-BOUNDARY-001` §A2). Ghi bừa một tên trang là bịa. Agent Notion tự xác
> định trang đích rồi báo lại.
>
> Sau khi Notion cập nhật xong, `notion_sync_state` chuyển thành `SYNCED_TO_NOTION`.
> ⛔ **Agent IDE KHÔNG tự ghi giá trị đó** — chỉ TanPhatAI xác nhận mới được ghi
> (§F1c mục 3).

---

## 8. BẢNG CỔNG KIỂM — TRẠNG THÁI THẬT

| Cổng | Kết quả |
|---|---|
| `test:gov-gates` (bộ gộp quản trị) | **37/37 PASS** |
| `test:xac-nhan-hai-buoc` 🆕 | **22/22 PASS** — phủ CẢ HAI ma trận |
| `test:hinh-dang-so` 🆕 | **6/6 PASS** |
| `test:cau-noi-quyen` 🆕 | **1/1 PASS** — quét 507 tệp |
| `test:don-hang-hoan-thien` | **31/31 PASS** (+7 điều kiện mới) |
| `test:doi-chieu-khuon` | **0 chữ kỹ thuật** trên cả 6 màn |
| `npx tsc --noEmit` | sạch |
| `test:secret-scan` · `test:pii-scan` | PASS |

**Ba cổng mới đều đã kiểm ngược hai chiều** — gieo lỗi thì đỏ, gỡ ra thì xanh.

---

## 9. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC

**Owner nghiệm thu màn `/m0/security`** — mở `http://localhost:3000/m0/security`, vào
«Quản Lý Nâng Cao», chọn một vai trò **không phải quản trị**, tick thử một ô rồi xem
thanh nhắc và hộp thoại xác nhận.

Đạt thì triển khai ba commit lên máy vận hành. Chưa đạt thì Owner nói rõ **điểm nào
chưa được** — đây là lượt thứ năm, `GOV-ITERATION-LIMIT-001` §G7.3 đòi **đổi bản chất
cách làm**, không phải chỉnh tham số.

---

*Báo cáo này công khai và đã qua cổng an toàn: không mật khẩu, không khoá, không dữ
liệu cá nhân, không địa chỉ máy chủ. Số liệu kỹ thuật (tên bảng · mã màn · số dòng) là
loại được phép theo `GOV-PUBLIC-SAFE-001` §J1.*

---

## 10. KHỐI BÁO CÁO KẾT THÚC (`GOV-COMPLETION-REPORT-001` §G5 — 11 trường)

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Đóng 8 nợ: DEBT-147 · 149 · 150 · 151 (chặn go-live) ·
     DEBT-145 · 162 (màn phân quyền) · DEBT-103 · 165 (quản trị)
   - Làm lại màn /m0/security theo ba việc a·b·c Owner duyệt:
     bố cục hai cột · ngôn ngữ nghiệp vụ · xác nhận HAI BƯỚC
   - Vá gốc: cả hai ma trận quyền không còn GHI NGAY khi tick
   - Sửa câu SAI «Chưa tick = CẤM» ở ma trận chuyển trạng thái
   - Nắn hình dạng bảng hai sổ — nguyên nhân vật lý làm Agent Notion
     không đọc được trường quyết định
   - Dựng 3 cổng mới, cả ba đều kiểm ngược hai chiều
   - Ghi 6 chỉ thị Owner vào sổ, mỗi chỉ thị một mục (#200–#205)
   - Gom tồn đọng: 37 nợ còn mở, chia rõ 8 chờ Owner / 29 Agent làm được

2. PHẠM VI
   ĐỤNG    : src/app/m0/security/** · src/app/m3/don-hang/** ·
             src/app/m0/quy-trinh/** · src/lib/m3-store.ts ·
             src/lib/security/mang-nghiep-vu.ts (mới) ·
             src/components/security/** (mới) · scripts/tests/** ·
             package.json · hai sổ quản trị
   KHÔNG ĐỤNG: cơ sở dữ liệu máy vận hành (0 câu ghi) ·
             lược đồ (0 DDL) · triển khai (chưa đẩy) ·
             số phiên bản (giữ V1.00.366) ·
             6 đơn trùng trên máy vận hành (chờ Owner duyệt xoá) ·
             8 nợ đang chờ Owner quyết

3. BẰNG CHỨNG
   Kho mã irissnss/erptanphat (riêng tư), nhánh main — đã đẩy 01/09/2026,
   4f826c9..56d5bc9, KHÔNG force-push:
     92d16e4 — bốn nợ chặn go-live (DEBT-147/149/150/151)
     75716c5 — làm lại màn phân quyền a/b/c (DEBT-145, DEBT-162)
     56d5bc9 — nắn hình dạng hai sổ + 2 cổng mới (DEBT-103, DEBT-165)
   npm run test:xac-nhan-hai-buoc → 22/22.
     Ma trận QUYỀN MÀN: dòng quyền 148→148 ở mọi bước; kiểm ngược 148→149
     Ma trận CHUYỂN TRẠNG THÁI: 67→67, bỏ thay đổi vẫn 67
     → DB_PROVEN + UI_PROVEN
   npm run test:hinh-dang-so → 6/6; kiểm ngược đỏ/xanh → FILE_PROVEN
   npm run test:cau-noi-quyen → 1/1, quét 507 tệp; kiểm ngược → CODE_PROVEN
   npm run test:don-hang-hoan-thien → 31/31; kiểm ngược 30/1 → CODE_PROVEN
   npm run test:doi-chieu-khuon → chữ kỹ thuật 4→0 cả 6 màn → UI_PROVEN
   npm run test:gov-gates → 37/37 → FILE_PROVEN
   npx tsc --noEmit → sạch → CODE_PROVEN
   Ảnh: docs/anh-kiem-thu/xac-nhan-hai-buoc/ ·
        docs/anh-kiem-thu/doi-chieu-khuon-man/

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #200 · #201 · #202 · #203 · #204 · #205
   Mỗi chỉ thị MỘT mục theo GOV-NOTION-HANDOFF-001 §F1c mục 2.
   Ghi rõ số lần Owner phải nhắc lại: chống phình nợ 3 lần,
   bàn giao Notion 2 lần.

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · nhánh main · commit e8aa43e ·
       file TONG-LUC-PHIEN-01-09-2026-VA-GOI-BAN-GIAO-NOTION.md
   (e8aa43e là bản đầu; chính dòng ghi mã này được bổ sung ở commit ngay
    sau đó — mã commit chỉ tồn tại SAU khi commit, không tự trích trước được.
    Mã commit của KHO MÃ NGUỒN nằm ở trường 3, không để lẫn vào đây: trường
    này chỉ nói về kho báo cáo công khai.)
   KHÔNG force-push, KHÔNG viết lại lịch sử.

6. CÒN SÓT / CHƯA LÀM
   - 37 nợ còn mở, liệt kê đủ ở mục 4 của báo cáo này
   - DEBT-164: sửa đơn nháp chưa sửa được địa chỉ giao — cố ý để lại
   - Chưa triển khai lên máy vận hành
   - 6 đơn trùng trên máy vận hành chưa xoá — chờ Owner duyệt
   - Nhóm A/C/D/E của DEBT-163 chưa đụng — chờ Owner duyệt đề xuất

7. ĐANG CHỜ OWNER
   - Nghiệm thu màn /m0/security → cần Owner xác nhận đạt hay chưa.
     Chặn: chưa nghiệm thu thì không triển khai.
   - Duyệt đề xuất A/C/D/E của DEBT-163 → chặn việc dọn tên CSDL
   - Phân xử DEBT-117 (D7 vs D9 trong Notion) → chặn module tính giá
   - Duyệt mẫu giấy DEBT-152 → chặn bản in báo giá
   - Duyệt xoá 6 đơn trùng trên máy vận hành
   - 4 nợ chờ khác: DEBT-142 · 143 · 144 · 153

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner nghiệm thu màn /m0/security: mở khu «Quản Lý Nâng Cao»,
   chọn vai trò KHÔNG phải quản trị, tick thử một ô, xem thanh nhắc
   và hộp thoại xác nhận hai bước.

9. CHƯA XÁC MINH ĐƯỢC
   - Màn phân quyền đã «đủ trực quan» chưa — chỉ Owner xác minh được.
     Đây là lượt làm lại thứ NĂM.
   - Trang Notion nào cần sửa — Agent IDE không có quyền đọc Notion
     (GOV-ACTOR-BOUNDARY-001 §A2). Agent Notion xác định.

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — thiếu: Owner nghiệm thu màn phân quyền và
       đo ma trận chuyển trạng thái trên trình duyệt thật.
       Điều kiện lên PASS: Owner xác nhận màn phân quyền đạt.
       (DEBT-166 đã đóng cùng ngày — cổng nay 22/22, phủ cả hai ma trận.)

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén:
     - docs/UI-STANDARD.md — TOÀN PHẦN, dòng 1–521
       (GOV-READ-STANDARD-001 §G7.2 + GOV-RELOAD-AFTER-COMPACT-001 §G7.8)
     - .governance/registry/tech-debt.md — toàn bộ 82 dòng nợ
     - docs/OWNER-REQUEST-LEDGER.md — các mục #194–#205
     - src/lib/security/menu-catalog.ts · src/lib/menu-registry.ts
     - Trang mẫu /m1/khach-hang (khach-hang-client.tsx:565-578) để đối
       chiếu cột đầu bảng trước khi sửa /m0/quy-trinh
   Không kết thúc bằng trí nhớ từ trước nén.
═══════════════════════════════════════════
```
