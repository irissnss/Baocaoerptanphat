# ✅ ĐÃ PHÁT HÀNH V1.00.355 — TÁCH BIỂU MẪU KHỎI NHÓM HỆ THỐNG

**Ngày:** 23/08/2026 · **Loại:** PHÁT HÀNH LÊN HỆ THỐNG VẬN HÀNH
**Hệ thống vận hành hiện chạy:** **`V1.00.355`** · commit **`<mã-nguồn-riêng>`** · nhánh `main`
**Trước đợt này:** `V1.00.354` · commit `<mã-nguồn-riêng>`

> Bản tin public-safe: chỉ nêu số lượng, mã kỹ thuật, tên bảng/màn hình. Không có thông tin đăng nhập,
> không có dữ liệu khách hàng, không có số tiền thật.

---

## 1. VIỆC NÀY GIẢI QUYẾT ĐIỀU GÌ

Đợt trước (V1.00.354) đã cho phép tick quyền *"sửa biểu mẫu"* cho tổng giám đốc.
**Nhưng đo lại thì hoá ra chưa dùng được:** màn quản lý biểu mẫu vẫn nằm trong nhóm **Hệ Thống** —
nhóm chứa phân quyền, tài khoản, cấu hình. Tổng giám đốc **có tick nhưng vẫn bị chặn ngay ở cửa màn hình**.

Nghĩa là muốn cho ai quản lý mẫu in thì vẫn phải mở cả nhóm bảo mật. Đúng thứ Chủ dự án cấm.

**Đợt này tách hẳn ra.**

---

## 2. ĐÃ LÀM

| Việc | Kết quả |
|---|---|
| **Địa chỉ mới độc lập** | Màn biểu mẫu nay là **mục riêng ngang hàng** trên thanh điều hướng, **không còn nằm trong nhóm Hệ Thống** |
| **Khoá quyền riêng, bất biến** | Quyền gắn bằng **mã khoá cố định**, không gắn bằng nhãn hiển thị. Đổi tên vai trò hay đổi nhãn menu **không ảnh hưởng quyền** |
| **Địa chỉ cũ vẫn dùng được** | Ai đã lưu dấu trang địa chỉ cũ thì **tự động chuyển** sang địa chỉ mới. Đã kiểm trên hệ thống vận hành: **chuyển đúng cả hai địa chỉ** |
| **Khoá đôi giữ nguyên** | Muốn sửa mẫu phải qua **cả hai** cổng: quyền vào màn Biểu Mẫu **và** ô tick *"sửa biểu mẫu"*. Xem được **khác** sửa được |
| **Thu hồi quyền Hệ Thống** | Nhóm Hệ Thống nay **chỉ còn vai trò quản trị**. Danh sách vai trò được giữ **suy từ cờ dữ liệu**, không viết cứng trong mã |
| **Dọn danh mục mẫu** | **10 dòng trùng** chuyển sang không dùng. **Không xoá dòng nào** — vẫn đủ 34 dòng, giữ dấu vết |

### Kết quả đo trên hệ thống vận hành

| Chỉ số | Kết quả |
|---|---|
| Vai trò xem được nhóm **Hệ Thống** | **chỉ vai trò quản trị** |
| Vai trò vào được **Biểu Mẫu** | quản trị + tổng giám đốc |
| Tổng giám đốc: có nhóm Hệ Thống? | **KHÔNG** |
| Tổng giám đốc: vào và sửa được Biểu Mẫu? | **CÓ** ← điều Chủ dự án yêu cầu |
| Loại chứng từ còn **nhiều hơn 1 mẫu đang dùng** | **0** (trước: báo giá 4 · đơn hàng 4 · phiếu thu 3 · phiếu chi 3) |
| Số dòng danh mục mẫu | **34** — giữ nguyên, không xoá cứng |
| Số bảng dữ liệu | **101** trước = **101** sau — không đổi cấu trúc |

---

## 3. HAI LỖI ĐIỀU HƯỚNG — CHỈ LỘ RA KHI XEM ẢNH

Sau khi phát hành lần đầu, mở **ảnh chụp màn hình thật** ra xem thì phát hiện hai lỗi mà **đọc mã không thấy**:

**1. Mục Biểu Mẫu vẫn nằm bên trong nhóm Hệ Thống.** Quyền đã tách đúng, nhưng vị trí trên thanh
điều hướng thì chưa — nên vẫn trông như một mục con của Hệ Thống. Đã chuyển ra **ngang hàng**.

**2. Nhóm Hệ Thống LUÔN hiện với mọi người dùng, bất kể quyền.** Trong mã có một dòng cho qua vô điều kiện.
Các trang bên trong **vẫn được chặn ở máy chủ nên không lộ dữ liệu**, nhưng người dùng **thấy lối vào rồi
bấm vào mới bị chặn** — trái chuẩn giao diện của dự án (*"ẩn theo quyền, không phải cho thấy rồi báo lỗi"*).
**Đã gỡ.** Nhóm Hệ Thống nay theo đúng ma trận như mọi menu khác.

Kèm một lỗi thứ ba cùng chỗ: quy tắc suy khoá quyền từ tên địa chỉ **cắt nhầm** tên địa chỉ mới, khiến mục
này **biến mất với mọi người không phải quản trị — kể cả người đã được cấp quyền**. Đã khai báo tường minh.

> 📌 Đây là **lần thứ tư trong ngày** việc **xem ảnh bằng mắt** bắt được lỗi mà rà mã bỏ qua.

---

## 4. ⚠️ MỘT VIỆC CHỦ DỰ ÁN CẦN XỬ LÝ

Vai trò **"Nhân viên"** trước đây **chỉ được cấp đúng một quyền: nhóm Hệ Thống** — không có quyền nào khác.
Sau khi thu hồi nhóm Hệ Thống theo yêu cầu, vai trò này còn **0 menu**.

| Số tài khoản mang vai trò này | Ảnh hưởng |
|---|---|
| 1 tài khoản | **Không ảnh hưởng** — còn mang thêm vai trò Kinh doanh |
| **2 tài khoản** | **Mất lối vào** — chỉ có mỗi vai trò này |

**Cách khắc phục:** vào màn phân quyền, **tick những menu mà nhân viên cần dùng**.
Đây là **thao tác dữ liệu** — **không cần sửa mã, không cần phát hành lại**, có hiệu lực ngay khi tick.

---

## 5. KIỂM THỬ

| Hạng mục | Kết quả |
|---|---|
| Bộ kiểm thử | **17 bộ · 697 điểm kiểm · 0 trượt** |
| Bộ mới *"tách biểu mẫu khỏi hệ thống"* | **25/25** |
| Bộ ma trận quyền hành động | 48 → **58** điểm kiểm |
| Kiểm kiểu dữ liệu · dựng bản phát hành | **0 lỗi** · thành công |
| **Kiểm khói sau phát hành** | **15/15 đạt · 0 lỗi máy chủ** |
| Địa chỉ cũ chuyển hướng đúng | ✅ cả hai địa chỉ |

**Chứng minh bộ kiểm thử có tác dụng thật:** các điểm kiểm mới khoá lại đúng hai lỗi điều hướng nêu trên —
trả mã về trạng thái cũ thì chúng **báo đỏ ngay**.

---

## 6. 🛑 PHẦN CÒN LẠI CỦA GIAI ĐOẠN 1 — DỪNG, CHỜ CHỦ DỰ ÁN

Yêu cầu còn lại là **làm mẫu Đơn Hàng** và **quy trình xuất PDF gửi khách**, dựa trên
*"biểu mẫu Chủ dự án đã đặt trong thư mục gốc"*.

**Đã tìm kỹ, và file đó KHÔNG có trong máy:**

| Cách tìm | Kết quả |
|---|---|
| Theo **tên file** ở thư mục gốc | **0** |
| Theo **nội dung** *"ĐƠN ĐẶT HÀNG"* trong toàn bộ kho | **0** (ngoài chính báo cáo của phía kỹ thuật) |
| Trong các thư mục cạnh kho mã | **0 tệp** văn bản/bảng tính/PDF nào |
| Tệp mẫu được nhắc tên trong yêu cầu | **0** |
| File tạo hoặc sửa sau 17:00 cùng ngày | **0** |

**Bằng chứng đối chứng:** thư mục tài nguyên công khai có sẵn **3 mẫu in** (báo giá · lệnh sản xuất ·
phiếu điều in) nhưng **không có mẫu đơn hàng** — khớp đúng ghi chú đã có sẵn trong mã nguồn từ trước:
*"chưa có mẫu Đơn Hàng nghiệp vụ được Chủ dự án phê duyệt"*.

**Phía kỹ thuật KHÔNG tự bịa mẫu chứng từ** — yêu cầu của Chủ dự án cấm tự phát minh điều khoản, thuế,
chữ ký hay thông tin công ty. Một mẫu đơn hàng bịa ra sẽ đi thẳng tới khách hàng.

**Cần Chủ dự án:** gửi lại file mẫu (đặt vào thư mục gốc hoặc gửi trực tiếp). Có file là làm tiếp được ngay —
phần khung đã sẵn sàng: danh mục mẫu, khoá phát hành, bảng lịch sử phát hành và bảng tệp đều đã có.

---

## 7. NẾU CẦN QUAY LUI

Không đổi cấu trúc dữ liệu (101 bảng trước = 101 bảng sau) ⇒ quay lui **chỉ cần đưa lại mã nguồn bản cũ**,
**không phải hoàn tác dữ liệu**. Mốc quay về: `<mã-nguồn-riêng>` (V1.00.354).
Riêng phần quyền: hoàn tác bằng **tick lại trong ma trận** — thuần dữ liệu, không cần phát hành.
Đã có bản sao lưu mới lấy ngay trước khi phát hành, kèm mã kiểm tra toàn vẹn.

---

*Báo cáo public-safe. Không chứa mã nguồn, thông tin đăng nhập, dữ liệu khách hàng hay số liệu tài chính thật.*
