# CỔNG DUYỆT GIÁ M3 + DỪNG NHÓM TÍNH GIÁ — 21/08/2026

> **Bản công khai (public-safe).** Nêu tên bảng · cột · route · mã commit. **KHÔNG** nêu thông tin đăng nhập, dữ liệu cá nhân, số tiền thật, hay thông tin hạ tầng máy chủ.
> **Phạm vi phiên:** CHỈ máy nội bộ. **KHÔNG** phát hành, **KHÔNG** nâng số phiên bản, **KHÔNG** ghi lên máy vận hành.
> **Kết quả tổng:** GĐ1 **ĐẠT** · GĐ2 **DỪNG (chờ Owner)** · GĐ3 **chưa chạy** · GĐ4 **xong (chỉ kế hoạch)**.

---

## 1. GĐ1 — CỔNG DUYỆT GIÁ: "LẬP ≠ DUYỆT" (ĐẠT)

### Vấn đề gốc
Cổng *"chỉ báo giá đã duyệt mới tạo được đơn"* đã có và chạy đúng. Nhưng **cửa duyệt bỏ ngỏ**: ai có quyền sửa báo giá (gồm chính người LẬP) đều tự chuyển trạng thái sang `approved_for_order` được — tức **tự duyệt giá của chính mình**. Cổng phía sau vì thế mất tác dụng thực tế.

### Đã làm

| Mã | Nội dung | Kết quả |
|---|---|---|
| A1 | **Danh sách trắng** cho đường cập nhật báo giá: chỉ cho ghi `id_khach_hang` · `id_nguoi_lien_he` · `ngay_bao_gia` · `ngay_hieu_luc` · `ghi_chu`. **CHẶN** `trang_thai` · `tong_tien` · `nguoi_tao` · `ngay_tao` · `id_nhan_vien_phu_trach` | ✅ |
| A2 | Tách quyền duyệt riêng `can_approve_bao_gia` (theo cơ chế `role_action_permission` sẵn có) → cấp cho **Admin + CEO**. Người LẬP không có quyền này | ✅ |
| A3 | **Khoá sửa-sau-duyệt** ở **7 cửa** tầng server action **+ một lớp chặn cuối ở tầng dữ liệu** (phủ cả đường script/module khác). Muốn sửa → hạ duyệt rồi duyệt lại | ✅ |
| A4 | Điều kiện *"mọi sản phẩm phải có một phương án được chọn"* nay kiểm **ở máy chủ** (trước đây chỉ kiểm ở trình duyệt nên gọi thẳng là đi vòng qua được) | ✅ |
| A5 | Danh sách chọn "Sale phụ trách" trả **mã nhân viên**, không trả mã tài khoản | ✅ |
| A6 | Gỡ 2 trang giao diện giả khỏi thực đơn (`/m3/bao-gia/item`, `/m3/bao-gia/option`) — nút Lưu/Xóa ở đó chỉ hiện thông báo, không ghi dữ liệu. **Giữ file**, gắn biển "KHÔNG DÙNG" | ✅ |
| A7 | Xoá script kiểm thử sót lại tự đặt trạng thái đã-duyệt | ✅ |
| A8 | Bỏ **13 chỗ** dùng giá trị mặc định cứng cho cột ghi-nhận-người-thao-tác → chuyển sang tra người đăng nhập thật, **dừng hẳn nếu không xác định được** | ✅ |

### Vì sao A5 là lỗi thật, không phải chuyện hình thức
Đo trên cơ sở dữ liệu nội bộ: cột `dm_khach_hang.sale_phu_trach` đang dùng dải mã **nhân viên**, trong khi danh sách chọn cũ phát ra dải mã **tài khoản**. **Hai dải không giao nhau** ⇒ mọi lượt chọn qua danh sách cũ đều gán người phụ trách sai.

---

## 2. PHÁT HIỆN NGOÀI ĐỀ BÀI — "GIAO DỊCH" TRƯỚC ĐÂY KHÔNG BẢO VỆ ĐƯỢC GÌ

Đề bài chỉ yêu cầu *"bổ sung giao dịch cho đường lưu báo giá"*. Khi đọc mã để làm việc đó thì thấy **mẫu giao dịch đang dùng vốn đã hỏng**.

**Nguyên nhân:** hàm truy vấn dùng chung lấy **một kết nối bất kỳ trong nhóm 10 kết nối** cho **mỗi lần gọi**. Vì vậy `START TRANSACTION`, các câu `INSERT`, rồi `COMMIT` **không bảo đảm chạy trên cùng một kết nối**.

**Đo thật (chạy được, lặp lại được):** hai luồng song song, luồng A quay lui (`ROLLBACK`), luồng B xác nhận (`COMMIT`).

| | Kỳ vọng nếu giao dịch đúng | Thực tế đo được |
|---|---|---|
| Dòng còn lại | `["B", "B2"]` | **`["A", "B2"]`** |

Nghĩa là: dòng của A **sống sót qua chính lệnh quay lui của nó**, còn dòng đầu của B **bị lệnh quay lui của A xoá mất**. Giao dịch kiểu cũ vừa không tự bảo vệ được, vừa **phá dữ liệu của người dùng khác đang thao tác cùng lúc**.

**Đã vá:** thêm hàm `withTransaction` ghim đúng một kết nối cho cả khối; áp cho 3 hàm của M3 (lập báo giá · lưu báo giá · tạo đơn từ báo giá). Kiểm ngược lại cho đúng `["B", "B2"]`.

> Nếu chỉ chép lại mẫu cũ theo đúng chữ của đề bài thì kết quả là **thêm một lớp bảo vệ giả** — đúng loại lỗi mà luật `GOV-GATE-REAL-INPUT-001` cấm.

---

## 3. MỘT LỖI CỦA CHÍNH PHIÊN NÀY — DO BỘ KIỂM THỬ BẮT ĐƯỢC

Khi cấp quyền duyệt, bản làm đầu tiên có ghi **tường minh** một dòng "Sale = không được duyệt" cho dễ đọc bảng phân quyền.

Bộ kiểm thử phân quyền M1 **bắt ngay**: mô hình phân quyền của hệ thống là **chỉ cấp quyền dương**. Hàm kiểm quyền lấy giá trị **lớn nhất** trong các vai trò của người dùng, nên với người mang nhiều vai trò thì `max(0, 1) = 1` ⇒ **dòng "không được" đó không từ chối được gì**, chỉ khiến người đọc tưởng là có cơ chế từ chối.

**Đã sửa:** gỡ dòng đó khỏi cả tập tin nâng cấp lẫn cơ sở dữ liệu nội bộ. Không có dòng nào = không có quyền — đó mới là cách từ chối thật. Sau khi sửa: **99 đạt / 0 hỏng**.

---

## 4. 🛑 GĐ2 — DỪNG THEO ĐÚNG ĐIỀU KIỆN DỪNG CỦA ĐỀ BÀI

**Điều kiện dừng đã kích hoạt:** *"công thức khổ trải mâu thuẫn tài liệu"*.

### Bằng chứng

**(a) Tài liệu quy định công thức phải nằm trong cấu hình sản phẩm**
- Tài liệu thiết kế nhóm tính giá ghi rõ: *"Nhóm Sản Phẩm quyết định logic tính khổ trải + bình bài"* và *"Cấu hình công thức tính khổ trải"* là việc của Quản trị.
- Chú thích của bảng `dm_blueprint` trong chính cơ sở dữ liệu ghi: *"Blueprint definitions for flat size + imposition"*.

**(b) Nhưng dữ liệu cấu hình gần như trống**

| Bảng cấu hình | Số dòng thật | Ghi chú |
|---|---|---|
| `dm_blueprint` | **1** | và là bản **DEMO**; phần cấu hình **không chứa công thức khổ trải** nào |
| `dm_auto_pricing_formula` | **1** | |
| `dm_bang_gia_cong_doan` | **10** | trên tổng **85** công đoạn đã khai ở `dm_cong_doan` |

**(c) Trong mã có hai công thức khác nhau, đều viết cứng**

| Nơi | Công thức | Nhận xét |
|---|---|---|
| Màn tính giá thủ công | `rộng = (W+H)×2 + 21` · `cao = L+W + 36` | **trùng khớp** bản vẽ minh hoạ hộp nắp cài (21 = mép dán 15 + bù xén 3×2; 36 = nắp 30 + 6) |
| Bộ máy tính giá tự động | `rộng = (L+W+20)×2` · `cao = H+W+20` | quy ước trục **L/W/H khác hẳn** |

⇒ Hai bên **không quy được về nhau** nếu chưa chốt quy ước trục và kiểu hộp. Đề bài **cấm tự chọn** (*"nếu mâu thuẫn nghiệp vụ → DỪNG, trình Owner, cấm tự chọn"*).

### Hệ quả dây chuyền
- **Không thể gỡ hết giá cứng ở màn tính giá thủ công** như mong muốn: giá giấy và giá công đoạn thì **lấy được từ dữ liệu** (18/19 vật tư đã có giá tham chiếu), nhưng **danh mục khổ giấy** hiện **không có chỗ lưu nào trong cơ sở dữ liệu**.
- Cách mô hình hoá cũng **lệch nhau**: màn thủ công tách "giá kẽm + giá lượt in", còn cơ sở dữ liệu gộp thành **một công đoạn "In Offset N Màu M Mặt"** có sẵn cả đơn giá lẻ và giá trọn gói. Đây là **thay đổi cách nhập liệu**, không phải đổi vài hằng số.
- **Không có bản thiết kế thật để chạy kiểm thử** ⇒ yêu cầu *"bộ kiểm thử phải chạy trên bản thiết kế THẬT"* hiện **không thực hiện được**.

### Đã KHÔNG làm gì trong vùng tính giá
**Không sửa một dòng mã nào** trong nhóm tính giá — kể cả phần được phép — vì mọi việc ở đó đều phụ thuộc câu trả lời về công thức.

---

## 5. GĐ3 — CHƯA CHẠY

Đề bài quy định *"GĐ trước PASS mới sang GĐ sau; FAIL → DỪNG toàn chuỗi, báo Owner"*. GĐ2 dừng nên **không tự ý chạy tiếp GĐ3**, dù phần đơn hàng độc lập về mặt kỹ thuật và đã khảo sát xong đường đi.

---

## 6. GĐ4 — KẾ HOẠCH NẠP DỮ LIỆU BẢN v5 (XONG — CHỈ KẾ HOẠCH)

Nâng bản v4 → **v5** với 9 mục, **mỗi mục đo trực tiếp** trên cơ sở dữ liệu hoặc đọc thẳng mã nguồn kèm vị trí dòng — không chép lại đề bài.

Vài điểm đáng chú ý:

- **Chốt dùng script riêng cho việc nạp**, **không** dùng đường nạp hàng loạt sẵn có: đường đó có **3 khối bắt lỗi để TRỐNG** (nuốt lỗi ghi địa chỉ/liên hệ) và **viết cứng mã tỉnh** ⇒ mọi khách sẽ rơi hết về một tỉnh.
- **Rủi ro âm thầm ở trạng thái nhân viên**: hàm chuẩn hoá nhận đúng 9 khoá, **mọi giá trị lạ đều lặng lẽ thành "thử việc"**. Nếu nguồn ghi có dấu tiếng Việt thì **cả danh sách** có thể bị quy về "thử việc" mà không báo gì. v5 bắt buộc thêm bước chuẩn hoá riêng + **đối chiếu số đếm theo từng trạng thái** sau khi nạp.
- **Đính chính đề bài:** mã khách hàng theo quy ước mới dài **14 ký tự** (không phải 15); **15 là mã nhà cung cấp**. Kết luận không đổi — cả hai vừa cột đích.
- **Hai thay đổi cấu trúc phải trình duyệt trước** (thêm cột ghi-nhận-người-thao-tác kiểu số cho nhà cung cấp; nới cột số điện thoại).
- Kết thúc bằng **9 điểm chờ Owner duyệt**.

---

## 7. BẰNG CHỨNG KIỂM THỬ

| Bộ kiểm | Kết quả |
|---|---|
| Cổng "chỉ báo giá đã duyệt mới ra đơn" (có sẵn) | **11/11 đạt** — không hồi quy |
| Cổng duyệt giá (**mới viết** phiên này) | **25/25 đạt** |
| Phân quyền linh hoạt M1 | **99/99 đạt** (sau khi sửa lỗi mục 3) |
| Quyền sở hữu + hợp đồng M1 | 90/90 đạt |
| Đa tác nhân M1 | 51/51 đạt |
| Hợp đồng thực đơn M1 | 39/39 đạt |
| Khoá sổ chi tiết M1 | 68/68 đạt |
| Quyết định Owner M1 | 67/67 đạt |
| Chống mồ côi M1 | 32/32 đạt |
| Giao việc thiết kế | 19/19 đạt |
| Nhập–xuất–tồn M5 | 15/15 đạt |
| Kiểm kiểu TypeScript | **0 lỗi** |
| Dựng bản phát hành | **thành công** |
| Bộ cổng quản trị | **toàn bộ đạt**, gồm 5 tập tin luật **giống hệt nhau từng byte** |

Dữ liệu kiểm thử **tự dọn về đúng mốc ban đầu**: 0 dòng thử còn sót, 0 bảng tạm còn sót.

---

## 8. VIỆC CÒN LẠI — ĐÃ GHI SỔ NỢ KỸ THUẬT

Ghi mới **9 mục** (`DEBT-040` → `DEBT-048`), đóng `DEBT-025`, chuyển `DEBT-026` sang *đang xử lý*. Nhóm chính:

- Chưa quét toàn kho tìm các chỗ khác còn dùng mẫu giao dịch cũ (mục 2).
- Nhóm tính giá đang chờ quyết định (công thức khổ trải · dữ liệu cấu hình · kiểm thử dùng bản thiết kế giả).
- Đường nạp hàng loạt trên giao diện vẫn còn khiếm khuyết (chỉ mới **chốt không dùng nó cho việc nạp thật**, chưa vá).
- Danh sách chọn "Sale phụ trách" chỉ hiện người **đã có tài khoản** — sau khi nạp danh sách nhân viên thật sẽ gần như không chọn được ai cho tới khi liên kết tài khoản xong.
- Sổ nợ có **mã trùng** (hai mã bị dùng hai lần cho hai nội dung khác nhau) — không tự đánh số lại vì sẽ phá các tham chiếu chéo.

---

## 9. ĐANG CHỜ OWNER QUYẾT

| # | Việc | Chặn gì |
|---|---|---|
| 1 | **Công thức khổ trải**: chốt quy ước trục L/W/H + kiểu hộp, và có đưa công thức vào cấu hình sản phẩm như tài liệu đã thiết kế không | Toàn bộ GĐ2 |
| 2 | **Danh mục khổ giấy** lưu ở đâu (bảng mới, hay dùng bảng tham số sẵn có) | Gỡ hết giá cứng ở màn tính giá |
| 3 | **Đổi cách nhập phần in** từ "kẽm + lượt in" sang chọn công đoạn có sẵn giá trong dữ liệu — đây là đổi giao diện nhập liệu | Gỡ hết giá cứng |
| 4 | **Nạp bản thiết kế thật** (hiện chỉ có 1 bản demo) | Kiểm thử tính giá trên dữ liệu thật |
| 5 | **Đường lưu kết quả tính giá thủ công** (bảng đích, liên kết, trạng thái) — đường cũ đang bị chặn từ 31/07 vì từng nhận số tiền thẳng từ trình duyệt | Lưu kết quả tính giá |
| 6 | **Cho phép chạy GĐ3 (đơn hàng) xen kẽ** hay chờ mở GĐ2 trước | GĐ3 |
| 7 | **Duyệt kế hoạch nạp dữ liệu bản v5** — 9 điểm liệt kê trong kế hoạch | Việc nạp 3 danh mục nền |

---

*Báo cáo nội bộ đầy đủ (kèm vị trí dòng, tên hàm, số đo chi tiết) nằm trong kho mã riêng tư. Bản này chỉ giữ phần công khai được.*
