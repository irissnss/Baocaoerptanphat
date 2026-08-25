# CÔNG THỨC KHỔ TRẢI THẬT CỦA TÂN PHÁT — RÚT TỪ TÀI LIỆU CHỦ DỰ ÁN CHỈ

> **Nguồn:** `docs/Tính Giá Offset/Code Tính Giá Của Netprint/docs/Test Tính Khổ Trải Tân Phát 19082025.xlsx`
> **Cách lấy:** giải nén, đọc công thức Excel thật trong XML — **không suy đoán, không tự chế**.
> **Vì sao có tài liệu này:** Chủ dự án chỉ 25/08/2026 (sổ `#175`): *«Hiện trong root có nhiều tài liệu em có thể xem về khổ giấy, về khổ máy về giá in về giá kẽm về scrip vẻ khuôn về khổ trải về tính khổ trải bình bài tối ưu khổ giấy tối ưu giá thành em xem kỹ lại thì hay hơn đó em»*.
> **Trạng thái:** `FILE_PROVEN` — công thức đọc trực tiếp từ ô Excel.

---

## 0. KẾT LUẬN TRƯỚC — ba điều quan trọng nhất

1. ✅ **Quy ước trục Chủ dự án chốt (sổ `#161`) ĐƯỢC TÀI LIỆU XÁC NHẬN** — công thức Túi Xách Giấy dùng đúng `(Dài + Rộng) × 2`, tức chu vi đáy = 2(L+W), thân cao = Cao.
2. 🔴 **Mỗi kiểu dáng một công thức HOÀN TOÀN KHÁC NHAU** — không phải cùng một công thức đổi hằng số. Hộp Nắp Cài **không** dùng `2(L+W)`.
3. 🔴 **Mô hình "một tấm trải + vài khoản cộng thêm" mà Agent vừa dựng KHÔNG PHỦ NỔI** hộp cứng — hộp cứng cần **4 mảnh × 3 lớp**.

---

## 1. TRANG `TinhKhoTrai` — hộp mềm / carton / túi

**Quy ước cột:** `C` = Dài · `D` = Rộng · `E` = Cao.
**Ba loại kết quả:** *Khổ Trải 1 Hộp* · *Khổ Trải 1/2 Hộp* · **_Khổ Trải 1 Hộp Tối Ưu_**.

### 1.1 🎯 Hộp Nắp Cài — *(Chủ dự án ưu tiên)*

Mẫu đo trong tài liệu: **28 × 17 × 11**

| Kết quả | Chiều Dài | Chiều Rộng |
|---|---|---|
| **Khổ trải 1 hộp** | `Dài + Cao×4 + 3` | `Rộng×2 + Cao×3 + 3` |
| **Khổ trải TỐI ƯU** | `Dài + Cao×3 + 3` | `Rộng×2 + Cao×3 + 3` |

> 🔴 **Điểm phải chú ý:** công thức này **KHÔNG** dùng `(Dài+Rộng)×2`. Nó dùng **`Dài + Cao×4`**.
> ⇒ Công thức đang chạy trong mã (`2(L+W) + 21`) **SAI cho kiểu dáng này**.
> 💡 **"Tối ưu" tiết kiệm đúng `Cao×1`** ở chiều Dài (`Cao×4` → `Cao×3`). Đây chính là *"tối ưu khổ"* Chủ dự án nhắc.

### 1.2 🎯 Túi Xách Giấy — *(Chủ dự án ưu tiên)*

Hai mẫu đo: **39 × 10 × 30** và **16 × 6,5 × 12**

| Kết quả | Chiều Dài | Chiều Rộng |
|---|---|---|
| **Khổ trải 1 hộp** | `(Dài + Rộng) × 2 + 2` | `Cao + 3 + Rộng÷2 + 2` |
| **Khổ trải 1/2 hộp** | `Dài + Rộng + 2` | `Cao + 3 + Rộng÷2 + 2` |

> ✅ **Khớp quy ước Chủ dự án chốt:** `(Dài+Rộng)×2` = chu vi đáy, `Cao + …` = thân cao.
> 🔴 **Có `Rộng ÷ 2`** — phụ cấp **phụ thuộc kích thước**, không phải hằng số. Mô hình Agent vừa dựng **chưa phủ** (đã khai trước ở sổ `#165`).

### 1.3 Nắp Đậy Cài Lưỡi Gà Đáy Cài Chéo

Mẫu đo: **14,5 × 6 × 19**

| Kết quả | Chiều Dài | Chiều Rộng |
|---|---|---|
| **Khổ trải 1 hộp** | `(Dài + Rộng) × 2 + 3` | `Cao + 3 + Rộng + (Rộng÷2 + 2)` |
| **Khổ trải 1/2 hộp** | `Dài + Rộng + 3` | *(như trên)* |

---

## 2. TRANG `TinhKhoTraiCung` — hộp cứng *(phức tạp hơn hẳn)*

> 🔴 **Đây là chỗ mô hình hiện tại của Agent SỤP ĐỔ.** Hộp cứng **không có một tấm trải**. Nó có **BỐN MẢNH** — *Nắp · Đáy · Thành · Khay* — và mỗi mảnh có **BA LỚP** — *Bìa · Áo Ngoài · Áo Lót*. Sau đó mới ghép thành *Khổ Trải 1* và *Khổ Trải 2*.

### 2.1 Hộp Cứng Âm Dương **Có Thành** — mẫu 32 × 23 × 8

| Mảnh | Lớp **Bìa** | Lớp **Áo Ngoài** | Lớp **Áo Lót** |
|---|---|---|---|
| **Nắp** | `(Dài+Cao+5) × (Rộng+Cao+5)` | `Bìa.Nắp + Cao÷2 + 2` *(cả hai chiều)* | `(Dài+2) × (Rộng+2)` |
| **Đáy** | `(Dài+Cao+5) × (Rộng+Cao+5)` | `Bìa.Đáy + Cao÷2 + 2` | `(Dài+2) × (Rộng+2)` |
| **Thành** | `(Dài+Rộng+2) × (Cao×2+2)` | `Dài: Bìa.Thành+3` · `Rộng: Bìa.Thành×2` | — |
| **Khay** | `(Dài+Cao÷2+2) × (Rộng+Cao÷2+2)` | — | *(bằng Bìa.Khay)* |

**Ghép lại:** `Khổ Trải 1 = Nắp + Đáy` *(cộng chiều Dài)* · `Khổ Trải 2 = Khay + Thành`

### 2.2 Hộp Cứng Nam Châm — mẫu 12 × 16 × 2,6 và 12 × 12 × 6

| Mảnh | Lớp **Bìa** | Lớp **Áo Ngoài** | Lớp **Áo Lót** |
|---|---|---|---|
| **Nắp** | `(Rộng×2 + Cao×2 + 2) × (Dài+2)` | `Bìa.Nắp + 5` | *(bằng Bìa)* |
| **Đáy** | `(Rộng + Cao×2 + 2) × (Dài + Cao×2 + 2)` | `Bìa.Đáy + Cao` | *(bằng Bìa)* |
| **Khay** | `(Rộng+Cao+2) × (Dài+Cao+2)` | — | *(bằng Bìa)* |

### 2.3 Hộp Cứng Âm Dương **Không Thành** — mẫu 19,5 × 10 × 5

| Mảnh | Lớp **Bìa** | Lớp **Áo Ngoài** | Lớp **Áo Lót** |
|---|---|---|---|
| **Nắp** | `(Dài + Cao×2 + 5) × (Rộng + Cao×2 + 5)` | `Bìa.Nắp + 5` | *(bằng Bìa)* |
| **Đáy** | `(Dài + Cao×2) × (Rộng + Cao×2)` | `Bìa.Đáy + Cao` | *(bằng Bìa)* |
| **Khay** | `(Dài+Cao+2) × (Rộng+Cao+2)` | — | *(bằng Bìa)* |

---

## 3. ĐỐI CHIẾU VỚI MÃ ĐANG CHẠY — mức lệch

| Kiểu dáng | Công thức THẬT *(tài liệu)* | Mã đang chạy | Khớp? |
|---|---|---|---|
| Túi Xách Giấy | `(D+R)×2 + 2` × `C + Rộng÷2 + 5` | `(D+R)×2 + 21` × `C + R + 36` | ⚠️ **cấu trúc chiều Dài ĐÚNG**, chiều Rộng **sai** *(thiếu `÷2`)* |
| Hộp Nắp Cài | `D + C×4 + 3` × `R×2 + C×3 + 3` | `(D+R)×2 + 21` × `C + R + 36` | 🔴 **SAI HOÀN TOÀN** |
| 3 loại hộp cứng | 4 mảnh × 3 lớp | một tấm trải duy nhất | 🔴 **KHÔNG BIỂU DIỄN ĐƯỢC** |

---

## 4. BỐN ĐIỀU MÔ HÌNH HIỆN TẠI CHƯA PHỦ

| # | Thiếu gì | Bằng chứng trong tài liệu |
|---|---|---|
| 1 | **Công thức riêng theo từng kiểu dáng** | 6 kiểu, 6 công thức khác nhau về **cấu trúc**, không chỉ khác hằng số |
| 2 | **Phụ cấp phụ thuộc kích thước** | `Rộng÷2` · `Cao÷2` xuất hiện ở hầu hết kiểu |
| 3 | **Nhiều mảnh · nhiều lớp** | Hộp cứng: 4 mảnh × 3 lớp, rồi mới ghép |
| 4 | **Khổ trải TỐI ƯU** *(khác khổ trải thường)* | Hộp Nắp Cài: `Cao×4` → `Cao×3`, tiết kiệm `Cao×1` |

---

## 5. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC — nêu rõ

| Điều | Vì sao chưa chắc |
|---|---|
| Đơn vị của các con số | Mẫu ghi `28 · 17 · 11` cho một hộp — nhiều khả năng là **cm**, không phải mm. Mã hiện tại dùng **mm**. **Phải hỏi Chủ dự án.** |
| Ý nghĩa *"Khổ Trải 1/2 Hộp"* | Là **nửa hộp** *(in 2 con ghép lại)*, hay **một nửa của tấm trải**? Chưa rõ |
| Ý nghĩa *"Khổ Trải 1"* / *"Khổ Trải 2"* ở hộp cứng | Là **hai tấm phôi riêng** phải in riêng, hay hai cách bố trí? Chưa rõ |
| Các số cộng thêm *(+2 · +3 · +5)* | Là **chừa xén**, **mép dán**, hay **bù co giấy**? Tài liệu không ghi tên |
| Vì sao *"tối ưu"* tiết kiệm được `Cao×1` | Nguyên lý gấp nào cho phép bớt? Cần Chủ dự án giải thích |

---

## 6. ĐỀ XUẤT — chờ Chủ dự án duyệt, **chưa làm gì**

Mô hình dữ liệu cần đổi từ *"một tấm trải + vài khoản cộng"* sang:

```
Kiểu dáng (nhóm sản phẩm L1)
   └── Bộ công thức khổ trải
         ├── Thường          → { biểu thức Dài , biểu thức Rộng }
         ├── Nửa hộp         → { … }
         └── Tối ưu          → { … }
   └── (hộp cứng) Danh sách MẢNH  × Danh sách LỚP
         └── mỗi ô: { biểu thức Dài , biểu thức Rộng }
   └── Quy tắc GHÉP mảnh thành tấm phôi
```

> ✅ **Tin tốt:** cấu trúc này **đã có sẵn chỗ đứng** — `dm_blueprint.blueprint_json` với khoá `formula_flat_size` + `constants`, và hàm `calculateFlatSize()` biết đọc biểu thức chuỗi. Hồ sơ kỹ thuật gốc (`Smart_Offset_Master_Technical_Spec.md`) mô tả đúng schema đó. **Chỉ là chưa ai nạp dữ liệu và chưa ai nối dây.**
> 🔴 **Phần CHƯA có chỗ đứng:** nhiều mảnh × nhiều lớp *(hộp cứng)*, và khổ trải **tối ưu** như một kết quả riêng.

---

*Mọi công thức trong tài liệu này đọc trực tiếp từ ô Excel, không suy đoán. Kích thước mẫu là số trong tài liệu, không phải dữ liệu khách hàng.*
