# BÁO CÁO — CHỐT LUẬT BÍ MẬT & AUDIT CHUẨN GIAO DIỆN (05/09/2026)

> **Kho:** báo cáo công khai (bản đã lọc an toàn) · **Ngày lập:** 05/09/2026
> **Phạm vi được cấp:** máy nội bộ · mã nguồn · quản lý phiên bản + kho báo cáo công khai. **KHÔNG** ghi Notion.
> **Kết quả tổng:** 5/6 việc XONG · 1 việc **BLOCKED** đúng theo cổng dừng do chính Owner đặt.

---

## 1. Owner giao gì

Sáu việc trong một phiên: **V1** xác minh khoá đăng nhập trên cả hai máy → **V2** chụp ảnh nghiệm thu `UI_PROVEN` → **V3** vá luật về nơi lưu thông tin bí mật → **V4** audit tài liệu chuẩn giao diện → **V5** đẩy ba báo cáo công khai → **V6** ghi sổ và sổ nợ.

Owner đặt sẵn **cổng dừng**: *"V1 FAIL cả 2 máy → dừng V2, vẫn làm tiếp V3–V6 và báo BLOCKED rõ"*.

Owner cũng gửi lại chỉ thị kèm khối **QUYỀN ĐÃ ĐƯỢC CẤP SẴN**:

> *"Đọc sổ bí mật trong root và dùng để đăng nhập là hành vi ĐƯỢC CẤP PHÉP SẴN — không xin phép, không báo trước, không viện 'sợ lộ public' để dừng. Luật public-safe chỉ cấm ĐƯA giá trị bí mật vào báo cáo/git public/log/chat — KHÔNG cấm đọc và dùng nội bộ. Dùng sai ranh giới này để dừng việc là SAI LUẬT."*

Đây là **một chấn chỉnh**: tôi đã từng lấy ranh giới "sợ lộ ra công khai" làm lý do để dừng một việc mà thực chất chỉ cần **đọc nội bộ**. Đã ghi vào sổ yêu cầu Owner với `lần_lặp` = 1.

---

## 2. V1 — xác minh khoá đăng nhập: **THẤT BẠI Ở CẢ HAI MÁY**, có bằng chứng vét cạn

### Cách làm — và vì sao **không** thử đăng nhập

Máy chủ **khoá tài khoản sau 5 lần sai**. Thử mò sẽ tự khoá mất tài khoản, và luật cũng **cấm đoán mật khẩu**. Nên thay vì đăng nhập, đã **đối chiếu mã băm thẳng trên cơ sở dữ liệu nội bộ** — cách này cho câu trả lời chắc chắn mà **không tiêu tốn lượt đăng nhập nào**.

### Kết quả

| Phép đo | Kết quả |
|---|---|
| Số chuỗi ứng viên quét được từ **cả hai** sổ bí mật trong gốc dự án | **25** |
| Số chuỗi khớp mã băm trong cơ sở dữ liệu | **0 / 25** |
| Trạng thái tài khoản | **đang hoạt động** |
| Tài khoản có đang bị khoá không | **KHÔNG** |
| Mốc đổi mật khẩu gần nhất ghi trong cơ sở dữ liệu | **26/08/2026** |
| Tệp bí mật trong gốc dự án được sửa gần nhất | **01/09/2026** |
| Số lượt đăng nhập bị tiêu tốn | **0** |

> Quá trình quét **chỉ ghi vị trí dòng**, tuyệt đối **không ghi giá trị** — kể cả trong nhật ký chạy.

### Kết luận

Đây **không phải** chuyện gõ sai. Mật khẩu **đã được đổi ngày 26/08** nhưng **chưa ai ghi lại vào sổ**, nên sổ bí mật đang giữ giá trị lỗi thời. Sổ bí mật là **nơi duy nhất** được luật chỉ định để tra ⇒ nó sai thì **không có nguồn thứ hai**.

**Đã báo Owner đúng một câu kèm bằng chứng**, đúng nghĩa vụ mới ở mục 3 dưới đây. **Không** đoán mật khẩu. **Không** thử lại giá trị đã đánh dấu hết hiệu lực. **Không** sửa cơ sở dữ liệu hay tài khoản để "cho vào được".

---

## 3. V2 — **BLOCKED**, đúng theo cổng dừng của Owner

Không đăng nhập được ⇒ **không lấy được ảnh nghiệm thu `UI_PROVEN`** cho bất kỳ tiêu chí giao diện nào. Toàn bộ kịch bản chụp ảnh nghiệm thu hiện **vô dụng**.

Đây là điều đáng kể chứ không phải chi tiết nhỏ: tài liệu chuẩn **bắt buộc** việc giao diện phải có bằng chứng ở lớp người dùng nhìn thấy, và **cấm** nghiệm thu bằng công cụ tìm chuỗi. Nghĩa là **mọi việc giao diện đang bị chặn ở khâu nghiệm thu cuối** cho tới khi khoá được cập nhật.

Nợ này giữ nguyên trạng thái **MỞ — CHỜ OWNER**.

---

## 4. V3 — vá luật về nơi lưu thông tin bí mật

### Luật cũ sai chỗ nào

Luật ghi **"CHỈ HAI nơi hợp lệ, không có nơi thứ ba"** và khoá cứng đúng hai đường dẫn.

Điều này **lệch ý Owner**. Owner đã chốt từ 23/08: *"các mật khẩu khác em tự tạo và lưu 1 nơi bí mật riêng trong root dùm anh"* — tức **nơi lưu do Agent tự chọn**, Owner không quản. Hệ quả trớ trêu: cuốn sổ được lập **theo đúng quyết định đó của Owner** lại trở thành "nơi thứ ba bất hợp lệ" theo chữ của luật.

### Đã vá gì

| Sửa | Nội dung |
|---|---|
| **Mục 1** *(sửa)* | **MỘT nơi** trong gốc dự án, **do Agent tự chọn và tự chốt**; phải được công cụ quản lý phiên bản bỏ qua; phải **khai tường minh trong luật** để phiên sau tra được. Danh sách nơi đang dùng là **trạng thái**, không phải giới hạn cứng |
| **Mục 1b** *(mới)* | **TỰ PHỤC VỤ.** Cần khoá thì **tự đọc** từ nơi đã khai rồi dùng. **CẤM** bắt Owner gõ tay, dán vào chat, hay xác nhận từng lần. Giá trị chỉ sống trong bộ nhớ tiến trình — không in ra, không ghi vào tệp |
| **Mục 1c** *(mới)* | Khoá sai/lỗi thời thì **báo đúng MỘT câu kèm bằng chứng**. CẤM đoán · CẤM thử lại giá trị hết hiệu lực · CẤM sửa cơ sở dữ liệu để vào. Nêu sẵn cách chẩn đoán **không tốn lượt** để khỏi chạm ngưỡng khoá 5 lần |

### Điểm quan trọng nhất — khối cấm viết lại thành **HAI VẾ ĐI LIỀN**

Khối cấm cũ chỉ có một vế: *"không đưa giá trị ra ngoài"*. Đọc một vế thì dễ suy ra "vậy thì đừng đụng vào cho chắc" — và đó chính là cái sai Owner vừa chấn chỉnh. Nay:

- **(a) CẤM đưa giá trị ra ngoài** — vào tệp được theo dõi · báo cáo · nhật ký · thông điệp commit · cửa sổ trò chuyện · kho công khai;
- **(b) CẤM lấy vế (a) làm cớ để không dùng** — từ chối/hoãn việc chỉ vì "sợ lộ" trong khi việc đó chỉ cần **đọc nội bộ**; hỏi Owner cung cấp khoá; báo BLOCKED khi **chưa** thực sự đọc và **chưa** thực sự thử.

### Kiểm chứng

- Năm bản luật **giống nhau từng byte** sau khi vá — cổng đồng bộ **PASS**.
- **Kiểm ngược bắt buộc:** cắm lại chuỗi cũ vào một bản → cổng chuyển **ĐỎ**; hoàn nguyên → cổng **XANH** trở lại, dấu vân tay của cả năm bản về đúng giá trị trước đó.
- Cổng đếm điều khoản **PASS**; cổng quét bí mật **PASS** (0 vi phạm).
- Dòng cũ giữ **nguyên văn** ở mục lịch sử sửa đổi, kèm lý do — theo `GOV-EDIT-PRESERVE-001`.

---

## 5. V4 — audit tài liệu chuẩn giao diện: **tìm ra 4 chỗ tài liệu tự chọi chính nó**

Đọc **toàn phần** tài liệu chuẩn (612 dòng) — bắt buộc, vì phiên đã bị nén ngữ cảnh nên **cấm làm tiếp bằng trí nhớ**.

Cách dựng đầu bảng **đã có sẵn và đã đúng** ở bốn nơi sau đợt vá 04/09 ⇒ **không cần thêm mục mới**. Nhưng phép quét cùng chủ đề tìm ra **bốn chỗ còn lệch**:

| # | Chỗ lệch | Vì sao nguy hiểm |
|---|---|---|
| 1 | Câu dẫn bảng điều kiện ghi **"BA điều kiện"** trong khi bảng ngay dưới đã có **4 dòng** | Người đọc đếm theo câu dẫn sẽ **bỏ qua đúng điều kiện vừa gây sự cố** |
| 2 | 🔴 **Điều kiện số 1 dạy đặt thuộc tính ghim lên Ô TIÊU ĐỀ** | Đây đúng là **tổ hợp bị cấm** ở mục §2, và bị chính điều kiện 4 bác. Ai đọc bảng từ trên xuống rồi dừng ở dòng 1 sẽ **dựng lại nguyên si lỗi Owner báo 04/09**. Đợt vá 04/09 đã sửa ba mục khác nhưng **bỏ sót chính bảng này** |
| 3 | Hàng phân xử xung đột số 8 phán *"ô tiêu đề không đặt nền"* nhưng **giữ nguyên vế "ô tiêu đề có ghim"** của nguồn cũ | **Cùng loại bẫy "mệnh lệnh nửa vời"**: thi hành đúng chữ sẽ ra **ghim ở ô tiêu đề + không nền = trong suốt** |
| 4 | Số bản tài liệu đứng yên dù đã nhận **ba đợt sửa** | Ai tra số bản để biết mình đang cầm bản nào sẽ **bị lệch 13 ngày** |

Cả bốn đã vá trong **một lượt**, dòng cũ giữ nguyên văn ở mục lịch sử. **Không quyết định nào của Owner bị đổi** — chỉ vá chỗ tài liệu ghi sai chính nó.

**Kiểm chứng:** cổng đếm mục **PASS** — **23 mục · 43 tiêu đề**, **bằng đúng** trước khi vá, không mục nào biến mất. Kiểm ngược bằng phép quét: mọi chỗ còn nhắc tới việc ghim đều đã đúng chuẩn, hoặc thuộc **bảng đo** (nêu tổ hợp bị cấm), hoặc thuộc **mục lịch sử**.

---

## 6. V6 — ghi sổ và sổ nợ

- **Hai mục sổ mới**, mỗi chỉ thị một mục theo đúng luật bàn giao: một mục cho việc Owner giao phiên này, một mục cho chấn chỉnh **QUYỀN ĐÃ ĐƯỢC CẤP SẴN**. Cả hai mang trạng thái **đã duyệt, chờ đồng bộ sang Notion**.
- **Nợ khoá đăng nhập** giữ nguyên **MỞ — CHỜ OWNER**, bổ sung toàn bộ bằng chứng vét cạn ở mục 2.

---

## 7. Điều CHƯA chứng minh được

| Điều | Vì sao | Ai gỡ được |
|---|---|---|
| Đầu bảng cam **thật sự** ghim và kín trên màn hình vận hành | Không đăng nhập được ⇒ không chụp được ảnh cuộn | **Owner** — cập nhật khoá hiện hành vào sổ, hoặc cấp một tài khoản kiểm thử riêng chỉ để chụp ảnh |
| Ba tiêu chuẩn còn lại của ngày 03/09 (25 dòng · cột panel · bo góc) đã đạt trên màn hình thật | Cùng lý do | Như trên |

Theo `GOV-EDIT-PRESERVE-001` và §G1, **`CODE_PROVEN` không được nâng lên thành `UI_PROVEN`**. Báo cáo này **không** tuyên bố các tiêu chuẩn giao diện đã nghiệm thu xong.

---

## 8. Trạng thái thật

| Việc | Trạng thái |
|---|---|
| V1 — xác minh khoá đăng nhập | ❌ **THẤT BẠI cả hai máy** — có bằng chứng vét cạn, 0 lượt đăng nhập bị tiêu tốn |
| V2 — ảnh nghiệm thu `UI_PROVEN` | 🔴 **BLOCKED** — đúng cổng dừng Owner đặt |
| V3 — vá luật nơi lưu bí mật | ✅ XONG — 5 bản giống nhau từng byte, kiểm ngược ĐỎ→XANH |
| V4 — audit tài liệu chuẩn giao diện | ✅ XONG — vá 4 chỗ tự chọi, cổng đếm PASS, không mục nào mất |
| V5 — ba báo cáo công khai | ✅ XONG — chính tài liệu này và hai tài liệu cùng đợt |
| V6 — ghi sổ và sổ nợ | ✅ XONG |

**Việc kế tiếp — đúng một việc:** Owner cập nhật mật khẩu hiện hành vào sổ bí mật nội bộ (hoặc cấp một tài khoản kiểm thử riêng chỉ dùng để chụp ảnh nghiệm thu). Đó là **điều kiện duy nhất** để mở lại V2 và đóng bằng chứng cho toàn bộ chuỗi việc giao diện từ 03/09.

---

*Bản công khai đã lọc theo `GOV-PUBLIC-SAFE-001`: không mã đăng nhập, không dữ liệu cá nhân, không số liệu kinh doanh, không dấu vết hạ tầng. Mã commit của kho mã nguồn nằm ở sổ nội bộ, không đưa ra kho công khai.*
