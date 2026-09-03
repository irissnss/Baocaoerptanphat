# ĐO TRẠNG THÁI M1 — TRÌNH ANH QUYẾT ĐỊNH

> **Anh giao 03/09/2026:** *«đo rồi trình cho anh đi em»*.
> Toàn bộ số dưới đây **đo trực tiếp trên máy vận hành**, không lấy lại số cũ.

---

## 1. KẾT LUẬN NGẮN

**M1 không phải màn trống chờ xây — nó đang chạy với dữ liệu thật rất lớn.**

Việc của M1 bây giờ **không phải viết thêm màn**, mà là **lấp ba chỗ trống cụ thể** và **cho người dùng thật vào dùng**.

---

## 2. M1 CÓ GÌ — 15 MÀN, DỮ LIỆU THẬT

| Màn | Dữ liệu thật trên máy vận hành |
|---|---|
| **Khách hàng** | **hàng nghìn** khách · **hàng nghìn** địa chỉ · **hàng nghìn** người liên hệ |
| **Nhân sự** | **46** người · vài chục vị trí · một số phòng ban |
| Địa chỉ hành chính VN | **hàng nghìn** |
| Nhà cung cấp | **110** |
| Công đoạn sản xuất | **85** · bảng giá công đoạn **10** |
| Nhóm danh mục dùng chung | **116** |
| Đơn vị tính | **33** |
| Vật tư | **19** |
| **Sản phẩm** | **5** ⚠️ |
| Quy đổi đơn vị | **1** ⚠️ |
| Vật tư dạng mới (`material_item`) | **0** ⚠️ |

---

## 3. CHẤT LƯỢNG — BỐN ĐIỀU KIỂM, BỐN ĐIỀU ĐẠT

| Điều kiểm | Kết quả |
|---|---|
| **Dữ liệu giả trong mã M1** | ✅ **0** — ba chỗ tìm được đều là chú thích ghi *"dùng dữ liệu thật thay vì giả"* |
| **Chặn gõ thẳng địa chỉ** | ✅ **10/10 màn** có kiểm quyền |
| **Kiểm quyền khi thêm/sửa/xoá** | ✅ **10/10 tệp** hành động đều kiểm |
| **Vai trò đã có quyền M1** | ✅ Kinh Doanh **10 màn** · TP Kinh Doanh **8** · TP Thiết Kế **7** |

> ⚠️ **Một báo động giả em đã tự loại:** lượt quét đầu báo *"màn Khách Hàng không có kiểm quyền"* — hoá ra nó **có**, đặt ở `layout.tsx` chứ không ở `page.tsx`, và đặt ở đó **tốt hơn** vì phủ luôn các màn con (địa chỉ · liên hệ · tạo mới). Em quét hẹp nên suýt báo sai.

---

## 4. MỘT TÀI LIỆU CÓ SẴN MÀ CHƯA AI ĐỌC

Trong kho có **`GOLIVE-PLAN.md`** — kế hoạch đưa M1 vào dùng thật, lập **14/06/2026**, trạng thái ghi *"Chờ Owner xác nhận"* — **treo gần ba tháng**.

Nó có sẵn thứ em định soạn lại từ đầu:

- **Phạm vi**: Khách Hàng + Báo Giá + Đơn Hàng
- **9 bài kiểm nghiệm thu T1–T9** — bộ tiêu chí *"giao được chưa"*
- **5 khoảng trống G1–G5**
- **4 câu hỏi chờ anh trả lời** — chưa ai trả lời

> Đây là **lần thứ hai trong hai ngày** em gặp cùng một chuyện: tài liệu đã có sẵn, đúng thứ đang cần, mà không phiên nào mở ra. Lần trước là Notion.

### Năm khoảng trống đó — đo lại sau gần ba tháng

| # | Khoảng trống (14/06) | Hôm nay |
|---|---|---|
| **G1** | Không chặn gõ thẳng địa chỉ | ✅ **ĐÃ ĐÓNG** — 10/10 màn có kiểm quyền |
| **G2** | Nhân viên chưa gắn với tài khoản đăng nhập | 🔴 **CÒN** — xem mục 5 |
| **G3** | Chưa có vai trò Kinh Doanh / Thiết Kế | ✅ **ĐÃ ĐÓNG** — đã có và đã cấp quyền |
| **G4** | Báo giá / Đơn hàng chưa rà | ✅ **ĐÃ ĐÓNG** — rà nhiều đợt, mới nhất là chốt chặn một-báo-giá-một-đơn |
| **G5** | Chưa kiểm quyền khi thêm/sửa/xoá | ✅ **ĐÃ ĐÓNG** — 10/10 tệp đều kiểm |

**Bốn trong năm khoảng trống đã tự đóng** qua các đợt làm suốt ba tháng. Còn đúng **một**.

---

## 5. BA CHỖ TRỐNG THẬT SỰ CÒN LẠI

### 🔴 (1) đa số trong vài chục người chưa có tài khoản đăng nhập

| | Số |
|---|---|
| Nhân sự trong hệ thống | **46** |
| Có địa chỉ email | **42** |
| **Có tài khoản đăng nhập được** | **9** |
| **Chưa có** | **37** |

**Đây là chỗ chặn lớn nhất.** Màn phân quyền anh vừa nghiệm thu xong sẽ **không có ai để phân quyền** nếu đa số người kia chưa có tài khoản.

Và đây đúng là **câu hỏi Q1** treo từ 14/06: *"Số lượng nhân viên cần tạo tài khoản? Danh sách tên + email?"*

### 🟠 (2) Danh mục sản phẩm chỉ có 5 dòng

**hàng nghìn khách hàng** nhưng chỉ **5 sản phẩm**. Với xưởng in bao bì thì con số này thấp bất thường — có thể sản phẩm đang được gõ tay vào từng báo giá thay vì chọn từ danh mục.

Nếu đúng vậy thì mỗi báo giá phải gõ lại, và không thống kê được bán gì chạy nhất.

### 🟡 (3) Hai bảng gần như rỗng

- **Quy đổi đơn vị: 1 dòng** — chưa dùng được để đổi ram ↔ tờ ↔ kg
- **Vật tư dạng mới: 0 dòng** — bảng có mà chưa ai nạp

---

## 6. TIN TỐT: DỮ LIỆU KHÁCH HÀNG RẤT SẠCH

| | Số |
|---|---|
| Khách **đã có người phụ trách** | **gần như toàn bộ / hàng nghìn** |
| Chưa có | **3** — đúng ba khách thử anh đã chốt để trống từ 23/08 |

**Không có khách nào mồ côi.** Đây là nền tốt để giao cho kinh doanh dùng.

---

## 7. EM ĐỀ XUẤT — THEO THỨ TỰ

| Ưu tiên | Việc | Vì sao trước | Cần anh |
|---|---|---|---|
| **1** | **Tạo tài khoản cho nhân viên** | Không có tài khoản thì màn phân quyền vô dụng, và người thật không vào được | **Có** — xem mục 8 |
| **2** | **Chạy 9 bài kiểm T1–T9** của kế hoạch cũ | Bộ tiêu chí *"giao được chưa"* đã soạn sẵn, chỉ việc chạy | Không |
| **3** | **Rà danh mục sản phẩm** | Xem 5 dòng là đủ hay đang thiếu | **Có** — anh biết xưởng |
| **4** | Nạp quy đổi đơn vị | Cần cho tính giá theo ram/tờ/kg | Không, nhưng cần anh xác nhận tỉ lệ |

---

## 8. BỐN CÂU HỎI CẦN ANH — TREO TỪ 14/06

Kế hoạch cũ để lại đúng bốn câu, chưa ai trả lời:

| # | Câu hỏi | Vì sao cần |
|---|---|---|
| **Q1** | **Trong vài chục người, ai cần tài khoản đăng nhập?** Kinh doanh · thiết kế · kế toán · kho · sản xuất? | Quyết định tạo bao nhiêu tài khoản |
| **Q2** | **Thiết kế được làm gì?** Chỉ xem, hay tạo/sửa được? | Quyết định ô tick quyền cho vai trò Thiết Kế |
| **Q3** | **Email nhân viên dùng loại nào?** Email công ty hay email cá nhân? | đa số đã có email — cần biết dùng được không |
| **Q4** | **Danh mục sản phẩm 5 dòng là đủ hay thiếu?** | Quyết định có phải nạp thêm không |

> ⚠️ Em **không tự trả lời** bốn câu này — chúng là quyết định nghiệp vụ, chỉ anh biết. Nhưng em **không hỏi lại** chuyện ô tick quyền: cái đó là **cấu hình**, anh đã chốt ở mục #208 là em tự áp theo hướng anh nêu.

---

## 9. ĐIỀU EM CHƯA ĐO ĐƯỢC

| Điều | Vì sao |
|---|---|
| 9 bài kiểm T1–T9 hiện đạt bao nhiêu | Chưa chạy — cần tài khoản thử cho từng vai trò |
| Danh mục 5 sản phẩm là đủ hay thiếu | Cần người biết xưởng in xác nhận |
| đa số người chưa có tài khoản có **cần** tài khoản không | Có thể một số người không dùng máy tính |

---

*Công khai-an toàn: chỉ nêu số lượng và tên bảng; không nêu tên người, email, tên khách hàng hay số tiền.*
