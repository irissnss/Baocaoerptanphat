# BÁO CÁO — ĐẦU BẢNG CAM: TỪ "GHIM NHƯNG TRONG SUỐT" ĐẾN MỘT CHUẨN DUY NHẤT CHO TOÀN HỆ (04/09/2026)

> **Kho:** báo cáo công khai (bản đã lọc an toàn) · **Ngày lập:** 05/09/2026 (ghi bù, giữ **mốc thật** 04/09/2026 theo `GOV-NOTION-HANDOFF-001` mục 5)
> **Lớp bằng chứng:** `CODE_PROVEN` cho phần áp dụng · `UI_PROVEN` cho **phần chứng minh cơ chế** (đo màu điểm ảnh trong trình duyệt thật) · `RUNTIME_PROVEN` cho phần phát hành
> **Đây là LƯỢT 2 và LƯỢT 3** của cùng một yêu cầu «đầu bảng cam phải ghim và phải đọc được». Owner **bác hai lần** trong ngày. Báo cáo này ghi trung thực cả hai lượt bác, theo `GOV-FAILURE-RECORD-001`.

---

## 1. Diễn biến trong ngày — bốn lượt trao đổi

| Lượt | Owner nói | Trạng thái thật |
|---|---|---|
| 1 | *"header cam có ghim nhưng mảng màu có vẻ trong suốt làm mất hiển thị"* (kèm ảnh đã cuộn) | 🔴 **BỊ BÁC** — bản vá 03/09 làm đúng việc ghim nhưng sinh ra lỗi mới |
| 2 | *"Vẫn còn nha em"* | 🔴 **BỊ BÁC LẦN HAI** — `lần_lặp` = 2 |
| 3 | *"Local thấy ghim rồi còn vps thì chưa cần deploy hết đi chứ em"* | ✅ Owner **duyệt phát hành** — cũng chính là lời giải cho lượt 2 |
| 4 | *"anh đã nói là tiêu chuẩn xử lý UI rồi mà giờ hỏi là sao em? … còn gradient phải chuẩn chỉnh nhất quán nha em"* | 🔴 **CHẤN CHỈNH cách làm việc** — xem mục 5 |

---

## 2. Vì sao "ghim rồi mà vẫn trong suốt" — nguyên nhân gốc

Bản vá 03/09 đặt **dải màu chuyển sắc trên hàng tiêu đề**, nhưng đặt **thuộc tính ghim trên từng ô tiêu đề**. Hai thứ nằm ở **hai hộp khác nhau**.

**Nền là thuộc tính sơn của hộp nào thì trôi theo hộp đó.** Khi cuộn:

- hàng tiêu đề (hộp mang dải màu) **không được ghim** ⇒ trôi lên khỏi tầm nhìn, **mang theo dải cam**;
- các ô tiêu đề **được ghim** ⇒ đứng yên, nhưng chúng là **hộp rỗng, không có nền**;
- ⇒ chữ tiêu đề đứng yên trên **nền trắng**, và **dòng dữ liệu chạy xuyên qua** phía dưới.

Đúng như Owner mô tả: *"mảng màu có vẻ trong suốt làm mất hiển thị"*.

### Đã chứng minh bằng đo đạc, không phải suy luận — `UI_PROVEN`

Dựng một trang thử tối giản sao đúng điều kiện của dự án (mô hình viền gộp mặc định của bộ khung trình bày), cuộn 90px, rồi **đo màu điểm ảnh ngay giữa dải đầu bảng** trong trình duyệt thật:

| Cách dựng | Màu đo được | Kết luận |
|---|---|---|
| Ghim đặt trên **hàng tiêu đề** mang dải màu *(chuẩn)* | cam | ✅ **KÍN** |
| Ghim đặt trên **ô tiêu đề** + ô có nền đặc | cam | ✅ kín, nhưng **mất dải chuyển sắc** |
| Ghim đặt trên **ô tiêu đề**, ô **không** có nền *(tổ hợp bị cấm)* | **trắng** | ❌ **TRONG SUỐT**, thấy thẳng dòng dữ liệu |

⇒ Nghi ngại *"ghim trên hàng tiêu đề không chạy được với mô hình viền gộp"* — **đã bị bác bỏ bằng số đo**. Đây là cách duy nhất vừa **kín** vừa **giữ được dải chuyển sắc**.

### Một hướng đi đã được cân nhắc rồi **loại bỏ**

Có phương án đổi bảng sang **mô hình viền tách rời** để né vấn đề. Đã đưa ra hội đồng thẩm định độc lập — **3/3 phiếu bác**: mô hình viền tách rời **bỏ qua đường viền khai trên hàng**, tức **xoá sạch đường kẻ giữa các dòng** của bảng, trong khi phép dò bằng công cụ tìm chuỗi vẫn thấy thuộc tính viền còn nguyên ⇒ **hỏng âm thầm**. Đã ghi lệnh cấm vào tài liệu chuẩn để phiên sau không thử lại.

---

## 3. Vì sao Owner bác **lần hai** — không phải lỗi bản vá

Lượt 2 (*"Vẫn còn nha em"*) khiến việc rà lại toàn bộ bản vá. Kết quả truy nguyên: **bản vá ĐÚNG, nhưng chỉ mới nằm trên máy nội bộ.** Bản đang chạy trên máy vận hành vẫn là bản cũ. Owner nhìn màn hình vận hành nên **thấy y nguyên lỗi cũ** — hoàn toàn hợp lý.

Bài học ghi lại: **"đã sửa" ≠ "Owner nhìn thấy đã sửa"**. Với việc giao diện, chưa phát hành thì chưa có gì để nghiệm thu.

---

## 4. Hai điểm chặn của dây chuyền phát hành — vá gốc

Sau khi Owner duyệt phát hành, dây chuyền **thất bại hai lần liên tiếp**. Cả hai đều là lỗi gốc của công cụ, không phải lỗi bản vá:

| # | Triệu chứng | Nguyên nhân gốc | Cách vá |
|---|---|---|---|
| 1 | Máy vận hành trả lỗi đường dẫn có ký tự lạ ở cuối | Chuỗi lệnh soạn trên máy Windows mang **ký tự xuống dòng kiểu Windows**; máy vận hành dùng kiểu khác nên đọc ký tự thừa thành **một phần của đường dẫn** | Chuẩn hoá ký tự xuống dòng trước khi gửi lệnh đi |
| 2 | Cổng an toàn chặn vì không khai rõ đợt này có nạp dữ liệu hay không | Kịch bản phát hành **không có đường khai tường minh** cho trường hợp *"đợt này không cần nạp dữ liệu"* | Thêm đường khai tường minh; chỉ khai sau khi **đã kiểm chứng** không có bước di trú và không đụng lớp lưu trữ |

> ✅ Lần thất bại thứ nhất **an toàn**: dây chuyền dừng **trước khi** chạm vào bất cứ thứ gì trên máy vận hành.

Sau hai bản vá, phiên bản **V1.00.369** phát hành thành công.

---

## 5. Chấn chỉnh của Owner — và điều tôi làm sai

Owner nêu **hai lỗi** trong cách làm việc của tôi:

> *"anh đã nói là tiêu chuẩn xử lý UI rồi mà giờ hỏi là sao em?"*

**Lỗi 1 — hỏi lại một điều đã là tiêu chuẩn.** Tiêu chuẩn đã chốt thì **áp cho mọi trang liên quan**, không hỏi từng trang một. Quy định *"không quét giao diện diện rộng"* chặn **đợt quét tự phát**, **không** chặn việc thi hành một tiêu chuẩn Owner đã khoá.

> *"trang đây chứ đây mà hỏi sau phát hành là gì làm mà không biết trang nào sao?"*

**Lỗi 2 — bắt Owner tự đi kiểm.** Việc mở trang, cuộn thử và đối chiếu là **việc của tôi**, không phải việc của Owner.

Và chỉ thị thứ ba, thành tiêu chuẩn mới:

> *"còn gradient phải chuẩn chỉnh nhất quán nha em."*

Cả ba đã ghi vào sổ yêu cầu Owner nội bộ.

---

## 6. Điều phát hiện khi thi hành «gradient phải nhất quán» — **dải màu là MÃ CHẾT ở bốn trang**

Rà toàn hệ theo chỉ thị trên, phát hiện điều không ai ngờ:

**Bốn trang — trong đó có chính TRANG MẪU VÀNG được cả dự án lấy làm chuẩn — đang phủ một lớp nền cam ĐẶC lên từng ô tiêu đề.** Lớp nền đặc đó **che kín** dải chuyển sắc khai ở hàng tiêu đề.

⇒ Dải chuyển sắc ở bốn trang đó là **mã chết, chưa bao giờ hiện ra**. Bốn trang đang hiển thị **cam phẳng**, không phải chuyển sắc — **kể cả trang mẫu vàng**.

Nghĩa là: ai chép theo trang mẫu vàng để dựng màn mới đều **chép lại một khiếm khuyết**, suốt một thời gian dài mà không ai biết.

### Thứ tự dọn **bắt buộc**, và điều CẤM tuyệt đối

1. chuyển thuộc tính ghim từ ô tiêu đề **lên hàng tiêu đề**;
2. **rồi mới** gỡ lớp nền đặc khỏi ô tiêu đề;
3. nghiệm thu bằng **ảnh cuộn thật**.

🔴 **CẤM làm mỗi bước (2).** Gỡ nền mà để ghim nguyên ở ô tiêu đề là **tái tạo đúng lỗi Owner vừa báo** — và lần này sẽ hỏng **bốn trang đang lành**, gồm cả trang mẫu vàng.

> Dòng nợ cũ trong tài liệu chuẩn chỉ ghi *"bỏ nền trên ô tiêu đề"* — một **mệnh lệnh nửa vời**. Ai thi hành đúng chữ đó sẽ làm hỏng bốn trang. Dòng này **đã được viết lại đủ ba bước**, dòng cũ giữ nguyên văn ở mục lịch sử.

---

## 7. Kết quả — toàn hệ về **một cách dựng duy nhất**

**Chín trang đã sửa:** Khách Hàng · Sản Phẩm · Nhân Sự · Kho Thành Phẩm *(trang mẫu vàng)* · Phòng Ban · Vị Trí · Công Đoạn · Đơn Vị Tính · Địa Chỉ.

**Cách dựng chuẩn, áp cho mọi bảng danh mục:**

- dải chuyển sắc **ba chặng** đặt trên **hàng tiêu đề**;
- thuộc tính ghim đặt trên **chính hàng tiêu đề đó**;
- ô tiêu đề **không nền, không ghim**.

**Số đo sau khi dọn (quét toàn bộ mã giao diện):**

| Phép đo | Kết quả |
|---|---|
| Ghim còn đặt nhầm trên ô tiêu đề | **0** |
| Nền đặc còn phủ lên ô tiêu đề | **0** |
| Dải chuyển sắc lệch chuẩn | **0** |
| Tổng số trang đang dùng đúng dải chuẩn | **18** |

---

## 8. Tài liệu chuẩn — vá cả nơi **đang dạy sai**

Điều nghiêm trọng phát hiện trong lượt này: **chính tài liệu chuẩn đang dạy đúng tổ hợp gây lỗi.** Mục «Bảng» viết thuộc tính ghim **nằm trong chuỗi thuộc tính của ô tiêu đề** — tức bất kỳ ai làm đúng theo tài liệu đều dựng ra màn hình trong suốt.

Đã vá **bốn vị trí trong một lượt** theo `GOV-EDIT-PRESERVE-001` mục 2 (quét mọi vị trí cùng chủ đề, sửa hết cùng lúc):

| Mục | Sửa gì |
|---|---|
| **§2** | Thêm ràng buộc: dải màu **và** thuộc tính ghim phải **cùng nằm trên hàng tiêu đề**; cấm tổ hợp tách rời; cấm chuyển sang mô hình viền tách rời |
| **§8** | Gỡ thuộc tính ghim khỏi chuỗi thuộc tính của ô tiêu đề — nơi đang dạy sai |
| **§8.2** | Thêm **điều kiện thứ 4**: *nền phải thuộc chính hộp được ghim*, kèm bảng đo màu điểm ảnh |
| **§16** | Viết lại dòng nợ nửa vời thành ba bước đúng thứ tự + lệnh cấm |

> ⚠️ Điều kiện 4 **chỉ lộ ra SAU KHI** điều kiện 3 được thoả. Bản vá 03/09 (nâng lên 25 dòng) chính là thứ **phơi ra** một khiếm khuyết vốn nằm sẵn từ trước — không phải nó tạo ra lỗi mới.

---

## 9. Còn nợ

| Nợ | Nội dung |
|---|---|
| Chiều cao ô tiêu đề | Ba trang cuối vẫn dùng chiều cao cũ thay vì chiều cao chuẩn — dọn khi đụng module |
| **Ảnh nghiệm thu** | ❌ **Vẫn chưa có `UI_PROVEN` trên chính các trang của hệ thống** — khoá đăng nhập trong sổ bí mật nội bộ đã lỗi thời ở cả hai máy. Số đo `UI_PROVEN` ở mục 2 là đo **cơ chế** trên trang thử, **không phải** đo trang thật |

---

## 10. Trạng thái thật

| Việc | Trạng thái |
|---|---|
| Truy nguyên "ghim nhưng trong suốt" | ✅ XONG — có số đo màu điểm ảnh |
| Loại bỏ hướng đi sai (mô hình viền tách rời) | ✅ XONG — 3/3 phiếu bác, đã ghi lệnh cấm |
| Vá hai điểm chặn dây chuyền phát hành | ✅ XONG |
| Phát hành **V1.00.369** lên máy vận hành | ✅ XONG |
| Chuẩn hoá 9 trang về một cách dựng duy nhất | ✅ XONG — số đo 0 / 0 / 0 |
| Vá 4 vị trí tài liệu chuẩn đang dạy sai | ✅ XONG |
| **Ảnh cuộn thật trên trang hệ thống** | ❌ **CHẶN** — chờ Owner cập nhật khoá đăng nhập |

---

*Bản công khai đã lọc theo `GOV-PUBLIC-SAFE-001`: không mã đăng nhập, không dữ liệu cá nhân, không số liệu kinh doanh, không dấu vết hạ tầng. Mã commit của kho mã nguồn nằm ở sổ nội bộ, không đưa ra kho công khai.*
