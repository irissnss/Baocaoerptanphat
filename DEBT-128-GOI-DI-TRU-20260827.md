# GÓI DI TRÚ — `DEBT-128` · ĐỊNH DANH NGƯỜI DÙNG ỔN ĐỊNH

**Ngày:** 27/08/2026
**Trạng thái:** ⏳ **CHỜ OWNER DUYỆT** — **CHƯA chạy DDL · CHƯA triển khai**
**Căn cứ:** Yêu cầu Owner mục G (10 điểm), phiên `M0-ROLLING-CLOSEOUT-20260827`

---

## 0. ĐIỀU QUAN TRỌNG NHẤT — BẢN GHI NỢ BAN ĐẦU CỦA CHÍNH TÔI ĐÃ SAI

Sáng 27/08/2026 tôi ghi `DEBT-128` với câu: *«đổi email là **cắt đứt toàn bộ vai trò** của tài khoản đó»*.
**Điều đó SAI.** Tôi đọc thấy bảng nối bằng `user_email` rồi suy ra hậu quả, thay vì đọc hết ràng buộc.

Đọc kỹ lược đồ **và làm thực nghiệm** cùng ngày cho kết quả ngược lại:

```
TRƯỚC đổi email: gán vai trò cho email CŨ  = 1
SAU  đổi email : còn ở email CŨ = 0 · đã theo sang email MỚI = 1
⇒ vai trò THEO email mới — ON UPDATE CASCADE có hiệu lực THẬT
```

*(Thực nghiệm dựng một tài khoản thử, gán vai trò, đổi email, đo, rồi xoá.
`user_account` trước 15 · sau 15 — nguyên vẹn.)*

**Mức độ thật của khoản nợ này thấp hơn nhiều** so với bản ghi đầu. Nó **không** phải rủi ro
mất quyền; nó là rủi ro **nhất quán dữ liệu và quy kết lịch sử**.

---

## 1. LƯỢC ĐỒ HAI BẢNG LIÊN QUAN (`SHOW CREATE TABLE`)

```sql
CREATE TABLE `user_account` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `email` varchar(100) NOT NULL COMMENT 'Email đăng nhập',
  `password_hash` varchar(255) DEFAULT NULL,
  `ten` varchar(100) DEFAULT NULL,
  `ho_ten` varchar(100) DEFAULT NULL,
  `trang_thai` enum('active','inactive') DEFAULT 'active',
  `last_login` datetime DEFAULT NULL,
  `refresh_token` text DEFAULT NULL,
  `failed_login_attempts` int(11) NOT NULL DEFAULT 0,
  `locked_until` datetime DEFAULT NULL,
  `must_change_password` tinyint(1) NOT NULL DEFAULT 0,
  `password_changed_at` datetime DEFAULT NULL,
  `ngay_tao` timestamp NULL DEFAULT current_timestamp(),
  `ngay_sua` timestamp NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  PRIMARY KEY (`id`),
  UNIQUE KEY `email` (`email`),
  KEY `idx_email` (`email`)
) ENGINE=InnoDB AUTO_INCREMENT=1453 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `user_role_mapping` (
  `user_email` varchar(100) NOT NULL,
  `ma_vai_tro` varchar(20) NOT NULL,
  `ngay_gan` datetime NOT NULL DEFAULT current_timestamp(),
  `nguoi_gan` varchar(100) NOT NULL DEFAULT 'system',
  `ngay_sua` datetime DEFAULT NULL,
  `nguoi_sua` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`user_email`,`ma_vai_tro`),
  KEY `idx_urm_user` (`user_email`),
  KEY `idx_urm_role` (`ma_vai_tro`),
  CONSTRAINT `fk_urm_role` FOREIGN KEY (`ma_vai_tro`) REFERENCES `dm_vai_tro` (`ma_vai_tro`)
    ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `fk_urm_user` FOREIGN KEY (`user_email`) REFERENCES `user_account` (`email`)
    ON DELETE CASCADE ON UPDATE CASCADE      -- ← điều bản ghi nợ đầu đã bỏ sót
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

Điểm cần chú ý: **khoá chính của bảng nối là `(user_email, ma_vai_tro)`** — tức chính email
là một nửa định danh dòng. Đây là gốc của vấn đề, chứ không phải việc thiếu cascade.

---

## 2. XÁC MINH NỐI BẰNG EMAIL — ĐO, KHÔNG SUY

- `user_role_mapping.user_email` → `user_account.email`, **có** khoá ngoại, **có** cascade.
- **9 khoá ngoại** trỏ vào `user_account`, trong đó:

| Bảng · cột | Trỏ tới | Ghi chú |
|---|---|---|
| `hr_employee_nhanvien.user_id` | `user_account.id` | đã dùng mã ổn định |
| `user_session.user_id` | `user_account.id` | đã dùng mã ổn định |
| `permission_log.user_email` | `user_account.email` | nối bằng email |
| `audit_log.nguoi_thuc_hien` | `user_account.email` | nối bằng email |
| `hr_bang_luong.nguoi_chi_luong` | `user_account.email` | nối bằng email |
| `hr_bang_luong.nguoi_duyet` | `user_account.email` | nối bằng email |
| `hr_bang_luong.nguoi_tinh_luong` | `user_account.email` | nối bằng email |
| `user_role_mapping.user_email` | `user_account.email` | nối bằng email |
| `hr_luong_phu_cap.nguoi_duyet` | `user_account.email` | nối bằng email |

**2/9 đã dùng mã ổn định. 7/9 nối bằng email.**

⚠️ **Rủi ro thật nằm ngoài danh sách này:** đo được **99 cột chữ** mang email hoặc người-thao-tác
(`nguoi_tao` · `nguoi_sua` · `nguoi_gan` · `actor_email` · `target_email` …) trên **90 bảng**
mà **KHÔNG có khoá ngoại**. Chúng **không cascade** — đổi email thì chúng giữ giá trị cũ.

---

## 3. SỐ LƯỢNG · TRÙNG · MỒ CÔI (máy phát triển, 27/08/2026)

| Phép đo | Kết quả |
|---|---|
| `user_account` | **15** dòng |
| `user_role_mapping` | **19** dòng |
| Email trùng (không phân biệt hoa/thường) | **0** |
| Gán vai trò **mồ côi** (không có tài khoản khớp) | **0** |
| Gán khớp nhưng **lệch hoa/thường** | **0** |
| Email dài nhất đang có | **39** ký tự (giới hạn cột 100) |

**Dữ liệu sạch.** Không có ca biên nào cần xử lý riêng trước khi nạp bù.

> ⚠️ Số trên là của **máy phát triển**. Phải đo lại trên **máy vận hành** ngay trước khi di trú —
> đó là điều kiện bắt buộc ở mục 9.

---

## 4. NƠI ĐỌC / GHI

**38 tệp** nhắc tới `user_role_mapping`.

| Nhóm | Số tệp | Ghi chú |
|---|---|---|
| **Mã sản phẩm — GHI** | **1** | `src/lib/security-store.ts` — **điểm ghi duy nhất trong sản phẩm** |
| Mã sản phẩm — đọc | 3 | `src/app/api/m0/progress/route.ts` · `src/app/m3/thiet-ke/actions.ts` · `src/lib/m1-3-store.ts` |
| Kịch bản vận hành — ghi | 6 | các kịch bản `golive/` · `rbac/` · `gan-vai-tro-dich-danh` · `ap-ma-tran-dich` |
| Bộ kiểm — ghi | 14 | tự dựng rồi tự dọn |
| Bản di trú · khác | 14 | đọc / lịch sử |

**Điểm mạnh cho việc di trú:** sản phẩm chỉ có **MỘT** nơi ghi. Đọc-ghi kép chỉ phải sửa một chỗ.

---

## 5. ĐỀ XUẤT — CỘT `user_account_id` ỔN ĐỊNH

```sql
-- BƯỚC 1 — thêm cột, cho phép NULL. Không phá gì đang chạy.
ALTER TABLE user_role_mapping
  ADD COLUMN user_account_id INT NULL AFTER user_email;
```

Vì sao **không** đổi thẳng khoá chính ngay: khoá chính hiện là `(user_email, ma_vai_tro)`.
Đổi khoá chính là thao tác khoá bảng và **không lùi được nhanh**. Đi hai nhịp thì mỗi nhịp đều lùi được.

---

## 6. NẠP BÙ VÀ ĐỌC-GHI KÉP

```sql
-- BƯỚC 2 — nạp bù. Chạy được nhiều lần, không hỏng gì.
UPDATE user_role_mapping m
  JOIN user_account u ON u.email = m.user_email
   SET m.user_account_id = u.id
 WHERE m.user_account_id IS NULL;

-- BƯỚC 3 — ĐỐI CHỨNG HAI ĐẦU. Phải bằng 0 mới đi tiếp.
SELECT COUNT(*) AS chua_nap_bu FROM user_role_mapping WHERE user_account_id IS NULL;
SELECT COUNT(*) AS lech        FROM user_role_mapping m
  JOIN user_account u ON u.id = m.user_account_id
 WHERE u.email <> m.user_email;
```

**Đọc-ghi kép** — giai đoạn chuyển tiếp, sửa **đúng một tệp** (`src/lib/security-store.ts`):

| Giai đoạn | Ghi | Đọc |
|---|---|---|
| **A** (hiện nay) | chỉ `user_email` | chỉ `user_email` |
| **B** | **cả hai** cột | vẫn `user_email` |
| **C** | cả hai | **`user_account_id`**, đối chiếu chéo với email, lệch thì báo động |
| **D** | chỉ `user_account_id` | chỉ `user_account_id` |

Giữa B và C phải chạy lại phép đối chứng ở BƯỚC 3. Chỉ sang D khi C chạy êm **ít nhất một chu kỳ vận hành**.

---

## 7. KHOÁ NGOẠI · CHỈ MỤC · DUY NHẤT

```sql
-- BƯỚC 4 — chỉ chạy khi BƯỚC 3 cho 0/0.
ALTER TABLE user_role_mapping
  MODIFY COLUMN user_account_id INT NOT NULL,
  ADD CONSTRAINT fk_urm_user_id FOREIGN KEY (user_account_id)
      REFERENCES user_account (id) ON DELETE CASCADE ON UPDATE CASCADE,
  ADD UNIQUE KEY uk_urm_user_id_role (user_account_id, ma_vai_tro),
  ADD KEY idx_urm_user_id (user_account_id);
```

- **Giữ nguyên** `fk_urm_user` theo email trong suốt giai đoạn B–C. Hai khoá ngoại cùng tồn tại
  là **có chủ đích**: chúng kiểm chéo lẫn nhau.
- `uk_urm_user_id_role` là bản sao của khoá chính hiện tại theo mã ổn định — thêm **trước** khi
  bỏ khoá chính cũ, để không có khoảnh khắc nào bảng thiếu ràng buộc duy nhất.
- Bỏ khoá chính cũ **chỉ ở giai đoạn D**, và là một bản di trú riêng.

---

## 8. HOÀN TÁC

| Bước | Cách lùi | Mất mát |
|---|---|---|
| 1 (thêm cột) | `ALTER TABLE user_role_mapping DROP COLUMN user_account_id;` | không |
| 2 (nạp bù) | `UPDATE user_role_mapping SET user_account_id = NULL;` | không |
| 3 (đối chứng) | chỉ đọc | không |
| 4 (ràng buộc) | `DROP FOREIGN KEY fk_urm_user_id; DROP INDEX uk_urm_user_id_role; DROP INDEX idx_urm_user_id; MODIFY user_account_id INT NULL;` | không |
| B–C (mã) | lùi về bản trước — cột thừa không gây hại vì `fk_urm_user` theo email vẫn còn | không |
| D (bỏ khoá cũ) | **KHÔNG lùi nhanh được** ⇒ phải là bản di trú riêng, có sao lưu ngay trước, Owner duyệt riêng | có nếu sai |

**Bốn bước đầu đều lùi được sạch, không mất dữ liệu.**

---

## 9. CÁCH THỬ TRÊN BẢN SAO GIỐNG MÁY VẬN HÀNH

1. Sao lưu máy vận hành → dựng lại vào một CSDL rỗng cùng phiên bản MariaDB **10.11 LTS**.
2. Chạy lại **mục 3** trên bản sao đó — số của máy phát triển **không** dùng thay được.
3. Chạy BƯỚC 1 → 2 → 3. Điều kiện đi tiếp: `chua_nap_bu = 0` **và** `lech = 0`.
4. Chạy BƯỚC 4.
5. Chạy toàn bộ cổng liên quan quyền trên bản sao:
   `test:p1-menu` · `test:p1-ban-giao` · `test:quyen-vai-tro` · `test:m1-rbac` ·
   `test:p1-quan-tri-khoi-phuc` · `test:p1-hop-quyen-truong` · `test:ma-tran-quyen-hd`.
6. **Thử lùi**: chạy hết phần hoàn tác của mục 8, rồi chạy lại các cổng trên. Phải xanh y như trước.
7. Chỉ khi 1–6 sạch mới trình Owner xin duyệt chạy trên máy vận hành.

---

## 10. GIỚI HẠN CỦA LƯỢT NÀY

- **KHÔNG chạy DDL.** Mọi câu lệnh trên là **đề xuất**, chưa câu nào được chạy.
- **KHÔNG triển khai** phần `DEBT-128` nào.
- **99 cột chữ không có khoá ngoại** (mục 2) **không** nằm trong gói này. Chúng là vấn đề
  quy kết lịch sử, cần một quyết định riêng của Owner: *giữ nguyên giá trị lịch sử* (thường là
  đúng với nhật ký kiểm toán) hay *chuẩn hoá về mã ổn định*. **Không tự quyết.**

---

## 11. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC

- **Số liệu máy vận hành.** Mọi con số ở mục 3 là của máy phát triển. Chưa đo máy vận hành.
- **Hành vi cascade trên máy vận hành.** Thực nghiệm chạy trên máy phát triển. Cùng phiên bản
  MariaDB nên khả năng giống là cao, nhưng **chưa đo**.
- **Chi phí khoá bảng ở BƯỚC 4** với dữ liệu thật. Máy phát triển chỉ có 19 dòng nên không đo được.
