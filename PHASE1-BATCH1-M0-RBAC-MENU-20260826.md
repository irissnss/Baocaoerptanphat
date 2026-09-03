# PHASE 1 · BATCH 1 — M0: TRUY CẬP, PHÂN QUYỀN VÀ MENU

**Kế hoạch con:** `PL-ERP-NON-PRICING-CLOSEOUT-20260826` — *Phase 1 — Module & Menu phục vụ kinh doanh*
**Kế hoạch cha:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`
**Ngày:** 26/08/2026
**Người thi hành:** Agent IDE — một người ghi duy nhất

> **Tính giá: KHÔNG mở.** Không công thức, không khổ trải, không bình bài, không di trú tính giá.
> **Không DDL.** Lược đồ hai môi trường đã khớp tuyệt đối — Batch 1 không cần đổi cấu trúc nào.

---

## 1. HIỆN TRẠNG — ĐO TỪ ĐĨA, KHÔNG TIN TÓM TẮT

| Đo | Giá trị |
|---|---|
| HEAD kho riêng (đầu lượt) | `<mã-nguồn-riêng>` |
| HEAD kho riêng (cuối lượt) | `<mã-nguồn-riêng>` |
| Commit đang chạy trên máy vận hành | `<mã-nguồn-riêng>` |
| Khoảng cách local ↔ vận hành | local đi trước **53 commit**, vận hành đi trước **0** |
| Rẽ nhánh | **KHÔNG** — commit vận hành là **tổ tiên** của local |

> ⚠️ **Một lệch với bản bàn giao, đã đo lại và báo:** bản bàn giao ghi *"single writer tại HEAD `<mã-nguồn-riêng>`"*.
> Đo thật đầu lượt là `<mã-nguồn-riêng>`. Nguyên nhân **không phải người ghi song song** mà là ba commit của chính
> lượt trước đã đẩy HEAD lên. Hai lần đo cách nhau 25 giây cho kết quả **giống hệt**, cây làm việc sạch,
> local = từ xa ⇒ **người ghi duy nhất: xác nhận**.

### Máy chủ dữ liệu — không suy từ chuỗi phiên bản

| Bằng chứng | Local | Máy vận hành |
|---|---|---|
| `VERSION()` | `10.11.10-MariaDB` | `10.11.10-MariaDB` |
| Biến `aria_*` (MariaDB có, **MySQL không**) | **22 biến** | — |
| Máy khách `mysql --version` | — | `Distrib 10.11.10-MariaDB` |
| **BASE TABLE** | **101** | **101** |
| **VIEW** | **0** | **0** |
| **Khoá ngoại** | **112** | **112** |

⇒ Khớp `MariaDB 10.11 LTS` ở cả hai chính sách, và khớp con số **101** Owner xác nhận.
**KHÔNG có xung đột.** Lược đồ hai bên **không lệch một chút nào** ⇒ Batch 1 **không cần di trú lược đồ**.

### Máy vận hành — phân định "tệp bẩn"

| Mục | Bản chất | Là hotfix? |
|---|---|---|
| một kịch bản shell | **chỉ đổi chế độ tệp** `100644`→`100755`, **0 dòng** khác biệt nội dung | **KHÔNG** |
| một thư mục | sản phẩm lúc chạy (tiến trình đang chạy từ đó) | **KHÔNG** |

⇒ **Không có hotfix ngoài Git.** Điều kiện dừng không kích hoạt.

---

## 2. CHÍN KHẲNG ĐỊNH MENU — GIẢI THÍCH ĐỦ CẢ CHÍN

Bản đầu cho **7/9**. Dưới đây là **toàn bộ**, kèm phân loại nguyên nhân.

| # | Khẳng định | Mong đợi | Thực tế đầu lượt | Kết quả | Phân loại nguyên nhân |
|---|---|---|---|---|---|
| 1 | Không tài khoản đang hoạt động nào thấy 0 menu | 0 | **2 tài khoản** | ĐỎ | **Yêu cầu đã bị thay** — xem §3 |
| 2 | Mọi vai trò đang được dùng đều thấy ≥1 menu | tất cả | **`USER` thấy 0** | ĐỎ | **Yêu cầu đã bị thay** — xem §3 |
| 3 | Không menu nào trỏ vào route không có trang | 0 | **0/51** | ✅ | — |
| 4 | Menu biểu mẫu trỏ `/bieu-mau`, không dưới `/m0` | đúng | đúng | ✅ | — |
| 5 | Quyền **sửa** biểu mẫu đúng bằng {ADMIN, CEO} | 2 vai trò | đúng 2 | ✅ | — |
| 6 | Màn quản trị hệ thống chỉ cấp cho {ADMIN} | chỉ ADMIN | đúng | ✅ | ban đầu ĐỎ — **dương tính giả**, xem dưới |
| 7 | Không route `/m0` **mới** lọt ra ngoài ADMIN | 0 | 0 | ✅ | chốt phụ thêm sau điều tra |
| 8 | Không menu nào trùng đường dẫn | 0 | 0 | ✅ | — |
| 9 | Có ≥20 mục menu để phép đo có nghĩa | ≥20 | 51 | ✅ | chốt an toàn của chính bộ kiểm |

### Khẳng định 6 — dương tính giả, đã sửa chính khẳng định

Bản đầu kiểm theo **tiền tố đường dẫn** `/m0` và báo ba lỗi: `CEO` · `HR` · `KE_TOAN` đều thấy
`/m0/phong-ban`. Điều tra tiếp cho thấy **không phải rò rỉ quyền**, ba bằng chứng ngược nhau:

1. Trang đó gác bằng **khoá riêng theo màn**, **không** phải cổng module M0 — cho ai xem Phòng Ban
   **không** mở cho họ bất kỳ màn quản trị nào khác.
2. Trong danh mục menu, mục Phòng Ban mang thứ tự **101**, nằm **ngay trước** Vị Trí (102) và Nhân Sự
   (103) — tức thuộc **cụm dữ liệu nền nhân sự**, không thuộc cụm quản trị.
3. Phòng ban là dữ liệu nền mà Nhân sự và Kế toán **cần theo nghiệp vụ**.

⇒ Đây là **route đặt sai module**, đã ghi nợ `DEBT-123` (`ROUTE_OWNERSHIP_NAVIGATION_MISMATCH`),
**không đổi route trong Batch 1** theo đúng chỉ đạo. Khẳng định được viết lại: liệt kê **tường minh**
các màn quản trị thật, **kèm chốt phụ** bắt route `/m0` **mới** lọt ra ngoài ADMIN mà chưa được chứng minh.

**Sau khi thi hành quyết định Owner: 11/11 đạt** (2 khẳng định viết lại + 2 chốt ngược chiều thêm mới).

---

## 3. QUYẾT ĐỊNH OWNER — `USER` = CHỜ CẤP PHÁT

Owner chốt: `USER` là vai trò **chờ cấp phát**, **không phải** vai trò nghiệp vụ. Tài khoản chỉ có `USER`
phải có **0 menu nghiệp vụ**, **0 hành động nghiệp vụ**, và **không được bàn giao** để làm việc.

**Điểm cốt lõi:** quyết định này **làm cho chính hai khẳng định đang đỏ trở thành sai**, chứ không phải
dữ liệu sai. `USER` thấy 0 menu nay là **yêu cầu**, không phải lỗi. Vì vậy việc đúng là **viết lại khẳng định**,
không phải "vá" dữ liệu.

| Khẳng định | Trước | Sau |
|---|---|---|
| 1 | "tài khoản **đang hoạt động**" | "tài khoản **đã bàn giao**" |
| 2 | "mọi vai trò đang được dùng" | "mọi vai trò **nghiệp vụ**" (loại trừ `USER`) |

Và **thêm hai chốt ngược chiều** — chặn đúng cách sửa sai mà một phiên sau rất dễ mắc: thấy màn trắng rồi
"vá" bằng cách cấp bừa một menu cho `USER`:

- `USER` **PHẢI** thấy **0** menu nghiệp vụ
- `USER` **PHẢI** có **0** hành động nghiệp vụ

### Phân biệt "chờ cấp phát" với "lỗi" — không cần đổi lược đồ

"Đã bàn giao hay chưa" đo được bằng **hai cột đã có sẵn**: chưa từng đăng nhập **và** vẫn còn mật khẩu
cấp phát ban đầu ⇒ **chưa tới tay ai**.

| Trạng thái | Điều kiện | Ý nghĩa |
|---|---|---|
| `CHO_CAP_PHAT` | 0 vai trò nghiệp vụ · **chưa** bàn giao | **Bình thường** |
| `BAN_GIAO_THIEU_VAI_TRO` | 0 vai trò nghiệp vụ · **đã** bàn giao | **LỖI** — có người cầm tài khoản mà không làm được gì |
| `DA_CAP_PHAT` | ≥1 vai trò nghiệp vụ | dùng được |
| `NGUNG_HOAT_DONG` | tài khoản bị ngưng | không đăng nhập được |

Trạng thái thứ hai mới là thứ đáng báo động — và trước đây nó **bị gộp chung** với trạng thái bình thường.

### Cổng chặn bàn giao — **14/14 đạt**, phủ đủ sáu ca Owner nêu

**Hai ca quan trọng nhất không tồn tại trong dữ liệu thật**, nên bộ kiểm **tự dựng tài khoản thử rồi dọn**.
Không làm vậy thì hai điều kiện đó **không bao giờ được kiểm**, và cổng sẽ xanh mà chẳng chứng minh được gì.

**Kiểm ngược trên dữ liệu thật:** mô phỏng một tài khoản chờ cấp phát thành *đã bàn giao* → **13/14**
(đỏ đúng khẳng định); khôi phục → **14/14**, số tài khoản chờ cấp phát về đúng như trước.

**Đo được:** **2 tài khoản** đang ở trạng thái chờ cấp phát, **cả hai chưa từng đăng nhập**
⇒ **hiện chưa có người thật nào gặp màn trắng**.

### Wireframe màn chờ — đã xuất, **chưa code một dòng nào**

13 tiêu chí nghiệm thu, tái sử dụng màn đổi mật khẩu **hiện có** (không viết song song).
Chờ Owner duyệt; theo đúng khoá, việc này **không chặn** các batch khác — và đã không chặn.

---

## 4. GIÁ VỐN CHỈ ADMIN + CEO — TÌM RA MỘT RÒ RỈ THẬT

### 4.1 Khoảng trống ban đầu: CEO bị từ chối

Bảng quyền-theo-trường **rỗng** ở cả hai môi trường, và `CEO` **không** mang cờ quản trị ⇒ rơi vào nhánh
mặc-định-ẩn ⇒ **CEO bị từ chối xem giá vốn**, trái đúng điều Owner khoá.

Cơ chế mặc-định-ẩn **đúng và an toàn** — nó chặn Kế toán, Sales, Trưởng phòng Kinh doanh đúng yêu cầu.
Thiếu sót **duy nhất** là CEO chưa được cấp ngoại lệ tường minh. Sửa bằng **thêm dữ liệu cấu hình**:
không sửa mã, không đụng lược đồ, chỉ cấp quyền **xem** (không cấp quyền sửa).

Di trú **chạy lại được nhiều lần** (áp hai lần vẫn đúng 4 dòng), **có bản hoàn tác**.

### 4.2 🔴 Rò rỉ thật ở một tuyến API

Một tuyến đọc bảng lịch sử báo giá bằng `SELECT *` — mà bảng đó có cột giá vốn nằm trong danh sách nhạy cảm.
Tuyến **có** gác quyền, nhưng **không hề che trường**. Quyền đó đang cấp cho **bốn** vai trò, trong đó có
**Sales** và **Trưởng phòng Kinh doanh** — hai vai trò Owner khoá **không được xem giá vốn**.

Bảng hiện có **0 bản ghi** nên **chưa lộ dữ liệu thật**, nhưng lỗ hổng là thật và sẽ lộ ngay khi có bản ghi
đầu tiên. **Gác quyền không phải là che trường — cần cả hai lớp.** Đã che **tại tuyến**, trước khi dữ liệu
rời máy chủ.

**Rà bốn tuyến còn lại** đọc bảng nhạy cảm — **đều an toàn**, có bằng chứng: ba tuyến chỉ đếm, một tuyến
liệt kê cột tường minh không có giá vốn.

### 4.3 Kiểm thử mở rộng: **26 → 45 khẳng định**, bốn lớp

| Lớp | Nội dung | Kết quả |
|---|---|---|
| 1+2 | Ma trận vai trò × trường nhạy cảm, **cả 9 vai trò** | ✅ |
| 3 | Fail-closed khi thiếu cấu hình / không phiên / tài khoản không tồn tại | ✅ |
| 4 | **Quét tĩnh**: mọi tuyến chạm cột nhạy cảm phải gọi hàm che | ✅ |

**Lớp 2 là thay đổi đáng kể nhất.** Trước đây ba vai trò `TP_KINH_DOANH` · `TP_SAN_XUAT` · `TP_THIET_KE`
bị ghi *"BỎ QUA — không có tài khoản nào mang vai trò này"*. Đó chính là **ba vai trò Owner nêu đích danh**.
Nay bộ kiểm **tự dựng tài khoản thuần khiết cho cả 9 vai trò** rồi dọn ⇒ không vai trò nào còn nằm ngoài phép đo.

**Lớp 4 chính là lớp đã tìm ra rò rỉ** — ma trận vai trò không thể thấy nó, vì quyền được gác đúng.

### 4.4 ⚠️ Lớp 4 ban đầu **bị hỏng** — chỉ lộ ra nhờ kiểm ngược

| Lỗi | Hậu quả |
|---|---|
| Chỉ tìm **tên hàm** ở đâu đó trong tệp | Dòng `import` cũng chứa tên đó ⇒ **gỡ hẳn lời gọi mà cổng vẫn XANH** |
| Một lần sửa qua shell biến `\b` thành **ký tự backspace thật** | Khẳng định **luôn đỏ**, và suýt bị đọc nhầm thành "kiểm ngược đạt" |

Cả hai **chỉ lộ ra nhờ kiểm ngược**. Sau khi sửa: bản vá đầy đủ → **45/45**; gỡ hàm che → **43/45**
(đỏ **đúng cả hai** khẳng định lớp 4); khôi phục → **45/45**.

> Bài học ghi lại: một cổng báo xanh **chưa chứng minh được gì** cho tới khi có ai đó cố ý làm nó đỏ.

---

## 5. 🔴 BÍ MẬT LỘ TRONG KHO — PHÁT HIỆN NGOÀI PHẠM VI DỰ KIẾN

Rà chỉ-đọc phát hiện **mật khẩu rõ** trong tệp bị git theo dõi, mà cổng quét bí mật **không bắt được**.

### Vì sao cổng mù

Biểu thức nhận dạng **bắt buộc phải có dấu gán** (`=` `:` `??` `||`) giữa từ khoá và giá trị — đúng với mã
nguồn. Nhưng **tài liệu không viết như mã nguồn**:

| Dạng | Vì sao lọt |
|---|---|
| **Văn xuôi** — *"All test users have password `…`"* | không có dấu gán |
| **Ô bảng** — từ khoá ở **dòng tiêu đề**, giá trị ở **dòng dữ liệu** | vòng quét chạy **từng dòng một** |

Tài liệu vận hành là nơi mật khẩu thật **hay bị chép vào nhất** — đúng chỗ cổng mù.

### Đã gỡ 8 chỗ trên 3 tệp

| Tệp | Số chỗ | Mức độ |
|---|---|---|
| Báo cáo đối soát go-live | 1 | mật khẩu 14 tài khoản **thử** |
| Runbook thí điểm nội bộ | 6 | cùng giá trị, dạng bảng |
| Báo cáo dựng môi trường | 1 | 🔴 **mật khẩu tài khoản QUẢN TRỊ** |

> ### ⛔ VIỆC BẮT BUỘC CHO OWNER: **ĐỔI MẬT KHẨU**
>
> Chỗ thứ ba **không phải tài khoản thử**. Đó là mật khẩu **tài khoản quản trị**, và tệp ghi rõ nó **dùng
> chung cho cả máy phát triển lẫn máy vận hành**.
>
> Giá trị này **đã nằm trong lịch sử git**, mà Owner khoá **không viết lại lịch sử**. **Gỡ khỏi cây làm
> việc là chưa đủ — một bí mật đã công bố thì không thu hồi được bằng cách xoá tệp.**
>
> **Phải đổi mật khẩu** trên **cả hai** môi trường, rồi ghi giá trị mới vào sổ bí mật nội bộ.
> Agent **không tự đổi** vì đây là thao tác chạm thông tin đăng nhập của Owner. Ghi nợ **`DEBT-120`**, hạn **27/08**.

### Vá cổng — siết theo **hình dạng**, không theo từ khoá

| Bản | Cách làm | Báo nhầm | Bí mật thật bắt được |
|---|---|---|---|
| 1 | theo từ khoá | **31** | 0 |
| 2 | + 4 bộ lọc giá trị | 10 | 0 |
| 3 | theo **hình dạng** mật khẩu | 3 | **1 — bí mật thật** |
| 4 | + lọc ví dụ hướng dẫn | **0** | — |

Hình dạng của một mật khẩu sinh tự động: dài 8–64, có **cả** chữ hoa **lẫn** chữ thường **lẫn** chữ số,
**không có ký tự phân cách nào**. Điều kiện cuối loại sạch tên biến, đường dẫn, lớp giao diện, tên lệnh.

Thêm một bộ kiểm **theo cấu trúc bảng**: đọc dòng tiêu đề lấy **chỉ số cột**, rồi soi ô cùng chỉ số ở các
dòng dữ liệu bên dưới — bịt điểm mù mà biểu thức một-dòng không thể thấy.

**Kiểm ngược 5/5:** văn xuôi → bắt · hai dòng bảng → bắt cả hai · bảng **không có** cột mật khẩu →
**không** báo nhầm · giá trị vô hại → không báo nhầm · gỡ tệp thử → xanh lại.

> Một cổng đỏ với mọi người sẽ bị bỏ qua. Đó là lý do bản 1 và 2 **không được giữ**.

---

## 6. LỆCH LOCAL ↔ MÁY VẬN HÀNH — BẢNG ĐỐI CHIẾU CHÍNH XÁC

**Không đồng bộ mù.** Đo từng dòng, phân loại theo phụ thuộc.

| Nhóm | Local | Vận hành | Thiếu ở vận hành | Chỉ có ở vận hành |
|---|---|---|---|---|
| Vai trò | 9 | 6 | **3** | 0 |
| Mục menu | 51 | 23 | **28** | 0 |
| Quyền menu (bật) | 149 | 49 | **102** | **2** |
| Quyền hành động (bật) | 67 | 30 | **37** | 0 |

**Ba vai trò thiếu** — đúng ba vai trò Owner nêu đích danh: Trưởng phòng Kinh doanh · Trưởng phòng Sản xuất ·
Trưởng phòng Thiết kế.

**28 mục menu thiếu** thuộc các nhóm: quản trị hệ thống (3) · bán hàng (6) · sản xuất (2) · kho (10) · tài chính (7).
**Không mục nào lệch đường dẫn**, **không mục nào chỉ có ở máy vận hành** ⇒ lệch **thuần cộng thêm**, không xung đột.

### Phân loại 102 dòng quyền thiếu — quyết định thứ tự đồng bộ

| Phân loại | Số dòng | Nghĩa |
|---|---|---|
| Phụ thuộc **vai trò chưa có** | **29** | phải tạo vai trò trước |
| Phụ thuộc **mục menu chưa có** | **66** | phải tạo menu trước |
| **Sẵn sàng đồng bộ ngay** | **7** | không phụ thuộc gì |

⇒ Thứ tự bắt buộc: **vai trò → mục menu → dòng quyền**. Đảo thứ tự sẽ hỏng khoá ngoại.

### ⚠️ Hai dòng **chỉ có ở máy vận hành** — cần xem xét, không xoá mù

Hai dòng cấp quyền **cấp module** cho Kế toán và Sales. Local đã bỏ chúng, nhiều khả năng do quyết định
23/08 chuyển sang **cấp quyền theo từng màn** thay vì cấp cả module. Đồng bộ theo hướng "cho khớp local"
nghĩa là **thu hồi quyền đang có** — rủi ro cao hơn thêm quyền. **Không tự làm**; ghi lại để xử lý ở batch
triển khai.

---

## 7. KHÔNG TRIỂN KHAI — VÌ SAO

Máy vận hành chậm **53 commit**. Đây **không phải** xung đột hotfix, nhưng là **khoảng cách phát hành**.

Owner khoá: *không triển khai toàn bộ 53 commit chỉ để đưa hai bản vá Batch 1 lên*, và *chỉ chọn phương án
bằng bằng chứng*. Việc xác định **hai bản vá Batch 1 có phụ thuộc các commit ở giữa hay không** **chưa
được thực hiện** trong lượt này.

⇒ Trạng thái giữ đúng: **`PUSHED` / `NOT_DEPLOYED`**. Không tuyên bố máy vận hành đã được sửa.

---

## 8. TRẠNG THÁI BẰNG CHỨNG

| Hạng mục | Trạng thái |
|---|---|
| CEO xem giá vốn | `TEST_VERIFIED` · `COMMITTED` · `PUSHED` · **`NOT_DEPLOYED`** |
| Vá rò rỉ giá vốn ở tuyến API | `TEST_VERIFIED` · `COMMITTED` · `PUSHED` · **`NOT_DEPLOYED`** |
| Chính sách `USER` chờ cấp phát | `TEST_VERIFIED` · `COMMITTED` · `PUSHED` · **`NOT_DEPLOYED`** |
| Gỡ mật khẩu rõ + vá cổng | `TEST_VERIFIED` · `COMMITTED` · `PUSHED` · **`NOT_DEPLOYED`** |
| Màn chờ phân quyền | wireframe `REPORT_PROVEN` · **chưa code** |
| Đồng bộ lệch local ↔ vận hành | `REPORT_PROVEN` · **chưa thi hành** |
| Máy vận hành | **`RUNTIME_UNVERIFIED`** cho mọi thay đổi lượt này |

**Số phiên bản giữ nguyên** — chưa phát hành.

---

## 9. CÒN LẠI

| Việc | Trạng thái |
|---|---|
| **Đổi mật khẩu quản trị** (`DEBT-120`) | ⛔ **CẦN OWNER LÀM** — hạn 27/08 |
| Duyệt wireframe màn chờ | chờ Owner |
| Sửa nhãn `DEBT-116` nhóm (A) — 5/7 tệp là **tên bịa**, không phải dữ liệu cá nhân (`DEBT-121`) | phiên kế tiếp |
| Sổ theo dõi di trú **rỗng** ở cả hai nơi (`DEBT-122`) | trước lần di trú lược đồ kế tiếp |
| Route đặt sai module (`DEBT-123`) | hoãn có chủ đích |
| Xác định phụ thuộc 53 commit trước khi triển khai | phiên triển khai |
| Đồng bộ 3 vai trò + 28 menu + 102 dòng quyền | batch triển khai, theo thứ tự ở §6 |

---

## 10. VIỆC KẾ TIẾP — ĐÚNG MỘT VIỆC

Sang **Batch 2** — M1 dữ liệu nền và M3 bán hàng: kiểm kê route, quyền, cổng nhập dữ liệu và ba khoá đơn hàng
(hủy đơn chỉ ADMIN+CEO · giao hàng **không** tự đóng đơn · chỉ đóng khi thu đủ tiền).

---

## 11. GÓI BÀN GIAO NOTION — CHỈ XUẤT, KHÔNG GHI

> Agent IDE **không** ghi Notion và **không** tự đặt trạng thái đã đồng bộ.

| Trường | Nội dung |
|---|---|
| Mã mục sổ | `#181` |
| Quyết định | `USER` = vai trò **chờ cấp phát**, không phải vai trò nghiệp vụ |
| Mốc thật | 26/08/2026 11:28 |
| Phạm vi | Phân quyền M0 + quy trình bàn giao tài khoản |
| **Cấm mở rộng** | Không suy ra rằng mọi vai trò ít quyền đều là "chờ cấp phát". Chỉ áp cho `USER` |
| Trang Notion cần sửa | **CHƯA XÁC ĐỊNH** — TanPhatAI định vị |
| Bằng chứng | 4 commit riêng · ba cổng Phase 1 đạt 11/11, 14/14, 45/45 |
| **Chưa chứng minh được** | **Chưa có gì chạy trên máy vận hành.** Mọi kết quả là local. Không được đọc báo cáo này thành "đã sửa xong trên hệ thống thật" |

Kèm hai việc cần Notion ghi nhận: **`DEBT-120`** (đổi mật khẩu quản trị — bắt buộc) và
**`DEBT-121`** (đính chính phân loại `DEBT-116`).

---

*Báo cáo an toàn công khai: chỉ nêu tên bảng, tên vai trò, số đếm và mã commit — không có mã nguồn,
không có bí mật, không có họ tên người, không có địa chỉ thư, không có thông tin hạ tầng.*

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Giải thích ĐỦ 9 khẳng định menu; sửa 2 khẳng định đã bị quyết định Owner làm cho sai
   - Thi hành USER = chờ cấp phát: bộ phân loại + cổng chặn bàn giao 14/14, KHÔNG cần DDL
   - Xuất wireframe màn chờ phân quyền (13 tiêu chí) — CHƯA code
   - Cấp quyền xem giá vốn cho CEO; mở rộng kiểm thử 26 → 45 khẳng định, 4 lớp
   - PHÁT HIỆN + VÁ rò rỉ giá vốn ở một tuyến API (Sales và TP.KD đọc được)
   - PHÁT HIỆN + GỠ mật khẩu rõ ở 3 tệp; 1 trong đó là MẬT KHẨU QUẢN TRỊ
   - Vá cổng quét bí mật: thêm 3 luật bắt dạng văn xuôi và dạng bảng
   - Lập bảng lệch chính xác local ↔ máy vận hành, phân loại theo phụ thuộc
   - Ghi 4 nợ mới (DEBT-120..123) + mục sổ Owner #181

2. PHẠM VI
   ĐỤNG    : src/lib/rbac-cap-phat.ts (mới) · src/app/api/tinh-gia/quotes/route.ts ·
             scripts/tests/p1-*.test.ts · scripts/tests/secret-scan-gate.test.mjs ·
             migrations/20260826_p1_ceo_xem_gia_von*.sql · package.json ·
             3 tệp tài liệu chứa mật khẩu rõ · sổ nợ · sổ Owner · docs/reports/
   KHÔNG ĐỤNG: lược đồ cơ sở dữ liệu (KHÔNG DDL) · máy vận hành · số phiên bản ·
             mã tính giá · Notion · lịch sử git · route /m0/phong-ban

3. BẰNG CHỨNG
   npm run test:gov-gates            → mã thoát 0 → RUNTIME_PROVEN
   npm run test:p1-menu              → 11/11 (từ 7/9) → RUNTIME_PROVEN
   npm run test:p1-ban-giao          → 14/14 → RUNTIME_PROVEN
   npm run test:p1-gia-von           → 45/45 (từ 26) → RUNTIME_PROVEN
   npx tsc --noEmit                  → mã thoát 0 → CODE_IMPLEMENTED
   Kiểm ngược cổng giá vốn           → 45/45 → 43/45 → 45/45 → RUNTIME_PROVEN
   Kiểm ngược cổng bí mật            → 5/5 đạt → RUNTIME_PROVEN
   Kiểm ngược cổng bàn giao (dữ liệu thật) → 14/14 → 13/14 → 14/14 → RUNTIME_PROVEN
   Đo DB local + máy vận hành (chỉ đọc) → 101 bảng · 112 khoá ngoại · MariaDB 10.11.10

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #181 (USER = PROVISIONING_PENDING, nguyên văn 10 điểm khoá)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · file PHASE1-BATCH1-M0-RBAC-MENU-20260826.md
       Kho riêng: <mã-nguồn-riêng> (bảo mật) · <mã-nguồn-riêng> (USER) · <mã-nguồn-riêng> (giá vốn) + commit này

6. CÒN SÓT / CHƯA LÀM
   - DEBT-120: ĐỔI MẬT KHẨU QUẢN TRỊ — cần Owner làm, hạn 27/08. Gỡ tệp CHƯA ĐỦ
   - DEBT-121: sửa nhãn DEBT-116 nhóm (A) — 5/7 tệp là tên bịa, không phải PII
   - DEBT-122: sổ theo dõi di trú rỗng ở cả hai môi trường
   - DEBT-123: route /m0/phong-ban đặt sai module (hoãn có chủ đích)
   - Chưa xác định phụ thuộc 53 commit ⇒ chưa triển khai được
   - Chưa đồng bộ 3 vai trò + 28 menu + 102 dòng quyền lên máy vận hành
   - 2 dòng quyền CHỈ CÓ ở máy vận hành — đồng bộ nghĩa là THU HỒI quyền, chưa tự làm

7. ĐANG CHỜ OWNER
   - ĐỔI MẬT KHẨU QUẢN TRỊ (DEBT-120) → chặn: việc bàn giao thêm tài khoản
   - Duyệt wireframe màn chờ phân quyền → chặn: viết mã màn đó (KHÔNG chặn batch khác)

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Batch 2 — kiểm kê M1 dữ liệu nền + M3 bán hàng, kèm ba khoá đơn hàng.

9. CHƯA XÁC MINH ĐƯỢC
   - Mọi thay đổi lượt này CHƯA chạy trên máy vận hành. Ai xác minh: phiên triển khai
     sau khi xác định xong phụ thuộc 53 commit.
   - Màn chờ phân quyền chưa tồn tại nên 13 tiêu chí N1–N13 chưa đo được.
     Ai xác minh: Agent IDE sau khi Owner duyệt wireframe.
   - Mật khẩu quản trị đã lộ trong lịch sử git — không thể thu hồi bằng kỹ thuật.
     Ai xử lý: Owner, bằng cách đổi mật khẩu.

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — Batch 1 đạt cổng local/private đầy đủ, nhưng NOT_DEPLOYED.
       Điều kiện lên PASS: Owner đổi mật khẩu (DEBT-120) + xác định phụ thuộc
       53 commit + triển khai + kiểm khói + bằng chứng vận hành.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ (nén ở các lượt trước)
   Tài liệu tham chiếu ĐÃ ĐỌC LẠI từ đĩa trong lượt này:
     - src/lib/action-permission.ts (canViewField · maskSensitiveFields · SENSITIVE_FIELDS)
     - src/lib/security-store.ts (canViewMenu) · src/lib/security-guard.ts (requireMenuView)
     - src/app/api/tinh-gia/quotes/route.ts (toàn phần) · src/lib/m3-pricing-store.ts
     - scripts/tests/secret-scan-gate.test.mjs (RULES + BENIGN + vòng quét)
     - migrations/20260305_m0_security_rbac.sql (seed vai trò + quyền)
     - .governance/registry/tech-debt.md · docs/OWNER-REQUEST-LEDGER.md
     - package.json (chuỗi cổng) · .env.deploy (chỉ tên khoá, KHÔNG đọc giá trị)
   Không kết luận nào dựa vào trí nhớ từ trước nén; hai commit được bàn giao
   (<mã-nguồn-riêng> · <mã-nguồn-riêng>) đã đọc lại diff thật trước khi dùng.
═══════════════════════════════════════════
```
