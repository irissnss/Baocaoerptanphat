# 📥 NẠP 3 DANH MỤC NỀN TỪ HỆ THỐNG CŨ (AppSheet) → ERP — KẾT QUẢ TRÊN MÁY PHÁT TRIỂN · 21/08/2026

> **Bản công khai** — chỉ nêu **số lượng + quy trình**. KHÔNG chứa dữ liệu thật, thông tin cá nhân, mật khẩu hay giá trị nhạy cảm.
> **Phạm vi:** CHỈ máy phát triển (local). **Chưa** đưa lên máy vận hành — production là bước riêng, sau khi Owner duyệt kết quả này.
> Owner duyệt kế hoạch (9 điểm) ngày 21/08/2026 trước khi thực thi.

---

## 1) Kết quả nạp

| Danh mục | Nguồn | Đã nạp | Bỏ qua | Lỗi | Đối chứng |
|---|---|---|---|---|---|
| **Nhân viên** | 17 dòng (16 sau gộp trùng người) | **16** | 0 | **0** | vào = ra + bỏ qua ✓ |
| **Khách hàng** | 1.692 | **1.692** | 0 | **0** | ✓ |
| ├ Liên hệ khách hàng | 1.692 (từ hồ sơ KH) + 1.644 (từ danh sách liên hệ) | **1.896** | 1.443 trùng đã gộp | 0 | ✓ (xem mục 6) |
| ├ Địa chỉ khách hàng | — | **2.009** | — | 0 | ✓ |
| **Nhà cung cấp** | 109 | **109** | 0 | **0** | ✓ |
| ├ Địa chỉ NCC | — | **109** | — | 0 | ✓ |
| └ Liên hệ NCC | — | **109** | — | 0 | ✓ |

**Tỉ lệ lỗi: 0%** (ngưỡng cho phép ≤2%).

## 2) Nhận diện địa chỉ (tỉnh/thành)

| Chỉ số | Kết quả |
|---|---|
| Tổng địa chỉ xử lý | ~5.400 dòng (3 nguồn) |
| **Nhận diện được tỉnh** | **98,6%** (ngưỡng yêu cầu ≥98%) |
| Chưa xác định (chờ bổ sung) | **1,8%** số địa chỉ đã nạp — gắn nhãn "Chưa xác định", **ẩn khỏi ô chọn địa chỉ**, cập nhật dần |

Cách nhận diện: **2 tầng** — đối chiếu tên tỉnh/thành **mới (34 đơn vị sau sáp nhập 2025)** trước; không khớp thì tra **bảng đổi tên cũ → mới (63 → 34)**; ngoài ra suy luận theo quận/huyện chỉ thuộc một thành phố. **Không bịa tỉnh** — không xác định được thì để "Chưa xác định".

## 3) Quy trình đã thực hiện (có cổng kiểm ở từng bước)

1. **Chuẩn bị cấu trúc** — diễn tập toàn bộ trên **bản sao cơ sở dữ liệu** (chạy → chạy lại → hoàn tác 100% đều đạt) rồi mới áp lên máy phát triển; **có sao lưu trước**.
2. **Bổ sung danh mục nền** — thêm 2 phòng ban và các vị trí công việc còn thiếu; **nhà cung cấp nay có bảng liên hệ + địa chỉ riêng** (đồng bộ cách quản lý với khách hàng — "một chuẩn, một biểu mẫu").
3. **Chạy thử (dry-run)** — xuất mẫu, bảng phân vai, tỉ lệ nhận diện địa chỉ, danh sách trùng/bỏ qua kèm lý do → **trình Owner duyệt** rồi mới ghi thật.
4. **Ghi thật** theo lô, mỗi bản ghi trong một giao dịch; **chạy lại không nhân đôi**; đối chứng số lượng hai đầu.
5. **Kiểm chứng** — mở ứng dụng xem danh sách 3 danh mục, thử ô tìm địa chỉ (vẫn hoạt động bình thường), kiểm tra kiểu dữ liệu 0 lỗi, dựng ứng dụng đạt, bộ kiểm thử nền đạt.

## 4) Nguyên tắc an toàn đã áp dụng

- **Không mang mật khẩu cũ** sang hệ thống mới. Tài khoản mới dùng mật khẩu tạm, **bắt buộc đổi ở lần đăng nhập đầu**; danh sách mật khẩu tạm giao Owner **kênh riêng, không đưa lên kho mã**.
- **Không nạp các cột số liệu tổng hợp** (giá trị đơn hàng, đã thanh toán, còn nợ) — hệ thống tự tính từ chứng từ.
- **Không cắt bớt dữ liệu quá dài** — nếu vượt độ rộng cột thì **báo cáo**, không âm thầm cắt (thực tế: 0 dòng vượt).
- **Mã mới**: nhân viên theo số thứ tự nội bộ; khách hàng và nhà cung cấp theo **thời điểm tạo** (đọc được ngày giờ, không lộ quy mô). Bản ghi cũ được **đổi mã đồng bộ** sang chuẩn mới, kiểm tra không còn liên kết mồ côi.
- **Dữ liệu nháp cũ** trong hệ thống được **lưu trữ tách biệt** (không trộn với dữ liệu thật), có mốc dọn dẹp về sau.
- Người thực hiện được ghi nhận bằng **tài khoản quản trị thật**, thời gian ghi theo **giờ Việt Nam**.

## 5) Trạng thái

✅ Hoàn tất trên **máy phát triển**. ⏳ **Chờ Owner duyệt kết quả** → sau đó mới lên kế hoạch đưa lên máy vận hành (bước riêng).

---

## 6) 📌 ĐÍNH CHÍNH (cùng ngày 21/08/2026) — số liên hệ khách hàng

**Phát hiện khi đối chiếu:** hệ thống cũ lưu **người liên hệ chính ở hai nơi** (trong hồ sơ khách hàng *và* trong danh sách liên hệ riêng). Lần chạy đầu cộng cả hai nguồn nên **nhân đôi 1.443 dòng** (báo cáo trước ghi 3.324 — con số đó **đã sai**).

**Đã xử lý (theo quyết định Owner):**
1. **Gộp thông tin trước khi xóa** — bù các trường mà bản giữ còn trống (1 email, 2 ghi chú) → **không mất thông tin nào**.
2. **Sao lưu bảng liên hệ** trước khi xóa (có điểm quay lui).
3. Xóa **1.443 dòng nhân bản** → còn **1.896 liên hệ** (1.695 chính + **201 liên hệ phụ thật**).
4. **Kiểm chứng:** 0 nhóm trùng · **100% khách hàng (1.695/1.695) đều có liên hệ** · 0 dòng mất tên.
5. **Vá công cụ nạp** để lần chạy sau (kể cả trên máy vận hành) **không lặp lại lỗi**; đã kiểm trên bản sao: **0 dòng dư**.

**Số đúng:** Liên hệ khách hàng = **1.896**. Các số còn lại trong báo cáo **không đổi**.

> 21/08/2026 · Agent IDE. Không chứa dữ liệu thật/PII/mật khẩu.
