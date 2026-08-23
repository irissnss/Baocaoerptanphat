# CHUẨN HOÁ LUẬT + KỸ NĂNG + GIAO DIỆN — MỘT LƯỢT

> **Ngày:** 23/08/2026 · **Owner duyệt:** 14:11 · **Actor:** Agent IDE (phiên "Luật Cho Dự Án TanPhat ERP")
> **Nhánh:** `main` · **Mốc đầu:** `da9bc22` → **Mốc cuối:** `c0e0b8f`
> **Phạm vi khoá:** LUẬT + KỸ NĂNG + GIAO DIỆN. **0 dòng `src/`. 0 file bị xoá hay đổi tên.**
> **Căn cứ:** Sổ Yêu Cầu Owner **mục #132**

---

## 1) BẢNG TÊN VIỆC ĐỜI THƯỜNG + SỐ TRÒN

*(Quy tắc trình bày mới Owner chốt 14:11 — mã kỹ thuật để trong ngoặc.)*

| # | Việc đã làm — nói bằng tiếng thường | Số tròn | Xong? |
|---|---|---|---|
| 1 | **Sửa 4 cái "máy kiểm tra an toàn" bị mù** — chúng không nhìn thấy file nào có tên tiếng Việt có dấu *(4 cổng thiếu cờ `-z`)* | **4 máy** | ✅ |
| 2 | **Máy vừa sáng mắt liền bắt được file chưa ai từng thấy** — nằm trong kho suốt từ trước | **7 file** | ✅ đã ghi sổ |
| 3 | **Nối nhãn "kỹ năng nào còn dùng được" vào đúng chỗ mà luật bắt tra** — đây là gốc của việc *"làm mãi vẫn vướng"* | **128 kỹ năng** | ✅ |
| 4 | **Làm một máy mới canh không cho hai cuốn sổ lệch nhau nữa** *(cổng `test:ui-skill-conflict`)* | **6 điều kiện** | ✅ |
| 5 | **Nối bộ tiêu chí nghiệm thu giao diện vào luật** — bộ này có từ sáng nhưng luật không hề trỏ tới | **58 tiêu chí** | ✅ |
| 6 | **Gộp 3 mảng kiến thức bị bỏ ngoài chuẩn giao diện** — biểu mẫu nhiều bước · viết hoa đầu chữ · quy đổi lớp giao diện | **3 mảng** | ✅ |
| 7 | **Sửa một câu SAI SỰ THẬT trong chuẩn giao diện** — chuẩn ghi *"4/4 trang mẫu đều đúng"*, đo lại chỉ **3/4** | **1 câu** | ✅ |
| 8 | **Ra luật: thông tin rủi ro (mật khẩu…) chỉ để đúng 2 nơi, phải chứng minh bằng lệnh** | **1 luật mới** | ✅ |
| 9 | **Điền ngưỡng lỗi cho việc nạp dữ liệu** — trước đây ô này để trống | **2% mỗi lô** | ✅ |
| 10 | **Sửa bài kiểm tra tự phá chính nó** *(cổng `test:version-policy`)* | **31 → 37 đạt** | ✅ |
| 11 | **Dọn sổ nợ:** vá dòng bảng bị vỡ + đánh lại mã bị cấp trùng | **5 mã trùng** | ✅ |
| 12 | **Ghi thêm nợ mới phát hiện** | **6 nợ mới** | ✅ |
| 13 | **Ra quy trình chống "đọc lỏi rồi kết luận"** cho tầng máy phụ | **1 quy trình** | ✅ |

**Tổng: 13/13 việc Owner duyệt — làm đủ, không sót.**

---

## 2) BA ĐIỀU ĐÁNG CHÚ Ý NHẤT

### 2.1 🔴 Gốc của *"làm mãi vẫn vướng"* — luật đang dẫn tới câu trả lời sai

Sáng nay 11:13 Owner duyệt hạ nhãn 4 kỹ năng xuống **"hết hiệu lực"**. Nhưng nhãn đó ghi ở **cuốn sổ A**, trong khi luật lại bắt tra **cuốn sổ B**. Cuốn B vẫn ghi *"còn tốt"*.

Nghĩa là một phiên làm việc **tuân thủ luật đúng từng bước** vẫn bị dẫn tới kỹ năng dạy sai:

| Bước | Việc |
|---|---|
| 1 | Cần dựng một bảng danh sách |
| 2 | Luật bắt tra sổ kỹ năng → tra |
| 3 | Thấy `master-list-data-table`, nhãn **"còn tốt"** → nạp |
| 4 | Được dạy 3 giá trị mà chuẩn giao diện **CẤM** |
| 5 | Owner bác |

**Càng tuân thủ luật càng code sai.** Nay hai cuốn sổ đã nối, và có máy canh không cho lệch lại.

### 2.2 🔴 Máy kiểm tra an toàn đã "báo đạt" sai ở mọi lần lưu suốt từ khi có nó

Bốn máy kiểm tra dùng một lệnh liệt kê file **thiếu một chữ**. Hậu quả: **mọi file có tên tiếng Việt có dấu đều vô hình**. Trong một dự án Việt Nam, đó là một mảng rất lớn.

**Bằng chứng đối chứng hai chiều** — thả 3 file thử có tên dấu tiếng Việt:

| | Máy bản CŨ | Máy bản MỚI |
|---|---|---|
| Thấy mấy file thử? | **0 / 3** → báo "ĐẠT" | **3 / 3** → báo "KHÔNG ĐẠT" |
| Gỡ file thử ra | — | báo "ĐẠT" trở lại |

Máy sáng mắt liền bắt được **7 file đã nằm sẵn trong kho**:

| Loại | Số | Đánh giá |
|---|---|---|
| Danh sách khách hàng *(đã biết từ hôm qua)* | 1 file | Owner đã chốt hướng xử lý |
| **Bảng tính trong thư mục biểu mẫu** | **5 file** | 🔴 **MỚI** — 2 file tên *"<ten-file-da-che>"*, nghi là **dữ liệu vận hành thật** |
| **Bản chép từ Notion chứa 34 địa chỉ email** | 1 file | 🔴 **MỚI** — tên thư mục có biểu tượng nên cũng vô hình |

> ⚠️ **Chưa mở được nội dung 5 bảng tính** — cần Owner xem giúp là dữ liệu thật hay chỉ là mẫu trống. Đã ghi sổ nợ, đã chặn không cho lọt thêm file mới.

### 2.3 🔴 Chuẩn giao diện có một câu sai sự thật — và trang mẫu đang lệch chính điều đó

Chuẩn ghi: *"quy ước này đã chạy thật ở **4/4 trang mẫu**"*.

Đếm lại bằng cách đếm **nút bấm trên từng dòng** (không đếm tên cột):

| Trang mẫu | Số nút Sửa/Xoá trên dòng |
|---|---|
| Sản phẩm · Khách hàng · Nhân sự | **0 · 0 · 0** |
| **Kho thành phẩm** | 🔴 **2** |

**Đúng là 3/4, không phải 4/4.**

**Vì sao lọt:** cột đó **không có tên**. Mọi cách dò theo chữ *"Thao tác"* đều không thấy — kể cả cách dò đã dùng để viết chính câu đó.

**Quyết định của Owner KHÔNG bị bác** — chỉ câu dẫn chứng là sai. Đã sửa, giữ nguyên văn câu cũ ở phần lịch sử, và ghi nợ cho **8 trang**. Đáng lo nhất: *kho thành phẩm* **là trang mẫu**, ai chép mẫu từ nó sẽ chép luôn cái sai.

---

## 3) EM PHẢI TỰ NHẬN — MÁY PHỤ CỦA EM KẾT LUẬN SAI 3/4 LẦN

Lượt đối chiếu hôm qua dùng **15 máy phụ** chạy song song. Bốn máy kết luận *"chuẩn giao diện ghi sai"*. Em mở mã kiểm lại từng cái → **3/4 là SAI**.

Nặng nhất: một máy dò thấy chuỗi `h-9` trong một file kỹ năng rồi kết luận ngay, **không kiểm chuỗi đó thuộc phần tử nào**. Hoá ra nó là **ô tìm kiếm bên trong hộp thả xuống**, không phải cái nút. Không hề có mâu thuẫn.

**Đây đúng là lỗi "đọc lỏi" đã gây ra ca hỏng 17–18/08** — chỉ khác là nay nó xảy ra ở **tầng máy phụ**, nơi luật cũ không với tới.

> **Bài học ghi thành quy trình:** thêm một tầng máy **KHÔNG** tự làm kết luận chắc hơn. Nó nhân số phán quyết lên, còn đúng/sai thì tuỳ cách mỗi máy đọc. Nhiều máy + đọc lỏi = **nhiều kết luận sai hơn**.

Đã ra quy trình bắt buộc: mọi phán quyết *"tài liệu chuẩn sai"* phải trích **ngữ cảnh ±10 dòng cả hai phía**, phải **nêu rõ phần tử**, và **phiên chính bắt buộc kiểm lại** trước khi sửa chuẩn.

---

## 4) 🔴 MỘT ĐIỂM CẦN OWNER BIẾT — NGƯỠNG VỪA BỊ VƯỢT

Sau khi gộp 3 mảng kiến thức, chuẩn giao diện tăng **453 → 521 dòng**.

Đo thật: đọc trọn file tốn **32.562 token** — **vượt hạn mức đọc một lần (25.000)**. Nghĩa là **không còn đọc trọn trong một lượt**, phải chia 2 lượt.

**Em KHÔNG nới luật** (đúng lệnh cấm của Owner 14:11) — **và không cần nới**: chính điều luật đó đã có sẵn khoản dành cho file **trên 500 dòng**, nay tự có hiệu lực.

Nhưng Owner nên biết: **ngưỡng này vừa bị vượt do chính đợt gộp hôm nay.** Càng gộp thêm thì càng đắt. Đây là lý do 5 kỹ năng còn lại được **TREO** chứ không gộp hết.

---

## 5) CÁCH KIỂM CHỨNG — CHẠY MỘT LỆNH

```
npm run test:gov-gates
```

Kết quả tại mốc `c0e0b8f`: **toàn bộ XANH**.

| Máy kiểm tra | Kết quả |
|---|---|
| Đếm điều khoản luật | ĐẠT — 392 ≥ 386 · 5 bản luật **giống hệt nhau** |
| Đếm mục chuẩn giao diện | ĐẠT — 0 mục biến mất |
| Kiểm đường dẫn bắt buộc tồn tại | ĐẠT — 57 đường dẫn, 0 hỏng |
| Kiểm danh mục kỹ năng | ĐẠT |
| **Kiểm hai sổ không lệch nhau** *(mới)* | ĐẠT — 6/6 điều kiện |
| Quét thông tin nhạy cảm | ĐẠT — 0 vi phạm mới |
| Quét dữ liệu cá nhân | ĐẠT — 0 vi phạm mới, 7 nợ đã biết |
| Kiểm chính sách số phiên bản | ĐẠT — **37/37** *(trước: 31/37)* |

**Mọi máy mới đều đã được thử ngược** — cố tình gài lỗi để xem nó có bắt được không:

| Thử gài | Máy phản ứng |
|---|---|
| Thả 3 file có tên tiếng Việt chứa dữ liệu nhạy cảm giả | 🔴 bắt **3/3** |
| Sửa lệch nhãn ở một cuốn sổ | 🔴 bắt đúng chỗ |
| Xoá khai báo cảnh báo của một kỹ năng | 🔴 bắt đúng chỗ |
| Thêm một máy mới viết sai kiểu cũ | 🔴 bắt đúng chỗ |
| Gỡ một máy khỏi danh sách để lách | 🔴 bắt đúng chỗ |
| Xoá trọn một mục của chuẩn giao diện | 🔴 bắt đúng chỗ |
| **Khôi phục lại** | ✅ xanh trở lại |

---

## 6) VIỆC CÒN LẠI / BÀN GIAO PHIÊN KHÁC

| Việc | Ai lo | Ghi chú |
|---|---|---|
| **5 bảng tính + 1 bản chép Notion vừa lộ ra** | **CHỜ OWNER** | Cần Owner mở xem là dữ liệu thật hay mẫu trống |
| **Gỡ file danh sách khách hàng khỏi kho** | Phiên bảo mật/kho | Owner đã chốt hướng nghiệp vụ |
| **Nơi lưu biểu mẫu luật** | **PHIÊN KHÁC** | Nghi đã có sẵn — cần kiểm đã hoàn chỉnh chưa |
| **Màu của một mô-đun** | **TREO** | Owner cho treo tới khi sửa mã tới đó |
| **Báo cáo lên sóng chuỗi bán hàng** | Phiên lên sóng | Đã xong ở kho mã, chưa phát hành |
| **6 nợ giao diện cũ** | Phiên sửa mã | Dọn khi đụng mô-đun |
| **5 kỹ năng còn treo + nối 2 thư viện kỹ năng** | Phiên vòng đời kỹ năng | Thư viện Notion 31 · thư viện máy 128, chưa có bảng nối |
| **8 trang có cột thao tác trên dòng** | Phiên sửa mã | Ưu tiên trang mẫu |

---

```
════════════ BÁO CÁO KẾT THÚC ════════════
1. ĐÃ LÀM
   - V1 va 4 cong mu duong dan tieng Viet (git ls-files thieu -z), ham dung chung
   - V2 noi nhan ky nang vao dung noi luat bat tra + cong may 6 dieu kien
   - V3 noi bo 58 tieu chi nghiem thu giao dien vao 5 file luat
   - V4 gop 3 khoang trong lon nhat vao chuan giao dien (§11.1 · §13 · §22)
   - V5 sua cau can cu SAI SU THAT cua §8.1 (4/4 -> 3/4)
   - V6 luat moi GOV-SECRET-LOCATION-001 + dong DEBT-051
   - V7 dien nguong nap 2%/lo + hoi to dot 22/08
   - V8 test:version-policy 31/37 -> 37/37
   - V9 va dong bang vo + danh lai 5 ma no trung (hau to -B)
   - V10 trang thai no 4 -> 6 gia tri
   - V11 tieu chi N0.1b + quy trinh phan-quyet-tai-lieu-chuan.md
   - V12 do THAT token doc chuan giao dien
   - V13 So Yeu Cau Owner muc #132

2. PHẠM VI
   ĐỤNG      : 5 file luat (chi THEM dong) · docs/UI-STANDARD.md ·
               docs/UI-ACCEPTANCE-CHECKLIST.md · .governance/registry/** ·
               .governance/procedures/** · scripts/tests/** · scripts/pre-commit-hook.sh ·
               package.json · docs/OWNER-REQUEST-LEDGER.md
   KHÔNG ĐỤNG: src/ (0 dong) · .cursor/skills/ (0 file) · migrations/ · DB ·
               deploy · so phien ban · file du lieu khach hang
   KHÔNG XOÁ / KHÔNG ĐỔI TÊN file nao

3. BẰNG CHỨNG
   npm run test:gov-gates -> XANH toan bo -> RUNTIME_PROVEN
   kiem nguoc file thu ten co dau: ban cu 0/3, ban moi 3/3, go ra xanh lai -> RUNTIME_PROVEN
   npm run test:version-policy -> 37/37 (truoc 31/37) -> RUNTIME_PROVEN
   npm run test:ui-skill-conflict -> 6/6 DK PASS, kiem nguoc 3/3 -> RUNTIME_PROVEN
   sha256 5 file luat -> 1 ma duy nhat -> FILE_PROVEN
   dem nut sua/xoa tren dong 4 trang mau -> 0 0 0 2 -> CODE_PROVEN
   wc -l = grep -c '' = 453 (truoc V4) -> 521 (sau V4) -> FILE_PROVEN
   doc tron chuan giao dien -> 32.562 token, vuot han muc 25.000 -> RUNTIME_PROVEN
   pre-commit chay that o 3 commit -> 3 cong PASS -> RUNTIME_PROVEN

4. GHI SỔ YÊU CẦU OWNER
   [x] ĐÃ GHI — mục #132 (va muc #131 phien truoc)

5. PUSH BÁO CÁO CÔNG KHAI
   [x] ĐÃ PUSH — kho Baocaoerptanphat · commit c27dd63 · file LUAT-SKILL-UI-GOP-20260823.md
   [x] Kho ma rieng tu: 1f5e379 · 30e077c · c0e0b8f — da push main

6. CÒN SÓT / CHƯA LÀM
   - 5 bang tinh + 1 ban chep Notion vua lo ra: CHUA xac minh noi dung (DEBT-091/092)
   - 5 ky nang BO SUNG con treo (DEBT-095) — co y treo vi chuan da cham nguong doc
   - Anh xa 2 thu vien ky nang (DEBT-096) — can Agent Notion phoi hop
   - 8 trang co cot thao tac tren dong (DEBT-094) — viec CODE, phien khac
   - Mau mo-dun M4 (OPEN-N2) — Owner cho treo
   - File du lieu khach hang (DEBT-066-B) — phien bao mat/kho

7. ĐANG CHỜ OWNER
   - Xac nhan 5 bang tinh la du lieu that hay mau trong -> chan viec dong DEBT-091
   - Biet ve nguong doc chuan giao dien vua bi vuot (521 dong / 32.562 token)
   - Neu Owner muon gop them ky nang: can quyet truoc vi moi lan gop lam
     chuan dat them cho MOI phien doc ve sau

8. BƯỚC KẾ TIẾP — ĐÚNG MỘT VIỆC
   Owner mo 2 file "NHAP LIEU HANG NGAY" xem co du lieu that khong.

9. CHƯA XÁC MINH ĐƯỢC
   - Noi dung 5 file .xlsx — agent khong mo duoc dinh dang nay trong phien.
     Ai xac minh: Owner
   - Co che moi giam duoc bao nhieu luot Owner bac — khong do duoc trong phien;
     chi do duoc so ky nang mang gia tri cam nay da co nhan (36/36)
   - Noi dung khung chat cac phien Claude Code khac — chi doc duoc san pham ghi ra dia

10. TRẠNG THÁI CHUNG
   [x] PASS — 13/13 muc Owner duyet da lam, moi cong XANH, moi cong moi da
       kiem nguoc. Cac muc o truong 6 deu la viec DA GHI SO, giao phien khac
       hoac cho Owner, khong phai viec bo do trong pham vi phien nay.

11. NÉN PHIÊN & ĐỌC LẠI THAM CHIẾU
   Phien co bi nen: CO
   Da doc lai sau nen (BUOC 0 do moc truoc khi doc):
   docs/UI-STANDARD.md (453 dong, sha 42a07779) · docs/UI-ACCEPTANCE-CHECKLIST.md
   · .governance/registry/{skills.yml,ui-standard-sources.md,tech-debt.md}
   · docs/OWNER-REQUEST-LEDGER.md · CLAUDE.md §L2 §G7.1 §G7.10 §V §W
   · scripts/pre-commit-hook.sh · scripts/tests/{pii-scan,secret-scan,path-audit,
     script-parse,standard-clause-count,skills-registry-build,version-policy}
   · .cursor/skills/{implement-wizard-step,title-auto-case,tailwind-v4-canonical-classes,
     searchable-dropdown,detail-panel-layout}/SKILL.md
   · src/app/m5/kho-thanh-pham/kho-thanh-pham-client.tsx (chi DOC)
   Moc do BUOC 0: nhanh main · HEAD da9bc22 · cay lam viec sach 0 file
═══════════════════════════════════════════
```
