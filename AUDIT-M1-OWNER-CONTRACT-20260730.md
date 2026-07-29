# 🔐 BÁO CÁO M1 — KHÓA HỢP ĐỒNG NGHIỆP VỤ & RÀ SOÁT PHẠM VI DỮ LIỆU

> **Ngày:** 30/07/2026 · **Module:** M1 (Dữ liệu nền) và các module tiêu thụ liên quan
> **Loại:** Rà soát + hoàn thiện + kiểm thử nhiều vai · **Môi trường:** máy phát triển nội bộ
> ⚠️ Báo cáo tóm tắt phục vụ đọc nhanh. Không chứa mã nguồn, cấu trúc dữ liệu, đường dẫn kỹ thuật hay dữ liệu thật.

---

## 1. Tóm tắt cho Owner

Đợt làm việc này tập trung vào **quyền xem dữ liệu khách hàng** và **tính toàn vẹn của dữ liệu nền**.

Kết quả chính:

- Đã **bịt các lỗ hổng phạm vi dữ liệu**: trước đây một số màn hình cho phép người dùng nhìn thấy dữ liệu khách hàng **ngoài phạm vi được giao**. Nay mọi màn hình liên quan đều dùng **chung một quy tắc phân quyền**.
- Đã **kiểm thử thật với nhiều vai người dùng** trên dữ liệu nội bộ (Kinh doanh A, Kinh doanh B, Kế toán, Quản trị, tài khoản chưa gắn nhân sự) — mỗi vai chỉ thấy đúng phần của mình.
- Đã **bảo toàn dữ liệu cũ** của bảng giá công đoạn: các dòng cấu hình cũ vẫn sửa được thông tin thường (giá, ghi chú, ngày) mà **không bị hệ thống tự ý chuẩn hóa hay làm mất cấu hình**.
- Đã **khóa hợp đồng đấu nối** để các module sau (Báo giá, Sản xuất, Kho, Tài chính) **không phải rà soát lại M1 từ đầu**.

Toàn bộ đợt này **chỉ chạy trên máy nội bộ**: không thay đổi cấu trúc dữ liệu, không đưa lên máy chủ vận hành.

---

## 2. Những gì đã sửa

| Nhóm vấn đề | Trước | Sau |
|---|---|---|
| Phạm vi khách hàng của nhân viên kinh doanh | Một số nhánh cho phép nhìn vượt phạm vi | Chỉ thấy khách được giao; sai/thiếu thông tin nhân sự thì **khóa lại**, không mở rộng |
| Truy cập trực tiếp bằng đường dẫn | Có thể mở hồ sơ khách không thuộc mình | Chặn ở phía máy chủ, thông báo không có quyền |
| Tự nhận khách của người khác | Có thể xảy ra khi chỉnh sửa hồ sơ | Chỉ người có **quyền chuyển giao** mới đổi được người phụ trách |
| Danh bạ khách hàng (tên, số điện thoại) | Trả về toàn bộ công ty | Chỉ trả về trong phạm vi được giao |
| Nhập khẩu danh sách khách từ tệp | Mặc định gán cho **một** nhân viên cố định | Gán theo nhân viên của người thực hiện; không xác định được thì **dừng lại**, không gán bừa |
| Nhật ký thao tác (ai tạo/ai sửa) | Ghi sai người thực hiện | Ghi đúng người đang đăng nhập |
| Các màn hình Báo giá / CRM / Đơn hàng | Lấy toàn bộ danh sách khách | Dùng chung quy tắc phạm vi của M1 |
| Bộ lọc đơn hàng ở Giao hàng | Hiển thị đơn ngoài phạm vi | Lọc theo phạm vi được giao |
| Thông báo thời gian thực | Có kèm mã hồ sơ | Chỉ báo “có thay đổi”, không kèm định danh hồ sơ |
| Đơn vị tính công đoạn | Phụ thuộc một mã số cứng | Tra theo **mã chuẩn nghiệp vụ**, ô chọn và kiểm tra dùng **cùng một nguồn** |
| Cấu hình tính giá công đoạn | Có nguy cơ mất cấu hình cũ khi lưu | **Giữ nguyên toàn bộ cấu hình cũ**, chỉ thay phần người dùng thực sự sửa |
| Sửa từng phần dòng mua hàng | Có thể làm thành tiền về 0 | Giữ giá trị hiện có; thiếu dữ liệu thì **dừng lại**, không ghi số giả |
| Thông tin giá vốn qua giao diện tra cứu | Ai cũng xem được | Bắt buộc đăng nhập; **chỉ người có quyền xem chi phí** mới thấy giá vốn |
| Chức năng kiểm thử kỹ thuật | Truy cập được ở mọi môi trường | Ẩn ở môi trường vận hành, chỉ quản trị mới dùng được ở môi trường phát triển |

---

## 3. Kết quả kiểm thử

Toàn bộ kiểm thử được **lưu trong dự án** và chạy lại được bằng lệnh chuẩn (không phải kiểm thử tạm thời).

| Bộ kiểm thử | Kết quả |
|---|---|
| Quy tắc phạm vi & hợp đồng dữ liệu | **75/75 đạt** |
| Nhiều vai người dùng (chạy trên dữ liệu thật của máy nội bộ) | **17/17 đạt** |
| Bảo toàn dữ liệu cũ bảng giá công đoạn + kiểm soát truy cập | **27/27 đạt** |
| Kiểm tra đồng bộ quy tắc quản trị | Đạt |
| Kiểm tra chuẩn dữ liệu dùng chung | Đạt |
| Chính sách phiên bản | **25/25 đạt** |
| Biên dịch kiểu dữ liệu | Đạt, không lỗi |
| Đóng gói ứng dụng (build) | **Thành công** |

**Ma trận nhiều vai (kiểm thử thật):**

| Vai | Nguồn quyền | Phạm vi nhìn thấy |
|---|---|---|
| Kinh doanh A | theo nhân sự được gắn | chỉ khách của mình |
| Kinh doanh B | theo nhân sự được gắn | chỉ khách của mình |
| Kế toán | **quyền được cấp thật** (không phải mặc định) | toàn bộ khách |
| Quản trị | quyền quản trị | toàn bộ |
| Tài khoản chưa gắn nhân sự | — | **không thấy gì** (khóa an toàn) |

Kiểm thử trên dữ liệu cũ **tự khôi phục nguyên trạng** sau khi chạy — không làm thay đổi dữ liệu.

---

## 4. Nguyên tắc nghiệp vụ đã khóa

1. **Người phụ trách khách hàng** là **nhân sự**, còn **người thao tác** là **tài khoản đăng nhập** — hai khái niệm tách bạch, không dùng lẫn.
2. Không xác định được nhân sự của người dùng → **khóa lại** (không mở rộng phạm vi).
3. Quyền “xem toàn bộ” phải đến từ **cấp quyền thật**, không suy đoán theo vai trò hay tên.
4. Mọi module tiêu thụ dữ liệu khách hàng **dùng chung một quy tắc phạm vi**, không tự viết riêng.
5. Giấy carton và giấy in **giữ hai đơn vị giá riêng biệt**, không quy đổi chung.
6. Dữ liệu cấu hình cũ **được giữ nguyên**, chỉ kiểm tra khi người dùng thực sự thay đổi.
7. Không dùng số 0 để thay cho “chưa có giá”.
8. Chứng từ mua hàng đi theo **một luồng duy nhất**: Nháp → Đã đặt → Đã nhận → Hoàn tất (hoặc Hủy); dùng thuật ngữ **“Công nợ”**.

---

## 5. Phần chưa làm (có chủ đích)

| Hạng mục | Lý do |
|---|---|
| Tính giá công đoạn tự động | Chưa triển khai trong đợt này — thuộc giai đoạn Tính Giá |
| Giao diện chuyển trạng thái mua hàng | Thuộc module Kho — đã khóa hợp đồng, chưa dựng giao diện |
| Phân quyền chi tiết cho nhóm chức năng tính giá | Chờ Owner duyệt danh sách quyền |
| Chuẩn hóa dữ liệu đơn vị tính cũ | Chờ Owner quyết định — **không tự động sửa dữ liệu** |
| Xử lý 2 sản phẩm thiếu liên kết khách hàng | Chờ Owner quyết định cách gán |

---

## 6. Việc cần Owner quyết

1. **Cấp quyền trên môi trường vận hành** — hiện chưa có quyền nào được cấp. Nếu đưa lên mà chưa cấp:
   - nhân viên kinh doanh chỉ thấy khách của mình (đúng thiết kế);
   - **kế toán sẽ chưa thấy toàn bộ khách**;
   - **giá vốn sẽ chưa hiển thị** cho người không phải quản trị.
2. **Cách gán 2 sản phẩm** đang thiếu liên kết khách hàng.
3. **Chuẩn hóa dữ liệu đơn vị tính cũ** của bảng giá công đoạn (7 dòng).
4. Một số **điểm cấu trúc dữ liệu** cần thống nhất trước khi mở rộng luồng mua hàng ↔ công nợ.

---

## 7. Trạng thái đợt làm việc

| Mục | Trạng thái |
|---|---|
| Hoàn thiện M1 trên máy nội bộ | ✅ Xong |
| Kiểm thử nhiều vai | ✅ Đạt |
| Bảo toàn dữ liệu cũ | ✅ Đạt (không thay đổi dữ liệu) |
| Khóa hợp đồng đấu nối cho module sau | ✅ Xong |
| Đóng gói bản triển khai | ✅ Đã tạo (kèm mã kiểm tra toàn vẹn) |
| Thay đổi cấu trúc dữ liệu | ❌ Không thực hiện |
| Đưa lên máy chủ vận hành | ❌ **Chưa** — chờ Owner duyệt phần cấp quyền |

---

## 8. Ghi chú

- Đợt này **không** thay đổi cấu trúc dữ liệu, **không** chỉnh dữ liệu vận hành, **không** đưa lên máy chủ.
- Cấu hình phục vụ kiểm thử chỉ nằm trên máy nội bộ và **có sẵn phương án hoàn tác**.
- Báo cáo kỹ thuật chi tiết được lưu nội bộ trong dự án; repo công khai này **chỉ đăng phần tóm tắt nghiệp vụ**.
