# NẠP 3 DANH MỤC NỀN LÊN HỆ THỐNG VẬN HÀNH + KHỞI TẠO TÀI KHOẢN

**Ngày:** 22/08/2026 · **Loại:** đợt **DỮ LIỆU** (không phát hành phiên bản mới; hệ thống vẫn V1.00.351)
**Mã ghi nhận (commit):** `<mã-nguồn-riêng>` · đợt nạp dữ liệu: `<mã-nguồn-riêng>`

> Bản tin public-safe: chỉ nêu **SỐ LƯỢNG** và định danh kỹ thuật. Không có tên người, số điện thoại,
> địa chỉ, email, thông tin đăng nhập hay giá trị tiền.

---

## 1. Ba danh mục nền đã vào hệ thống vận hành

| Danh mục | Vào | Ra | Bỏ qua | Lỗi |
|---|---|---|---|---|
| Nhân viên | 16 | **16** | 0 | **0%** |
| Khách hàng | gần như toàn bộ | **gần như toàn bộ** | 0 | **0%** |
| Nhà cung cấp | 109 | **109** | 0 | **0%** |

**Số dư sau khi nạp:** nhân viên tổng **46** (16 hồ sơ thật = 14 đang làm + 2 đã nghỉ · 30 hồ sơ nháp cũ
đã chuyển lưu trữ) · khách hàng tổng **hàng nghìn** · liên hệ khách hàng **hàng nghìn** · địa chỉ khách hàng **hàng nghìn** ·
nhà cung cấp tổng **110** kèm **109** địa chỉ + **109** liên hệ.

**Kiểm chứng sau nạp:** tỷ lệ nhận diện tỉnh/thành **98,16%** · không có khách hàng nào thuộc người đã nghỉ ·
không có hồ sơ "nghỉ việc mà thiếu ngày nghỉ" · không có liên hệ trùng · không có khoá ngoại mồ côi.

---

## 2. Một lỗi nghiêm trọng đã chặn được TRƯỚC khi chạm hệ thống vận hành

Dữ liệu trung gian giữ **số hiệu phòng ban / vị trí của máy phát triển**. Diễn tập trên **bản sao** của hệ
thống vận hành cho thấy nếu nạp thẳng:

| | Hậu quả |
|---|---|
| Khoá ngoại | **5/16** hồ sơ lỗi, không nạp được |
| Phòng ban | **1 hồ sơ bị xếp SAI phòng — không có lỗi nào báo** |
| Vị trí | **15/16 hồ sơ bị gán SAI vị trí — không có lỗi nào báo** |

Nguyên nhân sâu hơn: **danh mục nền trên hệ thống vận hành vẫn là dữ liệu tạm** (danh sách vị trí là
"Vị trí 1 … 20" đánh số máy móc), trong khi danh mục thật chỉ tồn tại ở máy phát triển.

**Cách xử lý (Chủ sở hữu chọn):** đồng bộ danh mục nền trước, rồi nạp nhân viên **giải khoá ngoại theo TÊN**
thay vì theo số hiệu, và **dừng-báo-lỗi nếu không tra được** — tuyệt đối không rơi về số hiệu thô.

Một chi tiết đáng ghi: ban đầu dự định khớp theo **mã**, nhưng đo ra thấy mã được sinh **theo số thứ tự
sẵn có của từng máy** — cùng một phòng nhưng hai máy hai mã khác nhau. Khớp theo mã sẽ **tạo ra 7 phòng
trùng tên**, đúng loại hỏng mà việc đồng bộ định tránh. Nên khoá khớp đổi sang **tên đã chuẩn hoá**;
riêng vị trí khoá theo (tên vị trí + tên phòng ban) vì tên vị trí lặp giữa các phòng.

**Kết quả sau khi sửa:** đối chiếu **16/16** hồ sơ giữa hai máy — **khớp tuyệt đối** cả phòng ban lẫn vị trí.

---

## 3. Dry-run từng nói dối — đã sửa

Bảng "chạy thử" trình cho Chủ sở hữu duyệt báo **hàng nghìn** liên hệ, trong khi đường ghi thật chỉ tạo **1.893**
(vì đường ghi thật có bước gộp trùng mà nhánh chạy thử bỏ qua). Dry-run chính là **cổng duyệt**, nên sai số
ở đó làm mất luôn giá trị của cổng. Đã tách phần gộp trùng thành một hàm dùng chung cho cả hai nhánh.

---

## 4. Gán người phụ trách và khởi tạo tài khoản

- **Người phụ trách khách hàng:** nguồn dữ liệu cũ **không có** trường này, nên toàn bộ khách nhập về đều
  trống. Theo quyết định của Chủ sở hữu, **gần như toàn bộ** khách được gán tạm về một hồ sơ để không có khách "vô chủ";
  đây là **gán tạm**, sẽ phân bổ lại cho đội kinh doanh sau. Có sao lưu bảng trước khi gán; sau khi gán:
  **0 khách còn trống · 0 khoá ngoại mồ côi · tổng số khách không đổi**.
- **Nhà cung cấp:** bảng nhà cung cấp **không có** cột người phụ trách. Theo thiết kế đã khoá, **không tự
  thêm cột** — ghi nhận thành một mục cần Chủ sở hữu quyết.
- **Tài khoản đăng nhập:** đã khởi tạo và **liên kết 9 hồ sơ với tài khoản**, mỗi tài khoản đúng một hồ sơ,
  **không tài khoản nào bị hai hồ sơ dùng chung**, **không tài khoản mồ côi**. Việc liên kết dùng **đúng
  đường ghi của ứng dụng** để chính ứng dụng canh luật, thay vì lệnh cơ sở dữ liệu thô.
- **7 hồ sơ chưa có tài khoản**: 4 hồ sơ **không có email** trong nguồn, 3 hồ sơ thuộc các **cặp dùng chung
  email** (mỗi email chỉ cấp được một tài khoản). Cần cấp email riêng thì mới đăng nhập được.

Mật khẩu tạm được ghi ra **tệp cục bộ nằm ngoài kho mã**, giao riêng cho Chủ sở hữu. **Không xuất hiện**
trong bản tin này, trong nhật ký, hay trong kho mã.

**Bằng chứng nhìn thấy được:** mở ô chọn "người phụ trách" trên hệ thống vận hành — hiện **8 người đang làm
việc**; một hồ sơ **đã nghỉ việc bị lọc ra đúng** như bản vá V1.00.351 thiết kế.

---

## 5. Kiểm thử lại sau tất cả thay đổi

**12 bộ kiểm thử · 548 khẳng định · xanh toàn bộ** · kiểm kiểu 0 lỗi · dựng bản phát hành 0 lỗi ·
**mã nguồn ứng dụng không sửa dòng nào** trong đợt này.

---

## 6. Việc còn lại, đã ghi sổ

| Nội dung | Trạng thái |
|---|---|
| Đổi mật khẩu tài khoản quản trị | Ứng dụng **đã có** màn đổi mật khẩu → Chủ sở hữu tự đổi. Chưa ghi hoàn tất vì việc đổi chưa xảy ra |
| Bảng phân vai trò | **Lệch** giữa con số nêu ra và dữ liệu đang chạy (tổng số lượt gán thì khớp). Chủ sở hữu sẽ chỉ đích danh; hệ thống **giữ nguyên**, không tự sửa |
| 7 hồ sơ chưa có tài khoản | Chờ cấp email riêng |
| gần như toàn bộ khách gán tạm một người | Chờ phân bổ cho đội kinh doanh |
| Nhà cung cấp chưa có cột phụ trách | Chờ quyết định có mở đợt đổi lược đồ hay không |
| Kiểm kiểu chưa phủ thư mục kịch bản | Cổng mới chỉ bắt lỗi cú pháp, chưa bắt lỗi kiểu |
