# BÁO CÁO — D3: VAI TRÒ RIÊNG BÁM ĐỊNH DANH TÀI KHOẢN ỔN ĐỊNH

**Gói việc:** `WP-ERP-M0-D3-IMPLEMENT-STABLE-ACCOUNT-OWNED-ROLE`
**Owner duyệt:** 14:07 · 28/08/2026 — `GO_D3_STABLE_ACCOUNT_OWNED_ROLE_WITH_BINDING_CONDITIONS`
**Bản phát hành:** `V1.00.360`
**Trạng thái:** `LIVE_VERIFIED_BY_REPORT`

---

## 1. TÓM TẮT CHO NGƯỜI KHÔNG ĐỌC MÃ

Trước lượt này, hệ thống chỉ có **vai trò dùng chung** (Kinh doanh, Kế toán, Kho…).
Muốn cho một người quyền **khác mẫu**, người quản trị phải tạo thêm một vai trò —
nhưng hệ thống **không biết** vai trò đó là của riêng ai. Hệ quả: vai trò riêng của
người A có thể bị gán nhầm cho người B, và không ai đếm được vai trò nào đã "mồ côi".

Nay hệ thống **biết** vai trò nào là riêng và riêng của ai, bám theo **số định danh
tài khoản** — không bám email. Đổi email không làm lệch quyền.

Kèm theo, màn phân quyền có thêm một thẻ hướng dẫn nói rõ **ba cách làm** và cho
**xem trước** hậu quả trước khi bấm.

---

## 2. VÌ SAO KHÔNG CHỈ "THÊM Ô TICK" — ĐIỀU DỄ HIỂU SAI NHẤT

Quyền hiệu lực trong hệ thống là **hợp các quyền**: một người mang nhiều vai trò thì
được cộng dồn những gì các vai trò đó cấp.

Vì vậy **bỏ tick ở một vai trò nghĩa là "vai trò này không cấp", KHÔNG phải "cấm"**.
Nếu giữ vai trò mẫu rồi bỏ tick ở vai trò riêng thì vai trò mẫu **vẫn cấp** — màn hình
sẽ nói đã thu hồi trong khi thực tế chưa.

Đây chính là điều kiện 2 Owner khoá: *«Không được giữ preset cũ trong khi UI tuyên bố
quyền đã bị thu hồi»*.

**Bằng chứng đo được** (bộ `test:d3-hopquyen`, mục A1–A5):

| Bước | Kết quả đo |
|---|---|
| Người mang vai trò mẫu, một màn đang có hiệu lực | nguồn cấp = vai trò mẫu |
| **Bỏ tick** màn đó trên vai trò riêng | màn **VẪN CÒN** — nguồn cấp vẫn là vai trò mẫu |
| **Gỡ** vai trò mẫu | màn **MỚI** thật sự mất |

⇒ Nên chế độ **Thu Hẹp So Với Mẫu** phải **nhân bản vai trò mẫu rồi THAY THẾ**, không
cho giữ lại vai trò mẫu cũ.

---

## 3. ĐÃ LÀM ĐỦ NĂM ĐIỀU KIỆN OWNER KHOÁ

| # | Điều kiện | Đã làm gì | Bằng chứng |
|---|---|---|---|
| **1** | CHECK phải bao luôn `la_admin = 1` ⇒ `la_vai_tro_rieng = 0` | Bản di trú tạo **2 ràng buộc CHECK** + khoá ngoại `ON DELETE RESTRICT` + chỉ mục. Chặn thêm ở **tầng lưu trữ** để người dùng nhận câu tiếng Việt thay vì lỗi CSDL | Máy vận hành: 2/2 CHECK, chặn thật 3/3 phép thử ngược (mục B1–B4 kiểm khối) |
| **2** | Nhân bản rồi thay thế khi thu hẹp | Ba chế độ: Giữ Nguyên · Cấp Thêm (giữ mẫu) · Thu Hẹp (nhân bản rồi thay thế **nguyên tử trong một giao dịch**) | Kiểm khối vận hành C5–C6: sinh đúng 1 vai trò riêng **và** vai trò mẫu đã bị gỡ |
| **3** | Định danh ổn định là đích của lệnh; không tin email từ trình duyệt | Giao diện · tầng hành động · tầng lưu trữ đều nhận `user_account.id`. Máy chủ khoá dòng tài khoản theo định danh rồi **tự đọc email dưới khoá** | Đo trên trình duyệt: **16 lệnh POST, 0 lệnh mang email mục tiêu** (C8) |
| **4** | Phạm vi giao dịch có giới hạn | Dùng lại `withTransaction` sẵn có. **Không** tái cấu trúc 14 hàm ghi | Tiêm lỗi giữa giao dịch: hoàn tác sạch, 0 rác, làm lại thì thành công (bộ đồng thời, mục 4a–4f) |
| **5** | Đóng `DEBT-137` trong cùng bản phát hành | Một hằng điều kiện dùng **chung** cho cả hai hàm đếm quản trị, có thêm vế "phải có mật khẩu" | Chứng minh bằng **đăng nhập thật** rồi vào màn quản trị, kèm kiểm ngược người không quyền bị đẩy sang `/403` |

---

## 4. MỘT PHÁT HIỆN KỸ THUẬT ĐÁNG GHI — CÚ PHÁP CSDL KHÔNG ĐỐI XỨNG

MariaDB 10.11 nhận **hai dạng khác nhau** cho hai loại ràng buộc:

```
khoá ngoại  :  ADD CONSTRAINT <tên> FOREIGN KEY IF NOT EXISTS (...)
ràng buộc CHECK:  ADD CONSTRAINT IF NOT EXISTS <tên> CHECK (...)
```

Viết ngược là **lỗi cú pháp**. Bản di trú đầu tiên viết ngược ⇒ **cả ba ràng buộc âm
thầm không được tạo**, trong khi lệnh vẫn có vẻ chạy xong.

Điều bắt được lỗi này là **5/5 phép thử ngược đều đỏ** đúng như phải thế — nếu chỉ nhìn
"lệnh chạy không báo lỗi" thì bản di trú đã lên máy vận hành với **0 ràng buộc**.

> Bài học ghi lại: ràng buộc chỉ được coi là có khi **đo được nó CHẶN THẬT**, không phải
> khi thấy tên nó trong danh mục.

---

## 5. HAI LỖI GIAO DIỆN DO ẢNH NGHIỆM THU CHỈ RA — ĐÃ SỬA TRONG CÙNG LƯỢT

| Lỗi | Vì sao nghiêm trọng | Đã sửa thế nào |
|---|---|---|
| Ô **"Mục Menu Sau"** tô **xanh lá cố định** | Khi người dùng sắp mất **sạch** quyền, ô đó hiện số **0** trên nền **xanh** — màu báo *tốt* đúng lúc kết quả *xấu nhất*. Màn hình nói ngược nội dung | Màu chọn theo **phép so**: giảm ⇒ hổ phách · mất hết ⇒ đỏ kèm chữ **"MẤT HẾT"** · giữ nguyên ⇒ xanh |
| Nhãn màn hình hiện **mã thô** | Hiện *"Menu M0 Phong Ban"* thay vì tên Owner quen mắt | Lấy **tên thật** từ cơ sở dữ liệu trước, mới đến bản đồ mã, cuối cùng mới Title Case |

Cả hai chỉ lộ ra khi **nhìn ảnh chụp thật**, không lộ ra qua bài kiểm tự động.

---

## 6. BẢNG KIỂM — SỐ ĐO, KHÔNG PHẢI LỜI KHẲNG ĐỊNH

### 6.1 Trên máy phát triển

| Bộ | Nội dung | Kết quả |
|---|---|---|
| `test:d3` | Bất biến sở hữu · quản trị không được là riêng · nhân bản-thay thế · chốt quản trị cuối · mồ côi | **32/32 đạt** |
| `test:d3-dongthoi` | 6 phiên cùng tạo · 4 phiên cùng thay thế · 3 phiên cùng gỡ quản trị · tiêm lỗi giữa giao dịch | **23/23 đạt** |
| `test:d3-hopquyen` | Hợp các quyền · quản trị được đếm phải đăng nhập được thật | **14/14 đạt** |
| `test:d3-anh` | Ảnh ba kích thước + hành vi thật trên trình duyệt | **22/22 đạt** |
| | **CỘNG** | **91/91 đạt** |

### 6.2 Kiểm ngược — cổng chỉ có giá trị khi gỡ ra thì nó đỏ

| Gỡ cái gì | Phép thử phải chuyển đỏ | Kết quả |
|---|---|---|
| Bất biến sở hữu ở tầng lưu trữ | gán vai trò riêng của A cho B | **đỏ đúng** |
| Chặn quản trị-riêng ở tầng lưu trữ | tạo vai trò quản trị + riêng | **đỏ đúng** |
| Điều kiện đếm quản trị (trả về bản cũ) | tài khoản không mật khẩu bị tính nhầm | **đỏ đúng** |
| Bước gỡ vai trò mẫu | "thay thế" hoá ra chỉ là "bổ sung" | **đỏ đúng** |
| Kiểm "đang mang vai trò nguồn" | thay thế thứ tài khoản không có | **đỏ đúng** |
| **Khoá dòng `FOR UPDATE`** | 6 phiên cùng tạo vai trò riêng | **đỏ đúng** — 5/6 phiên đụng mã |

⇒ **6/6 phép thử ngược đạt.** Không có cổng giả.

### 6.3 Diễn tập trên bản sao máy vận hành

Bản sao dựng từ bản sao lưu vận hành thật (101 bảng): **31/31 đạt**, gồm cả **lùi sạch**
và **ba lần chạy lặp** không đổi kết quả.

### 6.4 Trên máy vận hành thật

| Nhóm | Nội dung | Kết quả |
|---|---|---|
| A | Phiên bản · lược đồ · 0 trigger · 0 dòng cần nạp bù | 7/7 |
| B | Ràng buộc **chặn thật**, không phải chỉ có tên | 4/4 |
| C | Đăng nhập · thẻ D3 · xem trước · áp dụng · định danh trong lệnh | 8/8 |
| D | Bất biến sở hữu chặn khi gán chéo · quản trị người thật không đổi | 5/5 |
| Z | Dọn sạch về đúng mốc nền | 1/1 |
| | **CỘNG** | **24/24 đạt** |

**Mốc nền vận hành trước và sau đều bằng nhau:** 9 tài khoản · 9 vai trò · 11 lượt gán ·
101 bảng · 3 quản trị đăng nhập được. **Không một tài khoản người thật nào bị đổi quyền.**

---

## 7. LƯỢC ĐỒ — THÊM GÌ, VÀ NẠP BÙ BAO NHIÊU

| Đối tượng | Nội dung |
|---|---|
| Cột mới | `dm_vai_tro.la_vai_tro_rieng` · `dm_vai_tro.chu_so_huu_user_id` |
| Khoá ngoại | tới `user_account(id)`, `ON DELETE RESTRICT` — xoá tài khoản còn sở hữu thì **bị chặn** |
| CHECK 1 | vai trò riêng **phải** có chủ; vai trò dùng chung **phải** không có chủ |
| CHECK 2 | vai trò quản trị **không được** là vai trò riêng |
| Chỉ mục | tra nhanh vai trò riêng theo chủ sở hữu |
| **Nạp bù dữ liệu** | **0 dòng.** Đo trên máy vận hành: 9/9 vai trò là dùng chung, giá trị mặc định của hai cột mới **đúng bằng** giá trị cần ⇒ lệnh thêm cột **chính là** bước nạp bù |
| Trigger | **0** — Owner cấm tạo, đã đo lại sau khi triển khai |

---

## 8. ĐƯỜNG LÙI

Có kịch bản lùi `rollback:d3`, **đóng an toàn**:

1. Đếm vai trò riêng — **còn dòng nào thì DỪNG**, không chạy câu lệnh nào.
2. Xuất **bản kê quyền sở hữu** ra tệp ngoài cơ sở dữ liệu trước khi đụng gì.
3. Ghi nhận vào sổ di trú.

**Đã kiểm ngược:** dựng một vai trò riêng rồi chạy lệnh lùi ⇒ **từ chối**, mã thoát 1,
hai cột **còn nguyên**. Đúng hành vi cần có.

> Lưu ý thực dụng: bản di trú này chỉ **THÊM** cột, nên lùi *mã ứng dụng* về bản trước
> **không bắt buộc** phải gỡ cột — mã cũ không đọc hai cột đó nên vẫn chạy đúng.

---

## 9. HAI ĐIỀU TÔI NÓI SAI TRONG LÚC LÀM — NÊU RA ĐỂ KHÔNG AI TIN NHẦM

1. **Tôi nói dây chuyền máy vận hành không dùng mảng `MIGRATIONS`.** Sai. Bước 3/8 của
   kịch bản triển khai **có** gọi `migrate:m0-security`, tức mảng đó đúng là đường thật.
   Bản di trú D3 nay nằm ở **cả hai** chỗ (mảng đó và danh sách `DI_TRU_SQL`) — thừa
   nhưng an toàn, và có ghi chú tại chỗ để phiên sau khỏi vấp.

2. **Ba phép thử B1–B3 của bộ kiểm khối ban đầu báo đỏ oan.** Ràng buộc chặn đúng ngay
   từ đầu; bài kiểm đọc nhầm thông điệp lỗi (`mysql` thoát khác 0 nên thông điệp thật
   nằm ở luồng ra, không nằm ở `message`). Đã sửa bài kiểm, không sửa ràng buộc.

---

## 10. MỘT QUYẾT ĐỊNH VẬN HÀNH — NÊU RÕ, KHÔNG GIẤU

Cổng kích hoạt **chặn đúng** ở lần triển khai đầu: đòi phải chạy bước nạp dữ liệu bắt
buộc trước. Tôi đã **đo** rồi mới quyết:

| Đo cái gì | Kết quả |
|---|---|
| Bốn đợt nạp dữ liệu trước đó | **đã có đủ** trên máy vận hành (145 ô quyền menu · 66 ô quyền hành động · 3 vai trò trưởng phòng · 29 ô quyền chuyển trạng thái) |
| D3 cần nạp bù bao nhiêu dòng | **0** — 9/9 vai trò đã đúng mặc định, 0 dòng vi phạm ràng buộc |

⇒ Dùng đúng **lối ra mà chính cổng đó quy định** cho bản phát hành không cần nạp dữ liệu.
Bản kê phát hành ghi lại `DATA_STEP=KHAI_KHONG_CAN` nên việc này **tra lại được**, không
biến mất.

Trước đó đã chạy **tiền kiểm** đầy đủ (đạt toàn phần: phụ thuộc lúc chạy · cơ sở dữ liệu
đích · bản sao lưu mới · chạy khô ba kịch bản nạp · đường lùi).

---

## 11. MỘT LỖ TRUY VẾT PHÁT HIỆN VÀ VÁ NGAY TRONG LƯỢT NÀY

Lần triển khai đầu, dòng đầu bản kê ghi **`KHONG_KHAI`** thay vì mã nguồn.

Nguyên nhân: biến mã nguồn **chỉ** được truyền ở đường triển khai bằng gói nén, **không**
truyền ở đường triển khai qua git. Nghĩa là **mọi** lần triển khai qua git đều mất khả
năng đối chiếu *"máy vận hành đang chạy mã nào"* — đúng thứ cần nhất khi phải truy nguồn
sự cố.

Đã vá và triển khai lại: hai trường định danh nay **khớp nhau**.

---

## 12. NGOÀI PHẠM VI — CỐ Ý KHÔNG LÀM

| Việc | Vì sao không làm |
|---|---|
| `DEBT-128` — đổi khoá chính bảng gán quyền · 13 điểm tra theo email · 108 cột quy kết | Owner **cấm gộp**. Gộp là biến rủi ro thấp thành rủi ro cao và mất khả năng lùi từng phần |
| Cổng kiểm **kiểu** cho thư mục `scripts/` | Đã **đo chi phí**: 120 lỗi kiểu có sẵn. Vá hết sẽ phình xa ngoài phạm vi ⇒ ghi `DEBT-138` |
| `DEBT-131` · `DEBT-133` · `DEBT-134` · `DEBT-135` | Không đụng tới |
| Trigger cơ sở dữ liệu | Owner cấm. Đã đo lại sau triển khai: **0 trigger** |
| Đổi tuyến · menu · quyền sở hữu module · viết cứng vai trò theo tên người | Owner cấm |

---

## 13. SỔ SÁCH

| Sổ | Nội dung |
|---|---|
| Sổ Yêu Cầu Owner | mục **#190** (quyết định + kết quả đủ năm điều kiện) · mục **#191** (ba việc cố ý không làm) |
| Sổ nợ kỹ thuật | **đóng** `DEBT-136` · **đóng** `DEBT-137` · **thêm** `DEBT-138` |
| Bộ tiêu chí nghiệm thu giao diện | `docs/reports/UI-CHECKLIST-m0-security-d3-20260828.md` — sao ra **trước** khi sửa dòng mã đầu tiên, kết quả N1–N9 ghi đầy đủ, nêu **đích danh** 2 điểm lệch đã sửa và 1 nợ đã biết không sửa |

---

## 14. GÓI BÀN GIAO CHO NOTION (`GOV-NOTION-HANDOFF-001`)

| Trường | Nội dung |
|---|---|
| **Mã mục sổ** | `#190` · `#191` |
| **Nguyên văn Owner + mốc thật** | `GO_D3_STABLE_ACCOUNT_OWNED_ROLE_WITH_BINDING_CONDITIONS` — **14:07 · 28/08/2026** |
| **Phạm vi áp dụng** | Vai trò riêng bám định danh tài khoản + phép đếm quản trị khôi phục. Trong `dm_vai_tro`, tầng lưu trữ bảo mật, màn `/m0/security` |
| **Điều CẤM mở rộng** | Không đổi khoá chính bảng gán quyền · không sửa 13 điểm tra theo email · không di trú 108 cột quy kết · không tạo trigger · không mở Pricing Plan · không bảng quyền song song |
| **Trang Notion cần sửa** | **CHƯA XÁC ĐỊNH** — cần Agent Notion xác định trang mô tả mô hình phân quyền M0 |
| **Bằng chứng** | `V1.00.360` · mã nguồn triển khai ghi trong bản kê phát hành trên máy vận hành · lớp bằng chứng **RUNTIME_PROVEN** |
| **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | ① **So sánh hồi quy theo điểm ảnh chưa có** — dự án chưa có ảnh nền + ngưỡng tự động; nghiệm thu giao diện dựa trên đối chiếu **bằng mắt** trên ảnh thật. ② Bất biến sở hữu **không** được cơ sở dữ liệu giữ (nó nối hai bảng) — nó do **tầng lưu trữ** giữ; đã chứng minh chặn thật trên máy vận hành, nhưng ai viết đường ghi **thứ ba** mà không đi qua tầng đó thì vẫn lách được. ③ Chỉ đo với **9 vai trò** hiện có; chưa có số liệu ở quy mô hàng trăm vai trò riêng. |

---

## 15. CÒN LẠI

- `DEBT-138` — thư mục `scripts/` chưa có cổng kiểm kiểu (120 lỗi có sẵn, đã đo chi phí).
- `DEBT-128` — giữ **MỞ**, đúng chỉ đạo không gộp.
- `DEBT-129` — bước kích hoạt vẫn xoá thư mục phụ thuộc ở gốc kho; đã có hai kịch bản
  vá đường đi nhưng gốc chưa đổi.

---

---

## 16. KHỐI BÁO CÁO KẾT THÚC (`GOV-COMPLETION-REPORT-001`)

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Bản di trú D3: 2 cột + khoá ngoại ON DELETE RESTRICT + 2 ràng buộc CHECK + chỉ mục
   - Tầng nghiệp vụ mới src/lib/security/vai-tro-rieng.ts: khoá dòng, bất biến sở hữu,
     sinh mã bám định danh, đọc quyền menu KÈM NGUỒN CẤP, đếm vai trò riêng mồ côi
   - Tầng lưu trữ: assign/remove nhận ĐỊNH DANH SỐ + giao dịch + khoá dòng;
     hai luồng taoVaiTroRiengBoSung / nhanBanVaThayThe; nhân bản quyền hành động
   - Tầng hành động: 2 hàm đổi đầu vào sang user_id + 4 hàm mới (xem trước, cấp thêm,
     thay thế, đọc vai trò riêng)
   - Giao diện: thẻ "Cấp Quyền Riêng Cho Một Người" ở BƯỚC 2 của /m0/security —
     ba chế độ, hiện nguồn cấp từng quyền, so trước/sau, MỘT nút Áp Dụng nguyên tử
   - Đóng DEBT-137: một hằng điều kiện dùng chung cho hai hàm đếm quản trị
   - Lệnh lùi rollback:d3 đóng an toàn + kiểm ngược
   - Vá 2 lỗ dây chuyền triển khai: di trú chưa vào danh sách DI_TRU_SQL;
     đường git không ghi mã nguồn vào bản kê (KHONG_KHAI)
   - 5 bộ kiểm mới (115 phép thử) + sửa 2 bộ kiểm cũ vỡ do đổi chữ ký hàm

2. PHẠM VI
   ĐỤNG    : migrations/20260828_d3_* (2 tệp) · src/lib/security/vai-tro-rieng.ts ·
             src/lib/security-store.ts · src/app/m0/security/{actions.ts,
             security-client.tsx,the-vai-tro-rieng.tsx} · src/lib/version.ts ·
             scripts/{rollback-d3-*,run-m0-security-migration,vps-nap-du-lieu-phat-hanh,
             vps-pull-and-restart} · scripts/tests/d3-*(5 tệp) ·
             scripts/tests/p1-{quan-tri-khoi-phuc,hop-quyen-truong} ·
             package.json · .gitignore · sổ Owner · sổ nợ · 2 báo cáo
             CSDL: bảng dm_vai_tro (THÊM cột, KHÔNG đổi cột cũ)
   KHÔNG ĐỤNG: user_role_mapping (khoá chính giữ nguyên) · 13 điểm tra quyền theo email ·
             108 cột quy kết · Pricing Plan · tuyến · menu · quyền tài khoản người thật ·
             trigger (0 trước, 0 sau) · DEBT-128/131/133/134/135

3. BẰNG CHỨNG
   npm run test:d3                  -> 32/32 PASS            -> DB_PROVEN
   npm run test:d3-dongthoi         -> 23/23 PASS            -> DB_PROVEN
   npm run test:d3-hopquyen         -> 14/14 PASS            -> RUNTIME_PROVEN
   npm run test:d3-anh              -> 22/22 PASS            -> UI_PROVEN
   kiểm ngược 6 chốt chặn           -> 6/6 chuyển ĐỎ đúng    -> CODE_PROVEN
   diễn tập trên bản sao vận hành   -> 31/31 PASS            -> DB_PROVEN
   npm run test:d3-khoi-van-hanh    -> 24/24 PASS            -> RUNTIME_PROVEN
   npm run test:gov-gates           -> 37/37 PASS            -> FILE_PROVEN
   npm run build                    -> Compiled successfully -> CODE_PROVEN
   bản kê phát hành trên máy vận hành: APP_VERSION=V1.00.360, mã nguồn khớp
                                                             -> RUNTIME_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #190 (quyết định + kết quả) và #191 (ba việc cố ý không làm)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho erptanphat · commit 0060d615da856521887de6276b88fbfb33e86a0b
       (đây là commit CHA của chính báo cáo này — một trường không thể trích
        dẫn mã commit chứa chính nó; commit của báo cáo nằm ngay sau nó trên
        nhánh main)

6. CÒN SÓT / CHƯA LÀM
   - DEBT-138: thư mục scripts/ chưa có cổng kiểm KIỂU (đã đo: 120 lỗi có sẵn)
   - DEBT-128: giữ MỞ theo đúng chỉ đạo không gộp
   - DEBT-129: bước kích hoạt vẫn xoá thư mục phụ thuộc ở gốc kho — gốc chưa đổi
   - So sánh hồi quy theo điểm ảnh: dự án chưa có ảnh nền + ngưỡng tự động

7. ĐANG CHỜ OWNER
   - Trang Notion nào mô tả mô hình phân quyền M0 -> cần Owner/Agent Notion xác định
     để bàn giao mục #190. Chặn: chỉ chặn khâu đồng bộ tài liệu, KHÔNG chặn vận hành.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Bàn giao mục #190 cho Agent Notion theo gói ở mục 14.

9. CHƯA XÁC MINH ĐƯỢC
   - Hành vi ở quy mô hàng trăm vai trò riêng — vì sao: máy vận hành hiện có 9 vai trò,
     0 vai trò riêng thật. Ai xác minh được: chỉ sau khi Owner dùng thật một thời gian.
   - Đường ghi THỨ BA nào đó không đi qua tầng lưu trữ vẫn lách được bất biến —
     vì sao: bất biến nối hai bảng nên CSDL không giữ được. Ai xác minh được: cổng quét
     tĩnh tìm mọi nơi ghi vào user_role_mapping (chưa có).

10. TRẠNG THÁI CHUNG
   [x] PASS — đủ bằng chứng, không còn việc chặn

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Tài liệu ĐÃ ĐỌC LẠI sau nén:
     - docs/UI-STANDARD.md — TOÀN PHẦN, dòng 1-522 (hai lượt: 1-340 và 341-522)
     - docs/UI-ACCEPTANCE-CHECKLIST.md — đọc rồi sao ra bản riêng cho màn này
     - .governance/registry/tech-debt.md — tra trước khi ghi DEBT-138
     - docs/OWNER-REQUEST-LEDGER.md — tra mục #189 trước khi ghi #190
     - migrations/20260828_d3_*_rollback.sql — đọc lại trước khi viết lệnh lùi
     - scripts/vps-pull-and-restart.sh · vps-nap-du-lieu-phat-hanh.sh ·
       vps-activate-standalone.sh — đọc trước khi vá dây chuyền triển khai
═══════════════════════════════════════════
```

---

*Báo cáo này công khai được: không chứa email thật, mật khẩu, thẻ phiên, chuỗi kết nối,
định danh hạ tầng máy chủ, hay dữ liệu khách hàng.*
