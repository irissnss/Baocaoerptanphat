# 🔒 Governance Log — ERP Tân Phát

> Lịch sử thay đổi governance rules, skills, architecture decisions, và system audit.
>
> **Cập nhật:** 25/08/2026 — **NÂNG LUẬT `2.7 → 2.8`**: thêm luật **bàn giao quyết định về Notion** — sổ yêu cầu chủ dự án là **kênh vận chuyển**, **mỗi chỉ thị một mục**, trường trạng thái đồng bộ nay **có bảng giá trị**, và **bắt buộc có gói bàn giao**. Kèm **ghi bù 3 chỉ thị bị bỏ sót**. *(Cùng ngày, trước đó: Pha C3.1.)*
>
> **Trước đó:** 25/08/2026 — **PHA C3.1**: ghi sổ **chín quyết định nền** của chủ dự án (Notion là sổ gốc · chính sách **MariaDB 10.11 LTS** cả hai môi trường · **101 bảng** · định nghĩa *"đang chạy thật"*) · **đo read-only** · sửa các kết luận **vượt quá bằng chứng** · đóng gói bàn giao Notion. **Runtime CSDL/phiên bản: chưa đo được — còn mở.** (Trước đó: Pha C3, Pha C2, Pha C.)

---

## 25/08/2026 — NÂNG LUẬT 2.7 → 2.8: BÀN GIAO QUYẾT ĐỊNH VỀ NOTION

**Chủ dự án nêu vấn đề:** đôi lúc **code đi trước tài liệu** vì trao đổi trực tiếp liên tục cho liền mạch. Điều đó **hợp lệ** theo luật sẵn có. Nhưng việc **ghi nhận các xác nhận và chia sẻ của chủ dự án** phải được chuẩn hoá, để trợ lý Notion **không bỡ ngỡ và không phản bác** khi nhận bàn giao.

**Ba khe hở đo được:**

| # | Khe hở | Hậu quả |
|---|---|---|
| 1 | Trường **trạng thái đồng bộ Notion** bị luật **bắt buộc phải ghi**, nhưng **không có bảng giá trị ở bất kỳ đâu** — bộ luật định nghĩa 5 trục trạng thái, **không trục nào là nó** | mỗi phiên **tự chế** giá trị ⇒ đúng thứ làm bên nhận bỡ ngỡ |
| 2 | Phiên có **nhiều chỉ thị nối nhau** thì không luật nào bắt **mỗi chỉ thị một mục** | đo được **3 trong 5 chỉ thị** của chuỗi vừa rồi **bị bỏ sót** |
| 3 | **Không** luật nào bắt tạo **gói bàn giao** có cấu trúc | code đi trước tài liệu là hợp lệ, nhưng trợ lý Notion **không có gì để nhận** |

**Luật mới — năm điều:**
1. **Sổ là kênh vận chuyển**, **không** phải nguồn sự thật cạnh tranh với Notion. Cấm viện dẫn sổ để bác nội dung Notion.
2. **Mỗi chỉ thị riêng biệt = một mục sổ**, kể cả khi cùng ngày cùng chủ đề. **Quyết định hoãn cũng là quyết định** — phải ghi.
3. Trường trạng thái đồng bộ **chỉ nhận năm giá trị**; riêng *"đã đồng bộ"* **chỉ** được ghi khi trợ lý Notion **đã xác nhận** — bên viết mã **không tự ghi**.
4. **Gói bàn giao bắt buộc** khi kết thúc phần việc có quyết định mới, và **phải có mục "điều chưa chứng minh được"** — để bên nhận **không nhận nhầm** thành sự thật hiện hành.
5. **Ghi bù phải giữ mốc thật** của sự việc. Cấm sửa mốc cho khớp ngày ghi.

**Đã ghi bù 3 chỉ thị bị bỏ sót**, mỗi mục ghi **cả ngày ghi bù lẫn mốc thật**, kèm nguyên văn lời chủ dự án — trong đó có lần chủ dự án **từ chối duyệt mù** và yêu cầu trình bằng chứng đầy đủ.

**Kiểm chứng:** năm tệp luật **giống hệt nhau từng byte** · số điều khoản **tăng, không giảm** (399 ≥ 386, và **từng tệp** đều không giảm) · mọi tham chiếu bắt buộc **đều tồn tại thật** (57/57) · chuỗi kiểm tổng **16 cổng** đạt · kiểm ngược **7/7** · chính sách phiên bản **37/37** · sổ **147 mục, không trùng mã**.

**Không xoá điều nào.** Hai luật sẵn có về sổ **giữ nguyên** — luật mới **bổ sung**, không thay thế. Thay đổi vùng cấm **bằng 0**.

---

## 25/08/2026 — PHA C3.1: ĐÓNG CHUẨN NGUỒN SỰ THẬT · CHÍNH SÁCH CSDL · BÀN GIAO NOTION

**Bối cảnh:** chủ dự án chốt một loạt quyết định nền ngày 25/08. Lượt này ghi sổ, **đo lại** những gì đo được, sửa các kết luận **vượt quá bằng chứng** ở lượt trước, và đóng gói phần kỹ thuật cho trợ lý Notion.

**Nguyên tắc xuyên suốt:** *có chính sách* ≠ *đang chạy như vậy*; *có hồ sơ triển khai* ≠ *đã đo thấy*.

**Đã ghi sổ — mục `#144`**, trạng thái **chờ đồng bộ sang Notion**: Notion là **sổ gốc chuẩn** · quyết định chốt ngay trong lúc làm là bằng chứng hợp lệ nhưng **phải chuyển về Notion** (sổ là *kênh vận chuyển*, không phải nguồn cạnh tranh) · **chính sách CSDL MariaDB 10.11 LTS cho cả hai môi trường** · **101 bảng** · **"đang chạy thật" chỉ được nói khi chính lượt đó đã đo** · chuỗi khép code/fix · ranh giới công khai · điều kiện Notion được chỉ đạo thẳng vào bảng/cột · **bảo tồn lịch sử**.

**Đo read-only (không mutation nào):**
- **Cấu hình** máy phát triển: ✅ **đúng là MariaDB**, ở cổng riêng — cổng cũ chỉ còn là **đường lui**.
- **Đang chạy thật** máy phát triển: ⛔ **không đo được** — dịch vụ **đang tắt**, và không chứng minh được việc bật lên sẽ không tự khởi tạo/di trú ⇒ **không bật**.
- **Đang chạy thật** máy vận hành: ⛔ **không đo được** — lượt này **không được cấp** kênh đọc. Tệp cấu hình triển khai có tồn tại và đã bị git bỏ qua, **không mở**.
- ⇒ **Chưa kết luận được** khớp hay lệch chính sách. **Không** chuyển đổi, **không** sửa cấu hình.

**Sửa các kết luận vượt quá bằng chứng:**
- Bỏ câu *"tài liệu nền lỗi thời vì ghi chú trong kho 09/08 mới hơn"* — **sai thứ bậc nguồn**. Trình tự đúng: chính sách cũ → ghi nhận kỹ thuật 09/08 → **chủ dự án chốt chính sách 25/08** → tài liệu nền **chờ cập nhật**.
- M5: đổi từ *"đã tự động"* sang **mức bằng chứng** — *có mã · có kiểm thử · có hồ sơ triển khai*, **chưa đo khi chạy thật**.
- Phiên bản: tách **có hồ sơ triển khai** khỏi **đã đo thấy đang chạy**.
- Bảng Kiến Trúc trong README từng **tự mâu thuẫn** với bảng Tech Stack (một bên ghi máy phát triển dùng hệ cũ) — đã thống nhất theo chính sách, khối lịch sử **giữ nguyên**.
- Khối *"chuỗi bán hàng CHƯA phát hành"* và khối *M5 ngày 25/07* đã gắn nhãn **đã được thay thế**, giữ nguyên văn.

**Đính chính công trạng:** lỗi bộ kiểm đếm nhầm dòng trong khối mã ví dụ — **phát hiện và vá đều thuộc Pha C2** (xác định bằng lịch sử mã; tệp bộ kiểm **không** nằm trong phần thay đổi của Pha C3). Tài liệu đã ghi đúng từ đầu; chỉ **lời tường thuật** cuối Pha C3 là gộp nhầm.

**Kiểm thử:** chuỗi kiểm tổng **16 cổng** đạt · kiểm ngược **7/7** · chính sách phiên bản **37/37** · hook chạy thật ở **cả hai** lượt ghi mã · thay đổi vùng cấm **bằng 0** trên mọi mục.

**Không đụng:** mã ứng dụng · số phiên bản · tệp kỹ năng · năm tệp luật · cấu hình CSDL · cơ sở dữ liệu · máy vận hành · Notion. Phiên này **không có** kết nối Notion nên **không** tuyên bố đã đọc Notion.

**Trạng thái:** **HOÀN TẤT** phần quản trị/tài liệu · **xác minh runtime CÒN MỞ** · **sẵn sàng bàn giao Notion: CÓ**, nhưng chỉ phần chính sách và ánh xạ **đã chứng minh** — phần *đang chạy thật* **không** bàn giao như sự thật hiện hành.

**Chi tiết:** [PHA-C3-1-DONG-CHUAN-NGUON-SU-THAT-20260825.md](PHA-C3-1-DONG-CHUAN-NGUON-SU-THAT-20260825.md)

---

> 🔧 *Sửa định dạng 25/08/2026: dòng dưới vốn là câu tóm tắt ở đầu tệp (mốc 24/08). Lượt sửa trước vô tình biến nó thành một tiêu đề mục. Trả về đúng dạng ghi chú, **giữ nguyên văn**.*
>
> **Mốc cập nhật 24/08/2026 — PHA C3:** đồng bộ sổ số phát hành theo **ba lớp bằng chứng** · đóng khoản nợ về sổ đó · hoà giải **99 ↔ 101 bảng** · nêu đúng mức tự động hoá tồn kho **M5 theo từng nhánh** · phân định bốn con số **14/16/4/7**. **Còn 3 việc thiếu bằng chứng + bàn giao Notion.** (Cùng ngày, trước đó: Pha C2, Pha C.)

---

## 24/08/2026 — PHA C3: AUDIT SỰ THẬT HIỆN HÀNH · XỬ LÝ CUỐN CHIẾU

**Bối cảnh:** sau Pha C2, tài liệu còn vài chỗ **nói hai điều khác nhau về cùng một sự việc**. Đợt này rà từng chỗ theo ba mức: có bằng chứng thì sửa ngay; đang đúng thì để nguyên và ghi rõ vì sao; **thiếu bằng chứng thì không đoán**, ghi rõ thiếu gì và cần phép đo nào.

**Kết quả 23 mục:** 13 đang đúng · **7 đã sửa** · 1 hai nguồn nói khác nhau · **2 còn thiếu bằng chứng**.

**Bảy mục đã sửa:**
- **Sổ số phát hành nội bộ** dừng ở V1.00.351 → bổ sung đủ **V1.00.352→355**, mỗi đợt có mã phát hành + mốc giờ + mã đã lên máy vận hành. Tách **ba lớp bằng chứng**: *số trong mã nguồn* · *có hồ sơ phát hành* · *đã lên máy vận hành* — và **nói rõ giới hạn** của lớp thứ ba (dựa hồ sơ triển khai, không phải phép đo trực tiếp).
- **Đóng khoản nợ** về sổ đó, đúng điều kiện đã ghi khi mở nợ.
- **README nêu hai phiên bản như hiện hành** → chỉ còn một; mốc go-live cũ gắn nhãn **lịch sử**, không xoá.
- **"99 bảng"** → **101 bảng**, kèm giải thích: **lệch thời điểm**, không phải đếm sai. Giữa hai mốc có thêm **2 bảng**; chứng minh cả hai là **bảng thật** chứ không phải khung nhìn bằng chính câu lệnh tạo bảng trong mã.
- **M5 "còn thiếu tự động hoá tồn kho"** → nêu **từng nhánh**. Câu cũ **sai với 3 trong 5 nhánh**: giao hàng, phiếu nhập, phiếu xuất **đều đã tự động** (có chặn xuất vượt tồn và tự sinh bút toán đảo khi huỷ); chỉ *kiểm kê* mới một phần và *mua hàng → công nợ* còn thủ công. Xác định bằng cách liệt kê **mọi** nơi ghi sổ kho rồi truy ngược người gọi.
- **Danh sách việc treo của Pha B** → thêm bảng đối chiếu trạng thái hôm nay, giữ nguyên bảng cũ làm bằng chứng lịch sử.
- **Bốn con số 14/16/4/7** → tách rõ mỗi số đo cái gì.

**Một mục hai nguồn nói khác nhau — hệ quản trị CSDL.** Tài liệu nền bên ngoài ghi máy phát triển dùng một hệ, tài liệu trong kho ghi **cả hai máy dùng chung một hệ từ 09/08/2026**. Đo được: tài liệu nền phản ánh trạng thái **trước 09/08**; trong kho có quyết định ghi ngày đổi. ⇒ **tài liệu nền lỗi thời**, KHÔNG phải mã chạy sai chính sách. **Không sửa cấu hình.**

**Hai mục còn thiếu bằng chứng:** hệ quản trị CSDL *đang chạy thật* (cả máy phát triển lẫn máy vận hành) và **tách bảng với khung nhìn**. Cả hai cần một phép đo mà phiên quản trị **không được phép** chạy. Mỗi mục có ghi **cách đóng cụ thể**.

**Kiểm thử:** chuỗi kiểm tổng **16 cổng** đạt · kiểm ngược cổng chống trùng **7/7** · chính sách phiên bản **37/37** · hook chạy thật ở **cả bốn** lượt ghi mã.

**Cố ý KHÔNG làm:** sửa mã nghiệp vụ · tệp kỹ năng · cơ sở dữ liệu · máy vận hành · số phiên bản · năm tệp luật · cấu hình CSDL · Notion. Không gọi công cụ tra tài liệu ngoài (audit quản trị nội bộ thuộc điều kiện **cấm gọi**). Không viết lại lịch sử.

**Trạng thái:** **HOÀN TẤT MỘT PHẦN** · **chưa sẵn sàng bàn giao Notion** (3 việc trên ảnh hưởng trực tiếp nội dung sẽ bàn giao).

**Chi tiết:** [PHA-C3-AUDIT-TRANG-THAI-HIEN-HANH-20260824.md](PHA-C3-AUDIT-TRANG-THAI-HIEN-HANH-20260824.md)

---

## 24/08/2026 — PHA C2: VÁ MỤC #53 · NỐI CỔNG CHỐNG TRÙNG · ĐÍNH CHÍNH MỐC COMMIT PHA B

**Bối cảnh:** Pha C dựng xong cổng chống trùng mã nhưng **cố ý chưa nối** vào chuỗi tự động, vì sổ Yêu Cầu Owner còn một mục trùng chưa được quyết. Đợt này xử lý nốt.

**Đã làm:**

- **Tiếp quản phần việc của một phiên đã ngưng.** Một mục sổ (ngày 24/08) còn nằm ngoài lịch sử mã. Đã thẩm định (mã duy nhất · dãy số liên tục · không dữ liệu cấm · đủ trường bắt buộc) rồi **bảo toàn bằng một mốc riêng**, không trộn vào việc khác.
- **Vá mục `#53`.** Hai việc khác nhau cùng mang số 53 từ 16/08. Xác định ai lấy số trước bằng **chuỗi thời gian dựng hoàn toàn từ tệp đã lưu ở cả hai kho** — không dựa vào thứ tự dòng trong một lần lưu (hai dòng vào chung một lần nên thứ tự dòng *không* nói lên thứ tự cấp số). Kết quả: mục **kiểm tra nạp luật giữ số 53**; mục **thêm trường chống bẫy nén phiên** nhận **số kế tiếp còn trống**, tính lại từ sổ. Nội dung · ngày · người thực hiện **giữ nguyên**, chỉ đổi số, kèm dòng ghi rõ lý do. **Không thay thế hàng loạt.** Báo cáo công khai đang trỏ tới mục giữ số 53 nên **không phải sửa dòng nào**.
- **Nối cổng chống trùng** vào chuỗi kiểm tự động (**15 → 16 cổng**) và hook chạy trước mỗi lần ghi mã (**3 → 4 cổng**), kèm bộ kiểm **chống gỡ âm thầm**.
- **Kiểm ngược 5 → 7 kịch bản**, và hai kịch bản mới **bắt được lỗi thật**: bộ kiểm đang đếm cả dòng bảng nằm trong **khối mã ví dụ**, khiến một ví dụ minh hoạ bị tính thành lần cấp mã thứ hai. **Đã vá ở bộ kiểm, không vá ở dữ liệu thử.** Sau khi vá, số mục đọc được trên sổ thật không đổi ⇒ không làm rơi mục nào.
- **Đóng đúng khoản nợ** — xác định theo **nội dung**, không theo mã. Hai khoản mô tả đúng lỗi này đã đóng kèm bằng chứng; hai khoản mang mã gần giống nhưng **nói về việc khác thì giữ nguyên**.
- **Đính chính mốc commit của Pha B** dựa trên quan hệ cha–con thật trong lịch sử mã. Bản đầu ghi mốc hoàn tất là một commit **cha** của mốc thật, và đặt tiêu đề *"5 commit"* nhưng liệt kê **6 dòng**. Nay tách rõ: **5 mốc công việc**, cộng một commit ghi sổ và một commit chứa chính báo cáo; đồng thời phân biệt **mốc kỹ thuật · lượt công bố · lượt đóng dấu**. Nội dung mô tả từng commit **giữ nguyên**, không viết lại lịch sử, không ép ghi đè.
- **Nói lại cho chính xác** phần chú thích bản chụp luật: các tài liệu gốc trong bản chụp **không sửa một dòng nào**; riêng tệp mục lục được **bổ sung khối cảnh báo lịch sử**. Bản đầu nói gọn *"giữ nguyên vẹn, không sửa"* là **tự mâu thuẫn** với chính khối cảnh báo vừa thêm.
- **Cập nhật phần đầu README** về phiên bản đang chạy thật, sau khi đối chiếu **ba nguồn độc lập đều khớp**. Phần đầu còn dừng ở mốc 17/08 trong khi thân README và báo cáo phát hành đều đã nêu mốc mới hơn. Các khối cũ **giữ lại làm lịch sử**.

**Kiểm thử — bốn con số dưới đây ĐO BỐN THỨ KHÁC NHAU, không cùng mẫu số:**

| Con số | Đo cái gì | Ghi chú |
|---:|---|---|
| **16** | số **cổng trong chuỗi kiểm tổng** | chạy bằng một lệnh; đây là con số "bao nhiêu cổng" đúng nghĩa |
| **4** | số cổng chạy **trước mỗi lần ghi mã** | là **tập con** của 16 — chọn loại nhanh và chặn rủi ro không đảo ngược được |
| **7** | số **kịch bản kiểm ngược** *bên trong* cổng chống trùng mã | là **bài kiểm thử**, KHÔNG phải cổng |
| **37** | số phép kiểm bên trong bộ **chính sách phiên bản** | cũng là bài kiểm thử, không phải cổng |

*(Bản đầu của mục này ghi "14 cổng chạy riêng" — đó chỉ là số cổng được chạy lẻ trong lượt kiểm tay, **không phải một tập hợp có định nghĩa**. Đã bỏ cách diễn đạt đó vì dễ bị đọc thành một mẫu số thứ năm.)*

Hook chạy thật ở cả ba lượt ghi mã.

**Ghi nhận một nợ mới:** registry số phát hành nội bộ **lỗi thời** — còn ghi mốc cũ, thiếu bốn đợt gần nhất, trong khi ba nguồn độc lập đều cho số mới hơn. **Không tự sửa** vì ngoài phạm vi đợt này; cần một lượt cập nhật riêng kèm mốc triển khai từng đợt.

**Cố ý KHÔNG làm:** sửa mã nghiệp vụ · sửa tệp kỹ năng · sửa 5 tệp luật · chạm bản đồ phụ thuộc · kết nối thêm công cụ · gọi công cụ tra tài liệu ngoài (việc này là quản trị nội bộ — tệp trong máy và lịch sử mã mới là nguồn đúng) · viết lại lịch sử mã · ép ghi đè.

**Đồng bộ:** kho mã ✅ · kho báo cáo ✅ · Notion ⏳ **chưa — chờ bàn giao**, Agent IDE không được sửa Notion.

**Chi tiết:** [PHA-C-DONG-SO-CONTEXT7-TRIGGER-CHONG-TRUNG-MA-20260824.md](PHA-C-DONG-SO-CONTEXT7-TRIGGER-CHONG-TRUNG-MA-20260824.md)

---

## 24/08/2026 — PHA C: ĐIỀU KIỆN GỌI CONTEXT7 · CHỐNG TRÙNG MÃ SỔ · ĐÓNG SỔ PHA B

**Bối cảnh:** nối tiếp Pha B. Bộ luật bắt *đọc sổ công cụ trước khi gọi bất kỳ công cụ nào*, nhưng mục `context7` trong sổ đó chỉ có **4 dòng** — không có điều kiện được gọi, không có điều kiện cấm gọi, không có chính sách dữ liệu ra ngoài, không có ràng buộc phiên bản. Một dòng trạng thái **đọc như đã kiểm soát** nhưng **giá trị thi hành bằng 0**.

**Đã làm:**
- **Context7 (4 → 31 trường, không xoá trường cũ):** 6 điều kiện GỌI (phải đủ cả sáu) · 12 điều kiện CẤM GỌI (trúng một là không gọi) · ràng buộc phiên bản · chính sách dữ liệu *chỉ thông tin công khai của thư viện* · chống **chỉ dẫn lẫn trong dữ liệu** trả về · ranh giới quyền (lượt tra là chỉ đọc, không tự cấp quyền sửa mã/commit/triển khai) · thứ tự dự phòng đặt **mã nguồn thật trước** Context7.
- **Tách trục "sẵn sàng" khỏi "chế độ dùng":** đo **riêng 4 bề mặt**, cấm suy từ bề mặt này sang bề mặt kia. Cùng loại lỗi đã vá cho phần kỹ năng ở Pha B.
- **Đã chạy thử thật một lần có kiểm soát** trên một thư viện công khai đang dùng trong dự án: chỉ gửi tên thư viện + số phiên bản + tên hàm công khai; đối chiếu lại với định nghĩa kiểu cài trên máy → **khớp**; **không** phát sinh sửa mã.
- **Vá mã trùng `DEBT-097`** theo đúng tiền lệ sẵn có trong sổ (việc ghi trước giữ mã, việc cấp trùng sau nhận hậu tố `-B`, cả hai dòng ghi con trỏ sang nhau).
- **Cổng chống trùng mã** cho cả hai sổ, hiểu **hai quy ước cấp lại khác nhau** (số kế tiếp ở sổ Owner · hậu tố `-B` ở sổ nợ) nên không báo trùng giả trên bản đã vá đúng. **Kiểm ngược 5/5.**
- **Kiểm lại toàn bộ Pha B** bằng lệnh thật: 0 tệp mã nghiệp vụ bị đụng · 0 tệp kỹ năng bị đụng · 128/128 kỹ năng có nhãn hiệu lực · 5 tệp luật giống hệt từng byte · 0 lệnh "ma" · phiên bản **không tăng** (đúng chính sách).

**Cổng tự động:** chuỗi kiểm quản trị nâng lên **15 cổng** (thêm cổng Context7, kiểm ngược 4/4).

**Còn chờ chủ dự án — đúng một việc:** Sổ Yêu Cầu Owner có mục **`#53` bị hai việc dùng chung**. Đã dựng được chuỗi thời gian xác định bản nào lấy số trước (hoàn toàn từ tệp đã lưu ở hai kho). Cổng chống trùng **cố ý chưa nối** vào chuỗi kiểm tự động vì nối lúc này sẽ chặn mọi lần ghi mã.

**Cố ý KHÔNG làm:** sửa mã nghiệp vụ · sửa nội dung tệp kỹ năng · soát 127 kỹ năng *chưa soát* · làm mới bản đồ phụ thuộc · kết nối thêm công cụ · thêm khoá · sửa 5 tệp luật · sửa báo cáo lịch sử đã công bố · **đụng Sổ Yêu Cầu Owner** (có phiên khác đang ghi vào sổ này).

**Đồng bộ:** kho mã ✅ · kho báo cáo ✅ · Notion ⏳ chưa (thuộc phần việc của trợ lý Notion). **Không** tuyên bố toàn hệ thống đã đồng bộ khi phần Notion còn mở.

**Chi tiết:** [PHA-C-DONG-SO-CONTEXT7-TRIGGER-CHONG-TRUNG-MA-20260824.md](PHA-C-DONG-SO-CONTEXT7-TRIGGER-CHONG-TRUNG-MA-20260824.md) · [PHA-B-HOAN-TAT-20260823.md](PHA-B-HOAN-TAT-20260823.md)

---

> 🔧 *Sửa định dạng 24/08/2026: dòng dưới đây vốn là câu tóm tắt ở đầu tệp (mốc cập nhật 18/08). Lượt sửa trước vô tình biến nó thành một tiêu đề mục. Nay trả về đúng dạng ghi chú, **giữ nguyên văn**, không xoá chữ nào.*
>
> **Mốc cập nhật 18/08/2026 — KẾ HOẠCH IMPORT (PLAN-ONLY, đã nâng lên v2):** đối chiếu DB AppSheet cũ ↔ schema ERP để lập kế hoạch nạp 3 danh mục nền (Nhân viên → Khách hàng → Nhà cung cấp); v2 bổ sung cơ chế sinh mã tự động, cấp mật khẩu tạm bắt buộc đổi, đa địa chỉ + tự nhận diện tỉnh, và các quy tắc an toàn dữ liệu. **Chưa nạp dữ liệu — chờ Owner duyệt v2.** (Trước đó: tài liệu tham chiếu 32 sheet; V1.00.350 tồn kho vật tư.)

---

## 18/08/2026 — KẾ HOẠCH IMPORT 3 DANH MỤC NỀN TỪ APPSHEET (PLAN-ONLY, chưa thực thi)

**Bối cảnh:** Owner giao khai thác DB AppSheet cũ để nạp 3 danh mục nền cho ERP, đúng thứ tự **Nhân viên → Khách hàng → Nhà cung cấp**. Phiên này **chỉ lập kế hoạch** — không ghi dữ liệu, không sửa mã, không phát hành.

**Đã làm (đọc-đối chiếu):** đọc cấu trúc 3 bảng nguồn (Nhân viên, Khách hàng, Liên hệ) và đối chiếu với cấu trúc bảng đích trong ERP (nhân sự, tài khoản/vai trò, khách hàng + liên hệ + địa chỉ, nhà cung cấp). Lập bảng **map từng cột** + **ma trận THIẾU/ĐỦ/DƯ** + **quy trình thực thi** (làm sạch định dạng → bảng trung gian → chạy thử xuất mẫu → sao lưu → ghi local → kiểm đếm đối chứng → phương án hoàn tác).

**Số lượng danh mục (không kèm dữ liệu cá nhân):** Nhân viên ~17 · Khách hàng ~1.692 · Nhà cung cấp ~109 · Liên hệ ~1.764.

**Kết luận kỹ thuật (public-safe):** nhân sự và tài khoản đăng nhập là 2 bảng tách (quyền theo vai trò, không lưu theo từng người); khách hàng trong ERP đã có sẵn bảng con liên hệ + địa chỉ nhiều dòng; nhà cung cấp cấu trúc phẳng hơn. Một số điểm cần Owner quyết trước khi nạp (bổ sung phòng ban còn thiếu, cột ngân hàng, quy tắc trùng mã số thuế, giá trị mặc định bắt buộc). **KHÔNG** nạp cột số liệu tính toán (ERP tự tính) và **KHÔNG** mang mật khẩu cũ.

**An toàn:** file dữ liệu gốc + mọi thông tin cá nhân **KHÔNG** đưa lên kho công khai. Chờ Owner duyệt kế hoạch mới thực thi trên máy phát triển.

---

## 18/08/2026 — TÀI LIỆU: Tham chiếu DB AppSheet "Tân Phát Packaging" → nền tảng ERP

**Bối cảnh:** Owner cung cấp file DB đang vận hành thật bằng AppSheet (nhiều năm) để khai thác làm nền cho ERP.

**Đã làm:** phân tích toàn bộ **32 sheet** (đọc trực tiếp bằng openpyxl), rút **cấu trúc + danh mục cấu hình + thống kê quy mô**, viết tài liệu tham chiếu bài bản: [`APPSHEET-DB-REFERENCE.md`](./APPSHEET-DB-REFERENCE.md) — bản đồ thực thể/luồng nghiệp vụ, danh mục nền (KH/NCC/SP/vật tư/nhân viên/công đoạn), chứng từ (báo giá→đơn→giao→nghiệm thu→công nợ/thu chi), bộ trạng thái, ánh xạ sheet→module ERP, nhận định + vấn đề chất lượng dữ liệu.

**Điểm đáng giá rút ra:** KH và NCC gộp chung 1 danh bạ (phân bằng "Hạng mục"); **vật tư đã có sẵn mô hình tồn nhập-xuất-tồn** (khớp 100% quyết định "sổ kho" của ERP); 24 công đoạn sản xuất chuẩn; ~200 loại chất liệu giấy; quy ước trạng thái đánh số rõ ràng; và các vấn đề cần chuẩn hoá khi migrate (trùng danh mục do hoa/thường, quy trình sản xuất free-text).

**An toàn dữ liệu:** tài liệu đã **public-safe** — GỠ toàn bộ credential (token tích hợp), số liệu tài chính, và thông tin cá nhân (tên/SĐT/địa chỉ KH, mật khẩu nhân viên). File `.xlsx` gốc là dữ liệu nội bộ, **KHÔNG** đưa lên kho công khai.

---

## 18/08/2026 — PHÁT HÀNH V1.00.350 (Kho Hàng: nối TỒN VẬT TƯ cho Phiếu Nhập / Phiếu Xuất)

**Bối cảnh:** Owner chuyển hướng từ chỉnh giao diện sang **hoàn thiện chức năng thật** của module Kho Hàng. Rà soát toàn module cho thấy: **không có dữ liệu giả** (tốt), nhưng nhiều thao tác **chỉ lưu phiếu mà không tác động tồn kho** — nhập kho không làm tăng tồn, xuất kho không làm giảm tồn và không chặn xuất quá số còn.

**Quyết định Owner:** theo dõi **tồn vật tư kiểu SỔ KHO** (tồn = tổng nhập − tổng xuất, ghi vào sổ kho sẵn có) — **không thêm bảng mới, không đổi cấu trúc dữ liệu**.

**Đã làm (KHÔNG đổi cấu trúc dữ liệu):**
- **Phiếu Xuất** khi *Xác Nhận* → ghi sổ xuất, **trừ tồn**; **CHẶN nếu xuất vượt tồn** (báo lỗi rõ, phiếu không được xác nhận); *Hủy* phiếu đã xác nhận → tự **hoàn tồn** (bút toán đảo).
- **Phiếu Nhập** khi *Hoàn Thành* → ghi sổ nhập, **cộng tồn** (chỉ vật tư; thành phẩm theo mô hình riêng); *Hủy* phiếu đã hoàn thành → tự **gỡ tồn**.
- Chạy trong 1 giao dịch an toàn + khóa dòng chống ghi trùng (bấm lại không cộng/trừ 2 lần).
- **Sửa 2 lỗi tiềm ẩn** ở tạo dòng phiếu nhập (giá trị mặc định sai kiểu → trước đây tạo dòng nào cũng lỗi).

**Bằng chứng:** bộ kiểm thử mới **15/15 đạt** trên CSDL thật (nhập tăng tồn · xuất giảm tồn · chặn vượt tồn · hủy hoàn tồn · chống ghi trùng) + kiểm thử nền giao hàng 13/13 + 5/5 · kiểm kiểu 0 lỗi · build đạt.

**Phát hành:** V1.00.349 → **V1.00.350** — sao lưu CSDL trước · **không đổi cấu trúc dữ liệu** · **99 bảng khớp** · **đăng nhập 200**.

**Còn chờ Owner quyết chính sách:** Mua Hàng → sinh công nợ nhà cung cấp / phiếu chi; Kiểm Kê → sinh phiếu điều chỉnh tồn.

---

## 18/08/2026 — PHÁT HÀNH V1.00.349 (Đồng bộ giao diện M5 — đợt 2: 4 trang chứng từ)

**Bối cảnh:** tiếp nối đợt 1, Owner chọn **đồng bộ luôn nhóm 4 trang chứng từ** (Phiếu Nhập · Phiếu Xuất · Giao Dịch Kho · Kiểm Kê) vốn đang dùng thanh tiêu đề bảng màu xám/phẳng — khác nhóm danh mục.

**Đã làm (chỉ giao diện — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):**
- **Phiếu Nhập · Phiếu Xuất:** thanh tiêu đề bảng phẳng → **dải màu cam chuẩn** (chữ trắng) + **bo góc trên** + bảng nằm trong khung bo góc.
- **Giao Dịch Kho · Kiểm Kê:** thanh tiêu đề bảng xám → **dải cam chuẩn** (giữ tính năng dính khi cuộn) + khung bo góc; **dải màu đầu trang** đổi từ xanh dương → **cam** cho khỏi chọi màu với bảng.

**Kết quả:** toàn bộ 10 trang khu Kho Hàng nay **dùng chung một kiểu thanh tiêu đề bảng màu cam**.

**Mức làm & phần còn nợ (khai rõ):** đợt này chỉ đồng bộ **thanh tiêu đề bảng** — CHƯA dựng lại bố cục/panel chi tiết cho nhóm chứng từ (để sau, tránh rủi ro).

**Bằng chứng:** ảnh chụp 6 trang — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt.

**Phát hành:** V1.00.348 → **V1.00.349** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200**.

---

## 18/08/2026 — PHÁT HÀNH V1.00.348 (Đồng bộ giao diện M5 — đợt 1: 4 trang danh mục)

**Bối cảnh:** sau khi trang Giao Hàng được duyệt, Owner chọn hướng tiếp theo: **đồng bộ giao diện các trang M5 còn lại** theo tài liệu chuẩn (`docs/UI-STANDARD.md`).

**Khảo sát:** 10 trang trong khu Kho Hàng — 2 trang đã chuẩn (Giao Hàng, Kho Thành Phẩm), 8 trang lệch. Chia 2 đợt.

**Đợt 1 — 4 trang danh mục** (Nhà Cung Cấp · Mua Hàng · NCC Vật Tư · Giá Vật Tư):
- **Thanh tiêu đề bảng**: trước mỗi trang một kiểu (dải cam-đỏ 2 màu, vuông góc) → nay **dùng chung 1 dải màu cam chuẩn 3 sắc** + **bo góc trên** + bảng nằm trong khung bo góc — đồng bộ với các trang mẫu.
- **Mức làm:** "đồng bộ nhìn nhất quán" — CHỈ chuẩn hoá phần dễ thấy nhất (thanh tiêu đề bảng), **KHÔNG đập lại bố cục/panel** để giảm rủi ro và tiết kiệm thời gian.

**Bằng chứng:** ảnh chụp 4 trang (dải cam chuẩn + bo góc) — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt.

**Phát hành:** V1.00.347 → **V1.00.348** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200**. *(Đợt 2 — 4 trang chứng từ: Phiếu Nhập · Phiếu Xuất · Giao Dịch Kho · Kiểm Kê — đang làm tiếp.)*

---

## 18/08/2026 — PHÁT HÀNH V1.00.347 (Giao Hàng: nhãn thống kê đầu trang đồng bộ 1 tiêu chuẩn)

**Bối cảnh:** Owner gửi ảnh trang mẫu Khách Hàng, yêu cầu: **"góc bo giống như hình — 1 tiêu chuẩn 1"** (khoanh cụm nhãn thống kê đầu trang: Tổng / Doanh Nghiệp / Cá Nhân).

**Nguyên nhân:** nhãn thống kê đầu trang mẫu dùng **bo góc vừa** (rounded-md); bản Giao Hàng trước đó lại để **bo tròn hẳn** (rounded-full) → lệch chuẩn.

**Đã sửa (chỉ giao diện — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):** 3 nhãn Tổng / Đang giao / Đã giao → đổi về **bo góc vừa** + kiểu nền/viền/chữ y hệt trang Khách Hàng (nền nhạt trong suốt nhẹ · viền cùng tông · số in đậm · biểu tượng cùng màu). Nhãn "Tổng" dùng xanh dương như trang mẫu.

**Tài liệu chuẩn:** cập nhật `docs/UI-STANDARD.md` mục bo góc — **tách rõ**: nhãn thống kê ĐẦU TRANG = bo góc vừa; còn nhãn trạng thái/loại TRONG HÀNG bảng = bo tròn hẳn.

**Bằng chứng:** ảnh chụp danh sách (5 dòng mẫu) — nhãn thống kê đã bo góc vừa khớp mẫu — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13).

**Phát hành:** V1.00.346 → **V1.00.347** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200** · trang Giao Hàng phản hồi đúng.

---

## 18/08/2026 — PHÁT HÀNH V1.00.346 (Giao Hàng: bo góc trên thanh tiêu đề bảng)

**Bối cảnh:** Owner phản hồi (ảnh khoanh) trang Giao Hàng: **thanh tiêu đề bảng (dải màu cam) vẫn vuông góc 90°** dù bản trước đã nói "đồng bộ bo góc".

**Nguyên nhân:** trang mẫu chuẩn (Kho Thành Phẩm) bo **2 góc trên** của dải cam bằng cách bo ô tiêu đề đầu (góc trên-trái) và ô tiêu đề cuối (góc trên-phải); trang Giao Hàng **thiếu đúng 2 điểm bo này** nên dải cam vuông.

**Đã sửa (chỉ giao diện — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):** bo góc trên-trái ô "Số Phiếu" + góc trên-phải ô "Thao Tác" → dải cam đầu bảng **bo góc trên mềm**, đồng bộ trang mẫu.

**Bằng chứng:** ảnh chụp danh sách (5 dòng mẫu) — dải cam đầu bảng đã bo góc trên — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13).

**Phát hành:** V1.00.345 → **V1.00.346** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200** · trang Giao Hàng phản hồi đúng.

---

## 17/08/2026 — PHÁT HÀNH V1.00.345 (Giao Hàng: đồng bộ bo góc + TÀI LIỆU CHUẨN GIAO DIỆN)

**Bối cảnh:** Owner phản hồi trang Giao Hàng **thiếu bo góc nhẹ** ở các điểm (đầu mục, thẻ…) và yêu cầu **một tài liệu chuẩn giao diện chi tiết** để mỗi lần xử lý giao diện về sau đỡ tốn thời gian — lấy 4 trang nền tảng (Sản Phẩm · Khách Hàng · Nhân Sự · Kho Thành Phẩm) làm gốc thống nhất.

**Đã làm (chỉ giao diện + tài liệu — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):**
1. **Tài liệu chuẩn giao diện** — viết lại `docs/UI-STANDARD.md` toàn diện **16 mục**: bảng **bo góc theo TỪNG loại phần tử** (thẻ/khung/đầu mục/bảng bo vừa · ô nhập/lọc bo lớn hơn · nhãn tròn/ảnh đại diện bo tròn hẳn · ngăn kéo form bo góc cạnh) · bảng màu + 6 bộ màu mục · quy ước khung ngoài/đầu trang/thống kê/thanh công cụ/bảng/phân trang/panel/form/nút/nhãn/chữ/responsive · phần tra nhanh — kèm lớp giao diện thật rút từ 4 trang nền tảng. Đây là **chuẩn dùng lại** cho các trang sau.
2. **Đồng bộ bo góc trang Giao Hàng**: nhãn thống kê → **bo tròn hẳn**; ngăn kéo form → **bo góc cạnh + đầu form bo** (nút đóng hiện rõ); các thẻ/khung/mục giữ **bo góc vừa** thống nhất.

**Bằng chứng:** ảnh chụp form (ngăn kéo đã bo góc) — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13).

**Phát hành:** V1.00.344 → **V1.00.345** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200** · trang Giao Hàng phản hồi đúng.

---

## 17/08/2026 — PHÁT HÀNH V1.00.344 (Giao Hàng: 4 điểm giao diện theo chuẩn)

**Bối cảnh:** Owner gửi ảnh khoanh 4 điểm trang Giao Hàng còn lệch chuẩn (so với trang mẫu):

**Đã sửa (chỉ trình bày — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):**
1. **Lề 2 bên còn rộng** — nguyên nhân: khung bao ngoài đã có lề sẵn, trang lại thêm lề nữa (cộng dồn). Bỏ lề thừa của trang → danh sách sát mép, tận dụng tối đa bề ngang.
2. **Nhãn thống kê** (Tổng/Đang giao/Đã giao) tách **xuống hàng riêng** dưới tiêu đề (thay vì chen cùng hàng).
3. **Gộp [thanh tìm kiếm + lọc + bảng + phân trang] vào MỘT khung** liền mạch (như trang Khách Hàng); ô tìm kiếm thu gọn.
4. **Form tạo/sửa phiếu**: thêm **đầu form nền màu** + chia thành các **mục có màu + biểu tượng** (Thông Tin Phiếu · Khách Hàng & Giao Nhận · Phương Tiện · Ghi Chú) — thay form phẳng.

**Bằng chứng:** ảnh chụp danh sách (màn 1920) + form + bảng chi tiết — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13).

**Phát hành:** V1.00.343 → **V1.00.344** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200**.

---

## 17/08/2026 — PHÁT HÀNH V1.00.343 (Giao Hàng: bảng chi tiết theo chuẩn giao diện, hết "sơ sài")

**Bối cảnh:** Owner phản hồi giao diện trang Giao Hàng **quá đơn sơ** so với chuẩn giao diện chung của dự án (các trang mẫu: Sản Phẩm · Khách Hàng · Nhân Sự · Kho Thành Phẩm). Bảng chi tiết cũ chỉ là các dòng "nhãn — giá trị" trơn.

**Đã dựng lại bảng chi tiết cho GIÀU & CHỈN CHU như trang mẫu (chỉ giao diện — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):**
- **Đầu bảng chi tiết**: tên khách hàng lớn + mã phiếu + **cụm nút thao tác tròn** (Sửa · Xóa · Đóng) + nhãn nhanh (trạng thái có chấm màu · số đơn hàng · tổng số lượng).
- **Thân**: chia thành các **THẺ MỤC có màu riêng + biểu tượng**: Thông Tin Chung · Khách Hàng & Giao Nhận · Phương Tiện · Số Lượng & Giá Trị (ô số liệu lớn) · Ghi Chú · Chi Tiết Sản Phẩm (kèm đếm + nút thêm).
- Đầu bảng danh sách chỉnh màu chuyển sắc chuẩn.

**Bằng chứng:** ảnh chụp bảng chi tiết ĐANG MỞ đặt cạnh trang mẫu Khách Hàng — lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13 + 5/5).

**Phát hành:** V1.00.342 → **V1.00.343** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200**.

---

## 17/08/2026 — PHÁT HÀNH V1.00.342 (Giao Hàng: siết lề, tận dụng tối đa bề ngang)

**Bối cảnh:** Owner gửi ảnh so sánh (Khách Hàng vs Giao Hàng): V1.00.341 tuy đã dùng toàn bộ bề ngang nhưng **lề trái/phải vẫn còn rộng** (~24px mỗi bên) so với mong muốn.

**Đã sửa (chỉ trình bày — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):** giảm lề hai bên của trang từ 24px xuống **12px** → **danh sách trải gần sát mép**, gần như dùng hết bề ngang khu làm việc.

**Bằng chứng:** ảnh chụp màn 1920px — bảng dùng gần hết bề ngang, lề còn ~12px mỗi bên. Lưu máy phát triển/nội bộ. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13).

**Phát hành:** V1.00.341 → **V1.00.342** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200**.

---

## 17/08/2026 — PHÁT HÀNH V1.00.341 (Giao Hàng: dùng toàn bộ bề ngang màn hình)

**Bối cảnh:** Owner phản hồi V1.00.340 vẫn **chưa tối ưu không gian làm việc**. Rà lại: trang Giao Hàng bị **giới hạn bề ngang ~1400px + căn giữa** → trên màn hình rộng bị **bỏ trống ~300px hai bên**. Trang mẫu Khách Hàng dùng **toàn bộ bề ngang** (không giới hạn).

**Đã sửa (chỉ trình bày — KHÔNG đổi nghiệp vụ/cấu trúc dữ liệu):** bỏ giới hạn bề ngang → danh sách **trải hết màn hình**; thu nhỏ biểu tượng tiêu đề; thu gọn khoảng cách; **vùng danh sách cao hơn** (thấy nhiều phiếu hơn).

**Bằng chứng:** ảnh chụp ở màn 1920px — bảng trải hết bề ngang (bản trước chừa ~300px trống bên phải). Lưu máy phát triển/nội bộ, không đưa lên kho công khai. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13 + 5/5).

**Phát hành:** V1.00.340 → **V1.00.341** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200**.

---

## 17/08/2026 — PHÁT HÀNH V1.00.340 (Giao Hàng: tối ưu không gian + xem nhanh master-detail)

**Bối cảnh:** đợt V1.00.339 làm lại giao diện nhưng Owner kiểm tra vẫn thấy **(1) lãng phí diện tích/mật độ thấp** và **(2) thiếu phần xem nhanh chi tiết**. Đợt này **học theo trang mẫu Khách Hàng** (Owner chỉ định).

**Đã làm (chỉ trình bày/giao diện — KHÔNG đổi nghiệp vụ, KHÔNG đổi cấu trúc dữ liệu):**
- **Tối ưu không gian:** bỏ 4 thẻ thống kê to (chiếm nhiều chỗ) → thay bằng **nhãn nhỏ gọn cạnh tiêu đề** (Tổng / Đang giao / Đã giao) · dòng bảng **dày hơn** · vùng danh sách **cao hơn** (thấy nhiều phiếu hơn cùng lúc).
- **Xem nhanh (master-detail):** chọn 1 phiếu → **chi tiết mở ngay bên phải danh sách** (không còn lớp phủ che màn hình); danh sách tự co lại nhường chỗ; trên điện thoại chi tiết xếp xuống dưới. Bấm lại dòng đang chọn để đóng.
- Giữ chuẩn chung: viết hoa tên, màu trạng thái, ngày đầy đủ, chống bấm trùng, responsive không tràn.

**Bằng chứng:** ảnh chụp TRƯỚC (V1.00.339) / SAU (mật độ mới + xem nhanh) ở 3 kích thước + đặt cạnh trang mẫu Khách Hàng — lưu **máy phát triển/nội bộ** (dữ liệu giả), KHÔNG đưa lên kho công khai. Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (13/13 + 5/5).

**Phát hành:** V1.00.339 → **V1.00.340** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200** · trang Giao Hàng tải đúng.

---

## 17/08/2026 — PHÁT HÀNH V1.00.339 (giao diện trang Giao Hàng + sửa lỗi treo)

**Bối cảnh:** đợt V1.00.338 có ghi "nâng giao diện" nhưng thực chất chỉ chỉnh nhẹ và **không kiểm bằng mắt**. Owner kiểm tra thực tế → giao diện chưa đạt chuẩn. Đợt này làm lại đúng chuẩn + **chứng minh bằng ảnh chụp** (trước/sau, 3 kích thước, dữ liệu giả trên máy phát triển).

**Sửa lỗi quan trọng (phát hiện khi chụp ảnh có dữ liệu):** trang Giao Hàng **bị treo/hiện lỗi khi danh sách có phiếu** (do cách xử lý sắp xếp theo ngày). Lỗi này **có sẵn từ trước**, bị che vì trước đó danh sách trống. **Đã sửa triệt để** — sắp xếp an toàn với mọi kiểu dữ liệu ngày.

**Bảng đối chiếu chuẩn giao diện (so với trang mẫu Kho Thành Phẩm):**

| Tiêu chí | Trước | Sau |
|---|---|---|
| Tiêu đề trang | Chữ trơn | Biểu tượng huy hiệu (icon-badge) chuẩn |
| Thẻ thống kê | Không có | 4 thẻ: tổng phiếu / đang giao / đã giao / tổng giá trị |
| Bảng danh sách | Cuộn ngang, tiêu đề không dính | Cuộn dọc chuẩn + tiêu đề dính khi cuộn |
| Panel chi tiết | Tiêu đề trơn | Đầu panel nền chuyển sắc cam (hero) + lớp nền mờ |
| Viết hoa tên | Chưa | Viết Hoa Đầu Chữ tên khách/sản phẩm |
| Màu trạng thái | Xám/đỏ lệch chuẩn | Theo bảng màu chung (nháp = hổ phách, huỷ = hồng…) |
| Định dạng ngày | 2 chữ số năm | Đầy đủ DD/MM/YYYY |
| Chống bấm trùng | Nút xóa chưa khoá | Khoá + báo "đang xóa" cho nút xóa phiếu/dòng |
| Điện thoại/máy tính bảng | — | Không tràn ngang; thẻ thống kê 2×2; bảng cuộn trong khung |

**Không đổi:** luồng nghiệp vụ giao hàng (tự trừ tồn / giao đủ-thiếu-vượt của V1.00.338) · cấu trúc dữ liệu.

**Bằng chứng:** ảnh chụp TRƯỚC (treo)/SAU (chuẩn) ở 3 kích thước — lưu **máy phát triển/nội bộ, KHÔNG đưa lên kho công khai** (dữ liệu giả). Kiểm kiểu 0 lỗi · build đạt · kiểm thử nền đạt (giao hàng 13/13 + giao thiếu 5/5).

**Phát hành:** V1.00.338 → **V1.00.339** — sao lưu CSDL trước · không đổi cấu trúc dữ liệu · **99 bảng khớp** · **đăng nhập 200** · trang Giao Hàng tải đúng.

---

## 17/08/2026 — PHÁT HÀNH V1.00.338 (Kho Hàng M5) lên vận hành thật

**Đợt phát hành gộp 3 phần (đã kiểm thử máy phát triển trước, rồi mới lên máy chủ):**
- **Lát 1** — Phiếu giao hàng xác nhận "Đã Giao" → **tự trừ tồn kho thành phẩm + tự ghi sổ kho** (nguyên tử · chống trùng · bút toán đảo khi hủy · chặn khi tồn không đủ · nút 1 bấm).
- **Lát C (đã đảo theo Owner)** — giao **đủ / thiếu / vượt số đặt đều được**; nghiệm thu theo **số thực giao**; đóng đơn thiếu/vượt ghi lý do vào ô Ghi chú; tồn kho vẫn là chặn cứng. Không dùng âm kho.
- **Giao diện** — nâng trang giao hàng theo chuẩn chung (viết hoa đầu chữ tên KH/SP, màu trạng thái chuẩn, chống bấm trùng khi xóa, cuộn danh sách + tiêu đề dính).

**Quy trình phát hành (an toàn):**
- **Sao lưu CSDL vận hành TRƯỚC** khi deploy + ghi điểm quay lui.
- Xác nhận **không thay đổi cấu trúc dữ liệu** (không migration mới).
- Deploy mã nguồn → build → khởi động lại dịch vụ.
- **Kiểm chứng:** **99 bảng khớp** · **đăng nhập 200** (cả tên miền lẫn nội bộ) · trang Kho/Giao hàng phản hồi đúng (chuyển hướng đăng nhập cho khách chưa đăng nhập) · dịch vụ chạy ổn. **Không tạo phiếu thử trên dữ liệu thật.**

**Số phiên bản:** V1.00.337 → **V1.00.338** (local = GitHub = máy vận hành).

**Kế tiếp (chưa làm):** Lát 2 mua hàng (đặt hàng → tự phiếu chi/công nợ, nhập 2 nhãn kho) · mở rộng trả trước NCC + upload chứng từ. Làm ở máy phát triển, kiểm thử xong mới phát hành đợt sau.

---

## 17/08/2026 — Kho Hàng M5 · Lát C: Giao thiếu / đóng đơn thiếu (đã code, local)

**Chỉ local — chưa triển khai, chưa đổi số phiên bản, KHÔNG đổi cấu trúc dữ liệu.**

**Bối cảnh:** Owner yêu cầu thi công theo thứ tự C → A → B. Kiểm tra trước: phần **A (trả trước nhà cung cấp)** cần chuỗi "đặt hàng → tự phiếu chi/công nợ" (Lát 2) — mà **Lát 2 chưa có code** (đã xác minh). Vì vậy **A tạm gác**, không tự làm thay; Owner chốt làm **C và B** trước.

**Đã làm (Lát C — giao thiếu, không đổi cấu trúc dữ liệu):**
- Khi nhập/ sửa dòng hàng trên phiếu giao: hệ **chặn giao VƯỢT tổng đặt** — cộng dồn số giao của **mọi phiếu cùng đơn + cùng sản phẩm** (bỏ phiếu đã hủy) không được vượt số đặt ghi trên dòng.
- **Cho phép giao THIẾU** so với đặt (sản xuất có hư hỏng) → đóng đơn thiếu được, ghi lý do vào **ô Ghi chú sẵn có** của đơn.
- **Giao bù đợt sau** = phiếu giao thứ 2 cùng đơn, cộng dồn, chặn khi vượt.
- Trừ tồn theo **số thực giao** giữ nguyên cơ chế Lát 1 (nguyên tử · chống trùng · đảo khi hủy · chặn khi tồn không đủ) — tồn kho là chặn cứng thứ hai.

**Bằng chứng (máy phát triển):** bộ kiểm thử mới **6/6 đạt** (giao thiếu cho phép · giao bù cộng dồn đúng · vượt tổng đặt bị chặn · sửa gây vượt bị chặn) · Lát 1 **13/13 đạt** (không vỡ) · kiểm kiểu 0 lỗi · build đạt.

**Kế tiếp:** **B — upload chứng từ**: tạo 1 bảng chứng từ dùng chung (migration diễn tập trên bản sao dữ liệu vận hành trước) · lưu thư mục riêng có kiểm quyền · bổ sung sao lưu thư mục chứng từ cùng lịch sao lưu CSDL.

---

## 16/08/2026 — Kho Hàng M5 mở rộng: trả trước NCC · upload chứng từ · giao thiếu (khảo sát + chốt hướng)

**Chỉ đọc + thiết kế — chưa code, chưa đụng cơ sở dữ liệu, chưa triển khai.**

**Ba yêu cầu mới của Owner:**
- **A. Trả trước nhà cung cấp**: một số NCC bắt trả tiền trước khi giao → quy trình mua cho phép chi tiền sớm gắn đơn.
- **B. Upload chứng từ**: mọi phiếu (giao hàng có chữ ký, chứng nhận hàng, phiếu chi…) cần chỗ đính kèm ảnh/PDF; up chứng từ xong mới coi là hoàn tất quy trình.
- **C. Đính chính "tồn âm"**: ý thật là **đơn có thể kết thúc với số lượng thiếu** (sản xuất có hư hỏng) → hỗ trợ **giao thiếu + đóng đơn thiếu có ghi nhận**, KHÔNG dùng cơ chế âm kho.

**Khảo sát (bằng chứng):**
- Cơ chế nối mua hàng → tài chính/kho (Lát 2) **chưa có code**.
- Chức năng **upload file chứng từ: chưa có** (không có nơi nhận file, không có cột đính kèm trên phiếu nào; ảnh sản phẩm chỉ là danh sách link đặt tay công khai).
- Trả trước & giao thiếu: **phần lớn tận dụng được cột/trạng thái sẵn có**, không cần thêm bảng.

**Owner đã chốt hướng:**
1. **Duyệt tạo 1 bảng chứng từ dùng chung** cho mọi phiếu (gọn hơn thêm cột vào từng bảng; có kiểm quyền + ghi ai up/khi nào + xóa mềm). Đây là **thay đổi cấu trúc dữ liệu duy nhất** của đợt này.
2. Chứng từ (giấy tờ tiền) lưu **thư mục riêng có kiểm quyền**, không để công khai.
3. Hoàn tất phiếu khi **chưa có chứng từ = cảnh báo nhưng vẫn cho** (không kẹt việc).
4. **Đóng đơn thiếu** ghi lý do vào **ô Ghi chú** sẵn có (không thêm cột).

**Thứ tự thi công (làm từng lát, test từng lát):** C (giao thiếu) → A (trả trước) → B (upload chứng từ).

**Còn chờ Owner (nối tài chính tự động):** hợp nhất vòng đời đơn mua · ai xác nhận phiếu · quy tắc gom nợ.

---

## 16/08/2026 — Kho Hàng M5 · Lát 1: Giao hàng tự trừ tồn thành phẩm + ghi sổ (đã code, local)

**Chỉ local — CHƯA triển khai, chưa đổi số phiên bản, KHÔNG đổi cấu trúc dữ liệu.**

**Owner chốt (khóa):** mô hình tồn kho **C** — thành phẩm theo dõi tồn đầy đủ; vật tư mặc định "nhập = dùng trực tiếp" (ghi 1 lần, chưa theo dõi tồn), có đường nâng cấp sau. Giao hàng **xuất từ kho thành phẩm**. Tiêu chí duyệt = **ít bước thao tác** (Lát 1: 3 bước → 1 bấm).

**Đã làm (Lát 1):**
- Khi phiếu giao hàng chuyển sang **"Đã Giao" (khách ký nhận)** → hệ **tự động** ghi sổ giao dịch kho (dòng xuất) và **tự trừ tồn kho thành phẩm** đúng số lượng giao — trong 1 giao dịch nguyên tử.
- Thêm **1 nút "Xác Nhận Giao Hàng"**: 1 bấm chạy toàn bộ (đổi trạng thái + trừ tồn + ghi sổ).
- **Chặn khi tồn không đủ** (mặc định an toàn): báo lỗi tiếng Việt rõ ràng, phiếu **không** bị đánh dấu đã giao (không trừ nhầm).
- **Hủy phiếu sau khi đã giao** → ghi **bút toán đảo** + hoàn tồn; **không** sửa/xóa dòng sổ cũ (sổ chỉ thêm, không sửa).
- **Chống trừ trùng** (bấm lại/chạy lại không trừ 2 lần).

**Bằng chứng (máy phát triển):** bộ kiểm thử mới **13/13 đạt** (trừ đúng · chống trùng · đảo · chặn tồn không đủ và tự hoàn nguyên) · kiểm kiểu 0 lỗi · build đạt · cổng đồng bộ luật đạt · cổng khép phiên 7/7.

**Còn chờ Owner (liền mạch — để Agent Notion đọc trực tiếp):**
1. **Hợp nhất vòng đời mua hàng**: bản 30/07 (*Nháp → Đã đặt → Đã nhận → Hoàn tất*) và mô tả 16/08 (*nháp → yêu cầu báo giá → thêm giá → gửi đơn → duyệt*) — gộp thành một luồng thống nhất thế nào?
2. **Kho đối tác / kho Tân Phát**: chỉ cần 2 nhãn trên phiếu, hay cần danh mục kho riêng?
3. **Gom đơn công nợ**: quy tắc gom nhiều đơn mua vào 1 phiếu chi/công nợ (theo nhà cung cấp? theo kỳ?).
4. **Ai xác nhận** phiếu nhập/xuất/giao (vai trò nào)?
5. **Tồn âm khi giao vượt tồn**: hiện mặc định CHẶN — Owner giữ chặn hay cho phép âm kèm cảnh báo?

→ **Lát 1 chưa triển khai** (chờ Owner duyệt đưa lên máy vận hành). Lát 2 (mua hàng) & Lát 3 (nối tài chính, gom đơn) chờ trả lời các câu trên.

---

## 16/08/2026 (vòng 2) — Khảo sát Kho Hàng M5 theo quy trình nghiệp vụ thật của Owner

**Chỉ đọc — chưa viết mã, chưa đụng cơ sở dữ liệu, chưa triển khai.**

**1. Truy hồi lịch sử (có bằng chứng, không nhớ đại).**
- Nguyên tắc **"ít bước thao tác — 1 bước làm được thì không tách 2 bước"**: không tìm thấy câu nguyên văn, nhưng **đúng tinh thần đó Owner đã nhắc nhiều lần** ("mở từng lớp rườm rà", "đẻ ra nhiều quá — gom gọn", "lẩn quẩn chồng chéo").
- Quy trình mua hàng: Owner từng chốt luồng một chiều **Nháp → Đã đặt → Đã nhận → Hoàn tất**, dùng thuật ngữ **"Công nợ"**, và **đơn chỉ tạo từ báo giá đã duyệt giá**.
- **Bốn chi tiết mới** (tự tạo phiếu chi khi chi trực tiếp · gom đơn công nợ · kho đối tác / kho Tân Phát · nhập để dùng trực tiếp không xuất vòng): **chưa có trong bất kỳ tài liệu nào** — Owner mới nêu 16/08, đã **ghi nhận và đưa vào danh sách cần Owner xác nhận** (không tự suy diễn).

**2. Đối chiếu mã thật.** Module Kho Hàng và Mua Hàng hiện là **các bảng rời rạc**: khi phiếu xác nhận **không** tự ghi sổ giao dịch kho, **không** tự trừ/cộng tồn, **không** nối sang phân hệ Tài chính (phiếu chi / công nợ); **chưa có danh mục kho**; **chưa có nơi lưu tồn vật tư** (chỉ thành phẩm có tồn). Giao hàng hiện chỉ **đọc** kho thành phẩm để hiển thị, **chưa trừ tồn** — đúng hướng "giao từ kho thành phẩm" nhưng thiếu bước tự trừ.

**3. Trình Owner: 8 câu hỏi chốt** (đáng chú ý: **hợp nhất vòng đời mua hàng** giữa bản 30/07 và mô tả 16/08; **vật tư dùng trực tiếp có cần theo dõi tồn hay chỉ ghi 1 lần** — quyết định này chi phối có phải đổi cấu trúc dữ liệu hay không).

**4. Thiết kế 3 lát — đo bằng số bước thao tác** (tiêu chí duyệt của Owner):
- **Lát 1** (giao hàng tự trừ tồn thành phẩm + ghi sổ): **giảm 3 bước → 1 bước**, **không đổi cấu trúc dữ liệu**.
- **Lát 2** (mua hàng: duyệt 1 lần tự sinh phiếu chi/công nợ, nhập dùng trực tiếp 2 đích kho): **giảm ~5 bước → 1 bước**; **đổi cấu trúc dữ liệu tùy câu trả lời của Owner → chỉ trình**.
- **Lát 3** (chống ghi trùng chặt + gom đơn công nợ): **giảm n bước → 1 bước**; **đổi cấu trúc dữ liệu → chỉ trình**.

→ **Chờ Owner trả lời 8 câu** trước khi làm Lát 1. Các phần đụng cấu trúc dữ liệu: **chỉ trình, chưa làm.**

---

## 16/08/2026 — Khép vòng luật khép phiên + mở màn khảo sát tồn kho M5

**Việc 1 — Thêm trường 11 vào khối "Báo cáo kết thúc" (luật khép phiên).**

| Nội dung | Kết quả |
|---|---|
| Trường mới | "Nén phiên & đọc lại tham chiếu" — buộc khai phiên có bị nén ngữ cảnh không; nếu có mà việc đụng đối tượng cần tham chiếu thì phải **đọc lại tài liệu chuẩn TRƯỚC khi kết thúc**, cấm kết luận bằng trí nhớ trước nén |
| Lý do | Luật gốc được hệ thống tiêm lại sau khi nén, nhưng tài liệu tham chiếu (chuẩn giao diện, sổ đăng ký, kho lưu trữ) thì **không** — nên phải chủ động đọc lại |
| Đồng bộ | 5 file quản trị ghi **cùng lúc, giống nhau từng byte** (một mã băm chung), cổng kiểm đồng bộ đạt |
| Cổng kiểm tự động | Nâng từ 10 → **11 trường**; bộ tự kiểm **7/7 đạt** (đủ 11 → đạt · thiếu 11 → bị bắt) |

**Việc 2 — Khảo sát tồn kho Module Kho Hàng (M5), CHỈ ĐỌC, chưa viết dòng mã nào.**

- Rà toàn bộ 5 nghiệp vụ kho (nhập kho · xuất kho · giao hàng · mua hàng · kiểm kê) đối chiếu mã nguồn thật.
- **Phát hiện chính:** khi phiếu được **xác nhận**, hệ hiện **chưa tự động** ghi sổ giao dịch kho và **chưa tự động** cập nhật số tồn — hai việc này đang phải làm tay. Sổ giao dịch kho gần như chưa được dùng tự động.
- Đã có **thiết kế đề xuất trên giấy** (chia 3 lát theo mức rủi ro): lát đầu (giao hàng tự trừ tồn thành phẩm + ghi sổ) **không cần đổi cấu trúc dữ liệu**; các lát sau (tồn vật tư, chống ghi trùng mức chặt) **cần đổi cấu trúc dữ liệu → chỉ trình, chưa làm**.
- **Chưa viết mã, chưa đụng cơ sở dữ liệu, chưa triển khai.** Toàn bộ chờ Owner duyệt trước khi làm bước đầu tiên.

---

## Probe Cursor 16/08/2026 (tối) — câu advisor «sửa danh sách / đọc tài liệu trước mã»

> ✅ **VERIFIED bởi Agent IDE (Cursor) / Cursor Grok 4.6** — đã đọc thật, không viết mã `src/`.

| Việc advisor kiểm | Kết quả Cursor |
|---|---|
| Khai tài liệu trước dòng mã đầu tiên | PASS — luật §V · chuẩn UI hiện hành · archive Master List/Detail/Title Case · 5 skill list tối thiểu · sổ #13/#14 |
| Sửa «một màn hình bất kỳ» ngay | **KHÔNG LÀM** — thiếu màn + thiếu lỗi → mutation BLOCKED |
| Quét hết skill / tự pick trang | **KHÔNG LÀM** — skill theo trigger; mẫu list hiện hành = trang kho thành phẩm |

Báo cáo đầy đủ + chữ ký: `PROBE-CURSOR-DOC-TRUOC-MA-DANH-SACH-20260816.md`.

---

## Probe Cursor 16/08/2026 (tối) — nạp luật + thiếu thông tin

> ✅ **VERIFIED bởi Agent IDE (Cursor) / Cursor Grok 4.6** — không giả định vendor.

| Câu hỏi Owner | Trả lời đã xác minh |
|---|---|
| Đang tự nạp file luật nào? | **`.cursorrules`** (entry). Canonical = `TANPHAT_AGENT_RULESET`. 5 replica byte-identical, sha256 `c009a4d7…`. `.cursor/rules/*.mdc` chỉ bổ trợ. |
| Khi thiếu thông tin phải làm gì? | Discover trước → `PROVISIONAL` (chỉ-đọc) / `BLOCKED` (mutation) → hỏi Owner **một câu tối thiểu**. Cấm đoán schema/path/business rule. |

Báo cáo đầy đủ + chữ ký: `PROBE-CURSOR-NAP-LUAT-VA-THIEU-THONG-TIN-20260816.md`.

---

## Mô Hình Governance HIỆN HÀNH — 5 REPLICA NGANG HÀNG (GOV-FIVE-REPLICA-SYNC-001)

> ✅ **HIỆN HÀNH (chốt 16/08/2026):** 5 file quản trị (`.cursorrules` · `.antigravityrules` · `AGENTS.md` · `CLAUDE.md` · `GEMINI.md`) là **5 bản sao NGANG HÀNG, giống hệt nhau từng byte** — **KHÔNG file nào là "chủ"**. Ghi **cùng một lượt** cùng nội dung; kiểm đồng bộ bằng **mã băm sha256** (parity). Mục đích: Chủ dự án đổi công cụ IDE/AI bất kỳ, Agent bắt nhịp ngay.
>
> **3 phiên 16/08/2026:** (1) nâng cấp VNext (4.151 dòng) → (2) **khép vòng gọn còn 1.400 dòng** (phần lịch sử dời sang kho lưu trữ, trạng thái sang thư mục registry) → (3) cập nhật 2 cổng kiểm cho khớp cấu trúc mới. Mô hình 5 file **không bị loại bỏ**.

---

## Governance Files — mô hình CŨ (SUPERSEDED)

> ⏭️ **SUPERSEDED 16/08/2026 bởi mô hình 5 REPLICA NGANG HÀNG ở trên.** Khối "AGENTS.md là master, 4 file sync theo" dưới đây là **lịch sử** — nay KHÔNG còn "file chủ". Giữ nguyên làm lịch sử.

| File | Vai trò (cũ) | Size |
|------|---------|------|
| `AGENTS.md` | Master rules — 14 sections | ~87KB |
| `CLAUDE.md` | Sync 100% với AGENTS.md | ~87KB |
| `GEMINI.md` | Sync 100% với AGENTS.md | ~87KB |
| `.cursorrules` | Sync 100% với AGENTS.md | ~87KB |
| `.antigravityrules` | Sync 100% với AGENTS.md | ~87KB |

**Quy tắc (cũ):** Khi update 1 file → tự động sync sang 4 files còn lại. Verify bằng SHA256 hash.

---

## Governance Rules (14 sections trong AGENTS.md)

| # | Tên | Mô tả |
|---|-----|-------|
| 0 | CORE PRINCIPLES | SSOT, Notion MCP first, version+changelog bắt buộc, verify 100% |
| 1 | LANGUAGE | Tiếng Việt mặc định, ghi rõ nguồn, text-first |
| 2 | READ-FIRST ORDER | .cursorrules → AGENTS.md → SKILL.md → Notion → FullSpec |
| 3 | THINK BEFORE DO | Phân tích trước, hỏi khi mơ hồ |
| 4 | DATA SAFETY | Không xóa data gốc, không chia sẻ ngoài workspace |
| 5 | SCOPE CONTROL | Anti-scope-creep, CẤM refactor/drop/rename tự ý |
| 6 | PLAN LABELING | Plan ID, IN/OUT scope, LOCKED, Plan Ledger |
| 7 | 2-PHASE COMMIT | Phase 1 Plan-only → Phase 2 Implement |
| 8 | OUTPUT FORMAT | 10 sections bắt buộc mỗi report |
| 9 | EVIDENCE RULES | DB/UI/Code evidence, text-first |
| 10 | UNREADABLE POLICY | UNREADABLE → yêu cầu ảnh zoom/OCR |
| 11 | UI & FORMAT RULES | Title Case, DD/MM/YY, VN number format, Metronic mandatory |
| 12 | PRICING GUARDS | MM-only, Combo Gate, 2-key, markup% SSOT |
| 13 | COMPLETION GATE | Plan Ledger + Proof + Test tối thiểu |
| 14 | PUBLIC REPORT SYNC | Báo cáo sau mỗi code/fix/audit/deploy |

---

## Pre-Check Bắt Buộc (8 gates)

| # | Gate | Mô tả |
|---|------|-------|
| 0 | Quét Skills | Scan `.cursor/skills/`, chọn skill phù hợp |
| 1 | Quét Tài Liệu | Đọc 7 files governance + SSOT |
| 2 | SSOT Lock | Không đoán, không phát minh, bám docs |
| 3 | Code-Test-Fix Local | Code/test trên local trước |
| 4 | Title Auto Case | TẤT CẢ UI text dùng SSOT functions |
| 5 | Search Normalization | Tất cả search/filter hỗ trợ không dấu |
| 6 | Architecture Lock | Server Actions + Server Components + SSE |
| 7 | DB SSOT | 🕰️ *(ghi tại thời điểm đó)* MySQL là nguồn duy nhất, no mock — **phần "nguồn duy nhất, no mock" vẫn còn hiệu lực**; riêng **tên hệ quản trị** nay là **MariaDB 10.11 LTS** theo chính sách chủ dự án chốt 25/08/2026 |

---

## Skills Inventory (60+ skills)

### Categories:
| Category | Số lượng | Ví dụ |
|----------|----------|-------|
| UI/UX Patterns | 20+ | premium-table-styling, detail-panel-layout, status-color-mapping, mobile-responsive-ui-patterns |
| Module Scaffolding | 5+ | scaffold-module, transactional-page-redesign, premium-module-page-redesign |
| Data/Schema | 10+ | schema-migration-safe, fk-safe-delete-guard, bundle-transaction-pattern, phased-migration-with-backfill |
| Search/Filter | 5+ | search-normalization, inline-filter-bar-layout, searchable-multiselect-popover |
| Governance | 10+ | versioning-change-history, skill-mining-governance, text-first-report, ssot-verification-before-code |
| DevOps/Debug | 5+ | debug-systematic, windows-next-cache-stability, mysql-schema-extraction |
| Form/Input | 5+ | form-field-validation, autocomplete-input-component, implement-g2-ux |
| Architecture | 5+ | server-client-split-pattern, async-await-conversion, in-memory-to-db-migration |

---

## Sync Events Log

| Ngày | Version | Event | Verify |
|------|---------|-------|--------|
| 14/06/2026 | V0.216 | Full system audit + public report update | ✅ |
| 08/05/2026 | V0.216 | Add rule 14 PUBLIC REPORT SYNC + create public repo | SHA256 MATCH ✅ |
| 10/03/2026 | V0.184 | Restore full governance sync sau freeze investigation | Verified |
| 04/03/2026 | V0.146 | Sync governance addendum across 5 files | MD5 verified |
| 04/03/2026 | V0.145 | Sync governance files sau AGENTS change | Verified |
| 27/01/2026 | V0.26 | 5-WAY SYNC governance files | MD5 hash verified |
| 27/01/2026 | V0.25 | Synced 2 new skills to all 5 files | Verified |
| 26/01/2026 | V0.16 | Establish 3-layer version tracking + 5-way sync rule | Initial setup |

---

## Architecture Decisions

| Quyết định | Mô tả | Version | Locked |
|-----------|-------|---------|--------|
| Server Actions + Server Components | Không tách FE/BE theo REST API | V0.05+ | 🔒 LOCKED |
| SSE (Server-Sent Events) | Cho real-time features | V0.05+ | 🔒 LOCKED |
| Metronic Demo 1 | UI backbone mặc định | V0.197+ | 🔒 LOCKED |
| Next.js 16 + React 19 | Framework chính | V0.153+ | 🔒 LOCKED |
| ~~MySQL~~ → **MariaDB 10.11 LTS** | DB engine | V0.00 → **cập nhật 25/08/2026** | 🕰️ **HISTORICAL — ĐÃ ĐƯỢC THAY** |
| Tailwind 4 | CSS framework | V0.05+ | 🔒 LOCKED |
| No mock data | DB SSOT duy nhất, mock disabled V0.18 | V0.18+ | 🔒 LOCKED |
| 5-way governance sync | 5 files phải identical | V0.16+ | 🔒 LOCKED |
| Foundation Components | PageCanvas, PageHeader, StatCard, FilterBar, SectionPanel | V0.155+ | 🔒 LOCKED |
| Shell Provider | AppShellProvider với presets + CSS variables | V0.158+ | 🔒 LOCKED |
| Dark Navy Sidebar | Compact MISA-style, single light theme | V0.175+ | 🔒 LOCKED |
| Standalone Runtime | PM2 + node .standalone-run/server.js on VPS | V0.143+ | 🔒 LOCKED |

---

## Security Boundaries (Rule 14)

> ⏭️ **SUPERSEDED 16/08/2026 bởi `GOV-PUBLIC-SAFE-001`.** Cách hiểu cũ dưới đây **chặn cả tên bảng/cột/route** — nay ĐƯỢC nêu **tên định danh kỹ thuật** (bảng/cột/route/module) để truy vết; chỉ **CHẶN**: credential/token/secret · dữ liệu cá nhân (PII) · dữ liệu nghiệp vụ dính tiền (tên khách, đơn giá, giá vốn, công nợ, doanh thu) · hạ tầng máy chủ (IP/cổng/đường dẫn/tên & phiên bản dịch vụ). Danh sách cũ giữ nguyên làm lịch sử.

### ĐƯỢC PHÉP public:
- ✅ Version number + changelog entries
- ✅ Module progress (tên + trạng thái)
- ✅ Tech stack tổng quan
- ✅ Thống kê tổng hợp
- ✅ Architecture decisions (high-level)

### NGHIÊM CẤM public:
- ❌ Source code
- ❌ Database schema (CREATE TABLE, columns, FK)
- ❌ API endpoints (paths, request/response)
- ❌ Credentials (.env, SSH keys, passwords)
- ❌ Business logic chi tiết (pricing formulas, workflow rules)
- ❌ Dữ liệu thật (khách hàng, đơn hàng, tài chính)
- ❌ Server/VPS info (IP, ports, paths)
- ❌ Governance files (.cursorrules, AGENTS.md...)

---

## System Audit Summary (14/06/2026)

### Deployment Status:
| Item | Status |
|------|--------|
| VPS Runtime | ✅ Online (HTTP 307 → login) |
| HTTPS/HSTS | ✅ Enabled |
| nginx | ✅ Running |
| PM2 standalone | ✅ Active |
| Domain | ✅ erp.intanphat.com |
| VPS Version | V0.215 (deployed) |
| Local Version | V0.216 (uncommitted governance change) |

### Codebase Metrics:
| Item | Count |
|------|-------|
| App routes (top-level) | 19 directories |
| Library files | 75 files |
| Components | 14 groups |
| Migrations | 50 files |
| npm scripts | 53 scripts |
| Governance files | 5 files (~87KB each) |
| Skills | 60+ |
| WORK_LOG | 188KB |

---

## Architecture Decision Records (ADR)

### ADR-20260705: Architecture Pivot — No NestJS, No Prisma, No Mobile App

| Field | Value |
|-------|-------|
| **Date** | 05/07/2026 |
| **Status** | ✅ Confirmed |
| **Decision** | Kiến trúc thật: Next.js 16 (Server Actions + Server Components) + mysql2 raw SQL + SSE. Không NestJS, không Prisma ORM, không mobile app. |
| **Rationale** | Dự án ban đầu dự kiến NestJS+Prisma backend nhưng Owner quyết định dùng Next.js Server Actions thuần để đơn giản hóa stack. MySQL trực tiếp qua mysql2 (no ORM). |
| **Evidence** | `package.json` — 0 references to @nestjs/*, prisma. `src/lib/db.ts` — mysql2 raw query. grep toàn repo confirm. |
| **Impact** | Docs/README đã sửa lại cho khớp thực tế. AGENTS.md Architecture Lock đã đúng. |

