# ĐÃ TRIỂN KHAI `V1.00.367` LÊN MÁY VẬN HÀNH — 02/09/2026

> **Anh nói đúng:** *«module phân quyền giữa local và VPS đang khác nhau… em đang không hoàn tất deploy»*.
> Đúng vậy. Máy vận hành đứng ở `V1.00.366` suốt 36 giờ trong khi mã mới đã xong từ lâu.
> **Nay đã triển khai xong.**

---

## 1. NHẬN DẠNG BẢN PHÁT HÀNH

| Hạng mục | Trước | Sau |
|---|---|---|
| Phiên bản | `V1.00.366` | **`V1.00.367`** |
| Mã nguồn trên máy vận hành | `<mã-nguồn-riêng>` | **`<mã-nguồn-riêng>`** — khớp `origin/main` |
| Dấu vân tay bản dựng | `<mã-nguồn-riêng>…` | **``<vân-tay-bản-dựng — giữ ở sổ riêng>``** |
| Thời điểm dựng | — | 02/09/2026 23:17:54 |
| Thời điểm triển khai | — | 02/09/2026 16:17:56 UTC |
| Tiến trình | online | **online**, khởi động lại 0 lần |

**Chênh lệch đã đưa lên:** 23 commit · 69 tệp, tính từ `<mã-nguồn-riêng>`.

---

## 2. TẠI SAO MÀN PHÂN QUYỀN TRƯỚC ĐÂY KHÁC NHAU

Không phải lỗi dữ liệu, không phải lỗi cấu hình. Chỉ đơn giản là **mã mới chưa được đưa lên**:

- Máy phát triển đã có màn phân quyền làm lại (hai cột · xác nhận hai bước · móc kiểm ẩn)
- Máy vận hành vẫn chạy bản dựng cũ từ 36 giờ trước

Nay hai bên **chạy đúng cùng một mã nguồn** — `<mã-nguồn-riêng>`.

---

## 3. NHỮNG GÌ ĐI CÙNG BẢN NÀY

### Một báo giá — một đơn hàng *(quyết định của anh lúc 22:22)*

Em tìm ra **một lỗ hổng thật** mà chốt chặn cũ không bịt được:

> Chốt chặn cũ kiểm "đã có đơn chưa" **ở ngoài giao dịch**, rồi mới ghi ở trong.
> Hai người bấm cùng lúc — hoặc một người bấm hai lần vì mạng chậm — thì **cả hai đều thấy "chưa có đơn"** rồi **cả hai cùng ghi**.
> **Vẫn ra hai đơn.**

Đã vá bằng cách **khoá dòng báo giá** ngay đầu giao dịch. Người thứ hai phải chờ người thứ nhất xong mới đọc — lúc đó thấy đơn vừa tạo và bị chặn.

**Bài kiểm mới: 18/18 đạt**, gồm đủ 10 điều kiện anh yêu cầu. Quan trọng nhất là ca cuối:

```
Bỏ khoá  → hai yêu cầu đồng thời sinh ra 2 đơn
Có khoá  → hai yêu cầu đồng thời sinh ra 1 đơn
```

Tức khoá đó **thật sự là thứ đang chặn**, không phải may mắn.

**Đơn đã huỷ vẫn tính** — đúng như anh chốt. Có bài kiểm riêng cho ca này.

> ⚠️ Nói rõ một điều để không bị hiểu quá: bất biến này do **tầng phần mềm** giữ, **không phải cơ sở dữ liệu**. Bảng đơn hàng không có ràng buộc chống trùng ở tầng dữ liệu — thêm ràng buộc là đổi cấu trúc bảng, phải có đề xuất và anh duyệt riêng. Em **không tự thêm**.

### Hai hành động rỗng — hết im lặng nói dối

`Gửi thông báo` và `Gửi email` trước đây **chạy qua, không báo lỗi, người dùng tin là đã gửi** — trong khi chưa có dịch vụ nào được nối. Nay báo lỗi rõ bằng tiếng Việt kèm hướng dẫn thay thế.

> 🔴 **Và em phát hiện sổ nợ ghi sai:** sổ ghi *"create_task chạy được, chỉ notify/send_email là rỗng"*. **Sai.** Đo tận nơi thì **cả ba** đều rỗng. Đã sửa ghi nhận và thêm cảnh báo rõ cho `create_task`.

⚠️ **Một suýt nữa gây hỏng:** cả ba quy trình đang chạy (`báo giá`, `đơn hàng`, `thiết kế`) đều có cấu hình `Gửi thông báo`. Nếu chỗ gọi không bắt lỗi thì việc **chuyển trạng thái sẽ vỡ toàn bộ**. Em kiểm trước khi kết luận — chỗ gọi **có bắt lỗi từng hành động**, nên chỉ ghi log, không phá nghiệp vụ.

### Siết quyền màn Sơ Đồ Quy Trình

Lỗ hổng mở suốt nhiều ngày trên máy vận hành **nay đã đóng**. Cả 4 hàm ghi đều dùng khoá riêng của màn thay vì khoá cha rộng.

---

## 4. ĐỐI CHIẾU HAI MÔI TRƯỜNG — KHỚP TỪNG DÒNG

Em không so số đếm, em so **từng dòng một**:

| Lớp quyền | Máy phát triển | Máy vận hành | Kết quả |
|---|---|---|---|
| Quyền màn hình | 148 | 148 | **0 dòng lệch** |
| Quyền hành động | 67 | 67 | **0 dòng lệch** |
| Quyền trường nhạy cảm | 4 | 4 | khớp |
| Quyền phạm vi dữ liệu | 3 | 3 | khớp |
| Tài khoản thật | 9 | 9 | **khớp hoàn toàn** |

**Con số "66" trong báo cáo cũ là lỗi phép đo**, không phải dữ liệu lệch. Số thật luôn là 67 ở cả hai bên.

**Tài khoản của anh** mang **Quản trị + Giám đốc** ở **cả hai** môi trường. Máy vận hành có **2 tài khoản quản trị đăng nhập được** — không rơi vào thế chỉ còn một.

---

## 5. AN TOÀN VÀ ĐƯỜNG LÙI

| Bước | Kết quả |
|---|---|
| Sao lưu cơ sở dữ liệu trước khi động | ✅ có, và **đã thử phục hồi thật** |
| Diễn tập phục hồi | ✅ **101 bảng · 14/14 mốc nền khớp · băm quyền khớp** |
| Sao lưu bản chạy cũ | ✅ 48 MB |
| Mốc quay về | ✅ `<mã-nguồn-riêng>` / `V1.00.366` |
| Di trú cơ sở dữ liệu | **27/27 và 29/29 đạt · 0 lỗi nghiêm trọng** |

Diễn tập phục hồi ban đầu **không chạy được** trên máy vận hành (tài khoản không có quyền tạo cơ sở dữ liệu mới). Em **không bỏ qua** — em tải bản sao lưu về và phục hồi thật trên máy phát triển (cùng phiên bản MariaDB 10.11.10). Mã băm tệp khớp tuyệt đối hai đầu.

---

## 6. KIỂM SAU KHI TRIỂN KHAI

| Kiểm | Kết quả |
|---|---|
| Tiến trình | **online**, khởi động lại 0 lần |
| Tám tuyến chính | tất cả trả về đúng (chuyển hướng đăng nhập) |
| Cookie giả mạo | bị chặn |
| Lộ lỗi thô / mật khẩu ra trang | **0** |
| Dữ liệu thật | khách **hàng nghìn** · nhân sự **46** · quyền **148/67** — **nguyên vẹn** |
| Dư lượng tài khoản thử | **0** |

**Về 9 dòng lỗi trong nhật ký:** đều là *"Failed to find Server Action"* — xảy ra khi trình duyệt còn mở trang của bản cũ rồi gửi lệnh sang bản mới. **Bình thường sau mỗi lần triển khai**, tự hết khi người dùng tải lại trang. Không phải lỗi của bản này.

---

## 7. HAI BÁO ĐỘNG GIẢ EM ĐÃ TỰ LOẠI

Ghi lại vì cả hai đều suýt thành báo cáo sai:

1. **"Còn 5 chỗ dùng khoá quyền rộng"** — soi tận nơi thì **cả 5 đều là chú thích** ghi lại dòng cũ. Mã thật đã siết đủ 4/4. Đây là **lần thứ hai** phép đếm thô bắt nhầm chú thích trong dự án này.
2. **"Còn 1 dòng im lặng"** — dòng đó thuộc hàm khác, không phải hai hành động đang vá.

Em đo lại trước khi báo, thay vì báo theo con số đầu tiên nhìn thấy.

---

## 8. CÒN LẠI

| Việc | Trạng thái |
|---|---|
| **Anh nghiệm thu màn phân quyền trên máy thật** | ⏳ **Chỉ anh làm được** |
| `create_task` nối dịch vụ thật | Ngoài phạm vi đợt này — cần nối M8 |
| Ràng buộc chống trùng ở tầng dữ liệu | Cần đề xuất riêng, anh duyệt riêng |
| Nợ còn mở | **33** (giảm từ 36) |

---

## 9. CÂU HỎI DÀNH CHO ANH

Bản mới đã chạy trên máy thật. Anh vào `/m0/security` và thử ba việc:

1. **Cấp nhanh theo vai trò**
2. **Cấp thêm quyền riêng**
3. **Thu hẹp / thay mẫu**

Và để ý: tick một ô **chưa ghi gì cả** — phải bấm «Xem Lại & Lưu», xem bảng **Trước → Sau**, rồi xác nhận **hai bước** mới thật sự ghi.

> ### **Giao diện này đã đủ trực quan để anh tự phân quyền mà không cần hiểu kỹ thuật phân quyền chưa?**

---

*Công khai-an toàn: không mật khẩu, không địa chỉ máy chủ, không email hay tên người thật, không tên khách hàng, không số tiền.*
