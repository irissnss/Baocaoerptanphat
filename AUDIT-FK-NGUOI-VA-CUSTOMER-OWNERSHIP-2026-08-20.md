> ## ⚠️ BẢN RÚT GỌN CÔNG KHAI
>
> Đây là **bản công khai đã lược** của báo cáo *Audit FK cột người & quyền sở hữu khách hàng — đợt 1*.
> Bản đầy đủ nằm ở kho mã riêng tư.
>
> **Đã lược (Owner duyệt 21/08/2026, theo `GOV-PUBLIC-SAFE-001` §J1):**
>
> | Mã | Nội dung bị lược | Vì sao |
> |---|---|---|
> | `⟨SEC-01⟩` | Tên cụ thể của các route API ghi chưa có kiểm quyền | Hệ thống **đang chạy và chưa vá** — công bố tên route = đưa danh sách điểm vào cho người ngoài |
> | `⟨SEC-02⟩` | Đường dẫn màn quản trị chưa có cổng quyền | như trên |
> | `:▮` | **Số dòng** trong mọi tham chiếu mã nguồn | Giữ tên file để tra được, bỏ toạ độ chính xác |
>
> **KHÔNG lược:** kết luận · số đo · bảng diff schema · danh sách blocker · các mục cần Owner quyết · phần "không xác minh được".

---

# AUDIT FK `user_account` & CUSTOMER OWNERSHIP — 20/08/2026

> **LOẠI:** `EVIDENCE` + `STATE` — báo cáo audit **READ-ONLY**.
> **KHÔNG PHẢI** luật, không phải quyết định, không phải Plan.
> **KHÔNG ĐỤNG:** code · DDL · DML · migration · push · deploy. Không sửa một dòng nào của `src/`, `migrations/`, `sql/`.
> **MỤC ĐÍCH:** hoàn thiện tài liệu & đồng bộ nhất quán code ↔ tài liệu (cả 2 chiều).

---

## 0. HEADER — NGUỒN BẰNG CHỨNG & MỨC TIN CẬY

| Mục | Nội dung |
|---|---|
| `CURRENT_ACTOR` | Agent IDE (execution lane) |
| `PROJECT_SSOT_AS_OF` | 20/08/2026 |
| Nguồn **live DB** | MariaDB `10.11.10-MariaDB-ubu2204`, container `mariadb-1011`, `127.0.0.1:3308`, schema `tanphat_erp_dev` — **99 bảng** |
| Nguồn **đối chứng production** | schema `tanphat_erp_mirror` trên **cùng** máy chủ (bản sao máy vận hành, giữ sạch — `docs/HUONG-DAN-CHAY-LOCAL.md:104`) — **99 bảng**, **lệch 0** trên toàn bộ 206 khoá cột "người" |
| Nguồn **docs** | 267 file `.md` trong `docs/` (đã loại `_snapshot*` và `Lịch Sử Trò Chuyện`), trích **232 khai báo** cột-người/FK từ các khối `CREATE TABLE` |
| Nguồn **code** | `src/` — grep + đọc trực tiếp, dẫn `file:line` |
| `EVIDENCE_STATE` | `DB_PROVEN` (DDL + information_schema) · `CODE_PROVEN` (file:line) · **`UNVERIFIED` cho máy chủ vận hành thật qua SSH** — xem §7 |
| `LOAD_STATUS` | `PASS` cho phần đọc; **`PROVISIONAL`** cho mọi claim về VPS production (xem §7 mục X-5) |

**Cảnh báo về sức mạnh bằng chứng (`GOV-EVIDENCE-STRENGTH-001`):**
Toàn bộ số liệu DDL dưới đây là `DB_PROVEN` **trên bản local + bản mirror**. Mirror được tài liệu dự án khai là bản sao máy vận hành, nhưng **phiên này KHÔNG mở kết nối tới máy chủ vận hành thật** để xác nhận lại. Vì vậy claim "production cũng như vậy" chỉ ở mức **suy ra từ mirror**, không phải `RUNTIME_PROVEN` trên production.

---

## 1. TRẢ LỜI TRỰC TIẾP 7 NHIỆM VỤ

| # | Câu hỏi | Trả lời (live truth) | Lớp bằng chứng |
|---|---|---|---|
| 1 | `dm_khach_hang.sale_phu_trach` đang trỏ bảng nào? | **KHÔNG TRỎ BẢNG NÀO Ở MỨC DDL.** `int(11) DEFAULT NULL`, **không có bất kỳ ràng buộc FOREIGN KEY nào** trên toàn bảng `dm_khach_hang` ngoài 2 UNIQUE KEY. Không `ON DELETE`. Quan hệ tới `hr_employee_nhanvien.id` **chỉ tồn tại trong tầng code**, DB không cưỡng chế. | `DB_PROVEN` §3.1 |
| 2 | Nếu live trỏ `user_account(id)` thì báo diff | **Không trỏ `user_account(id)`, cũng không trỏ gì cả.** Diff thật nằm ở **docs**: `…M1 1 - Khách hàng….md:228` ghi `sale_phu_trach INT COMMENT 'FK to user_account[id]'`. Phác thảo đồng bộ ở §4. | `DB_PROVEN` + `DOC_PROVEN` |
| 3 | Kiểm kê toàn bộ cột "người" | **202 cột** trên **92/99 bảng**. **85 cột kiểu INT**, **114 cột kiểu VARCHAR**, 3 kiểu khác. **Chỉ 8/202 cột có ràng buộc FK thật** — **194 cột KHÔNG có FK nào.** Hai họ định danh (INT id-based và VARCHAR email-based) **cùng tồn tại**, kể cả **trong cùng một bảng**. | `DB_PROVEN` §5 |
| 4 | `lenh_san_xuat` có `nguoi_phu_trach` hay `nguoi_phe_duyet`? | **KHÔNG CÓ CẢ HAI.** Bảng chỉ có `nguoi_tao int(11) NULL` và `nguoi_sua int(11) NULL` (đều không FK). Có `hinh_anh_kiem_duyet longtext` (ảnh JSON, **không phải cột người**). 2 FK duy nhất: `don_hang_item(id)` và `phieu_dieu_in(id)`. | `DB_PROVEN` §3.2 |
| 5 | Xuất kho: `phieu_xuat` hay `phieu_xuat_kho`? | **`phieu_xuat_kho`** (+ `phieu_xuat_kho_item`). **KHÔNG tồn tại** bảng `phieu_xuat`. `nguoi_lap varchar(100) NOT NULL` — **KHÔNG có FK**. Bảng **không có** `nguoi_duyet` → **khớp đúng docs M5.2**. PK là `so_phieu`; bảng **không có FK nào cả**. | `DB_PROVEN` §3.3 |
| 6 | `bao_gia`: `id_nhan_vien_phu_trach` hay `nguoi_lap`? | **`id_nhan_vien_phu_trach int(11) NOT NULL`, KHÔNG FK** — đúng như audit live 02/2026. **KHÔNG tồn tại cột `nguoi_lap`.** `nguoi_tao int(11) NOT NULL` không FK. 2 FK duy nhất: `dm_khach_hang(id)`, `kh_lien_he(id)`. | `DB_PROVEN` §3.4 |
| 7 | Code join/filter theo id hay email? | **Cả hai, tách domain có chủ đích ở M1; chưa nhất quán ở M3/M5.** M1: ownership = `hr_employee_nhanvien.id`, audit = `user_account.id`, **cấm so chéo** (code ghi rõ). M3: `src/types/m3.ts:▮` chú thích `id_nhan_vien_phu_trach // FK → user_account` — **mâu thuẫn M1**, store còn hardcode `?? 1`. M5: `phieu_xuat_kho.nguoi_lap` là **ô nhập tay tự do**, fallback chuỗi `"system"` — không phải định danh của ai. | `CODE_PROVEN` §6 |

---

## 2. PHÁT HIỆN LỚN NHẤT (đọc trước khi vào bảng)

**PH-1 — Câu hỏi "FK trỏ đâu" phần lớn là câu hỏi rỗng ở tầng DB.**
194/202 cột người **không có ràng buộc FK nào**. Với hầu hết các cột, "trỏ `user_account(email)`" hay "trỏ `hr_employee_nhanvien.id`" **không phải sự thật DB** — đó là **quy ước do code giữ**. Mọi tài liệu ghi `FOREIGN KEY … REFERENCES …` cho các cột này đang mô tả **ý định**, không mô tả hiện trạng.

**PH-2 — DB đang chạy song song HAI hệ định danh người.**
85 cột INT (id-based) và 114 cột VARCHAR (email/tên-based). Ranh giới **không theo module** mà theo **thời điểm bảng được tạo**. Có bảng giữ **cả hai** cùng lúc — `chung_tu_ke_toan` có đủ `nguoi_tao int(11)` **và** `nguoi_tao_email varchar(100)`; `cong_no`, `phieu_thu`, `phieu_chi` cũng vậy.

**PH-3 — 8 FK thật đang trỏ `user_account(email)`, không phải `id`.**
`audit_log.nguoi_thuc_hien` · `hr_bang_luong.{nguoi_tinh_luong, nguoi_duyet, nguoi_chi_luong}` · `hr_luong_phu_cap.nguoi_duyet` · `permission_log.user_email` · `user_role_mapping.user_email` → tất cả `REFERENCES user_account(email)`. Chỉ **2 cột** trỏ `user_account(id)`: `hr_employee_nhanvien.user_id` (link authoritative theo PL4 §5.5) và `user_session.user_id`.

**PH-4 — Docs lệch live 135/232 khai báo (58%).**
97 khớp · 48 lệch FK · 47 cột docs khai mà live **không có** · 40 bảng docs khai mà live **không có**. Nguyên nhân cấu trúc: nhiều file Notion V3.43 chứa **hai lớp chồng nhau** — một khối *"DB hiện tại (dump …)"* khớp live, và một khối *"Schema:"* mô tả thiết kế cũ hoàn toàn khác (VD `bao_gia` intent dùng PK `so_bao_gia VARCHAR(50)` + `nguoi_lap` FK email, còn live dùng PK `id INT` + `id_nhan_vien_phu_trach`). Người đọc lấy nhầm khối là ra kết luận sai.

**PH-5 — Quyết định Owner 20/08 (Phương án B) ĐÃ được code thi hành từ trước.**
Code M1 đã fail-closed theo `hr_employee_nhanvien.id`. Việc còn thiếu là **tài liệu** và **sự nhất quán sang M3/M5** — không phải sửa cơ chế ownership M1.

---

## 3. BẰNG CHỨNG DDL GỐC (trích nguyên văn `SHOW CREATE TABLE`)

### 3.1 `dm_khach_hang` — cột sở hữu khách

```sql
`sale_phu_trach` int(11) DEFAULT NULL,
`nguoi_tao`      int(11) DEFAULT NULL,
`nguoi_sua`      int(11) DEFAULT NULL,
PRIMARY KEY (`id`),
UNIQUE KEY `ma_khach_hang` (`ma_khach_hang`),
UNIQUE KEY `ma_so_thue` (`ma_so_thue`)
-- HẾT. KHÔNG có dòng CONSTRAINT … FOREIGN KEY nào trong toàn bảng.
```

- Kiểu `int(11)` · Nullable **CÓ** · Default `NULL` · FK **KHÔNG** · `ON DELETE` **không áp dụng**
- `ENGINE=InnoDB AUTO_INCREMENT=56 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci`

**Đối chứng dữ liệu (chỉ đếm — không lộ dữ liệu cá nhân, `GOV-PII-HANDLING-001`):**

| Chỉ số | Giá trị |
|---|---|
| Tổng khách hàng | 3 |
| Có `sale_phu_trach` | 3 (0 giá trị NULL) |
| Giá trị phân biệt | 2 (`1`, `2`) |
| Khớp `hr_employee_nhanvien.id` tồn tại | **3/3** |
| Khớp `user_account.id` tồn tại | **2/3** |
| Mồ côi nếu áp FK → `hr_employee_nhanvien(id)` | **0** |
| Mồ côi nếu áp FK → `user_account(id)` | **1** |

> **Đọc số này cho đúng:** giá trị `sale_phu_trach = 2` **không tồn tại** trong `user_account.id` nhưng **có tồn tại** trong `hr_employee_nhanvien.id` ⇒ đây là bằng chứng dữ liệu **nghiêng về employee-id**. Nhưng giá trị `1` tồn tại ở **cả hai bảng** nên không phân định được, và toàn bộ mẫu chỉ có 3 dòng. **Dữ liệu KHÔNG đủ mạnh để tự chứng minh** — kết luận employee-id dựa trên **code** (§6), không dựa trên dữ liệu.

### 3.2 `lenh_san_xuat`

```sql
`ngay_tao` text NOT NULL,
`nguoi_tao` int(11) DEFAULT NULL,
`ngay_sua` text DEFAULT NULL,
`nguoi_sua` int(11) DEFAULT NULL,
...
`hinh_anh_kiem_duyet` longtext CHARACTER SET utf8mb4 COLLATE utf8mb4_bin DEFAULT NULL,
CONSTRAINT `fk_lsx_don_hang_item` FOREIGN KEY (`don_hang_item_id`) REFERENCES `don_hang_item` (`id`),
CONSTRAINT `fk_lsx_pdi` FOREIGN KEY (`phieu_dieu_in_id`) REFERENCES `phieu_dieu_in` (`id`) ON DELETE SET NULL,
```

**KHÔNG có `nguoi_phu_trach`. KHÔNG có `nguoi_phe_duyet`.** Dữ liệu: 7 dòng, `nguoi_tao` **NULL 7/7**.

### 3.3 `phieu_xuat_kho`

```sql
`nguoi_lap`  varchar(100) NOT NULL,
`nguoi_xuat` varchar(100) DEFAULT NULL,
`nguoi_tao`  varchar(100) NOT NULL,
`nguoi_sua`  varchar(100) DEFAULT NULL,
PRIMARY KEY (`so_phieu`)
-- KHÔNG có CONSTRAINT nào.
```

Bảng `phieu_xuat` **không tồn tại**. Dữ liệu hiện tại: **0 dòng**.

### 3.4 `bao_gia`

```sql
`id_nhan_vien_phu_trach` int(11) NOT NULL,
`nguoi_tao` int(11) NOT NULL,
`nguoi_sua` int(11) DEFAULT NULL,
CONSTRAINT `bao_gia_ibfk_1` FOREIGN KEY (`id_khach_hang`)    REFERENCES `dm_khach_hang` (`id`),
CONSTRAINT `bao_gia_ibfk_2` FOREIGN KEY (`id_nguoi_lien_he`) REFERENCES `kh_lien_he` (`id`)
```

**KHÔNG có `nguoi_lap`.** Dữ liệu: 7 dòng, `id_nhan_vien_phu_trach` toàn bộ = `1` ⇒ **không phân định được** id thuộc họ nào.

### 3.5 Hai bảng định danh

```sql
-- user_account:         id INT AI PK · email VARCHAR(100) UNIQUE · 7 dòng · AUTO_INCREMENT=58
-- hr_employee_nhanvien: id INT AI PK · ma_nv VARCHAR(255) UNIQUE · user_id INT UNIQUE NULL
--   CONSTRAINT fk_hr_employee_user FOREIGN KEY (user_id) REFERENCES user_account(id)
--     ON DELETE SET NULL ON UPDATE CASCADE        ← link authoritative (PL4 §5.5)
--   30 dòng · 4/30 có user_id · 3/7 user_account CHƯA gắn nhân viên
```

---

## 4. `sale_phu_trach` — PHƯƠNG ÁN ĐỒNG BỘ **TÀI LIỆU** (đúng phạm vi prompt)

### 4.1 Tình trạng ba bên

| Bên | Đang nói gì | Trạng thái |
|---|---|---|
| **Owner Decision 20/08/2026** | `sale_phu_trach → hr_employee_nhanvien.id` (Phương án B — gắn theo con người) | `OWNER_CONFIRMED` |
| **Code** (`src/lib/security/ownership-guard.ts:▮`) | Đúng y hệt Phương án B, fail-closed, cấm so chéo với cột audit | `CODE_PROVEN` — **ĐÃ KHỚP** |
| **Live DB** | `int(11) NULL`, **không FK** — trung lập: không mâu thuẫn Phương án B, cũng không cưỡng chế nó | `DB_PROVEN` — **KHÔNG CHẶN** |
| **Docs M1.1 (Notion V3.41)** | `sale_phu_trach INT COMMENT 'FK to user_account[id]'` | `DOC_PROVEN` — **SAI so với cả Owner Decision lẫn code** |

⇒ **Chỉ có TÀI LIỆU lệch.** Không cần đổi code, không cần đổi DB để khớp quyết định Owner.

### 4.2 Docs cần ghi gì cho khớp live + khớp Owner Decision

Đề xuất **câu chữ chính xác** để TanPhatAI thay vào tài liệu (Owner duyệt trước khi áp):

```
sale_phu_trach INT NULL
  -- Ý NGHĨA NGHIỆP VỤ: hr_employee_nhanvien.id — nhân viên phụ trách khách
  --   (Owner Decision 20/08/2026, Phương án B: gắn theo CON NGƯỜI, không theo tài khoản đăng nhập).
  -- HIỆN TRẠNG DB (đo 20/08/2026): int(11) DEFAULT NULL, KHÔNG có ràng buộc FOREIGN KEY.
  --   Quan hệ này do TẦNG CODE giữ (src/lib/security/ownership-guard.ts), DB không cưỡng chế.
  -- KHÔNG PHẢI user_account.id. Cột audit nguoi_tao/nguoi_sua mới là user_account.id
  --   — HAI ID DOMAIN KHÁC NHAU, CẤM so sánh chéo.
```

Ba điểm **bắt buộc** phải có trong câu chữ mới:

1. **Tách "ý nghĩa nghiệp vụ" khỏi "ràng buộc DB"** — nếu chỉ viết `FK → hr_employee_nhanvien(id)` thì tài liệu lại sai lần nữa theo chiều ngược lại (DB **không** có FK đó).
2. **Nêu rõ cột audit thuộc domain khác** — đây chính là chỗ tài liệu cũ gây hiểu nhầm.
3. **Đóng dấu ngày đo** — `GOV-LAW-STATE-SEPARATION-001`: số liệu DB là STATE, phải có mốc thời gian.

### 4.3 Các tài liệu cần sửa cùng lượt (quét cùng chủ đề — `GOV-EDIT-PRESERVE-001` yêu cầu 2)

| # | File | Dòng | Nội dung sai hiện tại |
|---|---|---|---|
| 1 | `docs/🏭 ERP TanPhat - FIX/📋 Module M1 - Master Data (13 Bảng - V3 43)/📊 M1 1 - Khách hàng (4 Bảng - V3 41) 2c81d3859bf8815b96fccf71faeaea80.md` | 228 | `sale_phu_trach INT COMMENT 'FK to user_account[id]'` |
| 2 | *(bản trùng)* `… 2c81d3859bf8815b96fccf71faeaea80 (1).md` | ~198 | Y HỆT — **hai bản sao cùng nội dung, phải sửa cả hai** |
| 3 | cùng file #1 và #2 | 229–230 | `nguoi_tao INT COMMENT 'FK to user_account[id]'` / `nguoi_sua` — đúng *domain* nhưng **claim ràng buộc FK không có thật** |
| 4 | `src/types/m3.ts` | 68 | `id_nhan_vien_phu_trach: number // FK → user_account` — chú thích trong **code**, mâu thuẫn quy ước M1 (xem X-1) |

> Điểm #4 nằm trong `src/` nên **phiên này KHÔNG sửa** — chỉ liệt kê để Owner quyết ở phase riêng.

### 4.4 PHÁC THẢO TRÊN GIẤY — nếu về sau Owner muốn cưỡng chế FK ở DB

> ⛔ **Đây là PHÁC THẢO, không phải migration.** Không có file `.sql` nào được tạo trong phiên này. Việc thực thi là **phase riêng**, phải qua Schema Gate (`GOV-SCHEMA-NO-INVENT-001` §H2) + Owner duyệt.

```
-- (PHÁC THẢO — CHƯA CHẠY, CHƯA TẠO FILE)
-- Bước 0. Kiểm mồ côi TRƯỚC: đếm dòng sale_phu_trach NOT NULL không khớp hr_employee_nhanvien.id
--         Đo 20/08/2026: 0 dòng mồ côi  →  hiện tại KHÔNG cần backfill.
-- Bước 1. Nếu tương lai có dòng mang user_account.id:
--           hr_employee_nhanvien.user_id chính là cầu nối (UNIQUE + FK thật).
--           new_value = (SELECT e.id FROM hr_employee_nhanvien e WHERE e.user_id = <giá trị cũ>)
-- Bước 2. Ca KHÔNG map được — tài khoản không có bản ghi nhân viên:
--           Đo 20/08/2026: 3/7 user_account chưa gắn nhân viên.
--           → CẤM tự đặt NULL, CẤM tự gán bừa. Phải xuất danh sách cho Owner phân xử
--             (Owner đã chốt: bàn giao là quyết định của CON NGƯỜI — không auto-reassign).
-- Bước 3. Forward:  ALTER TABLE dm_khach_hang
--                     ADD CONSTRAINT fk_kh_sale FOREIGN KEY (sale_phu_trach)
--                     REFERENCES hr_employee_nhanvien(id) ON DELETE RESTRICT ON UPDATE CASCADE;
--         Vì sao RESTRICT chứ không SET NULL: SET NULL làm khách "vô chủ" âm thầm khi xoá
--         nhân viên — trái đúng thứ luồng REASSIGNMENT_REQUIRED sinh ra để chặn.
-- Bước 4. Rollback: ALTER TABLE dm_khach_hang DROP FOREIGN KEY fk_kh_sale;
--         (rollback SẠCH — thêm ràng buộc không đổi dữ liệu, chỉ đổi ràng buộc)
-- Bước 5. Rủi ro tồn dư: RESTRICT sẽ CHẶN mọi lệnh xoá nhân viên còn giữ khách.
--         Đây là hành vi MONG MUỐN theo PL4, nhưng là THAY ĐỔI HÀNH VI —
--         phải Owner duyệt, KHÔNG được coi là "chỉ thêm ràng buộc cho chặt".
```

---

## 5. KIỂM KÊ ĐẦY ĐỦ CỘT "NGƯỜI" TRONG LIVE DB (202 cột / 92 bảng)

**Tiêu chí lọc:** khớp **danh sách tên khoá tường minh** (`nguoi_*`, `id_nguoi_*`, `id_nhan_vien_phu_trach`, `sale_phu_trach`, `*_by`, `user_id`, `truong_phong_user_id`, `nguoi_phu_trach_ids`). **Không đoán theo mẫu mờ** — đã loại các cột chỉ *nghe giống* (`ngay_duyet`, `tong_nhan_vien`, `thoi_han_thuc_hien`, `nguoi_dai_dien`, `nguoi_lien_he_khan_cap`) vì không phải cột định danh người dùng hệ thống.

| Thống kê | Số |
|---|---|
| Tổng cột người | **202** |
| Kiểu `INT` | 85 |
| Kiểu `VARCHAR` | 114 |
| Kiểu khác (`text`) | 3 |
| **Có ràng buộc FK thật** | **8** |
| **KHÔNG có FK** | **194** |
| Số bảng liên quan | 92 / 99 |

### 5.1 Tám FK người **có thật** trong live DB

| Bảng | Cột | Kiểu | FK thật | ON DELETE | ON UPDATE |
|---|---|---|---|---|---|
| `audit_log` | `nguoi_thuc_hien` | `varchar(100)` | `user_account(email)` | RESTRICT | RESTRICT |
| `hr_bang_luong` | `nguoi_tinh_luong` | `varchar(100)` | `user_account(email)` | RESTRICT | CASCADE |
| `hr_bang_luong` | `nguoi_duyet` | `varchar(100)` | `user_account(email)` | RESTRICT | CASCADE |
| `hr_bang_luong` | `nguoi_chi_luong` | `varchar(100)` | `user_account(email)` | RESTRICT | CASCADE |
| `hr_luong_phu_cap` | `nguoi_duyet` | `varchar(100)` | `user_account(email)` | SET NULL | CASCADE |
| `permission_log` | `user_email` | `varchar(100)` | `user_account(email)` | RESTRICT | CASCADE |
| `user_role_mapping` | `user_email` | `varchar(100)` | `user_account(email)` | CASCADE | CASCADE |
| `hr_employee_nhanvien` | `user_id` | `int(11)` | **`user_account(id)`** | SET NULL | CASCADE |

*(`user_session.user_id → user_account(id)` cũng là FK thật, nhưng là cột phiên đăng nhập, không phải cột nghiệp vụ "ai làm việc gì".)*
*(Cụm HR còn 7 FK thật trỏ `hr_employee_nhanvien(ma_nv)` — `hop_dong.nguoi_dai_dien_ky`, `hr_bang_luong_item.ma_nv`, `hr_cham_cong_raw.ma_nv`, `hr_cham_cong_tinh.ma_nv`, `hr_luong_phu_cap.ma_nv`, `hr_nghi_phep.ma_nv`, `hr_overtime.ma_nv` — trỏ **`ma_nv`**, KHÔNG trỏ `id`.)*

### 5.2 Bảng kiểm kê đầy đủ 202 cột

| Bảng | Cột | Kiểu | Null | FK thật trong DB |
|---|---|---|---|---|
| `audit_log` | `nguoi_thuc_hien` | varchar(100) | NOT NULL | `user_account(email)` |
| `bao_gia` | `id_nhan_vien_phu_trach` | int(11) | NOT NULL | **KHÔNG** |
| `bao_gia` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `bao_gia` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `bao_gia_item` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `bien_ban_nghiem_thu` | `nguoi_lap` | varchar(100) | NOT NULL | **KHÔNG** |
| `bien_ban_nghiem_thu` | `nguoi_ky` | varchar(100) | NULL | **KHÔNG** |
| `bien_ban_nghiem_thu` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `bien_ban_nghiem_thu` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `checklist_template` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `checklist_template` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `chung_tu_ke_toan` | `nguoi_lap` | varchar(100) | NULL | **KHÔNG** |
| `chung_tu_ke_toan` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `chung_tu_ke_toan` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `chung_tu_ke_toan` | `nguoi_tao_email` | varchar(100) | NULL | **KHÔNG** |
| `chung_tu_ke_toan` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `chung_tu_ke_toan` | `nguoi_sua_email` | varchar(100) | NULL | **KHÔNG** |
| `cong_no` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `cong_no` | `nguoi_tao_email` | varchar(100) | NULL | **KHÔNG** |
| `cong_no` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `cong_no` | `nguoi_sua_email` | varchar(100) | NULL | **KHÔNG** |
| `cong_no_doi_chieu` | `nguoi_lap` | varchar(100) | NOT NULL | **KHÔNG** |
| `cong_no_doi_chieu` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `cong_no_doi_chieu` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `cskh_nhat_ky` | `id_nguoi_phu_trach` | int(11) | NOT NULL | **KHÔNG** |
| `cskh_nhat_ky` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `cskh_nhat_ky` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_addon_type` | `nguoi_tao` | varchar(100) | NULL | **KHÔNG** |
| `dm_addon_type` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_auto_pricing_formula` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_auto_pricing_formula` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_bang_gia_cong_doan` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_bang_gia_cong_doan` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_blueprint` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_blueprint` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_cong_doan` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_cong_doan` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_dia_chi_vn` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_dia_chi_vn` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_entity_param_binding` | `nguoi_tao` | varchar(100) | NULL | **KHÔNG** |
| `dm_entity_param_binding` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_form_mau` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_form_mau` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_khach_hang` | `sale_phu_trach` | int(11) | NULL | **KHÔNG** |
| `dm_khach_hang` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_khach_hang` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_menu` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_menu` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_nha_cung_cap` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_nha_cung_cap` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_nhom_universal` | `created_by` | int(11) | NULL | **KHÔNG** |
| `dm_nhom_universal` | `updated_by` | int(11) | NULL | **KHÔNG** |
| `dm_param_registry` | `nguoi_tao` | varchar(100) | NULL | **KHÔNG** |
| `dm_param_registry` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_phong_ban` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_phong_ban` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_phong_ban` | `truong_phong_user_id` | int(11) | NULL | **KHÔNG** |
| `dm_pricing_addon` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_pricing_addon` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_pricing_test_case` | `nguoi_tao` | varchar(100) | NULL | **KHÔNG** |
| `dm_pricing_test_case` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_profit_margin_rule` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_profit_margin_rule` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_quy_trinh` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_quy_trinh` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_san_pham` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_san_pham` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_uom` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_uom` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_uom_conversion` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_uom_conversion` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dm_vai_tro` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `dm_vai_tro` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `dm_vat_tu` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `dm_vat_tu` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `dong_tien` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `don_hang` | `id_nhan_vien_phu_trach` | int(11) | NOT NULL | **KHÔNG** |
| `don_hang` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `don_hang` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `don_hang_item` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `form_file` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `form_phat_hanh` | `nguoi_phat_hanh` | int(11) | NOT NULL | **KHÔNG** |
| `form_phat_hanh` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `giao_dich_ngan_hang` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `giao_dich_ngan_hang` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `hop_dong` | `nguoi_tao_hop_dong` | varchar(100) | NULL | **KHÔNG** |
| `hop_dong` | `nguoi_dai_dien_ky` | varchar(20) | NULL | `hr_employee_nhanvien(ma_nv)` |
| `hop_dong` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `hop_dong` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hop_dong` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hop_dong_chi_tiet` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_bang_luong` | `nguoi_tinh_luong` | varchar(100) | NOT NULL | `user_account(email)` |
| `hr_bang_luong` | `nguoi_duyet` | varchar(100) | NULL | `user_account(email)` |
| `hr_bang_luong` | `nguoi_chi_luong` | varchar(100) | NULL | `user_account(email)` |
| `hr_bang_luong` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_bang_luong` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hr_bang_luong_item` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_ca_lam_viec` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_ca_lam_viec` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hr_cham_cong_raw` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_cham_cong_tinh` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_cham_cong_tinh` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hr_cham_cong_tinh` | `nguoi_khoa` | varchar(100) | NULL | **KHÔNG** |
| `hr_employee_nhanvien` | `user_id` | int(11) | NULL | `user_account(id)` |
| `hr_employee_nhanvien` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `hr_employee_nhanvien` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `hr_luong_phu_cap` | `nguoi_duyet` | varchar(100) | NULL | `user_account(email)` |
| `hr_luong_phu_cap` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_luong_phu_cap` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hr_nghi_phep` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `hr_nghi_phep` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_nghi_phep` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hr_overtime` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `hr_overtime` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_overtime` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `hr_thue_tncn_bac` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_thue_tncn_giam_tru` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `hr_vi_tri` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `hr_vi_tri` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `kho_giao_dich` | `nguoi_thuc_hien` | varchar(20) | NULL | **KHÔNG** |
| `kho_giao_dich` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `kho_thanh_pham` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `kho_thanh_pham` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `kh_dia_chi` | `nguoi_phu_trach_ids` | text | NULL | **KHÔNG** |
| `kh_dia_chi` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `kh_dia_chi` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `kh_lien_he` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `kh_lien_he` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `kiem_ke_kho` | `nguoi_phu_trach` | varchar(100) | NOT NULL | **KHÔNG** |
| `kiem_ke_kho` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `kiem_ke_kho` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `lenh_san_xuat` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `lenh_san_xuat` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `lenh_san_xuat_item` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `lenh_san_xuat_item` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `lich_su_cong_no` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `material_item` | `nguoi_tao` | text | NULL | **KHÔNG** |
| `material_item` | `nguoi_sua` | text | NULL | **KHÔNG** |
| `material_price` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `material_price` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `material_supplier` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `material_supplier` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `mau_hop_dong` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `mau_hop_dong` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `mua_hang` | `nguoi_dat_hang` | varchar(100) | NOT NULL | **KHÔNG** |
| `mua_hang` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `mua_hang` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `mua_hang` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `mua_hang_item` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `notification_queue` | `nguoi_nhan` | int(11) | NOT NULL | **KHÔNG** |
| `phieu_chi` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `phieu_chi` | `nguoi_tao_email` | varchar(100) | NULL | **KHÔNG** |
| `phieu_chi` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `phieu_chi` | `nguoi_sua_email` | varchar(100) | NULL | **KHÔNG** |
| `phieu_chi` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `phieu_dieu_in` | `created_by` | int(11) | NULL | **KHÔNG** |
| `phieu_dieu_in` | `updated_by` | int(11) | NULL | **KHÔNG** |
| `phieu_giao_hang` | `nguoi_nhan` | varchar(100) | NULL | **KHÔNG** |
| `phieu_giao_hang` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `phieu_giao_hang_item` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `phieu_nhap` | `nguoi_nhap` | varchar(20) | NULL | **KHÔNG** |
| `phieu_nhap` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `phieu_nhap` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `phieu_nhap` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `phieu_nhap_item` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `phieu_thu` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `phieu_thu` | `nguoi_tao_email` | varchar(100) | NULL | **KHÔNG** |
| `phieu_thu` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `phieu_thu` | `nguoi_sua_email` | varchar(100) | NULL | **KHÔNG** |
| `phieu_thu` | `nguoi_duyet` | varchar(100) | NULL | **KHÔNG** |
| `phieu_xuat_kho` | `nguoi_lap` | varchar(100) | NOT NULL | **KHÔNG** |
| `phieu_xuat_kho` | `nguoi_xuat` | varchar(100) | NULL | **KHÔNG** |
| `phieu_xuat_kho` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `phieu_xuat_kho` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `phieu_xuat_kho_item` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `pricing_quote_history` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `quy_tien_mat` | `id_nguoi_quan_ly` | int(11) | NULL | **KHÔNG** |
| `quy_tien_mat` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `quy_tien_mat` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `role_action_permission` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `role_action_permission` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `role_data_permission` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `role_data_permission` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `role_field_permission` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `role_field_permission` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `role_menu_permission` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `role_menu_permission` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `system_setting` | `nguoi_tao` | varchar(100) | NOT NULL | **KHÔNG** |
| `system_setting` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `tai_khoan_ngan_hang` | `nguoi_tao` | int(11) | NULL | **KHÔNG** |
| `tai_khoan_ngan_hang` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `task` | `nguoi_thuc_hien` | int(11) | NOT NULL | **KHÔNG** |
| `task` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `task_checklist` | `nguoi_hoan_thanh` | int(11) | NULL | **KHÔNG** |
| `task_comment` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `task_file` | `nguoi_upload` | int(11) | NOT NULL | **KHÔNG** |
| `task_log` | `nguoi_thuc_hien` | int(11) | NOT NULL | **KHÔNG** |
| `thiet_ke_yeu_cau` | `nguoi_tao` | int(11) | NOT NULL | **KHÔNG** |
| `thiet_ke_yeu_cau` | `nguoi_sua` | int(11) | NULL | **KHÔNG** |
| `user_role_mapping` | `nguoi_gan` | varchar(100) | NOT NULL | **KHÔNG** |
| `user_role_mapping` | `nguoi_sua` | varchar(100) | NULL | **KHÔNG** |
| `user_session` | `user_id` | int(11) | NOT NULL | `user_account(id)` |

---

## 6. BẢNG DIFF DOCS ↔ LIVE (232 khai báo trong docs)

**Cách đo:** quét 267 file `.md` trong `docs/`, trích mọi khối ```CREATE TABLE```, lấy các cột người + mọi dòng `FOREIGN KEY … REFERENCES …`, rồi đối chiếu từng khoá `bảng.cột` với `information_schema` của live DB.

| Kết quả | Số | Nghĩa |
|---|---|---|
| **KHỚP** | **97** | docs mô tả đúng live |
| **LỆCH FK** | **48** | cột có thật, kiểu đúng, nhưng docs khai FK mà live KHÔNG có (hoặc khác đích) |
| **LỆCH KIỂU** | **0** | không có ca nào lệch riêng kiểu dữ liệu |
| **CỘT không tồn tại** | **47** | docs khai cột mà live không có |
| **BẢNG không tồn tại** | **40** | docs khai bảng mà live không có (cụm `sx_*`, `portal_*`, `customer_portal_*`, `cskh_task`, `dm_kho`, `material_attribute*`…) |

> **Đọc bảng cho công bằng:** phần lớn ca `LỆCH FK` **không phải lỗi ghi chép**, mà là do docs mô tả *thiết kế dự kiến* trong khi live giữ *bản đã cài*. Ba nhóm phải tách bạch khi TanPhatAI cập nhật tài liệu:
>
> - **Nhóm A — docs sai, phải sửa:** cột tồn tại thật nhưng docs gán FK không có (VD `dm_khach_hang.sale_phu_trach`, `task.nguoi_tao`).
> - **Nhóm B — docs là spec chưa cài:** cả bảng chưa tồn tại (`sx_job`, `portal_*`) ⇒ phải gắn nhãn `PROPOSED`, KHÔNG sửa thành "không có".
> - **Nhóm C — docs mô tả bản thiết kế BỊ THAY:** `bao_gia.nguoi_lap`, `bao_gia.ma_khach_hang`, `bao_gia_item.so_bao_gia` ⇒ phải gắn `SUPERSEDED`, giữ nguyên văn theo `GOV-EDIT-PRESERVE-001`.

### 6.1 Toàn bộ 135 điểm LỆCH

| Trạng thái | Bảng.Cột | Docs ghi | Live thật |
|---|---|---|---|
| LECH-FK | `chung_tu_ke_toan.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `chung_tu_ke_toan.nguoi_lap` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `chung_tu_ke_toan.phieu_chi_id` | ? | phieu_chi(so_phieu_chi) | int(11) | NO_FK |
| LECH-FK | `chung_tu_ke_toan.phieu_thu_id` | ? | phieu_thu(so_phieu_thu) | int(11) | NO_FK |
| LECH-FK | `dm_form_mau.nguoi_sua` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `dm_form_mau.nguoi_tao` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `dm_khach_hang.nguoi_sua` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `dm_khach_hang.nguoi_tao` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `dm_khach_hang.sale_phu_trach` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `dm_nha_cung_cap.nguoi_tao` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `dm_san_pham.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | varchar(20) | NO_FK |
| LECH-FK | `dm_san_pham.nhom_san_pham_id` | ? | dm_nhom_universal(id) | int(11) | NO_FK |
| LECH-FK | `form_phat_hanh.nguoi_phat_hanh` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `hop_dong.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `hop_dong.nguoi_tao_hop_dong` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `hr_nghi_phep.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `hr_overtime.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `kh_dia_chi.nguoi_sua` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `kh_dia_chi.nguoi_tao` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `kh_dia_chi.nhom_dia_chi_id` | ? | dm_nhom_universal(id) | int(11) | NO_FK |
| LECH-FK | `kh_lien_he.nguoi_sua` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `kh_lien_he.nguoi_tao` | INT | user_account(id) | int(11) | NO_FK |
| LECH-FK | `kho_giao_dich.nguoi_thuc_hien` | ? | hr_employee_nhanvien(ma_nv) | varchar(20) | NO_FK |
| LECH-FK | `kiem_ke_kho.nguoi_phu_trach` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `material_price.material_id` | ? | material_item(id) | varchar(50) | NO_FK |
| LECH-FK | `material_supplier.material_id` | ? | material_item(id) | varchar(50) | NO_FK |
| LECH-FK | `mau_hop_dong.nguoi_tao` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `mua_hang_item.material_id` | ? | material_item(id) | varchar(50) | NO_FK |
| LECH-FK | `mua_hang.lien_ket_lenh_sx` | ? | lenh_san_xuat(so_lenh_sx) | varchar(50) | NO_FK |
| LECH-FK | `mua_hang.nguoi_dat_hang` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `mua_hang.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `phieu_chi.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `phieu_chi.so_hop_dong` | ? | hop_dong(so_hop_dong) | varchar(50) | NO_FK |
| LECH-FK | `phieu_giao_hang_item.ma_san_pham` | ? | dm_san_pham(ma_san_pham) | varchar(50) | NO_FK |
| LECH-FK | `phieu_giao_hang.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | varchar(50) | NO_FK |
| LECH-FK | `phieu_giao_hang.so_don_hang` | ? | don_hang(so_don_hang) | varchar(50) | NO_FK |
| LECH-FK | `phieu_nhap.lien_ket_lenh_sx` | ? | lenh_san_xuat(so_lenh_sx) | varchar(50) | NO_FK |
| LECH-FK | `phieu_nhap.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `phieu_nhap.nguoi_nhap` | ? | hr_employee_nhanvien(ma_nv) | varchar(20) | NO_FK |
| LECH-FK | `phieu_thu.nguoi_duyet` | ? | user_account(email) | varchar(100) | NO_FK |
| LECH-FK | `phieu_thu.so_hop_dong` | ? | hop_dong(so_hop_dong) | varchar(50) | NO_FK |
| LECH-FK | `phieu_xuat_kho_item.ma_vat_tu` | ? | dm_vat_tu(ma_vat_tu) | varchar(50) | NO_FK |
| LECH-FK | `phieu_xuat_kho.ma_kho` | ? | dm_kho(ma_kho) | varchar(20) | NO_FK |
| LECH-FK | `task_checklist.nguoi_hoan_thanh` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `task_file.nguoi_upload` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `task_log.nguoi_thuc_hien` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `task.nguoi_tao` | ? | user_account(email) | int(11) | NO_FK |
| LECH-FK | `task.nguoi_thuc_hien` | ? | user_account(email) | int(11) | NO_FK |
| COT-KHONG-TON-TAI | `bao_gia_item.ma_san_pham` | ? | dm_san_pham(ma_san_pham) | — |
| COT-KHONG-TON-TAI | `bao_gia_item.so_bao_gia` | ? | bao_gia(so_bao_gia) | — |
| COT-KHONG-TON-TAI | `bao_gia.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | — |
| COT-KHONG-TON-TAI | `bao_gia.nguoi_lap` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `cskh_nhat_ky.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | — |
| COT-KHONG-TON-TAI | `cskh_nhat_ky.nguoi_thuc_hien` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `dm_bang_gia_cong_doan.ma_cong_doan` | ? | dm_cong_doan(ma_cong_doan) | — |
| COT-KHONG-TON-TAI | `dm_cong_doan.ma_nhom_cong_doan` | ? | dm_nhom_universal(ma_nhom) | — |
| COT-KHONG-TON-TAI | `dm_menu.menu_cha` | ? | dm_menu(ma_menu) | — |
| COT-KHONG-TON-TAI | `dm_nhom_universal.category_id` | ? | dm_nhom_universal(id) | — |
| COT-KHONG-TON-TAI | `dm_nhom_universal.parent_node_id` | ? | dm_nhom_universal(id) | — |
| COT-KHONG-TON-TAI | `dm_phong_ban.phong_ban_cha` | ? | dm_phong_ban(ma_pb) | — |
| COT-KHONG-TON-TAI | `dm_phong_ban.truong_phong` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `dm_san_pham.ma_nhom_san_pham` | ? | dm_nhom_universal(id) | — |
| COT-KHONG-TON-TAI | `form_file.nguoi_tai_len` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `form_phat_hanh.ma_form` | ? | dm_form_mau(ma_form) | — |
| COT-KHONG-TON-TAI | `giao_dich_ngan_hang.phieu_chi_id` | ? | phieu_chi(so_phieu_chi) | — |
| COT-KHONG-TON-TAI | `giao_dich_ngan_hang.phieu_thu_id` | ? | phieu_thu(so_phieu_thu) | — |
| COT-KHONG-TON-TAI | `giao_dich_ngan_hang.tai_khoan_ngan_hang_id` | ? | tai_khoan_ngan_hang(id) | — |
| COT-KHONG-TON-TAI | `kh_dia_chi.khach_hang_id` | ? | dm_khach_hang(id) | — |
| COT-KHONG-TON-TAI | `kh_lien_he.khach_hang_id` | ? | dm_khach_hang(id) | — |
| COT-KHONG-TON-TAI | `kho_giao_dich.ma_kho` | ? | dm_kho(ma_kho) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat_item.ma_don_vi_tinh` | ? | dm_uom(ma_don_vi_tinh) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat_item.ma_lenh_sx` | ? | lenh_san_xuat(ma_lenh_sx) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat_item.ma_san_pham` | ? | dm_san_pham(ma_san_pham) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat_item.ma_vat_tu` | ? | material_item(ma_vat_tu) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat.ma_don_hang` | ? | don_hang(ma_don_hang) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | — |
| COT-KHONG-TON-TAI | `lenh_san_xuat.ma_routing` | ? | sx_routing(ma_routing) | — |
| COT-KHONG-TON-TAI | `phieu_chi.nguoi_chi` | ? | hr_employee_nhanvien(ma_nv) | — |
| COT-KHONG-TON-TAI | `phieu_chi.tai_khoan_ngan_hang_id` | ? | tai_khoan_ngan_hang(id) | — |
| COT-KHONG-TON-TAI | `phieu_thu.nguoi_thu` | ? | hr_employee_nhanvien(ma_nv) | — |
| COT-KHONG-TON-TAI | `phieu_thu.tai_khoan_ngan_hang_id` | ? | tai_khoan_ngan_hang(id) | — |
| COT-KHONG-TON-TAI | `quy_tien_mat.nguoi_quan_ly` | ? | hr_employee_nhanvien(ma_nv) | — |
| COT-KHONG-TON-TAI | `tai_khoan_ngan_hang.ma_nhan_vien` | ? | hr_employee_nhanvien(ma_nv) | — |
| COT-KHONG-TON-TAI | `task_checklist.ma_task` | ? | task(ma_task) | — |
| COT-KHONG-TON-TAI | `task_checklist.task_id` | ? | task(id) | — |
| COT-KHONG-TON-TAI | `task_comment.ma_task` | ? | task(ma_task) | — |
| COT-KHONG-TON-TAI | `task_comment.nguoi_comment` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `task_comment.parent_comment_id` | ? | task_comment(id) | — |
| COT-KHONG-TON-TAI | `task_comment.task_id` | ? | task(id) | — |
| COT-KHONG-TON-TAI | `task_file.task_id` | ? | task(id) | — |
| COT-KHONG-TON-TAI | `task_log.task_id` | ? | task(id) | — |
| COT-KHONG-TON-TAI | `task.nguoi_giao` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `thiet_ke_yeu_cau.nguoi_thiet_ke` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `thiet_ke_yeu_cau.nguoi_yeu_cau` | ? | user_account(email) | — |
| COT-KHONG-TON-TAI | `thiet_ke_yeu_cau.so_don_hang` | ? | don_hang(so_don_hang) | — |
| BANG-KHONG-TON-TAI | `cong_no_khach_hang.ma_khach_hang` | ? | dm_khach_hang(ma_kh) | — |
| BANG-KHONG-TON-TAI | `cong_no_nha_cung_cap.ma_nha_cung_cap` | ? | dm_nha_cung_cap(ma_ncc) | — |
| BANG-KHONG-TON-TAI | `cskh_task.id_khach_hang` | ? | dm_khach_hang(id) | — |
| BANG-KHONG-TON-TAI | `cskh_task.id_nguoi_duoc_giao` | int | NO_FK | — |
| BANG-KHONG-TON-TAI | `cskh_task.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | — |
| BANG-KHONG-TON-TAI | `cskh_task.nguoi_phu_trach` | ? | user_account(email) | — |
| BANG-KHONG-TON-TAI | `cskh_task.nguoi_sua` | int | NO_FK | — |
| BANG-KHONG-TON-TAI | `cskh_task.nguoi_tao` | int | NO_FK | — |
| BANG-KHONG-TON-TAI | `customer_portal_account.ma_khach_hang` | ? | dm_khach_hang(ma_khach_hang) | — |
| BANG-KHONG-TON-TAI | `customer_portal_activity_log.email_khach` | ? | customer_portal_account(email) | — |
| BANG-KHONG-TON-TAI | `customer_portal_quotation.email_khach` | ? | customer_portal_account(email) | — |
| BANG-KHONG-TON-TAI | `customer_portal_quotation.formula_id` | ? | dm_auto_pricing_formula(id) | — |
| BANG-KHONG-TON-TAI | `customer_portal_quotation.so_bao_gia_chinh_thuc` | ? | bao_gia(so_bao_gia) | — |
| BANG-KHONG-TON-TAI | `material_attribute_value.ma_thuoc_tinh` | ? | material_attribute(ma_thuoc_tinh) | — |
| BANG-KHONG-TON-TAI | `material_attribute_value.ma_vat_tu` | ? | material_item(ma_vat_tu) | — |
| BANG-KHONG-TON-TAI | `portal_bao_gia.ma_bao_gia` | ? | m3_bao_gia(ma_bao_gia) | — |
| BANG-KHONG-TON-TAI | `portal_bao_gia.tai_khoan_id` | ? | portal_tai_khoan(id) | — |
| BANG-KHONG-TON-TAI | `portal_hoat_dong.tai_khoan_id` | ? | portal_tai_khoan(id) | — |
| BANG-KHONG-TON-TAI | `portal_phien_dang_nhap.tai_khoan_id` | ? | portal_tai_khoan(id) | — |
| BANG-KHONG-TON-TAI | `portal_tai_khoan.ma_khach_hang` | ? | dm_khach_hang(ma_kh) | — |
| BANG-KHONG-TON-TAI | `sx_job_step.ma_cong_doan` | ? | dm_cong_doan(ma_cong_doan) | — |
| BANG-KHONG-TON-TAI | `sx_job_step.ma_job` | ? | sx_job(ma_job) | — |
| BANG-KHONG-TON-TAI | `sx_job_step.ma_routing_step` | ? | sx_routing_step(id) | — |
| BANG-KHONG-TON-TAI | `sx_job.ma_lenh_sx` | ? | lenh_san_xuat(ma_lenh_sx) | — |
| BANG-KHONG-TON-TAI | `sx_job.ma_routing` | ? | sx_routing(ma_routing) | — |
| BANG-KHONG-TON-TAI | `sx_job.ma_san_pham` | ? | dm_san_pham(ma_san_pham) | — |
| BANG-KHONG-TON-TAI | `sx_log.ma_job` | ? | sx_job(ma_job) | — |
| BANG-KHONG-TON-TAI | `sx_log.ma_job_step` | ? | sx_job_step(id) | — |
| BANG-KHONG-TON-TAI | `sx_log.ma_lenh_sx` | ? | lenh_san_xuat(ma_lenh_sx) | — |
| BANG-KHONG-TON-TAI | `sx_log.ma_qc` | ? | sx_quality_check(ma_qc) | — |
| BANG-KHONG-TON-TAI | `sx_metric.ma_job` | ? | sx_job(ma_job) | — |
| BANG-KHONG-TON-TAI | `sx_metric.ma_job_step` | ? | sx_job_step(id) | — |
| BANG-KHONG-TON-TAI | `sx_quality_check.ma_cong_doan` | ? | dm_cong_doan(ma_cong_doan) | — |
| BANG-KHONG-TON-TAI | `sx_quality_check.ma_job` | ? | sx_job(ma_job) | — |
| BANG-KHONG-TON-TAI | `sx_quality_check.ma_job_step` | ? | sx_job_step(id) | — |
| BANG-KHONG-TON-TAI | `sx_quality_check.ma_san_pham` | ? | dm_san_pham(ma_san_pham) | — |
| BANG-KHONG-TON-TAI | `sx_routing_step.ma_cong_doan` | ? | dm_cong_doan(ma_cong_doan) | — |
| BANG-KHONG-TON-TAI | `sx_routing_step.ma_routing` | ? | sx_routing(ma_routing) | — |
| BANG-KHONG-TON-TAI | `sx_routing.ma_san_pham` | ? | dm_san_pham(ma_san_pham) | — |
| BANG-KHONG-TON-TAI | `task_assignment.ma_task` | ? | task(ma_task) | — |

### 6.2 97 khai báo KHỚP (docs đúng — không cần đụng)

<details><summary>Bấm để mở danh sách 97 khoá khớp</summary>

| Bảng.Cột | Live thật |
|---|---|
| `audit_log.nguoi_thuc_hien` | varchar(100) | user_account(email) |
| `bao_gia_item.id_bao_gia` | int(11) | bao_gia(id) |
| `bao_gia_item.id_san_pham` | int(11) | dm_san_pham(id) |
| `bao_gia_item.nguoi_tao` | int(11) | NO_FK |
| `bao_gia_option.id_bao_gia` | int(11) | bao_gia(id) |
| `bao_gia_option.id_bao_gia_item` | int(11) | bao_gia_item(id) |
| `bao_gia.id_khach_hang` | int(11) | dm_khach_hang(id) |
| `bao_gia.id_nguoi_lien_he` | int(11) | kh_lien_he(id) |
| `bao_gia.id_nhan_vien_phu_trach` | int(11) | NO_FK |
| `bao_gia.nguoi_sua` | int(11) | NO_FK |
| `bao_gia.nguoi_tao` | int(11) | NO_FK |
| `cskh_nhat_ky.id_khach_hang` | int(11) | dm_khach_hang(id) |
| `cskh_nhat_ky.id_nguoi_phu_trach` | int(11) | NO_FK |
| `cskh_nhat_ky.nguoi_sua` | int(11) | NO_FK |
| `cskh_nhat_ky.nguoi_tao` | int(11) | NO_FK |
| `dm_cong_doan.nguoi_sua` | int(11) | NO_FK |
| `dm_cong_doan.nguoi_tao` | int(11) | NO_FK |
| `dm_cong_doan.nhom_cong_doan_id` | int(11) | dm_nhom_universal(id) |
| `dm_dia_chi_vn.nguoi_sua` | int(11) | NO_FK |
| `dm_dia_chi_vn.nguoi_tao` | int(11) | NO_FK |
| `dm_dia_chi_vn.parent_id` | int(11) | dm_dia_chi_vn(id) |
| `dm_nhom_universal.created_by` | int(11) | NO_FK |
| `dm_nhom_universal.nhom_cha_id` | int(11) | dm_nhom_universal(id) |
| `dm_nhom_universal.updated_by` | int(11) | NO_FK |
| `dm_san_pham.nguoi_sua` | int(11) | NO_FK |
| `dm_san_pham.nguoi_tao` | int(11) | NO_FK |
| `dm_uom_conversion.from_uom_id` | int(11) | dm_uom(id) |
| `dm_uom_conversion.nguoi_sua` | int(11) | NO_FK |
| `dm_uom_conversion.nguoi_tao` | int(11) | NO_FK |
| `dm_uom_conversion.nhom_don_vi_tinh_id` | int(11) | dm_nhom_universal(id) |
| `dm_uom_conversion.to_uom_id` | int(11) | dm_uom(id) |
| `dm_uom.nguoi_sua` | int(11) | NO_FK |
| `dm_uom.nguoi_tao` | int(11) | NO_FK |
| `dm_uom.nhom_don_vi_tinh_id` | int(11) | dm_nhom_universal(id) |
| `dm_vat_tu.ma_nhom_vat_tu` | int(11) | dm_nhom_universal(id) |
| `dm_vat_tu.nguoi_sua` | int(11) | NO_FK |
| `dm_vat_tu.nguoi_tao` | int(11) | NO_FK |
| `don_hang_item.id_don_hang` | int(11) | don_hang(id) |
| `don_hang_item.id_san_pham` | int(11) | dm_san_pham(id) |
| `don_hang_item.nguoi_tao` | int(11) | NO_FK |
| `don_hang.id_bao_gia` | int(11) | bao_gia(id) |
| `don_hang.id_khach_hang` | int(11) | dm_khach_hang(id) |
| `don_hang.id_nguoi_lien_he` | int(11) | kh_lien_he(id) |
| `don_hang.id_nhan_vien_phu_trach` | int(11) | NO_FK |
| `don_hang.nguoi_sua` | int(11) | NO_FK |
| `don_hang.nguoi_tao` | int(11) | NO_FK |
| `form_file.form_phat_hanh_id` | int(11) | form_phat_hanh(id) |
| `hop_dong_chi_tiet.so_hop_dong` | varchar(50) | hop_dong(so_hop_dong) |
| `hop_dong.ma_mau` | varchar(50) | mau_hop_dong(ma_mau) |
| `hop_dong.nguoi_dai_dien_ky` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_bang_luong_item.ma_bang_luong` | varchar(50) | hr_bang_luong(ma_bang_luong) |
| `hr_bang_luong_item.ma_nv` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_bang_luong.nguoi_chi_luong` | varchar(100) | user_account(email) |
| `hr_bang_luong.nguoi_duyet` | varchar(100) | user_account(email) |
| `hr_bang_luong.nguoi_tinh_luong` | varchar(100) | user_account(email) |
| `hr_cham_cong_raw.ma_nv` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_cham_cong_tinh.don_xin_phep_id` | varchar(50) | hr_nghi_phep(id) |
| `hr_cham_cong_tinh.ma_ca` | varchar(20) | hr_ca_lam_viec(ma_ca) |
| `hr_cham_cong_tinh.ma_nv` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_employee_nhanvien.nguoi_sua` | int(11) | NO_FK |
| `hr_employee_nhanvien.nguoi_tao` | int(11) | NO_FK |
| `hr_employee_nhanvien.phong_ban_id` | int(11) | dm_phong_ban(id) |
| `hr_employee_nhanvien.user_id` | int(11) | user_account(id) |
| `hr_employee_nhanvien.vi_tri_id` | int(11) | hr_vi_tri(id) |
| `hr_luong_phu_cap.ma_bang_luong` | varchar(50) | hr_bang_luong(ma_bang_luong) |
| `hr_luong_phu_cap.ma_nv` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_luong_phu_cap.nguoi_duyet` | varchar(100) | user_account(email) |
| `hr_nghi_phep.ma_nv` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_overtime.ma_nv` | varchar(20) | hr_employee_nhanvien(ma_nv) |
| `hr_vi_tri.nguoi_sua` | int(11) | NO_FK |
| `hr_vi_tri.nguoi_tao` | int(11) | NO_FK |
| `hr_vi_tri.phong_ban_id` | int(11) | dm_phong_ban(id) |
| `kh_lien_he.dia_chi_phu_trach_id` | int(11) | kh_dia_chi(id) |
| `material_item.dm_vat_tu_id` | int(11) | dm_vat_tu(id) |
| `material_item.ma_nhom_vat_tu` | int(11) | dm_nhom_universal(id) |
| `material_price.lien_ket_po` | varchar(50) | mua_hang(so_po) |
| `material_price.ma_ncc` | varchar(20) | dm_nha_cung_cap(ma_ncc) |
| `material_supplier.ma_ncc` | varchar(20) | dm_nha_cung_cap(ma_ncc) |
| `mua_hang_item.so_po` | varchar(50) | mua_hang(so_po) |
| `mua_hang.ma_ncc` | varchar(20) | dm_nha_cung_cap(ma_ncc) |
| `permission_log.user_email` | varchar(100) | user_account(email) |
| `phieu_giao_hang_item.so_phieu` | varchar(50) | phieu_giao_hang(so_phieu) |
| `phieu_nhap_item.so_phieu_nhap` | varchar(50) | phieu_nhap(so_phieu_nhap) |
| `phieu_nhap.lien_ket_po` | varchar(50) | mua_hang(so_po) |
| `phieu_xuat_kho_item.so_phieu` | varchar(50) | phieu_xuat_kho(so_phieu) |
| `role_data_permission.ma_vai_tro` | varchar(20) | dm_vai_tro(ma_vai_tro) |
| `role_menu_permission.ma_menu` | varchar(50) | dm_menu(ma_menu) |
| `role_menu_permission.ma_vai_tro` | varchar(20) | dm_vai_tro(ma_vai_tro) |
| `task.parent_task_id` | int(11) | task(id) |
| `thiet_ke_yeu_cau.id_bao_gia` | int(11) | bao_gia(id) |
| `thiet_ke_yeu_cau.id_don_hang` | int(11) | don_hang(id) |
| `thiet_ke_yeu_cau.id_khach_hang` | int(11) | dm_khach_hang(id) |
| `thiet_ke_yeu_cau.nguoi_sua` | int(11) | NO_FK |
| `thiet_ke_yeu_cau.nguoi_tao` | int(11) | NO_FK |
| `user_role_mapping.ma_vai_tro` | varchar(20) | dm_vai_tro(ma_vai_tro) |
| `user_role_mapping.user_email` | varchar(100) | user_account(email) |
| `user_session.user_id` | int(11) | user_account(id) |

</details>

---

## 7. BẰNG CHỨNG CODE — `src/` join/filter theo id hay email?

### 7.1 M1 — Ownership khách hàng: **employee id**, tách domain có chủ đích

| Bằng chứng | Nội dung |
|---|---|
| `src/lib/security/ownership-guard.ts:▮` | *"Ownership target: `dm_khach_hang.sale_phu_trach` → `hr_employee_nhanvien.id`; Audit actor: `user_account.id` (cột `nguoi_tao`/`nguoi_sua`) → **HAI ID DOMAIN KHÁC NHAU, TUYỆT ĐỐI KHÔNG so sánh chéo**"* |
| `src/lib/security/ownership-guard.ts:▮` | `canAccessCustomer` chỉ so `customer.sale_phu_trach === ctx.empId`; `empId` không hợp lệ → **DENY** (fail-closed) |
| `src/lib/security/ownership-guard.ts:▮` | `filterOwnedCustomers` lọc `kh.sale_phu_trach === ctx.empId` |
| `src/lib/action-permission.ts:▮` | `getCurrentUserId()` = `SELECT id FROM user_account WHERE email = ?` → **`user_account.id`** (dùng cho audit) |
| `src/lib/action-permission.ts:▮` | `getCurrentEmployeeId()` = `SELECT e.id FROM hr_employee_nhanvien e JOIN user_account u ON u.id = e.user_id WHERE u.email = ?` **AND** nhân viên chưa `nghi_viec`/`nghi_khong_luong` → **`hr_employee_nhanvien.id`** (dùng cho ownership) |
| `src/lib/security/customer-scope.ts:▮` | *"`sale_phu_trach` = `hr_employee_nhanvien.id`; audit actor = `user_account.id` (KHÔNG so chéo)"* |
| `src/app/m1/khach-hang/actions.ts:▮` | `empId = await getCurrentEmployeeId()`; `customers.filter(kh => kh.sale_phu_trach === empId)`; `empId` null → mảng rỗng |
| `src/lib/m1-1-store.ts:▮` | Ghi `sale_phu_trach ?? null` — **đã bỏ** hardcode `?? 1` |
| `src/lib/m1-1-store.ts:▮` | Ghi `nguoi_tao`/`nguoi_sua` = `bundleActorId` (từ `getCurrentUserId`) — **user_account.id** |
| `src/lib/m1/reference-guard.ts:▮` | `countCustomersOwnedBy(employeeId)` → `WHERE sale_phu_trach = ?` bằng **employee id** — đây là cổng chặn cho `REASSIGNMENT_REQUIRED` (PL4) |

> **Kết luận M1:** code **đã** thi hành đúng Phương án B trước khi Owner chốt ngày 20/08. Đường tạo (`resolveSaleAssignmentOnCreate`) và đường cập nhật (`resolveSaleAssignment`) đều khoá quyền gán, không tin giá trị client gửi.

### 7.2 M3 — báo giá / đơn hàng: **CHƯA nhất quán với M1**

| Bằng chứng | Nội dung | Vấn đề |
|---|---|---|
| `src/types/m3.ts:▮` | `id_nhan_vien_phu_trach: number // FK → user_account` | **Chú thích mâu thuẫn quy ước M1.** DB không có FK; nếu là "nhân viên phụ trách" theo nghĩa nghiệp vụ thì phải là `hr_employee_nhanvien.id` giống M1 |
| `src/lib/m3-store.ts:▮` | `const nguoiTao = (await getCurrentUserId()) ?? 1` | `nguoi_tao` = `user_account.id` — **đúng domain audit**, nhưng còn fallback `?? 1` |
| `src/lib/m3-store.ts:▮` | `data.id_nhan_vien_phu_trach ?? 1` | **Hardcode `?? 1`** cho cột phụ trách — đúng cái anti-pattern M1 đã gỡ ngày 01/08/2026 |
| `src/lib/m3-store.ts:▮`, `:▮`, `:▮`, `:▮` | Cùng mẫu `?? 1` / kế thừa từ báo giá sang đơn hàng | Lan sang `don_hang.id_nhan_vien_phu_trach` |
| `src/app/m3/bao-gia/actions.ts:▮` | `id_nhan_vien_phu_trach?: number` nhận thẳng từ client | Không thấy cổng gán tương đương `resolveSaleAssignment` của M1 |

### 7.3 M5 — kho: cột người là **ô nhập tay tự do**, không phải định danh

| Bằng chứng | Nội dung |
|---|---|
| `src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx:▮` | `<Label>Người Lập</Label><Input value={form.nguoi_lap} onChange=…>` — **người dùng gõ chữ tự do**, không phải chọn từ danh sách nhân viên |
| `src/lib/m5-store.ts:▮` | `data.nguoi_lap \|\| "system"` — fallback là **chuỗi `"system"`**, không tương ứng tài khoản nào |
| `src/lib/m5-store.ts:▮`, `:▮`, `:▮`, `:▮` | `const nguoi = nguoiThucHien \|\| "system"` — cùng mẫu cho `kho_giao_dich.nguoi_thuc_hien` |

> **Hệ quả:** với M5, câu hỏi "FK trỏ `user_account(email)` hay `hr_employee_nhanvien(ma_nv)`" **không có đáp án**, vì giá trị lưu **không được ràng buộc là email cũng không là mã nhân viên**. Docs M5.1 đang khai `phieu_nhap.nguoi_nhap → hr_employee_nhanvien(ma_nv)` và `mua_hang.nguoi_dat_hang → user_account(email)` — cả hai đều **không đúng hiện trạng**.

### 7.4 Đối chứng dữ liệu **toàn bộ 114 cột VARCHAR người** (chỉ đếm, không đọc giá trị cá nhân)

| Chỉ số | Giá trị |
|---|---|
| Tổng cột VARCHAR người | **114** |
| Cột **không có dữ liệu nào** | **87** |
| Cột **có dữ liệu** | **27** |
| Tổng giá trị đang lưu | **231** |
| Trong đó **dạng email** (`LIKE '%@%'`) | **3** |
| Khớp `user_account.email` có thật | **3** (đúng 3 giá trị email nói trên, đều ở `user_role_mapping`) |
| Khớp `hr_employee_nhanvien.ma_nv` có thật | **0** |

**Đọc số này:** trong 231 giá trị đang thực sự nằm trong DB dev, **228 giá trị (98,7%) KHÔNG phải email và KHÔNG phải mã nhân viên** — chúng là chuỗi tự do kiểu `system`/`admin` do seed hoặc do người dùng gõ. Chỉ 3 giá trị ở `user_role_mapping` (cột **có** FK thật tới `user_account(email)`) là email hợp lệ.

⇒ **Xác nhận §7.3 bằng dữ liệu:** ở đâu **có** ràng buộc FK thì giá trị đúng chuẩn; ở đâu **không** có FK thì cột "người" trên thực tế đang chứa chuỗi bất kỳ. Đây là hệ quả trực tiếp của `PH-1`.

> **Giới hạn của số đo này:** đây là CSDL **phát triển**, dữ liệu nghiệp vụ rất mỏng (nhiều bảng 0 dòng). Số 98,7% mô tả **dữ liệu dev**, KHÔNG suy ra được cho dữ liệu vận hành thật. Trạng thái: `PARTIAL`.

<details><summary>27 cột có dữ liệu — chi tiết</summary>

```
hr_ca_lam_viec.nguoi_tao          n=1   email=0  khop_user_email=0  khop_ma_nv=0
dm_param_registry.nguoi_tao       n=21  email=0  khop_user_email=0  khop_ma_nv=0
dm_param_registry.nguoi_sua       n=10  email=0  khop_user_email=0  khop_ma_nv=0
dm_addon_type.nguoi_tao           n=8   email=0  khop_user_email=0  khop_ma_nv=0
dm_menu.nguoi_tao                 n=22  email=0  khop_user_email=0  khop_ma_nv=0
dm_menu.nguoi_sua                 n=8   email=0  khop_user_email=0  khop_ma_nv=0
dm_quy_trinh.nguoi_tao            n=3   email=0  khop_user_email=0  khop_ma_nv=0
role_action_permission.nguoi_tao  n=26  email=0  khop_user_email=0  khop_ma_nv=0
role_action_permission.nguoi_sua  n=5   email=0  khop_user_email=0  khop_ma_nv=0
dm_blueprint.nguoi_tao            n=1   email=0  khop_user_email=0  khop_ma_nv=0
dm_nha_cung_cap.nguoi_tao         n=1   email=0  khop_user_email=0  khop_ma_nv=0
dm_nha_cung_cap.nguoi_sua         n=1   email=0  khop_user_email=0  khop_ma_nv=0
dm_pricing_addon.nguoi_tao        n=5   email=0  khop_user_email=0  khop_ma_nv=0
hr_thue_tncn_giam_tru.nguoi_tao   n=2   email=0  khop_user_email=0  khop_ma_nv=0
dm_pricing_test_case.nguoi_tao    n=5   email=0  khop_user_email=0  khop_ma_nv=0
role_menu_permission.nguoi_tao    n=48  email=0  khop_user_email=0  khop_ma_nv=0
role_menu_permission.nguoi_sua    n=22  email=0  khop_user_email=0  khop_ma_nv=0
phieu_nhap.nguoi_tao              n=1   email=0  khop_user_email=0  khop_ma_nv=0
dm_vai_tro.nguoi_tao              n=6   email=0  khop_user_email=0  khop_ma_nv=0
role_data_permission.nguoi_tao    n=3   email=0  khop_user_email=0  khop_ma_nv=0
role_data_permission.nguoi_sua    n=3   email=0  khop_user_email=0  khop_ma_nv=0
kho_thanh_pham.nguoi_tao          n=1   email=0  khop_user_email=0  khop_ma_nv=0
dm_auto_pricing_formula.nguoi_tao n=1   email=0  khop_user_email=0  khop_ma_nv=0
user_role_mapping.nguoi_gan       n=10  email=1  khop_user_email=1  khop_ma_nv=0   ← cột KHÔNG FK
user_role_mapping.nguoi_sua       n=2   email=2  khop_user_email=2  khop_ma_nv=0   ← cột KHÔNG FK
system_setting.nguoi_tao          n=6   email=0  khop_user_email=0  khop_ma_nv=0
hr_thue_tncn_bac.nguoi_tao        n=7   email=0  khop_user_email=0  khop_ma_nv=0
```

*87 cột còn lại có **0 dòng dữ liệu** — gồm toàn bộ `phieu_xuat_kho.*`, `audit_log.nguoi_thuc_hien`, `mua_hang.*`, `hop_dong.*`, `kiem_ke_kho.*`, `bien_ban_nghiem_thu.*`, `kho_giao_dich.*`, `hr_bang_luong.*`…*

</details>

---

## 8. XUNG ĐỘT CÒN LẠI — CẦN OWNER QUYẾT

| Mã | Xung đột | Sự thật đo được | Cần Owner quyết gì | Chặn việc gì nếu chưa trả lời |
|---|---|---|---|---|
| **X-1** | `bao_gia`/`don_hang.id_nhan_vien_phu_trach` thuộc domain nào? | Live: `int NOT NULL`, không FK. Code: `src/types/m3.ts:▮` ghi `// FK → user_account`; M1 lại quy ước phụ trách = `hr_employee_nhanvien.id`. Dữ liệu toàn `1` nên không phân định được. | **"Nhân viên phụ trách báo giá/đơn hàng" có cùng nghĩa với "sale phụ trách khách" không?** Nếu CÓ ⇒ phải là `hr_employee_nhanvien.id`, và chú thích code + docs M3 đều sai. Nếu KHÔNG ⇒ cần định nghĩa riêng bằng văn bản. | Không thể cập nhật docs M3 · không thể áp ownership/hoa hồng cho báo giá · không thể nối luồng bàn giao khách sang M3 |
| **X-2** | Cột người M5 là chuỗi tự do | `phieu_xuat_kho.nguoi_lap` là ô gõ tay, fallback `"system"`; docs khai FK tới `user_account(email)`/`hr_employee_nhanvien(ma_nv)` | **Giữ nguyên ô gõ tay** (⇒ sửa docs bỏ FK, ghi rõ "chuỗi tự do") **hay** chuyển thành chọn nhân viên (⇒ đây là đổi code, phase riêng)? | Không thể cập nhật docs M5.1/M5.2 dứt điểm |
| **X-3** | Hai họ định danh cùng tồn tại | 85 cột INT vs 114 cột VARCHAR; `chung_tu_ke_toan`/`cong_no`/`phieu_thu`/`phieu_chi` giữ **cả hai** (`nguoi_tao` + `nguoi_tao_email`) | **Có chốt một chuẩn duy nhất cho cột audit không?** (VD: mọi cột audit = `user_account.id`, cột email chỉ là snapshot hiển thị). Nếu KHÔNG chốt, tài liệu buộc phải mô tả cả hai mãi mãi. | Docs không thể có một quy tắc chung; mỗi bảng phải ghi riêng |
| **X-4** | 40 bảng trong docs không tồn tại | `sx_job`, `sx_routing`, `sx_log`, `portal_*`, `customer_portal_*`, `cskh_task`, `dm_kho`, `material_attribute*`… | **Gắn nhãn `PROPOSED` (chưa cài) hay `ARCHIVED` (bỏ)?** Hai nhãn dẫn tới hai cách viết tài liệu khác nhau. | TanPhatAI không biết nên giữ hay hạ nhãn 40 khối spec |
| **X-5** | Chưa xác minh trên máy chủ vận hành thật | Phiên này chỉ đọc `tanphat_erp_dev` + `tanphat_erp_mirror` (cùng máy local). Mirror khai là bản sao production nhưng **không tự chứng minh** đang đồng bộ với production **hôm nay**. | **Có cần mở phiên đọc trực tiếp production để đóng dấu `RUNTIME_PROVEN` không?** (`GOV-CONVENTION-BASELINE-002` yêu cầu soi chéo cả hai bên trước khi kết luận) | Mọi claim "production cũng vậy" giữ mức `PROVISIONAL` |
| **X-6** | `dm_khach_hang.nguoi_tao/nguoi_sua` docs khai FK `user_account(id)` | Live không có FK. Domain thì **đúng** (code ghi user_account.id). | **Docs nên ghi "FK" hay "quan hệ logic, DB không cưỡng chế"?** Câu trả lời áp cho **cả 194 cột không FK**, không riêng cột này. | Cách viết chuẩn cho toàn bộ tài liệu schema |

---

## 9. GHI NHẬN NỢ KỸ THUẬT (`GOV-TECH-DEBT-LEDGER-001`)

Các mục dưới đây là **phát hiện của phiên audit**, đã ghi vào `.governance/registry/tech-debt.md`:

| Mã | Nội dung | Vùng ảnh hưởng | Hạn đóng |
|---|---|---|---|
| `DEBT-025` | `src/types/m3.ts:▮` chú thích `id_nhan_vien_phu_trach // FK → user_account` mâu thuẫn quy ước ownership M1 | M3 (báo giá, đơn hàng) | chờ Owner X-1 |
| `DEBT-026` | `src/lib/m3-store.ts:▮/549/697/713/1155` còn hardcode `?? 1` cho `id_nhan_vien_phu_trach` — anti-pattern M1 đã gỡ 01/08/2026 | M3 | chờ Owner X-1 |
| `DEBT-027` | 2 bản sao trùng của file M1.1 Khách hàng trong `docs/` — sửa một bản là còn một bản sai | docs M1 | 27/08/2026 |
| `DEBT-028` | 194/202 cột người không có FK — quy ước chỉ do code giữ, không có cổng tự động phát hiện khi code lệch | toàn DB | NULL |
| `DEBT-029` | Docs Notion V3.43 chứa 2 lớp chồng nhau (khối "DB hiện tại" vs khối "Schema" intent) không gắn nhãn hiệu lực → người đọc lấy nhầm | docs toàn bộ module | chờ Owner X-4 |

---

## 10. BƯỚC KẾ TIẾP

Bàn giao báo cáo này cho Owner. **TanPhatAI** cập nhật tài liệu Notion theo §4.2 + §6 **sau khi** Owner trả lời X-1 → X-6. Mọi thay đổi `src/` hoặc DDL là **phase riêng**, không thuộc phiên này.

---

## 11. PHỤ LỤC — LỆNH ĐÃ CHẠY (tái lập được)

Toàn bộ truy vấn là `SELECT` / `SHOW CREATE TABLE` trên `information_schema` và bảng nghiệp vụ. **Không một lệnh `INSERT`/`UPDATE`/`DELETE`/`ALTER`/`DROP` nào được chạy.**

```
1. SHOW CREATE TABLE dm_khach_hang | lenh_san_xuat | bao_gia | phieu_xuat_kho
                                   | user_account | hr_employee_nhanvien
2. SELECT … FROM information_schema.KEY_COLUMN_USAGE k
     JOIN information_schema.REFERENTIAL_CONSTRAINTS r USING (CONSTRAINT_SCHEMA, CONSTRAINT_NAME)
   WHERE k.REFERENCED_TABLE_NAME IN ('user_account','hr_employee_nhanvien')
3. SELECT … FROM information_schema.COLUMNS LEFT JOIN KEY_COLUMN_USAGE …   (kiểm kê 202 cột)
4. SELECT COUNT(*) … EXISTS(…)   (đối chứng dữ liệu — CHỈ ĐẾM, không đọc giá trị cá nhân)
5. So sánh schema tanphat_erp_dev ↔ tanphat_erp_mirror trên 206 khoá cột người → lệch 0
6. Quét 267 file docs/**/*.md, trích 232 khai báo từ khối CREATE TABLE
7. grep + đọc src/ (ownership-guard, customer-scope, action-permission, m1-1-store,
   m3-store, m5-store, types/m3, reference-guard, khach-hang/actions, phieu-xuat-kho-client)
```

---

════════════ BÁO CÁO KẾT THÚC ════════════

**1. ĐÃ LÀM**
- Xác minh live DB: `SHOW CREATE TABLE` cho 6 bảng trọng yếu (`dm_khach_hang`, `lenh_san_xuat`, `bao_gia`, `phieu_xuat_kho`, `user_account`, `hr_employee_nhanvien`).
- Kiểm kê **202 cột "người"** trên **92/99 bảng** kèm kiểu · nullable · FK target thật · `ON DELETE`, truy từ `information_schema.KEY_COLUMN_USAGE` + `REFERENTIAL_CONSTRAINTS`.
- Đối chứng schema `tanphat_erp_dev` ↔ `tanphat_erp_mirror` (bản sao máy vận hành): **206 khoá cột người, lệch 0**.
- Quét **267 file docs**, trích **232 khai báo** cột-người/FK, dựng bảng diff docs↔live: **97 khớp / 48 lệch FK / 47 cột không tồn tại / 40 bảng không tồn tại**.
- Đối chứng dữ liệu (chỉ đếm) cho `sale_phu_trach`, 17 cột INT trọng yếu, và **toàn bộ 114 cột VARCHAR người** (87 rỗng · 27 có dữ liệu · 231 giá trị · chỉ 3 dạng email, đều nằm ở cột CÓ ràng buộc FK).
- Grep + đọc code `src/` cho M1 / M3 / M5, dẫn `file:line` cho từng claim.
- Viết báo cáo `docs/AUDIT-FK-NGUOI-VA-CUSTOMER-OWNERSHIP-2026-08-20.md`; ghi 5 nợ mới vào sổ nợ; ghi mục Sổ Yêu Cầu Owner.

**2. PHẠM VI**
- **ĐỤNG:** `docs/AUDIT-FK-NGUOI-VA-CUSTOMER-OWNERSHIP-2026-08-20.md` (tạo mới) · `.governance/registry/tech-debt.md` (thêm 5 dòng) · `docs/OWNER-REQUEST-LEDGER.md` (thêm 1 mục).
- **KHÔNG ĐỤNG:** `src/` (0 file) · `migrations/` (0 file) · `sql/` (0 file) · DDL (0 lệnh) · DML (0 lệnh) · version (không bump) · deploy (không) · push (không) · Notion (không).

**3. BẰNG CHỨNG**
- `SHOW CREATE TABLE dm_khach_hang` → `sale_phu_trach int(11) DEFAULT NULL`, **0 CONSTRAINT FOREIGN KEY** → `DB_PROVEN`
- Truy vấn `KEY_COLUMN_USAGE` → **8/202** cột người có FK; 7/8 trỏ `user_account(email)`, 1/8 trỏ `user_account(id)` → `DB_PROVEN`
- `SHOW CREATE TABLE lenh_san_xuat` → **không có** `nguoi_phu_trach`, **không có** `nguoi_phe_duyet` → `DB_PROVEN`
- `SELECT TABLE_NAME … LIKE '%xuat%'` → có `phieu_xuat_kho`, **không có** `phieu_xuat` → `DB_PROVEN`
- `SHOW CREATE TABLE bao_gia` → có `id_nhan_vien_phu_trach int NOT NULL`, **không có** `nguoi_lap` → `DB_PROVEN`
- `src/lib/security/ownership-guard.ts:▮` + `src/lib/action-permission.ts:▮` → ownership so bằng `hr_employee_nhanvien.id` → `CODE_PROVEN`
- Đối chứng đếm: `sale_phu_trach` khớp employee 3/3, khớp user 2/3, mồ côi theo employee = 0 → `DB_PROVEN` (mẫu nhỏ, đã khai rõ giới hạn)
- Đối chứng đếm 114 cột VARCHAR: 231 giá trị, **228 không phải email cũng không phải `ma_nv`** → `DB_PROVEN` trên dev, `PARTIAL` cho production

**4. GHI SỔ YÊU CẦU OWNER**
- [x] **ĐÃ GHI** — mục **#88** trong `docs/OWNER-REQUEST-LEDGER.md`

**5. PUSH BÁO CÁO CÔNG KHAI**
- [ ] ĐÃ PUSH
- [x] **CHƯA PUSH** — lý do: prompt Owner khai **READ-ONLY, cấm push/merge/deploy**. Báo cáo chỉ nằm ở cây làm việc local, **chưa commit, chưa push**. Chờ Owner cho phép.

**6. CÒN SÓT / CHƯA LÀM**
- Chưa mở kết nối tới **máy chủ vận hành thật** (chỉ đọc local + mirror) — xem X-5. *(không nằm trong 7 nhiệm vụ prompt, nhưng ảnh hưởng mức tin cậy)*
- Chưa rà cột người trong **view / stored procedure / trigger** — chỉ rà `BASE TABLE`.
- Chưa đối chiếu docs **Notion trực tuyến** (chỉ đối chiếu bản xuất `.md` trong repo `docs/`) — bản Notion có thể đã mới hơn.
- Chưa kiểm cột người ở các **module chưa có bảng** (`sx_*`, `portal_*`) — không có gì để đo.
- 5 nợ mới ghi sổ: `DEBT-025` … `DEBT-029`.

**7. ĐANG CHỜ OWNER**
- **X-1** `id_nhan_vien_phu_trach` (M3) thuộc domain nào → chặn: cập nhật docs M3, nối ownership/hoa hồng cho báo giá.
- **X-2** cột người M5 giữ chuỗi tự do hay đổi thành chọn nhân viên → chặn: cập nhật docs M5.1/M5.2.
- **X-3** có chốt một chuẩn duy nhất cho cột audit không → chặn: viết quy tắc chung cho tài liệu.
- **X-4** 40 bảng docs-không-có-live gắn `PROPOSED` hay `ARCHIVED` → chặn: hạ nhãn 40 khối spec.
- **X-5** có cần đọc trực tiếp production không → chặn: nâng `PROVISIONAL` lên `RUNTIME_PROVEN`.
- **X-6** cách viết chuẩn cho 194 cột không FK ("FK" vs "quan hệ logic") → chặn: toàn bộ tài liệu schema.
- Cho phép commit/push báo cáo này hay giữ local.

**8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC**
Owner đọc **§8 (X-1 → X-6)** và trả lời **X-1** trước (domain của `id_nhan_vien_phu_trach` ở M3) — đây là nút thắt chặn nhiều việc nhất và là chỗ duy nhất code đang **tự mâu thuẫn** với chính quy ước M1.

**9. CHƯA XÁC MINH ĐƯỢC**
- **Schema máy chủ vận hành thật hôm nay** — vì phiên này không mở kết nối production (prompt giới hạn read-only, và mở SSH tới production là hành động ngoài phạm vi đã giao). *Ai xác minh được:* Owner cho phép + Agent IDE chạy `SHOW CREATE TABLE` trên production.
- **Domain thật của `bao_gia.id_nhan_vien_phu_trach`** — dữ liệu toàn giá trị `1`, tồn tại ở **cả** `user_account.id` lẫn `hr_employee_nhanvien.id` ⇒ dữ liệu không phân định được; chú thích code lại mâu thuẫn M1. *Ai xác minh được:* **Owner** (đây là câu hỏi nghiệp vụ, không phải câu hỏi kỹ thuật).
- **Ý nghĩa thật của 114 cột VARCHAR người trên dữ liệu VẬN HÀNH** — đã đo đủ 114 cột trên DB **dev**: 87 cột rỗng, 27 cột có dữ liệu, 231 giá trị, chỉ **3** dạng email (đều ở cột có FK). Nhưng dev dữ liệu quá mỏng nên **không suy ra được cho production**. *Ai xác minh được:* đọc dữ liệu production thật.
- **Bản Notion trực tuyến có mới hơn bản `.md` trong repo không** — không có quyền/không mở Notion trong phiên này. *Ai xác minh được:* Agent Notion / TanPhatAI.

**10. TRẠNG THÁI CHUNG**
- [ ] PASS
- [x] **PROVISIONAL** — thiếu: (a) xác nhận trên máy chủ vận hành thật (X-5); (b) 6 quyết định Owner X-1…X-6. **Điều kiện lên PASS:** Owner trả lời X-1…X-6 **và** (nếu Owner yêu cầu) chạy lại bộ truy vấn trên production để đóng dấu `RUNTIME_PROVEN`.
- [ ] BLOCKED

> Riêng **7 nhiệm vụ audit tại §3 của prompt: 7/7 đã có câu trả lời kèm bằng chứng** (§1). Trạng thái PROVISIONAL là do lớp bằng chứng production và các quyết định nghiệp vụ còn treo, **không phải** do nhiệm vụ nào bỏ dở.

**11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU**
- Phiên có bị nén ngữ cảnh không: **KHÔNG**
- Tham chiếu đã đọc **trong phiên** (không dùng trí nhớ): `.governance/registry/path-registry.md` · `.governance/registry/tech-debt.md` (phần quy tắc + lịch sử) · `docs/OWNER-REQUEST-LEDGER.md` (đuôi sổ, xác định số mục kế tiếp = #88 — mục cao nhất đang có là #87) · `docs/HUONG-DAN-CHAY-LOCAL.md:104` (xác định vai trò `tanphat_erp_mirror`) · `docs/golive/PL4-hr-org-rbac/PHASE-1B-LINKAGE.md` (link `user_id` authoritative) · `scripts/lib/db-env.ts` · `scripts/load-local-env.cjs`.
- **KHÔNG đụng UI** ⇒ không cần đọc `docs/UI-STANDARD.md` (`GOV-READ-STANDARD-001` không kích hoạt: phiên không sửa chữ hiển thị, lớp trình bày, lề, màu, cột bảng, biểu tượng, component hay bố cục nào).

═══════════════════════════════════════════
