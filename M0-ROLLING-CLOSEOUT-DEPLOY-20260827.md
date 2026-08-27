# BÁO CÁO — ĐÓNG CUỐN CHIẾU M0 · VÀ VÌ SAO VIỆC TRIỂN KHAI ĐANG MỆT MỎI

**Mã việc:** `M0-ROLLING-CLOSEOUT-20260827`
**Ngày:** 27/08/2026
**Trạng thái triển khai:** ⛔ **NOT_DEPLOYED** — bản phát hành đã dựng xong và kiểm sạch, **chờ Owner chốt một việc**

---

## 0. ĐỌC PHẦN NÀY LÀ ĐỦ

Anh nói đúng, và em xác nhận bằng số đo: **vấn đề không nằm ở "local", mà ở chỗ máy vận hành
đã rẽ sang một nhánh bên và chưa bao giờ được kéo về.**

- Máy vận hành đứng ở `826817b` — **nhánh vá khẩn cấp** tách từ điểm cũ `0e73a7c`.
- Nó **cách `main` 70 commit** (số cũ 53 đã lỗi thời — em ghi lại cho đúng).
- Nhánh đó sinh ra ngày 27/08 theo đúng khoá của anh *«không kéo theo khoảng cách 53 commit»*
  khi chữa cháy lỗ API. **Lúc đó là quyết định đúng.** Nhưng hệ quả là từ đó mỗi lần đưa gì lên
  cũng thành mổ xẻ từng mảnh — đúng như anh mô tả.

**Đo được trong chính lượt này, máy vận hành KHÔNG HỀ CÓ:**

| Thứ thiếu | Thuộc | Hệ quả |
|---|---|---|
| `src/lib/security/menu-catalog.ts` | Đợt 1 | bản vá thanh bên **không dịch được** |
| `canViewManHinh` | Đợt 1/2 | máy chủ **chưa có** mô hình quyền theo từng màn |
| `requireManHinhView` | Đợt 1/2 | 7 trang Tài chính gác bằng khoá **module** |
| 2 cột trong danh mục che giá vốn | `9667995` | **giá vốn 2 chỗ chưa được che** (mục 6) |

⇒ **Toàn bộ Đợt 1, Đợt 2, Đợt 4, Đợt 5 chưa từng lên máy vận hành.**

**Một việc cần anh quyết — chỉ một:** làm **BẢN PHÁT HÀNH HỘI TỤ**, kéo máy vận hành lên
ngang `main`, rồi từ đó mỗi lần triển khai chỉ còn *đẩy main → chạy*. Chi tiết ở mục 10.

---

## 1. MÃ COMMIT

| Mốc | Mã |
|---|---|
| `main` đầu phiên | `83abc4b` |
| `main` cuối phiên | `45807fb` |
| Nhánh phát hành cách ly | `release/m0-closeout-20260827` |
| **Bản phát hành cuối** | **`a8f3283`** (dựng từ neo `826817b`) |
| Neo máy vận hành | `826817b` — **không đổi** |
| **Đã triển khai** | **KHÔNG** — `NOT_DEPLOYED` |

---

## 2. VÌ SAO EM DỪNG, KHÔNG TỰ ĐẨY LÊN

Anh đã cho GO. Em vẫn dừng, vì **ba lý do đủ nghiêm túc để không tự quyết**:

1. **Nội dung bản phát hành đã đổi so với thứ anh duyệt.** Anh duyệt *«port f50a99c changes»*.
   Rà phụ thuộc cho thấy chỉ port được **một phần** — bản vá thanh bên phải để lại vì máy vận
   hành thiếu Đợt 1/2. Đó là thay đổi **bản chất**, không phải chi tiết.
2. **Đường triển khai chuẩn của dự án chạy `migrate:*` trên máy vận hành.** Anh khoá *«Không DDL»*.
   Em **không** lách bằng cách tự sửa kịch bản triển khai.
3. Anh vừa nói dừng và báo cáo.

**Bản phát hành đã sẵn sàng, chạy được bất cứ lúc nào anh gật** — không phải làm lại từ đầu.

---

## 3. BỐN KHOẢN NỢ ĐÓNG, MỘT HẠ CẤP ĐỘ

| Mã | Kết quả | Nội dung |
|---|---|---|
| `DEBT-124` | **CLOSED_AS_EXPECTED** | Không có render trùng. **Số liệu ban đầu của chính em là sai** (mục 5) |
| `DEBT-125` | **FIXED** | Hai lỗi: một ở sản phẩm, một ở cổng kiểm |
| `DEBT-126` | **FIXED** | Không bao giờ còn 0 người khôi phục hệ thống |
| `DEBT-127` | **FIXED** | Hợp-các-quyền ở tầng trường, theo phân xử của anh |
| `DEBT-128` | **HẠ CẤP ĐỘ** | **Bản ghi nợ ban đầu của em cũng sai** (mục 5) |

### 3.1 `DEBT-126` — chốt chặn quản trị khôi phục cuối cùng

Hai lỗ cộng lại thành một ca mất hệ thống: khoá tài khoản **không có chốt chặn nào**, và phép
đếm quản trị **không lọc trạng thái** nên người đã bị khoá vẫn được tính là còn.

Nay **một bất biến dùng chung** cho cả đường khoá tài khoản lẫn đường gỡ vai trò, đếm đủ ba
điều kiện: có vai trò quản trị · đang hoạt động · không bị khoá. Chặn **trước khi ghi**.

Cổng mới **18/18**, có ca **đăng nhập THẬT của người thay thế**. Kiểm ngược gỡ vá → **8 đỏ**.

### 3.2 `DEBT-127` — hợp các quyền ở tầng trường

Theo phân xử của anh: quyết định 27/08 **thay** quyết định 17/07 trong phạm vi này.

⚠️ **Tác động lên dữ liệu hiện có = 0.** Bảng quyền-trường có đúng 4 dòng, cả 4 là CEO
`can_view=1`, **không tồn tại dòng cấm nào**. Đây là chỉnh **ngữ nghĩa**, không nới quyền —
chứng minh bằng chiều ngược: SALES và người không vai trò **vẫn bị che** cả 4 cột.

Cổng mới **16/16**. Kiểm ngược trả về cấm-thắng → **đúng 2 khẳng định phân biệt đỏ**, mọi
khẳng định "không nới quyền" vẫn xanh ở cả hai chiều.

### 3.3 `DEBT-125` — hai lỗi, một ở sản phẩm

**(a) Lỗi sản phẩm:** cổng gác API dùng mã hành động **giả** `__chi_can_dang_nhap__` để mượn
đường xác thực. Ngoài việc làm cổng đỏ, nó khiến hệ thống **ghi một dòng nhật ký từ-chối GIẢ**
cho **mọi người dùng thường** gọi tuyến "chỉ cần đăng nhập". Nhật ký kiểm toán bị nhiễu. Đã bỏ.

**(b) Điểm mù cổng kiểm:** bộ quét chỉ nhìn điểm gọi, không thấy mã nằm trong **bảng khai tuyến**.

Cổng: **56/58 → 59/59**. Hai đối chứng đều bắn đúng.

---

## 4. BA LỖI TRONG CHÍNH CÔNG CỤ CỦA EM — KIỂM NGƯỢC BẮT ĐƯỢC

Ghi lại vì đây là loại lỗi làm cổng **xanh giả** — nguy hiểm hơn cổng đỏ.

**4.1 Ký tự lùi (0x08) lẫn trong biểu thức chính quy.**
Bản sửa cổng đầu tiên của em **không khớp gì cả**, mà cổng vẫn xanh. Phát hiện bằng đối chứng
(cắm mã bịa → cổng **không** đỏ), truy ra bằng `cat -A`: regex là `/‹BS›hanhDong…/`.
Đã thêm khẳng định **`[D1a2]` cổng tự kiểm chính nó**: nhặt được 0 mã từ bảng khai ⇒ đỏ.

**4.2 Kịch bản chụp ảnh kết luận sai.**
Nó báo *"CEO không thấy màn tài chính nào"*. Sai — mục con **chỉ nằm trong DOM khi ngăn xếp mở**.
Dấu hiệu nhận ra: **ngay cả quản trị cũng không hiện**.

**4.3 Hai khoản nợ ghi bằng suy diễn thay vì đo** — mục 5.

---

## 5. HAI KHOẢN NỢ EM GHI SAI — SỬA LẠI

### `DEBT-124` — con số 5 là sai
Em ghi *«5 khoá xuất hiện HAI LẦN»*, đo bằng biểu thức chính quy trên tệp cấu hình, **bỏ qua cờ
`visible`**. Đo lại bằng **đối tượng lúc chạy**: bốn trong năm khoá mang `visible: false` nên
**chưa từng render hai lần**. Chỉ `m6` xuất hiện hai nơi, và đó là mẫu **đầu-ngăn-không-bấm-được**
có chủ đích.

Đã thay việc **ghim số** bằng **ba bất biến đo được**: không render trùng trong cùng ngăn ·
mỗi khoá đúng một định danh quyền · mỗi lối tắt đúng một chủ sở hữu hiển thị.

### `DEBT-128` — "mất vai trò" là sai
Em ghi *«đổi email là cắt đứt toàn bộ vai trò»*. **Sai.** Khoá ngoại mang `ON UPDATE CASCADE`.
**Thực nghiệm**: tạo tài khoản thử → gán vai trò → đổi email → **vai trò theo email mới, 1/1**.

Rủi ro thật nhỏ hơn nhưng có: **99 cột chữ** chứa email **không có khoá ngoại** ⇒ giữ giá trị cũ.
Đây là vấn đề **quy kết lịch sử**, không phải mất quyền.

Gói di trú đã lập đủ 10 mục: `docs/reports/DEBT-128-GOI-DI-TRU-20260827.md`. **Chưa chạy DDL.**

---

## 6. 🔴 MỘT LỖ ĐANG TỒN TẠI TRÊN MÁY VẬN HÀNH — PHÁT HIỆN KHI KIỂM BẢN PHÁT HÀNH

Chạy cổng trên **chính bản phát hành** làm lộ ra điều mà chạy trên `main` không thấy được:

```
FAIL  8b. SALES KHÔNG xem được cột giá vốn nào
```

Truy nguyên: danh mục cột nhạy cảm trên máy vận hành **chỉ có 2 cột**, `main` có **4**.

| Máy vận hành (`826817b`) | `main` |
|---|---|
| `bao_gia_option.gia_von` | `bao_gia_option.gia_von` |
| `don_hang_item.gia_von` | `don_hang_item.gia_von` |
| — | **`material_item.gia_von_trung_binh`** |
| — | **`pricing_quote_history.gia_von`** |

Và trên máy vận hành, việc che **chỉ được gọi** ở hai màn `bao_gia` và `don_hang`.
⇒ **Hai cột giá vốn kia hiện đang KHÔNG được che trên máy vận hành.**

Bản vá là commit `9667995` (*«che gia von o 2 cot con lot luoi»* — quyết định của anh, sổ #163).
Nó đã nằm trên `main` **từ lâu** nhưng **chưa bao giờ được triển khai** — vì máy vận hành đang ở nhánh bên.

> **Đây là bằng chứng cụ thể nhất cho việc phải hội tụ.** Không phải chuyện lý thuyết:
> một quyết định anh đã chốt, đã code, đã kiểm, mà người dùng thật vẫn chưa được bảo vệ.
>
> Em **không tự thêm** commit này vào bản phát hành, vì nó cần cả điểm gọi ở màn vật tư
> (`src/app/m1/material-item/*`) — nằm trong khoảng 70 commit. Đây là việc của bản hội tụ.

---

## 7. BẢN PHÁT HÀNH ĐÃ DỰNG — NỘI DUNG CHÍNH XÁC

Từ neo `826817b`, áp **đúng hai commit**, **không** chép cả tệp:

```
a8f3283  release(m0): tach ban va thanh ben ra khoi ban phat hanh
420cc55  fix(m0): DONG DEBT-124..127 + HA CAP DO DEBT-128
77768f7  fix(m0): VA THANH BEN ... (phan thanh ben da duoc go o a8f3283)
826817b  ← neo may van hanh
```

**Sáu tệp đổi, 240 dòng thêm:**

| Tệp | Phần |
|---|---|
| `src/lib/security-store.ts` | `DEBT-126` chốt chặn quản trị cuối |
| `src/lib/action-permission.ts` | `DEBT-127` hợp các quyền |
| `src/lib/api-guard.ts` | `DEBT-125` bỏ mã canh giả |
| `src/app/m0/security/actions.ts` · `security-client.tsx` · `tam-ngung.ts` | tạm ngưng nút ghi đè quyền hàng loạt |

**Chốt chặn đã kiểm:** `security-client.tsx` chỉ **+23 dòng** (nếu chép cả tệp sẽ là **+120** và
**kéo theo Đợt 4 + Đợt 5** chưa từng triển khai). Đã khẳng định bằng máy: `MaTranMenuCay` và
`MaTranChuyenTrangThai` **không xuất hiện** trong bản phát hành.

### Kiểm trên chính bản phát hành

| Cổng | Kết quả |
|---|---|
| kiểm kiểu | **sạch** |
| `test:p1-quan-tri-khoi-phuc` | **18/18** |
| `test:p1-ap-mau-quyen` | **8/8** |
| `test:ma-tran-quyen-hd` | **59/59** |
| `test:p1-hop-quyen-truong` | 15/16 — **1 đỏ là lỗ SẴN CÓ của máy vận hành**, mục 6 |
| cổng bí mật · PII | **PASS** |

---

## 8. KIỂM TRÊN `main` — 424 KHẲNG ĐỊNH, 0 HỎNG

| Cổng | | Cổng | |
|---|---|---|---|
| `p1-thanh-ben` | 15/15 | `p1-api-phien` | 13/13 |
| `p1-ap-mau-quyen` | 8/8 | `p1-api-matrix` | 6/6 |
| `p1-quan-tri-khoi-phuc` | 18/18 | `quyen-vai-tro` | 17/17 |
| `p1-hop-quyen-truong` | 16/16 | `m1-rbac` | 111/111 |
| `tach-form-m0` | 25/25 | `m1-menu` | 40/40 |
| `p1-menu` | 11/11 | `ma-tran-quyen-hd` | **59/59** |
| `p1-ban-giao` | 14/14 | kiểm kiểu | sạch |
| `p1-gia-von` | 45/45 | bí mật · PII · tham chiếu | PASS |
| `p1-giu-gia-von` | 11/11 | đồng bộ 5 file luật | PASS |

Lint: 847 vấn đề **toàn kho có sẵn**; riêng 8 tệp em đụng: **0 lỗi**, 5 cảnh báo.

---

## 9. CHỨNG MINH TRÊN TRÌNH DUYỆT — 15/15, SÁU ẢNH

Chrome thật, đăng nhập thật, **vai trò thật trong CSDL** (chỉ ca "chỉ có Hợp Đồng" phải dựng tạm):

| Ca | Đo được |
|---|---|
| Quản trị | 52 liên kết, có `/mf` · `/bieu-mau` · `/mc` |
| **CEO** | 44 liên kết, **đủ 7** `/mf/*`, **KHÔNG có** `/mf` — đúng ca anh khoá, **trên dữ liệu thật** |
| Chỉ 1 màn Tài chính | **đúng 1** (`/mf/cong-no`), không rò sang trang tổng |
| Chỉ Hợp Đồng | thấy `/mc`, **0** liên kết `/mf*` |
| Chờ cấp phát | **0** mục nghiệp vụ |
| Nút áp mẫu quyền | **mờ**, nêu rõ lý do |

Ảnh 1920×1080: `docs/anh-kiem-thu/m0-closeout-thanh-ben-20260827/` (thư mục đã bị git bỏ qua).

---

## 10. VIỆC DUY NHẤT CẦN ANH QUYẾT

**Làm bản phát hành HỘI TỤ hay không.**

| | Hội tụ (đề xuất) | Giữ như hiện nay |
|---|---|---|
| Máy vận hành | kéo lên ngang `main` | đứng ở nhánh bên |
| Lần triển khai sau | *đẩy main → chạy* | mổ xẻ từng mảnh, như lượt này |
| Lỗ giá vốn mục 6 | **được vá** | **vẫn còn** |
| Đợt 1·2·4·5 | lên máy vận hành | vẫn nằm im |
| Chi phí | **một** đợt kiểm như phát hành thật + có DDL | mỗi lượt lại mất thời gian như lượt này |

**Em đề xuất hội tụ**, và làm một lần cho xong. Nếu anh gật, em sẽ trình kế hoạch có: sao lưu ·
danh sách bản di trú phải chạy · thứ tự · cách lùi · bộ kiểm khói. **Không tự chạy.**

Nếu anh muốn đẩy **ngay** 4 phần đã kiểm ở mục 7 (không DDL, rủi ro thấp, trong đó có chốt chặn
chống mất quyền quản trị) thì nói một câu, em chạy — bản phát hành đã nằm sẵn.

---

## 11. MODULE KẾ TIẾP SAU M0

**M3 — Báo giá & Đơn hàng.** Chỉ một, không mở song song.

Còn tồn của M0, đã ghi sổ, **không chặn** việc chuyển module:
ba màn con M3 chưa che giá vốn · nâng bản vá lưu báo giá lên mã tuỳ chọn ổn định ·
lệch dữ liệu máy vận hành (3 vai trò · 28 menu · ~100 dòng quyền) · `DEBT-116` · `DEBT-121` · `DEBT-122` · `DEBT-123` · `DEBT-128`.

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Đóng DEBT-126 (chốt chặn quản trị khôi phục cuối cùng, một bất biến dùng
     chung cho cả đường khoá tài khoản lẫn đường gỡ vai trò)
   - Đóng DEBT-127 (hợp các quyền ở tầng trường, theo phân xử của Owner)
   - Đóng DEBT-125 (bỏ mã canh giả trong cổng gác API — nó còn ghi nhật ký
     từ-chối GIẢ cho mọi người dùng thường; và dạy cổng kiểm đọc bảng khai tuyến)
   - Đóng DEBT-124 CLOSED_AS_EXPECTED, sửa lại số liệu sai của chính khoản nợ
   - Hạ cấp độ DEBT-128 sau thực nghiệm, lập gói di trú đủ 10 mục
   - Viết 3 cổng kiểm mới + 1 kịch bản chụp ảnh trình duyệt; kiểm ngược từng cái
   - Thêm [D1a2] cho cổng ma trận quyền TỰ KIỂM CHÍNH NÓ
   - Dựng bản phát hành cách ly từ neo 826817b, tách phần không port được
   - Viết wireframe cuối, NHẬN LẠI ý gom điều hướng của Owner
   - Ghi sổ Owner #184 và #185

2. PHẠM VI
   ĐỤNG    : src/lib/security-store.ts · src/lib/action-permission.ts
             src/lib/api-guard.ts · scripts/tests/ (3 tệp mới + 2 tệp sửa)
             .governance/registry/tech-debt.md · docs/OWNER-REQUEST-LEDGER.md
             docs/reports/ (3 tệp mới) · package.json
   KHÔNG ĐỤNG: lược đồ CSDL (0 DDL) · dữ liệu quyền (0 dòng đổi)
             · máy vận hành (KHÔNG triển khai) · công thức giá · 5 file luật

3. BẰNG CHỨNG
   424 khẳng định trên main, 0 hỏng                    → CODE + DB_PROVEN
   4 cổng trên chính bản phát hành                     → CODE + DB_PROVEN
   kiểm ngược DEBT-126 → 8 đỏ · DEBT-127 → 2 đỏ        → CODE_PROVEN
   kiểm ngược DEBT-125 → 2 đối chứng bắn đúng          → CODE_PROVEN
   chụp ảnh trình duyệt 15/15, 6 ảnh 1920×1080         → UI_PROVEN
   thực nghiệm đổi email (cascade)                     → DB_PROVEN
   role_menu_permission sửa gần nhất 23/08, 0 dòng đổi → DB_PROVEN
   tsc sạch · secret-scan · pii-scan · check:governance → FILE_PROVEN
   ⚠ KHÔNG có RUNTIME_PROVEN cho máy vận hành — chưa triển khai

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #184 (mười khoá + GO triển khai)
   [x] ĐÃ GHI — mục #185 (Owner phản đối quy trình, NGUYÊN VĂN hai lượt)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — xem dòng cuối báo cáo

6. CÒN SÓT / CHƯA LÀM
   - CHƯA triển khai bản phát hành (lý do ở mục 2 của báo cáo)
   - CHƯA port bản vá thanh bên (máy vận hành thiếu Đợt 1/2)
   - CHƯA vá lỗ giá vốn 2 cột trên máy vận hành (cần commit 9667995 + điểm gọi)
   - CHƯA code gom điều hướng và Trung Tâm Phân Quyền (chờ duyệt wireframe)
   - CHƯA chạy gói di trú DEBT-128
   - Ba màn con M3 chưa che giá vốn · lệch dữ liệu máy vận hành

7. ĐANG CHỜ OWNER
   - Bản phát hành HỘI TỤ: làm hay không → chặn mọi việc triển khai về sau
   - Có đẩy ngay 4 phần đã kiểm ở mục 7 không → chặn việc đóng lỗ quản trị
   - Duyệt wireframe cuối → chặn việc code gom điều hướng + Trung Tâm Phân Quyền
   - Duyệt gói di trú DEBT-128 (cần DDL)

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Trình Owner kế hoạch bản phát hành HỘI TỤ (sao lưu · danh sách di trú · thứ
   tự · cách lùi · bộ kiểm khói), để chấm dứt việc vá từng mảnh.

9. CHƯA XÁC MINH ĐƯỢC
   - Hành vi trên máy vận hành — chưa triển khai. Máy vận hành VẪN CÒN lỗ giá
     vốn 2 cột, VẪN CÒN nút ghi đè quyền hàng loạt, VẪN CHƯA có chốt chặn
     quản trị cuối cùng.
   - Số liệu DEBT-128 trên máy vận hành — mọi con số là của máy phát triển.
   - Chi phí khoá bảng khi di trú với dữ liệu thật.

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — thiếu: quyết định của Owner về bản phát hành hội tụ và về
       việc có đẩy 4 phần đã kiểm hay không.
       Điều kiện lên PASS: Owner trả lời một trong hai câu ở mục 7.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén:
     - .governance/registry/tech-debt.md (định dạng, mã đang dùng)
     - docs/OWNER-REQUEST-LEDGER.md (định dạng mục sổ)
     - src/lib/security-store.ts · action-permission.ts · api-guard.ts
     - src/lib/app-navigation-metadata.ts (đo bằng đối tượng lúc chạy)
     - scripts/tests/d-chup-anh-ma-tran-quyen.ts (mẫu chụp ảnh sẵn có)
   Việc lượt này KHÔNG đụng giao diện mới (chỉ dịch bản vá đã có sang nhánh
   phát hành), nên không phát sinh nghĩa vụ đọc lại chuẩn UI.
═══════════════════════════════════════════
```
