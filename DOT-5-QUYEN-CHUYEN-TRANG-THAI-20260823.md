# ĐỢT 5 — QUYỀN CHUYỂN TRẠNG THÁI THEO TỪNG BƯỚC

> **Ngày:** 23/08/2026 · **Kho mã:** commit `7752cc5` · **Trạng thái:** ✅ XONG TRÊN MÁY PHÁT TRIỂN — **CHƯA đưa lên máy vận hành**
> **Máy vận hành hiện tại:** V1.00.355 (`0e73a7c`) — **không đụng tới trong đợt này**
> **Đây là lượt thứ 5** trong loạt 6 đợt mở rộng ma trận phân quyền (Owner khoá 23/08/2026 20:30)

---

## 1. LÀM ĐƯỢC GÌ

### 1.1 Mỗi bước của quy trình nay là một quyền riêng

Trước đợt này, ba loại chứng từ (**báo giá · đơn hàng · yêu cầu thiết kế**) đều chỉ được canh
bằng **một quyền chung**: quyền *Sửa* của mảng Bán Hàng. Hệ quả đo được:

> Ai sửa được chứng từ là **chốt đơn · đẩy sản xuất · giao hàng · đóng đơn · HUỶ đơn**
> và **duyệt mẫu thiết kế** được luôn.

Nay mỗi **trạng thái đích** là một ô tick riêng — **17 bước** trên 3 quy trình:

| Quy trình | Số bước | Ví dụ bước |
|---|---|---|
| Phê duyệt báo giá | **5** | Đã gửi · Khách chấp nhận · Đã duyệt tạo đơn · Từ chối · Hết hạn |
| Đơn hàng | **7** | Đã xác nhận · Đang sản xuất · Chờ vật tư · Đã sản xuất · Đã giao hàng · Đã đóng · Đã huỷ |
| Thiết kế | **5** | Đang thiết kế · Chờ duyệt · **Đã duyệt** · Từ chối · Chờ xử lý |

**Danh sách bước KHÔNG viết cứng trong mã** — nó được dẫn xuất từ cấu hình quy trình trong cơ sở
dữ liệu. Thêm một trạng thái vào quy trình là **ô tick mới tự xuất hiện**, không phải sửa file nào.
Đã kiểm ngược: dựng thêm một quy trình → mã quyền mới hiện ra (17→18), gỡ đi → về đúng 17.

### 1.2 Hai bước nâng lên cấp trưởng phòng

Theo yêu cầu Owner (*"trưởng phòng thiết kế và sản xuất cũng thế có thêm quyền duyệt ở những
trạng thái cao cấp hơn nhân viên"*):

| Bước | Trước | Sau |
|---|---|---|
| Duyệt mẫu thiết kế | **Nhân viên tự duyệt mẫu của mình** | Chỉ Trưởng Phòng Thiết Kế |
| Duyệt báo giá tạo đơn | Đã khoá riêng từ 21/08 | Thêm Trưởng Phòng Kinh Doanh |

Kết quả cấp quyền sau đợt: nhân viên kinh doanh **15 bước** (giữ nguyên hiện trạng, **trừ**
quyền tự duyệt mẫu) · Trưởng Phòng Kinh Doanh **5 bước** báo giá · Trưởng Phòng Thiết Kế **5 bước**
thiết kế · Tổng giám đốc **5 bước** báo giá.

### 1.3 Lập ≠ duyệt cho phiếu thu · phiếu chi

Trước đợt này, bước **duyệt** phiếu thu/chi dùng **chung một mã quyền** với bước **lập** phiếu
⇒ người lập phiếu **tự duyệt phiếu của chính mình**. Nay tách thành hai quyền riêng, và
**chưa vai trò nào được cấp** — mặc định CẤM cho tới khi Owner tick.

### 1.4 Màn phân quyền

Thêm mục **Quyền chuyển trạng thái** vào màn quản trị bảo mật: TAB theo từng quy trình
(hiện số đã cấp / tổng), ô tìm kiếm **bỏ dấu** trên toàn bộ quy trình, nút tick/bỏ tick cả quy trình,
và mỗi dòng ghi rõ **bước này đi từ trạng thái nào** bằng tiếng Việt.

---

## 2. 🔴 LỖI NẶNG PHÁT HIỆN VÀ ĐÃ VÁ TRONG ĐỢT NÀY

### Quyền GHI bị mất ở Đợt 3 — chỉ ảnh hưởng máy phát triển, máy vận hành không dính

Đợt 2–3 chuyển quyền từ **khoá mảng thô** sang **khoá từng màn** và thu hồi khoá thô.
Nhưng chỉ **đường XEM** được nối theo; **đường GHI** vẫn hỏi khoá mảng ⇒ tra vào dòng đã bị thu hồi
và **từ chối**. Đo trên máy phát triển: **vai trò kinh doanh không lập/sửa được báo giá, đơn hàng,
yêu cầu thiết kế** — mất sạch quyền ghi ở mảng chạy việc chính.

**Vì sao không cổng nào bắt được:** toàn bộ khoá bất biến của Đợt 1–4 đo qua bộ phân giải
**đường XEM**. Không cổng nào đo đường GHI. Nay đã thêm phép đo đi thẳng đường ghi.

**Đã vá 3 lớp:**

1. Bộ phân giải quyền ghi **lùi về khoá mảng cha chỉ khi màn đó chưa có dòng quyền riêng** —
   có dòng rồi thì dòng đó quyết định tuyệt đối, nên **bỏ tick vẫn ngưng ngay**.
2. Chuyển **42 điểm gọi** của mảng Bán Hàng sang khoá từng màn.
3. Chép lại **đúng cờ quyền ghi Owner đã duyệt ở Đợt A** xuống các khoá con — **17 dòng**,
   không thêm mảng nào, không thêm cờ nào.

**Chứng minh không ai mất quyền:** chụp ma trận trước/sau, so từng ô — **7/7 dòng có quyền ghi ở
mốc Owner duyệt vẫn còn nguyên**.

---

## 3. HAI LỖI KHÁC — CỔNG TỰ BẮT TRƯỚC KHI GHI VÀO DỮ LIỆU

| Lỗi | Hậu quả nếu lọt |
|---|---|
| Script cấp cờ Sửa cho **vai trò quản trị** | Bảng phình dòng vô nghĩa; ma trận trông như quyền đến từ ô tick trong khi thực tế đến từ cờ quản trị |
| Script mở cờ Sửa lên **cả 6 màn** chỉ vì một quyền duyệt báo giá | **Đúng lỗi nới quyền đã bắt ở Đợt 3** — cấp 1 màn hoá ra cấp cả mảng. Đã đổi sang ghi **đích danh màn** |

### Bẫy kỹ thuật đã gỡ

Mã của **khoá con** phải suy ra từ khoá, không đọc thẳng từ danh mục — đọc thẳng sẽ nhận về
**mã mảng thô cho mọi màn**, tức ghi quyền của 6 màn đè lên cùng một dòng. Bẫy này đã làm script nạp
chạy ra **kết quả rỗng mà không báo lỗi gì**. Phép suy trước đó bị **chép tay ở 2 nơi** — nay gom về
**một hàm chung**.

### Lỗi giao diện chỉ ảnh chụp mới thấy (lần thứ 6 trong ngày)

Dòng *"Từ: …"* của mỗi bước **bày mã kỹ thuật** cho người quản trị đọc, trong khi nhãn tiếng Việt
có sẵn ngay trong cấu hình. Đã tách: **mã để truy vết, nhãn để đọc** — hai việc khác nhau nên không
dùng chung một trường.

---

## 4. BẰNG CHỨNG ĐO ĐƯỢC

| Đo cái gì | Kết quả |
|---|---|
| Bộ kiểm mới (quyền chuyển trạng thái) | **29 / 29 PASS** |
| Toàn bộ bộ kiểm | **20 bộ · 560 khẳng định · 0 FAIL** |
| Cổng quản trị | **37 / 37 PASS** · 6 cổng riêng đều PASS |
| Ảnh chụp thật 1920 · 1024 · 390 px | **21 PASS** — có TAB, ô tick bấm được, **không tràn ngang** ở cả 3 |
| Kiểm kiểu + dựng bản phát hành | `tsc` 0 lỗi · dựng thoát mã 0 |
| Lược đồ cơ sở dữ liệu | **101 → 101 bảng — không đổi** |
| Bỏ tick 1 bước | Đúng bước đó ngưng ngay · **các bước khác không đổi** · tick lại dùng được ngay |
| Đổi **tên** vai trò | **17/17 bước** cho kết quả y hệt (liên kết bằng mã, không bằng nhãn) |

**Tiêu chí nghiệm thu 12 mục được chốt RA FILE TRƯỚC khi sửa dòng mã đầu tiên** — đạt **12/12**.

---

## 5. NÓI THẲNG — NHỮNG GÌ CHƯA LÀM

| Việc | Vì sao |
|---|---|
| **Che trường nhạy cảm** | Bộ phân giải có sẵn nhưng **0 nơi gọi**, và danh mục trường nhạy cảm **trùng đúng chức năng** với mã quyền đang chạy. Nối vào lúc này tạo **hai nguồn sự thật cạnh tranh** cho cùng một câu hỏi. **Chờ Owner chốt giữ cái nào** |
| **Quyền theo CẶP bước** (huỷ đơn nháp ≠ huỷ đơn đang sản xuất) | Mã sẽ dài **36 ký tự** > trần **32** của cột. Muốn có phải đổi lược đồ — cần Owner duyệt riêng |
| **Trưởng Phòng Sản Xuất chưa có bước nào để duyệt** | Cơ sở dữ liệu **không có quy trình sản xuất** — chỉ có 3 quy trình: báo giá · đơn hàng · thiết kế |
| **Bảy mảng còn lại vẫn canh quyền ghi bằng khoá mảng thô** | **211 điểm gọi**. Dọn khi đụng mảng vì lý do chức năng, **không mở đợt quét diện rộng** |
| Đưa Đợt 1–5 lên máy vận hành | Chưa — máy vận hành vẫn V1.00.355. Gộp một lần khi phát hành |

---

## 6. CẦN OWNER QUYẾT

1. **Bước cấp cao của đơn hàng** — hiện nhân viên kinh doanh vẫn giữ nguyên hiện trạng, tức
   **huỷ đơn** và **đóng đơn** được. Đây là parity, không phải quyết định của hệ thống.
   Owner muốn giới hạn thì **bỏ tick** trên màn phân quyền là xong.
2. **Trưởng Phòng Sản Xuất** — khai quy trình sản xuất, hay cấp thêm màn Đơn Hàng để tick
   hai bước sản xuất của đơn?
3. **Che trường nhạy cảm** — giữ mã quyền đang chạy, hay chuyển sang che theo trường?

---

*Báo cáo công khai — không chứa thông tin đăng nhập, dữ liệu khách hàng, số liệu tài chính
hay định danh hạ tầng.*
