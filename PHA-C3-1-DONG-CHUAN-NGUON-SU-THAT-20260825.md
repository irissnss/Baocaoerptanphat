# PHA C3.1 — ĐÓNG CHUẨN NGUỒN SỰ THẬT · CHÍNH SÁCH CSDL · 101 BẢNG · BÀN GIAO NOTION

**Ngày:** 25/08/2026 · **Loại:** quản trị + tài liệu + **đo read-only**
**Không đụng:** mã ứng dụng · cơ sở dữ liệu · máy vận hành · số phiên bản · Notion

---

## 1. Việc này giải quyết điều gì

Chủ dự án chốt một loạt quyết định nền ngày **25/08/2026**. Lượt này ghi sổ các quyết định đó, **đo lại** những gì đo được, sửa những kết luận **vượt quá bằng chứng** ở lượt trước, và đóng gói phần kỹ thuật để trợ lý Notion cập nhật.

**Điều quan trọng nhất của lượt này: nói đúng mức bằng chứng.** Có chính sách **không** có nghĩa là đã chạy như vậy. Có hồ sơ triển khai **không** có nghĩa là đã đo thấy.

---

## 2. Quyết định chủ dự án — đã ghi sổ

Ghi vào Sổ Yêu Cầu Owner, **mục `#144`**, trạng thái **chờ đồng bộ sang Notion**:

| | Nội dung |
|---|---|
| **a** | **Notion là sổ gốc chuẩn** cho mục tiêu · ngữ cảnh · chính sách · mô hình nghiệp vụ · quy trình · quyết định chủ dự án · cấu trúc kỹ thuật **đã được rà và bàn giao đầy đủ** |
| **b** | Quyết định chốt **ngay trong lúc làm** là **bằng chứng hợp lệ**, nhưng phải ghi sổ và **chuyển về Notion**. Sổ là **kênh vận chuyển**, không phải một nguồn sự thật cạnh tranh |
| **c** | **Chính sách CSDL:** máy phát triển **và** máy vận hành đều **MariaDB 10.11 LTS** |
| **d** | **Số hiện hành = 101 bảng**; 99 là mốc lịch sử |
| **e** | **"Đang chạy thật"** chỉ được nói khi **chính lượt đó đã đo trực tiếp** |
| **f** | Code/sửa lỗi đưa lên vận hành phải khép **đủ chuỗi**, không báo "xong" khi mới sửa ở máy phát triển |
| **g** | Báo cáo công khai **được** nêu tên bảng/cột/màn hình khi cần truy vết; **vẫn cấm** mã nguồn, khoá, dữ liệu thật |
| **h** | Notion **được** chỉ đạo thẳng vào bảng/cột/quy trình **khi** phần đó đã được rà và còn đúng; **chưa rõ thì chỉ đạo theo ngữ cảnh nghiệp vụ**, không bịa tên, **cấm tạo bảng/mã/quy trình song song** |
| **i** | **Lịch sử phải giữ**, gắn nhãn rõ, không để câu cũ trông như trạng thái hiện tại |

---

## 3. Chính sách CSDL — và điều **chưa** đo được

| Lớp | Kết quả |
|---|---|
| **Chính sách** (chủ dự án chốt 25/08) | **MariaDB 10.11 LTS** — cả máy phát triển lẫn máy vận hành |
| **Cấu hình** máy phát triển | ✅ **đúng là MariaDB** — dịch vụ đặt ở cổng riêng, cấu hình ghi rõ nhãn MariaDB; cổng cũ chỉ còn là **đường lui** |
| **Đang chạy thật** — máy phát triển | ⛔ **chưa đo được** — dịch vụ CSDL **đang tắt**, và không chứng minh được việc bật lên sẽ không tự khởi tạo/di trú dữ liệu ⇒ **không bật** |
| **Cấu hình** máy vận hành | MariaDB 10.11 |
| **Đang chạy thật** — máy vận hành | ⛔ **chưa đo được** — lượt này **không được cấp** kênh đọc |

⇒ **Chưa kết luận được** là đang khớp chính sách hay đang lệch. **Không** tự chuyển đổi, **không** sửa cấu hình.

> 🔧 **Đính chính một kết luận của lượt trước.** Lượt trước viết *"tài liệu nền lỗi thời vì trong kho có ghi chú ngày 09/08 mới hơn"*. Nói vậy là **sai thứ bậc nguồn** — một ghi chú kỹ thuật trong kho **không tự nó** thay được một tài liệu nền đã chuẩn hoá. Trình tự đúng: chính sách cũ → ghi nhận kỹ thuật 09/08 → **chủ dự án chốt chính sách 25/08** → tài liệu nền **chờ cập nhật**.

---

## 4. Số bảng

| Mục | Kết quả |
|---|---|
| Chủ dự án xác nhận | **101** |
| Đo ở máy phát triển | ⛔ dịch vụ đang tắt |
| Đo ở máy vận hành | ⛔ chưa được cấp kênh |
| Tách **bảng** ↔ **khung nhìn** | ⛔ **vẫn chưa làm được** |
| Mốc 99 | **lịch sử** — trước khi thêm 2 bảng, cả hai là **bảng thật** |

**Không** phát sinh mâu thuẫn với con số chủ dự án xác nhận — vì lượt này **chưa có phép đo nào** để mà mâu thuẫn. Giữ **101**.

---

## 5. Phiên bản

| Lớp | Giá trị |
|---|---|
| Số trong mã nguồn | **V1.00.355** ✅ đo được |
| Có hồ sơ triển khai | **V1.00.355** ✅ |
| **Đang chạy thật** | ⛔ **chưa đo** — không được cấp kênh |

Chưa loại trừ được khả năng lệch phiên bản, vì **chưa đo** được phía máy vận hành. Không tự triển khai gì trong lượt này.

---

## 6. M5 — nói lại theo **mức bằng chứng**

Lượt trước ghi thẳng *"đã tự động"*. Theo quy ước mới, phải tách: **có mã + có kiểm thử + có hồ sơ triển khai** là một mức; **đo trên máy đang chạy** là mức cao hơn.

| Nhánh | Mức đang có |
|---|---|
| Giao hàng · phiếu nhập vật tư · phiếu xuất vật tư | **có mã · có kiểm thử · có hồ sơ triển khai** *(chưa đo khi chạy thật)* |
| Nhập sổ kho thủ công | **có mã** |
| Kiểm kê | ⚠️ **một phần** — ghi nhận chênh lệch nhưng chưa tự ghi sổ kho |
| Mua hàng → công nợ | ❌ **còn thủ công** |

Kiểm thử vận hành thật cho ba nhánh đầu **cần một lượt được duyệt riêng**, vì phải tạo chứng từ thật và làm đổi tồn kho.

---

## 7. Đính chính công trạng

Lỗi bộ kiểm đếm nhầm dòng nằm trong khối mã ví dụ — **cả việc phát hiện lẫn việc vá đều thuộc lượt trước đó (Pha C2)**, xác định bằng lịch sử mã: commit vá là `0f593bd`, và tệp bộ kiểm **không** nằm trong phần thay đổi của Pha C3.

Các **tài liệu** đã ghi đúng từ đầu. Chỉ **lời tường thuật** cuối Pha C3 là gộp nhầm thành công của Pha C3 — nay ghi rõ: **Pha C3 chỉ kiểm chứng lại.**

---

## 8. Bàn giao cho Notion

Gói bàn giao **theo phần liên quan**, không kết xuất toàn bộ 101 bảng:

| Phần | Nội dung |
|---|---|
| Nền tảng CSDL | chính sách · cấu hình · **trạng thái đang chạy = chưa đo** · trang cần sửa (nơi còn ghi máy phát triển dùng hệ cũ) |
| Số bảng | 101 hiện hành · 99 lịch sử · chênh lệch 2 bảng kèm bằng chứng · **chưa tách bảng/khung nhìn** |
| Quy trình M5 | sáu nhánh kèm điểm kích hoạt, bảng bị đụng, hành vi khi huỷ, kiểm thử, phiên bản, bước thủ công còn lại |
| Metadata | commit bằng chứng · ngày rà · phiên bản mã · trạng thái triển khai · **trạng thái runtime = chưa đo** |

**Sẵn sàng bàn giao: CÓ — nhưng chỉ phần chính sách và ánh xạ đã chứng minh.** Phần *đang chạy thật* **không** được bàn giao như sự thật hiện hành vì chưa đo.

---

## 9. Còn lại

| # | Việc | Đóng bằng cách nào | Cần chủ dự án? |
|---|---|---|---|
| 1 | Hệ CSDL máy phát triển đang chạy | bật môi trường phát triển rồi đọc phiên bản | không — chỉ cần bật máy |
| 2 | Hệ CSDL máy vận hành đang chạy | cấp kênh đọc-thuần | **có** |
| 3 | Tách bảng ↔ khung nhìn | đếm phân loại trên CSDL | đi kèm 1 hoặc 2 |
| 4 | Phiên bản đang chạy trên máy vận hành | đọc phiên bản ứng dụng | **có** |
| 5 | Kiểm thử vận hành thật M5 | một lượt được duyệt riêng | **có** |
| 6 | Bàn giao Notion | trợ lý Notion cập nhật | — |

---

## 10. Kết luận

**PHA C3.1 = HOÀN TẤT** phần quản trị và tài liệu. Mọi câu trong tài liệu nay **nói đúng mức bằng chứng đang có**.

**Xác minh phiên bản/CSDL đang chạy thật: CÒN MỞ.**
**Chưa đụng:** mã ứng dụng · cơ sở dữ liệu · máy vận hành · số phiên bản · Notion.

---

*Báo cáo công khai — đã qua cổng an toàn. **Không chứa:** mã nguồn đầy đủ · bản kết xuất cấu trúc CSDL · khoá/mật khẩu/chuỗi kết nối · nội dung tệp cấu hình môi trường · dữ liệu cá nhân · dữ liệu nghiệp vụ · địa chỉ máy/cổng/đường dẫn máy · bản sao lưu · định danh hạ tầng nhạy cảm.*
*Được phép nêu **định danh kỹ thuật** khi cần truy vết: tên bảng · tên cột · tên màn hình · tên cổng kiểm · số phiên bản · số lượng · mã commit rút gọn.*
