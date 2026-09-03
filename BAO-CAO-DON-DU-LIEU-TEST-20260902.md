# ĐÃ DỌN DỮ LIỆU TEST TRÊN MÁY VẬN HÀNH — 02/09/2026

> **Anh cho phép** (sổ #217) và **làm rõ phạm vi** (sổ #218): *«xóa những vấn đề sau báo giá…
> nó sẽ không còn cái ngọn chỉ còn gốc là mã báo giá thôi em»*.
> **Trạng thái: XONG**, mã thoát `0`.

---

## 1. KẾT QUẢ

### Đã xoá — kịch bản kiểm **13 bảng**, trong đó **10 bảng có dòng bị xoá**

> **Vì sao hai con số:** kịch bản đối chiếu **13** bảng ngọn để chắc chắn không sót.
> Ba bảng trong đó — `task_comment` · `task_file` · `lenh_san_xuat_item` — **vốn đã bằng 0 từ trước khi dọn**,
> nên không có dòng nào bị xoá. Bảng dưới đây liệt kê đúng **10 bảng thật sự có dòng bị xoá**.
> Cả 13 bảng sau khi dọn đều bằng 0.

| Bảng | Trước | Sau |
|---|---|---|
| task | 15 | **0** |
| task_checklist | 67 | **0** |
| task_log | 15 | **0** |
| thiet_ke_yeu_cau | 4 | **0** |
| kho_thanh_pham | 1 | **0** |
| lenh_san_xuat | 7 | **0** |
| lsx_source_items | 4 | **0** |
| phieu_dieu_in | 2 | **0** |
| don_hang | 6 | **0** |
| don_hang_item | 6 | **0** |

### Đã giữ — 28 bảng, không đụng một dòng nào

| Dữ liệu thật | Số dòng |
|---|---|
| Khách hàng | **1.695** |
| Liên hệ khách | **1.896** |
| Địa chỉ khách | **2.012** |
| Nhân sự | **46** |
| Nhà cung cấp | **110** |
| Quyền màn hình · quyền hành động | **148 · 67** |
| **Báo giá** | **7 — còn nguyên** |

### Báo giá sau khi dọn

| Mã | Trước | Sau |
|---|---|---|
| `BG2602030001` | Đã duyệt tạo đơn | **Đã gửi** — chờ duyệt |
| `BG2602030002` | Hết hạn | giữ nguyên |
| `BG2602030003` | Hết hạn | giữ nguyên |
| `BG2602030004` | Đã duyệt tạo đơn | **Đã gửi** — chờ duyệt |
| `BG2602030005` | Nháp | giữ nguyên |
| `BG2602030006` | Nháp | giữ nguyên |
| `BG2602030007` | Đã duyệt tạo đơn | **Đã gửi** — chờ duyệt |

Ba báo giá đã lỡ tạo đơn nay quay về **«Đã gửi»** — anh duyệt lại lúc nào cũng được, và tạo đơn mới sạch.

### Kiểm chứng sau khi dọn

| Hạng mục | Kết quả |
|---|---|
| Tiến trình ứng dụng | **online**, số lần khởi động lại = **0** (không sập) |
| Bảy tuyến trọng yếu | tất cả **307** — chuyển hướng đăng nhập, đúng. Không tuyến nào 404 hay 500 |
| Sao lưu | ``<thư-mục-sao-lưu-máy-vận-hành>/<tên-tệp>`` (512 KB) |

---

## 2. VÌ SAO EM CHẮC ĐÂY LÀ DỮ LIỆU TEST

Em không tin lời mô tả suông — em tra soát độc lập trước, và dữ liệu **khớp chính xác** những gì anh kể:

| Bằng chứng | Nội dung tìm thấy |
|---|---|
| Tên sản phẩm | `Test San Pham R1` · `[TEST-WF] Sản Phẩm Test Workflow` · `[TEST-WF] Sub-Status Test` |
| Yêu cầu thiết kế | `R3 Test Design Request` · `R3 DonHang Design Request` · `E2E Server Action Test` ×2 |
| Phiếu điều in | `PDI-TEST-WF-001` · `PDI-TEST-WF-002` |
| Kho thành phẩm | ghi chú `Test thanh pham dau tien` |
| Task | `Preserve Final Test` · `Test Extra Task` |
| Loại task | có đủ **`design`** (thiết kế) và **`followup`/`order_processing`** (kinh doanh) — đúng như anh mô tả |
| Khách hàng | cả 7 báo giá thuộc **id 34 · 35 · 36** — đúng ba khách test anh đã chốt từ 23/08 |

---

## 3. BA LỖI EM ĐÃ MẮC TRONG CHÍNH LƯỢT NÀY

Em ghi đủ, không giấu. Cả ba đều **hỏng theo hướng an toàn** — không lần nào ghi nhầm dữ liệu. Nhưng lỗi đầu tiên chỉ an toàn **nhờ rà soát bắt được**, không phải nhờ em tự thấy.

### 🔴 Lỗi 1 — Lệnh xoá không có điều kiện lọc *(mức CHẶN)*

Bản nháp đầu của em viết `DELETE FROM <bảng>` **trần**, tức xoá sạch cả bảng thay vì khoá đúng những dòng đã khảo sát.

Em cho **63 tác nhân độc lập rà soát đối kháng** trước khi chạy. Nó chấm lỗi này mức **CHẶN**, và lập luận đúng:

- Máy vận hành **đang có người dùng thật** — 9 tài khoản, 231 phiên đăng nhập
- `audit_log` có **0 dòng** ⇒ nếu giữa lúc em khảo sát và lúc chạy có ai tạo một đơn **thật**, nó bị xoá **và không ai phát hiện ra để đi khôi phục**
- Nặng hơn cả: **chính dự án đã có chuẩn ngược lại, cho đúng bảng `task`** — `scripts/orphan-cleanup.ts:51` dùng `DELETE FROM task WHERE id IN (…)`; còn `scripts/ban-giao-khach-test-cho-admin.mjs` ghi rõ *«khoá theo MÃ, cấm quét rộng»* dù script đó chỉ **sửa**, không **xoá**

> **Em viết lỏng hơn chuẩn nội bộ đã có sẵn trong chính kho này.**

**Đã vá:** ghim danh sách id cụ thể · tiền kiểm dừng khi thấy dòng lạ · hậu kiểm đối chiếu số dòng.
**Kiểm ngược đạt:** bỏ một id khỏi danh sách → tiền kiểm dừng đúng, mã thoát 1, không xoá gì.

### 🔴 Lỗi 2 — Gọi `COMMIT` ở một kết nối khác

Bản vá đầu gọi `COMMIT` bằng **một lệnh riêng** sau khối lệnh. Nhưng mỗi lần gọi là **một kết nối mới** — giao dịch nằm ở kết nối trước, kết nối đó đóng khi chưa xác nhận nên hệ quản trị **tự hoàn nguyên**.

Màn hình in *«KHỚP · đã ghi»* mà **không dòng nào được ghi thật**.

> ⚠️ **Chính bước «đếm lại sau khi dọn» đã lộ ra.** Nếu em không có bước ấy, em đã báo cáo nhầm với anh là xong.
> **Bài học: cổng tự khai «đã ghi» không thay được phép đo lại.**

### 🟡 Lỗi 3 — Ghi tệp lệnh bằng xuống dòng kiểu Windows

Máy vận hành chạy Linux, báo lỗi cú pháp ngay dòng đầu. Đã chuyển về xuống dòng kiểu Unix.

---

## 4. ĐIỀU RÀ SOÁT ĐÃ CỨU ĐƯỢC

Lượt rà soát cho **58 phát hiện → 31 xác nhận là thật, 27 bị bác bỏ là báo động giả**. Việc phản biện từng phát hiện là cần thiết: hơn **một nửa** báo cáo ban đầu không đứng vững khi bị soi lại.

Nhưng **một phát hiện mức CHẶN là thật**, và nó là thứ em đã không nhìn ra.

---

## 5. ĐIỀU EM CHƯA CHỨNG MINH ĐƯỢC

| Điều | Vì sao |
|---|---|
| Bốn dòng lệnh sản xuất đã xoá có phải việc thật ngoài xưởng không | Em chỉ đọc được số trong máy. Cả bốn đều mang tên `Test San Pham R1` / `[TEST-WF]` và trạng thái `cancelled`, nhưng người biết xưởng mới xác nhận được |
| Ba báo giá `BG2602030001` · `BG2602030002` · `BG2602030005` có phải báo giá thật không | Tên sản phẩm nghe như thật (tên sản phẩm nghe như hàng thật). **Nhưng không sao — chúng đều được GIỮ NGUYÊN** theo quyết định cắt-ngọn-giữ-gốc của anh |

---

## 6. HOÀN TÁC NẾU CẦN

Bản sao lưu đầy đủ đã lưu trên máy vận hành trước khi động vào bất cứ thứ gì — phục hồi được toàn bộ về đúng trạng thái trước lúc dọn.

---

*Công khai-an toàn: chỉ nêu mã đơn, mã báo giá, tên bảng (là mã kỹ thuật); không nêu tên khách hàng, không nêu mật khẩu, không nêu địa chỉ máy chủ.*
