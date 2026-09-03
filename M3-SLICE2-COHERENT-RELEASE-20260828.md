# BÁO CÁO — M3 GỘP: NĂM CỔNG AN TOÀN LÁT CẮT 1 + `delivered` KHÁC `closed`

**Mã việc:** `WP-ERP-M3-SLICE2-COHERENT-RELEASE-20260828`
**Ngày:** 28/08/2026
**Nguồn giao việc:** Sổ Yêu Cầu Owner `#188`
**Kết luận:** `PASS_LIVE_CONVERGED`

---

## 0. ĐỌC PHẦN NÀY LÀ ĐỦ

Lượt trước tôi báo đã chống được ghi đè bằng mốc `bao_gia.ngay_sua`. **Kiểm lại thì mốc đó
không đủ tư cách.** Quét toàn bộ đường ghi cho ra **bảy hàm** sửa dòng con mà **không hàm nào
chạm cột mốc**. Nghĩa là:

> Người B sửa một phương án → mốc **vẫn y nguyên** → người A lưu cả gói với mốc cũ →
> **cổng cho qua** → việc của B **bị xoá sạch**.

Đúng cái hỏng mà cổng sinh ra để chặn. Nay thay bằng **vân tay nội dung** toàn báo giá, tính
**dưới khoá**. Không thêm một cột nào.

Cùng lượt, `delivered` và `closed` được tách thật: đơn chỉ đóng được khi **nguồn công nợ chuẩn
chứng minh đã thu đủ tiền**; không có chứng từ thì **từ chối**, không mặc định coi là 0 đồng.

Đã **phát hành lên máy vận hành** và kiểm bằng trình duyệt thật: `V1.00.357` → **`V1.00.359`**.

---

## 1. CURRENT STATE ĐẦU PHIÊN — ĐO LẠI, KHÔNG TIN BÁO CÁO

| Định danh | Đo được | Khớp báo cáo? |
|---|---|---|
| local HEAD | `f232d46` | ✅ |
| remote `main` | `f232d46` — trùng local | ✅ |
| **VPS Git working-copy HEAD** | `826817b` · **759 tệp lệch** | ✅ đúng như dự kiến (triển khai bằng tệp nén) |
| **Deployed content SHA** | `6d8f1d8` | ✅ |
| App version | `V1.00.357` | ✅ |
| Artifact fingerprint | `e73812151f8a2f627136604578da2bbe` | ✅ |
| Tiến trình | online · **0 lần khởi động lại** · `cwd = .standalone-run` | ✅ |
| Sổ di trú | **10 dòng** | ✅ |
| Huỷ đơn | chỉ còn `SALES=0` — **không vai trò nghiệp vụ nào được cấp** | ✅ |
| CSDL | 101 bảng · 51 menu · 9 vai trò · 148/67/4 quyền · 1695 KH | ✅ |

**Không có lệch nào cần xử.** Git ref cũ trên máy vận hành là `METADATA_DIFF`, không phải
trôi mã — bản kê `DEPLOYED_SHA.txt` tự khai điều đó.

> ⚠️ **Một chỗ báo cáo phiên trước ghi SAI, nay đính chính:** phiên trước kết luận *"không có
> khoá SSH nên chưa soi chéo được `DEBT-132`"*. **Sai.** Khoá có sẵn tại `~/.ssh/id_ed25519_tanphat`;
> lần đó tôi chỉ thiếu tham số `-i`. Lượt này soi chéo được và đóng luôn khoản nợ đó (§5).

---

## 2. NĂM CỔNG AN TOÀN CỦA LÁT CẮT 1

### 2.1 `B3` — mốc phiên bản: phát hiện nặng nhất

Quét `src/lib/m3-store.ts` cho ra **11 hàm ghi** vào ba bảng báo giá. Trong đó **7 hàm** sửa
dòng con mà **KHÔNG chạm `bao_gia.ngay_sua`**:

```
createBaoGiaItem · updateBaoGiaItem · deleteBaoGiaItem
createBaoGiaOption · updateBaoGiaOption · deleteBaoGiaOption
updateBaoGiaOptionSelection          (+ unified-pricing-adapter ghi thẳng bảng phương án)
```

Thêm nữa `bao_gia.ngay_sua` là kiểu `text`, **không** có `ON UPDATE`, **không** do CSDL tự quản.

**Cách vá — ưu tiên 2 của Owner, không cần DDL:** vân tay `SHA-256` trên **toàn bộ nội dung
nghiệp vụ** của báo giá (phần đầu + mọi dòng hàng + mọi phương án), tính **bên trong giao dịch**
sau khi đã khoá đủ ba tập dòng:

```sql
SELECT * FROM bao_gia        WHERE id = ?           FOR UPDATE
SELECT * FROM bao_gia_item   WHERE id_bao_gia = ?   FOR UPDATE
SELECT * FROM bao_gia_option WHERE id_bao_gia = ?   FOR UPDATE
```

Bất kỳ hàm nào trong 11 hàm đổi dữ liệu ⇒ vân tay đổi ⇒ gói cũ bị từ chối. Không sửa 11 hàm,
không thêm cột, không trigger.

> ⚠️ **Một cổng giả đã tránh được.** Bản nháp đầu định băm bằng `MD5(GROUP_CONCAT(...))` ngay
> trong SQL. Bỏ, vì `group_concat_max_len` mặc định **1024 byte**: báo giá quá ~30 dòng sẽ bị
> **cắt cụt trong im lặng**, và vân tay hoá ra **mù** với mọi thay đổi nằm sau chỗ cắt.

### 2.2 `B1` — gói thiếu định danh phải TỪ CHỐI, không đoán

Owner: *"payload item/option thiếu ID không được đoán theo tên hoặc thứ tự"*.

Đường ghép theo tên (thêm ở lượt trước làm "đường lui") đã **gỡ HẲN** khỏi mã sản phẩm. Nay:
báo giá **đã có dòng** mà gói **không kèm vân tay** ⇒ `REFRESH_REQUIRED`, **0 thay đổi**.
Tương thích = **từ chối an toàn và yêu cầu tải lại**, không phải "đoán tiếp".

Bài kiểm còn khoá ngược lại: mã sản phẩm **không được còn khả năng** đoán theo tên (4 khẳng
định quét thân mã, xem §4).

### 2.3 `B2` — định danh lạ phải huỷ cả giao dịch

Bản trước coi định danh lạ là **dòng mới**: an toàn về dữ liệu nhưng **che giấu** lỗi phía trình
duyệt hoặc gói bị sửa tay — người dùng tưởng vừa sửa, thực ra vừa tạo thêm. Nay **ném lỗi, huỷ
cả giao dịch**. Sáu ca đều chặn, **0 thay đổi**:

| Ca | Kết quả |
|---|---|
| định danh phương án của **báo giá khác** | `FOREIGN_ID_REJECTED` |
| định danh **dòng hàng** của báo giá khác | `FOREIGN_ID_REJECTED` |
| định danh **không tồn tại** | `FOREIGN_ID_REJECTED` |
| định danh **sai kiểu** | `FOREIGN_ID_REJECTED` |
| **hai dòng cùng định danh** trong một gói | `FOREIGN_ID_REJECTED` |
| phương án xếp **sang dòng hàng khác** trong cùng báo giá | `FOREIGN_ID_REJECTED` |

### 2.4 `B4` — trường không gửi thì GIỮ

Bệnh cũ: `data.ghi_chu ?? null`. Trình duyệt không gửi trường ⇒ `undefined` ⇒ `?? null` ⇒
**máy chủ tự xoá ghi chú của người dùng**. Chính lỗi này làm bước dọn của bài kiểm hụt ở lần
chạy đầu hôm trước.

Hợp đồng đúng: `undefined` → **giữ** · `null` gửi rõ → **xoá** · chuỗi rỗng **không tự quy đổi**.
Áp cho `ghi_chu` và `id_nguoi_lien_he` của phần đầu, `ghi_chu`/`quy_cach`/`id_san_pham` của dòng
hàng, `ghi_chu` của phương án.

### 2.5 `B5` — hai phiên tranh chấp THẬT

Không đọc mã rồi kết luận. Hai lời gọi `saveBaoGiaBundle` chạy **đồng thời** trên **hai kết nối
riêng** (mỗi `withTransaction` tự lấy một kết nối và ghim bằng `AsyncLocalStorage`).

| Vòng | Đo được |
|---|---|
| 2 phiên cùng vân tay | **đúng 1 ghi được**, phiên kia `GHI_DE_XUNG_DOT`, dữ liệu không lẫn lộn, giá vốn `5555` nguyên |
| phiên thua thử lại bằng vân tay cũ | **vẫn bị chặn** |
| phiên thua tải lại rồi lưu | **thành công** — cổng không khoá chết người dùng |
| **10 phiên cùng đua** | **1/10 ghi được**, 9 phiên nhận đúng loại lỗi, vẫn đúng 1 dòng, không treo |

### 2.6 `B6` — chứng minh trên trình duyệt thật

Hai **ngữ cảnh trình duyệt riêng** = hai phiên đăng nhập riêng, cùng mở một báo giá.

| Yêu cầu Owner | Đo được |
|---|---|
| Tab B nhận thông báo tiếng Việt dễ hiểu | *"Người khác vừa sửa báo giá này trong lúc bạn đang mở…"* |
| chỉ rõ hành động | *"Bấm F5 để xem bản mới nhất rồi nhập lại phần của bạn"* |
| KHÔNG lộ mã lỗi thô | ✅ |
| KHÔNG lộ SQL / vết ngăn xếp | ✅ |
| không mất dữ liệu | việc Tab A còn nguyên; giá vốn `4321` không về 0 |
| sau tải lại thấy bản mới | ✅ |

**14/14**, 4 ảnh.

> ⚠️ **Bộ kiểm này suýt là xanh giả.** Bản đầu chỉ có `try/finally`: một ngoại lệ giữa chừng
> nhảy thẳng xuống `finally`, in **"11/11 PASS"** rồi thoát mã 0 — trong khi khẳng định 11
> **chưa hề chạy**. Đã sửa để ngoại lệ thành một khẳng định **ĐỎ**.

---

## 3. LÁT CẮT 2 — `delivered` KHÁC `closed`

### 3.1 Truy nguồn công nợ chuẩn — đọc mọi đường ghi, không chọn cái tiện

| Ứng viên | Ai ghi | Khi nào | Kết luận |
|---|---|---|---|
| `don_hang.da_thanh_toan` | **KHÔNG AI** — chỉ đặt `0` lúc tạo đơn | không bao giờ | ❌ **cột chết** (đo trên vận hành: `0` ở **mọi** đơn) |
| `don_hang.con_lai` | **KHÔNG AI** | không bao giờ | ❌ cột chết (`con_lai = tong_tien` ở **mọi** đơn) |
| `bien_ban_nghiem_thu.da_thanh_toan` | `mf-nghiem-thu-store` | khi duyệt phiếu thu | ❌ phạm vi nghiệm thu |
| **`cong_no.so_tien_con_lai`** | `collectPersistedPhieuThu` | **khi phiếu thu sang `collected`** | ✅ **đường DUY NHẤT trừ tiền khi thực nhận** |

**Không có "nhiều nguồn cạnh tranh" cần Owner phân xử** — ba ứng viên kia không có writer,
tức chúng không phải nguồn, chúng là cột bỏ hoang.

### 3.2 Cổng đóng đơn

1. Xác thực + quyền chuyển trạng thái (`tt:dh:closed`) — đã có sẵn.
2. **Tiền kiểm** trước `executePostActions` — vì hàm đó có thể sinh việc/thông báo trước khi
   tới bước ghi, để cổng chặn ở bước sau thì đã lỡ có thay đổi một phần.
3. **Giao dịch + khoá dòng đơn + khoá dòng công nợ** ⇒ chống TOCTOU.
4. Cộng tiền bằng `SUM()` **trong CSDL** trên `decimal(15,2)`, mang về dạng **CHUỖI** —
   không đi qua số thực JavaScript.
5. **Không chứng từ ⇒ TỪ CHỐI.** `SUM` trên tập rỗng trả `NULL`; mọi cách viết `IFNULL(...,0)`
   sẽ biến *"chưa có chứng từ"* thành *"nợ 0 đồng"* ⇒ đóng được đơn chưa hề ghi nhận công nợ.
6. Bị từ chối ⇒ **0 thay đổi**, kể cả cột mốc thời gian (giao dịch rollback).

**Bỏ `nguoi_sua = 1` viết cứng** trong `updateDonHangStatus` — vi phạm `A8` có sẵn từ 21/08:
mọi lần đổi trạng thái đều bị ghi sang tài khoản người khác.

> ⚠️ **Hệ quả cần Owner biết:** máy vận hành hiện có **0 dòng `cong_no`**, **0 phiếu thu**,
> **0 phiếu giao hàng**. Nghĩa là **chưa đơn nào đóng được** cho tới khi kế toán lập công nợ
> và ghi nhận thu tiền. Đó **đúng** là điều Owner khoá, không phải tác dụng phụ ngoài ý muốn.
> Hiện cũng chưa đơn nào ở trạng thái `delivered` (đang có: `draft` · `confirmed` · `in_production`).

---

## 4. BẰNG CHỨNG

### 4.1 Bộ cổng M3 — **9 cổng, mã thoát 0**

| Cổng | Khẳng định |
|---|---|
| `test:m3-ghep-phuong-an` (nhân thuần) | **36/36** |
| `test:m3-dinh-danh` (CSDL thật) | **31/31** |
| `test:m3-tranh-chap` (hai phiên thật) | **15/15** |
| `test:m3-dong-don` (lát cắt 2) | **20/20** |
| `test:m3-huy-don` | **6/6** |
| `test:m3-duyet-gia` | **29/29** |
| `test:h3-bao-gia-don-gate` | **11/11** |
| `test:don-hang-hoan-thien` | **24/0** |
| `test:quyen-chuyen-trang-thai` | **29/0** |

**Bộ gộp KHÔNG nuốt lỗi** — gieo một khẳng định hỏng vào cổng con ⇒ bộ gộp thoát **mã 1**.

### 4.2 KIỂM NGƯỢC — **8/8 ca đạt**

Cổng chỉ có giá trị khi **gỡ bản vá ra thì nó ĐỎ**:

| Gỡ gì | Kết quả |
|---|---|
| `B2` chốt chặn định danh lạ | `E1 · E2` **ĐỎ** |
| `B3` kiểm vân tay | `F1 · I2` **ĐỎ** (4 khẳng định) |
| `B1` chặn gói thiếu vân tay | `G1` **ĐỎ** (3 khẳng định) |
| `B4` trả lại `?? null` | `H1` **ĐỎ** |
| `B3` (đo bằng bộ tranh chấp) | `1b · 1c` **ĐỎ** — hai phiên cùng ghi được |
| `C` cổng tài chính đóng đơn | **9 khẳng định ĐỎ** |
| `C` trả lại `nguoi_sua = 1` | `A3` **ĐỎ** |
| `D5` trả mốc cột audit về 67/64 | **3 khẳng định ĐỎ** |

Mọi tệp **khôi phục nguyên trạng** trong `finally`; xác minh lại bằng `tsc` sạch + chạy lại xanh.

### 4.3 Diễn tập phục hồi từ sao lưu VẬN HÀNH THẬT

| Bước | Kết quả |
|---|---|
| Sao lưu | `504K` · gzip đọc được · **101 bảng** · `SHA256 7993c4ed…` |
| Tải về, đối chiếu vân tay | **khớp từng ký tự** |
| Phục hồi lên MariaDB 10.11 | 101 bảng · **1695 KH** · 7 báo giá · 6 đơn · 148/67/4 · 10 di trú · 3 quản trị |
| Chạy trọn bộ kiểm M3 trên bản sao | 8/9 cổng xanh (xem ghi chú) |
| **Mốc nền sau khi chạy** | **mọi chỉ tiêu nguyên vẹn · 0 rác** |
| Đường lùi | `server.js` + `node_modules` + `.next` + bản kê cũ + sao lưu CSDL — **đủ và đọc được** |

> Cổng duy nhất không chạy trọn trên bản sao là `test:m3-duyet-gia` (**26/29**): nó tham chiếu
> tài khoản `uat_*@local.test` **chỉ có trên máy phát triển**; bản sao vận hành chỉ có 9 tài khoản
> người thật. **Không phải lỗi bản phát hành** — cùng bộ đó chạy **29/29** trên máy phát triển.
> Ghi `DEBT-134`.

### 4.4 Kiểm khói TRÊN MÁY VẬN HÀNH — **14/14**

Dựng dữ liệu thử **bằng `mysql` chạy ngay trên máy vận hành qua SSH** (không mở đường hầm CSDL),
kiểm hành vi **bằng trình duyệt thật vào địa chỉ công khai**.

| Nhóm | Đo được |
|---|---|
| `G2` CSRF **có phiên hợp lệ** | nguồn gốc **lạ → 403**, nguồn gốc **nhà → 409** ⇒ **phân biệt được** |
| `G3` định danh phương án | **giữ nguyên** sau khi lưu (`22 → 22`) |
| `G3` giá vốn | **`7777.00`**, không về 0 |
| `G3` ghi chú | **`GHI CHU GOC`**, không bị máy chủ xoá |
| `G4` đơn **chưa có chứng từ** | **không đóng được**, trạng thái không đổi |
| `G4` thông báo | **không lộ mã lỗi thô** |
| `G4` đơn **đã thu đủ** | **đóng được** (`closed`) |
| `Z` dọn sạch | tài khoản `9→9` · báo giá `7→7` · đơn `6→6` · công nợ `0→0` · KH `1695→1695` · quản trị `3→3` |

> Điểm đo CSRF chọn **có chủ đích** là `/api/tinh-gia/quotes` POST — tuyến **đã bị chặn ghi từ
> trước**, nên dù cổng CSRF có cho qua cũng **không sinh được một dòng dữ liệu nào**. Đo an ninh
> mà không đụng dữ liệu thật. (`/api/m3/stats` không dùng được: chỉ nhận GET nên POST trả **405
> trước cả cổng CSRF** — đo được 405 cho cả hai nguồn gốc, không phân biệt gì.)

### 4.5 An ninh — **14/14**

API ẩn danh **401** (3 tuyến) · phiên giả **401** · nguồn gốc lạ **bị chặn** (2 ca) · 4 trang
nghiệp vụ ẩn danh **đẩy về đăng nhập** · trang đăng nhập **không lộ** chuỗi kết nối/mật khẩu/khoá,
**không lộ** vết ngăn xếp · phiên bản người dùng thấy = **`V1.00.359`**.

### 4.6 Cổng tĩnh

`check:governance` **5/5 byte-identical** · `secret-scan` · `pii-scan` · `script-parse` ·
`ledger-dup-id` · `notion-sync-state` · `version-policy` **37/37** · `tsc` **0 lỗi** ·
`npm run build` **đạt** · **48/48 cổng mồ côi XANH, 0 đỏ**.

---

## 5. `DEBT-131` / `DEBT-132` — SỐ LIỆU CHÍNH XÁC

Owner chỉ ra báo cáo hôm trước **tự mâu thuẫn** (vừa nói "hai đỏ đã xử hết", vừa nói "còn một đỏ").
Nay số liệu nhất quán:

| | Lần trước | Lần này |
|---|---|---|
| Cổng mồ côi **đo được** | 55 | **48** (mẫu số giảm vì 7 cổng đã vào `test:m3-gates`) |
| XANH | 53 | **48** |
| ĐỎ | 2 | **0** |
| Chưa đo được (cần trình duyệt/tham số) | 12 | 13 |

**Ba cổng đỏ của lần trước đều đã đóng:** `test:h3-bao-gia-don-gate` **11/11** ·
`test:m1-owner` **75/0** · `test:audit-cols` **15/15**.

### `DEBT-132` — đóng bằng bằng chứng hai đầu

| | Đo được |
|---|---|
| Cổng chờ | `ngay_sua = 67` · `nguoi_sua = 64` |
| **Máy vận hành** | **69 · 66** |
| **Máy phát triển** | **69 · 66** |
| Danh sách **tên bảng** | **trùng khít từng tên** ở cả hai cột |
| **Hai bảng gây chênh lệch** | **`ncc_dia_chi` + `ncc_lien_he`** — tạo **21/08/2026** (`0e1ca8e`), sau mốc cũ `d56e726` (09/08) **12 ngày** |

⇒ **mốc lỗi thời**, không phải máy phát triển trôi. Đã cập nhật mốc trong cổng kèm dẫn nguồn,
giữ nguyên văn dòng cũ trong chú thích. **KHÔNG sửa CSDL cho khớp cổng.**

---

## 6. PHÁT HÀNH

| | |
|---|---|
| Phiên bản | `V1.00.357` → **`V1.00.359`** |
| Mã nguồn (private) | `91b594a` (358) → `ce6dadf` (359) → `98c4e95` (bộ kiểm) |
| **Mã đã triển khai** | **`ce6dadf3e84aab810c8c4b176595021be311e3eb`** |
| Vân tay bản dựng | **`a25cf21c390e198a9be1fd431309d318`** |
| Di trú / bước dữ liệu | **0 DDL**; bước nạp dữ liệu chạy đủ (`DATA_STEP=DA_NAP`) |
| Sao lưu | CSDL ``<thư-mục-sao-lưu-máy-vận-hành>/<tên-tệp>`` (`SHA256 7993c4ed…`) · thư mục chạy ``<thư-mục-sao-lưu-máy-vận-hành>/<tên-tệp>`` (122M) · bản kê cũ |
| Đường lùi | bản kê `V1.00.357` / `6d8f1d8` / vân tay `e7381215…` — đã kiểm đọc được |

Chuỗi phát hành chạy đúng thứ tự `DEBT-129`:
**dựng → tiền kiểm → nạp dữ liệu bắt buộc → cổng chặn → kích hoạt**.

---

## 7. TRIỂN KHAI

| | |
|---|---|
| Mã đã triển khai | `ce6dadf` |
| Mốc | build `2026-08-28 11:01:45` · deploy `2026-08-28T04:01:49Z` |
| Tiến trình | **online · 0 lần khởi động lại** · `cwd = .standalone-run` |
| Phiên bản người dùng thấy | **`V1.00.359`** |
| **VPS Git HEAD (ghi riêng)** | `826817b` — **`METADATA_DIFF`**, không phải trôi mã |
| Kích hoạt | đạt; hai dấu mốc đã dọn đúng |

**Một lần vá tiến giữa chừng, khai rõ:** `V1.00.358` lên máy vận hành xong thì kiểm khói bắt
được màn đơn hàng **in mã lỗi thô** cho người dùng. **Không lùi bản phát hành** — cổng tài chính
chạy đúng; lùi lại là gỡ một cổng đúng vì một tiền tố hiển thị. Vá tiến `V1.00.359`, kiểm lại
**14/14**.

Bản vá đó lại lộ **lỗi thứ hai của chính nó**, do **bản dựng sản phẩm** bắt: đặt hàm hiển thị
trong tệp có `import { query } from "@/lib/db"` ⇒ thành phần trình duyệt kéo theo `node:async_hooks`
⇒ dựng hỏng. **`tsc` báo 0 lỗi** — chỉ bản dựng mới bắt. Đã tách `src/lib/m3/loi-hien-thi.ts`.
Ghi `DEBT-135`.

---

## 8. KIỂM KHÓI SAU TRIỂN KHAI

| Kiểm | Kết quả |
|---|---|
| Xác thực / phân quyền / CSRF | **14/14** (§4.5) + CSRF có phiên **403 vs 409** |
| Toàn vẹn báo giá | **4/4** |
| Tranh chấp | chứng minh ở máy phát triển **15/15**, trình duyệt **14/14** |
| `delivered` / `closed` | **4/4** trên máy vận hành |
| Huỷ đơn | **không vai trò nghiệp vụ nào** được cấp |
| Nhật ký | tệp lỗi **không đổi** kể từ `04:42 (+07)` — trước lần triển khai `11:01` **6 giờ** ⇒ **0 lỗi mới** |
| Dọn dẹp | **0 tài khoản / báo giá / đơn / công nợ kiểm thử còn sót** |

---

## 9. ĐỐI CHIẾU BA NƠI

| Thành phần | Máy phát triển | Remote `main` | Máy vận hành | Kết luận |
|---|---|---|---|---|
| Mã nguồn (HEAD) | `98c4e95` | `98c4e95` | `ce6dadf` (đã triển khai) | ✅ vận hành đi sau đúng **1 commit chỉ đụng bộ kiểm** |
| App version | `V1.00.359` | `V1.00.359` | `V1.00.359` | ✅ |
| Vân tay bản dựng | — | — | `a25cf21c…` | ✅ |
| **Băm 7 tệp nguồn** (chuẩn hoá xuống dòng) | — | — | — | ✅ **7/7 trùng khít** |
| MariaDB | `10.11.10` | — | `10.11.10-log` | ✅ khác hậu tố bản dựng |
| Số bảng | 101 | — | 101 | ✅ |
| Cột audit | 69 / 66 | — | 69 / 66 | ✅ |
| Sổ di trú | — | — | 10 | ✅ |
| Quyền menu/hành động/trường | — | — | 148 / 67 / 4 | ✅ |
| **VPS Git working-copy HEAD** | — | — | `826817b` | ⚠️ **`METADATA_DIFF`** — triển khai bằng tệp nén |
| Tiến trình | — | — | online · 0 khởi động lại | ✅ |

**Kết luận: `FULL_RELEASE_CONVERGED`** (kèm `METADATA_DIFF` đã biết ở git ref của máy vận hành).

---

## 10. CHƯA XÁC MINH ĐƯỢC — GHI THẲNG

1. **Chưa có người dùng thật thao tác** trên bản này. Mọi bằng chứng vận hành đến từ **tài khoản
   tạm** do bài kiểm dựng rồi tự xoá. Chưa đổi quyền tài khoản người thật nào.
2. **Chưa đo đường đóng đơn với dữ liệu công nợ THẬT** — vì máy vận hành có **0 dòng công nợ**.
   Ca "đã thu đủ" chạy trên chứng từ do bài kiểm dựng rồi xoá.
3. **Công nợ phát sinh từ phiếu giao hàng chưa gom được** (`DEBT-133`) — hỏng ở lược đồ, cần DDL.
4. **`test:m3-duyet-gia` chưa diễn tập được** trên bản sao vận hành (`DEBT-134`).
5. **Chưa có cổng chặn sớm** việc thành phần trình duyệt nhập mô-đun kéo theo tầng máy chủ
   (`DEBT-135`) — lần này phải đợi tới bước dựng mới biết.
6. **Chưa đo tải thật**: khoá dòng được chứng minh bằng 10 phiên đua trong phòng thí nghiệm,
   chưa đo dưới lưu lượng người dùng thật.
7. **13 cổng cần trình duyệt/tham số** vẫn chưa nằm trong lần quét tự động nào.

---

## 11. ĐANG CHỜ OWNER

**KHÔNG CÓ** câu hỏi nào chặn.

`D3` (vai trò riêng từng người) vẫn treo từ lượt trước nhưng **không chặn M3** — và theo đúng
chỉ thị lần này, gói quyết định đó sẽ được **viết lại** sau khi M3 phát hành xong, không hỏi
Owner chọn A/B từ bản cũ.

---

## 12. NỢ KHÔNG CHẶN

| Mã | Nội dung | Bằng chứng | Điều kiện xử |
|---|---|---|---|
| `DEBT-133` | Công nợ từ **phiếu giao hàng** không nối được về đơn — `so_phieu` kiểu CHUỖI vs `id_phieu_giao_hang` kiểu SỐ | MariaDB trả `Unknown column 'pgh.id'` | **Cần Owner duyệt DDL**. Rủi ro hiện tại **0** (0 phiếu giao hàng, 0 công nợ) |
| `DEBT-134` | `test:m3-duyet-gia` không diễn tập được trên bản sao vận hành | 3/29 đỏ với `AUTH_REQUIRED`; 29/29 trên máy phát triển | Phiên dọn cổng kiểm |
| `DEBT-135` | `tsc` mù với lỗi nhập mã máy chủ vào thành phần trình duyệt | `tsc` 0 lỗi ↔ `build` hỏng | Phiên dọn cổng kiểm |
| `DEBT-131` | Còn nhóm M1/M5/P1/R11/R12 chưa vào bộ gộp | 48/48 xanh, 13 cổng chưa đo được | Phiên dọn cổng kiểm |
| `DEBT-116` `DEBT-121` `DEBT-123` `DEBT-128` `DEBT-130` | giữ nguyên backlog | — | không mở trong lượt này |

---

## 13. BÁO CÁO CÔNG KHAI

| | |
|---|---|
| Tệp | `M3-SLICE2-COHERENT-RELEASE-20260828.md` |
| Kho báo cáo công khai | `Baocaoerptanphat` · commit **`fe6c27d`** |
| Tệp raw | **HTTP 200** · 28.341 byte |
| Kho mã nguồn riêng | `b261c39` (báo cáo + sổ) · `98c4e95` (bộ kiểm) · `ce6dadf` (**mã đã triển khai**) |

Đã chạy cổng an toàn trước khi đẩy công khai: `secret-scan` · `pii-scan` ·
`script-parse` · `ledger-dup-id` — tất cả **PASS**. Báo cáo **không** chứa mật khẩu,
khoá, chi tiết SSH nhạy cảm, bản kết xuất dữ liệu, dữ liệu cá nhân hay đường dẫn bí mật.

---

## 14. VIỆC KẾ TIẾP — ĐÚNG MỘT VIỆC

**Viết lại gói quyết định `D3`** (vai trò riêng từng người) theo đúng hướng Owner đã chỉ:
`chu_so_huu_user_id` bám đúng kiểu của `user_account.id`, chốt chặn khi gán, chính sách mồ côi
/ vô hiệu hoá, gói di trú + hoàn tác + bài kiểm, và xem xét gộp với `DEBT-128`.

---

## 15. KHỐI BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Đo lại hiện trạng ba nơi, không tin báo cáo: 0 lệch cần xử
   - B3: phát hiện mốc `ngay_sua` KHÔNG đủ tư cách (7 hàm ghi dòng con không
     chạm nó) → thay bằng VÂN TAY NỘI DUNG tính dưới khoá, 0 DDL
   - B1: bỏ HẲN đường đoán theo tên; gói thiếu vân tay ⇒ REFRESH_REQUIRED
   - B2: định danh lạ ⇒ huỷ cả giao dịch (6 ca)
   - B4: trường không gửi thì GIỮ (trước: máy chủ tự xoá)
   - B5: hai phiên tranh chấp thật, 10 phiên cùng đua
   - B6: chứng minh trên trình duyệt thật, hai tab, 4 ảnh
   - Lát cắt 2: `closed` đòi bằng chứng thu đủ từ nguồn công nợ chuẩn;
     không chứng từ ⇒ fail-closed; giao dịch + khoá dòng chống TOCTOU
   - Bỏ `nguoi_sua = 1` viết cứng (vi phạm A8 có sẵn)
   - Vá tiến V1.00.359: cắt mã lỗi thô khỏi màn hình người dùng
   - Tách `loi-hien-thi.ts` sau khi bản dựng bắt lỗi nhập mã máy chủ
   - Đóng DEBT-131 (một phần) và DEBT-132 (bằng chứng hai đầu)
   - Diễn tập phục hồi từ sao lưu vận hành thật, triển khai, kiểm khói, đối chiếu

2. PHẠM VI
   ĐỤNG    : src/lib/m3-store.ts · src/lib/m3/{phien-ban-bao-gia,dong-don-hang,
             loi-hien-thi,ghep-phuong-an}.ts · src/app/m3/{bao-gia,don-hang}/* ·
             scripts/tests/ (4 bộ mới + 3 bộ sửa) · package.json · version.ts ·
             sổ nợ · sổ Owner · MÁY VẬN HÀNH (V1.00.357 → V1.00.359)
   KHÔNG ĐỤNG: lược đồ CSDL (0 DDL) · tuyến mới · khoá menu · kho quyền ·
             Pricing Plan · công thức giá · DEBT-128 · D3

3. BẰNG CHỨNG
   npm run test:m3-gates          → 9 cổng, mã thoát 0      → DB_PROVEN
   kiểm ngược 8 ca                → 8/8 ĐẠT                 → CODE_PROVEN
   npm run test:anh-xung-dot      → 14/14 (trình duyệt)     → UI_PROVEN
   npm run test:khoi-van-hanh     → 14/14 (máy vận hành)    → LIVE_VERIFIED
   kiểm khói an ninh              → 14/14 (máy vận hành)    → LIVE_VERIFIED
   diễn tập phục hồi              → mốc nền nguyên vẹn      → DB_PROVEN
   48 cổng mồ côi                 → 48 XANH / 0 ĐỎ          → CODE_PROVEN
   npx tsc --noEmit               → 0 lỗi                   → CODE_PROVEN
   npm run build                  → đạt                     → CODE_PROVEN
   đối chiếu 7 băm tệp nguồn      → 7/7 trùng khít          → RUNTIME_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #188

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit fe6c27d · tệp
       M3-SLICE2-COHERENT-RELEASE-20260828.md · raw HTTP 200
       (mã kho mã nguồn riêng ghi riêng ở mục 13 — cổng kiểm chỉ xác minh mã
        của kho báo cáo)

6. CÒN SÓT / CHƯA LÀM
   - DEBT-133 công nợ từ phiếu giao hàng (cần Owner duyệt DDL)
   - DEBT-134 test:m3-duyet-gia không diễn tập được trên bản sao vận hành
   - DEBT-135 chưa có cổng chặn sớm mã máy chủ lọt vào thành phần trình duyệt
   - DEBT-131 phần ngoài M3: nhóm M1/M5/P1/R11/R12 chưa vào bộ gộp
   - 13 cổng cần trình duyệt/tham số chưa vào lần quét tự động nào

7. ĐANG CHỜ OWNER
   KHÔNG CÓ câu hỏi chặn.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Viết lại gói quyết định D3 theo hướng `chu_so_huu_user_id`, kèm chốt chặn
   khi gán, chính sách mồ côi, gói di trú + hoàn tác + bài kiểm.

9. CHƯA XÁC MINH ĐƯỢC
   - Chưa có người dùng thật thao tác trên bản này
   - Chưa đo đóng đơn với công nợ THẬT (máy vận hành có 0 dòng công nợ)
   - Chưa đo dưới tải thật

10. TRẠNG THÁI CHUNG
   [x] PASS — đủ bằng chứng, đã live, không việc chặn

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén: CLAUDE.md (bản luật đang nạp) · docs/OWNER-REQUEST-LEDGER.md
   (#184–#187) · .governance/registry/tech-debt.md · package.json (toàn bộ danh
   mục cổng) · hai báo cáo M0-HYGIENE-TO-M3 và M3-DINH-DANH-PHUONG-AN ·
   lược đồ 6 bảng M3/công nợ đọc TRỰC TIẾP TỪ CSDL cả hai môi trường.
   Lượt này KHÔNG đụng lớp trình bày nên không cần đọc lại docs/UI-STANDARD.md —
   thay đổi giao diện duy nhất là NỘI DUNG câu thông báo lỗi, không đổi bố cục,
   màu, khoảng cách hay thành phần nào.
═══════════════════════════════════════════
```
