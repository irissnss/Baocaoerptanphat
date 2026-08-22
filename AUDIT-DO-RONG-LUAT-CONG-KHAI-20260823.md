# RÀ SOÁT ĐỘ RỖNG CỦA BỘ LUẬT QUẢN TRỊ — 23/08/2026 · BẢN CÔNG KHAI

> ⚠️ **BẢN ĐÃ CHE.** Bản đầy đủ (có đường dẫn `file:dòng`) giữ ở kho riêng tư.
> Báo cáo này **KHÔNG** nêu tên file, tên thư mục, tên cột hay mã commit của dữ liệu nhạy cảm — vì giá trị vẫn còn trong lịch sử kho mã.
> Chỉ nêu **số lượng**, **cơ chế** và **cách phòng ngừa** (`GOV-PUBLIC-SAFE-001` §J1 · `GOV-PII-HANDLING-001` §G7.13 mục 3).

## Câu hỏi của phiên

Ngày 20/08 dự án phát hiện một lỗi có tính hệ thống: **bộ luật bắt buộc tra một danh mục, nhưng danh mục đó chưa từng tồn tại** — và cổng kiểm không bắt được, vì điều luật viết dưới dạng **sơ đồ luồng** chứ không phải một đường dẫn tệp. Lỗi đã được vá.

Phiên 23/08 đặt câu hỏi tiếp theo: **đó có phải trường hợp duy nhất không?**

## Trả lời: KHÔNG

Phiên rà **chỉ đọc**, không sửa gì. Kết quả: còn nhiều chỗ cùng loại, và **một chỗ nghiêm trọng hơn ca gốc**.

---

## 1. Phát hiện nặng nhất — một cổng an toàn đang báo "đạt" trong khi nó mù

Dự án có một cổng tự động quét dữ liệu cá nhân trong các tệp được quản lý phiên bản. Cổng này **nối vào bước kiểm trước mỗi lần lưu thay đổi**, tức nó chạy liên tục.

**Vấn đề:** cổng lấy danh sách tệp bằng một lệnh **thiếu một tham số**. Hệ quả: với mọi đường dẫn có **dấu tiếng Việt**, công cụ quản lý phiên bản trả về chuỗi ở dạng **mã hoá thoát** thay vì chuỗi thật. Hai bộ mẫu nhận diện của chính cổng đó vì vậy **không khớp được**.

Thí nghiệm đối chứng chạy trực tiếp trên hai bộ mẫu của cổng:

| Chuỗi đưa vào | Mẫu phần mở rộng | Mẫu thư mục nguồn |
|---|---|---|
| Dạng mã hoá thoát (thứ cổng **thật sự** nhận) | **không khớp** | **không khớp** |
| Dạng thật (nếu thêm tham số còn thiếu) | khớp | khớp |

⇒ **Cổng mù với mọi đường dẫn có dấu tiếng Việt.** Trong một dự án Việt Nam, đó là một lớp tệp rất lớn.

**Hậu quả đo được:** một tệp dữ liệu nguồn **đang được quản lý phiên bản** dù luật cấm — và cổng canh việc đó vẫn báo **0 vi phạm**. Quy mô: **1.232 dòng dữ liệu**, **2.368 số điện thoại**, 22 địa chỉ thư điện tử, 2.432 mã số thuế, cùng một số cột thông tin thanh toán. Tệp đã được đồng bộ lên kho từ xa.

> Đây **chính xác** là ca 20/08 lặp lại, ở mức nặng hơn: luật bắt buộc một điều, cổng báo đạt, còn thứ luật bảo vệ thì đang hỏng.

**Chưa vá** — phiên này là phiên rà soát chỉ-đọc. Đã ghi sổ nợ, chờ chủ dự án quyết.

**Ghi nhận trung thực:** chính lệnh kiểm đầu tiên của người rà **cũng bỏ lọt** vì đúng lý do đó. Chỉ phát hiện được nhờ một lượt phản biện độc lập chạy lại bằng cách khác.

---

## 2. Còn 8 nghĩa vụ dạng khái niệm chưa được cổng phủ

Cổng kiểm tham chiếu (vá 20/08) hiện phủ **6** đối tượng bắt buộc và đang **đạt**. Nhưng rà toàn bộ hơn 2.000 dòng luật cho thấy còn **8 nghĩa vụ** khác mà đối tượng của chúng **không được nêu bằng đường dẫn tệp**, nên nằm ngoài tầm cổng:

| Nhóm | Số lượng | Trạng thái |
|---|---:|---|
| Biểu mẫu bắt buộc **chưa từng được điền lần nào**, và **không có nơi lưu được chỉ định** | **5** | ⛔ 0 lần |
| Sổ được luật gọi đích danh là một **nguồn sự thật** nhưng **không có hiện vật nào** | **1** | ⛔ không tồn tại |
| Biểu mẫu bắt buộc mới được dùng **đúng 1 lần** | 1 | ⚠️ |
| Đối tượng nằm ngoài kho mã, phiên chỉ-đọc không truy được | 1 | UNVERIFIED |

**Điểm chung:** luật định nghĩa **biểu mẫu** và **thời điểm** phải điền, nhưng **không nói biểu mẫu điền xong nằm ở đâu**. Không có nơi lưu ⇒ không ai điền ⇒ không cổng nào kiểm được. Đây là biến thể của cùng một lỗi: lần trước thiếu **nguồn tra cứu**, lần này thiếu **nơi chứa đầu ra**.

---

## 3. Chế tài thật sự được bao nhiêu

| Nhóm | Số luật |
|---|---:|
| **Tự chạy, không cần ai gõ lệnh** (nối vào bước kiểm trước khi lưu) | **3** |
| Khai "tự động" nhưng cổng chỉ chạy khi có người chủ động gõ lệnh | 6 |
| **Khai "tự động" mà cổng không kiểm đúng thứ luật đòi** | **2** |

Điểm tích cực có thật: rà từng tệp cho thấy **13/13 cổng đều đọc dữ liệu thật**, không cổng nào chạy trên chuỗi mẫu viết sẵn — tức lỗi "cổng giả" phát hiện hồi 18/08 **không tái diễn**.

Hai điểm hỏng:
- Một luật khai chế tài "tự động (đếm trường)" nhưng **không tồn tại cổng nào** làm việc đó. *(Lỗi này thực ra đã được ghi sổ từ trước — phiên 23/08 phát hiện lại một cách độc lập, rà chéo mới thấy trùng, và đã loại khỏi danh sách nợ mới.)*
- Một luật có cổng "tự động" nhưng cổng **kiểm nhầm đối tượng**: nó chứng minh **tệp mẫu** còn đó, chứ không chứng minh gói việc nào đã thực sự lập bảng theo mẫu.

**Phát hiện ngoài đề bài:** một bộ kiểm tra khác đang **hỏng 6/37 phép thử**. Nguyên nhân: bước kiểm-trước-khi-lưu thêm ngày 22/08 can thiệp vào kho thử của chính bộ kiểm đó. **Một cổng làm hỏng một cổng khác** — và vì nó không nằm trong chuỗi cổng chạy chung nên không ai thấy.

---

## 4. Sổ nợ kỹ thuật — có nội dung thật, nhưng lệch lược đồ

**Điểm tốt:** sổ đang sống thật — hàng chục dòng nợ, 14 dòng lịch sử sửa đổi, cập nhật liên tục tới ngày rà.

Ba vấn đề:
1. Sổ dùng **2 trạng thái mà luật không định nghĩa**. Nhiều khả năng **luật thiếu** chứ không phải sổ sai — nhưng hiện luật và sổ đang lệch, và không cổng nào bắt.
2. Có **3 mã nợ bị cấp trùng** — mỗi cặp là hai nợ khác chủ đề hoàn toàn, do hai phiên khác nhau cùng cấp một dãy số. Hệ quả thật: một trích dẫn mã nợ không xác định được là nợ nào.
3. Một việc bị chủ dự án cho dừng giữa chừng hồi 18/08 **chưa bao giờ được ghi vào sổ**.

---

## 5. Bốn câu hỏi định hướng — đều đã có đáp án

Cả bốn câu đặt ra ngày 18/08 **đều đã được trả lời** và đều ghi trong bộ luật, 5 bản đồng nhất tới từng byte.

Riêng câu về **nền tảng giao diện trả phí**: kết luận rõ ràng — **đã được xử lý đúng, KHÔNG phải lỗi cũ lặp lại**. Điều khoản bắt buộc gốc đã được gắn nhãn *lịch sử* và chuyển vào lưu trữ; cổng kiểm tham chiếu xử lý đúng phần trích dẫn còn lại như một **ca hỏng được nêu làm ví dụ**, không tính là tham chiếu sống.

Tồn tại một khoảng cách khác, đã ghi sổ: **luật đã xếp nền tảng đó vào diện lịch sử, nhưng mã nguồn vẫn còn phụ thuộc ở 12 tệp**.

Một điểm nhỏ: một sổ đăng ký nội bộ **vẫn ghi "chờ chủ dự án trả lời"** cho một câu đã được chốt từ 19/08 — sổ lỗi thời so với luật.

---

## 6. Việc nạp dữ liệu — tiền đề đã lỗi thời

Đề bài mô tả việc nạp khoảng 1.700 bản ghi là **việc phía trước, đang chờ duyệt**. Đo thật: **việc đã chạy xong**, trên máy nội bộ ngày 21/08 và **trên máy vận hành ngày 22/08**, kết quả báo lỗi 0%.

Câu hỏi vì vậy phải đảo lại: *ba luật mảng nạp dữ liệu có được thi hành khi việc đó diễn ra không?*

| Điều kiện luật đòi | Trạng thái |
|---|---|
| **Ngưỡng tỉ lệ lỗi được chủ dự án chốt** — luật ghi rõ *chưa chốt = chặn toàn bộ* | ⛔ **CHƯA CHỐT.** Ô điền trong quy trình vẫn để trống; con số duy nhất tìm thấy nằm trong **mẫu**, không phải quyết định |
| Cổng đối chứng số lượng hai đầu | ⚠️ Cổng có thật, nhưng nợ cũ vẫn mở: *chưa chạy trên một đợt nạp thật* |
| Cổng quét dữ liệu cá nhân phủ **tệp nguồn** | ⛔ **HỎNG** — xem mục 1 |
| Cổng quét phủ **bảng trung gian** | ⛔ Không có cổng nào chạm cơ sở dữ liệu |
| Bảng tiêu chí nghiệm thu cho việc nạp | ⛔ Không có — dù loại việc này nằm đúng trong điều kiện kích hoạt của luật |
| Quy tắc **gộp bản ghi trùng** | ⛔ Chưa có đáp án — công thức đối chứng có biến "gộp" nhưng không nơi nào định nghĩa **khi nào** gộp |
| Tệp nguồn bảng tính không bị quản lý phiên bản | ✅ **ĐẠT** |

**Kết luận:** đợt nạp lên máy vận hành đã diễn ra **khi ngưỡng lỗi chưa được chốt** (trường hợp luật quy định là chặn toàn bộ), **không có bảng tiêu chí nghiệm thu**, **quy tắc gộp chưa định nghĩa**, và **cổng quét dữ liệu cá nhân đang mù với chính tệp nguồn**.

Ghi nhận công bằng: kết quả đo được là **lỗi 0%**. Nhưng kết quả tốt **không** có nghĩa cổng đã hoạt động — nó có nghĩa lần này may.

---

## 7. Số liệu bản công bố có kiểm ngược được không

- **Không có cổng nào** kiểm mã băm/số liệu ghi trong bản công bố có khớp tệp thật hay không. Việc đối chiếu **hoàn toàn thủ công** — chính đó là lý do một mã băm sai trong bản 19/08 sống được một ngày.
- Kiểm ngược bản công bố mới nhất: mã băm danh mục kỹ năng **khớp**, số mục **khớp**; mã băm bộ luật **lệch** — đúng như thiết kế, vì bộ luật đã lên bản mới sau khi công bố. Nghĩa là **bản công bố hiện đang lỗi thời và chưa ai làm mới**.
- Cơ chế cảnh báo lỗi thời trong bản công bố **hoạt động đúng** — chính nó cho phép phát hiện điều trên chỉ bằng một phép so.

---

## 8. Kết quả phiên

| | |
|---|---|
| Luật ACTIVE đã duyệt | **34** (36 mã được nhắc; 32 khối định nghĩa chuẩn + 2 khối nội tuyến) |
| Hạ tầng luật trỏ tới | **tất cả đều tồn tại và có nội dung thật** — chỗ hỏng nằm ở khâu **thi hành**, không phải khâu tồn tại |
| Nợ mới ghi sổ | **16** |
| Phát hiện bị chính lượt phản biện **bác bỏ** | **3** — đã loại khỏi báo cáo |
| Nợ bị loại vì rà chéo thấy **trùng nợ cũ** | **1** |
| Đã sửa gì trong phiên | **KHÔNG** — phiên chỉ-đọc |
| Trạng thái | 🔴 **BLOCKED** — điều kiện dừng của chính phiên đã kích hoạt |

**Vì sao không ghi "đạt":** việc rà soát thì làm đủ, có dẫn chứng cho từng kết luận. Nhưng không thể ghi "đạt" khi phát hiện một tệp dữ liệu cá nhân đang nằm trong kho mã còn cổng canh nó thì mù.

**Hai việc gấp đang chờ chủ dự án quyết:** xử lý tệp dữ liệu nêu ở mục 1, và duyệt vá cổng — bản vá chỉ một dòng, nhưng **bắt buộc kèm phép kiểm ngược** chứng minh cổng thật sự bắt được lỗi sau khi vá.
