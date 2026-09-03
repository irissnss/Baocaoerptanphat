# BÁO CÁO — M3 LÁT CẮT 1: ĐỊNH DANH PHƯƠNG ÁN ỔN ĐỊNH + CHỐNG GHI ĐÈ

**Mã việc:** `M3-BAO-GIA-DON-HANG-INTEGRITY-AND-ACCESS` — lát cắt 1
**Ngày:** 28/08/2026
**Nguồn giao việc:** Sổ Yêu Cầu Owner `#187`, mục 12 — *"M3 lát cắt kế tiếp: định danh
phương án báo giá theo `id` ổn định, kèm giao dịch và khoá dòng/phiên bản chống ghi đè.
Dùng `withTransaction` đã có."*

---

## 0. ĐỌC PHẦN NÀY LÀ ĐỦ

Rà trước khi code đã tìm ra **nguyên nhân gốc** chứ không chỉ triệu chứng:
đường lưu báo giá **XOÁ TRẮNG** mọi dòng hàng + phương án rồi **CHÈN LẠI**, nên
`bao_gia_option.id` **đổi mới sau MỖI lần lưu**. Không có định danh nào bền thì
mọi thứ khác buộc phải đoán — và mã cũ đoán theo cặp *(thứ tự dòng hàng, tên phương án)*.

Cách đoán đó **hỏng thật**, hỏng lặng lẽ:

| Người dùng làm gì | Hậu quả trước bản này |
|---|---|
| Đổi tên một phương án | Giá vốn **âm thầm về 0** |
| **Đảo thứ tự dòng hàng** | Giá vốn của dòng này **dán sang phương án của dòng khác** — không báo lỗi, số vẫn có, **sai chủ** |
| Đặt hai phương án trùng tên trong một dòng | **Chặn cứng**, không lưu được gì |
| Hai người cùng sửa một báo giá | Người lưu sau **nuốt trọn** việc của người lưu trước |

Nay: ghép theo `id` thật. Đổi tên, đảo thứ tự, trùng tên — đều không ảnh hưởng.
Hai người cùng lưu thì có **khoá dòng**; ai mở form từ bản cũ thì bị **từ chối ghi đè**
kèm lời giải thích, thay vì lặng lẽ xoá việc của người kia.

**Bằng chứng:** 21/21 nhân thuần · 17/17 trên CSDL thật · **kiểm ngược 3/3** ·
tsc 0 lỗi · bản dựng sản phẩm đạt.

---

## 1. RÀ TRƯỚC KHI CODE

Owner khoá: *"audit before code"*. Đã đo, không đoán:

| Điều cần biết | Đo được |
|---|---|
| `bao_gia_option` có cột định danh không | **Có** — `id int(11) PRI AUTO_INCREMENT` |
| Có ai ngoài M3 tham chiếu `bao_gia_option.id` không | **Không** — `grep` toàn kho cho `id_bao_gia_option` ra **0** kết quả ⇒ phạm vi ảnh hưởng khép kín |
| Khoá ngoại | `bao_gia_item.id_bao_gia → bao_gia` (CASCADE) · `bao_gia_option.id_bao_gia_item → bao_gia_item` (CASCADE) · `don_hang.id_bao_gia → bao_gia` (**RESTRICT**) |
| Gói dữ liệu trình duyệt gửi lên có `id` không | **KHÔNG** — trình duyệt đang giữ sẵn `item.id`/`opt.id` nhưng **bỏ đi** khi dựng gói |
| `withTransaction` có ghim kết nối không | **Có** — dùng `AsyncLocalStorage`, nên `FOR UPDATE` khoá đúng dòng, không rơi sang kết nối khác |
| Đã có cơ chế phiên bản chưa | **Chưa** — nhưng `bao_gia.ngay_sua` dùng được làm mốc phiên bản, **không cần thêm cột** |

> ⚠️ **Không thêm bảng, không thêm cột, không thêm tuyến, không thêm khoá menu.**
> Toàn bộ lát cắt này chạy trên lược đồ hiện hành — đúng khoá §II của Owner.

**Một chi tiết bẩn tìm thấy khi rà:** `actions.ts` chứa **ba byte NUL (0x00)** nhét thẳng
vào mã nguồn làm ký tự ngăn khoá. Hệ quả: `grep` xếp tệp vào loại **nhị phân** và không
in ra dòng khớp nào — công cụ soi mã bỏ qua tệp này. Đã gỡ cả ba, thay bằng ký hiệu
`␟` nhìn thấy được.

---

## 2. ĐÃ SỬA GÌ

### 2.1 Định danh ổn định — sửa tại gốc

`saveBaoGiaBundle` đổi từ **xoá trắng → chèn lại** sang **đối chiếu**:

```
dòng đã có   → SỬA tại chỗ, giữ nguyên id
dòng mới     → CHÈN
dòng bị bỏ   → XOÁ
```

Mọi câu `UPDATE`/`DELETE` đều kèm `AND id_bao_gia = ?` nên **không thể chạm sang báo giá khác**.

### 2.2 Chốt chặn chiếm dụng dòng

Cho `id` vào gói dữ liệu mở ra một đường tấn công mới: gửi `id` của **báo giá khác**
lên để ghi đè dòng của họ. Hàm `idHopLe` chặn — chỉ nhận `id` **nằm trong chính báo giá
đang sửa**; `id` lạ bị coi là **dòng mới**, nên không dòng nào của người khác bị đụng tới.

### 2.3 Ghép giá vốn theo `id`, giữ đường lui theo tên

Tách sang mô-đun riêng `src/lib/m3/ghep-phuong-an.ts` để **bài kiểm nạp đúng mã đang
chạy**, không đo một bản chép. (Bài học đã trả giá ở lát cắt thanh bên: bài kiểm vẫn xanh
sau khi bản vá bị gỡ, vì nó đo bản sao.)

**Đường lui theo tên GIỮ LẠI có chủ đích.** Trình duyệt của người dùng có thể còn giữ bản
mã cũ (chưa gửi `id`). Bỏ đường lui đi thì **đúng lúc phát hành**, mọi bản mã cũ sẽ ghi
giá vốn về 0 — mất dữ liệu thật, trên diện rộng.

### 2.4 Khoá dòng + mốc phiên bản

- `SELECT ... FROM bao_gia WHERE id = ? FOR UPDATE` — hai người cùng bấm Lưu thì người
  thứ hai **đợi**, không chạy song song.
- Mốc `ngay_sua` đọc lúc mở form được gửi kèm; lệch ⇒ **`GHI_DE_XUNG_DOT`**, từ chối ghi
  đè và nói rõ vì sao. Không gửi mốc (bản mã cũ) ⇒ vẫn lưu được, vẫn có khoá dòng.

### 2.5 Trình duyệt gửi kèm định danh

Hai chỗ dựng gói trong `bao-gia-client.tsx` nay gửi `item.id`, `opt.id` và
`ngay_sua_luc_mo`.

---

## 3. BẰNG CHỨNG

### 3.1 Nhân thuần — `npm run test:m3-ghep-phuong-an` — **21/21**

Nạp **đúng mô-đun** đường chạy thật dùng. Ca phân biệt được đáng chú ý:

| Ca | Đo được |
|---|---|
| Đổi tên phương án | giá vốn **1000 giữ nguyên** (trước: về 0) |
| **Đảo thứ tự dòng hàng** | `[[3000,4000],[1000,2000]]` — theo đúng chủ (trước: dán nhầm) |
| `id` chỉ về giá 2000 nhưng mang tên của phương án giá 1000 | ra **2000** ⇒ `id` **thắng** tên |
| `id` của báo giá khác | ra **0**, không đọc trộm giá vốn |
| Trùng tên **có** `id` | lưu bình thường (trước: chặn cứng) |
| Trùng tên **không** `id` | vẫn chặn — không đoán bừa |

### 3.2 CSDL thật — `npm run test:m3-dinh-danh` — **17/17**

Đi qua **đúng `saveBaoGiaBundle`**, dữ liệu tự tạo tự dọn, chốt bằng mốc nền.

| Nhóm | Đo được |
|---|---|
| A. định danh ổn định | lưu 2 lần liên tiếp, `id` vẫn `169,170,171,172` · số dòng không phình |
| B. đổi tên | `id` giữ nguyên, giá vốn `1111.00` nguyên vẹn |
| C. **đảo thứ tự** | `169:1111 170:2222 171:3333 172:4444` — không dán nhầm dòng nào |
| D. bỏ một phương án | dòng `170` bị xoá, ba dòng còn lại **giữ nguyên id** |
| E. chiếm dụng | dòng của báo giá khác **không đổi một byte**; `id` lạ thành dòng mới |
| F. chống ghi đè | mốc lệch ⇒ `GHI_DE_XUNG_DOT`; mốc đúng ⇒ lưu được; lần bị từ chối **không để lại thay đổi nào** |
| G. tương thích | gói **không kèm `id`** vẫn lưu được, không mất dòng nào |
| Z. dọn sạch | `bao_gia 8→8 · item 8→8 · option 13→13` |

### 3.3 KIỂM NGƯỢC — **3/3 đạt**

Cổng chỉ có giá trị khi **gỡ bản vá ra thì nó ĐỎ**:

| Gỡ gì | Kết quả |
|---|---|
| Gỡ ghép theo `id` ở đường lưu | `A1 · A2 · A3` **ĐỎ** — `id` nhảy `205-208 → 213-216` |
| Gỡ kiểm mốc phiên bản | `F1 · F3` **ĐỎ** |
| Gỡ nhánh ghép `id` ở nhân thuần | **8 khẳng định ĐỎ** |

> Ca 1 còn cho thêm một bằng chứng ngoài dự kiến: gỡ bản vá ra thì bài kiểm **dừng hẳn**
> ở bước [B], vì `id` dòng hàng trong dữ liệu mẫu đã bị thay sạch ngay ở lần lưu đầu.
> Đó chính là cái hỏng mà bản vá này chữa.

Tệp được **khôi phục nguyên trạng** trong `finally`; đã xác minh lại bằng `tsc` sạch và
chạy lại bộ kiểm ra 17/17.

### 3.4 Không làm hỏng phần khác

| Cổng | Kết quả |
|---|---|
| `test:m3-duyet-gia` | **29/29** |
| `test:don-hang-hoan-thien` | **24 PASS / 0 FAIL** |
| `test:m3-huy-don` | đạt |
| `test:h3-bao-gia-don-gate` | **11/11** — sau khi sửa (xem §4) |
| `test:m1-owner` | **75 PASS / 0 FAIL** — sau khi viết lại khẳng định 5 (xem §5) |
| `test:quyen-chuyen-trang-thai` | **29 PASS / 0 FAIL** |
| **`test:m3-gates`** (bộ gộp mới, 7 cổng) | chạy trọn, **mã thoát 0** |
| 55 cổng mồ côi đo thật | **53 xanh · 2 đỏ · 0 treo** — chi tiết §5 |
| `tsc --noEmit` | **0 lỗi** |
| `npm run build` | **đạt** |

---

## 4. 🔴 MỘT CỔNG ĐÃ ĐỎ SUỐT 5 NGÀY MÀ KHÔNG AI BIẾT

`test:h3-bao-gia-don-gate` **đỏ** khi chạy. Truy nguồn:

| Việc | Commit | Ngày |
|---|---|---|
| Bắt buộc `ngayGiaoDuKien` vào `createDonHangFromBaoGia` | `<mã-nguồn-riêng>` | **23/08** |
| Lần sửa cuối của bài kiểm h3 | `<mã-nguồn-riêng>` | **21/08** |

Bài kiểm viết **trước** khi hợp đồng đổi, và **không lần chạy nào chạm tới nó** trong 5
ngày. Lý do: `test:h3-bao-gia-don-gate` **không nằm trong bộ cổng gộp nào**.

**Đã sửa BÀI KIỂM cho khớp hợp đồng hiện hành** — truyền ngày giao. **KHÔNG nới nghiệp vụ
để bài kiểm xanh** (đúng khoá §II của Owner). Kết quả **11/11**.

**Vấn đề gốc lớn hơn, đo được:** trong **85** lệnh kiểm của dự án, chỉ **18** nằm trong bộ
gộp `test:gov-gates`. **67 cổng còn lại mồ côi** — hỏng bao lâu cũng không ai hay.
Ghi nợ `DEBT-131`. Số liệu đo cụ thể ở §5.

---

## 5. ĐO THẬT 67 CỔNG MỒ CÔI

Đã chạy thật **55/67** cổng mồ côi (12 cổng còn lại cần trình duyệt hoặc tham số
đường dẫn nên chưa đo lượt này).

| | Số cổng |
|---|---|
| **XANH** | **53** |
| **ĐỎ** | **2** |
| TREO | 0 |
| chưa đo được lượt này | 12 |

**Hai cổng đỏ — cả hai đã xử:**

| Cổng | Bệnh | Xử lý |
|---|---|---|
| `test:h3-bao-gia-don-gate` | đỏ từ 23/08 (xem §4) | sửa **bài kiểm** cho khớp hợp đồng ⇒ **11/11** |
| `test:m1-owner` khẳng định 5 | đếm dòng `allowed = 0`, mà bản di trú huỷ đơn của Owner **cố ý** để lại dòng 0 để giữ dấu vết | viết lại thành **đo hành vi** hợp-các-quyền ⇒ **75/0** |

> ⚠️ `test:m1-owner` là cổng **THỨ BA** cùng một loại (sau `m1-flexible-rbac` và `N5.7`).
> Ba cổng nay dùng **cùng một câu đo**, dòng cũ giữ nguyên văn trong chú thích theo
> `GOV-EDIT-PRESERVE-001`. Điều thật sự cần bảo đảm không phải "không có dòng 0" mà là
> **dòng 0 không cướp quyền của dòng 1** — mạnh hơn phép đếm cũ.

**Còn một cổng đỏ KHÔNG đóng được lượt này — `test:audit-cols`.** Cổng chờ
`ngay_sua = 67 · nguoi_sua = 64` bảng, máy phát triển đo ra **69 · 66**. Mốc đó vào kho ở
commit `<mã-nguồn-riêng>` ngày **09/08** — 19 ngày và nhiều bản di trú trước.

**KHÔNG sửa con số cho khớp.** Làm vậy chính là *sửa hệ thống để số đo khớp báo cáo* —
điều Owner cấm thẳng ở §I; và `GOV-CONVENTION-BASELINE-002` bắt **soi chéo cả hai bên
trước**, không mặc định tin bên nào. Phiên này **không có khoá SSH** tới máy vận hành
(`Permission denied (publickey,password)`) nên chưa đọc được số bên đó.
Ghi nợ `DEBT-132`, kèm sẵn dữ kiện để lần sau đóng nhanh: hai bên đều **101 bảng**;
chênh lệch `69−66 = 3` khớp `67−64 = 3` ⇒ hình dạng nhất quán, chỉ lệch đúng 2 bảng.

### 5.1 Đã làm một phần: bộ gộp cho M3

Thêm `npm run test:m3-gates` gom **7 cổng** M3 — nhóm này không còn mồ côi.
Chạy trọn, **mã thoát 0**: `21/21 · 17/17 · huỷ đơn đạt · 29/29 · 11/11 · 24/0 · 29/0`.

Các nhóm khác (M1 · M5 · P1 · R11/R12 · hình thức) **vẫn mồ côi** — ghi `DEBT-131`,
trạng thái **ĐANG XỬ LÝ**.

---

## 6. CÒN LẠI CỦA M3

| Việc | Trạng thái |
|---|---|
| Định danh phương án theo `id` ổn định | ✅ **XONG** |
| Giao dịch + khoá dòng/phiên bản chống ghi đè | ✅ **XONG** |
| `delivered` ≠ `closed`, chỉ `closed` khi đã thu đủ tiền | ⬜ lát cắt kế tiếp |
| Huỷ đơn chỉ ADMIN + CEO | ✅ đã khoá ở lượt trước, kiểm lại trên vận hành: **không vai trò nghiệp vụ nào** |

---

## 7. TỰ KHAI — ĐIỀU CHƯA CHỨNG MINH ĐƯỢC

1. **Chưa chạy trên máy vận hành.** Toàn bộ bằng chứng ở đây là **CODE_PROVEN + DB_PROVEN
   trên máy phát triển**. Chưa `DEPLOYMENT_RECORDED`, chưa `LIVE_VERIFIED`.
2. **Chưa có bằng chứng trên trình duyệt thật.** Đường lưu đã được đo qua đúng hàm nghiệp
   vụ, nhưng chưa mở form trên trình duyệt để xem người dùng thật thấy gì khi gặp
   `GHI_DE_XUNG_DOT`.
3. **Chưa đo tranh chấp thật hai phiên song song.** Khoá dòng được chứng minh bằng mã và
   bằng cơ chế `withTransaction` ghim kết nối, chưa bằng hai tiến trình đua nhau.
4. **`saveBaoGiaBundle` ghi `ghi_chu` của báo giá về NULL** khi gói không kèm trường đó
   (`data.ghi_chu ?? null`). Đây là hành vi **sẵn có**, không do lát cắt này; đường web
   luôn gửi `ghi_chu` nên người dùng thật không gặp. Nêu ra vì chính nó làm bước dọn của
   bài kiểm hụt ở lần chạy đầu — và vì nó cùng một họ với lỗi giá vốn đã vá.

---

## 8. KHỐI BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Rà trước khi code: tìm ra gốc là XOÁ TRẮNG–CHÈN LẠI, không phải lỗi ghép
   - Đổi saveBaoGiaBundle sang ĐỐI CHIẾU theo id (sửa/chèn/xoá), giữ id ổn định
   - Thêm idHopLe chặn chiếm dụng dòng của báo giá khác
   - Tách nhân ghép sang src/lib/m3/ghep-phuong-an.ts để bài kiểm đo ĐÚNG mã thật
   - Ghép giá vốn theo id; giữ đường lui theo tên cho bản mã cũ trên trình duyệt
   - Gỡ 3 byte NUL (0x00) khỏi actions.ts — chúng làm grep coi tệp là nhị phân
   - Khoá dòng FOR UPDATE + mốc phiên bản ngay_sua chống ghi đè (KHÔNG thêm cột)
   - Trình duyệt gửi kèm item.id · opt.id · ngay_sua_luc_mo
   - Sửa test:h3-bao-gia-don-gate đã đỏ từ 23/08 (sửa BÀI KIỂM, không nới nghiệp vụ)
   - Đo 67 cổng mồ côi khỏi mọi bộ gộp

2. PHẠM VI
   ĐỤNG    : src/lib/m3-store.ts · src/lib/m3/ghep-phuong-an.ts (mới) ·
             src/app/m3/bao-gia/actions.ts · src/app/m3/bao-gia/bao-gia-client.tsx ·
             scripts/tests/ (2 tệp mới + sửa h3) · package.json
   KHÔNG ĐỤNG: lược đồ CSDL (0 bảng, 0 cột) · tuyến · khoá menu · kho quyền ·
             động cơ giá · máy vận hành · phiên bản ứng dụng

3. BẰNG CHỨNG
   npm run test:m3-ghep-phuong-an  → 21/21 PASS      → CODE_PROVEN
   npm run test:m3-dinh-danh       → 17/17 PASS      → DB_PROVEN
   kiểm ngược 3 ca                 → 3/3 ĐẠT         → CODE_PROVEN
   npm run test:m3-duyet-gia       → 29/29 PASS      → DB_PROVEN
   npm run test:don-hang-hoan-thien→ 24/0            → DB_PROVEN
   npm run test:h3-bao-gia-don-gate→ 11/11 PASS      → DB_PROVEN
   npx tsc --noEmit                → 0 lỗi           → CODE_PROVEN
   npm run build                   → đạt             → CODE_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #187 (kết quả mục 12 ghi bổ sung vào cùng mục; đây là việc
       Owner đã giao sẵn trong chỉ thị đó, KHÔNG phải chỉ thị mới)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit <mã-nguồn-riêng> · file
       M3-DINH-DANH-PHUONG-AN-20260828.md
       (mã trong kho mã nguồn riêng: <mã-nguồn-riêng> — cổng kiểm chỉ xác minh mã của kho
        báo cáo nên ghi riêng ra đây, không đưa vào dòng trên)

6. CÒN SÓT / CHƯA LÀM
   - M3 lát cắt sau: delivered ≠ closed, chỉ closed khi đã thu đủ tiền
   - DEBT-131: 67 cổng mồ côi khỏi bộ gộp — mới ghi sổ, chưa dựng bộ gộp đầy đủ
   - Chưa triển khai lên máy vận hành (cố ý — hết lát cắt M3 mới phát hành một lượt)
   - Chưa có bằng chứng trình duyệt cho thông báo GHI_DE_XUNG_DOT

7. ĐANG CHỜ OWNER
   - D3 (từ lượt trước): vai trò riêng từng người — phương án A hay B.
     KHÔNG chặn M3.
   - Lát cắt này KHÔNG cần Owner quyết gì thêm.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   M3 lát cắt 2: tách `delivered` khỏi `closed` — đơn chỉ được `closed` khi đã
   thu đủ tiền. Dùng đúng cổng chuyển trạng thái hiện có, không mở tuyến mới.

9. CHƯA XÁC MINH ĐƯỢC
   - Hành vi trên máy vận hành — vì cố ý chưa triển khai
   - Tranh chấp thật hai phiên đua nhau — cần hai tiến trình song song
   - Người dùng thật thấy gì khi gặp GHI_DE_XUNG_DOT — cần trình duyệt

10. TRẠNG THÁI CHUNG
   [x] PASS — đủ bằng chứng cho phạm vi đã khai (máy phát triển), không việc chặn

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén: docs/OWNER-REQUEST-LEDGER.md (mục #184–#187) ·
   .governance/registry/tech-debt.md (định dạng + DEBT-130) ·
   package.json (danh mục cổng) · lược đồ 3 bảng M3 đọc TRỰC TIẾP TỪ CSDL.
   Lát cắt này KHÔNG đụng giao diện nhìn thấy được nên không cần đọc lại
   docs/UI-STANDARD.md — 0 thay đổi về lớp trình bày, chỉ thêm trường vào gói
   dữ liệu gửi lên máy chủ.
═══════════════════════════════════════════
```
