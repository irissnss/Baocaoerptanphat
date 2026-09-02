# GÓI BÀN GIAO CHO AGENT NOTION — DELTA SAU CHECKPOINT `397ddc`

> **Theo** `GOV-NOTION-HANDOFF-001` (§F1c) — bắt buộc khi kết thúc một gói việc có quyết định Owner mới.
> **TanPhatAI đã đồng bộ tới checkpoint `397ddc`** (gồm quyết định *một báo giá → một đơn*) vào năm trang Notion:
> ERP root · ERP governance · M0 · Permission Matrix · M3.
> **Văn bản này chỉ bàn giao phần PHÁT SINH SAU mốc đó.** Không gửi lại nội dung đã đồng bộ.

---

## 1. NHẬN DẠNG BẢN PHÁT HÀNH

| Trường | Giá trị |
|---|---|
| Phiên bản ứng dụng | **`V1.00.367`** |
| Mã nguồn đang chạy trên máy vận hành | **`39ed9eb687957b1a34e0b0a622ad9a42de7447b8`** |
| `origin/main` hiện tại | `bc81d9538d5e26ff4782c9a54a760eb9d977bbbd` |
| Dấu vân tay bản dựng | `8e345660e69fb00883703dedcd5703df` |
| Thời điểm dựng | 2026-09-02 23:17:54 (giờ máy chủ) |
| Thời điểm triển khai | 2026-09-02T16:17:56Z |
| Mốc quay về | `ef0e195` / `V1.00.366` |
| Chênh lệch tổng | 24 commit · 71 tệp tính từ `ef0e195` |

---

## 2. KẾT LUẬN ĐỐI CHIẾU — DÙNG NHÃN CHÍNH XÁC

> ### `RUNTIME_SOURCE_CONVERGED_WITH_METADATA_DIFF`

**KHÔNG** dùng nhãn "toàn kho khớp tuyệt đối". Lý do, nêu đủ:

- **Mã chạy hội tụ tuyệt đối:** chênh lệch `39ed9eb..bc81d95` gồm đúng **1 commit**, đụng đúng **2 tệp** — sổ nợ kỹ thuật và một báo cáo. **0 tệp trong `src/`**, **0 thay đổi `package.json`**.
- **Khác biệt còn lại thuần tài liệu:** máy vận hành không cần và không nên chạy lại chỉ để nhận hai tệp văn bản.

⇒ Người đọc Notion cần hiểu: **mã đang chạy = mã đã duyệt**; phần lệch là sổ sách, không phải hành vi hệ thống.

---

## 3. QUYẾT ĐỊNH OWNER MỚI — ĐÃ GHI SỔ

| Mục sổ | Nội dung | Trạng thái |
|---|---|---|
| **#220** | *"Một báo giá đã duyệt chỉ được sinh một đơn hàng duy nhất trong toàn vòng đời"* — mốc 02/09/2026 22:22 ICT | `SYNCED_TO_NOTION` (TanPhatAI đã xác nhận) |

**Bốn hệ quả Owner nêu rõ, đã thi hành đủ:**

1. Đơn đã **huỷ vẫn là đơn** của báo giá đó — không mở lại suất ✅
2. Không cho tạo dòng đơn hàng thứ hai từ cùng báo giá ✅
3. Không tự gộp/xoá bản trùng lịch sử ✅ *(hiện không còn bản trùng nào: 7 báo giá · 0 đơn)*
4. Điều chỉnh phải đi qua quy trình hiện hữu ✅

> ⚠️ **Một ý định trước đó đã bị huỷ bỏ, Notion cần biết:** mục sổ #214 từng ghi ý *"sửa chốt chặn để bỏ qua đơn đã huỷ"*. Quyết định 22:22 nói **ngược lại**. Ý định đó **không thi hành**. Chốt chặn đếm **mọi** đơn kể cả đơn huỷ — và như vậy là **đúng ngay từ đầu**.

---

## 4. PHÂN ĐỊNH CUỐI CỦA CÁC KHOẢN NỢ

| Mã | Trạng thái | Lớp bằng chứng | Ghi chú cho Notion |
|---|---|---|---|
| `DEBT-144` | **ĐÃ XỬ LÝ** | `RUNTIME_PROVEN` | Đã triển khai. 4/4 hàm ghi dùng khoá riêng của màn. Phơi nhiễm trên máy vận hành **đã đóng** |
| `DEBT-168` | **ĐÃ XỬ LÝ** | `UI_PROVEN` + `RUNTIME_PROVEN` | Phân định đủ 8/8; 14/14 cổng · 193 điều kiện · 2 kiểm ngược đạt. Cổng máy vận hành nay đủ điều kiện chạy |
| `DEBT-169` | **ĐÃ XỬ LÝ** | `DB_PROVEN` | Bốn lớp quyền khớp **từng dòng**; 9 tài khoản thật khớp hoàn toàn |
| `DEBT-149` | **ĐÃ XỬ LÝ** | `CODE_PROVEN` + `DB_PROVEN` | Bất biến do **tầng lưu trữ** giữ, **không phải** ràng buộc cơ sở dữ liệu — xem §5 |
| `DEBT-143` | **CÒN MỞ** | — | **Ghi nhận cũ SAI, phải sửa trên Notion** — xem §6 |

**Nợ còn mở: 33 / 88.**

---

## 5. ĐIỀU NOTION KHÔNG ĐƯỢC GHI QUÁ

> ### Bất biến "một báo giá — một đơn" do **PHẦN MỀM** giữ, **KHÔNG** phải cơ sở dữ liệu.

Bảng đơn hàng **không có** ràng buộc chống trùng ở tầng dữ liệu. Cơ chế thật là **khoá dòng báo giá** đặt bên trong giao dịch ghi.

- ❌ **Cấm ghi:** *"cơ sở dữ liệu bảo đảm mỗi báo giá chỉ một đơn"*
- ✅ **Ghi đúng:** *"tầng lưu trữ giữ bất biến này bằng khoá hàng trong giao dịch; chưa có ràng buộc ở tầng dữ liệu"*

Thêm ràng buộc tầng dữ liệu là **đổi cấu trúc bảng** — cần bản đề xuất riêng và Owner duyệt riêng. **Chưa làm trong đợt này.**

**Bằng chứng cơ chế thật sự có tác dụng** (bài kiểm 18/18, ca quyết định):

```
bỏ khoá  → hai yêu cầu đồng thời sinh ra 2 đơn
có khoá  → hai yêu cầu đồng thời sinh ra 1 đơn
```

---

## 6. MỘT GHI NHẬN CŨ SAI — NOTION CẦN SỬA

Tài liệu và sổ nợ trước đây ghi:

> *"`create_task` chạy được; chỉ `notify` và `send_email` là hành động rỗng."*

**Sai.** Đo tận nơi: **cả ba** hành động đều kết thúc bằng mấy dòng bỏ trống — tức không làm gì.

**Đã xử lý khác nhau cho từng loại, và Notion cần ghi đúng như vậy:**

| Hành động | Xử lý | Lý do |
|---|---|---|
| `notify` · `send_email` | **Báo lỗi rõ** bằng tiếng Việt | Owner không kỳ vọng chúng hoạt động; im lặng báo thành công là nói dối người dùng |
| `create_task` | **Cảnh báo rõ trong nhật ký**, không báo lỗi | Owner chưa yêu cầu bật; dựng thật cần nối M8 — ngoài phạm vi |

⚠️ **Ca suýt gây hỏng, đáng ghi vào Notion làm bài học:** cả **ba** quy trình đang chạy (`WF_BAO_GIA` · `WF_DON_HANG` · `WF_THIET_KE`) đều có cấu hình `notify`. Nếu chỗ gọi không bắt lỗi từng hành động thì việc **chuyển trạng thái sẽ vỡ toàn bộ**. Đã kiểm trước khi kết luận: chỗ gọi **có** bắt lỗi riêng từng hành động, nên chỉ ghi nhật ký, không phá nghiệp vụ.

---

## 7. HAI CON SỐ TỪNG GÂY TRANH CÃI — ĐÃ PHÂN ĐỊNH

| Vấn đề | Phân định | Bằng chứng |
|---|---|---|
| **66 hay 67** quyền hành động | **`REPORT_QUERY_MISMATCH`** — lỗi phép đo, **không phải trôi dữ liệu** | Chạy **cùng một câu truy vấn** hai bên: 67 = 67, so **từng dòng**, **0 dòng lệch** |
| **13 hay 10** bảng được dọn | **Không mâu thuẫn** — câu chữ gây hiểu nhầm | Kịch bản **kiểm 13** bảng; báo cáo **liệt kê 10** bảng có dòng bị xoá. Chênh đúng 3 bảng vốn **đã bằng 0 từ trước** |

---

## 8. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC — NOTION KHÔNG ĐƯỢC NHẬN NHẦM THÀNH SỰ THẬT

| Điều | Vì sao chưa chứng minh được |
|---|---|
| **Owner đã nghiệm thu giao diện phân quyền** | **CHƯA.** Owner chưa trả lời. Agent **không tự ghi** kết quả nghiệm thu |
| `create_task` tạo được công việc thật | Chưa nối M8 task API — ngoài phạm vi đợt này |
| Ràng buộc chống trùng ở tầng dữ liệu | Chưa có, và **cố ý chưa làm** (cần đề xuất riêng) |
| Bốn dòng lệnh sản xuất đã xoá có phải việc thật ngoài xưởng | Chỉ đọc được số trong máy; cần người biết xưởng xác nhận |

---

## 9. TRẠNG THÁI GÓI VIỆC

> ### `LIVE_VERIFIED / AWAITING_OWNER_UAT`

- **Không** gọi gói việc là hoàn tất.
- **Không** mở M1.
- **Không** mở Pricing.
- Bước kế tiếp duy nhất: **Owner nghiệm thu trên máy vận hành**.

Câu hỏi đã đặt cho Owner, nguyên văn theo đặc tả:

> *"Giao diện này đã đủ trực quan để anh tự phân quyền mà không cần hiểu kỹ thuật phân quyền chưa?"*

---

*Công khai-an toàn: không mật khẩu, không khoá, không địa chỉ máy chủ, không đường dẫn tuyệt đối, không email hay tên người thật, không tên khách hàng, không số tiền, không mã nguồn.*
