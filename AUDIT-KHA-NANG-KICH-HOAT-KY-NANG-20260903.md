# AUDIT — KHẢ NĂNG VẬN DỤNG · CÁCH VẬN HÀNH · TỰ KÍCH HOẠT · TƯƠNG THÍCH CỦA THƯ VIỆN KỸ NĂNG

> **Mã audit:** `AUDIT-ERP-SKILL-TRIGGER-COMPAT-001`
> **Ngày:** 03/09/2026
> **Tầng thông tin:** LOCAL — đọc tệp trên đĩa · siêu dữ liệu · cấu hình đang hoạt động · cổng kiểm chỉ-đọc
> **Loại:** Audit CHỈ ĐỌC. **KHÔNG** sửa kỹ năng · **KHÔNG** sửa sổ đăng ký · **KHÔNG** sửa luật · **KHÔNG** sửa mã nguồn · **KHÔNG** đụng cơ sở dữ liệu · **KHÔNG** triển khai · **KHÔNG** đổi số phiên bản · **KHÔNG** bật chế độ tự kích hoạt
> **Phán quyết sử dụng:** ⛔ `AUTO_TRIGGER_NOT_APPROVED` — giữ nguyên trạng thái khoá

```
══════════════════════════════════════
  CHỮ KÝ AGENT — KHÔNG GIẢ MẠO
══════════════════════════════════════
  Vai trò     : Agent IDE
  Công cụ     : Claude Code (tiện ích mở rộng chạy TRONG Cursor)
  Lane        : execution / IDE
  Actor       : Agent IDE — KHÔNG phải Agent Notion, KHÔNG phải Coordinator
  Phạm vi     : audit hệ thống kỹ năng · một báo cáo công khai đã lọc an toàn
  Quyền dùng  : READ_ONLY_LOCAL_CODE_GIT_CONFIG_AND_PUBLIC_SAFE_REPORT
══════════════════════════════════════
```

**PHIẾU KỸ NĂNG CHO CHÍNH LƯỢT AUDIT NÀY** — `name=NONE` · `mode=NO_SKILL_REQUIRED` · `source=NONE` · `version/hash=NOT_APPLICABLE` · `trigger=AUDIT_THE_SKILL_SYSTEM_ITSELF` · `negative_check=PASS` · `risk=R0_READ_ONLY` · `permissions=UNCHANGED`.
**Lý do:** lượt này audit chính hệ thống kỹ năng. Dùng một kỹ năng chưa được kiểm sẽ làm nhiễu phép đo.

---

## 1. TÓM TẮT ĐIỀU HÀNH

Đã đo xong. Kết luận gọn trong bốn câu:

**Thứ nhất — Claude Code KHÔNG hề thấy thư viện kỹ năng của dự án.** Đây là phát hiện quan trọng nhất và đã **chứng minh được**, không phải suy đoán. Toàn bộ 128 kỹ năng nằm trong thư mục kỹ năng của Cursor. Thư mục kỹ năng dành cho Claude Code **không tồn tại** — không ở dự án, không ở mức người dùng, không có liên kết tượng trưng hay điểm nối nào bắc cầu giữa hai bên, và không cấu hình nào khai thêm đường dẫn kỹ năng. Bằng chứng quyết định: chính phiên audit này được cấp **17 kỹ năng**, tất cả đều là kỹ năng dựng sẵn của công cụ, và **không một kỹ năng nào trong 128 kỹ năng ERP xuất hiện trong danh sách đó**. Hệ quả: trên Claude Code, tự kích hoạt kỹ năng ERP là điều **không thể xảy ra** — không phải "chưa bật", mà là **không có đường dẫn phát hiện**. Đường duy nhất là con người/agent mở tệp ra đọc thủ công.

**Thứ hai — không một kỹ năng nào đủ điều kiện `AUTO_SAFE`, và điều này chốt ngay ở tiêu chí ĐẦU TIÊN.** Chuẩn `AUTO_SAFE` mà chính chỉ thị audit đặt ra đòi hỏi *"bằng chứng registry ACTIVE"*. Đo được: sổ đăng ký hiện có **127 kỹ năng nhãn `UNREVIEWED`** và **1 kỹ năng nhãn `DORMANT`** — tức **0 kỹ năng nhãn `ACTIVE`**. Vậy 128/128 trượt tiêu chí thứ nhất trước khi cần xét tới bảy tiêu chí còn lại. Đây là kết quả **sạch**: khoá hiện hành đang đúng, và nó đúng vì nội dung chưa được soát, chứ không vì thiếu công cụ.

**Thứ ba — nhãn trong sổ đăng ký KHÔNG có đường thi hành tự động.** Không có bất kỳ móc (hook) nào ở cả hai công cụ. Những nơi duy nhất đọc sổ đăng ký kỹ năng là **năm tệp cổng kiểm**, và các cổng đó chỉ chạy khi có người gõ lệnh. Móc trước-khi-commit của Git thì **có thật và đang khớp bản nguồn**, nhưng nó chỉ chạy **bốn** cổng — và **không cổng nào trong ba cổng kỹ năng** nằm trong số đó. Nghĩa là: nhãn `DORMANT`/`UNREVIEWED` là **quy ước văn bản** do Agent tự giác tuân, **không phải rào máy**. Ai muốn dùng nhãn này làm lớp bảo vệ thì cần biết rõ nó không chặn được gì.

**Thứ tư — rủi ro thật không nằm ở tệp thi hành, mà nằm ở NỘI DUNG DẠY SAI và ở CHỒNG CHÉO.** Điểm sáng: **0/128 kỹ năng có tệp thi hành** (không có tệp lệnh, tệp kịch bản hay tệp chương trình nào kèm theo) ⇒ **không có mặt tấn công dạng chạy mã**. Nhưng: **36 kỹ năng chọi trực tiếp với chuẩn giao diện của dự án**, trong đó **12 kỹ năng còn chứa chuỗi mà chuẩn CẤM** — kỹ năng nặng nhất mang **điểm chọi 10** kèm **2 chuỗi cấm**. Và **100/128 kỹ năng có phần mô tả không nêu ĐIỀU KIỆN DÙNG** — mà mô tả chính là thứ duy nhất công cụ đưa cho mô hình khi quyết định có nạp kỹ năng hay không. Cộng thêm **0/128 khai phạm vi đường dẫn** và bố cục **phẳng một tầng**, kết quả là: mọi kỹ năng đều là ứng viên cho mọi yêu cầu trong kho.

> ⚠️ **MỘT PHẦN PHÉP ĐO KHÔNG CHẠY ĐƯỢC — ghi trung thực.** Phần thử kích hoạt động (7 dạng câu hỏi × nhiều lượt lặp trên các ứng viên đã chọn) **KHÔNG THỰC HIỆN ĐƯỢC**: 51 tiến trình phụ trong 2 lần chạy đều bị nhà cung cấp trả lỗi quá tải, **0 lượt thành công, 0 token tiêu thụ**. Đây là lỗi hạ tầng phía nhà cung cấp, không phải kết quả đo. Vì vậy phía **Cursor**, khả năng tự kích hoạt bản địa được ghi **`NOT_PROVEN`** — xem mục 8.

---

## 2. BẢNG MÔI TRƯỜNG · PHIÊN BẢN (Pha 0)

| Hạng mục | Giá trị đo được | Trạng thái |
|---|---|---|
| Hệ điều hành | Windows 11 Pro, bản dựng 10.0.26200 | ✅ VERIFIED |
| Nền chạy | Node phiên bản 24 · npm phiên bản 11 | ✅ VERIFIED |
| **Cursor** | **3.18.25**, kiến trúc x64 | ✅ VERIFIED |
| **Claude Code** | **2.1.259** (bản 2.1.258 còn trên đĩa, bản đang bật là 2.1.259) | ✅ VERIFIED |
| Cách Claude Code chạy | **Tiện ích mở rộng nằm TRONG Cursor** — không phải tiến trình dòng lệnh độc lập | ✅ VERIFIED |
| Lệnh `claude` trên biến môi trường đường dẫn | Không có | ✅ VERIFIED (không phải lỗi) |
| Nhánh · nhánh theo dõi | `main` · theo dõi nhánh `main` của kho gốc | ✅ VERIFIED |
| Cây làm việc | **Sạch** ở thời điểm chốt (không tệp nào đang sửa dở) | ✅ VERIFIED |
| **Điểm chốt mã nguồn ĐÃ DỊCH trong lúc audit** | Đầu nhánh chuyển sang một commit mới hơn do **một phiên song song** ghi vào cùng cây làm việc | ⚠️ Xem mục 11-G |
| Năm bản luật quản trị | **Trùng khớp tuyệt đối — một giá trị băm duy nhất cho cả năm bản** (đo lại lần hai sau khi đầu nhánh dịch: vẫn trùng) | ✅ VERIFIED |
| Phiên bản tài liệu luật | `2.9` | ✅ VERIFIED |
| Phiên bản mã nguồn đo được | **V1.00.367** (đọc trực tiếp từ tệp phiên bản) | ✅ VERIFIED |
| Phiên bản trong sổ đăng ký phát hành | Còn ghi mốc 25/08 — **trễ so với mã nguồn** | ⚠️ `SYNC_OVERDUE`, **không** phải lỗi (§F1b) |
| Phiên bản đang chạy trên máy vận hành | ⛔ **UNVERIFIED** — lượt audit này **không được cấp** kênh đo môi trường vận hành | ✅ Ghi đúng giới hạn |
| Kho báo cáo công khai | Đầu nhánh **đúng bằng điểm chốt mà TanPhatAI đã biết**, cây làm việc sạch | ✅ VERIFIED |

**Bề mặt cấu hình ảnh hưởng tới kỹ năng · luật · móc · quyền · tiến trình phụ · lệnh tắt:**

| Bề mặt | Kết quả | Ghi chú |
|---|---|---|
| Cấu hình Claude Code mức người dùng | ✅ VERIFIED — có mục **quyền cho phép**, mô hình, mức nỗ lực. **Không có khoá `hooks`** | Chỉ báo dạng mẫu, không trích nội dung |
| Cấu hình Claude Code mức dự án | ✅ VERIFIED — có mục **quyền cho phép**. **Không có khoá `hooks`** | Chỉ báo dạng mẫu, không trích nội dung |
| Thư mục kỹ năng Claude Code — mức dự án | ✅ VERIFIED · **NOT_FOUND** | Then chốt cho câu F |
| Thư mục kỹ năng Claude Code — mức người dùng | ✅ VERIFIED · **NOT_FOUND** | Then chốt cho câu F |
| Thư mục tiện ích bổ trợ / chợ tiện ích | ✅ VERIFIED · **NOT_FOUND** | Không có tiện ích bổ trợ nào |
| Thư mục tiến trình phụ / lệnh tắt mức người dùng | ✅ VERIFIED · **NOT_FOUND** | — |
| Thư mục kỹ năng / luật của Cursor mức người dùng | ✅ VERIFIED · **NOT_FOUND** | Không có kỹ năng cá nhân |
| Thư mục kỹ năng Cursor mức dự án | ✅ VERIFIED — **128 thư mục kỹ năng** | Toàn bộ thư viện nằm ở đây |
| Luật riêng của Cursor | ✅ VERIFIED — **2 tệp**, **cả hai đều khai KHÔNG tự áp** | Không tệp nào tự tiêm vào phiên |
| Tệp bỏ qua đánh chỉ mục của Cursor | ✅ VERIFIED · **NOT_FOUND** | Không có cơ chế loại trừ |
| Móc của Git trước khi commit | ✅ VERIFIED — **có thật, đang khớp bản nguồn** (đã chạy lệnh đối chiếu) | Chỉ chạy **4** cổng — xem câu H |

---

## 3. MA TRẬN ĐƯỜNG DẪN PHÁT HIỆN · THỨ TỰ ƯU TIÊN THEO CÔNG CỤ (Pha 1)

| Đường dẫn phát hiện | Có thật? | Số kỹ năng | Cursor thấy? | Claude Code thấy? |
|---|---|---|---|---|
| Thư mục kỹ năng **Cursor** — mức dự án | ✅ CÓ | **128** | `NOT_PROVEN` (xem mục 8) | ⛔ **PROVEN_FALSE — KHÔNG thấy** |
| Thư mục kỹ năng **Claude Code** — mức dự án | ❌ KHÔNG | 0 | — | — |
| Thư mục kỹ năng **Claude Code** — mức người dùng | ❌ KHÔNG | 0 | — | — |
| Thư mục kỹ năng của các công cụ khác | ❌ KHÔNG | 0 | — | — |
| Thư mục kỹ năng lồng theo mô-đun | ❌ KHÔNG | 0 | — | — |
| Kỹ năng từ tiện ích bổ trợ / chợ tiện ích | ❌ KHÔNG | 0 | — | — |
| Kỹ năng **dựng sẵn của Claude Code** | ✅ CÓ | **17** (đo trong chính phiên này) | Không áp dụng | ✅ **THẤY — và CHỈ thấy nhóm này** |

**Thứ tự ưu tiên:** hiện **KHÔNG có xung đột ưu tiên nào để phân xử**, vì hai công cụ đang đọc **hai tập rời nhau**: Cursor đọc tập 128 kỹ năng dự án, Claude Code đọc tập 17 kỹ năng dựng sẵn. Hai tập **không giao nhau ở bất kỳ tên nào**. Đây vừa là tin tốt (không đụng độ) vừa là vấn đề thật (dùng chéo công cụ **không** thừa hưởng được thư viện).

**Đặc điểm bố cục:** thư viện **phẳng một tầng** — chỉ 2 thư mục con tồn tại (đều mang tên "khuôn mẫu") và **cả hai đều RỖNG**. Toàn thư viện có **đúng 1 tệp** không phải tệp kỹ năng (một danh mục kiểm kèm kỹ năng triển khai).

---

## 4. KIỂM KÊ TĨNH 128 KỸ NĂNG (Pha 1 + Pha 3)

### 4.1 Trường siêu dữ liệu — điểm danh toàn bộ

| Trường trong phần đầu tệp | Số kỹ năng có | Ý nghĩa cho việc tự kích hoạt |
|---|---|---|
| `name` | **128/128** | Đủ |
| `description` | **128/128** | Đủ về hình thức — nhưng xem 4.3 về CHẤT |
| `version` | 85/128 | 43 kỹ năng không khai phiên bản |
| `triggers` | **19/128** | Chỉ 15% khai điều kiện kích hoạt tường minh |
| `metadata` | 10/128 | — |
| `compatibility` | 10/128 | — |
| `superseded_by` | 3/128 | Xem 4.5 |
| `status` | 3/128 | — |
| **Trường điều khiển của công cụ** — khoá tự-gọi-bởi-mô-hình, cho-người-gọi, danh sách công cụ được/không được dùng, phạm vi đường dẫn, mẫu đường dẫn, ngữ cảnh, tách nhánh, chế độ tuỳ chỉnh, giấy phép, mô hình, gợi ý tham số | **0/128 — cho TỪNG trường, không trường nào có** | ⛔ **Không có bất kỳ van điều khiển nào ở mức tệp** |

> Đây là số đo nặng nhất của mục này: **không một kỹ năng nào trong 128 kỹ năng khai bất kỳ trường điều khiển nào** mà công cụ có thể dùng để giới hạn phạm vi, giới hạn công cụ, hay tắt việc tự gọi. Mọi giới hạn hiện nay đều nằm **ngoài tệp** — trong sổ đăng ký (không có đường thi hành) và trong sự tự giác của Agent.

### 4.2 Phân loại rủi ro theo **điều tệp DẠY AGENT LÀM**, không theo tên chủ đề

Phép đo dùng bộ dấu hiệu xác định (họ dấu hiệu · số lần khớp · vị trí), lấy **mức cao nhất** mà chỉ dẫn trong tệp chạm tới.

| Lớp rủi ro | Số kỹ năng | Định nghĩa |
|---|---|---|
| **R3_RELEASE** | **29** | Dạy commit · đẩy mã · gắn thẻ · tăng số phiên bản · tạo phiếu/yêu cầu hợp nhất · gộp nhánh · triển khai · tác động máy vận hành |
| **R2_DATA_OR_AUTH** | **18** | Dạy ghi cơ sở dữ liệu · đổi lược đồ · di trú · nạp bù · đổi quyền hoặc xác thực · xử lý thông tin nhạy cảm · cài đặt gói · gọi mạng |
| **R1_CODE** | **66** | Dạy sửa mã nguồn · thành phần giao diện · kiểu dáng · cấu hình trong kho, hoặc ghi vào tài liệu/sổ |
| **R0_READ_ONLY** | **15** | Chỉ soi · báo cáo · tư vấn — không chỉ dẫn nào làm đổi tệp hay trạng thái |

**29 kỹ năng lớp R3** là nhóm cần khoá chặt nhất. Trong đó nặng nhất là **kỹ năng triển khai** — một mình nó khớp **91 lần** dấu hiệu triển khai/máy vận hành cộng với dấu hiệu đẩy mã; kế đó là nhóm kỹ năng an toàn lược đồ, kỹ năng vận hành cơ sở dữ liệu, và **kỹ năng tạo phiếu công việc từ đặc tả** (khớp 4 lần dấu hiệu tạo phiếu/yêu cầu hợp nhất).

**Theo đúng chính sách của chỉ thị audit** — mọi kỹ năng chứa chỉ dẫn cài đặt · cập nhật · truyền mạng · xử lý bí mật · ghi dữ liệu bền · thao tác Git · tạo phiếu hoặc yêu cầu hợp nhất · gộp nhánh · commit · đẩy mã · triển khai hoặc phát hành thì **bắt buộc `EXPLICIT_ONLY` hoặc `BLOCKED`** — kết quả gán chế độ kích hoạt:

| Chế độ kích hoạt đề xuất | Số kỹ năng | Cơ sở |
|---|---|---|
| `AUTO_SAFE_CANDIDATE` | **0** | Trượt ở tiêu chí "bằng chứng registry ACTIVE" — xem mục 9 |
| `PROMPT_REQUESTED_CANDIDATE` | tối đa **66** (nhóm R1, và **chỉ sau khi** soát nội dung) | R1 không được vượt quá mức này |
| `EXPLICIT_ONLY` | **47** (29 lớp R3 + 18 lớp R2) | Bắt buộc theo chính sách |
| Cần soát nội dung trước khi xếp | **15** (nhóm R0) | Xem 4.4 |

### 4.3 Chất lượng phần mô tả — thứ QUYẾT ĐỊNH việc tự kích hoạt

Công cụ chỉ đưa cho mô hình **tên + mô tả** khi quyết định có nạp kỹ năng hay không. Vì vậy chất của phần mô tả **chính là** năng lực tự kích hoạt.

| Phép đo | Kết quả | Hệ quả |
|---|---|---|
| Mô tả **không nêu điều kiện dùng** (không có mệnh đề "khi/trước khi/sau khi/dùng khi/nếu") | **100/128 — 78%** | Mô hình **không có căn cứ** để biết khi nào kỹ năng áp dụng ⇒ hoặc bỏ sót, hoặc nạp bừa |
| Mô tả **ngắn dưới 80 ký tự** | **33/128** | Quá ít thông tin để phân biệt với kỹ năng lân cận |
| Có khai `triggers` tường minh | **19/128** | 109 kỹ năng dựa hoàn toàn vào mô tả |
| Có khai **điều kiện KHÔNG kích hoạt** trong sổ đăng ký | **32/128** (96 kỹ năng còn thiếu) | Bước "NEGATIVE TRIGGER" của quy trình chọn kỹ năng **chưa thi hành được đầy đủ** — đây là **nợ đã biết** |
| Có khai **triệu chứng** trong sổ đăng ký | **0/128** | Bước "tra theo triệu chứng" mà luật đòi **chưa có dữ liệu để tra** — **nợ đã biết** |

> **Đọc số này cho đúng:** đây **không** phải kết luận "thư viện kém". Thư viện được viết để **người/Agent tra rồi đọc**, và cho mục đích đó nó đang phục vụ tốt. Nhưng để **tự kích hoạt bản địa**, phần mô tả phải làm một việc khác hẳn: nói rõ **KHI NÀO**. Hiện 78% chưa làm việc đó.

### 4.4 Nhóm R0 — 15 kỹ năng không có dấu hiệu làm đổi trạng thái

Ba ứng viên **tĩnh** an toàn nhất (đã loại các trường hợp vướng nhãn):

| Kỹ năng | Mô tả | Nhãn nội dung | Nhãn đối chiếu chuẩn giao diện | Có `triggers`? |
|---|---|---|---|---|
| Ổn định bộ nhớ đệm khi phát triển trên Windows | 157 ký tự, **CÓ** mệnh đề điều kiện | `UNREVIEWED` | `KHONG_UI` | Không |
| Chẩn đoán nhanh môi trường phát triển Windows | 129 ký tự, **CÓ** mệnh đề điều kiện | `UNREVIEWED` | `KHONG_UI` | Không |
| Trực quan hoá lược đồ dữ liệu | 169 ký tự | `UNREVIEWED` | `KHONG_UI` | Không |

Các kỹ năng R0 còn lại **có vướng**, nêu đích danh lý do:
- **4 kỹ năng thuộc bộ công cụ đặc tả của bên thứ ba** — chưa hoàn tất quy trình cách ly bên thứ ba (xem 4.6)
- **1 kỹ năng** mang nhãn **`LOI_THOI`** (đã lỗi thời so với chuẩn) ⇒ **BLOCKED**
- **1 kỹ năng** mang nhãn **`SSOT_THANG`** (chọi chuẩn) ⇒ **BLOCKED** cho tự kích hoạt
- **2 kỹ năng** mang nhãn **`DA_GOP`** (nội dung đúng đã được rút sang chuẩn) ⇒ nạp lại có nguy cơ dạy bản cũ

### 4.5 Kỹ năng ĐÃ BỊ THAY THẾ mà vẫn phát hiện được đầy đủ

**3 kỹ năng** khai tường minh trong phần đầu tệp rằng chúng **đã bị thay thế** bởi một kỹ năng khác, đồng thời khai `status` là "tham chiếu hỗ trợ":

| Kỹ năng bị thay | Thay bởi | Nhãn nội dung | Còn phát hiện được? |
|---|---|---|---|
| Cập nhật sổ công việc | Kỹ năng ghi nhật ký phiên bản tự động | `UNREVIEWED` | ✅ **CÓ — đầy đủ** |
| Tăng số phiên bản khi có tính năng mới | Kỹ năng ghi nhật ký phiên bản tự động | `UNREVIEWED` | ✅ **CÓ — đầy đủ** |
| Lịch sử thay đổi phiên bản | Kỹ năng ghi nhật ký phiên bản tự động | `UNREVIEWED` | ✅ **CÓ — đầy đủ** |

> **Không có cơ chế nào ngăn công cụ nạp một kỹ năng đã bị thay thế.** Cả bốn kỹ năng trong chùm này (3 bản cũ + 1 bản thay) đều thuộc lớp **R3_RELEASE** và đều nằm cùng một chỗ, mô tả gần nhau ⇒ đây là **chùm chồng chéo rủi ro cao**, nêu ở mục 6.

### 4.6 Khoảng trống nguồn gốc — 10 kỹ năng của bên thứ ba

**10 kỹ năng** thuộc bộ công cụ đặc tả của bên thứ ba **cố ý không được Git theo dõi** (quyết định của Chủ dự án ngày 20/08/2026, ghi ngay trong tệp cấu hình bỏ qua của Git, liệt kê **đích danh** 10 kỹ năng thay vì dùng mẫu quét bừa — cách làm này **đúng** và đã sửa một vết cũ).

Nhưng đứng ở góc audit kích hoạt, hệ quả là:

| Phép đo trên 10 kỹ năng đó | Kết quả |
|---|---|
| Được Git theo dõi | ❌ **0/10** |
| Khai `version` trong tệp | ❌ **0/10 — cả mười đều KHÔNG khai** |
| Khai mã nguồn gốc từ bên thứ ba hoặc giấy phép | ❌ Không kỹ năng nào |
| ⇒ Chứng minh được nguồn gốc và bản gốc | ⛔ **KHÔNG** |

**Kết luận:** không thể cấp `AUTO_SAFE` cho 10 kỹ năng này vì tiêu chí *"nguồn và mã băm nội dung chính xác"* **không đạt được** — không có mã băm trong Git để đối chiếu, không có số hiệu bản gốc để tra. Quy trình cách ly kỹ năng bên thứ ba (§L7) mới đi tới bước quét, **chưa** tới bước thử trong bóng tối và phê duyệt.

### 4.7 Tham chiếu chết và tài sản hứa mà không có

- **22 kỹ năng** trỏ tới ít nhất một đường dẫn không tồn tại. **Cần đọc con số này cho đúng:** phần lớn là **chỗ trống trong khuôn mẫu** có chủ đích (kiểu "mô-đun-X", "tên-tệp", "ngày-tháng") — **không** phải lỗi. Nhưng phép kiểm đích danh **8 đường dẫn** trông như thật thì **cả 8 đều CHẾT** (ví dụ: tệp bảng màu mô-đun · tệp trợ giúp luồng công việc · một thư mục các bước của trình hướng dẫn · hai tệp kịch bản môi trường/khôi phục · một tệp cấu hình · một tệp hướng dẫn nhanh). Có kỹ năng còn trỏ vào tệp thuộc **ngôn ngữ khác hẳn** dự án này.
- **2 thư mục "khuôn mẫu" RỖNG** thuộc hai kỹ năng khác nhau — kỹ năng hứa có khuôn mẫu, thực tế không có gì.

### 4.8 Ngôn ngữ tự nhận quyền

**6 kỹ năng** có câu chữ mang tính **ghi đè / cao hơn luật**. Theo §L1, **kỹ năng KHÔNG phải authority** — nên đây là các điểm cần soát câu chữ. Đáng chú ý: một trong sáu chính là kỹ năng đã bị đưa về trạng thái `DORMANT`, và một là kỹ năng triển khai lớp R3.

**Không** có trùng tên: 128 giá trị `name` **đều khác nhau**, và **không** giá trị nào đụng tên với 17 kỹ năng dựng sẵn.

---

## 5. ĐỐI CHỨNG SỔ ĐĂNG KÝ ↔ KHẢ NĂNG GỌI BẢN ĐỊA (Pha 2)

### 5.1 Ma trận đối chứng

| Nhãn trong sổ đăng ký | Phần đầu tệp kỹ năng | Công cụ phát hiện ra | Gọi bản địa được? | Trạng thái quyền | Rủi ro thật | Phán quyết |
|---|---|---|---|---|---|---|
| `UNREVIEWED` × **127** | **0** trường điều khiển | Cursor: `NOT_PROVEN` · Claude Code: **KHÔNG** | Trên Claude Code: **KHÔNG THỂ** | Không móc chặn; chỉ có danh sách quyền + móc trước-commit | **Nội dung chưa soát** có thể dạy sai | ⛔ Không đủ cho `AUTO_SAFE` |
| `DORMANT` × **1** | **0** dấu hiệu máy đọc được — nhãn chỉ nằm trong sổ, **không** nằm trong tệp | Như trên | Như trên | Như trên | Nội dung đã hết hiệu lực từ 30/07 | ⛔ `BLOCKED` |
| `ACTIVE` × **0** | — | — | — | — | — | ⛔ **Không có ứng viên nào** |

### 5.2 Chứng minh / bác bỏ tám câu hỏi

**A. Nhãn hiệu lực nội dung có ảnh hưởng tới việc phát hiện kỹ năng bản địa khi KHÔNG có bộ chuyển đổi?**
→ ⛔ **PROVEN_FALSE — KHÔNG ảnh hưởng.** Bằng chứng: (1) quét toàn kho, những nơi **duy nhất** đọc sổ đăng ký kỹ năng và tệp ghi đè hiệu lực là **năm tệp cổng kiểm** trong thư mục kịch bản kiểm thử; (2) các cổng đó chỉ chạy khi có người gõ lệnh; (3) **không** khoá `hooks` trong cả hai tệp cấu hình; (4) móc trước-commit chỉ gọi **4** cổng, **không** cổng kỹ năng nào. ⇒ Nhãn là **quy ước văn bản**, đường thi hành duy nhất là sự tự giác của Agent cộng với việc Chủ dự án kiểm. **Độ tin: CAO.**

**B. Kỹ năng `UNREVIEWED`/`DORMANT` có còn được đưa ra cho mô hình?**
→ **Chia hai phía.** Phía **Claude Code**: **KHÔNG** — không kỹ năng ERP nào được đưa ra, bất kể nhãn gì (xem F). Phía **Cursor**: `NOT_PROVEN` về mặt quan sát, **nhưng chứng minh được rằng KHÔNG CÓ CƠ CHẾ CHẶN NÀO**: 0/128 có trường tắt-tự-gọi; kỹ năng `DORMANT` **tự tệp nó không mang dấu hiệu nào** — đã đọc phần đầu tệp, chỉ có tên, phiên bản, mô tả và **4 điều kiện kích hoạt còn nguyên**, không hề có cờ tắt; không có tệp bỏ qua đánh chỉ mục. ⇒ Nếu Cursor phát hiện thư mục này thì kỹ năng `DORMANT` được đưa ra **y như mọi kỹ năng khác**, và nó còn **mang sẵn 4 điều kiện kích hoạt**. **Độ tin: CAO cho phần "không có cơ chế chặn".**

**C. Khoá tắt-tự-gọi-bởi-mô-hình có hoạt động trên các phiên bản công cụ đang cài, và có đang được dùng?**
→ **Tách hai nửa, trả lời trung thực từng nửa.**
- Nửa **đang dùng**: ⛔ **PROVEN — 0/128**. Không kỹ năng nào khai khoá này, cũng không khai cho-người-gọi hay danh sách công cụ được/không được dùng.
- Nửa **công cụ có hỗ trợ hay không**: ⚠️ **`NOT_PROVEN`** — không có cách quan sát nội bộ công cụ từ máy này, và **trí nhớ về tài liệu nhà cung cấp KHÔNG phải bằng chứng trên bản đã cài**.
⇒ Kết luận dùng được: khoá này **hiện không liên quan** tới kho vì chưa ai dùng; và **chưa được phép** coi nó là lớp bảo vệ cho tới khi (a) khai vào tệp **và** (b) chứng minh trên chính bản đang cài.

**D. Giới hạn theo đường dẫn / lồng thư mục có hoạt động như tài liệu nói?**
→ ⛔ **PROVEN_FALSE ở cấu hình hiện tại.** Bằng chứng: **0/128** khai phạm vi đường dẫn hay mẫu đường dẫn; bố cục **phẳng một tầng** (chỉ 2 thư mục con, đều rỗng, đều tên "khuôn mẫu"). ⇒ Giới hạn theo đường dẫn **thậm chí không diễn đạt được** với bố cục hiện nay. **Hệ quả trực tiếp: mọi kỹ năng là ứng viên cho mọi yêu cầu trong kho** — không có cách thu hẹp theo mô-đun. **Độ tin: CAO.**

**E. Kỹ năng cá nhân / tiện ích bổ trợ / dựng sẵn có thể che hoặc đụng độ kỹ năng dự án?**
→ **Rủi ro thực tế hiện nay: THẤP. Rủi ro tiềm năng: có thật.** Bằng chứng: thư mục kỹ năng mức người dùng của cả hai công cụ **NOT_FOUND**; thư mục tiện ích bổ trợ **NOT_FOUND**; không cấu hình chợ tiện ích; 17 kỹ năng dựng sẵn **không tên nào** đụng 128 tên dự án; 128 giá trị `name` **không trùng nhau**. ⇒ Hôm nay **không có** đụng độ tên. Nhưng ngày nào có ai tạo thư mục kỹ năng mức người dùng thì rủi ro thành thật ngay, **và không có cổng nào canh việc đó**. **Độ tin: CAO cho hiện trạng.**

**F. Claude Code có thấy thư mục kỹ năng của Cursor? — KHÔNG ĐƯỢC PHỎNG ĐOÁN.**
→ ⛔ **PROVEN_FALSE — KHÔNG THẤY.** Bốn lớp bằng chứng, lớp thứ ba là lớp quyết định:
1. Thư mục kỹ năng của Claude Code **NOT_FOUND** ở **cả** mức dự án **và** mức người dùng.
2. **Không** liên kết tượng trưng, **không** điểm nối Windows nào bắc từ vùng cấu hình Claude Code sang thư mục kỹ năng Cursor (đã chạy cả lệnh liệt kê thuộc tính, lệnh dò liên kết trong Git, và lệnh liệt kê điểm nối của hệ điều hành — cả ba đều rỗng).
3. **BẰNG CHỨNG QUYẾT ĐỊNH — quan sát trực tiếp trên bản đã cài:** chính phiên audit này được cấp **17 kỹ năng**, và **không một tên nào** trong 128 kỹ năng ERP có mặt. Đã soi đích danh nhiều kỹ năng tiêu biểu — khuôn trang danh sách, bảng dữ liệu danh sách, kiểu bảng cao cấp, bố cục panel chi tiết, kỹ năng triển khai, cổng lọc an toàn báo cáo công khai, kỹ năng chữ nghĩa giao diện, kỹ năng dựng mô-đun, kỹ năng vận hành cơ sở dữ liệu, kỹ năng an toàn di trú lược đồ — **tất cả đều VẮNG MẶT**.
4. **Không** cấu hình nào ở cả hai tệp khai thêm đường dẫn kỹ năng.
⇒ **Trên Claude Code 2.1.259, tự kích hoạt kỹ năng ERP là điều KHÔNG THỂ XẢY RA.** Đường duy nhất là mở tệp đọc thủ công. **Độ tin: CAO.**

**G. Cursor có thấy thư mục kỹ năng dành cho Claude Code, và với thứ tự ưu tiên nào?**
→ **Câu hỏi VÔ HIỆU ở phần đầu, `NOT_PROVEN` ở phần sau.** Thư mục kỹ năng dành cho Claude Code **không tồn tại** ⇒ không có gì để Cursor đọc, nói khác đi là phỏng đoán. Điều **chứng minh được** là Cursor ở kho này có sẵn: **128** thư mục kỹ năng trong vùng của chính nó, và **2** tệp luật riêng mà **cả hai đều khai KHÔNG tự áp** ⇒ **không tệp luật riêng nào tự tiêm vào phiên** (một trong hai đã được hạ cờ có chủ đích ngày 26/08 và nội dung được gộp vào năm bản luật — đo lại hôm nay: cờ vẫn ở trạng thái không-tự-áp, **đúng như đã chốt**).
Về **tự kích hoạt bản địa phía Cursor**: ⚠️ **`NOT_PROVEN`.** Không có kênh quan sát Cursor từ phiên này. **Và kho cũng không có phép đo cũ nào lấp được:** bản dò Cursor ngày 16/08 chỉ ghi được rằng Cursor **tự nạp tệp luật gốc làm tệp vào**, **không** nói gì về việc tự nạp bất kỳ kỹ năng nào trong 128 kỹ năng. ⇒ **Chưa từng có phép đo nào chứng minh Cursor tự kích hoạt kỹ năng ERP.**

**H. Móc có mở-khi-lỗi hay đóng-khi-lỗi, có được nạp ở chế độ chạy này, và có canh được việc cần canh?**
→ **PROVEN — và đây là khoảng trống cần Chủ dự án biết rõ.**
- **Móc mức Agent: KHÔNG CÓ CÁI NÀO.** Không khoá `hooks` trong cấu hình mức người dùng, không có ở mức dự án, không thư mục móc, không móc phía Cursor. ⇒ **Không có cổng chặn trước-khi-gọi-công-cụ.** Không gì ngăn được một mutation do kỹ năng gây ra **ngay lúc nó xảy ra**.
- **Móc Git trước khi commit: CÓ THẬT và ĐANG KHỚP bản nguồn** (đã chạy lệnh đối chiếu, kết quả trùng khớp). Nhưng nó gọi **đúng 4 cổng**: quét bí mật · quét dữ liệu cá nhân · kiểm cú pháp kịch bản · kiểm trùng mã mục sổ. Với 4 cổng này nó **đóng-khi-lỗi** (cổng đỏ ⇒ chặn commit). Với **mọi thứ khác** nó **mở-khi-lỗi**.
- ⛔ **Ba cổng kỹ năng KHÔNG nằm trong móc trước-commit** — chúng chỉ chạy khi có người gõ lệnh. Chính tệp móc **ghi rõ đây là lựa chọn có chủ đích** (để commit không bị chậm). Hệ quả phải nói thẳng: **tính toàn vẹn của sổ đăng ký kỹ năng KHÔNG được canh ở cửa commit.**
- Một chi tiết nữa: hàm chạy cổng in "BỎ QUA" và **trả về thành công** khi kho không khai tên cổng đó — **mở-khi-lỗi có chủ đích** cho tình huống kho tạm, và được bù bằng một điều kiện của cổng đối chiếu kỹ năng khoá không cho gỡ âm thầm. Cách làm này hợp lý, nhưng cần biết là nó **có** một đường mở.
- **Lớp chặn tự động duy nhất còn lại là danh sách quyền cho phép.** Đo được: mức người dùng khoảng **10** mẫu, mức dự án khoảng **19** mẫu (chỉ báo **số lượng và dạng mẫu** — không trích nội dung, vì một số mẫu có chứa tham số kết nối). Các mẫu này cho phép cả những việc **có ghi** (chạy lệnh của kho, gắn thẻ Git, chạy kịch bản, xoá tệp kịch bản tạm) ⇒ **danh sách quyền hiện hành KHÔNG phải rào chắn chỉ-đọc.**

⇒ **Tổng hợp câu H: hàng rào nằm ở CỬA COMMIT, không nằm ở CỬA SỬA TỆP.** Một kỹ năng bị kích hoạt nhầm vẫn có thể sửa tệp trong cây làm việc; nó chỉ bị chặn nếu chạm đúng 4 cổng kia lúc commit.

---

## 6. MA TRẬN RỦI RO — TÁC DỤNG PHỤ · CÔNG CỤ · MẠNG · TỆP THI HÀNH (Pha 3)

### 6.1 Mặt tấn công dạng chạy mã — tin tốt, đã đo

| Phép đo | Kết quả | Ý nghĩa |
|---|---|---|
| Tệp thi hành kèm theo kỹ năng (tệp lệnh vỏ, tệp lệnh Windows, tệp chương trình, tệp kịch bản các loại) | ✅ **0/128 — KHÔNG CÓ TỆP NÀO** | ⇒ **Không có mặt tấn công dạng chạy mã.** Không cần chạy thử tệp nào, và audit này cũng **không chạy** tệp nào |
| Tệp không phải tệp kỹ năng trong toàn thư viện | ✅ **Đúng 1 tệp** (một danh mục kiểm, dạng văn bản) | Bề mặt cực nhỏ |
| Tổng dung lượng thư viện | Khoảng 1,1 MB | Toàn văn bản |

> Đây là lý do audit này chọn **soi tĩnh** thay vì chạy thử: **không có gì để chạy**. Rủi ro nằm trong **chữ dạy Agent làm gì**, không nằm trong tệp thi hành.

### 6.2 Rủi ro theo họ dấu hiệu — số kỹ năng chứa chỉ dẫn tương ứng

| Họ dấu hiệu | Số kỹ năng | Lớp bị đẩy tới | Chế độ bắt buộc |
|---|---|---|---|
| Triển khai · máy vận hành · trình quản lý tiến trình · kết nối máy chủ | **12** | R3 | `EXPLICIT_ONLY` / `BLOCKED` |
| Tăng số phiên bản · phát hành | nhiều kỹ năng, tập trung ở chùm ghi nhật ký phiên bản | R3 | `EXPLICIT_ONLY` |
| Đẩy mã lên kho | **3** | R3 | `EXPLICIT_ONLY` |
| Commit / thêm vào vùng chờ / gắn thẻ Git | **1** trực tiếp | R3 | `EXPLICIT_ONLY` |
| Tạo phiếu công việc hoặc yêu cầu hợp nhất | **1** (khớp 4 lần) | R3 | `EXPLICIT_ONLY` |
| Gộp nhánh · đặt lại · chuyển nền | **0** | — | Tin tốt |
| Ghi cơ sở dữ liệu (thêm · sửa · xoá · đổi cấu trúc · xoá trắng) | **thấy ở nhiều kỹ năng**, đậm nhất ở chùm giao dịch gói và chùm an toàn dữ liệu | R2 | `EXPLICIT_ONLY` |
| Di trú · nạp bù | **10** | R2 | `EXPLICIT_ONLY` |
| Cài đặt / cập nhật gói | **1** | R2 | `EXPLICIT_ONLY` |
| Gọi mạng ra ngoài | **2** | R2 | `EXPLICIT_ONLY` |
| Xử lý thông tin nhạy cảm (nhắc tới mật khẩu · thẻ truy cập · mã băm) | **6** | R2 | `EXPLICIT_ONLY` |
| Quyền · xác thực · vai trò | **thấy** | R2 | `EXPLICIT_ONLY` |
| Ghi tài liệu · sổ · nhật ký thay đổi | **thấy rộng** | R1 | tối đa `PROMPT_REQUESTED` |

### 6.3 Rủi ro **NỘI DUNG DẠY SAI** — nhóm nặng nhất, và là nhóm dễ bị bỏ qua nhất

Nguồn: cổng đối chiếu kỹ năng ↔ chuẩn giao diện, chạy lại hôm nay, **PASS 6/6 điều kiện**.

| Phán quyết đối chiếu chuẩn | Số kỹ năng | Nghĩa |
|---|---|---|
| ⛔ **`SSOT_THANG`** — kỹ năng **chọi** chuẩn | **36** | Chỉ được đọc để hiểu bối cảnh; **CẤM chép giá trị** |
| `DA_GOP` — nội dung đúng đã rút sang chuẩn | 11 | Nạp lại có nguy cơ dạy bản cũ |
| `BO_SUNG` — có quy tắc đúng mà chuẩn chưa phủ | 8 | Nợ đã biết, **hoãn có chủ đích** |
| `LOI_THOI` | 1 | ⛔ `BLOCKED` |
| `TRUNG_KHOP` | 1 | Khớp chuẩn |
| `KHONG_UI` | 71 | Không thuộc phạm vi giao diện |

**Trong 36 kỹ năng chọi chuẩn, 12 kỹ năng còn chứa chuỗi mà chuẩn CẤM** — cổng ghi rõ từng chuỗi cấm kèm số dòng. Xếp theo điểm chọi giảm dần, năm kỹ năng đầu:

| Thứ tự | Kỹ năng | Điểm chọi | Có chuỗi cấm? |
|---|---|---|---|
| 1 | Bảng dữ liệu danh sách | **10** | ✅ **2 chuỗi cấm** |
| 2 | Giao diện chạy kiểm thử | 8 | — |
| 3 | Thi công trải nghiệm biểu mẫu nhóm G2 | 7 | ✅ 1 chuỗi cấm |
| 4 | Bật/tắt panel chi tiết | 6 | ✅ 1 chuỗi cấm |
| 5 | Nhất quán xuyên mô-đun | 5 | ✅ 1 chuỗi cấm |

> ⚠️ **Đây chính là cái bẫy mà luật §L2 đã dựng cổng để chặn, và nó vẫn còn hiệu lực hôm nay:** một phiên **làm đúng luật từng bước** — tra sổ đăng ký, khớp điều kiện kích hoạt, nạp kỹ năng — vẫn có thể bị dẫn tới kỹ năng dạy đúng thứ chuẩn CẤM. Nếu bật tự kích hoạt cho nhóm 36 kỹ năng này thì **càng tuân thủ càng code sai**. Cổng đối chiếu hiện **PASS**, nghĩa là hai sổ đang khớp nhau — nhưng nó **không** ngăn được việc nạp; nó chỉ ngăn hai sổ lệch nhãn.

### 6.4 Chùm chồng chéo — nguy cơ kích hoạt nhầm cao nhất

Vì **0/128** khai phạm vi đường dẫn và bố cục **phẳng**, mọi kỹ năng cạnh tranh nhau trên toàn kho. Ba chùm nặng nhất:

**Chùm 1 — Trang danh sách / bảng · mức độ CAO · khoảng 8 kỹ năng cạnh tranh.** Khuôn trang danh sách · bảng dữ liệu danh sách · kiểu bảng cao cấp · hiện/ẩn cột · luồng cấu hình cột · thiết kế lại trang mô-đun cao cấp · thiết kế lại trang chứng từ · thiết kế lại thành phần bố cục. Một câu hỏi thường gặp như *"làm trang danh sách cho mô-đun mới"* khớp **cả tám**. Và kỹ năng **điểm chọi cao nhất toàn thư viện (10 điểm, 2 chuỗi cấm)** nằm **ngay trong chùm này**.

**Chùm 2 — Danh sách chọn / lớp phủ · mức độ CAO · khoảng 8 kỹ năng cạnh tranh.** Danh sách chọn có tìm kiếm · chọn nhiều có bảng nổi · lớp phủ an toàn qua cổng · chắn lớp phủ · vá cuộn trong bảng nổi · chỉ hiện tên trong danh sách chọn · lọc phạm vi danh sách chọn tham chiếu · ô nhập có gợi ý. Các mô tả **rất gần nhau**, mà **0** kỹ năng nào trong chùm khai điều kiện KHÔNG kích hoạt đủ để phân xử.

**Chùm 3 — Ghi nhật ký phiên bản · mức độ CAO · 4 kỹ năng, trong đó 3 đã bị thay thế · toàn bộ lớp R3.** Bản thay + 3 bản cũ vẫn **phát hiện được đầy đủ**, cùng chỗ, mô tả gần nhau, và **tất cả đều dạy việc phát hành**. Kích hoạt nhầm ở chùm này không chỉ sai cách làm — nó chạm lớp **R3**.

Chùm thứ tư đáng nêu: **lược đồ / di trú / sao lưu** — an toàn di trú lược đồ · an toàn xoá cột · di trú theo pha có nạp bù · chuyển từ bộ nhớ sang cơ sở dữ liệu · sao lưu trước khi thay đổi dữ liệu · vận hành cơ sở dữ liệu. Toàn bộ thuộc **R2/R3**.

---

## 7. BẰNG CHỨNG THỬ KÍCH HOẠT TRONG BÓNG TỐI (Pha 4)

### 7.1 Ứng viên đã chọn — theo đúng yêu cầu "không thử động toàn bộ 128 kỹ năng"

**Ba ứng viên an toàn nhất:** ổn định bộ nhớ đệm khi phát triển trên Windows · chẩn đoán nhanh môi trường phát triển Windows · trực quan hoá lược đồ dữ liệu.
**Ba chùm chồng chéo rủi ro cao nhất:** chùm trang danh sách/bảng · chùm danh sách chọn/lớp phủ · chùm ghi nhật ký phiên bản.
**Một mốc nền không dùng kỹ năng:** một câu hỏi số học thuần.

**Bảy dạng câu hỏi** đã soạn đầy đủ cho từng ứng viên: khớp đúng · khớp diễn đạt khác · gần-mà-không-phải nhưng dùng chung từ khoá · sai đường dẫn/mô-đun · đụng độ với kỹ năng lân cận · mồi thử tác dụng phụ · việc không cần kỹ năng. Mỗi dạng dự kiến **2 lượt lặp** để lộ tính bất định.

### 7.2 Kết quả — phải ghi trung thực

| Phép đo | Trạng thái | Bằng chứng |
|---|---|---|
| **Phía Claude Code — phép đo phát hiện** | ✅ **ĐÃ ĐO — MỘT KẾT QUẢ ÂM DỨT KHOÁT** | Danh sách kỹ năng được cấp cho chính phiên này: **17 kỹ năng, 0 kỹ năng ERP**. Vì **không có phát hiện**, cả bảy dạng câu hỏi đều **không thể** cho kết quả khác `NO_NATIVE_DISCOVERY` — kích hoạt là điều bất khả về mặt cấu trúc, không phải chuyện xác suất |
| **Phía Cursor — phép đo kích hoạt** | ⛔ **NOT_EXECUTED** | Không có kênh đo Cursor từ phiên này |
| **Ma trận 7 dạng × nhiều lượt trên tiến trình phụ** | ⛔ **NOT_EXECUTED** | **51 tiến trình phụ, 2 lần chạy, 100% bị nhà cung cấp trả lỗi quá tải trước khi làm được việc gì — 0 token tiêu thụ, 0 lượt gọi công cụ.** Lỗi hạ tầng phía nhà cung cấp, **không** phải kết quả đo |

> **Vì sao ma trận 7 dạng chỉ có ý nghĩa trên Cursor:** nó đo việc **chọn giữa các kỹ năng cạnh tranh**. Phép đo đó chỉ có nghĩa trên công cụ **thật sự phát hiện** thư viện. Trên Claude Code, tập ứng viên rỗng ⇒ không có gì để chọn. Vì vậy khoảng trống thật là **phía Cursor**, và nó **vẫn còn nguyên**.

### 7.3 Điều phép đo này **KHÔNG** chứng minh — nói rõ để không ai suy quá xa

- **KHÔNG** chứng minh Cursor tự kích hoạt kỹ năng ERP. Cũng **KHÔNG** chứng minh điều ngược lại.
- **KHÔNG** chứng minh 128 kỹ năng sẽ chọn đúng hay chọn sai khi cạnh tranh — chưa lượt thử nào chạy được.
- **KHÔNG** chứng minh khoá tắt-tự-gọi có tác dụng trên bản đang cài — chỉ chứng minh **chưa ai dùng nó**.
- Kết quả "17 kỹ năng, 0 kỹ năng ERP" là phép đo **tại một phiên, trên một bản công cụ**. Nó chứng minh **hiện không phát hiện được**; nó **không** nói về các bản công cụ khác hay cấu hình khác.

---

## 8. PHÁN QUYẾT TƯƠNG THÍCH

| Đối tượng | Phán quyết | Cơ sở |
|---|---|---|
| **Claude Code 2.1.259** — tự kích hoạt kỹ năng ERP bản địa | ⛔ **NOT_COMPATIBLE** | Không có đường dẫn phát hiện. Chứng minh bằng quan sát trực tiếp trên bản đã cài |
| **Claude Code 2.1.259** — đọc kỹ năng thủ công | ✅ Làm được | Tệp nằm trên đĩa, đọc được bình thường. **Nhưng đây không phải "kỹ năng" theo nghĩa công cụ** — không có điều kiện kích hoạt, không có van điều khiển |
| **Cursor 3.18.25** — tự kích hoạt kỹ năng ERP bản địa | ⚠️ **NOT_CHECKED** | Không có kênh đo. Không được kết luận theo cả hai hướng |
| **Dùng chéo hai công cụ** | ⛔ **NOT_COMPATIBLE hôm nay** | Hai công cụ đọc **hai tập rời nhau, không giao nhau ở bất kỳ tên nào**. Thư viện **không** mang được sang Claude Code |
| **128/128 kỹ năng** — mức `AUTO_SAFE` | ⛔ **BLOCKED_NEEDS_FIX** | Trượt tiêu chí **thứ nhất**: cần bằng chứng registry `ACTIVE`, thực tế có **0** kỹ năng `ACTIVE` |

### Đối chiếu từng tiêu chí `AUTO_SAFE` — kết quả cho toàn bộ 128 kỹ năng

| Tiêu chí bắt buộc | Kết quả | Đạt? |
|---|---|---|
| Bằng chứng registry `ACTIVE` | 127 `UNREVIEWED` · 1 `DORMANT` · **0 `ACTIVE`** | ⛔ **KHÔNG — trượt ngay đây** |
| Lớp R0 chỉ-đọc | Chỉ **15/128** đạt R0 | ⛔ Không cho 113 kỹ năng |
| Nguồn và mã băm nội dung chính xác | Đủ cho 118 kỹ năng được Git theo dõi; **10 kỹ năng bên thứ ba KHÔNG có** | ⛔ Không cho 10 |
| Thử khớp đúng ĐẠT | **NOT_EXECUTED** | ⛔ Không |
| Thử phủ định ĐẠT | **NOT_EXECUTED** | ⛔ Không |
| Thử gần-mà-không-phải ĐẠT | **NOT_EXECUTED** | ⛔ Không |
| Thử đụng độ ĐẠT | **NOT_EXECUTED** | ⛔ Không |
| Không có quyền gây tác dụng phụ | **0/128** khai danh sách công cụ được dùng ⇒ không kỹ năng nào **bị giới hạn** công cụ | ⛔ Không |
| Không còn xung đột tên / đường dẫn chưa giải | Tên: **sạch**. Đường dẫn: **0/128** khai phạm vi ⇒ chồng chéo **chưa giải** | ⛔ Không |
| Công cụ đang cài chứng minh có hỗ trợ | Claude Code: **PROVEN_FALSE**. Cursor: **NOT_PROVEN** | ⛔ Không |

⇒ **0 trong 10 tiêu chí đạt trọn cho bất kỳ kỹ năng nào. Phán quyết `AUTO_TRIGGER_NOT_APPROVED` giữ nguyên, và giữ nguyên là ĐÚNG.**

---

## 9. CHẾ ĐỘ VẬN HÀNH ĐỀ XUẤT CHO TỪNG ỨNG VIÊN

> Đây là **đề xuất để Chủ dự án quyết**, **không** phải việc đã làm. Audit này **không bật** gì cả.

| Nhóm | Số kỹ năng | Chế độ đề xuất | Điều kiện tiên quyết |
|---|---|---|---|
| 3 ứng viên R0 an toàn nhất (mục 4.4) | 3 | `READY_EXPLICIT_ONLY` | Soát hiệu lực nội dung → nếu còn đúng thì nâng nhãn lên `ACTIVE`; viết lại mô tả có mệnh đề **KHI NÀO** |
| Nhóm R0 còn lại, không vướng nhãn | ~5 | `READY_EXPLICIT_ONLY` | Như trên |
| Nhóm R0 vướng nhãn `LOI_THOI` / `SSOT_THANG` | 2 | ⛔ `BLOCKED` | Sửa nội dung hoặc gộp vào chuẩn trước |
| Nhóm R0 vướng nhãn `DA_GOP` | 2 | `READY_EXPLICIT_ONLY` kèm cảnh báo | Chuẩn mới là nguồn thắng, kỹ năng chỉ để tra lịch sử |
| 10 kỹ năng bên thứ ba | 10 | ⛔ `BLOCKED` | Hoàn tất quy trình cách ly bên thứ ba: xác minh nguồn → phiên bản/giấy phép → quét năng lực → quét vỏ/mạng/ghi → soát điều kiện kích hoạt → soát xung đột → thử trong bóng tối → phê duyệt |
| 66 kỹ năng R1 | 66 | `READY_PROMPT_REQUESTED` **sau khi** soát nội dung | **Ưu tiên xử 36 kỹ năng chọi chuẩn trước** |
| 18 kỹ năng R2 | 18 | `READY_EXPLICIT_ONLY` | Bắt buộc theo chính sách |
| 29 kỹ năng R3 | 29 | `READY_EXPLICIT_ONLY`, một số nên `BLOCKED` | Bắt buộc theo chính sách. 3 bản đã bị thay thế nên `BLOCKED` |
| **Kỹ năng nhãn `DORMANT`** | 1 | ⛔ `BLOCKED` | Chỉ mở lại bằng quyết định của Chủ dự án |

**Thứ tự làm hợp lý nhất nếu Chủ dự án muốn tiến tới thử nghiệm** — bốn bước, làm tuần tự, mỗi bước có phép đo riêng:
1. **Soát hiệu lực nội dung** cho một nhóm nhỏ (5–8 kỹ năng R0) → nâng nhãn lên `ACTIVE` nếu còn đúng. Đây là bước **bắt buộc đầu tiên** vì nó là tiêu chí trượt của cả 128 kỹ năng.
2. **Viết lại phần mô tả** của đúng nhóm đó theo khuôn "dùng khi …, KHÔNG dùng khi …". Đây là thứ **thực sự** điều khiển việc tự kích hoạt.
3. **Dựng kênh đo phía Cursor** rồi chạy ma trận 7 dạng × nhiều lượt trên đúng nhóm đó.
4. Chỉ khi ba bước trên đều đạt mới bàn tới việc bật, và bật **từng kỹ năng một**.

---

## 10. KHOẢNG TRỐNG CHÍNH XÁC CẦN VÁ — **KHÔNG thi công trong lượt này**

| Mã | Khoảng trống | Số đo | Trạng thái nợ |
|---|---|---|---|
| **G-1** | **0 kỹ năng nhãn `ACTIVE`** ⇒ chặn cứng mọi đường tới `AUTO_SAFE` | 127 `UNREVIEWED` · 1 `DORMANT` | **MỚI phát hiện trong lượt này** (là hệ quả có chủ đích của khoá 23/08, nhưng chưa ai nêu nó chặn `AUTO_SAFE`) |
| **G-2** | **78% mô tả không nêu điều kiện dùng** ⇒ tự kích hoạt không đáng tin | 100/128 | **MỚI** |
| **G-3** | **0/128 khai phạm vi đường dẫn** + bố cục phẳng ⇒ mọi kỹ năng là ứng viên cho mọi việc | 0/128 · 2 thư mục con đều rỗng | **MỚI** |
| **G-4** | **0/128 khai trường điều khiển nào** (tắt tự-gọi · danh sách công cụ · cho-người-gọi) ⇒ không van nào ở mức tệp | 0/128 mỗi trường | **MỚI** |
| **G-5** | **Không có móc mức Agent** ⇒ không cổng chặn trước-khi-gọi-công-cụ | 0 móc | **MỚI** (bổ sung cho nợ đã biết về việc cổng chỉ chạy khi gõ lệnh) |
| **G-6** | **Ba cổng kỹ năng không nằm trong móc trước-commit** ⇒ toàn vẹn sổ đăng ký không được canh ở cửa commit | 4/4 cổng trong móc, **0/3** cổng kỹ năng | **Trùng họ với nợ đã biết** — nay có số đo đích danh |
| **G-7** | **Claude Code không thấy thư viện** ⇒ đổi công cụ là mất toàn bộ 128 kỹ năng, trái tinh thần "đổi công cụ vẫn bắt nhịp" của mô hình năm bản luật | 0/128 phát hiện được | **MỚI — đáng chú ý nhất về mặt kiến trúc** |
| **G-8** | **Chưa có kênh đo phía Cursor** ⇒ không chứng minh được điều quan trọng nhất còn lại | — | **MỚI** |
| **G-9** | **10 kỹ năng bên thứ ba không có nguồn gốc kiểm được** | 0/10 khai phiên bản · 0/10 được Git theo dõi | Là hệ quả của quyết định 20/08 (đúng), nhưng **chặn** `AUTO_SAFE` |
| **G-10** | **3 kỹ năng đã bị thay thế vẫn phát hiện được đầy đủ**, cùng chùm R3 với bản thay | 3 kỹ năng | **MỚI** |
| **G-11** | **36 kỹ năng chọi chuẩn, 12 kỹ năng còn chứa chuỗi chuẩn CẤM** | 36 · 12 · điểm chọi cao nhất 10 | Cổng có canh **nhãn**, không canh **việc nạp** |
| **G-12** | **8 đường dẫn trông như thật nhưng đã chết** + **2 thư mục khuôn mẫu rỗng** | 8/8 kiểm đích danh đều chết | **MỚI** |
| **G-13** | **6 kỹ năng có câu chữ tự nhận quyền**, trái §L1 | 6 kỹ năng | **MỚI** |
| **G-14** | **Biểu mẫu phản hồi kỹ năng chưa từng được điền** ⇒ không có số liệu kích hoạt nhầm để học | Chỉ thấy trong văn bản luật và báo cáo audit, **không** thấy bản ghi đã điền | **Nợ đã biết** |
| **G-15** | **Chưa có bảng ánh xạ giữa hai thư viện kỹ năng** (thư viện Notion và thư viện 128 kỹ năng) | — | **Nợ đã biết** |
| **G-16** | **Sổ đăng ký phát hành trễ so với mã nguồn** | Sổ ghi mốc 25/08 · mã nguồn V1.00.367 | `SYNC_OVERDUE`, **không** phải lỗi |
| **G-17** | Một dòng nợ đã hết hiệu lực về nội dung mà vẫn treo trạng thái MỞ — số kỹ năng trong luật nay đã **đúng là 128** | Đo được: luật ghi 128, thực tế 128 | **Nên đóng ở lượt quản trị sau** |

> ⚠️ **Theo §G7.10, các dòng ghi "Nợ đã biết" phải được coi là NỢ ĐÃ BIẾT, KHÔNG phải lỗi mới.** Audit này chỉ **bổ sung số đo đích danh** cho chúng.

---

## 11. GHI NHẬN ĐIỀU KIỆN ĐO — trung thực về giới hạn

**11-G. Điểm chốt mã nguồn đã DỊCH trong lúc audit.** Đầu nhánh chuyển sang một commit mới hơn do **một phiên song song** ghi vào cùng cây làm việc (đúng như bối cảnh Chủ dự án mở nhiều phiên cùng lúc). Xử lý: **đo lại** các hạng mục then chốt tại đầu nhánh mới — năm bản luật **vẫn trùng khớp tuyệt đối**, phiên bản tài liệu luật **vẫn 2.9**, phiên bản mã nguồn **vẫn V1.00.367**, và **cả ba cổng kỹ năng vẫn PASS**. ⇒ Kết luận của audit **không đổi**. Nhưng phải nói rõ: audit này **không** chạy trên một trạng thái đóng băng.

**11-H. Xác nhận lại điểm chốt mà TanPhatAI đã biết.** Điểm chốt kho báo cáo công khai: **khớp đúng**, cây làm việc sạch. Lời khai *"V1.00.367 đã triển khai"*: **xác nhận được ở lớp MÃ NGUỒN** (đọc trực tiếp tệp phiên bản); **KHÔNG xác nhận được ở lớp MÁY VẬN HÀNH** vì lượt này không được cấp kênh đo — ghi đúng từ: `CODE_CURRENT` đạt, `RUNTIME_OBSERVED` là **UNVERIFIED**. Lời khai *"việc đã chuyển sang M1"*: **xác nhận được** qua sổ Chủ dự án (mục ghi việc đóng gói việc trải nghiệm phân quyền, ghi nhận nghiệm thu, và **mở M1**) cùng hai mục kế tiếp.

**11-I. Đã đọc kế hoạch/gói việc đang chạy và KHÔNG cắt ngang.** Gói việc hiện hành thuộc chuỗi đầu tháng 9. Audit này chỉ-đọc, không đụng tệp nào của gói việc đó, không chạy cổng nào có ghi.

**11-J. Nén ngữ cảnh.** Phiên **CÓ** bị gián đoạn giữa chừng. Sau khi tiếp tục, đã **đọc lại từ đĩa**: năm bản luật (đo lại băm), sổ đăng ký kỹ năng, tệp ghi đè hiệu lực nội dung, sổ đăng ký phát hành, sổ đăng ký đường dẫn, sổ nợ kỹ thuật, tệp móc trước-commit, hai tệp luật riêng của Cursor. **Không** kết luận nào dựa trên trí nhớ trước gián đoạn.

---

## 12. ĐƯỜNG LUI / THÁO BỎ NẾU THỬ NGHIỆM SAU NÀY THẤT BẠI

> Nêu **trước**, để nếu Chủ dự án cho thử nghiệm thì đường lui đã có sẵn chứ không phải nghĩ lúc đang cháy.

| Bước lui | Cách làm | Đảo được ngay? |
|---|---|---|
| 1. Tắt tự kích hoạt | Hạ nhãn hiệu lực nội dung của nhóm thử về `UNREVIEWED` trong **tệp nguồn viết tay**, rồi chạy lại bộ sinh sổ đăng ký | ✅ Ngay — sửa một tệp |
| 2. Chặn ở mức tệp | Thêm khoá tắt-tự-gọi vào phần đầu của đúng các tệp kỹ năng đó | ⚠️ **Phải chứng minh khoá có tác dụng trên bản đang cài TRƯỚC** khi tin nó |
| 3. Rút khỏi tầm phát hiện | Di chuyển nhóm thử ra khỏi thư mục kỹ năng | ✅ Đảo được, nhưng đụng đường dẫn — cần đối chiếu lại sổ đăng ký |
| 4. Chặn ở cửa commit | Đưa ba cổng kỹ năng vào móc trước-commit | ✅ Đảo được — nhưng làm commit chậm hơn, đây là điều đã cân nhắc có chủ đích trước đây |
| 5. Lui hoàn toàn | Quay về đúng trạng thái hôm nay: **0 kỹ năng `ACTIVE`**, không móc mức Agent, không trường điều khiển | ✅ Đây là trạng thái hiện tại — **chính là mốc lui an toàn** |

**Nguyên tắc lui:** vì thư viện **không có tệp thi hành nào** (0/128), việc lui **không** cần dọn tiến trình, không cần hoàn tác dữ liệu, không cần khôi phục bản sao lưu. Rủi ro cần lui là **rủi ro nội dung dạy sai** — nó biểu hiện thành mã nguồn sai, và mã nguồn sai thì lui bằng Git theo quy trình thường.

---

## 13. MA TRẬN HOÀN THÀNH HẠNG MỤC

| # | Hạng mục yêu cầu | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | Tóm tắt điều hành tiếng Việt | ✅ `READY_CANDIDATE` | Mục 1 |
| 2 | Bảng môi trường / phiên bản | ✅ `MEASURED_ONLY` | Mục 2 — cả hai công cụ có số hiệu chính xác |
| 3 | Ma trận đường dẫn phát hiện & thứ tự ưu tiên theo công cụ | ✅ `MEASURED_ONLY` | Mục 3 |
| 4 | Kiểm kê tĩnh **toàn bộ** kỹ năng áp dụng được | ✅ `AUDITED_ONLY` | Mục 4 — **128/128**, bằng phép đo xác định |
| 5 | Đối chứng sổ đăng ký ↔ khả năng gọi bản địa | ✅ `MEASURED_ONLY` | Mục 5 — tám câu hỏi đều có phán quyết |
| 6 | Ma trận rủi ro tác dụng phụ / công cụ / mạng / tệp thi hành | ✅ `MEASURED_ONLY` | Mục 6 |
| 7 | Bằng chứng thử kích hoạt cho ứng viên đã chọn | ⚠️ **`TESTED_SHADOW_ONLY` một phần** | Mục 7 — **một kết quả âm dứt khoát** phía Claude Code; ma trận 7 dạng **NOT_EXECUTED** do lỗi hạ tầng nhà cung cấp |
| 8 | Phán quyết tương thích cho Cursor · Claude Code · dùng chéo | ✅ `MEASURED_ONLY` | Mục 8 — Cursor ghi đúng là `NOT_CHECKED` |
| 9 | Chế độ vận hành đề xuất cho từng ứng viên | ✅ `READY_CANDIDATE` | Mục 9 — đề xuất, chưa thi hành |
| 10 | Khoảng trống chính xác cần vá, không thi công | ✅ `AUDITED_ONLY` | Mục 10 — 17 khoảng trống, phân biệt rõ nợ mới ↔ nợ đã biết |
| 11 | Đường lui / tháo bỏ | ✅ `READY_CANDIDATE` | Mục 12 |
| 12 | Ma trận hoàn thành hạng mục | ✅ | Chính mục này |
| 13 | LOCK-IN / OPEN ITEMS / NEXT ACTION đúng một việc | ✅ | Mục 14 |

> **Không hạng mục nào được ghi IMPLEMENTED · DEPLOYED · LIVE_VERIFIED** — đúng theo giới hạn của một lượt chỉ-đọc.

---

## 14. LOCK-IN · OPEN ITEMS · NEXT ACTION

### 🔒 LOCK-IN — chốt được, không cần đo lại

1. **Claude Code 2.1.259 KHÔNG phát hiện thư viện kỹ năng của dự án.** Tự kích hoạt trên công cụ này là **bất khả về cấu trúc**. Chứng minh bằng quan sát trực tiếp trên bản đã cài.
2. **0 kỹ năng nhãn `ACTIVE`** ⇒ 128/128 trượt tiêu chí `AUTO_SAFE` đầu tiên. **`AUTO_TRIGGER_NOT_APPROVED` giữ nguyên và giữ nguyên là đúng.**
3. **Nhãn trong sổ đăng ký không có đường thi hành tự động.** Là quy ước văn bản, không phải rào máy.
4. **Không có mặt tấn công dạng chạy mã** — 0/128 có tệp thi hành. Rủi ro nằm ở **nội dung dạy sai**, không ở tệp thi hành.
5. **Không có móc mức Agent.** Hàng rào tự động duy nhất ở **cửa commit** với **4** cổng — và **không** cổng kỹ năng nào trong đó.
6. **Năm bản luật trùng khớp tuyệt đối**, phiên bản tài liệu `2.9`, đã đo lại **hai lần** (lần hai sau khi đầu nhánh dịch).
7. **Không đụng độ tên** giữa 128 kỹ năng với nhau và với 17 kỹ năng dựng sẵn. Rủi ro thật là **chồng chéo ngữ nghĩa**, không phải trùng tên.

### ❓ OPEN ITEMS — chờ quyết định hoặc chờ phép đo

| # | Việc | Cần ai |
|---|---|---|
| O-1 | **Cursor có tự kích hoạt kỹ năng ERP hay không** — câu quan trọng nhất còn lại, chưa từng có phép đo nào trong kho | Cần **kênh đo phía Cursor** |
| O-2 | Soát hiệu lực nội dung để có kỹ năng `ACTIVE` đầu tiên — **cửa chặn số một** | **Chủ dự án** duyệt phạm vi nhóm soát |
| O-3 | Có viết lại phần mô tả theo khuôn "dùng khi / KHÔNG dùng khi" hay không | **Chủ dự án** |
| O-4 | Có đưa ba cổng kỹ năng vào móc trước-commit hay không (đổi lấy commit chậm hơn) | **Chủ dự án** |
| O-5 | Xử 36 kỹ năng chọi chuẩn thế nào — sửa nội dung, gộp vào chuẩn, hay khoá | **Chủ dự án** |
| O-6 | Có hoàn tất quy trình cách ly 10 kỹ năng bên thứ ba, hay khoá vĩnh viễn | **Chủ dự án** |
| O-7 | Có bắc cầu thư viện sang Claude Code hay không, và nếu có thì bằng cách nào — đây là câu hỏi **kiến trúc**, vì mô hình năm bản luật có tinh thần "đổi công cụ vẫn bắt nhịp" mà thư viện kỹ năng **không** đạt tinh thần đó | **Chủ dự án** |
| O-8 | Xử 3 kỹ năng đã bị thay thế mà vẫn phát hiện được | **Chủ dự án** |

### ➡️ NEXT ACTION — ĐÚNG MỘT VIỆC

> **Trình Chủ dự án quyết định phạm vi cho một lượt SOÁT HIỆU LỰC NỘI DUNG có giới hạn: chọn 5–8 kỹ năng lớp R0 để soát và nâng nhãn lên `ACTIVE`.**
>
> **Vì sao đúng việc này và không phải việc khác:** cả 128 kỹ năng trượt `AUTO_SAFE` ngay ở tiêu chí **thứ nhất** — thiếu bằng chứng `ACTIVE`. Mọi việc khác (viết lại mô tả, thêm phạm vi đường dẫn, thêm móc, dựng kênh đo Cursor) đều **vô nghĩa cho tới khi có ít nhất một kỹ năng `ACTIVE`**. Đây là cửa chặn duy nhất mở ra được đường đi tiếp, và bản thân nó là việc **cần Chủ dự án chốt phạm vi** chứ không phải việc Agent tự làm — vì khoá "cấm tự gán `ACTIVE` hàng loạt" là quyết định của Chủ dự án ngày 23/08/2026.
>
> ⛔ Việc này **KHÔNG** bao gồm: bật tự kích hoạt · sửa bất kỳ tệp kỹ năng nào · sửa sổ đăng ký · thêm móc · cài đặt hay cập nhật gì.

---

## 15. KHỐI BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Đo môi trường: Cursor 3.18.25 · Claude Code 2.1.259 (tiện ích chạy trong Cursor)
     · Node 24 · Windows 11 Pro. Đánh dấu trạng thái 14 bề mặt cấu hình.
   - Xác minh năm bản luật trùng khớp tuyệt đối (đo HAI lần: trước và sau khi
     đầu nhánh dịch do phiên song song). Phiên bản tài liệu luật 2.9.
   - Kiểm kê TOÀN BỘ 128 kỹ năng bằng phép đo xác định: trường siêu dữ liệu,
     lớp rủi ro R0-R3, chất lượng mô tả, tham chiếu chết, ngôn ngữ tự nhận quyền,
     trùng tên, kỹ năng đã bị thay thế, khoảng trống nguồn gốc.
   - Chứng minh hoặc bác bỏ tám câu hỏi đối chứng A-H, ghi rõ độ tin từng câu.
   - Chạy ba cổng kỹ năng chỉ-đọc (đã soi mã nguồn cổng TRƯỚC khi chạy để chắc
     chúng không ghi): cả ba PASS. Chạy lại sau khi đầu nhánh dịch: vẫn PASS.
   - Xác minh móc trước-commit có thật, đang khớp bản nguồn, và xác định ĐÍCH DANH
     bốn cổng nó chạy.
   - Chọn ứng viên thử kích hoạt (3 an toàn nhất + 3 chùm rủi ro cao + 1 mốc nền)
     và soạn đủ bảy dạng câu hỏi.
   - Xác nhận lại điểm chốt: điểm chốt kho báo cáo khớp; V1.00.367 xác nhận ở lớp
     mã nguồn; việc đã chuyển sang M1 xác nhận qua sổ Chủ dự án.

2. PHẠM VI
   ĐỤNG      : KHÔNG tệp nào trong kho mã nguồn. Chỉ tạo MỘT tệp báo cáo trong
               kho báo cáo công khai. Tệp tạm nằm trong vùng nháp của phiên.
   KHÔNG ĐỤNG: mã nguồn · cơ sở dữ liệu · triển khai · số phiên bản · kỹ năng ·
               sổ đăng ký · luật · móc · quyền · không bật AUTO_SAFE · không bật
               chế độ tuỳ chỉnh · không cài/cập nhật gì · không chạy tệp thi hành
               nào của kỹ năng (thư viện không có tệp thi hành nào để chạy).

3. BẰNG CHỨNG
   - Đối chiếu băm năm bản luật -> một giá trị duy nhất -> FILE_PROVEN
   - Liệt kê thư mục kỹ năng -> 128 thư mục, 1 tệp không phải tệp kỹ năng,
     0 tệp thi hành -> FILE_PROVEN
   - Điểm danh trường siêu dữ liệu -> 0/128 cho mọi trường điều khiển -> FILE_PROVEN
   - Danh sách kỹ năng được cấp cho chính phiên này -> 17 kỹ năng, 0 kỹ năng ERP
     -> RUNTIME_PROVEN (quan sát trực tiếp trên bản công cụ đã cài)
   - Ba cổng kỹ năng -> PASS, chạy hai lần ở hai đầu nhánh -> CODE_PROVEN
   - Lệnh đối chiếu móc -> hook đã cài trùng khớp bản nguồn -> FILE_PROVEN
   - Lệnh liệt kê thuộc tính + dò liên kết Git + liệt kê điểm nối hệ điều hành
     -> không có cầu nối nào tới thư mục kỹ năng Cursor -> FILE_PROVEN
   - Đọc trực tiếp tệp phiên bản -> V1.00.367 -> CODE_PROVEN
   - Máy vận hành -> KHÔNG ĐO -> UNVERIFIED (không được cấp kênh)

4. GHI SỔ YÊU CẦU OWNER
   [x] CHƯA — lý do: lượt này KHÔNG có quyết định/chỉnh hướng/yêu cầu MỚI nào
       của Chủ dự án phát sinh trong phiên. Chỉ thị audit là một yêu cầu THI HÀNH
       chỉ-đọc, và FINAL LOCK cấm sửa sổ đăng ký cùng tệp quản trị. Tám mục
       OPEN ITEMS ở mục 14 là việc CẦN Chủ dự án quyết — khi Chủ dự án trả lời,
       MỖI chỉ thị sẽ là MỘT mục sổ theo GOV-NOTION-HANDOFF-001.
       notion_sync_state đề xuất khi đó: OWNER_APPROVED_PENDING_NOTION_SYNC.

5. PUSH BÁO CÁO CÔNG KHAI
   [x] Xem dòng trạng thái ở cuối tệp này.

6. CÒN SÓT / CHƯA LÀM
   - Ma trận thử kích hoạt 7 dạng x nhiều lượt: NOT_EXECUTED. Nguyên nhân: 51
     tiến trình phụ trong 2 lần chạy đều bị nhà cung cấp trả lỗi quá tải trước
     khi làm được việc gì (0 token tiêu thụ). Lỗi hạ tầng, không phải kết quả đo.
   - Khả năng tự kích hoạt bản địa phía Cursor: NOT_PROVEN. Chưa có kênh đo.
   - Khoá tắt-tự-gọi có tác dụng trên bản công cụ đang cài hay không: NOT_PROVEN.
   - Chưa soát HIỆU LỰC NỘI DUNG của bất kỳ kỹ năng nào — ngoài phạm vi lượt này
     và bị FINAL LOCK cấm.
   - 17 khoảng trống ở mục 10 đều CHƯA vá (đúng yêu cầu: nêu, không thi công).

7. ĐANG CHỜ OWNER
   - Tám mục OPEN ITEMS ở mục 14.
   - Chặn việc gì nếu chưa trả lời: chặn TOÀN BỘ đường tiến tới bật tự kích hoạt,
     vì cửa chặn số một (0 kỹ năng ACTIVE) chỉ Chủ dự án mở được.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Trình Chủ dự án quyết định phạm vi cho một lượt SOÁT HIỆU LỰC NỘI DUNG có
   giới hạn: chọn 5-8 kỹ năng lớp R0 để soát và nâng nhãn lên ACTIVE.

9. CHƯA XÁC MINH ĐƯỢC
   - Cursor có tự kích hoạt kỹ năng ERP hay không — vì sao: không có kênh đo
     Cursor từ phiên Claude Code. Ai xác minh được: một phiên chạy TRONG Cursor,
     tự báo danh sách kỹ năng nó được cấp.
   - Việc chọn giữa các kỹ năng cạnh tranh (7 dạng câu hỏi) — vì sao: lỗi quá tải
     phía nhà cung cấp. Ai xác minh được: chạy lại khi hạ tầng rảnh, và chỉ có
     nghĩa trên công cụ THẬT SỰ phát hiện thư viện.
   - Phiên bản đang chạy trên máy vận hành — vì sao: không được cấp kênh đo.
     Ai xác minh được: Chủ dự án, hoặc một lượt có kênh chỉ-đọc được duyệt.
   - 10 kỹ năng bên thứ ba có khớp bản gốc thượng nguồn hay không — vì sao: không
     có phiên bản, không có mã băm trong Git để đối chiếu.

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — thiếu: phép đo kích hoạt phía Cursor (O-1) và ma trận 7 dạng.
       Điều kiện lên PASS: có kênh đo phía Cursor + chạy lại ma trận 7 dạng trên
       một nhóm kỹ năng đã được nâng nhãn ACTIVE.
       Ghi rõ: phần PHÁN QUYẾT SỬ DỤNG thì KHÔNG provisional —
       AUTO_TRIGGER_NOT_APPROVED đã chốt vững bằng tiêu chí 0 kỹ năng ACTIVE.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén/gián đoạn: CÓ.
   Tham chiếu ĐÃ ĐỌC LẠI TỪ ĐĨA sau gián đoạn: năm bản luật (đo lại băm) ·
   sổ đăng ký kỹ năng · tệp ghi đè hiệu lực nội dung kỹ năng · sổ đăng ký phát
   hành · sổ đăng ký đường dẫn · sổ nợ kỹ thuật · tệp móc trước-commit · hai tệp
   luật riêng của Cursor · tệp phiên bản mã nguồn.
   Không kết luận nào dựa trên trí nhớ trước gián đoạn.
═══════════════════════════════════════════
```

---

> **BÀN GIAO CHO:** TanPhatAI (Notion).
> **Trạng thái đồng bộ Notion đề xuất:** `OWNER_APPROVED_PENDING_NOTION_SYNC` — tám mục OPEN ITEMS ở mục 14 cần Chủ dự án quyết trước khi có gì để đồng bộ.
> **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC, nêu rõ để Notion KHÔNG nhận nhầm thành sự thật hiện hành:** khả năng tự kích hoạt phía Cursor · việc chọn giữa các kỹ năng cạnh tranh · tác dụng của khoá tắt-tự-gọi trên bản công cụ đang cài · phiên bản đang chạy trên máy vận hành · sự khớp bản gốc của 10 kỹ năng bên thứ ba.
> **Luật đã áp trong lượt này:** quy trình chọn kỹ năng và yêu cầu tra sổ đăng ký · tách trục sức khoẻ cấu trúc ↔ hiệu lực nội dung · kỹ năng không phải authority · cách ly kỹ năng bên thứ ba · claim không mạnh hơn bằng chứng · sổ nợ kỹ thuật · an toàn báo cáo công khai · khối báo cáo kết thúc · định nghĩa XONG · đọc lại tham chiếu sau khi phiên bị nén.
