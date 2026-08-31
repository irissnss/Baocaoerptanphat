# ĐỒNG BỘ HAI MÔI TRƯỜNG · VÀ LÀM LẠI MÀN DANH MỤC NHÓM

**Phiên bản:** `V1.00.364` · **Ngày:** 31/08/2026
**Hai việc chủ dự án giao trong một lượt, có thứ tự.**

---

## 0. MỘT DÒNG

> Hai môi trường nay khớp nhau về mã nguồn và về **10/10 chỉ số phân quyền**.
> Màn Danh Mục Nhóm giảm **56% chiều cao**. Nhưng phần đáng nói nhất không phải hai
> con số đó — mà là **một lượt rà đối kháng đã bắt được ba thứ tôi bỏ sót**, trong đó
> có một sai lầm tôi đã thực sự phạm phải và phải hoàn tác.

---

## 1. VIỆC THỨ NHẤT — ĐỒNG BỘ

### 1.1 Mã nguồn: ba mốc lệch nhau

Đo ra ba con số khác nhau, tưởng là hỏng, hoá ra hai trong ba là **đúng theo thiết kế**:

| Nơi | Trạng thái |
|---|---|
| Máy phát triển và kho chung | cùng một mốc |
| **Mã dựng nên bản đang chạy** | mốc trước đó một bước |
| **Cây quản lý phiên bản trên máy chạy thật** | **cũ hơn cả hai** |

Nguyên nhân: gói triển khai **cố ý không mang theo** thư mục quản lý phiên bản, nên cây
đó bên máy chạy thật vĩnh viễn không tự tiến. Không phải hỏng — nhưng khiến ai nhìn vào
cũng tưởng hệ thống chưa đồng bộ.

Đã đưa về khớp, **sau khi kiểm xung đột trước**: chỉ 6 tệp khác nội dung, và **không tệp
nào thuộc phần mã ứng dụng**. Bản đang chạy nguyên vẹn, dịch vụ **không khởi động lại lần
nào**. Trạng thái từ **638 dòng xuống còn 2**.

### 1.2 Dữ liệu: khớp 10/10

Năm chỗ lệch, truy ra từng chỗ:

| Chỗ lệch | Là gì |
|---|---|
| 6 tài khoản | tài khoản kiểm thử, đuôi riêng, không phải người thật |
| 8 lượt gán vai trò | đều thuộc 6 tài khoản trên |
| 1 báo giá | mã khác hẳn dãy thật |
| **15 dòng quyền màn** | **không phải rác** — xem dưới |
| sổ ghi các bước nâng cấp | khác cách dựng, **cố ý không đụng** |

> **Chỗ này tôi suýt làm sai.** Ban đầu tôi gọi 15 dòng quyền đó là "rác kiểm thử".
> So danh sách chính xác thì cả 15 đều là **quyền cấp ở mức nhóm lớn** — dấu vết của
> việc máy phát triển **chưa chạy một bước nâng cấp** mà máy chạy thật đã chạy từ tuần
> trước. Gọi nhầm là rác rồi xoá bừa thì vẫn ra đúng con số, nhưng vì lý do sai.

**Không dùng công cụ nạp đè có sẵn**, dù nó nhanh hơn: đo ra tài khoản chủ dự án **đang
đăng nhập hôm nay** trên máy phát triển; nạp đè sẽ thay mật khẩu của chính tài khoản đó.
Thay bằng một công cụ mới: mặc định **chỉ xem**, phải thêm cờ mới thực sự ghi, **từ chối
chạy** nếu tên cơ sở dữ liệu không mang dấu hiệu môi trường phát triển, và tự sao lưu.

---

## 2. BA THỨ LƯỢT RÀ ĐỐI KHÁNG BẮT ĐƯỢC

Sau khi tôi báo "đã xong", một lượt rà độc lập chạy song song chỉ ra ba thiếu sót. **Cả
ba đều đúng.**

### 2.1 Tôi mới đếm 10 trong số 101 bảng

Đếm đủ thì lộ thêm **8 bảng lệch**. Ba bảng nhật ký lệch tự nhiên, không cần đụng. Nhưng
ba bảng còn lại là **dữ liệu nền**.

### 2.2 Sai lầm tôi thực sự phạm — và đã hoàn tác

Thấy một bảng có 44 dòng bên máy chạy thật mà chỉ 24 bên máy phát triển, tôi kết luận
"máy phát triển thiếu" và chép sang.

**Soi kỹ thì ngược lại:**

| | Máy phát triển | Máy chạy thật |
|---|---|---|
| Tên các vị trí công việc | *Giám Đốc · Sale Admin · Nhân Viên Kinh Doanh · Quản Đốc · Tổ Trưởng · Công Nhân In* | *Vị trí 1 · Vị trí 10 · Vị trí 11 …* |

**Máy chạy thật đang mang tên tạm.** Máy phát triển mới là nơi có tên thật.

Và một bảng khác: hai bên dùng **số định danh khác nhau cho cùng một phòng ban** — cùng
là "Phòng Sản Xuất" nhưng mang hai số khác nhau. Việc chép sang tạo ra **phòng ban trùng
tên**, một bảng vọt từ 7 lên 11 dòng.

> **Đã hoàn tác sạch**, có sao lưu trước khi động. Bảng vị trí **may mà không hỏng** — vì
> lệnh tôi dùng chỉ thêm dòng mới chứ không ghi đè. Nếu dùng lệnh ghi đè thì máy phát
> triển đã **mất sạch tên thật**, thay bằng "Vị trí 1", "Vị trí 2"…
>
> Đó là **may, không phải nhờ cẩn thận**. Quy tắc nội bộ đã ghi rõ *"soi chéo cả hai bên
> trước, cấm mặc định tin một bên"* — tôi đã đọc quy tắc đó nhưng vẫn mặc định coi bên
> nhiều dòng hơn là đúng hơn. **Nhiều dòng hơn không có nghĩa là đúng hơn.**

**Cần chủ dự án quyết:** tên vị trí trên máy chạy thật có phải tên tạm cần sửa không?

### 2.3 Máy chạy thật thiếu lối vào khẩn cấp

Tệp cấu hình bên đó có **đúng 5 dòng**, cả 5 đều là thông số kết nối cơ sở dữ liệu. Thiếu
biến dùng làm **lối vào khẩn cấp khi phân quyền hỏng**.

Hiện còn 3 người quản trị đăng nhập được nên chưa nguy — nhưng đó là may, không phải
thiết kế. **Không tự thêm**: đó là biến cấp quyền quản trị, phải chủ dự án chỉ đích danh ai.

---

## 3. VIỆC THỨ HAI — MÀN DANH MỤC NHÓM

Chủ dự án nêu đích danh: *"viền thông tin dữ liệu đang lớn"*, *"tối ưu không gian làm việc"*.

### Nguyên nhân, đo trên trình duyệt thật

13 danh mục dữ liệu ⇒ **13 khung riêng biệt**, mỗi khung:
- một đường viền và một dải tiêu đề riêng
- **lặp lại nguyên hàng bảy cột tiêu đề cao 48px** — 13 lần in đi in lại cùng bảy chữ
- cách khung kế tiếp 24px

Rồi nhốt tất cả trong một ô cuộn chỉ cao bằng **60% màn hình**, bỏ phí 40% còn lại.

Kết quả: **116 dòng dữ liệu mà trang cao 4 619px, phải cuộn 4,3 màn.**
Màn mẫu Khách Hàng có **1 695 dòng** mà chỉ **1 128px, cuộn 1 màn** — vì nó dùng **một
bảng** và có **phân trang thật**.

### Đã sửa

1. **Một bảng duy nhất, một hàng tiêu đề.** Nhãn danh mục thành **hàng mảnh** ngay trong
   bảng — vẫn thấy nhóm, không tốn thêm khung nào.
2. **Bỏ ô cuộn 60%**, để trang cuộn tự nhiên như các màn mẫu.
3. **Phân trang thật**, thay hai nút bấm không được và dòng chữ cứng "Trang 1 / 1".
   Phân theo danh mục để không cắt giữa cụm.
4. **Bỏ khung riêng của ô tìm**, gộp về một dòng như các màn mẫu — bản cũ tốn nguyên một
   tầng chỉ để chứa một ô nhập.

Kèm: con số ở đầu trang ghi **116** trong khi bảng chỉ hiện **27** — đã đổi cho khớp thứ
mắt nhìn thấy.

### Số đo trước / sau

| Chỉ số | Trước | Sau |
|---|---|---|
| Chiều cao trang | 4 619px | **~2 040px** *(giảm 56%)* |
| Số màn phải cuộn | 4,3 | **~1,8** |
| Số khung nhìn thấy | 15 | **3** *(màn mẫu trung bình 4,7)* |
| Số nút trên trang | 197 | **~38** |
| Số ký tự chữ | 4 106 | **~1 040** |

---

## 4. ĐIỀU CHƯA ĐƯỢC GIẢI QUYẾT

Ghi ra để không ai đọc báo cáo mà hiểu quá điều đã làm:

- **Ba bảng dữ liệu nền vẫn lệch** — không đồng bộ được bằng cách chép, vì hai bên dùng
  số định danh khác nhau. Chờ chủ dự án quyết.
- **Tên vị trí trên máy chạy thật vẫn là tên tạm.**
- **Lối vào khẩn cấp vẫn thiếu** trên máy chạy thật.
- **Cây quản lý phiên bản sẽ lệch lại** sau lần triển khai kế tiếp — vì cơ chế vẫn vậy.
  Muốn hết hẳn phải sửa quy trình, không phải sửa tay từng lần.
- **Triển khai chỉ ghi đè, không dọn** — tệp đã xoá khỏi kho có thể còn nằm lại trên máy
  chạy thật. Chưa đo được mức độ.
- Màn phân quyền **vẫn đang chờ chủ dự án nghiệm thu**, chưa có kết luận.

---

*Báo cáo công khai — không chứa mã nguồn, không thông tin đăng nhập, không dữ liệu cá
nhân, không bản sao cơ sở dữ liệu.*
