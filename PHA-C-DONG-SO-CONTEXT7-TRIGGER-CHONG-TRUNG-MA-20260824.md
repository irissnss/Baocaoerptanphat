# PHA C — ĐÓNG SỔ PHA B · ĐIỀU KIỆN GỌI CONTEXT7 · CHỐNG TRÙNG MÃ SỔ

**Ngày:** 24/08/2026 · **Vai trò thực hiện:** Agent IDE (Claude Code)
**Kho mã:** riêng tư — **Kho báo cáo:** `irissnss/Baocaoerptanphat`
**Chuỗi báo cáo:** `AUDIT/DIAGNOSIS` → `LUAT-SKILL-UI-GOP` → **PHA B** → **PHA C** *(bản này)*

---

## 1. Tóm tắt cho chủ dự án

Phiên này làm bốn việc, **không đụng một dòng mã nghiệp vụ nào**:

| # | Việc | Kết quả |
|---|---|---|
| 1 | Kiểm lại toàn bộ Pha B bằng bằng chứng thật | ✅ Đạt |
| 2 | Khoá **điều kiện gọi Context7** (trước đây không có) | ✅ Xong + **một** cổng tự động, cổng đó kiểm **15 điều kiện** *(15 điều kiện bên trong một cổng — không phải 15 cổng)* |
| 3 | Vá **mã trùng `DEBT-097`** + dựng cổng chống trùng mã cho hai sổ | ✅ Xong |
| 4 | Công bố báo cáo Pha B + Pha C | ✅ Bản này |
| 5 | **Xử lý mục `#53` trùng trong Sổ Yêu Cầu Owner** | ✅ Xong — bổ sung ngày 24/08 (Pha C2) |
| 6 | **Nối cổng chống trùng vào chuỗi kiểm tự động + pre-commit** | ✅ Xong — bổ sung ngày 24/08 |
| 7 | **Đính chính mốc commit của Pha B** | ✅ Xong — bổ sung ngày 24/08 |

> 📌 **Bổ sung 24/08/2026 (Pha C2).** Bản đầu của báo cáo này còn treo một việc chờ chủ dự án (mục `#53`).
> Việc đó **đã xử lý xong**; các mục 3, 6, 8, 9 bên dưới đã cập nhật theo trạng thái cuối.

---

## 2. Vì sao phải làm phần Context7

Bộ luật của dự án bắt: **trước khi gọi bất kỳ công cụ nào, phải đọc sổ công cụ**. Nhưng mục `context7` trong sổ đó chỉ có **bốn dòng**: lớp, vai trò, trạng thái, và một câu mô tả khi nào nên dùng.

Bốn dòng đó **không trả lời được** bất kỳ câu nào sau đây:

- Khi nào **được phép** gọi? → không có điều kiện nào ⇒ gọi lúc nào cũng "đúng luật".
- Khi nào **cấm** gọi? → không có ⇒ hỏi cả nghiệp vụ nội bộ cũng không ai chặn.
- Được gửi **dữ liệu gì** ra ngoài? → không có dòng nào cấm gửi mã nguồn riêng.
- Tài liệu trả về **khác phiên bản** đang cài thì sao? → không có ràng buộc.

Đây đúng loại lỗ mà chính bộ luật dự án đã đặt ra để chặn: một dòng trạng thái **đọc như đã kiểm soát**, nhưng **giá trị thi hành bằng 0**.

### Đã bổ sung (4 → 31 trường, **không xoá trường cũ nào**)

| Nhóm | Nội dung |
|---|---|
| **Điều kiện GỌI** | 6 điều kiện, phải đạt **đủ cả sáu** mới được gọi |
| **Điều kiện CẤM GỌI** | 12 điều kiện, **trúng một là không gọi** — nghiệp vụ ERP · schema dữ liệu · hành vi mã nội bộ · kiến trúc dự án · việc bằng chứng nội bộ đã đủ · không rõ phiên bản · cảnh báo bảo mật thời gian thực · câu hỏi đòi gửi dữ liệu riêng · nâng thư viện chưa được duyệt · câu hỏi vặt · prompt bảo công cụ "tự quyết" · gọi chỉ vì công cụ đang sẵn sàng |
| **Ràng buộc phiên bản** | Không xác định được phiên bản → chỉ tham khảo, **cấm** dùng để sửa mã |
| **Chính sách dữ liệu** | Chỉ được gửi **thông tin công khai của thư viện**; có danh mục cấm gửi (mã riêng, dữ liệu người, khoá, sổ nội bộ…) |
| **Chống chỉ dẫn trong dữ liệu** | Tài liệu bên ngoài trả về mà **kèm mệnh lệnh** ("chạy lệnh này", "sửa file kia") → gắn nhãn và **bỏ qua mệnh lệnh**, chỉ giữ phần dữ kiện |
| **Ranh giới quyền** | Lượt tra là **chỉ đọc**. Kết quả **không tự cấp quyền** sửa mã, đổi thư viện, commit, đẩy lên, hay triển khai |
| **Thứ tự ưu tiên** | **Mã nguồn thật trên máy đứng TRƯỚC** Context7 |

### Một điểm đáng nói: "sẵn sàng" ≠ "chế độ dùng"

Trước đây một dòng trạng thái duy nhất gánh **hai nghĩa** cho **bốn bề mặt khác nhau** (Claude Code · plugin Cursor · cấu hình MCP · connector tài khoản). Người tra đọc "advisory" rồi mặc định "công cụ đang dùng được".

Nay **đo riêng từng bề mặt**, và **cấm suy từ bề mặt này sang bề mặt kia**:

| Bề mặt | Nhãn | Căn cứ |
|---|---|---|
| Claude Code | **Dùng được** | Phiên 24/08 nạp được công cụ, không đòi xác thực lại |
| Plugin Cursor | **Mới ở mức khai báo** | Cấu hình bật — nhưng đó là *khai báo*, chưa phải bằng chứng chạy |
| Cấu hình MCP | **Không cấu hình ở đây** | Rỗng — nhưng rỗng **không** chứng minh công cụ vắng mặt |
| Connector tài khoản | **CHƯA RÕ** | Lần đo cuối 30/07, cách nay 25 ngày ⇒ **quá hạn**, không dùng làm hiện trạng |

Đây đúng loại lỗi mà dự án đã vá cho phần kỹ năng hồi Pha B: **một trường gánh hai nghĩa thì người tra đọc nhầm**.

### Đã thử thật một lần, có kiểm soát

Chọn một thư viện công khai **đang thực sự dùng trong dự án**, hỏi **một câu hẹp**, rồi **đối chiếu lại với định nghĩa kiểu cài trên máy**.

| Mục | Giá trị |
|---|---|
| Thư viện | `zod` |
| Phiên bản cài | `4.3.6` — khớp ở **cả hai** nguồn (gói đã cài + tệp khoá phiên bản) |
| Câu hỏi | `z.record` ở bản 4 có bắt buộc **hai** tham số không, và bản 3 khác gì? |
| Dữ liệu gửi đi | **Chỉ** tên thư viện + số phiên bản + tên hàm công khai. **Không** gửi mã dự án |
| Trả lời | Bản 4 **bắt buộc hai** tham số; bản 3 cho một |
| Đối chiếu máy thật | **KHỚP** — kiểu cài trên máy đúng là hai tham số bắt buộc |
| Mệnh lệnh lẫn trong dữ liệu | **Không phát hiện** |
| Đã sửa gì chưa | **Không.** Hai chỗ dùng trong dự án vốn đã viết đúng bản 4 |

**Phán quyết:** khớp phiên bản đang cài. Không phát sinh việc phải sửa.

> ⚠️ Nhà cung cấp công cụ có khuyến nghị "cứ dùng kể cả khi đã biết câu trả lời". Khuyến nghị đó **không** ghi đè điều kiện gọi của dự án — luật dự án đứng trên phương pháp của bên thứ ba.

---

## 3. Chống trùng mã sổ

### Vấn đề

Trùng mã làm **hỏng truy vết**: một tham chiếu "#53" không còn xác định được trỏ tới việc nào. Sổ mất tính sổ.

Đo được **hai** chỗ trùng:

| Sổ | Mã trùng | Tình trạng |
|---|---|---|
| Sổ nợ kỹ thuật | `DEBT-097` | ✅ **Đã vá** (23:xx 24/08 — mục ghi sau nhận hậu tố `-B`) |
| Sổ Yêu Cầu Owner | `#53` | ✅ **Đã vá** (Pha C2 24/08 — xem mục 6) |

Sau khi vá, sổ Yêu Cầu Owner có **142 mục / 142 mã duy nhất**, dãy `1→142` liên tục, không khoảng trống. Sổ nợ có **107 mục**, không mã nào trùng.

### Đã vá `DEBT-097`

Hai việc khác hẳn nhau cùng đeo mã `DEBT-097`. Theo đúng **tiền lệ sẵn có trong chính sổ** (năm mã khác đã xử lý cùng cách hồi 23/08): việc ghi trước giữ mã gốc, việc cấp trùng sau nhận **hậu tố `-B`**, và **cả hai dòng đều được ghi con trỏ** sang nhau. **Không xoá nội dung nào.**

### Cổng chống trùng mã — điểm khó nhất

Hai sổ dùng **hai quy ước cấp lại khác nhau**:

- **Sổ Yêu Cầu Owner** — cấp lại bằng **số kế tiếp còn trống**
- **Sổ nợ kỹ thuật** — cấp lại bằng **hậu tố `-B`**

Hệ quả: một bộ kiểm viết ẩu sẽ coi `DEBT-030-B` là **lần xuất hiện thứ hai** của `DEBT-030` — tức **báo trùng giả ngay trên bản đã vá đúng**. Cổng có riêng một điều kiện khoá lỗi này.

Cổng cũng chỉ đọc **dòng định nghĩa mục**, bỏ qua mọi lần **nhắc lại trong văn mô tả**, và không nhầm ô ngày tháng ở bảng tóm tắt.

**Kiểm ngược 7/7 đạt** — cố tình làm sai thì cổng đỏ đúng chỗ, khôi phục thì xanh lại. Bảy kịch bản: trùng mã ở sổ Owner · trùng mã ở sổ nợ · hậu tố `-B` hợp lệ không báo trùng giả · khôi phục bản sạch · mã sai định dạng · **nhắc lại mã trong văn mô tả không bị tính là cấp mã** · **ngày tháng và khối mã ví dụ không bị hiểu là mã mục**.

> 🔎 **Hai kịch bản cuối bắt được lỗi thật khi thêm vào (24/08).** Parser đang đếm cả dòng bảng **nằm trong khối mã ví dụ** — một ví dụ minh hoạ `| 9 | … |` bị tính thành lần cấp mã thứ hai của `#9`. Đã vá **ở parser**, không vá ở fixture. Sau khi vá, số mục đọc được trên sổ thật **không đổi** (142 và 107) ⇒ vá không làm rơi mục nào.

### ✅ Đã nối vào chuỗi kiểm tự động (24/08 — Pha C2)

| Nơi nối | Trạng thái |
|---|---|
| `package.json` — khai lệnh chạy | ✅ |
| Chuỗi kiểm quản trị tổng | ✅ — nâng **15 → 16** cổng |
| Hook chạy trước mỗi lần ghi mã | ✅ — nâng **3 → 4** cổng |
| Bộ kiểm chống-gỡ-âm-thầm | ✅ — gỡ khai báo thì bộ kiểm **đỏ đúng chỗ**, khôi phục thì xanh |

**Kiểm ngược mức chuỗi tổng:** gieo một mã trùng thật vào sổ → chuỗi tổng **đỏ**, chỉ đúng dòng vừa gieo; khôi phục → **xanh**.

---

## 4. Kiểm lại Pha B

Toàn bộ khẳng định của Pha B được **chạy lại bằng lệnh thật**, không lấy lại số cũ:

| Kiểm | Kết quả |
|---|---|
| Đủ 5 mốc công việc + mốc hoàn tất có trên máy chủ | ✅ |
| Số tệp mã nghiệp vụ bị đụng | **0** |
| Số tệp kỹ năng bị đụng | **0** |
| Toàn bộ 128 kỹ năng đều có nhãn hiệu lực nội dung | ✅ 128/128 |
| Phân bố nhãn | 127 *chưa soát* + 1 *ngủ đông* — **không tự gán "còn hiệu lực"** cho cái nào |
| 5 tệp luật giống hệt nhau từng byte | ✅ cùng một mã băm |
| Lệnh "ma" trong sổ kỹ năng | **0** |
| Phiên bản phần mềm | **không tăng** — đúng chính sách (đây là việc công cụ/quản trị) |

**Hai điểm phải nói thẳng** (không ảnh hưởng kết luận, nhưng đã ghi nhận để không lặp lại):

1. Báo cáo Pha B ghi "13 cổng", chuỗi thật có **14 lệnh** — do một lệnh là chế độ *tự kiểm bộ kiểm*, không tính là cổng. Cách đếm khác nhau, **không phải khai sai**.
2. Phiên Pha B có **một lần sửa lại commit trước khi đẩy**. Commit bị bỏ **chưa từng lên máy chủ**, nên không cần ép ghi đè. Báo cáo Pha B đúng từng chữ, nhưng **không nêu** chi tiết này.

---

## 5. Việc CỐ Ý KHÔNG làm

| Việc | Lý do |
|---|---|
| Sửa mã nghiệp vụ | Ngoài phạm vi |
| Sửa nội dung tệp kỹ năng | Ngoài phạm vi |
| Soát hiệu lực 127 kỹ năng *chưa soát* | Việc riêng, làm theo lô sau |
| Làm mới bản đồ phụ thuộc mã | Chủ dự án đã khoá |
| Kết nối thêm công cụ | Chủ dự án đã khoá |
| Nâng gói / thêm khoá Context7 | Không cần, và sẽ phát sinh khoá phải cất đúng chỗ |
| Sửa 5 tệp luật | Pha C không đụng |
| Sửa **báo cáo lịch sử đã công bố** | Giữ nguyên vẹn lịch sử |
| **Sổ Yêu Cầu Owner** | 🕰️ *Đúng tại thời điểm Pha C:* có phiên khác đang ghi vào sổ này — tránh giẫm chân.<br>**Đã hết hiệu lực:** chủ dự án xác nhận phiên đó **đã ngưng hẳn**; Pha C2 tiếp quản, thẩm định rồi **bảo toàn phần còn sót bằng một mốc riêng** trước khi làm tiếp. |

---

## 6. ✅ ĐÃ XỬ LÝ — mục `#53` trùng số

Mục **`#53`** trong Sổ Yêu Cầu Owner từng bị **hai việc khác nhau** dùng chung:

- **Bản A** — thêm trường chống bẫy nén phiên vào khối khép phiên
- **Bản B** — đợt kiểm tra "agent đang nạp tệp luật nào"

**Đã tìm ra bản nào lấy số trước**, bằng chuỗi thời gian dựng hoàn toàn từ tệp đã lưu ở cả hai kho:

| Giờ | Bằng chứng |
|---|---|
| 19:13 | Sổ mới tới mục **52** — chưa có `#53` |
| 19:28 | Báo cáo của **bản B** trích mã băm **trước khi sửa** ⇒ bản A lúc này **chưa tồn tại** |
| 19:29 | Báo cáo kế tiếp tự nhận **`#54`** ⇒ `#53` **đã bị chiếm** rồi |
| 19:45 | Cả hai dòng mới được lưu chung một lần |

⇒ **Bản B lấy `#53` trước. Bản A mới là bản va chạm sau.**

Điều này **trùng khớp** với hướng chủ dự án đã nêu: giữ mục ghi trước, và đó **cũng chính là** mục đang được báo cáo công khai trỏ tới. **Không có mâu thuẫn.**

### Đã làm (24/08/2026 — Pha C2)

| Việc | Kết quả |
|---|---|
| Bản B (probe) giữ `#53` | ✅ — còn **đúng một** định nghĩa |
| Bản A (trường 11) cấp lại | ✅ → **`#142`**, số kế tiếp còn trống, **tính lại từ sổ** sau khi tiếp quản mục `#141` |
| Nội dung · ngày · người thực hiện của bản A | ✅ **giữ nguyên**, chỉ đổi số, kèm dòng ghi rõ `#53 → #142` và lý do |
| Tham chiếu ngữ nghĩa của bản A | ✅ cập nhật **đúng 1 chỗ** — báo cáo nội bộ về trường 11 |
| Báo cáo công khai trỏ tới bản B | ✅ **KHÔNG đụng dòng nào** — `#53` trong đó vẫn đúng mục |
| Cổng chống trùng | ✅ đã nối vào chuỗi tự động + hook trước khi ghi mã |
| Khoản nợ | ✅ đóng **`DEBT-018`** và **`DEBT-097-B`** *(hai khoản cùng mô tả lỗi này)* |

> ⚠️ **Nói rõ mã nợ đã đóng.** Khoản nợ mô tả lỗi `#53` hiện mang mã **`DEBT-097-B`**, không phải `DEBT-097` — vì chính mã `DEBT-097` cũng từng bị cấp trùng và mục ghi sau đã nhận hậu tố `-B`. `DEBT-097` (mã gốc) nói về việc khác và **vẫn giữ nguyên**.
>
> **`DEBT-082` KHÔNG đóng trong đợt này** — nó nói về trùng mã trong *sổ nợ* (`DEBT-030/031/032`), là việc khác. Giữ nguyên trạng thái.

**Không dùng thay-thế-hàng-loạt chuỗi `#53`.** Chỉ đổi đúng dòng định nghĩa của bản A và đúng một tham chiếu ngữ nghĩa. Mọi chỗ khác nhắc `#53` đều là **mô tả chính sự việc trùng** hoặc **báo cáo lịch sử** — giữ nguyên văn.

---

## 7. Trạng thái từng tầng

| Tầng | Trạng thái |
|---|---|
| Mã nguồn / kho riêng | ✅ Đã kiểm, đã đẩy |
| Kho báo cáo công khai | ✅ Bản này |
| Notion | ⏳ **Chưa** — thuộc phần việc của trợ lý Notion, Agent IDE **không** được đụng |

> **Không** tuyên bố toàn hệ thống "đã đồng bộ" khi phần Notion còn mở. Trạng thái đúng là **đồng bộ một phần**, và đây **không** phải mâu thuẫn — chỉ là chưa tới lượt.

---

## 8. Phán quyết

| Hạng mục | Phán quyết |
|---|---|
| **Pha B** | ✅ **ĐẠT TOÀN PHẦN** trong đúng phạm vi đã khai |
| **Cấu hình Context7** | ✅ **ĐẠT** — đủ điều kiện gọi, điều kiện cấm, ràng buộc phiên bản, chính sách dữ liệu, chống chỉ dẫn lẫn trong dữ liệu, cổng 15/15 |
| **Chạy thử Context7** | ✅ **ĐẠT** — đã thử thật, đối chiếu với máy, không gửi dữ liệu nội bộ |
| **Trùng mã sổ nợ** | ✅ **ĐÃ VÁ** |
| **Trùng mã sổ Owner** | ✅ **ĐÃ VÁ** — bản B giữ `#53`, bản A → `#142` |
| **Cổng chống trùng đã nối** | ✅ **ĐẠT** — chuỗi tự động 16 cổng · hook 4 cổng · kiểm ngược 7/7 |
| **Mốc commit Pha B** | ✅ **ĐÃ ĐÍNH CHÍNH** — dựa trên quan hệ cha-con thật trong lịch sử mã |
| **PHA C** | ✅ **MÃ NGUỒN + BÁO CÁO HOÀN TẤT** · phần Notion **chưa tới lượt** (mục 7) |

> Không ghi *"toàn hệ thống đã đồng bộ"*: phần Notion thuộc trợ lý Notion và **chưa** làm. Đúng trạng thái là **mã nguồn và báo cáo đã xong, Notion còn chờ bàn giao**.

---

## 9. Việc kế tiếp

> **Bàn giao cho trợ lý Notion** để đồng bộ tài liệu bên đó. Agent IDE **không** được sửa Notion, nên phần này không tự đóng được.

Ngoài ra, một phát hiện được ghi thành nợ, **không xử lý trong đợt này**: registry số phát hành nội bộ còn ghi mốc cũ (thiếu bốn đợt gần nhất), trong khi ba nguồn độc lập đều cho số mới hơn. Cần một lượt cập nhật riêng kèm mốc triển khai từng đợt.

---

*Báo cáo công khai — đã qua cổng an toàn: không chứa mã nguồn, dữ liệu thật, thông tin cá nhân, khoá, đường dẫn máy, hay định danh máy chủ.*
