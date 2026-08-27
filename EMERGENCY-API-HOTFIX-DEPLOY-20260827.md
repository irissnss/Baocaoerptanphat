# BẢN VÁ KHẨN CẤP — TẦNG API ĐÒI ĐĂNG NHẬP

**Loại:** phát hành khẩn cấp, cách ly
**Ngày:** 27/08/2026
**Quyết định Owner:** 27/08/2026 13:25 — *GO, triển khai bản vá API khẩn cấp*
**Người thi hành:** Agent IDE — một người ghi duy nhất (đã xác nhận bằng hai lần đo)

> **Trạng thái: `LIVE_VERIFIED`** — nhưng **chỉ trong phạm vi bản vá phiên/xác thực tầng API**.
> Không suy rộng ra toàn Batch 2, cũng không ra toàn hệ thống.

---

## 1. BẰNG CHỨNG TRƯỚC KHI VÁ

Gọi thẳng qua HTTPS công khai, **không kèm cookie phiên**:

| Tuyến | Trước khi vá |
|---|---|
| `/api/m3/stats` | **HTTP 200** — trả số liệu nghiệp vụ |
| `/api/tinh-gia/params` | **HTTP 200** — trả ~8 KB cấu hình |
| `/api/tinh-gia/blueprints` | **HTTP 200** — trả dữ liệu bản thiết kế |
| `/api/mf/stats` | **HTTP 200** |

Đếm bằng máy trên mã nguồn: **27/31** tệp tuyến không tự gác gì, trong đó **12** tệp có
phương thức ghi.

**Nguyên nhân gốc:** cổng chung của ứng dụng gác toàn bộ trang, nhưng có **một dòng** cho
cả tầng `/api` đi qua vô điều kiện. Tầng API không có cổng chung nào khác.

> Điểm cần nói thẳng: các phép đo ở lượt trước chạy trên **máy phát triển**. Lượt này mới
> đo **máy vận hành**, và kết quả cho thấy máy vận hành có **đúng** lỗ hổng đó.

---

## 2. GỐC VÀ BẢN PHÁT HÀNH

| Mục | Giá trị |
|---|---|
| SHA đang chạy trước khi vá | `0e73a7c` |
| Nhánh vá | `hotfix/api-session-gate` |
| Gốc của nhánh vá | **`0e73a7c`** — đúng commit đang chạy |
| Commit lớp 1 | `a4d5f52` |
| **SHA đã triển khai** | **`826817b`** |
| Số commit kéo theo | **0** — không lấy khoảng cách 53 commit |

Xác minh trước khi ghép: tệp cổng chung tại `0e73a7c` **byte-identical** với bản trước khi vá
(mã băm khớp) ⇒ bản vá áp sạch, không có xung đột ngầm.

---

## 3. PHẠM VI TỆP — ĐÚNG NHỮNG GÌ CẦN

| Nhóm | Số tệp |
|---|---|
| Cổng chung (lớp 1) | 1 |
| Cổng gác tập trung (lớp 2, tệp mới) | 1 |
| Tệp tuyến API được gắn cổng gác | 27 |
| Bài kiểm | 2 |
| Khai báo lệnh chạy kiểm thử | 1 |

**Không đụng:** lược đồ cơ sở dữ liệu (**không DDL**) · dữ liệu vai trò/menu (**không đồng bộ
hàng loạt**) · công thức tính giá, giá, khổ trải · giao diện · bất kỳ module nghiệp vụ nào.

### Cố ý KHÔNG port — nói rõ để không ai hiểu nhầm là đã xong

Hàm **che giá vốn** cho một tuyến báo giá **không** nằm trong bản khẩn cấp này. Lý do: tại gốc
triển khai, danh sách trường nhạy cảm chỉ có **2 mục** và **chưa** bao gồm bảng đó — port hàm
che sẽ kéo theo việc mở rộng danh sách, tức thay đổi ngoài phạm vi khẩn cấp.

Đo trên máy vận hành: bảng đó có **0 bản ghi** ⇒ tuyến đó **không rò rỉ gì hôm nay**. Việc mở
rộng danh sách thuộc bản phát hành Batch 2 bình thường.

---

## 4. HAI LỚP — VÀ VÌ SAO LỚP MỘT KHÔNG ĐỦ

**Lớp 1 — cổng chung.** Chỉ kiểm *có cookie phiên hay không*. Chạy trước mọi request nên không
tra cơ sở dữ liệu. Đây là **ngăn chặn**, **không phải xác thực**: cookie bịa · phiên hết hạn ·
phiên bị thu hồi · tài khoản bị khoá — **đều qua được**.

**Lớp 2 — cổng gác tập trung.** Bốn điều trên bị chặn thật, bằng cách tra cơ sở dữ liệu. Thêm:

- **401** chưa đăng nhập / phiên hỏng · **403** đã đăng nhập nhưng thiếu quyền — tách bạch,
  vì hai việc đó cần xử lý khác nhau.
- **Chống CSRF**: request ghi phải có nguồn gốc thuộc chính ứng dụng.
- **Fail-closed**: quyền khai **tập trung**; tuyến **không có** trong bảng thì **bị từ chối**.
  Thêm tuyến mới mà quên khai quyền ⇒ **chết ngay**, không **mở toang im lặng**.

Ma trận quyền **31/31 tuyến**, **không ô nào "chưa rõ"** — có cổng kiểm tự động canh.

---

## 5. KIỂM TRƯỚC KHI TRIỂN KHAI

Chạy trên **chính bản build sắp triển khai**, **đúng phương thức HTTP**, có **phiên đăng nhập
thật** (dựng tài khoản thử rồi dọn sạch).

| Điều kiện | Kết quả |
|---|---|
| Không cookie → 401 (4 tuyến) | ✅ |
| Cookie giả → 401 | ✅ |
| Đăng nhập sai → 401, không 404/500 | ✅ |
| **Đăng nhập hợp lệ → thành công + cấp phiên** | ✅ |
| Phiên hợp lệ + có quyền → **không bị chặn nhầm** | ✅ |
| Ghi + nguồn gốc lạ → 403 **và không phát sinh bản ghi nào** | ✅ |
| Ghi + nguồn gốc đúng + cookie giả → 401 | ✅ |
| Phiên hết hạn → 401 | ✅ |
| Có phiên nhưng thiếu quyền → **403** (không phải 401) | ✅ |
| Trang được bảo vệ không phiên → chuyển hướng đúng | ✅ |
| Tuyến chưa khai quyền → fail-closed | ✅ |

**21/21 đạt.** Kèm: kiểm kiểu ✅ · build sản xuất ✅ · cổng quản trị ✅ · ma trận API 31/31 ✅.

### Kiểm trước-deploy đã bắt được một lỗi thật — trước khi chạm máy vận hành

Bản đầu khai các tuyến thống kê theo **khoá module**. Nhưng Owner đã chốt 23/08 rằng quyền cấp
**theo từng màn**, nên đo được: khoá module có `can_view = 0` với **mọi vai trò trừ quản trị**,
trong khi vai trò kinh doanh vẫn thấy 5 màn con.

⇒ Khai theo khoá module sẽ **chặn nhầm hàng loạt người dùng hợp lệ** — đúng điều kiện hoàn tác
Owner nêu. Đã sửa sang ngữ nghĩa đúng: *"được xem ít nhất một màn của module"*.

**Nếu không kiểm bằng phiên đăng nhập thật thì lỗi này đã lên máy vận hành.**

### Hai lỗi của chính giàn kiểm thử — cũng đã sửa

1. Bản build thử thiếu tệp cấu hình môi trường ⇒ nối nhầm cơ sở dữ liệu khác, đăng nhập thất
   bại. Suýt bị đọc thành "bản vá làm hỏng đăng nhập".
2. Bước dọn dẹp thiếu thứ tự xoá theo khoá ngoại ⇒ **tài khoản thử còn lại** trong cơ sở dữ
   liệu. Đã sửa thứ tự và dọn sạch.

---

## 6. TRIỂN KHAI

| Mục | Giá trị |
|---|---|
| Bắt đầu | **2026-08-27T07:06:53Z** |
| Kết thúc | **2026-08-27T07:08:56Z** |
| Thời lượng | ~2 phút |
| SHA trước | `0e73a7c` |
| **SHA sau** | **`826817b`** |
| Cách làm | chuyển sang đúng SHA đã kiểm → cài phụ thuộc → build → kích hoạt → khởi động lại |
| Di trú / gieo dữ liệu / đồng bộ menu | **KHÔNG chạy** |

**Sao lưu trước khi triển khai** (giữ nguyên, chưa xoá): sản phẩm đang chạy (48 MB nén) ·
cổng chung · cấu hình môi trường · cấu hình tiến trình · **lệnh hoàn tác ghi sẵn**.

---

## 7. KIỂM KHÓI TRÊN MÁY VẬN HÀNH (HTTPS công khai)

### Ẩn danh — lỗ hổng đã đóng

| Tuyến | Trước | **Sau** |
|---|---|---|
| `/api/m3/stats` | 200 | **401** |
| `/api/tinh-gia/params` | 200 | **401** |
| `/api/tinh-gia/blueprints` | 200 | **401** |
| `/api/mf/stats` | 200 | **401** |
| `/api/m0/progress` | 200 | **401** |

### Các chiều còn lại

| Phép thử | Kết quả |
|---|---|
| Cookie giả → 401 | ✅ |
| Ghi + nguồn gốc lạ → **403** | ✅ |
| Trang được bảo vệ không phiên → **307** | ✅ |
| Trang đăng nhập → **200** | ✅ |
| Đăng nhập sai → **401** | ✅ |
| **Đăng nhập hợp lệ → 200 + cấp phiên** | ✅ |
| Tuyến chưa khai quyền → không phải 200 | ✅ |
| Không lộ trường giá vốn | ✅ (0 lần xuất hiện) |

### Phân quyền đúng theo dữ liệu của chính máy vận hành

Tài khoản thử mang vai trò kinh doanh:

| Tuyến | Kết quả | Đúng chưa |
|---|---|---|
| `/api/m3/stats` | **200** | ✅ vai trò đó **có** quyền module bán hàng |
| `/api/tinh-gia/params` | **200** | ✅ có mã quyền tương ứng |
| `/api/mf/stats` | **403** | ✅ vai trò đó **không** có quyền module tài chính |
| Tuyến quản trị tính giá | **403** | ✅ thiếu quyền |

Đối chiếu ngược với bảng phân quyền trên máy vận hành: module tài chính chỉ cấp cho quản trị
và kế toán ⇒ **403 là đúng, không phải chặn nhầm**.

### Nhật ký

| Kiểm | Kết quả |
|---|---|
| Tiến trình | **online**, **0 lần khởi động lại**, 114 MB |
| Lỗi 500 mới | **không có** |
| Vòng lặp chuyển hướng | **không có** |
| Ghi dữ liệu ngoài ý muốn | **không** — phép thử CSRF xác nhận số bản ghi không đổi |
| Lỗi mới sau triển khai | **không có** — nhật ký lỗi ghi lần cuối **~6 giờ TRƯỚC** khi triển khai |

Tài khoản thử đã **dọn sạch** (kiểm lại: còn 0).

---

## 8. HOÀN TÁC

**Không cần hoàn tác.** Không điều kiện nào của Owner kích hoạt:

| Điều kiện hoàn tác | Trạng thái |
|---|---|
| Đăng nhập hợp lệ hỏng | **KHÔNG** — đã kiểm, hoạt động |
| API hợp lệ bị chặn hàng loạt | **KHÔNG** — đúng theo phân quyền thật |
| Lỗi 500 tăng | **KHÔNG** |
| Vòng lặp chuyển hướng | **KHÔNG** |
| Tiến trình không ổn định | **KHÔNG** — 0 lần khởi động lại |
| Luồng nghiệp vụ gãy | **KHÔNG** phát hiện |

Bản sao lưu và lệnh hoàn tác vẫn giữ nguyên, dùng được ngay nếu cần.

---

## 9. NHỮNG GÌ CHƯA SỬA

| Việc | Ghi chú |
|---|---|
| Che giá vốn ở tuyến báo giá tính giá | Cố ý không port (mục 3). Bảng có **0 bản ghi** ⇒ chưa rò rỉ |
| Che giá vốn ở **ba trang con** của module bán hàng | Batch 2 |
| Không gửi trường giá vốn ra client thiếu quyền | Batch 2 |
| Lưu báo giá: nâng từ **khoá theo tên** lên **mã phương án ổn định** + giao dịch/khoá/kiểm phiên bản | Batch 2 — bản hiện tại chỉ là **ngăn chặn** |
| An toàn nhập liệu module dữ liệu nền | Batch 2 |
| Ba khoá đơn hàng | Batch 2 |
| Máy vận hành thiếu **3 vai trò · 28 menu · ~100 dòng quyền** so với bản phát triển | **Không** đồng bộ trong bản khẩn cấp này, theo đúng chỉ đạo |
| Khoảng cách phát hành **53 commit** | Vẫn còn — bản khẩn cấp cố ý **không** kéo theo |

---

## 10. VIỆC KẾ TIẾP

Đưa bản sửa ma trận quyền về nhánh chính (nhánh chính đang giữ bản khai theo **khoá module** —
tức bản **có lỗi chặn nhầm** đã phát hiện ở mục 5), rồi tiếp tục Batch 2 theo thứ tự Owner nêu:
che giá vốn ba trang con → không gửi trường giá vốn ra client → nâng lưu báo giá sang mã phương
án ổn định kèm giao dịch → an toàn nhập liệu → ba khoá đơn hàng → báo cáo Batch 2.

---

*Báo cáo an toàn công khai: không có tên miền nội bộ, cookie, token, thông tin đăng nhập, dữ
liệu thô, mã nguồn, hay thông tin hạ tầng.*
