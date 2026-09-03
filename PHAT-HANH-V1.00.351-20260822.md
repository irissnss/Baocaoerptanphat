# PHÁT HÀNH V1.00.351 — ẨN HỒ SƠ ĐÃ NGHỈ VIỆC KHỎI DANH SÁCH LÀM VIỆC

**Ngày:** 22/08/2026 · **Loại:** phát hành production, **chỉ mã nguồn — KHÔNG đổi cấu trúc dữ liệu**
**Mã phát hành (commit):** `<mã-nguồn-riêng>`
**Bản trước:** V1.00.350 — `<mã-nguồn-riêng>`

> Bản tin public-safe: chỉ nêu **định danh kỹ thuật và SỐ LƯỢNG**. Không có dữ liệu cá nhân,
> không giá trị tiền, không thông tin đăng nhập, không cấu hình máy chủ.

---

## 1. Việc gì được sửa

Danh sách nhân viên hiển thị **cả hồ sơ đã nghỉ việc**, và các ô chọn "Sale phụ trách"
cũng liệt kê những hồ sơ đó ⇒ người dùng có thể chọn nhầm người không còn làm.

**Sửa 3 lớp, gốc nằm ở tầng đọc dữ liệu:**

| Lớp | Thay đổi |
|---|---|
| Tầng đọc | `listNhanVien()` **mặc định loại** hồ sơ trạng thái nghỉ việc; muốn lấy đủ phải khai rõ tham số |
| Trang quản trị nhân sự | khai rõ tham số để vẫn xem được hồ sơ nghỉ việc trong tab riêng |
| Giao diện | bộ lọc mặc định **"Đang Làm Việc"**, thêm tuỳ chọn **"Đã Nghỉ Việc"**, nút đặt lại quay về mặc định |

Vì sửa ở tầng đọc nên **mọi nơi chọn nhân viên** (3 mô-đun) tự động sạch theo, không phải sửa từng chỗ.

**Kèm trong đợt:** giao dịch cơ sở dữ liệu ghim đúng **một kết nối**; ô chọn "Sale phụ trách"
trả **đúng họ định danh nhân viên** (bản cũ trả họ định danh tài khoản — hai dải số không giao nhau
nên mọi lượt chọn đều gán sai người phụ trách).

---

## 2. Một phát hiện đáng nói

Bản vá này **ban đầu không có bộ kiểm thử nào bảo vệ**: cố ý cắm lỗi vào hàm đọc dữ liệu mà
**cả 12 bộ kiểm thử vẫn báo xanh**. Đã viết bộ kiểm thử riêng (7 khẳng định, tự tạo dữ liệu
rồi tự dọn), và **kiểm ngược**: cắm lỗi → 3 khẳng định đỏ; trả mã → xanh lại.

Cùng đợt, **6 bộ kiểm thử cũ** bị phát hiện đang **mượn dữ liệu nền có sẵn** thay vì tự dựng
dữ liệu mẫu — nghĩa là chúng xanh/đỏ theo trạng thái cơ sở dữ liệu chứ không theo chất lượng mã.
Đã sửa cả 6 để tự tạo và tự dọn.

**Kết quả:** 12 bộ · **548 khẳng định** · xanh toàn bộ · **chạy 2 lượt liên tiếp cho kết quả giống hệt**
· mốc dữ liệu nền không đổi · không sót dữ liệu mẫu.

---

## 3. Hai lỗi CHẶN TRIỂN KHAI phát hiện ngay trong lúc triển khai

| # | Lỗi | Hệ quả thật | Xử lý |
|---|---|---|---|
| 1 | Một tệp kịch bản di trú có **chuỗi ký tự bị xuống dòng thật** (công cụ vá nuốt ký tự thoát khi gỡ thông tin nhạy cảm, đợt 20/08) | Trình biên dịch kiểm kiểu **không phủ thư mục kịch bản** ⇒ lỗi nằm im 2 ngày rồi **làm đứng chuỗi triển khai production** | Vá chuỗi + **dựng cổng tự động mới** |
| 2 | Cùng tệp đó kiểm biến môi trường mật khẩu khởi tạo **ngay khi vào hàm**, trong khi biến này chỉ cần cho lần dựng đầu | **Mọi lần triển khai production đều bị chặn** | Đổi vị trí kiểm xuống đúng nhánh cần; **thôi in giá trị ra nhật ký** |

**Cổng tự động mới:** phân tích cú pháp **thật** toàn bộ **770 tệp mã** của kho bằng chính bộ dịch
mà hệ thống dùng — đọc **đầu ra thật** của kho, không chạy trên chuỗi mẫu viết cứng.
Đã nối vào cổng trước khi ghi nhận thay đổi và chuỗi cổng quản trị.
**Kiểm ngược:** cắm lại lỗi → cổng báo hỏng đúng tệp đúng dòng; trả mã → 770/770 đạt.
Cổng còn bắt thêm **một tệp kịch bản cũ khác** cũng lỗi cú pháp, tồn từ lâu không ai biết.

---

## 4. Diễn tập trước khi triển khai

Dựng **bản sao mới** từ bản trích xuất cơ sở dữ liệu production lấy **ngay tại thời điểm chạy**:

| Hạng mục | Kết quả |
|---|---|
| Số bảng sau khi chạy ứng dụng | **99 → 99** ⇒ **không có di trú cấu trúc** |
| Đăng nhập | đạt |
| Vòng nghiệp vụ báo giá → duyệt → sinh đơn (dữ liệu giả) | **9/9**, gồm: chưa duyệt thì sửa được giá · **duyệt xong thì chặn sửa giá** · sinh đơn được · hạ về nháp thì **chặn sinh đơn** |
| Tự dọn sau diễn tập | mốc **trở về nguyên vẹn ở 8 bảng**, kể cả **công việc sinh kèm đơn hàng** — 0 dòng sót |
| Màn có ô chọn "Sale phụ trách" **khi rỗng** | 2 màn đều mở bình thường, **0 lỗi trình duyệt** |

Ghi chú: ô chọn trên production **đang thật sự rỗng** (chưa hồ sơ nhân viên nào gắn tài khoản),
nên đây là kiểm thử trên điều kiện thật chứ không phải tình huống giả định.

---

## 5. Triển khai và kiểm chứng trên production

- Sao lưu cơ sở dữ liệu **mới, ngay trước khi triển khai** (không dùng bản sao lưu cũ).
- **Kế hoạch lùi viết sẵn trước khi triển khai**, có ghi rõ điểm lùi và điều kiện kích hoạt.

| Kiểm chứng | Kết quả |
|---|---|
| Số hiệu bản chạy | **V1.00.351** (hiển thị ở chân trang) |
| Tiến trình ứng dụng | trực tuyến, không khởi động lại ngoài ý muốn |
| **Vân tay cấu trúc dữ liệu trước / sau** | **giống hệt** — 99 bảng · 1526 cột ⇒ **0 di trú** |
| Số lượng dữ liệu nghiệp vụ | **không đổi** ở 4 bảng đối chứng |
| Đăng nhập | đạt |
| Màn danh sách nhân viên | đạt · lọc mặc định "Đang Làm Việc" · có tuỳ chọn "Đã Nghỉ Việc" · 0 lỗi trình duyệt |
| Màn có ô chọn "Sale phụ trách" (rỗng) | đạt, 0 lỗi |
| **Mở một báo giá THẬT** | đạt — hồ sơ đã duyệt hiển thị đúng trạng thái, **nút Duyệt và Sửa bị khoá**, chỉ còn Tạo Đơn / Sao Chép / In |

---

## 6. Nói thẳng một điểm

Bản vá này **chỉ ẩn hồ sơ ĐÃ NGHỈ VIỆC**. Trên production hiện **chưa hồ sơ nào ở trạng thái nghỉ việc**,
nên **ngay sau khi phát hành, danh sách vẫn còn các hồ sơ nháp** — đúng thiết kế, **không phải lỗi**.
Chúng chỉ biến mất ở **đợt nạp dữ liệu tiếp theo**, khi kịch bản lưu trữ đánh dấu các hồ sơ nháp
là nghỉ việc và nạp danh sách nhân viên thật. Điều kiện nghiệm thu "danh sách chỉ còn người thật"
sẽ được **đo lại sau đợt đó**, không tuyên bố sớm.

---

## 7. Việc còn nợ, đã ghi sổ

| Mã nợ | Nội dung | Hạn |
|---|---|---|
| 051 | Một giá trị nhạy cảm bị hiển thị ra phiên làm việc khi đọc sổ nội bộ bằng lệnh chưa che giá trị. **Không lộ ra kho mã** (tệp vẫn bị loại khỏi theo dõi), nhưng **cần đổi khoá** | 25/08/2026 |
| 052 | Trình kiểm kiểu không phủ thư mục kịch bản — cổng mới chỉ bắt lỗi cú pháp, chưa bắt lỗi kiểu | — |
| 053 | Cấu trúc dữ liệu production còn thiếu **2 bảng + 9 cột** so với máy phát triển; **bắt buộc bổ sung** trước đợt nạp dữ liệu tiếp theo | 22/08/2026 |
| 054 | Còn nhiều kịch bản dùng-một-lần trong kho chưa rà, một cái đã phát hiện lỗi cú pháp tồn lâu | — |

Ngoài ra, hai gói thay đổi cũ thuộc **vùng tính giá** được Owner chốt **giữ nguyên, không đụng trong đợt này**;
và việc rà soát dữ liệu người phụ trách cũ **chỉ trình, không tự sửa**, thực hiện **sau** đợt nạp dữ liệu.
