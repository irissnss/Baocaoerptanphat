# WIREFRAME ĐÃ SỬA — THANH BÊN + TRUNG TÂM PHÂN QUYỀN

**Ngày:** 27/08/2026
**Căn cứ:** Mười khoá của Owner ngày 27/08/2026 (sổ mục **#183**)
**Trạng thái:** ⏳ **CHỜ OWNER DUYỆT** — **chưa đổi lược đồ · chưa DDL · chưa triển khai**
**Thay cho:** `WIREFRAME-SIDEBAR-GOM-TAI-CHINH-20260827.md` · `WIREFRAME-SECURITY-CENTER-20260827.md`
(hai bản đó **giữ nguyên tại chỗ** làm lịch sử — bản này nêu rõ chỗ nào đã sửa và vì sao)

---

## A. NHỮNG CHỖ BẢN TRƯỚC SAI — SỬA THEO ĐO ĐẠC

| # | Bản trước nói | Đo lại được | Sửa thành |
|---|---|---|---|
| 1 | Gom *Biểu Mẫu* và *Hợp Đồng* vào dưới *Tài Chính* | Ba mục là **ba module khác nhau**: `form_mau` (route `/bieu-mau`) · `mc` (route `/mc`) · `mf` (route `/mf`, 7 màn con). Gom dưới một cha sẽ khiến một khoá xuất hiện ở hai chỗ — **đúng điều Owner cấm** ở mục III.7 | **KHÔNG gom.** Giữ ba mục cấp cao nhất. Việc cần chữa không phải chỗ đứng của mục, mà là **khoá quyền sai** khiến mục biến mất |
| 2 | "Bảng quyền-trường hiện **RỖNG**" | **4 dòng, đều là CEO**, do chính bản di trú 26/08 thêm vào | Đã sửa trong bản trước; giữ đúng ở đây |
| 3 | Không nêu luật hợp nhất đa vai trò | Ba tầng dùng **hai luật trái nhau** (mục C dưới đây) | Nêu rõ, **không tự chọn bên** — ghi `DEBT-127` chờ Owner phân xử |

---

## B. THANH BÊN — HÌNH DUNG SAU KHI SỬA

### B.1 Người dùng chỉ được cấp **một** màn Tài chính (ví dụ Công nợ)

```
┌────────────────────────────┐
│  ▸ Tài Chính           ▾   │ ← chữ thường, KHÔNG bấm được:
│      Công nợ               │   thiếu quyền trang tổng /mf
│                            │   (chỉ là accordion để mở/đóng)
│    Hợp Đồng                │ ← không hiện (chưa được cấp)
└────────────────────────────┘
```
**Trước khi sửa:** không thấy màn nào — cả bảy màn con đều quy về một khoá `mf`.

### B.2 Người dùng được cấp trang tổng Tài chính + vài màn con

```
┌────────────────────────────┐
│  ▸ Tài Chính           ▾   │ ← bấm được, dẫn tới /mf
│      Phiếu thu             │
│      Công nợ               │
└────────────────────────────┘
```

### B.3 Người dùng được cấp Biểu Mẫu

```
┌────────────────────────────┐
│    Biểu Mẫu / Mẫu In       │ ← HIỆN
└────────────────────────────┘
```
**Trước khi sửa:** **không bao giờ hiện**, kể cả khi đã được cấp quyền —
thanh bên hỏi khoá `"bieu-mau"`, còn khoá thật là `"form_mau"`.

### B.4 Tài khoản chờ cấp phát (vai trò `USER`)

```
┌────────────────────────────┐
│  (không mục nghiệp vụ nào) │
└────────────────────────────┘
      Tài khoản đang chờ phân quyền
```
Đúng khoá Owner 26/08/2026 11:28. **Không** cấp "menu nền" để tránh màn trắng.

### B.5 Bốn quy tắc thanh bên nay tuân theo

1. **Khoá suy từ ROUTE**, qua đúng danh mục màn hình mà máy chủ dùng — không đoán bằng cắt gạch nối.
2. **Hợp các quyền** (Owner I.6): cần **một** khoá áp dụng được cấp là hiện. Bỏ tick không phải cấm.
3. **Không suy được khoá thì ẩn** (đóng-khi-thiếu). Không đoán bừa.
4. **Mục cha bấm được chỉ khi có quyền TRANG RIÊNG của nó** (Owner III.8–III.9), không nới theo màn con.

---

## C. HỢP NHẤT QUYỀN — HIỆN TRẠNG ĐO ĐƯỢC, KHÔNG PHẢI ĐỀ XUẤT

### C.1 Tám đường phân giải quyền đang tồn tại

| Đường | Tầng | Nơi khai | Số tệp gọi | Hợp nhất đa vai trò |
|---|---|---|---:|---|
| `canViewMenu` | menu | `security-store.ts:538` | 2 | `MAX(IFNULL(...))` — **cấp-thắng** |
| `canViewManHinh` | màn hình | `security-store.ts:523` | 4 | như trên |
| `requireMenuView` | trang | `security-guard.ts:28` | 27 | qua `canViewMenu` |
| `requireManHinhView` | trang theo route | `security-guard.ts:72` | 30 | qua `canViewManHinh` |
| `requireActionPermission` | hành động | `action-permission.ts:55` | 51 | `MAX(IFNULL(allowed,0))` — **cấp-thắng** |
| `requireSpecificAction` | hành động đích danh | `action-permission.ts:122` | 19 | như trên |
| `maskSensitiveFields` | trường nhạy cảm | `action-permission.ts:418` | 9 | `rows.some(...)` — **CẤM-THẮNG** |
| `guardApi` | tuyến API | `api-guard.ts:175` | 28 | `some(...)` — cấp-thắng |

### C.2 ⚠️ MÂU THUẪN CẦN OWNER PHÂN XỬ — KHÔNG TỰ QUYẾT

- Owner **27/08/2026** mục I.6: *«bỏ tick KHÔNG phải cấm tường minh»* nghĩa là **cấp-thắng**.
- Owner **17/07/2026**, ghi ngay trong mã (`action-permission.ts:313`): tầng trường **cấm-thắng**.

Hai quyết định Owner ở hai thời điểm nói khác nhau về **cùng một việc**.
Owner đã cấm *«tự phát minh thứ tự cho/cấm»* nên **không chọn bên**. Ghi `DEBT-127`, chờ Owner.
**Hiện chưa gây hại**: bảng quyền-trường chỉ có 4 dòng, đều CEO, đều cho xem.

### C.3 Đề xuất dịch vụ quyền hợp nhất — **CHỈ LÀ ĐỀ XUẤT, CHƯA VIẾT**

Một cửa duy nhất trả về quyền hiệu lực của một phiên, tám đường trên gọi vào đó:

```
quyenHieuLuc(phiên) →
   quản trị?              ← nhánh riêng, giữ nguyên như hiện nay
   menu[] · màn[]         ← hợp các quyền giữa các vai trò
   hànhĐộng[]             ← hợp các quyền
   phạmViDữLiệu           ← hợp các quyền
   trường[]               ← ⚠ CHỜ OWNER PHÂN XỬ (mục C.2)
```

Và **giữa các tầng dùng VÀ** (Owner I.7): tài khoản/phiên **VÀ** màn **VÀ** hành động **VÀ** dữ liệu **VÀ** trường.
**Không viết lại hàng loạt trong phiên này** — Owner đã khoá.

---

## D. TRUNG TÂM PHÂN QUYỀN — HÌNH DUNG SAU KHI SỬA

```
┌─ TRUNG TÂM PHÂN QUYỀN ─────────────────────────────────────────────┐
│                                                                     │
│  [ Vai trò ]  [ Tài khoản ]  [ Nhật ký ]                            │
│                                                                     │
│  Vai trò: ADMIN ▾          ⚠ Sửa vai trò này ảnh hưởng 3 tài khoản  │
│                                                                     │
│  ┌───────────────────────────────────────────────────────────────┐  │
│  │ ⛔ Tạm ngưng: Áp mẫu quyền nhanh                              │  │
│  │ Tạm ngưng để bổ sung bước xem trước thay đổi quyền.           │  │
│  │ Nút này ghi đè TOÀN BỘ quyền menu của vai trò chỉ bằng một    │  │
│  │ cú bấm, không cho xem trước mình sắp mất quyền nào.           │  │
│  │ Cách thay thế dùng ngay: tick từng menu rồi bấm Lưu.          │  │
│  └───────────────────────────────────────────────────────────────┘  │
│                                                                     │
│   Menu                       xem  thêm  sửa  xoá  xuất              │
│   ▾ Tài Chính                 ☑    ☐    ☐    ☐    ☐                 │
│       Phiếu thu               ☑    ☑    ☐    ☐    ☐                 │
│       Công nợ                 ☑    ☐    ☐    ☐    ☐                 │
│     Biểu Mẫu / Mẫu In         ☑    ☐    ☐    ☐    ☐                 │
│                                                                     │
│   ☑ = vai trò NÀY cấp    ☐ = vai trò NÀY không cấp                  │
│   ☐ KHÔNG phải "cấm" — vai trò khác của cùng người vẫn có thể cấp.  │
│                                                                     │
│                                        [ Huỷ ]  [ Lưu thay đổi ]    │
└─────────────────────────────────────────────────────────────────────┘
```

**Ba điểm bản trước thiếu, nay có:**
1. Câu giải nghĩa ô tick — chống hiểu nhầm bỏ tick là cấm (Owner I.6).
2. Cảnh báo **số tài khoản bị ảnh hưởng** khi sửa vai trò dùng chung (Owner mục VI).
3. Ô tạm ngưng nêu **lý do thật và đường thay thế**, không chỉ mờ nút.

---

## E. KHẢ NĂNG KHÔI PHỤC — ĐO ĐƯỢC, CÓ MỘT LỖ THẬT

### E.1 Bậc thang khôi phục hiện có

| Bậc | Cơ chế | Có thật? | Kích hoạt khi nào |
|---|---|---|---|
| 1 | Tài khoản mang vai trò quản trị | ✅ **3 tài khoản**, đều đang hoạt động, không bị khoá | luôn luôn |
| 2 | `BOOTSTRAP_ADMIN_EMAILS` (biến môi trường) | ✅ có mã, có đặt giá trị | **CHỈ khi hạ tầng RBAC hỏng** (`isRbacReady()` sai) |
| 3 | Vai trò `DEV` cứu hộ | ❌ **KHÔNG TỒN TẠI** — `dm_vai_tro` có 9 vai trò, không có `DEV` | — |

### E.2 🔴 Lỗ thật — ghi `DEBT-126`

| Đường | Có chốt "quản trị cuối cùng"? |
|---|---|
| Gỡ vai trò (`removeRoleFromUser`) | ✅ **CÓ** — chặn khi chỉ còn 1 |
| **Khoá tài khoản** (`setUserLockState`) | ❌ **KHÔNG CÓ CHỐT NÀO** — khoá xong thu hồi mọi phiên ngay |
| Xoá vai trò khỏi danh mục | — không có đường này trong mã |
| Xoá tài khoản | — không có đường này trong mã |

Cộng thêm lỗ thứ hai: `countAdminUsers()` đếm quản trị **không lọc trạng thái tài khoản**,
nên một quản trị **đã bị khoá vẫn được tính là còn**.

**Hai lỗ cộng lại:** khoá hết quản trị thì chốt chặn kia vẫn tưởng còn người, dẫn tới
**0 người khôi phục được**, mà bậc 2 **không kích hoạt** (bảng vẫn lành, chỉ dữ liệu sai).
Trái khoá Owner 27/08 mục I.10. **Hiện CHƯA xảy ra** — 3/3 quản trị đang dùng được.

### E.3 Lỗ nhận dạng — ghi `DEBT-128`

`user_role_mapping` nối bằng **email**, không có cột trỏ tới `user_account.id`.
Đổi email một tài khoản là **cắt đứt toàn bộ vai trò** của tài khoản đó.

---

## F. XÁC NHẬN CÁC ĐIỀU OWNER CẤM

| Điều Owner cấm | Trạng thái |
|---|---|
| Tạo kho quyền song song theo từng người | ✅ **KHÔNG có** — đã quét 6 tên gọi khả dĩ trong `src/` và `migrations/`, cả 6 **không tồn tại** |
| Tạo lược đồ mới / DDL | ✅ không đụng |
| Tự phát minh thứ tự cho/cấm | ✅ không — mâu thuẫn C.2 ghi nợ, chờ Owner |
| Hỏi Owner về tên bảng/cột/menu/route | ✅ không hỏi — tra bằng đo đạc |
| Triển khai | ✅ không triển khai |
| Đổi quyền đang có | ✅ **0 dòng thay đổi** — chứng minh bằng ảnh chụp bảng trước/sau |
