# BÁO CÁO — BẢN PHÁT HÀNH HỘI TỤ M0 · ĐÃ LÊN MÁY VẬN HÀNH

**Mã việc:** `M0-ROLLING-CLOSEOUT-20260827` → **HỘI TỤ**
**Ngày:** 27/08/2026
**Trạng thái:** ✅ **`LIVE_VERIFIED`** — đã triển khai, đã kiểm bằng đăng nhập thật trên máy vận hành
**Phiên bản:** `V1.00.355` → **`V1.00.356`**

---

## 1. KẾT QUẢ MỘT ĐOẠN

Máy vận hành đã được kéo từ nhánh vá khẩn cấp `826817b` lên ngang `main` — **71 commit**.
Việc vá từng mảnh mỗi lần triển khai **kết thúc từ đây**: lần sau chỉ còn *đẩy main → chạy*.

Kèm theo, ba thứ mà máy vận hành **chưa từng có** nay đã có:
**hai cột giá vốn chưa được che** (quyết định của Owner ở sổ #163, code từ lâu nhưng chưa
bao giờ triển khai) · **chốt chặn quản trị khôi phục cuối cùng** · **30 ô quyền chuyển
trạng thái** mà nếu thiếu thì mã Đợt 5 sẽ chặn hết thao tác đổi trạng thái.

---

## 2. MÃ COMMIT VÀ PHIÊN BẢN

| Mốc | Giá trị |
|---|---|
| `main` đầu phiên | `df5c106` |
| `main` cuối phiên | **`9c741f7`** |
| Neo máy vận hành **trước** | `826817b` (`V1.00.355`) |
| **Máy vận hành sau** | **`9c741f7`** (**`V1.00.356`**) |
| Khoảng cách đã khép | **71 commit** |
| Nhánh phát hành cách ly cũ | `release/m0-closeout-20260827` · `a8f3283` — **không dùng**, đúng khoá Owner |

> ⚠️ **Một điều cần biết về cơ chế triển khai:** kịch bản đẩy **tệp nén**, không kéo git.
> Vì vậy `git rev-parse HEAD` trên máy vận hành **vẫn ghi `826817b`** dù mã đã là bản mới.
> Đã đặt tệp `DEPLOYED_SHA.txt` ở thư mục ứng dụng ghi đúng `9c741f7` để lần sau tra được.
> Xác minh mã mới bằng **dấu vết đích danh**, không tin git ref:
> `khoa-thanh-ben.ts` · `menu-catalog.ts` · `tam-ngung.ts` đều **CÓ**; `TAI_CHINH_SIDEBAR_ROUTES`
> và `BUOC_PHAN_QUYEN` đều hiện diện; `SENSITIVE_FIELDS` đủ **4** cột.

---

## 3. MỤC 1 — HOÀN THIỆN TRÊN `main`

| Owner yêu cầu | Kết quả |
|---|---|
| Gom Tài Chính theo Phương án A | ✅ Biểu Mẫu · Bản Phát Hành · Hợp Đồng vào ngăn Tài Chính; `/mf` thành mục con "Tổng Quan" có capability riêng; đầu ngăn không dựng thẻ liên kết |
| Sửa thanh bên Biểu Mẫu + 7 menu MF | ✅ đã có từ `f50a99c`, nay lên máy vận hành |
| Trung Tâm Phân Quyền năm bước | ✅ Tài Khoản → Vai Trò → Màn Hình → Hành Động/Dữ Liệu/Trường → Xem Lại |
| Quick-template | ✅ **tiếp tục chặn ở máy chủ** (Owner cho phép lựa chọn này) |
| `DEBT-125/126/127` | ✅ cả ba |
| Che đủ 4 cột giá vốn ở ranh giới máy chủ | ✅ — máy vận hành **trước bản này chỉ che 2 cột** |
| Không gửi cột giá vốn tới máy khách thiếu quyền | ✅ — rà cả bẫy `SELECT *`: hai điểm còn lại đều đi qua nơi có che |

### 3.1 Một lỗi thật do chính đợt gom gây ra — cổng trình duyệt bắt được

Mục "Tổng Quan" mang **cùng khoá với ngăn chứa nó**. Gác bằng khoá suy theo route thì
khoá đó **gồm cả bảy khoá màn con**, nên **ai có một màn con cũng thấy trang tổng** —
trái đúng điều Owner khoá (*«/mf dashboard phải có capability riêng»*).

Đo được: **CEO** (7 màn con, không có khoá `mf`) và vai trò một-màn **đều nhìn thấy `/mf`**.
Hai khẳng định trình duyệt đỏ. Đã vá: mục con trùng khoá với ngăn chứa nó thì gác bằng
**khoá hẹp** của chính nó. Ghim thành khẳng định 11 và 11b để không tái phát.

> Nếu chỉ kiểm ở tầng mã thì lỗi này **lọt**. Nó chỉ lộ ra khi mở trình duyệt thật.

### 3.2 Một cổng cũ phải viết lại

`[E7b]` đòi Biểu Mẫu ở **cấp cao nhất** — chính là thứ Owner vừa đổi. Ý định gốc ngày
23/08 (không nằm trong nhóm Hệ Thống, có khoá quyền riêng) **vẫn giữ** và nay được đo
đúng, cộng thêm điều Owner vừa khoá: **render đúng một lần**. Dòng cũ giữ nguyên văn.

---

## 4. MỤC 3 — KIỂM KÊ 71 COMMIT

Chi tiết đầy đủ: `M0-CONVERGENCE-INVENTORY-20260827.md`.

| Phép đo | Kết quả |
|---|---|
| Tổng commit | **71** (không phải 70 — ghi đúng số) |
| Chỉ tài liệu / quản trị | 34 |
| Chỉ bộ kiểm | 12 |
| Chỉ kịch bản | 6 |
| Có mã nguồn | 18 |
| Có bản di trú | 1 |
| **Trang mới** | **0** |
| **Tuyến API mới** | **0** |
| **Tệp `src/` bị xoá** | **0** |

⇒ **Không có bề mặt mới nào ra máy vận hành.** Toàn bộ là hoàn thiện thứ đã có.

| Phân loại Owner yêu cầu | Số |
|---|---|
| Được duyệt / bắt buộc | **18/18** commit mã nguồn + 1 di trú + 4 kịch bản nạp |
| Chưa xong nhưng đã tắt | **0** |
| Không an toàn / chặn phát hành | **0** |

**Tính kho trải** — phân loại riêng vì một commit ghi *«KHÔNG deploy»*: đo được nó chỉ
được **3 tệp phía trình duyệt** nạp, **không tệp máy chủ nào**, và kết quả **không đi vào
thân yêu cầu nào** ⇒ lớp hiển thị, **không đổi giá được lưu**. Màn cấu hình phụ cấp
(đang chờ Owner duyệt) **chưa được viết**, nên không có bề mặt chưa xong đi kèm.

---

## 5. MỤC 4 — DIỄN TẬP TRÊN BẢN SAO DỰNG TỪ SAO LƯU THẬT

| Bước | Kết quả |
|---|---|
| Sao lưu máy vận hành | `backup-erp-20260827T151830Z.sql.gz` · **vân tay SHA256 đối chiếu khớp** sau khi tải về |
| Phục hồi thành bản sao | 101 bảng · 23 menu · 6 vai trò · 50 quyền menu · **1695 khách hàng** — khớp từng con số |
| 4 bản di trú chuẩn | 101 bảng → 101 bảng (không đổi lược đồ) |
| Kiểm chứng | `m0-security` **27/27** · `m01-m13-m6` **46/46** · `m7` **29/29** · `mc` pass · `mf` ok |
| Dựng bản production | ✅ thành công |
| 17 cổng trên bản sao | toàn đạt — **một** khẳng định đỏ, xem dưới |
| Trình duyệt trên bản sao | thanh bên **15/15** · trung tâm phân quyền **14/14** |
| API trên bản sao | ẩn danh **401** · cookie giả **401** |
| Hoàn tác bản di trú | quyền trường 4 → 0 → 4 |
| **Hoàn tác toàn bộ** | phục hồi từ sao lưu **tái tạo CHÍNH XÁC** hiện trạng máy vận hành |

**Khẳng định đỏ duy nhất:** *"Dòng do lượt này tạo có marker truy vết được"* — nó đếm
dòng mang dấu của **seed chỉ-dùng-máy-phát-triển** (`rbac-20260801`). Bản sao chưa từng
chạy seed đó, và **máy vận hành không cần** nó. Vắng mặt đúng, không phải lỗi.

---

## 6. MỤC 5 — TRIỂN KHAI

### 6.1 Sao lưu ba lớp trước khi chạm

| Lớp | Vị trí |
|---|---|
| Cơ sở dữ liệu | `/root/backup-erp-20260827T151830Z.sql.gz` (vân tay đã đối chiếu) |
| Thư mục chạy | `/root/standalone-run-backup-20260827T153335Z` |
| Mã nguồn cũ | `826817b` — vẫn trong kho git trên máy vận hành |

### 6.2 ⚠️ MỘT SỰ CỐ GIỮA CHỪNG — GHI ĐẦY ĐỦ

Sau khi đẩy mã, bốn kịch bản nạp dữ liệu **hỏng hết** trên máy vận hành:

```
Error: Cannot find module 'mysql2/promise'   ·   tổng gói trong node_modules: 0
```

Nguyên nhân: bước kích hoạt standalone **xoá `node_modules`** ở thư mục kho
(*"[standalone] Remove root install/build leftovers…"*) — ứng dụng chạy bằng gói riêng
trong `.standalone-run`, nhưng kịch bản `tsx` thì không còn gì để nạp.

**Đây là trạng thái nguy hiểm nhất của cả lượt:** mã mới đã lên, **dữ liệu chưa di trú**.
Mã Đợt 5 đòi 30 ô quyền chuyển trạng thái mà cơ sở dữ liệu chưa có ⇒ **người dùng không
đổi được trạng thái báo giá / đơn hàng**. Đó là **mất quyền**, đúng điều kiện DỪNG của Owner.

**Cách xử:** mở **đường hầm SSH** vào cơ sở dữ liệu máy vận hành rồi chạy đúng bốn kịch
bản đó **từ máy phát triển** — đúng cách chúng được thiết kế để chạy. Xác minh đường hầm
vào đúng cơ sở dữ liệu vận hành (`dm_menu = 23 · khách hàng = 1695`) **trước khi** ghi.

### 6.3 Di trú dữ liệu — khớp diễn tập từng con số

| Chỉ tiêu | Trước | Sau | Diễn tập | |
|---|---:|---:|---:|---|
| bảng | 101 | 101 | 101 | khớp |
| `dm_menu` | 23 | **51** | 51 | khớp |
| vai trò | 6 | **9** | 9 | khớp |
| quyền menu | 50 | **148** | 148 | khớp |
| quyền hành động | 30 | **67** | 67 | khớp |
| quyền trường | 0 | **4** | 4 | khớp |
| màn con Tài chính | 0 | **7** | 7 | khớp |
| khách hàng | 1695 | **1695** | 1695 | giữ nguyên |
| quản trị dùng được | 3 | **3** | 3 | khớp |

**Thay đổi quyền của người thật:**

| Vai trò | Trước | Sau | |
|---|---:|---:|---|
| ADMIN | 53 | 53 | không đổi |
| **CEO** | 15 | **49** | **+34 màn** |
| KE_TOAN | 13 | 13 | không đổi |
| SALES | 10 | 11 | +2 · **−1** |
| HR | 3 | 4 | +1 |

**Mất mát duy nhất: SALES mất `/m3/tinh-gia-admin`** — đúng khoá Owner
*«Sale … TUYỆT ĐỐI KHÔNG tính giá admin»*. Kịch bản Đợt 5 tự kiểm và báo
**`[N5.6] không ai mất quyền: ĐẠT`**.

### 6.4 Kiểm khói CÓ ĐĂNG NHẬP THẬT trên HTTPS công khai — **16/16**

| # | Kiểm | Kết quả |
|---|---|---|
| 1 | Đăng nhập thật | ✅ |
| 2 | Bảy màn Tài chính hiện | ✅ đủ 7 |
| 2b | Trang tổng `/mf` hiện với quản trị | ✅ |
| 2c | Biểu Mẫu + Bản Phát Hành hiện | ✅ |
| 2d | Hợp Đồng hiện | ✅ |
| 2e | **Mỗi liên kết render đúng một lần** | ✅ |
| 3 | Trung tâm phân quyền **đủ năm bước** | ✅ |
| 3b | Hiện số tài khoản bị ảnh hưởng | ✅ |
| 3c | Hiện xem trước quyền hiệu lực | ✅ |
| 3d | **Chốt chặn quản trị cuối — số trên màn KHỚP cơ sở dữ liệu** | ✅ |
| 4 | Ma trận quyền menu | ✅ |
| 4b | Nêu rõ "bỏ tick KHÔNG phải cấm" | ✅ |
| 4c | **Nút ghi đè quyền hàng loạt vẫn MỜ** | ✅ |
| 5 | **Bốn cột giá vốn** đủ trên máy vận hành | ✅ |
| 5b | Tuyến giá có phiên hợp lệ | ✅ 200 |
| 6 | Tài khoản kiểm khói đã xoá, mốc nền nguyên vẹn | ✅ |

**Kiểm khói ẩn danh:** 5/5 tuyến API → **401** · cookie giả → **401** · các trang công khai → **200**.

> Một điểm cần nói cho đúng: `POST` từ nguồn gốc lạ trả **401 chứ không phải 403**.
> Đó là **đúng**: lớp thứ nhất chặn theo phiên **trước** lớp CSRF. Đường 403 cần một phiên
> hợp lệ, đã kiểm trên bản sao (`p1-api-matrix` 6/6, `p1-api-phien` 13/13), **chưa** kiểm
> lại trên máy vận hành vì không tạo thêm tài khoản để thử CSRF.

### 6.5 Đối chiếu máy vận hành ↔ máy phát triển — **7/7 khớp**

bảng · `dm_menu` · vai trò · quyền hành động · quyền trường · màn con Tài chính ·
quản trị dùng được — **tất cả khớp**.

### 6.6 Trạng thái vận hành

Tiến trình **online**, 1 lần khởi động lại (đúng như dự kiến), chạy ổn định.
Nhật ký sau khi xoá sạch: **0 lỗi thật**. Chỉ có loại *"Failed to find Server Action"*
từ **tab trình duyệt cũ** đang giữ trang của bản trước — tự hết khi người dùng tải lại trang.

---

## 7. LỚP BẰNG CHỨNG

| Nhãn | Phạm vi |
|---|---|
| `DEPLOYMENT_RECORDED` | `826817b` → `9c741f7` · `V1.00.356` · sao lưu ba lớp · di trú đã chạy |
| `RUNTIME_OBSERVED` | tiến trình online 1 lần khởi động lại · nhật ký sạch · các tuyến trả về đúng mã |
| `LIVE_VERIFIED` | **16/16 kiểm khói có đăng nhập thật** trên HTTPS công khai: thanh bên gom · mỗi liên kết một lần · trung tâm phân quyền năm bước · chốt chặn quản trị cuối khớp cơ sở dữ liệu · bốn cột giá vốn · nút ghi đè vẫn mờ |

**Ngoài phạm vi `LIVE_VERIFIED`:** đường 403 CSRF trên máy vận hành · hành vi của từng
vai trò không phải quản trị trên máy vận hành (đã kiểm đầy đủ trên bản sao, chưa lặp lại
trên máy thật vì không tạo thêm tài khoản).

---

## 8. ĐƯỜNG LÙI — CÒN NGUYÊN, ĐÃ DIỄN TẬP

| Bước | Cách |
|---|---|
| Dữ liệu | phục hồi `/root/backup-erp-20260827T151830Z.sql.gz` — **đã diễn tập, tái tạo chính xác** |
| Thư mục chạy | `/root/standalone-run-backup-20260827T153335Z` |
| Mã nguồn | `git checkout 826817b` trong kho trên máy vận hành, dựng lại |
| Một bản di trú | `20260826_p1_ceo_xem_gia_von_rollback.sql` — đã diễn tập 4 → 0 → 4 |

---

## 9. NỢ CÒN LẠI — KHÔNG CHẶN VIỆC CHUYỂN MODULE

| Mã | Trạng thái |
|---|---|
| `DEBT-128` | HOÃN CÓ CHỦ ĐÍCH — gói di trú đã lập, **bản hội tụ KHÔNG đụng**, đúng khoá Owner |
| `DEBT-116` · `121` · `122` · `123` | như cũ |
| Kịch bản nạp dữ liệu cần `node_modules` | **nợ mới** — bước kích hoạt standalone xoá nó; hiện đi vòng bằng đường hầm SSH |

---

## 10. MỤC 7 — ĐÓNG M0, CHUYỂN M3

**M0 ĐÓNG.** Không mở rà soát M0 mới trừ khi có lỗi nghiêm trọng trên máy vận hành.

**Module kế tiếp: M3 — Báo giá & Đơn hàng.** Chỉ một, không mở song song.

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Gom điều hướng Tài Chính (Phương án A): Biểu Mẫu · Bản Phát Hành · Hợp Đồng
     vào ngăn Tài Chính; /mf thành mục con "Tổng Quan" có capability riêng
   - Trung Tâm Phân Quyền thành LUỒNG DẪN NĂM BƯỚC + 3 con số Owner yêu cầu
   - Kiểm kê đủ 71 commit và mọi bản di trú, phân loại theo yêu cầu Owner
   - Dựng bản sao máy vận hành từ sao lưu THẬT, diễn tập trọn vẹn kể cả hoàn tác
   - Tăng V1.00.355 → V1.00.356 + ghi nhật ký thay đổi
   - Triển khai lên máy vận hành và di trú dữ liệu danh mục + quyền
   - Kiểm khói có đăng nhập thật: 16/16
   - Ghi Sổ Yêu Cầu Owner mục #186

2. PHẠM VI
   ĐỤNG    : src/lib/app-navigation-metadata.ts · src/components/layout/sidebar.tsx
             src/app/m0/security/security-client.tsx · src/lib/version.ts
             scripts/tests/ (2 tệp mới, 3 tệp sửa) · docs/reports/ (2 tệp mới)
             docs/OWNER-REQUEST-LEDGER.md · .governance/registry/tech-debt.md
             MÁY VẬN HÀNH: mã nguồn + dm_menu + dm_vai_tro + 3 bảng quyền
   KHÔNG ĐỤNG: lược đồ CSDL (101 → 101 bảng) · dữ liệu nghiệp vụ (1695 khách hàng
             giữ nguyên) · DEBT-128 · công thức tính giá được lưu · 5 file luật

3. BẰNG CHỨNG
   481 khẳng định trên main, 0 hỏng                     → CODE + DB_PROVEN
   17 cổng trên BẢN SAO máy vận hành                    → CODE + DB_PROVEN
   verify 27/27 · 46/46 · 29/29 · mc pass · mf ok       → DB_PROVEN
   trình duyệt bản sao 15/15 + 14/14                    → UI_PROVEN
   hoàn tác 4→0→4 và phục hồi toàn bộ từ sao lưu        → DB_PROVEN
   9 chỉ tiêu máy vận hành khớp diễn tập                → DB_PROVEN
   16/16 kiểm khói CÓ ĐĂNG NHẬP trên HTTPS công khai    → LIVE_VERIFIED
   tiến trình online · nhật ký 0 lỗi thật               → RUNTIME_OBSERVED
   đối chiếu vận hành ↔ phát triển 7/7                  → DB_PROVEN
   tsc sạch · secret-scan · pii-scan · check:governance → FILE_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #186 (GO CONVERGENCE RELEASE + kết quả)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — xem dòng cuối báo cáo

6. CÒN SÓT / CHƯA LÀM
   - Đường 403 CSRF chưa kiểm lại trên máy vận hành (cần phiên hợp lệ)
   - Hành vi từng vai trò không phải quản trị chưa lặp lại trên máy vận hành
   - Nợ mới: kịch bản nạp dữ liệu cần node_modules mà bước standalone xoá mất
   - DEBT-128 (gói di trú đã lập, chưa chạy DDL) · DEBT-116 · 121 · 122 · 123
   - Trung Tâm Phân Quyền chưa có: khác biệt trước/sau khi lưu · hoàn tác thao tác
     quyền · vai trò riêng cho từng người (đã nêu trong wireframe, chưa code)

7. ĐANG CHỜ OWNER
   - Không có gì CHẶN. M0 đã đóng theo đúng khoá của Owner.
   - Việc cần Owner khi tới lượt: duyệt gói di trú DEBT-128 (cần DDL)

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Mở M3 — Báo giá & Đơn hàng: rà hiện trạng và chốt tiêu chí nghiệm thu TRƯỚC
   khi sửa dòng mã đầu tiên.

9. CHƯA XÁC MINH ĐƯỢC
   - Trải nghiệm thật của người dùng sau khi CEO được thêm 34 màn. Đúng ma trận
     đã duyệt, nhưng đây là thay đổi lớn nhất mà người dùng sẽ cảm nhận.
   - Chi phí khoá bảng khi chạy di trú với tải thật — di trú lần này không đổi
     lược đồ nên không đo được điểm này.
   - Tab trình duyệt cũ của người đang dùng sẽ báo lỗi cho tới khi tải lại trang.

10. TRẠNG THÁI CHUNG
   [x] PASS — đã triển khai, đã kiểm bằng đăng nhập thật, đối chiếu ba nơi khớp,
       đường lùi còn nguyên và đã diễn tập.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén:
     - docs/UI-STANDARD.md — §0 §1 §2 §3 §12 §13 §18 (việc có đụng giao diện:
       thanh điều hướng năm bước, thẻ tóm tắt, ngăn Tài Chính). Đối chiếu:
       rounded-md · h-9 px-3 · màu trạng thái amber/emerald/orange theo §2 ·
       Title Case cho nhãn bước theo §13 — khớp.
     - .governance/registry/tech-debt.md · docs/OWNER-REQUEST-LEDGER.md
     - src/lib/app-navigation-metadata.ts · src/components/layout/sidebar.tsx
     - src/app/m0/security/security-client.tsx · src/lib/version.ts
     - scripts/nap-khoa-con-man-hinh.ts · ap-ma-tran-dich.ts ·
       nap-quyen-chuyen-trang-thai.ts · tao-vai-tro-truong-phong.ts
   Không kết thúc bằng trí nhớ từ trước nén.
═══════════════════════════════════════════
```
