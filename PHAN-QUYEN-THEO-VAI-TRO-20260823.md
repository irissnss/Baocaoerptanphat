# PHÂN QUYỀN THEO VAI TRÒ — KHÔNG THEO CHỨC DANH

**Ngày:** 23/08/2026 · **Loại:** đợt **DỮ LIỆU + KIỂM THỬ** (không phát hành phiên bản mới)
**Mã ghi nhận (commit):** `c5e9133` · hệ thống vận hành vẫn ở **V1.00.352**

> Bản tin public-safe: chỉ nêu số lượng, mã kỹ thuật và tên vai trò. Không có thông tin đăng nhập,
> không có giá trị nhạy cảm.

---

## 1. Nguyên tắc Chủ sở hữu khoá

> *"Quyền phải cấp linh hoạt theo NGƯỜI, cấm gắn cứng theo tên chức danh.
> Đổi tên chức danh (TP.KD → TP.Kinh Doanh → Business) không được ảnh hưởng quyền."*

---

## 2. Rà soát: kết quả khác dự đoán

Đã quét **toàn bộ 770 tệp mã** trong hai thư mục mã nguồn và kịch bản, tìm mọi chỗ có thể suy ra
quyền từ chức danh / vị trí công việc:

| Nhóm quét | Kết quả |
|---|---|
| Tên chức danh gắn cứng trong nhánh quyền | **0** |
| Trường vị trí / chức vụ dùng trong điều kiện phân quyền | **0** (25 chỗ tìm được đều chỉ là hiển thị hoặc quản lý danh mục) |
| Ba tệp của tầng phân quyền có nhắc tới chức danh | **0** |
| Chuỗi "TP.KD" trong mã | **0** — chuỗi gần giống duy nhất là **mã phòng ban**, không phải chức danh |

Quyền hiện được xác định theo chuỗi: **tài khoản → vai trò của tài khoản → quyền của vai trò**.
Không có đường nào đi qua chức danh.

**Kết luận:** nguyên tắc Chủ sở hữu khoá thì mã **đã tuân thủ sẵn** ⇒ **không sửa mã nguồn ứng dụng
một dòng nào**. Sửa bừa chỗ đang đúng là tự tạo rủi ro.

---

## 3. Chỗ thiếu thật sự: chưa có gì KHOÁ nguyên tắc đó

Mã đúng ở hiện tại, nhưng **không có bài kiểm thử nào giữ cho nó tiếp tục đúng**. Đây đúng loại lỗ
hổng từng gặp ở bản vá trước: mã đúng, nhưng cố ý cắm lỗi vào mà **toàn bộ bài kiểm thử vẫn báo xanh**.

Đã bổ sung một bộ kiểm thử chuyên khoá nguyên tắc này — **17/17 đạt**:

| Nhóm | Nội dung kiểm |
|---|---|
| **Cổng tĩnh** | Ba tệp của tầng phân quyền **không được phép** nhắc tới chức danh / vị trí |
| **Đổi tên chức danh** | Đổi `TP.KD` → `TP.Kinh Doanh` → `Business Head`, rồi đổi hẳn sang **vị trí khác** → **quyền y hệt**, so bằng dấu vân tay quyền |
| **Đổi vai trò** | Gán quyền quản trị → **có quyền ngay**; gỡ ra → **mất quyền ngay**; chức danh **không hề bị đụng** |
| **Kiểm ngược** | Người ở vai trò thấp cố dùng chức năng ngoài quyền → **bị chặn** |

**Chứng minh bộ kiểm thử có tác dụng thật:** cố ý cắm vào tầng phân quyền đúng loại lỗi bị cấm —
đọc tên chức danh rồi cho qua nếu khớp "TP.KD" — bộ kiểm thử **báo đỏ ngay 3 điểm**. Trả mã về
nguyên bản → **17/17 đạt trở lại**, mã nguồn sạch.

---

## 4. Gán vai trò đích danh

Chủ sở hữu chỉ đích danh người giữ vai trò quản trị bổ sung. Việc gán được ghi **theo tài khoản**,
**không theo chức danh** — đúng nguyên tắc ở mục 1.

**Kết quả (số dòng gán vai trò):**

| Vai trò | Sau khi gán | Chủ sở hữu duyệt |
|---|---|---|
| Quản trị (gồm điều hành) | **4** | 4 ✅ |
| Kinh doanh | **3** | 3 ✅ |
| Kế toán | **1** | 1 ✅ |

**Đo quyền thật trên hệ thống vận hành (chỉ đọc, 16/16 đạt):** 3 tài khoản kinh doanh **bị chặn**
chức năng duyệt giá · 3 tài khoản quản trị **được** duyệt · cờ quản trị khớp vai trò ở **9/9** tài khoản.

*Ghi chú cách kiểm:* **không** đăng nhập bằng tài khoản của nhân viên, vì các tài khoản này đang bật
cờ bắt buộc đổi mật khẩu lần đầu — không được đụng vào mật khẩu người khác. Thay vào đó đo thẳng
tầng quyết định quyền, mạnh hơn việc bấm giao diện và không tạo phiên đăng nhập nào.

---

## 5. Dọn dữ liệu theo xác nhận của Chủ sở hữu

- **3 khách hàng thử nghiệm** → trả về **không có người phụ trách**. Đối chiếu hai chiều mã ↔ định danh
  trước khi ghi; sau khi ghi: tổng số khách **không đổi**, **1.692 khách còn lại vẫn nguyên**.
- **7 hồ sơ nhân sự chưa có tài khoản đăng nhập** → **tạm để vậy** theo quyết định của Chủ sở hữu,
  không tạo tài khoản. Hồ sơ vẫn đầy đủ, chỉ là chưa đăng nhập được.
- **Mật khẩu quản trị** → Chủ sở hữu **đã tự đổi** qua màn hình mới. Khoản nợ này **đóng**.

Mỗi bước ghi dữ liệu đều có **sao lưu bảng ngay trước khi ghi**.

---

## 6. Một điểm lệch so với yêu cầu — khai rõ

Yêu cầu ban đầu có bước **phát hành phiên bản mới**. Nhưng đợt này **mã nguồn ứng dụng không đổi một
dòng nào** (chỉ thêm bài kiểm thử và kịch bản xử lý dữ liệu) ⇒ tăng số hiệu phiên bản sẽ là **một con
số giả**, trái quy định nội bộ về đánh số phát hành.

**Đã không tăng phiên bản, không triển khai lại.** Hệ thống vận hành giữ nguyên **V1.00.352** với mã
chạy y hệt. Mã nguồn mới đã được đẩy lên kho.

---

## 7. Kiểm thử tổng thể sau tất cả thay đổi

**13 bộ · 565 khẳng định · xanh toàn bộ** · kiểm kiểu 0 lỗi · dựng bản phát hành 0 lỗi ·
cổng quét thông tin nhạy cảm 0 · cổng quét dữ liệu cá nhân 0 · cổng phân tích cú pháp **770/770**.

---

## 8. Việc để lại sau khi vận hành ổn định (ghi nhận, chưa làm)

Chủ sở hữu chỉ đạo **dồn lực cho chức năng chính để đưa vào vận hành sớm**, chưa đầu tư ba mục sau:

| Mục | Hiện trạng |
|---|---|
| Trang người dùng tự quản lý hồ sơ | Mới có màn đổi mật khẩu rời |
| Chuông thông báo theo từng người | Nút chuông đang **tĩnh** — chấm đỏ luôn hiện, bấm không ra gì |
| Chế độ sáng/tối + dọn bộ biểu tượng | Gộp làm một lượt |

Ngoài ra còn một khoản đang mở: **1.692 khách hàng đang gán tạm cho một người phụ trách** — chờ phân
bổ cho đội kinh doanh trong quá trình vận hành.
