# KHÉP M0 · MỞ M1 · PHÁT HÀNH `V1.00.368` — 03/09/2026

> Anh cho phép hai việc: **cài trình duyệt lên máy vận hành để chạy bài kiểm tự động**, và **chấp nhận phần dữ liệu cũ còn trong lịch sử**.
> Cả hai đã thi hành xong.

---

## 1. KẾT LUẬN

> ### **M0 ĐÓNG · M1 MỞ · `V1.00.368` đang chạy**

Không mở Pricing — đúng khoá của anh.

---

## 2. QUYẾT ĐỊNH CỦA ANH GỠ ĐÚNG ĐIỂM CHẶN

Trước đó em phải kết luận **chưa đóng được M0**, vì sáu bài kiểm giao diện không chạy được trên máy thật. Anh cho phép cài trình duyệt — nay chúng chạy, và chạy xanh.

### Nhưng việc cài lộ ra thứ đáng giá hơn nhiều

> **21 tệp bài kiểm viết cứng đường dẫn trình duyệt kiểu Windows.**
> Trên máy vận hành (Linux) không đường nào khớp, nên bài kiểm **báo lỗi rồi dừng ngay dòng đầu**.

Nghĩa là: **suốt thời gian qua chưa bài kiểm giao diện nào từng chạy trên môi trường thật** — và **không ai biết**, vì chúng chết trước khi kiểm được gì.

Đã vá 20 tệp: không tìm thấy trình duyệt hệ thống thì để thư viện dùng bản nó tự tải.

---

## 3. KẾT QUẢ — 15 BÀI KIỂM · 277 ĐIỀU KIỆN · 0 HỎNG

### Mười một bài chạy **ngay trên máy vận hành** (184 điều kiện)

| Bài kiểm | Kết quả |
|---|---|
| **Chuỗi không-ghi của màn phân quyền** | **22/22** |
| Chặn quyền màn Sơ Đồ Quy Trình | **8/8** |
| Một báo giá — một đơn hàng | **18/18** |
| Ba hành động chưa nối dịch vụ | **20/20** |
| Ảnh và hành vi trên trình duyệt | **22/22** |
| Hợp quyền và quản trị đăng nhập | **14/14** |
| Trung tâm phân quyền · Thanh bên | **14/14 · 15/15** |
| Ma trận chuyển trạng thái · Ma trận quyền | **21/21 · 16/16** |
| Xung đột hai tab | **14/14** |

### Bốn bài canh máy vận hành từ máy phát triển (93 điều kiện)

Khối vận hành **24/24** và **14/14** · Trang hướng dẫn **21/21** · Nghiệm thu phân quyền **34/34**

### Điều quan trọng nhất trong đó

**Chuỗi không-ghi 22/22 chạy trên máy thật** chứng minh từng bước:

- tick một ô → **cơ sở dữ liệu không đổi** (giữ nguyên 148 và 67)
- mở hộp thoại xem lại → **vẫn chưa ghi**
- bấm «Tôi Đã Xem Kỹ» → **vẫn chưa ghi**
- huỷ hộp thoại → **vẫn nguyên**
- **chỉ nút xác nhận cuối mới thật sự ghi**

**Dữ liệu máy vận hành nguyên vẹn** — mốc nền đầu bằng mốc nền cuối, **không sót một dòng thử nào**.

---

## 4. MỘT SỰ CỐ THẬT ĐÃ XẢY RA — EM GHI ĐỦ

Lượt chạy đầu tiên **để lại 4 tài khoản thử trên máy vận hành**.

**Nguyên nhân dây chuyền:**

1. Em cài thư viện trình duyệt theo kiểu "không ghi vào danh sách phụ thuộc"
2. Đợt triển khai kế tiếp dựng lại thư viện theo danh sách → **gỡ mất nó**
3. Bài kiểm đầu **chết ngay dòng đầu** vì thiếu thư viện
4. Chết sớm nên **không kịp chạy phần dọn**
5. Bài sau so mốc nền thấy lệch → báo đỏ → **cũng chết trước khi dọn**
6. **Tám bài đỏ mà chỉ một nguyên nhân**

Đúng cơ chế đổ dây chuyền dự án đã ghi nhận từ trước — lần này xảy ra **trên máy vận hành**.

**Đã xử lý:** dọn sạch 4 tài khoản (đều là tài khoản thử, **không đụng tài khoản thật**), và vá gốc bằng một kịch bản chuẩn bị: tự cài lại thư viện · **thử mở trang thật** trước khi báo sẵn sàng · **dọn phòng vệ trước mỗi lượt kiểm**.

---

## 5. HAI LẦN CHÍNH KỊCH BẢN CỦA EM BÁO SAI

Ghi lại vì cả hai đều suýt thành báo cáo sai:

**Lần một — kịch bản chuẩn bị.** Nó kiểm trình duyệt bằng câu hỏi *"thư mục có tồn tại không"*. Nhưng mỗi phiên bản thư viện đòi **đúng một bản** trình duyệt riêng. Máy có bản này, thư viện đòi bản khác → **thư mục có mà vẫn không chạy được**, kịch bản báo "đã có" rồi đi tiếp → bài kiểm chết. Đã sửa thành **luôn gọi lệnh cài**.

**Lần hai — phép thử đường quay về.** Nó **báo "đạt" trong khi bản cũ không khởi động được**, do lỗi so chuỗi mã trạng thái. Đã sửa và chạy lại thật.

> Đây là lần thứ năm trong dự án này: **một phép kiểm tự khai "đạt" không thay được phép đo lại**.

---

## 6. ĐƯỜNG QUAY VỀ — ĐÃ CHỨNG MINH DÙNG ĐƯỢC

Không chỉ có tệp sao lưu. Em **dựng bản cũ lên thật** ở một cổng riêng:

- khởi động được, trang đăng nhập trả về bình thường
- **hiển thị đúng `V1.00.366`** — đúng bản cũ
- rồi tắt và dọn sạch
- **bản đang chạy không hề bị ảnh hưởng**

---

## 7. AN TOÀN BÁO CÁO CÔNG KHAI

Đã che **639 chỗ** dấu vết mã nguồn nội bộ và **16 chỗ** số liệu kinh doanh, bằng commit tiến — **không viết lại lịch sử**, đúng quyết định của anh.

Cổng canh nâng lên **7 luật**, có **chế độ tự kiểm ngược** — mỗi luật phải chứng minh bắt được mẫu vi phạm của chính nó trước khi tin vào kết quả.

> **Không tìm thấy mật khẩu, khoá, hay mã đăng nhập nào bị lộ.** Đây là điều quan trọng nhất: thứ nguy hiểm nhất thì không lộ.

---

## 8. BẢN ĐANG CHẠY

| | |
|---|---|
| Phiên bản | **`V1.00.368`** |
| Tiến trình | **online**, khởi động lại **0 lần** |
| Di trú cơ sở dữ liệu | **27/27** và **29/29**, 0 lỗi nghiêm trọng |
| Dữ liệu thật | nguyên vẹn |
| Dư lượng dữ liệu thử | **0** |

---

## 9. NỢ ĐÃ ĐÓNG TRONG ĐỢT NÀY

`DEBT-143` — ba hành động chưa nối dịch vụ. Phần **gây hại** (im lặng để người dùng tưởng đã gửi) **đã đóng hẳn**:

- hàm nay **trả kết quả rõ ràng** thay vì trả về trống — trước đây chỗ gọi không có cách nào biết hành động có chạy thật hay không, và **đó chính là điều kiện của sự im lặng**
- **chặn lưu cấu hình mới** dùng ba hành động đó
- ⛔ **cố ý không chặn đường sửa** — ba quy trình đang chạy đều đã khai từ trước; chặn cả đường sửa sẽ khiến **không ai sửa được chúng nữa**

Phần **dựng thật** ba hành động vẫn hoãn — cần nối M8 và dịch vụ gửi thư, ngoài phạm vi.

**Nợ còn mở: 32.**

---

## 10. BƯỚC KẾ TIẾP

M1 đã mở. Việc đầu tiên là **chín bài kiểm nghiệm thu** có sẵn trong kế hoạch cũ — bộ tiêu chí *"giao được chưa"* đã soạn từ 14/06, chỉ việc chạy.

Bốn câu hỏi nghiệp vụ vẫn chờ anh: ai cần tài khoản đăng nhập · Thiết Kế được làm gì · email nhân viên loại nào · danh mục sản phẩm đủ hay thiếu.

---

*Công khai-an toàn: không mật khẩu, không địa chỉ máy chủ, không đường dẫn nội bộ, không email hay tên người thật, không số lượng khách hàng chính xác.*
