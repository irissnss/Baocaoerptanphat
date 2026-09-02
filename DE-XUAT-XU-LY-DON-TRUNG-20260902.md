# BẢN ĐỀ XUẤT — XỬ LÝ 6 ĐƠN HÀNG TRÙNG TRÊN MÁY VẬN HÀNH

> **Anh giao ngày 02/09/2026:** *«Em lập bản đề xuất, anh duyệt rồi mới xoá»*.
> **Trạng thái:** ⏸️ **CHỜ ANH DUYỆT — em CHƯA đụng gì vào dữ liệu.**
> Số liệu đo **trực tiếp trên máy vận hành** lúc lập bản này, không lấy lại số cũ.

---

## 1. ĐIỀU BẤT NGỜ — GIẢ ĐỊNH CŨ SAI

Sổ nợ ghi từ 30/08: *«cả 6 đơn là dữ liệu thử, chưa có thiệt hại»*. Em đo lại thì **không đơn giản như vậy**.

> ### ⚠️ Hai trong sáu đơn đã đi vào SẢN XUẤT — có lệnh sản xuất thật gắn vào.
> Nếu xoá theo giả định cũ, **sẽ mất 4 dòng lệnh sản xuất và 1 yêu cầu thiết kế.**

Đây là lý do anh bắt lập bản đề xuất trước khi xoá — và nó vừa cứu đúng một lần.

---

## 2. SỐ LIỆU THẬT — ĐO TRÊN MÁY VẬN HÀNH

**Toàn bộ máy vận hành chỉ có 6 đơn hàng, và cả 6 đều là đơn trùng** (3 báo giá × 2 đơn).

| Cặp | Báo giá | Đơn | Mã đơn | Trạng thái | Dòng hàng | Công nợ | Yêu cầu thiết kế | **Lệnh sản xuất** |
|---|---|---|---|---|---|---|---|---|
| **A** | `BG2602030001` | id 1 | `DH2602030001` | **đang sản xuất** | 1 | 0 | 0 | **0** |
| **A** | | id 2 | `DH2602030002` | đã xác nhận | 1 | 0 | 0 | **0** |
| **B** | `BG2602030004` | id 3 | `DH2602030003` | nháp | 1 | 0 | 0 | **0** |
| **B** | | id 5 | `DH2602060001` | nháp | 1 | 0 | 0 | **0** |
| **C** | `BG2602030007` | id 4 | `DH2602030004` | đã xác nhận | 1 | 0 | **1** | **2** ⚠️ |
| **C** | | id 6 | `DH2602060002` | đã xác nhận | 1 | 0 | 0 | **2** ⚠️ |

**Đọc bảng này thế nào:**
- **Công nợ = 0 ở cả sáu đơn** → chưa đơn nào phát sinh tiền. Đây là tin tốt nhất.
- **Cặp C nguy hiểm nhất:** cả hai đơn đều có **2 dòng lệnh sản xuất**, và một đơn còn kèm yêu cầu thiết kế.
- **Cặp A hơi lạ:** đơn id 1 mang trạng thái «đang sản xuất» nhưng **không có lệnh sản xuất nào**. Tức trạng thái đó được đặt bằng tay, không có việc thật phía sau.
- **Cặp B sạch nhất:** cả hai còn ở nháp, không ràng buộc gì.

---

## 3. BA CÁCH XỬ LÝ — EM ĐỀ XUẤT CÁCH 2

### Cách 1 — Xoá 3 đơn thừa (mỗi cặp giữ đơn cũ hơn)

| Xoá | Giữ |
|---|---|
| id 2 `DH2602030002` · id 5 `DH2602060001` · id 6 `DH2602060002` | id 1 · id 3 · id 4 |

- ✅ Sạch nhất, cả 3 báo giá tạo lại đơn mới được ngay.
- 🔴 **Mất 2 dòng lệnh sản xuất của đơn id 6.** Không lấy lại được nếu không có sao lưu.
- 🔴 Xoá dữ liệu trên máy đang chạy — rủi ro cao nhất.

### ✅ Cách 2 — Đánh dấu HUỶ, không xoá gì *(em đề xuất)*

Chuyển 3 đơn thừa sang trạng thái `cancelled`, giữ nguyên mọi dữ liệu.

- ✅ **Không mất một dòng nào** — lệnh sản xuất, yêu cầu thiết kế đều còn.
- ✅ Hoàn tác được: đổi trạng thái ngược lại là xong.
- ✅ Giữ được vết để sau này tra «vì sao có hai đơn».
- ⚠️ **Cần em làm thêm một việc nhỏ:** chốt chặn hiện tại đếm **mọi** đơn, kể cả đơn đã huỷ ⇒ ba báo giá đó **vẫn kẹt**, không tạo được đơn mới. Em sẽ sửa chốt chặn thành **bỏ qua đơn đã huỷ** — khoảng 1 dòng mã, và có bài kiểm canh.
- 🟡 Ba đơn huỷ vẫn hiện trong danh sách (có nhãn «đã huỷ»).

### Cách 3 — Không đụng gì

- ✅ Rủi ro bằng 0.
- 🔴 Ba báo giá `BG2602030001` · `BG2602030004` · `BG2602030007` **vĩnh viễn không tạo được đơn mới** sau khi triển khai.
- 🔴 Sổ sách để lại 6 đơn cho 3 việc — người xem báo cáo sẽ hiểu sai số liệu.

---

## 4. VÌ SAO EM ĐỀ XUẤT CÁCH 2

Ba lý do, xếp theo thứ tự quan trọng:

1. **Không mất dữ liệu nào.** Cặp C có lệnh sản xuất thật. Dù nó sinh từ thao tác thử, việc xoá lệnh sản xuất trên máy đang chạy là loại việc không nên làm khi có cách khác.
2. **Hoàn tác được trong một câu lệnh.** Nếu anh đổi ý, đảo trạng thái lại là xong. Xoá thì không.
3. **Sửa được cả gốc lẫn ngọn.** Việc sửa chốt chặn để bỏ qua đơn huỷ là **đúng về nghiệp vụ**: một đơn đã huỷ thì không nên chặn việc tạo đơn mới — dù có đợt dọn này hay không.

---

## 5. NẾU ANH DUYỆT CÁCH 2 — EM SẼ LÀM ĐÚNG NHỮNG BƯỚC NÀY

| # | Bước | Bảo vệ |
|---|---|---|
| 1 | Sao lưu trọn cơ sở dữ liệu máy vận hành, ghi rõ tên tệp và dung lượng | Có đường lui |
| 2 | Sửa chốt chặn: bỏ qua đơn ở trạng thái `cancelled` | Kèm bài kiểm canh, chạy trước khi lên |
| 3 | Chạy **xem trước**: in ra đúng 3 đơn sẽ đổi, chưa ghi gì | Anh xem trước khi ghi thật |
| 4 | Đổi trạng thái 3 đơn trong **một giao dịch** (hỏng thì hoàn nguyên trọn) | Không có trạng thái nửa vời |
| 5 | Đo lại: 3 đơn huỷ · 3 đơn còn hiệu lực · **lệnh sản xuất vẫn đủ 4 dòng** | Chứng minh không mất gì |
| 6 | Thử tạo đơn mới từ một báo giá vừa gỡ kẹt | Chứng minh đã thông |

**Ba đơn đề nghị đánh dấu huỷ** (mỗi cặp giữ đơn được tạo trước):

| Huỷ | Giữ | Vì sao giữ cái kia |
|---|---|---|
| id 2 `DH2602030002` | id 1 `DH2602030001` | id 1 đang ở trạng thái sản xuất |
| id 5 `DH2602060001` | id 3 `DH2602030003` | id 3 tạo trước |
| id 6 `DH2602060002` | id 4 `DH2602030004` | id 4 có kèm yêu cầu thiết kế |

⚠️ **Riêng cặp C xin anh xác nhận thêm:** cả id 4 và id 6 đều có 2 dòng lệnh sản xuất. Em chọn giữ id 4 vì nó có thêm yêu cầu thiết kế. Nếu anh biết đơn nào mới là đơn thật thì anh chỉ giúp em.

---

## 6. ĐIỀU EM CHƯA CHỨNG MINH ĐƯỢC

| Điều | Vì sao |
|---|---|
| Sáu đơn này có đúng là **dữ liệu thử** không | Bảng khách hàng trên máy vận hành **không có cột tên** như em đoán, nên em chưa đọc được tên khách. Em **không đoán bừa**. Anh mở màn Đơn Hàng nhìn là biết ngay |
| Bốn dòng lệnh sản xuất kia có phải việc thật không | Cần người biết xưởng xác nhận — em chỉ đọc được số, không biết ngoài xưởng có in thật hay không |

---

*Công khai-an toàn: chỉ nêu mã đơn và mã báo giá (là mã kỹ thuật), không nêu tên khách, không nêu số điện thoại, không nêu địa chỉ máy chủ.*
