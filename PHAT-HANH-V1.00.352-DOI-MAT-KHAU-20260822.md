# PHÁT HÀNH V1.00.352 — NGƯỜI DÙNG TỰ ĐỔI ĐƯỢC MẬT KHẨU

**Ngày:** 22/08/2026 · **Loại:** phát hành giao diện + trải nghiệm, **không đổi cấu trúc dữ liệu**
**Mã phát hành (commit):** `fb8d6ea` · hồ sơ kèm theo: `7542c02`
**Bản trước:** V1.00.351

> Bản tin public-safe: không có tên người, thông tin đăng nhập, hay giá trị nhạy cảm.

---

## 1. Vấn đề Chủ sở hữu chỉ ra

Trên thanh trên cùng chỉ có ba thứ: **điều chỉnh giao diện · thông báo · tên người dùng**.
**Không có chỗ nào để đổi mật khẩu.**

Báo cáo trước đó của trợ lý kỹ thuật ghi *"đã có chức năng đổi mật khẩu"* — **đúng về mã nguồn, sai về
người dùng**. Trang đổi mật khẩu có tồn tại, nhưng **lối vào duy nhất** là bị hệ thống tự chuyển tới
sau khi đăng nhập **trong trường hợp tài khoản bị bắt buộc đổi**. Người dùng bình thường không có
đường nào tới đó ⇒ trên thực tế là **không có chức năng**.

Đây là một bài học đáng ghi: **"mã có" không đồng nghĩa "người dùng dùng được"**.

---

## 2. Đã làm gì

**(a) Tạo lối vào.** Khu vực người dùng trên thanh trên cùng nay là **nút mở menu tài khoản**:
đầu menu hiển thị tên + địa chỉ đăng nhập, bên dưới là **"Đổi Mật Khẩu"** và **"Đăng Xuất"**.
Nút thoát nhanh cũ **giữ nguyên** để không làm mất thói quen đang dùng.

**(b) Dựng lại trang đổi mật khẩu theo chuẩn giao diện dự án.** Trước khi sửa, đã **đọc toàn phần**
tài liệu chuẩn giao diện (371/371 dòng) theo đúng quy định nội bộ. Những gì thay đổi:

| Trước | Sau |
|---|---|
| Ba ô nhập **chỉ có chữ mờ gợi ý**, không có nhãn | Mỗi ô có **nhãn rõ ràng** kèm dấu bắt buộc |
| Lỗi hiện bằng **một dòng chữ đỏ** | **Khung cảnh báo chuẩn** (viền + nền đỏ nhạt + biểu tượng), đúng quy định hiển thị lỗi |
| Nút bấm màu xám trung tính | Nút chính theo **màu thương hiệu**, nút phụ viền |
| Đang gửi chỉ đổi chữ | Có **biểu tượng quay** theo chuẩn trạng thái đang tải |
| **Không có đường ra** — vào rồi kẹt | Có nút **"Quay Lại"** |
| Không báo gì khi nhập thiếu | Báo tại chỗ: *"còn thiếu N ký tự nữa"*, *"hai ô mật khẩu mới chưa khớp nhau"* |

**Không đổi** giao diện lập trình phía sau, **không đổi** quy tắc mật khẩu: vẫn tối thiểu **10 ký tự**,
vẫn **buộc đăng nhập lại** sau khi đổi thành công.

---

## 3. Kiểm chứng

| Nội dung | Kết quả |
|---|---|
| Kiểm giao diện trên máy phát triển | **12/12 đạt** — có menu · điều hướng đúng · có nhãn · có nút quay lại · khung lỗi đúng chuẩn · cảnh báo nhập liệu · **0 lỗi trình duyệt** |
| **Vòng đổi mật khẩu đầu-cuối** | **5/5 đạt** trên một **tài khoản thử tự tạo rồi tự xoá**: đăng nhập bằng mật khẩu cũ → đổi → hiện báo thành công → **bị đẩy về trang đăng nhập** → **mật khẩu cũ hết dùng được** → **mật khẩu mới đăng nhập được** |
| Bộ kiểm thử nền | **12 bộ · 548 khẳng định · xanh toàn bộ** |
| Kiểm kiểu · dựng bản phát hành | **0 lỗi · 0 lỗi** |
| Trên hệ thống vận hành | **7/7 đạt** · số hiệu bản chạy hiển thị **V1.00.352** · **dữ liệu không đổi** |

Có **sao lưu cơ sở dữ liệu mới ngay trước khi triển khai**; đợt này **không đổi cấu trúc dữ liệu**.

---

## 4. Một điểm tự khai

Thanh trên cùng đang dùng **một bộ biểu tượng khác** với bộ mà tài liệu chuẩn giao diện quy định.
Đây là **lệch có sẵn từ trước**, không phải do đợt này sinh ra. Khi thêm menu, đã **cố ý bám theo mã
xung quanh** thay vì trộn hai bộ biểu tượng trong cùng một thành phần — và **ghi vào sổ nợ kỹ thuật**
để đợt dọn giao diện xử lý một lượt, thay vì sửa lẻ tẻ.
