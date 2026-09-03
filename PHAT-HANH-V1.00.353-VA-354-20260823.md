# ✅ ĐÃ PHÁT HÀNH — V1.00.353 và V1.00.354 (23/08/2026)

**Ngày:** 23/08/2026 · **Loại:** **PHÁT HÀNH LÊN HỆ THỐNG VẬN HÀNH** — hai đợt liên tiếp
**Hệ thống vận hành hiện chạy:** **`V1.00.354`** · mã commit **`<mã-nguồn-riêng>`** · nhánh `main`
**Trước đợt này:** `V1.00.352` · commit `<mã-nguồn-riêng>`

> Bản tin public-safe: chỉ nêu số lượng, mã kỹ thuật, tên bảng/màn hình. Không có thông tin đăng nhập,
> không có dữ liệu khách hàng, không có số tiền thật.

---

## 1. TÊN FILE + MÃ COMMIT (đầy đủ, không ghi tắt)

| Mốc | Mã commit | Nội dung |
|---|---|---|
| Trước phát hành | `<mã-nguồn-riêng>` | V1.00.352 — bản đang chạy trước đó |
| **Phát hành 1** | **`<mã-nguồn-riêng>`** | **V1.00.353** — go-live chuỗi bán hàng (4 đợt A·B·C·D) + ngày về chuẩn `DD/MM/YYYY` |
| **Phát hành 2** | **`<mã-nguồn-riêng>`** | **V1.00.354** — quyền sửa biểu mẫu chỉ quản trị + tổng giám đốc, cấp bằng ô tick |
| Sau phát hành | `<mã-nguồn-riêng>` | bộ kiểm khói máy vận hành (bổ sung màn Biểu Mẫu) |

**Báo cáo công khai trước đó của chuỗi này:**
- `THI-CONG-GOLIVE-BAN-HANG-DOT-A-D-20260823.md` — commit `<mã-nguồn-riêng>`
- `RA-BIEU-MAU-VA-TRANG-THAI-PHAT-HANH-20260823.md` — commit `<mã-nguồn-riêng>`

---

## 2. QUY TRÌNH PHÁT HÀNH — TỪNG BƯỚC, CÓ SỐ ĐO

### Trước khi đụng hệ thống vận hành

| Bước | Kết quả |
|---|---|
| Kho mã sạch, khớp bản trên GitHub | ✅ `<mã-nguồn-riêng>` = `<mã-nguồn-riêng>` |
| Sao lưu **MỚI** (cấm dùng bản cũ) | ✅ lấy lúc **13:35** — 18 MB, **101 bảng**; đợt 2 lấy lại lúc **16:08** |
| Kế hoạch quay lui viết **TRƯỚC** | ✅ commit `<mã-nguồn-riêng>` |
| Diễn tập trên **bản sao mới** của dữ liệu vận hành | ✅ 101 bảng khôi phục = 101 bảng nguồn · **12/12 bảng trọng yếu khớp tuyệt đối số dòng** |
| Chạy kiểm thử trên dữ liệu hình dạng thật | ✅ **118 điểm kiểm, 0 trượt** |
| Nghiệm thu 12 điểm | ✅ **12/12 đạt cả hai lớp** (truy vấn thật + ảnh thật) |
| Hồi quy toàn bộ | ✅ **16 bộ · 675 điểm kiểm · 0 trượt** · kiểm kiểu dữ liệu 0 lỗi · dựng bản phát hành thành công |

### Sau khi phát hành — kiểm khói trên hệ thống vận hành thật

**12/12 ĐẠT.** Không phải chỉ "mở được" — mỗi đường kiểm **3 thứ**: mã trả lời, có chữ đặc trưng của
màn đó, và **không có dấu hiệu lỗi trong chữ người dùng nhìn thấy**.

| # | Màn | Kết quả |
|---|---|---|
| 1 | Đăng nhập | ✅ mã 200 |
| 1b | **Đăng nhập thật vào hệ thống vận hành** | ✅ vào được |
| 2 | Khách hàng | ✅ |
| 3 | Nhân sự | ✅ |
| 4 | Báo giá | ✅ |
| 5 | Đơn hàng | ✅ |
| 6 | Bảo mật — ma trận quyền hành động | ✅ |
| 6b | Ma trận hiện đủ ô tick | ✅ **16 ô** |
| 7 | Công nợ | ✅ |
| 8 | **Biểu mẫu / mẫu in** | ✅ |
| 10 | Giao diện hiển thị đúng phiên bản | ✅ **V1.00.354** |
| 11 | **Không có lỗi máy chủ nào suốt lượt kiểm** | ✅ **0 lỗi 5xx** |

### Đối chiếu hệ thống vận hành trước ↔ sau

| Chỉ số | Trước | Sau |
|---|---|---|
| Mã commit | `<mã-nguồn-riêng>` | **`<mã-nguồn-riêng>`** |
| Phiên bản | 352 | **354** |
| Số bảng dữ liệu | 101 | **101** — *không đổi, đúng như cam kết 0 thay đổi cấu trúc* |
| Tiến trình ứng dụng | đang chạy | **đang chạy** |

---

## 3. ĐỢT 2 — QUYỀN SỬA BIỂU MẪU (Chủ dự án chốt 23/08)

> Nguyên văn: *"Sửa biểu mẫu chỉ có admin, hoặc CEO được sửa nha em"*

### Vấn đề trước đó (đo được, không suy đoán)

Muốn cho ai sửa mẫu in thì phải cấp cả cụm quyền **M0** — mà M0 còn chứa **phân quyền, phòng ban, quy trình**.
Nghĩa là **cho sửa mẫu in là lỡ tay mở luôn quyền động vào bảo mật**.

### Cách làm — **không viết cứng vai trò**

Đây là điểm quan trọng nhất của đợt này:

| Cách làm SAI (không dùng) | Cách đã làm |
|---|---|
| Viết trong mã: *"nếu vai trò là ADMIN hoặc CEO thì cho sửa"* | Thêm **một quyền mới** vào danh mục, rồi **TICK** cho hai vai trò đó trong ma trận |
| Đổi người giữ quyền ⇒ phải sửa mã + phát hành lại | Đổi người giữ quyền ⇒ **bỏ tick / tick lại**, xong ngay, không đụng mã |

Đúng nguyên tắc Chủ dự án khoá vĩnh viễn: *cấm gắn cứng quyền theo tên, nhãn, chức danh — và theo cả mã vai trò*.

### Khoá đôi

Ba đường ghi (thêm · sửa · xoá biểu mẫu) **giữ nguyên** kiểm quyền M0 **và thêm** kiểm quyền mới.
**Phải qua CẢ HAI.** Vai trò có quyền M0 nhưng chưa được tick ⇒ **vẫn bị chặn**.

### Đo được

| Điểm | Kết quả |
|---|---|
| Quyền mới có trong danh mục | ✅ danh mục nay **16 quyền**, quyền này ở **tầng NHẠY CẢM** |
| Đúng **2 vai trò** được phép | ✅ trong đó có vai trò quản trị hệ thống |
| **Có quyền M0 đầy đủ nhưng chưa tick → VẪN BỊ CHẶN** | ✅ **đây chính là lỗ hổng Chủ dự án yêu cầu bịt** |
| Tick bật → được sửa **ngay**; bỏ tick → mất **ngay** | ✅ đo qua tầng phân quyền thật |
| Đường ghi không nhắc mã vai trò nào | ✅ **0 lần** |
| Trên hệ thống vận hành | ✅ đúng 2 vai trò được cấp, không thừa |

**Chứng minh bộ kiểm thử có tác dụng thật:** cố ý gỡ chốt quyền ở đường **sửa** → bộ kiểm thử **báo đỏ ngay**
(*"2/3 chốt"*); trả mã về nguyên bản → **đạt trở lại**.

**Thứ tự thực hiện có tính toán:** **tick quyền TRƯỚC, phát hành mã SAU** — để không có khoảnh khắc nào
tổng giám đốc bị chặn giữa hai bước.

---

## 4. TRẠNG THÁI DỌN DỮ LIỆU THỬ

| Nơi | Tài khoản thử | Vai trò thử | Gán vai trò | Quyền thử | Hồ sơ NV thử |
|---|---|---|---|---|---|
| **Hệ thống vận hành** | **0** | **0** | **0** | **0** | **0** |
| **Máy nội bộ** | **0** | **0** | **0** | **0** | **0** |

✅ **Đã dọn đúng quy trình.** Mọi bộ kiểm thử **tự tạo dữ liệu thử rồi tự xoá**, và **đối chiếu số bản ghi
trở về đúng mốc nền** trước khi kết thúc.

✅ **Bản sao dữ liệu dùng để diễn tập: ĐÃ XOÁ** sau khi phát hành xong (đã kiểm lại: còn **0** bản sao).
Đây là điều đã cam kết trong bản tin trước.

✅ **Tệp cấu hình tạm trên máy chủ: ĐÃ XOÁ.** Đường hầm kết nối tạm: **đã đóng.**

📦 **Còn giữ có chủ đích:** hai bản sao lưu dữ liệu trước mỗi lần phát hành, đặt **ngoài kho mã**, để quay lui
khi cần.

**Nguồn ảnh nghiệm thu:** **100% do phía kỹ thuật tự chụp bằng máy** — không có ảnh nào do Chủ dự án cung cấp.
Ảnh **chứa dữ liệu cá nhân thật** nên **KHÔNG đẩy lên kho công khai**.

---

## 5. HAI LỖI CỦA CHÍNH PHÍA KỸ THUẬT — TỰ PHÁT HIỆN, TỰ SỬA

**1. Bộ kiểm khói báo đỏ giả 6/7 đường.** Lượt kiểm khói đầu tiên sau phát hành báo **6 đường trượt**, trong
khi mã trả lời đều là 200, **0 lỗi máy chủ**, và giao diện hiển thị đúng. Mở ảnh ra xem thì trang **chạy hoàn hảo**.

Nguyên nhân là lỗi của **chính bộ kiểm**:
- nó quét **mã nguồn trang** (gồm cả gói JavaScript) thay vì **chữ người dùng nhìn thấy**, nên trúng một câu
  báo lỗi nằm sẵn trong gói chung của khung ứng dụng;
- và lấy `"500"` làm dấu hiệu lỗi — chuỗi này **khớp luôn số tiền `5.000.000`**.

Đã sửa sang đọc **chữ hiển thị thật**, chạy lại: **11/11 đạt**, rồi **12/12** sau khi thêm màn Biểu Mẫu.
**Bài học ghi lại: cổng kiểm phải đo thứ người dùng THẤY, không đo mã nguồn trang.**

**2. Ngày hiển thị sai chuẩn — chỉ lộ ra khi xem ảnh.** Màn đơn hàng hiển thị `2026-02-06` thay vì
`06/02/2026`. Không bộ kiểm thử nào bắt được, đọc mã cũng không thấy. Đã sửa và **đã lên hệ thống vận hành**
trong đợt V1.00.353.

> 📌 Đây là **lần thứ ba trong ngày** việc **xem ảnh bằng mắt** bắt được lỗi mà rà mã bỏ qua.

---

## 6. VIỆC CÒN CHỜ CHỦ DỰ ÁN

| # | Việc | Chặn gì |
|---|---|---|
| 1 | **Xem lại danh sách 12 điểm nghiệm thu** — do phía kỹ thuật soạn, Chủ dự án chỉ nêu con số 12 chứ không liệt kê | Không chặn vận hành; chỉ để công nhận nghiệm thu là chính thức |
| 2 | **Mẫu Đơn Hàng** — cần phê duyệt mẫu nghiệp vụ trước khi làm | Chặn việc in đơn hàng (hiện **cố ý** chưa làm, không bịa thuế/điều khoản/chữ ký) |
| 3 | **Dọn dòng trùng trong danh mục biểu mẫu** — báo giá 4 dòng, đơn hàng 4 dòng cùng trạng thái đang dùng | Không chặn; gây khó hiểu khi tra |
| 4 | **Phân bổ khách hàng cho từng nhân viên kinh doanh** | Việc vận hành, thuộc Chủ dự án |

---

## 7. NẾU CẦN QUAY LUI

Cả hai đợt **không đổi cấu trúc dữ liệu** (101 bảng trước = 101 bảng sau), nên quay lui **chỉ cần đưa lại
mã nguồn bản cũ** — **không phải hoàn tác dữ liệu**. Thời gian dự kiến **5–10 phút**.
Mốc quay về: `<mã-nguồn-riêng>` (V1.00.352) hoặc `<mã-nguồn-riêng>` (V1.00.353).
Sáu ngưỡng bắt buộc quay lui và bảy đường kiểm khói đã chốt sẵn từ trước khi phát hành.

---

*Báo cáo public-safe. Không chứa mã nguồn, thông tin đăng nhập, dữ liệu khách hàng hay số liệu tài chính thật.*
