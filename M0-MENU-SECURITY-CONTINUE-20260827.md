# BÁO CÁO — VÁ LỖI THANH BÊN + TẠM NGƯNG ÁP MẪU QUYỀN + RÀ SOÁT M0

**Mã việc:** `M0-MENU-SECURITY-CONTINUE-20260827`
**Ngày:** 27/08/2026
**Căn cứ:** Mười khoá của Owner ngày 27/08/2026 — Sổ Yêu Cầu Owner mục **#183**
**Nhánh:** `main` · **Không triển khai** (đúng khoá Owner)

---

## 1. TÓM TẮT MỘT ĐOẠN

Hai lỗi được Owner cho phép sửa đã sửa xong, mỗi lỗi kèm một cổng kiểm mới **và một lần
kiểm ngược chứng minh cổng có giá trị thi hành**. Lỗi thứ nhất khiến mục *Biểu Mẫu*
**không bao giờ hiện** với người đã được cấp quyền, và khiến bảy màn *Tài chính*
**không phân biệt được với nhau**. Lỗi thứ hai là một nút ghi đè **toàn bộ** quyền của
một vai trò chỉ bằng một cú bấm, không xem trước, không giao dịch — nay bị chặn **ở máy
chủ**, không phải chỉ mờ nút. **Không một dòng quyền nào bị thay đổi.** Phần rà soát tìm
được năm khoản nợ, trong đó **một lỗ khả năng khôi phục có thật** (`DEBT-126`) và **một
mâu thuẫn giữa hai quyết định Owner** mà tôi **không tự phân xử** (`DEBT-127`).

---

## 2. VÁ LỖI 1 — THANH BÊN DÙNG SAI KHOÁ QUYỀN

### 2.1 Bệnh — hai đường sinh khoá khác nhau cho cùng một câu hỏi

Thanh bên suy khoá quyền bằng `ENTITY_MENU_KEYS[key] ?? key.split("-")[0]`.
Máy chủ suy bằng danh mục màn hình. **Hai đường lệch nhau**, đo được hai hậu quả thật:

| # | Mục | Thanh bên hỏi khoá | Khoá THẬT trong `dm_menu` | Hậu quả |
|---|---|---|---|---|
| 1 | Biểu Mẫu / Mẫu In | `"bieu-mau"` | `"form_mau"` | **không bao giờ khớp** — người có quyền vẫn **không thấy mục** |
| 2 | Bảy màn Tài chính | tất cả về `"mf"` | `mf_phieu_thu` · `mf_phieu_chi` · `mf_cong_no` · `mf_ngan_hang` · `mf_doi_chieu` · `mf_nghiem_thu` · `mf_ke_toan` | **không phân biệt được**: ai chỉ được cấp một màn thì **không thấy màn nào** |

Điểm đáng chú ý: dòng `"bieu-mau": "bieu-mau"` đã có sẵn kèm chú thích nói rằng nó
**CHỮA** lỗi mất menu. Thực tế nó chỉ đổi một khoá sai lấy một khoá sai khác.

### 2.2 Thuốc

Khoá nay suy từ **ROUTE**, qua đúng danh mục màn hình mà máy chủ dùng
(`khoaQuyenChoRoute` — cùng hàm `canViewManHinh` gọi). Route là định danh ổn định:
nó nằm trong `dm_menu.duong_dan`, và chính các trang `/mf/*` cũng gác bằng route.

Hai hàm suy khoá đặt ở **một nguồn duy nhất** `src/lib/security/khoa-thanh-ben.ts`,
thanh bên và bài kiểm cùng nạp một bản — **không tạo resolver song song** (Owner I.4).

Thêm: mục cha chỉ **bấm được** khi có quyền **trang riêng** của chính nó; thiếu thì nó
lui về **accordion hiển thị** (Owner III.8–III.9). Nếu dùng khoá theo route của `/mf`
cho việc bấm thì ai có một màn con cũng vào được trang tổng — sai đúng điều Owner khoá.

**KHÔNG** tạo khoá mới · **KHÔNG** đổi route · **KHÔNG** đổi `dm_menu` · **KHÔNG** đổi hợp đồng quyền.

### 2.3 Cổng kiểm — `npm run test:p1-thanh-ben` — **13/13**

Năm ca hồi quy Owner yêu cầu đều có: quản trị · người được cấp Biểu Mẫu ·
người chỉ được cấp một màn Tài chính · người chỉ được cấp Hợp Đồng · tài khoản chờ cấp phát.

### 2.4 Kiểm ngược — **5 khẳng định đỏ**

Tái dựng đúng trạng thái trước khi sửa (gỡ bản vá **và** trả ghi đè về giá trị sai cũ):

```
FAIL 1. Mọi mục thanh bên suy ra khoá CÓ THẬT trong dm_menu
FAIL 2. `bieu-mau` suy ra khoá chuẩn `form_mau`
FAIL 3. Bảy màn Tài chính có bảy khoá RIÊNG, không gộp về `mf`
FAIL 4. Cấp đúng 1 màn Tài chính ⇒ thấy đúng 1 màn
FAIL 5. Cấp `form_mau` ⇒ thấy mục Biểu Mẫu
PASS 6. Chỉ cấp Hợp Đồng ⇒ thấy Hợp Đồng    ← ca này chưa từng hỏng, xanh là đúng
```

---

## 3. HAI LỖI CỦA CHÍNH TÔI — KIỂM NGƯỢC BẮT ĐƯỢC

Ghi lại vì đây là loại lỗi làm cổng **xanh giả**, nguy hiểm hơn cổng đỏ.

**3.1 Bài kiểm đo bản chép, không đo thanh bên.**
Bản đầu **chép lại** phép suy khoá vào tệp kiểm. Kiểm ngược lần một: gỡ bản vá mà
**sáu khẳng định hành vi vẫn xanh** — vì chúng đo bản chép. Chỉ một khẳng định tĩnh
bắt được. Đã sửa bằng cách tách hàm ra nguồn chung để hai bên nạp **cùng một bản**,
và thêm khẳng định `8c` chốt việc thanh bên thật sự nạp nguồn đó.

**3.2 Khẳng định đo chú thích thay vì đo mã.**
Khẳng định *"không còn `split("-")`"* tìm chuỗi trên **toàn văn bản**, nên chính chú
thích mô tả lỗi cũ cũng làm nó đỏ. Đã sửa: bóc hết chú thích trước khi đo.
(Cùng họ với một lỗi đã trả giá trước đây — dòng `import` từng làm một khẳng định xanh
giả sau khi lời gọi thật đã bị xoá.)

---

## 4. VÁ LỖI 2 — TẠM NGƯNG NÚT "ÁP MẪU QUYỀN NHANH"

### 4.1 Bốn điểm nguy hiểm đo được cùng lúc

1. **Không có bước xem trước** — người bấm không thấy mình sắp **mất** quyền nào.
2. **Không có giao dịch** — mỗi menu là một lệnh ghi riêng; hỏng giữa chừng thì vai trò
   nằm ở trạng thái **ghi được một nửa**, không có đường lùi.
3. **Mẫu viết cứng** cho 5 vai trò, trong khi cơ sở dữ liệu có **9 vai trò**.
4. **Vai trò dùng chung nhiều tài khoản** — một cú bấm đổi quyền của **nhiều người**,
   không ai được cho biết số lượng.

### 4.2 Chặn ở máy chủ, không phải chỉ mờ nút

Đây là server action: **gọi thẳng vẫn chạy** nếu chỉ mờ nút. Nên lời từ chối đặt ở
**dòng đầu tiên** của hành động, trước cả bước kiểm quyền. Thân cũ **giữ nguyên văn**
(bảo toàn theo `GOV-EDIT-PRESERVE-001`) và hiện **không nơi nào gọi**.

Giao diện là **lớp thứ hai**: nút mờ, kèm ô nêu lý do thật và **đường thay thế dùng
được ngay** — tick từng menu rồi bấm Lưu. Lý do lấy từ **cùng một nguồn** với máy chủ
nên hai bên không thể nói khác nhau.

### 4.3 Cổng kiểm — `npm run test:p1-ap-mau-quyen` — **8/8**

Khẳng định cốt lõi: **gọi THẲNG hành động → 0 dòng quyền thay đổi** (chụp ảnh toàn bộ
bảng quyền trước và sau, so chuỗi).

### 4.4 Kiểm ngược — và một khuyết điểm của chính cổng

Lần một: nối lại thân cũ thì bài kiểm **treo**, không đỏ không xanh. **Một cổng đứng im
thì không thi hành được gì.** Nguyên nhân: bản gỡ vá ném lỗi ở `cookies` ngoài ngữ cảnh
yêu cầu, và tiến trình không thoát vì bể kết nối chưa đóng.

Đã sửa hai chỗ: thêm hạn giờ để **treo bị kết luận là hỏng**, và đóng bể kết nối kể cả
khi hỏng. Kiểm ngược lần hai: **2 khẳng định đỏ, tiến trình thoát sạch.**

---

## 5. RÀ SOÁT — TÁM ĐƯỜNG PHÂN GIẢI QUYỀN

Chi tiết bảng đầy đủ ở `WIREFRAME-DA-SUA-M0-20260827.md` mục C.
Ba điểm cần Owner biết:

- **Không có kho quyền song song theo từng người.** Đã quét 6 tên gọi khả dĩ trong
  `src/` và `migrations/` — **cả 6 không tồn tại**. Quyền chỉ đi qua vai trò.
- **Tầng menu và tầng hành động dùng cấp-thắng**, khớp khoá Owner I.6.
- ⚠️ **Tầng trường nhạy cảm dùng cấm-thắng**, theo quyết định Owner **17/07/2026** ghi
  ngay trong mã. **Trái** với khoá Owner 27/08 mục I.6. Owner đã cấm tự phát minh thứ tự
  cho/cấm nên **tôi không chọn bên** — ghi `DEBT-127`, chờ Owner phân xử.
  Hiện **chưa gây hại**: bảng quyền-trường chỉ có 4 dòng, đều CEO, đều cho xem.

---

## 6. RÀ SOÁT — KHẢ NĂNG KHÔI PHỤC (LỖ THẬT)

| Bậc | Cơ chế | Có thật? |
|---|---|---|
| 1 | Tài khoản mang vai trò quản trị | ✅ 3 tài khoản, đều đang dùng được |
| 2 | `BOOTSTRAP_ADMIN_EMAILS` | ✅ có — nhưng **chỉ kích hoạt khi hạ tầng RBAC hỏng** |
| 3 | Vai trò `DEV` cứu hộ | ❌ **KHÔNG TỒN TẠI** |

🔴 **Lỗ (`DEBT-126`):** gỡ vai trò **có** chốt chặn quản trị cuối, nhưng **khoá tài khoản
KHÔNG có chốt nào** — khoá xong thu hồi mọi phiên ngay. Cộng thêm: phép đếm quản trị
**không lọc trạng thái tài khoản**, nên một quản trị **đã bị khoá vẫn được tính là còn**.
Hai lỗ cộng lại có thể dẫn tới **0 người khôi phục được**, mà bậc 2 **không cứu** vì bảng
vẫn lành, chỉ dữ liệu sai. **Hiện CHƯA xảy ra** — 3/3 quản trị đang hoạt động.

🟠 **Lỗ nhận dạng (`DEBT-128`):** vai trò nối với tài khoản bằng **email**, không phải mã
tài khoản ổn định. Đổi email là **cắt đứt toàn bộ vai trò** của tài khoản đó.

---

## 7. NĂM KHOẢN NỢ MỚI

| Mã | Mức | Nội dung |
|---|---|---|
| `DEBT-124` | 🟡 | 5 khoá xuất hiện hai lần trong cây điều hướng (liên kết chéo M6). Bản vá **không tạo thêm**; cổng đã ghim đúng 5 khoá làm mốc. Không phải rò rỉ quyền |
| `DEBT-125` | 🟠 | Cổng `test:ma-tran-quyen-hd` hỏng 2/58 **sẵn trên HEAD** do `api-guard.ts`. **Đã chứng minh không do phiên này** bằng cách dựng cây phụ ở HEAD sạch — kết quả y hệt |
| `DEBT-126` | 🔴 | Khoá tài khoản không kiểm quản trị cuối cùng — có thể còn 0 người khôi phục |
| `DEBT-127` | 🟠 | Hai quyết định Owner trái nhau về hợp nhất đa vai trò ở tầng trường — **chờ Owner phân xử** |
| `DEBT-128` | 🟠 | Vai trò nối bằng email thay vì mã tài khoản ổn định |

---

## 8. CỔNG KIỂM ĐÃ CHẠY

| Cổng | Kết quả |
|---|---|
| `test:p1-thanh-ben` (mới) | **13/13** · kiểm ngược 5 đỏ |
| `test:p1-ap-mau-quyen` (mới) | **8/8** · kiểm ngược 2 đỏ |
| `test:p1-menu` | 11/11 |
| `test:p1-ban-giao` | 14/14 |
| `test:p1-gia-von` | 45/45 |
| `test:p1-giu-gia-von` | 11/11 |
| `test:p1-api-phien` | 13/13 |
| `test:p1-api-matrix` | 6/6 |
| `test:quyen-vai-tro` | 17/17 |
| `test:m1-rbac` | 111/111 |
| `test:m1-menu` | 40/40 |
| `test:khoa-con-man-hinh` | 15/15 |
| `test:danh-muc-man-hinh` | 13/13 |
| `test:tach-form-m0` | **25/25** — xem mục 9 |
| `test:ma-tran-quyen-hd` | 56/58 — **hỏng sẵn trên HEAD**, ghi `DEBT-125` |
| kiểm kiểu toàn dự án | sạch |

---

## 9. MỘT CỔNG CŨ ĐÃ GHIM CHÍNH LỖI LẠI

Khẳng định `[E7c]` của `test:tach-form-m0` đòi đúng cặp `"bieu-mau": "bieu-mau"` —
mà `"bieu-mau"` **không phải khoá thật**. Nghĩa là khẳng định ấy **bắt buộc phải sai thì
mới xanh**: nó chống được lỗi cắt gạch nối, nhưng lại **chốt chặt một khoá không tồn tại**
vào đúng chỗ đó, nên mục Biểu Mẫu vẫn biến mất.

Đã viết lại để **đo hành vi** qua đúng nguồn máy chủ dùng, **giữ nguyên văn dòng cũ**
trong chú thích theo `GOV-EDIT-PRESERVE-001`. Cổng trở lại **25/25**.

---

## 10. XÁC NHẬN CÁC ĐIỀU OWNER CẤM

| Điều Owner cấm | Trạng thái |
|---|---|
| Hỏi Owner về tên bảng/cột/menu/route | ✅ không hỏi — tra bằng đo đạc |
| Tạo lược đồ mới / DDL | ✅ không đụng |
| Triển khai | ✅ **không triển khai** |
| Tạo kho quyền/menu/khoá/route/resolver song song | ✅ không — trái lại, **gộp** hai bản chép về một nguồn |
| Tự phát minh thứ tự cho/cấm | ✅ không — ghi nợ, chờ Owner |
| Đổi quyền đang có | ✅ **0 dòng** — chứng minh bằng ảnh chụp bảng trước/sau |
| Mật khẩu mặc định · cửa hậu · bí mật dạng chữ trong giao diện/nhật ký/kho/báo cáo | ✅ không có |
| Sửa CSDL thủ công không giao dịch/nhật ký | ✅ không sửa CSDL |

**Kiểm chứng độc lập:** `role_menu_permission` — lần sửa gần nhất **23/08/2026**,
**0 dòng** thay đổi trong 60 phút quanh phiên làm việc.

---

## 11. CÒN LẠI, CHƯA LÀM

- Ba màn con M3 chưa che giá vốn · không gửi trường giá vốn ra máy khách không có quyền
- Nâng bản vá lưu báo giá lên mã tuỳ chọn ổn định + giao dịch/khoá dòng/phiên bản
- An toàn nhập liệu M1 · ba khoá đơn hàng
- Đồng bộ lệch máy vận hành (3 vai trò · 28 menu · ~100 dòng quyền)
- Khoảng cách 53 commit chưa phát hành
- `DEBT-116` · `DEBT-121` · `DEBT-122` · `DEBT-123` và năm nợ mới ở mục 7

---

## 12. ĐANG CHỜ OWNER

1. **`DEBT-127` — phân xử hợp nhất đa vai trò ở tầng trường.** Quyết định 17/07 nói
   cấm-thắng, khoá 27/08 nói cấp-thắng. **Chặn**: chưa thể chốt dịch vụ quyền hợp nhất.
2. **Duyệt wireframe đã sửa** (`WIREFRAME-DA-SUA-M0-20260827.md`). **Chặn**: chưa viết
   giao diện chính thức cho trung tâm phân quyền.
3. **`DEBT-126`** — có sửa ngay chốt chặn quản trị cuối ở đường khoá tài khoản không.
4. **`DEBT-128`** — có duyệt thêm cột mã tài khoản ổn định không (**cần DDL**).

---

## 13. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC

Thêm chốt chặn **quản trị cuối cùng** vào đường khoá tài khoản và sửa phép đếm quản trị
cho **lọc trạng thái tài khoản** (`DEBT-126`) — không cần DDL, không cần Owner duyệt thêm
vì chính Owner đã khoá *«không được để hệ thống còn 0 người khôi phục được»*.

---

## 14. CHƯA XÁC MINH ĐƯỢC

- **Hiển thị thật trên trình duyệt.** Bằng chứng hiện ở mức mã và cơ sở dữ liệu; chưa có
  ảnh chụp màn hình đối chiếu. Ai xác minh được: Owner mở máy, hoặc một phiên có công cụ
  kiểm trên trình duyệt.
- **Hành vi trên máy vận hành.** Chưa triển khai, đúng khoá Owner. Máy vận hành vẫn đang
  chạy bản cũ, **vẫn còn cả hai lỗi này**.
- **`DEBT-125` có phải lỗi thật của cổng hay của `api-guard` không.** Đã chứng minh là
  hỏng sẵn trên HEAD; chưa phân tích tới cùng nên sửa bên nào.


---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Vá lỗi 1: thanh bên suy khoá quyền từ ROUTE qua đúng danh mục máy chủ dùng,
     thay phép đoán cắt gạch nối. Chữa 2 lỗi đo được: mục Biểu Mẫu không bao giờ
     hiện với người đã được cấp quyền; bảy màn Tài chính gộp về một khoá.
   - Gộp hai hàm suy khoá về MỘT nguồn `src/lib/security/khoa-thanh-ben.ts` để
     thanh bên và bài kiểm nạp cùng một bản (không tạo resolver song song).
   - Mục cha chỉ bấm được khi có quyền TRANG RIÊNG; thiếu thì lui về accordion.
   - Vá lỗi 2: tạm ngưng nút Áp Mẫu Quyền Nhanh — chặn ở MÁY CHỦ dòng đầu tiên,
     giao diện chỉ là lớp thứ hai. Thân cũ giữ nguyên văn, không nơi nào gọi.
   - Viết 2 cổng kiểm mới, mỗi cổng kiểm ngược ít nhất một lần.
   - Viết lại khẳng định [E7c] của cổng cũ vì nó ghim chính lỗi lại.
   - Rà 8 đường phân giải quyền · kho quyền song song · chốt chặn quản trị cuối.
   - Ghi 5 khoản nợ DEBT-124..128 · ghi Sổ Yêu Cầu Owner mục #183.
   - Viết wireframe đã sửa + báo cáo này.

2. PHẠM VI
   ĐỤNG    : src/components/layout/sidebar.tsx · src/lib/security/khoa-thanh-ben.ts (mới)
             src/app/m0/security/actions.ts · security-client.tsx · tam-ngung.ts (mới)
             scripts/tests/p1-thanh-ben-khoa-chuan.test.ts (mới)
             scripts/tests/p1-ap-mau-quyen.test.ts (mới)
             scripts/tests/e-tach-form-khoi-m0.test.ts · package.json
             docs/OWNER-REQUEST-LEDGER.md · .governance/registry/tech-debt.md
             docs/reports/ (2 tệp mới)
   KHÔNG ĐỤNG: lược đồ CSDL (0 DDL) · dữ liệu quyền (0 dòng đổi) · dm_menu · route
             · máy vận hành (KHÔNG triển khai) · công thức giá · 5 tệp luật quản trị

3. BẰNG CHỨNG
   npm run test:p1-thanh-ben       → 13 đạt / 0 hỏng          → CODE + DB_PROVEN
   kiểm ngược (tái dựng lỗi gốc)   → 5 khẳng định ĐỎ          → CODE_PROVEN
   npm run test:p1-ap-mau-quyen    → 8 đạt / 0 hỏng           → CODE + DB_PROVEN
   kiểm ngược (nối lại thân cũ)    → 2 khẳng định ĐỎ          → CODE_PROVEN
   npm run test:tach-form-m0       → 25 PASS · 0 FAIL         → CODE_PROVEN
   13 cổng khác (p1-menu · m1-rbac · quyen-vai-tro …) → toàn đạt → CODE + DB_PROVEN
   npx tsc --noEmit                → sạch                     → CODE_PROVEN
   npm run test:secret-scan        → PASS (0 vi phạm)         → FILE_PROVEN
   npm run test:pii-scan           → PASS (0 vi phạm)         → FILE_PROVEN
   npm run check:governance        → 5 tệp luật đồng bộ       → FILE_PROVEN
   truy vấn role_menu_permission   → sửa gần nhất 23/08, 0 dòng đổi trong 60 phút → DB_PROVEN
   cây phụ git ở HEAD f1bc271 sạch → ma-tran-quyen-hd vẫn 56/58 → CODE_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #183

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho irissnss/erptanphat · commit f50a99c · nhánh main
       f1bc271..f50a99c · 13 tệp · +1089 / -23
       file: docs/reports/M0-MENU-SECURITY-CONTINUE-20260827.md
             docs/reports/WIREFRAME-DA-SUA-M0-20260827.md

6. CÒN SÓT / CHƯA LÀM
   - Ba màn con M3 chưa che giá vốn; chưa chặn gửi trường giá vốn ra máy khách
   - Chưa nâng bản vá lưu báo giá lên mã tuỳ chọn ổn định + giao dịch/khoá dòng
   - Chưa làm an toàn nhập liệu M1 · ba khoá đơn hàng
   - Chưa đồng bộ lệch máy vận hành (3 vai trò · 28 menu · ~100 dòng quyền)
   - Khoảng cách 53 commit chưa phát hành
   - DEBT-116 · 121 · 122 · 123 và năm nợ mới DEBT-124..128
   - Chưa có ảnh chụp màn hình đối chiếu cho hai thay đổi giao diện

7. ĐANG CHỜ OWNER
   - DEBT-127 phân xử hợp nhất đa vai trò ở tầng trường (17/07 nói cấm-thắng,
     27/08 nói cấp-thắng) → chặn việc chốt dịch vụ quyền hợp nhất
   - Duyệt wireframe đã sửa → chặn việc viết giao diện chính thức trung tâm phân quyền
   - DEBT-126 có sửa ngay chốt chặn quản trị cuối ở đường khoá tài khoản không
   - DEBT-128 có duyệt thêm cột mã tài khoản ổn định không (cần DDL)

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Thêm chốt chặn quản trị cuối cùng vào đường khoá tài khoản và sửa phép đếm
   quản trị cho lọc trạng thái tài khoản (DEBT-126).

9. CHƯA XÁC MINH ĐƯỢC
   - Hiển thị thật trên trình duyệt — chưa có ảnh chụp. Ai xác minh: Owner, hoặc
     một phiên có công cụ kiểm trên trình duyệt.
   - Hành vi trên máy vận hành — chưa triển khai đúng khoá Owner; máy vận hành
     VẪN CÒN cả hai lỗi này.
   - DEBT-125 nên sửa bên cổng kiểm hay bên api-guard — chưa phân tích tới cùng.

10. TRẠNG THÁI CHUNG
   [x] PASS — hai việc được phép code đã xong, có bằng chứng đúng lớp, đã kiểm
       ngược cả hai, không còn việc chặn trong phạm vi được giao.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén:
     - docs/UI-STANDARD.md (chuẩn giao diện) — §0 §1 §2 §3 §12 §13 §18, vì việc
       này có đụng giao diện (ô cảnh báo + nút vô hiệu ở trung tâm phân quyền).
       Đối chiếu: rounded-md · h-9 px-3 · text-slate-400 (màu vô hiệu §2) ·
       amber cho trạng thái chờ (§2) — khớp. MỘT chỗ lệch §13 đã sửa: tiêu đề
       cảnh báo đưa về Title Case.
     - .governance/registry/tech-debt.md (định dạng dòng nợ, số nợ đang dùng)
     - docs/OWNER-REQUEST-LEDGER.md (định dạng mục sổ, mục cuối #182)
     - src/components/layout/sidebar.tsx · src/lib/security/menu-catalog.ts
     - src/lib/security-store.ts · src/lib/action-permission.ts
     - src/app/m0/security/actions.ts · security-client.tsx
   Không kết thúc bằng trí nhớ từ trước nén.
═══════════════════════════════════════════
```
