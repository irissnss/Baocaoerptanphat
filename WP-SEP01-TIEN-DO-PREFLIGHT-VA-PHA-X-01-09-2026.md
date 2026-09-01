# `WP-ERP-SEP01` — BÁO CÁO TIẾN ĐỘ: PREFLIGHT · ĐỊNH DANH PHÁT HÀNH · PHA X

> **Gói việc:** `WP-ERP-SEP01-CONVERGED-RELEASE-AND-OWNER-UAT` (Sổ Owner mục **#207**)
> **Kho mã:** `irissnss/erptanphat` · nhánh `main` · đã đẩy tới **`b1800c7`**
> **Ngày:** 01/09/2026
> **Trạng thái:** ⚙️ **ĐANG THỰC HIỆN** — xong Pha 0, Pha V, Pha VII, một phần Pha X
> **Máy vận hành:** vẫn `V1.00.366` — **CHƯA triển khai**

---

## 0. GỬI AGENT NOTION

Bốn câu, không hơn:

1. Owner ra quyết định **trực tiếp trong phiên code**, không qua Notion trước.
2. **Code hợp lệ đi trước tài liệu** — `GOV-SESSION-DECISION-001` §F1b mục 4.
3. Notion chưa kịp = **`SYNC_OVERDUE`**, là **nợ đồng bộ**, **KHÔNG phải sai**.
4. ⛔ Lệch nhau thì **tra sổ trước**, **cấm kết luận «code sai»**.

**Chỉ thị mới — mục #207:** Owner khoá 13 quyết định và **gỡ trạng thái CHỜ cho 9
mục** mà Agent từng xếp nhầm là chờ Owner. Ranh giới mới: những mục đó **không còn
chờ**; Agent phải tự xử theo hướng Owner đã chỉ.

---

## 1. PHA 0 — PREFLIGHT. CHỈ ĐỌC, KHÔNG GHI GÌ LÊN MÁY VẬN HÀNH

### 1.1 Bảng đối chiếu

| Đối tượng | Kỳ vọng | Máy nội bộ | `main` | Máy vận hành | Phán quyết |
|---|---|---|---|---|---|
| HEAD kho riêng | `4c51611` | `4c51611` | `4c51611` | — | ✅ **KHỚP** — báo cáo trước đúng |
| Cây làm việc | sạch | sạch | 0 ahead/0 behind | — | ✅ |
| Phiên bản | — | V1.00.366 | V1.00.366 | **V1.00.366** | 🔴 **ĐỤNG ĐỘ ĐỊNH DANH** |
| Mã nguồn triển khai | — | `4c51611` | — | `ef0e195` | ⚠️ lệch **11 commit · 42 tệp** |
| Vân tay bản dựng | — | — | — | `9af23488…` | ✅ có ghi trong bản kê |
| Tiến trình dịch vụ | chạy | — | — | **online 12h · 0 restart** | ✅ |
| Tuyến trọng yếu | lành | — | — | `/auth/login` **200** · các tuyến bảo vệ **307** | ✅ |
| MariaDB | 10.11 LTS | 10.11.10 | — | **10.11.10** | ✅ |
| Số bảng thật | **101** | — | — | **101** · 0 view | ✅ **khớp mốc Owner** |
| Quyền hành động | — | 66 | — | **66** | ✅ **0 lệch** |
| Tài khoản gieo `@example.invalid` | — | — | — | **0** | 🟢 xem mục 3 |
| Tài khoản thử | 0 | — | — | **0** | ✅ sạch |
| Quản trị đăng nhập được | ≥1 | — | — | **3** | ✅ |
| Sao lưu gần nhất | có | — | — | trước `V1.00.366` | ✅ |

### 1.2 Hai lượt đo đầu TRƯỢT — vì lỗi phép đo, không phải lỗi hệ thống

Ghi lại theo `GOV-FAILURE-RECORD-001` §G7.4:

| Lỗi | Hậu quả | Sửa |
|---|---|---|
| Gọi `127.0.0.1:3000` trong khi app nghe ở **`127.0.1.1:3000`** | Bốn tuyến trả `000` — trông như dịch vụ chết, **thật ra vẫn chạy** | Đo lại đúng địa chỉ → 200/307 |
| Nhiều tầng thoát ký tự (Node → ssh → bash) | Lệnh vỡ cú pháp, hai lượt liền | Gửi kịch bản qua **đầu vào chuẩn** (`ssh … 'bash -s' < tệp`) |

> **Bài học:** một phép đo sai làm hệ thống **trông như hỏng**. Nếu tin ngay lượt
> đo đầu thì đã đi «sửa» một dịch vụ hoàn toàn lành mạnh.

---

## 2. PHA V — ĐỤNG ĐỘ ĐỊNH DANH PHÁT HÀNH

### 2.1 Vấn đề, nói bằng số

Máy vận hành chạy **`V1.00.366`** từ mã nguồn `ef0e195`.
Kho riêng tư cũng khai **`V1.00.366`**, nhưng HEAD là `4c51611` — **cách nhau 11
commit và 42 tệp**.

⇒ **Cùng một số phiên bản đang chỉ vào HAI nội dung khác nhau.**

Triển khai lúc này thì bản kê sẽ ghi `APP_VERSION=V1.00.366` cho một nội dung khác
hẳn bản `V1.00.366` cũ. Từ đó về sau, câu hỏi *«máy vận hành đang chạy gì»* **mất
câu trả lời duy nhất**.

### 2.2 Cổng cũ không phủ được

`verify-release.mjs` kiểm **bậc tăng phiên bản** và **cây làm việc sạch** — không
kiểm bốn điều §V.4 đòi. Đó là khoảng trống thật.

### 2.3 Cổng mới `test:dinh-danh-phat-hanh`

Bốn điều kiện: cùng phiên bản khác mã nguồn · bản kê thiếu mã nguồn · vân tay lệch ·
khai bản mới mà tệp còn cũ.

**Tự kiểm 6/6** trên **dữ liệu giả trong bộ nhớ**, KHÔNG chạm máy vận hành — đúng
yêu cầu §XI *«negative controls phải chạy trong fixture/sandbox»*.

**Chạy thật — cổng bắt đúng đụng độ:**

```
FAIL  [R1] KHÔNG đụng độ định danh
      cùng «V1.00.366» nhưng mã nguồn khác nhau
      (kho 4c5161148 ≠ vận hành ef0e1956a)

⛔ CẤM TRIỂN KHAI cho tới khi tăng số phiên bản.
```

Số kế tiếp chưa dùng: **`V1.00.367`** (cao nhất đã dùng là 366; không có thẻ git).

---

## 3. PHA VII — `DEBT-021` RUNTIME: 🟢 `RUNTIME_NOT_EXPOSED`

Kiểm **chỉ đọc**, không cần Owner cung cấp mật khẩu, không in mã băm ra bất kỳ đâu.

| Câu hỏi | Kết quả |
|---|---|
| 14 tài khoản gieo có tồn tại trên máy vận hành? | **KHÔNG — 0 tài khoản `@example.invalid`** |
| Có phiên đăng nhập nào? | Không có tài khoản thì không có phiên |
| Cần containment? | **KHÔNG** |

⇒ Tệp gieo **chưa từng được chạy** trên máy vận hành. Phần mã nguồn đã dọn hôm nay
(commit `63c1245`); phần runtime **không có gì để dọn**.

---

## 4. PHA X — BA MỤC ĐÃ XỬ DỨT ĐIỂM

### 4.1 `DEBT-144` — lỗ quyền CÓ THẬT, đo trên dữ liệu thật

Bốn hàm ghi của `/m0/quy-trinh` hỏi khoá **module thô** `"m0"`. Khối 3 của
`action-permission.ts` duyệt **mọi khoá con của module** và chấp nhận nếu **bất kỳ**
khoá con nào bật cờ.

🔴 **Đo được:** `MENU_M0_PHONG_BAN` · vai trò `HR` · `can_create = 1` ·
`can_update = 1`
⇒ Người chỉ được cấp quyền **sửa Phòng Ban** lại **sửa được cả SƠ ĐỒ QUY TRÌNH** —
thứ quyết định luồng duyệt báo giá, đơn hàng, thiết kế.

**Đã siết** sang khoá **theo MÀN** `m0_quy_trinh`. Không viết cứng tên module, tên
vai trò hay tên màn — khoá đọc từ danh mục màn hình.

**Ba căn cứ chứng minh siết không khoá nhầm ai** — đã kiểm, không đoán:

1. Vai trò quản trị bỏ qua toàn bộ kiểm tra (`action-permission.ts:63`).
2. `khoaQuyenChoCongModule("m0_quy_trinh")` trả về **đúng một phần tử là chính nó** —
   vì tập `MODULE_DA_NOI_CONG_TUNG_MAN` chứa `"m0"` chứ **không** chứa
   `"m0_quy_trinh"` ⇒ khối 3 **không kích** cho khoá màn. **Đây là điều làm cho việc
   siết có tác dụng thật.**
3. Đo trên CSDL: **không vai trò thường nào** đang hợp lệ ghi Quy Trình ⇒ siết
   **không lấy mất quyền của ai đang dùng thật**.

**Cổng mới `test:quyen-so-do-quy-trinh` — 8/8.** Đọc **chính hàm quyết định quyền**
và **chính CSDL**, không mở trình duyệt.
**Kiểm ngược:** trả một hàm về khoá module thô → **6/2 đỏ**; khôi phục → **8/8**.

### 4.2 `DEBT-143` — TIỀN ĐỀ CỦA CHÍNH DÒNG NỢ NÀY LÀ SAI

Sổ nợ ghi tab «Hành động sau chuyển» là *«cấu hình chết — cấu hình xong KHÔNG chạy
gì»*.

**Đọc lại mã: KHÔNG ĐÚNG.**
`executePostActions` **được** `transitionState` gọi, và `transitionState` **được gọi
thật** từ màn Báo Giá và Đơn Hàng. Cả **ba** quy trình trong CSDL đều **có** cấu
hình. ⇒ Tab **CÓ chạy**. Không ẩn, không tắt.

🔴 **Nhưng sự thật ở tầng sâu hơn, và nguy hiểm hơn điều sổ nợ mô tả:**

| Loại hành động | Thực tế |
|---|---|
| `create_task` | ✅ **chạy thật** — tạo việc trong bảng `task` |
| `notify` | ❌ **khối rỗng** — `void` tham số rồi thoát êm |
| `send_email` | ❌ **khối rỗng** — `void` tham số rồi thoát êm |

Người quản trị cấu hình «gửi email khi duyệt báo giá» sẽ **tin là email đã gửi**.
Không ai báo lỗi, vì hàm **thoát êm**.
**Đây là loại hỏng tệ nhất: hỏng mà không kêu.**

**Đã vá:** hai loại đó mang nhãn **«— CHƯA HOẠT ĐỘNG»** ngay trên ô chọn, ở **cả
hai bản** của bộ dựng biểu mẫu. ⛔ Không tự xây dịch vụ thông báo/email — ngoài phạm
vi (§X.2, §XIV).

> **Bài học:** dòng nợ này ghi 29/08 dựa trên một lượt đọc **không đủ sâu** — dừng ở
> *«có ai gọi hàm không»* thay vì đi tiếp tới *«hàm đó thật sự làm gì»*. Kết luận
> **ngược hẳn**.

### 4.3 `DEBT-117` — Owner khoá D9 FINAL

`Decision D9` là **FINAL**. `Decision D7` gắn **SUPERSEDED** ở **đúng phần xung đột**,
không phải toàn bộ:

1. `flat_price` là **số cố định, KHÔNG nhân số kẽm**; tiền kẽm tính **riêng**.
2. In **hai mặt A-B** thì nhân đôi **số tờ**, `flat` vẫn **một lần**.

⛔ **Không đụng mã tính giá trong đợt này** — §XIV cấm mở pricing engine. Đây là
**phân xử tài liệu**; thi hành vào mã thuộc gói việc sau.

---

## 5. ĐÍNH CHÍNH BÁO CÁO TRƯỚC — SÁU ĐƠN TRÙNG

Báo cáo lượt hai ghi *«cả sáu đơn trùng là dữ liệu thử»*. **SAI.** Đo thật trên máy
vận hành:

| Báo giá | Đơn | Khách hàng |
|---|---|---|
| bg=1 | id 1 · 2 | `Sđsfdsfds` — rõ là thử |
| bg=5 | id 3 · 5 | **một khách mang tên công ty đầy đủ** |
| bg=10 | id 4 · 6 | **một khách mang tên công ty đầy đủ** |

**4/6 đơn không phải dữ liệu thử.** Việc xoá vì vậy **càng phải qua Decision Pack** —
đúng như §X.9 đã khoá. Agent **không xoá** trong đợt này.

---

## 6. SỐ LIỆU NỢ — ĐỌC TỪ SỔ, KHÔNG ĐẾM CHUỖI

| | Số |
|---|---|
| Tổng dòng nợ | **87** |
| **Còn mở / đang xử lý** | **33** |
| Đã xử lý | 46 |
| Không còn hợp lệ · hoãn có chủ đích | 8 |

Đóng thêm trong gói việc này: `DEBT-117` · `DEBT-143` · `DEBT-144`
(nợ mở **36 → 33**).

Số đọc bằng `npm run test:hinh-dang-so` — cổng **tự in**, không đếm tay.

---

## 7. BẢNG CỔNG KIỂM

| Cổng | Kết quả |
|---|---|
| `test:gov-gates` | **37/37 PASS** |
| `test:dinh-danh-phat-hanh:selftest` 🆕 | **6/6** |
| `test:dinh-danh-phat-hanh` (thật) | **3 PASS / 1 FAIL** — đỏ **đúng ý đồ**, chặn triển khai |
| `test:quyen-so-do-quy-trinh` 🆕 | **8/8** |
| `test:cong-mo-coi` | **3/3** |
| `test:hinh-dang-so` | **7/7** |
| `npx tsc --noEmit` | sạch |

---

## 8. CÒN LẠI CỦA GÓI VIỆC

| Pha | Nội dung | Trạng thái |
|---|---|---|
| **VI** | Tám cổng trình duyệt — `DEBT-168` | ⚙️ **đã đo, chưa xử** |
| **VIII** | Khoá gói phát hành hỗn hợp (8 nhóm A–H) | ⏳ chưa |
| **IX** | Nghiệm thu Permission UX — ba luồng | ⏳ chưa |
| **X** | Năm mục còn lại: `142` · `153` · `163` · `152` · `069`/`082` · sáu đơn | ⏳ chưa |
| **XI** | Bộ cổng đầy đủ trước triển khai | ⏳ chưa |
| **XII** | Sao lưu · diễn tập · triển khai · smoke | ⏳ chưa |
| **XIII** | Owner UAT trên runtime VPS | ⏳ chưa |

**Tám cổng trình duyệt — đã đo trên CSDL sạch, cả 8 hỏng.** Ba nguyên nhân đã nhận
diện:

1. Chờ thanh **«Năm bước»** — đã gỡ từ 29/08 (`anh-phan-quyen`, `anh-thanh-ben`).
2. **Viết cứng mã khách `#2477`** (`d3-anh`).
3. Chờ phần tử ảnh chụp của giao diện cũ (`anh-chuyen-trang-thai`,
   `anh-ma-tran-quyen`, `mobile-bar`, `ux-cong`, `khoi-van-hanh`).

---

## 9. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC

1. **Chưa triển khai.** Máy vận hành vẫn `V1.00.366` từ `ef0e195`. Hai commit của gói
   việc này (`b0f3fb4`, `b1800c7`) mới nằm ở kho.
2. **Permission UX chưa được Owner nghiệm thu** — và theo §XIII, nghiệm thu **phải
   trên runtime VPS đã triển khai**, không phải trên bản local.
3. **Tám cổng trình duyệt chưa có disposition** — chưa quyết viết lại hay gắn
   SUPERSEDED.
4. **Chưa có bản diễn tập trên bản sao khôi phục từ máy vận hành** (§XII.2).
5. **Chưa lập Decision Pack** cho `DEBT-163` (A/C/D/E), `DEBT-152` (mẫu in) và sáu
   đơn trùng.

---

## 10. KHỐI BÁO CÁO KẾT THÚC (`GOV-COMPLETION-REPORT-001` §G5)

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Pha 0: preflight đầy đủ local/main/VPS/DB — CHỈ ĐỌC
   - Pha V: dựng cổng định danh phát hành, chứng minh đụng độ hiện tại
   - Pha VII: DEBT-021 runtime = RUNTIME_NOT_EXPOSED
   - Pha X (một phần): DEBT-117 · DEBT-143 · DEBT-144
   - Đính chính báo cáo trước về sáu đơn trùng

2. PHẠM VI
   ĐỤNG    : src/app/m0/quy-trinh/actions.ts ·
             src/components/workflow/** (2 tệp) ·
             scripts/tests/kiem-dinh-danh-phat-hanh.mjs (mới) ·
             scripts/tests/kiem-quyen-so-do-quy-trinh.test.ts (mới) ·
             scripts/preflight-*.{mjs,sh} (mới) · package.json ·
             hai sổ quản trị
   KHÔNG ĐỤNG: CSDL máy vận hành (0 câu ghi) · lược đồ (0 DDL) ·
             triển khai (chưa đẩy) · số phiên bản (giữ V1.00.366) ·
             mã tính giá (§XIV cấm) · sáu đơn trùng ·
             cổng kiểm lỗi thời (chưa quyết)

3. BẰNG CHỨNG
   Kho mã irissnss/erptanphat, nhánh main — đã đẩy 01/09/2026,
   4c51611..b1800c7, KHÔNG force-push:
     b0f3fb4 — preflight + cổng định danh phát hành
     b1800c7 — siết quyền sơ đồ quy trình + 2 hành động rỗng
   npm run test:gov-gates                    → 37/37 PASS
   npm run test:dinh-danh-phat-hanh:selftest → 6/6
   npm run test:dinh-danh-phat-hanh          → bắt đúng đụng độ
   npm run test:quyen-so-do-quy-trinh        → 8/8, kiểm ngược 6/2
   npm run test:cong-mo-coi                  → 3/3
   npm run test:hinh-dang-so                 → 7/7
   npx tsc --noEmit                          → sạch
   Preflight máy vận hành: MariaDB 10.11.10 · 101 bảng · 66=66 quyền ·
     0 tài khoản @example.invalid · pm2 online 12h
   → lớp bằng chứng: CODE_PROVEN · DB_PROVEN · RUNTIME_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #207 (gói việc WP-ERP-SEP01, 13 quyết định khoá)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · nhánh main · commit 77a5b77 ·
       báo cáo lượt hai TONG-LUC-LUOT-HAI-01-09-2026-XU-LY-DUT-DIEM.md
   Báo cáo này (tệp thứ ba) đẩy ngay sau khi ghi khối này.
   Mã commit KHO MÃ NGUỒN nằm ở trường 3 — không để lẫn vào đây.
   KHÔNG force-push, KHÔNG viết lại lịch sử.

6. CÒN SÓT / CHƯA LÀM
   - Pha VI: tám cổng trình duyệt — đã đo, chưa xử
   - Pha VIII, IX, XI, XII, XIII — chưa bắt đầu
   - Pha X: còn DEBT-142 · 153 · 163 · 152 · 069/082 · sáu đơn
   - 33 nợ còn mở

7. ĐANG CHỜ OWNER
   - KHÔNG có mục nào chặn. Owner đã gỡ trạng thái chờ cho 9 mục ở #207
     và khoá "khi gates PASS thì tự deploy, không xin lại approval".
   - Owner UAT chỉ diễn ra SAU khi triển khai (§XIII) — chưa tới bước đó.

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Pha VI: lập bảng disposition 8/8 cho tám cổng trình duyệt, viết lại
   theo bất biến ngữ nghĩa hoặc gắn SUPERSEDED có bằng chứng thay thế.

9. CHƯA XÁC MINH ĐƯỢC
   - Nội dung candidate chạy đúng trên runtime VPS — chưa triển khai
   - Permission UX có đủ trực quan không — chỉ Owner xác minh, và phải
     trên runtime VPS theo §XIII
   - Bản diễn tập trên bản sao khôi phục — chưa làm

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — thiếu: Pha VI, VIII, IX, XI, XII, XIII.
       Điều kiện lên PASS: hoàn tất bộ cổng đầy đủ + triển khai + smoke.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén:
     - .governance/registry/tech-debt.md — toàn bộ 87 dòng nợ
     - docs/OWNER-REQUEST-LEDGER.md — các mục #194–#207
     - src/lib/action-permission.ts — trọn cơ chế ba khối quyết định quyền
     - src/lib/security/menu-catalog.ts — hàm khoaQuyenChoCongModule
     - src/lib/workflow-service.ts — trọn chuỗi executePostActions
     - scripts/verify-release.mjs — để biết cổng cũ phủ gì, tránh dựng trùng
   Không kết thúc bằng trí nhớ từ trước nén.
═══════════════════════════════════════════
```
