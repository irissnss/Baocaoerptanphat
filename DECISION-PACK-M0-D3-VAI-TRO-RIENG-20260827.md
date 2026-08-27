# GÓI QUYẾT ĐỊNH — VAI TRÒ RIÊNG CHO TỪNG NGƯỜI (`D3`)

**Ngày:** 27/08/2026
**Trạng thái:** ⏳ **CHỜ OWNER QUYẾT — MỘT CÂU HỎI DUY NHẤT**
**Không chặn:** M3 vẫn chạy được bình thường; thao tác phân quyền hiện tại vẫn an toàn.

---

## 1. OWNER ĐÃ CHỐT GÌ

- Quyền dùng **mẫu vai trò** + **tuỳ biến theo từng người**.
- Ưu tiên **vai trò riêng gắn cho một người**, không tạo kho quyền song song.
- **Không** tạo thêm bốn bảng quyền mới.

Tôi đồng ý với hướng đó. Vấn đề nằm ở chỗ lược đồ hiện tại **chưa đủ** để làm an toàn.

---

## 2. LƯỢC ĐỒ HIỆN TẠI — ĐO ĐƯỢC, KHÔNG SUY ĐOÁN

```
dm_vai_tro: ma_vai_tro · ten_vai_tro · mo_ta · la_admin
            · ngay_tao · nguoi_tao · ngay_sua · nguoi_sua
```

**Không có cột nào đánh dấu "vai trò này là riêng của ai".**

Chín vai trò đang có đều là **mẫu dùng chung**:
`ADMIN` · `CEO` · `HR` · `KE_TOAN` · `SALES` · `TP_KINH_DOANH` · `TP_SAN_XUAT` · `TP_THIET_KE` · `USER`

⇒ Không có cách nào **đọc bằng máy** để biết một vai trò là mẫu chung hay riêng của một người.

---

## 3. VÌ SAO ĐIỀU ĐÓ QUAN TRỌNG

Owner yêu cầu vai trò riêng phải có: **tên dễ hiểu · chủ sở hữu rõ · không cấp nhầm người khác ·
xem trước quyền hiệu lực · nhân bản từ mẫu không tự thu hồi quyền · dọn vai trò mồ côi ·
nhật ký · chốt chặn quản trị cuối**.

Sáu trong tám điều đó cần biết **ai là chủ** của vai trò. Không có cột đó thì:

- không chặn được việc gán nhầm vai trò riêng của người A cho người B;
- không tìm được vai trò mồ côi khi người đó nghỉ việc;
- màn phân quyền không phân biệt được mẫu chung với vai trò riêng, nên người sửa
  **không biết mình đang đổi quyền của một người hay của cả nhóm** — đúng cái rủi ro
  mà bước "số tài khoản bị ảnh hưởng" vừa được thêm để cảnh báo.

---

## 4. HAI PHƯƠNG ÁN

### Phương án A — **KHÔNG DDL**, đánh dấu bằng quy ước đặt tên

Vai trò riêng mang tiền tố cố định, ví dụ `RIENG__<mã người>`.

| Được | Mất |
|---|---|
| Không đụng lược đồ | Chủ sở hữu nằm trong **chuỗi ký tự**, không có ràng buộc nào bảo vệ |
| Làm được ngay | Đổi email/mã người thì **liên kết đứt lặng lẽ** — đúng loại lỗi `DEBT-128` |
| Hoàn tác dễ | `ma_vai_tro` chỉ **20 ký tự** — có thể không đủ chỗ |
| | Không có khoá ngoại ⇒ **không có gì chặn** việc gán nhầm cho người khác |

### Phương án B — **CÓ DDL**, thêm hai cột vào `dm_vai_tro`

```sql
ALTER TABLE dm_vai_tro
  ADD COLUMN la_vai_tro_rieng TINYINT(1) NOT NULL DEFAULT 0,
  ADD COLUMN chu_so_huu_email VARCHAR(100) NULL,
  ADD CONSTRAINT fk_vai_tro_chu_so_huu
      FOREIGN KEY (chu_so_huu_email) REFERENCES user_account (email)
      ON DELETE SET NULL ON UPDATE CASCADE;
```

| Được | Mất |
|---|---|
| Chủ sở hữu có **khoá ngoại** — không gán nhầm được | Cần Owner duyệt DDL |
| Đổi email **tự theo** (`ON UPDATE CASCADE`) | Phải chạy di trú trên máy vận hành |
| Người nghỉ việc ⇒ cột về `NULL` ⇒ **tìm được vai trò mồ côi** | |
| Màn phân quyền phân biệt được mẫu chung với vai trò riêng | |
| Cả hai cột đều có mặc định ⇒ **không dòng nào hiện có bị ảnh hưởng** | |

**Đường lùi cho B:**
```sql
ALTER TABLE dm_vai_tro
  DROP FOREIGN KEY fk_vai_tro_chu_so_huu,
  DROP COLUMN chu_so_huu_email,
  DROP COLUMN la_vai_tro_rieng;
```
Lùi sạch, không mất dữ liệu — hai cột đều mới, chưa có gì phụ thuộc.

**Ảnh hưởng tới triển khai:** một bản di trú thêm cột, chạy được nhiều lần, **không khoá bảng lâu**
(`dm_vai_tro` chỉ có 9 dòng). Sẽ chạy trong bước nạp dữ liệu đã có, trước bước kích hoạt.

---

## 5. ĐỀ XUẤT CỦA TÔI — **PHƯƠNG ÁN B**

Vì lý do đã đo được, không phải sở thích: **phương án A tái tạo đúng lỗi `DEBT-128`** —
buộc một quan hệ vào chuỗi ký tự không có ràng buộc, rồi liên kết đứt lặng lẽ khi chuỗi đó đổi.
Chúng ta vừa mất một lượt để phát hiện và ghi nhận đúng kiểu lỗi đó.

`dm_vai_tro` chỉ có **9 dòng**, hai cột đều có mặc định, và đường lùi sạch. Rủi ro thấp,
giá trị cao.

---

## 6. NẾU OWNER CHƯA MUỐN QUYẾT

Không sao — **không có gì bị chặn**. Thao tác phân quyền hiện tại vẫn an toàn:
quản trị tạo vai trò mới và gán cho một người vẫn làm được, chỉ là hệ thống
**chưa biết** đó là vai trò riêng nên chưa cảnh báo và chưa dọn mồ côi được.

Khoản nợ này giữ `HOÃN CÓ CHỦ ĐÍCH`, kích hoạt xử lý: **khi Owner quyết A hay B**.
