# 📜 SNAPSHOT TÀI LIỆU QUẢN TRỊ (GOVERNANCE) — 20/08/2026

> 🕰️ **BẢN CHỤP LỊCH SỬ — DOC 2.4. ĐÃ CÓ BẢN MỚI HƠN.**
>
> Bộ luật trong kho riêng tư đã được mở rộng lên **Doc 2.7** trong **Pha B ngày 23/08/2026**
> (thêm luật tách trục *sức khoẻ cấu trúc* khỏi *hiệu lực nội dung* cho kỹ năng).
> Xem [`PHA-B-HOAN-TAT-20260823.md`](../PHA-B-HOAN-TAT-20260823.md) và
> [`PHA-C-DONG-SO-CONTEXT7-TRIGGER-CHONG-TRUNG-MA-20260824.md`](../PHA-C-DONG-SO-CONTEXT7-TRIGGER-CHONG-TRUNG-MA-20260824.md).
>
> ⚠️ **KHÔNG dùng `skills.yml` trong bản chụp này làm danh mục kỹ năng đang chạy.**
> Danh mục thật đã đổi sau bản chụp: mọi mục nay mang thêm trường **hiệu lực nội dung**,
> và các lệnh không chạy được đã bị loại khỏi trường điều kiện kiểm.
>
> Bản chụp này **giữ nguyên vẹn, không sửa** — theo `GOV-EDIT-PRESERVE-001`.
> Nó là **bằng chứng lịch sử**, không phải chuẩn thi hành hiện hành.

> ⚠️ **BẢN CHỤP MỘT THỜI ĐIỂM (20/08/2026) — HÃY KIỂM CÒN HẠN HAY KHÔNG.**
> Đây là mục lục bản công bố, **chỉ tồn tại ở bản công bố** (không có bản gốc tương ứng trong kho riêng tư để đối chiếu mã băm).
> **Cách kiểm:** đối chiếu ngày chụp với bản công bố mới nhất trong kho báo cáo; có bản mới hơn → bản này đã **LỖI THỜI**.

> **Bản này thay `LUAT-QUAN-TRI-20260819/`.** Bản cũ **GIỮ NGUYÊN, không xoá** — theo `GOV-EDIT-PRESERVE-001` (dòng cũ không bị ghi đè im lặng).

> **Bản công bố công khai** cho Notion + công cụ AI + đối tác đọc, phục vụ minh bạch cách dự án ERP Tân Phát quản trị luật/quy ước.
> ⚠️ Đây là **tài liệu luật/quy ước**, KHÔNG phải mã nguồn. KHÔNG chứa credential/schema/dữ liệu nhạy cảm/đường dẫn nhạy cảm.

## Có gì mới so với bản 19/08

| | Thay đổi |
|---|---|
| **Doc Version** | `2.3` → **`2.4`** |
| **File mới** | **`skills.yml`** — danh mục 128 skill, lần đầu được công bố |
| **Luật sửa** | §L2 + §L6 nay trỏ tới **danh mục skill có thật**; §V thêm một dòng chỉ mục |
| **Cổng mới** | `test:skills-registry` (4 điều kiện FAIL, đã kiểm ngược 4/4 đạt) |
| **Cổng vá** | `ref-exists-gate` nay kiểm cả **tham chiếu khái niệm**, không chỉ đường dẫn |

**Vấn đề gốc được vá:** bộ luật §L2 bắt buộc luồng *"tra danh mục skill trước khi chọn skill"*, nhưng **danh mục đó không hề tồn tại**. Cổng kiểm tham chiếu không phát hiện được, vì §L2 viết dưới dạng **sơ đồ luồng** chứ không phải một đường dẫn file. Kết quả: toàn bộ thư viện skill nằm ngoài tầm với của bộ luật.

## Nội dung

| File | Là gì |
|---|---|
| `CLAUDE.md` | **Một bản replica** của bộ luật quản trị. Bộ luật gồm **5 file byte-identical** (`.cursorrules` · `.antigravityrules` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md`); công bố 1 bản là đủ. **sha256 bản gốc = `0a1cb3bf…`** · **parity 5/5 OK** (`npm run check:governance`). |
| **`skills.yml`** | **MỚI.** Danh mục tra cứu skill theo triệu chứng — 128 mục, mỗi mục đủ 8 trường mà §L6 quy định (symptom · aliases · failure family · domain · operation · quality gate · previous result · trap_seen_before). **sha256 bản gốc = `d0d8d392…`** |
| `UI-STANDARD.md` | Chuẩn giao diện DUY NHẤT của dự án (SSOT UI) — bảng bo góc, màu, component, layout list/detail. |
| `ui-standard-sources.md` | Registry nguồn của chuẩn UI (rút từ trang nào, class/hex thật). |
| `secret-exposure-status-CONG-KHAI.md` | Trạng thái rà + gỡ thông tin nhạy cảm — **BẢN ĐÃ CHE** (không lộ file:dòng, không lộ mã commit, không lộ giá trị). |
| `acceptance-template.md` | Mẫu bảng tiêu chí nghiệm thu (acceptance) cho các gói việc. |

## Cách đọc `skills.yml`

Hai nhãn dễ hiểu nhầm — đọc kỹ trước khi kết luận:

- **`NEEDS_OWNER_INPUT`** = giá trị **không rút được từ nội dung thật** của skill đó. Đây là nhãn **trung thực bắt buộc**; bộ sinh **bị cấm bịa** để lấp chỗ trống. Hiện còn **97/128** mục thiếu `negative_trigger` và **13** mục chưa suy ra được `domain` — đó là khoảng trống đã biết, không phải lỗi.
- **`NO_USAGE_EVIDENCE`** = **không có dấu vết văn bản**, **KHÔNG** đồng nghĩa "chưa bao giờ dùng". Kho không ghi log kích hoạt skill, nên con số này **không được** dùng làm căn cứ đề xuất xoá skill.

Mọi giá trị khác đều truy được về **một dòng văn bản có thật** trong `SKILL.md`, hoặc về **một phép đếm có thật** (số file được git theo dõi, số lần được nhắc trong nhật ký làm việc và tài liệu).

## Ghi chú an toàn

- Toàn bộ file trong thư mục này đã qua **`npm run test:secret-scan` (2 chế độ: theo MẪU + theo GIÁ TRỊ)** và **`npm run test:pii-scan`** → **0 vi phạm** trước khi công bố.
- `skills.yml` đã qua thêm cổng lọc public-safe theo `GOV-PUBLIC-SAFE-001` §J1 — **5/5 nhóm chặn = 0 hit**: credential/hash · dữ liệu cá nhân · dữ liệu dính tiền · dump thô · định danh hạ tầng máy chủ.
- Bản `secret-exposure-status-CONG-KHAI.md` đã **che** mọi vị trí file:dòng + mã commit của registry nội bộ tương ứng (bản đầy đủ ở kho riêng tư).
- KHÔNG đăng: mã nguồn (`src/`), script, file kiểm thử, schema, dữ liệu thật.

## Đính chính bản 19/08

README của bản `LUAT-QUAN-TRI-20260819/` ghi `sha256 (8 ký tự đầu) = 05a8ff93` cho `CLAUDE.md` — **con số đó lỗi thời**. Mã đúng tại thời điểm chụp 19/08 là `ad2a510b…`, và **banner bên trong chính file `CLAUDE.md` của bản đó đã ghi đúng** — chỉ dòng trong README sai. Bản 19/08 vẫn giữ nguyên, không sửa đè; đính chính ghi tại đây.
