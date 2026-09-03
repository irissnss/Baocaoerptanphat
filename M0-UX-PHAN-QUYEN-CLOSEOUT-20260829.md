# BÁO CÁO — LÀM LẠI MÀN PHÂN QUYỀN: NĂM BƯỚC THÀNH BA PHA

**Gói việc:** `WP-ERP-M0-PERMISSION-UX-FINAL-CLOSEOUT-20260829`
**Owner chỉ thị:** 00:06 · 29/08/2026 — *"giao diện phân quyền hiện tại vẫn rối, không trực quan và khó sử dụng"*
**Bản phát hành:** `V1.00.361`
**Phân loại:** `OWNER_UAT_FAIL` / `UX_REWORK_REQUIRED` — **không** phải lỗi chức năng, **không** phải lùi bảo mật

---

## 1. NGUYÊN TẮC CỦA LƯỢT NÀY

> **Bài kiểm đã xanh KHÔNG được dùng để phản bác nhận xét của Owner.**
>
> Lượt trước có **91/91** bài kiểm xanh mà Owner vẫn bác giao diện. Không có gì mâu thuẫn ở đây:
> mọi bài kiểm đều hỏi *"hệ thống làm đúng chưa"*, không bài nào hỏi *"người dùng làm được không"*.
> Hai câu hỏi khác nhau. Lượt này trả lời câu thứ hai, và trả lời bằng **số đo trên trình duyệt thật**.

---

## 2. ĐO TRƯỚC KHI SỬA — TRÊN MÁY VẬN HÀNH

### 2.1 Màn đầu

| Chỉ tiêu | 1920×1080 | 1440×900 | 390×844 |
|---|---|---|---|
| control nhìn thấy | 12 | 12 | 12 |
| **khối cảnh báo thường trực** | **2** | **2** | **2** |
| phải lăn bao nhiêu màn | 1,3 | 1,5 | **2,6** |

### 2.2 Từng bước (khổ 1920)

| Bước | Tên | Control | Ô tick | Cao cuộn | Phải lăn |
|---|---|---|---|---|---|
| 1 | Tài Khoản | 12 | 0 | 1 356px | 1,3 màn |
| 2 | Vai Trò | **41** | 0 | **1 823px** | 1,7 màn |
| 3 | Màn Hình | 40 | **20** | 1 140px | 1,1 màn |
| 4 | Hành Động · Dữ Liệu · Trường | 34 | **23** | **2 719px** | **2,5 màn** |
| 5 | Xem Lại | 6 | 0 | 1 128px | 1,0 màn |

### 2.3 Chín lỗi cụ thể — nêu đích danh, không nói "trông rối"

| # | Lỗi | Bằng chứng |
|---|---|---|
| 1 | **Ba luồng nghiệp vụ cùng hiện một lúc** ở bước 2 | 41 control một màn |
| 2 | **Năm bước** trong khi luật cho tối đa **ba pha** | `BUOC_PHAN_QUYEN` có 5 mục |
| 3 | **Mã kỹ thuật làm nhãn chính** ở ≥ 7 chỗ | ô chọn vai trò · chip lọc · huy hiệu từng dòng · ô nhập mã · mã quyền hành động · mã chuyển trạng thái |
| 4 | **Thuật ngữ kỹ thuật trong phụ đề trang** | *"…vai trò, **menu permission** và bảo mật tài khoản"* |
| 5 | 🔴 **CÂU SAI VỀ NGỮ NGHĨA** | *"Ô chưa tick = **CẤM**"* — xuất hiện 3 chỗ |
| 6 | 🔴 **Tài khoản quản trị hiện "0 màn · 0 hành động"** | đứng ngay cạnh thẻ ghi vai trò quản trị |
| 7 | **Email làm nhãn chính**, tên người chỉ là huy hiệu nhỏ | bảng gán quyền |
| 8 | **Bức tường cảnh báo thường trực** — 2 khối ở **mọi** bước | bước 4 lặp lại cùng một câu 2 lần |
| 9 | **Không có nơi duy nhất để Xem lại & Áp dụng**, và **không có gói thay đổi / đường hoàn tác** | không tồn tại trong mã |

**Vì sao lỗi 5 nghiêm trọng:** quyền hiệu lực là **hợp các quyền**. Bỏ tick ở một vai trò nghĩa là
*"vai trò NÀY không cấp"* — nếu người đó còn vai trò khác có cấp thì họ **vẫn làm được**.
Viết "CẤM" khiến người quản trị tin là đã thu hồi trong khi chưa. Đó là màn hình nói ngược sự thật.

---

## 3. SAU KHI SỬA

| Chỉ tiêu màn đầu | Trước | Sau |
|---|---|---|
| Ô tick lộ ra | 20–23 (bước 3–4) | **0** |
| Khối cảnh báo thường trực | **2** | **0** |
| Mã vai trò làm nhãn chính | ≥ 7 chỗ | **0** |
| Mã màn làm nhãn chính | có | **0** |
| Thuật ngữ kỹ thuật ở nhãn | có | **0** |
| Số bước điều hướng | 5 | **2 khu** (chính + nâng cao) |
| Nút Áp Dụng | không có | **đúng 1**, thanh dính đáy |
| Tràn ngang (3 khổ màn) | không | **không** |
| Tràn chữ trong khung | — | **0 phần tử** |

### 3.1 Ba pha thay năm bước

**PHA 1 — Chọn người & mục đích.** Gõ **tên**; email và số tài khoản là dòng phụ chữ nhạt.
Mỗi người hiện: trạng thái · vai trò bằng **tên nghiệp vụ** · số màn đang dùng được ·
hoặc *"Chờ cấp quyền — chưa có vai trò nào"*. Bên dưới là **ba thẻ mục đích**.
**Không** ma trận, **không** ô tick, **không** cảnh báo.

**PHA 2 — Cấu hình.** Chỉ hiện phần thuộc mục đích đang chọn:

| Mục đích | Nội dung |
|---|---|
| **Cấp Nhanh Theo Vai Trò** | thẻ vai trò có mô tả *"được làm gì"* + số người đang dùng. **0 ô tick** |
| **Cấp Thêm Quyền Riêng** | nói rõ *"mọi quyền hiện có được **GIỮ NGUYÊN**"* |
| **Thu Hẹp / Thay Mẫu** | nói rõ *"nhân bản thành bản riêng rồi **thay thế**"*, và *"người khác đang dùng nó **không bị đụng tới**"* |

**Tùy chỉnh nâng cao** — cây màn hình có ô tìm, chọn hết/bỏ hết, và **huy hiệu nguồn cấp bằng tiếng Việt**.
**ĐÓNG MẶC ĐỊNH.**

**PHA 3 — Xem lại & Áp dụng.** Ba con số **+ thêm / − mất / = giữ nguyên**; danh sách màn sẽ mất;
danh sách màn **vẫn còn dù đã bỏ chọn** (vì vai trò khác cũng cấp) — chỗ dễ hiểu nhầm nhất;
một nút Áp Dụng, một nút Huỷ, thanh **dính đáy**.

### 3.2 Cảnh báo theo ngữ cảnh, không dựng tường

| Cảnh báo | Chỉ hiện khi |
|---|---|
| Vai trò dùng chung | vai trò đó **thật sự** có người khác mang |
| Bản xem trước đã cũ | người dùng đổi lựa chọn sau khi tính ⇒ **khoá nút Áp Dụng** |
| Áp dụng sẽ gỡ vai trò mẫu | chỉ ở chế độ thu hẹp |

---

## 4. GÓI THAY ĐỔI & HOÀN TÁC — **0 CÂU LỆNH ĐỔI CẤU TRÚC CSDL**

### 4.1 Vì sao không cần bảng mới — bằng chứng lược đồ

Gói việc bắt *"ưu tiên tái dùng kho kiểm toán sẵn có; không tạo nguồn quyền song song"*.
Đã rà **6 bảng nhật ký** trên máy vận hành:

| Bảng | Dòng thật | Có ảnh chụp trước/sau? | Kết luận |
|---|---|---|---|
| `auth_audit_log` | 24 745 | không — chỉ có trường thông điệp 255 ký tự | giữ vai trò *nhật ký thao tác* |
| `permission_log` | 25 | không — ghi lần **kiểm tra** quyền, không phải lần **đổi** | sai loại |
| **`audit_log`** | **0** | **CÓ** — hai trường giá trị cũ/mới kiểu văn bản dài | ✅ **đúng thứ cần**, đã có chỉ mục sẵn |

`audit_log` **đã được dựng sẵn cho đúng việc này** và đã có mã dùng nó từ 27/08/2026.

### 4.2 Trạng thái hoàn tất: giải bằng giao dịch, không bằng cột

`audit_log` không có cột trạng thái. Thay vì thêm cột, dòng nhật ký được ghi **BÊN TRONG cùng
giao dịch** với thay đổi quyền. Giao dịch hỏng ⇒ dòng nhật ký biến mất cùng. Nghĩa là:

> **Mọi dòng tồn tại trong sổ đều là gói ĐÃ HOÀN TẤT.** Không thể có gói nửa vời.

**Hoàn tác** ghi **một dòng đảo riêng**; dòng gốc **không bị sửa, không bị xoá**.
Câu hỏi *"gói này hoàn tác chưa?"* thành *"có dòng đảo nào trỏ tới nó không?"* — tra bằng chỉ mục sẵn có.

### 4.3 Năm chốt đóng-an-toàn khi hoàn tác

1. gói phải tồn tại · 2. chưa từng bị hoàn tác · 3. **trạng thái hiện tại phải còn khớp ảnh chụp sau**
· 4. vai trò cần gỡ phải còn tồn tại · 5. sau khi hoàn tác vẫn phải còn quản trị đăng nhập được.

**Giao diện CẤM ghi "có thể hoàn tác"** nếu không đọc được một gói đủ điều kiện.

---

## 5. BA TỒN ĐỌNG KỸ THUẬT — ĐÓNG DỨT ĐIỂM

### 5.1 Cổng chặn ghi vòng (mới)

Bất biến sở hữu nằm ở **tầng lưu trữ**, nên bất kỳ mã nào viết lệnh CSDL thẳng vào sáu bảng quyền
đều **đi vòng qua nó**. Cổng mới đọc mã nguồn thật và phân loại:

| Nhóm | Số câu |
|---|---|
| **Được phép** — trong tầng bảo mật dùng chung | **9** |
| **Được miễn** — di trú · gieo mầm · kiểm thử · bảo trì một lần, **mỗi tệp một lý do** | 147 |
| **Vi phạm** | **0** |

Kiểm ngược **3/3**. Cổng tự nó cũng lộ một lỗ: bản đầu chỉ quét tệp đã commit nên **bỏ sót tệp mới** —
đã sửa để quét cả tệp chưa commit.

### 5.2 `DEBT-129` — vá **gốc**

**Đo trước khi vá, trên máy vận hành:** `mysql2` · `dotenv` · `tsx` đều **KHÔNG nạp được**
⇒ lệnh lùi, di trú, sao lưu **không chạy nổi** — đúng lúc cần cứu hệ thống nhất thì công cụ cứu lại hỏng.

Nguyên nhân: bước kích hoạt xoá thư mục phụ thuộc ở gốc kho để tiết kiệm đĩa.
**Đo chi phí:** đĩa 49 GB còn trống **40 GB**; cả thư mục ứng dụng **244 MB**.

⇒ **Thôi xoá**, chỉ còn dọn bản dựng trung gian (an toàn — bản chạy thật có gói riêng).
Thêm **tự kiểm phụ thuộc ngay sau khi kích hoạt**; thiếu thì thoát khác 0.

**Chứng minh trên máy vận hành, ngay trong lần triển khai này:**
`ĐẠT nạp được mysql2/promise` · `ĐẠT nạp được dotenv` · `ĐẠT có tsx`.

Kèm bài kiểm tiêm lỗi **11/11**: giấu một phụ thuộc ⇒ tiền kiểm hỏng, **không ghi dấu**,
và bước kích hoạt **từ chối chạy** ⇒ không rơi vào trạng thái mã-mới-dữ-liệu-cũ.

### 5.3 `DEBT-138` — cổng kiểm kiểu

Phân loại **122 lỗi** theo mức quan trọng **thật** (kịch bản nào được chuỗi phát hành gọi):

| Nhóm | Lỗi / tệp | Xử lý |
|---|---|---|
| **1 · PHÁT HÀNH** | 6 / 5 | **SỬA HẾT** đúng hợp đồng kiểu, **không dùng lối tắt kiểu tuỳ ý** |
| 2 · BẢO TRÌ | 0 / 0 | — |
| 3 · KIỂM THỬ | 25 / 7 | xem mục 7 |
| 4 · LỊCH SỬ | 91 / 22 | **cách ly có lý do đo được**: không lệnh nào gọi tới |

Cổng canh **15 tệp**, hiện **0 lỗi**, kiểm ngược chứng minh cổng đỏ được (tiêm một lỗi thật ⇒ 0 → 1).

---

## 6. BẢNG KIỂM

| Bộ | Nội dung | Kết quả |
|---|---|---|
| `test:ux-cong` | 20 tiêu chí nghiệm thu giao diện × 3 khổ màn | **32/32** |
| `test:ux-cong -- --van-hanh` | y hệt, **trên máy vận hành** | **32/32** |
| `test:goi-thay-doi` | gói thay đổi · hoàn tác · xung đột · đồng thời · xem trước chỉ-đọc | **34/34** |
| `test:cong-ghi-quyen` | chặn ghi vòng | **PASS** + kiểm ngược 3/3 |
| `test:kieu-phat-hanh` | kiểm kiểu chuỗi phát hành | **PASS** + kiểm ngược |
| `test:tiem-loi-phu-thuoc` | tiêm lỗi phụ thuộc phát hành | **11/11** |
| `test:d3` · `test:d3-dongthoi` · `test:d3-hopquyen` | hồi quy D3 | 32/32 · 23/23 · 14/14 |
| `test:d3-anh` | hành vi + ảnh trên trình duyệt | **22/22** |
| `test:d3-khoi-van-hanh` | **kiểm khối trên máy vận hành** | **24/24** |
| `test:gov-gates` | cổng quản trị | 37/37 |

### Kiểm ngược — cổng chỉ có giá trị khi gỡ ra thì nó đỏ

| Gỡ cái gì | Kết quả |
|---|---|
| mở sẵn khu nâng cao | cổng giao diện **đỏ đúng** |
| bỏ giao dịch | bài kiểm **đỏ 9 mục** rồi chết — gói thay đổi không sinh nổi |
| bỏ chốt bản-cũ | bài kiểm đồng thời/ghi-đè **đỏ đúng** |
| bỏ chốt hoàn-tác-hai-lần | **đỏ đúng** |
| lách tầng dùng chung | cổng chặn ghi **đỏ đúng** |
| tạo lỗi kiểu trong kịch bản phát hành | cổng kiểm kiểu **đỏ đúng** |
| giấu phụ thuộc phát hành | tiền kiểm **đỏ**, kích hoạt **từ chối** |

⇒ **7/7.** Không có cổng giả.

---

## 7. NHỮNG ĐIỀU NÓI RÕ — KHÔNG GIẤU

### 7.1 Ba lỗi do **chính cổng của lượt này** bắt được

| Lỗi | Vì sao nghiêm trọng | Đã sửa |
|---|---|---|
| **Nút Áp Dụng bị che ở cả ba khổ màn** | ứng dụng đã có **hai** thanh dính đáy; thanh mới đè lên chúng ⇒ người dùng không bấm được nút quan trọng nhất | đo chiều cao thật rồi đẩy lên trên, **không viết cứng con số** |
| **Chốt "không hoàn tác mù vai trò dùng chung" sai phạm vi** | bản đầu chặn mọi lần gỡ một *lượt gán* khi người khác cũng mang vai trò đó. Nhưng gỡ lượt gán chỉ xoá một dòng của riêng người đó — **định nghĩa vai trò và lượt gán của người khác không bị đụng**. Chặn như vậy làm hoàn tác gần như không dùng được mà chẳng bảo vệ ai | sửa lại đúng phạm vi: chặn khi vai trò **không còn tồn tại**, và chốt quản trị cuối |
| **Dọn không sạch trên máy vận hành — sót 3 tài khoản tạm** | bài kiểm xoá tài khoản trước khi xoá dòng sổ mà chính nó vừa ghi; khoá ngoại chặn, lỗi bị nuốt ⇒ **bài kiểm báo xong trong khi dữ liệu còn nằm lại** | dọn tay ngay, máy vận hành về đúng mốc nền; vá thứ tự dọn ở **hai** bộ kiểm |

### 7.2 Một cáo buộc SAI suýt đưa vào báo cáo

Phép thử *"lệnh gửi lên không mang email"* báo đỏ. Nếu dừng ở đó, báo cáo này đã ghi
*"ứng dụng làm rò rỉ email"*. **Truy tới cùng thì không phải:** mở trang bằng trình duyệt sạch
và ghi lại đích của mọi yêu cầu ⇒ toàn bộ lưu lượng không thuộc ứng dụng đi tới
**mô-đun bảo vệ web của phần mềm diệt vi-rút trên chính máy phát triển**, nó gửi lại nội dung trang.
Ứng dụng chỉ gửi **số định danh**. Bài kiểm nay tách rõ *"yêu cầu tới máy chủ ứng dụng"*
khỏi *"yêu cầu ra ngoài"*.

### 7.3 Điều CHƯA làm được

| Việc | Vì sao |
|---|---|
| **So sánh hồi quy theo điểm ảnh** | chưa có ảnh nền + ngưỡng sai lệch tự động. Nghiệm thu giao diện lượt này dựa trên **số đo trên trang thật** (control · ô tick · cảnh báo · tràn · vị trí và độ che của nút) + ảnh đối chiếu bằng mắt. **Không tự khai là đã có.** Ghi `DEBT-139` |
| 3 lỗi kiểu ở một kịch bản kiểm khối | sửa được, nhưng phải đổi **kiểu của M1** — gói việc **cấm sửa M1 cùng commit**. Ghi `DEBT-140`, **không lách** |
| 25 lỗi kiểu ở nhóm kiểm thử | ngoài danh sách bắt buộc của gói việc (build · di trú · triển khai · lùi · sao lưu · kiểm khối). Ghi rõ, không giấu |

### 7.4 Một quyết định vận hành — nêu rõ

Lần triển khai đầu **bị ngắt vì hết giờ chờ** ở bước cài phụ thuộc. Kiểm ngay:
máy vận hành **vẫn phục vụ bản cũ**, trang sống, không tiến trình treo ⇒ **an toàn**, không phải sự cố.
Chạy lại ở chế độ nền cho tới khi xong.

Cổng kích hoạt đòi bước nạp dữ liệu; đã **đo** rồi mới quyết dùng lối ra mà chính cổng đó quy định:
bốn đợt nạp trước **đã có đủ**, và bản phát hành này **0 dòng cần nạp bù**.
Bản kê ghi lại quyết định đó nên **tra lại được**.

---

## 8. ĐIỀU CỐ Ý KHÔNG LÀM

`DEBT-128` phần còn lại · đổi khoá chính bảng gán quyền · 13 điểm tra quyền theo email ·
108 cột quy kết · `DEBT-133` · Pricing Plan · sửa M1/M3/M4/M5/MF cùng commit ·
kho quyền song song · trigger CSDL · viết cứng vai trò theo tên người.

---

## 9. SỔ SÁCH

| Sổ | Nội dung |
|---|---|
| Sổ Yêu Cầu Owner | mục **#192** (chỉ thị + kết quả) · mục **#193** (bốn điều tự khai) |
| Sổ nợ kỹ thuật | **đóng** `DEBT-129` · **đóng** `DEBT-138` · **thêm** `DEBT-139` · **thêm** `DEBT-140` |
| Bản đồ hiện trạng + wireframe + tiêu chí | `docs/reports/UX-PHAN-QUYEN-BAN-DO-VA-WIREFRAME-20260829.md` — lập **trước** khi sửa dòng mã đầu tiên |

---

## 10. GÓI BÀN GIAO CHO NOTION

| Trường | Nội dung |
|---|---|
| **Mã mục sổ** | `#192` · `#193` |
| **Nguyên văn Owner + mốc thật** | *"giao diện phân quyền hiện tại vẫn rối, không trực quan và khó sử dụng"* — **00:06 · 29/08/2026** |
| **Phạm vi áp dụng** | màn `/m0/security`; tầng bảo mật; chuỗi phát hành |
| **Điều CẤM mở rộng** | xem mục 8 |
| **Bằng chứng** | `V1.00.361` · mã nguồn triển khai ghi trong bản kê trên máy vận hành · lớp bằng chứng **RUNTIME_PROVEN** |
| **Trang Notion cần sửa** | **CHƯA XÁC ĐỊNH** — cần Agent Notion xác định trang mô tả màn phân quyền M0 |
| **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | ① chưa có so sánh hồi quy theo **điểm ảnh** ② bất biến sở hữu do **tầng lưu trữ** giữ, không phải CSDL — ai viết đường ghi **thứ ba** không qua tầng đó vẫn lách được; cổng chặn ghi bắt được ở mức **mã nguồn**, không bắt được lúc chạy ③ chưa đo ở quy mô hàng trăm vai trò riêng ④ *"dễ dùng hơn"* mới chứng minh được bằng **số đo** (control · ô tick · cảnh báo · số bước), **chưa** có Owner nghiệm thu lại bằng mắt |

---

## 11. CÒN LẠI

`DEBT-139` (chưa có hồi quy điểm ảnh) · `DEBT-140` (kiểu M1 quá chặt) ·
`DEBT-128` giữ **MỞ** đúng chỉ đạo không gộp.

---

---

## 11b. LƯỢT HAI — OWNER XEM RỒI HỎI BỐN VIỆC (`V1.00.362`)

Owner mở `V1.00.361` trên **cả hai môi trường**, chụp ảnh, **khoanh đỏ ba thẻ thống kê**
ở đầu màn, và hỏi bốn câu. **Owner đúng cả bốn.**

### 11b.1 "Deploy full chưa? Sao không nhất quán local và VPS?"

**Deploy đã đủ.** Đo lại hai môi trường:

| | Máy phát triển | Máy vận hành |
|---|---|---|
| Phiên bản | `V1.00.361` | `V1.00.361` ✓ |
| **Vai trò** | **9** | **9** ✓ |
| Kho dữ liệu | `…_dev` | kho thật |
| Tài khoản | 15 (mã 1–65) | 9 (mã 1–9) |
| Dòng quyền màn hình | 163 | 148 |

Khác nhau là **DỮ LIỆU**, không phải mã nguồn. Chênh **đúng 15 dòng** ở **6 vai trò**:

| Vai trò | Máy phát triển | Máy vận hành | Chênh |
|---|---|---|---|
| CEO | 47 | 43 | 4 |
| TP_SAN_XUAT | 14 | 11 | 3 |
| TP_THIET_KE | 10 | 7 | 3 |
| TP_KINH_DOANH | 11 | 8 | 3 |
| HR | 5 | 4 | 1 |
| SALES | 11 | 10 | 1 |
| ADMIN · KE_TOAN · USER | 51 · 13 · 1 | 51 · 13 · 1 | **khớp** |

⇒ Dữ liệu thử tích luỹ trên máy phát triển. **Không ảnh hưởng máy vận hành**, nhưng
là rủi ro thật cho việc kiểm thử ⇒ ghi **`DEBT-141`**.

### 11b.2 Ba thẻ Owner khoanh đỏ — em BỎ SÓT

Lượt trước dọn thuật ngữ kỹ thuật ở **phần dưới** màn mà **để sót ba thẻ trên cùng**
(*Vai trò hệ thống* · *Quyền menu đã cấp*). **Dọn nửa vời còn khó hiểu hơn không dọn.**
Và chính ba con số đó khác nhau giữa hai môi trường, nên đặt ở chỗ trang trọng nhất
màn khiến người xem tưởng hệ thống chưa đồng bộ.

**Nay:** dời vào khu **Quản Lý Nâng Cao** kèm câu giải thích tại sao lệch;
thay bằng **dải ba bước dẫn việc** + nút mở trang hướng dẫn.

### 11b.3 Trang hướng dẫn `/m0/security/huong-dan`

Owner: *"lâu lâu mới dùng menu này nhưng nó là menu cực kỳ quan trọng"*.

| Phần | Nội dung |
|---|---|
| Một điều cần nhớ | quyền là **tổng** các vai trò — kèm ví dụ thật |
| Ba tình huống | **22 bước đánh số**, mỗi bước ghi rõ bấm gì và thấy gì |
| **Bảng tra** | *"muốn gì thì dùng cách nào"* + cột **"có làm mất quyền không"** — đúng câu Owner hỏi về sửa quyền người cũ |
| Hoàn tác | cách làm + **ba lý do** khi không hoàn tác được |
| Hệ thống tự chặn | bốn việc Owner **không cần nhớ** |
| Hai thắc mắc | vì sao quản trị hiện *"Toàn quyền"* · vì sao số liệu hai môi trường khác nhau |
| Khi nào gọi kỹ thuật | bốn tình huống |

**Bằng chứng:** `test:ux-huong-dan` **21/21** trên máy phát triển **và 21/21 trên máy vận hành**
(vào được từ màn chính · đủ ba tình huống · bước đánh số · nói rõ việc làm mất quyền ·
0 mã kỹ thuật · không tràn ở 3 khổ màn · người không quyền bị đẩy sang `/403`).
`test:ux-cong` **32/32** trên cả hai, không vỡ.

### 11b.4 Hai lỗi của chính bài kiểm — không phải lỗi trang

| Lỗi | Nguyên nhân |
|---|---|
| Bảng tra báo "không tìm thấy" | tiêu đề bảng dùng `uppercase` của CSS, nên `innerText` trả về **chữ HOA**; so khớp phân biệt hoa-thường nên trượt dù nội dung có thật |
| Chữ "TICK" bị bắt là mã kỹ thuật | phép dò mã kỹ thuật bắt mọi cụm chữ hoa ≥ 4 ký tự. Đổi thành chữ thường — đọc cũng mượt hơn |

---

## 12. KHỐI BÁO CÁO KẾT THÚC (`GOV-COMPLETION-REPORT-001`)

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Màn phân quyền: 5 bước -> 3 pha. Màn đầu 0 ô tick, 0 khối cảnh báo, 0 mã kỹ thuật
   - Ba mục đích rõ ràng; chỉ hiện phần thuộc mục đích đang chọn
   - Ma trận chi tiết lui về khu nâng cao, ĐÓNG MẶC ĐỊNH — không xoá chức năng nào
   - Gói thay đổi + hoàn tác đóng-an-toàn, tái dùng `audit_log`, 0 câu DDL
   - Sửa 2 câu nói sai: "bỏ tick = cấm" và quản trị "0 quyền"
   - Cổng chặn ghi vòng (mới) · cổng kiểm kiểu (mới) · bài kiểm tiêm lỗi phụ thuộc (mới)
   - DEBT-129 vá GỐC · DEBT-138 đóng
   - Gỡ `the-vai-tro-rieng.tsx` (đã bị thay thế) kèm con trỏ tra lại

2. PHẠM VI
   ĐỤNG    : src/app/m0/security/{security-client,trung-tam-phan-quyen,actions,
             ma-tran-menu-cay} · src/lib/security/{ap-dung-quyen,goi-thay-doi-quyen} ·
             src/lib/version.ts · scripts/{vps-activate-standalone,preflight-schema-contract,
             rollback-d3-*,run-m0-security-migration,run-pl4-migrations,smoke-mf} ·
             scripts/tests/* (6 tệp mới, 4 tệp sửa) · package.json · sổ Owner · sổ nợ · 2 báo cáo
   KHÔNG ĐỤNG: LƯỢC ĐỒ CSDL (0 câu DDL) · user_role_mapping (khoá chính) ·
             13 điểm tra quyền theo email · 108 cột quy kết · M1/M3/M4/M5/MF ·
             Pricing Plan · trigger (0 trước, 0 sau) · quyền tài khoản người thật

3. BẰNG CHỨNG
   npm run test:ux-cong                  -> 32/32  -> UI_PROVEN
   npm run test:ux-cong -- --van-hanh    -> 32/32  -> RUNTIME_PROVEN (máy vận hành)
   npm run test:goi-thay-doi             -> 34/34  -> DB_PROVEN
   npm run test:cong-ghi-quyen           -> PASS, 0 vi phạm + kiểm ngược 3/3 -> CODE_PROVEN
   npm run test:kieu-phat-hanh           -> PASS + kiểm ngược -> CODE_PROVEN
   npm run test:tiem-loi-phu-thuoc       -> 11/11  -> RUNTIME_PROVEN
   npm run test:d3-khoi-van-hanh         -> 24/24  -> RUNTIME_PROVEN (máy vận hành)
   npm run test:d3 / dongthoi / hopquyen -> 32/32 · 23/23 · 14/14 -> DB_PROVEN
   npm run test:d3-anh                   -> 22/22  -> UI_PROVEN
   npm run test:gov-gates                -> 37/37  -> FILE_PROVEN
   npm run build                         -> Compiled successfully -> CODE_PROVEN
   KIỂM NGƯỢC LƯỢT NÀY: 7/7 chốt đều đỏ được khi gỡ ra
   Bản kê máy vận hành: APP_VERSION=V1.00.361, mã nguồn khớp cả bốn nơi

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #192 (chỉ thị + kết quả) và #193 (bốn điều tự khai)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho BÁO CÁO CÔNG KHAI `Baocaoerptanphat` · commit <mã-nguồn-riêng>
       file M0-UX-PHAN-QUYEN-CLOSEOUT-20260829.md
       Kho MÃ NGUỒN: commit <mã-nguồn-riêng> là commit
       cuối của phần mã; bản sao báo cáo nằm ở commit ngay sau đó (một trường
       không thể trích dẫn mã commit chứa chính nó).

6. CÒN SÓT / CHƯA LÀM
   - DEBT-139: chưa có so sánh hồi quy theo ĐIỂM ẢNH (đã CẤM tự khai là có)
   - DEBT-140: 3 lỗi kiểu ở kịch bản kiểm khối — phải đổi kiểu M1, gói việc cấm
   - 25 lỗi kiểu nhóm KIỂM THỬ (ngoài danh sách bắt buộc của gói việc)
   - 91 lỗi kiểu nhóm LỊCH SỬ — cách ly có lý do đo được
   - DEBT-128 giữ MỞ đúng chỉ đạo không gộp

7. ĐANG CHỜ OWNER
   - Trang Notion nào mô tả màn phân quyền M0 -> để bàn giao mục #192.
     Chặn: chỉ chặn khâu đồng bộ tài liệu, KHÔNG chặn vận hành.
   - Owner nghiệm thu lại bằng MẮT trên máy vận hành. Số đo nói màn đã gọn hơn
     nhiều, nhưng "dễ dùng" thì chỉ Owner mới kết luận được.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   M1 operational closeout.

9. CHƯA XÁC MINH ĐƯỢC
   - "Dễ dùng hơn" — vì sao: đo được số control/ô tick/cảnh báo/bước, KHÔNG đo
     được cảm nhận. Ai xác minh: Owner, bằng mắt, trên máy vận hành.
   - Đường ghi THỨ BA không qua tầng lưu trữ vẫn lách được bất biến — cổng chặn
     ghi bắt ở mức MÃ NGUỒN, không bắt lúc chạy. Ai xác minh: cổng lúc chạy (chưa có).
   - Quy mô hàng trăm vai trò riêng — máy vận hành hiện 9 vai trò, 0 vai trò riêng thật.

10. TRẠNG THÁI CHUNG
   [x] PASS — đủ bằng chứng, không còn việc chặn

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Tài liệu ĐÃ ĐỌC LẠI sau nén:
     - docs/UI-STANDARD.md — TOÀN PHẦN, dòng 1-522
     - docs/UI-ACCEPTANCE-CHECKLIST.md
     - .governance/registry/tech-debt.md — tra trước khi ghi DEBT-139/140
     - docs/OWNER-REQUEST-LEDGER.md — tra mục #191 trước khi ghi #192
     - src/lib/security/nhat-ky-doi-quyen.ts — trước khi thiết kế gói thay đổi
     - scripts/vps-{activate-standalone,pull-and-restart,preflight-release,
       nap-du-lieu-phat-hanh}.sh — trước khi vá gốc DEBT-129
     - src/app/m0/security/security-client.tsx — toàn bộ, trước khi tái cấu trúc
═══════════════════════════════════════════
```

---

*Báo cáo này công khai được: không chứa email thật, mật khẩu, thẻ phiên, chuỗi kết nối,
định danh hạ tầng máy chủ, hay dữ liệu khách hàng.*
