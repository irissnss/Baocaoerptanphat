# GÓI QUYẾT ĐỊNH PHA 4 + PHA 5 — 25/08/2026

> **Plan of Record:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`
> **Lớp bằng chứng:** `DB_PROVEN` + `CODE_PROVEN` — đo trực tiếp trên CSDL máy phát triển (MariaDB 10.11.10, 101 bảng thật) và trên mã nguồn tại `5ba3299`.
> **Cách làm:** 11 vùng audit chạy song song, sau đó **82 kết luận nặng bị đưa qua kiểm ngược đối kháng độc lập** — **17 kết luận bị BÁC hoặc phải SỬA**. Báo cáo này **chỉ dùng bản đã qua kiểm ngược**.
>
> ⚠️ **Nói trước cho rõ:** bản audit đầu tiên nói *"công thức khổ trải làm lệch 416.500đ"*. **Con số đó SAI** — kiểm ngược chứng minh nó là **con số ảo** sinh từ một giả định chưa nói ra. Chi tiết ở §1. Nếu em trình anh bản đầu thì anh đã quyết trên số sai.

---

## 0. BA VIỆC CẦN ANH QUYẾT — TÓM TẮT MỘT TRANG

| # | Việc | Câu hỏi cho anh | Vì sao chỉ anh trả lời được |
|---|---|---|---|
| **1** | **Quy ước trục hộp** | Khi nhập **Dài × Rộng × Cao**, **đáy hộp** là hai chiều nào? | Đây là nghiệp vụ bao bì, không suy ra được từ mã |
| **2** | **Hằng số phụ cấp** | Mép dán · chừa xén · nắp · **bù hao** — mỗi thứ bao nhiêu mm / bao nhiêu %? | Mã đang dùng số cứng không ai giải thích được |
| **3** | **Quyền xem giá vốn** | Ngoài Quản trị + Tổng giám đốc, ai được xem? | Nghiệp vụ |

**Hai tin tốt em tìm được — đỡ cho anh rất nhiều việc:**

- 🎉 **Anh KHÔNG phải gán tay 1.692 khách hàng.** Dữ liệu AppSheet **đã có sẵn** người phụ trách — chỉ là chưa ai chuyển sang. Gán tự động được **1.664/1.692 khách (98,3%)**, còn đúng **28 khách** cần anh xem. Chi tiết §3.
- 🎉 **Ngưỡng lỗi nạp dữ liệu KHÔNG cần anh chốt** — đã chốt rồi từ 23/08 (≤2%/lô · tách tỉnh ≥98%). Sổ nợ ghi *"chưa chốt"* là **lỗi thời**. Chi tiết §4.

---

## 1. 🔴 CÔNG THỨC KHỔ TRẢI — VIỆC CHẶN TIỀN

### 1.1 Sự thật đã qua kiểm ngược

Sổ nợ ghi *"hai công thức khác nhau"*. Đo được: **sáu chỗ tính khổ trải** trong mã. Nhưng điều quan trọng nhất là:

> **F1 và F2 KHÔNG phải hai hình học khác nhau. Chúng là CÙNG MỘT công thức, chỉ khác ở chỗ gọi chiều nào là "Dài", chiều nào là "Cao".**

Chứng minh bằng đại số, kiểm trên 3 mẫu chuẩn + 6 mẫu ngẫu nhiên, **luôn đúng**:

```
F2(H, W, L)  =  F1(L, W, H) + (19mm, −16mm)      ← đúng với MỌI đầu vào
```

Nghĩa là hai công thức chỉ lệch nhau đúng **hai điều**:

| Điều lệch | F1 (màn tính giá thủ công) | F2 (đường tiền thật) |
|---|---|---|
| **Đáy hộp là hai chiều nào** | Rộng × Cao *(thân hộp cao bằng "Dài")* | Dài × Rộng *(thân hộp cao bằng "Cao")* |
| **Phụ cấp: mép dán** | **21mm** = mép dán 15 + chừa xén 3×2 | **40mm** — không tài liệu nào giải thích |
| **Phụ cấp: nắp** | **36mm** = nắp 30 + chừa xén 3×2 | **20mm** — không giải thích |

### 1.2 Con số "416.500đ" là con số ẢO — đừng dùng

Bản audit đầu tiên tính: hộp 200×150×100mm, 5.000 cái → F1 được 2 con/tờ, F2 được 3 con/tờ → chênh **416.500đ**.

**Kiểm ngược bác con số đó**, vì nó so hai công thức ở **cùng nhãn L/W/H** — mà chính điều đó là thứ **đã biết là không đúng** (hai công thức dùng quy ước trục khác nhau; chính sổ nợ đã ghi vậy).

Khi **căn trục cho khớp**, con số lật hoàn toàn:

| Mẫu hộp (5.000 cái) | So cùng nhãn *(sai)* | Căn trục cho khớp *(đúng)* |
|---|---:|---:|
| 200 × 150 × 100 mm | 416.500 đ | **0 đ** |
| 300 × 200 × 80 mm | 0 đ | **1.250.000 đ** |

⇒ **Con số chênh phụ thuộc HOÀN TOÀN vào việc quy ước trục nào đúng.** Chốt được §0 việc 1 thì con số này tự hết.

### 1.3 Tiền thật chảy ở đâu — ngược với hình dung ban đầu

| Công thức | Có nối vào tiền thật không? | Bằng chứng |
|---|---|---|
| **F1** — màn tính giá thủ công | ❌ **KHÔNG.** Bị cách ly | Nút lưu chỉ là thông báo giả · màn có banner *"không phải báo giá chính thức, không được gửi khách"* · API lưu **bị chặn từ 31/07/2026** |
| **F2** — `unified-pricing-adapter` | ✅ **CÓ — đây mới là đường tiền** | Ghi thẳng vào `bao_gia_option`: cập nhật `gia_von` và `thanh_tien` |

⇒ **Không đồng nào từ F1 tới khách hàng.** Trên đường tiền thật (F2), sai số khổ trải rơi vào **diện tích**, mức **0,2 % – 2,6 %** — **không** phải vào số tờ giấy.

⇒ **Mức độ: NẶNG, nhưng KHÔNG phải đang cháy.** Nó là **cổng chặn** cho hai việc: mở khoá lại màn tính giá thủ công, và đưa công thức về dữ liệu thay vì viết cứng.

### 1.4 Ba lỗi thật khác, đo được

1. **Không có bù hao ở bất kỳ công thức nào.** Quét toàn bộ **101 bảng** không có cột nào lưu mép dán · chừa xén · bù hao.
2. **Ép âm thầm về 1 con/tờ.** Khi khổ trải **không lọt tờ giấy**, mã có `Math.max(..., 1)` biến "không lọt" thành "1 con/tờ" — **đáp án đắt nhất** — **không cảnh báo gì**. Và **không bao giờ thử xoay tờ**.
3. **Chỗ đúng đã có sẵn nhưng CHẾT.** Hàm `calculateFlatSize` đọc công thức từ blueprint — đúng kiến trúc anh cần — nhưng **không ai gọi**, và ô dữ liệu nuôi nó thì **rỗng** (`dm_blueprint` có 1 dòng demo, không có công thức khổ trải).

### 1.5 ❓ CÂU HỎI CHO ANH — trả lời được ngay

> **Câu 1 — Quy ước trục.** Khi nhập một cái hộp là **Dài × Rộng × Cao**, thì **mặt đáy** của hộp là hai chiều nào?
>
> - **(a)** Đáy = **Dài × Rộng**, thân hộp cao bằng **Cao** *(cách F2 đang dùng — và F2 là đường tiền thật)*
> - **(b)** Đáy = **Rộng × Cao**, thân hộp cao bằng **Dài** *(cách F1 đang dùng)*
>
> *Gợi ý của em: gần như chắc là **(a)** — đó là cách nói thông thường trong ngành, và cũng là công thức đang nối vào tiền. Nhưng em **không dám tự quyết** vì đây là nghiệp vụ.*

> **Câu 2 — Bốn con số phụ cấp.** Điền giúp em:
>
> | Khoản | F1 đang dùng | F2 đang dùng | **Anh chốt** |
> |---|---:|---:|---|
> | Mép dán | 15 mm | *(gộp trong 40)* | ______ mm |
> | Chừa xén mỗi bên | 3 mm | *(không có)* | ______ mm |
> | Nắp | 30 mm | *(gộp trong 20)* | ______ mm |
> | **Bù hao** | **không có** | **không có** | ______ % |
>
> *Nếu bốn con số này **thay đổi theo loại hộp**, anh nói giúp em loại nào khác loại nào — em sẽ dựng thành bảng cấu hình thay vì viết cứng.*

**Sau khi anh trả lời 2 câu trên, em làm được ngay mà không hỏi thêm:** dồn 6 chỗ về **một nguồn duy nhất đọc từ dữ liệu** · nạp công thức vào blueprint · thay `Math.max(...,1)` bằng **cảnh báo thật** + thử xoay tờ · viết bộ kiểm với kết quả mong đợi.

---

## 2. 🟠 QUYỀN XEM GIÁ VỐN — đúng chỗ anh nói "chưa hiểu"

Anh nhắc việc này nhiều lần. Em tìm ra **vì sao nó rối**: dự án có **HAI mô hình quyền rời rạc cho cùng một khái niệm**.

| | Cách A — quyền hành động | Cách B — che theo trường |
|---|---|---|
| Cơ chế | Ô tick `can_view_cost_price` ở `/m0/security` | Danh sách trường nhạy cảm **viết cứng trong mã** |
| Ai đang có | **Quản trị + Tổng giám đốc** (2 dòng, đo được) | — |
| Bảng dữ liệu | có | `role_field_permission` — **0 dòng** |
| Có giao diện quản lý không | ✅ có | ❌ **không có màn nào tạo được** |
| Được đọc ở mấy chỗ | **đúng 1 chỗ** | — |

**Giá vốn nằm ở 4 cột:** `bao_gia_option.gia_von` · `don_hang_item.gia_von` · `material_item.gia_von_trung_binh` · `pricing_quote_history.gia_von`.

> 🔴 **Hệ quả thật:** cách A chỉ được kiểm ở **đúng 1 chỗ**, còn cách B thì bảng dữ liệu **rỗng và không có cách nào điền**. ⇒ **Giá vốn hiện KHÔNG được che ở phần lớn nơi hiển thị.**

### ❓ CÂU HỎI CHO ANH

> **Câu 3.** Ngoài **Quản trị** và **Tổng giám đốc**, ai được xem giá vốn?
>
> Vai trò đang có trong hệ thống: `ADMIN` · `CEO` · `HR` · `KE_TOAN` *(kế toán)* · `SALES` *(kinh doanh)* · `TP_KINH_DOANH` · `TP_SAN_XUAT` · `TP_THIET_KE` · `USER`.
>
> - **(a)** Giữ nguyên — chỉ Quản trị + Tổng giám đốc
> - **(b)** Thêm **Kế toán** *(khuyến nghị — kế toán cần giá vốn để tính lãi lỗ)*
> - **(c)** Thêm **Kế toán + Trưởng phòng kinh doanh**
> - **(d)** Anh nêu danh sách khác
>
> Em **đề xuất bỏ hẳn cách B** (che theo trường) vì nó chưa bao giờ hoạt động, và **dùng một cách A duy nhất** cho cả 4 cột — như vậy anh chỉ cần tick ô ở `/m0/security` là xong, không phải nhớ hai chỗ.

---

## 3. 🎉 GÁN NGƯỜI PHỤ TRÁCH 1.692 KHÁCH — ANH KHÔNG PHẢI LÀM TAY

Sổ nợ `DEBT-060` ghi *"nguồn AppSheet KHÔNG có cột người phụ trách"*. Đo lại **file AppSheet gốc**: câu đó **đúng với bảng danh mục khách hàng**, nhưng **SAI với bảng giao dịch**.

| Nguồn tín hiệu | Có dữ liệu không | Phủ được bao nhiêu / 1.692 khách |
|---|---|---:|
| `BaoGia` → cột **"Người báo giá"** | ✅ **4.098/4.098 dòng đều có** | **1.664 khách (98,3%)** |
| `DonHang` → cột **"Người nhận đơn"** | ✅ **1.780/1.780 dòng đều có** | 296 khách |
| Người tạo bản ghi | ✅ 1.692/1.692 | 1.692 *(nhưng yếu nhất)* |

> **Đề xuất luật gán — anh chỉ duyệt LUẬT, không phải duyệt 1.692 dòng:**
>
> 1. Người phụ trách = **người báo giá ở báo giá gần nhất** của khách đó → phủ **1.664 khách**
> 2. Không có báo giá → lấy **người nhận đơn ở đơn gần nhất**
> 3. Vẫn không có → **để trống**, đưa vào danh sách chờ → còn đúng **28 khách**, anh xem một lượt là xong
>
> ⚠️ Muốn chạy luật này phải **chuyển bảng `BaoGia` từ AppSheet sang** trước — tài liệu tham chiếu đang ghi bảng đó *"cần dựng"*. Đây là việc kỹ thuật, em làm được.

**Nhà cung cấp (`DEBT-061`):** đã kiểm — bảng nhà cung cấp **không có** cột nào đóng vai người phụ trách. Cần thêm một cột cho phép trống, giống hệt bên khách hàng. Em **chưa chạy** — di trú CSDL cần cổng riêng.

---

## 4. 🎉 NGƯỠNG LỖI NẠP DỮ LIỆU — ĐÃ CHỐT RỒI, KHÔNG CẦN ANH QUYẾT LẠI

Kế hoạch 25/08 đề xuất *"chốt ngưỡng 2%"*. Đo được: **đã điền từ 23/08/2026 lúc 15:06**, gồm **hai** con số: tỉ lệ lỗi **≤ 2%/lô** và tách tỉnh **≥ 98%**.

Sổ nợ `DEBT-069` vẫn ghi *"chưa chốt `___%`"* vì nó được viết **lúc 06:22 cùng ngày** — **trước** khi điền 9 tiếng. **Lại một tiền đề lỗi thời**, cùng loại với `DEBT-104`.

> 🔴 **Nhưng có việc gấp hơn mà chưa ai nêu:** có **5 đường nạp dữ liệu**, không phải 1. Ba đường là script chạy tay (đã dùng cho đợt nạp 22/08). **Hai đường còn lại nằm ngay trong ứng dụng** — nhập CSV khách hàng và nhập chấm công — **bất kỳ ai có quyền tạo đều chạy được**, và **cả hai KHÔNG có một dòng kiểm ngưỡng nào**.
>
> Nghĩa là: ngưỡng anh chốt chỉ áp cho đường chạy tay. Hai cửa còn lại đang mở toang. Em ghi nợ và đề xuất vá — **không cần anh quyết**, đây là việc kỹ thuật.

---

## 5. OIL #141 — SÁU VIỆC ANH GIAO 24/08

> **Đo thẳng:** từ 24/08 tới nay có **11 mốc commit**, và **0 tệp nào trong `src/`** bị đụng. Toàn bộ là tài liệu và sổ sách. ⇒ **Sáu việc anh giao chưa động tới mã.** Em nói thẳng vậy.

| # | Việc | Sự thật đo được |
|---|---|---|
| **1** | Đơn hàng | 🟡 **Đã có một phần từ Đợt 5.** Máy trạng thái có **8 trạng thái** thật trong CSDL, quyền theo từng trạng thái đích đã có, và **điều anh chốt (c) đã đúng sẵn**: `đã sản xuất xong` và `đang giao` **tách rời** `đóng`. Còn thiếu: nối `đóng đơn` với **đã thu đủ tiền** |
| **2** | Quyền Trưởng phòng Sản xuất | 🟡 **Vai trò ĐÃ TỒN TẠI** — có 14 menu **chỉ đọc**, **0 quyền hành động**, **0 người dùng**. Gốc: `dm_quy_trinh` có **3 quy trình** (báo giá · đơn hàng · thiết kế), **không có quy trình sản xuất** ⇒ không sinh ra được quyền chuyển trạng thái sản xuất nào |
| **3** | Quyền xem giá vốn | 🔴 Xem §2 — **cần anh quyết** |
| **4** | Giao diện `/m0/security` | ⏸️ Chưa làm. Đã đọc **toàn phần** chuẩn giao diện (521 dòng). ⚠️ Bản nghiệm thu cũ **dùng lại KHÔNG được** — nó chấm trên bản chuẩn 451 dòng, nay là 521 dòng, **lệch 70 dòng** |
| **5** | `/bieu-mau` chuyển vào `/m1` hay `/mf` | ⏸️ Chưa làm — đã audit xong người dùng |
| **6** | `/m0/quy-trinh` dùng để làm gì | ✅ **Trả lời được:** **KHÔNG** phải màn cấu hình suông. Ba dòng trong đó **sinh ra toàn bộ 17 mã quyền chuyển trạng thái** đang được thi hành ở **3 đường ghi nghiệp vụ**, và **điều khiển máy trạng thái lúc chạy**. ⇒ **Đây là màn quan trọng, không được xoá** |

---

## 6. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC — nêu rõ để Notion không nhận nhầm

| Điều | Vì sao chưa chứng minh được | Ai xác minh được |
|---|---|---|
| Mọi số liệu **máy vận hành** | Phiên này **không được cấp kênh đo**. Tất cả số ở trên là **máy phát triển** | Owner cấp kênh đọc-thuần |
| Quy ước trục nào **đúng nghiệp vụ** | Nhãn trên giao diện chỉ ghi trống *"L/W/H (mm)"*, không nói nghĩa. Blueprint duy nhất là **bản demo**, không có công thức | **Chỉ Owner** |
| Bốn con số phụ cấp | Viết cứng trong mã, **không mốc commit nào giải thích lý do**. Cả bốn công thức vào kho trong **cùng một mốc gốc** | **Chỉ Owner** |
| `DEBT-039` (độ rộng cột) | Trên máy phát triển **đã hết lệch** (cả hai đầu đều 255). Nhưng phát hiện **tác dụng phụ chưa ai ghi**: lệnh nới cột **làm mất ràng buộc "không được trống"** | Đo lại trên máy vận hành |

---

## 7. BƯỚC KẾ TIẾP

**Chờ anh trả lời 3 câu ở §0.** Trong lúc chờ, em làm tiếp phần **không cần anh quyết**:

- Vá 2 đường nạp dữ liệu trong ứng dụng đang không có kiểm ngưỡng (§4)
- Sửa lệnh nới cột để không làm mất ràng buộc "không được trống" (§6)
- Sao bản nghiệm thu giao diện mới cho `/m0/security` (§5 việc 4)

---

*Mọi con số trong báo cáo này đã qua kiểm ngược đối kháng độc lập. Con số nào bị bác đã bị loại bỏ, không đưa vào đây.*
