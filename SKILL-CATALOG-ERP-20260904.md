# DANH MỤC CHI TIẾT TOÀN BỘ KỸ NĂNG AGENT IDE — ERP TÂN PHÁT

> **Mã audit:** `AUDIT-ERP-SKILL-CATALOG-002` · thi hành nguyên văn prompt canonical tại §4
> **Ngày lập:** 04/09/2026 (phiên chạy vắt qua nửa đêm 03→04/09/2026)
> **Chế độ:** `READ_ONLY_FULL_SKILL_CATALOG` — **KHÔNG** sửa kỹ năng · sổ đăng ký · luật · móc · mã nguồn · CSDL; **KHÔNG** kích hoạt kỹ năng; **KHÔNG** cài/cập nhật tự động
> **Audit tiền nhiệm:** [AUDIT-ERP-SKILL-TRIGGER-COMPAT-001](AUDIT-KHA-NANG-KICH-HOAT-KY-NANG-20260903.md)
> **Artefact máy đọc:** `SKILL-CATALOG-ERP-20260904.json` · **Checksum:** `SKILL-CATALOG-ERP-20260904.sha256`

```
══════════════════════════════════════
  CHỮ KÝ AGENT — KHÔNG GIẢ MẠO
══════════════════════════════════════
  Vai trò     : Agent IDE
  Công cụ     : Claude Code (tiện ích mở rộng chạy TRONG Cursor)
  Lane        : execution / IDE
  Actor       : Agent IDE — KHÔNG phải Agent Notion, KHÔNG phải Coordinator
  Phạm vi     : lập danh mục chi tiết 128 kỹ năng + artefact máy đọc
══════════════════════════════════════
```

**PHIẾU KỸ NĂNG CHO LƯỢT NÀY** — `requested=NONE` · `loaded=NONE` · `mode=NO_SKILL_REQUIRED` · lý do: lập danh mục chính hệ thống kỹ năng, dùng một kỹ năng chưa kiểm sẽ làm nhiễu phép đo.

---

## 1. TÓM TẮT ĐIỀU HÀNH

Đã lập xong hồ sơ **một dòng cho mỗi kỹ năng**, đủ **128/128** kỹ năng, kèm artefact máy đọc để TanPhatAI nhập thẳng vào danh mục Notion mà không phải diễn giải lại.

**Bốn điều cần biết trước khi đọc bảng:**

**Thứ nhất — con số tổng vẫn là 128, không lệch.** Prompt §4 nêu "kỳ vọng 128" và yêu cầu đo lại. Đo được: **128** thư mục kỹ năng, **128** tệp kỹ năng, **128** mục trong sổ đăng ký. **Chênh lệch = 0.** Cả ba cổng máy của thư viện đều XANH.

**Thứ hai — không một kỹ năng nào đủ điều kiện tự kích hoạt, và điều đó không đổi so với audit trước.** Trạng thái nội dung: **127 chưa soát · 1 ngủ đông · 0 còn hiệu lực**. Vì `AUTO_SAFE` đòi bằng chứng "còn hiệu lực" nên **128/128 trượt ngay tiêu chí đầu**. Số kỹ năng mang chế độ `AUTO_SAFE_CANDIDATE` trong artefact này: **0**.

**Thứ ba — phân bố chế độ gọi an toàn tối đa HÔM NAY:** EXPLICIT_ONLY **42** · PROMPT_REQUESTED **35** · REFERENCE_ONLY **33** · BLOCKED **18**. Nghĩa là **51** kỹ năng hiện **không được dùng làm chỉ dẫn thi hành**, và **42** kỹ năng chỉ được gọi khi nêu đích danh.

**Thứ tư — rủi ro thật nằm ở nội dung, không ở tệp thi hành.** Toàn thư viện có **0 kỹ năng kèm tệp thi hành** ⇒ không có mặt tấn công dạng chạy mã. Nhưng: **36 kỹ năng chọi chuẩn giao diện**, trong đó **11 kỹ năng còn chứa chuỗi mà chuẩn CẤM**; **100/128 mô tả không nêu điều kiện dùng**; **3 kỹ năng đã bị thay thế mà vẫn phát hiện được đầy đủ**; **10 kỹ năng bên thứ ba không chứng minh được nguồn gốc**; **6 kỹ năng có câu chữ tự nhận quyền** (trái §L1 — kỹ năng không phải authority).

---

## 2. ĐỐI CHIẾU SỐ LƯỢNG VÀ ĐIỂM CHỐT

| Hạng mục | Giá trị | Trạng thái |
|---|---|---|
| Thư mục kỹ năng trên đĩa | 128 | ✅ đo trực tiếp |
| Tệp kỹ năng đọc được | 128 | ✅ đo trực tiếp |
| Mục trong sổ đăng ký kỹ năng | 128 | ✅ cổng `test:skills-registry` PASS |
| Dòng trong artefact JSON | 128 | ✅ khớp |
| Dòng trong bảng B của báo cáo này | 128 | ✅ khớp |
| **Chênh lệch so với kỳ vọng 128 của §4** | **0** | ✅ không có delta |
| Kỹ năng phát hiện thêm ngoài thư viện ERP | 0 | không có kỹ năng dự án/người dùng/tiện ích bổ trợ nào khác |
| Kỹ năng dựng sẵn mà phiên Claude Code thực sự được cấp | 17 | ✅ quan sát trực tiếp |
| Điểm chốt bộ luật | năm bản **trùng khớp tuyệt đối**, phiên bản tài liệu **2.9** | ✅ đo hai lần |
| Điểm chốt sổ đăng ký | ghi trong artefact JSON (mã băm nội dung một chiều) | ✅ |
| Điểm chốt kho mã nguồn riêng | **CHE ở bản công khai** — giao riêng cho Chủ dự án/TanPhatAI | ⚠️ xem mục 12 |
| Tệp đang sửa dở lúc kết xuất | 2 (**của phiên song song, KHÔNG phải của lượt audit này**) | ⚠️ ghi nhận |

> ⚠️ **Đầu nhánh kho mã nguồn DỊCH nhiều lần trong lúc audit** do Chủ dự án chạy nhiều phiên song song trên cùng cây làm việc. Đã đo lại parity bộ luật và ba cổng kỹ năng ở đầu nhánh mới — **vẫn trùng khớp, vẫn XANH**. Kết luận không đổi. Nhưng phải nói rõ: audit này **không** chạy trên một trạng thái đóng băng.

---

## 3. BỀ MẶT CÔNG CỤ ĐÃ CÀI (COVERAGE B)

| Hạng mục | Cursor | Claude Code |
|---|---|---|
| Phiên bản | **3.18.25** | **2.1.259** |
| Kênh phát hành | `stable` | tiện ích mở rộng chạy **trong** Cursor |
| Nền chạy | Windows 11 Pro 10.0.26200 · x64 | Node 24 · Windows 11 Pro 10.0.26200 · x64 |
| Đường dẫn phát hiện — dự án | `.cursor/skills` (**128**) · `.cursor/rules` (**2**, cả hai **không** tự áp) | `.claude/skills` — **KHÔNG TỒN TẠI** |
| Đường dẫn phát hiện — người dùng | không có thư mục kỹ năng mức người dùng | **KHÔNG TỒN TẠI** |
| Tiện ích bổ trợ / chợ tiện ích | không có | **KHÔNG TỒN TẠI** |
| Số kỹ năng thực sự cấp ra cho phiên | `NOT_CHECKED` | **17** (toàn bộ là kỹ năng dựng sẵn) |
| Kỹ năng ERP được cấp ra | `NOT_CHECKED` | **0 / 128** |
| Phát hiện bản địa | ⚠️ **`NOT_CHECKED`** | ⛔ **`PROVEN_FALSE`** |
| Cách lấy bằng chứng | không có kênh quan sát phiên Cursor từ phiên Claude Code | quan sát TRỰC TIẾP danh sách kỹ năng mà chính phiên được cấp |
| Giới hạn của phép đo | KHÔNG được suy đoán theo cả hai hướng | phép đo tại MỘT phiên, trên MỘT bản công cụ |

**Danh sách 17 kỹ năng dựng sẵn mà phiên Claude Code được cấp** (an toàn công khai, đây là năng lực của công cụ chứ không phải tài sản dự án): `design` · `dataviz` · `artifact-design` · `artifact-diagramming` · `artifact-capabilities` · `update-config` · `keybindings-help` · `code-review` · `simplify` · `fewer-permission-prompts` · `loop` · `schedule` · `claude-api` · `workflow-authoring` · `run` · `init` · `security-review`.

Không tên nào trong 17 kỹ năng này trùng với bất kỳ tên nào trong 128 kỹ năng ERP ⇒ **không có đụng độ tên**.

---

## 4. BẢNG A — ĐẾM THEO TỪNG TRỤC

| Trục | Phân bố |
|---|---|
| Hệ kỹ năng | ERP_LOCAL_CURSOR_SKILLS **128** |
| Actor sử dụng | Agent IDE **128** |
| Lớp rủi ro | R1_CODE **72** · R3_RELEASE **28** · R2_DATA_OR_AUTH **18** · R0_READ_ONLY **10** |
| Trạng thái nội dung | UNREVIEWED **127** · DORMANT **1** |
| Chế độ gọi an toàn tối đa | EXPLICIT_ONLY **42** · PROMPT_REQUESTED **35** · REFERENCE_ONLY **33** · BLOCKED **18** |
| Trạng thái thử | NOT_CHECKED **128** |
| Phát hiện bản địa | ClaudeCode=PROVEN_FALSE; Cursor=NOT_CHECKED **128** |
| Đối chiếu chuẩn giao diện | KHONG_UI **71** · SSOT_THANG **36** · DA_GOP **11** · BO_SUNG **8** · LOI_THOI **1** · TRUNG_KHOP **1** |

**Các phép đếm chất lượng khác:**

| Phép đo | Số kỹ năng |
|---|---|
| Có bằng chứng Sổ Yêu Cầu Chủ dự án | **10** |
| Không được Git theo dõi (thiếu nguồn gốc) | **10** |
| Có tệp thi hành kèm theo | **0** |
| Có tham chiếu THẬT đã chết | **17** |
| Hứa tài sản kèm theo mà thư mục rỗng | **2** |
| Có câu chữ tự nhận quyền | **6** |
| Mô tả KHÔNG nêu điều kiện dùng | **100** |
| Đã bị thay thế mà vẫn phát hiện được | **3** |
| Còn chứa chuỗi chuẩn CẤM | **17** |

---

## 5. BẢNG B — MỘT DÒNG MỖI KỸ NĂNG

> Tóm tắt an toàn công khai. **Hồ sơ đầy đủ** (chức năng · mục đích · dùng khi · không dùng khi · đầu vào/đầu ra · luồng làm việc · rủi ro · nguồn/phiên bản · đối chiếu chuẩn · bằng chứng Chủ dự án · khuyến nghị · việc còn mở) nằm ở trường `details_markdown` của từng bản ghi trong artefact JSON.

| # | Slug | Nhóm công dụng | Chức năng (tóm tắt) | Rủi ro | Nội dung | Chế độ | Chuẩn GD |
|---|---|---|---|---|---|---|---|
| 1 | `annotated-screenshot-review` | Quy trình làm việc — đọc ảnh chú thích củ… | Đặt quy trình bắt buộc để đọc ảnh mà Chủ dự án gửi kèm có đánh dấu bằng tay: khoanh đỏ (vuông/tròn/oval), mũi tên đỏ và ghi chú chữ. Kỹ năng bắt phải… | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | DA_GOP |
| 2 | `async-await-conversion` | Mã nguồn — chuyển hàm đồng bộ sang bất đồ… | Hướng dẫn chuyển các hàm chạy đồng bộ sang dạng bất đồng bộ (async/await) một cách an toàn, khi lớp lưu trữ của một mô-đun chuyển từ giữ dữ liệu tron… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 3 | `audit-gap-report` | Quản trị — rà soát có hệ thống và báo cáo… | Đặt khuôn cho việc rà soát một mô-đun hoặc toàn hệ thống theo cách đối chiếu từng yêu cầu với phần đã làm thật, rồi kết xuất ra một BẢNG KHOẢNG TRỐNG… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 4 | `audit-ui` | Giao diện — hiển thị thông tin vết ghi (a… | Chuẩn hoá cách hiển thị thông tin vết ghi trên các bảng danh mục: đặt một biểu tượng lịch sử ngay cạnh mã của từng dòng, bấm vào mở hộp thoại chỉ-đọc… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 5 | `auto-name-generation-formula` | Dữ liệu và biểu mẫu — sinh tên đầy đủ tự … | Đặt khuôn cho việc tự ghép tên đầy đủ của một thực thể từ nhiều trường nhỏ theo một công thức cố định (ví dụ nhóm + tên ngắn + màu + thông số + kích … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 6 | `autocomplete-input-component` | Giao diện — ô nhập biểu thức có danh sách… | Hướng dẫn dựng một ô nhập kèm danh sách gợi ý xổ xuống, chia ba nhóm biến · hàm · toán tử, có lọc theo chữ đang gõ và bấm để chèn đúng vị trí con trỏ… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 7 | `backup-before-db-mutation` | Cơ sở dữ liệu — sao lưu bắt buộc & bằng c… | Kỹ năng dạy agent MỘT quy trình cố định phải chạy TRƯỚC mọi thao tác đổi cấu trúc hoặc dữ liệu CSDL: tạo bản sao lưu, rồi tự CHỨNG MINH bản sao đó kh… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 8 | `bundle-transaction-pattern` | Cơ sở dữ liệu — ghi nhiều bảng liên quan … | Đặt khuôn ghi một cụm dữ liệu cha–con–cháu vào nhiều bảng trong CÙNG một giao dịch, theo đúng thứ tự phụ thuộc khoá ngoại: ghi bảng cha trước để lấy … | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 9 | `column-config-workflow` | Giao diện — cấu hình cột hiển thị cho bản… | Quy ước làm việc ba nhịp để chọn nhanh cột hiển thị: trợ lý liệt kê toàn bộ cột kèm số thứ tự, người dùng gửi lại một dãy số cho BẢNG và một dãy số c… | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 10 | `confirm-action-dialog` | Giao diện — hộp thoại xác nhận hành động … | Đặt quy tắc cho hộp thoại xác nhận hành động nguy hiểm: chữ trên nút phải đúng nghĩa (Hủy để bỏ, Xóa để thực hiện), phải có trạng thái đang chạy (đổi… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 11 | `contract-preservation-check` | Cơ sở dữ liệu và kiến trúc — bảo toàn cam… | Quy trình sáu bước bảo vệ những thành phần đang đọc hoặc ghi một tài nguyên (cột, bảng, đầu nối) trước khi tài nguyên đó bị đổi tên, đổi kiểu, xoá, h… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 12 | `cross-module-consistency` | Giao diện — nhất quán liên mô-đun bằng cá… | Khung làm việc bắt buộc chọn một mô-đun đã làm xong làm mẫu tham chiếu trước khi dựng hoặc thiết kế lại mô-đun mới, kèm ma trận gợi ý mô-đun nào nên … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 13 | `currency-input-preview` | Giao diện — ô nhập số tiền kèm bản xem tr… | Mẫu ba nhịp cho ô nhập số tiền: ô nhập kiểu số, hậu tố đơn vị tiền đặt chìm bên phải trong ô, và một dải nhỏ ngay dưới hiển thị lại số vừa gõ đã chèn… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 14 | `data-access-layer` | Kiến trúc mã — tầng truy cập dữ liệu (tác… | Đặt ba nguyên tắc tổ chức mã đọc-ghi cơ sở dữ liệu: trang và thành phần giao diện chỉ hiển thị, mọi truy vấn dồn về các tệp store trong thư mục thư v… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 15 | `data-safety-legacy-columns` | Cơ sở dữ liệu — xử lý cột cũ không còn dù… | Quy trình sáu bước để xử lý an toàn các cột còn trong cơ sở dữ liệu nhưng không còn được nhắc trong bản đặc tả mới. Bắt đọc bản đặc tả mới nhất, so v… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 16 | `database-ops` | Vận hành cơ sở dữ liệu — sao lưu, phục hồ… | Sổ tay vận hành cơ sở dữ liệu gồm bốn phần: danh mục các kịch bản PowerShell dùng để lập môi trường, sao lưu, phục hồi và nạp lại danh mục địa chính;… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 17 | `db-id-lookup-from-backup` | Cơ sở dữ liệu — tra cứu định danh từ bản … | Nêu cách tìm ra mã định danh (ID) THẬT của một bản ghi bằng cách lục các tệp sao lưu .sql trong kho, tìm câu lệnh chèn dữ liệu của bảng cần tra, tách… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 18 | `db-label-to-human-text` | Giao diện — chuẩn hoá nhãn hiển thị cho n… | Quy trình đổi chữ đang hiển thị trên giao diện từ tên bảng/cột cơ sở dữ liệu (kiểu gạch dưới, không dấu) sang tiếng Việt dễ đọc. Gồm sáu bước: liệt k… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 19 | `debug-systematic` | Kỹ thuật — quy trình gỡ lỗi có hệ thống | Đặt ra sáu bước cố định khi gặp lỗi: Thu thập bằng chứng lỗi, Cô lập tới tệp/hàm, Phân tích tìm nguyên nhân gốc, Sửa với thay đổi nhỏ nhất, Xác minh … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 20 | `demo-before-implement` | Giao diện — dựng bản mẫu trình duyệt trướ… | Trước khi lập trình một phần giao diện lớn, dựng một trang HTML mẫu độc lập (dùng thư viện kiểu dáng nạp qua mạng và phông chữ ngoài), chụp ảnh trang… | R1 sửa mã | chưa soát | **KHOÁ** | LOI_THOI |
| 21 | `dependency-relationship-scan` | Kỹ thuật — quét quan hệ phụ thuộc trước k… | Bắt quét đầy đủ mọi mối liên hệ trước khi sửa/tái cấu trúc: liệt kê phụ thuộc TRỰC TIẾP (hàm được gọi, trạng thái đọc/ghi, thành phần giao diện, quy … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 22 | `deploy-local-github-vps` | Vận hành — triển khai mã lên máy chủ vận … | Quy trình triển khai từ máy phát triển qua kho mã rồi xuống máy chủ vận hành, chia rõ hai nhánh: chỉ-mã (khi lược đồ dữ liệu không đổi) và mã-kèm-cơ-… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 23 | `detail-panel-layout` | Giao diện — bảng chi tiết bên phải (bố cụ… | Bộ mẫu mã cho panel chi tiết mở ra bên phải khi bấm một dòng bảng: phần đầu panel chuyển sắc chứa mã, tên, huy hiệu trạng thái và cụm nút (sửa, đóng)… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 24 | `detail-panel-toggle` | Giao diện — bảng chi tiết bên phải (trạng… | Mẫu triển khai bố cục danh sách kèm panel chi tiết bên phải, tập trung vào phần trạng thái và tương tác: một biến giữ bản ghi đang chọn, hàm xử lý bấ… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 25 | `deventry-link-management` | Giao diện — trang tổng quan mô-đun và điề… | Chuẩn hoá cách khai báo và hiển thị nhóm liên kết "Dev Entry" (lối vào từng màn con) trên trang tổng quan của một mô-đun. Quy định kiểu dữ liệu cho m… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 26 | `documentation-sync` | Quản trị — đồng bộ bộ luật 5 bản, có cổng… | Dạy agent MỘT quy trình duy nhất khi sắp sửa bộ luật: chỉ sửa MỘT bản gốc (tệp khai là canonical), xem khác biệt, rồi sao chép đè lên bốn bản còn lại… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 27 | `dropdown-display-name-only` | Giao diện — ô chọn danh mục (dropdown) | Quy tắc cho ô chọn danh mục: phần chữ của mỗi lựa chọn chỉ hiển thị TÊN, không hiện mã nghiệp vụ hay số định danh nội bộ; giá trị gửi về máy chủ vẫn … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 28 | `dual-key-standard` | Cơ sở dữ liệu — quy ước khoá cho dữ liệu … | Quy ước dùng hai khoá song song cho dữ liệu danh mục: một khoá hệ thống dạng số tự tăng dùng cho quan hệ, cập nhật, xoá và chống trùng tuyệt đối; một… | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 29 | `duplicate-file-1-resolution` | An toàn tệp — xử lý bản sao trùng tên | Quy trình bắt buộc trước khi xoá một tệp bản sao có hậu tố "(1)" bên cạnh tệp gốc: thu thập bằng chứng gồm thời điểm sửa cuối và kết quả so sánh nội … | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 30 | `erp-change-control` | Quản trị — khung kiểm soát thay đổi | Khung chín mục để kiểm soát mọi thay đổi đáng kể của hệ thống, trả lời bốn câu hỏi trước khi làm: ai có thẩm quyền, nguồn nào là sự thật hiện hành, l… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 31 | `feature-flag-phased-rollout` | Phát hành — bật tắt tính năng theo giai đ… | Quy trình bảy bước để đưa một tính năng mới ra theo từng chặng bằng một cờ bật/tắt: đặt tên cờ theo quy ước, khai cờ ở nơi cấu hình với giá trị mặc đ… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 32 | `field-name-consistency-check` | Sửa lỗi — khớp tên trường giữa lớp hàm tr… | Dạy agent một quy trình ba bước cho đúng MỘT loại lỗi: giá trị trả về của hàm trợ giúp / hàm lưu trữ mang tên trường khác với tên trường mà thành phầ… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 33 | `file-update-safe-workflow` | Quản trị tài liệu — quy trình cập nhật tệ… | Đặt ra quy trình sáu bước để cập nhật một tệp tài liệu quan trọng mà không làm mất nội dung cũ: đọc trọn tệp trước, giữ nguyên phần đang có, chỉ thêm… | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 34 | `fk-ref-by-id-not-name` | Cơ sở dữ liệu — quy ước tham chiếu khoá n… | Dạy cách lọc và tham chiếu dữ liệu qua khoá ngoại bằng ID số nguyên thay vì bằng tên hay mã dạng chuỗi. Kèm bốn bước: xác định bảng nguồn và bảng tha… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 35 | `fk-safe-delete-guard` | Cơ sở dữ liệu + Giao diện — chặn xoá khi … | Chuẩn hoá thao tác xoá khi bản ghi đang bị bảng khác tham chiếu: đếm số dòng tham chiếu trước, nếu còn thì trả về thông báo thân thiện kèm số lượng t… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 36 | `fk-snapshot-field-pattern` | Cơ sở dữ liệu — mẫu trường ảnh chụp cạnh … | Mẫu thiết kế: bên cạnh trường khoá ngoại, thêm một cột lưu sẵn tên hoặc giá trị lấy từ bảng danh mục tại thời điểm tạo bản ghi. Nhờ vậy khi truy vấn … | R2 dữ liệu/quyền | chưa soát | **KHOÁ** | ⛔ chọi |
| 37 | `flex-layout-expand-fix` | Giao diện — bố cục dọc co giãn và vùng cu… | Sửa lỗi khối nội dung bung ra bị che, bị tràn, hoặc không cuộn được khi nằm trong một khung bố cục dọc có chiều cao giới hạn. Cách làm là gán đúng va… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 38 | `form-field-validation` | Giao diện — kiểm tra hợp lệ trường biểu m… | Gom các cách kiểm tra hợp lệ cho biểu mẫu: dùng thuộc tính sẵn có của trình duyệt (bắt buộc nhập, kiểu thư điện tử/điện thoại/số/ngày, khuôn mẫu ký t… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 39 | `form-helper-text-pattern` | Giao diện — ghi chú hướng dẫn dưới trường… | Tách ghi chú dưới ô nhập thành hai loại: ghi chú cho người dùng cuối thì luôn hiển thị và viết bằng lời dễ hiểu; ghi chú kỹ thuật cho lập trình viên … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 40 | `globalthis-singleton-store` | Kiến trúc mã nguồn — kho dữ liệu tạm tron… | Mẫu dựng kho dữ liệu tạm nằm trong bộ nhớ tiến trình, gắn vào đối tượng toàn cục có tên riêng theo mô-đun, để dữ liệu không bị dựng lại mỗi lần trang… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 41 | `hard-enforcement-review` | Quy trình làm việc — nghi thức rà soát & … | Đặt ra một nghi thức 8 bước bắt buộc cho việc rà soát/sửa giao diện: nhắc lại yêu cầu thành danh sách R1..Rn, đối chiếu hiện trạng với mẫu kỳ vọng (đ… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 42 | `header-inline-badge` | Giao diện — bố cục phần đầu trang (tiêu đ… | Mẫu chuyển các nhãn/thẻ thống kê từ một dòng riêng nằm dưới tiêu đề trang lên NẰM CÙNG HÀNG với tiêu đề. Nêu bốn thay đổi cụ thể: đổi căn dọc của khu… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 43 | `hierarchical-leaf-only-filter` | Dữ liệu danh mục — lọc lựa chọn cho ô thả… | Quy tắc và hàm lọc để ô thả xuống tham chiếu danh mục phân cấp chỉ hiện các nút LÁ (nút không có con), tự ẩn mọi nút cha. Gồm hàm lọc dùng chung, ba … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 44 | `icon-style-guideline` | Giao diện — chuẩn biểu tượng (icon) | Quy định phong cách biểu tượng cho toàn ERP: CẤM emoji làm biểu tượng giao diện, CẤM bọc emoji trong vòng tròn màu, CẤM chấm tròn đặc (trừ chỉ báo tr… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 45 | `implement-g2-ux` | Giao diện — biểu mẫu trong ngăn trượt/hộp… | Hướng dẫn gắn bộ ba chống-mất-dữ-liệu vào mọi biểu mẫu tạo mới/chỉnh sửa nằm trong ngăn trượt hay hộp thoại: móc theo dõi biểu mẫu đã bị sửa hay chưa… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 46 | `implement-wizard-step` | Giao diện — quy trình nhập liệu nhiều bướ… | Hướng dẫn tạo hoặc ghép thêm một BƯỚC vào quy trình nhập liệu nhiều bước: nêu tiêu chí chọn giữa quy trình nhiều bước và biểu mẫu một lượt, ba nguyên… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 47 | `in-memory-to-db-migration` | Dữ liệu — chuyển kho tạm trong bộ nhớ san… | Quy trình 8 bước chuyển một mô-đun từ kho dữ liệu tạm trong bộ nhớ tiến trình sang lưu bền trong cơ sở dữ liệu: liệt kê toàn bộ hàm của kho cũ, xác m… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 48 | `inline-filter-bar-layout` | Giao diện — thanh lọc và tìm kiếm của tra… | Mẫu dựng thanh lọc gồm ô "Tổng số" + ô thả xuống chọn nhóm + ô tìm kiếm nằm TRÊN CÙNG MỘT HÀNG NGANG, không xuống dòng, kèm nút xoá lọc ở cuối. Gồm 6… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 49 | `input-validation-on-blur` | Giao diện — kiểm tra dữ liệu nhập | Hướng dẫn gắn bước kiểm tra dữ liệu vào thời điểm ô nhập MẤT tiêu điểm (rời ô), thay vì kiểm theo từng phím gõ. Đưa ra ba mẫu kiểm sẵn: biểu thức có … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 50 | `layout-component-redesign` | Giao diện — khung vỏ ứng dụng (thanh bên … | Đưa ra quy trình bảy bước để thiết kế lại ba thành phần khung vỏ dùng chung: thanh bên, thanh trên và bố cục chính. Bắt buộc xem mã hiện tại trước, d… | R3 phát hành | chưa soát | **KHOÁ** | ⛔ chọi |
| 51 | `master-list-data-table` | Giao diện — bảng danh sách dữ liệu | Đặc tả cách dựng bảng danh mục: hàng tiêu đề đúng một dòng (không nhét ô lọc vào), biểu tượng sắp xếp trên cột sắp xếp được, tiêu đề ghim khi cuộn, v… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 52 | `master-list-page-template` | Giao diện — khuôn trang danh sách | Khuôn dựng trọn một trang danh sách gồm chín khối: khung trang, phần đầu trang (tiêu đề + huy hiệu thống kê + nút tạo mới), thanh lọc, bảng, phân tra… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 53 | `mobile-responsive-ui-patterns` | Giao diện — thích ứng màn hình điện thoại | Gom trọn bộ quy tắc làm giao diện thích ứng điện thoại thành tám khối: mốc bề rộng màn hình, viết tắt nhãn trên điện thoại và hiện đủ trên máy tính, … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 54 | `mock-data-generator` | Dữ liệu — sinh dữ liệu giả cho giai đoạn … | Hướng dẫn sinh bộ dữ liệu giả chuẩn cho một phân hệ hoặc bảng mới khi chưa nối được cơ sở dữ liệu: đọc định nghĩa kiểu dữ liệu, xác định trường bắt b… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 55 | `module-color-palette` | Giao diện — bảng màu theo phân hệ | Bảng tra màu cho mười phân hệ (M0, M1, M3 tới M9 và MF), mỗi phân hệ có màu chính, hai đầu dải chuyển sắc, màu biểu tượng và màu viền. Kèm sáu mẫu áp… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | DA_GOP |
| 56 | `mysql-schema-extraction` | Cơ sở dữ liệu — trích xuất cấu trúc bảng … | Quy trình sáu bước để lấy cấu trúc bảng từ cơ sở dữ liệu MySQL trên máy Windows và dán vào báo cáo dạng chữ: tìm chương trình dòng lệnh, đọc thông số… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 57 | `not-implemented-placeholder` | Giao diện — trạng thái rỗng / tính năng c… | Hướng dẫn dựng một khối giao diện thay thế cho tab hoặc mục chức năng đã lên kế hoạch nhưng chưa làm: gắn phù hiệu cảnh báo lên tab, dựng khối cảnh b… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 58 | `nullable-conditional-logic` | Chất lượng mã — an toàn logic với giá trị… | Chỉ ra một lỗi logic khó thấy: khi so sánh thuộc tính qua toán tử truy cập an toàn trên một đối tượng có thể rỗng, kết quả trả về ngược với mong đợi … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 59 | `number-input-live-preview` | Giao diện — ô nhập số và tiền tệ | Quy định cách làm ô nhập số/tiền: ô nhập giữ SỐ THUẦN (không dấu phân cách), còn số đã định dạng theo kiểu Việt Nam được hiện ở một dòng xem trước NG… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 60 | `optimistic-ui-updates` | Giao diện — phản hồi tức thì sau thao tác… | Khuôn mẫu cập nhật danh sách trên màn hình NGAY sau khi tạo/sửa/xoá thành công, không chờ tải lại từ máy chủ: thêm bản ghi lên đầu danh sách, thay bả… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 61 | `phase-2-placeholder-notes` | Giao diện — ghi chú tính năng làm dở | Hướng dẫn gắn một dòng ghi chú nhỏ, in nghiêng, màu mờ ngay dưới ô nhập hoặc trong ô bảng, cho những chỗ tính năng mới làm ở mức tối thiểu — ví dụ ng… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 62 | `phased-migration-with-backfill` | Cơ sở dữ liệu — di trú lược đồ có nạp bù … | Quy trình 8 bước để thêm cột vào bảng ĐANG CÓ dữ liệu thật rồi nạp bù giá trị cho các dòng cũ: soi lược đồ và đếm dòng hiện có, chốt lược đồ đích, ch… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 63 | `plan-execution-protocol` | Quản trị — vòng đời kế hoạch và cổng thực… | Dạy agent chạy một việc lớn theo vòng SÁU bước bắt buộc CHUẨN BỊ → CỔNG → THỰC THI → XÁC MINH → ĐỐI CHIẾU → ĐÓNG, mỗi bước gắn đích danh một mã luật … | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 64 | `popover-scroll-fix` | Giao diện — sửa lỗi cuộn trong lớp phủ lồ… | Sửa lỗi không cuộn được chuột bên trong danh sách thả xuống khi thành phần đó nằm bên trong ngăn trượt hoặc hộp thoại: nguyên nhân là sự kiện cuộn bị… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 65 | `portal-safe-dropdown-overlay` | Giao diện — lớp phủ chọn (dropdown/combob… | Quy định cách dựng danh sách chọn (dropdown/combobox) sao cho lớp phủ không bị cắt cụt khi nằm trong thẻ/khu vực có thuộc tính chặn tràn. Bắt buộc đư… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 66 | `premium-module-page-redesign` | Giao diện — thiết kế lại trang tổng quan … | Đặt khuôn mẫu nền cho việc thiết kế lại các trang tổng quan/trang cổng của một mô-đun: khung trang, thứ bậc thị giác, bản sắc màu của mô-đun, và quy … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | TRUNG_KHOP |
| 67 | `premium-table-styling` | Giao diện — trình bày dòng và đầu bảng da… | Gom tám khuôn mẫu trình bày cho bảng danh sách: ô định danh đầu dòng có hộp biểu tượng đổi màu theo trạng thái, huy hiệu trạng thái, cột tổng tiền nh… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 68 | `print-html-template` | Giao diện — in chứng từ bằng trang HTML t… | Khuôn mẫu tạo chức năng In cho chứng từ (báo giá, đơn hàng, phiếu): mở một cửa sổ trình duyệt mới, ghi vào đó một trang HTML tự dựng gồm năm khối (đầ… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 69 | `props-from-server-not-mock` | Kiến trúc dữ liệu — nguồn dữ liệu cho ô c… | Bảo đảm thành phần phía trình duyệt nhận dữ liệu danh mục từ thành phần phía máy chủ (đã đọc cơ sở dữ liệu thật) truyền xuống qua thuộc tính, thay vì… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 70 | `public-report-sanitize-gate` | An toàn — cổng lọc nội dung trước khi côn… | Cổng bắt buộc chạy trước khi đẩy bất kỳ nội dung nào lên kho báo cáo công khai. Nó ĐIỀU PHỐI hai cổng máy đã có sẵn của dự án (quét bí mật hai chế độ… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 71 | `radix-overlay-guard` | Giao diện — lớp phủ lồng nhau và cổng trợ… | Hai phần. Phần đầu là cổng trợ năng bắt buộc: mọi bảng trượt và hộp thoại phải có một tiêu đề đúng thành phần chuẩn (ẩn về mặt hình ảnh nếu đầu trang… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 72 | `ref-dropdown-scope-filter` | Giao diện — lọc ô chọn danh mục theo phạm… | Khuôn mẫu lọc danh sách lựa chọn của một ô chọn tham chiếu để chỉ hiện đúng MỘT nhóm/phân loại theo nghiệp vụ, thay vì đổ toàn bộ bảng danh mục. Việc… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 73 | `ref-options-from-server` | Dữ liệu & giao diện — nạp danh mục tham c… | Đặt khuôn cho các ô chọn (dropdown/combobox) trỏ tới bảng danh mục: dữ liệu phải được nạp từ cơ sở dữ liệu ngay trong thành phần máy chủ (Server Comp… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 74 | `refactor-extract-component` | Chất lượng mã — tách thành phần giao diện… | Quy trình 5 bước để gỡ một đoạn giao diện bị chép đi chép lại thành MỘT thành phần dùng chung: nhận diện mẫu lặp, xác định danh sách thuộc tính thay … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 75 | `remove-raw-json-editing` | Giao diện biểu mẫu — gỡ ô sửa dữ liệu cấu… | Chuyển các ô cho phép người dùng gõ trực tiếp chuỗi cấu hình dạng JSON thô trong biểu mẫu thành khối CHỈ ĐỌC kèm một nút "Xem" mở hộp thoại hiển thị … | R2 dữ liệu/quyền | chưa soát | **KHOÁ** | ⛔ chọi |
| 76 | `repo-integrity-recovery-audit` | Vận hành & phục hồi — rà toàn vẹn kho mã … | Quy trình 6 bước bắt buộc để rà toàn vẹn kho mã sau khi tệp biến mất không rõ nguyên nhân: kiểm toàn vẹn kho quản lý phiên bản, băm lại TOÀN BỘ tệp đ… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 77 | `reusable-component-from-demo` | Giao diện — dựng thành phần dùng lại từ b… | Quy trình 6 bước biến một mẫu giao diện lặp lại trong bản dựng thử dạng trang tĩnh thành một thành phần dùng chung có thuộc tính: nhận diện mẫu, rút … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 78 | `root-directory-verification` | Vận hành & hạ tầng — xác minh thư mục gốc… | Quy trình 5 bước xác minh thư mục gốc dự án và ép mọi tệp thực thi SUY GỐC ĐỘNG thay vì viết cứng đường dẫn: lấy gốc hiện hành từ sổ đăng ký đường dẫ… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 79 | `safe-cross-module-error` | Kiến trúc & xử lý lỗi — cách ly hỏng hóc … | Khuôn xử lý lỗi cho tình huống một mô-đun kích hoạt hành động sang mô-đun khác: hoàn tất trọn vẹn thao tác CHÍNH trước, rồi mới gọi thao tác PHỤ tron… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 80 | `scaffold-module` | Kiến trúc — dựng khung mô-đun mới theo mẫ… | Hướng dẫn dựng khung một mô-đun mới với đầy đủ thao tác thêm/sửa/xoá theo bốn quy tắc kiến trúc mà dự án coi là bất di bất dịch: trang mặc định là th… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 81 | `schema-column-drop-safe` | Cơ sở dữ liệu — đổi lược đồ (xoá cột an t… | Đưa ra quy trình bảy bước để xoá một hoặc nhiều cột khỏi một bảng dữ liệu mà không mất dữ liệu và không làm gãy ràng buộc khoá ngoại. Quy trình đi từ… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 82 | `schema-migration-safe` | Cơ sở dữ liệu — khung an toàn dữ liệu khi… | Khung tổng quát cho MỌI việc làm thay đổi lược đồ cơ sở dữ liệu: bắt buộc soi lược đồ đang chạy TRƯỚC, rồi phân loại mức an toàn của từng trường bị ả… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 83 | `schema-visualization` | Cơ sở dữ liệu — trình bày lược đồ để Chủ … | Quy định ĐỊNH DẠNG ĐẦU RA khi Chủ dự án yêu cầu xem toàn bộ lược đồ của một thực thể hoặc một mô-đun: phải xuất ba bảng markdown trực quan — bảng lượ… | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 84 | `screenshot-verification` | Kiểm thử — chụp ảnh màn hình để đối chiếu… | Quy trình chụp ảnh màn hình sau khi làm xong giao diện rồi đối chiếu với bản mẫu: điều hướng tới trang, chờ tải xong, chụp ảnh toàn trang, mở ảnh ra … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 85 | `search-normalization` | Giao diện — chuẩn hoá ô tìm kiếm và phạm … | Đặt chuẩn cho mọi ô tìm kiếm và ô lọc trong hệ thống: cắt khoảng trắng thừa, đưa về chữ thường, và tách dấu tiếng Việt để người dùng gõ có dấu hay kh… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 86 | `searchable-dropdown` | Giao diện — ô chọn MỘT giá trị có tìm kiếm | Khuôn mẫu dựng ô chọn danh mục có ô tìm kiếm bên trong, dùng khi danh sách lựa chọn dài. Bao gồm: tự đưa con trỏ vào ô tìm kiếm khi mở, lọc không phâ… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 87 | `searchable-multiselect-popover` | Giao diện — ô chọn NHIỀU giá trị có tìm k… | Khuôn mẫu dựng ô chọn NHIỀU giá trị dạng lớp phủ có ô tìm kiếm: lọc không dấu (kèm đổi chữ đ thành d), lớp phủ được đưa ra ngoài cây bố cục nên không… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 88 | `section-field-reorder` | Giao diện — chuyển một trường từ thẻ mục … | Quy trình cắt-dán một TRƯỜNG hiển thị từ thẻ mục này sang thẻ mục khác trong biểu mẫu hoặc bảng chi tiết, theo yêu cầu của Chủ dự án. Gồm bốn bước: k… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 89 | `server-client-split-pattern` | Kiến trúc mã nguồn — tách trang thành phầ… | Hướng dẫn tách một trang vốn chạy trọn vẹn phía trình duyệt thành ba tệp: tệp trang chạy phía máy chủ lo lấy dữ liệu và bọc khung bố cục, tệp thành p… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 90 | `server-page-filter-defaults` | Giao diện — bộ lọc và tìm kiếm trên trang… | Đặt quy tắc để bộ lọc trên trang danh sách không bị lệch giữa phần máy chủ đọc tham số trên thanh địa chỉ và phần trình duyệt điều khiển ô chọn. Trọn… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 91 | `server-props-lookup-pattern` | Giao diện — hiển thị đúng tên thay vì mã … | Chữa lỗi màn hình hiện ra mã số thay vì tên gọi (ví dụ hiện một con số thay vì tên loại vật tư). Nguyên nhân gốc được nêu là thành phần phía trình du… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | — |
| 92 | `skill-mining-governance` | Quản trị — kiểm kê và vòng đời thư viện k… | Đặt quy trình quản lý thư viện kỹ năng như một hệ thống thay vì một đống lời nhắc rời rạc. Kỹ năng này quét sâu lịch sử lỗi lặp, gỡ lỗi lặp và sửa lặ… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 93 | `speckit-analyze` | Bộ công cụ ngoài (Spec Kit) — soát chéo n… | Chạy một lượt phân tích CHỈ ĐỌC, soi chéo ba tài liệu của một tính năng — bản đặc tả, bản kế hoạch và bản danh sách công việc — để tìm chỗ trùng lặp,… | R2 dữ liệu/quyền | chưa soát | **KHOÁ** | — |
| 94 | `speckit-checklist` | Bộ công cụ ngoài (Spec Kit) — sinh bảng k… | Sinh ra một bảng kiểm theo lĩnh vực do người dùng chọn, nhưng bảng kiểm này KHÔNG kiểm hệ thống chạy đúng hay sai — nó kiểm chính chất lượng của các … | R3 phát hành | chưa soát | **KHOÁ** | — |
| 95 | `speckit-clarify` | Bộ công cụ ngoài (Spec Kit) — hỏi làm rõ … | Quét bản đặc tả của tính năng theo một bảng phân loại mười nhóm (phạm vi chức năng, mô hình dữ liệu, luồng tương tác, thuộc tính phi chức năng, tích … | R0 chỉ-đọc | chưa soát | **KHOÁ** | — |
| 96 | `speckit-constitution` | Bộ công cụ ngoài (Spec Kit) — tạo và sửa … | Điền và cập nhật tệp hiến chương của bộ khung Spec Kit: thay các chỗ trống khuôn mẫu dạng từ khoá viết hoa trong ngoặc vuông bằng nội dung cụ thể, qu… | R3 phát hành | chưa soát | **KHOÁ** | — |
| 97 | `speckit-converge` | Quy trình đặc tả — đối chiếu mã nguồn với… | Đọc bộ ba tệp của một tính năng (đặc tả · kế hoạch · danh sách việc) rồi soi mã nguồn hiện tại để tìm phần chưa làm, làm dở, làm ngược ý, hoặc làm th… | R1 sửa mã | chưa soát | **KHOÁ** | — |
| 98 | `speckit-erp-ssot-adapter` | Quản trị công cụ — lớp keo quy định cách … | Không phải một lệnh chạy được, mà là lớp quy ước đặt lên trên bộ lệnh Spec Kit khi dùng trên kho ERP đã có sẵn mã. Nó chốt một thứ tự nguồn sự thật t… | R3 phát hành | **NGỦ ĐÔNG** | **KHOÁ** | — |
| 99 | `speckit-implement` | Quy trình đặc tả — thi hành danh sách việ… | Đọc danh sách việc và kế hoạch của một tính năng rồi thi hành từng đầu việc theo đúng thứ tự pha và ràng buộc phụ thuộc, đánh dấu hoàn thành ngược lạ… | R2 dữ liệu/quyền | chưa soát | **KHOÁ** | — |
| 100 | `speckit-plan` | Quy trình đặc tả — sinh tài liệu thiết kế… | Biến một đặc tả tính năng thành bộ tài liệu thiết kế: điền bối cảnh kỹ thuật và đánh dấu những chỗ còn mù mờ, chạy bước kiểm đối chiếu hiến chương, r… | R0 chỉ-đọc | chưa soát | **KHOÁ** | — |
| 101 | `speckit-specify` | Quy trình đặc tả — dựng đặc tả tính năng … | Nhận một câu mô tả tính năng bằng ngôn ngữ thường, đặt tên rút gọn hai đến bốn từ, tạo thư mục tính năng đánh số, chép khuôn đặc tả vào rồi điền: kịc… | R2 dữ liệu/quyền | chưa soát | **KHOÁ** | — |
| 102 | `speckit-tasks` | Quy trình đặc tả — sinh danh sách việc xế… | Đọc kế hoạch và đặc tả (kèm mô hình dữ liệu, hợp đồng giao diện, ghi chép nghiên cứu nếu có) rồi sinh ra một tệp danh sách việc thi hành được ngay. V… | R1 sửa mã | chưa soát | **KHOÁ** | — |
| 103 | `speckit-taskstoissues` | Quy trình đặc tả — xuất danh sách việc ra… | Đọc tệp danh sách việc của một tính năng rồi tạo mỗi đầu việc thành một phiếu việc trên kho mã từ xa. Trước khi tạo, nó lấy danh sách phiếu việc đã c… | R0 chỉ-đọc | chưa soát | **KHOÁ** | — |
| 104 | `ssot-docs-extraction` | Nguồn sự thật — trích đặc tả từ tài liệu … | Quy trình bảy bước để lấy đặc tả ĐÚNG và MỚI NHẤT trước khi viết mã: xác định đối tượng cần tra, quét thư mục tài liệu, so phiên bản để chọn bản mới … | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 105 | `ssot-verification-before-code` | Quy trình — cổng kiểm nguồn sự thật trước… | Bắt buộc chạy một quy trình 7 bước trước khi sửa mã liên quan lược đồ hoặc nghiệp vụ: đọc tài liệu nguồn mới nhất, trích nguyên văn yêu cầu, đối chiế… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 106 | `status-color-mapping` | Giao diện — bảng quy ước màu theo trạng t… | Đưa ra một bảng quy ước ánh xạ trạng thái nghiệp vụ sang màu (nháp/chờ = amber, đang chạy = blue, thành công/đã duyệt = emerald, huỷ/lỗi = rose, mặc … | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 107 | `status-workflow-actions` | Giao diện — nút hành động theo luồng trạn… | Chuẩn hoá cách hiện nút hành động trên các màn giao dịch (đơn hàng, báo giá, quan hệ khách hàng, sản xuất) theo trạng thái hiện tại của bản ghi: định… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 108 | `sticky-gradient-sheet-header` | Giao diện — tiêu đề chuyển sắc dính đỉnh … | Chỉ cách làm cho dải tiêu đề chuyển sắc trong bảng trượt (Sheet) nằm sát mép trên, không bị một dải trắng chen phía trên, và giữ dính đỉnh khi người … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 109 | `tab-separated-content-dialog` | Giao diện — hộp thoại nhiều thẻ, tách nội… | Chỉ cách tách nội dung riêng cho từng thẻ trong một hộp thoại thay vì dồn tất cả vào một màn: giữ một biến trạng thái cho thẻ đang mở, khai một bảng … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 110 | `table-column-visibility` | Giao diện — quản lý cột hiển thị của bảng… | Đưa ra quy trình ẩn, hiện, thêm hoặc xoá cột trong bảng danh sách kèm một ràng buộc cốt lõi: dữ liệu của cột bị ẩn PHẢI xuất hiện ở nơi khác (thường … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 111 | `tailwind-color-visibility` | Giao diện — tăng độ rõ của màu nhạt | Đưa ra một công thức cố định để làm nổi các phần tử màu pastel quá nhạt trên nền trắng: nâng nền một bậc sắc độ (từ mức 100 lên 200), nâng chữ một bậ… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 112 | `tailwind-v4-canonical-classes` | Giao diện — quy đổi cú pháp lớp Tailwind … | Cung cấp một bảng quy đổi cố định từ cú pháp lớp Tailwind kiểu cũ (giá trị tuỳ ý trong ngoặc vuông) sang cú pháp gọn của phiên bản 4, phủ 10 nhóm: bi… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 113 | `test-execution-ui` | Giao diện — bảng chạy thử và hiển thị kết… | Hướng dẫn dựng một khối giao diện cho phép bấm chạy từng bộ dữ liệu kiểm thử (test case) đang lưu trong cơ sở dữ liệu, rồi hiển thị ngay kết quả Đạt/… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 114 | `text-first-report` | Quản trị — khuôn mẫu báo cáo dạng văn bản… | Quy định khuôn báo cáo "ưu tiên chữ" gồm bốn phần bắt buộc — bằng chứng cấu trúc bảng dữ liệu, các quyết định về nguồn sự thật, danh sách thay đổi th… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 115 | `title-auto-case` | Giao diện — chuẩn viết hoa đầu chữ cho vă… | Quy định mọi văn bản hiển thị trong hệ thống phải được tự động chuyển sang dạng Viết Hoa Đầu Chữ tiếng Việt, không phụ thuộc cách người nhập gõ. Liệt… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | BO_SUNG |
| 116 | `transactional-page-redesign` | Giao diện — thiết kế lại trang tác nghiệp… | Định nghĩa quy trình thiết kế lại riêng cho các trang tác nghiệp — nơi thân trang là mặt bằng làm việc gồm hàng số liệu tổng hợp, thanh lọc, bảng/dan… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 117 | `ui-components-usage` | Giao diện — cẩm nang dùng thành phần dựng… | Tập hợp tám mục hướng dẫn dùng đúng các thành phần giao diện hay gây lỗi: bộ chọn ngày (tránh lệch múi giờ giữa máy chủ và trình duyệt), bảng dữ liệu… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 118 | `ui-highlight-styling` | Giao diện — khuôn tô nhấn khối thông tin … | Khuôn tô nhấn thị giác cho các khối quan trọng trong màn hình (ghi chú, danh sách hàng hoá, khối tổng kết, cảnh báo nhẹ) bằng bộ lớp nền mờ, viền và … | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 119 | `ui-section-reorder` | Bảo trì mã — quy trình đổi thứ tự khối tr… | Quy trình sáu bước để đổi thứ tự các thẻ, khối hoặc thẻ mục trong một biểu mẫu hay một trang: xác định thứ tự hiện tại kèm số dòng, chốt thứ tự mới v… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 120 | `ui-typography` | Giao diện — chuẩn phông chữ và độ nổi của… | Nêu ba điều: toàn hệ thống dùng một phông chữ duy nhất nạp qua cơ chế phông của khung nền; phần đầu bảng phải đậm và rõ để quét mắt nhanh; và độ đậm–… | R1 sửa mã | chưa soát | **CHỈ KHI PROMPT YÊU CẦU** | DA_GOP |
| 121 | `update-work-log` | Quản trị — ghi nhật ký công việc (hỗ trợ) | Hướng dẫn cách sửa tệp nhật ký công việc `WORK_LOG.md` một cách an toàn: chèn mục mới ở ĐẦU tệp, đọc lại tệp ngay trước khi ghi để không đè mất mục m… | R1 sửa mã | chưa soát | **KHOÁ** | — |
| 122 | `version-bump-on-feature` | Quản trị — quyết mức tăng phiên bản (hỗ t… | Kỹ năng phụ trợ giúp phân loại một thay đổi là `patch`, `minor` hay `major`: sửa chữ nghĩa/định dạng/lỗi nhỏ là patch; mở rộng tính năng đáng kể, thê… | R3 phát hành | chưa soát | **KHOÁ** | — |
| 123 | `versioning-auto-log` | Quản trị — truy vết thay đổi (phiên bản ·… | Kỹ năng CHÍNH của cụm đánh phiên bản. Nó buộc mỗi thay đổi có ý nghĩa phải được phân loại mức (patch/minor/major), xác định đủ các nơi phải ghi dấu v… | R3 phát hành | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 124 | `versioning-change-history` | Quản trị — từ điển trường lịch sử thay đổ… | Kỹ năng tra cứu nghĩa và cách điền các trường của một mục lịch sử thay đổi: Phạm vi (UI/Logic/Data/Docs/Governance), Đối tượng bị ảnh hưởng (mô-đun/t… | R3 phát hành | chưa soát | **KHOÁ** | — |
| 125 | `windows-dev-troubleshoot-quick` | Môi trường phát triển — chẩn đoán nhanh t… | Bản kiểm nhanh khoảng hai phút dành cho môi trường phát triển trên Windows: khi giao diện "bấm không phản hồi" hoặc biểu mẫu không gửi được, hãy soi … | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 126 | `windows-next-cache-stability` | Môi trường phát triển — ổn định bộ nhớ đệ… | Bản đầy đủ về cách xử lý khi bộ nhớ đệm dựng của Next.js trên Windows bị hỏng: liệt kê các triệu chứng nhận biết, các nguyên nhân phổ biến trên Windo… | R0 chỉ-đọc | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |
| 127 | `wizard-crud-bundle` | Giao diện + nghiệp vụ — biểu mẫu nhiều bư… | Chuẩn hoá cách dựng thêm/sửa/xoá cho các mô-đun có nhiều bảng con đi cùng nhau (ví dụ Khách hàng kèm Liên hệ kèm Địa chỉ) theo dạng biểu mẫu nhiều bư… | R1 sửa mã | chưa soát | **CHỈ ĐỌC THAM KHẢO** | ⛔ chọi |
| 128 | `workflow-status-transition` | Nghiệp vụ — vòng đời trạng thái chứng từ | Khuôn mẫu cài đặt vòng đời trạng thái cho các chứng từ (báo giá, đơn hàng, phiếu): khai kiểu dữ liệu cho tập trạng thái, khai bảng các bước chuyển HỢ… | R2 dữ liệu/quyền | chưa soát | **CHỈ GỌI ĐÍCH DANH** | — |

---

## 6. BẢNG C — KỸ NĂNG CÓ BẰNG CHỨNG TỪ SỔ YÊU CẦU CHỦ DỰ ÁN

> `Owner ưu tiên = true` **chỉ** khi tra được mục sổ đích danh. **Không suy diễn.** 10/128 kỹ năng đạt điều kiện này.

| Slug | Bằng chứng (mục sổ) | Rủi ro | Chế độ |
|---|---|---|---|
| `backup-before-db-mutation` | Sổ Yêu Cầu Owner mục #87 | R2 dữ liệu/quyền | CHỈ GỌI ĐÍCH DANH |
| `documentation-sync` | Sổ Yêu Cầu Owner mục #87 · Sổ Yêu Cầu Owner mục #131 | R3 phát hành | CHỈ GỌI ĐÍCH DANH |
| `master-list-data-table` | Sổ Yêu Cầu Owner mục #131 | R1 sửa mã | CHỈ ĐỌC THAM KHẢO |
| `mobile-responsive-ui-patterns` | Sổ Yêu Cầu Owner mục #128 | R1 sửa mã | CHỈ ĐỌC THAM KHẢO |
| `module-color-palette` | Sổ Yêu Cầu Owner mục #128 | R2 dữ liệu/quyền | CHỈ GỌI ĐÍCH DANH |
| `plan-execution-protocol` | Sổ Yêu Cầu Owner mục #87 | R3 phát hành | CHỈ GỌI ĐÍCH DANH |
| `public-report-sanitize-gate` | Sổ Yêu Cầu Owner mục #87 · Sổ Yêu Cầu Owner mục #131 | R3 phát hành | CHỈ GỌI ĐÍCH DANH |
| `repo-integrity-recovery-audit` | Sổ Yêu Cầu Owner mục #87 | R2 dữ liệu/quyền | CHỈ GỌI ĐÍCH DANH |
| `speckit-erp-ssot-adapter` | Sổ Yêu Cầu Owner mục #139 | R3 phát hành | KHOÁ |
| `status-color-mapping` | Sổ Yêu Cầu Owner mục #128 | R1 sửa mã | CHỈ KHI PROMPT YÊU CẦU |

> ⚠️ **Đọc bảng này cho đúng:** "có bằng chứng sổ" nghĩa là **tên kỹ năng có xuất hiện trong một mục sổ**, KHÔNG tự động nghĩa là Chủ dự án đã duyệt cho kỹ năng đó chạy tự động. 118 kỹ năng còn lại đặt `Owner ưu tiên = false` vì **không tra được bằng chứng** — đúng luật, không suy diễn.

---

## 7. BẢNG D — CHÙM CHỒNG CHÉO VÀ QUAN HỆ THAY THẾ

**D1. Chùm chồng chéo ngữ nghĩa** — vì **0/128** kỹ năng khai phạm vi đường dẫn và bố cục **phẳng một tầng**, mọi kỹ năng cạnh tranh nhau trên toàn kho.

| Chùm | Số kỹ năng | Mức | Vì sao nguy hiểm |
|---|---|---|---|
| Bộ công cụ đặc tả bên thứ ba | 11 | 🔴 CAO | Một yêu cầu thông thường khớp cùng lúc nhiều kỹ năng — mô hình phải tự chọn mà không có điều kiện loại trừ |
| Trang danh sách / bảng dữ liệu | 10 | 🔴 CAO | Một yêu cầu thông thường khớp cùng lúc nhiều kỹ năng — mô hình phải tự chọn mà không có điều kiện loại trừ |
| Danh sách chọn / lớp phủ | 8 | 🔴 CAO | Một yêu cầu thông thường khớp cùng lúc nhiều kỹ năng — mô hình phải tự chọn mà không có điều kiện loại trừ |
| Lược đồ · di trú · sao lưu | 8 | 🔴 CAO | Một yêu cầu thông thường khớp cùng lúc nhiều kỹ năng — mô hình phải tự chọn mà không có điều kiện loại trừ |
| Nguồn sự thật & đồng bộ tài liệu | 5 | 🟠 VỪA | Có cạnh tranh nhưng phạm vi hẹp hơn |
| Biểu mẫu & kiểm tra dữ liệu nhập | 5 | 🟠 VỪA | Có cạnh tranh nhưng phạm vi hẹp hơn |
| Panel chi tiết & hộp thoại | 4 | 🟠 VỪA | Có cạnh tranh nhưng phạm vi hẹp hơn |
| Ghi nhật ký & số phiên bản | 4 | 🟠 VỪA | Có cạnh tranh nhưng phạm vi hẹp hơn |

**Thành viên từng chùm:**

- **Bộ công cụ đặc tả bên thứ ba** (11): `speckit-analyze` · `speckit-checklist` · `speckit-clarify` · `speckit-constitution` · `speckit-converge` · `speckit-erp-ssot-adapter` · `speckit-implement` · `speckit-plan` · `speckit-specify` · `speckit-tasks` · `speckit-taskstoissues`
- **Trang danh sách / bảng dữ liệu** (10): `column-config-workflow` · `data-safety-legacy-columns` · `layout-component-redesign` · `master-list-data-table` · `master-list-page-template` · `premium-module-page-redesign` · `premium-table-styling` · `schema-column-drop-safe` · `table-column-visibility` · `transactional-page-redesign`
- **Danh sách chọn / lớp phủ** (8): `autocomplete-input-component` · `dropdown-display-name-only` · `popover-scroll-fix` · `portal-safe-dropdown-overlay` · `radix-overlay-guard` · `ref-dropdown-scope-filter` · `searchable-dropdown` · `searchable-multiselect-popover`
- **Lược đồ · di trú · sao lưu** (8): `backup-before-db-mutation` · `database-ops` · `in-memory-to-db-migration` · `mysql-schema-extraction` · `phased-migration-with-backfill` · `schema-column-drop-safe` · `schema-migration-safe` · `schema-visualization`
- **Nguồn sự thật & đồng bộ tài liệu** (5): `contract-preservation-check` · `documentation-sync` · `speckit-erp-ssot-adapter` · `ssot-docs-extraction` · `ssot-verification-before-code`
- **Biểu mẫu & kiểm tra dữ liệu nhập** (5): `currency-input-preview` · `form-field-validation` · `form-helper-text-pattern` · `input-validation-on-blur` · `number-input-live-preview`
- **Panel chi tiết & hộp thoại** (4): `detail-panel-layout` · `detail-panel-toggle` · `sticky-gradient-sheet-header` · `tab-separated-content-dialog`
- **Ghi nhật ký & số phiên bản** (4): `update-work-log` · `version-bump-on-feature` · `versioning-auto-log` · `versioning-change-history`

**D2. Kỹ năng ĐÃ BỊ THAY THẾ mà vẫn phát hiện được đầy đủ** — không cơ chế nào ngăn công cụ nạp chúng.

| Kỹ năng cũ | Bị thay bởi | Lớp rủi ro | Chế độ | Còn phát hiện được? |
|---|---|---|---|---|
| `update-work-log` | `versioning-auto-log` | R1 sửa mã | **KHOÁ** | ✅ CÓ — đầy đủ |
| `version-bump-on-feature` | `versioning-auto-log` | R3 phát hành | **KHOÁ** | ✅ CÓ — đầy đủ |
| `versioning-change-history` | `versioning-auto-log` | R3 phát hành | **KHOÁ** | ✅ CÓ — đầy đủ |

> Cả chùm này (3 bản cũ + bản thay) đều thuộc lớp **R3 phát hành**, nằm cùng chỗ, mô tả gần nhau. Kích hoạt nhầm ở đây không chỉ sai cách làm mà chạm thẳng lớp rủi ro cao nhất.

---

## 8. BẢNG E — CHỌI CHUẨN GIAO DIỆN VÀ CHUỖI BỊ CẤM

**36** kỹ năng mang phán quyết **chọi chuẩn**. Theo §L2: **chỉ đọc để hiểu bối cảnh, CẤM chép giá trị**.
Trong đó **11** kỹ năng còn chứa chuỗi mà chuẩn CẤM — cổng đối chiếu ghi rõ từng chuỗi kèm số dòng.

| Slug | Điểm chọi | Có chuỗi CẤM? | Rủi ro | Chế độ |
|---|---|---|---|---|
| `master-list-data-table` | 10 | ⛔ **CÓ** — ["rounded-xl/2xl@323", "max-h-vh@25"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `test-execution-ui` | 8 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `implement-g2-ux` | 7 | ⛔ **CÓ** — ["rounded-xl/2xl@306"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `detail-panel-toggle` | 6 | ⛔ **CÓ** — ["rounded-xl/2xl@93"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `cross-module-consistency` | 5 | ⛔ **CÓ** — ["rounded-xl/2xl@238"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `sticky-gradient-sheet-header` | 5 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `tailwind-color-visibility` | 5 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `header-inline-badge` | 4 | ⛔ **CÓ** — ["text-3xl@16"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `inline-filter-bar-layout` | 4 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `mobile-responsive-ui-patterns` | 4 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `status-workflow-actions` | 4 | ⛔ **CÓ** — ["rounded-xl/2xl@143"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `ui-highlight-styling` | 4 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `deventry-link-management` | 3 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `fk-snapshot-field-pattern` | 3 | — | R2 dữ liệu/quyền | **KHOÁ** |
| `input-validation-on-blur` | 3 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `number-input-live-preview` | 3 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `ref-dropdown-scope-filter` | 3 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `reusable-component-from-demo` | 3 | ⛔ **CÓ** — ["rounded-xl/2xl@29"] | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `scaffold-module` | 3 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |
| `confirm-action-dialog` | 2 | — | R1 sửa mã | **CHỈ ĐỌC THAM KHẢO** |


_16 kỹ năng chọi chuẩn còn lại (điểm chọi thấp hơn) xem đầy đủ trong artefact JSON._

> ⚠️ **Đây đúng cái bẫy mà §L2 đã dựng cổng để chặn:** một phiên **làm đúng luật từng bước** — tra sổ đăng ký, khớp điều kiện, nạp kỹ năng — vẫn có thể bị dẫn tới kỹ năng dạy đúng thứ chuẩn CẤM. Bật tự kích hoạt cho nhóm này thì **càng tuân thủ càng code sai**. Cổng `test:ui-skill-conflict` hiện **XANH**, nghĩa là hai sổ đang khớp nhãn — nhưng nó **không** ngăn việc NẠP, nó chỉ ngăn hai sổ lệch nhãn.

**Tham chiếu chết và tài sản hứa mà rỗng:**

- **17** kỹ năng trỏ tới đường dẫn THẬT đã chết (đã tách khỏi chỗ trống khuôn mẫu cố ý — chỗ trống khuôn mẫu **không** tính là lỗi).
- **2** kỹ năng hứa có thư mục khuôn mẫu kèm theo nhưng thư mục **rỗng**: `implement-g2-ux` · `scaffold-module`.
- **6** kỹ năng có câu chữ tự nhận quyền, trái §L1: `database-ops` · `deploy-local-github-vps` · `documentation-sync` · `duplicate-file-1-resolution` · `speckit-erp-ssot-adapter` · `text-first-report`.

---

## 9. BẢNG F — KHOẢNG TRỐNG NGUỒN GỐC (BÊN THỨ BA)

**10** kỹ năng thuộc bộ công cụ đặc tả của bên thứ ba, **cố ý không được Git theo dõi** theo quyết định của Chủ dự án ngày 20/08/2026 (ghi đích danh trong tệp cấu hình bỏ qua của Git — cách làm này **đúng**, đã sửa một vết cũ dùng mẫu quét bừa).

| Slug | Git theo dõi | Khai phiên bản | Khai giấy phép/bản gốc | Chứng minh được nguồn gốc? | Chế độ |
|---|---|---|---|---|---|
| `speckit-analyze` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-checklist` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-clarify` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-constitution` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-converge` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-implement` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-plan` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-specify` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-tasks` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |
| `speckit-taskstoissues` | ❌ KHÔNG | ❌ KHÔNG | ❌ KHÔNG | ⛔ **KHÔNG** | **KHOÁ** |

**Hệ quả:** không thể cấp `AUTO_SAFE` cho nhóm này vì tiêu chí *"nguồn và mã băm nội dung chính xác"* **không đạt được** — không có mã băm trong Git để đối chiếu, không có số hiệu bản gốc để tra. Quy trình cách ly kỹ năng bên thứ ba (§L7) mới đi tới bước **quét**, chưa tới bước **thử trong bóng tối** và **phê duyệt**.

**Ánh xạ Matt Pocock:** ⚠️ **`NOT_CHECKED` cho toàn bộ 128 kỹ năng.** Bộ kỹ năng Matt **KHÔNG được cài** ở máy này (đã kiểm cả ba đường dẫn kỹ năng, đều không tồn tại), và ảnh chụp tham chiếu mà §4 nêu **không giải được** trong kho này. Theo đúng chỉ thị *"không tuyên bố Matt skills đã có"* — audit này **không** khẳng định bất kỳ quan hệ nào.

---

## 10. BẢNG G — KHUYẾN NGHỊ ĐỊNH TUYẾN PROMPT

> Dành cho TanPhatAI khi viết prompt: kỹ năng nào được **nhắc tới ngay**, kỹ năng nào **phải nêu đích danh**, kỹ năng nào **cấm**.

| Chế độ | Số kỹ năng | TanPhatAI được làm gì | Điều kiện tiên quyết |
|---|---|---|---|
| **CHỈ ĐỌC THAM KHẢO** | 33 | Được trích **bối cảnh**, **CẤM chép giá trị** vào prompt hay mã | Chuẩn giao diện là nguồn thắng, không phải kỹ năng |
| **KHOÁ** | 18 | **KHÔNG** nhắc tới như chỉ dẫn thi hành | Cần Chủ dự án mở khoá từng trường hợp |
| **CHỈ GỌI ĐÍCH DANH** | 42 | Chỉ đưa vào prompt khi **nêu rõ tên** và có lý do; không để tự chọn | Thuộc R2/R3 — chạm dữ liệu, quyền hoặc phát hành |
| **CHỈ KHI PROMPT YÊU CẦU** | 35 | Được nêu như ứng viên, nhưng prompt phải nói rõ mới nạp | Thuộc R1 — sửa mã; vẫn cần soát nội dung trước |
| **TỰ KÍCH HOẠT AN TOÀN** | **0** | ⛔ **KHÔNG CÓ kỹ năng nào** | Cần ít nhất một kỹ năng nhãn "còn hiệu lực" — hiện là 0 |

**Ánh xạ sang thư viện Notion (KN) — trạng thái `PROPOSED`, TanPhatAI phải tự kiểm, không chép mù:**

| Nhóm kỹ năng trong kho | Số kỹ năng | KN đề xuất |
|---|---|---|
| Giao diện / bố cục | 61 | KN21 — Thiết kế Giao diện ERP |
| Lược đồ · cơ sở dữ liệu | 15 | KN09 — An toàn CSDL |
| Bộ công cụ đặc tả bên thứ ba | 11 | KHÔNG có tương đương |
| Quản trị | 9 | KN17 · KN01 · KN03 |
| Triển khai / môi trường | 7 | KN23* — Git / Phát hành |
| Số phiên bản | 6 | KN23* — Git / Phát hành |
| Kiểm thử | 4 | KN19 / KN20 |
| Báo cáo công khai | 1 | KN22 — Bảo mật / Bí mật |
| Tính giá | 1 | `NOT_CHECKED` |
| Chưa suy ra được nhóm | 13 | `NOT_CHECKED` — sổ đăng ký ghi cần Chủ dự án bổ sung |

> Căn cứ: bản đồ hai thư viện phía Notion (23/08/2026), nay đã mang trạng thái **lịch sử**. Vì vậy toàn bộ ánh xạ trên là **ứng viên**, không phải quan hệ đã chốt — đúng như §4 yêu cầu.

---

## 11. BẢNG H — VIỆC CẦN CHỦ DỰ ÁN QUYẾT

| # | Việc | Vì sao cần Chủ dự án | Đang chặn gì |
|---|---|---|---|
| H-1 | Chốt phạm vi lượt **soát hiệu lực nội dung** để có kỹ năng "còn hiệu lực" đầu tiên | Khoá "cấm tự gán hiệu lực hàng loạt" là quyết định của Chủ dự án 23/08/2026 — Agent không tự mở | **Chặn toàn bộ** đường tiến tới tự kích hoạt |
| H-2 | Xử 36 kỹ năng chọi chuẩn giao diện: sửa nội dung, gộp vào chuẩn, hay khoá hẳn | Đụng chuẩn giao diện — thuộc thẩm quyền Chủ dự án | Nhóm lớn nhất của thư viện chưa dùng an toàn được |
| H-3 | Xử 3 kỹ năng đã bị thay thế mà vẫn phát hiện được | Xoá/giữ kỹ năng là quyết định vòng đời | Rủi ro kích hoạt nhầm ở lớp phát hành |
| H-4 | Hoàn tất cách ly 10 kỹ năng bên thứ ba, hay khoá vĩnh viễn | Liên quan quyết định 20/08 về việc giữ chúng ngoài Git | Không cấp được chế độ nào cao hơn KHOÁ |
| H-5 | Có viết lại phần mô tả theo khuôn "dùng khi / KHÔNG dùng khi" hay không | 100/128 mô tả hiện không nêu điều kiện dùng — đây là thứ THỰC SỰ điều khiển tự kích hoạt | Tự kích hoạt không đáng tin dù có mở khoá |
| H-6 | Có dựng kênh đo phía Cursor hay không | Cần quyết định về công cụ và cách đo | Câu hỏi quan trọng nhất còn lại vẫn `NOT_CHECKED` |
| H-7 | Có đưa ba cổng kỹ năng vào móc trước-commit hay không | Đổi lấy việc commit chậm hơn — đã từng cân nhắc có chủ đích | Toàn vẹn sổ đăng ký không được canh ở cửa commit |
| H-8 | Xác nhận cách xử lý điểm chốt kho riêng trong artefact công khai | Xung đột giữa yêu cầu §4 và luật công khai của dự án — xem mục 12 | Đã tự xử theo hướng an toàn, cần Chủ dự án xác nhận |

---

## 12. HAI SAI LỆCH SO VỚI §4 — KHAI BÁO MINH BẠCH

**12.1 — Điểm chốt kho mã nguồn riêng bị CHE trong artefact công khai.**
§4 bắt buộc trường `private_source_checkpoint`. Nhưng luật công khai của chính dự án xếp **"vân tay kho riêng"** vào nhóm CHẶN, và cổng máy chặn đúng dạng chuỗi đó. Hai yêu cầu **xung đột thật**.
**Cách xử:** giữ trường (đúng schema), đặt giá trị `REDACTED_PUBLIC_SAFETY`, kèm ghi chú giải thích; **giá trị đầy đủ giao TRỰC TIẾP cho Chủ dự án và TanPhatAI** qua kênh riêng. Mục đích đối chiếu vẫn đạt: artefact vẫn mang **mã băm nội dung sổ đăng ký** và **mã băm bộ luật** — cả hai là hàm một chiều, không phục hồi được nội dung, và không phải vân tay trạng thái kho.
**Cần Chủ dự án xác nhận** cách xử này (mục H-8).

**12.2 — Ma trận thử kích hoạt vẫn `NOT_CHECKED`.**
Trường `test_state` của **toàn bộ 128** kỹ năng là `NOT_CHECKED`. Audit tiền nhiệm đã cố chạy và **thất bại vì lỗi hạ tầng nhà cung cấp**, không phải vì kết quả đo. Lượt này §4 **không** yêu cầu chạy lại phép thử động, nên audit giữ nguyên trạng thái đó thay vì suy ra kết quả. **Không được đọc `NOT_CHECKED` thành "đã đạt" hay "đã hỏng".**

---

## 13. MA TRẬN HOÀN THÀNH HẠNG MỤC

| # | Hạng mục §4 | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | Danh mục máy đọc (JSON) | ✅ `READY_CANDIDATE` | 128 bản ghi · đúng 27 khoá/bản ghi · parse được |
| 2 | Hồ sơ người đọc (Markdown) | ✅ `READY_CANDIDATE` | Chính tệp này · bảng A–H đầy đủ |
| 3 | COVERAGE A — thư viện ERP | ✅ `AUDITED_ONLY` | 128/128, delta = 0 |
| 4 | COVERAGE B — bề mặt công cụ | ✅ `MEASURED_ONLY` | Claude Code có bằng chứng trực tiếp; Cursor `NOT_CHECKED` |
| 5 | COVERAGE C — một bản ghi mỗi kỹ năng | ✅ `AUDITED_ONLY` | Đủ 24 mục nội dung cho từng kỹ năng |
| 6 | Tóm tắt điều hành + đối chiếu số | ✅ `READY_CANDIDATE` | Mục 1 và mục 2 |
| 7 | Checksum artefact | ✅ `READY_CANDIDATE` | Tệp `.sha256` kèm theo |
| 8 | Cổng an toàn công khai | ✅ `MEASURED_ONLY` | Kết quả ghi ở mục 14 |
| 9 | Commit và URL báo cáo | ✅ `READY_CANDIDATE` | Ghi ở mục 14 |
| 10 | Ánh xạ Notion | ⚠️ `AUDITED_ONLY` | Toàn bộ ở trạng thái **ứng viên** — TanPhatAI phải tự kiểm |
| 11 | Ánh xạ Matt Pocock | ⚠️ `NOT_APPLICABLE` | **`NOT_CHECKED`** — không cài ở máy này, không tuyên bố gì |
| 12 | Thử kích hoạt động | ⛔ `NOT_STARTED` | `NOT_CHECKED` — §4 không yêu cầu lượt này |
| 13 | Nhập danh mục Notion | ⛔ `NOT_STARTED` | Việc của TanPhatAI, không phải của Agent IDE |
| 14 | Bật tự kích hoạt | ⛔ `BLOCKED` | **0 kỹ năng đủ điều kiện** — giữ nguyên khoá |

> **Không hạng mục nào được ghi ĐÃ THI CÔNG · ĐÃ TRIỂN KHAI · ĐÃ XÁC MINH KHI CHẠY** — đúng giới hạn của một lượt chỉ-đọc.

---

## 14. LOCK-IN · OPEN ITEMS · NEXT ACTION

### 🔒 LOCK-IN

1. **128/128 kỹ năng đã có hồ sơ đầy đủ**, mỗi kỹ năng đúng một bản ghi chuẩn, không trùng slug, artefact parse được và có checksum.
2. **0 kỹ năng đủ điều kiện tự kích hoạt** — trượt ngay tiêu chí "còn hiệu lực" vì thư viện có **0** kỹ năng mang nhãn đó.
3. **Chênh lệch số lượng so với kỳ vọng §4 = 0.**
4. **Claude Code không phát hiện thư viện ERP** — tái xác nhận bằng quan sát trực tiếp trong chính lượt này.
5. **0 kỹ năng có tệp thi hành** ⇒ không có mặt tấn công dạng chạy mã.
6. **8/8 điều kiện VALIDATION của §4 đều ĐẠT.**
7. **Không sửa gì**: không đụng mã nguồn riêng, kỹ năng, sổ đăng ký, tệp ghi đè, luật, móc, quyền, cổng hay cơ sở dữ liệu. Không commit/push vào kho mã nguồn riêng.

### ❓ OPEN ITEMS

Tám mục ở **Bảng H**. Trong đó **H-1 là cửa chặn số một** — mọi việc khác vô nghĩa cho tới khi nó được mở.

### ➡️ NEXT ACTION — ĐÚNG MỘT VIỆC

> **TanPhatAI nhận artefact JSON, kiểm theo tám điều kiện ở §5 của gói audit, rồi nhập từng bản ghi vào danh mục Notion — và KHÔNG chép trạng thái `Owner ưu tiên` hay ánh xạ KN một cách mù quáng, mà phải tự kiểm bằng chứng.**

Đây đúng bước kế tiếp mà chính §7 của gói audit đã chỉ định. Việc mở khoá hiệu lực nội dung (H-1) là **quyết định của Chủ dự án**, xảy ra **sau** khi danh mục đã vào Notion.

---

## 15. KHỐI BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Tìm và đọc nguyên văn prompt canonical §4 của gói audit trên Notion (không tự chế nội dung).
   - Tái xác minh READ_FIRST: năm bản luật trùng khớp (đo 2 lần), sổ đăng ký, sổ ghi đè hiệu lực,
     sổ đăng ký đường dẫn, sổ nợ kỹ thuật, kế hoạch/gói việc hiện hành, ba cổng kỹ năng (đều XANH).
   - COVERAGE A: đo lại số kỹ năng thật = 128; delta so với kỳ vọng = 0.
   - COVERAGE B: ghi bề mặt hai công cụ đã cài, kèm cách lấy bằng chứng và giới hạn.
   - COVERAGE C: lập hồ sơ 128/128 kỹ năng, mỗi kỹ năng đủ 24 mục nội dung.
   - Kết xuất artefact máy đọc đúng hợp đồng §4 (27 khoá/bản ghi) + tệp checksum.
   - Chạy 8 điều kiện VALIDATION của §4: ĐẠT 8/8.
   - Tự kiểm an toàn công khai trên artefact JSON theo 7 luật (cổng máy chỉ quét tệp .md).
   - Khai báo minh bạch 2 sai lệch so với §4 (mục 12).

2. PHẠM VI
   ĐỤNG      : KHÔNG tệp nào trong kho mã nguồn riêng. Chỉ thêm 3 tệp vào kho báo cáo công khai
               (báo cáo .md, artefact .json, tệp checksum .sha256). Tệp tạm ở vùng nháp của phiên.
   KHÔNG ĐỤNG: mã nguồn · CSDL · triển khai · số phiên bản · kỹ năng · sổ đăng ký · tệp ghi đè ·
               luật · móc · quyền · cổng. KHÔNG kích hoạt kỹ năng nào. KHÔNG cài/cập nhật gì.
               KHÔNG thi hành chỉ dẫn bên trong bất kỳ kỹ năng nào.

3. BẰNG CHỨNG
   - Đối chiếu băm năm bản luật -> một giá trị duy nhất, đo 2 lần -> FILE_PROVEN
   - Đếm thư mục/tệp/mục sổ đăng ký -> 128 = 128 = 128 -> FILE_PROVEN
   - test:skills-registry · test:skill-content-status · test:ui-skill-conflict -> PASS -> CODE_PROVEN
   - Danh sách kỹ năng phiên được cấp -> 17, không kỹ năng ERP nào -> RUNTIME_PROVEN
   - Đọc toàn văn 128 tệp kỹ năng qua 19 tiến trình phụ, 0 lỗi -> FILE_PROVEN
   - JSON parse + 8 điều kiện VALIDATION -> ĐẠT 8/8 -> CODE_PROVEN
   - Máy vận hành -> KHÔNG ĐO -> UNVERIFIED (lượt này không được cấp kênh, và §4 không yêu cầu)

4. GHI SỔ YÊU CẦU OWNER
   [x] CHƯA — lý do: lượt này KHÔNG phát sinh quyết định/chỉnh hướng MỚI nào của Chủ dự án.
       Chỉ thị là một yêu cầu THI HÀNH, và FORBIDDEN của §4 cấm sửa mọi tệp trong kho riêng,
       trong đó có Sổ Yêu Cầu Owner. Tám mục Bảng H là việc CẦN Chủ dự án quyết; khi có trả lời,
       MỖI chỉ thị sẽ là MỘT mục sổ theo GOV-NOTION-HANDOFF-001.

5. PUSH BÁO CÁO CÔNG KHAI
   [x] Trạng thái ghi ở dòng cuối tệp này.

6. CÒN SÓT / CHƯA LÀM
   - Thử kích hoạt động: NOT_CHECKED cho cả 128 kỹ năng (§4 không yêu cầu lượt này).
   - Phát hiện bản địa phía Cursor: NOT_CHECKED — chưa có kênh đo.
   - Ánh xạ Matt Pocock: NOT_CHECKED — không cài ở máy này.
   - Ánh xạ Notion: mới ở mức ỨNG VIÊN, chưa xác nhận.
   - Chưa soát hiệu lực nội dung của bất kỳ kỹ năng nào — bị FORBIDDEN của §4 cấm.

7. ĐANG CHỜ OWNER
   - Tám mục ở Bảng H, trong đó H-1 chặn toàn bộ đường tiến tới tự kích hoạt.
   - H-8: xác nhận cách xử điểm chốt kho riêng trong artefact công khai (mục 12.1).

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   TanPhatAI nhận artefact JSON, kiểm theo tám điều kiện §5, rồi nhập từng bản ghi vào danh mục
   Notion — không chép mù trạng thái Owner ưu tiên và ánh xạ KN.

9. CHƯA XÁC MINH ĐƯỢC
   - Cursor có tự kích hoạt kỹ năng ERP không — cần một phiên chạy TRONG Cursor tự báo danh sách
     kỹ năng nó được cấp.
   - Việc chọn giữa các kỹ năng cạnh tranh — cần phép thử động, và chỉ có nghĩa trên công cụ
     THẬT SỰ phát hiện thư viện.
   - 10 kỹ năng bên thứ ba có khớp bản gốc thượng nguồn không — không có phiên bản,
     không có mã băm trong Git để đối chiếu.
   - Nội dung từng kỹ năng còn ĐÚNG với hệ thống hiện tại không — đó là việc soát hiệu lực (H-1).

10. TRẠNG THÁI CHUNG
   [x] PASS — đủ bằng chứng cho phạm vi §4 giao. 8/8 điều kiện VALIDATION đạt.
       Các mục NOT_CHECKED đều nằm NGOÀI phạm vi lượt này và đã khai rõ, không phải thiếu sót.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén/gián đoạn: CÓ (phiên trước đã gián đoạn; lượt này nối tiếp).
   Tham chiếu ĐÃ ĐỌC LẠI TỪ ĐĨA: năm bản luật (đo lại băm) · sổ đăng ký kỹ năng · sổ ghi đè hiệu
   lực nội dung · sổ đăng ký đường dẫn · sổ nợ kỹ thuật · Sổ Yêu Cầu Owner · tệp móc trước-commit ·
   hai tệp luật riêng của Cursor · toàn văn 128 tệp kỹ năng · prompt canonical §4 trên Notion ·
   bản đồ hai thư viện kỹ năng trên Notion.
   Không kết luận nào dựa trên trí nhớ trước gián đoạn.
═══════════════════════════════════════════
```

---

## 16. BÀN GIAO CHO TANPHATAI

**Ánh xạ trường sang danh mục Notion** — tên trường trong artefact JSON ↔ tên cột trong §2 của gói audit:

| Cột Notion (§2) | Khoá JSON | Kiểu |
|---|---|---|
| Tên skill | `title` | chuỗi |
| Mã / Slug | `slug` | chuỗi — **định danh chuẩn** |
| Hệ skill | `skill_system` | chuỗi |
| Actor sử dụng | `actor` | chuỗi |
| Nguồn đề xuất | `proposal_sources` | **mảng** |
| Nhóm công dụng | `function_groups` | **mảng** |
| Chức năng / Công dụng | `function_summary_vi` | chuỗi |
| Mục đích sử dụng | `purpose_vi` | chuỗi |
| Khi nào dùng | `use_when_vi` | chuỗi |
| Không dùng khi | `do_not_use_when_vi` | chuỗi |
| Đầu vào / Đầu ra | `inputs_outputs_vi` | chuỗi |
| Risk class | `risk_class` | chuỗi |
| Content state | `content_state` | chuỗi |
| Invocation mode | `invocation_mode` | chuỗi |
| Test state | `test_state` | chuỗi |
| Client hỗ trợ | `supported_clients` | **mảng** |
| Native discovery | `native_discovery` | chuỗi |
| Path / URL nguồn | `source_reference` | chuỗi — đường dẫn **tương đối** |
| Version / Hash | `version_or_hash` | chuỗi |
| SSOT verdict | `ssot_verdict` | chuỗi |
| Owner ưu tiên | `owner_priority` | **đúng/sai** |
| Audit report | `audit_report_url` | chuỗi |
| Lần audit gần nhất | `last_audited` | ngày |
| Skill liên quan | `related_skills` | **mảng** |
| Ghi chú | `notes_vi` | chuỗi |
| (nội dung trang chi tiết) | `details_markdown` | chuỗi Markdown |

> Cột **Nguồn đề xuất** và **Owner evidence**: artefact dùng thêm khoá `owner_evidence_reference`. Cột `Owner ưu tiên` **chỉ** đặt đúng khi tra được mục sổ đích danh.

**Bốn điều TanPhatAI phải TỰ KIỂM, không chép mù:**

1. **Ánh xạ KN** — toàn bộ ở trạng thái **ứng viên**, suy từ nhóm công dụng, chưa đối chiếu nội dung từng kỹ năng Notion.
2. **`Owner ưu tiên`** — `true` chỉ nghĩa là *tên kỹ năng xuất hiện trong một mục sổ*, **không** nghĩa là Chủ dự án đã duyệt cho chạy.
3. **`content_state`** — toàn bộ là *chưa soát* (trừ 1 kỹ năng ngủ đông). **Không** được diễn giải thành *còn hiệu lực*.
4. **`test_state`** — toàn bộ `NOT_CHECKED`. **Không** đọc thành đạt hay hỏng.

**Xác nhận:** lượt này **KHÔNG** sửa mã nguồn riêng, kỹ năng, sổ đăng ký, tệp ghi đè, luật, móc, quyền, cổng hay cơ sở dữ liệu; **KHÔNG** commit/push vào kho mã nguồn riêng; **KHÔNG** kích hoạt kỹ năng nào; **KHÔNG** cài hay cập nhật gì.

---

_Artefact máy đọc: `SKILL-CATALOG-ERP-20260904.json` · checksum SHA-256 ghi trong `SKILL-CATALOG-ERP-20260904.sha256`._
