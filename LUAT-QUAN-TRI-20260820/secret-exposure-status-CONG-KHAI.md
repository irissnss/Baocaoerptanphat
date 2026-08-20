# TRẠNG THÁI PHƠI NHIỄM THÔNG TIN NHẠY CẢM — **BẢN CÔNG KHAI (đã che vị trí)**


> ⚠️ **BẢN CHỤP MỘT THỜI ĐIỂM (19/08/2026) — HÃY KIỂM CÒN HẠN HAY KHÔNG.**
> Đây là trạng thái phơi nhiễm (bản đã che vị trí), **chỉ tồn tại ở bản công bố** (không có bản gốc tương ứng trong kho riêng tư để đối chiếu mã băm).
> **Cách kiểm:** đối chiếu ngày chụp với bản công bố mới nhất trong kho báo cáo; có bản mới hơn → bản này đã **LỖI THỜI**.

> **Bản này KHÔNG chứa:** giá trị nhạy cảm · đường dẫn file:dòng cụ thể · mã commit. Mọi vị trí đã **CHE** (bản đầy đủ nằm trong registry nội bộ — kho riêng tư).
> **Doc Version:** 1.0-public · **Lập:** 19/08/2026 bởi Agent IDE · **Thuộc luật:** `GOV-SECRET-IN-CODE-001`.
> Mục đích công bố: minh bạch quy trình rà + gỡ + khuyến nghị đổi khoá, để công cụ AI/đối tác đọc được cách dự án xử lý an toàn — KHÔNG lộ vị trí có thể dùng để dò lịch sử.

---

## 1) VÌ SAO CÓ FILE NÀY

Đợt gỡ 16/08/2026 tưởng đã xong nhưng **để sót**. Đợt 19/08/2026 quét lại đầy đủ (theo GIÁ TRỊ + theo MẪU) và tìm thêm — nhiều hơn hẳn con số ban đầu (**gấp ~4 lần**). Đây đúng loại lỗi mà `GOV-SECRET-IN-CODE-001` + cổng `test:secret-scan` sinh ra để chặn.

| Mốc | Số vị trí được nêu ban đầu | Số vị trí THỰC TẾ tìm được |
|---|---|---|
| Đầu vào (18/08) | 3 script | — |
| Quét lại (19/08) | — | **12 vị trí / 11 file** |

---

## 2) TỔNG HỢP ĐÃ GỠ **12/12** — (vị trí đã che)

| Nhóm giá trị | Mô tả (không lộ vị trí) | Số vị trí | Trạng thái |
|---|---|---|---|
| **A** (13 ký tự) — mật khẩu đăng nhập dùng chung máy nội bộ + vận hành | trong vài script tiện ích + 1 script migration | 4 | ✅ ĐÃ GỠ → chuyển đọc biến môi trường, thiếu là báo lỗi + exit ≠ 0 |
| **B** (12 ký tự) — mật khẩu seed tài khoản quản trị | trong một số báo cáo/tài liệu triển khai + 1 chú thích SQL | 7 | ✅ ĐÃ GỠ → thay placeholder |
| **C** (11 ký tự) — mật khẩu CSDL máy nội bộ | trong 1 báo cáo | 1 | ✅ ĐÃ GỠ → thay placeholder |

> Hai phát hiện đáng chú ý (đã che vị trí): (i) một vị trí **nằm trong file `.sql`** mà cổng bản đầu **bỏ qua toàn bộ đuôi `.sql`** → đã sửa cổng; (ii) một vị trí nằm trong **báo cáo tự tuyên bố "0 credential"** nhưng chính dòng đó lại trích credential làm ví dụ.

---

## 3) LOẠI TRỪ — CÓ LÝ DO, KHÔNG LOẠI TRỪ IM LẶNG (vị trí đã che)

| Nhóm | Lý do loại trừ |
|---|---|
| Sổ bí mật nội bộ | **Nơi được phép duy nhất.** Đã gitignore, `git ls-files` = 0 |
| File cấu hình môi trường (`.env*`) | Không commit — nơi được phép |
| Script triển khai có **placeholder tường minh** | Giá trị là chữ mô tả, không phải bí mật |
| File mẫu `.env*.example` | Placeholder |
| Tài liệu nêu **đường dẫn tới file khoá SSH** | Là đường dẫn, không phải giá trị khoá; file khoá không được git theo dõi |
| Ảnh chụp lịch sử (`_snapshot-*`) | Chỉ chứa placeholder; hồ sơ lịch sử, luật cấm sửa |
| Chú thích nêu **tên biến** môi trường | Không có giá trị |
| ~21 vị trí khớp mẫu khác | Dương tính giả: placeholder · biến môi trường · nội suy template · đường dẫn · ví dụ tài liệu |

---

## 4) ⚠️ KHUYẾN NGHỊ ĐỔI KHOÁ (ROTATION)

| Giá trị | Đã lên remote? | Khuyến nghị |
|---|---|---|
| **A** (13 ký tự) | CÓ (từ commit đầu kho) | 🔴 **ĐỔI NGAY** |
| **B** (12 ký tự) | CÓ (nhiều file đã commit) | 🔴 **ĐỔI NGAY** |
| **C** (11 ký tự) | CÓ | 🟡 **NÊN ĐỔI** (chỉ CSDL máy nội bộ trong Docker, không mở ra ngoài) |

> **Nguyên tắc:** giá trị đã tới remote thì **coi như đã phơi nhiễm**, bất kể kho công khai hay riêng tư. Gỡ khỏi cây làm việc **KHÔNG** hoàn tác phơi nhiễm.
> **Cập nhật 19/08/2026:** giá trị **A** đã được lên lịch **đổi mật khẩu tài khoản quản trị** (đồng bộ máy nội bộ + máy vận hành) — chi tiết ở báo cáo gói việc 19/08.

---

## 5) VIẾT LẠI GIT HISTORY — **HOÃN** (Owner quyết)

| Mục | Trạng thái |
|---|---|
| Gỡ khỏi **cây làm việc** | ✅ XONG — 12/12 |
| **Viết lại git history** | ⏸️ **HOÃN** — Owner chốt 19/08: kho mã `origin` là **RIÊNG TƯ** → mức độ thấp hơn, **KHÔNG viết lại history** (viết lại làm hỏng mọi bản clone); chỉ đổi khoá |

---

## 6) CỔNG THI HÀNH — ĐÃ KIỂM NGƯỢC (không phải cổng giả)

`npm run test:secret-scan` chạy **2 chế độ song song**: theo **MẪU** (biến có tên gợi ý password/token/secret) + theo **GIÁ TRỊ** (đúng chuỗi thật, đọc từ sổ bí mật nội bộ) trên **mọi file git theo dõi**.

| Kiểm ngược (`GOV-GATE-REAL-INPUT-001`) | Kết quả |
|---|---|
| Cắm 4 dạng thực tế | Bản đầu bắt 0/4 → đã vá → nay **4/4** |
| Phục hồi 4 file bản trước khi gỡ rồi quét | Bắt đúng **4/4** |
| Kho hiện tại | **0 vi phạm** cả hai chế độ |

> Nếu chưa kiểm ngược thì sẽ không phát hiện bản đầu bỏ lọt 4/4 — đúng loại "cổng báo PASS trong khi thứ nó gác đang hỏng".

---

## 7) LỊCH SỬ

| Ngày | Người | Nội dung |
|---|---|---|
| 19/08/2026 | Agent IDE (Owner duyệt 18/08) | Bản công khai (đã che vị trí) của registry nội bộ `secret-exposure-status`. Ghi 12 vị trí đã gỡ (che) · loại trừ có lý do · khuyến nghị đổi khoá · hoãn viết lại history (Owner chốt kho riêng tư). |
