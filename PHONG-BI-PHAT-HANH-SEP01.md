# PHONG BÌ PHÁT HÀNH — `WP-ERP-SEP01` §VIII

> **Mục đích:** khoá lại **chính xác cái gì sẽ được đưa lên máy vận hành**, để không có thứ gì
> đi kèm mà không ai biết. Mỗi nhóm phải trả lời đủ: đụng tệp nào · đổi hành vi gì · có đụng cơ sở
> dữ liệu không · có cần di trú không · bài kiểm nào canh · hoàn tác thế nào · **có vào đợt này không**.
>
> **Ngày lập:** 02/09/2026 · **Máy vận hành đang chạy:** `V1.00.366` (`<mã-nguồn-riêng>`)
> **Bản ứng viên:** `<mã-nguồn-riêng>` · **Chênh lệch: 17 commit · 63 tệp**

---

## 0. HAI CÂU TRẢ LỜI QUAN TRỌNG NHẤT — ĐO TRƯỚC, KHÔNG ĐOÁN

| Câu hỏi | Đo được | Ý nghĩa |
|---|---|---|
| Đợt này có **đổi cấu trúc cơ sở dữ liệu** không? | `migrations/` = **0 tệp** · câu `ALTER`/`CREATE`/`DROP TABLE` trong delta = **0 dòng** | **KHÔNG.** Thuần mã nguồn. Không cần di trú, không cần dừng dịch vụ để đổi bảng |
| Đợt này có **đổi thư viện phụ thuộc** không? | `package-lock.json` = **0 tệp đổi** · phần `dependencies` = **0 dòng đổi** | **KHÔNG.** Bản dựng dùng đúng bộ thư viện đang chạy |

> **Vì sao hai câu này đứng đầu:** chúng quyết định đợt phát hành **dễ hay khó hoàn tác**.
> Không đụng cấu trúc dữ liệu và không đổi thư viện ⇒ hoàn tác = **quay về mã cũ**, dữ liệu
> không cần đụng tới. Đây là loại đợt phát hành **an toàn nhất**.

---

## 1. PHÂN NHÓM — 63 TỆP

### 1.1 Vào bản dựng (chạy trên máy vận hành): **14 tệp**

| Nhóm | Tệp | Đổi hành vi gì | Đụng CSDL | Bài kiểm canh | Vào đợt này |
|---|---|---|---|---|---|
| **A. Màn phân quyền — làm lại theo yêu cầu Owner** | `m0/security/trung-tam-phan-quyen.tsx` · `ma-tran-menu-cay.tsx` · `ma-tran-chuyen-trang-thai.tsx` · **MỚI** `components/security/hop-thoai-xac-nhan-quyen.tsx` · **MỚI** `lib/security/mang-nghiep-vu.ts` | ⚠️ **ĐỔI HÀNH VI GHI — đây là thay đổi lớn nhất đợt này.** Trước: **tick một cái là ghi ngay vào CSDL**, không Save, không xem trước. Nay: tick chỉ ghi vào **bản nháp**; phải bấm «Xem Lại & Lưu» → hộp thoại liệt kê quyền **TRƯỚC và SAU** → xác nhận **hai bước** mới ghi | Ghi vào bảng quyền **khi người dùng bấm xác nhận** — không đổi cấu trúc | `test:xac-nhan-hai-buoc` **22/22** — đo **số dòng CSDL trước/sau từng bước**, chứng minh tick **không** ghi (148→148, 67→67) | ✅ **CÓ** |
| **B. Siết quyền màn sơ đồ quy trình** | `m0/quy-trinh/actions.ts` | Bốn hàm đổi từ kiểm quyền `"m0"` (quyền cha, rộng) sang `"m0_quy_trinh"` (quyền đúng màn) | Không | `test:quyen-so-do-quy-trinh` **8/8** | 🔴 **CÓ — BẮT BUỘC.** Đây là **lỗ hổng đang mở trên máy vận hành** (`DEBT-144`) |
| **C. Chặn đơn hàng trùng** | `lib/m3-store.ts` | ⚠️ **ĐỔI HÀNH VI GHI.** Trước: bấm lại nút «tạo đơn» từ cùng báo giá là sinh thêm đơn, **không cảnh báo**. Nay: chặn ở **tầng lưu trữ**, báo lỗi kèm mã đơn đã có | Thêm **một câu đọc** trước khi ghi. Không đổi cấu trúc | `test:m3-don-hang-hoan-thien` **31/31** | ✅ **CÓ** — xem cảnh báo ở mục 2 |
| **D. Sửa nhãn kỹ thuật trên màn quy trình** | `m0/quy-trinh/quy-trinh-client.tsx` | Gỡ mã `WF_*` khỏi bảng (vẫn tra được ở ô tìm kiếm, hộp thoại nhật ký và form sửa) | Không | `test:ux-do` | ✅ **CÓ** |
| **E. Vá màn đơn hàng** | `m3/don-hang/don-hang-client.tsx` | Bốn nút mở panel **không sửa được gì** nay nói rõ là chế độ xem · thẻ «đã thu» **không còn hiện số 0 giả** (cột `da_thanh_toan` là **số chết**, chưa nối công nợ) | Không | `test:m3-don-hang-hoan-thien` | ✅ **CÓ** |
| **F. Đánh dấu hai hành động rỗng** | `components/workflow/workflow-form-builder.tsx` ×2 | Hai lựa chọn `notify` · `send_email` thêm nhãn **«— CHƯA HOẠT ĐỘNG»** | Không | — | ✅ **CÓ** — là **che chắn**, không phải vá gốc (`DEBT-143` vẫn mở) |
| **G. Vá kiểu dữ liệu + lỗi lint** | `lib/m1-3-store.ts` · `m3/tinh-gia-manual/tinh-gia-manual-client.tsx` | Sửa kiểu sai · gom 3 `useEffect` thành 1 hàm · `any`→`unknown` · đổi dấu nháy trong JSX | Không | `npm run lint` sạch | ✅ **CÓ** |
| **H. Danh mục màn hình** | `lib/security/menu-catalog.ts` | Thêm khoá màn con còn thiếu | Không | `test:danh-muc-man-hinh` | ✅ **CÓ** |

### 1.2 KHÔNG vào bản dựng: **49 tệp**

| Loại | Số tệp | Vì sao không ảnh hưởng máy vận hành |
|---|---|---|
| Bài kiểm (`scripts/tests/`) | 21 | Chỉ chạy trên máy phát triển, không được đóng gói |
| Kịch bản chạy tay (`scripts/`) | 15 | Chạy tay khi cần, không nằm trong vòng đời trang web |
| Tài liệu (`docs/`) | 11 | Văn bản |
| Sổ nợ · sổ Owner · nhật ký | 2 | Văn bản |

> ⚠️ **`package.json` là ngoại lệ đáng nói:** nó **có** trong bản dựng, nhưng thay đổi **chỉ là thêm
> lệnh chạy bài kiểm** — đã kiểm bằng lệnh: phần `dependencies` **0 dòng đổi**, `package-lock.json`
> **0 tệp đổi**. Nên nó **không** làm đổi thứ gì trên máy vận hành.

---

## 2. HAI ĐIỀU PHẢI BÁO OWNER TRƯỚC KHI TRIỂN KHAI

### ⚠️ (1) Chặn đơn trùng sẽ **chặn thật** ngay khi lên

Máy vận hành **đang có 6 đơn sinh từ 3 báo giá** (mỗi báo giá bị bấm hai lần). Sau khi triển khai,
**ba báo giá đó không tạo được đơn mới nữa** — vì hệ thống thấy chúng đã có đơn.

- Đây là **đúng thiết kế**, không phải lỗi.
- Cả 6 đơn đó là **dữ liệu thử** (khách gõ bừa, sản phẩm tên «Test…») nên **chưa có thiệt hại thật**.
- ❓ **Điều cần Owner xác nhận:** trong nghiệp vụ thật, **có khi nào một báo giá hợp lệ sinh ra
  hai đơn hàng không?** (ví dụ khách chốt 1000 cái nhưng tách làm hai đợt giao, mỗi đợt một đơn).
  - Nếu **KHÔNG bao giờ** → chặn cứng như hiện tại là đúng.
  - Nếu **CÓ** → cần thêm đường cho phép, và hiện tại **chưa có**.
- ⛔ **Sáu đơn trùng đó em KHÔNG xoá** — theo khoá của anh, việc xoá cần một bản đề xuất riêng
  và anh duyệt riêng.

### ⚠️ (2) Màn phân quyền đổi cách làm việc — cần anh nghiệm thu trước

Trước đây tick là **ghi ngay**. Nay phải **bấm Lưu → xem lại → xác nhận**. Đây chính là điều anh
yêu cầu, nhưng nó **đổi thói quen thao tác**, nên phải để anh **thử trên máy thật** trước khi coi là xong
(§XIII — nghiệm thu của Owner **chỉ tính trên máy vận hành**, không tính trên máy phát triển).

---

## 3. HOÀN TÁC — NẾU CÓ CHUYỆN

| Tình huống | Cách xử lý | Mất gì |
|---|---|---|
| Bản mới lỗi, cần quay về ngay | Đưa mã về `<mã-nguồn-riêng>`, dựng lại, khởi động lại dịch vụ | **Không mất dữ liệu nào** — vì đợt này **không đụng cấu trúc CSDL** |
| Chỉ một nhóm hỏng | Không tách được — 14 tệp đi cùng một bản dựng | Phải quay về trọn bản |
| Dữ liệu bị ghi sai bởi màn phân quyền mới | Bảng quyền có **gói thay đổi** và nút hoàn tác trong màn | — |

> **Điểm mạnh của đợt này:** hoàn tác **không cần đụng tới dữ liệu**. Đây là hệ quả trực tiếp của
> việc **0 di trú**.

---

## 4. THỨ TỰ CÒN LẠI TRƯỚC KHI TRIỂN KHAI

| Bước | Việc | Trạng thái |
|---|---|---|
| §VIII | Khoá phong bì phát hành | ✅ **Chính là văn bản này** |
| §IX | Nghiệm thu ba luồng phân quyền (cấp nhanh · cấp thêm · thu hẹp) | ⏳ Kế tiếp |
| §X | Các nợ còn lại cần bản đề xuất, không tự làm | ⏳ |
| §V | Nâng số phiên bản `V1.00.366` → `V1.00.367` **sau khi chốt mã** | ⏳ — cổng `test:dinh-danh-phat-hanh` **đang cố ý ĐỎ** vì hiện có **hai nội dung khác nhau mang cùng một số phiên bản**; nâng số là cách sửa đúng |
| §XI | Chạy trọn bộ cổng | ⏳ — ba nhóm đã xanh riêng lẻ (**811 điều kiện đạt**) |
| §XII | Sao lưu → diễn tập trên bản sao → triển khai → kiểm khói | ⏳ |
| §XIII | **Anh nghiệm thu trên máy vận hành** | ⏳ — em **không tự ghi «đã nghiệm thu»** |

---

## 5. LỚP BẰNG CHỨNG

| Khẳng định | Lớp | Lệnh kiểm lại |
|---|---|---|
| 0 di trú · 0 câu DDL | `CODE_PROVEN` | `git diff --name-only <mã-nguồn-riêng>..HEAD -- migrations/` |
| 0 đổi thư viện | `CODE_PROVEN` | `git diff <mã-nguồn-riêng>..HEAD -- package-lock.json` |
| Engine tính giá **không bị đụng** | `CODE_PROVEN` | Đã lọc bỏ chú thích khỏi diff `tinh-gia-manual-client.tsx`: chỉ còn gom `useEffect`, `any`→`unknown`, đổi dấu nháy. **Không dòng nào sửa phép tính** |
| Máy vận hành đang là `V1.00.366` | `RUNTIME_PROVEN` | Kiểm khói phần trước |
| 14 tệp vào bản dựng | `CODE_PROVEN` | `git diff --stat <mã-nguồn-riêng>..HEAD -- src/` |

---

*Công khai-an toàn: không mật khẩu, không khoá, không dữ liệu cá nhân, không địa chỉ máy chủ.*
