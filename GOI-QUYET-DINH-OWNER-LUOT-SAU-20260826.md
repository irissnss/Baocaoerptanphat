# GÓI QUYẾT ĐỊNH CHỦ DỰ ÁN — LƯỢT SAU · 26/08/2026

> **Đã đọc hết bằng chứng trước khi hỏi.** Trong **10 câu** lượt trước, **6 câu đã tự giải đáp được** từ Notion — không hỏi lại. Chỉ **4 câu** thật sự cần Chủ dự án, cộng **2 mâu thuẫn** phải phân xử.
> **Nguồn đã đọc:** Notion *Smart Offset Master* (Decision D7 · D9 · SSOT giá thật) · *CASE STUDY Túi Xách Giấy Có Dây* · `Test Tính Khổ Trải Tân Phát 19082025.xlsx` · `Smart_Offset_Master_Technical_Spec.md` · CSDL đọc-thuần.

---

## PHẦN 0 — SÁU CÂU ĐÃ TỰ GIẢI ĐÁP, **KHÔNG HỎI LẠI**

| Câu lượt trước | Trả lời tìm được | Nguồn |
|---|---|---|
| **Đơn vị cm hay mm?** | **MM-ONLY**, Chủ dự án đã chốt: *"`dm_san_pham.kich_thuoc` chốt MM-ONLY (không dùng cm, không số thập phân, không auto-convert)"*. Case study dùng `310 × 100 × 280mm`. Dữ liệu cũ nhập cm phải chuẩn hoá về mm **trước khi** bật tính giá thật | Notion *Smart Offset Master* |
| **Mốc 3.000 áp thế nào?** | `T = threshold_qty` nằm trong `dm_bang_gia_cong_doan.cach_tinh`, **chọn theo `press_group` (SMALL/LARGE)** — tức **có thể khác theo máy**, và **cấm viết cứng 3000 trong mã** | Decision D7 · D9 |
| **Tờ rớt có đơn giá riêng?** | **CÓ.** `overage_per_sheet_per_color`: có **đơn giá riêng theo máy nhỏ / máy lớn** *(giá trị thật ở kho riêng tư — không nêu ở bản công khai)* | Decision D9 |
| **7.000 tờ tách thế nào?** | **1 lượt + phần vượt**, không phải 2 lượt. `overage = (số tờ − 3000) × đơn giá × số màu` | Decision D9 |
| **"Đơn giá" là giá gì?** | **Hai đơn giá riêng biệt**: `plate_price` (giá **1 tấm kẽm**) và `flat_price` (giá **chạy máy trọn lô ≤3000 tờ**) | Decision D9 |
| **Kẽm và công in — một hay hai khoản?** | **HAI thành phần**: *Kẽm (Plate)* + *Chạy máy (Run)*, nhưng **một dòng cấu hình duy nhất** trong bảng giá công đoạn, logic nằm trong JSON | Decision D9 |

**Máy in lớn/nhỏ cũng đã có SSOT:** phân loại **tự động theo khổ in**, không lộ cho nhân viên bán hàng — `SMALL` khi cạnh lớn ≤720mm **và** cạnh nhỏ ≤520mm; `LARGE` khi ≤1020 × ≤720; vượt cả hai thì **từ chối**.

**Số kẽm cũng đã chốt:** 1 mặt / 2 mặt tự trở / 2 mặt trở nhíp → `K = số màu`; **2 mặt A-B → `K = số màu × 2`**.

> ✅ **Đã thi hành ngay một điều:** bù hao mặc định sửa từ **0% → 3%**. Bản trước để 0 vì *"không công thức nào trong mã có bù hao"* — đúng với **mã** nhưng **sai với tài liệu**. Notion ghi rõ *"hao phí mặc định 3%"*, case study tính **250 tờ + 3% = 258 tờ**.

---

## 🔴 QĐ-01 — HAI QUYẾT ĐỊNH TRONG NOTION MÂU THUẪN VỀ CÁCH TÍNH TIỀN IN

**Câu hỏi:** Khi tính tiền in, phần **chạy máy trọn lô** có nhân với **số kẽm** không?

**Chủ dự án đã chốt liên quan — không hỏi lại:** giá kẽm tính **theo tấm**; chi phí in gồm **hai thành phần** Kẽm + Chạy máy; máy nhỏ/lớn có bộ giá khác nhau.

**Bằng chứng đã tìm:** hai quyết định trong cùng trang Notion nói khác nhau.

| | Cách A *(theo quyết định cũ hơn)* | Cách B *(theo quyết định ghi **FINAL 01/02/2026**)* |
|---|---|---|
| Tiền kẽm | gộp trong công thức chung | **tách riêng** = số kẽm × giá 1 tấm |
| Tiền chạy máy trọn lô | **nhân với số kẽm** | **KHÔNG nhân** — là số cố định |
| In 2 mặt A-B | tính tiền trọn lô **2 lần** | tính **1 lần**, chỉ nhân đôi số tờ |

**Điều tự xác định được:** áp **cách B** vào ba ví dụ số thật mà chính quyết định đó ghi kèm → **khớp trọn vẹn cả ba**. Áp **cách A** với cùng bộ giá → ra **khác hẳn**.

**Phần duy nhất còn thiếu:** xác nhận rằng cách B là bản còn hiệu lực.

| Phương án | Ưu | Nhược | Rủi ro | Tác động dữ liệu | Công sức | Lùi lại |
|---|---|---|---|---|---|---|
| **B — dùng bản FINAL** *(khuyến nghị)* | Khớp cả 3 ví dụ số thật; có ngày cụ thể; mang nhãn FINAL | Bản cũ hơn vẫn nằm trong tài liệu, người sau đọc nhầm | Thấp | Không | Thấp | Đổi cấu hình JSON |
| A — dùng bản cũ | — | Không khớp ví dụ nào | **Tính sai tiền** | Không | Thấp | — |
| C — hỏi lại xưởng | Chắc chắn nhất | Chậm | Thấp | Không | Trung bình | — |

**Khuyến nghị:** **B**. Lý do: có ngày cụ thể, mang nhãn FINAL, và **kiểm được bằng ba ví dụ số thật** — cách A không kiểm được bằng gì.
**Mặc định an toàn nếu chưa quyết:** **không nối tính giá vào tiền thật**, giữ nguyên trạng thái chặn.

> **CHỦ DỰ ÁN TRẢ LỜI: ______**

**Sau khi chọn:** em gạch bản thua thành *lịch sử*, nạp bộ giá vào cấu hình, dựng bộ kiểm lấy đúng **ba ví dụ số thật** làm chuẩn. Không hỏi thêm gì.

---

## 🔴 QĐ-02 — "KHỔ TRẢI 1/2 HỘP" NGHĨA LÀ GÌ

**Câu hỏi:** Cột *"Khổ Trải 1/2 Hộp"* trong bảng tính của anh nghĩa là gì?

**Bằng chứng đã tìm:** tệp `Test Tính Khổ Trải Tân Phát` có **ba cột kết quả** — *Khổ Trải 1 Hộp* · *Khổ Trải 1/2 Hộp* · *Khổ Trải 1 Hộp Tối Ưu*. Với túi xách giấy, cột "1/2" đúng bằng **một nửa chiều dài** của cột "1 hộp".

**Điều tự xác định được — và nó KHÔNG khớp:** case study túi xách giấy trong Notion ghi kích thước `310 × 100 × 280mm`, khổ giấy `52 × 72cm`, bình bài **4 con/tờ**. Nhưng công thức trong bảng tính cho ra tấm trải **822 × 335mm** — **không lọt** tờ 720mm dù xoay. Nếu dùng cột "1/2" thì lọt, nhưng chỉ ra **2 con/tờ**, không phải 4.

**Phần duy nhất còn thiếu:** nghĩa nghiệp vụ của cột đó.

| Phương án | Nghĩa | Hệ quả cho bình bài |
|---|---|---|
| **A** | In **nửa túi** rồi ghép hai nửa lại | Số tờ **gấp đôi**, mỗi tờ chứa nửa sản phẩm |
| **B** | Tấm trải **gấp đôi được**, in một nửa rồi gập | Số tờ **như cũ**, tiết kiệm giấy |
| **C** | Chỉ là **cột tham khảo**, không dùng để tính tiền | Bỏ qua khi tính |

**Khuyến nghị:** em **không dám khuyến nghị** — ba cách cho ra **số tờ khác nhau**, tức **tiền khác nhau**. Đây là nghiệp vụ xưởng.
**Mặc định an toàn:** dùng cột *"Khổ Trải 1 Hộp"*, **chặn** kiểu dáng nào không tính ra được.

> **CHỦ DỰ ÁN TRẢ LỜI: ______**

---

## 🔴 QĐ-03 — HỘP CỨNG: "KHỔ TRẢI 1" VÀ "KHỔ TRẢI 2"

**Câu hỏi:** Với hộp cứng, *"Khổ Trải 1"* và *"Khổ Trải 2"* là **hai tấm phôi in riêng**, hay hai cách xếp trên cùng một tấm?

**Bằng chứng đã tìm:** hộp cứng trong bảng tính có **4 mảnh** (*Nắp · Đáy · Thành · Khay*) × **3 lớp** (*Bìa · Áo Ngoài · Áo Lót*). Rồi ghép: *Khổ Trải 1* = Nắp + Đáy · *Khổ Trải 2* = Khay + Thành.

**Điều tự xác định được:** cách ghép là **cộng chiều dài, giữ chiều rộng** — đọc được từ chính công thức ô Excel.

**Phần duy nhất còn thiếu:** hai tấm đó có phải **in riêng hai lần** không.

| Phương án | Hệ quả tiền |
|---|---|
| **A — hai phôi in riêng** | **Hai lần** tính tiền kẽm + chạy máy |
| **B — hai cụm trên cùng tấm** | **Một lần** |
| **C — hai phương án chọn một** | Một lần, chọn cái tiết kiệm hơn |

**Khuyến nghị:** không khuyến nghị — chênh lệch tiền rất lớn.
**Mặc định an toàn:** **chặn hộp cứng**, chưa cho tính giá.

> **CHỦ DỰ ÁN TRẢ LỜI: ______**

---

## 🟠 QĐ-04 — CÁC SỐ CỘNG THÊM `+2` `+3` `+5` TÊN LÀ GÌ

**Câu hỏi:** Trong công thức khổ trải có các số cộng thêm `+2`, `+3`, `+5` — mỗi số là khoản gì?

**Bằng chứng đã tìm:** con số **khác nhau theo kiểu dáng** — túi xách `+2`, nắp cài `+3`, hộp cứng `+5`. Bảng tính **không ghi tên** cho chúng.

**Điều tự xác định được:** hệ thống đã có sẵn ba khái niệm **mép dán · chừa xén · nắp**. Các số này **có thể** là một trong ba, nhưng **không có nguồn nào xác nhận**.

**Phần duy nhất còn thiếu:** tên nghiệp vụ của từng số.

**Khuyến nghị:** anh chỉ cần nói **một câu** cho mỗi kiểu dáng, ví dụ *"`+3` ở hộp nắp cài là chừa xén"*. Có tên rồi thì chúng thành **khoản cấu hình sửa được**, không còn là số bí ẩn.
**Mặc định an toàn:** giữ nguyên giá trị, gắn nhãn *"chưa đặt tên"*, **không cho sửa** ở màn cấu hình.

> **CHỦ DỰ ÁN TRẢ LỜI: ______**

---

## 🟠 QĐ-05 — VÌ SAO "KHỔ TỐI ƯU" BỚT ĐƯỢC MỘT LẦN CHIỀU CAO

**Câu hỏi:** Với hộp nắp cài, cột *"tối ưu"* bớt đúng **một lần chiều Cao** so với cột thường. Nguyên lý gấp nào cho phép bớt?

**Bằng chứng đã tìm:** thường `Dài + Cao×4`, tối ưu `Dài + Cao×3`. Chỉ đo được **con số**, không đo được **lý do**.

**Phần duy nhất còn thiếu:** nguyên lý — để em áp đúng cho các kiểu dáng khác.

**Khuyến nghị:** anh giải thích **một câu** về cách gấp. Ví dụ *"nắp và đáy cài lồng nhau nên bớt được một tầng"*. Hiểu nguyên lý thì em suy ra được cho kiểu khác; không hiểu thì mỗi kiểu phải hỏi lại.
**Mặc định an toàn:** dùng cột **thường** (đắt hơn, an toàn hơn), không tự dùng cột tối ưu.

> **CHỦ DỰ ÁN TRẢ LỜI: ______**

---

## 🟡 QĐ-06 — CỔNG KIỂM KHÔNG QUÉT ĐƯỢC HỌ TÊN THẬT

**Câu hỏi:** Có muốn em làm cổng quét **họ tên thật** trong mã không, hay chấp nhận rủi ro?

**Bằng chứng đã tìm:** cổng hiện chỉ quét **email · số điện thoại · số căn cước**. Khi vá cổng hôm nay, em phát hiện một tệp script viết cứng **cả email lẫn họ tên đầy đủ** của một người — cổng bắt được email nhưng **hoàn toàn không thấy tên**. Đã dọn tệp đó. Quét toàn kho còn **5 tệp** chứa họ tên thật.

**Điều tự xác định được:** khó vá — tên người Việt **không có khuôn dạng máy nhận được**, khác email/số điện thoại. Quét bằng danh sách họ phổ biến sẽ **báo nhầm tràn lan** (tên công ty, tên đường, tên sản phẩm).

| Phương án | Ưu | Nhược | Rủi ro |
|---|---|---|---|
| **A — dọn tay 5 tệp, không làm cổng** *(khuyến nghị)* | Nhanh, không báo nhầm | Không chặn được lần sau | Trung bình |
| B — làm cổng quét theo danh sách họ | Chặn tự động | **Báo nhầm nhiều**, dễ bị vô hiệu hoá | Cao |
| C — chấp nhận rủi ro | Không tốn công | Tên thật tiếp tục lọt | Trung bình |

**Khuyến nghị:** **A** — dọn 5 tệp, ghi vào sổ nợ để phiên sau biết.
**Mặc định an toàn:** giữ nguyên, nợ vẫn mở.

> **CHỦ DỰ ÁN TRẢ LỜI: ______**

---

## PHẦN CUỐI — ĐIỀU EM CHƯA LÀM ĐƯỢC TRONG LƯỢT NÀY

Nói thẳng để anh biết trạng thái thật:

| Việc | Trạng thái |
|---|---|
| Vá cổng đọc nội dung từ **bản đã đưa vào commit** thay vì từ đĩa | ⏸️ **CHƯA** — mới khảo sát xong |
| Vá chuỗi cổng gọi **bản tự kiểm** thay vì cổng đọc đầu ra thật | ⏸️ **CHƯA** — mới khảo sát xong |
| Kiểm tra quyền giá vốn ở **cả 4 vùng** + gọi thẳng đường dẫn | ⏸️ CHƯA |
| Đơn hàng — nguồn công nợ chuẩn để chốt *"đã thu đủ"* | ⏸️ CHƯA |
| Nạp dữ liệu trong ứng dụng — 2 đường chưa có ngưỡng | ⏸️ CHƯA |
| Bảng công thức đầy đủ cho **40 nhóm sản phẩm** | ⏸️ CHƯA |
| Gán người phụ trách 1.692 khách — đo các tín hiệu | ⏸️ CHƯA |
| Quyền Trưởng phòng Sản xuất | ⏸️ CHƯA |
| Giao diện phân quyền · quản lý tài khoản | ⏸️ CHƯA |

**Chưa triển khai lên máy vận hành.** Phiên này không được cấp kênh đo, nên mọi số liệu là **máy phát triển**.
