# PHA C3 — AUDIT SỰ THẬT HIỆN HÀNH · XỬ LÝ CUỐN CHIẾU

**Ngày:** 24/08/2026 · **Loại:** quản trị + tài liệu
**Không đụng:** mã nghiệp vụ · cơ sở dữ liệu · máy vận hành · số phiên bản · Notion

---

## 1. Việc này giải quyết điều gì

Sau Pha C2, tài liệu của dự án còn một số chỗ **nói hai điều khác nhau về cùng một sự việc**. Đợt này rà từng chỗ, và xử lý theo đúng ba mức:

- có bằng chứng rõ → **sửa ngay**, chạy cổng kiểm, đẩy lên;
- kiểm xong thấy đang đúng → **để nguyên**, ghi rõ vì sao;
- **thiếu bằng chứng** → **không đoán**, ghi rõ thiếu gì và cần phép đo nào để đóng.

**Kết quả: 23 mục.** 13 mục đang đúng · **7 mục đã sửa** · 1 mục hai nguồn nói khác nhau · **2 mục còn thiếu bằng chứng**.

---

## 2. Bảy mục đã sửa

| # | Trước | Sau | Vì sao |
|---|---|---|---|
| 1 | Sổ số phát hành nội bộ dừng ở **V1.00.351** | bổ sung đủ **V1.00.352 → V1.00.355** | thiếu bốn đợt; ai tra sổ sẽ đọc ra số sai |
| 2 | Khoản nợ về sổ này còn treo | **đã đóng** đúng điều kiện đã ghi khi mở nợ | điều kiện là "cập nhật sổ kèm mốc từng đợt" — đã làm đúng thế |
| 3 | Phần đầu README ghi **hai** phiên bản như đang chạy | chỉ còn **V1.00.355**; mốc go-live cũ gắn nhãn **lịch sử** | một tài liệu không thể có hai phiên bản cùng là hiện hành |
| 4 | *"99 bảng — kiểm đếm 23/07"* | **101 bảng** (đo 23/08), kèm giải thích | xem mục 4 |
| 5 | M5: *"còn thiếu tự động hoá tồn kho"* | nêu **từng nhánh** đã tự động / còn thủ công | câu cũ **sai với 3 trong 5 nhánh** |
| 6 | Danh sách việc treo của Pha B không có nhãn thời điểm | thêm bảng đối chiếu **trạng thái hôm nay** | tránh đọc nhầm danh sách cũ thành việc còn tồn |
| 7 | Bốn con số **14 / 16 / 4 / 7** bị đọc như cùng một mẫu số | tách rõ mỗi số đo cái gì | xem mục 6 |

---

## 3. Sự thật phiên bản — tách **ba lớp**

Một số nằm trong mã **không tự chứng minh** nó đang chạy trên máy vận hành. Nên tách:

| Lớp | Giá trị | Bằng chứng |
|---|---|---|
| Số trong **mã nguồn** | **V1.00.355** | tệp khai số phiên bản |
| Có **hồ sơ phát hành** | **V1.00.355** | commit phát hành `c637fb9` (23/08 18:31) |
| **Đã lên máy vận hành** | **V1.00.355** | hồ sơ triển khai ghi mã `0e73a7c` (23/08 18:43) |

**Dòng thời gian bổ sung vào sổ:** V1.00.351 (22/08) → 352 (22/08) → 353 (23/08) → 354 (23/08) → **355 (23/08)**, mỗi đợt có mã phát hành và mốc giờ.

> **Một điểm từng gây bối rối, nay đã rõ:** báo cáo triển khai ghi mã `0e73a7c` còn commit phát hành là `c637fb9`. Đo lại: `c637fb9` là commit **tăng số**, `0e73a7c` là bản vá **12 phút sau**, **vẫn cùng số V1.00.355**, và là **hậu duệ** của commit kia. Hai mã khác nhau nhưng **một phiên bản** — không mâu thuẫn.

⚠️ **Nói thẳng giới hạn:** lớp thứ ba dựa vào **hồ sơ triển khai của dự án**, không phải phép đo trực tiếp từ phiên này — phiên quản trị **không được** nối vào máy vận hành.

---

## 4. 99 hay 101 bảng — là **lệch thời điểm**, không phải đếm sai

| Mốc | Số bảng |
|---|---:|
| 23/07/2026 | 99 |
| 22/08/2026 | 99 — kèm ghi nhận **thiếu 2 bảng** |
| 23/08/2026 | **101** |

**99 + 2 = 101.** Hai đối tượng bổ sung là **bảng thật**, không phải khung nhìn — chứng minh bằng chính câu lệnh tạo bảng trong mã.

⚠️ **Còn thiếu bằng chứng:** chưa nguồn nào tách riêng **bảng** với **khung nhìn** — mọi hồ sơ dùng chung một chữ "bảng". Con số 101 là **tổng đối tượng theo cách hồ sơ phát hành đếm**. Muốn tách phải có một phép đếm phân loại chạy trên chính cơ sở dữ liệu.

---

## 5. M5 — tồn kho đã tự động tới đâu

Cách xác định: liệt kê **mọi** nơi trong mã ghi vào sổ kho, rồi truy ngược xem **ai gọi** chúng. Ra **6 điểm tự động** + **1 điểm nhập tay**.

| Nhánh | Trạng thái | Ghi chú |
|---|---|---|
| Giao hàng (thành phẩm) | ✅ **Đã tự động** | tự trừ tồn khi giao, **tự hoàn lại** khi huỷ |
| Phiếu nhập vật tư | ✅ **Đã tự động** | tự ghi sổ kho, **không cộng hai lần** |
| Phiếu xuất vật tư | ✅ **Đã tự động** | tự ghi sổ, **chặn xuất vượt tồn**, huỷ thì tự sinh bút toán đảo |
| Nhập sổ kho thủ công | ✅ có màn riêng | dành cho trường hợp nhập tay |
| **Kiểm kê** | ⚠️ **Một phần** | ghi nhận chênh lệch + số phiếu điều chỉnh, nhưng **chưa tự** ghi sổ kho — phải lập phiếu điều chỉnh riêng |
| **Mua hàng → công nợ** | ❌ **Còn thủ công** | chưa tự sinh công nợ |

⇒ Câu cũ *"còn thiếu tự động hoá tồn kho"* **sai với 3 trong 5 nhánh nghiệp vụ**. Đã thay bằng mô tả theo từng nhánh.

---

## 6. Bốn con số — mỗi số đo **một thứ khác nhau**

| Số | Đo cái gì |
|---:|---|
| **16** | số **cổng kiểm** trong chuỗi kiểm tổng — đây là con số "bao nhiêu cổng" đúng nghĩa |
| **4** | số cổng chạy **trước mỗi lần ghi mã** — là **tập con** của 16 |
| **7** | số **kịch bản thử phá** *bên trong* cổng chống trùng mã — là **bài kiểm thử**, không phải cổng |
| **37** | số phép kiểm *bên trong* bộ kiểm chính sách phiên bản — cũng là bài kiểm thử |

*(Con số "14" từng xuất hiện chỉ là số cổng được chạy lẻ trong một lượt kiểm tay — **không phải một tập hợp có định nghĩa**. Đã bỏ khỏi tài liệu.)*

---

## 7. Hệ quản trị cơ sở dữ liệu — hai nguồn nói khác nhau

| Lớp | Nội dung |
|---|---|
| Tài liệu nền bên ngoài | máy phát triển **MySQL 8.4** · máy vận hành **MariaDB 10.11** |
| Tài liệu trong kho — **bản cũ** | trùng khớp với tài liệu nền ở trên |
| Tài liệu trong kho — **bản mới** | **cả hai** đều **MariaDB 10.11** — ghi rõ mốc: *"Từ 09/08/2026 máy phát triển và máy vận hành dùng chung hệ quản trị"* |
| Máy phát triển — **đang chạy thật** | ❓ **chưa đo được** |
| Máy vận hành — **đang chạy thật** | ❓ **chưa đo được** — phiên quản trị không được nối |

**Kết luận:** tài liệu nền bên ngoài đang phản ánh trạng thái **trước 09/08/2026**; trong kho có **quyết định ghi ngày 09/08** đổi máy phát triển sang cùng hệ với máy vận hành. Đây là **tài liệu nền lỗi thời**, **không phải** mã chạy sai chính sách.

**Không sửa cấu hình trong đợt này.** README của kho báo cáo đang ghi đúng theo quyết định 09/08 nên **giữ nguyên**.

---

## 8. Còn lại — 4 việc, mỗi việc có cách đóng cụ thể

| # | Việc | Thiếu gì | Đóng bằng cách nào |
|---|---|---|---|
| 1 | Hệ quản trị CSDL máy phát triển | phép đo thật | bật môi trường phát triển rồi đọc phiên bản hệ quản trị |
| 2 | Hệ quản trị CSDL máy vận hành | phép đo thật | một lượt đọc-thuần khi có phiên được cấp quyền |
| 3 | Tách **bảng** với **khung nhìn** | phân loại đối tượng | một phép đếm phân loại trên cơ sở dữ liệu |
| 4 | Bàn giao Notion | — | trợ lý Notion đồng bộ |

**Cả ba việc đầu đều cần một phép đo mà phiên quản trị này không được phép chạy.** Không đoán, không nhờ chủ dự án "chọn" một sự thật kỹ thuật.

---

## 9. Kết luận

**PHA C3 = HOÀN TẤT MỘT PHẦN.** Mọi mục có bằng chứng đều đã xử lý, kiểm thử và đẩy lên. Còn **3 việc kỹ thuật** cần phép đo ngoài quyền hạn của phiên này, cộng bàn giao Notion.

**Chưa sẵn sàng bàn giao Notion** — ba việc trên ảnh hưởng trực tiếp nội dung sẽ bàn giao (hệ quản trị CSDL, số bảng).

**Không** dùng nhãn *"toàn hệ thống đã đồng bộ"*.

**Phạm vi không đụng:** mã nghiệp vụ · tệp kỹ năng · cơ sở dữ liệu · máy vận hành · số phiên bản · năm tệp luật · Notion. Không viết lại lịch sử, không ép ghi đè.

---

*Báo cáo công khai — đã qua cổng an toàn: không chứa mã nguồn, dữ liệu thật, thông tin cá nhân, khoá, đường dẫn máy, hay định danh máy chủ.*
