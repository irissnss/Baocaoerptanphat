# BÁO CÁO — XÁC MINH HỘI TỤ · DỌN RỦI RO TRIỂN KHAI · ĐÓNG M0 · MỞ M3

**Mã việc:** `WP-ERP-M0-HYGIENE-TO-M3-20260827`
**Ngày:** 27–28/08/2026

---

## 0. ĐỌC PHẦN NÀY LÀ ĐỦ

Báo cáo hội tụ hôm trước **đúng**: đo lại độc lập cho **0 lệch** trên cả mã nguồn lẫn cơ sở dữ liệu.

Nhưng lượt này tìm ra **một lỗ quyền thật đang tồn tại trên máy vận hành**:
**nhân viên kinh doanh huỷ được đơn hàng**. Đã sửa cả hai đầu — dữ liệu và kịch bản.

Và bản vá `DEBT-129` **vừa tự chứng minh trên một lần triển khai thật**: tiền kiểm phát hiện
một bước chắc chắn hỏng và **dừng chuỗi trước khi kích hoạt**, máy vận hành không hề bị đụng tới.

---

## 1. PHA A — XÁC MINH HỘI TỤ (CHỈ ĐỌC)

### 1.1 Bốn định danh, tách riêng

| Định danh | Giá trị |
|---|---|
| Local working-tree HEAD | `46c92c7` (0 thay đổi, 0 tệp chưa theo dõi) |
| Remote private `main` | `46c92c7` — **trùng local** |
| **VPS Git working-copy HEAD** | **`826817b`** — cũ, do triển khai bằng tệp nén |
| **Deployed content identity** | `DEPLOYED_SHA.txt` = `9c741f7` · `PATCH = 356` · tiến trình nạp `.standalone-run` · vân tay bản dựng `f2482464d7a42206` |

`9c741f7 → 46c92c7` chỉ có **một commit tài liệu**; `git diff` trên `src/ scripts/ migrations/ package.json`
**rỗng** ⇒ nội dung mã đã triển khai **bằng** mã trên `main`.

### 1.2 Đối chiếu mã — 11/11 khớp

| Tệp | Kết luận |
|---|---|
| `khoa-thanh-ben.ts` · `menu-catalog.ts` · `tam-ngung.ts` · `transition-catalog.ts` | khớp trực tiếp |
| `app-navigation-metadata.ts` · `sidebar.tsx` · `security-client.tsx` · `action-permission.ts` · `security-store.ts` · `api-guard.ts` · `version.ts` | **khớp sau khi chuẩn hoá xuống dòng** — lệch ban đầu chỉ do CRLF/LF |

**Chốt chặn quản trị cuối — đọc thẳng mã ĐÃ DỰNG đang chạy**, không tin cây nguồn:

```js
async function ag(a,b,d){ if(b){ await $(a,"khoá tài khoản này"), await (0,c.query)(`UPDATE user_account SET locked_until = …
async function $(a,b){ if(await Z(a) && await Y(a) < 1) throw Error(`Kh\xf4ng thể ${b}…
```

`$` = chốt chặn, `Z` = `laQuanTriKhoiPhucDuoc`, `Y` = `demQuanTriKhoiPhucDuoc`. Chốt chặn
**chạy trước lệnh ghi**.

> ⚠️ Suýt kết luận sai: `grep` cho **0 kết quả** với chuỗi thông báo lỗi, và một hàm cục bộ
> biến mất tên sau khi dựng. Nguyên nhân thật là **bộ đóng gói thoát ký tự phi-ASCII**
> (`Kh\xf4ng`), không phải thiếu mã. Tải khối mã về đọc bằng Node mới ra sự thật.

### 1.3 Đối chiếu cơ sở dữ liệu — 9/9 khớp báo cáo

| Chỉ tiêu | Báo cáo | Vận hành | Kết luận |
|---|---|---|---|
| BASE TABLE | 101 | **101** | khớp |
| `dm_menu` | 51 | **51** | khớp |
| vai trò | 9 | **9** | khớp |
| quyền menu | 148 | **148** | khớp |
| quyền hành động | 67 | **67** | khớp |
| quyền trường | 4 | **4** | khớp |
| màn con Tài chính | 7 | **7** | khớp |
| quản trị khôi phục được | 3 | **3** | khớp |
| khách hàng | 1695 | **1695** | khớp |

MariaDB: cùng `10.11.10`, khác hậu tố bản dựng (`-log` trên vận hành, `-ubu2204` trên phát triển).

### 1.4 Kết luận Pha A

**`CONTENT_AND_DB_CONVERGED`** — đồng thời **`METADATA_ONLY_DIVERGENCE`**.

Git ref trên máy vận hành cũ **không phải** lệch mã. Nhưng cũng **không** nói "mọi thứ giống hệt tuyệt đối".

---

## 2. 🔴 LỖ QUYỀN THẬT TÌM ĐƯỢC

Audit hiện trạng M3 làm lộ ra:

```
tt:dh:cancelled   SALES   allowed=1
```

**Nhân viên kinh doanh huỷ được đơn hàng.** Owner khoá nhiều lần: *«Hủy đơn chỉ ADMIN + CEO»*.

**Nguyên nhân:** kịch bản Đợt 5 cấp *"mọi bước của quy trình mà vai trò sửa được màn"* — nên
cấp luôn bước huỷ. Chính bản phát hành hội tụ hôm qua đã chạy kịch bản đó.
**Mã máy chủ gác đúng** (`requireChuyenTrangThai`); **chỉ dữ liệu sai**.

**Sửa cả hai đầu:**

| Đầu | Cách |
|---|---|
| Dữ liệu | Bản di trú đặt cờ về 0, **giữ dấu vết** (không xoá dòng). Đã áp lên vận hành: `SALES=0`, 6 bước khác **không đụng tới** |
| Kịch bản | Thêm `BUOC_KHONG_TU_CAP` — **không tự cấp lại** ở lần triển khai sau |

**Không viết cứng mã vai trò** (dự án cấm): dùng **mặc-định-cấm**, quản trị tick tay cho CEO.
Việc tick tay được ghi nhật ký, khác hẳn cấp âm thầm bằng kịch bản.

Cổng `npm run test:m3-huy-don` — **6/6**, khoá cả hai đầu. Hoàn tác diễn tập hai chiều: 6→5→6.

---

## 3. PHA B — DỌN RỦI RO TRIỂN KHAI

### 3.1 `DEBT-129` — đã sửa, và **đã tự chứng minh trên máy thật**

**Tái hiện** trong hộp cát: chạy đúng hai dòng 62–63 của kịch bản kích hoạt ⇒ loader hỏng ngay.

**Đồ thị phụ thuộc:**
```
npm ci → node_modules → di trú → dựng → KÍCH HOẠT → rm -rf node_modules
                                            ↑
              4 kịch bản nạp dữ liệu KHÔNG nằm trong chuỗi — chạy tay, lúc đó đã muộn
```

**Bản vá** (sửa kịch bản đang có, **không dựng khung thứ hai**):

```
dựng → TIỀN KIỂM → NẠP DỮ LIỆU → KÍCH HOẠT (có CỔNG CHẶN)
```

- **Tiền kiểm** (`vps-preflight-release.sh`, chỉ đọc): nạp được phụ thuộc · đúng CSDL đích ·
  có sao lưu đọc được và mới ≤24h · chạy khô được các kịch bản · có đường lùi.
- **Nạp dữ liệu** (`vps-nap-du-lieu-phat-hanh.sh`): chạy khi `node_modules` **còn nguyên**.
- **Cổng chặn**: thiếu dấu `.release-data-ok` ⇒ **DỪNG**, trừ khi khai tường minh
  `KHONG_CAN_NAP_DU_LIEU=1`. Dấu **bị dọn sau mỗi lượt** nên lượt sau buộc kiểm lại.

**Đối chứng** (`npm run test:debt129` — **16/16**): thiếu phụ thuộc ⇒ tiền kiểm **ĐỎ** và
**không ghi dấu** · chưa tiền kiểm ⇒ nạp dữ liệu **TỪ CHỐI** · thiếu dấu dữ liệu ⇒ kích hoạt
**DỪNG trước khi đụng pm2**.

### 3.2 Bản vá tự chứng minh — và tìm thêm một điểm hỏng

Triển khai `V1.00.357` **bị chính tiền kiểm chặn**:

```
HỎNG  chạy khô HỎNG: nap-quyen-chuyen-trang-thai.ts
      → ENOENT: docs/anh-kiem-thu/stg-ma-tran-quyen.json
TIỀN KIỂM HỎNG: 1 điều kiện. KHÔNG ghi dấu ⇒ chuỗi triển khai phải dừng.
```

**Máy vận hành không bị đụng tới:** vẫn `V1.00.356`, chạy liên tục **78 phút**, **0 lần khởi
động lại thêm**, cả hai dấu đều vắng, trang vẫn 200.

**Điều tìm ra:** kịch bản Đợt 5 đọc một tệp mốc nằm trong thư mục **bị git bỏ qua**, nên nó
**không bao giờ chạy được trên máy vận hành**. Lần hội tụ hôm qua nó chạy được **chỉ vì** được
chạy từ máy phát triển qua đường hầm SSH.

**Sửa:** bỏ nó khỏi danh sách chạy, thay bằng **kiểm KẾT QUẢ** — đếm số ô `tt:%` đang được cấp;
bằng 0 thì **báo đỏ** và chỉ rõ phải chạy từ máy phát triển. Không im lặng bỏ qua.

> Chép tệp mốc lên máy vận hành sẽ trái `GOV-SECRET-LOCATION-001` — không làm.

### 3.2b Triển khai lại — chuỗi mới chạy trọn

Sau khi chuyển Đợt 5 sang **kiểm kết quả**, chuỗi chạy hết:

```
TIỀN KIỂM ĐẠT — đã ghi dấu .release-preflight-ok
  ô quyền chuyển trạng thái đang được cấp: 29
NẠP DỮ LIỆU ĐẠT — đã ghi dấu .release-data-ok
[standalone] Cổng dữ liệu ĐẠT
```

⚠️ **Lần triển khai đó lộ ra hai lỗi trong chính bản kê** — mà bản kê sinh ra để nói đúng:

| Trường | Ghi sai | Nguyên nhân |
|---|---|---|
| mã nguồn | `KHONG_DOC_DUOC` | dùng `require()` trong tệp **ESM** ⇒ luôn ném lỗi, bị `catch` nuốt |
| `DATA_STEP` | `KHAI_KHONG_CAN` | bước **dọn dấu chạy TRƯỚC** bước ghi bản kê |

Đã sửa cả hai và triển khai lại. **Bản kê cuối cùng ghi đúng:**

```
6d8f1d8b3ce44565caf14ca3fae5ef548b3dd9ac
APP_VERSION=V1.00.357
BUILD_TIMESTAMP=2026-08-28 00:08:40
DEPLOY_TIMESTAMP=2026-08-27T17:08:44Z
ARTIFACT_FINGERPRINT=e73812151f8a2f627136604578da2bbe
ENVIRONMENT=production
DATA_STEP=DA_NAP
GIT_WORKTREE_HEAD=826817b126961a995e6a0517ade7b29c339d340d
```

Dòng cuối là điều đáng giá nhất: bản kê **tự nói ra** rằng git ref trên máy vận hành
**khác** mã đã triển khai — không để ai nhầm nữa.

**Ghi nợ `DEBT-130`** cho phần còn lại: dựng môi trường MỚI thì Đợt 5 vẫn phải chạy tay
từ máy phát triển. Cần chốt hẳn — đưa tệp mốc vào kho ở vị trí hợp lệ sau khi xác minh
nó không chứa bí mật, hoặc sinh lại tệp mốc từ chính cơ sở dữ liệu.

### 3.3 `B2` — bản kê phát hành đọc được bằng máy

Tái dùng `DEPLOYED_SHA.txt` (không mở tuyến API mới). Mỗi lần kích hoạt ghi:
mã nguồn · phiên bản · mốc dựng · mốc triển khai · **vân tay bản dựng** · môi trường ·
bước dữ liệu · `GIT_WORKTREE_HEAD` (để thấy rõ nó khác mã đã triển khai).

### 3.4 `DEBT-122` — đã sửa, **không DDL**

**Audit trước khi làm:** `schema_migrations` **đã tồn tại** ở cả hai môi trường, đủ ba cột,
**đã có sẵn `UNIQUE KEY (name)`** — nhưng **0 dòng ở cả hai** và **không tệp mã nào ghi vào**.

Tái dùng đúng bảng đó: `INSERT IGNORE`, gọi **sau** khi câu lệnh chạy xong. Ghi sổ hỏng
**không** làm hỏng bước di trú nhưng có báo ra.

**Kiểm trên bản sao dựng từ sao lưu vận hành thật:** 0 → **10 dòng**; chạy lại vẫn **10**.

⚠️ **Không nạp bù lịch sử.** Chứng minh được **rằng** các bản cũ đã chạy, nhưng **không** chứng
minh được **lúc nào**. Bịa mốc thời gian để sổ trông đầy đủ là làm hỏng chính thứ sổ dùng để làm.

Cổng `npm run test:debt122` — **19/19**, có khẳng định *"không dựng hệ thống di trú thứ hai"*.

---

## 4. PHA C — BẰNG CHỨNG VẬN HÀNH

⚠️ Máy vận hành **không có tài khoản kiểm thử chuyên dụng nào** — cả **9/9 đều là người thật**.
Nên tôi dựng **tài khoản tạm riêng**, gán vai trò **đã có sẵn**, dùng xong **xoá ngay**, và
khẳng định cuối so mốc nền. **Không một tài khoản người thật nào bị đụng.**

### 4.1 CSRF — 4/4 trên HTTPS công khai

| Kiểm | Kết quả |
|---|---|
| Đăng nhập thật lấy phiên | ✅ |
| Phiên hợp lệ + nguồn gốc **đúng** | 400 (kiểm dữ liệu) — **không** phải 403 ✅ |
| Phiên hợp lệ + nguồn gốc **lạ** | **403 · "Nguồn gốc yêu cầu không hợp lệ"** ✅ |
| Không `Origin`/`Referer` (máy-sang-máy) | qua CSRF ✅ |
| Mutation | **0** — `bao_gia=7 · don_hang=6 · pricing_quote_history=0` không đổi ✅ |

### 4.2 Ma trận vai trò — 5 vai trò, kiểm cả payload máy chủ

| Kiểm | Kết quả |
|---|---|
| Chỉ ADMIN thấy Trung tâm phân quyền | ✅ |
| CEO thấy đủ 7 màn Tài chính, **không** vào được `/mf` | ✅ |
| KE_TOAN thấy màn Tài chính | ✅ |
| USER (chờ cấp phát) thấy **0** mục nghiệp vụ | ✅ |
| USER vào **thẳng** route ⇒ bị đẩy về `/403` | ✅ |
| USER gọi API nghiệp vụ ⇒ 403 | ✅ |
| Giá vốn **không** có trong payload của SALES · KE_TOAN · USER | ✅ |
| Mốc nền sau khi dọn | ✅ nguyên vẹn |

### 4.3 ⚠️ HAI ĐIỂM ĐỎ BAN ĐẦU ĐỀU LÀ **LỖI CÁCH ĐO CỦA TÔI**

Ghi lại vì suýt báo nhầm thành lỗ hổng:

1. **"Mọi route trả 200 cho USER"** — sai. `fetch()` **tự đi theo chuyển hướng** rồi báo mã của
   `/403`. Đo lại bằng **điều hướng thật**: USER **bị đẩy về `/403`** ở cả ba route; ADMIN vào
   đúng trang. Cổng gác hoạt động đúng.
2. **"CSRF không bắn"** — sai. **Trình duyệt cấm đặt tiêu đề `Origin`**; `fetch` trong trang
   lặng lẽ bỏ nó, nên hai lần gọi **giống hệt nhau**. Phải gửi **từ ngoài trình duyệt**.

### 4.4 C3 — CEO real-user UAT

**`REAL_USER_UAT_PENDING`.** Không có tài khoản CEO kiểm thử chuyên dụng, và tôi **không dùng**
tài khoản CEO của người thật. Các kiểm kỹ thuật cho vai trò CEO đã làm bằng tài khoản tạm (mục 4.2).

---

## 4.5 KIỂM KHÓI SAU TRIỂN KHAI `V1.00.357`

| Kiểm | Kết quả |
|---|---|
| `/` · `/auth/login` · `/mf` · `/mc` · `/bieu-mau` · `/m0/security` | **200** cả sáu |
| API ẩn danh (`/api/m3/stats` · `/api/tinh-gia/quotes`) | **401** |
| Tiến trình | **online · 0 lần khởi động lại** |
| Lỗi thật trong nhật ký (không tính tab cũ) | **0** |
| **Huỷ đơn — vai trò nghiệp vụ còn được cấp** | **(không ai)** ✅ |
| **Sổ di trú trên máy vận hành** | **10 dòng** — `DEBT-122` chạy thật ✅ |
| Chỉ tiêu CSDL | 101 · 51 · 9 · 148 · 67 · 4 — **không đổi** |
| Khách hàng · quản trị khôi phục | **1695** · **3** — giữ nguyên |
| Tài khoản kiểm thử còn sót | **0** |

## 5. PHA D — SECURITY CENTER, CÓ GIỚI HẠN

Audit lược đồ trước khi code:

| Mục | Lược đồ đủ chưa | Kết quả |
|---|---|---|
| **D1** khác biệt trước/sau | ✅ `audit_log` có `gia_tri_cu` · `gia_tri_moi` (0 dòng, chưa ai dùng) | **ĐÃ LÀM** |
| **D2** hoàn tác quyền | ✅ cùng bảng đó | **CHƯA LÀM** — nền đã có |
| **D3** vai trò riêng từng người | ❌ **không có cột đánh dấu chủ sở hữu** | **GÓI QUYẾT ĐỊNH** |
| **D4** quick-template | — | **giữ chặn ở máy chủ** |

### 5.1 D1 — đã làm

Trước đợt này, sửa quyền menu **không để lại dấu vết nào**: `permission_log` chỉ ghi các lần
**bị từ chối** (25 dòng), `auth_audit_log` không có cột cũ/mới.

Nay ghi giá trị **trước và sau** vào `audit_log` — **không DDL**, tái dùng bảng đã có.
Ghi **sau** khi ghi thành công, và **chỉ khi thật sự có cờ đổi** (một dòng "đã đổi" mà chẳng
đổi gì làm loãng sổ). Kèm **số tài khoản chịu ảnh hưởng**.

Cổng `npm run test:nhat-ky-quyen` — **15/15**. Bài kiểm còn làm lộ một ràng buộc thật:
`audit_log` có khoá ngoại tới `user_account`, nên nhật ký **chỉ ghi được cho người thao tác có
tài khoản** — trên máy chạy thật luôn đúng vì người thao tác lấy từ phiên đăng nhập.

### 5.2 D3 — cần Owner quyết

`dm_vai_tro` **không có cột nào** đánh dấu "vai trò riêng của ai". Sáu trong tám yêu cầu của
Owner cho vai trò riêng đều cần biết chủ sở hữu.

**Một câu hỏi duy nhất**, hai phương án đầy đủ bằng chứng, có đề xuất:
`DECISION-PACK-M0-D3-VAI-TRO-RIENG-20260827.md`. **Không chặn M3.**

---

## 6. PHA E — SỔ NỢ

| Mã | Trạng thái | Chặn M3? |
|---|---|---|
| `DEBT-116` | 🔴 **MỞ** — chưa đủ bằng chứng để đóng | không |
| `DEBT-121` | 🟡 **MỞ** — cần đối chiếu nguồn ngoài kho mã; xử cùng lượt với `DEBT-116` | không |
| `DEBT-122` | ✅ **ĐÃ XỬ LÝ** — không DDL, 19/19 | — |
| `DEBT-123` | 🟡 **HOÃN CÓ CHỦ ĐÍCH** — không đổi route lượt này | không |
| `DEBT-128` | 🟡 **HOÃN CÓ CHỦ ĐÍCH** — **không đụng DDL**, đúng khoá Owner | không |
| `DEBT-129` | ✅ **ĐÃ XỬ LÝ** — tái hiện · bản vá · 16/16 đối chứng · **đã tự chứng minh trên máy thật** | — |
| `DEBT-130` | 🟡 **HOÃN CÓ CHỦ ĐÍCH** (mới) — Đợt 5 không chạy được trên máy vận hành; đã xử bằng kiểm-kết-quả, còn lại là ca dựng môi trường mới | không |

**Không ghi "sạch hết debt".** Bốn khoản còn mở, mỗi khoản có lý do và kích hoạt xử lý.

---

## 7. GATE ĐÓNG M0 — HAI LỚP

### 7.1 `M0_PHASE1_OPERATIONAL_BASELINE` — **`CLOSED / LIVE_VERIFIED`**

| Điều kiện | |
|---|---|
| Content identity xác định | ✅ 11/11 |
| DB indicators khớp | ✅ 9/9 |
| Release đang chạy | ✅ |
| Finance grouping đúng | ✅ |
| Security Center năm bước | ✅ |
| API auth/authz không thụt lùi | ✅ CSRF 4/4 · ma trận 5 vai trò |
| Bốn cost fields được bảo vệ | ✅ |
| Last-admin guard | ✅ đọc được trong mã đã dựng |
| Rollback/backup dùng được | ✅ đã diễn tập |
| Blocker production thật | **đã tìm được một** (huỷ đơn) và **đã sửa** |

### 7.2 `M0_ADVANCED_ENHANCEMENTS` — còn mở, có kích hoạt

| Mục | Trạng thái | Kích hoạt |
|---|---|---|
| Khác biệt trước/sau khi lưu | ✅ **XONG** (máy chủ) | — |
| Hoàn tác thao tác quyền | 🟡 **MỞ** — nền đã có, chưa dựng luồng | phiên sau M3 slice 1 |
| Vai trò riêng từng người | 🟡 **CHỜ OWNER** | Owner chọn A hay B |
| UAT đầy đủ vai trò không-quản-trị | ✅ đã kiểm bằng tài khoản tạm | — |
| CEO real-user UAT | 🟡 `REAL_USER_UAT_PENDING` | Owner tự kiểm |

---

## 8. PHA F — M3, MỘT LÁT CẮT

**Workstream:** `M3-BAO-GIA-DON-HANG-INTEGRITY-AND-ACCESS`. Không mở song song module nào khác.

### 8.1 Audit hiện trạng — xong

| Đối tượng | Đo được |
|---|---|
| Trang M3 | 12 trang |
| Tuyến API liên quan | 18 tuyến |
| Hành động máy chủ | `bao-gia` 16 · `don-hang` 9 · `thiet-ke` 9 · `crm` 9 · `tinh-gia-manual` 2 |
| **Định danh phương án** | `bao_gia_option` **đã có khoá chính `id`** — không cần tạo gì mới |
| Helper giao dịch | `withTransaction` **đã có** ở `src/lib/db.ts`, dùng ở 3 tệp, **M3 chưa dùng** |
| Nguồn thu tiền | `don_hang.da_thanh_toan` · `don_hang.con_lai` · `cong_no.so_tien_con_lai` — đều có, **0 dòng** |
| Che giá vốn | **4/4 cột đã che ở ranh giới máy chủ**, kể cả bẫy `SELECT *` |

### 8.2 Đã làm trong lượt này

- ✅ **Che giá vốn ở ranh giới máy chủ** — đã đủ, kiểm cả payload trên vận hành
- ✅ **Không gửi cột giá vốn tới máy khách thiếu quyền** — kiểm trên vận hành, 3 vai trò
- ✅ **Quyền huỷ đơn chỉ ADMIN + CEO** — sửa cả dữ liệu lẫn kịch bản, có cổng kiểm

### 8.3 Chưa làm — lát cắt kế tiếp

- ⬜ Định danh phương án ổn định theo `id` (hiện dùng `(thứ tự dòng, tên phương án)` — chỉ là ngăn chặn)
- ⬜ Giao dịch + khoá dòng/phiên bản chống ghi đè (dùng `withTransaction` đã có)
- ⬜ `delivered` ≠ `closed` và chốt chặn "chỉ đóng khi thu đủ tiền"

> Ba mục này **không cần Owner quyết thêm** — tiêu chí đã rõ. Chúng là lát cắt kế tiếp,
> làm tuần tự, một bản phát hành duy nhất.

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. CURRENT STATE ĐẦU PHIÊN
   local HEAD          : 46c92c7 (sạch)
   remote main HEAD    : 46c92c7
   VPS Git HEAD        : 826817b  (cũ — triển khai bằng tệp nén)
   deployed content SHA: 9c741f7  (DEPLOYED_SHA.txt + PATCH 356 + vân tay bản dựng)
   app version         : V1.00.356
   DB                  : MariaDB 10.11.10 · 101 bảng · 51 menu · 9 vai trò ·
                         148 quyền menu · 67 quyền hành động · 4 quyền trường ·
                         7 màn Tài chính · 3 quản trị khôi phục · 1695 khách hàng
   mismatch            : 0 chỉ tiêu lệch. Chỉ METADATA (git ref).

2. ĐÃ LÀM
   A: đo lại 4 định danh · 11/11 mã khớp · 9/9 CSDL khớp · đọc mã ĐÃ DỰNG để
      chứng minh chốt chặn quản trị cuối có thật
   B: DEBT-129 tái hiện + vá + 16/16 đối chứng · bản kê phát hành · DEBT-122 vá
      không DDL 19/19 · tìm thêm điểm hỏng Đợt 5 và chuyển sang KIỂM KẾT QUẢ
   C: CSRF 4/4 trên vận hành · ma trận 5 vai trò kiểm cả payload · truy ra 2 lỗi
      cách đo của chính mình
   D: D1 nhật ký trước/sau 15/15 · D3 lập gói quyết định · D4 giữ chặn
   E: cập nhật DEBT-121/122/129, không ghi "sạch hết debt"
   F: audit hiện trạng M3 · sửa lỗ quyền huỷ đơn (dữ liệu + kịch bản) 6/6

3. PHẠM VI
   ĐỤNG    : scripts/vps-preflight-release.sh · vps-nap-du-lieu-phat-hanh.sh (mới)
             scripts/vps-activate-standalone.sh · deploy-to-vps.mjs
             scripts/lib/ghi-so-di-tru.ts (mới) · 4 kịch bản di trú
             scripts/nap-quyen-chuyen-trang-thai.ts
             src/lib/security/nhat-ky-doi-quyen.ts (mới) · m0/security/actions.ts
             migrations/ (2 tệp mới) · scripts/tests/ (4 tệp mới, 2 tệp sửa)
             src/lib/version.ts · sổ Owner · sổ nợ · docs/reports/
             MÁY VẬN HÀNH: role_action_permission (thu hồi 1 cờ) · DEPLOYED_SHA.txt
   KHÔNG ĐỤNG: DEBT-128 (0 DDL) · pricing engine/formula/khổ trải/bình bài
             · tài khoản người thật · dữ liệu nghiệp vụ (1695 khách hàng nguyên)
             · 5 file luật · route/menu key/module ownership

4. BẰNG CHỨNG
   23 cổng · 573 khẳng định · 0 hỏng                    → CODE + DB_PROVEN
   DEBT-129: 16/16 với đối chứng thiếu-phụ-thuộc        → CODE_PROVEN
   DEBT-122: 0→10 dòng trên bản sao, chạy lại vẫn 10    → DB_PROVEN
   D1: 15/15, có ca ràng buộc khoá ngoại                → DB_PROVEN
   huỷ đơn: 6/6 + hoàn tác hai chiều 6→5→6              → DB_PROVEN
   CSRF trên HTTPS công khai: 4/4, 0 mutation           → LIVE_VERIFIED
   ma trận 5 vai trò trên vận hành, kiểm payload        → LIVE_VERIFIED
   điều hướng thật: USER → /403, ADMIN → trang thật     → LIVE_VERIFIED
   tiền kiểm CHẶN một lần triển khai thật               → RUNTIME_OBSERVED
   tsc sạch · secret/pii/parity PASS                    → FILE_PROVEN

5. M0 BASELINE
   M0_PHASE1_OPERATIONAL_BASELINE = CLOSED / LIVE_VERIFIED
   Phạm vi: hội tụ mã+dữ liệu · điều hướng · Trung tâm phân quyền năm bước ·
   xác thực/phân quyền API · che 4 cột giá vốn · chốt chặn quản trị cuối ·
   quyền huỷ đơn · đường lùi còn dùng được.

6. M0 ENHANCEMENT/DEBT CÒN LẠI
   D2 hoàn tác quyền      — MỞ    · nền đã có, chưa dựng luồng · không chặn M3
   D3 vai trò riêng       — CHỜ OWNER · thiếu cột chủ sở hữu   · không chặn M3
   CEO real-user UAT      — PENDING · không dùng tài khoản người thật · không chặn
   DEBT-116 / DEBT-121    — MỞ    · cần nguồn ngoài kho mã     · không chặn M3
   DEBT-123 / DEBT-128    — HOÃN CÓ CHỦ ĐÍCH                   · không chặn M3

7. M3
   audited    : 12 trang · 18 tuyến API · 45 hành động · 5 bảng · nguồn thu tiền
   implemented: che giá vốn (đã đủ) · không gửi cột giá vốn · quyền huỷ đơn
   tested     : 6/6 huỷ đơn · payload 3 vai trò trên vận hành
   deployed   : xem mục 9
   chưa làm   : định danh phương án theo id · giao dịch+khoá dòng · delivered≠closed

8. SỰ CỐ
   (a) Triển khai V1.00.357 bị TIỀN KIỂM CHẶN — Đợt 5 đọc tệp mốc nằm trong thư
       mục bị git bỏ qua nên không bao giờ chạy được trên máy vận hành.
       Containment: máy vận hành KHÔNG bị đụng (vẫn V1.00.356, 78 phút liên tục).
       Prevention: chuyển sang KIỂM KẾT QUẢ; bằng 0 thì báo đỏ, không im lặng.
       Debt: DEBT-129 (mở rộng phạm vi bản vá).
   (b) Hai điểm đỏ Pha C là LỖI CÁCH ĐO của tôi: fetch tự đi theo chuyển hướng;
       trình duyệt cấm đặt Origin. Đã truy ra và đo lại đúng.
   (c) Một lần chạy kiểm bị cắt vì quá hạn 10 phút, để sót 5 tài khoản tạm trên
       máy vận hành. Phát hiện ngay, xoá ngay, mốc nền khớp nguyên vẹn.
       Nguyên nhân: gọi .text() trên tuyến LUỒNG SỰ KIỆN — không bao giờ kết thúc.
   (d) Hai lần dấu ngoặc ngược trong lệnh python qua vỏ lệnh bị thực thi nhầm.
       Lần một chạy nhầm kịch bản kích hoạt trên máy phát triển — dừng ở
       "pm2 not found", node_modules còn nguyên 388 gói, không tệp mã nào bị sửa.

9. COMMIT/PUSH/DEPLOY
   private SHA    : 6d8f1d8 (main, đã đẩy)
   app version    : V1.00.357
   deployed SHA   : 6d8f1d8 (nội dung mã đang chạy trên máy vận hành)
   artifact       : e73812151f8a2f627136604578da2bbe
   VPS Git HEAD   : 826817b (cũ — bản kê tự khai điều này)
   public report  : xem dòng cuối
   rollback anchor: CSDL /root/backup-erp-20260827T165620Z.sql.gz ·
                    thư mục chạy /root/standalone-run-backup-20260827T165620Z ·
                    mã V1.00.356 = 9c741f7

10. ĐANG CHỜ OWNER
   MỘT câu hỏi duy nhất: D3 — vai trò riêng cho từng người, phương án A (không
   DDL, quy ước tên) hay B (thêm 2 cột có khoá ngoại). Gói quyết định đầy đủ ở
   DECISION-PACK-M0-D3-VAI-TRO-RIENG-20260827.md. KHÔNG chặn M3.

11. CHƯA XÁC MINH
   - CEO real-user UAT (không dùng tài khoản người thật)
   - Chi phí khoá bảng khi di trú với tải thật
   - DEBT-121: phân loại 5/7 tên tổng hợp — cần nguồn ngoài kho mã
   - Trải nghiệm người dùng thật sau khi CEO được thêm 34 màn (từ lượt trước)

12. BƯỚC TIẾP THEO — ĐÚNG MỘT VIỆC
   M3 lát cắt kế tiếp: định danh phương án báo giá theo `id` ổn định, kèm giao
   dịch và khoá dòng/phiên bản chống ghi đè. Dùng `withTransaction` đã có.

13. VERDICT
   PASS_WITH_NONBLOCKING_DEBT
═══════════════════════════════════════════
```
