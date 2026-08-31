# MÀN PHÂN QUYỀN — LƯỢT BA, VÀ VÁ LỖ TRONG CHÍNH CỔNG NGHIỆM THU

**Phiên bản:** `V1.00.363` · **Ngày:** 31/08/2026
**Gói việc:** `WP-ERP-M0-PERMISSION-UX-FINAL-CLOSEOUT-20260829` — CONTINUATION R2
**Trạng thái:** kỹ thuật đạt · **đang chờ Owner nghiệm thu**

---

## 0. MỘT DÒNG

> Màn phân quyền bị Owner bác **lần thứ ba**. Luật buộc dừng cách cũ. Owner ra đặc tả đầy đủ,
> khoá được gỡ, và lượt này sửa **bốn việc — mỗi việc xuất phát từ một số đo thật trên trình
> duyệt, không từ suy đoán**. Quan trọng hơn cả: tìm ra **vì sao cổng nghiệm thu báo 32/32 ĐẠT
> mà Owner vẫn thấy khó dùng** — và vá lỗ đó.

---

## 1. VÌ SAO PHẢI DỪNG CÁCH CŨ

Luật nội bộ `GOV-ITERATION-LIMIT-001` quy định: cùng một yêu cầu bị bác **lần thứ ba** thì
phải **dừng cách làm cũ**, báo lại đã thử gì, và đề xuất cách khác **về bản chất**.

| Lượt | Đã làm | Kết quả |
|---|---|---|
| 1 | Làm lại năm bước thành ba pha | Bác |
| 2 | Bỏ ba thẻ thống kê kỹ thuật, thêm trang hướng dẫn | Bác |
| 3 | — | *"vẫn cảm giác khó dùng, khó nhìn, khó hiểu lắm"* |

**Cách bị bác, chung cả ba lượt:** tự thiết kế lại màn theo ý mình, rồi chạy cổng kiểm tự động
và lấy kết quả ĐẠT làm bằng chứng.

**Đổi cách — đổi lớp bằng chứng.** Thay vì tin cổng, lượt này dựng hai công cụ đo mới, chạy trên
trình duyệt thật, chụp ảnh từng bước:

- **đối chiếu sáu màn** — ba màn Owner đã duyệt và đang dùng hàng ngày, đặt cạnh ba màn đang bị
  phàn nàn, đo bằng **cùng một thước**;
- **đi trọn hành trình** — từ lúc mở màn tới lúc chạm nút Áp Dụng, đo sau **từng cú bấm**.

---

## 2. PHÁT HIỆN QUAN TRỌNG NHẤT — CỔNG XANH MÀ NGƯỜI DÙNG VẪN KHỔ

Công cụ "đi trọn hành trình" cho kết quả này:

| Bước | Chữ kỹ thuật lộ ra |
|---|---|
| Vừa mở màn (0 lần bấm) | **0** |
| Sau khi chọn một người | 0 |
| **Sau khi chọn việc (2 lần bấm)** | **4** |

Cổng nghiệm thu **có** điều kiện *"không dùng mã vai trò làm nhãn chính"* — nhưng nó **chỉ chạy
trên ảnh chụp màn lúc vừa mở**. Bấm vào rồi thì không phép kiểm nào soi nữa.

Đó chính là lời giải cho nghịch lý *"cổng báo đạt mà Owner vẫn bác"*.

### Vá — và hai lần kiểm ngược thất bại trước khi đạt

Thêm hai phép kiểm mới cho các bước sau. Rồi **kiểm ngược**: thả lại thứ vừa gỡ, cổng **phải đỏ**.

- **Lần 1 — vẫn xanh.** Truy ra: phép đo cũ lấy chữ của từng phần tử rồi **bỏ qua phần tử dài hơn
  90 ký tự**. Thẻ vai trò là một nút chứa cả tên + mô tả + dòng mã ⇒ dài hơn 90 ⇒ **bị loại khỏi
  phép quét**. Cổng mù trước đúng ca cần bắt. Thêm phép đo quét **toàn văn** màn.
- **Lần 2 — vẫn xanh.** Truy ra: cổng chỉ đi **một trong ba đường** cấp quyền, mà mầm lại thả ở
  đường khác.
- **Lần 3 — đỏ đúng 2 điều kiện.** Gỡ mầm ra ⇒ xanh lại **34/34**.

> Nếu dừng ở lần 1 thì đã báo "đã vá" trong khi cổng vẫn mù. **Kiểm ngược là thứ ngăn điều đó.**

Việc "cổng chỉ đi một trong ba đường" được ghi thành một khoản nợ riêng, **không giấu**.

---

## 3. BỐN VIỆC ĐÃ SỬA

### 3.1 Bỏ dải "Ba bước" ở đầu màn

Đo được: trước khi tới việc thật, người dùng phải đọc qua **ba lớp hướng dẫn chồng nhau** — mô tả
trang, dải này, và một dòng **lặp lại đúng nội dung dải này**. Ba lần nói cùng một điều.

Dải chiếm nguyên một tầng mà **không bấm được**. Ba màn mẫu Owner đã duyệt **không màn nào có lớp
hướng dẫn nào** — vào là thấy dữ liệu ngay.

Nút "Hướng Dẫn Phân Quyền" **được giữ** (Owner yêu cầu đích danh), nay nhập vào thanh phía dưới,
góc phải — đúng chỗ ba màn mẫu đặt nút hành động.

### 3.2 Gỡ mã vai trò khỏi thẻ chọn

Ảnh chụp cho thấy danh sách bày *"mã ..."* **tám lần** trên một màn, ở đúng bước người dùng phải
ra quyết định. Cỡ chữ nhỏ và màu nhạt không đủ để gọi là "chi tiết phụ" khi nó lặp tám lần.

Mã **vẫn tra được** ở khu Quản Lý Nâng Cao — chỗ dành cho người cần nhìn cấu hình kỹ thuật.

### 3.3 Màn đầu nay nói đủ bốn điều

Trước: chỉ có tên, số màn dùng được, danh sách vai trò. Nay thêm:

| Thêm gì | Lấy từ đâu |
|---|---|
| Trạng thái tài khoản (đang dùng · bị khoá · chờ kích hoạt) | dữ liệu tài khoản thật |
| Hồ sơ nhân sự đã liên kết + chức danh | nối sang bảng nhân sự |
| **Số quyền nhạy cảm đang giữ** | đếm qua danh mục quyền — thêm quyền mới vào danh mục là số này tự đúng |
| **Việc nên làm tiếp** | sinh từ dữ liệu thật, **không** viết cứng theo tên người |

### 3.4 Địa chỉ thư điện tử chỉ hiện khi trùng tên

Trước, mỗi thẻ đều bày một dòng thông tin kỹ thuật; sáu thẻ tốn sáu dòng mà chín phần mười không
ai cần. **Không bỏ hẳn** — vì đó là thứ duy nhất phân biệt hai người **trùng tên**, và cấp nhầm
quyền cho người trùng tên là hỏng thật.

---

## 4. SỐ ĐO TRƯỚC / SAU

| Chỉ số | Trước | Sau |
|---|---|---|
| Chữ kỹ thuật lộ ra, bước "chọn việc" | **4** | **0** |
| Chữ kỹ thuật, tính trên cả bảy bước | 4 | **0** |
| Chiều cao màn ở bước "chọn việc" | 1 736px | **1 550px** |
| Chiều cao sau khi chọn người | 1 220px | **1 128px** (hết phải cuộn) |
| Số điều kiện cổng nghiệm thu | 32 | **34** |
| Vùng ở tầng đầu màn | 4 | giảm một tầng |

---

## 5. BẢNG KIỂM

| Phép kiểm | Kết quả |
|---|---|
| Dựng bản phát hành | ĐẠT |
| Kiểm kiểu toàn dự án | sạch |
| Cổng nghiệm thu giao diện | **34 / 34** |
| Cổng chặn ghi tắt tầng bảo mật | ĐẠT — 0 câu ghi đi vòng |
| Cổng kiểm kiểu kịch bản phát hành | ĐẠT — 15 tệp, 0 lỗi |
| Chín cổng chuỗi bán hàng | **9 / 9** |
| Quét thông tin nhạy cảm | ĐẠT |
| Quét dữ liệu cá nhân | ĐẠT |
| Đi trọn hành trình, bảy bước | 0 chữ kỹ thuật ở **mọi** bước |
| Dọn dữ liệu thử | sạch — mốc nền trở về nguyên vẹn |

**Một ghi nhận đáng nói:** cổng quét dữ liệu cá nhân đã **chặn commit** vì một tệp dựng tự sinh
lọt vào kho (chuỗi số trong đó bị nhận nhầm là số giấy tờ). Cổng làm đúng việc — tệp đó không nên
vào kho. Đã gỡ và chặn vĩnh viễn.

---

## 6. ĐIỀU CHƯA ĐƯỢC CHỨNG MINH

Phần này **cố ý** liệt kê, để không ai đọc báo cáo mà hiểu quá điều đã làm:

- **Chưa có kết luận là đã dễ dùng.** Đó là điều chỉ Owner nói được. Báo cáo này **không** tự ghi
  đạt nghiệm thu.
- **Cổng chỉ nghiệm một trong ba đường cấp quyền.** Hai đường còn lại chưa có phép kiểm nào sau
  bước chọn việc — đã ghi nợ riêng, không giấu.
- Số đo chụp trên máy phát triển. Bố cục giống máy vận hành vì cùng mã nguồn, nhưng **số liệu
  khác** — đã ghi nợ từ trước về việc dữ liệu hai môi trường trôi khỏi nhau.

---

## 7. VIỆC KẾ TIẾP — ĐÚNG MỘT

**Owner nghiệm thu màn phân quyền trên máy vận hành.**

Ba tình huống cần thử, mỗi cái chừng một phút:

1. **Cấp nhanh theo vai trò** — chọn một người, chọn một vai trò mẫu, xem hệ quả, áp dụng.
2. **Cấp thêm quyền riêng** — giữ nguyên quyền đang có, cấp thêm phần mới cho riêng người đó.
3. **Thu hẹp / thay mẫu** — bớt quyền; màn phải nói rõ **quyền nào sẽ mất**.

Câu hỏi duy nhất cần Owner trả lời:

> **Giao diện này đã đủ trực quan để tự phân quyền mà không cần hiểu kỹ thuật phân quyền chưa?**

---

*Báo cáo công khai — không chứa mã nguồn, không chứa thông tin đăng nhập, không chứa dữ liệu cá
nhân, không chứa bản sao cơ sở dữ liệu.*
