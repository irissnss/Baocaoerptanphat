# KHÉP VÒNG LUẬT · KỸ NĂNG · CÔNG CỤ · MẶT PHẲNG ĐIỀU KHIỂN

**Work package:** `WP-GOV-SKILL-TOOL-CLOSEOUT-20260826`
**Ngày:** 26/08/2026
**Plan of Record:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`
**Người thi hành:** Agent IDE (một người ghi duy nhất)

> **Tính giá: ĐÓNG BĂNG trong phiên này.** Chỉ ghi nhận tri thức, **không** thi hành.
> Hạng mục tính giá đi theo kế hoạch con riêng `PL-ERP-TINH-GIA-20260825`.

---

## 1. VIỆC LÀM ĐƯỢC — TÓM TẮT MỘT TRANG

| # | Việc | Kết cục | Lớp bằng chứng |
|---|---|---|---|
| 1 | `DEBT-105` — cổng an toàn đọc **cây làm việc** thay vì **chỉ mục** | ✅ VÁ XONG, kiểm ngược 4/4 | RUNTIME_PROVEN |
| 2 | `DEBT-106` — chuỗi cổng gọi chế độ **tự kiểm**, không đọc đầu ra thật | ✅ VÁ XONG, kiểm ngược đo song song | RUNTIME_PROVEN |
| 3 | `DEBT-107` — kiểm 6 dạng biến thể ⇒ lộ lỗ hổng **phạm vi miễn trừ** | ✅ VÁ XONG, kiểm ngược 2/2 | RUNTIME_PROVEN |
| 4 | Rà 5 bản sao luật | ✅ ĐỒNG NHẤT TUYỆT ĐỐI | FILE_PROVEN |
| 5 | Tìm nguồn luật cạnh tranh | ⚠️ **PHÁT HIỆN 1** → ghi `DEBT-119` | FILE_PROVEN |
| 6 | Rà công cụ | ✅ 6 công cụ, Context7 gọi thật được | RUNTIME_PROVEN |
| 7 | Rà cách ly 128 kỹ năng | ✅ 127 cách ly + 1 ngủ đông | FILE_PROVEN |

---

## 2. BA NỢ KỸ THUẬT ĐÃ ĐÓNG

### 2.1 `DEBT-105` — cổng an toàn đang gác nhầm bản

**Bệnh.** Hai cổng `secret-scan` và `pii-scan` lấy **danh sách** đường dẫn từ chỉ mục git,
nhưng đọc **nội dung** từ đĩa. Hai thứ đó có thể khác nhau.

Hậu quả đo được: một tệp được `git add` khi còn chứa bí mật, rồi sửa sạch trên đĩa mà **chưa
add lại** ⇒ cổng đọc bản **sạch** trên đĩa và cho ĐẠT, trong khi bản **bẩn** mới là bản sắp
được commit. Hai cổng này gác `pre-commit` — tức là **bảo đảm sai ở đúng nơi được tin nhất**.

**Thuốc.** Thêm ba hàm vào nguồn dùng chung `scripts/tests/lib/tracked-files.mjs`:

| Hàm | Việc |
|---|---|
| `stagedFiles()` | Đọc `git diff --cached --name-status -z`; xử lý đúng ca đổi tên/sao chép (hai đường dẫn) |
| `readStagedBlob()` | Đọc đúng bản trong chỉ mục; trả rỗng cho tệp nhị phân và kho con |
| `docDeQuet()` | Ưu tiên bản chỉ mục khi tệp đang staged, lùi về cây làm việc khi không |

Hai cổng nay in rõ nguồn quét trong dòng bằng chứng.

**Kiểm ngược — 4/4 đạt** (hai cổng × hai chiều):

| Cổng | Chỉ mục | Đĩa | Kết quả | Đúng chưa |
|---|---|---|---|---|
| `pii-scan` | BẨN | sạch | **KHÔNG ĐẠT** | ✅ bắt được |
| `pii-scan` | sạch | BẨN | ĐẠT | ✅ không báo nhầm |
| `secret-scan` | BẨN | sạch | **KHÔNG ĐẠT** | ✅ bắt được |
| `secret-scan` | sạch | BẨN | ĐẠT | ✅ không báo nhầm |

---

### 2.2 `DEBT-106` — chuỗi cổng đang tự chấm điểm chính mình

**Bệnh.** Chuỗi `test:gov-gates` gọi mắt xích `test:completion-report-gate`, mà lệnh đó
phân giải thành chế độ **tự kiểm** — chỉ chạy **ba chuỗi mẫu viết cứng trong chính tệp kiểm**.
Chuỗi luôn xanh ở mọi phiên, **giá trị thi hành cho báo cáo thật bằng 0**. Cổng đọc đầu ra
thật thì **không nằm trong chuỗi**.

Đây đúng loại lỗi mà `GOV-GATE-REAL-INPUT-001` sinh ra để chặn — và chính §G5 đã cảnh báo
từ 19/08, nhưng chuỗi cổng vẫn giữ biến thể tự kiểm suốt từ đó.

**Thuốc.** Thêm chế độ mới: tự tìm **báo cáo kết thúc mới nhất theo mốc sửa** trong
`docs/reports/`, rồi chạy bộ kiểm 11 trường trên **đầu ra thật**. Không tìm thấy tệp nào
⇒ **KHÔNG ĐẠT**, không "bỏ qua cho xanh". Chuỗi đổi sang mắt xích mới; chế độ tự kiểm
**giữ nguyên** làm cổng kiểm-chính-bộ-kiểm.

**Kiểm ngược — đo song song trên CÙNG một đầu ra sai.** Đầu ra sai được dựng cố ý: thiếu
trường 7·8·9·11, ghi «ĐÃ PUSH» mà không có mã commit, viện dẫn mục sổ không tồn tại.

| Bản | Mã thoát | Nghĩa |
|---|---|---|
| Bản **CŨ** (tự kiểm) | `0` | **XANH GIẢ** — không thấy gì cả |
| Bản **VÁ** (đầu ra thật) | `1` | **BẮT ĐƯỢC 6 vấn đề** |

Gỡ tệp sai ⇒ về `0`. Trọn chuỗi sau vá: mã thoát `0`.

Đáng chú ý: cổng còn **đối chiếu chéo sổ thật** — nó phát hiện mục sổ được viện dẫn
không hề tồn tại. Đó là bằng chứng cổng đang đọc dữ liệu thật, không phải chuỗi mẫu.

---

### 2.3 `DEBT-107` — kiểm biến thể làm lộ một lỗ hổng khác

Kiểm 6 dạng viết địa chỉ mà một địa chỉ thật có thể ẩn dưới:

| Dạng | Bắt được |
|---|---|
| Liên kết markdown `mailto:` | ✅ |
| Mã inline trong dấu huyền ngược | ✅ |
| Chuỗi trong JSON | ✅ |
| Dòng thêm trong khối diff | ✅ |
| Địa chỉ có dấu tiếng Việt đầy đủ | ✅ |
| Ô trong CSV | ⚠️ chặn bởi điều kiện khác, **nội dung không được quét** |

Truy tiếp ca CSV lộ ra **lỗ hổng thật**: miễn trừ trước đây là **MIỄN TẤT CẢ** — một mục
trong danh sách miễn trừ khiến tệp đó **không bao giờ** được quét nữa. Với khoá phụ thuộc
và với chính tệp cổng thì đúng (nội dung của chúng vốn là địa chỉ tác giả gói và mẫu nhận
dạng). Nhưng với **tệp dữ liệu** thì sai bản chất: miễn trừ đáng ra chỉ miễn điều kiện
*cấm đưa tệp nguồn hàng loạt lên kho*, **không** được miễn luôn việc quét bên trong.

**Thuốc.** Mục miễn trừ nay có **phạm vi**:

- `ALL` — miễn cả việc quét nội dung. Chỉ dùng khi nội dung **vốn dĩ** là mẫu/khoá phụ thuộc.
- `BULK` — chỉ miễn điều kiện tệp hàng loạt. **Nội dung vẫn quét.** Mặc định an toàn cho dữ liệu.

**Kiểm ngược 2/2 đạt:** địa chỉ người thật đặt trong tệp `.csv` miễn trừ phạm vi `BULK`
→ **bắt được**; phạm vi `ALL` → cho qua có chủ đích.

Kèm sửa một lỗi hiển thị lặp chữ trong dòng báo.

---

## 3. NĂM BẢN SAO LUẬT — ĐỒNG NHẤT TUYỆT ĐỐI

Đo trực tiếp trên đĩa, không tin khai báo:

| Bản | Mã băm (16 ký tự đầu) | Cỡ | Số dòng | Commit |
|---|---|---|---|---|
| `AGENTS.md` | `d100fae2cb03172d` | 117 864 B | 2 304 | `d9e823a` |
| `CLAUDE.md` | `d100fae2cb03172d` | 117 864 B | 2 304 | `d9e823a` |
| `.cursorrules` | `d100fae2cb03172d` | 117 864 B | 2 304 | `d9e823a` |
| `.antigravityrules` | `d100fae2cb03172d` | 117 864 B | 2 304 | `d9e823a` |
| `GEMINI.md` | `d100fae2cb03172d` | 117 864 B | 2 304 | `d9e823a` |

**5/5 trùng khớp từng byte.** Cùng bản tài liệu `2.8`, cùng kết thúc dòng, cùng commit.
Kiến trúc năm bản sao **còn nguyên**.

Số định nghĩa luật đếm được: **36**.

---

## 4. PHÁT HIỆN — CÓ NGUỒN LUẬT NGOÀI VÒNG QUẢN (`DEBT-119`)

Rà toàn bộ tệp chỉ đạo agent đang bị theo dõi, tìm ra **một** nguồn tự áp nằm ngoài năm
bản sao:

| Tệp | Tự áp mọi phiên | Cổng parity có quản | Kết luận |
|---|---|---|---|
| `.cursor/rules/deploy-schema-compatibility.mdc` | **CÓ** | **KHÔNG** | ⚠️ ngoài vòng quản |
| `.cursor/rules/graphify.mdc` | không | không | ✅ chỉ nạp khi gọi — không thuộc diện |

Cổng parity so đúng năm tệp và **không hề biết tệp thứ nhất tồn tại**.

§A1 mục 7 đã chốt: chỉ thị riêng cho công cụ **vẫn phải nằm trong cả năm file** kèm điều
kiện kích hoạt — chính là để năm file giữ được tính đồng nhất. Một tệp tự áp bên ngoài
phá đúng nguyên tắc đó.

**Mức độ:** nội dung tệp đó hiện **không chọi** luật nào trong năm bản. Rủi ro là **cấu
trúc**, chưa phải mâu thuẫn thực tế — nhưng nó là đúng loại khe hở mà năm-bản-sao sinh ra
để bịt.

**Chờ Owner quyết hướng** — hai lối, Agent không tự chọn vì cả hai đều đổi kiến trúc luật:
- **(a)** gộp nội dung vào năm bản kèm điều kiện kích hoạt, rồi hạ tệp đó xuống không-tự-áp;
- **(b)** mở rộng cổng parity để quản luôn nhóm tệp tự áp.

---

## 5. CÔNG CỤ — SÁU MỤC, MỘT GỌI THẬT ĐƯỢC

| Công cụ | Trạng thái sổ | Đo được | Lớp bằng chứng |
|---|---|---|---|
| **Context7** | sẵn-sàng-khi-gọi | **Gọi thật thành công** | **RUNTIME_PROVEN** |
| Graphify | tư vấn | khai báo | DOC_PROVEN |
| Graphify (dạng máy chủ) | chưa nối | khai báo | DOC_PROVEN |
| Spec Kit | ngủ đông | khai báo | DOC_PROVEN |
| WebApp Testing | Owner duyệt, **chưa cài** | khai báo | DOC_PROVEN |
| Superpowers | khoá | khai báo | DOC_PROVEN |

**Context7 — kiểm sâu theo đúng §M5.** Không dừng ở "cấu hình bật là coi như chạy được":

1. Gọi thật một truy vấn phân giải thư viện → **trả kết quả**.
2. Đo phiên bản **thật đang cài** trong `node_modules`, không tin dòng khai trong tệp cấu hình.

| Thư viện | Khai báo | Cài thật | Khớp |
|---|---|---|---|
| Next.js | `^16.1.6` | **16.1.6** | ✅ |
| React | `^19.2.4` | **19.2.4** | ✅ |
| TypeScript | `^5.9.3` | **5.9.3** | ✅ |
| Tailwind CSS | `^4.2.1` | **4.2.1** | ✅ |

3. Danh mục Context7 **có đúng bản `v16.1.6`**.

⇒ Kết luận theo luật: **khớp phiên bản** — *không* phải hạng "chỉ để tham khảo". Đây là
nâng hạng có bằng chứng so với ghi nhận cũ trong sổ công cụ, vốn mới dừng ở mức khai báo.

---

## 6. KỸ NĂNG — CÁCH LY CÒN NGUYÊN

| Đo | Số |
|---|---|
| Thư mục kỹ năng thật | **128** |
| Mục trong sổ đăng ký | **128** |
| Lệch | **0** |

Trạng thái **hiệu lực nội dung** — trục quyết định việc có được tự kích hoạt hay không:

| Trạng thái | Số | Nghĩa |
|---|---|---|
| `UNREVIEWED` | **127** | **Chưa ai soát hiệu lực ⇒ KHÔNG phải đang dùng được** |
| `DORMANT` | **1** | Ngủ đông — **cấm tự kích hoạt** |
| `ACTIVE` | **0** | — |

Đúng như §G7.15 đòi hỏi: **không có kỹ năng nào được gán "đang dùng được" hàng loạt**.
Cách ly còn nguyên vẹn — 127 kỹ năng vẫn bắt buộc phải mở nguồn gốc và đối chiếu sự thật
hiện hành trước khi dùng cho bất kỳ thay đổi nào.

Trục **sức khoẻ cấu trúc** (đo việc *có đủ khai báo hay không*, **không** đo hiệu lực):
58 lành · 34 thiếu điều kiện kích hoạt · 36 chưa rõ mục đích.

> Hai trục này **độc lập** — một kỹ năng có thể lành về cấu trúc mà vẫn ngủ đông về nội dung.
> Đây chính là ca đã sinh ra §G7.15.

---

## 7. CÒN LẠI — NÓI THẲNG

| Việc | Vì sao chưa xong |
|---|---|
| `DEBT-116` — quét **họ tên thật**, 5 tệp còn dính | **Chờ Owner quyết hướng.** Họ tên người Việt không có khuôn dạng máy nhận chắc chắn; quét bằng danh sách họ phổ biến sẽ báo nhầm tràn lan (tên công ty, tên đường, tên sản phẩm) |
| `DEBT-119` — nguồn luật ngoài vòng quản | **Chờ Owner quyết hướng** (mục 4) |
| Hạng mục tính giá | **Đóng băng có chủ đích** — theo kế hoạch con riêng |

---

## 8. VIỆC KẾ TIẾP — ĐÚNG MỘT VIỆC

Owner chọn hướng cho `DEBT-119`: **(a)** gộp vào năm bản kèm điều kiện kích hoạt, hay
**(b)** mở rộng cổng parity. Chọn xong, Agent thi hành ngay trong phiên kế tiếp.

---

*Báo cáo này an toàn công khai: chỉ nêu tên tệp, tên bảng, số đếm và mã commit — không có
mã nguồn, không có bí mật, không có dữ liệu cá nhân, không có thông tin hạ tầng.*
