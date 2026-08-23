# THI CÔNG GO-LIVE CHUỖI BÁN HÀNG — 4 ĐỢT A · B · C · D

**Ngày:** 23/08/2026 · **Loại:** đợt **MÃ NGUỒN + KIỂM THỬ** — **CHƯA phát hành lên hệ thống vận hành**
**Hệ thống vận hành vẫn ở:** `V1.00.352` · **Phiên bản dự kiến phát hành:** `V1.00.353` (chưa deploy)

**Mã ghi nhận (commit) — kiểm lại được trên kho mã:**

| Đợt | Mã commit | Tiêu đề |
|---|---|---|
| **A** | `e122510` | nạp ma trận phân quyền lên hệ thống vận hành (không sửa mã ứng dụng) |
| **B** | `5348619` | vá 6 lỗ hổng CHẶN go-live + bộ khoá 29 khẳng định |
| **C** | `ffe01ce` | hoàn thiện đơn hàng (ngày giao · địa chỉ giao · sửa/xoá đơn nháp · tiền đúng) |
| **D** | `e430ee3` | ma trận tick quyền hành động trên màn quản trị bảo mật |
| *(kèm)* | `71b7482` | kế hoạch quay lui `V1.00.353` — viết **trước** khi deploy |

> Bản tin public-safe: chỉ nêu số lượng, mã kỹ thuật, tên màn hình và tên quyền.
> **Không** có thông tin đăng nhập, **không** có dữ liệu khách hàng, **không** có số tiền/công nợ thật.

---

## 0. Vì sao có 4 đợt này

Trước đó đã có một lượt **khảo sát go-live** (chỉ đo, không sửa mã) rà chuỗi nghiệp vụ
**Khách hàng → Báo giá → Đơn hàng** trên dữ liệu thật. Lượt khảo sát đó tìm ra **27 điểm chặn**.

Chủ dự án chốt: **thi công tuần tự A → B → C → D**, đợt trước phải đạt mới được sang đợt sau,
**sai bất kỳ đâu thì dừng cả chuỗi**, và **chỉ phát hành đúng MỘT lần ở cuối**.

Báo cáo này tổng kết cả 4 đợt.

---

## 1. NGUYÊN TẮC CHỦ DỰ ÁN KHOÁ VĨNH VIỄN (nền của đợt A và D)

> Quyền = **ma trận tick ĐƯỢC / CẤM từng quyền một**.
> **Cấm gắn cứng quyền theo tên, theo nhãn, theo chức danh — và theo cả mã vai trò.**
> Gắn cứng chỉ dành cho quyền khoá chặt không bao giờ được cấp.
> Mọi liên kết nối bằng **khoá dữ liệu**, không nối bằng tên — **đổi tên bao nhiêu lần quyền cũng không đổi**.

Hai đợt A và D là để **thi hành nguyên tắc này bằng công cụ**, chứ không chỉ bằng lời hứa.

---

## 2. ĐỢT A — NẠP MA TRẬN PHÂN QUYỀN LÊN HỆ THỐNG VẬN HÀNH

**Không sửa một dòng mã ứng dụng nào.** Chỉ nạp **dữ liệu phân quyền**.

### Đã làm

| Việc | Chi tiết |
|---|---|
| Xuất ma trận từ máy nội bộ | Kịch bản xuất riêng, ghi ra tệp trung gian **không đưa lên kho** |
| Nạp lên hệ thống vận hành | Kịch bản nạp **CHỈ THÊM và CẬP NHẬT — TUYỆT ĐỐI KHÔNG XOÁ**. Dòng nào chỉ có ở hệ thống vận hành thì **liệt kê ra cho chủ dự án xem**, không tự ý gỡ |
| Chạy thử trước | Mặc định là chế độ **xem trước**; phải thêm cờ riêng mới thật sự ghi |
| Áp mẫu quyền cho Kinh doanh | Theo đúng bộ quyền đã chốt |
| Kế toán | Đúng quyết định đã khoá ngày 01/08: **đọc được toàn bộ danh bạ khách hàng**, nhưng **không sửa, không xoá, không chuyển giao, không xem giá vốn** |
| Duyệt giá | Cấp cho **quản trị hệ thống** và **tổng giám đốc** |
| Phạm vi khách hàng của kinh doanh | Giữ nguyên mô hình **chặn-mặc-định** (mỗi người chỉ thấy khách mình phụ trách) — chủ dự án chốt **không đổi mã** |

### Cách kiểm — **đo, không phải bấm thử**

Không đăng nhập thay nhân viên. Thay vào đó gọi thẳng **tầng phân quyền thật** của hệ thống
cho từng tài khoản, rồi so kết quả với bảng quyền mong muốn.

**Kết quả: 22/22 điểm đo đạt.**

> ⚠️ **Một lỗi của chính lượt đo, tự phát hiện tự sửa:** bản đầu của kịch bản đo đọc **sai tên trường**
> nên kết quả luôn là "không có quyền" — mà "không có quyền" lại đúng bằng kỳ vọng, nên **đạt vì lý do sai**.
> Đã phát hiện, sửa tên trường, đo lại. Ghi lại đây để nhớ: **đạt chưa chắc đã đúng**.

---

## 3. ĐỢT B — VÁ 6 LỖ HỔNG CHẶN GO-LIVE

Đây là 6 điểm mà nếu đưa vào vận hành sẽ **gây thiệt hại thật**.

| # | Lỗ hổng | Trước khi vá | Sau khi vá |
|---|---|---|---|
| **B1** | **Vượt cửa duyệt giá** | Trạng thái báo giá lấy từ dữ liệu **trình duyệt gửi lên**. Người không có quyền duyệt vẫn tự đặt trạng thái "đã duyệt" rồi đi tiếp | Trạng thái **do máy chủ tự quyết**, không nhận từ trình duyệt. Báo giá tạo mới **luôn là nháp** |
| **B2** | **Lộ giá vốn ở 3 chỗ** | Ba màn khác nhau trả dữ liệu **kèm giá vốn** cho cả người không được xem | Cả ba đi qua **một bộ lọc chung** che đúng trường nhạy cảm |
| **B3** | **Duyệt được báo giá giá 0đ** | Duyệt xong, đơn hàng sinh ra **tiền bằng 0** | Bắt buộc **mọi dòng phải có đơn giá > 0 và số lượng > 0** mới cho duyệt, báo rõ tên sản phẩm nào thiếu giá |
| **B4** | **Nuốt lỗi im lặng** | Thao tác thất bại nhưng màn hình **không báo gì** — người dùng tưởng đã lưu | Mọi nhánh lỗi đều **hiện thông báo tiếng Việt**, nói rõ việc gì thất bại |
| **B5** | **Quyền gắn cứng theo mã vai trò** | Màn giao việc thiết kế kiểm quyền bằng cách **so tên vai trò trong mã nguồn** — trái nguyên tắc chủ dự án khoá | Chuyển sang đọc **ma trận quyền** |
| **B6** | **Đặt lại trùng mật khẩu cũ** | Hệ thống **cho đặt lại đúng mật khẩu đang dùng**. Ngày đổi và mã băm đều thay đổi, nên **mọi dấu hiệu đều báo "đã đổi"** trong khi giá trị không đổi | Chặn ở **hai tầng** (giao diện + máy chủ), so bằng thuật toán mã hoá chứ không so chuỗi |

> 🔴 **Lỗ hổng B6 do chính chủ dự án phát hiện khi dùng thật** — không phải máy tìm ra.
> Đây là loại lỗi nguy hiểm nhất: **mọi tín hiệu đều báo an toàn trong khi thực tế không an toàn.**

### Bằng chứng

- Bộ kiểm thử riêng cho 6 lỗ hổng: **29/29 đạt**.
- **Chứng minh bộ kiểm thử có tác dụng thật:** cố ý cắm lại từng lỗi vào → bộ kiểm thử **báo đỏ đúng chỗ**; trả mã về nguyên bản → **đạt trở lại**.

> ⚠️ **Đợt B để sót một điểm và bị chính bộ kiểm thử bắt:** lượt vá đầu chỉ sửa **một** trong **hai**
> chỗ gắn cứng mã vai trò. Bộ kiểm thử phát hiện, không phải con người. Đây là lý do phải viết kiểm thử
> trước khi tuyên bố xong.

---

## 4. ĐỢT C — HOÀN THIỆN ĐƠN HÀNG

| # | Nội dung | Kết quả |
|---|---|---|
| **C1** | **Ngày giao dự kiến** | Người dùng **tự chọn**, sửa được khi đơn còn nháp. Bắt buộc nhập, và **không cho chọn ngày trước ngày đặt hàng** |
| **C2** | **Địa chỉ giao hàng** | Chọn từ **danh bạ địa chỉ của đúng khách hàng đó**, hoặc **thêm địa chỉ mới ngay trên biểu mẫu**. Địa chỉ mới **được ghi vào danh bạ của khách** để lần sau dùng lại. Tỉnh/thành **tự nhận diện** theo danh mục hành chính; không nhận ra thì đánh dấu **"Chưa xác định"** và **giữ nguyên văn bản gốc** — không bịa, không bỏ |
| **C3** | **Sửa / xoá đơn** | Đơn **còn nháp**: sửa và xoá được. Đơn **đã xác nhận**: khoá lại, không sửa không xoá |
| **C4** | **Tiền và công nợ** | Số liệu tổng hợp **loại trừ đơn còn nháp và đơn đã huỷ** — trước đây đơn nháp cũng bị tính vào công nợ |

**Chốt chặn quan trọng ở C2:** chọn nhầm địa chỉ của **khách khác** thì hệ thống **từ chối thẳng**.
Với gần **1.700 khách hàng**, nhầm địa chỉ nghĩa là **giao hàng sai nơi**.

**Dọn một nguồn sai lệch có sẵn:** trước đây có **hai đường tạo đơn khác nhau** (từ màn báo giá và từ màn
đơn hàng), mỗi đường một cách xử lý. Nay **gộp về một đường duy nhất** — đây đúng loại gốc rễ mà lượt khảo
sát đã chỉ ra: cùng một việc, mỗi nơi làm một kiểu, rồi số liệu lệch nhau.

**Bằng chứng:** bộ kiểm thử riêng **24/24 đạt**. Kiểm ngược: cắm 3 lỗi vào → **đúng 3 điểm báo đỏ** →
trả mã về → **24/24 đạt trở lại**.

---

## 5. ĐỢT D — MA TRẬN TICK QUYỀN HÀNH ĐỘNG

### Vấn đề trước đợt này

Màn quản trị bảo mật **chỉ tick được quyền vào từng khu vực (menu)**. Còn các **quyền hành động**
(duyệt báo giá · xem giá vốn · chuyển khách hàng · lập phiếu thu chi …) thì **không có chỗ nào bật lên được**.

Hệ quả: một quyền mới sinh ra trong hệ thống là **CẤM vĩnh viễn** với mọi vai trò không phải quản trị —
muốn cấp phải nhờ kỹ thuật chạy lệnh tay.

### Đã làm

| Việc | Chi tiết |
|---|---|
| **Danh mục 15 quyền hành động** | Gom theo **6 tầng** đúng như đã khoá ngày 01/08: **Vào · Xem · Thêm · Sửa · Duyệt · Nhạy cảm** |
| **Ma trận tick** | Tick theo **vai trò × hành động** ngay trên màn quản trị bảo mật. Mỗi dòng có tên tiếng Việt + câu giải thích *"bật quyền này thì người dùng làm được gì"* |
| **Ô chưa tick = CẤM** | Ghi rõ ngay trên màn hình, không để người quản trị phải đoán |
| **Lưu ngay khi tick** | Không phải bấm thêm nút. Máy chủ từ chối thì **ô tick tự quay về giá trị cũ** — tránh cảnh nhìn thấy đã tick mà thực tế chưa cấp |
| **Vai trò quản trị** | Hiện **đã tick sẵn + khoá lại + có dòng giải thích** — vì quản trị vốn có sẵn mọi quyền. Không để người dùng bấm rồi mới báo lỗi |
| **Ghi nhật ký** | Mỗi lần cấp / thu hồi quyền đều **ghi nhật ký ai làm, làm gì, lúc nào** |

### Ba chốt an toàn ở đường ghi

1. **Mã quyền phải có trong danh mục** — chặn ghi mã lạ thành dòng rác không tầng nào đọc.
2. **Vai trò phải tồn tại thật** — chặn dòng mồ côi.
3. **Khoá quá dài thì báo lỗi, KHÔNG tự cắt** — cắt cho vừa là sinh trùng khoá, hai quyền khác nhau đè lên nhau.

### Một cổng tự động chống bịa và chống sót

Có cổng **quét ngược toàn bộ 482 tệp mã nguồn**, đối chiếu với danh mục 15 quyền:

- Thêm một quyền **không có thật** vào danh mục → cổng **báo đỏ**.
- Thêm quyền mới vào mã mà **quên khai** vào danh mục → cổng cũng **báo đỏ**.

Nghĩa là ma trận **không thể trôi lệch khỏi mã nguồn** theo thời gian.

### Kết quả đo — đúng 3 điều chủ dự án yêu cầu

| Yêu cầu | Kết quả đo |
|---|---|
| Tick đổi → quyền đổi **NGAY** | ✅ Đo qua **tầng phân quyền thật**: bật → có quyền ngay; tắt → mất quyền ngay |
| Đổi tên chức danh → quyền **y hệt** | ✅ Đổi tên vai trò **3 lần** liên tiếp → dấu vân tay quyền **0 khác biệt** |
| Bộ khoá bất biến **17/17** vẫn xanh | ✅ 17/17 |

**Thêm các điểm đo khác:**

- Vai trò **mới tinh chưa tick gì** → **15/15 quyền đều CẤM** (đúng mặc-định-cấm).
- Tick **một** quyền **không làm rò rỉ** 14 quyền còn lại.
- Vai trò không tồn tại và mã quyền bịa → **đều bị từ chối**, không sinh dòng rác.
- Quyền ghi bằng **khoá dữ liệu thật** (đã kiểm có ràng buộc khoá ngoại), không ghi bằng tên.

**Bộ kiểm thử: 48/48 đạt.**
**Kiểm ngược:** cắm **4 lỗi** riêng biệt → **4 lần báo đỏ đúng chỗ** → trả mã về → **xanh lại toàn bộ**.

### Giao diện — làm theo tài liệu chuẩn, và bị ảnh chụp bắt lỗi

Trước khi sửa giao diện đã **đọc toàn bộ tài liệu chuẩn giao diện (451 dòng)** và lập
**bảng nghiệm thu 57 tiêu chí**: **26 đạt · 30 không áp dụng (đều ghi rõ lý do) · 1 nợ đã biết · 0 trượt.**

Đã chụp **13 ảnh kiểm thử thật** ở **3 kích thước màn hình** (máy để bàn · máy tính bảng · điện thoại).

> 🔴 **Ảnh điện thoại lộ một lỗi thật mà đọc mã không thấy:** cột tick bị đẩy **ra khỏi màn hình**,
> người dùng điện thoại không bấm được. Đã sửa theo đúng cách tài liệu chuẩn quy định (ẩn cột phụ ở
> màn nhỏ), chụp lại xác nhận, và **thêm một cổng tự động khoá lại điểm đó** để không tái diễn.
>
> Đây là lý do tài liệu chuẩn **bắt chụp ảnh trước khi tuyên bố đạt**.

---

## 6. TỔNG BẰNG CHỨNG CỦA CẢ 4 ĐỢT

| Hạng mục | Số đo |
|---|---|
| Bộ kiểm thử chạy | **17 bộ** |
| Tổng số điểm kiểm | **666 khẳng định** |
| Số điểm **trượt** | **0** |
| Bộ khoá nguyên tắc "quyền theo vai trò, không theo chức danh" | **17/17** |
| Kiểm tra kiểu dữ liệu toàn dự án | **0 lỗi** |
| Dựng bản phát hành | **thành công** |
| Cổng quét thông tin nhạy cảm / dữ liệu cá nhân / lỗi cú pháp | **đều đạt** |
| **Thay đổi cấu trúc cơ sở dữ liệu** | **0** — đã kiểm bằng máy, cả 4 đợt **không có** thay đổi lược đồ |

**Đã kiểm ngược tổng cộng 8 lỗi cắm vào** (3 ở đợt C, 4 ở đợt D, và ở đợt B) — **lần nào cũng báo đỏ đúng chỗ**,
trả mã về thì xanh lại. Nói cách khác: các bộ kiểm thử này **không phải xanh giả**.

---

## 7. 🔴 CHƯA PHÁT HÀNH — VÀ VÌ SAO

Chủ dự án ra lệnh **"deploy đúng MỘT lần ở cuối"**, kèm **cổng nghiệm thu bắt buộc trước khi phát hành**:
12 điểm nghiệm thu, mỗi điểm phải có **ảnh chụp màn hình thật** trên hệ thống vận hành **và** **truy vấn dữ liệu thật**.

| Việc | Trạng thái |
|---|---|
| Kế hoạch quay lui | ✅ **Đã viết sẵn trước khi deploy** (đúng thứ tự chủ dự án yêu cầu) |
| Truy vấn dữ liệu thật trên hệ thống vận hành | ✅ Làm được |
| **Ảnh chụp thật trên hệ thống vận hành** | ❌ **KHÔNG làm được** |

**Lý do:** chủ dự án đã **đổi mật khẩu quản trị hệ thống vận hành** ngày 23/08 và **tự giữ giá trị mới**
(đúng nguyên tắc bảo mật — sổ nội bộ không ghi). Nên phía kỹ thuật **không đăng nhập được** để chụp ảnh.

**Hai hướng, chủ dự án chọn một:**

| Cách | Chủ dự án làm gì | Phía kỹ thuật làm gì |
|---|---|---|
| **(a)** | Đưa mật khẩu qua kênh riêng | Chụp đủ 12 điểm, chạy trọn diễn tập → nghiệm thu → phát hành → kiểm khói |
| **(b)** | Tự mở 7 màn, chụp ảnh gửi lại | Lo phần truy vấn dữ liệu + diễn tập + phát hành + kiểm khói |

> ⚖️ **Sẽ KHÔNG phát hành khi chưa qua cổng nghiệm thu** — làm vậy là bỏ qua chính cổng chủ dự án đặt ra.

### Việc quay lui đã được chuẩn bị sẵn

Vì cả 4 đợt **không đổi cấu trúc cơ sở dữ liệu**, nên quay lui **chỉ cần đưa lại mã nguồn bản cũ** —
**không phải hoàn tác dữ liệu**, không có bước nguy hiểm. Thời gian dự kiến **5–10 phút**.
Đã chốt sẵn **6 ngưỡng bắt buộc quay lui ngay** và **7 đường kiểm khói sau phát hành**.

---

## 8. NHỮNG ĐIỀU NÓI THẲNG

**1. Chuỗi báo cáo công khai đã bị đứt 4 đợt.**
Đợt A, B, C, D đều đã xong ở kho mã nhưng **không đợt nào có báo cáo công khai** cho tới bản tin này.
Đây là **thiếu sót về kỷ luật báo cáo**, đã ghi nhận.

**2. Trong quá trình làm có 4 lần tự bắt lỗi của chính mình:**

| Lỗi | Cách phát hiện |
|---|---|
| Kịch bản đo đợt A đọc sai tên trường → **đạt vì lý do sai** | tự rà lại |
| Đợt B sót một trong hai chỗ gắn cứng mã vai trò | **bộ kiểm thử bắt**, không phải người |
| Cổng kiểm tra ở đợt D cắt nhầm vùng mã → báo vi phạm của **đoạn mã khác** | tự rà lại |
| Khẳng định "một tệp tài liệu không tồn tại" — **sai**, tệp có thật | tự kiểm lại bằng lệnh khác |

**3. Một lần suýt đọc nhầm tài liệu chuẩn.**
Công cụ đọc tệp trả về **bản cũ 371 dòng** trong khi bản thật trên đĩa là **451 dòng** — thiếu 80 dòng,
trong đó có **2 quy định mới** áp thẳng vào việc đang làm. Đã đối chiếu số dòng và đọc lại bản thật
trước khi sửa. Nếu không kiểm, đợt D đã bỏ sót 2 quy định đó.

**4. Ba việc còn chờ chủ dự án quyết** *(chi tiết ở mục 7 và trong sổ nợ kỹ thuật nội bộ)*:
mật khẩu để chụp ảnh nghiệm thu · một nút quản trị còn gắn cứng mã vai trò (mức thấp, chỉ ghi vào ma trận
chứ không phải tầng thi hành quyền) · một cổng tự động có phạm vi quét hẹp hơn phạm vi luật.

---

## 9. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC

**Chủ dự án chọn cách (a) hoặc (b) ở mục 7.** Chọn xong là chạy được liền:
diễn tập trên bản sao mới → nghiệm thu 12 điểm → sao lưu → phát hành `V1.00.353` → kiểm khói 7 đường.

---

*Báo cáo public-safe. Không chứa mã nguồn, thông tin đăng nhập, dữ liệu khách hàng hay số liệu tài chính thật.*
