# BÁO CÁO — NĂM YÊU CẦU GIAO DIỆN OWNER NGÀY 03/09/2026

> **Kho:** báo cáo công khai (bản đã lọc an toàn) · **Ngày lập:** 05/09/2026 (ghi bù, giữ **mốc thật** 03/09/2026 theo `GOV-NOTION-HANDOFF-001` mục 5)
> **Lớp bằng chứng:** `CODE_PROVEN` + `DOC_PROVEN` — **KHÔNG** có `UI_PROVEN` (lý do ở mục 6)
> **Đây là LƯỢT 1** của yêu cầu «đầu bảng cam phải ghim». Owner **BÁC lượt này ngày 04/09/2026** — xem báo cáo `BAO-CAO-CHUAN-HOA-DAU-BANG-TOAN-HE-20260904.md`. Báo cáo này **không** phải bản tin thành công.

---

## 1. Owner yêu cầu gì

Owner gửi ba ảnh chụp màn hình có khoanh đỏ trên trang danh mục **Khách Hàng** của bản vận hành, kèm năm chỉ thị:

| # | Nguyên văn (rút gọn, giữ đúng ý) | Loại |
|---|---|---|
| 1 | *"header màu cam này không được ghim cố định — cần xử lý ngay, đây là 1 tiêu chuẩn trong thiết kế UI"* | TIÊU CHUẨN |
| 2 | *"Mặc định danh sách hiển thị 25 dòng. Mỗi trang là 25 dòng — đây cũng là tiêu chuẩn thiết kế UI"* | TIÊU CHUẨN |
| 3 | *"Sale 33 không trùng khớp với danh sách nhân sự — cần xem kỹ lại"* | LỖI |
| 4 | *"khung đỏ dài chưa tối ưu không gian giống link nhân sự"* | TIÊU CHUẨN |
| 5 | *"các card section và các card khác bo góc quá lớn, tròn quá, không đúng tiêu chuẩn thiết kế UI"* | TIÊU CHUẨN |

Owner nói rõ: **cả bốn điều tiêu chuẩn phải được ghi lại để chuẩn hoá cho mọi màn hình tương tự về sau**, không phải sửa một trang rồi thôi.

---

## 2. Điều đáng kể nhất — chuẩn cũ **có** yêu cầu ghim, nhưng phép nghiệm thu **không bắt được**

Trước 03/09, yêu cầu ghim đầu bảng **chỉ nằm ẩn trong một chuỗi thuộc tính trình bày** của mục «Bảng» trong tài liệu chuẩn giao diện. Nó **không có tên mục riêng**, nên:

- phép dò bằng công cụ tìm chuỗi thấy thuộc tính ghim ⇒ báo **ĐẠT**;
- màn hình thật vẫn **trôi** khi cuộn.

Đây đúng là khoảng cách giữa `CODE_PROVEN` và `UI_PROVEN` mà bộ luật §G1 cảnh báo.

### Nguyên nhân gốc: ghim cần **ba điều kiện đồng thời**, và điều kiện thứ ba bị bỏ sót hoàn toàn

| # | Điều kiện | Bỏ sót thì sao |
|---|---|---|
| 1 | Đầu bảng có định vị dính | Không ghim — dễ thấy, dễ sửa |
| 2 | Có **khung cuộn riêng** bao ngoài bảng | Không có gì để ghim vào |
| 3 | **Nội dung phải THỰC SỰ TRÀN khung đó** | 🔴 Khung không bao giờ cuộn ⇒ người dùng cuộn **thân trang** ⇒ đầu bảng trôi theo, **dù điều kiện 1 và 2 đều đủ** |

Vỏ trang dùng chiều cao tối thiểu bằng màn hình chứ **không khoá** chiều cao, nên **thân trang cuộn được**. Định vị dính chỉ bám theo **khung cuộn gần nhất**; khung trong không cuộn thì nó trôi cùng thân trang.

**Ca đo được — hai trang, cùng công thức khung, khác kết quả:**

| Trang | Công thức khung | Số dòng | Khung có tràn? | Ghim? |
|---|---|---|---|---|
| Nhân Sự | giống hệt | **25** | 25 × ≈49px ≈ 1225px > ≈640px → **CÓ** | ✅ |
| Khách Hàng *(trước sửa)* | **giống hệt** | **10** | 10 × ≈49px ≈ 490px < ≈640px → **KHÔNG** | ❌ |

⇒ Yêu cầu **1** và yêu cầu **2** của Owner **không phải hai việc rời nhau**: đặt lại 25 dòng chính là **điều kiện đủ** để việc ghim chạy được. Đặt về 10 dòng sẽ làm hỏng ghim — **không phải** thay đổi vô hại.

---

## 3. Yêu cầu 3 — «Sale 33» **là lỗi HIỂN THỊ, không phải sai dữ liệu**

Panel chi tiết in số khoá nội bộ đứng cạnh tên người phụ trách. Owner mở trang Nhân Sự đối chiếu thì mã nghiệp vụ cùng con số đó lại là **một người khác** ⇒ đọc ra "gán sai khách hàng cho nhân viên".

Truy nguyên:

- Con số hiển thị là **khoá nội bộ tự tăng** của bảng nhân viên — chính tệp chiếu dữ liệu đã ghi chú rõ *"đây KHÔNG phải khoá tài khoản người dùng"*.
- Mã dạng `NVTP00xx` là **mã nghiệp vụ**, thuộc **hệ đánh số khác hẳn**.
- Hai dãy số **không liên quan nhau** nhưng cùng là số hai chữ số đặt cạnh tên người ⇒ người đọc **buộc phải** hiểu nhầm.

**Dữ liệu trong cơ sở dữ liệu ĐÚNG.** Sai ở chỗ đem khoá nội bộ ra trước mắt người dùng.

**Quy định mới ban hành (mục §19.1 của chuẩn giao diện):**

- ❌ CẤM hiển thị khoá nội bộ ở **bất kỳ đâu người dùng nhìn thấy** — nhãn · thẻ · thông báo · bản in. Trước đây chỉ cấm trong **ô chọn**, nay mở rộng ra **mọi nơi hiển thị**.
- ❌ CẤM mẫu nối chuỗi «khoá — tên»; CẤM dự phòng kiểu «ID: …» khi tra không thấy → dùng dấu gạch ngang.
- ✅ Cần cho người dùng một mã để đối chiếu → hiện **mã nghiệp vụ**, không hiện khoá nội bộ.

---

## 4. Yêu cầu 4 và 5 — cột panel và bo góc

Mục «Panel chi tiết» của chuẩn trước đây chỉ tả **nội dung bên trong** panel, **không ràng buộc cột chứa nó**. Hệ quả: trang Khách Hàng thiếu vỏ bọc cột và thiếu bám dính, nên khung kéo dài hết trang — đúng "khung đỏ dài" Owner khoanh.

**Bốn điều bắt buộc mới của cột panel (mục §10.0), lấy đúng trang Nhân Sự mà Owner chỉ đích danh làm mẫu:**

| # | Bắt buộc | Thiếu thì sao |
|---|---|---|
| 1 | Cột panel có **vỏ bọc riêng** | Ô lưới nở theo nội dung, không co lại được trên màn hẹp |
| 2 | Panel **bám dính khi cuộn** | 🔴 Khung kéo dài hết trang, cuộn là trôi mất |
| 3 | Đổ bóng **vừa** | Nặng nề, chọi mẫu |
| 4 | Hero **gọn** | Ngốn chiều cao vô ích |

Về bo góc: đây **không phải quy định mới** — chuẩn đã ghi từ trước rằng thẻ mục của trang Khách Hàng đang lệch và *"đụng thì đưa về bo góc chuẩn"*. Ngày 03/09 **đang đụng** ⇒ đã đưa **5/5** chỗ về đúng bậc. Trang Nhân Sự đo được **0** chỗ lệch — đó là lý do Owner chỉ nó làm mẫu.

> ⚠️ Chênh **4px** nghe nhỏ, nhưng nhân với năm thẻ xếp dọc thì mắt thấy rõ. Muốn phân định đúng/sai: **mở hai trang cạnh nhau**, đừng đo bằng cảm giác trên một trang.

---

## 5. Đã thay đổi những gì

**Tài liệu chuẩn giao diện — bốn mục, tất cả đều là TIÊU CHUẨN CHUNG, không phải vá riêng một trang:**

| Mục | Nội dung |
|---|---|
| **§8.2** *(mới)* | Đầu bảng phải ghim — nêu đủ **ba điều kiện đồng thời** + **cấm nghiệm thu bằng công cụ tìm chuỗi** |
| **§9** *(sửa)* | Số dòng mặc định = **25 cho MỌI trang**. Bản cũ ghi *"10 cho nhóm này / 25 cho nhóm kia"* — bỏ phân biệt theo nhóm |
| **§10.0** *(mới)* | Bốn điều bắt buộc của cột panel |
| **§19.1** *(mới)* | Cấm rò khoá nội bộ ra mọi nơi hiển thị |

Mọi dòng cũ bị thay đều **giữ nguyên văn** ở mục Lịch sử sửa đổi của chính tài liệu, kèm lý do — theo `GOV-EDIT-PRESERVE-001`.

**Bộ tiêu chí nghiệm thu giao diện:** thêm hai tiêu chí đo được — «số dòng khởi tạo = 25» và «đầu bảng ghim, **CẤM** nghiệm thu bằng công cụ tìm chuỗi».

**Mã nguồn — trang Khách Hàng:** số dòng khởi tạo 10 → **25**; tra tên người phụ trách theo **danh sách nhân sự thật** thay vì in khoá nội bộ (hàm cũ giữ lại, gắn nhãn *ngừng dùng*, không xoá); cột panel thêm vỏ bọc + bám dính + đổ bóng vừa + hero gọn; **5** chỗ bo góc đưa về đúng bậc.

**Số liệu đối chiếu toàn hệ tại thời điểm đo:** **11/14** trang danh mục đã dùng 25 dòng sẵn ⇒ quyết định của Owner **hợp thức hoá điều đa số đang làm**, chỉ còn hai trang lệch (một trang 10 dòng, một trang 50 dòng) → đã ghi **nợ kỹ thuật**, dọn khi đụng module. Ngoài ra **8 trang** không có khung cuộn nên **không thể ghim** và **5 trang** dùng đơn vị chiều cao cũ → ghi nợ riêng.

---

## 6. Điều CHƯA chứng minh được — và vì sao

**Không có ảnh `UI_PROVEN` cho lượt này.** Khoá đăng nhập trong sổ bí mật nội bộ **đã lỗi thời ở cả hai máy**, nên mọi kịch bản chụp ảnh nghiệm thu đều không vào được màn hình. Nợ này đã ghi vào sổ nợ kỹ thuật và **đang chờ Owner**.

⇒ Bằng chứng của báo cáo này dừng ở lớp **mã nguồn + tài liệu**. Theo §G1 của bộ luật, `CODE_PROVEN` **không** đủ để tuyên bố đạt cho một việc giao diện. Đây chính là lý do lượt này **bị Owner bác ngày hôm sau**.

---

## 7. Trạng thái thật

| Việc | Trạng thái |
|---|---|
| Ghi bốn tiêu chuẩn vào tài liệu chuẩn (§8.2 · §9 · §10.0 · §19.1) | ✅ XONG |
| Sửa trang Khách Hàng theo cả năm yêu cầu | ✅ XONG trên máy nội bộ |
| **Nghiệm thu bằng ảnh cuộn thật** | ❌ **KHÔNG LÀM ĐƯỢC** — chặn bởi khoá đăng nhập lỗi thời |
| **Owner nghiệm thu** | 🔴 **BỊ BÁC 04/09/2026** — xem báo cáo tiếp theo |

---

*Bản công khai đã lọc theo `GOV-PUBLIC-SAFE-001`: không mã đăng nhập, không dữ liệu cá nhân, không số liệu kinh doanh, không dấu vết hạ tầng. Mã commit của kho mã nguồn nằm ở sổ nội bộ, không đưa ra kho công khai.*
