# BÁO CÁO TỔNG LỰC — LƯỢT HAI NGÀY 01/09/2026

> **Chỉ thị Owner (Sổ mục #206), nguyên văn:**
> *«Tiếp đi em **các vấn đề rõ ràng, xác định em tự chủ động xử lý dùm anh hết đi**
> và **báo cáo github tổng lực** nha em.»*
>
> **Kho mã:** `irissnss/erptanphat` (riêng tư) · nhánh `main` · đã đẩy tới `aad7af9`
> **Phiên bản:** `V1.00.366` — **chưa triển khai lên máy vận hành**
> **Báo cáo lượt một:** `TONG-LUC-PHIEN-01-09-2026-VA-GOI-BAN-GIAO-NOTION.md`

---

## 0. GỬI AGENT NOTION — ĐỌC TRƯỚC

Báo cáo lượt một đã nêu đủ cơ chế. Nhắc lại **bốn câu** để không phải tra lại:

1. Owner ra quyết định **trực tiếp trong phiên code**, không qua Notion trước.
2. Vì vậy **code hợp lệ đi trước tài liệu** — `GOV-SESSION-DECISION-001` §F1b mục 4.
3. Notion chưa kịp = **`SYNC_OVERDUE`**, là **nợ đồng bộ**, **KHÔNG phải sai**.
4. ⛔ Lệch nhau thì **tra sổ trước** (`docs/OWNER-REQUEST-LEDGER.md`), **cấm kết
   luận «code sai»**. Khớp một mục đã ghi → đồng bộ tài liệu, **không sửa ngược code**.

**Chỉ thị mới của lượt này — mục #206:** Owner **uỷ quyền mở rộng** cho Agent tự
quyết và tự làm với mọi nợ đã xác định rõ.
⚠️ **Ranh giới KHÔNG đổi:** 8 nợ ghi «CHỜ OWNER» **vẫn chờ**, vì chúng cần **quyết
định nghiệp vụ** (phân xử mâu thuẫn trong Notion · duyệt mẫu giấy · chốt kế hoạch
treo · duyệt đổi tên cơ sở dữ liệu), không phải cần thêm công sức kỹ thuật.
Uỷ quyền «xử lý việc đã rõ» **không** đồng nghĩa uỷ quyền «tự quyết thay Owner
việc chưa rõ» (`GOV-OWNER-AUTHORITY-001` §C1).

---

## 1. SỐ LIỆU — ĐO TRÊN SỔ THẬT

| | Số |
|---|---|
| Tổng dòng nợ trong sổ | **87** |
| **Đóng trong ngày 01/09** | **16** |
| Còn mở / đang xử lý | **36** |
| — trong đó **chờ Owner quyết** | **8** |
| — trong đó **Agent làm được** | **28** |

**16 nợ đóng hôm nay:** `147` · `149` · `150` · `151` · `145` · `162` · `103` ·
`165` · `166` · `021` · `108` · `131` · `140` · `110` · `109` · `167`.

> ⚠️ **Con số tồn đọng đã sai HAI LẦN trong cùng ngày** (báo 37 → sửa 42 → thật
> ra 41). Nguyên nhân tìm được ở mục 4.3, và **đã dựng cổng máy tự đếm** để không
> còn phải đếm tay.

---

## 2. NĂM NHÓM VIỆC ĐÃ LÀM

### 2.1 🔴 BẢO MẬT — hai nợ đỏ (commit `63c1245`)

**`DEBT-021` — mã băm mật khẩu trong tệp được git theo dõi.**
Một mã băm **dùng chung cho 14 tài khoản** nằm ngay trong `seed-golive-p01.sql`.
Bẻ được **một** là vào được **cả mười bốn**.

Vá **cả hai đầu** trong một lượt — chỉ gỡ tệp thì mai lại có tệp khác chép vào;
chỉ vá cổng thì tệp này vẫn bẩn:
- **Gỡ:** 14 mã băm → chỗ trống. Tệp gieo **cố ý không còn chạy thẳng được**;
  chạy qua `scripts/chay-seed-golive-p01.mjs`, đọc mật khẩu từ biến môi trường
  rồi tự băm. **Ba chốt chặn, đã thử cả ba**: thiếu biến → DỪNG · mật khẩu dưới
  12 ký tự → DỪNG · trỏ tới máy không phải nội bộ → DỪNG.
  Chạy thẳng bằng dòng lệnh MySQL sẽ ghi đúng chuỗi chỗ-trống vào cột mật khẩu ⇒
  **không ai đăng nhập được**. Hướng an toàn: **hỏng thì KHOÁ, không MỞ**.
- **Vá cổng:** thêm 3 luật bắt theo **hình dạng** (bcrypt · argon2 · crypt).

**Hai lần kiểm ngược mới ra cổng thật — bài học đắt:**

| Lần | Chuyện gì | Vì sao |
|---|---|---|
| **1** | Thêm luật xong, thả mã băm vào → cổng **VẪN PASS** | Mã băm **mở đầu bằng `$`** nên bộ lọc «biến môi trường» coi là vô hại; thân mã băm **chứa `.` và `/`** nên bộ lọc «có dấu chấm hoặc gạch» cũng nuốt. **Ba luật mới bị chính bộ lọc nuốt sạch.** |
| **2** | Cổng bật lên bắt **5 chỗ** | Cả 5 là **chuỗi giả cố ý** trong bài kiểm. Không miễn trừ cả tệp (miễn trừ tệp thì mã băm **thật** lọt vào tệp đó cũng qua) — lọc ở **mức giá trị**. |

Tiêu chí đầu («≥4 dấu chấm VÀ không chữ hoa») vẫn **bỏ lọt 2 chỗ**. Nay còn **một
tiêu chí, chắc hơn hai cái cũ cộng lại**: thân mã băm thật là 53 ký tự base64 lấy
từ bảng có **26 chữ HOA**, nên xác suất **không có chữ hoa nào** là
`(38/64)^53 ≈ 10⁻¹²`. **Không cần danh sách tên — danh sách thì luôn thiếu, xác
suất thì không.**

> **Nếu không kiểm ngược, đã đóng nợ bằng một cổng KHÔNG HOẠT ĐỘNG** — đúng thứ
> `GOV-GATE-REAL-INPUT-001` §G7.7 cấm.

**`DEBT-108` — địa chỉ máy chủ vận hành trong tệp được git theo dõi.**

Đo trước khi làm: quét **1 633 tệp**, phân loại địa chỉ theo **tính chất kỹ thuật**
chứ không đếm thô. `127.0.0.1` (234 lần) · `0.0.0.0` · `192.168.x` đều **riêng, vô
hại**; `1.2.3.4` · `123.45.67.89` là **ví dụ tài liệu**. Chỉ **hai** giá trị công
cộng thật: địa chỉ máy chủ (**21 lần**) và **địa chỉ công cộng máy Owner ở nhà**
(**2 lần** — sổ nợ gốc **không nêu**; nó chỉ ra vị trí và nhà mạng của Owner nên
thuộc `GOV-PII-HANDLING-001` §G7.13).

Thứ tự bắt buộc, làm đúng: **ghi vào sổ bí mật TRƯỚC, gỡ khỏi kho SAU**.
**23 lần trong 15 tệp → 0.**

**Phát hiện thêm, quan trọng hơn cả việc gỡ:** **6 kịch bản** dùng địa chỉ đó làm
**giá trị mặc định** — đúng câu `GOV-SECRET-IN-CODE-001` §G7.5 cấm. Đổi cả sáu
sang **DỪNG HẲN khi thiếu biến**, kèm câu chỉ rõ tra giá trị ở đâu.

Giá trị nay **chỉ còn ở hai nơi hợp lệ** (`.env.deploy` và sổ bí mật) — **đã chứng
minh bằng lệnh** `git check-ignore`, cả hai đều bị chặn.

### 2.2 🔴 SỔ VỠ BẢNG — nguyên nhân vật lý của điều Owner lo (commit `56d5bc9`)

`DEBT-103`: sổ Owner **vỡ hình dạng bảng Markdown**. Ô thứ tư trở đi **không hiển
thị** — nuốt mất `decision_state` và `notion_sync_state`, đúng hai trường dựng ra
**cho Agent Notion đọc**.

🔴 **Trong 9 hàng vỡ có hai dòng chính Agent vừa ghi sáng hôm đó** (`DEBT-150`,
`DEBT-151`) — lỗi này **vẫn đang tái sinh**, không phải chuyện cũ.

Đã nắn: 23 lần sửa, **0 còn lại**. Cổng mới `test:hinh-dang-so` — nay **7 điều
kiện**, và **tự in số tồn đọng** để không ai phải đếm tay.

### 2.3 🔴 80 CỔNG MỒ CÔI — nối lại và lộ ra 6 cổng hỏng thật (commit `78f83fe`)

Đo: **108 lệnh `test:*`**, chỉ **28** được bộ gộp gọi ⇒ **80 mồ côi**.

**Không gộp mù vào một bộ** — chúng cần những thứ khác nhau; gộp mù thì bộ **luôn
đỏ** trên máy thiếu điều kiện, và bộ luôn đỏ **sẽ bị bỏ qua**.

⇒ Chia **ba bộ theo điều kiện chạy**: `nhom-nhanh` **35** · `nhom-csdl` **22** ·
`nhom-trinh-duyet` **16**. Cộng cổng mới `test:cong-mo-coi` canh hai điều: mọi lệnh
phải thuộc một bộ, và mọi lệnh được gọi phải **tồn tại**.

**Chạy thật — phần đáng giá nhất:**

| Nhóm | Kết quả |
|---|---|
| `nhom-nhanh` | **34 đạt / 6 hỏng** → sau khi vá: **35/35, mã thoát 0** |
| `nhom-csdl` | **22/22 đạt** |
| `nhom-trinh-duyet` | 4 đạt / 12 hỏng — **con số này SAI**, xem 2.4 |

Sáu cổng hỏng của nhóm nhanh, phân loại và xử lý từng cái:
- **`test:danh-muc-man-hinh`** — trang `/m0/security/huong-dan` thêm **29/08** mà
  **quên khai** vào danh mục. Cổng hỏng suốt từ 29/08. ✅ Đã khai → **13/13**.
- **`test:h2-giao-viec` · `test:m1-multi-actor` · `test:m1-orphan`** — **không phải
  lỗi mã**: cần **6 tài khoản UAT** mà một lượt dọn nào đó đã xoá. Dựng
  `scripts/tao-tai-khoan-uat.mjs`. ⚠️ Lượt đầu chỉ khai 4 tài khoản đọc từ **một**
  bài kiểm ⇒ hai cổng vẫn đỏ; phải quét **toàn bộ** mới ra đủ 6. ✅ Cả ba đạt.
- **`test:import-reconcile` · `test:import-threshold`** — **không hỏng**: chúng
  **cố ý từ chối chạy** khi thiếu đầu vào, đúng hành vi `GOV-GATE-REAL-INPUT-001`
  đòi. Chuyển sang **miễn trừ có ghi lý do**.

### 2.4 🔴 CƠ CHẾ HỎNG DÂY CHUYỀN — phát hiện chưa ai biết (commit `aad7af9`)

Nhóm trình duyệt báo **12/16 hỏng**. Nhưng chạy **riêng** thì
`test:xac-nhan-hai-buoc` đạt **22/22** và `test:doi-chieu-khuon` cũng đạt.

**Chuỗi nhân quả đo được:**

```
(1) một cổng hỏng vì lý do riêng
 → (2) nó CHẾT TRƯỚC KHỐI DỌN, để lại một tài khoản …@kiemthu.local
 → (3) cổng kế tiếp so MỐC NỀN số tài khoản, thấy lệch → báo hỏng
 → (4) cổng đó cũng chết trước khi dọn, để lại thêm một tài khoản
 → (5) đổ dây chuyền
```

**Bằng chứng vật lý:** sau lượt chạy, cơ sở dữ liệu còn sót
`dokhuondc0a1402@kiemthu.local` — tiền tố `dokhuon` **đúng của
`test:doi-chieu-khuon`**, một trong những cổng hỏng **đầu tiên**.

> ⇒ **Một cổng hỏng THẬT kéo theo nhiều cổng hỏng GIẢ.** Đọc «12/16 hỏng» mà tin
> là 12 lỗi thật thì sẽ **đi sửa nhầm khoảng 10 chỗ không hỏng**.

Đã vá: `scripts/don-tai-khoan-thu.mjs` — dọn **phòng vệ ở ĐẦU** nhóm. Dọn cuối vẫn
giữ (nó đúng), nhưng **dọn cuối không chạy được khi tiến trình chết giữa chừng**.
Chỉ xoá đuôi `@kiemthu.local`; **không** đụng bộ UAT cố định, **không** đụng tài
khoản thật.

### 2.5 🟡 KIỂU DỮ LIỆU VÀ LINT (commit `b7480d0`)

**`DEBT-140` — kiểu sai, không phải bài kiểm sai.** `Omit<…>` biến **mọi** trường
còn lại thành bắt buộc (29 trường) trong khi hàm chỉ thật sự đòi **năm**. Người
đọc dễ kết luận nhầm là bài kiểm sai rồi đi sửa bài kiểm.
⚠️ **Sổ ghi «2 trường», mã thật kiểm «5»** — mã đã đổi từ 29/08. Khai theo **mã
đang chạy**, không theo con số trong sổ; tin sổ thì kiểu mới thiếu 3 trường bắt
buộc và lỗi lại lọt.

**`DEBT-110` — 6 lỗi lint → 0.** Ba lỗi `set-state-in-effect` gốc là ba effect cùng
theo dõi `congNghe`, mỗi cái tự dọn state phụ thuộc bằng `setState` gọi thẳng
trong thân effect ⇒ **kết xuất dây chuyền**. Dồn việc dọn về **đúng nơi sự việc
xảy ra** — làm được trọn vẹn vì `setCongNghe` chỉ gọi ở **hai chỗ**. **Hành vi
người dùng thấy y hệt như cũ.**

**`DEBT-109` — không còn hợp lệ.** Nó sinh ra **chỉ để chặn** việc đóng `DEBT-016`
cho tới khi Owner xác nhận. Owner đã xác nhận 25/08 và `DEBT-016` đã đóng.

---

## 3. NHỮNG ĐIỀU AGENT LÀM SAI — GHI TRUNG THỰC

`GOV-FAILURE-RECORD-001` §G7.4 bắt ghi đúng, không làm mềm.

| # | Sai gì | Hậu quả thật | Đã xử lý |
|---|---|---|---|
| 1 | **Chạy thẳng kịch bản triển khai** để thử chốt chặn | Biến môi trường có sẵn nên chốt không kích hoạt — **kịch bản triển khai thật bắt đầu chạy**, tới bước tạo tệp nén 24 MB | **Máy vận hành KHÔNG bị đụng** (bước tải lên là bước 2, chưa tới). Tệp nén đã bị `.gitignore` chặn sẵn và đã xoá. **Bài học:** thử chốt chặn phải **gỡ biến ra**, tuyệt đối không chạy thẳng kịch bản triển khai để «xem nó báo gì» |
| 2 | Thêm 3 luật bắt mã băm **mà không kiểm ngược ngay** | Ba luật bị chính bộ lọc vô hại nuốt sạch — cổng vẫn PASS | Phát hiện nhờ kiểm ngược; đã sửa |
| 3 | Đếm tồn đọng bằng **tìm chuỗi con** | Nhận nhầm dòng **đã đóng** thành **còn mở**; con số báo Owner **sai hai lần trong cùng một ngày** | Đọc trạng thái theo **chữ đầu** của cột; cổng nay **tự in số** |
| 4 | Kịch bản tạo tài khoản UAT chỉ đọc **một** bài kiểm | Thiếu 2 tài khoản ⇒ hai cổng vẫn đỏ | Quét **toàn bộ** bài kiểm; lệnh quét ghi thẳng vào chú thích để lần sau không sót |
| 5 | Đặt tệp kiểm ngược kiểu trong `scripts/` | `tsconfig` **loại** thư mục đó ⇒ tệp **không được kiểm**, tưởng là đạt | Chuyển vào `src/`, ra đúng một lỗi mong đợi |
| 6 | Dùng **dấu huyền ngược** trong chuỗi mẫu — **đúng cái bẫy đã tự ghi cảnh báo ở đầu tệp** | Vỡ cú pháp | Đã bỏ |
| 7 | Ghi hai dòng sổ nợ có **dấu ngăn cột chưa thoát** | Hai dòng đó **vỡ bảng ngay trong ngày** | Cổng `test:hinh-dang-so` nay chặn |

---

## 4. TỒN ĐỌNG — 36 NỢ, GOM ĐỦ

### 4.1 CHỜ OWNER QUYẾT — 8 nợ, Agent **không** tự làm

| Nợ | Owner cần quyết gì |
|---|---|
| `DEBT-163` | **Duyệt đề xuất nhóm A/C/D/E** của việc đặt tên cơ sở dữ liệu |
| `DEBT-152` | **Duyệt mẫu giấy** bản in báo giá (hiện in ra chỉ ghi mã khách, không có thông tin công ty) |
| `DEBT-153` | **Kế hoạch Phase 1 treo từ 14/06/2026** — 9 bài nghiệm thu + 4 câu hỏi chưa trả lời. Đọc và chốt, hay huỷ |
| `DEBT-143` | Tab «Hành động sau chuyển» là **cấu hình chết** — bỏ tab, hay làm cho nó chạy thật |
| `DEBT-144` | Quyền sửa sơ đồ quy trình canh ở mức module — chấp nhận, hay siết theo màn |
| `DEBT-142` | Quyền chuyển bước cấp cho vai trò **không có người mang** — gán người, hay bỏ quyền |
| `DEBT-069` · `DEBT-082` | Hai nợ quản trị cũ — xác nhận để đóng |

### 4.2 AGENT LÀM ĐƯỢC — 28 nợ

**Cụm tính giá (7)** — `112` · `113` · `114` · `115` · `111` · `118` · `117`:
đã **tìm được công thức khổ trải thật của Tân Phát**, và nó chứng minh **mô hình
hiện tại không đủ** — một công thức đang dùng chung cho **40 nhóm sản phẩm**, trong
đó có **~20 kiểu dáng hộp khác nhau về hình học**. Màn cấu hình khổ trải **đã tồn
tại đầy đủ nhưng chưa được nối** vào đường tính giá.
*(`DEBT-117` cần Owner phân xử mâu thuẫn D7↔D9 trong Notion trước khi làm.)*

**Nhóm chất lượng (7)** — `133` · `134` · `135` · `139` · `155` · `158` · `168`
**Nhóm giao diện và quyền (10)** — `066-B` · `067-B` · `068` · `093` · `094` ·
`096` · `098` · `099` · `100` · `104`
**Nhóm nhỏ (4)** — `116` (cổng không quét họ tên) · `121` · `154` · `164`

### 4.3 Ba nợ **sinh mới** trong ngày, đã đóng ngay hoặc ghi rõ

- `DEBT-165` · `DEBT-166` · `DEBT-167` — **đóng ngay trong cùng phiên**
- `DEBT-164` · `DEBT-168` — còn mở, ghi rõ việc cần làm

---

## 5. CHƯA CHỨNG MINH ĐƯỢC — BẮT BUỘC NÊU

1. ~~Nhóm cổng trình duyệt chưa có con số thật.~~ ✅ **ĐÃ ĐO XONG** — bổ sung sau
   khi viết mục này, xem **mục 9** bên dưới. Con số thật: **2 đạt / 10 hỏng**, và
   lỗi dây chuyền **chỉ giải thích được 2 trong 12 ca**. Ba cổng đã vá; **8 cổng
   còn lại** cần quyết định, ghi rõ ở mục 9.
2. **Hai cổng chắc chắn LỖI THỜI** — `test:anh-phan-quyen` và `test:anh-thanh-ben`
   chờ phần tử `nav ol li button`, tức thanh **«Năm bước»** của màn phân quyền, đã
   **gỡ từ 29/08**. Chúng kiểm một giao diện **không còn tồn tại** và đã hỏng liên
   tục **hơn ba ngày**. **Agent không tự xoá cổng kiểm** — xoá cổng là giảm mức
   canh, phải khai rõ và cần quyết định.
3. **14 tài khoản của tệp gieo có đang sống trên máy vận hành hay không** — chưa
   kiểm, và **không tự kiểm** vì đụng dữ liệu vận hành. Nếu đang sống thì mật khẩu
   cũ **vẫn còn hiệu lực** cho tới khi đổi.
4. **Màn `/m0/security` chưa được Owner nghiệm thu** — lượt làm lại **thứ năm**.
5. **Chưa triển khai lên máy vận hành.** Máy vận hành vẫn `V1.00.366`; bốn commit
   của lượt này mới nằm ở kho.
6. **10 cảnh báo lint** còn lại trên màn tính giá — không đụng, vì §E1 cấm tiện tay
   sửa ngoài phạm vi và Owner chưa chốt việc dọn cảnh báo.

---

## 6. BẢNG CỔNG KIỂM

| Cổng | Kết quả |
|---|---|
| `test:gov-gates` | **37/37 PASS** |
| `test:nhom-nhanh` | **35/35**, mã thoát 0 |
| `test:nhom-csdl` | **22/22** |
| `test:nhom-trinh-duyet` | đang đo lại — xem mục 5 |
| `test:cong-mo-coi` 🆕 | **2/2** |
| `test:hinh-dang-so` | **7/7** (thêm điều kiện trạng thái + tự in số tồn đọng) |
| `test:secret-scan` | PASS — nay bắt được **bcrypt · argon2 · crypt** |
| `npx eslint` màn tính giá | 6 lỗi → **0** |
| `npx tsc --noEmit` | sạch |

**Bốn cổng mới trong ngày, đều đã kiểm ngược hai chiều:** `test:xac-nhan-hai-buoc`
(22/22) · `test:hinh-dang-so` · `test:cau-noi-quyen` · `test:cong-mo-coi`.

---

## 7. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC

**Owner nghiệm thu màn `/m0/security`.** Mở `http://localhost:3000/m0/security` →
«Quản Lý Nâng Cao» → chọn vai trò **không phải quản trị** → tick thử một ô → xem
thanh nhắc vàng và hộp thoại xác nhận hai bước.

Đạt thì triển khai bốn commit lên máy vận hành. Chưa đạt thì Owner nói rõ **điểm
nào chưa được** — đây là lượt thứ năm, `GOV-ITERATION-LIMIT-001` §G7.3 đòi **đổi
bản chất cách làm**, không chỉnh tham số.

---

*Báo cáo công khai, đã qua cổng an toàn: không mật khẩu, không khoá, không mã băm,
không dữ liệu cá nhân, không địa chỉ máy chủ. Số liệu kỹ thuật (tên bảng · mã màn ·
số dòng) là loại được phép theo `GOV-PUBLIC-SAFE-001` §J1.*

---

## 8. KHỐI BÁO CÁO KẾT THÚC (`GOV-COMPLETION-REPORT-001` §G5 — 11 trường)

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - Đóng 16 nợ trong ngày: DEBT-147·149·150·151·145·162·103·165·166
     ·021·108·131·140·110·109·167
   - Bảo mật: gỡ 14 mã băm bcrypt khỏi tệp gieo + 23 lần địa chỉ máy chủ
     khỏi 15 tệp; 6 kịch bản đổi từ "giá trị mặc định" sang "dừng hẳn"
   - Nối lại 80 cổng mồ côi thành 3 bộ theo điều kiện chạy
   - Bắt được cơ chế HỎNG DÂY CHUYỀN của nhóm cổng trình duyệt
   - Vá kiểu CreateHrNhanVienInput và 6 lỗi lint
   - Dựng 4 cổng mới, tất cả đã kiểm ngược hai chiều

2. PHẠM VI
   ĐỤNG    : scripts/** · src/lib/m1-3-store.ts ·
             src/lib/security/menu-catalog.ts ·
             src/app/m3/tinh-gia-manual/** · package.json ·
             hai sổ quản trị · SO-BI-MAT-NOI-BO.md (gitignore)
   KHÔNG ĐỤNG: máy vận hành (0 câu ghi) · lược đồ (0 DDL) ·
             triển khai (chưa đẩy) · số phiên bản (giữ V1.00.366) ·
             8 nợ đang chờ Owner quyết ·
             mật khẩu và khoá (thuộc quyền Owner) ·
             cổng kiểm lỗi thời (không tự xoá — xoá cổng là giảm mức canh)

3. BẰNG CHỨNG
   Kho mã irissnss/erptanphat, nhánh main — đã đẩy 01/09/2026:
     63c1245 — gỡ mã băm + địa chỉ máy chủ (DEBT-021, DEBT-108)
     78f83fe — nối lại 80 cổng mồ côi (DEBT-131)
     b7480d0 — vá kiểu nhân sự + 6 lỗi lint (DEBT-140, 110, 109)
     aad7af9 — cơ chế hỏng dây chuyền (DEBT-167)
   npm run test:gov-gates          → 37/37 PASS
   npm run test:nhom-nhanh         → 35/35, mã thoát 0
   nhóm CSDL chạy rời 22 cổng      → 22/22 ĐẠT
   npm run test:cong-mo-coi        → 2/2
   npm run test:hinh-dang-so       → 7/7
   npm run test:secret-scan        → PASS, nay bắt bcrypt/argon2/crypt
   npx eslint màn tính giá         → 6 lỗi → 0
   npx tsc --noEmit                → sạch
   git check-ignore .env.deploy SO-BI-MAT-NOI-BO.md → cả hai BỊ CHẶN
   → lớp bằng chứng: CODE_PROVEN · DB_PROVEN · FILE_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #206 (chỉ thị uỷ quyền tự xử lý)
   Ghi kèm ranh giới: uỷ quyền "xử lý việc đã rõ" KHÔNG đồng nghĩa uỷ
   quyền "tự quyết thay Owner việc chưa rõ" (GOV-OWNER-AUTHORITY-001 §C1).

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · nhánh main · commit f851489 ·
       báo cáo lượt một TONG-LUC-PHIEN-01-09-2026-VA-GOI-BAN-GIAO-NOTION.md
   Báo cáo lượt hai (tệp này) đẩy ngay sau khi ghi khối này.
   KHÔNG force-push, KHÔNG viết lại lịch sử.

6. CÒN SÓT / CHƯA LÀM
   - 36 nợ còn mở: 8 chờ Owner · 28 Agent làm được (liệt kê ở mục 4)
   - Nhóm cổng trình duyệt chưa có con số thật (DEBT-168)
   - Hai cổng lỗi thời chưa xử lý — cần quyết định viết lại hay hạ nhãn
   - Chưa triển khai lên máy vận hành
   - 10 cảnh báo lint trên màn tính giá

7. ĐANG CHỜ OWNER
   - Nghiệm thu màn /m0/security → chặn việc triển khai
   - Duyệt đề xuất A/C/D/E của DEBT-163 → chặn dọn tên cơ sở dữ liệu
   - Phân xử DEBT-117 (D7 vs D9 trong Notion) → chặn cụm tính giá
   - Duyệt mẫu giấy DEBT-152 → chặn bản in báo giá
   - Chốt hoặc huỷ kế hoạch treo DEBT-153
   - Ba nợ quyền: DEBT-142 · 143 · 144
   - Xoá 6 đơn trùng trên máy vận hành (đều là dữ liệu thử)

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner nghiệm thu màn /m0/security: mở khu Quản Lý Nâng Cao, chọn vai
   trò KHÔNG phải quản trị, tick thử một ô, xem thanh nhắc và hộp thoại
   xác nhận hai bước.

9. CHƯA XÁC MINH ĐƯỢC
   - Con số thật của nhóm cổng trình duyệt — lượt đo lại đang chạy
   - 14 tài khoản của tệp gieo có đang sống trên máy vận hành không —
     không tự kiểm vì đụng dữ liệu vận hành
   - Màn phân quyền đã đủ trực quan chưa — chỉ Owner xác minh được
   - Trang Notion nào cần sửa — Agent IDE không có quyền đọc Notion

10. TRẠNG THÁI CHUNG
   [x] PROVISIONAL — thiếu: con số thật nhóm trình duyệt (DEBT-168) và
       Owner nghiệm thu màn phân quyền.
       Điều kiện lên PASS: đóng DEBT-168 + Owner xác nhận màn đạt.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phiên có bị nén ngữ cảnh không: CÓ
   Đã đọc lại sau nén:
     - docs/UI-STANDARD.md — TOÀN PHẦN, dòng 1–521
     - .governance/registry/tech-debt.md — toàn bộ 87 dòng nợ
     - docs/OWNER-REQUEST-LEDGER.md — các mục #194–#206
     - src/lib/security/menu-catalog.ts · src/lib/menu-registry.ts
     - scripts/tests/secret-scan-gate.test.mjs — đọc trọn bộ luật và
       bộ lọc vô hại trước khi thêm luật mới
     - package.json — toàn bộ 108 lệnh test:*
   Không kết thúc bằng trí nhớ từ trước nén.
═══════════════════════════════════════════
```

---

## 9. BỔ SUNG SAU KHI ĐO XONG NHÓM TRÌNH DUYỆT — `DEBT-168`

Viết thêm sau khi lượt đo nền hoàn tất (commit `4c51611`).

**Con số thật, đo trên cơ sở dữ liệu sạch** (dọn phòng vệ trước **mỗi** cổng, để
tách lỗi thật khỏi lỗi dây chuyền): **2 đạt / 10 hỏng**.

⇒ Lỗi dây chuyền **chỉ giải thích được 2 trong 12 ca**. Con số ban đầu «12 hỏng»
gần đúng, **nhưng vì lý do khác hẳn** — và nếu không đo lại thì đã đi sửa nhầm
hướng.

### 9.1 Nguyên nhân chung — không phải 10 lỗi riêng lẻ

Gần hết số đó **kiểm giao diện CŨ của màn phân quyền**. Màn này bị làm lại **hai
lần trong bốn ngày**: 29/08 (năm bước → ba pha) và 01/09 (ba pha → hai cột + xác
nhận hai bước). Các cổng vẫn bám hình dạng cũ.

### 9.2 Ba loại hỏng khác nhau — đã vá cả ba

| Loại | Ca cụ thể | Vì sao nguy hiểm |
|---|---|---|
| **(a) Bám số phiên bản viết cứng** | `d3-khoi-van-hanh` đòi `V1.00.361`, máy vận hành chạy `V1.00.366` | **Cổng đỏ mà không có gì hỏng cả.** Hỏng theo **thời gian**, không theo **mã** — không ai đụng vào mà cổng vẫn đỏ. Đây chính là một lý do khiến 80 cổng thành mồ côi: đỏ vô cớ thì người ta gỡ khỏi bộ gộp rồi quên luôn |
| **(b) Bám nhãn chữ chính xác** | `ux-huong-dan` báo *«màn phân quyền MẤT lối vào trang hướng dẫn»* | **SAI SỰ THẬT** — lối vào vẫn còn, chỉ đổi nhãn và đổi chỗ ngày 31/08. Báo động giả **nguy hiểm hơn cả bỏ sót**: nó khiến người đọc tưởng có lỗi mới và đi sửa thứ không hỏng |
| **(c) Bám hình dạng bố cục** | Cùng cổng đó đòi **dải ba bước**, đã gỡ có chủ đích 31/08, thay bằng một dòng gọn | Cổng canh **hình dạng** thay vì canh **điều cần canh** |

**Vá gốc cho loại (a):** thêm **điều kiện thứ ba** vào `test:cong-mo-coi` — quét
**110 tệp bài kiểm**, **cấm viết cứng số phiên bản** ngoài chú thích. Miễn trừ duy
nhất là `version-policy.test.mjs` (dựng dự án giả để kiểm chính sách đánh số — số ở
đó là **dữ liệu đầu vào**, không phải kỳ vọng về hệ thống), **có ghi lý do**.
Kiểm ngược: thả `V1.00.999` vào một tệp → cổng **đỏ**, chỉ đúng tệp và dòng; gỡ ra
→ **3/3**.

**Nguyên tắc chung cho (b) và (c):** cổng phải canh **ĐIỀU CẦN CANH**, không canh
**HÌNH DẠNG CỤ THỂ**. Canh «có đường tới trang hướng dẫn không» thay vì «nhãn có
đúng bốn chữ đó không». Canh «màn có nói trình tự làm việc không» thay vì «có đúng
cái dải ba ô đó không».
**Bám chữ và bám hình dạng là cách chắc chắn nhất để cổng hỏng ở lần đổi giao diện
kế tiếp.** `test:ux-huong-dan` nay **21/21**.

### 9.3 Còn lại — 8 cổng, CẦN QUYẾT ĐỊNH

`anh-chuyen-trang-thai` · `anh-ma-tran-quyen` · `anh-phan-quyen` · `anh-thanh-ben` ·
`d3-anh` · `khoi-van-hanh` · `mobile-bar` · `ux-cong`.

**Hai cổng chắc chắn LỖI THỜI:** `anh-phan-quyen` và `anh-thanh-ben` chờ phần tử
`nav ol li button` — thanh **«Năm bước»**, đã gỡ từ 29/08.

⚠️ **Agent KHÔNG tự xoá cổng kiểm — xoá cổng là giảm mức canh.** Hai hướng, cần
Owner hoặc phiên sau quyết:
- **(a)** viết lại chúng cho giao diện mới, hoặc
- **(b)** gắn nhãn SUPERSEDED và nêu rõ cổng thay thế — ứng viên:
  `test:xac-nhan-hai-buoc` (**22/22**) và `test:doi-chieu-khuon`, cả hai đều đo màn
  phân quyền **mới** và cả hai đều **ĐẠT**.

