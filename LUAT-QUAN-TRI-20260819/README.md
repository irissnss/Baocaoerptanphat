# 📜 SNAPSHOT TÀI LIỆU QUẢN TRỊ (GOVERNANCE) — 19/08/2026


> ⚠️ **BẢN CHỤP MỘT THỜI ĐIỂM (19/08/2026) — HÃY KIỂM CÒN HẠN HAY KHÔNG.**
> Đây là mục lục bản công bố, **chỉ tồn tại ở bản công bố** (không có bản gốc tương ứng trong kho riêng tư để đối chiếu mã băm).
> **Cách kiểm:** đối chiếu ngày chụp với bản công bố mới nhất trong kho báo cáo; có bản mới hơn → bản này đã **LỖI THỜI**.

> **Bản công bố công khai** cho Notion + công cụ AI + đối tác đọc, phục vụ minh bạch cách dự án ERP Tân Phát quản trị luật/quy ước.
> ⚠️ Đây là **tài liệu luật/quy ước**, KHÔNG phải mã nguồn. KHÔNG chứa credential/schema/dữ liệu nhạy cảm/đường dẫn nhạy cảm.
> Owner duyệt công bố 19/08/2026 (D3).

## Nội dung

| File | Là gì |
|---|---|
| `CLAUDE.md` | **Một bản replica** của bộ luật quản trị. Bộ luật gồm **5 file byte-identical** (`.cursorrules` · `.antigravityrules` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md`); công bố 1 bản là đủ. **sha256 (8 ký tự đầu) = `05a8ff93`** · **parity 5/5 OK** (`npm run check:governance`). |
| `UI-STANDARD.md` | Chuẩn giao diện DUY NHẤT của dự án (SSOT UI) — bảng bo góc, màu, component, layout list/detail. |
| `ui-standard-sources.md` | Registry nguồn của chuẩn UI (rút từ trang nào, class/hex thật). |
| `secret-exposure-status-CONG-KHAI.md` | Trạng thái rà + gỡ thông tin nhạy cảm — **BẢN ĐÃ CHE** (không lộ file:dòng, không lộ mã commit, không lộ giá trị). |
| `acceptance-template.md` | Mẫu bảng tiêu chí nghiệm thu (acceptance) cho các gói việc. |

## Ghi chú an toàn

- Bản `secret-exposure-status-CONG-KHAI.md` đã **che** mọi vị trí file:dòng + mã commit của registry nội bộ tương ứng (bản đầy đủ ở kho riêng tư).
- Toàn bộ file trong folder này đã qua cổng `npm run test:secret-scan` (2 chế độ: theo MẪU + theo GIÁ TRỊ) → **0 vi phạm** trước khi công bố.
- KHÔNG đăng: mã nguồn (`src/`), script, file kiểm thử, schema, dữ liệu thật.
