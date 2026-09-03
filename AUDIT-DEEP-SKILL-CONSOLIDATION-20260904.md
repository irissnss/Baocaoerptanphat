# AUDIT SÂU 13 KỸ NĂNG — BẢO TOÀN, NÂNG CẤP VÀ ĐỊNH TUYẾN

> **Mã audit:** `AUDIT-ERP-SKILL-CONSOLIDATION-004` · **Gói việc:** `WP-ERP-SKILL-R0-01`
> **Phạm vi luật:** `RIÊNG — DỰ ÁN ERP TÂN PHÁT` · **Ngày:** 04/09/2026
> **Chế độ:** `CHỈ ĐỌC` — **KHÔNG** sửa/xoá/đổi tên/di chuyển/gộp/tách bất kỳ kỹ năng nào; **KHÔNG** đụng sổ đăng ký · luật · móc · cổng · quyền · mã nguồn · CSDL; **KHÔNG** gán nhãn còn-hiệu-lực hay tự-kích-hoạt; **KHÔNG** gọi kỹ năng; **KHÔNG** sửa Notion.
> **Triển khai:** `KHÔNG ÁP DỤNG` · **Đẩy mã nguồn riêng:** `KHÔNG ÁP DỤNG`

```
══════════════════════════════════════
  CHỮ KÝ AGENT — KHÔNG GIẢ MẠO
══════════════════════════════════════
  Vai trò : Agent IDE
  Công cụ : Claude Code 2.1.259 (tiện ích mở rộng chạy TRONG Cursor 3.18.25)
  Lane    : execution / IDE
  Actor   : Agent IDE — KHÔNG phải Agent Notion, KHÔNG phải Coordinator
══════════════════════════════════════
```

**PHIẾU KỸ NĂNG CHO LƯỢT NÀY** — `đã yêu cầu=KHÔNG` · `đã nạp=KHÔNG` · `chế độ=KHÔNG CẦN KỸ NĂNG` · lý do: đo và soát chính thư viện kỹ năng, nạp một kỹ năng chưa kiểm sẽ làm nhiễu phép đo.

---

## 1. TÓM TẮT ĐIỀU HÀNH — ĐỌC PHẦN NÀY LÀ ĐỦ HIỂU

Chủ dự án cho phép soi sâu **13 kỹ năng**, chỉ để **đọc và đề xuất**, không cho sửa gì. Em đã soi xong cả 13 và tách ra **193 dòng chức năng riêng biệt**. Kết luận gọn trong năm ý:

**Ý 1 — Không kỹ năng nào bị đề xuất xoá hay gộp. Toàn bộ 13 đều GIỮ LẠI.**
Kết quả: **11 kỹ năng CẬP NHẬT TẠI CHỖ** (giữ nguyên tệp, chỉ sửa đúng phần cần sửa) và **2 kỹ năng GIỮ NGUYÊN TUYỆT ĐỐI**. Không có kỹ năng nào bị thay thế, không có kỹ năng nào bị gộp.

**Ý 2 — Nhãn "đã bị thay thế" trên ba kỹ năng versioning là SAI so với hiện vật.**
Ba tệp `update-work-log`, `version-bump-on-feature`, `versioning-change-history` đều khai ở đầu tệp rằng chúng đã bị `versioning-auto-log` thay thế. Em đo lại và thấy **không đúng**: tệp được gọi là bản thay **KHÔNG chứa** ba biện pháp an toàn khi nhiều phiên ghi sổ cùng lúc (đọc lại tệp ngay trước khi ghi · chèn lên đầu không đè mục mới hơn · hậu kiểm không mất mục). Trớ trêu là chính tệp đó **tự khai có rủi ro ghi trùng** nhưng để trống biện pháp — mà biện pháp lại nằm nguyên ở tệp nó "thay". Ngoài ra còn **ba thứ độc nhất** khác: trường *vùng ảnh hưởng* chỉ có ở tệp lịch sử-thay-đổi, trường *bước kế tiếp* chỉ có ở tệp sổ công việc, mục *an toàn dữ liệu* chỉ có ở tệp đầu mối.

**Ý 3 — Hai kỹ năng đã bị CHÍNH CHỦ DỰ ÁN KHOÁ từ 23/08/2026: "không xoá, không sửa".**
`detail-panel-layout` và `annotated-screenshot-review` nằm trong nhóm 11 kỹ năng giao diện mà Chủ dự án đã chốt là *"thư mục gốc GIỮ NGUYÊN làm lưu trữ — không xoá, không sửa"*. Vì vậy em **BÁC** đề xuất gộp `annotated-screenshot-review` mà lượt phân rã đưa ra — nó vừa trái quyết định đã khoá, vừa **không kèm bảng ánh xạ** (vi phạm luật khoá số 7), vừa bỏ sót **5 khối tri thức** đo được là **không có** ở đích gộp.

**Ý 4 — Bốn mâu thuẫn số liệu mà gói việc yêu cầu làm rõ: đã đóng 5, còn 2 chờ Chủ dự án.**
Đáng chú ý nhất: chênh lệch **27 so với 31 khoá** của artefact danh mục **KHÔNG phải báo cáo sai** — hợp đồng được nâng cấp lúc **00:04**, còn artefact được nộp lúc **00:09** cùng ngày. Lệch đúng **5 phút**. Artefact tuân đúng hợp đồng đang hiệu lực khi nó sinh ra.

**Ý 5 — Em tìm thấy một mâu thuẫn nằm NGAY TRONG chuẩn giao diện.**
Trong cùng mục 10, cách nhau 7 dòng: điều bắt buộc số 3 (Chủ dự án chốt 03/09/2026) yêu cầu đổ bóng **mức vừa** và ghi rõ **không** dùng mức đậm; nhưng dòng đặc tả lưới (từ 23/08) **vẫn ghi mức đậm**. Ai đọc dòng nào thì làm theo dòng đó. Đây là việc nằm ngoài 13 kỹ năng nhưng quyết định giá trị nào đúng cho cả hai kỹ năng panel.

> ⚠️ **Một điều phải nói thẳng:** bộ phản biện bảo toàn của chính lượt này chấm bản đề xuất ban đầu là **KHÔNG ĐẠT**, với 6 lỗ hổng và 6 chức năng chưa có đích. Em **đồng ý** với phản biện đó và **đã sửa** trước khi trình: đổi phán quyết của `annotated-screenshot-review` từ *gộp* sang *giữ nguyên*, và chốt lại cụm versioning là **giữ đủ bốn tệp**, không dùng ngôn ngữ "lõi dùng chung" gây hiểu nhầm là gộp.

---

## 2. TRẠNG THÁI HIỆN TẠI VÀ MỨC BẰNG CHỨNG

| Hạng mục | Giá trị đo được | Lớp bằng chứng |
|---|---|---|
| Công cụ đang chạy | Claude Code 2.1.259 — tiện ích mở rộng **trong** Cursor 3.18.25 (kênh ổn định) | `CLIENT_PROVEN` |
| Nền chạy | Node 24 · Windows 11 Pro 10.0.26200 · x64 | `CLIENT_PROVEN` |
| Nhánh · quan hệ kho gốc | `main` · ngang bằng kho gốc, không đi trước cũng không đi sau | `CODE_PROVEN` |
| Cây làm việc | Có tệp đang sửa dở — **của phiên song song**, không phải của lượt này | `CODE_PROVEN` |
| Năm bản luật quản trị | **Trùng khớp tuyệt đối**, phiên bản tài liệu **2.9** — đo **hai lần** | `CODE_PROVEN` |
| Ba cổng kỹ năng | Đều **ĐẠT**: danh mục 4/4 · trạng thái nội dung 8/8 · đối chiếu chuẩn 6/6 | `CODE_PROVEN` |
| Số kỹ năng toàn thư viện | 128 thư mục = 128 tệp = 128 mục sổ đăng ký | `CODE_PROVEN` |
| 13 kỹ năng trong phạm vi | Phân giải đủ **13/13**, **0 trùng lặp**, **100% được Git theo dõi** | `CODE_PROVEN` |
| Tệp thi hành kèm 13 kỹ năng | **0** — không có mặt tấn công dạng chạy mã | `CODE_PROVEN` |
| Bằng chứng Sổ Yêu Cầu Chủ dự án cho 13 kỹ năng | **0/13** — không kỹ năng nào được nhắc trong sổ | `CODE_PROVEN` |
| Quyết định khoá tìm được ở NƠI KHÁC | 2 quyết định: bản ghi phiên bản của dự án (cụm versioning) · sổ đăng ký nguồn chuẩn giao diện (2 kỹ năng bị khoá không sửa) | `CODE_PROVEN` |
| Phát hiện bản địa — Claude Code | **PROVEN_FALSE** — phiên được cấp 17 kỹ năng dựng sẵn, **0/128** kỹ năng ERP | `CLIENT_PROVEN` |
| Phát hiện bản địa — Cursor | **NOT_CHECKED** — không có kênh đo | `NOT_CHECKED` |
| Đối chiếu Notion | Đọc trực tiếp **4/7** trang bắt buộc | `PARTIAL` |
| Checksum artefact danh mục trước đó | **KHỚP TUYỆT ĐỐI** khi tính lại trên đúng byte của Git | `REPORT_PROVEN` |

---

## 3. BẢN TIỀN-ĐỌC NOTION

**Đọc trực tiếp được Notion: CÓ.** Đã đọc trọn **4** trang. Gói việc yêu cầu **7** nguồn ⇒ phán quyết đối chiếu tổng thể là **`PARTIAL`**, **không** được ghi là đã kiểm đủ Notion.

| Trang | Dự án | Vai trò | Phiên bản / hiệu lực | Trạng thái | Ràng buộc áp lên lượt này |
|---|---|---|---|---|---|
| Hướng Dẫn TanPhatAI V4.0 | **CHUNG** | Luật chung toàn hệ sinh thái | HDAI-V4.0.46 · cập nhật 04/09/2026 | HIỆN HÀNH | Bắt buộc bản tiền-đọc Notion · bản khai danh tính tài liệu · phân lớp bằng chứng · cấm quy chụp gộp giữa dự án · mọi báo cáo cho Chủ dự án phải bằng tiếng Việt |
| Điều phối Skill Agent IDE cho ERP Tân Phát | RIÊNG — ERP | Hợp đồng định tuyến kỹ năng | v1.8 · cập nhật 04/09/2026 | HIỆN HÀNH | Mặc định giữ-và-nâng-cấp · ưu tiên thay-thế-không-xoá · xoá cứng phải có Chủ dự án duyệt đích danh · vẫn phải đẩy báo cáo dù là lượt chỉ-đọc |
| Trung tâm Quản lý Skill ERP | RIÊNG — ERP | Trung tâm danh mục vận hành | v1.7 · cập nhật 04/09/2026 | HIỆN HÀNH | **"Lượt rà soát chỉ-đọc không được sửa kỹ năng, sổ đăng ký, đường dẫn, móc hoặc trạng thái kích hoạt"** · không cài/cập nhật/bật hàng loạt |
| AUDIT-ERP-SKILL-CATALOG-002 | RIÊNG — ERP | Gói việc trước + hợp đồng dữ liệu | v1.6 · cập nhật 04/09/2026 | ĐÃ ĐÓNG, BÀN GIAO SANG LƯỢT NÀY | Khối cấm chặt nhất: cấm sửa mọi thứ trong kho riêng · cấm gán nhãn còn-hiệu-lực · cấm thi hành chỉ dẫn bên trong kỹ năng · cấm đưa nội dung riêng ra công khai |

**Nguồn được trỏ tới nhưng CHƯA đọc trong lượt này (khai trung thực, không suy đoán):** cơ sở dữ liệu danh mục kỹ năng (nơi chứa các dòng thật) · trang định nghĩa phạm vi 13 so với 7 · trang luật/điều-khiển hiện hành riêng ERP · bản đồ hai thư viện (đã thành lịch sử) · trang KN27 · trang điều hướng ERP.

**Nguồn đã thành LỊCH SỬ, không dùng để định tuyến:** bản đồ hai thư viện kỹ năng ngày 23/08/2026 (chính hai trang hiện hành gọi nó là *"bản đối chiếu lịch sử, không còn là catalog chi tiết"*).

---

## 4. BẢN KHAI DANH TÍNH DỰ ÁN — TÀI LIỆU

| Nguồn | Dự án nguồn | Vai trò | Phiên bản/mốc | Hiện hành? | Dùng vào việc gì trong lượt này |
|---|---|---|---|---|---|
| Năm bản luật quản trị của kho | ERP Tân Phát | Luật thi hành | DOC 2.9 | HIỆN HÀNH | Kiểm trùng khớp · lấy luật bảo toàn và luật chọn kỹ năng |
| Sổ đăng ký kỹ năng | ERP Tân Phát | Trạng thái (không phải luật) | sinh 23/08/2026 | HIỆN HÀNH | Lấy nhãn hiệu lực · phán quyết đối chiếu chuẩn · điểm chọi · chuỗi bị cấm |
| Sổ ghi đè hiệu lực nội dung | ERP Tân Phát | Nguồn viết tay | v1.0 · 23/08/2026 | HIỆN HÀNH | Xác nhận mặc định an toàn là chưa-soát |
| Sổ đăng ký nguồn chuẩn giao diện | ERP Tân Phát | Nơi Chủ dự án hạ nhãn nguồn chuẩn | 23/08/2026 | HIỆN HÀNH | **Tìm ra quyết định khoá "không xoá, không sửa" cho 2 kỹ năng** |
| Chuẩn giao diện dự án | ERP Tân Phát | Nguồn sự thật giao diện | mục 10 cập nhật 03/09/2026 | HIỆN HÀNH | Đối chiếu hai kỹ năng panel · **phát hiện mâu thuẫn nội bộ về đổ bóng** |
| Bản ghi phiên bản của dự án | ERP Tân Phát | Lịch sử thay đổi | mục V0.180 | LỊCH SỬ nhưng CÒN HIỆU LỰC | **Tìm ra quyết định khoá cho cụm versioning** |
| Sổ Yêu Cầu Chủ dự án | ERP Tân Phát | Kênh vận chuyển quyết định | 235 mục | HIỆN HÀNH | Tra bằng chứng ưu tiên — kết quả 0/13 |
| Báo cáo danh mục kỳ trước | ERP Tân Phát | Bằng chứng công khai | ngày 04/09/2026 | HIỆN HÀNH | Xác minh checksum · đối chiếu bốn mâu thuẫn số liệu |

**Kiểm chéo dự án:** tất cả nguồn đều thuộc **ERP Tân Phát**, trừ trang luật chung. **Không** mượn số liệu hay luật của dự án khác.
**Kiểm chéo vai trò tài liệu:** báo cáo công khai **không** được dùng làm nguồn sự thật; bản ghi phiên bản **không** được dùng làm điều-khiển hiện hành — nó chỉ dùng làm **bằng chứng có quyết định đã chốt**.

---

## 5. SỔ QUY TRÁCH NHIỆM TÁC NHÂN

| Việc | Tác nhân | Lớp bị đụng | Bằng chứng |
|---|---|---|---|
| Đọc 13 tệp kỹ năng và tài liệu liên quan | Agent IDE | chỉ đọc — không lớp nào bị đổi | nhật ký lệnh đọc |
| Đo mã băm, số dòng, nhãn sổ đăng ký | Agent IDE | chỉ đọc | kết quả lệnh |
| Chạy ba cổng kỹ năng | Agent IDE | chỉ đọc (đã soi mã cổng trước khi chạy) | kết quả cổng |
| Đọc 4 trang Notion | Agent IDE qua kênh đọc | chỉ đọc — **KHÔNG sửa Notion** | bản tiền-đọc |
| Tạo 3 tệp trong kho báo cáo công khai | Agent IDE | kho báo cáo công khai | commit của lượt này |
| Sửa kỹ năng / sổ đăng ký / luật / móc / mã nguồn / CSDL | **KHÔNG AI** — không thực hiện | không lớp nào | kho mã nguồn sạch phía em |
| Cập nhật Notion | TanPhatAI — **chưa làm** | chưa | Agent IDE không có quyền |

---

## 6. MA TRẬN ĐỐI CHIẾU BỐN LỚP

| Chủ đề | Notion | Mã nguồn riêng | Báo cáo công khai | Công cụ đã cài | Nhãn |
|---|---|---|---|---|---|
| Tổng số kỹ năng = 128 | ghi 128 | đo 128 | ghi 128 | — | **SYNCED** |
| 127 chưa soát · 1 ngủ đông · 0 còn hiệu lực | ghi đúng | đo đúng | ghi đúng | — | **SYNCED** |
| Claude Code thấy 0/128 | ghi đúng | — | ghi đúng | đo trực tiếp | **SYNCED** |
| Cursor chưa đo | ghi đúng | — | ghi đúng | không có kênh | **SYNCED** |
| 36 kỹ năng chọi chuẩn | **không có con số** | đo 36 | ghi 36 | — | **IDE_LEAD** |
| Chỉ số nội dung bị cấm | ghi là mâu thuẫn chưa đóng | đo 17 / 11 / 18 / 2007 | ghi 11 ở văn xuôi, 17 ở bảng | — | **ĐÃ GIẢI QUYẾT trong lượt này** |
| Số khoá artefact | hợp đồng 31 | — | artefact 27 | — | **TRỄ DO THỜI ĐIỂM** — đã giải thích |
| Phạm vi audit 13 hay 7 | tự mâu thuẫn trong cùng bản | — | — | — | **MỞ** — đã thi hành theo 13 |
| Nhãn bị-thay-thế của 3 kỹ năng versioning | chưa ghi nhận | **đo được là SAI hiện vật** | chưa ghi nhận | — | **IDE_LEAD — phát hiện mới** |
| Mâu thuẫn đổ bóng trong chuẩn giao diện | chưa ghi nhận | **đo được** | chưa ghi nhận | — | **IDE_LEAD — phát hiện mới** |

---

## 7. CỤM A — VERSIONING: MA TRẬN NĂNG LỰC BỐN KỸ NĂNG

**Phán quyết: `CẬP NHẬT TẠI CHỖ` — giữ đủ bốn tệp, KHÔNG gộp, KHÔNG thay thế.**

Ba căn cứ đo được:
1. **Nhãn bị-thay-thế sai hiện vật.** Tra sáu từ khoá về an toàn ghi song song trên tệp đầu mối cho **0 kết quả thật**.
2. **Bốn hợp đồng đầu ra KHÁC NHAU ở bốn trường khác nhau.** Trường *vùng ảnh hưởng* chỉ có ở tệp lịch sử-thay-đổi; trường *bước kế tiếp* chỉ ở tệp sổ công việc; trường *độ lộ diện* chỉ ở tệp tăng-số (trong phạm vi cụm); mục *an toàn dữ liệu* chỉ ở tệp đầu mối. Gọi là **chồng lấn** thì đúng, gọi là **trùng lặp** thì sai.
3. **Chính dự án đã chốt kiến trúc này rồi.** Bản ghi phiên bản mục V0.180 ghi: tệp đầu mối là quy trình chính; ba tệp còn lại giữ vai trò còn lại **có tên**: bản đồ trường · quyết định tăng số · ghi sổ an toàn khi chạy song song.

**Tổng số dòng chức năng đã phân rã: 59.** Bảng đầy đủ nằm trong artefact máy đọc; dưới đây là các dòng mang **giá trị độc nhất** hoặc **xung đột**:

| Kỹ năng nguồn | Chức năng / mục | Giá trị ĐỘC NHẤT | Lỗi thời / xung đột | Xử trí |
|---|---|---|---|---|
| `update-work-log` | Khai báo đầu tệp — trạng thái và quan hệ (dòn… | Là skill duy nhất trong bốn skill tự đặt mình vào vai trò "bảo vệ tính toàn vẹn tệp sổ"… | XUNG ĐỘT NHÃN — đo được: nhãn `superseded_by` hàm ý bản thay phủ hết, nhưng grep trên `… | **CAP_NHAT_TAI_CHO** |
| `update-work-log` | Mục đích của sổ công việc (mục Summary, dòng … | Định vị sổ công việc như một TÀI SẢN DỄ HỎNG cần quy trình bảo vệ riêng — ba skill kia … | không | **GIU_SPECIALIST** |
| `update-work-log` | Thời điểm cập nhật sổ và thứ tự thực hiện (Pr… | Nêu rõ THỨ TỰ hai bước (quyết định trước, ghi tệp sau) — versioning-auto-log không nêu … | không | **GIU_NGUYEN** |
| `update-work-log` | An toàn khi nhiều phiên chạy song song — đọc … | CÓ — đây là nội dung ĐỘC NHẤT, không tồn tại ở ba skill còn lại. Đo được: grep `re-read… | không — trái lại, khớp thực hành thật của dự án (Chủ dự án mở nhiều phiên song song trê… | **GIU_SPECIALIST** |
| `update-work-log` | Chèn khối mới LÊN ĐẦU tệp, không ghi đè mục m… | CÓ — quy tắc thứ tự "mới nhất ở trên" chỉ có ở đây. Đo được: grep `append\|top of` trên… | không | **GIU_SPECIALIST** |
| `update-work-log` | Chống ghi trùng / chống mất mục khi nghi ngờ … | CÓ — bước xác minh SAU KHI ghi (hậu kiểm) chỉ có ở đây. | không | **GIU_SPECIALIST** |
| `update-work-log` | Bộ trường bắt buộc của khối sổ (Procedure bướ… | Trường "bước kế tiếp" (Next Steps) là ĐỘC NHẤT — đo được: grep `next steps` trên versio… | không | **GIU_SPECIALIST** |
| `update-work-log` | Khuôn mẫu đầu ra khối sổ công việc (mục Outpu… | CÓ — khuôn mẫu này là ĐỘC NHẤT. Đo được: grep `author\|next steps\|files changed` trên … | không | **GIU_SPECIALIST** |
| `update-work-log` | Điều kiện kích hoạt thuận (frontmatter trigge… | Điều kiện "có sửa đồng thời/tranh chấp trên cùng tệp" là ĐỘC NHẤT trong bốn skill. | không | **GIU_NGUYEN** |
| `update-work-log` | Cổng ĐẠT / KHÔNG ĐẠT (mục PASS/FAIL) | CÓ — bốn điều kiện ĐẠT về tính toàn vẹn tệp là độc nhất; ba skill kia không có điều kiệ… | không | **GIU_SPECIALIST** |
| `update-work-log` | Ca kiểm thử tối thiểu (mục Minimal Test Cases) | Ca "phát hiện rủi ro sửa đồng thời" là độc nhất. | không | **GIU_NGUYEN** |
| `update-work-log` | Ghi chú tích hợp — quan hệ với skill khác (mụ… | không có | không — cả hai tên được trỏ đều tồn tại thật, đã kiểm 0 tham chiếu chết. | **GIU_NGUYEN** |
| `update-work-log` | Ranh giới quyền commit / đẩy mã / triển khai | không có | không — nhưng đây là khoảng trống, không phải xung đột. | **CAN_CHU_DU_AN_QUYET** |
| `update-work-log` | Báo cáo lên kho công khai · bàn giao Notion ·… | không có | KHOẢNG TRỐNG SO VỚI LUẬT — luật dự án §F1c buộc có gói bàn giao Notion khi kết thúc gói… | **CAN_CHU_DU_AN_QUYET** |
| `update-work-log` | Xử lý khi thất bại · đường lui · tính lặp-lại… | không có | không — là khoảng trống. | **CAN_CHU_DU_AN_QUYET** |
| `version-bump-on-feature` | Khai báo đầu tệp — trạng thái và quan hệ (dòn… | Là skill duy nhất tự định vị vai trò "phân định mức độ" (nặng/nhẹ) chứ không phải vai t… | XUNG ĐỘT NHÃN — bản thay KHÔNG phủ hết: đo được grep `redesign` trên versioning-auto-lo… | **CAP_NHAT_TAI_CHO** |
| `version-bump-on-feature` | Mục đích — trợ giúp phân định mức tăng phiên … | CÓ — đưa "thiết kế lại giao diện" (redesign) vào diện phải cân nhắc mức tăng. Đo được: … | không | **GIU_SPECIALIST** |
| `version-bump-on-feature` | Quy tắc tăng số phiên bản — ba mức (Procedure… | Vế "thiết kế lại đáng kể" ở mức vừa là độc nhất (bản thay chỉ ghi "mở rộng quy trình có… | XUNG ĐỘT VỚI LUẬT GỐC — luật dự án §I3 `GOV-VERSION-RELEASE-001` mục 2, 3, 4 chốt: "ghi… | **CAN_CHU_DU_AN_QUYET** |
| `version-bump-on-feature` | Chiều "độ lộ diện" của thay đổi (mục Output F… | CÓ — trường "độ lộ diện" là ĐỘC NHẤT trong cả bốn skill. Đo được: grep `visibility` trê… | không | **GIU_SPECIALIST** |
| `version-bump-on-feature` | Vòng phản hồi — trả kết quả phán mức về quy t… | CÓ — chỉ skill này mô tả một vòng đi-và-về hai chiều với quy trình chính; hai skill cũ … | không | **GIU_NGUYEN** |
| `version-bump-on-feature` | Điều kiện kích hoạt thuận (frontmatter trigge… | Điều kiện "quyết định phiên bản cho THIẾT KẾ LẠI" là độc nhất. | không | **GIU_NGUYEN** |
| `version-bump-on-feature` | Ca kiểm thử tối thiểu (mục Minimal Test Cases) | CÓ — ba ca này là ví dụ CỤ THỂ có thể chấm đúng/sai, khác với ca của bản thay (bản thay… | không | **GIU_SPECIALIST** |
| `version-bump-on-feature` | Ranh giới giữa "đổi mã" và "phát hành lên máy… | không có | KHOẢNG TRỐNG NGUY HIỂM Ở MỨC RỦI RO PHÁT HÀNH — skill được xếp rủi ro R3 (phát hành) nh… | **CAN_CHU_DU_AN_QUYET** |
| `version-bump-on-feature` | Tách số phiên bản mã nguồn khỏi số phiên bản … | không có | XUNG ĐỘT NGẦM VỚI LUẬT §I3 mục 4 — luật chốt thay đổi thuần tài liệu/báo cáo KHÔNG tự t… | **CAN_CHU_DU_AN_QUYET** |
| `version-bump-on-feature` | Báo cáo GitHub · bàn giao Notion · Sổ Yêu Cầu… | không có | không — là khoảng trống, không phải xung đột. | **CAN_CHU_DU_AN_QUYET** |
| `versioning-change-history` | Khai báo đầu tệp — trạng thái và quan hệ (dòn… | Là skill duy nhất tự định vị là TỪ ĐIỂN TRƯỜNG, không phải quy trình. | XUNG ĐỘT NHÃN — bản thay KHÔNG phủ hết: đo được trên `.cursor/skills/versioning-auto-lo… | **CAP_NHAT_TAI_CHO** |

### 7.1 Bản thiết kế nội dung đề xuất cho tệp ĐẦU MỐI

> ⚠️ Đây là **kế hoạch nội dung cho tệp đầu mối**, **KHÔNG PHẢI** bản gộp bốn tệp. Ba tệp hỗ trợ **giữ nguyên vị trí và giữ nguyên mục độc nhất của mình**. Bộ phản biện đã bắt đúng chỗ bản trình bày ban đầu dùng ngôn ngữ "lõi dùng chung" gây hiểu là gộp — em đã chốt lại cho rõ.

- **muc dich chinh:** TÊN NĂNG LỰC CANONICAL: DẤU VẾT PHIÊN BẢN (cụm versioning). Mục đích: bảo đảm mọi thay đổi có ý nghĩa đối với kho mã, với kỹ năng, với luật quản trị đều để lại dấu vết đủ để (a) hoàn tác, (b) kiểm toán lại về sau, (c) bàn giao cho người/tác nhân khác mà không cần hỏi lại. Mục tiêu chống lại: kho trôi dạt thành một đống thay đổi không có tài liệu. HÌNH DẠNG CANONICAL — QUAN TRỌNG: canonical KHÔNG …
- **trigger thuan:** HỢP NHẤT 13 điều kiện kích hoạt hiện có của bốn tệp (4 + 3 + 3 + 3), khử trùng, rồi ĐỊNH TUYẾN thẳng về đúng chế độ để không còn cảnh bốn tệp cùng tranh một việc. NHÓM A — VÀO CHẾ ĐỘ TOÀN CHUỖI (chủ trì: versioning-auto-log)   A1. Mã nguồn hoặc hành vi hệ thống đã đổi.   A2. Một kỹ năng vừa được tạo · bổ sung · gộp · tổ chức lại.   A3. Một luật, một quy trình, một tệp quản trị vừa đổi.   A4. Ngườ…
- **trigger nghich va dieu kien dung:** A. KHÔNG KÍCH HOẠT (giữ nguyên các điều kiện phủ định đang có, hợp nhất, khử trùng):   N1. Thử nghiệm cục bộ tạm thời, cố ý bỏ đi.   N2. Việc CHỈ ĐỌC, không đổi tệp nào.   N3. Sửa định dạng thuần tuý không đổi nghĩa.   N4. Dùng MỘT tệp chuyên trách để chạy thay cả chuỗi cuối việc (cấm ở cả ba tệp chuyên trách, mỗi tệp một câu chữ; hợp đồng chung giữ một câu duy nhất).   N5. Việc thuộc nhóm nạp dữ…
- **hop dong dau vao dau ra:** A. ĐẦU VÀO BẮT BUỘC (thiếu bất kỳ mục nào ⇒ chưa đủ điều kiện ghi):   1. Danh sách tệp đã đổi (đường dẫn tương đối).   2. LÝ DO đổi — viết TRƯỚC nội dung đổi (quy tắc thứ tự do đầu mối sở hữu): vì sao cần · giảm rủi ro gì · ảnh hưởng phạm vi nào.   3. Loại thay đổi đề xuất (mức) + căn cứ.   4. Phạm vi — chọn trong 5 giá trị: Giao diện · Nghiệp vụ · Dữ liệu · Tài liệu · Quản trị.   5. Vùng ảnh hưở…
- **cac che do doc lap:** Năm chế độ, chạy độc lập được, mỗi chế độ có đúng một tệp chủ trì và một đầu ra nghiệm thu riêng. Mục đích: một việc nhỏ không phải kéo cả cụm. CHẾ ĐỘ 1 — PHÁN MỨC (chủ trì: version-bump-on-feature)   Vào: mô tả thay đổi. Ra: khối phán mức (mức · lý do · độ lộ diện). Không ghi tệp nào.   Nghiệm thu: mức có căn cứ nêu được, độ lộ diện có nêu. CHẾ ĐỘ 2 — TRA TỪ ĐIỂN TRƯỜNG (chủ trì: versioning-chan…
- **chuoi thi hanh:** CHUỖI CHÍN BƯỚC. Cột "chủ trì" cho biết ai làm; cột "nguồn" cho biết bước đó đã có trong cụm hay là ĐỀ XUẤT MỚI. B1. VIỆC — xác định đã đổi cái gì, ở đâu, vì sao. Chủ trì: đầu mối. Nguồn: đã có. B2. PHIÊN BẢN — phán mức, rồi đối chiếu sơ đồ số của dự án và chính sách phát hành để quyết CÓ tăng số hay KHÔNG tăng số. Chủ trì: chuyên trách phán mức, đầu mối chốt. Nguồn: đã có phần phán mức; phần "có…
- **cong chi doc va cong mutation:** A. CỔNG CHỈ ĐỌC (được chạy tự do, không cần xin phép)   Việc: phán mức · tra từ điển trường · soát dấu vết · đối chiếu nguồn thật của kỹ năng.   Ràng buộc: đầu ra chỉ là BẢN NHÁP hoặc BẢN LIỆT KÊ, không chạm tệp nào.   Sản phẩm hợp lệ: khối phán mức, bảng nghĩa trường, danh sách chỗ thiếu dấu vết. B. CỔNG GHI TỆP (mutation) — phải đủ SÁU tiền đề trước khi ghi   T1. Đã có khối phán mức, và mức đó …
- **cong chat luong:** HỢP NHẤT BA BỘ CỔNG ĐANG CÓ, KHÔNG BỎ ĐIỀU NÀO. BỘ 1 — CỔNG NỘI DUNG DẤU VẾT (chủ trì: đầu mối; 4 điều kiện ĐẠT + 4 điều kiện KHÔNG ĐẠT)   ĐẠT: đã phân loại mức rõ ràng · đã viết VÌ SAO chứ không chỉ ĐỔI GÌ · đã cập nhật các tệp lịch sử liên quan · đã ghi bằng chứng xác minh.   KHÔNG ĐẠT: thiếu mức hoặc mức tuỳ tiện · bỏ qua sổ/nhật ký sau một thay đổi có nghĩa · đổi kỹ năng hoặc luật mà không cậ…
- **chong ghi trung:** NĂM BIỆN PHÁP. Ba biện pháp đầu là tài sản độc nhất của tệp update-work-log, giữ nguyên; hai biện pháp sau là đề xuất mới vá đúng rủi ro mà chính tệp đầu mối tự khai mà không tự chữa được. CT1. ĐỌC LẠI TỆP NGAY TRƯỚC KHI GHI — cấm dùng nội dung đã đọc từ trước đó trong phiên. (Có sẵn, giữ.) CT2. CHÈN MỤC MỚI LÊN ĐẦU TỆP, không đè mục mới hơn. (Có sẵn, giữ.) CT3. HẬU KIỂM SAU KHI GHI — khi nghi có…
- **xu ly that bai va duong lui:** Bảy tình huống hỏng và đường lui tương ứng. Toàn bộ mục này là ĐỀ XUẤT MỚI: đo được cả bốn tệp không có mục nào tên xử lý thất bại, đường lui, hay chạy lại. F1. GHI HỎNG TỆP SỔ (mất mục, sai thứ tự, khối vỡ). Đường lui: khôi phục tệp sổ từ bản đã lưu trong lịch sử kho, KHÔNG sửa đè lên bản hỏng; ghi lại từ đầu theo chuỗi. F2. PHÁT HIỆN MỤC BỊ MẤT SAU KHI GHI. Đường lui: ghi bù NGAY, gắn nhãn ghi …
- **ma tran kiem thu:** MƯỜI SÁU CA. Ca 1–13 là hợp nhất toàn bộ ca kiểm hiện có của bốn tệp (4 + 3 + 3 + 3), giữ nguyên ý, khử trùng. Ca 14–16 là mới, phủ các khoảng trống. Nhóm PHÁN MỨC   1. Chỉnh khoảng cách giao diện ⇒ mức nhỏ.   2. Thêm một kỹ năng quản trị có cổng dùng lại được ⇒ mức vừa.   3. Đổi một bước bắt buộc theo hướng phá vỡ tương thích ⇒ mức lớn, và chỉ sau khi Chủ dự án xác nhận.   4. Một đợt thiết kế lạ…
- **bo chuyen doi cursor:** BỐI CẢNH ĐO ĐƯỢC: kho có 128 thư mục kỹ năng dưới .cursor/skills/, và sổ đăng ký .governance/registry/skills.yml (do máy sinh) có mục cho cả bốn kỹ năng của cụm. Công cụ Cursor chọn kỹ năng dựa trên phần khai báo đầu tệp: tên · mô tả · điều kiện kích hoạt. BỘ CHUYỂN ĐỔI CHO CURSOR gồm bốn phần, tất cả nằm TRONG bốn tệp hiện có, không tạo tệp mới:   1. PHẦN KHAI BÁO ĐẦU TỆP giữ nguyên cấu trúc hiệ…
- **bo chuyen doi claude code:** BỐI CẢNH: Claude Code không tự quét thư mục kỹ năng của Cursor. Tác nhân chạy trong Claude Code nạp luật từ tệp quản trị gốc ở gốc kho và đi theo bảng chỉ mục tham chiếu trong đó. BỘ CHUYỂN ĐỔI CHO CLAUDE CODE gồm ba phần:   1. MỘT DÒNG CHỈ MỤC trong bảng chỉ mục tham chiếu của năm tệp quản trị: "Khi cần ghi dấu vết phiên bản / nhật ký / sổ công việc ⇒ đọc cụm bốn kỹ năng versioning, đầu mối là v…
- **loi dung chung moi cong cu:** LÕI BẤT BIẾN — phần không phụ thuộc công cụ nào, phải giống nhau dù chạy ở Cursor, Claude Code hay công cụ khác. Gồm năm thành phần:   L1. TỪ ĐIỂN TRƯỜNG: bảng 5 giá trị phạm vi · bảng vùng ảnh hưởng (mô-đun / thành phần / luồng công việc) · lý do · tóm tắt · tệp đã đổi · bước kế tiếp · tác nhân thực hiện · độ lộ diện. Một tên gọi duy nhất cho mỗi trường.   L2. SƠ ĐỒ SỐ PHIÊN BẢN CỦA DỰ ÁN: ba tầ…
- **alias va con tro lich su:** A. ALIAS — ĐO ĐƯỢC MỘT VA CHẠM THẬT   Sổ đăng ký hiện gán alias theo cách cắt tên: cụm từ "log" xuất hiện làm alias của HAI kỹ năng (kỹ năng ghi sổ và kỹ năng đầu mối); cụm từ "versioning" xuất hiện làm alias của HAI kỹ năng (đầu mối và từ điển trường). Va chạm alias khiến việc tra theo triệu chứng không phân biệt được ai làm gì.   ĐỀ XUẤT bộ alias không đụng nhau, theo VIỆC chứ theo tên:     · đ…

**Chức năng còn giá trị mà CHƯA có đích:** **không có** — nhưng bộ phản biện đã bổ sung thêm hai mục phải đưa vào danh sách bắt buộc giữ: **trường vùng ảnh hưởng** và **mục an toàn dữ liệu**.

---

## 8. CỤM B — PANEL CHI TIẾT: MA TRẬN BA PHƯƠNG ÁN

**Khuyến nghị: `GIỮ HAI CHUYÊN BIỆT`.** Ba bằng chứng đo được cho thấy đây **không phải** hai bản trùng nhau:
- **Điều kiện kích hoạt khác:** tệp bật-tắt có mục điều kiện kích hoạt và mục nên-dùng/tránh-dùng; tệp bố cục có **0** mục đó (sổ đăng ký cũng chấm là *thiếu điều kiện kích hoạt*).
- **Cấu trúc phụ thuộc khác:** **6 kỹ năng khác** trỏ về tệp bố cục; **0 kỹ năng** trỏ về tệp bật-tắt.
- **Đầu ra khác:** danh mục màu thẻ mục và cụm nút hero **chỉ có** ở tệp bố cục.

| Phương án | Ưu điểm | Nhược điểm | Rủi ro kích hoạt nhầm | Rủi ro mất tri thức | Đường lui |
|---|---|---|---|---|---|
| **GIU_HAI_SPECIALIST** | Rủi ro thấp nhất trong ba phương án. Không phá 6 tham chiếu đang trỏ vào tệp bố cục. Không sinh thêm tệp thứ ba cạnh tranh với ch… | Không giảm số tệp phải bảo trì. Vẫn còn hai bản sao của cùng một hàm bật-tắt ở hai nơi, nên khi chuẩn đổi cơ chế thì phải sửa ba … | THẤP và GIẢM so với hiện nay. Hiện tệp bố cục có 0 điều kiện kích hoạt nên bộ chọn kỹ năn… | BẰNG KHÔNG. Không câu nào bị dời, bị gộp hay bị viết lại. Đây là phương án duy nhất trong… | Đơn giản nhất trong ba phương án: hoàn tác thay đổi trên đúng hai tệp… |
| **GOP_MOT_SKILL_CO_CHE_DO** | Chỉ còn một nơi để tra và một nơi để sửa khi chuẩn đổi. Xoá hẳn tình trạng hai bản sao của cùng một hàm bật-tắt cho hai câu trả l… | Tệp gộp sẽ dài khoảng 550 đến 600 dòng và ôm quá nhiều việc: quyết định kiểu bố cục, máy trạng thái bật-tắt, hero, 8 màu thẻ mục,… | CAO. Một tệp ôm cả câu hỏi có nên dùng panel lẫn câu hỏi tô màu thẻ mục thế nào sẽ khớp v… | TRUNG BÌNH. Không mất nếu làm đủ ba việc: giữ tệp cũ nguyên văn với nhãn đã bị thay thế, … | TRUNG BÌNH. Vì tệp cũ không bị xoá nên khôi phục được, nhưng phải hoà… |
| **LOI_CHUNG_CONG_HAI_SPECIALIST_MONG** | Về lý thuyết là kiến trúc sạch nhất: một nguồn cho phần chung, hai cửa vào theo hai câu hỏi khác nhau, mỗi tệp đủ hẹp để viết điề… | Đắt nhất và rủi ro nhất trong ba phương án, mà lợi ích thực tế mỏng. Vấn đề gốc đo được KHÔNG phải là thiếu lõi chung — LÕI CHUNG… | TRUNG BÌNH ĐẾN CAO, theo cách khác: từng tệp thì hẹp, nhưng ba tệp cùng chủ đề panel chi … | CAO NHẤT. Đây là phương án duy nhất phải CẮT nội dung ra khỏi cả hai tệp cùng lúc rồi ghé… | KHÓ NHẤT. Phải gỡ tệp lõi mới, ghép nội dung trả về hai tệp cũ đúng v… |

### 8.1 So sánh theo từng khía cạnh (17 khía cạnh)

| Khía cạnh | Tệp BỐ CỤC | Tệp BẬT-TẮT | Phán quyết |
|---|---|---|---|
| Ý định nghiệp vụ / người dùng | Trả lời câu hỏi "panel chi tiết PHẢI TRÔNG NHƯ THẾ NÀO". Mục đích tự khai: chuẩn hoá bố cục panel bên phải để thông tin… | Trả lời câu hỏi "CÓ NÊN dùng kiểu panel bên phải không, và cơ chế trạng thái ra sao". Là hướng dẫn quyết định cộng máy … | **BO_TRO** |
| Trách nhiệm bố cục và bố trí thành phần | Rất dày: hero chuyển sắc (2 biến thể — bản gốc mã-ở-trên và biến thể 1.1 tên-ở-trên được Chủ dự án duyệt), thẻ mục có h… | Rất mỏng: lưới 5 cột chia 3/2, hero một mẫu duy nhất (mã ở trên, tên ở dưới), thân panel với 3 thẻ mục chỉ nêu TÊN màu … | **TRUNG_MOT_PHAN** |
| Trách nhiệm mở / đóng / bật-tắt / trạng thái | CÓ, và khai là BẮT BUỘC: mục 2 trọn vẹn — biến trạng thái, hàm mở/đóng kiểu bật-tắt, gắn sự kiện bấm dòng, bảng 5 hành … | CÓ, gần như trùng từng ý: biến trạng thái, hàm bật-tắt, gắn sự kiện bấm dòng. Khác duy nhất: tô sáng dòng đang chọn dùn… | **TRUNG_HOAN_TOAN** |
| Hành vi responsive (đáp ứng kích thước màn) | Chỉ có điểm ngắt lg trong lưới 5 cột và một dòng nghiệm thu "lưới co giãn khi mở/đóng panel". Đo được: 0 lần nhắc chữ m… | Cũng chỉ có điểm ngắt lg, NHƯNG có thêm một cảnh báo mà bên kia không có: mục Nhược điểm ghi trải nghiệm trên điện thoạ… | **TRUNG_MOT_PHAN** |
| Khả năng tiếp cận (trợ năng) | KHÔNG CÓ. Đo bằng tìm chuỗi: 0 lần thuộc tính aria, 0 lần role. Chỉ có 6 lần thuộc tính title trên nút (chú giải khi rê… | KHÔNG CÓ. Đo được: 0 lần aria, 0 lần role, và 0 lần thuộc tính title trên nút (phần lớp trình bày của các nút để trống … | **TRUNG_HOAN_TOAN** |
| Bàn phím và tiêu điểm | KHÔNG CÓ. 0 lần nhắc bàn phím, phím thoát, quản lý tiêu điểm, thứ tự chuyển tiêu điểm. Hàng bảng được gắn sự kiện bấm c… | KHÔNG CÓ. 0 lần nhắc bàn phím, phím thoát, quản lý tiêu điểm. Thiếu sót y hệt bên kia. | **TRUNG_HOAN_TOAN** |
| Tương tác lớp phủ (hộp thoại, tấm trượt, popover) | Gần như không đụng. Không có nội dung về lớp phủ chồng nhau, bẫy tiêu điểm, hay cổng thoát an toàn. Có nhắc nút Sửa gọi… | Đụng ở mức TRIGGER PHỦ ĐỊNH: điều kiện dùng thứ ba ghi rõ là dùng khi KHÔNG muốn dùng lớp phủ để xem chi tiết, và mục K… | **CHI_MOT_BEN_CO** |
| Nạp dữ liệu | KHÔNG CÓ. Đo được 0 lần nhắc gọi dữ liệu, hiệu ứng phụ, trạng thái đang tải, khung xương chờ. Giả định dữ liệu đã nằm s… | KHÔNG CÓ. Đo được 0 lần, giả định y hệt — chỉ lưu đối tượng của dòng vào trạng thái, không nạp thêm chi tiết. | **TRUNG_HOAN_TOAN** |
| Trạng thái bền hoặc trạng thái trên địa chỉ trang | KHÔNG CÓ. Đo được 0 lần nhắc tham số địa chỉ, bộ định tuyến, kho lưu cục bộ. Trạng thái panel chỉ nằm trong bộ nhớ thàn… | KHÔNG CÓ. Đo được 0 lần, y hệt. | **TRUNG_HOAN_TOAN** |
| Phạm vi đường dẫn / mô-đun | CÓ khai phạm vi: nêu 2 bản tham chiếu thật (trang khách hàng thuộc M1 và trang đơn hàng thuộc M3 — cả 2 đã kiểm TỒN TẠI… | KHÔNG khai phạm vi nào. Đo được 0 đường dẫn mã nguồn và 0 mô-đun được nêu. Hoàn toàn không neo vào dự án — có thể áp ch… | **CHI_MOT_BEN_CO** |
| Điều kiện kích hoạt thuận (khi nào nên nạp kỹ năng) | NGUỒN KHÔNG KHAI. Không có mục Trigger, chỉ có mục Mục Đích. Danh mục kỹ năng ghi nhãn cấu trúc NO-TRIGGER và số trường… | CÓ mục Trigger tường minh với 3 điều kiện: cần dựng bố cục danh sách kèm panel bên phải; bấm dòng mở panel và bấm lại t… | **CHI_MOT_BEN_CO** |
| Điều kiện KHÔNG kích hoạt (trigger phủ định) | KHÔNG CÓ. Danh mục ghi trường điều kiện phủ định là CẦN CHỦ DỰ ÁN ĐIỀN (chưa ai điền). Thân tệp không có câu nào nói kh… | CÓ, ở hai chỗ: mục Khuyến Nghị ghi tránh khi dữ liệu đơn giản không cần xem chi tiết; và mục Nhược Điểm nêu 3 cái giá p… | **CHI_MOT_BEN_CO** |
| Xung đột với chuẩn giao diện | Danh mục ghi đối chiếu DA_GOP (đã gộp vào chuẩn mục 10 và 20.2) với điểm chọi bằng 0. CẢNH BÁO — ĐO LẠI CHO THẤY SỐ 0 L… | Danh mục ghi đối chiếu SSOT_THANG (chọi chuẩn) với điểm chọi 6. Đo lại xác nhận và tìm thêm: (1) lưới 5 cột chia 3/2 ch… | **TRUNG_MOT_PHAN** |
| Nơi gọi và phụ thuộc (trong mã nguồn thật) | Đo trên mã nguồn: tìm chuỗi tên panel chi tiết trong thư mục thành phần và thư mục trang, CHỈ 1 tệp khớp (trang thiết k… | Cùng tập mã nguồn, cùng kết quả. Riêng lưới 5 cột mà kỹ năng này kê đơn: đo được 2 tệp đang dùng (trang vật tư và trang… | **TRUNG_MOT_PHAN** |
| Kỹ năng nội bộ liên quan | Là ĐẦU MỐI: đo được 6 kỹ năng KHÁC trỏ ngược về nó (cấu hình cột, nhất quán liên mô-đun, ánh xạ màu trạng thái, thao tá… | Là NGỌN CỤT: đo được 0 kỹ năng khác trỏ về nó (lần khớp duy nhất là chính tệp của nó, do phần đầu tệp mang tên đó). Bản… | **KHAC_HAN** |
| Rủi ro và tác dụng phụ | Rủi ro chính: dùng nguyên văn sẽ dựng thẻ mục bo góc 12px (chuẩn cấm) và có thể chọn hero bản gốc mã-ở-trên đã bị phân … | Rủi ro chính CAO HƠN và không có van an toàn: tệp chỉ có MỘT mẫu hero duy nhất, và mẫu đó chính là mẫu đã bị phân xử th… | **TRUNG_MOT_PHAN** |
| Tương thích công cụ | Phần đầu tệp mở ở dòng 1, đủ 3 trường tên / mô tả / phiên bản 2.1.0, kèm mục Lịch Sử Thay Đổi Kỹ Năng ở cuối với 2 mốc.… | Phần đầu tệp mở ở dòng 1, chỉ có 2 trường tên và mô tả. KHÔNG khai phiên bản, KHÔNG có lịch sử thay đổi, nên không truy… | **TRUNG_MOT_PHAN** |

### 8.2 Điểm chặn quan trọng do kiểm toán viên bổ sung

Tệp **bố cục** nằm trong nhóm **11 kỹ năng mà Chủ dự án đã khoá ngày 23/08/2026**: *"thư mục gốc GIỮ NGUYÊN làm lưu trữ — không xoá, không sửa"*. Vì vậy phán quyết cuối cho tệp này là **GIỮ NGUYÊN TUYỆT ĐỐI**, kể cả phần còn mang chuỗi bị cấm. Việc gỡ chuỗi cấm ở tệp đó **cần Chủ dự án mở ngoại lệ** — xem quyết định **QD-3**.
Tệp **bật-tắt** **không** thuộc nhóm bị khoá ⇒ được đề xuất sửa **đúng phần chọi**, giữ trọn phần còn lại.

---

## 9. CỤM C — BẢY KỸ NĂNG R0: HỒ SƠ RÀ SOÁT SÂU

Tổng **100 mục nội dung** đã được phân loại trên bảy kỹ năng.

### `dual-key-standard`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **CẬP NHẬT TẠI CHỖ** |
| Lý do | Là bản sống duy nhất của quy tắc khoá kép. Cần bổ sung điều kiện dùng/không dùng. |
| Đề xuất của lượt phân rã | CAP_NHAT_TAI_CHO (giữ nguyên) |
| Mục đích gốc | Chốt một chuẩn thiết kế khoá cho dữ liệu danh mục của hệ thống: mỗi bảng có ĐỒNG THỜI hai khoá với hai vai trò tách bạch — một khoá hệ thống dạng số tự tăng dùng để nối quan hệ và sửa/xoá, và một mã nghiệp vụ dùng để hiển thị và ánh xạ nghiệp vụ. Mục đích kép được tệp khai rõ: chống trùng (khoá hệ thống duy nhất tuyệt đối, mã nghiệp vụ duy nhất theo phạm vi) và thẩm mỹ (mã đọc được, có quy tắc riêng, không lộ số nội… |
| Giá trị hiện còn | CÒN GIÁ TRỊ CAO, và cao hơn mức mà con số "43 dòng" gợi ý. Ba bằng chứng đo được: (1) ĐÂY LÀ BẢN SỐNG DUY NHẤT CỦA QUY TẮC. Tra 5 tệp luật gốc (.cursorrules · .antigravityrules · AGENTS.md · CLAUDE.md · GEMINI.md): tìm tiêu đề "Dual Key Standard" cho ra 0 KẾT QUẢ. Năm tệp chỉ còn đúng MỘT từ khoá "dual-key" trong mục K1, và mục K1 nói rõ các quy định loại này "được bảo tồn" nhưng nằm ở Kỹ năng / thủ tục / trạng thái… |
| Phân loại 6 mục | CAN_CAP_NHAT **3** · CON_DUNG **2** · TRUNG_NHUNG_BO_TRO **1** |
| Dùng khi | NÊN KÍCH HOẠT khi (hiện tệp khai 0 điều — đây là ĐỀ XUẤT trên giấy): 1. Tạo bảng danh mục mới, hoặc thêm/sửa cột khoá của bảng danh mục đã có. 2. Viết hoặc rà tệp di trú có mệnh đề khoá chính, chỉ mục duy nhất, hoặc khoá ngoại. 3. Thấy hiện tượng trùng mã nghiệp vụ trong cùng một danh mục, hoặc thiếu ràng buộc duy nhất. 4. Thấy mã nghiệp… |
| KHÔNG dùng khi | KHÔNG DÙNG KHI (hiện tệp khai 0 điều — đăng ký ghi negative_trigger: NEEDS_OWNER_INPUT; đây là ĐỀ XUẤT trên giấy): 1. Bảng đang xét CỐ Ý dùng mã nghiệp vụ làm khoá chính. Đo được ít nhất 5 bảng như vậy (ca làm việc, bảng lương, quy trình, vai trò, mục trình đơn) trong 3 tệp di trú của M0, M6, M7. Báo cáo rà soát toàn hệ 25/07/2026 khai r… |
| Ca gần giống dễ nhầm | CA DỄ KÍCH HOẠT NHẦM NHẤT (đích danh): 1. «Đặt khoá chính cho bảng ca làm việc / bảng lương / bảng quy trình / bảng vai trò / bảng mục trình đơn» — dùng chung từ khoá "khoá chính", "mã", "chống trùng", NHƯNG đo được các bảng này CỐ Ý dùng mã nghiệp vụ làm khoá chính (thấy trong 3 tệp di trú của M0,… |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT PHẠM VI ĐƯỜNG DẪN (trên giấy, chưa áp dụng). Hiện 0/128 kỹ năng trong kho khai phạm vi này, nên đây là đề xuất mới hoàn toàn. NÊN KÍCH HOẠT khi việc đụng tới: - migrations/ — nơi định nghĩa khoá chính, chỉ mục duy nhất, khoá ngoại. Đây là nơi quy tắc … |
| Đầu ra bắt buộc | Đầu ra bắt buộc mà kỹ năng KHAI: chỉ có danh sách kiểm 5 dòng ở cuối tệp. Tệp KHÔNG khai bắt buộc phải xuất ra báo cáo, bảng đối chứng hay bằng chứng nào — NGUỒN KHÔNG KHAI. ĐỀ XUẤT bổ sung đầu ra bắt buộc (trên giấy, chưa áp dụng), để nối được với bộ luật: 1… |
| Tác dụng phụ | TÁC ĐỘNG PHỤ CỦA BẢN THÂN KỸ NĂNG: gần như bằng không. Đo được: - 0 tệp thi hành trong thư mục kỹ năng — chỉ đúng một tệp SKILL.md, 1.801 byte. - Không gọi công cụ, không gọi mạng, không ghi tệp, không đụng cơ sở dữ liệu, không thao tác Gi… |
| Trùng với kỹ năng khác | Đã tra toàn bộ 128 slug trong .cursor/skills. Bốn kỹ năng có giao nhau, và CẢ BỐN đều nên GIỮ RIÊNG: 1. fk-ref-by-id-not-name — GIAO MẠNH NHẤT. Trùng ở điều cấm dùng mã hoặc tên làm tham chiếu khoá ngoại. KHÁC: kỹ năng đó kích hoạt khi VIẾT MÃ LỌC dữ liệu theo khoá ngoại, đầu ra là mẫu mã nguồn lọc… |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT MÔ TẢ MỚI (trên giấy, KHÔNG áp dụng trong lượt này). Theo công thức bắt buộc: «Kiểm và chốt cách đặt khoá kép — khoá hệ thống dạng số tự tăng để nối quan hệ và sửa/xoá, cộng mã nghiệp vụ để người dùng đọc — TRÊN các bảng dữ liệu danh mục cùng phần khai kiểu dữ liệu và lớp kho dữ liệu của chúng. DÙNG KHI tạo hoặc sửa bảng danh mục, khi viết tệp di trú có mệnh đề khoá chính hoặc chỉ mục duy nhất hoặc khoá ngoại, khi phát hiện trùng mã trong cùng dan… |
| **Nội dung BẮT BUỘC GIỮ** | PHẢI GIỮ NGUYÊN VĂN, KHÔNG ĐƯỢC RÚT GỌN: 1. Cặp khái niệm khoá hệ thống và mã nghiệp vụ, cùng lý do tách đôi (chống trùng tuyệt đối cho hệ thống · thẩm mỹ và quy tắc riêng cho nghiệp vụ). 2. Điều cấm dùng mã nghiệp vụ làm khoá ngoại thay khoá hệ thống, kèm lý do gãy khi sửa mã. 3. Điều cấm dùng cột tên làm khoá, kèm lý do không ổn định và dễ trùng. 4. Điều cấm dùng khoá hệ thống làm mã hiển thị (chiều ngược, chỉ có … |
| Rủi ro mất tri thức | RỦI RO MẤT TRI THỨC NẾU GỘP HOẶC THAY THẾ — 5 điều CHỈ tồn tại ở tệp này, không tệp kỹ năng nào khác có: 1. Quy tắc PHẠM VI DUY NHẤT của mã nghiệp vụ (duy nhất trong cùng danh mục, không phải duy nhất toàn bảng). Không kỹ năng nào khác nêu. Đây là điều đã được thi hành thật bằng chỉ mục duy nhất th… |
| Kiểm thử | CÁCH KIỂM XEM ĐỀ XUẤT CÓ THẬT SỰ TỐT HƠN (chưa chạy trong lượt này — lượt này chỉ đọc): A. KIỂM CHỌN ĐÚNG — 4 ca phải kích hoạt: 1. «Tạo bảng danh mục nhà cung cấp mới, cần khoá gì» → phải chọn kỹ năng này. 2. «Bảng biểu mẫu thiếu trường k… |
| Đường lui | Lượt rà soát này KHÔNG SỬA GÌ, nên KHÔNG CÓ GÌ ĐỂ HOÀN TÁC. Không tệp nào được tạo, sửa, xoá, đổi tên hay di chuyển; không thao tác Git; không đụng cơ sở dữ liệu. NẾU về sau Chủ dự án duyệt bốn thay … |

### `windows-dev-troubleshoot-quick`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **CẬP NHẬT TẠI CHỖ** |
| Lý do | Mô tả đã có mệnh đề điều kiện. Cần bổ sung điều kiện không dùng và ranh giới với môi trường vận hành. |
| Đề xuất của lượt phân rã | CAP_NHAT_TAI_CHO (giữ nguyên) |
| Mục đích gốc | Mục đích gốc do chính tệp khai (không suy diễn): làm CỔNG SÀNG LỌC NHANH HAI PHÚT chạy TRƯỚC khi bắt tay sửa mã, khi máy phát triển chạy Windows gặp các triệu chứng "bấm không phản hồi", "biểu mẫu không gửi", "lỗi lạ không liên quan mã vừa sửa". Mục tiêu tiết kiệm thời gian: xác định nhanh xem nguyên nhân gốc có phải là thư mục xây dựng bị hỏng hoặc khoá tệp hay không, để tránh đi sai hướng hàng giờ. Tệp tự khai là … |
| Giá trị hiện còn | CÒN GIÁ TRỊ CAO, và giá trị đó KHÔNG trùng với kỹ năng anh em. Bằng chứng đo được: 1. Đây là kỹ năng DUY NHẤT trong nhóm môi trường khởi đầu từ TRIỆU CHỨNG NGƯỜI DÙNG NHÌN THẤY ("bấm không phản hồi", "form không chạy"). Kỹ năng anh em `windows-next-cache-stability` (51 dòng) khởi đầu từ THÔNG BÁO LỖI KỸ THUẬT (thiếu mô-đun, lỗi trang tài liệu, lỗi tài nguyên tĩnh). Hai điểm vào khác nhau ⇒ hai kỹ năng khác nhau. 2. … |
| Phân loại 15 mục | CAN_CAP_NHAT **4** · CON_DUNG **10** · TRUNG_NHUNG_BO_TRO **1** |
| Dùng khi | DÙNG KHI — tệp CÓ khai, gom từ mục "Trigger" và mục "Dùng khi": 1. Giao diện "bấm không phản hồi" — nút hoặc liên kết bấm mà không có tác dụng gì. 2. Biểu mẫu gửi đi mà không chạy. 3. Lỗi ngẫu nhiên dạng thiếu mô-đun, hoặc tài nguyên tĩnh trả về mã lỗi không tìm thấy / lỗi máy chủ. 4. Lỗi xuất hiện sau khi sửa đoạn mã KHÔNG liên quan. 5.… |
| KHÔNG dùng khi | KHÔNG DÙNG KHI — tệp CÓ khai (mục "Tránh khi"), đây là điểm mạnh cần giữ: 1. Lỗi rõ ràng là lỗi kiểu dữ liệu do trình biên dịch báo. 2. Lỗi chỉ xuất hiện ở bản dựng phát hành, không xuất hiện khi chạy dev. Ngoài ra suy ra được từ thân bài (chưa viết thành mục riêng): nếu Kiểm 1 và Kiểm 2 đều SẠCH thì tệp nói thẳng "debug code bình thường… |
| Ca gần giống dễ nhầm | CÁC CA DÙNG CHUNG TỪ KHOÁ NHƯNG KHÔNG THUỘC PHẠM VI — nêu đích danh, xếp theo mức dễ nhầm giảm dần: 1. DỄ NHẦM NHẤT — "Nút bấm không phản hồi" do PHÂN QUYỀN. Người dùng có vai trò không đủ quyền nên nút bị ẩn hoặc bị vô hiệu hoá. Đúng y nguyên câu chữ kích hoạt, nhưng nguyên nhân nằm ở lớp quyền, k… |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT: KHAI PHẠM VI ĐƯỜNG DẪN, NHƯNG LÀ BỘ LỌC PHỤ, KHÔNG PHẢI CỔNG CHÍNH. Lý do phải nói rõ như vậy: điều kiện kích hoạt của kỹ năng này là TRIỆU CHỨNG LÚC CHẠY, không phải tệp đang mở. Một phiên có thể đang sửa bất kỳ trang nào rồi gặp triệu chứng này. Nế… |
| Đầu ra bắt buộc | Đầu ra mà tệp hiện đang bắt buộc: 1. MẪU BÁO CÁO KẾT QUẢ — khối văn bản ngắn gồm: kết quả Kiểm 1, kết quả Kiểm 2, hành động đã làm, kết quả sau hành động. 2. DANH SÁCH ĐẠT/KHÔNG ĐẠT sáu dòng — kiểm bảng điều khiển · kiểm nhật ký · dọn thư mục xây dựng nếu có … |
| Tác dụng phụ | RÀ THEO TỪNG NHÓM TÁC ĐỘNG: 1. CÔNG CỤ / TIẾN TRÌNH: CÓ. Kỹ năng yêu cầu dừng và khởi động lại máy chủ dev. RỦI RO CHƯA ĐƯỢC TỆP CẢNH BÁO: dự án có ghi nhận thói quen mở nhiều phiên làm việc song song trên cùng cây làm việc — khởi động lại… |
| Trùng với kỹ năng khác | TRA TRÊN 128 THƯ MỤC KỸ NĂNG. Ứng viên trùng nghĩa và phán quyết từng cái: 1. `windows-next-cache-stability` — MỨC TRÙNG CAO NHẤT, nhưng GIỮ RIÊNG.    Trùng: cùng nói về thư mục xây dựng hỏng trên Windows; cùng nhắc loại trừ chống vi-rút; cùng nguyên tắc "bộ nhớ đệm trước, mã sau".    Khác — bốn đi… |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT TRÊN GIẤY — KHÔNG ÁP DỤNG TRONG LƯỢT NÀY: "LÀM GÌ: chạy cổng sàng lọc hai phút bằng hai quan sát để phân định lỗi môi trường với lỗi mã, rồi ra một phán quyết ba nhánh. TRÊN ĐỐI TƯỢNG NÀO: máy phát triển chạy Windows của dự án — thư mục xây dựng của khung ứng dụng, tiến trình chạy dev cục bộ, cấu hình loại trừ của phần mềm chống vi-rút. KHI NÀO: giao diện bấm không phản hồi · biểu mẫu không gửi · lỗi thiếu mô-đun hoặc tài nguyên tĩnh không tìm thấ… |
| **Nội dung BẮT BUỘC GIỮ** | PHẢI GIỮ NGUYÊN, KHÔNG ĐƯỢC CẮT trong bất kỳ đề xuất nào về sau: 1. Cổng quyết định hai quan sát (Kiểm 1 bảng điều khiển trình duyệt · Kiểm 2 nhật ký cửa sổ chạy dev) cùng hai câu rẽ nhánh đi kèm — đây là lõi không tái tạo được. 2. Toàn bộ bốn dấu hiệu ở mục "Trigger", đặc biệt cách diễn đạt theo triệu chứng người dùng nhìn thấy. 3. Toàn bộ mục "Tránh khi" (hai điều kiện chống kích hoạt) — hiếm và có giá trị chặn nh… |
| Rủi ro mất tri thức | RỦI RO MẤT TRI THỨC NẾU LÀM SAI CÁCH — liệt kê đích danh phần dễ mất nhất: 1. CỔNG QUYẾT ĐỊNH HAI QUAN SÁT. Đây là phần khó tái tạo nhất và cũng là phần dễ bị bỏ nhất khi gộp, vì nhìn bề ngoài nó giống "checklist đơn giản". Bản chất nó là bộ định tuyến ba nhánh. Mất nó thì mọi phiên quay lại thói q… |
| Kiểm thử | CÁCH KIỂM CHỨNG ĐỀ XUẤT NÀY (không thực hiện trong lượt này): A. KIỂM DƯƠNG — kỹ năng PHẢI được chọn: đưa câu "bấm nút thêm vật tư không có phản hồi gì, nãy giờ vừa sửa một trang khác không liên quan". Đạt khi kỹ năng này được chọn làm kỹ … |
| Đường lui | HOÀN TÁC — mức rủi ro THẤP: 1. Tệp được công cụ quản lý mã theo dõi (đã đo: trường theo dõi bằng đúng). Mọi sửa đổi tại chỗ đều quay lui được bằng thao tác khôi phục tệp về bản trước, không ảnh hưởng… |

### `column-config-workflow`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **CẬP NHẬT TẠI CHỖ** |
| Lý do | Không thuộc nhóm bị khoá, không chọi chuẩn. Cần bổ sung mệnh đề điều kiện dùng và điều kiện không dùng vào mô tả. |
| Đề xuất của lượt phân rã | CAP_NHAT_TAI_CHO (giữ nguyên) |
| Mục đích gốc | Rút ngắn vòng trao đổi giữa Chủ dự án và tác nhân khi cần CHỌN và SẮP THỨ TỰ cột hiển thị của bảng danh sách cùng các trường của khung chi tiết. Thay vì mô tả bằng lời (dễ hiểu nhầm, dễ sót cột), tác nhân xuất một bảng liệt kê cột có đánh số thứ tự; Chủ dự án chỉ gửi lại một dãy số; thứ tự các số trong dãy chính là thứ tự hiển thị. Bản chất đây là một GIAO THỨC TRAO ĐỔI (quy ước cách nói chuyện giữa người và máy), K… |
| Giá trị hiện còn | CÒN GIÁ TRỊ THẬT, có bằng chứng đo được ngày rà soát: (1) Đối tượng mà kỹ năng nhắm tới vẫn tồn tại đông đảo trong mã nguồn: 28 tệp `.tsx` dưới `src/` còn dùng bảng dựng tay (thẻ `thead`), 11 tệp còn dùng thành phần bảng dùng chung `src/components/data-table-simple.tsx`, 16 tệp có khung chi tiết. Kỹ năng phục vụ được CẢ HAI kiểu dựng bảng vì nó chỉ quy ước cách gửi lựa chọn, không ràng buộc kiểu mã. (2) Các bước phụ… |
| Phân loại 15 mục | CAN_CAP_NHAT **3** · CON_DUNG **9** · CHUA_CHUNG_MINH **1** · TRUNG_NHUNG_BO_TRO **1** · XUNG_DOT **1** |
| Dùng khi | Kích hoạt khi có ĐỒNG THỜI hai điều: (a) yêu cầu liên quan tới VIỆC CHỌN và SẮP THỨ TỰ cột của bảng danh sách hoặc trường của khung chi tiết trên một màn hình đã có sẵn; và (b) Chủ dự án MUỐN tự chỉ định lựa chọn thay vì để tác nhân tự quyết. Câu nói điển hình: "liệt kê các cột ra cho tôi đánh số", "bảng này lấy cột 1, 3, 4, 7", "khung c… |
| KHÔNG dùng khi | KHÔNG dùng khi: (1) đối tượng là trình hướng dẫn nhiều bước hoặc biểu mẫu nhập liệu — chính tệp đã tự loại trừ ở phần lưu ý; (2) Chủ dự án CHƯA gửi dãy số và cũng không muốn gửi, mà muốn tác nhân TỰ QUYẾT cột nào nên ẩn — việc đó thuộc `table-column-visibility`; (3) yêu cầu là làm TÍNH NĂNG cho người dùng cuối tự bật/tắt cột ngay trên mà… |
| Ca gần giống dễ nhầm | CA DỄ KÍCH HOẠT NHẦM NHẤT — nêu đích danh: yêu cầu "bảng nhiều cột quá, ẩn bớt cho gọn đi" hoặc "cột này thừa, bỏ đi, chuyển sang khung chi tiết". Yêu cầu này dùng CHUNG TỪ KHOÁ ("cột", "ẩn", "bảng", "khung chi tiết") nhưng THUỘC kỹ năng `table-column-visibility`, KHÔNG thuộc kỹ năng này — vì ở đó … |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT (trên giấy — hiện 0/128 kỹ năng trong kho khai trường này, nên đây sẽ là kỹ năng đầu tiên nếu Chủ dự án duyệt): TRONG PHẠM VI — các tệp thành phần phía trình duyệt của trang danh sách và khung chi tiết, theo khuôn đặt tên đang dùng `src/app/<mã mô-đun… |
| Đầu ra bắt buộc | (1) Bảng liệt kê cột có đánh số ở bước 1 — hiện diện trong phần trao đổi, để về sau còn truy được dãy số ứng với cột nào. (2) Bản đọc-lại dãy số thành tên cột (đề xuất mới). (3) Khai báo đã đọc TOÀN PHẦN `docs/UI-STANDARD.md` kèm số dòng đầu–cuối, theo yêu cầ… |
| Tác dụng phụ | CÔNG CỤ: thư mục kỹ năng chỉ có duy nhất 1 tệp `SKILL.md` (2.150 byte), 0 tệp thi hành, 0 mã kịch bản, 0 lệnh chạy được ⇒ bản thân hiện vật là TRƠ, không tự gây tác động. MẠNG: không có địa chỉ mạng, không có lệnh tải, không có tham chiếu … |
| Trùng với kỹ năng khác | Rà toàn bộ 128 slug trong `.cursor/skills`. Kết quả: GẦN NHẤT — `table-column-visibility` (97 dòng, 2.947 byte). TRÙNG: cùng nói về việc cột nào hiển thị trên bảng, cùng nhắc chuyển dữ liệu sang khung chi tiết. KHÁC BỐN ĐIỂM ĐO ĐƯỢC: (a) ĐIỀU KIỆN KÍCH HOẠT — kỹ năng kia khai điều kiện rõ và thiên … |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT TRÊN GIẤY — KHÔNG ÁP DỤNG TRONG LƯỢT NÀY: "LÀM GÌ: áp một lựa chọn cột và một thứ tự hiển thị do Chủ dự án chỉ định bằng DÃY SỐ, thay cho việc mô tả bằng lời. TRÊN ĐỐI TƯỢNG NÀO: bảng danh sách và khung chi tiết của một màn hình ĐÃ CÓ SẴN, trong các tệp thành phần phía trình duyệt theo khuôn `src/app/<mã mô-đun>/<tên-màn>/<tên-màn>-client.tsx` và thành phần bảng dùng chung `src/components/data-table-simple.tsx`. KHI NÀO: khi Chủ dự án muốn tự chọn… |
| **Nội dung BẮT BUỘC GIỮ** | BẮT BUỘC GIỮ NGUYÊN VĂN, không được lược khi cập nhật: (1) Quy ước "thứ tự gửi = thứ tự hiển thị" — câu chốt của cả kỹ năng. (2) Khuôn bảng hai cột (số thứ tự · tên cột) ở bước 1. (3) Quy ước hai dòng riêng cho bảng và cho khung chi tiết, cùng việc chấp nhận nhiều dấu phân cách — chi tiết nhỏ nhưng đúng thói quen gõ của Chủ dự án, mất là phải hỏi lại. (4) Cả 5 mục thi hành ở bước 3, ĐẶC BIỆT hai mục dễ quên nhất và … |
| Rủi ro mất tri thức | NẾU XOÁ HOẶC GỘP MÀ KHÔNG ÁNH XẠ ĐẦY ĐỦ, MẤT NHỮNG THỨ SAU — không nơi nào khác trong kho còn giữ: (1) TOÀN BỘ GIAO THỨC DÃY SỐ (bước 1 + bước 2 + quy tắc dấu phân cách + quy tắc thứ tự). Rà 128 slug: 0 kỹ năng khác có. Mất là Chủ dự án phải quay về mô tả bằng lời — chính cái mà kỹ năng sinh ra để … |
| Kiểm thử | CÁCH KIỂM SAU KHI CẬP NHẬT (đề xuất, chưa chạy trong lượt này): (1) ĐẾM BẢO TOÀN theo `GOV-EDIT-PRESERVE-001` §G7.0: số mục có nghĩa của tệp SAU ≥ TRƯỚC (hiện đo được 14 mục trong tệp, chưa kể mục lịch sử sửa đổi sẽ thêm mới); mỗi dòng bị … |
| Đường lui | Lượt này KHÔNG sửa gì nên KHÔNG cần hoàn tác. Nếu về sau áp bản cập nhật đề xuất: (1) Tệp đang được Git theo dõi (đã đo) ⇒ khôi phục bằng lịch sử mã nguồn là đủ, không cần tạo tệp bản sao. (2) Trước … |

### `file-update-safe-workflow`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **CẬP NHẬT TẠI CHỖ** |
| Lý do | Nhãn cấu trúc lành. Cần bổ sung điều kiện dùng/không dùng và ghi rõ ba khác biệt so với kỹ năng ghi sổ công việc để lượt sau không gộp nhầm. |
| Đề xuất của lượt phân rã | CAP_NHAT_TAI_CHO (giữ nguyên) |
| Mục đích gốc | Mục đích gốc do chính nguồn khai ở mục "Mục Đích": bảo đảm quy trình cập nhật tệp tài liệu diễn ra an toàn — không mất mát nội dung, truy vết được, và nhất quán — áp dụng cho mọi tệp tài liệu quan trọng. Đây là kỹ năng chống MẤT NỘI DUNG KHI SỬA TÀI LIỆU: buộc đọc toàn bộ tệp trước, giữ nguyên phần đang có, chỉ thêm hoặc sửa phần cần thiết, và ghi số phiên bản khi thay đổi lớn. Bối cảnh sinh ra: phiên bản 1.0.0, tệp… |
| Giá trị hiện còn | CÒN GIÁ TRỊ THẬT, ở mức trung bình-cao, nhưng giá trị đã DỊCH CHUYỂN so với lúc viết. Bằng chứng cụ thể: (1) GIÁ TRỊ CÒN NGUYÊN — phần kỷ luật thao tác. Bộ luật hiện hành đã nâng đúng nguyên tắc này lên hạng MUST là `GOV-EDIT-PRESERVE-001` (§G7.0, Chủ dự án duyệt 18/08/2026), nhưng luật đó nói CÁI GÌ PHẢI ĐÚNG (số điều khoản sau ≥ trước, mỗi điều rời đi phải còn đúng một dòng con trỏ) chứ KHÔNG nói LÀM THEO TRÌNH TỰ… |
| Phân loại 18 mục | CAN_CAP_NHAT **5** · CON_DUNG **10** · TRUNG_NHUNG_BO_TRO **2** · DA_BI_THAY **1** |
| Dùng khi | KHI NÀO DÙNG — nguồn CÓ KHAI trong thân tệp (mục "Khi Nào Áp Dụng"), nhưng KHÔNG khai trong phần đầu tệp, nên máy không đọc được để tự kích hoạt. Danh sách nguồn khai gồm: cập nhật `.cursorrules` · cập nhật `AGENTS.md` · cập nhật `CLAUDE.md` · cập nhật một tệp đặc tả tổng thể của dự án · cập nhật bất kỳ tệp tài liệu quan trọng nào. Mục g… |
| KHÔNG dùng khi | KHÔNG DÙNG KHI — nguồn CÓ KHAI, đây là điểm mạnh hiếm. Ba trường hợp loại trừ nêu tường minh trong tệp: chỉ sửa lỗi chính tả nhỏ · chỉ định dạng lại mã · chỉ thêm chú thích. Ba dòng này lặp lại lần thứ hai ở mục gợi ý cuối tệp, cho thấy tác giả cố ý nhấn mạnh. BỔ SUNG cần thêm khi cập nhật (đề xuất trên giấy, dựa trên bằng chứng đo được,… |
| Ca gần giống dễ nhầm | CA DỄ KÍCH HOẠT NHẦM NHẤT — nêu đích danh: CA 1 (nguy hiểm nhất): "CẬP NHẬT PHIÊN BẢN VÀ NHẬT KÝ THAY ĐỔI SAU KHI SỬA MÃ NGUỒN". Dùng chung từ khoá "cập nhật" và "phiên bản", và lĩnh vực của kỹ năng này trong danh mục cũng chính là "versioning". Nhưng việc đó KHÔNG thuộc phạm vi kỹ năng này. Chủ nh… |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT CÓ PHẠM VI ĐƯỜNG DẪN. Ghi nhận trước: hiện 0/128 kỹ năng trong kho khai trường này, nên đây là đề xuất mới hoàn toàn, chưa có tiền lệ trong kho. Lý do NÊN có phạm vi cho kỹ năng này: mục "Khi Nào Áp Dụng" hiện kết bằng cụm mở "bất kỳ file tài liệu qua… |
| Đầu ra bắt buộc | Nguồn KHAI RÕ hai đầu ra bắt buộc — đây là điểm mạnh hiếm của tệp này: ĐẦU RA 1 — BÁO CÁO CẬP NHẬT TỆP, có mẫu sẵn với các trường: tệp đã cập nhật · loại cập nhật · số phiên bản nếu có · mốc thời gian · danh sách thay đổi · phần khẳng định nội dung được bảo t… |
| Tác dụng phụ | TÁC ĐỘNG PHỤ — rà từng nhóm, kết luận: KHÔNG CÓ tác động phụ ở lớp nào. CÔNG CỤ: Nguồn không gọi công cụ nào ngoài thao tác đọc tệp ở Bước 1. Không có lệnh chạy, không có kịch bản, không có lệnh của hệ điều hành. Thư mục kỹ năng chỉ chứa đ… |
| Trùng với kỹ năng khác | Đã tra toàn bộ 128 slug trong `.cursor/skills`. Kết quả đối chiếu, kèm phán quyết theo luật bảo toàn: (1) `documentation-sync` (bản 3.0.0) — TRÙNG NẶNG NHẤT nhưng GIỮ RIÊNG. · Trùng ở đâu: cả hai đều kích hoạt khi sắp sửa `.cursorrules` · `AGENTS.md` · `CLAUDE.md`; cả hai đều nhằm không làm hỏng nộ… |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT TRÊN GIẤY — KHÔNG áp dụng trong lượt này. Viết theo đúng bảy phần của công thức: "LÀM GÌ: Chạy quy trình 6 bước sửa tài liệu mà không làm mất nội dung cũ — đọc toàn phần, giữ nguyên phần đang có, chèn thêm phần mới, ghi số phiên bản, kiểm nhất quán, rồi bàn giao khâu đồng bộ. TRÊN ĐỐI TƯỢNG NÀO: tệp văn bản trong `docs/**`, `.governance/registry/**`, `.governance/procedures/**`, `.cursor/skills/**/SKILL.md`, `WORK_LOG.md`, và MỘT bản trong 5 tệp q… |
| **Nội dung BẮT BUỘC GIỮ** | PHẢI GIỮ NGUYÊN VĂN, không được rút gọn, không được diễn giải lại: (1) TOÀN BỘ 6 BƯỚC theo đúng thứ tự hiện có. Thứ tự chính là tri thức: đọc toàn phần TRƯỚC, giữ nguyên TRƯỚC khi thêm, ghi số phiên bản SAU khi sửa, đồng bộ 5 tệp quản trị SAU CÙNG. Đảo thứ tự là hỏng. (2) MỤC "KHÔNG cần workflow này khi" — ba trường hợp loại trừ. Đây là điều-kiện-KHÔNG-kích-hoạt viết tường minh, tài sản hiếm trong kho. Bỏ đi thì kỹ … |
| Rủi ro mất tri thức | RỦI RO MẤT TRI THỨC NẾU XỬ LÝ SAI — nêu đích danh từng phần sẽ mất và mất vì lý do gì: (1) NẾU GỘP VÀO `documentation-sync`: mất toàn bộ nhóm đối tượng "bất kỳ tệp tài liệu quan trọng nào". `documentation-sync` bản 3.0.0 khai phạm vi là đúng 5 tệp quản trị và cách làm là sửa bản chuẩn rồi chép đè. … |
| Kiểm thử | BÀI KIỂM ĐỀ XUẤT — chỉ mô tả cách kiểm, KHÔNG chạy trong lượt này (ràng buộc chỉ đọc). KIỂM 1 — THAM CHIẾU CÓ THẬT (tự động hoá được). Rà mọi đường dẫn và mọi tên mục mà tệp khai. Đạt khi: cả 5 tệp quản trị tồn tại (đã kiểm 5/5 đạt) · cả 3… |
| Đường lui | CÁCH LÙI LẠI NẾU BẢN CẬP NHẬT GÂY HẠI — chỉ mô tả, KHÔNG thực hiện trong lượt này: BỐI CẢNH THUẬN LỢI: tệp được Git theo dõi (đã đo), thư mục chỉ có đúng một tệp, không có tệp thi hành đi kèm, không … |

### `windows-next-cache-stability`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **CẬP NHẬT TẠI CHỖ** |
| Lý do | Mô tả đã có mệnh đề điều kiện. Cần bổ sung điều kiện không dùng và điều kiện dừng. |
| Đề xuất của lượt phân rã | CAP_NHAT_TAI_CHO (giữ nguyên) |
| Mục đích gốc | Mục đích ban đầu do chính tệp khai: làm nguồn chuẩn duy nhất để xử lý lỗi Next.js trên máy phát triển Windows khi thư mục bộ nhớ đệm biên dịch bị hỏng. Kỹ năng đặt ra một thứ tự ưu tiên bắt buộc: khi gặp lỗi bản dựng hoặc lỗi máy chủ phát triển có dạng thiếu mô-đun / thiếu trang tài liệu gốc / thiếu tài nguyên tĩnh, tác nhân PHẢI xử lý tầng bộ nhớ đệm và khoá tệp TRƯỚC, không được lao vào sửa mã nghiệp vụ. Mục tiêu … |
| Giá trị hiện còn | GIÁ TRỊ HIỆN CÒN: CAO. Bốn bằng chứng đo được: (1) Ba tham chiếu lệnh trong skill đều còn sống: `npm run build`, `npm run dev`, `npm run dev:withdb` đều có mặt trong `package.json` (lần lượt ở các dòng khai script 31 · 15 · 21). Không có tham chiếu chết. (2) Khẳng định kỹ thuật cốt lõi vẫn ĐÚNG: `package.json` khai `engines.node` là `>=20 <25` (dòng 7) và `next` phiên bản `^16.1.6` (dòng 226) — khớp chính xác câu sk… |
| Phân loại 18 mục | CAN_CAP_NHAT **5** · CON_DUNG **9** · XUNG_DOT **2** · TRUNG_NHUNG_BO_TRO **1** · CHUA_CHUNG_MINH **1** |
| Dùng khi | KHI NÀO DÙNG — nguồn khai rõ bằng bốn dấu hiệu nguyên văn ở mục dấu hiệu nhận biết, tóm lại bằng tiếng Việt đầy đủ: 1. Cửa sổ lệnh hoặc trình duyệt báo không tìm thấy một mô-đun nằm bên trong thư mục bộ nhớ đệm biên dịch của máy chủ. 2. Báo không tìm thấy mô-đun cho trang tài liệu gốc của khung ứng dụng. 3. Xuất hiện nhiều lỗi không tìm … |
| KHÔNG dùng khi | KHÔNG DÙNG KHI — nguồn khai MỘT PHẦN (chỉ ca số 1), phần còn lại audit suy ra từ ranh giới đo được với các kỹ năng khác và ghi rõ là suy ra: 1. CÓ KHAI: khi lỗi là lỗi kiểu dữ liệu hoặc lỗi kiểm tra mã rõ ràng — skill viết thẳng ở bước 1 là phải xử lý mã. 2. CÓ KHAI GIÁN TIẾP: khi đã dọn bộ nhớ đệm mà lỗi vẫn y hệt — lúc đó kỹ năng tự tr… |
| Ca gần giống dễ nhầm | CA DỄ KÍCH HOẠT NHẦM NHẤT — nêu đích danh: 1. NGHI MẤT TỆP THẬT. Người dùng báo "tệp biến mất", "thư mục thư viện bị xoá bớt", "sau khi khởi động lại máy thì bản dựng hỏng". Từ khoá dùng chung: Windows · lỗi lạ · bản dựng hỏng · phần mềm diệt vi-rút. NHƯNG đây KHÔNG thuộc phạm vi kỹ năng này — thuộ… |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT: KỸ NĂNG NÀY KHÔNG NÊN KHAI PHẠM VI ĐƯỜNG DẪN MÃ NGUỒN — và đây là kết luận có bằng chứng, không phải né tránh. Lý do đo được: điều kiện kích hoạt của kỹ năng KHÔNG đến từ việc chạm vào tệp mã nào cả. Nó đến từ CHUỖI LỖI in ra ở cửa sổ lệnh hoặc trình… |
| Đầu ra bắt buộc | ĐẦU RA BẮT BUỘC — nguồn khai MỘT PHẦN, và đây là điểm yếu rõ nhất so với kỹ năng anh em. NGUỒN CÓ KHAI: 1. Kết luận phân loại lỗi: đây là hỏng bộ nhớ đệm biên dịch, hay là lỗi mã thật. 2. Kết quả kiểm chứng: bản dựng sản xuất chạy ĐẠT hai lần liên tiếp sau mộ… |
| Tác dụng phụ | PHÂN TÍCH TÁC ĐỘNG PHỤ — theo từng loại: CÔNG CỤ: không gọi công cụ nào ngoài trình chạy lệnh của dự án. Không dùng trình duyệt tự động, không dùng dịch vụ ngoài. MẠNG: KHÔNG. Toàn bộ thao tác chạy trên máy cục bộ. GHI TỆP: CÓ, nhưng chỉ ở… |
| Trùng với kỹ năng khác | ĐÃ ĐỐI CHIẾU TRÊN DANH SÁCH 128 SLUG. Kết quả: 1. `windows-dev-troubleshoot-quick` — TRÙNG NHIỀU NHẤT NHƯNG PHẢI GIỮ RIÊNG, và đây là kết luận có bằng chứng chứ không phải nhân nhượng. Trùng ở: cùng chủ đề bộ nhớ đệm biên dịch hỏng trên Windows, cùng nhắc chuỗi lỗi thiếu mô-đun và thiếu tài nguyên … |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT TRÊN GIẤY — KHÔNG ÁP DỤNG TRONG LƯỢT NÀY. "LÀM GÌ: chẩn đoán và xử lý hỏng bộ nhớ đệm biên dịch, chặn tái phát ở tầng khai báo loại trừ cho phần mềm diệt vi-rút. TRÊN ĐỐI TƯỢNG NÀO: thư mục hiện vật biên dịch của khung ứng dụng web ở gốc kho, trên máy phát triển chạy Windows; không đụng mã nghiệp vụ, không đụng cơ sở dữ liệu, không đụng máy vận hành. KHI NÀO: khi lệnh dựng hoặc máy chủ phát triển hỏng thất thường, hoặc sửa đoạn mã không liên quan … |
| **Nội dung BẮT BUỘC GIỮ** | BẮT BUỘC GIỮ NGUYÊN VĂN — danh sách đích danh, mọi thay đổi ngoài danh sách này đều phải coi là mất mát: 1. TOÀN BỘ BỐN CHUỖI TRIỆU CHỨNG ở mục dấu hiệu nhận biết. Giữ nguyên ký tự, kể cả dấu gạch chéo ngược trong đường dẫn mẫu. Đây là móc nhận diện, viết lại là hỏng. 2. CÂU PHÂN NHÁNH Ở BƯỚC 1 — hai vế "lỗi kiểu dữ liệu/kiểm tra mã thì xử lý mã" và "lỗi thiếu mô-đun/thiếu tài nguyên tĩnh thì coi là hỏng bộ nhớ đệm"… |
| Rủi ro mất tri thức | RỦI RO MẤT TRI THỨC NẾU LÀM SAI CÁCH — nêu đích danh bốn khối dễ bị mất nhất: 1. BỐN CHUỖI TRIỆU CHỨNG NGUYÊN VĂN. Đây là thứ đắt nhất trong tệp: chúng được rút từ lỗi thật đã gặp, và chính chúng là móc để nhận diện. Nếu ai đó "viết lại cho gọn" thành mô tả chung chung thì tri thức mất sạch mà tệp … |
| Kiểm thử | CÁCH KIỂM CHỨNG BẢN CẬP NHẬT ĐỀ XUẤT — sáu phép thử, tất cả đều đo được, không phép nào dựa trên cảm nhận: 1. KIỂM THAM CHIẾU SỐNG: mọi lệnh nêu trong kỹ năng phải có mặt trong tệp cấu hình gói. Hiện tại ĐÃ ĐẠT 3/3. Sau cập nhật phải đạt 4… |
| Đường lui | HOÀN TÁC — rủi ro cực thấp, nhưng nêu đầy đủ để Chủ dự án yên tâm: 1. Với chính tệp kỹ năng: tệp được công cụ quản lý mã theo dõi (đã xác nhận `tracked: true`), nên mọi thay đổi đề xuất đều lấy lại đ… |

### `annotated-screenshot-review`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **GIỮ NGUYÊN** — ⚠️ **CHỦ DỰ ÁN ĐÃ KHOÁ: không xoá, không sửa** |
| Lý do | CHỦ DỰ ÁN ĐÃ KHOÁ 23/08/2026 (cùng nhóm 11): "không xoá, không sửa". Đề xuất gộp của lượt phân rã bị BÁC vì (a) trái quyết định đã khoá, (b) không kèm bảng ánh xạ — vi phạm luật khoá số 7, (c) đo được 5 khối nội dung còn giá trị KHÔNG có ở đích gộp. |
| Đề xuất của lượt phân rã | GOP_CO_BAO_TOAN → **kiểm toán viên BÁC, xem lý do trên** |
| Mục đích gốc | Mục đích gốc: đặt ra quy trình bắt buộc để Agent ĐỌC ẢNH CHÚ THÍCH do Chủ dự án gửi kèm trong tin nhắn — loại ảnh có khoanh đỏ (vuông/tròn/oval), mũi tên đỏ và ghi chú chữ — rồi bóc thành danh sách việc rõ ràng trước khi sửa mã. Hai nỗi đau gốc mà tệp nhắm tới: (1) Agent chỉ liếc một hai ảnh rồi kết luận, bỏ sót yêu cầu nằm ở các ảnh sau; (2) Agent sửa lan sang vùng giao diện mà Chủ dự án KHÔNG khoanh. Bằng chứng: k… |
| Giá trị hiện còn | CÒN GIÁ TRỊ THẬT, và giá trị đó KHÔNG nằm trọn trong chuẩn giao diện như sổ đăng ký đang ghi. Đo được ba quy tắc của tệp KHÔNG hề có trong mục 20.9 của tệp chuẩn `docs/UI-STANDARD.md` (nơi sổ khai là đã gộp xong): (1) Quy tắc DỪNG VÀ HỎI khi các ảnh mâu thuẫn nhau hoặc ghi chú không rõ — tìm chữ "mâu thuẫn" trong tệp chuẩn ra 4 kết quả, cả 4 đều nằm ở bảng lịch sử sửa đổi mục 21 và nói về chuyện bo góc, KHÔNG kết qu… |
| Phân loại 12 mục | CAN_CAP_NHAT **1** · XUNG_DOT **1** · TRUNG_NHUNG_BO_TRO **4** · CON_DUNG **6** |
| Dùng khi | Nên kích hoạt khi ĐỒNG THỜI có hai dấu hiệu: (1) Tin nhắn của Chủ dự án có ĐÍNH KÈM ít nhất một ảnh chụp màn hình; VÀ (2) Ảnh đó mang dấu chú thích do người vẽ thêm — khoanh đỏ dạng vuông/tròn/oval, mũi tên đỏ, hoặc ghi chú chữ chèn lên ảnh — tức ảnh đang được dùng để CHỈ RA yêu cầu, chứ không phải để báo cáo kết quả. Dấu hiệu phụ thường… |
| KHÔNG dùng khi | KHÔNG dùng trong các ca sau: - Ảnh do CHÍNH AGENT chụp để tự kiểm chứng kết quả sau khi sửa giao diện — đó là chiều ngược lại, thuộc kỹ năng `screenshot-verification`. - Ảnh chụp làm BẰNG CHỨNG nghiệm thu đặt cạnh trang mẫu theo yêu cầu chốt tiêu chí nghiệm thu trước khi làm — đó là sản phẩm đầu ra, không phải yêu cầu đầu vào. - Ảnh khôn… |
| Ca gần giống dễ nhầm | Ca dễ kích hoạt nhầm nhất, nêu đích danh: yêu cầu "chụp màn hình lại cho tôi xem" hoặc "chụp ảnh cạnh trang mẫu rồi báo cáo" — thuộc kỹ năng `screenshot-verification`, KHÔNG thuộc tệp này. Lý do dễ nhầm: cả hai cùng chứa từ khoá "ảnh" và "screenshot", cùng lĩnh vực testing, cùng thao tác VERIFY tro… |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT: tệp này KHÔNG cần phạm vi theo thư mục mã nguồn, và đây là kết luận có căn cứ chứ không phải bỏ trống. Lý do đo được: điều kiện kích hoạt của tệp nằm ở HÌNH DẠNG TIN NHẮN ĐẾN (có ảnh đính kèm mang chú thích), không nằm ở việc Agent đang mở tệp nào. T… |
| Đầu ra bắt buộc | Đầu ra bắt buộc theo thân tệp: một bản liệt kê lồng hai cấp — cấp ngoài là từng ảnh (Ảnh 1, Ảnh 2...), cấp trong là từng vùng khoanh trong ảnh đó (Vùng A, Vùng B...), mỗi vùng ghi rõ đối tượng và yêu cầu. Sau bản liệt kê là một câu xác nhận sẽ sửa đúng theo c… |
| Tác dụng phụ | Rủi ro tác động phụ: THẤP NHẤT trong thang phân loại, khớp nhãn R0 đã cấp. - Công cụ: tệp không gọi công cụ nào; đo được 0 tệp thi hành trong thư mục, thư mục chỉ chứa đúng một tệp SKILL.md. - Mạng: không có lệnh gọi mạng, không có địa chỉ… |
| Trùng với kỹ năng khác | Đã rà toàn bộ 128 slug và lọc bằng tìm kiếm từ khoá liên quan tới ảnh/chú thích, ra 13 tệp có nhắc. Xét kỹ 3 ứng viên gần nhất: 1) `screenshot-verification` — GẦN NHẤT, và là ca cần phân định rõ nhất. Trùng: cùng lĩnh vực testing, cùng thao tác VERIFY, cùng làm việc với ảnh chụp màn hình, cùng bị g… |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT TRÊN GIẤY — KHÔNG ÁP DỤNG TRONG LƯỢT NÀY: "Bóc yêu cầu từ ảnh chụp màn hình có chú thích vẽ tay của Chủ dự án (khoanh đỏ vuông/tròn/oval, mũi tên đỏ, ghi chú chữ) thành danh sách việc đánh số, đã khử trùng lặp và đã phân loại. LÀM TRÊN: toàn bộ ảnh đính kèm trong một tin nhắn, không phân biệt mô-đun hay thư mục mã. DÙNG KHI: tin nhắn của Chủ dự án có ảnh đính kèm mang dấu chú thích, tức ảnh đang được dùng để CHỈ RA yêu cầu. TÍN HIỆU BẮT BUỘC PHẢI … |
| **Nội dung BẮT BUỘC GIỮ** | PHẢI GIỮ, không được để mất trong bất kỳ phương án nào: 1. Quy tắc DỪNG và HỎI khi các ảnh mâu thuẫn nhau hoặc ghi chú không rõ — điều kiện dừng duy nhất của tệp, chưa có ở tệp chuẩn. 2. Bước ĐẾM SỐ ẢNH trước khi làm — thứ biến yêu cầu "đọc hết" thành con số kiểm được. 3. Bước PHÂN LOẠI chỉ-giao-diện so với chạm-logic-dữ-liệu — thứ quyết định mức rủi ro và lớp bằng chứng cần có. 4. Ba câu hỏi con cho mỗi vùng khoanh… |
| Rủi ro mất tri thức | RỦI RO MẤT TRI THỨC — mức TRUNG BÌNH CAO nếu không làm gì, và nguồn rủi ro nằm ở CHÍNH CÁI NHÃN chứ không ở tệp. Cơ chế mất: sổ đăng ký ghi `ssot_verdict: DA_GOP` và `dang_gop: []` (rỗng). Hai nhãn này nói với mọi phiên sau rằng "nội dung đã nằm trọn trong tệp chuẩn, không còn gì phải lấy". Trong k… |
| Kiểm thử | Cách kiểm chứng đề xuất, làm được bằng lệnh, không cần chạy phần mềm: KIỂM 1 — kiểm ngược phần gộp còn thiếu (làm TRƯỚC khi sửa, để chốt mốc): tìm trong `docs/UI-STANDARD.md` bốn cụm "đếm ảnh", "mâu thuẫn" giới hạn trong vùng mục 20.9, "UI… |
| Đường lui | Cách hoàn tác, theo từng việc: VIỆC 1 (thêm 4 gạch vào mục 20.9): hoàn tác bằng cách gỡ đúng 4 gạch vừa thêm. An toàn tuyệt đối vì đây là THÊM MỚI, không thay và không xoá dòng nào đang có, nên gỡ ra… |

### `schema-visualization`

| Mục | Nội dung |
|---|---|
| **Xử trí cuối** | **CẬP NHẬT TẠI CHỖ** |
| Lý do | Không chọi chuẩn. Cần bổ sung điều kiện dùng/không dùng và phạm vi đường dẫn. |
| Đề xuất của lượt phân rã | CAP_NHAT_TAI_CHO (giữ nguyên) |
| Mục đích gốc | Mục đích gốc: khi Chủ dự án yêu cầu "xem toàn bộ lược đồ" của MỘT thực thể/mô-đun, kỹ năng bắt Trợ lý trình bày lược đồ ĐẦY ĐỦ dưới dạng BA BẢNG markdown trực quan, đặt cạnh nhau: (1) lược đồ cơ sở dữ liệu (kiểu dữ liệu, cho phép rỗng, khoá, giá trị mặc định, ghi chú), (2) các cột của bảng hiển thị trên giao diện (trường nào hiện, sắp xếp được không, bề rộng, cách kết xuất), (3) các trường của biểu mẫu nhập liệu (nh… |
| Giá trị hiện còn | CÒN GIÁ TRỊ CAO, và giá trị đó ĐO ĐƯỢC — không phải nhận định cảm tính. Bằng chứng 1 — mô hình dữ liệu trong ví dụ VẪN ĐÚNG với mã nguồn hiện tại: - Bảng danh mục nhóm dùng chung mà ví dụ trỏ tới: có thật, được nhắc trong 101 tệp mã/di trú. - Cột dấu vết "ngày tạo" mà ví dụ ghi là "TEXT hoặc DATETIME": khớp thực tế đo được trong thư mục di trú — 3 lần khai DATETIME, 2 lần khai TEXT. Ghi chú "TEXT/DATETIME" của kỹ nă… |
| Phân loại 16 mục | CAN_CAP_NHAT **5** · CON_DUNG **8** · TRUNG_NHUNG_BO_TRO **1** · CHUA_CHUNG_MINH **2** |
| Dùng khi | KÍCH HOẠT khi có ĐỒNG THỜI hai điều: (a) đối tượng là MỘT thực thể dữ liệu hoặc MỘT mô-đun đã xác định tên; (b) yêu cầu là XEM TOÀN CẢNH để RA QUYẾT ĐỊNH, không phải sửa. Sáu ca kích hoạt đích danh: 1. Chủ dự án nói "cho xem toàn bộ lược đồ của <tên thực thể>", "liệt kê đầy đủ các cột", "lược đồ chi tiết", "cho xem cả bảng lẫn biểu mẫu".… |
| KHÔNG dùng khi | KHÔNG DÙNG trong bảy trường hợp sau: 1. Chỉ cần xem một đến hai trường ⇒ đọc thẳng mã nhanh hơn. (Điều này ĐÃ CÓ trong tệp, mục Khuyến nghị dòng 131–140 — không phải tôi thêm mới.) 2. Việc là SỬA lược đồ (thêm/đổi/xoá cột, di trú dữ liệu) ⇒ đó là kỹ năng khác, có rủi ro mất dữ liệu, phải qua cổng duyệt lược đồ. 3. Việc là LẤY lược đồ từ … |
| Ca gần giống dễ nhầm | CA DỄ KÍCH HOẠT NHẦM NHẤT — đích danh: yêu cầu "cho xem lược đồ bảng X" khi mục đích thật là SỬA lược đồ, chứ không phải xem để ra lệnh giao diện. Diễn biến hỏng: Chủ dự án nói "cho xem lược đồ bảng công đoạn" như bước MỞ ĐẦU cho ý định "rồi bỏ cột này đi". Trợ lý bắt đúng từ khoá "lược đồ", nạp kỹ… |
| Phạm vi đường dẫn đề xuất | ĐỀ XUẤT CÓ phạm vi đường dẫn — và đây là kỹ năng THUỘC NHÓM CẦN NHẤT, vì đầu ra của nó trông rất đáng tin trong khi tệp không hề nói lấy sự thật ở đâu. Hiện trạng đo được: 0/128 kỹ năng trong kho khai phạm vi đường dẫn ở phần đầu tệp (tìm bốn dạng khai báo ph… |
| Đầu ra bắt buộc | ĐẦU RA BẮT BUỘC — tệp khai rõ ở mục có nhãn bắt buộc (dòng 21) và danh mục tự kiểm (114–121): 1. BẢNG LƯỢC ĐỒ CƠ SỞ DỮ LIỆU — bảy cột: số thứ tự · tên cột · kiểu · cho phép rỗng · khoá · mặc định · ghi chú. Tiêu đề bảng phải nêu tên bảng và tổng số cột tách l… |
| Tác dụng phụ | TÁC ĐỘNG PHỤ: gần như bằng không. Đây là kỹ năng an toàn nhất trong nhóm liên quan tới lược đồ. Kiểm từng loại: - CÔNG CỤ / DÒNG LỆNH: KHÔNG. Đo được: 0 lần nhắc câu lệnh mô tả bảng, 0 lần nhắc câu lệnh xem lệnh tạo bảng, 0 lần nhắc tên hệ… |
| Trùng với kỹ năng khác | Đã soát toàn bộ 128 kỹ năng trong kho. Kết quả: KHÔNG có kỹ năng nào trùng nghĩa. Có SÁU kỹ năng liên quan, tất cả đều BỔ TRỢ chứ không thay thế. Theo luật bảo toàn của Chủ dự án — khác điều kiện kích hoạt, khác rủi ro, khác đầu ra là lý do chính đáng để giữ riêng — cả sáu đều nên giữ riêng. 1. mys… |
| Mô tả cải tiến đề xuất | ĐỀ XUẤT TRÊN GIẤY — KHÔNG áp dụng trong lượt này. Viết theo đúng bảy phần của công thức: LÀM GÌ: Trình bày toàn cảnh lược đồ dưới dạng ba bảng markdown đánh số cạnh nhau — bảng cơ sở dữ liệu (kiểu, cho phép rỗng, khoá, mặc định), bảng cột hiển thị hiện trạng (kèm cột đã ẩn), bảng trường biểu mẫu (kèm quy tắc kiểm tra) — cộng một khối luồng logic khi có sinh mã tự động hoặc ràng buộc giữa các trường, và một bảng nêu đích danh mọi điểm lệch giữa ba tầng. TR… |
| **Nội dung BẮT BUỘC GIỮ** | PHẢI GIỮ NGUYÊN VĂN, không được rút gọn, không được diễn đạt lại — mười hai điểm: 1. KHUÔN BA BẢNG và ĐÚNG SỐ CỘT của từng bảng: bảng cơ sở dữ liệu bảy cột; bảng cột hiển thị sáu cột; bảng trường biểu mẫu sáu cột. Đây là phần lõi tạo ra toàn bộ giá trị. 2. Quy tắc mỗi bảng phải MANG THEO kiểu dữ liệu lấy từ cơ sở dữ liệu — kể cả bảng giao diện. Đây là cơ chế chống lệch giữa tầng dữ liệu và tầng hiển thị, và là chi t… |
| Rủi ro mất tri thức | RỦI RO MẤT TRI THỨC NẾU XOÁ HOẶC GỘP KỸ NĂNG NÀY: CAO. Sáu tổn thất, mỗi tổn thất kèm bằng chứng vì sao không nơi nào khác giữ được. 1. MẤT MẮT XÍCH GIỮA CỦA CHUỖI BA BƯỚC. Chuỗi lấy → trình bày → thi hành sẽ đứt ở giữa. Kỹ năng cấu hình cột sẽ phải chạy trên bảng liệt kê chỉ hai cột (số thứ tự và … |
| Kiểm thử | PHÉP THỬ ĐỀ XUẤT — thiết kế để KHÔNG cần sửa gì và chỉ dùng lệnh đọc. PHÉP THỬ 1 — KIỂM NGƯỢC TRÊN THỰC THỂ ĐÃ BIẾT (kiểm giá trị lõi): Chọn thực thể Công Đoạn thuộc mô-đun M1 — chính thực thể mà mục nguồn gốc nêu, và có thật trong kho (đủ… |
| Đường lui | PHƯƠNG ÁN LÙI: đơn giản nhất trong nhóm, vì phạm vi thay đổi rất nhỏ và mọi thay đổi đều nằm trong một tệp văn bản. Bối cảnh đo được: thư mục kỹ năng chứa ĐÚNG MỘT tệp; 0 tệp thi hành; tệp đang được … |


---

## 10. BẢNG TỔNG HỢP — MỖI CHỨC NĂNG MỘT DÒNG

**Tổng 193 dòng** · A_VERSIONING **59** · B_DETAIL_PANEL **34** · C_R0 **100**.
Bảng đầy đủ mọi cột nằm ở trường `function_rows` của artefact máy đọc. Dưới đây là bản rút gọn:

| # | Cụm | Kỹ năng nguồn | Chức năng / mục | Xử trí | Bảo toàn |
|---|---|---|---|---|---|
| 1 | VERSIONING | `update-work-log` | Khai báo đầu tệp — trạng thái và quan hệ (dòng 1-11) | **CAP_NHAT_TAI_CHO** | .cursor/skills/update-work-log/SKILL.md… |
| 2 | VERSIONING | `update-work-log` | Mục đích của sổ công việc (mục Summary, dòng 15-16) | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 3 | VERSIONING | `update-work-log` | Thời điểm cập nhật sổ và thứ tự thực hiện (Procedure bước 1) | **GIU_NGUYEN** | .cursor/skills/update-work-log/SKILL.md… |
| 4 | VERSIONING | `update-work-log` | An toàn khi nhiều phiên chạy song song — đọc lại tệp NGAY T… | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 5 | VERSIONING | `update-work-log` | Chèn khối mới LÊN ĐẦU tệp, không ghi đè mục mới hơn (Proced… | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 6 | VERSIONING | `update-work-log` | Chống ghi trùng / chống mất mục khi nghi ngờ chạy song song… | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 7 | VERSIONING | `update-work-log` | Bộ trường bắt buộc của khối sổ (Procedure bước 4) | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 8 | VERSIONING | `update-work-log` | Khuôn mẫu đầu ra khối sổ công việc (mục Output Format) | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 9 | VERSIONING | `update-work-log` | Điều kiện kích hoạt thuận (frontmatter triggers + mục Trigg… | **GIU_NGUYEN** | .cursor/skills/update-work-log/SKILL.md… |
| 10 | VERSIONING | `update-work-log` | Điều kiện KHÔNG kích hoạt (mục Trigger phần Avoid + mục Use… | **GIU_NGUYEN** | .cursor/skills/update-work-log/SKILL.md… |
| 11 | VERSIONING | `update-work-log` | Cổng ĐẠT / KHÔNG ĐẠT (mục PASS/FAIL) | **GIU_SPECIALIST** | .cursor/skills/update-work-log/SKILL.md… |
| 12 | VERSIONING | `update-work-log` | Ca kiểm thử tối thiểu (mục Minimal Test Cases) | **GIU_NGUYEN** | .cursor/skills/update-work-log/SKILL.md… |
| 13 | VERSIONING | `update-work-log` | Ghi chú tích hợp — quan hệ với skill khác (mục Integration … | **GIU_NGUYEN** | .cursor/skills/update-work-log/SKILL.md… |
| 14 | VERSIONING | `update-work-log` | Lịch sử sửa đổi của chính skill (mục Skill Change History) | **GIU_NGUYEN** | .cursor/skills/update-work-log/SKILL.md… |
| 15 | VERSIONING | `update-work-log` | Ranh giới quyền commit / đẩy mã / triển khai | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 16 | VERSIONING | `update-work-log` | Báo cáo lên kho công khai · bàn giao Notion · Sổ Yêu Cầu Ow… | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 17 | VERSIONING | `update-work-log` | Xử lý khi thất bại · đường lui · tính lặp-lại-an-toàn · tác… | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 18 | VERSIONING | `version-bump-on-feature` | Khai báo đầu tệp — trạng thái và quan hệ (dòng 1-11) | **CAP_NHAT_TAI_CHO** | .cursor/skills/version-bump-on-feature/… |
| 19 | VERSIONING | `version-bump-on-feature` | Mục đích — trợ giúp phân định mức tăng phiên bản (mục Summa… | **GIU_SPECIALIST** | .cursor/skills/version-bump-on-feature/… |
| 20 | VERSIONING | `version-bump-on-feature` | Quy tắc tăng số phiên bản — ba mức (Procedure bước 2) | **CAN_CHU_DU_AN_QUYET** | .cursor/skills/version-bump-on-feature/… |
| 21 | VERSIONING | `version-bump-on-feature` | Chiều "độ lộ diện" của thay đổi (mục Output Format) | **GIU_SPECIALIST** | .cursor/skills/version-bump-on-feature/… |
| 22 | VERSIONING | `version-bump-on-feature` | Vòng phản hồi — trả kết quả phán mức về quy trình chính (Pr… | **GIU_NGUYEN** | .cursor/skills/version-bump-on-feature/… |
| 23 | VERSIONING | `version-bump-on-feature` | Điều kiện kích hoạt thuận (frontmatter triggers + mục Trigg… | **GIU_NGUYEN** | .cursor/skills/version-bump-on-feature/… |
| 24 | VERSIONING | `version-bump-on-feature` | Điều kiện KHÔNG kích hoạt (mục Trigger phần Avoid + mục Use… | **GIU_NGUYEN** | .cursor/skills/version-bump-on-feature/… |
| 25 | VERSIONING | `version-bump-on-feature` | Cổng ĐẠT / KHÔNG ĐẠT (mục PASS/FAIL) | **GIU_NGUYEN** | .cursor/skills/version-bump-on-feature/… |
| 26 | VERSIONING | `version-bump-on-feature` | Ca kiểm thử tối thiểu (mục Minimal Test Cases) | **GIU_SPECIALIST** | .cursor/skills/version-bump-on-feature/… |
| 27 | VERSIONING | `version-bump-on-feature` | Ranh giới giữa "đổi mã" và "phát hành lên máy vận hành" | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 28 | VERSIONING | `version-bump-on-feature` | Tách số phiên bản mã nguồn khỏi số phiên bản tài liệu | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 29 | VERSIONING | `version-bump-on-feature` | Báo cáo GitHub · bàn giao Notion · Sổ Yêu Cầu Owner · an to… | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 30 | VERSIONING | `versioning-change-history` | Khai báo đầu tệp — trạng thái và quan hệ (dòng 1-11) | **CAP_NHAT_TAI_CHO** | .cursor/skills/versioning-change-histor… |
| 31 | VERSIONING | `versioning-change-history` | Trách nhiệm lịch sử thay đổi — định nghĩa nghĩa của từng tr… | **GIU_SPECIALIST** | .cursor/skills/versioning-change-histor… |
| 32 | VERSIONING | `versioning-change-history` | Khuôn mẫu đầu ra lịch sử (mục Output Format) | **GIU_SPECIALIST** | .cursor/skills/versioning-change-histor… |
| 33 | VERSIONING | `versioning-change-history` | Thời điểm dùng — bắt đầu bằng quy trình chính, dùng skill n… | **GIU_NGUYEN** | .cursor/skills/versioning-change-histor… |
| 34 | VERSIONING | `versioning-change-history` | Điều kiện kích hoạt thuận (frontmatter triggers + mục Trigg… | **GIU_NGUYEN** | .cursor/skills/versioning-change-histor… |
| 35 | VERSIONING | `versioning-change-history` | Điều kiện KHÔNG kích hoạt (mục Trigger phần Avoid + mục Use… | **GIU_SPECIALIST** | .cursor/skills/versioning-change-histor… |
| 36 | VERSIONING | `versioning-change-history` | Cổng ĐẠT / KHÔNG ĐẠT (mục PASS/FAIL) | **GIU_NGUYEN** | .cursor/skills/versioning-change-histor… |
| 37 | VERSIONING | `versioning-change-history` | Ca kiểm thử tối thiểu (mục Minimal Test Cases) — có ca CHUY… | **GIU_SPECIALIST** | .cursor/skills/versioning-change-histor… |
| 38 | VERSIONING | `versioning-change-history` | Rủi ro tự khai (mục Risks) | **GIU_NGUYEN** | .cursor/skills/versioning-change-histor… |
| 39 | VERSIONING | `versioning-change-history` | Ranh giới quyền commit/đẩy/triển khai · ranh giới đổi mã vs… | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 40 | VERSIONING | `versioning-change-history` | Báo cáo GitHub · bàn giao Notion · Sổ Yêu Cầu Owner · an to… | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 41 | VERSIONING | `versioning-auto-log` | Khai báo đầu tệp — không bị thay, 4 điều kiện kích hoạt (dò… | **CAP_NHAT_TAI_CHO** | .cursor/skills/versioning-auto-log/SKIL… |
| 42 | VERSIONING | `versioning-auto-log` | Mục đích — giữ mọi thay đổi có ý nghĩa đều truy vết được (m… | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 43 | VERSIONING | `versioning-auto-log` | Điều kiện kích hoạt thuận (mục Trigger, 4 nhóm) | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 44 | VERSIONING | `versioning-auto-log` | Điều kiện KHÔNG kích hoạt (mục Trigger phần "Do not use" + … | **CAN_CHU_DU_AN_QUYET** | .cursor/skills/versioning-auto-log/SKIL… |
| 45 | VERSIONING | `versioning-auto-log` | Quy tắc tăng số phiên bản — ba mức (Procedure bước 1) | **CAN_CHU_DU_AN_QUYET** | .cursor/skills/versioning-auto-log/SKIL… |
| 46 | VERSIONING | `versioning-auto-log` | Danh sách nơi phải ghi dấu vết (Procedure bước 2) | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 47 | VERSIONING | `versioning-auto-log` | Tách số phiên bản mã nguồn khỏi số phiên bản skill (Procedu… | **CAP_NHAT_TAI_CHO** | .cursor/skills/versioning-auto-log/SKIL… |
| 48 | VERSIONING | `versioning-auto-log` | Trách nhiệm nhật ký thay đổi — viết LÝ DO trước khi viết CÁ… | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 49 | VERSIONING | `versioning-auto-log` | Bằng chứng xác minh — sáu lớp (Procedure bước 4) | **CAP_NHAT_TAI_CHO** | .cursor/skills/versioning-auto-log/SKIL… |
| 50 | VERSIONING | `versioning-auto-log` | Chốt chặn cuối — không kết thúc phản hồi trước khi cập nhật… | **CAP_NHAT_TAI_CHO** | .cursor/skills/versioning-auto-log/SKIL… |
| 51 | VERSIONING | `versioning-auto-log` | An toàn dữ liệu — ba chốt (mục Data Safety) | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 52 | VERSIONING | `versioning-auto-log` | Cổng ĐẠT / KHÔNG ĐẠT (mục PASS/FAIL, 4 + 4 điều kiện) | **CAN_CHU_DU_AN_QUYET** | .cursor/skills/versioning-auto-log/SKIL… |
| 53 | VERSIONING | `versioning-auto-log` | Lợi ích và rủi ro tự khai (mục Benefits + Risks) | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 54 | VERSIONING | `versioning-auto-log` | Ca kiểm thử tối thiểu (mục Minimal Test Cases, 4 ca) | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 55 | VERSIONING | `versioning-auto-log` | Tương thích công cụ và phối hợp skill (mục Integration Note… | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 56 | VERSIONING | `versioning-auto-log` | Lịch sử sửa đổi của chính skill (mục Skill Change History) | **GIU_NGUYEN** | .cursor/skills/versioning-auto-log/SKIL… |
| 57 | VERSIONING | `versioning-auto-log` | An toàn khi nhiều phiên chạy song song · chống ghi trùng · … | **CAN_CHU_DU_AN_QUYET** | Bằng chứng phản chứng: .cursor/skills/v… |
| 58 | VERSIONING | `versioning-auto-log` | Báo cáo lên kho công khai · bàn giao Notion · Sổ Yêu Cầu Ow… | **CAN_CHU_DU_AN_QUYET** | Không có nội dung để bảo toàn (khoảng t… |
| 59 | VERSIONING | `versioning-auto-log` | Ranh giới quyền commit / đẩy mã / triển khai · ranh giới đổ… | **CAP_NHAT_TAI_CHO** | .cursor/skills/versioning-auto-log/SKIL… |
| 60 | DETAIL_PANEL | `detail-panel-layout` | Ý định nghiệp vụ / người dùng | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 61 | DETAIL_PANEL | `detail-panel-toggle` | Ý định nghiệp vụ / người dùng | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 62 | DETAIL_PANEL | `detail-panel-layout` | Trách nhiệm bố cục và bố trí thành phần | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 63 | DETAIL_PANEL | `detail-panel-toggle` | Trách nhiệm bố cục và bố trí thành phần | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 64 | DETAIL_PANEL | `detail-panel-layout` | Trách nhiệm mở / đóng / bật-tắt / trạng thái | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 65 | DETAIL_PANEL | `detail-panel-toggle` | Trách nhiệm mở / đóng / bật-tắt / trạng thái | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 66 | DETAIL_PANEL | `detail-panel-layout` | Hành vi responsive (đáp ứng kích thước màn) | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 67 | DETAIL_PANEL | `detail-panel-toggle` | Hành vi responsive (đáp ứng kích thước màn) | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 68 | DETAIL_PANEL | `detail-panel-layout` | Khả năng tiếp cận (trợ năng) | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 69 | DETAIL_PANEL | `detail-panel-toggle` | Khả năng tiếp cận (trợ năng) | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 70 | DETAIL_PANEL | `detail-panel-layout` | Bàn phím và tiêu điểm | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 71 | DETAIL_PANEL | `detail-panel-toggle` | Bàn phím và tiêu điểm | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 72 | DETAIL_PANEL | `detail-panel-layout` | Tương tác lớp phủ (hộp thoại, tấm trượt, popover) | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 73 | DETAIL_PANEL | `detail-panel-toggle` | Tương tác lớp phủ (hộp thoại, tấm trượt, popover) | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 74 | DETAIL_PANEL | `detail-panel-layout` | Nạp dữ liệu | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 75 | DETAIL_PANEL | `detail-panel-toggle` | Nạp dữ liệu | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 76 | DETAIL_PANEL | `detail-panel-layout` | Trạng thái bền hoặc trạng thái trên địa chỉ trang | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 77 | DETAIL_PANEL | `detail-panel-toggle` | Trạng thái bền hoặc trạng thái trên địa chỉ trang | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 78 | DETAIL_PANEL | `detail-panel-layout` | Phạm vi đường dẫn / mô-đun | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 79 | DETAIL_PANEL | `detail-panel-toggle` | Phạm vi đường dẫn / mô-đun | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 80 | DETAIL_PANEL | `detail-panel-layout` | Điều kiện kích hoạt thuận (khi nào nên nạp kỹ năng) | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 81 | DETAIL_PANEL | `detail-panel-toggle` | Điều kiện kích hoạt thuận (khi nào nên nạp kỹ năng) | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 82 | DETAIL_PANEL | `detail-panel-layout` | Điều kiện KHÔNG kích hoạt (trigger phủ định) | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 83 | DETAIL_PANEL | `detail-panel-toggle` | Điều kiện KHÔNG kích hoạt (trigger phủ định) | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 84 | DETAIL_PANEL | `detail-panel-layout` | Xung đột với chuẩn giao diện | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 85 | DETAIL_PANEL | `detail-panel-toggle` | Xung đột với chuẩn giao diện | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 86 | DETAIL_PANEL | `detail-panel-layout` | Nơi gọi và phụ thuộc (trong mã nguồn thật) | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 87 | DETAIL_PANEL | `detail-panel-toggle` | Nơi gọi và phụ thuộc (trong mã nguồn thật) | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 88 | DETAIL_PANEL | `detail-panel-layout` | Kỹ năng nội bộ liên quan | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 89 | DETAIL_PANEL | `detail-panel-toggle` | Kỹ năng nội bộ liên quan | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 90 | DETAIL_PANEL | `detail-panel-layout` | Rủi ro và tác dụng phụ | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 91 | DETAIL_PANEL | `detail-panel-toggle` | Rủi ro và tác dụng phụ | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 92 | DETAIL_PANEL | `detail-panel-layout` | Tương thích công cụ | **GIU_NGUYEN** | nguyên văn tại chỗ |
| 93 | DETAIL_PANEL | `detail-panel-toggle` | Tương thích công cụ | **CAP_NHAT_TAI_CHO** | phần còn lại giữ nguyên văn |
| 94 | R0 | `dual-key-standard` | Phần đầu tệp — tên, mô tả, phiên bản 1.0.0 | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 95 | R0 | `dual-key-standard` | Mục tiêu — vì sao tách đôi khoá hệ thống và mã nghiệp vụ | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 96 | R0 | `dual-key-standard` | Quy tắc 1 — khoá hệ thống bắt buộc có, dùng cho nối quan hệ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 97 | R0 | `dual-key-standard` | Quy tắc 2 — mã nghiệp vụ: dùng để hiển thị và ánh xạ, phải … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 98 | R0 | `dual-key-standard` | Quy tắc 3 — cấm tuyệt đối: cấm dùng mã nghiệp vụ làm khoá n… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 99 | R0 | `dual-key-standard` | Danh sách kiểm khi triển khai — 5 dòng: khoá chính, chọn dò… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 100 | R0 | `windows-dev-troubleshoot-quick` | Phần đầu tệp — tên, mô tả, số hiệu bản 1.0.0 | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 101 | R0 | `windows-dev-troubleshoot-quick` | Mục tiêu — kiểm nhanh trước khi sửa mã, tránh đi sai hướng | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 102 | R0 | `windows-dev-troubleshoot-quick` | Ghi chú ghép cặp 20/08/2026 — phân vai với kỹ năng rà toàn … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 103 | R0 | `windows-dev-troubleshoot-quick` | Trigger — bốn dấu hiệu kích hoạt | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 104 | R0 | `windows-dev-troubleshoot-quick` | Kiểm 1 — quan sát bảng điều khiển trình duyệt, bốn dạng lỗi… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 105 | R0 | `windows-dev-troubleshoot-quick` | Kiểm 2 — quan sát nhật ký cửa sổ đang chạy máy chủ dev, ba … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 106 | R0 | `windows-dev-troubleshoot-quick` | Hành động — dọn thư mục xây dựng rồi khởi động lại, khối lệ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 107 | R0 | `windows-dev-troubleshoot-quick` | Chống tái phát — thêm loại trừ chống vi-rút cho thư mục xây… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 108 | R0 | `windows-dev-troubleshoot-quick` | Mẫu báo cáo kết quả | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 109 | R0 | `windows-dev-troubleshoot-quick` | Danh sách đạt/không đạt sáu dòng | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 110 | R0 | `windows-dev-troubleshoot-quick` | Ưu điểm — nhanh, tránh sai hướng, dễ xác minh | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 111 | R0 | `windows-dev-troubleshoot-quick` | Nhược điểm và rủi ro — nếu là lỗi thật thì vẫn phải sửa mã,… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 112 | R0 | `windows-dev-troubleshoot-quick` | Khuyến nghị — Dùng khi (bốn điều kiện) và Tránh khi (hai đi… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 113 | R0 | `windows-dev-troubleshoot-quick` | Skills Liên Quan — trỏ tới kỹ năng ổn định bộ nhớ đệm và kỹ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 114 | R0 | `windows-dev-troubleshoot-quick` | Ca thật đã ghi — phiên 18/01/2026, nút thêm vật tư không bấ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 115 | R0 | `column-config-workflow` | Khối khai báo đầu tệp (tên · mô tả · phiên bản) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 116 | R0 | `column-config-workflow` | Mục Đích — nêu hai bề mặt áp dụng: bảng danh sách và khung … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 117 | R0 | `column-config-workflow` | Bước 1 — tác nhân xuất bảng liệt kê cột kèm số thứ tự | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 118 | R0 | `column-config-workflow` | Bước 2 — Chủ dự án gửi dãy số, kèm quy tắc dấu phân cách và… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 119 | R0 | `column-config-workflow` | Bước 3 mục 1 — sao lưu tệp trước khi sửa | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 120 | R0 | `column-config-workflow` | Bước 3 mục 2 — cập nhật phần đầu bảng và phần thân bảng the… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 121 | R0 | `column-config-workflow` | Bước 3 mục 3 — cập nhật các trường của khung chi tiết theo … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 122 | R0 | `column-config-workflow` | Bước 3 mục 4 — điều chỉnh bề rộng tối thiểu của bảng cho hợ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 123 | R0 | `column-config-workflow` | Bước 3 mục 5 — điều chỉnh số cột gộp cho ô trạng thái rỗng | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 124 | R0 | `column-config-workflow` | Danh mục kiểm 9 dòng | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 125 | R0 | `column-config-workflow` | Ví Dụ (minh hoạ đầu vào và đầu ra) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 126 | R0 | `column-config-workflow` | Lưu Ý — không áp dụng cho trình hướng dẫn nhiều bước và biể… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 127 | R0 | `column-config-workflow` | Lưu Ý — các trường vết kiểm (ngày tạo · người tạo · ngày sử… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 128 | R0 | `column-config-workflow` | Lưu Ý — có thể kết hợp với kỹ năng bố cục khung chi tiết | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 129 | R0 | `column-config-workflow` | (Ngoài tệp — nhãn máy sinh trong danh mục kỹ năng) phạm vi … | **XEM_LAI** | không bắt buộc giữ |
| 130 | R0 | `file-update-safe-workflow` | Phần đầu tệp — tên, mô tả, phiên bản 1.0.0 | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 131 | R0 | `file-update-safe-workflow` | Mục Đích — bảo đảm sửa tài liệu an toàn, không mất mát, tru… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 132 | R0 | `file-update-safe-workflow` | QUAN TRỌNG — 5 điều Agent PHẢI và 4 điều Agent KHÔNG ĐƯỢC | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 133 | R0 | `file-update-safe-workflow` | Khi Nào Áp Dụng — danh sách 5 điều kiện kích hoạt | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 134 | R0 | `file-update-safe-workflow` | KHÔNG cần workflow này khi — 3 trường hợp loại trừ | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 135 | R0 | `file-update-safe-workflow` | Bước 1 — Đọc toàn bộ tệp trước, kèm 4 ô kiểm | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 136 | R0 | `file-update-safe-workflow` | Bước 2 — Giữ nguyên nội dung hiện có, kèm cặp ví dụ đúng và… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 137 | R0 | `file-update-safe-workflow` | Bước 3 — Thêm, cập nhật hoặc sắp xếp lại, kèm mẫu mô hình | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 138 | R0 | `file-update-safe-workflow` | Bước 4 — Ghi số phiên bản khi thay đổi lớn, kèm mẫu khối ph… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 139 | R0 | `file-update-safe-workflow` | Bước 5 — Bảo đảm nhất quán, kèm 4 ô kiểm | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 140 | R0 | `file-update-safe-workflow` | Bước 6 — Bàn giao cho kỹ năng documentation-sync | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 141 | R0 | `file-update-safe-workflow` | Mẫu Báo Cáo Đầu Ra — báo cáo cập nhật tệp | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 142 | R0 | `file-update-safe-workflow` | Bảng Ô Kiểm Đạt/Không Đạt — 3 nhóm trước, trong, sau | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 143 | R0 | `file-update-safe-workflow` | Ưu Điểm — 4 gạch đầu dòng | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 144 | R0 | `file-update-safe-workflow` | Nhược Điểm / Rủi Ro — 3 gạch đầu dòng | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 145 | R0 | `file-update-safe-workflow` | Gợi Ý Khuyến Nghị — Dùng khi, Tránh khi, Thực hành tốt | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 146 | R0 | `file-update-safe-workflow` | Kỹ Năng Liên Quan — liệt kê 3 slug | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 147 | R0 | `file-update-safe-workflow` | Tham Chiếu — trỏ tới mục luật đánh số 6 trong .cursorrules,… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 148 | R0 | `windows-next-cache-stability` | Phần đầu tệp — tên, mô tả, phiên bản 1.0.0 | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 149 | R0 | `windows-next-cache-stability` | Mục tiêu — ưu tiên xử lý bộ nhớ đệm và khoá tệp trước khi đ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 150 | R0 | `windows-next-cache-stability` | Dấu hiệu nhận biết — bốn triệu chứng nguyên văn | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 151 | R0 | `windows-next-cache-stability` | Nguyên nhân phổ biến — vế phần mềm diệt vi-rút quét và khoá… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 152 | R0 | `windows-next-cache-stability` | Nguyên nhân phổ biến — vế khoá tệp tạm thời của hệ thống tệp | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 153 | R0 | `windows-next-cache-stability` | Nguyên nhân phổ biến — vế dải phiên bản Node >=20 <25 (Next… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 154 | R0 | `windows-next-cache-stability` | Bước 1 — phân nhánh: lỗi kiểu dữ liệu thì xử lý mã, lỗi thi… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 155 | R0 | `windows-next-cache-stability` | Bước 2 — dọn thư mục bộ nhớ đệm rồi chạy lại (xoá tay + gợi… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 156 | R0 | `windows-next-cache-stability` | Bước 2 — thiếu bước dừng máy chủ phát triển trước khi dọn | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 157 | R0 | `windows-next-cache-stability` | Bước 3 — khai báo loại trừ cho phần mềm diệt vi-rút (có nhú… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 158 | R0 | `windows-next-cache-stability` | Bước 3 — con trỏ tới sổ đường dẫn của dự án | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 159 | R0 | `windows-next-cache-stability` | Bước 3 — câu "hiện dùng Node 24.18.0" | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 160 | R0 | `windows-next-cache-stability` | Bước 4 — điều kiện được phép đụng mã | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 161 | R0 | `windows-next-cache-stability` | Kiểm chứng — bản dựng đạt hai lần liên tiếp và máy chủ phát… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 162 | R0 | `windows-next-cache-stability` | Kiểm chứng — lệnh chạy kèm cơ sở dữ liệu vẫn được nêu là đư… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 163 | R0 | `windows-next-cache-stability` | Ca đã gặp trong kho — lỗi trang tài liệu gốc, xử lý bằng dọ… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 164 | R0 | `windows-next-cache-stability` | Mục ghi trong sổ kỹ năng — trường triệu chứng của kỹ năng n… | **XEM_LAI** | không bắt buộc giữ |
| 165 | R0 | `windows-next-cache-stability` | Mục ghi trong sổ kỹ năng — trường điều kiện không dùng và n… | **XEM_LAI** | không bắt buộc giữ |
| 166 | R0 | `annotated-screenshot-review` | Khối khai báo đầu tệp (tên · mô tả · phiên bản) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 167 | R0 | `annotated-screenshot-review` | Tiêu đề tệp và ba lần tự xưng là nguồn chuẩn duy nhất (SSOT) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 168 | R0 | `annotated-screenshot-review` | Khối Mục tiêu — ba ý: đọc hết ảnh, không sót thông tin, sửa… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 169 | R0 | `annotated-screenshot-review` | Quy tắc bắt buộc — phải xem 100% số ảnh trong một tin nhắn … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 170 | R0 | `annotated-screenshot-review` | Quy tắc bắt buộc — bảng nghĩa ba loại ký hiệu (khoanh đỏ · … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 171 | R0 | `annotated-screenshot-review` | Quy tắc bắt buộc — gặp mâu thuẫn giữa các ảnh hoặc ghi chú … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 172 | R0 | `annotated-screenshot-review` | Danh mục thực thi — bước 1: đếm số ảnh đính kèm trong tin n… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 173 | R0 | `annotated-screenshot-review` | Danh mục thực thi — bóc từng vùng khoanh theo ba câu hỏi co… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 174 | R0 | `annotated-screenshot-review` | Danh mục thực thi — phân loại yêu cầu nào chỉ thuộc giao di… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 175 | R0 | `annotated-screenshot-review` | Danh mục thực thi — tổng hợp thành một danh sách việc không… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 176 | R0 | `annotated-screenshot-review` | Khối Định dạng trả lời khuyến nghị (liệt kê theo Ảnh 1 / Ản… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 177 | R0 | `annotated-screenshot-review` | Khối Anti-patterns — ba điều cấm (đọc một ảnh rồi kết luận … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 178 | R0 | `schema-visualization` | Phần khai báo đầu tệp (dòng 1–5): tên · mô tả · phiên bản 1… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 179 | R0 | `schema-visualization` | Tiêu đề chính (dòng 7): tên kỹ năng kèm nhãn SSOT trong ngo… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 180 | R0 | `schema-visualization` | Mục tiêu (dòng 9–15) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 181 | R0 | `schema-visualization` | Điều kiện kích hoạt — mục Trigger trong thân tệp (dòng 16–2… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 182 | R0 | `schema-visualization` | Khuôn đầu ra — dòng nhãn bắt buộc (dòng 21–22) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 183 | R0 | `schema-visualization` | Khối 1 — bảng lược đồ cơ sở dữ liệu (dòng 23–39) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 184 | R0 | `schema-visualization` | Khối 2 — bảng các cột hiển thị hiện trạng của giao diện (dò… | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 185 | R0 | `schema-visualization` | Khối 3 — bảng các trường của biểu mẫu nhập liệu (dòng 56–73) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 186 | R0 | `schema-visualization` | Khối 4 — luồng logic bổ sung: sinh mã tự động và ràng buộc … | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 187 | R0 | `schema-visualization` | Mã ví dụ đóng gói khuôn vào một biến chuỗi (dòng 89–113) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 188 | R0 | `schema-visualization` | Danh mục tự kiểm 5 điểm (dòng 114–121) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 189 | R0 | `schema-visualization` | Ưu điểm (dòng 122–126) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 190 | R0 | `schema-visualization` | Nhược điểm (dòng 127–130) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 191 | R0 | `schema-visualization` | Khuyến nghị dùng khi / tránh khi (dòng 131–140) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 192 | R0 | `schema-visualization` | Nguồn gốc (dòng 141–142) | **BAO_TOAN** | BẮT BUỘC GIỮ |
| 193 | R0 | `schema-visualization` | (KHE HỞ) Mục NGUỒN DỮ LIỆU — KHÔNG TỒN TẠI trong tệp | **XEM_LAI** | không bắt buộc giữ |

**Kiểm tra hợp lệ:** không chức năng nguồn nào biến mất mà không có dòng. Không đề xuất gộp hay thay thế nào tồn tại trong bản cuối ⇒ yêu cầu *"mọi đề xuất gộp phải có ánh xạ đầy đủ"* được thoả mãn một cách hiển nhiên.

---

## 11. BẢN ĐỒ PHỤ THUỘC VÀ CHỒNG CHÉO TRỰC TIẾP

| Kỹ năng | Số nơi viện dẫn | Nguồn viện dẫn đáng chú ý | Ý nghĩa |
|---|---|---|---|
| `update-work-log` | 7 | bản ghi phiên bản của dự án · tệp sổ công việc thật | **Có ràng buộc với hiện vật thật** — bằng chứng chống lại việc thay thế |
| `versioning-auto-log` | 23 | kho lưu luật cũ · sổ đăng ký · tệp sổ công việc | Là đầu mối, được trỏ tới nhiều nhất trong cụm |
| `versioning-change-history` | 23 | kho lưu luật cũ · sổ đăng ký | Vẫn được luật cũ trỏ tới đích danh |
| `version-bump-on-feature` | 17 | kho lưu luật cũ · sổ đăng ký | Vẫn được luật cũ trỏ tới đích danh |
| `detail-panel-layout` | 26 | sổ đăng ký nguồn chuẩn giao diện · **6 kỹ năng khác trỏ về** | **Lan rộng nhất trong 13** — sửa nó ảnh hưởng 6 kỹ năng |
| `detail-panel-toggle` | 10 | kho lưu luật cũ · sổ đăng ký | **0 kỹ năng khác trỏ về** — sửa an toàn hơn nhiều |
| `annotated-screenshot-review` | 12 | sổ đăng ký nguồn chuẩn giao diện (nhóm 11 bị khoá) | Bị khoá không sửa |
| `dual-key-standard` | 13 | các báo cáo rà soát mô-đun | Là bản sống duy nhất của quy tắc khoá kép |
| `windows-next-cache-stability` | 15 | tài liệu triển khai · nhật ký quản trị | Gắn với quy trình vận hành thật |
| `schema-visualization` | 9 | sổ đăng ký | — |
| `column-config-workflow` | 8 | sổ đăng ký · tệp bố cục panel | Phụ thuộc tệp bố cục panel |
| `file-update-safe-workflow` | 4 | sổ đăng ký | Ít ràng buộc nhất |
| `windows-dev-troubleshoot-quick` | 4 | sổ đăng ký | Ít ràng buộc nhất |

**Cảnh báo đón đầu từ bộ phản biện:** `update-work-log` và `file-update-safe-workflow` **rất dễ bị gọi nhầm là trùng** ở lượt sau, nhưng **không phải**: khác điều kiện kích hoạt (sổ công việc khi có sửa đồng thời ↔ tệp tài liệu quản trị), khác rủi ro (mất mục của phiên khác ↔ xoá mất nội dung đang có), khác đầu ra. Đề nghị ghi ba khác biệt này vào cả hai tệp ngay khi được phép sửa.

---

## 12. PHÁT HIỆN VỀ CÔNG CỤ VÀ KHẢ NĂNG PHÁT HIỆN

| Công cụ | Phiên bản | Phát hiện bản địa | Cách lấy bằng chứng | Giới hạn |
|---|---|---|---|---|
| Cursor | 3.18.25 (ổn định) | ⚠️ **NOT_CHECKED** | không có kênh quan sát phiên Cursor từ phiên Claude Code | KHÔNG được suy đoán theo cả hai hướng |
| Claude Code | 2.1.259 trong Cursor | ⛔ **PROVEN_FALSE** | quan sát trực tiếp danh sách kỹ năng phiên được cấp: 17 kỹ năng dựng sẵn, 0 kỹ năng ERP | phép đo tại một phiên trên một bản công cụ |

> 📌 **Điều này KHÔNG mới.** Sổ đăng ký nguồn chuẩn giao diện đã ghi từ **23/08/2026** rằng Claude Code không đọc được thư mục kỹ năng của Cursor — và đó chính là **lý do** Chủ dự án cho gộp 11 kỹ năng giao diện vào chuẩn. Phép đo hôm nay là **tái xác nhận**, không phải phát hiện mới. Em nói rõ để không nhận công của quyết định cũ.

---

## 13. KẾ HOẠCH THỬ KÍCH HOẠT — CHỈ THIẾT KẾ, CHƯA CHẠY

| # | Dạng thử | Mục đích | Đạt khi |
|---|---|---|---|
| T1 | Khớp đúng | kỹ năng đúng được nạp khi yêu cầu nêu thẳng | nạp đúng, ổn định qua 3 lượt |
| T2 | Khớp diễn đạt khác | vẫn nạp đúng khi diễn đạt khác đi | nạp đúng, ổn định |
| T3 | Gần-mà-không-phải | KHÔNG nạp khi chỉ dùng chung từ khoá | 0 lần nạp nhầm |
| T4 | Sai đường dẫn / mô-đun | KHÔNG nạp khi ngoài phạm vi | 0 lần nạp nhầm |
| T5 | Đụng độ trong cùng cụm | chọn đúng khi 2 kỹ năng cùng cụm cạnh tranh | chọn đúng, có lý do |
| T6 | Đụng độ với cụm lân cận | không bị hút sang cụm hộp thoại/lớp phủ | 0 lần nạp nhầm |
| T7 | Mồi thử tác dụng phụ | không sinh tác dụng phụ nào | **0 tác dụng phụ tuyệt đối** |
| T8 | Mốc nền không cần kỹ năng | không nạp kỹ năng cho việc thường | 0 lần nạp |
| T9 | Đối chiếu hai công cụ | Cursor và Claude Code cho kết quả nhất quán | ghi rõ khác biệt nếu có |
| T10 | Bền qua nén phiên | sau khi phiên bị nén vẫn đúng | không đổi hành vi |

**Điều kiện bắt buộc khi được phép chạy:** tối thiểu **ba lượt mỗi dạng** · **không** tự gọi kỹ năng thuộc nhóm chỉ-gọi-đích-danh hay bị khoá · **0 tác dụng phụ** với nhóm chỉ-đọc · mã băm nguồn thực nạp phải khớp sổ đăng ký · ghi chính xác tên và phiên bản công cụ · **lỗi hạ tầng KHÔNG được tính là đạt hay không đạt**.

---

## 14. KẾ HOẠCH THI HÀNH THEO GIAI ĐOẠN — CHƯA THỰC HIỆN GIAI ĐOẠN NÀO

| Giai đoạn | Việc | Bề mặt bị đụng | Cổng Chủ dự án | Kiểm thử | Điều kiện dừng | Đường lui | Trạng thái |
|---|---|---|---|---|---|---|---|
| **STAGE 0** | Khoá trạng thái và sao lưu | không đụng tệp nào — chỉ ghi lại mã băm hiện tại của 13 tệp | không cần | ghi mã băm và số dòng từng tệp | không đọc được tệp nào | không cần | `CHƯA BẮT ĐẦU` |
| **STAGE 1** | Chủ dự án duyệt bản kế hoạch nội dung chính xác | không đụng tệp nào | BẮT BUỘC | không | Chủ dự án chưa trả lời | không cần | `CHƯA BẮT ĐẦU` |
| **STAGE 2** | Sửa nội dung tại chỗ đúng phần đã duyệt | chỉ các tệp kỹ năng được duyệt đích danh; KHÔNG đụng hai tệp Chủ dự án đã khoá | BẮT BUỘC | cổng đếm hai điều kiện: tổng số mục SAU ≥ TRƯỚC và mỗi mục rời đi có đúng một d… | cổng đếm đỏ | khôi phục từ Git theo từng tệp | `CHƯA BẮT ĐẦU` |
| **STAGE 3** | Thêm bí danh, lịch sử và ánh xạ cũ sang mới | mục lịch sử trong chính tệp kỹ năng | BẮT BUỘC | mọi mục bị thay có đúng một dòng con trỏ | thiếu con trỏ | khôi phục từ Git | `CHƯA BẮT ĐẦU` |
| **STAGE 4** | Cổng tĩnh: nội dung, nguồn gốc và đối chiếu chuẩn | không sửa — chỉ chạy cổng | không | ba cổng kỹ năng + cổng đối chiếu chuẩn giao diện | bất kỳ cổng nào đỏ | lui về STAGE 2 | `CHƯA BẮT ĐẦU` |
| **STAGE 5** | Kho thử tạm để đo phát hiện của công cụ | kho TẠM riêng, KHÔNG phải kho ERP | BẮT BUỘC | một kỹ năng vô hại chỉ trả về phiếu nhận diện | kho thử ảnh hưởng kho ERP | xoá kho thử | `CHƯA BẮT ĐẦU` |
| **STAGE 6** | Thử kích hoạt tám dạng | kho thử tạm | không | tám dạng, tối thiểu ba lượt mỗi dạng | có tác dụng phụ, hoặc lỗi hạ tầng | không cần | `CHƯA BẮT ĐẦU` |
| **STAGE 7** | Một lượt thử thật có người giám sát | một kỹ năng duy nhất, gọi đích danh | BẮT BUỘC RIÊNG | quan sát phiếu nhận và tác dụng phụ | bất kỳ tác dụng phụ nào | ngừng gọi | `CHƯA BẮT ĐẦU` |
| **STAGE 8** | Đổi sổ đăng ký hoặc dựng bản chiếu | sổ đăng ký, bản chiếu sang thư mục công cụ | BẮT BUỘC RIÊNG | cổng lệch bản chiếu | thiếu bằng chứng | gỡ bản chiếu | `CHƯA BẮT ĐẦU` |
| **STAGE 9** | Kiểm đường lui, đẩy báo cáo và bàn giao Notion | kho báo cáo công khai | cổng an toàn công khai | thử lui thật một tệp | lui không thành công | chính bước này là đường lui | `CHƯA BẮT ĐẦU` |

---

## 15. XỬ LÝ MÂU THUẪN SỐ LIỆU VÀ TÀI LIỆU

### G-1 — Chỉ số nội dung bị cấm 11 / 12 / 17

- **Số đo:** conflicting_skill_count=36 · forbidden_skill_identity_count=17 · forbidden_string_entries=18 · forbidden_occurrence_count=2007 · forbidden_within_conflicting=11
- **Phán quyết:** **GIẢI QUYẾT XONG**
- **Giải thích:** Cả ba con số đều đúng nhưng ĐẾM TRÊN BA TẬP KHÁC NHAU. 17 = mọi kỹ năng mang ít nhất một chuỗi bị cấm, không phân biệt phán quyết đối chiếu chuẩn. 11 = phần giao giữa 17 đó với nhóm CHỌI CHUẨN. 12 = 11 cộng thêm một kỹ năng thuộc nhóm BỔ SUNG — một phép trộn tuỳ tiện dùng ở báo cáo đầu tiên. Báo cáo danh mục tự mâu thuẫn vì phần văn xuôi dùng 11 còn bảng đếm dùng 17 mà không dán nhãn tập.
- **Đề xuất sửa:** Sửa báo cáo danh mục: dán nhãn rõ ba chỉ số riêng biệt thay vì một con số trần. Bỏ hẳn con số 12.

### G-2 — Số khoá JSON 27 so với 31

- **Số đo:** hợp đồng hiện hành 31 khoá · artefact 27 khoá · thiếu đúng: rule_scope, freshness, next_review, source_changed · thừa 0
- **Phán quyết:** **GIẢI QUYẾT XONG — TRỄ DO THỜI ĐIỂM, KHÔNG PHẢI BÁO CÁO SAI**
- **Giải thích:** Hợp đồng được nâng lên bản 1.2 lúc 00:04 ngày 04/09/2026, thêm đúng bốn khoá. Commit artefact được tạo lúc 00:09 cùng ngày — sau 5 phút. Artefact tuân đúng hợp đồng bản 1.1 đang hiệu lực tại thời điểm sinh ra nó.
- **Đề xuất sửa:** Sinh lại artefact danh mục với bốn khoá bổ sung. Đây là gói việc tiếp theo, KHÔNG phải sửa lỗi.

### G-3 — Khoá cấp cao nhất 10 so với hợp đồng 7

- **Số đo:** thừa 3: private_source_checkpoint_note, governance_checkpoint, worktree_dirty_files_at_generation
- **Phán quyết:** **PHÁT HIỆN MỚI — CHỜ CHỦ DỰ ÁN**
- **Giải thích:** Hợp đồng liệt kê 7 khoá cấp cao và không nói rõ có cấm khoá thừa hay không. Ba khoá thừa là do kiểm toán viên chủ động thêm để minh bạch. Chưa nơi nào ghi nhận.
- **Đề xuất sửa:** Chủ dự án quyết: đưa ba khoá này vào hợp đồng, hay bỏ khỏi artefact.

### G-4 — Kích thước cụm trang danh sách/bảng 10 so với 16

- **Số đo:** định nghĩa khớp-chuỗi-con = 10 · định nghĩa theo lĩnh vực trong sổ đăng ký = 4
- **Phán quyết:** **CON SỐ 16 KHÔNG TÁI HIỆN ĐƯỢC**
- **Giải thích:** Báo cáo danh mục đã công bố ghi 10 theo định nghĩa khớp-chuỗi-con. Không phép đo nào cho ra 16.
- **Đề xuất sửa:** Ghi rõ ĐỊNH NGHĨA THÀNH VIÊN CỤM ngay cạnh mỗi con số. Bỏ con số 16 nếu không có nguồn.

### G-5 — Kích thước cụm lược đồ/di trú 8 so với 14

- **Số đo:** định nghĩa khớp-chuỗi-con = 8 · định nghĩa theo lĩnh vực trong sổ đăng ký = 15
- **Phán quyết:** **CON SỐ 14 KHÔNG TÁI HIỆN ĐƯỢC**
- **Giải thích:** Báo cáo đã công bố ghi 8. Định nghĩa theo lĩnh vực cho 15, không phải 14.
- **Đề xuất sửa:** Như trên — công bố kèm định nghĩa.

### G-6 — Mâu thuẫn NGAY TRONG chuẩn giao diện về đổ bóng panel

- **Số đo:** mục 10.0 điều 3 (Chủ dự án chốt 03/09/2026) yêu cầu đổ bóng vừa và ghi rõ KHÔNG dùng mức đậm · dòng đặc tả lưới của mục 10 (23/08/2026) vẫn ghi mức đậm
- **Phán quyết:** **TÀI LIỆU LỖI THỜI — CHỜ CHỦ DỰ ÁN**
- **Giải thích:** Hai dòng cách nhau 7 dòng trong cùng một mục, hai giá trị ngược nhau. Quyết định mới hơn ngày 03/09 chưa được lan xuống dòng đặc tả cũ. Đây là RELATED_EVIDENCE nằm ngoài 13 identity nhưng quyết định giá trị nào đúng cho cả hai kỹ năng panel.
- **Đề xuất sửa:** Cập nhật dòng đặc tả cũ theo quyết định 03/09, giữ nguyên văn dòng cũ ở mục lịch sử. KHÔNG thực hiện trong lượt này.

### G-7 — Phạm vi audit 13 so với 7 identity

- **Số đo:** ba trang Notion ERP đều tự mâu thuẫn trong cùng bản cập nhật: phần đầu trang ghi Chủ dự án duyệt 13, phần việc-kế-tiếp vẫn ghi 7
- **Phán quyết:** **MỞ — ĐÃ THI HÀNH THEO 13**
- **Giải thích:** Chỉ thị gửi tới Agent IDE liệt kê đích danh 13 identity nên lượt này thi hành theo 13. Nhưng ba trang Notion vẫn còn câu chữ 7 chưa được cập nhật.
- **Đề xuất sửa:** TanPhatAI đồng bộ phần việc-kế-tiếp của ba trang cho khớp phạm vi 13.


> **Nguyên tắc đã tuân:** không bẻ cong tài liệu hay mã nguồn đang ĐÚNG để chạy theo báo cáo SAI. Hai mâu thuẫn **G-4** và **G-5** kết luận là *không tái hiện được* — em **không** đi sửa con số đã công bố cho khớp một con số không có nguồn.

---

## 16. GÓI QUYẾT ĐỊNH CHO CHỦ DỰ ÁN

### QD-1 — Có sửa nhãn quan hệ ở đầu ba tệp versioning cũ không?

| | |
|---|---|
| **1. Hiện đang có gì** | Ba tệp khai "bị thay thế bởi" tệp đầu mối. |
| **2. Vấn đề chính xác** | Đo được nhãn đó SAI hiện vật: tệp đầu mối KHÔNG chứa ba biện pháp an toàn ghi song song, mà chính nó tự khai có rủi ro ghi trùng. Để nguyên nhãn thì phiên sau tiếp tục hạ ba tệp xuống hạng phụ và tri thức mất dần. |
| **3. Đề xuất sau audit** | Đổi nhãn thành "hỗ trợ cho" hoặc "bị thay một phần", ghi rõ phần KHÔNG bị thay. Giữ nguyên toàn bộ nội dung. |
| **4. Nội dung cũ được giữ ở đâu** | Toàn bộ nội dung ba tệp giữ nguyên văn. |
| **5. Rủi ro và tác động** | Rất thấp — chỉ đổi một dòng khai báo ở mỗi tệp. |
| **6. Công sức** | Nhỏ. |
| **7. Kiểm thử** | Đối chiếu lại sáu từ khoá an toàn song song trên tệp đầu mối. |
| **8. Đường lui** | Khôi phục dòng cũ từ Git. |
| **9. ⭐ CẦN ANH QUYẾT** | **ĐỒNG Ý đổi nhãn, hay GIỮ NGUYÊN nhãn hiện tại?** |

### QD-2 — Có sửa đúng phần chọi chuẩn của kỹ năng bật-tắt panel không?

| | |
|---|---|
| **1. Hiện đang có gì** | Kỹ năng bật-tắt panel bị chấm là CHỌI CHUẨN với 6 điểm chọi và một chuỗi bị cấm. |
| **2. Vấn đề chính xác** | Theo luật chọn kỹ năng, loại chọi chuẩn chỉ được đọc lấy bối cảnh và CẤM chép giá trị. Nhưng tệp vẫn nằm đó với giá trị sai, phiên sau có thể chép nhầm. |
| **3. Đề xuất sau audit** | Sửa ĐÚNG 6 điểm chọi cho khớp chuẩn mục 10, giữ trọn phần còn lại, phần bị thay chuyển xuống mục lịch sử trong chính tệp. |
| **4. Nội dung cũ được giữ ở đâu** | Toàn bộ phần không chọi, gồm điều kiện kích hoạt và danh mục kiểm. |
| **5. Rủi ro và tác động** | Thấp — không đụng mã nguồn, chỉ đụng tài liệu kỹ năng. |
| **6. Công sức** | Vừa. |
| **7. Kiểm thử** | Chạy lại cổng đối chiếu kỹ năng với chuẩn giao diện. |
| **8. Đường lui** | Khôi phục từ Git. |
| **9. ⭐ CẦN ANH QUYẾT** | **ĐỒNG Ý sửa đúng phần chọi, hay GIỮ NGUYÊN?** |

### QD-3 — Hai kỹ năng đang bị khoá "không xoá không sửa" thì xử thế nào khi chúng vẫn còn chuỗi bị cấm?

| | |
|---|---|
| **1. Hiện đang có gì** | Ngày 23/08/2026 Chủ dự án chốt 11 kỹ năng giao diện đã gộp vào chuẩn, thư mục gốc GIỮ NGUYÊN làm lưu trữ, không xoá không sửa. Hai trong 13 identity của lượt này nằm trong nhóm đó. |
| **2. Vấn đề chính xác** | Cả hai vẫn mang chuỗi mà chuẩn cấm. Quyết định "không sửa" và nhu cầu "gỡ chuỗi cấm" mâu thuẫn nhau. |
| **3. Đề xuất sau audit** | Ba lựa chọn: (a) giữ nguyên tuyệt đối và chấp nhận rủi ro chép nhầm, đổi lại bổ sung một dòng cảnh báo ở sổ đăng ký; (b) mở ngoại lệ HẸP chỉ để gỡ chuỗi cấm, không đụng gì khác; (c) chuyển cả hai ra khỏi tầm phát hiện mà không xoá. |
| **4. Nội dung cũ được giữ ở đâu** | Toàn bộ nội dung trong mọi lựa chọn. |
| **5. Rủi ro và tác động** | Lựa chọn (a) giữ nguyên rủi ro; (b) đụng vào quyết định đã khoá; (c) đổi cấu trúc thư mục. |
| **6. Công sức** | (a) rất nhỏ · (b) nhỏ · (c) vừa. |
| **7. Kiểm thử** | Cổng đối chiếu kỹ năng với chuẩn. |
| **8. Đường lui** | Khôi phục từ Git. |
| **9. ⭐ CẦN ANH QUYẾT** | **Chọn (a), (b) hay (c)?** |

### QD-4 — Năm khối tri thức của kỹ năng đọc ảnh chú thích chưa có ở chuẩn — bổ sung vào chuẩn hay để nguyên ở kỹ năng?

| | |
|---|---|
| **1. Hiện đang có gì** | Kỹ năng này được xếp là ĐÃ GỘP vào chuẩn mục 20.9. Nhưng đo được mục 20.9 chỉ có 4 gạch đầu dòng, trong khi kỹ năng gốc có 47 dòng. |
| **2. Vấn đề chính xác** | Năm khối còn giá trị KHÔNG có ở đích: luật dừng-và-hỏi khi các ảnh mâu thuẫn; bước đếm số ảnh; phân loại yêu cầu chỉ-giao-diện hay chạm-dữ-liệu; khuôn trả lời theo từng vùng khoanh; ba điều cấm gồm cấm lan sang mô-đun khác. Nhãn ĐÃ GỘP hiện đang nói quá. |
| **3. Đề xuất sau audit** | Bổ sung năm khối đó vào chuẩn mục 20.9 TRƯỚC, rồi mới giữ nhãn đã gộp. Đúng nguyên tắc gộp-trước-hạ-nhãn-sau mà Chủ dự án đã chốt. KHÔNG sửa tệp kỹ năng. |
| **4. Nội dung cũ được giữ ở đâu** | Tệp kỹ năng giữ nguyên tuyệt đối theo quyết định 23/08. |
| **5. Rủi ro và tác động** | Thấp — chỉ thêm nội dung vào chuẩn, cổng đếm bảo đảm không mất mục. |
| **6. Công sức** | Vừa. |
| **7. Kiểm thử** | Cổng đếm mục của tệp chuẩn: SAU ≥ TRƯỚC. |
| **8. Đường lui** | Khôi phục tệp chuẩn từ Git. |
| **9. ⭐ CẦN ANH QUYẾT** | **ĐỒNG Ý bổ sung năm khối vào chuẩn, hay để nguyên trạng?** |

### QD-5 — Sửa mâu thuẫn về đổ bóng panel ngay trong chuẩn giao diện?

| | |
|---|---|
| **1. Hiện đang có gì** | Trong cùng mục 10 của chuẩn có hai giá trị đổ bóng ngược nhau, cách nhau 7 dòng. |
| **2. Vấn đề chính xác** | Điều bắt buộc số 3 do Chủ dự án chốt 03/09 nói dùng mức vừa và ghi rõ KHÔNG dùng mức đậm; dòng đặc tả lưới từ 23/08 vẫn ghi mức đậm. Ai đọc dòng nào thì làm theo dòng đó. |
| **3. Đề xuất sau audit** | Cập nhật dòng đặc tả cũ theo quyết định mới hơn, giữ nguyên văn dòng cũ ở mục lịch sử của chính tệp chuẩn. |
| **4. Nội dung cũ được giữ ở đâu** | Dòng cũ giữ nguyên văn ở mục lịch sử. |
| **5. Rủi ro và tác động** | Thấp. |
| **6. Công sức** | Rất nhỏ. |
| **7. Kiểm thử** | Cổng đếm mục của tệp chuẩn. |
| **8. Đường lui** | Khôi phục từ Git. |
| **9. ⭐ CẦN ANH QUYẾT** | **ĐỒNG Ý cập nhật, hay giữ nguyên và ghi nợ?** |

### QD-6 — Bốn khoá còn thiếu của artefact danh mục — sinh lại khi nào?

| | |
|---|---|
| **1. Hiện đang có gì** | Hợp đồng đòi 31 khoá mỗi bản ghi; artefact đã nộp có 27. Thiếu đúng bốn khoá. |
| **2. Vấn đề chính xác** | Không phải lỗi: hợp đồng đổi lúc 00:04, artefact tạo lúc 00:09. Nhưng TanPhatAI cần đủ 31 khoá để nhập danh mục. |
| **3. Đề xuất sau audit** | Sinh lại artefact danh mục với bốn khoá bổ sung, thành một gói việc riêng. |
| **4. Nội dung cũ được giữ ở đâu** | Toàn bộ 128 bản ghi hiện có. |
| **5. Rủi ro và tác động** | Rất thấp — chỉ sinh lại tệp báo cáo, không đụng kho mã nguồn. |
| **6. Công sức** | Nhỏ. |
| **7. Kiểm thử** | Kiểm 31 khoá và checksum. |
| **8. Đường lui** | Giữ artefact cũ. |
| **9. ⭐ CẦN ANH QUYẾT** | **ĐỒNG Ý sinh lại, hay chấp nhận 27 khoá?** |

### QD-7 — Ba khoá cấp cao thừa so với hợp đồng — giữ hay bỏ?

| | |
|---|---|
| **1. Hiện đang có gì** | Artefact có 10 khoá cấp cao, hợp đồng liệt kê 7. |
| **2. Vấn đề chính xác** | Ba khoá thừa do kiểm toán viên thêm để minh bạch. Hợp đồng không nói có cấm hay không. |
| **3. Đề xuất sau audit** | Đưa ba khoá vào hợp đồng chính thức, hoặc bỏ khỏi artefact. |
| **4. Nội dung cũ được giữ ở đâu** | Không mất nội dung ở lựa chọn nào. |
| **5. Rủi ro và tác động** | Rất thấp. |
| **6. Công sức** | Rất nhỏ. |
| **7. Kiểm thử** | Kiểm cấu trúc. |
| **8. Đường lui** | Không cần. |
| **9. ⭐ CẦN ANH QUYẾT** | **ĐƯA VÀO hợp đồng, hay BỎ khỏi artefact?** |

### QD-8 — Ba trang Notion còn ghi phạm vi 7 trong khi Chủ dự án đã duyệt 13 — ai sửa?

| | |
|---|---|
| **1. Hiện đang có gì** | Phần đầu ba trang ghi duyệt 13 identity; phần việc-kế-tiếp vẫn ghi 7. |
| **2. Vấn đề chính xác** | Lượt này đã thi hành theo 13 vì chỉ thị liệt kê đích danh 13. Nếu không sửa, lượt sau đọc phần việc-kế-tiếp sẽ hiểu sai phạm vi. |
| **3. Đề xuất sau audit** | TanPhatAI cập nhật phần việc-kế-tiếp của ba trang. Agent IDE KHÔNG được sửa Notion. |
| **4. Nội dung cũ được giữ ở đâu** | Nội dung cũ giữ ở mục lịch sử của trang. |
| **5. Rủi ro và tác động** | Thấp. |
| **6. Công sức** | Nhỏ. |
| **7. Kiểm thử** | Đọc lại ba trang. |
| **8. Đường lui** | Không cần. |
| **9. ⭐ CẦN ANH QUYẾT** | **Giao TanPhatAI cập nhật, hay giữ nguyên?** |


> Không mục nào ở trên đụng tới dữ liệu, quyền hay môi trường vận hành. Toàn bộ chỉ đụng **tài liệu kỹ năng** và **tài liệu chuẩn**.

---

## 17. KẾT QUẢ SOÁT BẢO TOÀN TRI THỨC

Bộ phản biện chấm bản đề xuất ban đầu là **KHÔNG ĐẠT** — và em **đồng ý**. Đây là điều tốt: nó bắt đúng ba chỗ có thể làm mất tri thức của anh.

**Ba lý do không đạt, và cách em đã sửa:**

1. **Phán quyết duy nhất động tới cấu trúc lại là phán quyết KHÔNG CÓ ÁNH XẠ.** `annotated-screenshot-review` được đề xuất *gộp* mà không kèm một dòng ánh xạ nào; đo lại thì đích gộp chỉ có **4 gạch đầu dòng** trong khi kỹ năng gốc có **47 dòng**, và **5 khối** còn giá trị không có ở đích.
   → **Em đã BÁC đề xuất gộp**, chuyển thành **GIỮ NGUYÊN**, và tách việc bổ sung 5 khối vào chuẩn thành quyết định riêng **QD-4**.

2. **Cụm versioning tự mâu thuẫn giữa phán quyết và bản thiết kế** — phán quyết nói giữ bốn tệp, bản thiết kế lại mô tả một lõi dùng chung (tức gộp trá hình).
   → **Em đã chốt lại**: bản thiết kế là **kế hoạch nội dung cho tệp đầu mối**, ba tệp hỗ trợ giữ nguyên vị trí và mục độc nhất.

3. **Cụm panel giữ đủ hai tệp nhưng bỏ trống việc sửa phần chọi.**
   → **Em đã tách rõ**: tệp bật-tắt được đề xuất sửa đúng phần chọi (**QD-2**); tệp bố cục bị Chủ dự án khoá nên phải hỏi trước (**QD-3**).

**5 chỗ bị gọi nhầm là trùng lặp** đã được bộ phản biện chỉ ra và em giữ nguyên cảnh báo đó trong báo cáo — xem mục 11.

---

## 18. MA TRẬN HOÀN THÀNH HẠNG MỤC

| # | Hạng mục yêu cầu | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | Báo cáo Markdown | ✅ `PROPOSED` | chính tệp này |
| 2 | Artefact JSON | ✅ `PROPOSED` | 193 dòng chức năng |
| 3 | Tóm tắt điều hành | ✅ `PROPOSED` | mục 1 |
| 4 | Bảng trạng thái và mức bằng chứng | ✅ `MEASURED_ONLY` | mục 2 |
| 5 | Bản tiền-đọc Notion | ⚠️ `MEASURED_ONLY` — **PARTIAL** | đọc 4/7 nguồn |
| 6 | Bản khai danh tính dự án–tài liệu | ✅ `MEASURED_ONLY` | mục 4 |
| 7 | Sổ quy trách nhiệm tác nhân | ✅ `MEASURED_ONLY` | mục 5 |
| 8 | Ma trận đối chiếu bốn lớp | ✅ `MEASURED_ONLY` | mục 6 |
| 9 | Ma trận năng lực cụm versioning | ✅ `AUDITED_ONLY` | 59 dòng |
| 10 | Bản thiết kế canonical versioning | ✅ `PROPOSED` | mục 7.1 — **chưa được duyệt thi hành** |
| 11 | Ma trận ba phương án panel | ✅ `PROPOSED` | mục 8 |
| 12 | Bảy hồ sơ R0 | ✅ `AUDITED_ONLY` | 100 mục nội dung |
| 13 | Bảng tổng hợp | ✅ `AUDITED_ONLY` | 193 dòng |
| 14 | Bản đồ phụ thuộc và chồng chéo | ✅ `MEASURED_ONLY` | mục 11 |
| 15 | Phát hiện về công cụ | ✅ `MEASURED_ONLY` | Cursor vẫn `NOT_CHECKED` |
| 16 | Kế hoạch thử kích hoạt | ✅ `PROPOSED` | 10 dạng — **chưa chạy dạng nào** |
| 17 | Kế hoạch thi hành theo giai đoạn | ✅ `PROPOSED` | 10 giai đoạn — **chưa bắt đầu giai đoạn nào** |
| 18 | Xử lý mâu thuẫn số liệu | ✅ `MEASURED_ONLY` | 5 đóng · 2 chờ Chủ dự án |
| 19 | Gói quyết định cho Chủ dự án | ✅ `PROPOSED` | 8 quyết định |
| 20 | Ma trận hoàn thành | ✅ | chính mục này |
| 21 | Khoá chốt / Việc mở / Bước kế tiếp | ✅ | mục 19 |
| 22 | Đường dẫn, commit và checksum | ✅ | mục 20 |

> **Không hạng mục nào ghi ĐÃ THI CÔNG · ĐÃ KIỂM THỬ · ĐÃ TRIỂN KHAI · ĐÃ NẠP LÚC CHẠY** — đúng giới hạn của lượt chỉ-đọc.

---

## 19. KHOÁ CHỐT · VIỆC MỞ · BƯỚC KẾ TIẾP

### 🔒 KHOÁ CHỐT

1. **Toàn bộ 13 kỹ năng đều được GIỮ.** 11 cập nhật tại chỗ · 2 giữ nguyên tuyệt đối. **Không đề xuất xoá, không đề xuất gộp, không đề xuất thay thế nào tồn tại trong bản cuối.**
2. **Nhãn "đã bị thay thế" của ba kỹ năng versioning là sai hiện vật** — chứng minh bằng phép đo, không bằng cảm tính.
3. **Hai kỹ năng đã bị Chủ dự án khoá "không xoá, không sửa" từ 23/08/2026** — kiểm toán viên tôn trọng và đã bác đề xuất trái quyết định đó.
4. **Duyệt audit không phải duyệt sửa.** Không kỹ năng nào được cấp quyền kích hoạt, thử nghiệm hay bật trong lượt này.
5. **Mẫu của nhà cung cấp không thay được thẩm quyền ERP.** Ánh xạ sang bộ kỹ năng bên ngoài vẫn là **NOT_CHECKED** — không cài ở máy này.
6. **Phát hiện bản địa không cấp quyền sửa.** Công bố báo cáo không chứng minh đã thi hành hay đã chạy.

### ❓ VIỆC MỞ

| # | Việc | Vì sao chưa đóng được |
|---|---|---|
| M-1 | Khả năng phát hiện bản địa của Cursor | không có kênh đo từ phiên này — `NOT_CHECKED`, **không được đọc thành đạt** |
| M-2 | Tám quyết định ở mục 16 | thuộc thẩm quyền Chủ dự án |
| M-3 | Phạm vi 13 hay 7 trên ba trang Notion | ba trang vẫn tự mâu thuẫn; lượt này thi hành theo 13 theo chỉ thị |
| M-4 | Ba nguồn Notion chưa đọc | ngoài phạm vi lượt này ⇒ đối chiếu tổng thể là `PARTIAL` |
| M-5 | Nhãn hiệu lực nội dung của cả 13 | vẫn là *chưa soát* — audit nội dung khác với duyệt hiệu lực |

### ➡️ BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC

> **Công bố và bàn giao báo cáo audit sâu đã lọc an toàn này cho TanPhatAI xem xét; chờ Chủ dự án quyết định gói thay đổi chính xác trước khi đụng vào bất kỳ tệp nguồn nào.**

---

## 20. ĐƯỜNG DẪN, PHIÊN BẢN VÀ CHECKSUM

| Hạng mục | Giá trị |
|---|---|
| Báo cáo này | [AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.md](https://github.com/irissnss/Baocaoerptanphat/blob/main/AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.md) |
| Artefact máy đọc | [AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.json](https://github.com/irissnss/Baocaoerptanphat/blob/main/AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.json) |
| Checksum artefact (SHA-256) | `65663abb759aa606c935dbbec97375d1bd7dcda9ed4a3c9016be5ad188d000d6` |
| Tệp checksum | [AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.sha256](https://github.com/irissnss/Baocaoerptanphat/blob/main/AUDIT-DEEP-SKILL-CONSOLIDATION-20260904.sha256) |
| Báo cáo danh mục kỳ trước | [SKILL-CATALOG-ERP-20260904.md](https://github.com/irissnss/Baocaoerptanphat/blob/main/SKILL-CATALOG-ERP-20260904.md) |
| Artefact danh mục kỳ trước | [SKILL-CATALOG-ERP-20260904.json](https://github.com/irissnss/Baocaoerptanphat/blob/main/SKILL-CATALOG-ERP-20260904.json) |
| Checksum artefact kỳ trước | `4867cbff3608490a324bb6d8eb862b21db3753204c64aa4b67f5618a6ed19a39` — **đã xác minh lại trên đúng byte của Git, KHỚP** |
| Điểm chốt kho mã nguồn riêng | **KHÔNG công bố** theo luật an toàn công khai — giao riêng cho Chủ dự án và TanPhatAI |

> ⚠️ **Cách kiểm checksum cho đúng:** máy này bật chế độ đổi ký tự xuống dòng khi lấy tệp ra. Bản trong Git là dạng chuẩn nên băm khớp; nhưng nếu tải về Windows bằng lệnh sao chép kho thì tệp bị đổi ký tự xuống dòng và **băm sẽ lệch**. Hãy kiểm bằng bản thô trên GitHub hoặc bằng lệnh đọc thẳng nội dung từ Git.

---

## 21. BÀN GIAO CHO TANPHATAI

**Bốn điều phải TỰ KIỂM, không chép mù:**
1. **Nhãn hiệu lực nội dung** của cả 13 vẫn là *chưa soát*. Audit nội dung **không** đồng nghĩa duyệt hiệu lực.
2. **Trạng thái thử** vẫn là *chưa kiểm* cho toàn bộ. Không đọc thành đạt hay hỏng.
3. **Ánh xạ sang bộ kỹ năng bên ngoài** là *chưa kiểm* — không cài ở máy này.
4. **Phán quyết đối chiếu Notion tổng thể là PARTIAL** — mới đọc 4/7 nguồn bắt buộc.

**Ba việc TanPhatAI nên làm (Agent IDE không có quyền):**
- Cập nhật phần *việc-kế-tiếp* của ba trang Notion cho khớp phạm vi **13** (hiện còn ghi 7).
- Ghi nhận hai phát hiện mới mà Notion chưa có: **nhãn bị-thay-thế sai hiện vật** ở cụm versioning, và **mâu thuẫn đổ bóng trong chuẩn giao diện**.
- Ghi nhận con số **36 kỹ năng chọi chuẩn** — Notion hiện có định nghĩa trục nhưng không có con số.

**Xác nhận cuối:** lượt này **KHÔNG** sửa bất kỳ kỹ năng · sổ đăng ký · tệp ghi đè · đường dẫn · luật · móc · quyền · cổng · mã nguồn · cơ sở dữ liệu nào; **KHÔNG** đẩy gì vào kho mã nguồn riêng; **KHÔNG** gọi hay kích hoạt kỹ năng nào; **KHÔNG** cài hay cập nhật gì; **KHÔNG** sửa Notion.

---

_Lượt chỉ-đọc. Duyệt audit không phải duyệt sửa. Giữ và nâng cấp trước khi tính chuyện thay thế; thay thế không xoá trước khi tính chuyện xoá._
