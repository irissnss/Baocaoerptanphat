# PLAN — HẠNG MỤC TÍNH GIÁ · `PL-ERP-TINH-GIA-20260825`

> **Trạng thái:** `ACTIVE` — lập 25/08/2026 theo chỉ thị Chủ dự án (sổ `#177`).
> **Nguyên văn chỉ thị:** *«cái này em cần tạo một plan lớn để xử lý riêng mảng này nó phức tạp hơn em tưởng đó em ko đi nhanh trong phiên này được em ghi nhận lại tất cả rồi ghi số là vừa đó em»*
>
> ⚖️ **RANH GIỚI VỚI PLAN ĐANG CHẠY** — khai rõ để không vi phạm `GOV-ONE-PLAN-OF-RECORD-001` §E3:
> Đây là **workstream RIÊNG**, tách khỏi `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`.
> §E3 cấm **hai Plan cùng điều khiển MỘT workstream** — không cấm mỗi workstream một Plan.
> Các việc còn lại của Plan gốc (`OIL #141` mục 1·2·4·5·6 · Pha 6 bàn giao Notion · các nợ cổng)
> **KHÔNG bị chặn** bởi hạng mục này, và ngược lại.

---

## 1. VÌ SAO PHẢI TÁCH RIÊNG — bằng chứng đo được

Chủ dự án nói *«nó phức tạp hơn em tưởng»*. Đây là số đo chứng minh điều đó:

| # | Sự thật đo được | Nguồn |
|---|---|---|
| 1 | **Một công thức khổ trải** đang áp cho **40 nhóm sản phẩm**, trong đó ~20 là kiểu dáng hộp khác nhau | `DEBT-113` |
| 2 | Công thức thật khác nhau **VỀ CẤU TRÚC** giữa các kiểu — *Túi Xách* dùng `(D+R)×2`, *Nắp Cài* dùng `D + C×4` | `DEBT-115` |
| 3 | Phụ cấp **phụ thuộc kích thước** (`Rộng÷2`, `Cao÷2`) là chuyện **thường xuyên**, không hiếm | `DEBT-115` |
| 4 | **Hộp cứng** cần **4 mảnh × 3 lớp**, rồi mới ghép — mô hình *một tấm trải* không biểu diễn nổi | `DEBT-115` |
| 5 | **Khổ trải TỐI ƯU** là kết quả **riêng**, không phải biến thể | `DEBT-115` |
| 6 | **6 chỗ** tính khổ trải viết cứng trong mã, không chỗ nào dẫn nguồn | `DEBT-043` |
| 7 | Màn cấu hình **đã tồn tại đầy đủ** nhưng **không nối dây** — hàm đọc công thức có **0 nơi gọi** | `DEBT-112` |
| 8 | **4 khái niệm** Chủ dự án nêu (*tối ưu khổ · chi phí máy in · tờ rớt · lượt in*) **không có cột nào** trong 101 bảng | `DEBT-114` |
| 9 | Định nghĩa **lượt in** của Chủ dự án **khác** cách mã đang tính | sổ `#173` |
| 10 | `phieu_dieu_in` có `may_in enum('lon','nho')` — **không có bảng giá theo máy**, bảng chỉ **2 dòng**, **không nối** tính giá | `DEBT-114` |

---

## 2. KIẾN THỨC NGHIỆP VỤ CHỦ DỰ ÁN ĐÃ CHO — giữ nguyên văn

### 2.1 Quy ước trục *(sổ `#161`)*
> Đáy hộp = **Dài × Rộng**, thân hộp cao bằng **Cao**.

✅ **Đã xác nhận bằng tài liệu:** công thức *Túi Xách Giấy* dùng `(Dài+Rộng)×2` — khớp.

### 2.2 Tờ rớt *(sổ `#172`)*
> *«tờ rớt là số tờ in vượt chuẩn mốc 3.000 lượt / tờ đó em ví dụ 3500 tờ thì 500 tờ được gọi là tờ rớt»*

Hiểu là: mốc chuẩn **3.000 tờ/lượt**; phần vượt gọi là **tờ rớt**.

### 2.3 Lượt in *(sổ `#173`)*
> *«1 lượt = số kẽm tương đương số màu x đơn giá x 1 mặt»*

Hiểu là: **số kẽm = số màu**; tiền một mặt = `số màu × đơn giá`.

### 2.4 Máy in *(sổ `#174`)*
> *«máy in nhỏ lớn khác nhau về đơn giá , giá kẽm tất cả mọi thứ đều khá nhau»*

⇒ Máy in là **một chiều cấu hình đầy đủ**, mỗi máy một bộ giá riêng.

### 2.5 Kiểu dáng ưu tiên *(sổ `#171`)*
> **Túi Xách Giấy** · **Hộp Nắp Cài**

### 2.6 Phụ cấp *(sổ `#170`)*
> Linh hoạt thêm/xoá được. Danh mục **nên đưa vào `dm_nhom_universal`** — Chủ dự án giao Agent tự đánh giá.

---

## 3. CÔNG THỨC THẬT ĐÃ TÌM ĐƯỢC

Nguồn: `Test Tính Khổ Trải Tân Phát 19082025.xlsx` — đọc **trực tiếp từ ô Excel**.
Chi tiết đầy đủ: [`docs/reports/KHO-TRAI-CONG-THUC-THAT-TAN-PHAT-20260825.md`](reports/KHO-TRAI-CONG-THUC-THAT-TAN-PHAT-20260825.md)

**Sáu kiểu dáng có công thức:** Hộp Nắp Cài · Túi Xách Giấy · Nắp Đậy Cài Lưỡi Gà Đáy Cài Chéo · Hộp Cứng Âm Dương Có Thành · Hộp Cứng Nam Châm · Hộp Cứng Âm Dương Không Thành.

**Ba loại kết quả** *(hộp mềm/carton)*: `Khổ Trải 1 Hộp` · `Khổ Trải 1/2 Hộp` · **`Khổ Trải 1 Hộp Tối Ưu`**
**Hộp cứng:** 4 mảnh *(Nắp · Đáy · Thành · Khay)* × 3 lớp *(Bìa · Áo Ngoài · Áo Lót)* → ghép thành `Khổ Trải 1` · `Khổ Trải 2`

---

## 4. 🔴 MƯỜI ĐIỀU CHƯA BIẾT — chặn thiết kế

Không trả lời được thì **không thiết kế đúng được**. Agent **không đoán**.

### Nhóm A — về khổ trải *(5 câu, đã hỏi, chưa có lời đáp)*

| # | Câu hỏi | Vì sao chặn |
|---|---|---|
| A1 | **Đơn vị: cm hay mm?** Mẫu ghi `28 · 17 · 11` cho một hộp | Sai đơn vị ⇒ mọi con số lệch **10 lần** |
| A2 | *«Khổ Trải 1/2 Hộp»* nghĩa là gì | Không biết thì không dựng được kết quả thứ hai |
| A3 | Hộp cứng: *«Khổ Trải 1»* / *«Khổ Trải 2»* là **hai tấm phôi in riêng**, hay hai cách bố trí | Quyết định số lần bình bài, tức số tờ giấy |
| A4 | Các số cộng thêm `+2` `+3` `+5` **tên là gì** | Cần tên để đưa vào danh mục phụ cấp |
| A5 | Vì sao *«tối ưu»* bớt được `Cao×1` | Cần nguyên lý để áp cho kiểu dáng khác |

### Nhóm B — về tờ rớt *(3 câu mới)*

| # | Câu hỏi |
|---|---|
| B1 | Mốc **3.000** cố định cho mọi máy, hay đổi theo **máy lớn/nhỏ**? |
| B2 | Tờ rớt có **đơn giá riêng**, hay chỉ là **cách gọi tên** phần vượt? |
| B3 | In **7.000 tờ** = **2 lượt + 1.000 rớt**, hay **1 lượt + 4.000 rớt**? |

### Nhóm C — về lượt in *(2 câu mới)*

| # | Câu hỏi |
|---|---|
| C1 | *«đơn giá»* là **giá một lượt in** hay **giá một kẽm**? |
| C2 | **Tiền kẽm** và **tiền công in** là **một khoản** hay **hai khoản tách rời**? |

---

## 5. KIẾN TRÚC ĐỀ XUẤT — **CHƯA DUYỆT**, chờ §4

```
NHÓM SẢN PHẨM (cấp L0)          ví dụ: Hộp Mềm · Thùng Carton · Lịch
   └── KIỂU DÁNG (cấp L1)        ví dụ: Hộp Carton Nắp Cài · Túi Xách Giấy
         │
         ├── BỘ CÔNG THỨC KHỔ TRẢI
         │     ├── Thường     → { biểu thức Dài , biểu thức Rộng }
         │     ├── Nửa hộp    → { … }
         │     └── TỐI ƯU     → { … }
         │
         ├── (hộp cứng) MẢNH × LỚP
         │     mảnh: Nắp · Đáy · Thành · Khay
         │     lớp : Bìa · Áo Ngoài · Áo Lót
         │     └── mỗi ô: { biểu thức Dài , biểu thức Rộng }
         │     └── quy tắc GHÉP mảnh → tấm phôi
         │
         └── DANH MỤC PHỤ CẤP        (tên khoản lấy từ dm_nhom_universal)
               mỗi khoản: { tên , biểu thức , áp vào chiều nào }

MÁY IN  (bảng danh mục MỚI — hiện chưa có)
   └── mỗi máy: khổ giấy tối đa · đơn giá lượt · giá kẽm · mốc tờ rớt · đơn giá tờ rớt
```

> ✅ **Phần đã có chỗ đứng:** `dm_blueprint.blueprint_json` với khoá `formula_flat_size` + `constants`; hàm `calculateFlatSize()` biết đọc biểu thức chuỗi; màn `/m3/tinh-gia-admin` đã có ô nhập công thức. **Chỉ chưa nối và chưa nạp.**
> 🔴 **Phần CHƯA có chỗ đứng:** nhiều mảnh × nhiều lớp · khổ trải tối ưu như kết quả riêng · **bảng danh mục máy in** · tờ rớt · lượt in theo định nghĩa Chủ dự án.

---

## 6. SÁU CHẶNG — thứ tự bắt buộc

| Chặng | Nội dung | Điều kiện vào | Kết quả |
|---|---|---|---|
| **T0** | **Hỏi & chốt nghiệp vụ** — 10 câu ở §4 | *(đang ở đây)* | Bảng trả lời có chữ ký Chủ dự án |
| **T1** | **Mô hình dữ liệu** — kiểu dáng · công thức · mảnh/lớp · máy in · tờ rớt | T0 xong | Bản thiết kế + di trú CSDL, **chưa chạy** |
| **T2** | **Lõi tính toán** — bộ phân giải biểu thức, nhiều mảnh/lớp, khổ tối ưu | T1 duyệt | Bộ kiểm đối chứng **đúng số trong tệp Excel** |
| **T3** | **Nạp dữ liệu 2 kiểu ưu tiên** — Túi Xách Giấy · Hộp Nắp Cài | T2 xanh | Hai kiểu tính ra đúng số mẫu trong tài liệu |
| **T4** | **Nối màn cấu hình đã có** vào lõi — `/m3/tinh-gia-admin` | T3 xong | Chủ dự án tự sửa được công thức, không cần Agent |
| **T5** | **Bình bài + tối ưu + chi phí máy in + tờ rớt + lượt in** | T4 xong | Tính ra giá thành đầy đủ |
| **T6** | **Nạp nốt các kiểu dáng còn lại** | T5 xong | Phủ hết 40 nhóm |

> ⚠️ **Cổng ra bắt buộc của T2 và T3:** kết quả tính phải **trùng khít** số trong `Test Tính Khổ Trải Tân Phát 19082025.xlsx`. Đó là **bằng chứng khách quan** — không phải Agent tự chấm.

---

## 7. NGUYÊN TẮC THI HÀNH — rút từ chính các lần vấp trong phiên 25/08

| Nguyên tắc | Vì sao có |
|---|---|
| **Đọc tài liệu có sẵn TRƯỚC khi hỏi** | Agent hỏi 4 câu mà tài liệu đã trả lời; Chủ dự án phải nhắc mới đi đọc (sổ `#175`) |
| **Tìm rộng, đừng tìm một từ khoá** | Agent quét thư mục tài liệu **một lần**, hụt vì chỉ tìm 3 từ khoá tiếng Anh, rồi loại luôn thư mục đó |
| **Đo trước khi đề xuất** | Agent đề xuất 2 phương án lưu cấu hình, Chủ dự án bác *«bậy bạ rồi»* vì sai đơn vị cấu hình |
| **Tìm cái đã có trước khi dựng mới** | Agent suýt dựng màn cấu hình **đã tồn tại** (`DEBT-112`) — vi phạm `OD-08` |
| **Việc giao diện: chốt tiêu chí + demo trước** | `GOV-ACCEPTANCE-FIRST-001` §G7.1 + Chủ dự án chốt có làm demo (sổ `#168`) |
| **Không đoán nghiệp vụ** | 10 câu ở §4 phải có lời đáp, không suy diễn |

---

## 8. NHẬT KÝ CHẶNG

| Chặng | Trạng thái | Mốc | Ghi chú |
|---|---|---|---|
| T0 | 🔄 **ĐANG Ở ĐÂY** | 25/08/2026 | 10 câu đã soạn, chờ Chủ dự án |
| T1…T6 | ⏳ | — | Chặn bởi T0 |

---

## 9. LỊCH SỬ SỬA ĐỔI

| Ngày | Sửa gì | Lý do |
|---|---|---|
| 25/08/2026 | Tạo mới | Chủ dự án chốt tách mảng tính giá thành Plan riêng (sổ `#177`), sau khi đo được 10 sự thật ở §1 chứng minh đây là **thiết kế lại một miền nghiệp vụ**, không phải sửa vài con số |
