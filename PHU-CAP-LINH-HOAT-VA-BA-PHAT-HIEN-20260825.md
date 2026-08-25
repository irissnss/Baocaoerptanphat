# PHỤ CẤP LINH HOẠT + BA PHÁT HIỆN LỚN VỀ TÍNH GIÁ — 25/08/2026

> **Bản công khai đã lọc.** Không chứa mã nguồn · dữ liệu khách hàng · email · IP · cổng máy chủ.
> **Plan:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`
>
> ⚠️ **ĐÂY LÀ BÁO CÁO PUSH BÙ.** Hai đợt việc dưới đây hoàn tất lúc **20:39** và **21:05** ngày 25/08 nhưng **chưa được đẩy lên kho công khai** đúng lúc. Chủ dự án hỏi lại mới phát hiện. Mốc thật giữ nguyên, **không sửa cho khớp ngày đẩy**.

---

## PHẦN A — PHỤ CẤP KHỔ TRẢI THÀNH DANH SÁCH MỞ *(hoàn tất 20:39)*

### A.1 Chủ dự án yêu cầu gì

> *"cái này có thể linh hoạt thêm mục phụ cấp nào tùy chỉnh chứ em nhỉ"*

### A.2 Đã đổi mô hình

**Trước:** bốn trường cứng — mép dán, chừa xén, nắp, bù hao. Cứng nhắc, không thêm được.

**Sau:** **danh sách mở**. Mỗi khoản khai: tên · giá trị · **áp vào đâu** · có tính mỗi bên không.

Bốn kiểu "áp vào" — bắt buộc khai, vì tấm trải có **hai chiều** và bù hao thì **không cộng vào kích thước** mà cộng vào **số tờ giấy**:

| Áp vào | Nghĩa | Ví dụ |
|---|---|---|
| Bề ngang | cộng vào chiều ngang tấm trải | mép dán |
| Bề dọc | cộng vào chiều dọc | nắp |
| Cả hai chiều | cộng vào cả hai | chừa xén |
| Lượng giấy (%) | không đụng kích thước, cộng thêm số tờ | bù hao |

Thêm ô **"nhân đôi"** cho khoản tính *mỗi bên* — chừa xén 3mm mỗi bên thành 6mm mỗi chiều.

### A.3 Chứng minh mô hình không làm mất gì

Kiểm bằng máy: mô hình mới **phủ được cả hai công thức đang có trong hệ thống**.

- Đặt mép dán 15 + chừa xén 3 (mỗi bên) + nắp 30 → ra **đúng công thức A** đang chạy
- Đặt mép dán 40 + nắp 20 + chừa xén 0 → ra **đúng công thức B** *(đường tiền thật)*

⇒ Đổi mô hình **không đổi hành vi**, chỉ mở rộng khả năng.

### A.4 Bộ kiểm

**56 đạt / 0 hỏng.** Gồm:

- Thêm khoản mới vào **bề ngang** → chỉ bề ngang đổi
- Thêm khoản vào **bề dọc** → chỉ bề dọc đổi
- Thêm khoản **cả hai chiều + nhân đôi** → cả hai chiều tăng gấp đôi giá trị
- **Thêm 20 khoản** → cộng đủ 20mm ⇒ **không giới hạn số khoản**
- Bù hao 10% → **số tờ tăng đúng 10%**, kích thước **không đổi**
- Bắt 6 loại nhập sai: mã trùng · mã có dấu · giá trị âm · thiếu tên · phần trăm quá 100 · dùng "nhân đôi" sai chỗ

### A.5 Giới hạn đã khai với Chủ dự án

Mô hình phủ khoản **cộng thêm một lượng cố định** (mm hoặc %). Khoản mà giá trị **phụ thuộc kích thước hộp** — kiểu *"nắp bằng một nửa chiều rộng"* — là **công thức**, không phải hằng số ⇒ **chưa phủ**.

> 🔴 **Về sau chứng minh giới hạn này là vấn đề THẬT.** Tài liệu công thức gốc của Tân Phát cho thấy `Rộng÷2` và `Cao÷2` xuất hiện ở **hầu hết** kiểu dáng — không phải trường hợp hiếm.

---

## PHẦN B — BA PHÁT HIỆN LỚN *(hoàn tất 21:05)*

### B.1 🔴 Màn cấu hình **đã tồn tại** — suýt dựng trùng

Chủ dự án yêu cầu *"dựng màn cấu hình"*. Đã soạn xong gói nghiệm thu để dựng mới. Nhưng khi đo lại theo hướng Chủ dự án chỉ, phát hiện:

**Màn "Tính Giá Admin" đã tồn tại đầy đủ** — có route thật, có trong menu, có cổng phân quyền, và có sẵn trình soạn Blueprint với **5 tab**: Tham Số · **Bố Cục** · **Bình Bản** · Xem Trước · JSON.

Tab **Bố Cục** đã có sẵn đúng hai ô cần: *Công Thức Chiều Rộng* và *Công Thức Chiều Cao*. Tab **Bình Bản** đã có *Chế Độ*, *Khoảng Cách Tối Thiểu*, và cả **Cho Phép Xoay**.

**Vì sao nó vô dụng:** bảng blueprint chỉ có **1 dòng demo rỗng**, hàm đọc công thức có **0 nơi gọi**, và 6 chỗ tính khổ trải đều viết cứng.

> Có nhà, có ổ khoá, có chìa — **nhưng không ai nối dây điện**.

⇒ Việc thật là **NỐI + NẠP DỮ LIỆU**, không phải dựng màn. Dựng mới sẽ **tạo trùng**, vi phạm nguyên tắc *ưu tiên tái dùng*.

### B.2 🔴 Một công thức đang áp cho **40 nhóm sản phẩm**

Đo danh mục nhóm sản phẩm: **40 nhóm, hai tầng**.

- **Tầng trên** — loại lớn: Hộp Mềm · Thùng Carton · Catalog · Lịch · Sổ Tay
- **Tầng dưới — chính là KIỂU DÁNG**: Hộp Mềm Thẳng · Hộp Mềm Khoá Đáy · Hộp Mềm Đáy Dán · Hộp Carton Nắp Cài · Hộp Carton Nắp Xếp Đậy · Hộp Carton Âm Dương · Hộp Cứng Nam Châm · Hộp Cứng Dạng Rút · Lịch Treo 13 Tờ · Lịch Block 365 Ngày…

⇒ Chủ dự án nói đúng nguyên văn: *"nhóm sản phẩm liên quan đến kiểu dáng sản phẩm"*.

Nhưng cả **6 chỗ** tính khổ trải trong mã đều dùng **một công thức**, còn tệp vẽ hình tự khai ngay dòng 13 là *"SVG Mockup cho **Hộp Nắp Cài**"*.

⇒ **Một công thức của một kiểu dáng đang áp cho cả 40 nhóm.** Lịch treo, catalog, sổ tay — không phải hộp — vẫn đi qua đúng công thức hộp nắp cài đó.

### B.3 🟠 Bốn khái niệm Chủ dự án nêu **chưa có chỗ đứng nào**

| Chủ dự án nêu | Hiện trạng đo được |
|---|---|
| Tối ưu khổ | Quét **101 bảng**: không cột nào. Không có trong mã |
| Tối ưu chi phí máy in | Có bảng phiếu điều in với máy lớn/nhỏ, số mặt, số màu, số kẽm, khuôn — nhưng **chỉ 2 dòng** và **không nối vào tính giá** |
| Tờ rớt | Chỉ xuất hiện ở **một tệp hướng dẫn**, không ở bất kỳ chỗ tính toán nào |
| Lượt in | Có dạng giá theo **số màu** — nhưng khác định nghĩa Chủ dự án đưa |

⇒ Bốn khái niệm này hiện là **kiến thức trong đầu Chủ dự án**, chưa thành dữ liệu hay quy tắc trong hệ thống.

---

## PHẦN C — BỐN QUYẾT ĐỊNH CHỦ DỰ ÁN CHỐT

| Việc | Chủ dự án chốt |
|---|---|
| **Lịch sử kho mã** | **Giữ nguyên**, chấp nhận rủi ro. Không xoá, không viết lại. Ba loại vẫn phải bảo vệ: mật khẩu · địa chỉ IP · khoá API |
| **Quản lý tài khoản** | Yêu cầu **mới**: xây giao diện quản lý bài bản + phân quyền + trang người dùng tự quản lý *(đổi mật khẩu · ảnh đại diện · bí danh; **không** cho sửa tên/email/tên đăng nhập)* |
| **Bản demo trước khi code** | **CÓ** — mọi màn mới phải có bản demo cho Chủ dự án xem trước |
| **Đơn vị cấu hình** | **Bác** cả hai phương án đề xuất. Phải theo **nhóm sản phẩm / kiểu dáng sản phẩm**. Đồng thời yêu cầu xem lại: tối ưu khổ · chi phí máy in · tờ rớt · lượt in |

---

## PHẦN D — MỘT VIỆC LÀM SAI, TỰ KHAI

Báo cáo này lẽ ra phải được đẩy lên **ngay sau hai đợt việc**, lúc 20:39 và 21:05. Nó không được đẩy. Chủ dự án hỏi *"mỗi đợt đã push báo cáo hết chưa?"* mới phát hiện.

**Hai nguyên nhân, cả hai là lỗi quy trình:**

1. Đợt 20:39 gộp hai commit vào một lời khai *"không cần đẩy — tài liệu nội bộ"*. Lời khai đó **đúng cho gói nghiệm thu**, nhưng **sai cho commit mã nguồn** đi kèm.
2. Đợt 21:05 **không xuất khối báo cáo kết thúc** — nên trường *"đã đẩy công khai chưa"* **không được hỏi tới**, và việc thiếu **trôi qua im lặng**.

**Mốc thật giữ nguyên** trong báo cáo này, không sửa cho khớp ngày đẩy bù.

---

*Mọi con số tự đo trong lượt làm việc tương ứng. Bản đầy đủ kèm bằng chứng chi tiết nằm ở kho riêng tư.*
