# BÁO CÁO HỢP NHẤT MỘT LUỒNG — 25/08/2026

> **Bản công khai đã lọc an toàn.** Không chứa mã nguồn · dữ liệu khách hàng · email · số điện thoại · địa chỉ IP · cổng máy chủ · đường dẫn hạ tầng. Tên bảng/cột/route được phép nêu để truy vết (`OD-07`).
>
> **Plan of Record:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`
> **Người thực hiện:** Agent IDE — **một phiên điều phối duy nhất**
> **Mốc sự thật:** `ERP-SNAPSHOT-20260825-182541`
> **Trạng thái:** Pha 0 · 1 · 2 · 3 **XONG** — Pha 4 · 5 · 6 **CÒN**

---

## 0. TRẢ LỜI THẲNG BA CÂU CHỦ DỰ ÁN HỎI

| Câu hỏi | Trả lời |
|---|---|
| **Xong chưa?** | **CHƯA.** Xong 4/7 pha. Còn Pha 4 (dữ liệu & tiền), Pha 5 (sản phẩm), Pha 6 (bàn giao Notion). |
| **Push báo cáo đầy đủ chưa?** | **RỒI — lúc viết dòng này.** Kho mã đã đẩy 2 mốc; kho công khai đẩy cùng báo cáo này. |
| **Sao toàn tiếng Anh?** | **Lỗi của Agent.** Đã sửa, ghi sổ mục `#155`, từ đây viết hoàn toàn tiếng Việt. |

---

## 1. VIỆC GẤP NHẤT ĐÃ XỬ LÝ XONG — DỮ LIỆU KHÁCH HÀNG

### 1.1 Ba tệp dữ liệu thật đã gỡ khỏi kho mã

Trước hôm nay, kho mã đang **theo dõi 7 tệp** có dấu hiệu dữ liệu cá nhân. Cổng tự kiểm báo *xanh* — nhưng xanh vì **đã ghi nhận là nợ đã biết**, **không phải vì đã dọn**.

Đo lại bằng cách mở cấu trúc tệp (không đọc giá trị ô), kết quả tách bạch:

| Tệp | Đo được | Kết luận |
|---|---|---|
| Danh mục khách hàng + nhà cung cấp (`.csv`) | 1.232 dòng × 28 cột · 2.667 chuỗi giống số điện thoại · 22 email · có cột số tài khoản, chủ tài khoản, ngân hàng, còn nợ | 🔴 **DỮ LIỆU THẬT** |
| `NHẬP LIỆU HÀNG NGÀY` (2 tệp, gần như trùng nhau) | 17 trang tính · bảng `DS KH` **264 dòng** · `DS NCC` **42 dòng** · `DS NV` **6 dòng** · 512 chuỗi riêng biệt | 🔴 **DỮ LIỆU THẬT** |
| `Bộ Biểu Mẫu Tân Phát 2026` (3 tệp) | **0 dòng dữ liệu lặp** · chỉ 1 chuỗi tiêu đề công ty | ✅ **Biểu mẫu trống** |

> **Điểm đáng chú ý:** sổ nợ trước đây chỉ ghi *"nghi là dữ liệu thật, chưa mở được để xác nhận"* cho hai tệp `NHẬP LIỆU`. Phép đo hôm nay **đóng câu hỏi bỏ ngỏ đó**: chúng có bảng danh sách khách hàng 264 dòng — là dữ liệu vận hành thật.

**Đã làm:** ba tệp đỏ **gỡ khỏi kho mã**, **tệp vẫn còn nguyên trên máy**. Ba biểu mẫu trống **giữ lại** đúng chủ đích (là tài sản nghiệp vụ cần version hoá).

**Chưa làm — cần chủ dự án quyết:** ba tệp đó **vẫn nằm trong lịch sử git đã đẩy lên**. Gỡ khỏi hiện tại **không** gỡ khỏi lịch sử. Xem mục 5.

### 1.2 Kho công khai — đã dọn sạch

| Thứ bị lộ | Số chỗ | Xử lý |
|---|---:|---|
| Bảng **14 người** kèm họ tên thật + email công ty (tài liệu chuẩn bị go-live) | 14 dòng | Che họ tên + email, **giữ cơ cấu 10 kinh doanh + 4 thiết kế** để vẫn tra được |
| Email nhân sự thật rải trong 4 tài liệu | 5 chỗ | Che |
| Địa chỉ IP máy chủ vận hành | 1 chỗ | Che |
| Số cổng nội bộ trong 2 báo cáo | 4 chỗ | Thay bằng *"đang chạy"* — trạng thái mới là điều đáng báo, số cổng thì không |

**Đo lại sau khi dọn:** kho công khai còn **0 email thật · 0 địa chỉ IP thật**. Ba số còn lại là **mã số thuế công ty** (công khai hợp pháp) và **số mẫu tuần tự** — không phải dữ liệu cá nhân.

### 1.3 Tệp cấu hình mẫu bị theo dõi

`.env.deploy.example` là tệp **mẫu** nhưng đang chứa **giá trị thật**: địa chỉ IP máy chủ, tên tài khoản đăng nhập, đường dẫn trên máy chủ, tên miền vận hành, đường dẫn khoá. Đã thay **9 chỗ** bằng chỗ trống. Giá trị thật tra ở sổ bí mật nội bộ (đã bị git bỏ qua).

---

## 2. GỐC CỦA CHUYỆN "KHÔNG LIỀN MẠCH" — VÀ CÁCH VÁ

Chủ dự án mở khoảng 10 phiên làm việc song song trên cùng một cây thư mục. Không phiên nào nói sai — **kho đi nhanh hơn tốc độ một phiên đo xong rồi viết báo cáo**.

Nhưng có **hai lỗ thi hành thật**, đã vá xong hôm nay:

### 2.1 Không có cơ chế cấp số → hai phiên cấp trùng mã

Cổng chống trùng mã hiện có bắt được lỗi — nhưng bắt **SAU KHI** đã ghi. Khoảng trống nằm đúng ở giữa:

```
   đọc sổ  ─────────────────────────►  ghi sổ
           ▲                        ▲
           └── phiên khác chen vào ─┘   ⇒ hai phiên cùng ra một mã
```

**Đã dựng cơ chế cấp số nguyên tử:** giữ khoá ở tầng hệ điều hành → **đọc lại sổ SAU KHI đã giữ khoá** → cấp mã → kiểm 3 điều kiện → ghi tệp tạm → đổi tên nguyên tử → nhả khoá kể cả khi lỗi.

Ba điều kiện kiểm **trước khi** ghi đè: (1) nội dung mới không rỗng; (2) **mọi mã cũ vẫn còn** — chống mất mục; (3) mã mới xuất hiện đúng một lần.

**Kiểm ngược thật — 20/20 đạt:** 4 tiến trình **thật** chạy đồng thời × 10 vòng × cả hai sổ, không vòng nào trùng mã, không vòng nào nuốt mục; khoá chết tự gỡ; khi hàm ghi ném lỗi thì khoá vẫn nhả và sổ giữ nguyên bản cũ.

> **Bài kiểm ngược này bắt được một lỗi thật ngay trong chính bản vá.** Bản đầu nhận dạng loại sổ bằng cách **so đường dẫn tuyệt đối** với hai sổ đã biết — nên mọi đường dẫn khác **âm thầm** rơi về định dạng sổ Owner (cấp `#1` thay vì `DEBT-001`), không một lời cảnh báo. Kho này **đã đổi chỗ hai lần** trong lịch sử dự án ⇒ so đường dẫn là nền móng cát. Đã sửa: nhận dạng bằng **tên tệp** trước, rồi mới ngửi nội dung.

### 2.2 Ba tài liệu cùng tự nhận vai trò điều phối

Ngày 25/08 có **ba** tài liệu cùng hiệu lực: một tự gọi mình là *"Plan of Record"*, một tự đặt mình *"bọc ngoài"* tài liệu kia, một tự đặt mình *"lớp điều phối trên cùng"*. Đúng thứ luật một-Plan-duy-nhất cấm.

**Đã khép về MỘT.** Ba tài liệu kia nhận nhãn *chỉ để tra cứu* — **thêm 9 dòng nhãn mỗi tài liệu, không xoá dòng nào**.

---

## 3. NHỮNG ĐIỂM MÙ CỦA CỔNG TỰ KIỂM — TÌM RA HÔM NAY

Đây là phần đáng lo nhất, vì cổng tự kiểm là thứ dự án đang **tin tưởng**.

| Điểm mù | Hậu quả thật | Trạng thái |
|---|---|---|
| Cổng khớp giá trị trạng thái đồng bộ **không nhận dạng có dấu hai chấm** — chính là dạng luật dùng làm chuẩn | Giá trị ghi **đúng luật** thì cổng **không đếm** | ✅ **Đã vá** — 22 → 23 giá trị |
| Hai cổng bảo mật lấy **danh sách** tệp từ chỉ mục git nhưng đọc **nội dung** từ đĩa | Tệp đã đưa vào commit với nội dung bẩn, rồi sửa sạch trên đĩa mà chưa đưa lại → cổng đọc bản sạch và cho qua, trong khi bản **bẩn** mới là bản được commit. Cổng này nối vào bước kiểm trước commit ⇒ **bảo đảm sai ở đúng nơi được tin nhất** | 📋 Ghi nợ `DEBT-105` |
| Chuỗi cổng quản trị gọi cổng báo cáo ở **chế độ tự kiểm** (chỉ chạy chuỗi mẫu viết cứng) thay vì cổng đọc đầu ra thật | Giá trị thi hành cho báo cáo thật **bằng 0** | 📋 Ghi nợ `DEBT-106` |
| Ngưỡng mật độ email = 15 | **77 địa chỉ email định danh thật** nằm rải trên **101 tệp** bị theo dõi mà cổng không thấy. Một tài liệu có **14** email — qua cổng **cách ngưỡng đúng 1**. Đáng lo nhất là bản xuất tài liệu **tiền lương** | 📋 Ghi nợ `DEBT-107` — **cần chủ dự án quyết ngưỡng** |

> Bốn dòng trên đều thuộc đúng loại lỗi mà luật *"cổng tự động phải đọc đầu ra thật"* sinh ra để chặn: **khai có kiểm, thi hành bằng 0**.

---

## 4. ĐÍNH CHÍNH BA ĐIỀU CÁC BÁO CÁO TRƯỚC NÓI SAI

Phần này quan trọng vì các báo cáo cũ đang được dùng làm căn cứ quyết định.

### 4.1 Con số "21 hay 22" — cả hai đều sai cách đếm

Các báo cáo cũ cãi nhau: một bên ghi *"21 mục"*, một bên ghi *"20/153"*, phân bố cộng lại không khớp.

**Sự thật đo được:** **23 dòng** chứa trường đó · **25 lần xuất hiện** · **22 giá trị hợp lệ**. Chênh nhau vì **một mục nhắc trường đó 3 lần trong cùng một dòng**. Không ai đếm sai — họ đếm **ba thứ khác nhau** rồi so với nhau.

### 4.2 "Cổng chưa được vá" — sai, đã vá từ 23/08

Một gói quyết định trình chủ dự án đề xuất *"vá cổng trước"*. Việc đó **đã xong từ 23/08**. Phiên đo sai vì đếm trong **tệp cổng**, còn bản vá đã rút ra **tệp dùng chung** mà 4 cổng cùng gọi.

> ⚠️ **Nếu duyệt nguyên văn**, một phiên sẽ đi *"vá"* cổng đã vá — có nguy cơ **làm hỏng ngược bản vá đang tốt**. Bước đó phải **gạch bỏ**; các bước còn lại vẫn đúng.

### 4.3 "Đã đổi khoá rồi" — KHÔNG được suy ra như vậy

Hai dòng nợ bảo mật trỏ về **cùng một tài khoản quản trị**. Nhưng việc *"đổi khoá"* mà quyết định cũ nhắc tới **không nằm ở dòng nợ đó** — nó là một dòng khác, đã đóng 20/08 với trạng thái **chủ dự án chấp nhận rủi ro**.

⇒ **Không được kết luận khoá hạ tầng đã đổi.** Trạng thái đúng là **CHƯA XÁC ĐỊNH ĐƯỢC** — không phải *đã xong*, cũng không phải *chưa làm*. Ghi nợ `DEBT-109`, **chờ chủ dự án xác nhận**. Agent **không tự đóng** một nợ bảo mật.

---

## 5. VIỆC CẦN CHỦ DỰ ÁN QUYẾT

### 5.1 🔴 Lịch sử git còn chứa dữ liệu khách hàng — chọn 1 trong 3

Ba tệp đã gỡ khỏi **hiện tại**, nhưng **vẫn nằm trong lịch sử** đã đẩy lên. Số đo quyết định:

| Phương án | Chi phí đo được | Ưu | Nhược |
|---|---|---|---|
| **A. Giữ lịch sử, siết quyền truy cập kho** | 0 mốc bị viết lại | Không phá gì. Kho vốn **riêng tư** | Dữ liệu vẫn nằm đó với ai đã có quyền |
| **B. Xoá khỏi lịch sử** | Riêng tệp danh mục khách hàng: **viết lại 425/505 mốc (84%)**. Cả 7 tệp: **501/505 (99,2%)** | Sạch triệt để | **Hỏng mọi bản sao đang có.** Luật hiện hành **CẤM** viết lại lịch sử trong đợt này |
| **C. Chuyển sang kho mới sạch** | Dựng kho mới | Sạch, không phá bản sao cũ | Mất liên kết lịch sử; tốn công |

> **Khuyến nghị:** **A**, vì kho là **riêng tư** và B phá 84–99% lịch sử. Nếu chủ dự án thấy rủi ro cao hơn, chọn **C** thay vì B.

### 5.2 Ngưỡng phát hiện email

Hiện ngưỡng là 15. Có **77 địa chỉ định danh thật** lọt dưới ngưỡng. Chọn: **hạ ngưỡng**, hay thêm luật *"bất kỳ địa chỉ nào trên tên miền công ty đều tính, không cần mật độ"*? — Khuyến nghị **cách thứ hai**.

### 5.3 Ba biểu mẫu trống

Đo ra **0 dữ liệu thật**. Đề xuất **giữ trong kho** (là biểu mẫu chuẩn cần version hoá) và khai nhận vào bản đồ thư mục. Cần chủ dự án gật đầu.

### 5.4 Xác nhận đóng một nợ bảo mật

Hai dòng nợ chồng lấn nhau (mục 4.3). Cần chủ dự án xác nhận là **cùng một việc** thì mới đóng được dòng còn lại.

---

## 6. SỐ ĐO HIỆN TRẠNG *(tự đo trong lượt này, không dùng lại số phiên khác)*

| Mục | Giá trị | Cách đo |
|---|---|---|
| Nền tảng CSDL (máy phát triển) | **MariaDB 10.11.10** | Truy vấn đọc-thuần |
| Số bảng | **101 bảng thật · 0 khung nhìn** | Truy vấn hệ thống |
| Bộ ký tự | 101/101 đồng nhất | Truy vấn hệ thống |
| Số phiên bản trong mã | **V1.00.355** | Đọc tệp nguồn |
| Bộ luật — 5 bản sao | **Giống hệt nhau từng byte** (1 mã băm × 5 tệp) | Băm SHA-256 |
| Sổ yêu cầu chủ dự án | **155 mục**, 0 trùng | Cổng |
| Sổ nợ kỹ thuật | **115 mục** (thêm 5 hôm nay) | Cổng |
| Cổng quản trị | **18/18 ĐẠT**, mã thoát 0 | Chạy thật |

> ⚠️ **Số bảng 101 khớp đúng con số chủ dự án đã chốt.** Phép đo hôm nay còn **đóng thêm một điểm bỏ ngỏ**: hồ sơ cũ ghi *"chưa tách được bảng thật và khung nhìn"* — nay đã tách: **101 đều là bảng thật, không có khung nhìn nào**.
>
> ⚠️ **Máy vận hành:** phiên này **không được cấp kênh đo**, nên mọi kết luận về máy vận hành là **CHƯA XÁC MINH**. Số liệu trên là **máy phát triển**.

---

## 7. CÒN LẠI — NÓI RÕ, KHÔNG GIẤU

| Pha | Nội dung | Trạng thái |
|---|---|---|
| 4 | Dữ liệu & tiền — độ rộng cột, **công thức khổ trải**, bảng giá công đoạn, ngưỡng nạp, gán khách hàng, phụ trách nhà cung cấp | Audit **đã chạy xong**, đang gom thành gói quyết định |
| 5 | Sản phẩm — đơn hàng, quyền xem giá vốn, quyền trưởng phòng sản xuất, giao diện phân quyền, biểu mẫu, quy trình | Audit **đã chạy xong**, chưa viết mã |
| 6 | Gói bàn giao cho Notion | Chưa bắt đầu |

**Chưa triển khai lên máy vận hành.** Đợt này là **tài liệu + công cụ quản trị**, không đụng mã nghiệp vụ, **không tăng số phiên bản**, **không triển khai**.

---

## 8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC

**Trình chủ dự án gói quyết định Pha 4 + Pha 5**, trong đó việc chặn tiền là **công thức khổ trải** — audit xác nhận đang có **nhiều hơn một công thức trong mã**, nghĩa là một trong số đó **đang tính sai tiền**.

---

*Báo cáo do Agent IDE lập. Mọi con số đều tự đo trong lượt này. Bản đầy đủ kèm bằng chứng chi tiết nằm ở kho riêng tư.*
