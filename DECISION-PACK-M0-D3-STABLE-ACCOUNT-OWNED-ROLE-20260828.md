# GÓI QUYẾT ĐỊNH — `D3`: VAI TRÒ RIÊNG BÁM ĐỊNH DANH TÀI KHOẢN ỔN ĐỊNH

**Mã việc:** `WP-ERP-M0-D3-STABLE-ACCOUNT-OWNED-ROLE-DECISION-PACK-20260828`
**Ngày:** 28/08/2026
**Loại:** RÀ SOÁT + GÓI QUYẾT ĐỊNH — **CHƯA TRIỂN KHAI**
**Thay thế:** `DECISION-PACK-M0-D3-VAI-TRO-RIENG-20260827.md` (xem §2.3)

---

## 1. KẾT LUẬN ĐIỀU HÀNH

**Khuyến nghị: `GO_D3_STABLE_ACCOUNT_OWNED_ROLE`.**

Ba lý do, đều đo được chứ không phải suy đoán:

1. **Định danh ổn định ĐÃ CÓ SẴN và đã chạy.** `user_account.id` là `int(11)` khoá chính
   tự tăng; phiên đăng nhập **đã** phân giải theo nó (`user_session.user_id` nối
   `user_account.id`, và `resolveCurrentSession` trả về `userId`). D3 **không phải phát minh**
   định danh ổn định — chỉ phải *dùng* cái đang có.

2. **Nạp bù bằng 0.** Toàn bộ **9 vai trò** hiện có đều là vai trò dùng chung; chưa có khái niệm
   vai trò riêng nên **không dòng nào cần suy ra chủ sở hữu**. Hai cột mới đều có giá trị mặc định
   ⇒ bản di trú **không phải đoán, không có dòng nhập nhằng, không có dòng mồ côi**.
   Đây là điểm khác căn bản so với lo ngại thường gặp khi thêm quyền sở hữu.

3. **Đường lùi sạch.** `dm_vai_tro` có **9 dòng**. Thêm cột · khoá ngoại · ràng buộc kiểm,
   rồi gỡ ra, không mất dữ liệu nào có trước.

**Một điều gói này KHÔNG hứa:** khoá ngoại **không** tự bảo đảm *"vai trò riêng của A không bị gán
cho B"*. Lý do kỹ thuật chính xác ở §14. Bất biến đó **bắt buộc** phải do máy chủ giữ, và gói này
chỉ rõ phải đặt nó ở đâu để **mọi** đường ghi đi qua.

---

## 2. PHẠM VI

### 2.1 Trong phạm vi
Rà chỉ đọc lược đồ · dữ liệu · mã · tài liệu; đề xuất kiến trúc; DDL đề xuất; kế hoạch di trú,
diễn tập, hoàn tác; ma trận nghiệm thu; một câu hỏi quyết định cho Owner.

### 2.2 NGOÀI phạm vi — đã tuân thủ
**0** thay đổi mã ứng dụng · **0** thay đổi mã kiểm thử · **0** câu DDL đã chạy · **0** di trú dữ liệu ·
**0** lần triển khai · **0** thay đổi cấu hình vận hành · **0** thay đổi quyền tài khoản thật ·
không mở module/luồng việc mới · **không tự đánh dấu `D3` là đã đóng**.

### 2.3 Vì sao gói cũ bị thay
Gói `20260827` đưa ra hai phương án:
- **A** — quy ước đặt tên `RIENG__<mã người>`: chủ sở hữu nằm trong chuỗi ký tự, không ràng buộc.
- **B** — thêm cột **`chu_so_huu_email VARCHAR(100)`** với khoá ngoại tới `user_account.email`.

**Cả hai đều trái quyết định Owner đã khoá sau đó** (*"role/tùy chỉnh riêng phải bám stable account
ID, không bám email"*). Phương án B tuy có khoá ngoại nhưng vẫn **bám email** — chính thứ Owner bác.
Gói này thay bằng **`chu_so_huu_user_id INT(11)`** → `user_account.id`.

---

## 3. QUYẾT ĐỊNH OWNER ĐÃ KHOÁ — KHÔNG HỎI LẠI

| # | Nội dung |
|---|---|
| 3.1 | Hỗ trợ **CẢ HAI**: vai trò mẫu dùng chung **và** tuỳ chỉnh riêng từng tài khoản |
| 3.2 | Nhiều vai trò ⇒ **hợp các quyền được cấp**; giữa các lớp độc lập ⇒ **AND** |
| 3.3 | `USER` = `PROVISIONING_PENDING`; chưa cấp vai trò nghiệp vụ ⇒ không menu, không hành động |
| 3.4 | Luôn phải còn ít nhất một quản trị hợp lệ; `DEV` chỉ là đường phá kính; không cửa hậu |
| 3.5 | Quyền riêng bám **định danh tài khoản ổn định**, không bám email; đổi email không mất quyền; tài khoản mới dùng lại email cũ **không** kế thừa |
| 3.6 | Báo cáo công khai được nêu tên bảng/cột, **cấm** nêu email thật, mật khẩu, token, dữ liệu khách hàng |

---

## 4. ĐỊNH DANH KHO MÃ VÀ BẢN ĐANG CHẠY

| Thành phần | Đo được | Nhãn |
|---|---|---|
| Gốc kho | `D:/Viber Coding Webapp/TanPhat ERP` | CODE_PROVEN |
| Nhánh · HEAD | `main` · `4af6062` | CODE_PROVEN |
| Remote `main` | `4af6062` — trùng local | CODE_PROVEN |
| Cây làm việc | **0 thay đổi · 0 tệp chưa theo dõi** | CODE_PROVEN |
| Kho báo cáo | `91bc68d` · 0 thay đổi | CODE_PROVEN |
| Phiên bản mã nguồn | `V1.00.359` | CODE_PROVEN |
| **Nội dung đã triển khai** | `ce6dadf` · `V1.00.359` | REPORT_PROVEN |
| VPS Git metadata HEAD | `826817b` | REPORT_PROVEN |
| MariaDB vận hành | **`10.11.10-MariaDB-log`** | DB_PROVEN |
| Số bảng | 101 | DB_PROVEN |

> ⚠️ **Lệch so với mốc nêu trong chỉ thị, đã truy nguyên nhân.** Chỉ thị ghi `local/main = 98c4e95`.
> Đo được `4af6062`. Giữa `ce6dadf` (nội dung đã triển khai) và `HEAD` có **3 commit**, và
> `git diff --name-only ce6dadf..HEAD -- src/ migrations/ package.json` cho **0 tệp**.
> Ba commit đó gồm: một commit **chỉ sửa bộ kiểm** (`98c4e95`) và hai commit **chỉ sửa tài liệu**
> (`b261c39`, `4af6062` — báo cáo M3 + sổ Owner + sổ nợ, viết SAU khi chỉ thị được soạn).
>
> **Nhãn hội tụ chính xác:**
> `RUNTIME_RELEASE_CONVERGED_WITH_TEST_AND_DOC_ONLY_REPO_DELTA + METADATA_DIFF`
> — chặt hơn nhãn chỉ thị đề nghị, vì delta gồm cả tài liệu chứ không chỉ bộ kiểm.

---

## 5. LƯỢC ĐỒ CHÍNH XÁC — ĐO TRỰC TIẾP TỪ MÁY VẬN HÀNH

### 5.1 `user_account` — nơi định danh ổn định đang nằm

| Cột | Kiểu | Null | Khoá | Mặc định | Thêm |
|---|---|---|---|---|---|
| **`id`** | **`int(11)`** (CÓ dấu) | NO | **PRI** | NULL | **`auto_increment`** |
| `email` | `varchar(100)` | NO | **UNI** | NULL | — |
| `password_hash` | `varchar(255)` | YES | — | NULL | — |
| `ten` · `ho_ten` | `varchar(100)` | YES | — | NULL | — |
| `trang_thai` | `enum('active','inactive',…)` | YES | — | `'active'` | — |
| `last_login` | `datetime` | YES | — | NULL | — |
| `refresh_token` | `text` | YES | — | NULL | — |
| `failed_login_attempts` | `int(11)` | NO | — | `0` | — |
| `locked_until` | `datetime` | YES | — | NULL | — |
| `must_change_password` | `tinyint(1)` | NO | — | `0` | — |
| `password_changed_at` | `datetime` | YES | — | NULL | — |
| `ngay_tao` | `timestamp` | YES | — | `current_timestamp()` | — |
| `ngay_sua` | `timestamp` | YES | — | `current_timestamp()` | `on update current_timestamp()` |

**Chỉ mục:** `PRIMARY(id)` · `email(email)` UNIQUE · `idx_email(email)` (không unique — **trùng lặp
với chỉ mục UNIQUE ở trên**; ghi nhận, không sửa trong gói này).
**Không có** cột xoá-mềm ngoài `trang_thai`. `ENGINE=InnoDB`, `utf8mb4_unicode_ci`. *(DB_PROVEN)*

### 5.2 `dm_vai_tro`

| Cột | Kiểu | Null | Khoá | Mặc định |
|---|---|---|---|---|
| **`ma_vai_tro`** | **`varchar(20)`** | NO | **PRI** | NULL |
| `ten_vai_tro` | `varchar(100)` | NO | — | NULL |
| `mo_ta` | `text` | YES | — | NULL |
| `la_admin` | `tinyint(1)` | NO | — | `0` |
| `ngay_tao` | `datetime` | NO | — | `current_timestamp()` |
| `nguoi_tao` | `varchar(100)` | NO | — | `'system'` |
| `ngay_sua` · `nguoi_sua` | `datetime` · `varchar(100)` | YES | — | NULL |

**`la_vai_tro_rieng` và `chu_so_huu_user_id` KHÔNG TỒN TẠI.** Quét toàn CSDL cho cột mang nghĩa
riêng/chủ sở hữu (`%rieng%` · `%private%` · `%owner%` · `%chu_so_huu%` · `%owned%`) ⇒ **0 kết quả**.
Mọi nhắc tới hai tên đó trong tài liệu này là **`PROPOSED_NOT_EXECUTED`**. *(DB_PROVEN)*

### 5.3 `user_role_mapping` — **khoá theo EMAIL, đây là mấu chốt**

| Cột | Kiểu | Null | Khoá |
|---|---|---|---|
| **`user_email`** | **`varchar(100)`** | NO | **PRI (phần 1)** |
| **`ma_vai_tro`** | **`varchar(20)`** | NO | **PRI (phần 2)** |
| `ngay_gan` · `nguoi_gan` | `datetime` · `varchar(100)` | NO | — |
| `ngay_sua` · `nguoi_sua` | `datetime` · `varchar(100)` | YES | — |

**KHÔNG có cột `user_id`.** Bảng gán quyền nối tới tài khoản **bằng email**. *(DB_PROVEN)*

### 5.4 Khoá ngoại liên quan

| Khoá ngoại | ON DELETE | ON UPDATE |
|---|---|---|
| `user_role_mapping.user_email` → `user_account.email` | **CASCADE** | **CASCADE** |
| `user_role_mapping.ma_vai_tro` → `dm_vai_tro.ma_vai_tro` | CASCADE | CASCADE |
| `role_menu_permission.ma_vai_tro` → `dm_vai_tro.ma_vai_tro` | CASCADE | CASCADE |
| `role_action_permission.ma_vai_tro` → `dm_vai_tro.ma_vai_tro` | CASCADE | CASCADE |
| `role_field_permission.ma_vai_tro` → `dm_vai_tro.ma_vai_tro` | CASCADE | CASCADE |
| `role_data_permission.ma_vai_tro` → `dm_vai_tro.ma_vai_tro` | CASCADE | CASCADE |
| **`user_session.user_id` → `user_account.id`** | CASCADE | CASCADE |
| **`hr_employee_nhanvien.user_id` → `user_account.id`** | SET NULL | CASCADE |
| `permission_log.user_email` → `user_account.email` | RESTRICT | CASCADE |
| `audit_log.nguoi_thuc_hien` → `user_account.email` | RESTRICT | RESTRICT |
| `hr_bang_luong.*` (3 cột) → `user_account.email` | RESTRICT | CASCADE |
| `hr_luong_phu_cap.nguoi_duyet` → `user_account.email` | SET NULL | CASCADE |

**Chỉ hai cột trong toàn CSDL nối tới tài khoản bằng `id`:** `user_session.user_id` và
`hr_employee_nhanvien.user_id` — **cả hai đều `int(11)` có dấu**, khớp `user_account.id`.
Đó là **khuôn mẫu sẵn có** để cột mới noi theo. *(DB_PROVEN)*

### 5.5 Ràng buộc kiểm (CHECK) — cơ chế đã được dùng thật

Toàn CSDL vận hành có **31 ràng buộc CHECK đang hiệu lực** (tất cả dạng `json_valid(...)`).
Nghĩa là MariaDB `10.11.10` ở đây **thi hành CHECK thật**, và đây **không phải** mẫu hình mới lạ
với lược đồ này. Ba bảng định danh hiện có **0** ràng buộc CHECK. *(DB_PROVEN)*

**Số trigger trong CSDL: 0.** Dự án chưa dùng trigger ở đâu cả — điều này ảnh hưởng tới lựa chọn
ở §14. *(DB_PROVEN)*

### 5.6 Đối chiếu lược đồ hai môi trường

Ba bảng định danh, so từng cột và từng khoá ngoại giữa **máy phát triển** và **máy vận hành**:

| | máy phát triển | máy vận hành | lệch |
|---|---|---|---|
| Cột | 28 | 28 | **0** |
| Khoá ngoại (`user_role_mapping`) | 2 | 2 | **0** |

⇒ **Không có điều kiện dừng** về mâu thuẫn lược đồ. *(DB_PROVEN)*

---

## 6. HÌNH DẠNG DỮ LIỆU — SỐ ĐẾM, KHÔNG PII

### 6.1 Tài khoản (máy vận hành)

| Chỉ tiêu | Giá trị |
|---|---|
| Tổng tài khoản | **9** |
| `active` / `inactive` | **8 / 1** |
| Đang bị khoá (`locked_until` còn hiệu lực) | **0** |
| Không có mật khẩu | **0** |
| Buộc đổi mật khẩu | 8 |
| `id` nhỏ nhất / lớn nhất | **1 / 9** |
| `id` NULL hoặc ≤ 0 | **0** |
| Email rỗng · trùng sau chuẩn hoá · thừa khoảng trắng · có chữ HOA | **0 · 0 · 0 · 0** |

⇒ **Không có tài khoản nào thiếu định danh ổn định.** Không có va chạm email sau chuẩn hoá.

### 6.2 Vai trò

| Mã | quản trị | số lượt gán | menu | hành động | trường | tạo bởi |
|---|---|---|---|---|---|---|
| `ADMIN` | **1** | 3 | 51 | 12 | 0 | hệ thống |
| `CEO` | 0 | 1 | 43 | 12 | 4 | hệ thống |
| `HR` | 0 | **0** | 4 | 3 | 0 | hệ thống |
| `KE_TOAN` | 0 | 1 | 13 | 6 | 0 | hệ thống |
| `SALES` | 0 | 3 | 10 | 17 | 0 | hệ thống |
| `TP_KINH_DOANH` | 0 | **0** | 8 | 10 | 0 | người |
| `TP_SAN_XUAT` | 0 | **0** | 11 | 0 | 0 | người |
| `TP_THIET_KE` | 0 | **0** | 7 | 7 | 0 | người |
| `USER` | 0 | 3 | 1 | 0 | 0 | hệ thống |

Tổng **9 vai trò**: 1 vai trò quản trị · 6 do hệ thống tạo · 3 do người tạo ·
**4 vai trò chưa ai được gán** · 0 vai trò trùng tên · 0 vai trò không có dòng quyền nào ·
mã dài nhất đang dùng **13/20 ký tự** (còn 7 ký tự trống).

### 6.3 Gán vai trò

| Chỉ tiêu | Giá trị |
|---|---|
| Tổng lượt gán | **11** |
| Tài khoản có ≥1 vai trò | **9 / 9** |
| Tài khoản không có vai trò nào | **0** |
| Tài khoản có **nhiều** vai trò | **2** |
| Gán mồ côi (email không có tài khoản) | **0** |
| Gán trỏ vai trò không tồn tại | **0** |

### 6.4 Quản trị và khả năng phục hồi

| Chỉ tiêu | Giá trị |
|---|---|
| Tài khoản mang vai trò quản trị | **3** |
| …trong đó `active` **và** không bị khoá | **3** |
| …trong đó **có mật khẩu** (đăng nhập được) | **3** |
| Mã vai trò quản trị | `ADMIN` (duy nhất) |
| Tài khoản `active` **không** có mật khẩu | **0** |

⇒ Hiện có **3 quản trị thật sự khôi phục được**. Biên an toàn tốt.

---

## 7. KIỂM KÊ ĐƯỜNG ĐỌC / GHI

### 7.1 Bảng đường ghi (mã sản phẩm, không tính kịch bản và bộ kiểm)

| Đường ghi | Cửa vào | Ghi bảng | Giao dịch | Khoá dòng | Chốt quyền | Chốt quản trị cuối |
|---|---|---|---|---|---|---|
| `assignRoleToUser` | `m0/security/actions.ts::assignRoleAction` | `user_role_mapping` | **KHÔNG** | KHÔNG | ở tầng hành động | **KHÔNG** |
| `removeRoleFromUser` | `…::removeRoleAction` | `user_role_mapping` | **KHÔNG** | KHÔNG | ở tầng hành động | **CÓ** |
| `createRoleDefinition` | `…::createRoleAction` | `dm_vai_tro` | **KHÔNG** | KHÔNG | ở tầng hành động | KHÔNG (không cần) |
| `upsertRoleMenuPermission` | `…::upsertRoleMenuPermissionAction` | `role_menu_permission` | **KHÔNG** | KHÔNG | ở tầng hành động | KHÔNG |
| `upsertRoleActionPermission` | `…::upsertRoleActionPermissionAction` | `role_action_permission` | **KHÔNG** | KHÔNG | ở tầng hành động + store | KHÔNG |
| `setUserLockState` | `…::lockUserAction` / `unlockUserAction` | `user_account` | **KHÔNG** | KHÔNG | ở tầng hành động | **CÓ** (khi khoá) |
| `activateUserAccount` | `…::activateUserAccountAction` | `user_account` | **KHÔNG** | KHÔNG | ở tầng hành động | KHÔNG (không giảm) |
| `setUserPassword` | `api/auth/change-password` · `m0/security` | `user_account` | **KHÔNG** | KHÔNG | có (phiên/quản trị) | KHÔNG |
| `createInactiveUserAccount` | **`m1/nhan-su/actions.ts`** | `user_account` | **KHÔNG** | KHÔNG | **`m1_nhan_su` + 2 hành động HR — KHÔNG đòi quản trị** | KHÔNG (không giảm) |
| `issueSession` · `clearSession` · `revokeAllSessionsForUser` · `verifyUserPassword` | đăng nhập/đăng xuất | `user_account` · `user_session` | KHÔNG | KHÔNG | theo phiên | KHÔNG |

**Tổng: 14 hàm ghi trong 2 tệp mã sản phẩm** (`src/lib/security-store.ts`,
`src/lib/security-session-store.ts`). **0/14 dùng giao dịch.** *(CODE_PROVEN)*

### 7.2 Ba kết luận kiến trúc rút ra

1. **Chốt quyền nằm ở TẦNG HÀNH ĐỘNG, không ở tầng lưu trữ.** `m0/security/actions.ts` gọi
   `requireActionPermission("m0","update")` **và** `requireAdminContext()` (12 lần trên 10 hàm).
   Tầng store **không tự kiểm quyền**. ⇒ Bất kỳ đường ghi nào **không** đi qua tầng hành động
   sẽ **không** có chốt.

2. **Đã tồn tại đường ghi thứ hai với chốt KHÁC.** `m1/nhan-su/actions.ts` gọi thẳng
   `createInactiveUserAccount` và `setUserLockState`, gác bằng `m1_nhan_su` + hai hành động HR —
   **không đòi quản trị**. Thiết kế này hợp lý cho nghiệp vụ nhân sự, nhưng chứng minh rằng
   **đặt bất biến sở hữu ở tầng hành động là KHÔNG ĐỦ**.

3. **Đường mẫu nhanh đang đóng an toàn.** `apMauQuyenAction` đã tạm ngưng; thân cũ giữ nguyên văn
   ở `thanApMauQuyen_TAM_NGUNG`, lý do dùng chung ở `tam-ngung.ts`. Không phải lo trong D3.
   *(CODE_PROVEN)*

### 7.3 Chốt chặn quản trị cuối — hình dạng chính xác

```
demQuanTriKhoiPhucDuoc(loaiTruEmail?)  →  COUNT(DISTINCT urm.user_email)
   FROM user_role_mapping urm
   JOIN dm_vai_tro dvt  ON dvt.ma_vai_tro = urm.ma_vai_tro
   JOIN user_account ua ON ua.email       = urm.user_email
  WHERE IFNULL(dvt.la_admin,0) = 1
    AND ua.trang_thai = 'active'
    AND (ua.locked_until IS NULL OR ua.locked_until <= NOW())
```

Gọi từ **đúng hai** đường: `setUserLockState(lock=true)` và `removeRoleFromUser`.

**Đó cũng là đúng hai đường lúc chạy có thể làm giảm số quản trị**, vì:
- **không có** đường vô hiệu hoá tài khoản lúc chạy (chỉ có `activateUserAccount`);
- **không có** đường `DELETE FROM user_account` lúc chạy;
- **không có** đường `DELETE`/`UPDATE dm_vai_tro` lúc chạy ⇒ `la_admin` không đổi được. *(CODE_PROVEN)*

> ⚠️ **Một lỗ tiềm ẩn, hiện KHÔNG lộ:** phép đếm **không kiểm `password_hash`**. Một tài khoản
> `active`, không khoá, **nhưng chưa có mật khẩu** vẫn được tính là "khôi phục được", trong khi
> nó **không đăng nhập được**. Đo trên máy vận hành: **0 tài khoản** như vậy, và **0** trong số đó
> mang vai trò quản trị ⇒ **phơi nhiễm hiện tại bằng 0**. Nhưng `createInactiveUserAccount` tạo
> tài khoản **không mật khẩu**, nên tình huống đó **có thể phát sinh**. Xem §15.

---

## 8. ĐỊNH DANH HIỆN HÀNH VÀ PHỤ THUỘC EMAIL

### 8.1 Định danh ổn định ĐÃ chảy qua phiên đăng nhập

```
user_session.user_id ──FK──> user_account.id          (int(11), ON DELETE CASCADE)

lookupSessionRowByTokenHash():
    SELECT us.id, us.user_id, ua.email, …
      FROM user_session us
INNER JOIN user_account ua ON ua.id = us.user_id      ← nối bằng ID
     WHERE us.session_token_hash = ?

resolveCurrentSession()  →  { sessionPk, userId, email }
```

**`ResolvedSession.userId` mang đúng định danh ổn định.** *(CODE_PROVEN)*

### 8.2 …nhưng tầng phân quyền vứt bỏ nó

```
resolveSessionEmail()  →  chỉ trả  current?.email
getSecurityContext(email)  →  SELECT … FROM user_account WHERE email = ?
                           →  vai trò tra bằng user_role_mapping.user_email
```

Đếm được: **`.userId` xuất hiện 0 lần** trong mã quyết định quyền ở `src/lib`.
**13 nơi** gọi `resolveSessionEmail()`. Toàn bộ chuỗi menu → hành động → trường → chuyển trạng thái
đều tra theo **email**. *(CODE_PROVEN)*

### 8.3 Đổi email và dùng lại email — hành vi THẬT

| Tình huống | Hành vi thật | Bằng chứng |
|---|---|---|
| Đổi email | Gán vai trò **THEO** email mới | `fk_urm_user … ON UPDATE CASCADE`; thực nghiệm 27/08 cho **1/1** |
| Xoá tài khoản | Gán vai trò bị **xoá theo** | `ON DELETE CASCADE` |
| Tài khoản mới dùng lại email cũ | **Không kế thừa** vai trò | tài khoản cũ phải bị xoá trước (email UNIQUE), mà xoá thì mapping đã mất; **và không có đường xoá lúc chạy** |
| Lịch sử/quy kết | **Kế thừa nhầm về mặt hiển thị** | 113 cột chữ mang email/người-thao-tác, chỉ **5** có khoá ngoại ⇒ **108 cột giữ giá trị cũ** |

⇒ Rủi ro **mất quyền** khi đổi email: **không có**.
Rủi ro **quy kết lịch sử lệch**: **có thật**, và đó chính là phần lõi còn lại của `DEBT-128`.
*(DB_PROVEN + CODE_PROVEN)*

---

## 9. QUAN HỆ VỚI `DEBT-128`

**Kết luận: phương án 2 — `D3` đóng phần SỞ HỮU VAI TRÒ; `DEBT-128` vẫn còn phụ thuộc khác.**

| Phần của `DEBT-128` | `D3` có đóng không |
|---|---|
| Vai trò riêng bám email | ✅ **ĐÓNG** — chuyển sang `chu_so_huu_user_id` |
| Gán vai trò (`user_role_mapping`) khoá theo email | ❌ **KHÔNG** — cần đổi khoá bảng, phạm vi lớn hơn nhiều |
| Chuỗi phân quyền tra theo email | ❌ **KHÔNG** — 13 nơi gọi, chạm mọi resolver |
| 108 cột chữ quy kết không khoá ngoại | ❌ **KHÔNG** — thuần dữ liệu/quy kết |

**KHÔNG được đóng `DEBT-128` khi triển khai `D3`.** Sau `D3`, `DEBT-128` phải được viết lại phạm vi
còn lại đúng ba dòng ❌ ở trên. *(DB_PROVEN + CODE_PROVEN)*

**Có nên gộp `D3` và `DEBT-128` thành một bản phát hành không? — KHÔNG.**
`D3` là **thêm hai cột, nạp bù bằng 0**. `DEBT-128` phần còn lại là **đổi khoá chính của bảng gán
quyền + sửa 13 điểm gọi + nạp bù 108 cột**. Gộp lại là biến một bản phát hành rủi ro thấp thành
một bản phát hành rủi ro cao, và làm mất khả năng lùi từng phần.

---

## 10. MÔ HÌNH ĐE DOẠ

| # | Kịch bản | Chặn được nhờ | Còn hở? |
|---|---|---|---|
| T1 | Gán vai trò riêng của A cho B qua giao diện | chốt máy chủ (§14) | không, **nếu** chốt đặt đúng tầng |
| T2 | …qua gọi thẳng hành động máy chủ, bỏ qua giao diện | cùng chốt đó | không |
| T3 | …qua kịch bản nạp/di trú | **hiện KHÔNG có chốt** — kịch bản ghi thẳng SQL | **CÓ** → §14.4 |
| T4 | Sửa gói dữ liệu để đổi `chu_so_huu_user_id` | chỉ đường quản trị mới ghi được cột này | không |
| T5 | Đổi email của A để chiếm vai trò riêng | sở hữu bám `id`, không bám email | không |
| T6 | Tạo tài khoản mới trùng email cũ để kế thừa | email UNIQUE + không có đường xoá lúc chạy | không |
| T7 | Xoá tài khoản chủ sở hữu để làm vai trò mồ côi | `ON DELETE RESTRICT` (§13) | không |
| T8 | Vô hiệu hoá chủ sở hữu nhưng vai trò vẫn cấp quyền | chuỗi phiên đã kiểm `trang_thai` (§8.1) | không |
| T9 | Gỡ quản trị cuối qua vai trò riêng | chốt quản trị cuối hiện có | không |
| T10 | Hai quản trị cùng sửa một mục tiêu | **hiện KHÔNG có giao dịch nào** | **CÓ** → §11.7 |
| T11 | Vai trò riêng vượt qua lớp menu/hành động/trường | các lớp vẫn AND (Owner khoá 3.2) | không |
| T12 | Quản trị tự đặt mình làm chủ sở hữu vai trò của người khác | tách bạch *người thực hiện* và *mục tiêu* (§11.4) | không |

---

## 11. BẤT BIẾN BẮT BUỘC

**B1 — Vai trò dùng chung.** Không thuộc riêng ai; gán được cho nhiều tài khoản; sửa nó ảnh hưởng
mọi người đang mang nó ⇒ giao diện **phải cảnh báo phạm vi ảnh hưởng trước khi sửa**
(số tài khoản chịu ảnh hưởng đã có sẵn hàm đếm: `demTaiKhoanChiuAnhHuong`).

**B2 — Vai trò riêng.**
`la_vai_tro_rieng = 1` ⇒ `chu_so_huu_user_id` **BẮT BUỘC** không NULL.
`la_vai_tro_rieng = 0` ⇒ `chu_so_huu_user_id` **BẮT BUỘC** NULL.
Chủ sở hữu là **`user_account.id`**, không bao giờ là email.
Vai trò riêng của A **chỉ** được gán cho A.

**B3 — Bất biến phía máy chủ.**
```
NẾU  vai_tro.la_vai_tro_rieng = 1
THÌ  gan.tai_khoan_id PHẢI BẰNG vai_tro.chu_so_huu_user_id
NGƯỢC LẠI  áp chính sách vai trò dùng chung
```
Phải áp cho: hành động máy chủ · tuyến API · giao diện quản trị · mẫu nhanh · nạp/nhập ·
di trú · dịch vụ nội bộ · gán hàng loạt. **Không được chỉ khoá ở giao diện.**

**B4 — Tạo tuỳ chỉnh cho người khác.** Quản trị cấu hình quyền riêng cho một tài khoản thì:
vai trò riêng mới có **chủ sở hữu là tài khoản đích**; quản trị là **người thực hiện**, không tự
thành chủ sở hữu; **không** lấy vai trò riêng của A gán cho B; sao chép từ mẫu ⇒ **tạo vai trò riêng
MỚI** thuộc B; nhật ký ghi **tách bạch** người thực hiện và mục tiêu.

**B5 — Vòng đời.** Xem §16.

**B6 — Quản trị cuối.** Xem §15.

**B7 — Giao dịch và tranh chấp.** Xem §11.7 dưới đây.

### 11.7 Ranh giới giao dịch bắt buộc (hiện **chưa có**)

Mọi thao tác đổi vai trò phải nằm trong **một** giao dịch, theo thứ tự khoá cố định để tránh kẹt:

```
1. SELECT … FROM user_account  WHERE id = :tai_khoan_dich   FOR UPDATE
2. SELECT … FROM dm_vai_tro    WHERE ma_vai_tro = :ma        FOR UPDATE
3. kiểm quyền người thực hiện
4. kiểm bất biến sở hữu (B3)
5. kiểm quản trị cuối (nếu thao tác làm giảm)
6. GHI
7. ghi nhật ký
```

Thứ tự **tài khoản trước, vai trò sau** — cố định cho mọi đường, để hai giao dịch không bắt tay chéo.
Kiểm phải nằm **cùng giao dịch** với ghi, nếu không vẫn hở TOCTOU.
Hỏng ở bất kỳ bước nào ⇒ **hoàn tác toàn bộ**.

Hôm nay **0/14 hàm ghi có giao dịch** ⇒ đây là phần việc thật của bản triển khai, không phải
tô điểm. *(CODE_PROVEN)*

---

## 12. KIẾN TRÚC KHUYẾN NGHỊ — MỘT PHƯƠNG ÁN DUY NHẤT

**Vai trò do tài khoản sở hữu, đánh dấu bằng cờ + khoá ngoại theo `id`, bất biến gán do máy chủ giữ.**

| Thành phần | Quyết định |
|---|---|
| Đánh dấu riêng/chung | `dm_vai_tro.la_vai_tro_rieng TINYINT(1) NOT NULL DEFAULT 0` |
| Chủ sở hữu | `dm_vai_tro.chu_so_huu_user_id INT(11) NULL` → `user_account(id)` |
| Nhất quán cờ ↔ chủ sở hữu | **ràng buộc CHECK cùng dòng** (DB giữ được) |
| Chủ sở hữu tồn tại | **khoá ngoại** (DB giữ được) |
| Gán đúng chủ | **máy chủ giữ**, ở tầng lưu trữ dùng chung (DB không giữ được — §14) |
| Khoá bảng gán quyền | **GIỮ NGUYÊN theo email** trong lượt này |
| Sinh mã vai trò riêng | `R<id>_<số thứ tự>`, ví dụ `R7_01` — bám `id`, **không** bám email; vừa `varchar(20)` (hiện dùng nhiều nhất 13/20) |

**Vì sao không đổi khoá `user_role_mapping` sang `user_id` trong lượt này:** đó là thay khoá chính
của bảng gán quyền, kéo theo 13 điểm gọi và toàn bộ chuỗi phân giải. Thuộc `DEBT-128`, không thuộc
`D3`. Gộp vào sẽ biến bản phát hành rủi ro thấp thành rủi ro cao.

---

## 13. DDL ĐỀ XUẤT — **CHƯA CHẠY** (`PROPOSED_NOT_EXECUTED`)

```sql
-- ══════════════════════════════════════════════════════════════════════
-- D3 — VAI TRÒ RIÊNG BÁM ĐỊNH DANH TÀI KHOẢN ỔN ĐỊNH
-- TRẠNG THÁI: ĐỀ XUẤT — CHƯA CHẠY Ở BẤT KỲ MÔI TRƯỜNG NÀO
-- Đích: MariaDB 10.11.10 · InnoDB · utf8mb4_unicode_ci
-- ══════════════════════════════════════════════════════════════════════

ALTER TABLE `dm_vai_tro`
  ADD COLUMN `la_vai_tro_rieng`   TINYINT(1) NOT NULL DEFAULT 0
      COMMENT '1 = vai tro rieng cua mot tai khoan; 0 = vai tro dung chung',
  ADD COLUMN `chu_so_huu_user_id` INT(11)    NULL
      COMMENT 'user_account.id cua chu so huu. BAT BUOC khi la_vai_tro_rieng=1';

-- Kiểu PHẢI khớp `user_account.id` = int(11) CÓ DẤU (đã đo, không suy đoán).
ALTER TABLE `dm_vai_tro`
  ADD CONSTRAINT `fk_vai_tro_chu_so_huu`
      FOREIGN KEY (`chu_so_huu_user_id`) REFERENCES `user_account` (`id`)
      ON DELETE RESTRICT
      ON UPDATE RESTRICT;

-- Nhất quán cờ ↔ chủ sở hữu. Cùng dòng nên CHECK giữ được.
ALTER TABLE `dm_vai_tro`
  ADD CONSTRAINT `chk_vai_tro_rieng_co_chu`
      CHECK (
        (`la_vai_tro_rieng` = 0 AND `chu_so_huu_user_id` IS NULL)
        OR
        (`la_vai_tro_rieng` = 1 AND `chu_so_huu_user_id` IS NOT NULL)
      );

-- Tra "mọi vai trò riêng của một người" — dùng ở màn phân quyền và ở bước dọn mồ côi.
ALTER TABLE `dm_vai_tro`
  ADD INDEX `idx_vai_tro_chu_so_huu` (`chu_so_huu_user_id`, `la_vai_tro_rieng`);
```

### 13.1 Vì sao `ON DELETE RESTRICT`, không phải `SET NULL` / `CASCADE`

| Lựa chọn | Điều gì xảy ra khi xoá tài khoản chủ sở hữu | Đánh giá |
|---|---|---|
| `SET NULL` | `chu_so_huu_user_id → NULL` trong khi `la_vai_tro_rieng` vẫn `1` ⇒ **vi phạm chính ràng buộc CHECK** ⇒ lệnh xoá hỏng với lỗi CHECK khó hiểu | ❌ **không tương thích** với CHECK ở trên |
| `CASCADE` | Xoá tài khoản ⇒ **xoá luôn vai trò** ⇒ CASCADE tiếp xuống 4 bảng quyền ⇒ **mất sạch dấu vết** | ❌ mất dữ liệu kiểm toán |
| Chỉ xoá mềm | Không có ràng buộc DB nào; phụ thuộc hoàn toàn kỷ luật mã | ❌ đúng loại lỗi đang muốn tránh |
| **`RESTRICT`** | Lệnh xoá **bị chặn ngay và rõ ràng**; buộc xử lý quyền sở hữu trước | ✅ **khuyến nghị** |

Thêm hai dữ kiện củng cố: **không có đường xoá tài khoản lúc chạy** (đo được), nên `RESTRICT`
không cản trở thao tác nào đang có. Và `user_account.id` là khoá chính tự tăng **không bao giờ bị
sửa**, nên `ON UPDATE` chọn `RESTRICT` cho rõ ràng thay vì `CASCADE` không bao giờ kích hoạt.

### 13.2 DB giữ được gì, KHÔNG giữ được gì

| Bất biến | DB giữ? | Bằng cách nào |
|---|---|---|
| Chủ sở hữu phải là tài khoản có thật | ✅ | khoá ngoại |
| Vai trò riêng phải có chủ | ✅ | CHECK cùng dòng |
| Vai trò chung không được có chủ | ✅ | CHECK cùng dòng |
| Không xoá được tài khoản còn sở hữu vai trò | ✅ | `ON DELETE RESTRICT` |
| Đổi email không mất quyền sở hữu | ✅ | sở hữu bám `id`, không bám email |
| **Vai trò riêng của A không bị gán cho B** | ❌ **KHÔNG** | xem §14 |

---

## 14. BẤT BIẾN GÁN — VÌ SAO KHOÁ NGOẠI KHÔNG ĐỦ

### 14.1 Lý do kỹ thuật chính xác

Bất biến cần giữ nối **hai bảng**: `user_role_mapping` (ai được gán) và `dm_vai_tro` (ai sở hữu).

- **CHECK không làm được:** MariaDB chỉ cho CHECK tham chiếu **cột cùng dòng cùng bảng**.
- **Khoá ngoại thường không làm được:** khoá ngoại bảo đảm *giá trị tồn tại ở bảng cha*, chứ không
  bảo đảm *hai giá trị bằng nhau giữa hai bảng*.
- **Thêm nữa, `user_role_mapping` không có cột `user_id`** — nó khoá theo email. Nên ngay cả khoá
  ngoại ghép cũng không có gì để so.

### 14.2 Đã cân nhắc và LOẠI: khoá ngoại ghép

Có thể thêm `user_id` + một cột soi gương `rang_buoc_chu_so_huu` vào bảng gán, thêm chỉ mục UNIQUE
`(ma_vai_tro, chu_so_huu_user_id)` trên `dm_vai_tro`, rồi khoá ngoại ghép. Loại vì:

1. Kéo theo **đổi khoá bảng gán quyền** — thuộc `DEBT-128`, không thuộc `D3`.
2. **Vẫn còn một lỗ**: gán một vai trò riêng mà để cột soi gương `NULL` (giả vờ là vai trò chung)
   thì khoá ngoại ghép **không kích hoạt**, vì trong InnoDB khoá ngoại có thành phần NULL thì
   không được kiểm. Muốn biết vai trò đó *có phải riêng không* phải đọc **bảng cha** — điều mà
   không ràng buộc cùng dòng nào làm được.

⇒ Phức tạp hơn nhiều **mà vẫn không đóng kín**.

### 14.3 Đã cân nhắc và LOẠI: trigger

Trigger `BEFORE INSERT/UPDATE` trên bảng gán **có thể** đọc `dm_vai_tro` và đóng kín bất biến.
Loại vì: **CSDL hiện có 0 trigger** (đo được) ⇒ đưa vào là thêm một mặt phẳng thi hành mới, vô hình
với mã ứng dụng và với người bảo trì; và mọi bộ kiểm hiện tại đều đo ở tầng ứng dụng.

### 14.4 KHUYẾN NGHỊ: **một chốt duy nhất ở TẦNG LƯU TRỮ**

Đặt bất biến trong `src/lib/security-store.ts`, **không** ở tầng hành động — vì §7.2 đã chứng minh
đang tồn tại đường ghi thứ hai (`m1/nhan-su`) với chốt khác.

```
Hàm dùng chung, ví dụ  chanNeuGanSaiChuSoHuu(tai_khoan_dich_id, ma_vai_tro)
  · gọi TRONG cùng giao dịch, TRƯỚC mọi lệnh ghi
  · đọc dm_vai_tro dưới khoá dòng
  · vai trò riêng mà tài khoản đích ≠ chủ sở hữu  ⇒  NÉM LỖI, huỷ cả giao dịch
  · dùng mã lỗi có cấu trúc, giao diện dịch sang tiếng Việt (mẫu `loiChoNguoiDung`)
```

Ràng buộc kèm theo, phải nghiệm thu được:
1. **Mọi** hàm ghi `user_role_mapping` gọi nó — hôm nay là `assignRoleToUser`; cổng kiểm phải
   **đếm số đường ghi** và báo đỏ nếu xuất hiện đường mới không gọi (giống cách
   `test:m3-dinh-danh` khoá bảy hàm ghi dòng con của M3).
2. Kịch bản nạp/di trú (**T3**) cũng phải đi qua hàm này, hoặc có cổng riêng chứng minh không tạo
   gán sai chủ.
3. Kiểm ngược bắt buộc: gỡ chốt ⇒ ca "gán vai trò riêng của A cho B" phải **ĐỎ**.

---

## 15. QUẢN TRỊ · PHỤC HỒI · QUẢN TRỊ CUỐI

### 15.1 `D3` không mở thêm đường làm mất quản trị

| Đường | Trước `D3` | Sau `D3` |
|---|---|---|
| Gỡ vai trò quản trị | có chốt | **không đổi** |
| Khoá tài khoản | có chốt | **không đổi** |
| Vô hiệu hoá tài khoản | không có đường lúc chạy | **không tạo mới** |
| Xoá tài khoản | không có đường lúc chạy | `RESTRICT` làm **chặt hơn** |
| Xoá / sửa vai trò | không có đường lúc chạy | **không tạo mới** |
| Đổi cờ riêng/chung | — | **cấm** đổi trên vai trò mang `la_admin = 1` |
| Gán hàng loạt / mẫu nhanh | đang tạm ngưng | phải qua cùng chốt khi mở lại |

### 15.2 Ba điều bản triển khai phải làm thêm

1. **Vai trò quản trị không được biến thành vai trò riêng.** Bất biến bổ sung:
   `la_admin = 1` ⇒ `la_vai_tro_rieng` phải `= 0`. Đây là ràng buộc **cùng dòng** ⇒ đưa được vào
   CHECK. Không có nó, một vai trò quản trị bị biến thành riêng rồi chủ sở hữu bị khoá sẽ làm
   số quản trị hợp lệ tụt mà không đường nào hiện tại phát hiện.

2. **Vá lỗ "quản trị không mật khẩu"** nêu ở §7.3: phép đếm nên thêm
   `AND ua.password_hash IS NOT NULL AND ua.password_hash <> ''`.
   Phơi nhiễm hiện tại **bằng 0**, nhưng `createInactiveUserAccount` tạo tài khoản không mật khẩu
   nên tình huống có thể phát sinh. **Đây là sửa nhỏ, độc lập với `D3`** — nêu ở đây để không mất,
   không gộp vào phạm vi `D3`.

3. **Phân biệt rõ ba loại** trong mọi báo cáo và mọi phép đếm: quản trị thường · `DEV` phá kính ·
   quản trị khôi phục được (active + không khoá + **có mật khẩu**). Không coi một tài khoản
   không đăng nhập được là đường phục hồi hợp lệ.

---

## 16. VÔ HIỆU HOÁ · XOÁ · MỒ CÔI

| Tình huống | Hành vi HIỆN TẠI (đo được) | Chính sách ĐỀ XUẤT |
|---|---|---|
| Chủ sở hữu bị **khoá** | phiên bị chặn ở tầng phiên; chốt quản trị cuối áp dụng | **giữ nguyên**; vai trò riêng vẫn còn nhưng không cấp quyền vì chủ không đăng nhập được |
| Chủ sở hữu bị **vô hiệu hoá** | **không có đường lúc chạy**; `resolveCurrentSession` thu hồi phiên khi `trang_thai` không phải active | quyền **mất hiệu lực ngay**; vai trò riêng **giữ lại để tra cứu**, không cấp quyền |
| Chủ sở hữu **được kích hoạt lại** | `activateUserAccount` đòi **phải có mật khẩu** trước | cùng `id` ⇒ quyền riêng **trở lại**; đây là hệ quả tự nhiên của việc bám `id` |
| Chủ sở hữu bị **xoá cứng** | **không có đường lúc chạy** | `ON DELETE RESTRICT` chặn; muốn xoá phải xử lý quyền sở hữu trước — **không tự chuyển cho ai** |
| Vai trò riêng còn **dòng quyền** | 4 bảng quyền CASCADE theo vai trò | không đổi |
| Vai trò riêng **mồ côi** | *không thể xảy ra* với `RESTRICT` | có báo cáo đếm định kỳ để phát hiện sớm |
| Email cũ **bị dùng lại** | không kế thừa vai trò (§8.3) | **không đổi** — nhưng lịch sử/quy kết vẫn lệch: thuộc `DEBT-128` |

> ⚠️ **Không bịa nhu cầu.** Chỉ thị Owner nói rõ: *"Nếu code hiện tại không hỗ trợ hard delete thì
> không được bịa thêm use case bắt buộc"*. Áp dụng đúng như vậy cho **cả** xoá cứng **và** vô hiệu
> hoá — hôm nay **cả hai đều không có đường lúc chạy**. Bảng trên ghi chính sách để bản triển khai
> có cái mà tuân, **không** hàm ý phải xây hai chức năng đó.

---

## 17. KẾ HOẠCH DI TRÚ VÀ NẠP BÙ

### 17.1 Phân loại vai trò hiện có

| Nhóm | Số lượng | Cách phân loại |
|---|---|---|
| Vai trò hệ thống / dùng chung | **6** | `nguoi_tao = 'system'` |
| Vai trò dùng chung do người tạo | **3** | `TP_KINH_DOANH` · `TP_SAN_XUAT` · `TP_THIET_KE` — vai trò **phòng ban**, 0 lượt gán |
| Vai trò riêng xác định được chủ | **0** | khái niệm chưa tồn tại |
| Vai trò nhập nhằng | **0** | — |
| Vai trò mồ côi | **0** | — |
| Vai trò trùng / hỏng | **0** | 0 tên trùng, 0 dòng quyền rỗng |

⇒ **Toàn bộ 9 vai trò được phân loại DỨT KHOÁT là DÙNG CHUNG.**

### 17.2 Nạp bù: **KHÔNG CÓ**

Đây là điểm mạnh nhất của gói này. Hai cột mới có mặc định `0` và `NULL`, đúng bằng giá trị mà cả
9 dòng hiện có phải mang. Nghĩa là:

- **không** phải suy chủ sở hữu từ email ⇒ **không** có rủi ro suy sai;
- **không** có dòng nhập nhằng ⇒ **không** cần quy tắc *fail-closed* cho suy luận;
- **không** cần bảng ánh xạ tạm;
- lệnh `ALTER` **chính là** bước nạp bù.

*(Đây là khác biệt then chốt so với `DEBT-128`, nơi nạp bù đụng 108 cột.)*

### 17.3 Trình tự triển khai

```
1. sao lưu CSDL vận hành + chụp vân tay SHA256
2. dựng bản sao từ chính sao lưu đó (MariaDB 10.11)
3. chạy ALTER trên BẢN SAO
4. đối chiếu: 9/9 vai trò có la_vai_tro_rieng=0, chu_so_huu_user_id NULL
5. chạy trọn bộ kiểm quyền trên bản sao
6. diễn tập HOÀN TÁC trên bản sao, rồi tiến lại
7. đối chiếu mốc nền: 101 bảng · 9 tài khoản · 9 vai trò · 11 lượt gán · 3 quản trị
8. chỉ khi 1–7 đạt mới chạy trên máy vận hành, theo đúng chuỗi DEBT-129:
   dựng → tiền kiểm → nạp dữ liệu → cổng chặn → kích hoạt → kiểm khói
```

### 17.4 Truy vấn kiểm chứng (chạy sau khi ALTER, trên bản sao)

```sql
-- 1) Không dòng nào vi phạm bất biến cờ ↔ chủ sở hữu
SELECT COUNT(*) AS phai_bang_0 FROM dm_vai_tro
 WHERE (la_vai_tro_rieng = 1 AND chu_so_huu_user_id IS NULL)
    OR (la_vai_tro_rieng = 0 AND chu_so_huu_user_id IS NOT NULL);

-- 2) Không có chủ sở hữu trỏ vào tài khoản không tồn tại
SELECT COUNT(*) AS phai_bang_0 FROM dm_vai_tro v
 WHERE v.chu_so_huu_user_id IS NOT NULL
   AND NOT EXISTS (SELECT 1 FROM user_account u WHERE u.id = v.chu_so_huu_user_id);

-- 3) Không có vai trò riêng nào bị gán cho người khác chủ
SELECT COUNT(*) AS phai_bang_0
  FROM user_role_mapping m
  JOIN dm_vai_tro v ON v.ma_vai_tro = m.ma_vai_tro
  JOIN user_account u ON u.email = m.user_email
 WHERE v.la_vai_tro_rieng = 1 AND u.id <> v.chu_so_huu_user_id;

-- 4) Không vai trò quản trị nào bị biến thành vai trò riêng
SELECT COUNT(*) AS phai_bang_0 FROM dm_vai_tro
 WHERE la_admin = 1 AND la_vai_tro_rieng = 1;

-- 5) Mốc nền không đổi
SELECT (SELECT COUNT(*) FROM dm_vai_tro)        AS vai_tro,       -- kỳ vọng 9
       (SELECT COUNT(*) FROM user_account)      AS tai_khoan,     -- kỳ vọng 9
       (SELECT COUNT(*) FROM user_role_mapping) AS luot_gan;      -- kỳ vọng 11
```

---

## 18. KẾ HOẠCH DIỄN TẬP

| Bước | Nội dung | Tiêu chí đạt |
|---|---|---|
| R1 | Phục hồi từ sao lưu **vận hành thật**, đối chiếu vân tay SHA256 | vân tay khớp từng ký tự |
| R2 | Đối chiếu mốc nền trước khi ALTER | 101 · 9 · 9 · 11 · 3 |
| R3 | Chạy ALTER | không lỗi; thời gian khoá **không đáng kể** (9 dòng) |
| R4 | Chạy 5 truy vấn §17.4 | 4 truy vấn đầu **= 0**; truy vấn 5 **không đổi** |
| R5 | Chạy trọn bộ kiểm quyền hiện có | không cổng nào **đỏ thêm** so với trước ALTER |
| R6 | Kiểm ngược ràng buộc: cố ghi `la_vai_tro_rieng=1, chu_so_huu_user_id=NULL` | CSDL **TỪ CHỐI** (CHECK) |
| R7 | Kiểm ngược khoá ngoại: cố ghi chủ sở hữu = `id` không tồn tại | CSDL **TỪ CHỐI** (FK) |
| R8 | Kiểm ngược `RESTRICT`: cố xoá tài khoản đang sở hữu vai trò riêng | CSDL **TỪ CHỐI** |
| R9 | Diễn tập hoàn tác rồi tiến lại | mốc nền về đúng, rồi lên lại đúng |
| R10 | Dọn bản sao | **0 dấu vết kiểm thử** |

> R6–R8 quan trọng ngang phần chạy xuôi: chúng chứng minh ràng buộc **thực sự thi hành**,
> không phải chỉ khai báo cho đẹp.

---

## 19. KẾ HOẠCH HOÀN TÁC

```sql
-- HOÀN TÁC — ĐỀ XUẤT, CHƯA CHẠY
-- ⚠️ TRƯỚC KHI CHẠY: xuất bản kê quyền sở hữu, nếu không sẽ MẤT ánh xạ.
--    SELECT ma_vai_tro, la_vai_tro_rieng, chu_so_huu_user_id
--      FROM dm_vai_tro WHERE la_vai_tro_rieng = 1;
--    → lưu ra tệp bản kê kèm mốc thời gian, ĐỂ NGOÀI CSDL.

ALTER TABLE `dm_vai_tro` DROP INDEX  `idx_vai_tro_chu_so_huu`;
ALTER TABLE `dm_vai_tro` DROP CONSTRAINT `chk_vai_tro_rieng_co_chu`;
ALTER TABLE `dm_vai_tro` DROP FOREIGN KEY `fk_vai_tro_chu_so_huu`;
ALTER TABLE `dm_vai_tro` DROP COLUMN `chu_so_huu_user_id`;
ALTER TABLE `dm_vai_tro` DROP COLUMN `la_vai_tro_rieng`;
```

| Khi nào lùi được an toàn | Khi nào **PHẢI VÁ TIẾN** thay vì lùi |
|---|---|
| Ngay sau ALTER, **chưa** vai trò riêng nào được tạo (mất mát = 0) | Đã có vai trò riêng đang dùng thật ⇒ lùi là **xoá quyền sở hữu**, không phục hồi được từ lược đồ |
| Bản sao diễn tập | Bản kê sở hữu chưa xuất |
| Lỗi phát hiện trong cùng cửa sổ phát hành, chưa ai tạo vai trò riêng | Chỉ hỏng phần giao diện — vá tiến rẻ hơn nhiều |

**Ba lớp hoàn tác:** mã (lùi bản dựng trước) · lược đồ (khối trên) · dữ liệu (**không cần**, vì
nạp bù không ghi gì ngoài giá trị mặc định).

---

## 20. MA TRẬN NGHIỆM THU VÀ KIỂM NGƯỢC

Ký hiệu: **KQ** = kết quả mong đợi · **Δ CSDL** = thay đổi dữ liệu mong đợi.

### 20.1 Định danh

| # | Dữ liệu mẫu | Thao tác | KQ | Δ CSDL | Nhật ký |
|---|---|---|---|---|---|
| 1 | A có vai trò riêng | đổi email của A | quyền riêng **vẫn thuộc A** | 0 | có |
| 2 | A bị xoá, tài khoản mới dùng email cũ | đọc quyền | **không kế thừa** | 0 | — |
| 3 | email khác hoa/thường/khoảng trắng | phân giải chủ sở hữu | không ảnh hưởng (bám `id`) | 0 | — |
| 4 | phiên đăng nhập hợp lệ | phân giải người dùng | theo **`user_id`** ổn định | 0 | — |

### 20.2 Gán

| # | Thao tác | KQ | Δ CSDL |
|---|---|---|---|
| 5 | gán vai trò riêng của A **cho A**, người thực hiện là quản trị | **thành công** | 1 dòng gán |
| 6 | gán vai trò riêng của A **cho B** | **TỪ CHỐI phía máy chủ** | **0** |
| 7 | như (6) nhưng gọi thẳng hành động máy chủ, bỏ qua giao diện | **TỪ CHỐI** | **0** |
| 8 | như (6) nhưng qua đường hàng loạt / mẫu nhanh | **TỪ CHỐI** | **0** |
| 9 | gán vai trò **dùng chung** hợp lệ | thành công | 1 dòng |
| 10 | sao chép mẫu cho B | tạo vai trò riêng **MỚI thuộc B** | 1 vai trò + n dòng quyền |
| 10b | người thực hiện là quản trị, mục tiêu là B | chủ sở hữu = **B**, không phải quản trị | — |

### 20.3 Hợp quyền

| # | Thao tác | KQ |
|---|---|---|
| 11 | tài khoản có vai trò chung + vai trò riêng | **hợp các quyền được cấp** |
| 12 | quyền menu/hành động/trường/chuyển trạng thái | vẫn **AND** giữa các lớp |
| 13 | `USER` chưa cấp vai trò nghiệp vụ | **không** menu, **không** hành động |
| 14 | vai trò riêng cấp `can_view` cột giá vốn cho người không được phép | **vẫn bị che** — vai trò riêng không vượt lớp trường |

### 20.4 Vòng đời

| # | Thao tác | KQ |
|---|---|---|
| 15 | khoá chủ sở hữu | quyền **mất hiệu lực** |
| 16 | vô hiệu hoá chủ sở hữu | quyền **mất hiệu lực**, vai trò còn để tra cứu |
| 17 | kích hoạt lại cùng `id` | quyền riêng **trở lại** |
| 18 | tài khoản mới cùng email | **không** kế thừa |
| 19 | sau mọi thao tác | **0 vai trò mồ côi** |
| 20 | xoá cứng chủ sở hữu | **bị chặn** (`RESTRICT`) |

### 20.5 Quản trị và phục hồi

| # | Thao tác | KQ |
|---|---|---|
| 21 | gỡ/khoá/vô hiệu hoá quản trị **cuối** | **bị chặn** |
| 22 | thu hồi quản trị cuối qua đường hàng loạt/mẫu | **bị chặn** |
| 23 | hai giao dịch cùng lúc, mỗi bên gỡ một quản trị (còn 2) | **không thể** về 0 |
| 24 | `DEV` phá kính | **không** biến thành cửa thường |
| 24b | vai trò `la_admin=1` bị đặt `la_vai_tro_rieng=1` | **bị chặn** (§15.2) |
| 24c | quản trị `active`, không khoá, **không mật khẩu** | **không** được tính là khôi phục được (§15.2) |

### 20.6 Giao dịch và tranh chấp

| # | Thao tác | KQ |
|---|---|---|
| 25 | hai quản trị cùng sửa một mục tiêu | tuần tự hoá, không lẫn lộn |
| 26 | hai quản trị cùng gán một vai trò | đúng một dòng, không nhân bản |
| 27 | một bên vô hiệu hoá trong khi bên kia gán | **không** có trạng thái lai |
| 28 | hỏng giữa chừng | **hoàn tác toàn bộ** |
| 29 | gọi lại (retry) | **không** sinh dòng gán trùng |

### 20.7 Di trú

| # | Thao tác | KQ |
|---|---|---|
| 30 | chạy khô | **0** thay đổi |
| 31 | ánh xạ email nhập nhằng | *không áp dụng* — nạp bù **bằng 0** (§17.2). Vẫn phải có khẳng định chứng minh điều đó |
| 32 | báo cáo trùng/mồ côi | **0 / 0** |
| 33 | chạy nạp bù hai lần | **bất biến** (`ALTER` có `IF NOT EXISTS` hoặc kiểm trước) |
| 34 | diễn tập hoàn tác | bản kê sở hữu **được giữ** |
| 35 | dọn bản sao | **0 dấu vết** |

### 20.8 Kiểm ngược bắt buộc (thiếu = cổng vô giá trị)

| Gỡ gì | Ca phải **ĐỎ** |
|---|---|
| chốt bất biến sở hữu ở tầng lưu trữ | 6 · 7 · 8 |
| ràng buộc CHECK | R6 |
| khoá ngoại chủ sở hữu | R7 · 20.4 #20 |
| chốt quản trị cuối | 21 · 22 |
| giao dịch/khoá dòng | 23 · 27 · 28 |

---

## 21. BẢNG BẰNG CHỨNG (AN TOÀN CÔNG KHAI)

| # | Khẳng định | Bằng chứng | Nhãn |
|---|---|---|---|
| E1 | `user_account.id` = `int(11)` có dấu, PK, tự tăng | `information_schema.COLUMNS` (vận hành) | DB_PROVEN |
| E2 | `la_vai_tro_rieng` / `chu_so_huu_user_id` **chưa tồn tại** | quét toàn CSDL 5 mẫu tên ⇒ 0 kết quả | DB_PROVEN |
| E3 | `user_role_mapping` khoá theo `(user_email, ma_vai_tro)` | `information_schema` | DB_PROVEN |
| E4 | `fk_urm_user` mang `ON UPDATE CASCADE` ⇒ đổi email **không** mất quyền | `REFERENTIAL_CONSTRAINTS` + thực nghiệm 27/08 | DB_PROVEN |
| E5 | Phiên đã mang `userId` ổn định | `lookupSessionRowByTokenHash` nối `ua.id = us.user_id` | CODE_PROVEN |
| E6 | Tầng quyền **không** dùng `userId` | `.userId` = 0 lần trong mã quyền `src/lib` | CODE_PROVEN |
| E7 | 14 hàm ghi, **0** dùng giao dịch | kiểm kê tự động toàn `src/` | CODE_PROVEN |
| E8 | Chốt quyền ở **tầng hành động**, không ở tầng lưu trữ | `requireAdminContext` 12 lần / 10 hàm hành động | CODE_PROVEN |
| E9 | Tồn tại đường ghi thứ hai với chốt khác | `m1/nhan-su/actions.ts` gọi thẳng store | CODE_PROVEN |
| E10 | **Không** có đường xoá/vô hiệu hoá tài khoản lúc chạy | 0 kết quả cho `DELETE FROM user_account` và `trang_thai='inactive'` trong `src/` | CODE_PROVEN |
| E11 | **Không** có đường xoá/sửa `dm_vai_tro` lúc chạy | 0 kết quả | CODE_PROVEN |
| E12 | Chốt quản trị cuối **không** kiểm mật khẩu | đọc thân hàm | CODE_PROVEN |
| E13 | Phơi nhiễm của E12 hiện **bằng 0** | 0 tài khoản active không mật khẩu | DB_PROVEN |
| E14 | 31 ràng buộc CHECK đang hiệu lực | `information_schema.CHECK_CONSTRAINTS` | DB_PROVEN |
| E15 | **0 trigger** trong CSDL | `information_schema.TRIGGERS` | DB_PROVEN |
| E16 | 9 vai trò, 0 vai trò riêng, 4 chưa ai được gán | truy vấn đếm | DB_PROVEN |
| E17 | 9 tài khoản, 3 quản trị khôi phục được | truy vấn đếm | DB_PROVEN |
| E18 | 113 cột chữ mang email/người-thao-tác, chỉ **5** có khoá ngoại | truy vấn `information_schema` | DB_PROVEN |
| E19 | Lược đồ định danh **giống hệt** hai môi trường | 28 cột · 2 khoá ngoại · 0 lệch | DB_PROVEN |
| E20 | Mã vai trò dài nhất 13/20 ký tự | truy vấn đếm | DB_PROVEN |
| E21 | Gói `D3` cũ đề xuất `chu_so_huu_email` | đọc nguyên văn tệp | REPORT_PROVEN |
| E22 | Mẫu nhanh đang đóng an toàn | `thanApMauQuyen_TAM_NGUNG` | CODE_PROVEN |
| E23 | Toàn bộ DDL trong tài liệu này **chưa chạy** | không lệnh nào được thực thi | PROPOSED_NOT_EXECUTED |

**Không có trong tài liệu này:** email thật · tên người · mật khẩu · cookie · token · chuỗi kết nối ·
chi tiết SSH · dữ liệu khách hàng · dữ liệu cá nhân.

---

## 22. ĐIỀU CHƯA BIẾT

| # | Chưa biết | Vì sao | Cách biết |
|---|---|---|---|
| U1 | Owner có muốn vai trò riêng hiện trên màn phân quyền như một mục **tách riêng** không | thuộc trải nghiệm, không suy ra từ mã | wireframe ở bản triển khai |
| U2 | Có cần giới hạn **số vai trò riêng** mỗi tài khoản không | chưa có chính sách | mặc định: không giới hạn, thêm sau nếu cần |
| U3 | Khi vô hiệu hoá tài khoản, có nên tự **thu hồi** các gán của vai trò riêng không | chưa có đường vô hiệu hoá lúc chạy nên chưa gặp | quyết khi xây chức năng đó |
| U4 | Hành vi khi **gộp** hai tài khoản | chưa có chức năng gộp | ngoài phạm vi |
| U5 | Bản triển khai có nên vá luôn lỗ "quản trị không mật khẩu" (§15.2) không | là sửa nhỏ độc lập | khuyến nghị tách riêng, không gộp vào `D3` |

**Không mục nào ở trên chặn quyết định của Owner.**

---

## 23. OWNER CẦN QUYẾT — **MỘT CÂU DUY NHẤT**

> **Duyệt `GO_D3_STABLE_ACCOUNT_OWNED_ROLE` không?**
>
> Tức: cho phép thêm **hai cột** `la_vai_tro_rieng` + `chu_so_huu_user_id` vào `dm_vai_tro`,
> kèm **khoá ngoại `ON DELETE RESTRICT`** tới `user_account(id)`, **ràng buộc CHECK** giữ nhất quán
> cờ ↔ chủ sở hữu, và **một chốt bất biến ở tầng lưu trữ** bảo đảm vai trò riêng của A không bị gán
> cho B — theo đúng kế hoạch di trú, diễn tập, hoàn tác và ma trận nghiệm thu ở tài liệu này.

**Nếu duyệt:** bước kế tiếp ở §24.
**Nếu chưa duyệt:** không có gì bị chặn. Quản trị vẫn tạo vai trò mới và gán cho một người được như
hôm nay; hệ thống chỉ **chưa biết** đó là vai trò riêng nên chưa cảnh báo phạm vi ảnh hưởng và chưa
phát hiện được vai trò mồ côi. `D3` giữ trạng thái **HOÃN CÓ CHỦ ĐÍCH**.

Owner **không cần** chọn lại: mô hình dùng chung/riêng · hợp các quyền · `USER` chờ cấp phát ·
quản trị cuối · bám định danh ổn định. Năm điều đó đã khoá và gói này tuân theo.

---

## 24. GÓI VIỆC KẾ TIẾP NẾU ĐƯỢC DUYỆT

**`WP-ERP-M0-D3-IMPLEMENT-STABLE-ACCOUNT-OWNED-ROLE`** — một gói **có biên**, gồm đúng:

1. Bản di trú thêm 2 cột + khoá ngoại + CHECK + chỉ mục, kèm tệp hoàn tác.
2. Chốt bất biến sở hữu **ở tầng lưu trữ** + giao dịch + khoá dòng cho các đường ghi vai trò.
3. Sinh mã vai trò riêng theo `id`; sao chép mẫu tạo vai trò riêng mới thuộc **tài khoản đích**.
4. Màn phân quyền phân biệt mẫu chung với vai trò riêng; cảnh báo phạm vi ảnh hưởng khi sửa vai trò chung.
5. Bộ kiểm theo §20 **kèm kiểm ngược §20.8**.
6. Sao lưu · phục hồi lên bản sao · diễn tập §18 · hoàn tác hai chiều.
7. Triển khai đúng bản dựng đã kiểm; kiểm khói vận hành; đối chiếu ba nơi.
8. Cập nhật `DEBT-128` với **phạm vi còn lại** (§9); **không** đóng nó.

**Ngoài phạm vi gói đó:** đổi khoá `user_role_mapping` sang `user_id` · 108 cột quy kết ·
`DEBT-133` · `DEBT-134` · `DEBT-135` · phần ngoài M3 của `DEBT-131`.

---

## 25. BÀN GIAO CHO NOTION

**Điều đã chứng minh**
- Định danh tài khoản ổn định **đã tồn tại** (`user_account.id`, `int(11)`) và **đã** được phiên
  đăng nhập dùng; tầng phân quyền thì chưa.
- Vai trò riêng **chưa tồn tại** trong lược đồ — mọi tên cột nhắc tới là **đề xuất**.
- Đổi email **không** làm mất quyền; dùng lại email **không** kế thừa quyền; rủi ro thật nằm ở
  **quy kết lịch sử** (108 cột không khoá ngoại).
- Hôm nay có **3 quản trị khôi phục được**; **0/14** hàm ghi dùng giao dịch.

**Khuyến nghị:** `GO_D3_STABLE_ACCOUNT_OWNED_ROLE`.

**Chưa giải quyết:** năm mục ở §22 — không mục nào chặn.

**Owner cần quyết:** đúng một câu ở §23.

**Trạng thái nợ:** `D3` = **HOÃN CÓ CHỦ ĐÍCH**, chờ Owner duyệt · `DEBT-128` = **MỞ**, phạm vi còn
lại theo §9 · `DEBT-131` `DEBT-133` `DEBT-134` `DEBT-135` = **không đổi**.

**Mục tài liệu cần cập nhật trên Notion:** trang mô hình phân quyền (thêm khái niệm vai trò riêng,
ghi rõ **ĐỀ XUẤT**) · trang lược đồ CSDL (**chưa** đổi — DDL chưa chạy) · trang sổ quyết định
(thêm mục chờ duyệt).

> ⚠️ **KHÔNG** nâng DDL đề xuất thành lược đồ hiện hành. Cho tới khi Owner duyệt và bản triển khai
> chạy xong, lược đồ thật của `dm_vai_tro` vẫn là **8 cột** như §5.2.

---

## 26. KHỐI BÁO CÁO KẾT THÚC

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Rà chỉ đọc lược đồ định danh/phân quyền trên MÁY VẬN HÀNH: 3 bảng lõi,
     4 bảng quyền, 13 khoá ngoại, 31 ràng buộc CHECK, 0 trigger
   - Đối chiếu lược đồ hai môi trường: 28 cột · 2 khoá ngoại · 0 lệch
   - Rà hình dạng dữ liệu: 9 tài khoản · 9 vai trò · 11 lượt gán · 3 quản trị
     khôi phục được · 0 mồ côi · 0 email trùng sau chuẩn hoá
   - Kiểm kê 14 hàm ghi trong 2 tệp mã sản phẩm, kèm chốt đi kèm từng hàm
   - Truy chuỗi phiên: định danh ổn định ĐÃ có và ĐÃ chảy qua phiên đăng nhập,
     nhưng tầng phân quyền vứt bỏ nó
   - Kết luận quan hệ với DEBT-128: D3 đóng phần sở hữu vai trò, KHÔNG đóng
     ba phần còn lại
   - Viết gói quyết định 26 mục, thay gói cũ (gói cũ dùng chu_so_huu_email —
     trái quyết định Owner đã khoá)

2. PHẠM VI
   ĐỤNG    : docs/reports/ (1 tệp mới) · sổ Owner · sổ nợ
   KHÔNG ĐỤNG: mã ứng dụng · mã kiểm thử · lược đồ CSDL · dữ liệu · máy vận hành

3. BẰNG CHỨNG
   23 khẳng định có nhãn ở §21 (DB_PROVEN · CODE_PROVEN · REPORT_PROVEN ·
   PROPOSED_NOT_EXECUTED). Không dùng LIVE_VERIFIED/DEPLOYED/CLOSED cho D3.

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #189

5. PUSH BÁO CÁO CÔNG KHAI
   <!-- MUC5 -->

6. CÒN SÓT / CHƯA LÀM
   - D3 chưa triển khai — đúng phạm vi gói này
   - Lỗ "quản trị không mật khẩu" (§15.2) chưa vá — phơi nhiễm hiện tại bằng 0
   - Chỉ mục idx_email trùng lặp với UNIQUE(email) — ghi nhận, chưa xử
   - DEBT-128 phần còn lại chưa viết lại phạm vi (làm ở gói triển khai)

7. ĐANG CHỜ OWNER
   ĐÚNG MỘT câu hỏi ở §23: duyệt GO_D3_STABLE_ACCOUNT_OWNED_ROLE không?
   Chưa duyệt thì KHÔNG chặn việc gì.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Nếu Owner duyệt: tạo gói WP-ERP-M0-D3-IMPLEMENT-STABLE-ACCOUNT-OWNED-ROLE
   theo đúng 8 mục ở §24.

9. CHƯA XÁC MINH ĐƯỢC
   - Hành vi thật của ràng buộc CHECK/FK đề xuất — chưa chạy ở đâu
   - Chi phí khoá bảng khi ALTER dưới tải thật (9 dòng nên dự kiến không đáng kể)
   - Năm mục ở §22

10. TRẠNG THÁI CHUNG
   [x] PASS — gói quyết định đủ bằng chứng để Owner quyết trong một lượt

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén: CLAUDE.md · docs/OWNER-REQUEST-LEDGER.md (#184–#188) ·
   .governance/registry/tech-debt.md (DEBT-126/127/128/131/133/134/135) ·
   DECISION-PACK-M0-D3-VAI-TRO-RIENG-20260827.md (nguyên văn) ·
   M3-SLICE2-COHERENT-RELEASE-20260828.md · M0-HYGIENE-TO-M3-20260827.md ·
   src/lib/security-store.ts · security-session-store.ts ·
   src/app/m0/security/actions.ts · src/app/m1/nhan-su/actions.ts ·
   lược đồ 7 bảng đọc TRỰC TIẾP TỪ CSDL cả hai môi trường.
   Gói này KHÔNG đụng lớp trình bày nên không cần đọc lại docs/UI-STANDARD.md.
═══════════════════════════════════════════
```
