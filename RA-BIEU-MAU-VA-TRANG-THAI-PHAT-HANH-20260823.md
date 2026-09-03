# RÀ BIỂU MẪU / MẪU IN + TRẠNG THÁI PHÁT HÀNH V1.00.353

**Ngày:** 23/08/2026 · **Loại:** **RÀ SOÁT CHỈ-ĐỌC** (không sửa một dòng mã nào) + **báo trạng thái phát hành**
**Hệ thống vận hành đang chạy:** `V1.00.352` · mã commit `<mã-nguồn-riêng>` · nhánh `main`

> Bản tin public-safe: chỉ nêu số lượng, mã kỹ thuật, tên bảng/màn hình. Không có thông tin đăng nhập,
> không có dữ liệu khách hàng, không có số tiền thật.

---

# PHẦN 1 — TRẠNG THÁI PHÁT HÀNH V1.00.353 (nói thẳng)

## 🔴 CHƯA PHÁT HÀNH. Không ghi tắt, không ghi "gần xong".

| Việc | Trạng thái thật | Bằng chứng |
|---|---|---|
| Mã nguồn V1.00.353 | ✅ **XONG**, đã đẩy lên kho mã | commit **`<mã-nguồn-riêng>`** |
| Kế hoạch quay lui | ✅ **Viết TRƯỚC khi deploy** | commit `<mã-nguồn-riêng>` |
| Sao lưu máy vận hành | ✅ **Lấy MỚI lúc 13:35 ngày 23/08** — 18 MB, 101 bảng | tệp nén 509.607 byte, để **ngoài kho mã** |
| N1 — diễn tập trên bản sao mới | ✅ **ĐẠT** | 101 bảng khôi phục = 101 bảng nguồn · **12/12 bảng trọng yếu KHỚP tuyệt đối số dòng** · 4 bộ kiểm thử chạy trên dữ liệu hình dạng thật: **118 điểm kiểm, 0 trượt** |
| N2 — nghiệm thu 12 điểm | ✅ **12/12 đạt CẢ HAI LỚP** (truy vấn thật + ảnh thật) | xem bảng dưới |
| **N3 — đẩy lên máy vận hành** | ❌ **CHƯA CHẠY** | **lệnh deploy bị hệ thống an toàn chặn quyền.** Tác nhân **KHÔNG lách** bằng cách gõ tay từng lệnh SSH — đó là cố tình đi vòng qua chính cái chặn đó |
| Kiểm khói 7 đường trên máy vận hành | ❌ chưa — vì chưa deploy | — |

**Đã kiểm lại máy vận hành lúc viết báo cáo này:** `git rev-parse --short HEAD` → **`<mã-nguồn-riêng>`**;
`const PATCH` trong tệp phiên bản → **`352`**; trang đăng nhập trả mã **200**.
⇒ Máy vận hành **vẫn đang chạy bản cũ**, đúng như nêu trên.

## 12 điểm nghiệm thu — kết quả

> ⚠️ **Nguồn của 12 điểm:** lệnh Chủ dự án nêu *"12 điểm nghiệm thu 100% vận hành"* nhưng **không liệt kê
> từng điểm**. 12 điểm dưới đây **do phía kỹ thuật soạn**, bám đúng phạm vi 4 đợt A·B·C·D và chuỗi bán hàng.
> **Không nhận là lời Chủ dự án.** Chủ dự án xem lại có thiếu điểm nào cần thêm không.

| # | Điểm nghiệm thu | Truy vấn thật | Ảnh |
|---|---|---|---|
| 01 | Đăng nhập + nạp phân quyền | 6 vai trò · 48 quyền menu · 28 quyền hành động | ✅ |
| 02 | Danh mục khách hàng hiện đủ | hàng nghìn khách hàng | ✅ |
| 03 | Danh mục nhân sự hiện đủ | 46 hồ sơ | ✅ |
| 04 | Báo giá — danh sách mở được | 7 báo giá | ✅ |
| 05 | **Không có báo giá ĐÃ DUYỆT mà đơn giá ≤ 0** (đợt B3) | **0 bản ghi vi phạm** | ✅ |
| 06 | Đơn hàng — danh sách mở được | 6 đơn | ✅ |
| 07 | Đơn nháp và đơn đã khoá tách bạch (đợt C3) | 2 nháp · 4 đã xác nhận | ✅ |
| 08 | **Địa chỉ giao đều thuộc đúng khách** (đợt C2) | **0 đơn có địa chỉ lạc** · hàng nghìn địa chỉ trong danh bạ | ✅ |
| 09 | Thiết kế — màn giao việc mở được (đợt B5) | quyền đọc từ ma trận, không đọc mã vai trò viết cứng | ✅ |
| 10 | Tài chính — công nợ mở được (đợt C4) | công nợ **loại trừ 2 đơn nháp** | ✅ |
| 11 | Ma trận tick quyền hành động hiện đủ (đợt D) | 28 dòng quyền đã cấp | ✅ |
| 12 | **Quyền nối bằng KHOÁ, đổi tên không ảnh hưởng** (đợt D) | **1 ràng buộc khoá ngoại** tới bảng vai trò | ✅ |

**Nguồn ảnh:** **100% do phía kỹ thuật tự chụp bằng máy**, trên **bản sao dữ liệu máy vận hành** chạy ở
máy nội bộ. **Không có ảnh nào do Chủ dự án cung cấp** trong đợt này.
⚠️ Ảnh **chứa dữ liệu cá nhân thật** (hàng nghìn khách hàng) ⇒ lưu ở thư mục đã khoá khỏi kho, **KHÔNG đẩy công khai**.

## Trạng thái dọn dữ liệu thử

| Nơi | Tài khoản thử | Vai trò thử | Gán vai trò thử | Quyền thử | Hồ sơ NV thử |
|---|---|---|---|---|---|
| **Máy vận hành** | **0** | **0** | **0** | **0** | **0** |
| **Máy nội bộ** | **0** | **0** | **0** | **0** | **0** |

✅ **Đã dọn đúng quy trình.** Mọi bộ kiểm thử **tự tạo dữ liệu thử rồi tự xoá**, và **đối chiếu số bản ghi
trở về đúng mốc nền** trước khi kết thúc — không bộ nào mượn dữ liệu có sẵn.
Tài khoản tạm dùng để chụp ảnh: **sinh ngẫu nhiên trong bộ nhớ, không in ra, không ghi tệp, xoá ngay sau khi dùng**.

⚠️ **CÒN MỘT THỨ CHƯA XOÁ, khai rõ:** cơ sở dữ liệu diễn tập trên **máy nội bộ** vẫn còn — nó là **bản sao
đầy đủ dữ liệu máy vận hành**, giữ lại để dùng cho lượt phát hành sắp tới. **Sẽ xoá ngay sau khi phát hành
xong**, hoặc ngay khi Chủ dự án nói dừng. Nó nằm trên máy Chủ dự án, không đẩy đi đâu.

## Một lỗi chỉ lộ ra khi XEM ẢNH

Khi mở ảnh nghiệm thu ra xem, phát hiện ngày hiển thị dạng `2026-02-06` trong khi chuẩn giao diện của dự
án bắt **`DD/MM/YYYY`**. Cột "Giao Dự Kiến" chính là cột **mới thêm ở đợt C** ⇒ đúng quy tắc *"đụng đâu sửa đó"*.
Đã sửa 5 chỗ, chụp lại: **06/02/2026**.

> 📌 **Không bộ kiểm thử nào bắt được lỗi này, đọc mã cũng không thấy.** Đây là lần thứ hai trong ngày việc
> **xem ảnh** bắt được lỗi mà rà mã bỏ qua (lần trước: cột tick bị đẩy khỏi màn hình điện thoại).

---

# PHẦN 2 — RÀ BIỂU MẪU / MẪU IN (chỉ đọc, không sửa gì)

## (a) Hệ thống CÓ biểu mẫu / mẫu in không?

**CÓ — và nhiều hơn dự đoán ban đầu.** Có **hai lớp**, không phải một:

| Lớp | Nội dung | Số lượng đo được |
|---|---|---|
| **Lớp 1 — DANH MỤC trong cơ sở dữ liệu** | bảng `dm_form_mau` + màn quản lý `/m0/form-mau` + màn phát hành `/m0/form-phat-hanh` | **34 dòng** |
| **Lớp 2 — BỘ DỰNG bản in trong mã nguồn** | 11 tệp `*-print-content` · `*-print-template` · `*-print-form-sections` | **2.176 dòng mã** |

Ngoài ra có 6 bảng liên quan khác: `form_phat_hanh` (0 dòng) · `form_file` (0 dòng) · `mau_hop_dong` (0 dòng) ·
`checklist_template` (3 dòng) · `dm_blueprint` (1 dòng) · `dm_auto_pricing_formula` (1 dòng).

## (b) Chúng nằm ở đâu?

**KHÔNG phải "viết cứng hết", cũng KHÔNG phải "quản lý trong bảng hết". Là kiến trúc TÁCH ĐÔI CÓ CHỦ ĐÍCH.**

Điều này được ghi rõ ngay trong mã, tại `src/lib/form-output-registry.ts` — nguyên văn:

> *"`dm_form_mau` là DANH MỤC + siêu dữ liệu + nơi ĐĂNG KÝ bộ dựng. Nó KHÔNG phải nơi quyết định đầu ra
> nghiệp vụ bằng HTML. Bằng chứng: 4 mẫu có bộ dựng thật chỉ lưu chuỗi giữ chỗ 28–35 ký tự — đầu ra thật
> đến từ MÃ NGUỒN."*

Số đo xác nhận đúng như tài liệu nói: trong 34 dòng của bảng, phần lớn ô nội dung mẫu chỉ dài **34–39 ký tự**
(chuỗi giữ chỗ), chỉ 4 dòng có nội dung đáng kể (dài nhất 1.606 ký tự).

> ⚠️ **Đính chính của chính báo cáo này:** thoạt nhìn số liệu đó rất giống *"bảng trang trí, sửa vào không ăn thua"*.
> Sau khi mở tệp đăng ký ra đọc thì **không phải** — đây là thiết kế có chủ đích, có ghi lý do. Suýt báo nhầm
> một kiến trúc đúng thành lỗi.

## (c) Danh sách từng mẫu + chỗ dùng + ai xem được

**5 mẫu CÓ bộ dựng thật** (đăng ký ở `src/lib/form-output-registry.ts`):

| Mã mẫu | Tên | Bộ dựng nằm ở | Quyền BẮT BUỘC |
|---|---|---|---|
| `FORM_BAO_GIA` | Báo giá | `src/app/m3/bao-gia/bao-gia-print-content.ts` (301 dòng) | **M3** |
| `FORM_LENH_SAN_XUAT` | Lệnh sản xuất | `src/app/m4/lenh-san-xuat/…/lsx-print-*` (730 dòng) | **M4** |
| `FORM_PHIEU_DIEU_IN` | Phiếu điều in | `src/app/m4/phieu-dieu-in/pdi-print-*` (547 dòng) | **M4** |
| `FORM_NGHIEM_THU` | Biên bản nghiệm thu | `src/app/mf/nghiem-thu/nghiem-thu-print-*` (295 dòng) | **MF** |
| `FORM_XAC_NHAN_CONG_NO` | Biên bản đối chiếu & xác nhận công nợ | `src/app/mf/doi-chieu/doi-chieu-print-*` (303 dòng) | **MF** |

**Ai xem được:**
- Xem/sửa **danh mục mẫu** (`/m0/form-mau`): chặn bởi quyền **M0** — `view` để xem, `create`/`update`/`delete` để sửa.
- Xem **bản in nghiệp vụ**: chặn bởi quyền **module nghiệp vụ tương ứng** (M3 / M4 / MF), kiểm **ở phía máy chủ**,
  trình duyệt **không được chọn** bộ dựng.

**Ba điểm kỷ luật đáng ghi nhận trong thiết kế hiện tại:**
1. **Chặn-mặc-định**: mẫu không có trong sổ đăng ký ⇒ coi như **không hỗ trợ**, không in.
2. **Không bịa cho đầy bảng**: mã ghi rõ *"TUYỆT ĐỐI KHÔNG bịa bộ dựng để làm đầy bảng phủ"*.
3. **Chống dữ liệu giả**: mẫu nào mà bảng nguồn chưa có bản ghi thật thì **bắt buộc đóng dấu "DỮ LIỆU MINH HOẠ"**
   lên bản in, không cho lẫn với dữ liệu thật.

**Một mẫu CỐ Ý chưa làm:** `FORM_DON_HANG` — mã ghi lý do nguyên văn *"Chưa có mẫu Đơn Hàng nghiệp vụ được
Owner phê duyệt"*, kèm ghi chú không được sao chép bố cục Báo giá, **không bịa thuế / điều khoản / chữ ký**.
⇒ Đây là **việc đang chờ Chủ dự án**, không phải thiếu sót kỹ thuật.

**Tệp dữ liệu giả:** có `src/lib/mock-m0-form-in.ts` — đã kiểm: nó **đánh dấu ngưng dùng** và **trả về mảng rỗng**,
kho dữ liệu thật đọc thẳng từ bảng. ⇒ **Không có dữ liệu giả nào chen vào đường chạy thật.**

## (d) ĐỀ XUẤT (chỉ đề xuất — không làm gì trong lượt này)

**Câu hỏi "có nên đưa vào bảng quản lý không" thì câu trả lời là: ĐÃ CÓ RỒI.** Nên đề xuất chuyển sang
**3 việc tinh gọn còn thiếu**, xếp theo mức đáng làm:

| # | Đề xuất | Vì sao | Mức |
|---|---|---|---|
| **1** | **Thêm quyền riêng "sửa biểu mẫu" vào ma trận tick** | Hiện muốn cho ai sửa mẫu in thì phải cấp cả cụm quyền **M0 update** — mà M0 còn chứa phân quyền, phòng ban, quy trình. Nghĩa là **muốn cho sửa mẫu thì lỡ tay mở luôn quyền động vào bảo mật**. Đúng nguyên tắc Chủ dự án khoá (*ai được sửa mẫu = tick quyền riêng*), nên tách thành một ô tick riêng | **CAO** |
| **2** | **Dọn dòng trùng trong danh mục** | Đo được **nhiều dòng cùng một loại chứng từ và cùng ở trạng thái đang dùng** (ví dụ báo giá có 4 dòng, đơn hàng 4 dòng). Người quản trị nhìn vào **không biết dòng nào đang có hiệu lực**. Việc dọn là **dữ liệu**, không phải sửa mã | **CAO** |
| **3** | **Ghi ngay trên màn hình: dòng nào có bộ dựng thật, dòng nào chỉ là giữ chỗ** | Bảng có ô "nội dung mẫu" cho sửa, nhưng với 5 mẫu nghiệp vụ thì **sửa ô đó KHÔNG đổi bản in** (bản in đến từ mã nguồn — đúng thiết kế). Người dùng không đọc mã sẽ **tưởng mình đã sửa mẫu in**. Chỉ cần một nhãn *"Mẫu này dựng từ bộ dựng chính thống — sửa ở đây không đổi bản in"* | **TRUNG BÌNH** |

**KHÔNG đề xuất:** chuyển 2.176 dòng bộ dựng vào cơ sở dữ liệu. Lý do: bản in nghiệp vụ có phép tính,
định dạng tiền/ngày, phân trang, và **phải chặn mã độc chèn qua HTML** — để trong mã nguồn thì có kiểm thử
và có cổng chặn; đưa vào ô nhập văn bản là **mở một lối tấn công mới** đổi lấy sự tiện lợi mà thực tế
ít khi dùng tới.

---

# PHẦN 3 — VIỆC CÒN CHỜ CHỦ DỰ ÁN

| # | Việc | Chặn cái gì |
|---|---|---|
| 1 | **Cho phép chạy lệnh phát hành** (đang bị hệ thống an toàn chặn) | Chặn toàn bộ N3: phát hành · kiểm khói · báo cáo phát hành |
| 2 | **Duyệt danh sách 12 điểm nghiệm thu** (do phía kỹ thuật soạn, không phải lời Chủ dự án) | Chặn việc coi N2 là nghiệm thu chính thức |
| 3 | **Quyết 3 đề xuất về biểu mẫu** ở Phần 2 (d) | Không chặn gì — làm sau go-live được |
| 4 | **Mẫu Đơn Hàng** — cần Chủ dự án phê duyệt mẫu nghiệp vụ | Chặn việc in đơn hàng (hiện cố ý chưa làm) |

---

*Báo cáo public-safe. Không chứa mã nguồn, thông tin đăng nhập, dữ liệu khách hàng hay số liệu tài chính thật.*
*Lượt rà biểu mẫu là **chỉ đọc** — không sửa, không tạo, không xoá bất kỳ tệp hay bản ghi nào.*
