> ## ⚠️ BẢN RÚT GỌN CÔNG KHAI
>
> Đây là **bản công khai đã lược** của báo cáo *Audit tổng lực đợt 2 — verify production · M5 · 22 bảng · flow M3 · naming*.
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

# AUDIT TỔNG LỰC ĐỢT 2 — PRODUCTION · M5 · 22 BẢNG · FLOW M3 · NAMING

**Ngày:** 20/08/2026 · **Loại:** `EVIDENCE` + `STATE` · **Chế độ:** READ-ONLY
**Tổ chức:** multi-agent song song (4 agent) + 1 luồng chính đọc production

> **KHÔNG ĐỤNG:** code (`src/` 0 file) · DDL · DML · migration · commit · push · deploy.
> Trên production **chỉ chạy `SELECT` / `SHOW` / `information_schema`**. Không tạo user, không cấp quyền, không ghi một byte nào.
> Mọi đề xuất sửa code/DB là **phase riêng**, chờ Owner GO.

---

## 0. TÓM TẮT ĐIỀU HÀNH — 7 ĐIỀU ĐÁNG CHÚ Ý NHẤT

| # | Phát hiện | Mức |
|---|---|---|
| **1** | **Production khớp dev/mirror gần như tuyệt đối**: 99 bảng · 1.526 cột · 108 FK · 383 index — **giống hệt**, chỉ lệch **4 cột** (đều thuộc cụm `mua_hang`, do một migration cố ý chưa chạy trên máy vận hành). Toàn bộ 7 phép đo của audit 20/08 nay là **`RUNTIME_PROVEN`**. | ✅ Đóng X-5 |
| **2** | **Ma trận phân quyền CHƯA được nạp lên production.** Production có **3** dòng `role_action_permission` (chỉ 3 hành động HR), dev có **26**. `role_data_permission`: prod **0**, dev **3**. Toàn bộ quyền `can_view_all_customers` / `can_transfer_customer` / `can_view_cost_price`… **không tồn tại trên máy vận hành**. | 🔴 Ngoài phạm vi |
| **3** | **0/30 nhân viên trên production được liên kết tài khoản** (`hr_employee_nhanvien.user_id` NULL toàn bộ). Vì `getCurrentEmployeeId()` join qua `user_id`, mọi tài khoản **không phải admin** sẽ fail-closed về **0 khách hàng**. Hệ thống hiện chỉ vận hành được nhờ đúng **1 tài khoản duy nhất** mang vai trò `ADMIN`. | 🔴 Ngoài phạm vi |
| **4** | **M5: 38 điểm hardcode `"system"`** trong `src/lib/m5-store.ts` + **9 ô gõ tay** trong UI (7 ô thuộc diện X-2 phải chuyển). **0/13 server action của M5 lấy actor từ phiên** — trừ đúng 1 nhánh giao hàng. | 🟠 N2 |
| **5** | **M3 không có cổng quyền nào trên `id_nhan_vien_phu_trach`.** Giá trị thực tế luôn là hằng `1`; `updateBaoGiaAction` nhận `Partial<BaoGia>` nguyên khối (bề mặt mass-assignment); `listBaoGia()`/`listDonHang()` **không lọc phạm vi** — mọi actor có `m3:view` thấy 100% báo giá và đơn hàng. | 🟠 N4 |
| **6** | **Naming:** rà 1.526 cột → **58 cột `ngay_*` đang lưu kiểu `text`** (không sắp xếp/lọc theo ngày đúng được) · **`nguoi_tao` mang HAI nghĩa** (38 bảng `int` FK vs 45 bảng `varchar` chứa tên script như `"pl4-phase1a"`) · `da_thanh_toan` tên đọc như cờ đúng/sai nhưng **chứa số tiền** · chỉ **12,7% cột có mô tả**, 21 mô tả hỏng mã tiếng Việt. | 🟠 N5 |
| **7** | **Đính chính số liệu báo cáo trước:** con số "40 bảng docs-có/live-không" là **40 KHAI BÁO**, không phải 40 bảng. Quét lại đầy đủ mọi `CREATE TABLE` trong docs: **98 bảng khai · 76 có thật · 22 KHÔNG có**. | ⚠️ Đính chính |

---

## 1. N1 — VERIFY PRODUCTION (X-5)

### 1.1 Cách kết nối & bằng chứng an toàn

| Mục | Nội dung |
|---|---|
| Đường vào | SSH bằng khoá có sẵn trong `.env.deploy` (`VPS_KEY`, đã tồn tại trên máy) → mở đường hầm cục bộ `13307 → 127.0.0.1:3306` trên máy chủ |
| Vì sao dùng đường hầm | Cổng 3306 của máy vận hành **đã đóng ra ngoài**. Đường hầm cho phép dùng thư viện `mysql2` ⇒ **mật khẩu không xuất hiện trên dòng lệnh** (tránh đúng lỗi `DEBT-017`) |
| Tài khoản | Tài khoản ứng dụng sẵn có trong `.env.deploy`. **KHÔNG tạo user mới, KHÔNG cấp quyền, KHÔNG đổi gì** |
| Lệnh đã chạy | Chỉ `SELECT`, `SHOW CREATE TABLE`, `SHOW GRANTS`, truy vấn `information_schema`. **0 lệnh ghi** |
| Dọn dẹp | Đường hầm **đã đóng** (xác nhận cổng 13307 không còn mở, 0 tiến trình `ssh` còn lại); file cấu hình tạm chứa bí mật **đã xoá** |
| Danh tính máy đo được | `VERSION()` = `10.11.10-MariaDB-log` · `DATABASE()` = `tanphat-erp` · `@@hostname` = `⟨SEC-03 tên máy chủ⟩` · **99 bảng** |

### 1.2 Bảng verify — 7 phép đo chính

| # | Mục đo | dev + mirror | **PRODUCTION** | Khớp? | Nhãn |
|---|---|---|---|---|---|
| 1 | `dm_khach_hang.sale_phu_trach` | `int(11) DEFAULT NULL`, **0 FK** trên toàn bảng | `int(11) DEFAULT NULL`, **0 FK** trên toàn bảng | ✅ | **`RUNTIME_PROVEN`** |
| 2 | 8 FK người thật | 8 (7 → `user_account(email)`, 1 → `user_account(id)`) | **giống hệt**, cùng `ON DELETE`/`ON UPDATE` từng dòng | ✅ | **`RUNTIME_PROVEN`** |
| 3 | `bao_gia.id_nhan_vien_phu_trach` | `int(11) NOT NULL`, **0 FK**; **không có** `nguoi_lap` | `int(11) NOT NULL`, **0 FK**; đếm cột `nguoi_lap` = **0** | ✅ | **`RUNTIME_PROVEN`** |
| 4 | `lenh_san_xuat` | **không có** `nguoi_phu_trach` **lẫn** `nguoi_phe_duyet`; chỉ `nguoi_tao`/`nguoi_sua` `int(11)` | truy vấn 2 tên cột → **trả về rỗng**; cột người = đúng 2 cột `int(11)` | ✅ | **`RUNTIME_PROVEN`** |
| 5 | Xuất kho | bảng là `phieu_xuat_kho`; **không có** `phieu_xuat`; `nguoi_lap varchar(100) NOT NULL`, 0 FK | bảng khớp `%xuat%` = `lenh_san_xuat`, `lenh_san_xuat_item`, `phieu_xuat_kho`, `phieu_xuat_kho_item` → **không có `phieu_xuat`**; `nguoi_lap varchar(100) NOT NULL`; **FK của bảng = 0** | ✅ | **`RUNTIME_PROVEN`** |
| 6 | Kiểm kê cột người | 202 cột · 85 INT · 114 VARCHAR · 92 bảng | **202 · 85 · 114 · 92** | ✅ | **`RUNTIME_PROVEN`** |
| 7 | 22 bảng docs-có/live-không | vắng mặt trên dev | **vắng mặt trên production (0/22 có mặt)** | ✅ | **`RUNTIME_PROVEN`** |

**Kết luận X-5:** trạng thái `PROVISIONAL` của báo cáo 20/08 **được gỡ**. Cả 7 phép đo nay là `RUNTIME_PROVEN` trên máy vận hành thật.

### 1.3 Diff toàn schema production vs dev — **4 điểm khác, đã truy ra nguyên nhân**

Phép so sánh chữ ký đầy đủ (bảng + cột + FK + index):

| Chỉ số | PRODUCTION | DEV | Khác |
|---|---|---|---|
| Số bảng | 99 | 99 | 0 |
| Số cột | 1.526 | 1.526 | 0 |
| Số FK | 108 | 108 | 0 |
| Số index | 383 | 383 | 0 |
| **Tổng điểm khác** | — | — | **4** |

| # | Cột | PRODUCTION | DEV | Ghi chú |
|---|---|---|---|---|
| 1 | `mua_hang.ma_ncc` | `varchar(20)` **NOT NULL** | `varchar(20)` **NULL** | |
| 2 | `mua_hang_item.don_gia` | `decimal(15,2)` **NOT NULL** | `decimal(15,2)` **NULL** | |
| 3 | `mua_hang_item.thanh_tien` | `decimal(15,2)` **NOT NULL** | `decimal(15,2)` **NULL** | |
| 4 | `mua_hang_item.material_id` | `varchar(50)` **NOT NULL** | `varchar(50)` **NULL** | |

**Nguyên nhân — không phải sự cố, mà là migration cố ý chưa chạy.** Chính file migration tự khai điều đó:

`migrations/20260731_a2p2_mua_hang_draft_nullable.sql` (dòng đầu khối chú thích):
> `-- MOI TRUONG: LOCAL ONLY (MySQL 8.4.3, tanphat_erp_dev).`
> `--   CHUA duoc chay tren VPS (MariaDB 10.11.10) — xem MariaDB compatibility note`
> `--   trong bao cao closeout truoc khi mo Gate VPS.`

**Hệ quả nghiệp vụ (báo để Owner biết, không tự sửa):** hợp đồng A2-P2 cho phép chứng từ mua hàng ở trạng thái `draft` chưa có NCC / đơn giá / thành tiền / vật tư. **Trên production 4 cột đó vẫn `NOT NULL`** ⇒ nếu tính năng draft được dùng trên máy vận hành, lệnh ghi sẽ **lỗi**. Migration treo từ **31/07/2026** (~20 ngày). Cả 2 bảng đang **0 dòng** trên production nên chưa gây hỏng dữ liệu.

### 1.4 Dữ liệu thật trên production (chỉ đếm — không đọc giá trị cá nhân)

**a) `sale_phu_trach`**

| Chỉ số | PRODUCTION | DEV |
|---|---|---|
| Tổng khách hàng | 3 | 3 |
| Có `sale_phu_trach` | 3 | 3 |
| Giá trị phân biệt | **1** (chỉ giá trị `1`) | 2 (`1`, `2`) |
| Khớp `hr_employee_nhanvien.id` | 3/3 | 3/3 |
| Khớp `user_account.id` | **3/3** | 2/3 |
| Mồ côi theo employee | 0 | 0 |
| Mồ côi theo user | **0** | 1 |

> ⚠️ **Điểm quan trọng, phải đọc kỹ:** trên **dev** có giá trị `2` **không tồn tại** trong `user_account.id` ⇒ dev có một mẩu bằng chứng nghiêng về employee-id. Trên **production KHÔNG có mẩu bằng chứng đó** — mọi giá trị đều là `1`, tồn tại ở **cả hai** bảng. Vậy **dữ liệu production KHÔNG phân định được** domain của cột. Kết luận "employee-id" vẫn **chỉ dựa trên code**, đúng như báo cáo trước đã nêu.

**b) Số dòng các bảng nghiệp vụ chính trên production**

```
dm_khach_hang=3   hr_employee_nhanvien=30   user_account=1   bao_gia=7   don_hang=6
lenh_san_xuat=7   task=15   cskh_nhat_ky=3   dm_san_pham=5   dm_vat_tu=19
dm_nha_cung_cap=1 kho_thanh_pham=1
phieu_xuat_kho=0  phieu_nhap=0  mua_hang=0  kho_giao_dich=0  kiem_ke_kho=0
phieu_giao_hang=0 hop_dong=0  cong_no=0  phieu_thu=0  phieu_chi=0  audit_log=0
```

> `audit_log = 0` trên máy vận hành: **chưa có bản ghi kiểm toán nào**. Toàn bộ cụm kho/mua hàng/tài chính cũng đang rỗng.

**c) Cột INT người trên production — vẫn không phân định được domain**

17 cột đo được: **mọi cột có dữ liệu đều chỉ chứa đúng giá trị `1`** (`distinct = 1`), khớp **cả** `hr_employee_nhanvien.id` **lẫn** `user_account.id`. Hai cột rỗng hoàn toàn: `lenh_san_xuat.nguoi_tao` (7 dòng, NULL 7/7) và `dm_phong_ban.truong_phong_user_id` (6 dòng, NULL 6/6).

**d) 114 cột VARCHAR người trên production**

| Chỉ số | PRODUCTION | DEV |
|---|---|---|
| Cột rỗng hoàn toàn | **93** | 87 |
| Cột có dữ liệu | 21 | 27 |
| Tổng giá trị | **124** | 231 |
| Dạng email | **3** | 3 |
| Khớp `user_account.email` | 3 | 3 |
| Khớp `hr_employee_nhanvien.ma_nv` | **0** | 0 |

> **121/124 giá trị (97,6%) không phải email và không phải mã nhân viên.** Đúng 3 giá trị email nằm ở `user_role_mapping` — bảng **có** ràng buộc FK thật tới `user_account(email)`. Xác nhận lại quy luật đã nêu ở báo cáo trước: **ở đâu có FK thì giá trị đúng chuẩn; ở đâu không có FK thì cột chứa chuỗi bất kỳ.**


---

## 2. N2 — CỘT NGƯỜI MODULE M5 (X-2)

**Đích Owner đã chốt (X-2):** `nguoi_lap` = **TỰ ĐỘNG từ user đang tạo phiếu (server-side từ session)** — KHÔNG dropdown, KHÔNG gõ tay.

### 2.1 Bảng truy vết đầy đủ

| cột | kiểu live | FK live | code GHI gì (file:line) | code ĐỌC ở đâu (file:line) | so với đích X-2 |
|---|---|---|---|---|---|
| `mua_hang.nguoi_dat_hang` | `varchar(100)` NOT NULL | KHÔNG | **Gõ tay** — UI [mua-hang-client.tsx:▮](src/app/m5/mua-hang/mua-hang-client.tsx#L853-L858) (`<Input placeholder="VD: Nguyễn Văn A">`) → INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L329) `data.nguoi_dat_hang \|\| "system"`; UPDATE [:374](src/lib/m5-store.ts#L374) | Lọc tìm kiếm [:217](src/app/m5/mua-hang/mua-hang-client.tsx#L217); hiển thị [:640](src/app/m5/mua-hang/mua-hang-client.tsx#L640) | **CẦN CHUYỂN** |
| `mua_hang.nguoi_duyet` | `varchar(100)` NULL | KHÔNG | INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L330) `\|\| null`; UPDATE [:375](src/lib/m5-store.ts#L375). **KHÔNG UI/action nào set giá trị** | Hiển thị [mua-hang-client.tsx:▮](src/app/m5/mua-hang/mua-hang-client.tsx#L644) | **CẦN CHUYỂN** (khi mở luồng duyệt) — hiện là **cột chết**, luôn NULL |
| `mua_hang.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — INSERT [m5-store.ts:▮,337](src/lib/m5-store.ts#L335); UPDATE [:380](src/lib/m5-store.ts#L380). Client không gửi 2 field này | Map ra type [:272,274](src/lib/m5-store.ts#L272). **Không UI nào đọc** | **CẦN CHUYỂN** — thực tế 100% = `"system"` |
| `mua_hang_item.nguoi_tao` | `varchar(100)` NOT NULL | KHÔNG | **Hardcode `"system"`** [m5-store.ts:▮](src/lib/m5-store.ts#L442) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| `phieu_nhap.nguoi_nhap` | `varchar(20)` NULL | KHÔNG | **Gõ tay** — UI [phieu-nhap-client.tsx:▮](src/app/m5/phieu-nhap/phieu-nhap-client.tsx#L864-L869) → INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L1684) | Lọc [:186](src/app/m5/phieu-nhap/phieu-nhap-client.tsx#L186); hiển thị [:571](src/app/m5/phieu-nhap/phieu-nhap-client.tsx#L571) | **CẦN CHUYỂN** — ⚠️ `varchar(20)` rất hẹp |
| `phieu_nhap.nguoi_duyet` | `varchar(100)` NULL | KHÔNG | Chỉ trong allowedFields UPDATE [m5-store.ts:▮](src/lib/m5-store.ts#L1718). **Không UI nào gửi** | Hiển thị [phieu-nhap-client.tsx:▮](src/app/m5/phieu-nhap/phieu-nhap-client.tsx#L710) | **CẦN CHUYỂN** — cột chết |
| `phieu_nhap.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — INSERT [:1687](src/lib/m5-store.ts#L1687); UPDATE [:1736](src/lib/m5-store.ts#L1736) | Hiển thị [phieu-nhap-client.tsx:▮,741](src/app/m5/phieu-nhap/phieu-nhap-client.tsx#L733) | **CẦN CHUYỂN** — `nguoi_sua` còn bị **tái dùng làm actor ghi sổ kho** [:1746](src/lib/m5-store.ts#L1746) |
| `phieu_nhap_item.nguoi_tao` | `varchar(100)` NOT NULL | KHÔNG | **Hardcode `"system"`** [m5-store.ts:▮](src/lib/m5-store.ts#L1950) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| **`phieu_xuat_kho.nguoi_lap`** | `varchar(100)` NOT NULL | KHÔNG | **Gõ tay** — UI [phieu-xuat-kho-client.tsx:▮](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L870-L877) → INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L2065) `\|\| "system"`; UPDATE [:2096](src/lib/m5-store.ts#L2096) | Lọc [:178](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L178); hiển thị [:563](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L563) | **CẦN CHUYỂN** ← **cột trung tâm của X-2** |
| `phieu_xuat_kho.nguoi_xuat` | `varchar(100)` NULL | KHÔNG | **Gõ tay** [phieu-xuat-kho-client.tsx:▮](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L879-L886) → INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L2066) | Hiển thị [:709](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L709) | **CẦN CHUYỂN** |
| `phieu_xuat_kho.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — INSERT [:2069](src/lib/m5-store.ts#L2069); UPDATE [:2100](src/lib/m5-store.ts#L2100) | Hiển thị [phieu-xuat-kho-client.tsx:▮,734](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L726) | **CẦN CHUYỂN** — `nguoi_sua` tái dùng làm actor ghi sổ [:2120](src/lib/m5-store.ts#L2120) |
| `phieu_xuat_kho_item.nguoi_tao` | `varchar(100)` NOT NULL | KHÔNG | **Hardcode `"system"`** [m5-store.ts:▮](src/lib/m5-store.ts#L2331) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| `kiem_ke_kho.nguoi_phu_trach` | `varchar(100)` NOT NULL | KHÔNG | **Gõ tay, 2 ô** — tạo [kiem-ke-kho-client.tsx:▮](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L512-L518), sửa [:600-606](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L600-L606) → INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L2517); UPDATE [:2550](src/lib/m5-store.ts#L2550) | Lọc [:171](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L171); chi tiết [:382](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L382); prefill [:444](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L444) | **CẦN CHUYỂN** |
| `kiem_ke_kho.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — [:2523](src/lib/m5-store.ts#L2523), [:2556](src/lib/m5-store.ts#L2556) | Hiển thị [kiem-ke-kho-client.tsx:▮](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L421) | **CẦN CHUYỂN** |
| `dm_nha_cung_cap.nguoi_lien_he` | `varchar(100)` NULL | KHÔNG | **Gõ tay** [nha-cung-cap-client.tsx:▮](src/app/m5/nha-cung-cap/nha-cung-cap-client.tsx#L903-L913) → INSERT [m5-store.ts:▮](src/lib/m5-store.ts#L137) | Lọc [:184](src/app/m5/nha-cung-cap/nha-cung-cap-client.tsx#L184); thẻ [:494-497](src/app/m5/nha-cung-cap/nha-cung-cap-client.tsx#L494-L497) | **KHÔNG THUỘC DIỆN** — người liên hệ **phía NCC** (đối tác ngoài), gõ tay là đúng |
| `dm_nha_cung_cap.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — [:151,153](src/lib/m5-store.ts#L151), [:195](src/lib/m5-store.ts#L195) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| `kho_giao_dich.nguoi_thuc_hien` | `varchar(20)` NULL | KHÔNG | **2 đường ghi khác namespace**: (a) **gõ tay** [kho-giao-dich-client.tsx:▮](src/app/m5/kho-giao-dich/kho-giao-dich-client.tsx#L538-L544) → [m5-store.ts:▮](src/lib/m5-store.ts#L2436); (b) **tự động** từ engine tồn kho [:828,890,1813,1862,2212,2262](src/lib/m5-store.ts#L828) = `String(hr_employee_nhanvien.id)` (chỉ nhánh giao hàng) hoặc `"system"` | Lọc [:152](src/app/m5/kho-giao-dich/kho-giao-dich-client.tsx#L152); hiển thị [:397](src/app/m5/kho-giao-dich/kho-giao-dich-client.tsx#L397) | **CẦN CHUYỂN** — ⚠️ **cột trộn 3 loại giá trị**: tên gõ tay / ID nhân viên dạng số / `"system"`, trong `varchar(20)` |
| `kho_giao_dich.nguoi_tao` | `varchar(100)` NOT NULL | KHÔNG | (a) [m5-store.ts:▮](src/lib/m5-store.ts#L2439) `\|\| "system"`; (b) auto [:829,891,1814,1863,2213,2263](src/lib/m5-store.ts#L829) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| `kho_thanh_pham.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | [m5-store.ts:▮,1521](src/lib/m5-store.ts#L1496), [:1567](src/lib/m5-store.ts#L1567). Caller ngoài M5: [m4/lenh-san-xuat/actions.ts:▮,987](src/app/m4/lenh-san-xuat/actions.ts#L952) `String(nguoi_sua \|\| "system")` — nhưng tham số `nguoi_sua` ở [:822](src/app/m4/lenh-san-xuat/actions.ts#L822) **không caller nào truyền** ⇒ thực tế `"system"`. `nguoi_sua` còn nhận ID nhân viên tại [:810-811,871-873](src/lib/m5-store.ts#L810) | Hiển thị [kho-thanh-pham-client.tsx:▮](src/app/m5/kho-thanh-pham/kho-thanh-pham-client.tsx#L492-L493) | **CẦN CHUYỂN** — cũng trộn namespace |
| `phieu_giao_hang.nguoi_nhan` | `varchar(100)` NULL | KHÔNG | **Gõ tay** [giao-hang-client.tsx:▮](src/app/m5/giao-hang/giao-hang-client.tsx#L960-L966) → [m5-store.ts:▮](src/lib/m5-store.ts#L625) | Hiển thị [:760](src/app/m5/giao-hang/giao-hang-client.tsx#L760) | **KHÔNG THUỘC DIỆN** — người nhận hàng phía khách |
| `phieu_giao_hang.nguoi_tao` | `varchar(100)` NOT NULL | KHÔNG | **Hardcode `"system"`** [m5-store.ts:▮](src/lib/m5-store.ts#L634). Action [giao-hang/actions.ts:▮](src/app/m5/giao-hang/actions.ts#L65-L70) **không** dùng `getCurrentEmployeeId()` **dù file đã import ở [:17](src/app/m5/giao-hang/actions.ts#L17)** | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** — nghịch lý: nhánh update cùng file *có* session ([:87](src/app/m5/giao-hang/actions.ts#L87)), nhánh create thì không |
| `phieu_giao_hang_item.nguoi_tao` | `varchar(100)` NOT NULL | KHÔNG | **Hardcode `"system"`** [m5-store.ts:▮](src/lib/m5-store.ts#L1000) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| `material_price.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — [:1344,1346](src/lib/m5-store.ts#L1344), [:1390](src/lib/m5-store.ts#L1390) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |
| `material_supplier.nguoi_tao` / `nguoi_sua` | `varchar(100)` | KHÔNG | **Hardcode `"system"`** — [:1185,1187](src/lib/m5-store.ts#L1185), [:1232](src/lib/m5-store.ts#L1232) | KHÔNG TÌM THẤY nơi đọc | **CẦN CHUYỂN** |

### 2.2 Chuỗi truyền `nguoiThucHien` — chỉ 1/3 nhánh có phiên, 2 nhánh là **tham số chết**

**Nhánh A — GIAO HÀNG (nhánh DUY NHẤT lấy được actor từ phiên):**

```
giao-hang-client.tsx  →  src/app/m5/giao-hang/actions.ts:▮  updatePhieuGiaoHangAction
                          :85  requireActionPermission("m5","update")
                          :87  const empId = await getCurrentEmployeeId()   ← NGUỒN GỐC = SESSION
                          :88  updatePhieuGiaoHang(so_phieu, patch, empId ? String(empId) : undefined)
   → action-permission.ts:▮  SELECT e.id FROM hr_employee_nhanvien e
                                    JOIN user_account u ON u.id = e.user_id WHERE u.email = ?
   → m5-store.ts:▮ → :714-732 → :752-758 / :842-848  const nguoi = nguoiThucHien || "system"
   → GHI: kho_thanh_pham.nguoi_sua (:▮)
          kho_giao_dich.nguoi_thuc_hien + nguoi_tao (:▮)
```

**Nhánh B — PHIẾU NHẬP (tham số chết):**
`src/app/m5/phieu-nhap/actions.ts:▮` gọi `updatePhieuNhap(so_phieu_nhap, patch)` — **chỉ 2 tham số**. Tham số thứ 3 `nguoiThucHien` khai ở `m5-store.ts:▮` **luôn `undefined`** ⇒ `:▮`, `:▮` rơi về `"system"`.

**Nhánh C — PHIẾU XUẤT KHO (tham số chết):**
`src/app/m5/phieu-xuat-kho/actions.ts:▮` gọi `updatePhieuXuatKho(soPhieu, data as any)` — **chỉ 2 tham số**. `m5-store.ts:▮` nhận `nguoiThucHien` **luôn `undefined`** ⇒ `:▮`, `:▮` rơi về `"system"`.

**Bảng dùng session trong toàn M5 — đầy đủ 4 điểm, tất cả trong 1 file:**

| file:line | hàm | dùng để làm gì |
|---|---|---|
| [giao-hang/actions.ts:▮](src/app/m5/giao-hang/actions.ts#L17) | import | — |
| [giao-hang/actions.ts:▮](src/app/m5/giao-hang/actions.ts#L30) | `getCurrentEmployeeId()` | Lọc quyền, **không** ghi cột |
| [giao-hang/actions.ts:▮](src/app/m5/giao-hang/actions.ts#L87) | `getCurrentEmployeeId()` | **Điểm ghi DUY NHẤT của toàn M5** |
| [giao-hang/actions.ts:▮](src/app/m5/giao-hang/actions.ts#L200) | `getCurrentEmployeeId()` | Lọc quyền, **không** ghi cột |

- `getCurrentUserId()`: **0 điểm trong toàn bộ M5**.
- `src/lib/m5-store.ts`: **0 điểm** — tầng store hoàn toàn không biết đến phiên đăng nhập.

### 2.3 Chín ô gõ tay trong UI M5

| # | file:line | ô nhập | cột đích | Thuộc diện X-2? |
|---|---|---|---|---|
| 1 | [phieu-xuat-kho-client.tsx:▮](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L870-L877) | Người Lập | `phieu_xuat_kho.nguoi_lap` | **CÓ** (trung tâm) |
| 2 | [phieu-xuat-kho-client.tsx:▮](src/app/m5/phieu-xuat-kho/phieu-xuat-kho-client.tsx#L879-L886) | Người Xuất | `phieu_xuat_kho.nguoi_xuat` | **CÓ** |
| 3 | [phieu-nhap-client.tsx:▮](src/app/m5/phieu-nhap/phieu-nhap-client.tsx#L864-L869) | Người Nhập | `phieu_nhap.nguoi_nhap` | **CÓ** |
| 4 | [mua-hang-client.tsx:▮](src/app/m5/mua-hang/mua-hang-client.tsx#L853-L858) | Người Đặt Hàng | `mua_hang.nguoi_dat_hang` | **CÓ** |
| 5 | [kiem-ke-kho-client.tsx:▮](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L512-L518) | Người Phụ Trách (form TẠO) | `kiem_ke_kho.nguoi_phu_trach` | **CÓ** |
| 6 | [kiem-ke-kho-client.tsx:▮](src/app/m5/kiem-ke-kho/kiem-ke-kho-client.tsx#L600-L606) | Người Phụ Trách (form SỬA) | `kiem_ke_kho.nguoi_phu_trach` | **CÓ** |
| 7 | [kho-giao-dich-client.tsx:▮](src/app/m5/kho-giao-dich/kho-giao-dich-client.tsx#L538-L544) | Người Thực Hiện | `kho_giao_dich.nguoi_thuc_hien` | **CÓ** |
| 8 | [nha-cung-cap-client.tsx:▮](src/app/m5/nha-cung-cap/nha-cung-cap-client.tsx#L903-L913) | Người Liên Hệ | `dm_nha_cung_cap.nguoi_lien_he` | KHÔNG (đối tác ngoài) |
| 9 | [giao-hang-client.tsx:▮](src/app/m5/giao-hang/giao-hang-client.tsx#L960-L966) | Người Nhận | `phieu_giao_hang.nguoi_nhan` | KHÔNG (khách hàng) |

**7/9 ô thuộc diện X-2 cần chuyển.** Không có `<Select>`/dropdown nào cho cột người trong M5.

### 2.4 Điểm sẽ cần sửa khi Owner GO (chỉ liệt kê — KHÔNG sửa)

| Nhóm | Nội dung | Số điểm |
|---|---|---|
| **1. Gỡ ô gõ tay UI** | 7 ô ở §2.3 (#1–#7) + dọn state khởi tạo `kiem-ke-kho-client.tsx:▮,135` · `kho-giao-dich-client.tsx:▮,233` | 7 + 4 |
| **2. Tiêm actor từ phiên ở server action** | 13 action M5 hiện **không** có: `phieu-xuat-kho/actions.ts:▮,40,77,91` · `phieu-nhap/actions.ts:▮,50,94,110` · `mua-hang/actions.ts:▮,67,113,129` · `kiem-ke-kho/actions.ts:▮,27` · `nha-cung-cap/actions.ts:▮,51` · `kho-giao-dich/actions.ts:▮` · `kho-thanh-pham/actions.ts:▮,51` · `gia-vat-tu/actions.ts:▮,45` · `ncc-vat-tu/actions.ts:▮,45` · `giao-hang/actions.ts:▮,128` | ~23 |
| **3. Nối 2 nhánh tham số chết** | `phieu-nhap/actions.ts:▮` · `phieu-xuat-kho/actions.ts:▮` | 2 |
| **4. Gỡ fallback `"system"`** | `m5-store.ts`: `:▮` + 2 fallback lai `:▮` | **38** |
| **5. Đường ghi ngoài M5** | `m4/lenh-san-xuat/actions.ts:▮,987` (+ tham số `:▮` không ai truyền) | 3 |
| **6. Quyết định namespace** | Cột hẹp cần cân nhắc khi đổi sang ID/email: `phieu_nhap.nguoi_nhap` `varchar(20)`, `kho_giao_dich.nguoi_thuc_hien` `varchar(20)` | 2 |
| **7. Nơi ĐỌC sẽ vỡ nếu đổi giá trị lưu** | Lọc theo chữ: 5 điểm · hiển thị thô (sẽ hiện ra số ID): 13 điểm · prefill form: 1 điểm | 19 |

### 2.5 Không xác minh được (N2)

1. **Không tìm thấy nơi ĐỌC** cho: `mua_hang.nguoi_tao/nguoi_sua`, 4 cột `*_item.nguoi_tao`, `kho_giao_dich.nguoi_tao`, `phieu_giao_hang.nguoi_tao`, `dm_nha_cung_cap.nguoi_tao/nguoi_sua`, `material_price.*`, `material_supplier.*` — đã grep trên từng thư mục trang tương ứng, 0 điểm. Các cột này **write-only**.
2. **`mua_hang.nguoi_duyet` và `phieu_nhap.nguoi_duyet` không có đường GHI nào có giá trị** — đã grep `nguoi_duyet` toàn `src/`; trong M5 chỉ có INSERT null, passthrough, allowedFields, và 2 chỗ hiển thị. Không UI/action/API/job nào set.
3. **Không có API route nào cho M5** — `src/app/api/` không có thư mục `m5`. Mọi đường ghi qua server action.
4. **Không có writer nào khác ngoài `m5-store.ts`** cho 10 bảng M5 — đã grep `INSERT INTO <bảng>` toàn `src/`, 0 kết quả ngoài `m5-store`.

---

## 3. N3 — PHÂN LOẠI BẢNG DOCS-CÓ / LIVE-KHÔNG-CÓ

### 3.0 ⚠️ ĐÍNH CHÍNH SỐ LIỆU BÁO CÁO TRƯỚC

Báo cáo `AUDIT-FK-NGUOI-VA-CUSTOMER-OWNERSHIP-2026-08-20.md` ghi **"40 bảng không tồn tại"**. Con số đó là **40 KHAI BÁO** (mỗi cột/FK tính một khai báo), **không phải 40 bảng**. Quét lại đầy đủ mọi khối `CREATE TABLE` trong `docs/`:

| Chỉ số | Số |
|---|---|
| Bảng khai trong docs (`CREATE TABLE`) | **98** |
| Có thật trong live (dev **và** production) | **76** |
| **KHÔNG có trong live** | **22** |

Đã kiểm chéo trên production: **0/22 bảng** có mặt.

### 3.1 Bảng phân loại đầy đủ 22 bảng

| # | Bảng | Module | Tuyên bố số phận trong docs | `src/` | migration/sql/scripts | Ứng viên thay thế | **NHÃN** |
|---|---|---|---|---|---|---|---|
| 1 | `cong_no_khach_hang` | MF Finance | Được **ADDED** ở changelog V3.42, vẫn nằm trong "8 bảng MF" V3.43. Nhưng hardening 04/03/2026 (mới hơn) lại "bám DB thật" trên `cong_no` | 0 | 0 | `cong_no` (live) — mà docs gọi là DEPRECATED | **CẦN OWNER HỎI** |
| 2 | `cong_no_nha_cung_cap` | MF Finance | Như #1, cùng khối changelog | 0 | 0 | `cong_no` | **CẦN OWNER HỎI** |
| 3 | `cskh_task` | M3 CRM | **CÓ tuyên bố bỏ**: `"cskh_task table dropped in P2"`, `"DEPRECATED - NOT altered"`, `"will be phased out"` | 14 điểm — **toàn bộ là comment/cảnh báo, 0 chuỗi SQL thật** | 52 điểm; không có migration CREATE; có NOTE deprecation trong `20260202_v3.44_m8_foundation.sql:▮` | **`task`** (M8, live) | **ARCHIVED** |
| 4 | `customer_portal_account` | M9 Portal | Không tuyên bố bỏ; là **tên đã KHOÁ theo naming V3.43**; M9 readiness = `placeholder` | 2 (chuỗi placeholder UI) | 0 | — | **PROPOSED** |
| 5 | `customer_portal_activity_log` | M9 Portal | Như #4 | 2 | 0 | — | **PROPOSED** |
| 6 | `customer_portal_quotation` | M9 Portal | Như #4 | 2 | 0 | — | **PROPOSED** |
| 7 | `material_attribute` | M1.2 | **CÓ**: `"❌ DEPRECATED - BẢNG 7: material_attribute (Đã loại bỏ V3.27)"` + `"❌ XÓA 2 BẢNG: material_attribute + material_attribute_value (EAV pattern)"` | 0 | 0 | **`material_item`** | **ARCHIVED** |
| 8 | `material_attribute_value` | M1.2 | **CÓ**: `"❌ DEPRECATED - BẢNG 6 … (Đã loại bỏ V3.27)"` | 0 | 0 | **`material_item`** (cột `nha_san_xuat`, `xuat_xu`) | **ARCHIVED** |
| 9 | `material_group` | M1.2 | **CÓ**: `"❌ Đã xóa: dm_nhom_san_pham, material_group → Migrate sang dm_nhom_universal"`; SPEC có luôn `DROP TABLE material_group;` | 0 | 0 | **`dm_nhom_universal`** | **ARCHIVED** |
| 10 | `migration_mapping_v328` | SPEC Universal Group | Docs mô tả `"Tạo bảng tạm để map ID cũ sang ID mới"`; V3.28 đã `MigrationComplete` | 0 | 0 | — (không phải bảng nghiệp vụ) | **ARCHIVED** |
| 11 | `portal_bao_gia` | M9 (spec V3.41) | Không có dòng "bỏ", nhưng **bị superseded bởi naming lock V3.43** | 0 | 0 | **`customer_portal_quotation`** | **ARCHIVED** |
| 12 | `portal_hoat_dong` | M9 (V3.41) | Như #11 | 0 | 0 | **`customer_portal_activity_log`** | **ARCHIVED** |
| 13 | `portal_phien_dang_nhap` | M9 (V3.41) | Như #11 + `version.ts:▮` chốt "M0 là nơi triển khai auth/session hiện tại; M9 chỉ là hướng kế thừa **trong tương lai**" | 0 | 0 | **`user_session`** (M0, đã có migration) | **ARCHIVED** |
| 14 | `portal_tai_khoan` | M9 (V3.41) | Như #11 | 0 | 0 | **`customer_portal_account`** | **ARCHIVED** |
| 15 | `sx_routing` | **M4 Production** | **CÓ tuyên bố hoãn**: `"ROADMAP, giữ nguyên không xóa — chỉ gắn banner ⚠️ CHƯA TRIỂN KHAI"` | 0 | 0 | — | **PROPOSED** |
| 16 | `sx_routing_step` | M4 | Như #15 | 0 | 0 | — | **PROPOSED** |
| 17 | `sx_job` | M4 | Như #15 | 0 | 0 | — | **PROPOSED** |
| 18 | `sx_job_step` | M4 | Như #15 | 0 | 0 | — | **PROPOSED** |
| 19 | `sx_quality_check` | M4 | Như #15 | 0 | 0 | — | **PROPOSED** |
| 20 | `sx_log` | M4 | Như #15 | 0 | 0 | — | **PROPOSED** |
| 21 | `sx_metric` | M4 | Như #15 | 0 | 0 | — | **PROPOSED** |
| 22 | `task_assignment` | M8 | Không tuyên bố bỏ. Chỉ đánh dấu `"(optional)"` / `"OPTIONAL"`, và **bị loại khỏi danh sách "5 bảng M8" của bản index V3.43** | 0 | 0 | Ứng viên yếu: `task.nguoi_thuc_hien` (1 người) — không thay thế 1-1 | **CẦN OWNER HỎI** |

**Tổng: `PROPOSED` 10 · `ARCHIVED` 9 · `CẦN OWNER HỎI` 3.**

### 3.2 Trích nguyên văn — các dòng quyết định

**Cụm M4 `sx_*`** — `docs/🏭 ERP TanPhat - FIX/📋 Module M4 - Production (9 Bảng - V3 43) ….md:33`:
> **M4.2 / M4.3 / M4.4 (routing, job, QC, OEE metrics — 7 bảng, ~114 cột):** đã grep toàn bộ `src/` — **0 dòng code** cho `sx_routing`, `sx_routing_step`, `sx_job`, `sx_job_step`, `sx_quality_check`, `sx_log`, `sx_metric`. Nội dung dưới đây là **ROADMAP, giữ nguyên không xóa** — chỉ gắn banner "⚠️ CHƯA TRIỂN KHAI" ngay trước mục M4.2.

**Cụm `material_*`** — `…/📦 M1 2 - Sản phẩm & Vật tư (3 Bảng - V3 38) ….md:1602`:
> `## ❌ DEPRECATED - BẢNG 7: material_attribute (Đã loại bỏ V3.27)` … `✅ Thay thế: Chỉ cần 2 cột trong material_item là đủ`

`docs/🏭 ERP TanPhat - FIX/📋 SPEC Universal Group Pattern - dm_nhom_universal ….md:421-423`:
> ```
> DROP TABLE dm_nhom_san_pham;
> DROP TABLE material_group;
> DROP TABLE dm_nhom_cong_doan;
> ```

**`cskh_task`** — `src/lib/m3-store.ts:▮`:
> `console.warn("[m3-store] listCskhTask called but cskh_task table was dropped - use M8 task instead")`

`migrations/20260202_v3.44_m8_foundation.sql:▮`:
> `-- NOTE: cskh_task is DEPRECATED per Option B (Hub/Reference Layer)` / `-- Do NOT add FK to cskh_task - it will be phased out`

**Cụm portal → customer_portal** — `src/lib/app-navigation-metadata.ts:▮`:
> `note: "Khoá naming V3.43: customer_portal_account, customer_portal_quotation, customer_portal_activity_log, dm_auto_pricing_formula (shared)."`

**Cụm công nợ — mâu thuẫn hai chiều.** `…/📋 Module MF - Finance (8 Bảng - V3 43) ….md:1098` (khối `V3.42 BREAKING CHANGES → 🗑️ REMOVED`):
> `- ❌ cong_no (polymorphic) - DEPRECATED`

Nhưng cùng file, mục "🔄 Hardening 04/03/2026" (**mới hơn** V3.42), dòng 23:
> `- phieu_thu, phieu_chi, cong_no hiện bám DB thật cho page/actions/stats chính, không còn chạy memory-only path ở luồng vận hành chuẩn.`

### 3.3 Gom cụm — nên quyết theo cụm, không quyết lẻ

| Cụm | Bảng | Đề xuất |
|---|---|---|
| **1. Sản xuất M4** | 7 bảng `sx_*` | **MỘT quyết định: `PROPOSED`.** Bằng chứng đồng nhất tuyệt đối (cùng một câu tuyên bố ROADMAP), 0 điểm code, 0 migration. **Cụm an toàn nhất — không cần hỏi Owner.** |
| **2. Portal nhánh cũ** | `portal_tai_khoan`, `portal_phien_dang_nhap`, `portal_bao_gia`, `portal_hoat_dong` | **MỘT quyết định: `ARCHIVED`** (lý do "đổi tên"), kèm ánh xạ 1-1 sang `customer_portal_*`; riêng `portal_phien_dang_nhap` → `user_session` |
| **3. Portal nhánh V3.43** | 3 bảng `customer_portal_*` | **MỘT quyết định: `PROPOSED`.** ⚠️ **Cụm 2 và 3 là hai mặt của cùng một quyết định** — nếu Owner huỷ hẳn M9 thì cả 7 bảng đều thành `ARCHIVED` |
| **4. EAV vật tư & nhóm** | `material_attribute`, `material_attribute_value`, `material_group` | **MỘT quyết định: `ARCHIVED`.** Cụm chắc chắn thứ hai — có `DROP TABLE` trong SPEC và bảng thay thế đang chạy thật |
| **5. Công nợ MF** | `cong_no_khach_hang`, `cong_no_nha_cung_cap` | **CẦN OWNER HỎI.** Câu hỏi: *"Có còn giữ kế hoạch tách `cong_no` thành 2 bảng AR/AP (spec V3.42), hay chốt luôn `cong_no` sau hardening 04/03/2026 và hạ V3.42 xuống ARCHIVED?"* |
| **6. Đơn lẻ** | `cskh_task` → `ARCHIVED` (đủ mạnh, không cần hỏi) · `task_assignment` → **CẦN OWNER HỎI** (*"M8 có giữ multi-assignee không?"*) · `migration_mapping_v328` → `ARCHIVED` nhưng **nên loại khỏi thống kê nghiệp vụ** (bảng scratch của migration, lọt vào do bộ quét bắt mọi `CREATE TABLE`) | |

### 3.4 Không xác minh được (N3)

1. **Không có migration `CREATE TABLE` nào cho cả 22 bảng.** `cskh_task` từng tồn tại thật (có output `SHOW CREATE TABLE` trong `scripts/schema-output/02_cskh_task.txt:3`), nhưng **lệnh DROP nằm ngoài `migrations/`** — không tìm thấy file DROP versioned.
2. **Không quét `docs/_snapshot*`** (theo yêu cầu loại trừ). Nếu có tuyên bố số phận chỉ nằm trong snapshot thì báo cáo bỏ sót.
3. **Chưa quét `backup-vps-to-local.sql` (3,5 MB), `WORK_LOG.md` (402 KB)** — file backup đặc biệt có thể chứa bằng chứng dứt điểm bảng nào từng tồn tại; đề nghị quét riêng nếu cần chốt `cskh_task` / `task_assignment`.
4. **Ánh xạ `portal_phien_dang_nhap` → `user_session`** là **suy luận** từ `src/lib/version.ts:▮` + `migrations/20260309_m0_internal_user_session.sql:▮`; **không có dòng docs nào nói thẳng** điều đó.
5. Báo cáo trước (`AUDIT-FK-…:471`) từng gợi ý gắn `PROPOSED` cho **cả** `portal_*`. Đề xuất ở đây (`ARCHIVED` cho `portal_*`) dựa trên bằng chứng naming-lock V3.43 **cụ thể hơn**, xuất hiện **sau** gợi ý đó. Nếu Owner ưu tiên dòng 471, 4 bảng `portal_*` chuyển thành `PROPOSED`.

---

## 4. N4 — FLOW MAP `id_nhan_vien_phu_trach` (X-1)

### 4.1 Sơ đồ luồng — `bao_gia.id_nhan_vien_phu_trach`

**Đường TẠO MỚI:**

```
[UI]  bao-gia-client.tsx:▮  handleSave() dựng payload
      → payload gồm: id_khach_hang, id_nguoi_lien_he, ngay_bao_gia, ngay_hieu_luc,
        ghi_chu, trang_thai, items[]
      → KHÔNG CÓ id_nhan_vien_phu_trach   ← không có ô nhập, không có dropdown
        ↓ (:▮)
[ACTION] m3/bao-gia/actions.ts:▮  createBaoGiaBundleAction(data)
      :73  khai `id_nhan_vien_phu_trach?: number`   ← OPTIONAL, nhận thẳng từ client
      :96  requireActionPermission("m3","create")   ← CHỈ kiểm quyền CRUD cấp MENU
      :97  createBaoGiaBundle(data)                 ← truyền nguyên payload, KHÔNG resolve
        ↓
[STORE] m3-store.ts:▮  createBaoGiaBundle()
      :425  const nguoiTao = (await getCurrentUserId()) ?? 1     (audit actor = user_account.id)
      :442  GHI  data.id_nhan_vien_phu_trach ?? 1                ← HARDCODE FALLBACK = 1
      :549  object trả về cũng ?? 1
        ↓
[SQL]  m3-store.ts:▮  INSERT INTO bao_gia (…, id_nhan_vien_phu_trach, …) VALUES (…)
```

> **Trên đường UI thực tế, giá trị LUÔN = hằng `1`.** Không phiên, không tra nhân viên, không cổng quyền.

**Đường SỬA:** `bao-gia-client.tsx:▮` `buildBundlePayload()` cũng **không** có cột này → `actions.ts:▮` → `m3-store.ts:▮` ghi `data.… ?? existing.id_nhan_vien_phu_trach` (giữ nguyên giá trị cũ).

**Đường SỬA HEADER (bề mặt lộ ra nhưng UI không dùng):** `actions.ts:▮` `updateBaoGiaAction(id, data: Partial<BaoGia>)` → `m3-store.ts:▮` build `UPDATE … id_nhan_vien_phu_trach = ?` từ **giá trị client gửi, không lọc**. Grep `updateBaoGiaAction` toàn `src/` → **chỉ 1 kết quả là chính định nghĩa**, không client nào gọi. Nhưng đây là Next.js Server Action (`"use server"` ở `actions.ts:▮`) ⇒ **vẫn là endpoint POST gọi được từ ngoài**.

### 4.2 Sơ đồ luồng — `don_hang.id_nhan_vien_phu_trach`

**Đường TẠO — kế thừa 100% từ báo giá, không có đường tạo thủ công:**

```
[UI]  don-hang-client.tsx (chọn báo giá + chọn item)
      → grep `id_nhan_vien_phu_trach` trong src/app/m3/**/*.tsx = 0 kết quả
        ↓
[ACTION] m3/don-hang/actions.ts:▮  createDonHangFromBaoGiaAction({baoGiaId, selectedBaoGiaItemIds})
      :59  requireActionPermission("m3","create")
      ⇒ input CHỈ 2 trường; client KHÔNG thể gửi người phụ trách
        ↓
[STORE] m3-store.ts:▮  createDonHangFromBaoGia()
      :1111  getBaoGiaBundle(...)               ← đọc báo giá nguồn
      :1113-1115  gate DUY NHẤT: trang_thai phải = 'approved_for_order'
      :1155  GHI  baoGia.id_nhan_vien_phu_trach ← KẾ THỪA nguyên xi
      :1164  nguoi_tao = hằng 1  (nhánh này KHÔNG dùng getCurrentUserId)
        ↓
[SQL]  m3-store.ts:▮  INSERT INTO don_hang (…, id_nhan_vien_phu_trach, …)
```

**Đường SỬA — KHÔNG TỒN TẠI.** Không có `UPDATE don_hang SET … id_nhan_vien_phu_trach` ở bất kỳ đâu. `updateDonHangStatus` (`m3-store.ts:▮`) chỉ đụng `trang_thai`, `ngay_giao_thuc_te`, `ngay_sua`, `nguoi_sua`. ⇒ Sau khi tạo, cột này **bất biến trong code**; muốn đổi phải sửa DB tay.

### 4.3 Mọi điểm đọc / ghi (tổng 23 điểm trong `src/` + 2 seed + 5 script)

| file:line | Đọc/Ghi | Ngữ cảnh | Giá trị từ đâu |
|---|---|---|---|
| `src/types/m3.ts:▮` | khai báo | `BaoGia.id_nhan_vien_phu_trach: number // FK → user_account` | **DEBT-025** — chú thích mâu thuẫn quy ước M1 |
| `src/types/m3.ts:▮` | khai báo | `DonHang.id_nhan_vien_phu_trach: number` — **không chú thích domain** | — |
| `src/lib/m3-store.ts:▮` / `:▮` | khai báo | shape row `BaoGiaRow` / `DonHangRow` | DB |
| `src/lib/m3-store.ts:▮` / `:▮` | **ĐỌC** | `rowToBaoGia` / `rowToDonHang` — mọi `SELECT *` đi qua đây | DB |
| `src/lib/m3-store.ts:▮` | ĐỌC | `listBaoGia` — `SELECT * FROM bao_gia ORDER BY id DESC` | DB — **không WHERE, không scope** |
| `src/lib/m3-store.ts:▮` | ĐỌC | `getBaoGia` | DB |
| `src/lib/m3-store.ts:▮` | ĐỌC | `listDonHang` — `SELECT * FROM don_hang ORDER BY id DESC` | DB — **không WHERE, không scope** |
| `src/lib/m3-store.ts:▮` · `:▮` | ĐỌC | `getDonHangBundle` · `updateDonHangStatus` đọc lại row | DB |
| `src/lib/m3-store.ts:▮` | khai báo | `BaoGiaBundleInput.id_nhan_vien_phu_trach?: number` — **optional** | client |
| `src/lib/m3-store.ts:▮` / `:▮` / `:▮` | **GHI** | cột trong INSERT / `?? 1` / mirror | client (thực tế undefined) → **hardcode 1** |
| `src/lib/m3-store.ts:▮` / `:▮` / `:▮` | **GHI + ĐỌC** | UPDATE `saveBaoGiaBundle` / `?? existing.…` / mirror | client hoặc giá trị cũ |
| `src/lib/m3-store.ts:▮` | **GHI** | `updateBaoGia` build dynamic UPDATE | **client gửi thẳng, không lọc** |
| `src/lib/m3-store.ts:▮` / `:▮` / `:▮` | **GHI + ĐỌC** | INSERT `don_hang` / kế thừa / mirror | **kế thừa từ `bao_gia`** |
| `src/app/m3/bao-gia/actions.ts:▮` | khai báo | tham số action | **bề mặt nhận từ client** |
| `src/app/m3/bao-gia/actions.ts:▮` | **ĐỌC** | `nhan_vien_phu_trach: baoGia.id_nhan_vien_phu_trach` → workflow | DB |
| `src/app/m3/don-hang/actions.ts:▮` | **ĐỌC** | như trên, cho đơn hàng | DB |
| `src/lib/workflow-service.ts:▮` | khai báo | `EntityData.nhan_vien_phu_trach?: number` | caller |
| `src/lib/workflow-service.ts:▮` | **ĐỌC** | `resolveAssignee`/`resolveRecipient` dùng key này làm **user id người nhận** | EntityData |
| `src/lib/workflow-integration-example.ts:▮` | **ĐỌC** | hàm mẫu, export nhưng không ai gọi trong `src/` | object BaoGia |
| `migrations/20260328_dm_quy_trinh_seed.sql:▮` / `:▮` | tiêu thụ (data) | `bao_gia` và `don_hang`: `notify.to = "nhan_vien_phu_trach"` | seed workflow |
| `scripts/phase6-m8-fail-proof.ts:▮` | GHI (script) | INSERT `bao_gia` với hằng `1` | hardcode |
| `scripts/test-r3-design-task.ts:▮,56` | ĐỌC + GHI | đọc `bg.id_nhan_vien_phu_trach` rồi ghi vào `thiet_ke_yeu_cau.nguoi_tao` | **trộn thẳng 2 domain trong 1 câu INSERT** |
| `scripts/tests/h3-bao-gia-don-gate.test.ts:▮,63` · `bao-gia-print-parity.test.ts:▮` · `form-output-security-browser.test.ts:▮` | fixture | luôn `1` | hardcode |

**Không tồn tại bất kỳ:** `WHERE id_nhan_vien_phu_trach = ?`, `JOIN … ON … id_nhan_vien_phu_trach`, hay điểm hiển thị cột này ra UI.

### 4.4 Đối chiếu M1 vs M3 — vì sao X-1 quan trọng

| Tiêu chí | **M1** (`dm_khach_hang.sale_phu_trach`) | **M3** (`bao_gia`/`don_hang.id_nhan_vien_phu_trach`) |
|---|---|---|
| Domain ID được khai | `hr_employee_nhanvien.id`, khai rõ, **cấm so chéo** — `ownership-guard.ts:▮` | **Mâu thuẫn**: `types/m3.ts:▮` ghi `user_account`; `types/m3.ts:▮` không ghi gì; DB không có FK ⇒ **không ai phân xử** |
| Ai quyết giá trị khi TẠO | **Server** — `resolveSaleAssignmentOnCreate` (`ownership-guard.ts:▮`) | **Không ai.** Client có thể gửi; UI không gửi ⇒ rơi về hằng `1` |
| Ai quyết giá trị khi SỬA | **Server** — `resolveSaleAssignment` (`ownership-guard.ts:▮`) | Client gửi gì ghi nấy (`m3-store.ts:▮`) |
| Cổng quyền gán/chuyển giao | **Có** — `hasSpecificAction("can_transfer_customer")` | **KHÔNG CÓ.** Grep `resolveSaleAssignment` toàn `src/` → chỉ ở `ownership-guard.ts` + `m1/khach-hang/actions.ts`. M3 chỉ có `requireActionPermission("m3", …)` = quyền CRUD cấp **module**, không biết đến chủ sở hữu bản ghi |
| Fallback hardcode | **Đã gỡ** 01/08/2026; cột nullable, `NULL` = chưa có người phụ trách | **Còn nguyên** 5 điểm; cột live là **`NOT NULL`** nên **không có cách biểu diễn "chưa gán"** |
| Fail-closed | **Có** — `ownership-guard.ts:▮,52`; `m1-1-store.ts:▮` ném `OWNERSHIP_UNRESOLVED` | **Fail-open** — không resolve được thì ghi `1` và tiếp tục thành công |
| Lọc phạm vi đọc | **Có** — `listCustomersScoped`, `listOwnedCustomerCodes` | **Không dùng cột này để lọc** (xem §4.5) |
| UI chọn người phụ trách | **Có** — dropdown **nhân viên** (`wizard-client.tsx:▮`, nguồn `m1/nhan-su/actions.ts:▮`) | **Không có ô nào** |
| Bàn giao khi nghỉ việc | **Có chặn** — `m1/nhan-su/actions.ts:▮` `REASSIGNMENT_REQUIRED` | Không có; `don_hang` còn **không có đường UPDATE** cột này |

### 4.5 Phạm vi đọc của M3 — phát hiện quan trọng

Cột này **KHÔNG** được dùng để lọc phạm vi. Phạm vi M3 hiện lọc **theo khách hàng**, và **chỉ lọc các danh sách phụ trợ**:

- `m3/bao-gia/page.tsx:▮` `listCustomersScoped()` ✅ · `:▮` lọc liên hệ theo `ownedMaKh` ✅ · `:▮` lọc sản phẩm ✅
- **Nhưng** `m3/bao-gia/page.tsx:▮` gọi `listBaoGia()` = `SELECT * FROM bao_gia ORDER BY id DESC` (`m3-store.ts:▮`) — **toàn bộ báo giá xuống client, không lọc**
- `m3/don-hang/page.tsx:▮`: `listDonHang()` + `listDonHangItems()` **không lọc**
- Client cũng không lọc lại: `bao-gia-client.tsx:▮` chỉ lọc theo `searchTerm` + `statusFilter`

⇒ **Mọi actor có quyền `m3:view` đang thấy 100% báo giá và đơn hàng của mọi Sale.**

### 4.6 Điểm sẽ cần sửa khi Owner GO (chỉ liệt kê — KHÔNG sửa)

| # | file:line | Vấn đề | Rủi ro nếu bỏ qua |
|---|---|---|---|
| 1 | `src/types/m3.ts:▮` | Chú thích `// FK → user_account` | Lập trình viên sau so `=== getCurrentUserId()` → lệch namespace, phạm vi sai âm thầm (`DEBT-028`) |
| 2 | `src/types/m3.ts:▮` | Không có chú thích domain | Cùng cột, hai cách hiểu |
| 3 | `src/lib/m3-store.ts:▮` | `?? 1` khi INSERT | Mọi báo giá rơi về người id=1; Sale tạo xong không sở hữu |
| 4 | `src/lib/m3-store.ts:▮` | `?? 1` mirror vào object trả về | Client tin giá trị `1` là thật |
| 5 | `src/lib/m3-store.ts:▮` · 6 · `:▮` | `?? existing.…` + mirror | Rác gốc **không bao giờ** được làm sạch qua UI |
| 7 | `src/lib/m3-store.ts:▮` | Ghi thẳng giá trị client gửi | Bất kỳ ai có `m3:update` gán báo giá cho người khác, vòng qua mọi quyền chuyển giao |
| 8 | `src/app/m3/bao-gia/actions.ts:▮,96-97` | Action nhận rồi truyền thẳng, không resolve | Lỗ gán chéo ngay đường TẠO |
| 9 | `src/app/m3/bao-gia/actions.ts:▮` | Nhận `Partial<BaoGia>` nguyên khối, không allow-list | **Mass-assignment**: client đặt được `id_nhan_vien_phu_trach`, `nguoi_tao`, `tong_tien`, `trang_thai`… |
| 10 | `src/lib/m3-store.ts:▮` (+`:▮`) | Đơn hàng kế thừa giá trị rác; `nguoi_tao` = hằng `1` | Một giá trị sai ở `bao_gia` nhân bản sang `don_hang`, rồi sang M8/M4 |
| 11 | `src/lib/m3-store.ts:▮` | `(await getCurrentUserId()) ?? 1` | **Fail-open**: hết phiên/DB lỗi (`action-permission.ts:▮` nuốt exception) → audit actor = 1 |
| 12 | Không có đường ghi cho `don_hang` | Không thể chuyển giao đơn hàng bằng code | Nhân viên nghỉ việc → đơn hàng treo tên người đó vĩnh viễn, phải sửa DB tay |
| 13 | `workflow-service.ts:▮` + `20260328_dm_quy_trinh_seed.sql:▮,29` | `resolveRecipient` coi giá trị là **user id** người nhận | Khi nối thông báo thật (hiện `void to`, chưa gửi), **gửi nhầm người ngay ngày đầu** |
| 14 | `scripts/test-r3-design-task.ts:▮` | Dùng `bg.id_nhan_vien_phu_trach` làm `nguoi_tao` | Chạy script là gieo dữ liệu lệch domain vào bảng thật |
| 15 | `m3/bao-gia/page.tsx:▮` · `m3/don-hang/page.tsx:▮` | Danh sách **không** qua scope nào | Xem §4.5 |
| 16 | `src/lib/workflow-integration-example.ts:▮` | File "example" export hàm thật, không ai gọi | Code chết dễ bị sao chép, nhân bản pattern sai |

### 4.7 Không xác minh được (N4)

1. **Domain thật của cột** — không xác định được từ code (đó chính là X-1 đang treo). Dữ liệu **cả dev lẫn production** đều chỉ có giá trị `1`, tồn tại ở cả hai bảng ⇒ **dữ liệu không phân xử được**. Đây là **câu hỏi nghiệp vụ**, không phải kỹ thuật.
2. **Có ai gọi `updateBaoGiaAction` từ ngoài codebase** (client tự chế, `curl`) — không kiểm chứng được bằng grep; chỉ khẳng định trong `src/` không có caller.
3. **`workflow-integration-example.ts` có chạy ở runtime không** — không thấy caller trong các file đã đọc; không khẳng định tuyệt đối.

---

## 5. N5 — TÊN BẢNG / TÊN CỘT KHÓ HIỂU

**Nguồn:** 99 bảng · 1.526 cột · 108 khoá ngoại (`information_schema`, 20/08/2026).
**Cách đếm chi phí:** đếm số **lần xuất hiện** (không phải số file) trên 3 vùng: `src/` · `migrations/`+`sql/`+`scripts/` · `docs/` (loại `docs/_snapshot*`).
**Ngưỡng:** THẤP <10 · TRUNG BÌNH 10–50 · CAO 51–200 · RẤT CAO >200.

### 5.1 Bảng đề xuất (65 mục)

| # | Nhóm | Tên hiện tại | Vì sao khó hiểu | Tên đề xuất | src/ | mig+sql+scripts | docs | Chi phí | Nên đổi? |
|---|---|---|---|---|---|---|---|---|---|
| 1 | N-A | `phieu_dieu_in.sl_can_lay` | `sl` = số lượng | `so_luong_can_lay` | 8 | 0 | 0 | THẤP (8) | **NÊN** |
| 2 | N-A | `lenh_san_xuat.sl_giay_gia_cong` | `sl` viết tắt | `so_luong_giay_gia_cong` | 8 | 0 | 1 | THẤP (9) | **NÊN** |
| 3 | N-A | `lenh_san_xuat.sl_carton` | `sl` viết tắt | `so_luong_carton` | 8 | 0 | 1 | THẤP (9) | **NÊN** |
| 4 | N-A | `lenh_san_xuat.loai_giay_gc_id` | `gc` = gia công, dễ tưởng "giấy carton" | `loai_giay_gia_cong_id` | 8 | 0 | 1 | THẤP (9) | **NÊN** |
| 5 | N-A | `phieu_dieu_in.loai_giay_gc_ref_id` | `gc` + `ref` — hai lớp viết tắt chồng nhau | `loai_giay_gia_cong_id` | 8 | 0 | 0 | THẤP (8) | **NÊN** |
| 6 | N-A+D | `lenh_san_xuat.kho_giay_gc_dai_mm` | `gc` viết tắt **và** `kho` ở đây là **"khổ"** (kích thước), không phải "kho hàng" | `kho_giay_gia_cong_dai_mm` | 9 | 0 | 1 | T.BÌNH (10) | **NÊN** |
| 7 | N-A+D | `lenh_san_xuat.kho_giay_gc_rong_mm` | như trên | `kho_giay_gia_cong_rong_mm` | 9 | 0 | 1 | T.BÌNH (10) | **NÊN** |
| 8 | N-A | `pricing_quote_history.kich_thuoc_l/_w/_h` | ký tự đơn `l/w/h`, không rõ đơn vị | `kich_thuoc_dai_mm` / `_rong_mm` / `_cao_mm` | 12 | 8 | 0 | T.BÌNH (20) | **NÊN** |
| 9 | N-A | `pricing_quote_history.nhom_sp_id` | `sp` = sản phẩm; bảng khác viết đủ `nhom_san_pham_id` | `nhom_san_pham_id` | 14 | 7 | 2 | T.BÌNH (23) | **NÊN** |
| 10 | N-A | `bien_ban_nghiem_thu.danh_sach_so_phieu_gh` | `gh` = giao hàng | `danh_sach_so_phieu_giao_hang` | 9 | 1 | 2 | T.BÌNH (12) | **NÊN** |
| 11 | N-A | `cong_no_doi_chieu.tong_no_31_60d` | `d` = ngày; `31_60` dính liền số | `tong_no_qua_han_31_60_ngay` | 12 | 1 | 0 | T.BÌNH (13) | **NÊN** |
| 12 | N-A | `lenh_san_xuat.trang_thai_sx` | `sx` viết tắt trong khi tên bảng đã có `san_xuat` đầy đủ | `trang_thai_san_xuat` | 18 | 0 | 6 | T.BÌNH (24) | **NÊN** |
| 13 | N-A | `mua_hang.lien_ket_lenh_sx` · `phieu_nhap.lien_ket_lenh_sx` | `sx` + `lien_ket` mơ hồ (thực chất chứa mã LSX) | `ma_lenh_san_xuat` | 20 | 3 | 15 | T.BÌNH (38) | **NÊN** |
| 14 | N-A | `kho_thanh_pham.lien_ket_lsx` | `lsx` + `lien_ket` (chứa `ma_lsx`, không phải id) | `ma_lenh_san_xuat` | 22 | 5 | 13 | T.BÌNH (40) | **NÊN** |
| 15 | N-A | `lsx_id` (4 bảng) | `lsx` viết tắt | `lenh_san_xuat_id` | 43 | 3 | 4 | T.BÌNH (50) | **NÊN** |
| 16 | N-A | `lenh_san_xuat.ma_lsx` | `lsx` viết tắt | `ma_lenh_san_xuat` | 45 | 5 | 6 | CAO (56) | CHỜ OWNER |
| 17 | N-A | `dm_vat_tu.uom_tc_id` | `uom` (EN) + `tc` — hai viết tắt; **có FK** → `dm_uom.id` | `don_vi_tinh_tham_chieu_id` | 31 | 8 | 17 | CAO (56) | CHỜ OWNER |
| 18 | N-A | `kh_lien_he.chuc_vu_khg_id` | `khg` phi chuẩn (chỗ khác dùng `kh`) | `chuc_vu_khach_hang_id` | 35 | 3 | 13 | CAO (51) | CHỜ OWNER |
| 19 | N-A | `dvt` (3 bảng) | `dvt`; **8 bảng khác đã dùng `don_vi_tinh` đầy đủ** ⇒ phá nhất quán | `don_vi_tinh` | 51 | 10 | 9 | CAO (70) | **NÊN** |
| 20 | N-A | `dm_phong_ban.viet_tat` | 3 bảng khác dùng `ten_viet_tat` | `ten_viet_tat` | 35 | 39 | 4 | CAO (78) | **NÊN** |
| 21 | N-A | `so_po` · `lien_ket_po` (4 bảng) | `po` = purchase order (EN) trộn vào hệ VI | `so_don_mua_hang` | 96 | 24 | 73 | CAO (193) | CHỜ OWNER |
| 22 | N-A | `id_task_m8` (3 bảng) | `m8` là mã module; cột tên `id_*` nhưng trỏ `task.ma_task` (chuỗi) | `ma_task` | 38 | 123 | 17 | CAO (178) | CHỜ OWNER |
| 23 | N-A | `ma_nv` (8 bảng HR) | `nv` viết tắt; **là đích của 6 FK** | `ma_nhan_vien` | 176 | 85 | 182 | RẤT CAO (443) | **KHÔNG NÊN** |
| 24 | N-A | `ma_ncc`/`ten_ncc`/`loai_ncc` (5 bảng) | `ncc` viết tắt; **đích của 3 FK** | `ma_nha_cung_cap` | 165 | 25 | 73 | RẤT CAO (263) | **KHÔNG NÊN** |
| 25 | N-B | `material_price` *(bảng)* | tên EN, không gắn module | `dm_vat_tu_gia` | 15 | 7 | 23 | T.BÌNH (45) | **NÊN** |
| 26 | N-B | `material_supplier` *(bảng)* | tên EN, trùng ngữ nghĩa `dm_nha_cung_cap` | `dm_vat_tu_nha_cung_cap` | 14 | 7 | 15 | T.BÌNH (36) | **NÊN** |
| 27 | N-B | `form_file` *(bảng)* | quá rộng; thực chất là tệp đính kèm của `form_phat_hanh` (có FK) | `form_phat_hanh_file` | 15 | 0 | 28 | T.BÌNH (43) | **NÊN** |
| 28 | N-B+C | `lsx_source_items` *(bảng)* | trộn `lsx` (VI tắt) + `source_items` (EN) | `lenh_san_xuat_nguon` | 17 | 0 | 21 | T.BÌNH (38) | **NÊN** |
| 29 | N-B | `task` *(bảng)* | danh từ EN chung nhất; đụng từ khoá SQL/JS khắp nơi | `m8_cong_viec` | 17\* | 155\* | 12\* | CAO (184\*) | CHỜ OWNER |
| 30 | N-B | `system_setting` *(bảng)* | rộng, nhưng là quy ước ngành phổ biến | *(giữ)* | 2 | 14 | 64 | CAO (80) | **KHÔNG NÊN** |
| 31 | N-B | `kho_giao_dich.item_id`/`ten_item`/`item_type` | `item` chung chung + EN | `doi_tuong_kho_id`… | 118 | 20 | 63 | RẤT CAO (201) | **KHÔNG NÊN** |
| 32 | N-B | `material_item` *(bảng)* | **trùng ngữ nghĩa với `dm_vat_tu`** (cả hai có `ma_vat_tu`, `ma_nhom_vat_tu`); còn có FK `material_item.dm_vat_tu_id → dm_vat_tu.id` | gộp về `dm_vat_tu` hoặc `dm_vat_tu_mo_rong` | 26 | 132 | 306 | RẤT CAO (464) | CHỜ OWNER |
| 33 | N-B | `hr_employee_nhanvien` *(bảng)* | **lặp nghĩa hai ngôn ngữ** (employee = nhân viên); anh em cùng nhóm thuần VI | `hr_nhan_vien` | 36 | 116 | 242 | RẤT CAO (394) | **KHÔNG NÊN** |
| 34 | N-B | `dm_nhom_universal` *(bảng)* | `universal` EN + quá rộng: một bảng gánh ~10 loại nhóm qua `danh_muc_nhom` | `dm_nhom_dung_chung` | 109 | 325 | 408 | RẤT CAO (842) | **KHÔNG NÊN** |
| 35 | N-B | `doi_tuong_id` + `lien_ket_id` (6 bảng) | khoá đa hình không kèm cột loại rõ ràng | `doi_tuong_lien_quan_id` + `loai_doi_tuong` bắt buộc | 164 | 89 | 112 | RẤT CAO (365) | **KHÔNG NÊN** |
| 36 | N-C | `dm_nhom_universal.created_at/updated_at/created_by/updated_by` | **83 bảng khác dùng `ngay_tao`/`nguoi_tao`** | `ngay_tao`/`ngay_sua`/`nguoi_tao`/`nguoi_sua` | 73† | 32† | 40† | CAO (145†) | **NÊN** |
| 37 | N-C | `phieu_dieu_in.created_at/updated_at/created_by/updated_by` | như trên | như trên | 73† | 32† | 40† | CAO (145†) | **NÊN** |
| 38 | N-C | `lsx_source_items.created_at` | bảng chỉ có 1 cột audit và lại là EN | `ngay_tao` | 29† | 15† | 10† | CAO (54†) | **NÊN** |
| 39 | N-C | `system_code_counter.*` | toàn bảng thuần EN giữa hệ VI | `ngay_tao`/`ngay_sua`/`khoa_dem`/`gia_tri_cuoi` | 6 | 59 | 17 | CAO (82) | CHỜ OWNER |
| 40 | N-C+D | `chung_tu_ke_toan` · `cong_no` · `phieu_thu` · `phieu_chi`: có **cả** `nguoi_tao int` **lẫn** `nguoi_tao_email varchar` | hai cột cùng nghĩa, hai kiểu — không rõ cột nào là nguồn sự thật | bỏ `*_email`, giữ FK `nguoi_tao → user_account.id` | 47 | 47 | 66 | CAO (160) | CHỜ OWNER |
| 41 | N-C | `dm_param_registry`: `description`/`label`/`status`/`type`/`unit` cạnh `nguoi_tao`/`ngay_tao` | trộn ngôn ngữ trong cùng bảng | `mo_ta`/`nhan`/`trang_thai`/`loai`/`don_vi` | 8 | 52 | 13 | CAO (73) | CHỜ OWNER |
| 42 | N-C | `dm_pricing_test_case`: `name`/`enabled`/`input_json`… | trộn ngôn ngữ | thuần VI hoặc thuần EN (chọn 1) | 9 | 77 | 13 | CAO (99) | CHỜ OWNER |
| 43 | N-C | `dm_profit_margin_rule`: `active`/`priority` | trộn ngôn ngữ | `dang_hoat_dong`/`do_uu_tien` | 1 | 56 | 12 | CAO (69) | CHỜ OWNER |
| 44 | N-C | `dm_dia_chi_vn`: `parent_id`/`path`/`level` | trộn; `dm_nhom_universal` cũng có bộ `path`/`level` EN cạnh `nhom_cha_id` VI | `dia_chi_cha_id`/`duong_dan`/`cap` | 29 | 13 | 169 | RẤT CAO (211) | **KHÔNG NÊN** |
| 45 | N-C | `dm_uom_conversion.from_uom_id`/`to_uom_id` | `from/to` EN + `uom` EN cạnh `nhom_don_vi_tinh_id` VI; **có FK** | `uom_nguon_id`/`uom_dich_id` | 46 | 14 | 38 | CAO (98) | CHỜ OWNER |
| 46 | N-C | tiền tố `is_` trên thân VI (6 cột) | nơi khác dùng `la_` (`la_ncc_uu_tien`, `la_gia_hien_hanh`) | `la_*` | 285\* | 108\* | 111\* | RẤT CAO (504\*) | **KHÔNG NÊN** |
| 47 | N-C | `hr_overtime` *(bảng)* | `overtime` EN giữa `hr_nghi_phep`, `hr_cham_cong_raw`, `hr_ca_lam_viec` | `hr_tang_ca` | 9 | 14 | 22 | T.BÌNH (45) | **NÊN** |
| 48 | N-C | `hr_cham_cong_raw` / `hr_cham_cong_tinh` | `raw` EN vs `tinh` VI — hai hậu tố khác hệ trên cặp bảng đối xứng | `hr_cham_cong_tho` / `hr_cham_cong_da_tinh` | 19 | 40 | 62 | CAO (121) | CHỜ OWNER |
| **49** | **N-D** | **58 cột ngày kiểu `text`** — `ngay_tao` (32 bảng), `ngay_sua` (26 bảng), `bao_gia.ngay_hieu_luc`, `cong_no.han_thanh_toan`, `don_hang.ngay_giao_du_kien/thuc_te`, `hr_employee_nhanvien.ngay_sinh` + 9 cột ngày HR, `phieu_thu.ngay_thu`, `phieu_chi.ngay_chi`, `dong_tien.ngay_giao_dich`… | tên nói "ngày" nhưng kiểu `text` ⇒ **không so sánh / sắp xếp / index đúng được**; dữ liệu thật lưu chuỗi `"2026-01-27 13:25:49"` | **giữ nguyên tên, đổi KIỂU** → `datetime`/`date` | 913 | 579 | 439 | RẤT CAO (1.931) | **NÊN** (đổi kiểu, KHÔNG đổi tên) |
| **50** | **N-D** | **`nguoi_tao`/`nguoi_sua` mang HAI nghĩa**: 38 bảng `int(11)` (FK `user_account.id`) vs 45 bảng `varchar(100)`/`text` chứa nhãn tự do — giá trị thật đo được: `"system"`, `"pl4-phase1a"`, `"rbac-20260801"`, `"a2p1-20260731"` | cùng tên cột, hai kiểu, hai ngữ nghĩa ⇒ **không join thống nhất được, không audit được ai làm gì** | tách `nguoi_tao_id int` (FK) vs `nguon_tao varchar` (script/hệ thống) | 1.011 | 607 | 846 | RẤT CAO (2.464) | CHỜ OWNER |
| 51 | N-D | `kh_dia_chi.nguoi_phu_trach_ids` : `text` | tên số nhiều chứa danh sách trong 1 ô ⇒ vi phạm 1NF, không FK được (**hiện NULL cả 3 dòng**) | bảng nối `kh_dia_chi_nguoi_phu_trach` | 22 | 0 | 3 | T.BÌNH (25) | **NÊN** |
| 52 | N-D | `pricing_quote_history.gia_cong_ids` : `text` | danh sách trong 1 ô + nhập nhằng: "gia công" hay "giá công"? | `danh_sach_cong_doan_id` + bảng nối | 9 | 2 | 1 | T.BÌNH (12) | **NÊN** |
| 53 | N-D | `pricing_quote_history.nguyen_lieu_ids` : `text` | danh sách trong 1 ô | `danh_sach_vat_tu_id` + bảng nối | 9 | 2 | 1 | T.BÌNH (12) | **NÊN** |
| **54** | **N-D** | `don_hang.da_thanh_toan` `decimal(15,2)` · `bien_ban_nghiem_thu.da_thanh_toan` `decimal(18,2)` | **tên đọc như cờ boolean ("đã thanh toán?") nhưng chứa SỐ TIỀN** | `tien_da_thanh_toan` | 26 | 7 | 20 | CAO (53) | **NÊN** |
| 55 | N-D | `audit_log.nguoi_thuc_hien` `varchar(100)` **FK → `user_account.email`** | tên `nguoi_*` nhưng nội dung là email; cùng tên này ở bảng khác là `int` (`task`, `task_log`) hoặc `varchar(20)` (`kho_giao_dich`) — **3 kiểu cho 1 tên** | `email_nguoi_thuc_hien` | 58 | 14 | 60 | CAO (132) | CHỜ OWNER |
| 56 | N-D | `hop_dong.nguoi_dai_dien_ky` `varchar(20)` **FK → `hr_employee_nhanvien.ma_nv`** | tên gợi "tên người", thực chất là mã nhân viên | `ma_nv_dai_dien_ky` | 14 | 2 | 7 | T.BÌNH (23) | **NÊN** |
| 57 | N-D | `hr_bang_luong.nguoi_duyet`/`nguoi_tinh_luong`/`nguoi_chi_luong` · `hr_luong_phu_cap.nguoi_duyet` `varchar(100)` **FK → email** | tên `nguoi_*` nhưng chứa email | `email_nguoi_duyet`… | 62 | 18 | 102 | CAO (182) | CHỜ OWNER |
| 58 | N-D | `dm_vat_tu.gia_tham_chieu_updated_at` | thân VI + hậu tố `updated_at` EN, cạnh 2 cột thuần VI | `gia_tham_chieu_ngay_cap_nhat` | 8 | 1 | 12 | T.BÌNH (21) | **NÊN** |
| 59 | N-D | `material_item.kho_dai_mm_default`/`kho_rong_mm_default` | `kho` = "khổ" nhưng đọc thành "kho hàng"; thêm hậu tố `default` EN | `kho_giay_dai_mm_mac_dinh` | 20 | 3 | 6 | T.BÌNH (29) | **NÊN** |
| 60 | N-D | `material_item.kho_giay` `varchar(20)` | `kho` = khổ giấy, trùng mặt chữ với "kho" (warehouse) ở `kho_thanh_pham`, `kho_giao_dich`, `phieu_xuat_kho` | `khuon_kho_giay` / `kich_thuoc_giay` | 24 | 10 | 16 | T.BÌNH (50) | CHỜ OWNER |
| 61 | N-D | `phieu_xuat_kho.ma_kho` `varchar(50)` | tên `ma_*` gợi FK nhưng **không có bảng `dm_kho`**; comment của chính cột ghi "dm_kho chưa tồn tại" **và bị hỏng mã tiếng Việt** | `ten_kho_tu_do` cho tới khi có `dm_kho` | 17 | 3 | 19 | T.BÌNH (39) | CHỜ OWNER |
| 62 | N-D | `hr_cham_cong_tinh.don_xin_phep_id` FK → `hr_nghi_phep.id` | tên là `don_xin_phep` nhưng bảng đích tên `hr_nghi_phep` — **hai tên cho cùng một thực thể** | `nghi_phep_id` | 14 | 12 | 6 | T.BÌNH (32) | **NÊN** |
| 63 | N-D | `trang_thai` kiểu `text` ở **14 bảng** | trong khi 20 bảng khác dùng `enum(...)` cho cùng khái niệm ⇒ **không có ràng buộc giá trị** | giữ tên, đổi kiểu `enum`/`varchar(30)`+CHECK | — | — | — | — | CHỜ OWNER |
| 64 | N-D | 6 cột boolean khai `int(11)` thay vì `tinyint(1)` | driver trả `number` chứ không `boolean` | giữ tên, đổi kiểu `tinyint(1)` | — | — | — | — | **NÊN** (đổi kiểu) |
| 65 | N-E | `id_khach_hang` (8 bảng) vs `nhom_khach_hang_id` | **49 cột dùng tiền tố `id_*`, 90 cột dùng hậu tố `*_id`**; `phieu_thu` và `phieu_chi` dùng **cả hai trong cùng một bảng** | thống nhất `*_id` | 175 | 159 | 75 | RẤT CAO (409) | **KHÔNG NÊN** |

> \* **Cảnh báo về con số:** `task`, `is_active`, `is_default`, `is_selected`, `item_id` là từ khoá chung của TS/React. Với `task` đã đếm riêng dạng định danh SQL (`FROM|JOIN|INTO|UPDATE|TABLE task`) = 184; đếm thô của từ `task` là 986. Nhóm `is_*` con số 504 là **tổng thô, bao gồm cả biến UI không liên quan bảng**.
> † `created_at`/`updated_at`/`created_by`/`updated_by` chỉ do 6 bảng dùng, nhưng grep là toàn kho nên 145 là **tổng gộp 4 từ khoá**, không tách được theo bảng bằng grep thuần.

### 5.2 Lỗi charset trong `COLUMN_COMMENT` (ghi riêng — không phải lỗi đặt tên)

**21/194 comment có tiếng Việt bị hỏng mã**, chia 2 loại khác nhau về nguyên nhân:

- **Loại 1 — mất dấu thành `?` (12 cột), KHÔNG phục hồi được** (đã ghi ở kết nối latin1):
  `dm_nha_cung_cap.thoi_gian_giao_hang_trung_binh` · `dm_vat_tu.mau_sac` · `kho_giao_dich.so_luong` · `lenh_san_xuat_item.lsx_id` · `lenh_san_xuat_item.vat_tu_id` · `material_price.ngay_ket_thuc` · `material_supplier.thoi_gian_giao_hang` · `phieu_dieu_in.may_in` · `phieu_dieu_in.loai_kem` · `phieu_dieu_in.nha_cung_cap_kem_id` · `phieu_dieu_in.nha_in_offset_id` · **`phieu_xuat_kho.ma_kho`**
- **Loại 2 — UTF-8 bị mã hoá hai lần (9 cột), CÒN phục hồi được** bằng chuyển mã:
  `dm_vat_tu.gia_tham_chieu_tinh` · `gia_tham_chieu_dong_30d` · `gia_tham_chieu_updated_at` · `material_item.uom_mua_mac_dinh_id` · `kho_dai_mm_default` · `kho_rong_mm_default` · `phieu_dieu_in.ma_phieu` · `so_mat` · `so_mau`

> **Nguyên nhân gốc khiến tên viết tắt không giải được:** chỉ **194/1.526 cột (12,7%)** có comment — **87% cột không có mô tả nào**.

### 5.3 Quy luật đặt tên hiện tại

**Quy ước đang tồn tại (đo được):**

1. **snake_case, tiếng Việt không dấu** — chuẩn chủ đạo, ~85% cột.
2. **Tiền tố bảng theo nhóm chức năng** (KHÔNG phải theo mã module M0–MF):

| Tiền tố | Số bảng | Nghĩa | Ví dụ |
|---|---|---|---|
| `dm_` | 23 | danh mục (master data) | `dm_vat_tu`, `dm_khach_hang`, `dm_uom` |
| `hr_` | 12 | nhân sự M6/M7 | `hr_bang_luong`, `hr_nghi_phep` |
| `phieu_` | 9 | chứng từ kho/giao hàng | `phieu_nhap`, `phieu_xuat_kho` |
| `task_` | 4 (+`task`) | M8 | `task_checklist`, `task_log` |
| `role_` | 4 | RBAC phân quyền | `role_action_permission` |
| `user_` | 3 | RBAC tài khoản | `user_account`, `user_session` |
| `material_` | 3 | **tiếng Anh** — vật tư | `material_item`, `material_price` |
| `kh_` | 2 | bảng con của khách hàng | `kh_dia_chi`, `kh_lien_he` |
| `system_` | 2 | cấu hình hệ thống | `system_setting` |
| `lsx_` | 1 | lệnh sản xuất | `lsx_source_items` |

3. **Hậu tố `_item`** cho bảng chi tiết chứng từ — nhất quán 8 bảng, nhưng là từ EN.
4. **Cột audit chuẩn:** `ngay_tao` / `nguoi_tao` / `ngay_sua` / `nguoi_sua`.
5. **Khoá tự nhiên** kiểu `ma_*` — nhiều FK trỏ vào `ma_*` chứ không vào `id`.

**Chín chỗ phá vỡ quy luật:**

| # | Chỗ phá | Mô tả |
|---|---|---|
| P1 | **28 bảng không có tiền tố nào** | `bao_gia`, `don_hang`, `hop_dong`, `mua_hang`, `cong_no`, `lenh_san_xuat`, `kho_thanh_pham`, `kho_giao_dich`, `kiem_ke_kho`, `dong_tien`, `quy_tien_mat`, `giao_dich_ngan_hang`, `tai_khoan_ngan_hang`, `thiet_ke_yeu_cau`, `cskh_nhat_ky`, `bien_ban_nghiem_thu`, `chung_tu_ke_toan`, `cong_no_doi_chieu`, `mau_hop_dong`, `form_file`, `form_phat_hanh`, `audit_log`, `auth_audit_log`, `permission_log`, `schema_migrations`, `pricing_quote_history`, `checklist_template`, `notification_queue`, `lich_su_cong_no`, `task` |
| P2 | **Tiền tố tiếng Anh xen kẽ** | `material_*` (3 bảng) song song với `dm_vat_tu` — **hai họ tên cho cùng khái niệm vật tư** |
| P3 | **Bảng lai hai ngôn ngữ ngay trong tên** | `hr_employee_nhanvien` (lặp nghĩa) · `lsx_source_items` · `hr_overtime` · `hr_cham_cong_raw` · `pricing_quote_history` · `checklist_template` |
| P4 | **Hai kiểu đặt tên FK song song** | 49 cột `id_*` (chủ yếu M3/MF) vs 90 cột `*_id` (chủ yếu M0/M1/M4/M5). `phieu_thu` và `phieu_chi` dùng **cả hai cùng lúc** |
| P5 | **Cột audit hai hệ ngôn ngữ** | 83 bảng dùng `nguoi_tao`/`ngay_tao`; 6 bảng dùng `created_by`/`created_at` |
| P6 | **Cùng tên cột, hai kiểu dữ liệu** | `nguoi_tao`: 38 bảng `int` vs 45 bảng `varchar/text`. `ngay_tao`: 53 bảng `datetime` vs 32 bảng `text` vs 2 `timestamp` |
| P7 | **Kiểu khoá chính không nhất quán** | 20 bảng dùng `id varchar(50)`; phần còn lại `id int(11) AUTO_INCREMENT` |
| P8 | **Tiền tố boolean lẫn lộn** | `is_*` (6 cột) vs `la_*` (2 cột) vs `co_*` / `da_*` |
| P9 | **Không tồn tại tiền tố mã module** | Đề bài nêu `m3_`/`sx_` làm ví dụ — thực tế **không có** tiền tố mã module nào trong DB; module chỉ tồn tại ở tầng thư mục `src/app/m0..mf` |

### 5.4 TOP 10 ƯU TIÊN (xếp theo tỉ lệ lợi / chi phí)

| Hạng | Tên | Chi phí | Vì sao đứng đầu |
|---|---|---|---|
| 1 | `dvt` → `don_vi_tinh` (3 bảng) | CAO (70) | Chỉ 3 bảng lạc chuẩn giữa 8 bảng đã đúng; sửa xong **hết một loại viết tắt trong toàn hệ** |
| 2 | `sl_*` → `so_luong_*` (3 cột) + `*_gc_*` → `*_gia_cong_*` (4 cột) | THẤP (8–10/cột) | **Rẻ nhất bảng này**; không cột nào có FK, 0 điểm trong migrations |
| 3 | `dm_phong_ban.viet_tat` → `ten_viet_tat` | CAO (78) | Đưa 1 cột lạc về đúng chuẩn 3 bảng khác; không FK |
| 4 | `da_thanh_toan` → `tien_da_thanh_toan` (2 bảng) | CAO (53) | **Bẫy nghiệp vụ tiền bạc**: tên boolean chứa số tiền — sai một lần là sai công nợ |
| 5 | 58 cột `ngay_*` kiểu `text` → `datetime`/`date` | RẤT CAO (1.931) nhưng **đổi kiểu, giữ tên** | Không đổi tên ⇒ **không phải sửa mã gọi tên cột**; chỉ cần migration + kiểm dữ liệu. Lợi rất lớn: hết lỗi sắp xếp/lọc theo ngày |
| 6 | `created_*`/`updated_*` (6 bảng) → `ngay_tao`/`nguoi_tao`… | CAO (145) | Xoá hẳn nhóm N-C ở tầng cột audit; 6 bảng là hữu hạn, rà hết được |
| 7 | `hr_overtime` → `hr_tang_ca` | T.BÌNH (45) | Bảng **0 dòng dữ liệu**, không FK trỏ tới — đổi lúc này rẻ nhất |
| 8 | `material_price` → `dm_vat_tu_gia` · `material_supplier` → `dm_vat_tu_nha_cung_cap` | T.BÌNH (45 + 36) | Cả hai đang **0 dòng**; đưa họ `material_*` về chuẩn `dm_*` |
| 9 | `kh_dia_chi.nguoi_phu_trach_ids` → bảng nối | T.BÌNH (25) | Cột đang **toàn NULL (3/3 dòng)** ⇒ đổi bây giờ **không mất dữ liệu nào** |
| 10 | `lsx_id` → `lenh_san_xuat_id` (4 bảng) + `lien_ket_lsx` → `ma_lenh_san_xuat` | T.BÌNH (50 + 40) | Xoá viết tắt `lsx` khỏi tầng cột; **giữ `ma_lsx`** (mục 16) vì là mã hiển thị cho người dùng |

### 5.5 CẢNH BÁO — tuyệt đối không nên đổi

| Tên | Rủi ro |
|---|---|
| **`ma_nv`** | Là **đích của 6 khoá ngoại** (`hr_bang_luong_item`, `hr_cham_cong_raw`, `hr_cham_cong_tinh`, `hr_luong_phu_cap`, `hr_nghi_phep`, `hr_overtime`, `hop_dong.nguoi_dai_dien_ky`). **443 điểm**. Đổi = rebuild toàn M6+M7 |
| **`ma_ncc`** | Đích của 3 FK. 263 điểm, trải M1+M5 |
| **`ma_khach_hang`** | **610 điểm**; đích FK của `kh_dia_chi`, `kh_lien_he`. Trải M1+M3+MF |
| **`dm_nhom_universal`** | **842 điểm — 12 khoá ngoại từ 10 bảng khác** trỏ vào. Bảng xương sống toàn hệ |
| **`hr_employee_nhanvien`** | 394 điểm; tên xấu (lặp nghĩa) nhưng là đích của 6 FK + 3 FK đi ra |
| **`id_khach_hang` / chuẩn hoá `id_*` vs `*_id`** | Đụng 139 cột, 409+ điểm, trải cả 8 module. Đây là **refactor kiến trúc, không phải đổi tên** |
| **`ma_vai_tro`** | 290 điểm; đích của 4 FK RBAC. Đổi = rủi ro fail-closed toàn hệ phân quyền |
| **`so_phieu`** | 271 điểm; **khoá tự nhiên** của 3 cặp bảng cha-con |
| **`nguoi_tao` / `nguoi_sua`** | **1.358 + 1.106 điểm.** Đây là **vấn đề ngữ nghĩa nghiêm trọng nhất** (hai kiểu, hai nghĩa) nhưng cũng là chi phí lớn nhất toàn DB. Phải có quyết định Owner + **kế hoạch riêng**, không gộp vào bất kỳ gói việc nào khác |
| **`item_id`/`ten_item`/`item_type`** của `kho_giao_dich` | 201 điểm; khoá đa hình — đổi tên mà không đổi mô hình thì tốn công, không được lợi |
| **Mọi cột có FK** (108 khoá) | Đổi tên cột có FK cần DROP + ADD constraint; chạy trên máy vận hành phải khoá bảng. Các cột đáng đổi mà **có FK**: `uom_tc_id`, `from_uom_id`, `to_uom_id`, `lsx_id`, `don_xin_phep_id`, `nguoi_dai_dien_ky` |

### 5.6 Không xác minh được (N5)

1. **Con số grep cho từ khoá chung không sạch** — `task` (986 thô), `is_active` (252), `is_default` (61), `is_selected` (120), `item_id` (75), `created_at` (54), `updated_at` (34), và `path`/`level`/`status`/`type`/`name`: các chuỗi này xuất hiện trong mã UI/React/config không liên quan cột DB. Đã đếm riêng được `task` theo dạng định danh SQL (184), **nhưng không tách được** cho nhóm `is_*` và `created_*`. *Cần AST analysis — ngoài phạm vi phiên đọc-only.*
2. **`created_by`/`updated_by`/`created_at`/`updated_at` không tách được theo bảng** — 6 bảng dùng, nhưng grep toàn kho nên 145 là tổng gộp.
3. **Ý nghĩa nghiệp vụ của 4 tên cần Owner xác nhận:**
   - `dm_vat_tu.uom_tc_id`: `tc` là **"tham chiếu"** (suy từ comment `??VT TC`) hay **"tiêu chuẩn"**?
   - `pricing_quote_history.gia_cong_ids`: **"gia công"** (công đoạn) hay **"giá công"**?
   - `phieu_dieu_in.loai_kem`/`so_kem`/`kem_moi`: "kẽm" (bản in offset)? — chỉ suy được từ comment **đã hỏng mã**
   - `material_item.kho_giay`/`kho_dai_mm_default`: **"khổ"** hay **"kho"**? Suy từ đơn vị `mm` là "khổ", nhưng comment gốc hỏng mã
4. **Quan hệ thật giữa `material_item` và `dm_vat_tu`** — `material_item.dm_vat_tu_id` FK trỏ sang `dm_vat_tu`, nhưng **cả hai đều có** `ma_vat_tu`, `ma_nhom_vat_tu`. `material_item` hiện **0 dòng**, `dm_vat_tu` có **19 dòng**. Không rõ là bảng mở rộng có chủ đích hay **tàn dư di trú**. *Ai xác minh: Owner, hoặc đọc lịch sử migration M1.*
5. **Không đọc dữ liệu PII** — theo `GOV-PII-HANDLING-001`, chỉ đọc mẫu 3–5 dòng của cột kỹ thuật; **không đọc** `email`, `so_cmnd_cccd`, `sdt_*`, `dia_chi_*`. Nhận định về cột PII chỉ dựa trên tên + kiểu.
6. **Chi phí chưa tính `.governance/`, 5 file luật gốc, Notion** — chỉ đếm 3 vùng được yêu cầu.
7. **63/99 bảng có `TABLE_ROWS = 0`** — nhưng đó là **ước lượng của InnoDB**, không phải `COUNT(*)` chính xác; không chạy `COUNT(*)` trên 99 bảng để tránh tải.

---

## 6. N6 — PHÁT HIỆN NGOÀI PHẠM VI

> Ghi riêng theo yêu cầu, **không trộn** vào N1–N5. Tất cả là quan sát phát sinh khi đo, **không nằm trong 5 nhiệm vụ được giao**.

### 🔴 NP-1 — Ma trận phân quyền CHƯA được nạp lên máy vận hành

| Bảng phân quyền | PRODUCTION | DEV |
|---|---|---|
| `role_action_permission` | **3** dòng | **26** dòng |
| `role_menu_permission` | **24** dòng | **48** dòng |
| `role_data_permission` | **0** dòng | **3** dòng |
| `role_field_permission` | 0 | 0 |

Ba dòng duy nhất trên production đều là hành động HR: `HR:HR_CREATE_USER_ACCOUNT`, `HR:HR_LINK_USER_ACCOUNT`, `HR:HR_UNLINK_USER_ACCOUNT`.

**Toàn bộ quyền nghiệp vụ KHÔNG tồn tại trên production:** `can_view_all_customers` · `can_transfer_customer` · `can_view_cost_price` · `can_view_customer_debt` · `can_view_supplier_debt` · `can_manage_customer_debt` · `can_manage_supplier_debt` · `can_manage_payment` · `can_manage_pricing_settings` · `can_use_manual_pricing` — cho cả 4 vai trò `ADMIN`/`CEO`/`KE_TOAN`/`SALES`.

Seed tương ứng `migrations/20260731_a2p1_permission_matrix_seed.sql` **chưa chạy trên máy vận hành**.

### 🔴 NP-2 — 0/30 nhân viên trên production được liên kết tài khoản

| Chỉ số | PRODUCTION | DEV |
|---|---|---|
| `hr_employee_nhanvien` | 30 dòng, **100% `trang_thai = chinh_thuc`** | 30 dòng |
| Có `user_id` (liên kết tài khoản) | **0 / 30** | 4 / 30 |
| `user_account` | **1** tài khoản | 7 tài khoản |
| Vai trò đã gán | `ADMIN` ×1, `CEO` ×1 (cùng một tài khoản) | 6 vai trò, 10 lượt gán |

**Hệ quả suy ra từ code** (`action-permission.ts:▮`): `getCurrentEmployeeId()` join `hr_employee_nhanvien.user_id → user_account.id`. Với `user_id` NULL toàn bộ, hàm trả **`null` cho mọi tài khoản**. Theo `customer-scope.ts:▮`, tài khoản **không phải admin** fail-closed về `{canViewAll:false, empId:null}` ⇒ `filterOwnedCustomers` trả **mảng rỗng** ⇒ **thấy 0 khách hàng**.

Hệ thống vận hành được **chỉ vì** tài khoản duy nhất mang vai trò `ADMIN` (`dm_vai_tro.la_admin = 1`) nên đi đường bypass. **Tài khoản thứ hai bất kỳ, nếu không phải admin, sẽ không thấy dữ liệu nào.**

> Đây là quan sát về **cấu hình dữ liệu vận hành**, không phải lỗi code — cơ chế fail-closed đang chạy **đúng thiết kế**. Nhưng nghĩa là công đoạn go-live "liên kết 30 nhân viên ↔ tài khoản" **chưa được thực hiện**.

### 🟠 NP-3 — Migration treo trên production 20 ngày

`migrations/20260731_a2p2_mua_hang_draft_nullable.sql` (hợp đồng Owner A2-P2, duyệt 31/07/2026) **chỉ chạy trên local** — chính file tự khai `LOCAL ONLY … CHUA duoc chay tren VPS`. Đây là toàn bộ nguyên nhân 4 điểm lệch schema ở §1.3. Hai bảng liên quan đang 0 dòng trên production nên **chưa hỏng dữ liệu**, nhưng tính năng chứng từ mua hàng `draft` sẽ **lỗi ghi** nếu dùng trên máy vận hành.

### 🟠 NP-4 — `audit_log` trên production đang rỗng

`audit_log = 0 dòng`. Đây là cột **duy nhất có FK thật tới `user_account(email)`** trong nhóm nhật ký. Chưa có bản ghi kiểm toán nào được sinh trên máy vận hành.

### 🟠 NP-5 — Tham số `nguoiThucHien` chết ở 2/3 nhánh ghi sổ kho

Phát hiện khi làm N2 nhưng **vượt ra ngoài X-2** (X-2 hỏi về `nguoi_lap`, đây là về actor ghi **sổ kho**): `updatePhieuNhap` và `updatePhieuXuatKho` **khai** tham số thứ 3 `nguoiThucHien` nhưng **không caller nào truyền** (`phieu-nhap/actions.ts:▮`, `phieu-xuat-kho/actions.ts:▮`) ⇒ mọi bút toán `kho_giao_dich` từ 2 nhánh này ghi actor = `"system"`. Chỉ nhánh giao hàng truyền thật.

### 🟠 NP-6 — `updateBaoGiaAction` là bề mặt mass-assignment

`src/app/m3/bao-gia/actions.ts:▮` nhận `Partial<BaoGia>` **nguyên khối, không allow-list trường**, rồi `m3-store.ts:▮` build `UPDATE` động từ đó. Không client nào trong `src/` gọi hàm này, **nhưng nó là Next.js Server Action** (`"use server"` ở dòng 1) ⇒ vẫn là endpoint POST gọi được từ ngoài. Client tự chế có thể đặt `id_nhan_vien_phu_trach`, `nguoi_tao`, `tong_tien`, `trang_thai`.

### 🟠 NP-7 — Báo giá và đơn hàng không lọc phạm vi

`m3/bao-gia/page.tsx:▮` gọi `listBaoGia()` = `SELECT * FROM bao_gia ORDER BY id DESC`; `m3/don-hang/page.tsx:▮` gọi `listDonHang()` tương tự — **không WHERE, không scope**. Mọi actor có quyền `m3:view` nhận **toàn bộ** báo giá và đơn hàng của mọi Sale xuống client. (Danh sách khách hàng/liên hệ/sản phẩm **có** lọc; riêng hai danh sách chính thì không.)

### 🟠 NP-8 — `nguoi_tao` đang chứa **tên script**, không phải tên người

Phát hiện khi làm N5. Trong 45 bảng có `nguoi_tao` kiểu `varchar`, giá trị thật đo được gồm: `"system"`, **`"pl4-phase1a"`**, **`"rbac-20260801"`**, **`"a2p1-20260731"`** — tức **mã đợt migration/seed**, không phải người. Cùng lúc, 38 bảng khác có `nguoi_tao` kiểu `int` trỏ `user_account.id`. ⇒ **Một tên cột, hai kiểu, hai ngữ nghĩa** — không join thống nhất được, và **không truy được ai đã làm gì**. Chi phí đổi: 2.464 điểm (lớn nhất toàn CSDL) — xem N5 mục 50.

### 🟠 NP-9 — `da_thanh_toan` là **số tiền**, không phải cờ đúng/sai

`don_hang.da_thanh_toan` `decimal(15,2)` và `bien_ban_nghiem_thu.da_thanh_toan` `decimal(18,2)`. Tên đọc như boolean ("đã thanh toán?") nhưng chứa **số tiền**. Đây là **bẫy nghiệp vụ tiền bạc**: lập trình viên sau đọc tên rồi viết `if (don_hang.da_thanh_toan)` sẽ được `true` với **mọi** đơn đã trả dù chỉ 1 đồng. Chi phí đổi tên chỉ 53 điểm.

### 🟡 NP-10 — `kho_giao_dich.nguoi_thuc_hien` trộn 3 loại giá trị trong `varchar(20)`

Ba nguồn ghi khác namespace vào cùng một cột: tên người **gõ tay** (UI), **`String(hr_employee_nhanvien.id)`** (engine tồn kho nhánh giao hàng), và chuỗi **`"system"`**. Cột rộng 20 ký tự.

### 🟡 NP-11 — 87% cột không có mô tả, và 21 mô tả đang hỏng mã tiếng Việt

Chỉ **194/1.526 cột (12,7%)** có `COLUMN_COMMENT`. Trong 194 đó, **21 cột có tiếng Việt bị hỏng mã** — 12 cột mất dấu thành `?` (**không phục hồi được**), 9 cột UTF-8 mã hoá hai lần (còn phục hồi được). Đây chính là **nguyên nhân gốc** khiến các tên viết tắt ở N5 trở nên không giải được: không có mô tả để tra.

### 🟡 NP-12 — `dm_kho` được comment tham chiếu nhưng không tồn tại

`phieu_xuat_kho.ma_kho` mang comment `'T??n/m?? kho (dm_kho ch??a t???n t???i)'` — vừa xác nhận bảng `dm_kho` **chưa tồn tại**, vừa là một ca hỏng mã tiếng Việt (thuộc NP-11).

### ⚠️ NP-13 — Đính chính số liệu báo cáo đợt 1

Xem §3.0: **"40 bảng"** thực chất là **40 khai báo**; số bảng phân biệt là **22**. TanPhatAI cần sửa con số này khi cập nhật tài liệu.

---

## 7. TRẠNG THÁI AN TOÀN KHI KẾT PHIÊN

| Mục | Trạng thái |
|---|---|
| Đường hầm SSH tới production | **ĐÃ ĐÓNG** — xác nhận cổng 13307 không còn mở, 0 tiến trình `ssh` còn lại |
| File tạm chứa bí mật kết nối | **ĐÃ XOÁ** |
| Ghi lên production | **0 lệnh** — chỉ `SELECT` / `SHOW` / `information_schema` |
| `src/` · `migrations/` · `sql/` | **0 file bị đụng** |
| Commit / push / deploy | **KHÔNG** — đúng ràng buộc READ-ONLY |
| Docker MariaDB cục bộ | **đang chạy** (bật ở phiên trước để đọc DB). Tắt bằng `npm run tat` nếu không cần |

---

## 8. PHỤ LỤC — LỆNH ĐÃ CHẠY

**Trên production (qua đường hầm SSH, chỉ đọc):**
```
SHOW CREATE TABLE dm_khach_hang
SELECT … information_schema.KEY_COLUMN_USAGE JOIN REFERENTIAL_CONSTRAINTS    (FK người)
SELECT … information_schema.COLUMNS     (bao_gia · lenh_san_xuat · phieu_xuat_kho · kiểm kê 202 cột)
SELECT … information_schema.TABLES      (danh sách bảng · kiểm 22 bảng docs-only)
SELECT … information_schema.STATISTICS  (383 index — dựng chữ ký schema để so)
SHOW GRANTS
SELECT COUNT(*) / COUNT(DISTINCT) / EXISTS(...)   (đối chứng dữ liệu — CHỈ ĐẾM, không đọc giá trị cá nhân)
```

**Trên máy phát triển:** cùng bộ truy vấn trên `tanphat_erp_dev` để dựng chữ ký so sánh, cộng `information_schema.COLUMNS` đầy đủ cho N5.

**Trên mã nguồn:** grep + đọc trực tiếp `src/lib/m5-store.ts`, `src/lib/m3-store.ts`, `src/lib/security/*`, `src/lib/action-permission.ts`, `src/app/m3/**`, `src/app/m5/**`, `src/types/m3.ts`, `migrations/`, `scripts/tests/`, `docs/🏭 ERP TanPhat - FIX/**`.

---

════════════ BÁO CÁO KẾT THÚC ════════════

**1. ĐÃ LÀM**
- **N1:** kết nối read-only tới production qua đường hầm SSH; chạy lại 7 phép đo chính → **7/7 `RUNTIME_PROVEN`**; so chữ ký **toàn schema** prod↔dev (99 bảng · 1.526 cột · 108 FK · 383 index) → **4 điểm lệch**, đã truy ra nguyên nhân; đo dữ liệu thật production (chỉ đếm).
- **N2:** truy vết 24 cột người M5 kèm `file:line` cho cả điểm ghi lẫn điểm đọc; lần ngược 3 chuỗi truyền actor; liệt kê 9 ô gõ tay UI; gom 7 nhóm điểm cần sửa.
- **N3:** quét lại **toàn bộ** `CREATE TABLE` trong docs (98 bảng) → **đính chính "40 bảng" thành 22 bảng**; phân loại từng bảng kèm trích nguyên văn; gom 6 cụm quyết định.
- **N4:** dựng 2 sơ đồ luồng (báo giá, đơn hàng), bảng 30 điểm đọc/ghi, bảng đối chiếu M1↔M3 chín tiêu chí, 16 điểm cần sửa.
- **N5:** rà 1.526 cột / 99 bảng → **65 mục đề xuất** kèm chi phí đếm thật 3 vùng; 9 chỗ phá vỡ quy luật đặt tên; TOP 10 ưu tiên; 11 cảnh báo không nên đổi; 21 comment hỏng mã.
- **N6:** 13 phát hiện ngoài phạm vi, ghi riêng.
- Viết báo cáo `docs/AUDIT-TONG-LUC-DOT2-2026-08-20.md`.

**2. PHẠM VI**
- **ĐỤNG:** `docs/AUDIT-TONG-LUC-DOT2-2026-08-20.md` (tạo mới) · `docs/OWNER-REQUEST-LEDGER.md` (thêm mục #89) · `.governance/registry/tech-debt.md` (thêm `DEBT-030`…`DEBT-034`).
- **KHÔNG ĐỤNG:** `src/` (0 file) · `migrations/` (0) · `sql/` (0) · DDL (0 lệnh) · DML (0 lệnh) · production (0 lệnh ghi) · version (không bump) · commit/push/deploy (không).

**3. BẰNG CHỨNG**
- Production `SHOW CREATE TABLE dm_khach_hang` → `sale_phu_trach int(11) DEFAULT NULL`, **0 CONSTRAINT FOREIGN KEY** → **`RUNTIME_PROVEN`**
- Production `information_schema` → 202 cột người · 85 INT · 114 VARCHAR · 92 bảng — **khớp tuyệt đối dev** → **`RUNTIME_PROVEN`**
- So chữ ký toàn schema prod↔dev → 4 điểm lệch, đều thuộc `mua_hang`; đối chiếu `migrations/20260731_a2p2_…sql` khai `LOCAL ONLY` → **`DB_PROVEN` + `FILE_PROVEN`**
- Production `role_action_permission` = **3 dòng** vs dev **26 dòng** → **`RUNTIME_PROVEN`** (NP-1)
- Production `hr_employee_nhanvien` 30 dòng, `SUM(user_id IS NOT NULL)` = **0** → **`RUNTIME_PROVEN`** (NP-2)
- M5: `m5-store.ts` 38 điểm fallback `"system"`; `giao-hang/actions.ts:▮` là **điểm ghi actor duy nhất** → **`CODE_PROVEN`**
- M3: `m3-store.ts:▮,549,697,713,1155` hardcode `?? 1`; `bao-gia/page.tsx:▮` `listBaoGia()` không scope → **`CODE_PROVEN`**
- Docs: 98 bảng khai / 76 có thật / **22 không có**; kiểm chéo production **0/22 có mặt** → **`DOC_PROVEN` + `RUNTIME_PROVEN`**
- Naming: 194/1.526 cột có comment, 21 comment hỏng mã; 58 cột ngày kiểu `text`; `nguoi_tao` 38 bảng `int` vs 45 bảng `varchar` → **`DB_PROVEN`**

**4. GHI SỔ YÊU CẦU OWNER**
- [x] **ĐÃ GHI** — mục **#94**

**5. PUSH BÁO CÁO CÔNG KHAI**
- [x] **CHƯA PUSH** — lý do: prompt khai **READ-ONLY, cấm commit/push/deploy**. Báo cáo nằm ở cây làm việc local, chưa commit. Chờ Owner cho phép.

**6. CÒN SÓT / CHƯA LÀM**
- Chưa quét cột người trong **view / stored procedure / trigger** (chỉ rà `BASE TABLE`) → `DEBT-030`
- Chưa quét `backup-vps-to-local.sql` (3,5 MB) và `WORK_LOG.md` (402 KB) — có thể chứa bằng chứng dứt điểm cho `cskh_task` / `task_assignment` → `DEBT-031`
- Chưa đối chiếu bản **Notion trực tuyến** (chỉ đối chiếu bản xuất `.md` trong repo) → `DEBT-032`
- Chưa kiểm `docs/_snapshot*` (cố ý loại trừ) → `DEBT-032`
- Con số grep của N5 cho **từ khoá chung** (`task`, `is_*`, `created_*`) chưa sạch — lẫn biến UI, cần AST analysis mới tách được → `DEBT-033`
- Chưa chạy `COUNT(*)` chính xác trên 99 bảng (dùng `TABLE_ROWS` ước lượng của InnoDB) → `DEBT-034`

**7. ĐANG CHỜ OWNER**
- **NP-1 + NP-2**: có nạp ma trận phân quyền và liên kết 30 nhân viên ↔ tài khoản lên production không → **chặn: mọi tài khoản không-phải-admin dùng được hệ thống**.
- **NP-3**: có chạy migration A2-P2 lên production không → chặn: tính năng chứng từ mua hàng `draft`.
- **X-4 còn 3 bảng CẦN OWNER HỎI**: cặp `cong_no_khach_hang`/`cong_no_nha_cung_cap` (giữ kế hoạch tách AR/AP hay chốt `cong_no`?) và `task_assignment` (M8 có giữ multi-assignee không?).
- **NP-6 + NP-7**: có coi mass-assignment và thiếu lọc phạm vi M3 là việc phải sửa không → chặn: mở phase code M3.
- **N5**: duyệt TOP 10 đổi tên? Riêng mục 5 (58 cột ngày kiểu `text` → `datetime`) và mục 50 (`nguoi_tao` hai nghĩa) **cần kế hoạch riêng**, không gộp gói.
- **4 câu hỏi nghĩa nghiệp vụ** (N5 §5.6 mục 3): `uom_tc_id` — `tc` là gì? · `gia_cong_ids` — "gia công" hay "giá công"? · `loai_kem` — "kẽm" in offset? · `kho_giay` — "khổ" hay "kho"?
- Cho phép commit/push báo cáo này hay giữ local.

**8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC**
Owner quyết **NP-2** (liên kết 30 nhân viên ↔ tài khoản trên production) — đây là nút thắt lớn nhất: chừng nào chưa làm, hệ thống vận hành **chỉ dùng được bằng đúng một tài khoản admin**, và mọi công trình ownership/phân quyền đã xây ở M1 đều **chưa phát huy tác dụng trên máy thật**.

**9. CHƯA XÁC MINH ĐƯỢC**
- **Domain thật của `id_nhan_vien_phu_trach`** — production cũng **toàn giá trị `1`**, tồn tại ở cả `user_account.id` lẫn `hr_employee_nhanvien.id` ⇒ **cả hai môi trường đều không phân xử được bằng dữ liệu**. *Ai xác minh:* **Owner** (câu hỏi nghiệp vụ X-1).
- **Ý nghĩa 121/124 giá trị VARCHAR người trên production** — không phải email, không phải `ma_nv`; là chuỗi seed/gõ tay. *Ai xác minh:* Owner.
- **Có ai gọi `updateBaoGiaAction` từ ngoài codebase** — grep chỉ chứng minh trong `src/` không có caller, không chứng minh được ngoài.
- **4 tên cột nghĩa nghiệp vụ mơ hồ** (N5 §5.6 mục 3) — comment gốc đã hỏng mã, không tra được.
- **Quan hệ thật `material_item` ↔ `dm_vat_tu`** — hai bảng trùng ngữ nghĩa, `material_item` 0 dòng. Không rõ là mở rộng có chủ đích hay tàn dư di trú. *Ai xác minh:* Owner hoặc lịch sử migration M1.

**10. TRẠNG THÁI CHUNG**
- [ ] PASS
- [x] **PROVISIONAL** — thiếu: 6 nhóm quyết định Owner ở trường 7. **Điều kiện lên PASS:** Owner trả lời các mục đang chờ.
- [ ] BLOCKED

> **6/6 nhiệm vụ N1–N6 hoàn tất đầy đủ kèm bằng chứng.** Riêng **X-5 đã ĐÓNG DỨT ĐIỂM** — trạng thái `PROVISIONAL` của báo cáo đợt 1 nay được gỡ: 7/7 phép đo là `RUNTIME_PROVEN` trên máy vận hành thật. Trạng thái `PROVISIONAL` của báo cáo này là do **quyết định nghiệp vụ còn treo**, không phải do nhiệm vụ nào bỏ dở.

**11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU**
- Phiên có bị nén ngữ cảnh không: **KHÔNG**
- Tham chiếu đã đọc **trong phiên**: `migrations/20260731_a2p2_mua_hang_draft_nullable.sql` (+ file rollback) · `scripts/pull-from-vps.ps1` · `scripts/lib/db-env.ts` · `docs/AUDIT-FK-NGUOI-VA-CUSTOMER-OWNERSHIP-2026-08-20.md` (kết quả đợt 1) · `docs/OWNER-REQUEST-LEDGER.md` · `.governance/registry/tech-debt.md`. Bốn agent con đọc thêm: `docs/🏭 ERP TanPhat - FIX/**` (M1/M3/M4/M8/M9/MF), `src/lib/m5-store.ts`, `src/lib/m3-store.ts`, `src/lib/security/*`, `src/app/m3/**`, `src/app/m5/**`, `information_schema` của cả hai môi trường.
- **KHÔNG đụng UI** ⇒ `GOV-READ-STANDARD-001` không kích hoạt (phiên không sửa chữ hiển thị, lớp trình bày, màu, cột bảng, biểu tượng, component hay bố cục nào).

═══════════════════════════════════════════
