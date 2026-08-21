# 📋 KẾ HOẠCH IMPORT DANH MỤC NỀN (v5) — 9 ĐIỂM CHỜ OWNER DUYỆT · 21/08/2026

> **Bản công khai — chỉ mô tả quyết định cần duyệt.** KHÔNG chứa dữ liệu thật/PII/số tiền/credential.
> **Bối cảnh:** kế hoạch nạp 3 danh mục nền từ hệ thống cũ (AppSheet) vào ERP — thứ tự **Nhân viên → Khách hàng → Nhà cung cấp**, CHỈ trên máy phát triển (local), chưa chạy. Kế hoạch đầy đủ nằm ở kho riêng tư (`docs/PLAN-IMPORT-APPSHEET-DANHMUC-NEN.md`).
> **Trạng thái:** ⛔ **CHỜ OWNER DUYỆT 9 ĐIỂM** — chưa duyệt đủ thì **KHÔNG thực thi import** (cổng vào fail-closed).

---

## Vì sao cần duyệt trước khi chạy

Nạp dữ liệu thật vào ERP là thao tác **khó hoàn tác**. Kế hoạch đã rà kỹ và tìm ra một số điểm **bắt buộc Owner quyết trước** — gồm 2 thay đổi cấu trúc dữ liệu (schema) và một số quy tắc xử lý. Nếu chạy khi chưa chốt, rủi ro: mất dữ liệu âm thầm (cắt câm), gán sai mặc định, hoặc trộn dữ liệu nháp với dữ liệu thật.

---

## 9 điểm chờ duyệt

| # | Điểm cần Owner quyết | Loại | Gợi ý của agent |
|---|---|---|---|
| 1 | **Thêm cột "người tạo/người sửa" (dạng số)** cho bảng Nhà Cung Cấp — để ghi đúng người thao tác theo chuẩn ERP (cột hiện tại đang là chữ, không lưu được id số) | Đổi schema | Thêm cột mới, **giữ cột cũ** (không phá dữ liệu lịch sử) |
| 2 | **Số điện thoại NCC** đang giới hạn 15 ký tự; dữ liệu cũ hay ghi nhiều số trong 1 ô → chọn: **(a) nới rộng cột** / (b) giữ nguyên + tách số + ghi nhật ký / (c) làm cả hai | Đổi schema / quy tắc | **(a) nới rộng** — an toàn, không mất dữ liệu |
| 3 | **Duyệt bảng quy đổi phân loại NCC** (loại NCC, trạng thái) từ chữ tiếng Việt của hệ cũ sang mã chuẩn ERP — sau khi chạy thử in ra danh sách giá trị gốc thật | Quy tắc ánh xạ | Duyệt sau bước chạy thử (dry-run) |
| 4 | **Ngưỡng tỉ lệ lỗi cho phép mỗi lô** khi nạp — bắt buộc chốt TRƯỚC khi chạy; vượt ngưỡng thì dừng + báo | Quy tắc an toàn | Owner nêu con số (VD ≤ 2%/lô) |
| 5 | **30 dòng nhân viên hiện có là dữ liệu NHÁP** → xử lý: **(a) lưu trữ (archive)** / (b) xoá + kiểm ràng buộc / (c) giữ | Xử lý dữ liệu | **(a) archive** — giữ lịch sử, không trộn với dữ liệu thật |
| 6 | Có **đồng bộ độ rộng cột mã khách hàng** giữa hai bảng (hiện lệch nhau) để tránh lỗi về sau không? | Đổi schema (tuỳ chọn) | Nên đồng bộ để phòng xa |
| 7 | **Cách đổi mã bản ghi cũ** khi có ràng buộc khoá ngoại chặt: chạy trong một giao dịch (transaction) hay tạm tắt kiểm khoá ngoại? | Quy tắc kỹ thuật | Ưu tiên transaction (an toàn hơn) |
| 8 | Xác nhận **cách đánh dấu địa chỉ "Chưa xác định"** sao cho KHÔNG làm hỏng ô tìm kiếm địa chỉ đang chạy mượt (đặt ở mức ẩn khỏi danh sách chọn) | Quy tắc kỹ thuật | Xác nhận phương án đã thiết kế |
| 9 | **5 điểm còn lại từ bản trước:** vai trò từng nhân viên · các bước chuẩn bị danh mục (phòng ban/vị trí) · ngưỡng nhận diện tỉnh tự động (≥ 80%) · cách sinh mã cho khách hàng/NCC | Gộp | Duyệt theo kế hoạch |

---

## Sau khi duyệt

Khi Owner duyệt đủ 9 điểm (đánh dấu ngay trong kế hoạch), quy trình chạy sẽ là: chuẩn bị danh mục nền → chạy thử (dry-run) xuất mẫu để Owner xem → nạp thật trên máy phát triển → đối chứng số lượng hai đầu → kiểm chứng trên ứng dụng. **Máy vận hành (production) là bước riêng, sau khi Owner duyệt kết quả trên máy phát triển.**

> Cập nhật 21/08/2026 · Agent IDE. Chỉ mô tả quyết định — không dữ liệu thật.
