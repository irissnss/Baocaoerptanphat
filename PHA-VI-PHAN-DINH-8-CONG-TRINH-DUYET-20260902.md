# PHA VI — PHÂN ĐỊNH 8/8 CỔNG TRÌNH DUYỆT (`DEBT-168`)

> **Theo đặc tả** `CONTINUATION R3` §III — mỗi cổng phải có phân định rõ, không được im lặng bỏ qua cổng nào.
> **Ngày:** 02/09/2026 · **Kho:** `TanPhat ERP` · **Nhánh:** `main`

---

## 1. TÓM TẮT CHO NGƯỜI KHÔNG LÀM KỸ THUẬT

Hệ thống có một bộ **bài kiểm tự động chạy trên trình duyệt** — nó mở màn hình thật, bấm thật, rồi
kiểm xem kết quả có đúng không. Bộ này có **14 bài**.

Trước lượt này: **10 bài đỏ**. Nhưng đỏ **không có nghĩa là phần mềm hỏng**.

Sự thật đo được: **màn phân quyền đã được làm lại ba lần trong bốn ngày** (29/08 · 31/08 · 01/09) —
mỗi lần đều theo yêu cầu của anh và mỗi lần đều tốt hơn. Nhưng **các bài kiểm vẫn đi tìm màn hình cũ**.
Chúng bấm vào nút không còn ở đó nữa, rồi đứng chờ 30–60 giây và báo hỏng.

> **Ví dụ dễ hình dung nhất:** anh yêu cầu gỡ mã kỹ thuật (`#191`, `KE_TOAN`) khỏi màn vì nó khó hiểu
> với người dùng. Em gỡ đúng. Nhưng bài kiểm lại đang **chọn người bằng chính con số đó** — nó tìm nút
> có chữ `#191`, không thấy, và treo. **Phần mềm đúng hơn trước, bài kiểm thì hỏng.**

Nay **14/14 bài xanh**, **193 điều kiện đạt / 0 hỏng**, và cơ sở dữ liệu **sạch tuyệt đối** sau khi chạy.

⚠️ **Một điều em phải nói thẳng:** làm bài kiểm xanh **không phải** là mục tiêu. Bài kiểm xanh mà không
canh gì thì còn tệ hơn bài kiểm đỏ — vì nó cho cảm giác an toàn giả. Nên em đã **cố ý phá giao diện
để xem bài kiểm có bắt được không** (mục 4). Nó bắt được, và chỉ đúng chỗ.

---

## 2. BẢNG PHÂN ĐỊNH 8/8 — ĐỦ CÁC CỘT `R3 §III.1` YÊU CẦU

| # | Cổng | Tệp | Bất biến GỐC (điều thật sự cần canh) | Giả định CŨ đã gãy | Vì sao đỏ | Giao diện HIỆN TẠI | Phân định | Kiểm ngược | Trước → Sau | Nợ được bảo vệ |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | `anh-chuyen-trang-thai` | `h-chup-anh-chuyen-trang-thai.ts` | Ma trận **chuyển trạng thái** chụp được ở ba khổ màn để nghiệm thu | Ma trận nằm ngay trên `/m0/security`, không phải bấm gì thêm | Màn chia **hai khu** 29/08; ma trận dời sang khu «Quản Lý Nâng Cao» ⇒ cổng chờ chữ ở khu KIA, treo 60 s | `/m0/security` → khu «Quản Lý Nâng Cao» → mục ma trận | **VIẾT_LẠI** — chỉ đổi **đường đi**, giữ nguyên phép kiểm | Gián tiếp — cùng cơ chế mở khu với ca kiểm ngược #2 (mục 4) | treo 60 s → **21/21** | `DEBT-168` |
| 2 | `anh-ma-tran-quyen` | `d-chup-anh-ma-tran-quyen.ts` | Ma trận **hành động** chụp được ở ba khổ màn | (như trên) | (như trên) | (như trên) | **VIẾT_LẠI** — đổi đường đi | Gián tiếp — như trên | treo 60 s → **16/16** | `DEBT-168` |
| 3 | `anh-phan-quyen` | `p1-anh-trung-tam-phan-quyen.ts` | **Năm vùng quyền đều còn tồn tại**, mỗi vùng hiện đúng nội dung của nó | Năm vùng = năm nút trên thanh «Năm Bước» (`nav ol li button`) | Thanh «Năm Bước» **gỡ 29/08** khi làm lại màn thành ba pha | Năm **mục gập** trong khu «Quản Lý Nâng Cao» | **VIẾT_LẠI** — bám tiêu đề mục thay vì nút thanh. **Ý định bảo vệ giữ 100 %** | ✅ **ĐÃ CHẠY THẬT** — ẩn một vùng → cổng đỏ, báo đúng `4/5` và đích danh vùng bị ẩn; hoàn nguyên → **14/14** | treo 60 s → **14/14** | `DEBT-168` |
| 4 | `anh-thanh-ben` | `p1-anh-thanh-ben-theo-vai-tro.ts` | Thanh bên hiện **đúng menu theo vai trò** đang chọn | Điều hướng qua `nav ol li button` | (như #3) | Bấm khu + mục | **VIẾT_LẠI** — đổi đường đi | Gián tiếp — cùng cơ chế với #2. **Chưa chạy kiểm ngược riêng, ghi rõ để không nói quá** | treo 60 s → **15/15** | `DEBT-168` |
| 5 | `d3-anh` | `d3-anh-va-hanh-vi-tren-trinh-duyet.ts` | Ba luồng cấp quyền chạy đúng · **Thu Hẹp tự khoá** khi không còn vai trò mẫu · màn nói rõ **đang thao tác trên AI** | (a) chọn người bằng `#id` **in trên thẻ**; (b) đọc chữ trong **một cột**; (c) chờ đúng câu «N màn hình đang dùng được» | (a) số tài khoản **đã gỡ** theo lệnh Owner «mã kỹ thuật không làm nhãn»; (b) màn chia **hai cột** 01/09; (c) câu đổi sang **tiếng nghiệp vụ** theo yêu cầu Owner | Thẻ người mang **móc kiểm ẩn** `data-tai-khoan`; tóm tắt đọc «Làm được: Bán Hàng · Kho Hàng» | **VIẾT_LẠI** — định danh đổi từ **mã** sang **tên người**; đọc chữ **toàn trang**; chấp nhận **cả hai** cách nói | ✅ **ĐÃ CHẠY THẬT** — gỡ móc `data-tai-khoan` → cổng rơi **22 → 2** điều kiện, treo đúng ở chính móc đó; hoàn nguyên → **22/22** | treo 30 s → **22/22** | `DEBT-168` · `DEBT-148` |
| 6 | `ux-cong` | `ux-cong-nghiem-thu-phan-quyen.ts` | Nghiệm thu phân quyền — chọn được vai trò và áp được | Chọn vai trò bằng **mã kỹ thuật đọc từ màn** | ⚠️ **Ca trớ trêu nhất đợt này:** chính `DEBT-148` bắt gỡ mã kỹ thuật khỏi màn — và **cổng này là cổng đã phát hiện ra `DEBT-148`**. Cổng làm đúng việc, bản sửa theo nó làm chính nó treo | Thẻ vai trò mang **móc kiểm ẩn** `data-vai-tro` | **VIẾT_LẠI** — bám điểm neo **không hiển thị** | Cùng cơ chế móc ẩn với ca kiểm ngược #1 | treo → **34/34** | `DEBT-168` · `DEBT-148` |
| 7 | `mobile-bar` | `mobile-action-bar.browser.test.ts` | Thanh hành động trên màn hẹp | — (không gãy) | **Không hỏng.** Cần biến `TEST_EMAIL`; **cố ý từ chối khi thiếu** theo `GOV-SECRET-IN-CODE-001` — thà đỏ còn hơn viết cứng tài khoản vào mã | — | **MIỄN_TRỪ_CÓ_LÝ_DO** — chạy tay khi có biến môi trường | Không áp dụng | — | — |
| 8 | `khoi-van-hanh` | `d3-khoi-van-hanh.ts` | Khối vận hành đúng phiên bản + chọn được người **trên máy vận hành** | (a) bám `V1.00.361` **viết cứng**; (b) bám `#id` | (a) **hỏng theo THỜI GIAN, không theo MÃ** — không ai đụng mà vẫn đỏ; (b) số tài khoản đã gỡ | Máy vận hành **chưa có bản vá** (chưa triển khai) | **ĐÃ VÁ + HOÃN CÓ CHỦ ĐÍCH** — vá xong cả hai lỗi, nhưng cổng **chạy trên runtime máy vận hành** nên **chưa thể xanh trước ngày triển khai** | Không áp dụng cho tới sau triển khai | — | `DEBT-168` |

### Ghi chú trung thực về cổng #8

**Cổng đúng, mã đúng — chỉ là hai thứ đó chưa gặp nhau.** Nếu để nó trong bộ chạy trên máy phát triển
thì nó **đỏ vĩnh viễn cho tới ngày triển khai**, mà một bộ luôn đỏ thì bị bỏ qua — đúng cơ chế đã biến
80 cổng thành mồ côi (`DEBT-131`).

⛔ **Đây KHÔNG phải miễn trừ vĩnh viễn.** Cổng này là **một phần của bước smoke SAU TRIỂN KHAI**
(`WP-ERP-SEP01` §XII), đã ghi lý do và thời điểm chạy lại vào `scripts/tests/kiem-cong-mo-coi.mjs`
để không ai quên.

---

## 3. NGUYÊN TẮC RÚT RA — ĐIỀU ĐÁNG GIÁ NHẤT CỦA CẢ ĐỢT

> ### Cổng phải canh **BẤT BIẾN NGỮ NGHĨA**, không canh **HÌNH DẠNG CỤ THỂ**.

| Canh SAI (bám hình dạng) | Canh ĐÚNG (bám bất biến) |
|---|---|
| «nhãn có đúng bốn chữ «Xem Hướng Dẫn Đầy Đủ» không» | «có đường tới trang hướng dẫn không» |
| «có đúng cái dải ba ô đó không» | «màn có nói trình tự làm việc không» |
| «màn có in ra `#191` không» | «màn có nói rõ đang thao tác trên **ai** không» |
| «máy vận hành có chạy đúng `V1.00.361` không» | «máy vận hành có chạy đúng **phiên bản đang phát hành** không» |
| «năm vùng có nằm trên thanh `nav ol li` không» | «**năm vùng có còn tồn tại** không» |

Và một hệ quả sắc hơn, rút từ ca #6:

> ### Cổng nghiệm thu giao diện **không được bám vào chính thứ mà nó đang đòi gỡ đi.**
> Nó phải bám một **điểm neo không hiển thị**.

Đó là lý do em thêm **móc kiểm ẩn** `data-tai-khoan` / `data-vai-tro`. Người dùng **không nhìn thấy**,
trình đọc màn hình **không đọc ra**, nhưng bài kiểm bám được và **không hỏng khi đổi nhãn**.
Đặc tả `CONTINUATION R3` §III.2.C cho phép đúng trường hợp này.

**Hai hướng khác đã cân nhắc và LOẠI:**
- ❌ Đưa mã kỹ thuật **trở lại màn** cho test xanh — `WP-ERP-SEP01` §XIV **cấm** («sửa UI cũ trở lại chỉ
  để thoả test lỗi thời»), và chọi thẳng quyết định của anh.
- ❌ Cho test bám **tên hiển thị** — tên người **trùng nhau được**; chính `d3-anh` đã ghi nhận sự cố
  bắt nhầm người ngày 29/08.

---

## 4. KIỂM NGƯỢC — BẰNG CHỨNG CỔNG THẬT SỰ CANH

> **Vì sao bắt buộc:** một cổng xanh chưa chứng minh được gì. Trong chính đợt này em đã gặp ca
> **ba luật quét mã băm bị nuốt im lặng** — cổng xanh 100 % mà **không hề canh gì**, chỉ lộ ra khi
> kiểm ngược. Không kiểm ngược thì em đã đóng nợ bằng một cổng chết.

### Ca #1 — gỡ móc kiểm khỏi giao diện

```
Thao tác   : đổi tên thuộc tính data-tai-khoan trên thẻ chọn người
             thành một tên không cổng nào biết
Kỳ vọng    : d3-anh phải ĐỎ
Kết quả    : ✅ ĐỎ — mã thoát 1, rơi từ 22 xuống 2 điều kiện
             treo đúng tại chính móc kiểm vừa bị gỡ
Hoàn nguyên: git checkout → cây làm việc 0 tệp đổi → chạy lại: mã thoát 0, 22/22
```

### Ca #2 — ẩn một trong năm vùng quyền

```
Thao tác   : đổi tên vùng «Ma Trận Thao Tác · Dữ Liệu · Trường Nhạy Cảm»
             thành một tên không cổng nào biết
Kỳ vọng    : anh-phan-quyen phải ĐỎ, và phải chỉ ĐÚNG vùng bị ẩn
Kết quả    : ✅ ĐỎ — mã thoát 1
             FAIL  1. Đủ NĂM vùng quyền → đếm được 4/5
             FAIL  3.4 Vùng 4 (Hành Động) hiện đúng nội dung của nó
             ⇒ không chỉ đỏ, mà đỏ ĐÚNG CHỖ
Hoàn nguyên: git checkout → cây làm việc 0 tệp đổi → chạy lại: mã thoát 0, 14/14
```

**Kết luận:** cổng **đỏ khi bất biến bị vi phạm**, **xanh khi hoàn nguyên**, và **chỉ đích danh chỗ hỏng**.

---

## 5. SỐ ĐO CUỐI

| Hạng mục | Trước | Sau |
|---|---|---|
| Cổng trình duyệt chạy trọn bộ | **2 đạt / 10 hỏng** | **14 đạt / 0 hỏng** — mã thoát `0` |
| Tổng điều kiện | — | **193 đạt / 0 hỏng** |
| Tài khoản thử `@kiemthu.local` còn sót | 1 (`dokhuon…`) | **0** |
| Tài khoản UAT `@local.test` | 6 | **6** — nguyên vẹn, không bị dọn nhầm |
| Tổng tài khoản | 15 | **15** |
| Quyền màn hình · quyền hành động | 148 · 67 | **148 · 67** — không dòng nào bị bài kiểm ghi vào |
| Vai trò tạm còn sót | — | **0** |

> Hai dòng cuối là **quan trọng nhất**: chúng chứng minh bộ bài kiểm **chạy xong không để lại dấu vết
> nào trong dữ liệu** — không tạo thừa tài khoản, không ghi nhầm quyền, không xoá nhầm bộ UAT.

---

## 6. CÒN LẠI — NÓI RÕ ĐỂ KHÔNG AI TƯỞNG ĐÃ XONG

| Việc | Trạng thái | Chặn gì |
|---|---|---|
| Cổng `khoi-van-hanh` xanh trên máy vận hành | **Chờ triển khai** | Không chặn — thuộc bước smoke §XII |
| Cổng `mobile-bar` | **Chờ biến `TEST_EMAIL`** | Không chặn |
| Kiểm ngược riêng cho `anh-thanh-ben` | **Chưa chạy** — mới suy ra gián tiếp | Không chặn, nhưng là nợ đo lường |
| `DEBT-144` (siết quyền sơ đồ quy trình) | **Đã vá, CHƯA triển khai** | 🔴 **CHẶN đợt này** — lỗ hổng trên máy vận hành còn mở |
| `DEBT-143` (hai hành động rỗng) | **Chờ kiểm chứng runtime** | Còn mở |
| `DEBT-169` (lệch quyền hai môi trường) | Một tài khoản thật là `ADMIN` trên máy vận hành, `SALES` ở nội bộ | ⚠️ Chờ anh quyết — **em không tự sửa bên nào** |

---

## 7. LỚP BẰNG CHỨNG

| Khẳng định | Lớp | Cách kiểm lại |
|---|---|---|
| 14 cổng xanh, 193 điều kiện đạt | `UI_PROVEN` | `npm run test:nhom-trinh-duyet` → mã thoát 0 |
| Cổng thật sự canh (không xanh giả) | `UI_PROVEN` | Hai ca kiểm ngược ở mục 4, hoàn nguyên đã xác nhận |
| Dữ liệu không bị bài kiểm làm bẩn | `DB_PROVEN` | Truy vấn đếm ở mục 5 |
| Máy vận hành **chưa có** bản vá | `RUNTIME_PROVEN` | Cổng #8 đỏ vì đúng lý do đó |

---

*Báo cáo này công khai-an toàn: không có mật khẩu, khoá, mã băm, hay dữ liệu cá nhân.
Danh tính `người-A` ở `DEBT-169` lưu tại `SO-BI-MAT-NOI-BO.md` (tệp đã bị git bỏ qua).*
