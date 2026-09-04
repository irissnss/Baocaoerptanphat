# VÁ LỖI — CẤU HÌNH QUY TRÌNH HỎNG KHÔNG CÒN LÀM SẬP MÀN PHÂN QUYỀN

> **Gói việc:** `ERP-GO-LIVE-CODE-FIX-006` · **Nợ xử lý:** `DEBT-174` · **Ngày:** 04/09/2026
> **Phát hành:** **V1.00.368 → V1.00.369** · **Phạm vi:** thuần mã nguồn — **0 di trú**, **không** đụng lớp lưu trữ, **không** đụng cơ sở dữ liệu
> **Trạng thái triển khai:** ⛔ **`DEPLOY_BLOCKED`** — bị chặn bởi **cổng của chính dự án**, xem mục 7

```
══════════════════════════════════════
  CHỮ KÝ AGENT — KHÔNG GIẢ MẠO
══════════════════════════════════════
  Vai trò : Agent IDE
  Công cụ : Claude Code 2.1.260 (tiện ích chạy TRONG Cursor 3.19.7)
  Actor   : Agent IDE — KHÔNG phải Cursor native, KHÔNG phải Agent Notion
══════════════════════════════════════
```

**PHIẾU KỸ NĂNG** — `đã yêu cầu=KHÔNG` · `đã nạp=KHÔNG` · `công cụ=Claude Code 2.1.260` · `mã băm nguồn=KHÔNG ÁP DỤNG` · `kích hoạt=không có` · `kiểm âm=ĐẠT` · `kiểm đụng độ=KHÔNG ÁP DỤNG` · `quyền ban đầu=CHỈ ĐỌC rồi được cấp quyền sửa theo gói việc` · `rủi ro tối đa=R1 sửa mã` · `dùng đường lui=SAI`.
Kỹ năng ERP: **không nạp cái nào** — Claude Code không phát hiện được chúng (đo lại trên bản 2.1.260: 0/128). Có dùng **một năng lực dựng sẵn** của công cụ để **soát an ninh** phần khác biệt, đúng phạm vi gói việc cho phép.

---

## 1. TÓM TẮT — CHỌN VIỆC GÌ VÀ VÌ SAO VIỆC ĐÓ TRƯỚC

**Việc đã chọn: vá `DEBT-174` — cấu hình quy trình hỏng làm sập toàn bộ màn Phân Quyền.**

**Vì sao việc này trước mọi việc khác:** đây là cái bẫy tự khoá. Màn Phân Quyền là **nơi duy nhất** sửa được quyền. Trước bản vá, chỉ **một dòng cấu hình quy trình sai** là mất **cả trang**: danh sách tài khoản · vai trò · ma trận quyền menu · nhật ký đăng nhập. Nghĩa là hỏng cấu hình thì **mất luôn công cụ để tự sửa**. Sát ngày đưa vào vận hành, đây là chốt chặn nguy hiểm nhất trong các mục đang mở.

**Bất đối xứng đo được:** màn Quy Trình **đã có** lưới đỡ từ trước; màn Phân Quyền thì **không có gì**.

**Bốn phần đã vá:**
1. **Lưới đỡ** — bọc riêng lời gọi danh mục bước chuyển. Một dòng hỏng không còn kéo cả trang xuống.
2. **Nói thật** — màn hình hiện cảnh báo rõ: danh sách **trống vì lỗi**, *không phải* vì vai trò hết quyền. Kèm nguyên văn câu lỗi để biết hỏng ở đâu.
3. **Chặn từ lúc lưu** — cổng kiểm chặn **cả ba** tình huống gây lỗi, áp cho **cả tạo lẫn sửa**.
4. **Chống vòng cổng** — đường sửa ghép dữ liệu theo **đúng luật mà lớp lưu trữ dùng để ghi**.

> ⚠️ **Phần 4 do chính lượt soát an ninh của gói việc này tìm ra**, không phải từ báo cáo bên ngoài: bản vá đầu ghép bằng toán tử «nếu rỗng thì lấy bản cũ», trong khi lớp lưu trữ lại quyết định ghi bằng «có gửi lên hay không». Chênh nhau ở đúng giá trị rỗng ⇒ gửi giá trị rỗng thì **lọt cổng lúc kiểm** nhưng **vẫn bị ghi xuống**. Đã sửa và có bài kiểm chặn tái diễn.

---

## 2. TRẠNG THÁI HIỆN TẠI VÀ PHIÊN CHẠY SONG SONG

| Hạng mục | Giá trị |
|---|---|
| Công cụ | Claude Code **2.1.260** trong Cursor **3.19.7** (cả hai vừa tự cập nhật trong ngày) |
| Nhánh | `main` — ngang bằng kho gốc trước và sau khi đẩy |
| Năm bản luật quản trị | **Trùng khớp tuyệt đối**, phiên bản tài liệu 2.9 |
| Phiên bản mã nguồn | **V1.00.368 → V1.00.369** |
| **Phiên song song** | **CÓ** — 2 tệp sổ đang sửa dở từ đêm trước, **chỉ thêm 9 dòng**, em **giữ nguyên tuyệt đối** |
| Cơ sở dữ liệu nội bộ | ⛔ **ĐANG TẮT** — xem mục 6 về hệ quả |

**Xử lý phiên song song:** em **không** dùng bất kỳ lệnh Git phá huỷ nào (không hoàn tác, không cất tạm, không dọn sạch), và khi commit chỉ đưa **đích danh 7 tệp của mình**. Hai tệp sổ của phiên kia **còn nguyên** trạng thái đang sửa dở.

---

## 3. MA TRẬN ĐỐI CHIẾU BỐN LỚP

| Chủ đề | Chủ dự án / Notion | Mã nguồn | Cơ sở dữ liệu | Máy vận hành | Báo cáo |
|---|---|---|---|---|---|
| M0 Phân quyền đã đóng | đã ghi nhận nghiệm thu | khớp | — | — | khớp |
| `DEBT-174` tồn tại | ghi trong sổ nợ | **đã đo lại: đúng** | — | — | mới |
| Bản vá `DEBT-174` | chưa có trên Notion | ✅ **đã có mã, đã đẩy** | không đụng | ⛔ **chưa triển khai** | tệp này |
| Phiên bản V1.00.369 | chưa có trên Notion | ✅ đã ghi | — | ⛔ máy vận hành vẫn ở bản trước | tệp này |
| Trạng thái máy vận hành | — | — | — | ⚠️ **CHƯA ĐO trong lượt này** | — |

**Nhãn:** `CODE_AHEAD` — mã nguồn đi trước Notion và đi trước máy vận hành. Đây là **trạng thái hợp lệ**, không phải xung đột.

---

## 4. TRƯỚC → SAU

| Tệp | Trước | Sau |
|---|---|---|
| Trang Phân Quyền (máy chủ) | Gọi thẳng danh mục bước chuyển trong khối chạy song song, **không có lưới đỡ** ⇒ một lỗi giết cả trang | Bọc riêng trong lưới đỡ; lỗi được giữ lại và **đưa xuống giao diện**; phần còn lại của trang vẫn sống |
| Màn Phân Quyền (giao diện) | Danh sách rỗng trông y hệt «vai trò không có quyền nào» | Hiện **dải cảnh báo vàng**: trống **vì lỗi**, kèm nguyên văn câu lỗi và chỉ dẫn sang màn Quy Trình |
| Hành động lưu quy trình | Chỉ chặn hành-động-chưa-hỗ-trợ; **không kiểm mạch lạc gì** | Thêm cổng chặn **cả ba** tình huống gây lỗi, áp cho **cả tạo lẫn sửa**; đường sửa ghép bản đang có rồi mới kiểm |
| Kiểu dữ liệu quy trình | Chỉ có kiểu và hai hàm trợ giúp | Thêm **một hàm kiểm thuần** — không đọc cơ sở dữ liệu nên **kiểm được bằng bài kiểm thật** |
| Bộ kiểm | **Không bài kiểm nào** phủ nhánh «trạng thái đích chưa khai» | Cổng mới **21 điều kiện**, đã nối vào nhóm kiểm nhanh |

**Không đụng:** lớp lưu trữ · cơ sở dữ liệu · di trú · quyền · luật quản trị · kỹ năng · sổ đăng ký · máy vận hành.

---

## 5. KIỂM THỬ — CÓ ĐỐI CHỨNG ÂM VÀ KIỂM NGƯỢC

**Cổng mới: 21 ĐẠT / 0 HỎNG.** Chia hai lớp: **hành vi** (gọi đúng hàm thật, không dựng bản sao) và **cấu trúc** (đọc mã thật, chống gỡ âm thầm).

| Nhóm | Nội dung | Kết quả |
|---|---|---|
| Đối chứng **dương** | Cấu hình mạch lạc **phải qua** — chống chặn quá tay | ĐẠT |
| Đối chứng **âm** ×8 | Trạng thái đích chưa khai · nguồn chưa khai · bắt đầu chưa khai · không khai trạng thái nào · mã trùng · mã rỗng · đích rỗng · giá trị rỗng gửi lên | ĐẠT — **bắt hết** |
| Luật độ dài mã quyền | Gọi **đúng hàm thật**: mã quá dài phải ném lỗi, mã bình thường không được ném | ĐẠT cả hai chiều |
| Cấu trúc ×10 | Lưới đỡ tồn tại **và thật sự được gọi** · lỗi truyền xuống giao diện · giao diện có hiển thị · cả tạo lẫn sửa đều qua cổng · đường sửa ghép đúng luật · cổng dùng lại hàm thật | ĐẠT |

**Kiểm ngược — chạy 3 lần, mỗi lần cổng phải ĐỎ rồi phục hồi phải XANH lại:**

| Lần | Cố ý phá | Kết quả |
|---|---|---|
| 1 | Gỡ lời gọi lưới đỡ | 🔴 **ĐỎ** đúng điều kiện, mã thoát 1 |
| 2 | Như trên, sau khi vá điểm mù | 🔴 **ĐỎ hai điều kiện** — lộ ra điểm mù đã vá đúng |
| 3 | Đổi lại cách ghép dữ liệu sai | 🔴 **ĐỎ** đúng điều kiện |
| — | Phục hồi cả ba | ✅ **XANH lại 21/21** |

> ⚠️ **Một điểm mù thật đã lộ ra ở lần kiểm ngược thứ nhất:** điều kiện «có lưới đỡ» vẫn **XANH** dù lời gọi đã bị gỡ — vì phần định nghĩa vẫn còn trong tệp. Em đã thêm điều kiện riêng kiểm **hàm bọc có thật sự được gọi hay không**. Đây đúng loại «cổng xanh chưa chứng minh được gì» mà dự án đã ghi nhận nhiều lần.

**Các cổng khác:** kiểm kiểu **ĐẠT** · dựng bản production **ĐẠT** · bộ cổng quản trị **ĐẠT toàn bộ** · chính sách phiên bản **37/37** · bốn cổng trước-commit **ĐẠT**.

**Một cổng đã bắt lỗi của em:** cổng «lệnh mồ côi» phát hiện em thêm lệnh kiểm mà **chưa nối vào bộ gộp nào**. Đã nối vào nhóm kiểm nhanh, cổng xanh lại.

**Soát an ninh phần khác biệt:** **không lỗ hổng mới nào đạt ngưỡng báo cáo**. Đã chứng minh riêng một điểm: mã trạng thái do người dùng đặt **không thể giả mạo hay đụng trùng** mã quyền của đối tượng khác. Lượt soát này cũng chính là nơi tìm ra lỗi vòng cổng ở phần 4.

---

## 6. ĐIỀU CHƯA KIỂM ĐƯỢC — NÓI THẲNG

**Cơ sở dữ liệu nội bộ đang TẮT**, và em **không tự bật**. Lý do có căn cứ: nền ảo hoá không chạy và kho **không có tệp cấu hình dựng nào được Git theo dõi**, nên **không chứng minh được** việc bật lên sẽ không tự khởi tạo hay tự chạy di trú. Đây đúng điều kiện dừng-an-toàn mà quản trị đã đo và chốt từ 25/08/2026.

**Hệ quả trung thực:**
- Bộ kiểm phân quyền cần cơ sở dữ liệu **chưa chạy được** trong lượt này.
- Đường **đầu-cuối** (lưu một cấu hình hỏng thật rồi mở màn Phân Quyền thật) **chưa đo được**. Em đã ghi thẳng điều này **trong chính tệp kiểm**, không giấu.
- Một bài kiểm trong nhóm nhanh (`test:gate-b`) **hỏng vì thiếu cơ sở dữ liệu** — lỗi **có sẵn từ trước**, không do bản vá này; cổng của em đã chạy và xanh **trước** nó.

**Máy vận hành: CHƯA ĐO trong lượt này.** Em không tuyên bố gì về trạng thái đang chạy ở đó.

---

## 7. TRIỂN KHAI — BỊ CHẶN, VÀ CHẶN ĐÚNG

**Trạng thái: ⛔ `DEPLOY_BLOCKED`.**

**Chặn bởi cổng của chính dự án** (`release:verify`), không phải do em ngại:

| Điều kiện cổng | Kết quả |
|---|---|
| HEAD là một commit phát hành hợp lệ | ✅ **ĐẠT** — V1.00.368 → V1.00.369 |
| Tệp phiên bản trên đĩa khớp HEAD | ✅ **ĐẠT** |
| Bản cục bộ = kho gốc | ✅ **ĐẠT** |
| **Cây làm việc sạch** | ⛔ **HỎNG** — còn 2 tệp sổ **của phiên song song** đang sửa dở |

**Vì sao em KHÔNG vượt qua:** cách duy nhất làm sạch cây làm việc là hoàn tác / cất tạm / dọn sạch — tức **huỷ việc đang dở của phiên khác**. Điều đó bị cấm tuyệt đối, và chính cổng này sinh ra để chặn việc triển khai khi có người khác đang viết dở.

**Việc an toàn đã hoàn tất:** vá mã · kiểm thử đầy đủ có kiểm ngược · soát an ninh · ghi nhật ký · tăng số phiên bản · commit · **đẩy lên kho mã nguồn riêng**.

**Trạng thái đường lui:** lui bằng cách hoàn tác **đúng một commit**. Không có di trú, không đổi cơ sở dữ liệu, nên lui là **sạch tuyệt đối**. **Máy vận hành CHƯA bị chạm** — vẫn ở bản trước, không chịu bất kỳ ảnh hưởng nào.

**Thang bằng chứng đạt tới:** `CODE_IMPLEMENTED` → `TEST_VERIFIED` → `COMMITTED` → `PUSHED`. **Dừng ở đây.** Chưa đạt `DEPLOYMENT_RECORDED`, chưa đạt `RUNTIME_OBSERVED`, chưa đạt `LIVE_VERIFIED`.

---

## 8. BÀN GIAO CHO TANPHATAI

| Trang / chủ đề | Hiện tại | Mới | Lớp bằng chứng |
|---|---|---|---|
| Sổ nợ kỹ thuật — `DEBT-174` | MỞ | **Đã có bản vá, chờ triển khai** — chưa đóng được vì chưa có bằng chứng lúc chạy | `PUSHED` |
| Trạng thái phát hành | V1.00.368 | **V1.00.369** đã có mã và đã đẩy, **chưa triển khai** | `PUSHED` |
| Màn Quy Trình | Lưu được cấu hình hỏng | **Có cổng chặn** cả ba tình huống, cả tạo lẫn sửa | `PUSHED` |
| Màn Phân Quyền | Một dòng hỏng giết cả trang | **Có lưới đỡ + cảnh báo nói thật** | `PUSHED` |
| Bộ kiểm | Nhánh «đích chưa khai» không ai phủ | **Cổng mới 21 điều kiện**, đã nối nhóm nhanh | `TEST_VERIFIED` |

**Ba việc TanPhatAI phải tự kiểm, không chép mù:**
1. `DEBT-174` **chưa đóng** — mới có mã, chưa có bằng chứng lúc chạy trên máy vận hành.
2. **Không** nâng `PUSHED` thành `DEPLOYED` hay `LIVE_VERIFIED`.
3. Việc cập nhật sổ nợ và Sổ Yêu Cầu Chủ dự án **em cố ý không làm**, vì hai tệp đó đang do phiên song song viết dở — sửa vào sẽ đè mất việc của họ.

---

## 9. MA TRẬN HOÀN THÀNH

| # | Hạng mục | Trạng thái |
|---|---|---|
| 1 | Tái xác minh trạng thái hiện tại | ✅ `MEASURED_ONLY` |
| 2 | Chọn đúng một mục tiêu | ✅ `DEBT-174` |
| 3 | Tái hiện lỗi trước khi sửa | ✅ `MEASURED_ONLY` — đo cấu trúc thật, không đoán |
| 4 | Bản vá nhỏ nhất, mạch lạc | ✅ `CODE_IMPLEMENTED` — 7 tệp, +506/−5 |
| 5 | Kiểm thử có đối chứng âm | ✅ `TEST_VERIFIED` — 21/21, kiểm ngược 3 lần |
| 6 | Soát an ninh phần khác biệt | ✅ `TEST_VERIFIED` — 0 lỗ hổng đạt ngưỡng |
| 7 | Số phiên bản và nhật ký | ✅ `CODE_IMPLEMENTED` — V1.00.369 |
| 8 | Commit và đẩy kho riêng | ✅ `PUSHED` |
| 9 | Triển khai | ⛔ **`DEPLOY_BLOCKED`** — cây làm việc chưa sạch vì phiên song song |
| 10 | Kiểm khói máy vận hành | ⛔ `NOT_STARTED` — phụ thuộc mục 9 |
| 11 | Đối chiếu ba nơi | 🟡 `PARTIAL` — cục bộ = kho gốc **ĐẠT**; máy vận hành **CHƯA ĐO** |
| 12 | Báo cáo công khai | ✅ tệp này |

---

## 10. KHOÁ CHỐT · VIỆC MỞ · BƯỚC KẾ TIẾP

### 🔒 KHOÁ CHỐT
1. Bẫy tự khoá ở màn Phân Quyền **đã được vá ở mức mã nguồn** và đã đẩy lên kho.
2. Bản vá **không đụng** cơ sở dữ liệu, di trú hay lớp lưu trữ ⇒ tương thích lược đồ **thoả mãn hiển nhiên**.
3. Cổng mới **có kiểm ngược thật**, đã chứng minh bắt được lỗi ba lần.
4. **Việc của phiên song song còn nguyên vẹn** — không lệnh Git phá huỷ nào được dùng.
5. **Máy vận hành chưa bị chạm.** Đường lui sạch: hoàn tác một commit.

### ❓ VIỆC MỞ
1. **Triển khai** — chờ phiên song song commit xong để cây làm việc sạch.
2. **Kiểm khói trên máy vận hành** — phụ thuộc mục 1.
3. **Đường đầu-cuối** — cần cơ sở dữ liệu; hiện bị chặn an toàn.
4. **Cập nhật sổ nợ và Sổ Yêu Cầu Chủ dự án** — cố ý chưa làm để không đè việc phiên khác.

### ➡️ BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
> **Chờ phiên song song commit hai tệp sổ, rồi chạy lại cổng chặn triển khai và đưa V1.00.369 lên máy vận hành kèm kiểm khói.**

---

_Bản vá đã đẩy, chưa triển khai. `PUSHED` không phải `DEPLOYED`. Máy vận hành chưa bị chạm._
