# KHẢO SÁT TRƯỚC KHI ĐƯA CHUỖI BÁN HÀNG VÀO VẬN HÀNH THẬT

**Ngày:** 23/08/2026 · **Loại:** **KHẢO SÁT** — không sửa một dòng mã nào, không đổi một dòng dữ liệu nào
**Mã ghi nhận (commit):** `<mã-nguồn-riêng>` · Hệ thống vận hành giữ nguyên **V1.00.352**

> Bản tin public-safe: chỉ nêu số lượng, tên màn hình và tên vai trò. Không có tên người,
> tên khách hàng, thông tin đăng nhập hay giá trị nhạy cảm.

---

## 1. Kết luận một dòng

> **Chuỗi Khách hàng → Báo giá → Đơn hàng chạy được — nhưng hiện chỉ tài khoản quản trị dùng được.**
> Ba vai trò vận hành (kinh doanh · điều hành · kế toán) **không thấy menu bán hàng** và
> **không thấy khách hàng nào**. Ngoài ra có một lỗ hổng cho phép **đi vòng qua cổng duyệt giá**
> và hai đường **lộ giá vốn**.

---

## 2. Cách khảo sát

Sáu tác nhân đọc mã chạy song song (chỉ đọc), cộng với đo trực tiếp cơ sở dữ liệu vận hành
(chỉ đọc) và chụp màn hình thật. Mọi khẳng định trong báo cáo nội bộ đều kèm **đường dẫn tệp
và số dòng** — không có suy đoán.

---

## 3. Ma trận phân quyền đo thật

| Bảng phân quyền | Số dòng trên hệ thống vận hành |
|---|---|
| Quyền theo **menu** | 24 |
| Quyền theo **hành động** | **3** — và cả ba đều thuộc nhân sự |
| Quyền theo **phạm vi dữ liệu** | **0** |
| Quyền theo **trường dữ liệu** | **0** |

**Hệ quả đo được:**

- Menu **CRM & Bán hàng**: chỉ vai trò **quản trị** xem được. Vai trò **kinh doanh không có một dòng
  phân quyền nào**.
- Quyền **duyệt giá** không cấp được cho ai — chỉ quản trị đi vòng qua cơ chế miễn trừ. Điều này
  **trái với quyết định đã khóa** của Chủ sở hữu (“người lập khác người duyệt; quản trị/điều hành duyệt”).
- **Ba tài khoản kinh doanh và một tài khoản kế toán thấy 0 khách hàng.** Nguyên nhân: hệ thống
  chặn-mặc-định, người không phải quản trị chỉ thấy khách **do chính mình phụ trách**, mà toàn bộ
  **gần như toàn bộ khách đang gán tạm cho một người**.

*Giảm nhẹ đã có sẵn:* nhân viên kinh doanh tạo khách **mới** thì hệ thống **tự gán về chính họ**.

---

## 4. Bảng kê điểm chặn (vòng đầu — 15 điểm)

| Nhóm | Số điểm | Ví dụ tiêu biểu |
|---|---|---|
| **Phân quyền** | 4 | Vai trò kinh doanh không có menu · quyền duyệt giá không cấp được · **chưa có giao diện tick quyền hành động** · nhân viên kinh doanh không thấy khách hàng |
| **Lỗ hổng nghiệp vụ / bảo mật** | 4 | **Đi vòng qua cổng duyệt giá** · **lộ giá vốn** ở ba đường · duyệt mà **không kiểm đơn giá > 0** · **nuốt lỗi** (lưu thất bại vẫn báo thành công) |
| **Đơn hàng chưa đủ dùng** | 4 | Ngày giao **bị đặt bằng ngày tạo**, không sửa được · **địa chỉ giao không bao giờ được ghi** · **đơn nháp không sửa/xoá được** · **số tiền công nợ tính sai** (gồm cả đơn nháp) |
| **Cổng quyền còn hở** | 2 | **13/14 tuyến giao diện lập trình của phần tính giá không có cổng quyền nào** · một chỗ **gắn cứng vai trò trong mã** thay vì tick qua ma trận |
| **Vòng đời báo giá** | 1 | Thao tác “huỷ” là **một chiều** và **không có nút xoá** ⇒ bản ghi hỏng nằm lại vĩnh viễn |

Ngoài ra: **13 điểm “nên có”** (số đếm trên nhãn lọc sai, ngày hiển thị sai chuẩn, khoá-sau-duyệt
chỉ phủ 3/10 đường ghi, danh sách báo giá không lọc theo phạm vi, một nút không nối hàm nhưng vẫn
báo thành công…) và **4 điểm “sau cũng được”**.

---

## 4b. BỔ SUNG SAU VÒNG RÀ SÓT — tổng điểm chặn **15 → 27**

Hai tác nhân rà soát độc lập chạy sau vòng đọc mã (một đóng vai người soát khắt khe, một đóng vai
nhân viên kinh doanh mới vào làm) tìm thêm **24 phát hiện**, nhiều điểm **nặng hơn** vòng đầu:

| Nhóm | Phát hiện bổ sung |
|---|---|
| **Phá dữ liệu chạy ngầm** | Mở trang báo giá là hệ thống **tự động chuyển sang “hết hạn”** mọi báo giá quá 15 ngày **của cả công ty** — chạy ngay khi mở trang và lặp mỗi 60 phút, **không hỏi, không báo**, và “hết hạn” là trạng thái **không quay lại được**. Một nhân viên mở trang là báo giá của đồng nghiệp chết hàng loạt |
| **Mất dữ liệu khi lưu** | Người **không được xem giá vốn** mở báo giá ra rồi bấm Lưu ⇒ **giá vốn bị xoá về 0** trên cơ sở dữ liệu, không cảnh báo. Nguyên nhân: hệ thống che giá trị khi đọc, nhưng khi lưu lại ghi đúng giá trị đã che |
| **Tài liệu gửi khách** | Bản in báo giá in **mã nội bộ tự bịa** thay cho tên khách; **thiếu** thông tin bên bán, địa chỉ khách, dòng thuế, điều kiện thanh toán, chỗ ký; ngược lại **in cả nhãn trạng thái nội bộ**, và **in được cả bản nháp / bản bị từ chối** |
| **Con số không nhất quán** | Thuế 10% chỉ là **con số bịa trên màn hình** — cơ sở dữ liệu **không có cột thuế**, bản in **không có dòng thuế** ⇒ **ba con số khác nhau cho cùng một báo giá**. Không có bất kỳ cơ chế chiết khấu nào |
| **Quy trình hai người không chạy** | **Không có nút “Gửi duyệt”** (hàm viết đủ nhưng không nối vào nút nào); **không ai được thông báo** khi cần duyệt hay khi đã duyệt; màn hình **không tự cập nhật** khi người khác duyệt xong |
| **Người dùng bị kẹt** | Nhân viên kinh doanh bấm “Duyệt” thì **nhận lỗi kỹ thuật tiếng Anh** mà hộp tạo đơn **vẫn bật lên**; nút “Huỷ” **một cú bấm là chết vĩnh viễn, không hỏi lại**; danh sách hiện tên khách của đồng nghiệp thành **dãy số vô nghĩa kèm số tiền hợp đồng của họ** |
| **Không truy được trách nhiệm** | **Không có dấu vết kiểm toán** cho chính cổng duyệt giá — không có bảng lịch sử trạng thái, không có lý do huỷ, dấu người sửa bị lần sửa sau **ghi đè** |
| **Tiền đi đường khác** | Nút “Đánh dấu giao hàng” **chỉ đổi một chữ** — không trừ kho, không sinh phiếu. Và mối nối đơn hàng ↔ phiếu giao hàng là **chuỗi gõ tay không ràng buộc** (ô số đơn hàng là ô nhập tự do, gợi ý còn **sai định dạng**). **Đây là lời giải thật** cho việc số tiền đã thanh toán trên đơn không bao giờ đổi: tiền chưa bao giờ chảy qua đơn hàng, nó chảy qua phiếu giao hàng |

**Ba nguyên nhân gốc:**
1. **Ma trận phân quyền chưa được cấu hình** ⇒ chỉ quản trị dùng được.
2. **Nhiều cổng và thao tác được viết ra nhưng không nối vào đâu** ⇒ đọc mã tưởng có, chạy thật thì không.
3. **Số tiền và định danh không đi qua một đường duy nhất** ⇒ mỗi nơi một con số.

---

## 5. Hai điểm tự đính chính

**(a)** Phiên trước báo *“không có chỗ nào gắn cứng quyền theo chức danh”*. Điều đó **đúng với chức
danh**, nhưng lần quét đó **bỏ sót** một chỗ **gắn cứng theo mã vai trò** — vẫn là thứ không tick được
qua ma trận, tức vẫn trái nguyên tắc Chủ sở hữu đã khóa.

**(b)** Phiên trước **đóng khoản nợ “đổi mật khẩu quản trị”** dựa trên lời trong yêu cầu, **không đo lại**.
Lần này đo cơ sở dữ liệu: mật khẩu **chưa hề được đổi**. Khoản nợ đã **mở lại**.
Bài học ghi vào sổ: **không đóng nợ bảo mật bằng lời khai — phải đo**.

---

## 6. Ba tin tốt

1. **Có ô nhập giá bằng tay** ngay trên màn báo giá ⇒ việc màn tính giá đang tạm khoá **không chặn**
   đưa vào vận hành. Người lập báo giá tự quyết giá và gõ vào.
2. **Điểm chặn lớn nhất gỡ được mà không cần viết mã**: màn quản trị bảo mật **đã có sẵn ma trận tick**
   và **mẫu quyền cho vai trò kinh doanh** — chỉ cần áp mẫu.
3. Nút “Tạo Đơn Hàng” **không vi phạm** quyết định đã khóa: nó chỉ nhận báo giá **đã duyệt**; form nhập
   tay giả đã được gỡ đúng như chỉ đạo trước đó.

---

## 7. Đề xuất nghiệm thu “100% vận hành”

12 mục, mỗi mục kèm cách chứng minh bằng **ảnh màn hình thật** và **truy vấn dữ liệu**: nhân viên thật
đăng nhập thấy menu → thấy khách → tạo khách mới → lập báo giá → **bị chặn khi tự duyệt** → người có
quyền duyệt được → **không thể đi vòng qua cổng duyệt** → sinh đơn có ngày giao và địa chỉ giao thật →
đơn nháp sửa được, đơn đã xác nhận bất biến → **giá vốn bị che** → **số tiền hiển thị đúng** → đổi tên
chức danh không ảnh hưởng quyền.

**Chốt tiêu chí trước, thi công sau.**

---

## 8. Đề xuất bốn đợt thi công (chỉ đề xuất, chưa chạy)

| Đợt | Nội dung | Cần viết mã? |
|---|---|---|
| **A** | Tick ma trận quyền cho ba vai trò vận hành · cấp quyền duyệt giá · quyết cách cho kinh doanh thấy khách · đổi mật khẩu quản trị | **Không** |
| **B** | Vá 5 lỗ hổng chặn (đi vòng cổng duyệt · lộ giá vốn · duyệt không kiểm giá · nuốt lỗi · gắn cứng vai trò) | Có, kèm kiểm thử |
| **C** | Hoàn thiện đơn hàng (ngày giao · địa chỉ giao · sửa/xoá đơn nháp · tiền và công nợ tính đúng) | Có, kèm kiểm thử |
| **D** | Bổ sung **ma trận tick quyền hành động** vào màn quản trị bảo mật | Có |

Ba mục để lại sau khi vận hành ổn định (trang người dùng tự quản lý · chuông thông báo · chế độ
sáng-tối) **giữ nguyên không động**, đúng chỉ đạo dồn lực đưa hệ thống vào vận hành sớm.
