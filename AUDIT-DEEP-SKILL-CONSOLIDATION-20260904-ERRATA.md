# ĐÍNH CHÍNH BÁO CÁO AUDIT SÂU 13 KỸ NĂNG

> **Đính chính cho:** [AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.md](https://github.com/irissnss/Baocaoerptanphat/blob/main/AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.md)
> **Do gói việc:** `ERP-SKILL-ROUTING-CANARY-005` · **Ngày:** 04/09/2026
> **Bản gốc GIỮ NGUYÊN làm bằng chứng lịch sử** — không sửa, không gỡ. Tệp này chỉ bổ sung đính chính.

Sáu điểm cần đính chính. Bốn điểm là **lỗi số học hoặc lỗi phân loại của chính em**; hai điểm là **làm rõ**, không phải sai.

---

## Đ-1 · SỐ TRANG NOTION ĐÃ ĐỌC — GHI SAI, VÀ GHI THẤP HƠN THỰC TẾ

**Bản gốc ghi:** đã đọc **4**, cần **7**, rồi liệt kê **7** mục chưa đọc.
**Sai ở hai chỗ:**
1. **Số học không khớp:** 4 + 7 = 11, lớn hơn tổng 7.
2. **Một mục trong danh sách "chưa đọc" lại ghi thẳng là "đã đọc riêng"** — chính là trang gói việc mà em đã tự đọc.

**Số đúng:** em đã đọc **6** trang, không phải 4. Con số 4 chỉ đếm phần giao cho tiến trình phụ, **bỏ sót 2 trang em tự đọc trực tiếp**: trang gói việc và trang bản đồ hai thư viện.

| Trang | Trạng thái đúng |
|---|---|
| Hướng Dẫn TanPhatAI V4.0 | ✅ ĐÃ ĐỌC (qua tiến trình phụ) |
| Điều phối Skill Agent IDE cho ERP Tân Phát | ✅ ĐÃ ĐỌC (qua tiến trình phụ) |
| Trung tâm Quản lý Skill ERP | ✅ ĐÃ ĐỌC (qua tiến trình phụ) |
| AUDIT-ERP-SKILL-CATALOG-002 | ✅ ĐÃ ĐỌC (cả trực tiếp lẫn qua tiến trình phụ) |
| WP-ERP-SKILL-R0-01 | ✅ ĐÃ ĐỌC TRỰC TIẾP — **bản gốc xếp nhầm vào nhóm chưa đọc** |
| Bản đồ hai thư viện kỹ năng (đã thành lịch sử) | ✅ ĐÃ ĐỌC TRỰC TIẾP — **bản gốc không tính** |
| Cơ sở dữ liệu danh mục kỹ năng | ⛔ CHƯA ĐỌC |
| Trang định nghĩa phạm vi 13 so với 7 | ⛔ CHƯA ĐỌC |
| Trang luật/điều-khiển hiện hành riêng ERP | ⛔ CHƯA ĐỌC |
| Trang KN27 | ⛔ CHƯA ĐỌC |

**Phán quyết đối chiếu Notion vẫn là `PARTIAL`** — vì vẫn còn 4 nguồn chưa đọc. Đính chính này **không** nâng phán quyết lên đủ.

---

## Đ-2 · ĐẾM MÂU THUẪN SỐ LIỆU — GHI SAI

**Bản gốc ghi:** "5 đóng · 2 chờ Chủ dự án".
**Đếm lại theo đúng phán quyết của từng mục:** **4 đóng · 3 còn mở**.

| Mã | Chủ đề | Phán quyết thật | Nhóm |
|---|---|---|---|
| G-1 | Chỉ số nội dung bị cấm 11/12/17 | GIẢI QUYẾT XONG | **ĐÓNG** |
| G-2 | Số khoá JSON 27 so với 31 | GIẢI QUYẾT XONG — trễ do thời điểm | **ĐÓNG** |
| G-3 | Ba khoá cấp cao thừa | PHÁT HIỆN MỚI — chờ Chủ dự án | **MỞ** |
| G-4 | Cụm danh sách-bảng 10 so với 16 | KHÔNG TÁI HIỆN ĐƯỢC | **ĐÓNG** |
| G-5 | Cụm lược đồ 8 so với 14 | KHÔNG TÁI HIỆN ĐƯỢC | **ĐÓNG** |
| G-6 | Mâu thuẫn đổ bóng trong chuẩn giao diện | TÀI LIỆU LỖI THỜI — chờ Chủ dự án | **MỞ** |
| G-7 | Phạm vi 13 so với 7 | MỞ | **MỞ** |

---

## Đ-3 · PHÂN LOẠI RỦI RO — SAI VỀ NGUYÊN TẮC, ĐÂY LÀ ĐIỂM NẶNG NHẤT

**Bản gốc ghi cả 7 kỹ năng nhóm R0 là `R0 chỉ-đọc`**, dựa trên lập luận *"không có tệp thi hành ⇒ không có mặt tấn công dạng chạy mã"*.

**Lập luận đó SAI về nguyên tắc.** Rủi ro phải đo bằng **tác động tối đa khi ĐI THEO quy trình mà kỹ năng dạy**, không phải bằng việc kỹ năng có kèm tệp chạy được hay không. Một tệp văn bản thuần vẫn có thể dạy agent xoá thư mục, sửa tệp, đổi lược đồ hay tăng số phát hành.

**Kết quả phân loại lại — 6 trong 11 kỹ năng đổi lớp:**

| Kỹ năng | Bản gốc ghi | Đúng phải là | Vì sao |
|---|---|---|---|
| `dual-key-standard` | R0 chỉ-đọc | **R2 dữ liệu/quyền** | Đã tự kiểm lại bằng nội dung thật của tệp, không chép gợi ý. Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh, có hai việc và cả hai đều nằm ở mục Checklist bắt buộc: Việc thứ nhấ… |
| `windows-dev-troubleshoot-quick` | R0 chỉ-đọc | **R1 sửa mã/tệp** | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: mục "Action" dạy agent XOÁ ĐỆ QUY CƯỠNG BỨC một thư mục trên đĩa (thư mục dựng tạm `.next`), sau đó DỪNG và CHẠY LẠI tiến trình máy chủ phá… |
| `file-update-safe-workflow` | R0 chỉ-đọc | **R1 sửa mã/tệp** | HÀNH ĐỘNG CAO NHẤT MÀ KỸ NĂNG DẠY, nêu đích danh: Bước 3 dạy agent THÊM, SỬA và SẮP XẾP LẠI nội dung của một tệp tài liệu ĐANG TỒN TẠI trên đĩa; Bước 4 dạy GHI một dòng nhãn phiên bản vào c… |
| `windows-next-cache-stability` | R0 chỉ-đọc | **R1 sửa mã/tệp** | Hành động CAO NHẤT mà kỹ năng dạy agent làm, nêu đích danh theo thứ tự tăng dần: 1. XOÁ TOÀN BỘ MỘT THƯ MỤC trên đĩa (thư mục biên dịch `.next`, bước 2). Đây là hành động GHI/XOÁ trên hệ tệ… |
| `column-config-workflow` | R0 chỉ-đọc | **R1 sửa mã/tệp** | Hành động CAO NHẤT mà kỹ năng dạy đích danh, nêu ở Bước 3: sửa trực tiếp mã nguồn giao diện của một trang danh sách đang vận hành — viết lại phần đầu bảng và phần thân bảng, viết lại danh s… |
| `update-work-log` | R1 sửa mã/tệp | **R3 phát hành** | Nêu đích danh chuỗi hành động cao nhất mà kỹ năng DẠY agent làm, theo đúng thứ tự trong tệp: · SÀN RỦI RO — R1: bước 3 dạy CHÈN một khối nội dung vào đầu `WORK_LOG.md`, và bước 5 dạy xử lý … |

**Sau khi phân loại lại, chỉ còn ĐÚNG MỘT kỹ năng thật sự là R0:** `schema-visualization`.

Phân bố mới: R3 phát hành **4** · R2 dữ liệu/quyền **1** · R1 sửa mã/tệp **5** · R0 chỉ-đọc **1**.

Đồng thời tách làm **hai trường riêng, cấm suy ra nhau**:
- **Quyền lúc bắt đầu** — hầu hết là *chỉ đọc*, vì mở tệp ra đọc thì chưa đổi gì.
- **Rủi ro tối đa** — lớp cao nhất mà việc **làm theo** kỹ năng có thể chạm tới.

---

## Đ-4 · LỖ HỔNG BẢO TOÀN — CHƯA TÁCH "BAN ĐẦU" VỚI "CÒN LẠI"

**Bản gốc ghi:** bộ phản biện tìm 6 lỗ hổng và 6 chức năng chưa có đích, rồi nói em đã sửa — nhưng **không nói rõ còn lại bao nhiêu**.

**Tách rõ:**

| Lỗ hổng ban đầu | Sau khi kiểm toán viên sửa |
|---|---|
| 1. Cụm versioning: phán quyết mâu thuẫn bản thiết kế | ✅ **ĐÃ ĐÓNG** — chốt rõ bản thiết kế là kế hoạch nội dung cho tệp đầu mối, không phải bản gộp |
| 2. Nhãn bị-thay-thế sai mà không có hành động sửa | ✅ **ĐÃ ĐÓNG** — nâng thành quyết định QD-1 |
| 3. Cụm panel giữ hai tệp nhưng bỏ trống việc sửa phần chọi | ✅ **ĐÃ ĐÓNG** — tách thành QD-2 và QD-3 |
| 4. Tệp bố cục cũng có chuỗi cấm mà không nhắc | ✅ **ĐÃ ĐÓNG** — gộp vào QD-3 |
| 5. Bảy kỹ năng R0 không khai cổng bảo toàn | 🟡 **ĐÓNG MỘT PHẦN** — cổng đếm hai điều kiện đã nằm ở giai đoạn 2 và 3 của kế hoạch; lượt này bổ sung chứng minh bảo toàn cho từng phiếu |
| 6. Toàn bộ vẫn ở trạng thái chưa-soát, không ai nói ai soát | ⛔ **CÒN MỞ** — vẫn chưa có người và tiêu chí soát hiệu lực nội dung |

**Còn lại: 1 lỗ hổng mở hoàn toàn, 1 đóng một phần.**

---

## Đ-5 · CỤM TỪ "12 KỸ NĂNG" TRONG BÁO CÁO ĐẦU TIÊN

Báo cáo đầu tiên (ngày 03/09) ghi *"12 kỹ năng còn chứa chuỗi chuẩn CẤM"*. Con số đó là một **phép trộn tuỳ tiện** giữa hai tập khác nhau và **không nên dùng lại**.

**Ba chỉ số đúng, có nhãn rõ:**
- **17** — số kỹ năng mang ít nhất một chuỗi bị cấm (mọi phán quyết đối chiếu chuẩn)
- **11** — phần giao giữa 17 đó với nhóm **chọi chuẩn**
- **36** — số kỹ năng mang phán quyết chọi chuẩn
- Bổ sung: **18** mục chuỗi cấm · **2007** lần xuất hiện

Báo cáo đầu tiên **giữ nguyên làm bằng chứng lịch sử**; con số 12 nay mang nhãn **KHÔNG DÙNG LẠI**.

---

## Đ-6 · KIỂM LẠI ARTEFACT AUDIT-004

| Phép kiểm | Kết quả |
|---|---|
| JSON đọc được | ✅ ĐẠT |
| Số dòng chức năng | ✅ 193 — khớp giữa báo cáo và artefact |
| Số kỹ năng trong phạm vi | ✅ 13, không trùng |
| Checksum tính lại trên đúng byte của Git | ✅ **KHỚP TUYỆT ĐỐI** |
| Nội dung riêng bị lộ | ✅ KHÔNG — cổng an toàn 7/7 đạt |

**Không có nội dung nào của AUDIT-004 bị rút lại.** Sáu điểm trên là đính chính số liệu và phân loại, **không** thay đổi phán quyết cốt lõi: giữ toàn bộ 13 kỹ năng, không gộp, không xoá.

---

_Bản gốc giữ nguyên. Tệp này là lớp đính chính đứng cạnh, không thay thế._
