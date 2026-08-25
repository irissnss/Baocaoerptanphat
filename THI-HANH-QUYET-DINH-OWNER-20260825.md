# THI HÀNH QUYẾT ĐỊNH CHỦ DỰ ÁN — 25/08/2026

> **Bản công khai đã lọc.** Không chứa mã nguồn · dữ liệu khách hàng · email · IP · cổng máy chủ.
> **Plan of Record:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825`

---

## Ba quyết định — đã thi hành

### 1. Quy ước trục hộp: đáy = Dài × Rộng, thân cao bằng Cao

**Tin tốt lớn nhất của đợt này:** quy ước chủ dự án chốt **chính là quy ước công thức đang nối vào tiền thật** đang dùng.

> ⇒ **Đường tiền vốn đã tính đúng. Không đồng nào bị tính sai trên báo giá thật.**

Việc phải sửa là màn tính giá thủ công và hai tệp vẽ hình — chúng dùng quy ước **ngược lại**.

**Đã làm:** dồn **sáu chỗ** tính khổ trải rải rác về **một nguồn duy nhất**. Nơi dùng không còn phải tự đoán trục nào là trục nào — đó chính là gốc sinh ra cả vấn đề.

**Chứng minh bằng máy, không bằng lời:** bộ kiểm cho thấy sau khi căn trục, công thức mới và công thức đường-tiền chỉ lệch nhau **đúng hai hằng số** (kiểm trên 3 mẫu chuẩn + 3 mẫu biến động mạnh); và khi đặt hằng số bằng của đường-tiền thì **ra kết quả trùng khít**.

**Hai lỗi thật được vá kèm:**
- Khi khổ trải **không lọt tờ giấy**, bản cũ âm thầm ép về **1 con/tờ** — đáp án **đắt nhất có thể** — không cảnh báo gì. Nay báo rõ.
- Bản cũ **không bao giờ thử xoay tờ 90°**, nên bỏ lỡ những cách xếp tiết kiệm giấy hơn. Nay có thử.

### 2. Bốn hằng số phụ cấp: làm màn cấu hình để chủ dự án tự gán

**Hạ tầng đã xong:** hằng số nay nằm ở **đúng một chỗ**, và hàm tính **đã nhận cấu hình từ bên ngoài** — sẵn sàng nối vào màn cấu hình.

**Còn thiếu:** màn cấu hình + giá trị chủ dự án cung cấp. Màn cấu hình là **việc giao diện**, nên theo quy định nội bộ phải **chốt bảng tiêu chí nghiệm thu trước khi viết dòng mã đầu tiên**.

Giá trị đang chạy được giữ nguyên và **gắn nhãn rõ ràng là chưa được chốt** — không phải đề xuất.

> Riêng **bù hao** hiện là **0** — đó là **sự thật hiện trạng**, không phải đề xuất: rà toàn bộ 101 bảng dữ liệu **không có cột nào lưu bù hao**, và không công thức nào trong mã có bù hao.

### 3. Quyền xem giá vốn: chỉ Quản trị + Tổng giám đốc

**Đính chính báo cáo trước:** báo cáo trước nói *"giá vốn không được che ở phần lớn nơi hiển thị"* — **sai**. Đo lại kỹ: cơ chế che **có hoạt động**, chỉ là **thiếu hai cột**.

| Cột giá vốn | Trước | Sau |
|---|---|---|
| Báo giá | ✅ đã che | ✅ |
| Đơn hàng | ✅ đã che | ✅ |
| Vật tư | ❌ **hở** | ✅ **đã vá** |
| Lịch sử tính giá | ❌ **hở** | ✅ **đã vá** |

Chỗ hở thật là **2/4 cột**. Rủi ro thực tế thấp vì hai bảng đó đang rỗng — nhưng sẽ hở ngay khi có dữ liệu đầu tiên.

Đề xuất thêm **Kế toán** đã bị chủ dự án **từ chối** — giữ đúng phạm vi được duyệt.

---

## Bằng chứng

| Phép kiểm | Kết quả |
|---|---|
| Bộ kiểm khổ trải | **35 đạt / 0 hỏng** |
| Bộ kiểm phân quyền | **111 đạt / 0 hỏng** |
| Kiểm kiểu mã nguồn | sạch |
| Cổng quản trị | **18/18 đạt** |

**Kiểm ngược bắt buộc** — bộ kiểm chỉ có giá trị nếu nó **đỏ khi gỡ bản vá**:

| Gỡ gì | Kết quả | Kết luận |
|---|---|---|
| Gỡ khai báo cột giá vốn vật tư | **3 hỏng** | bộ kiểm có giá trị |
| Đảo ngược quy ước trục | **9 hỏng** | bộ kiểm có giá trị |
| Khôi phục cả hai | **0 hỏng** | không phá gì |

---

## Một bài học tự ghi nhận

Trong đợt này, bộ kiểm mới **báo nhầm một lần**: nó soi cả tệp để bắt việc *"còn dùng công thức cũ"*, nhưng **chính lời chú thích giải thích bản vá** có nhắc lại công thức cũ nên bị tính là vi phạm.

Đó **đúng cái bẫy** vừa được vá cho một cổng an toàn khác cùng ngày — cổng đó khớp một cụm từ ở **bất kỳ đâu trong dòng**, kể cả trong phần mô tả. Đã sửa cả hai theo cùng nguyên tắc: **chỉ soi phần có thẩm quyền, không soi phần giải thích**.

---

## Chưa làm — nói rõ

- **Chưa triển khai lên máy vận hành.** Phiên này không được cấp kênh đo máy vận hành, nên mọi số liệu là **máy phát triển**.
- **Chưa có màn cấu hình hằng số** — chờ chủ dự án cung cấp giá trị hoặc duyệt bảng tiêu chí nghiệm thu.
- **Hai đường nạp dữ liệu trong ứng dụng vẫn chưa có kiểm ngưỡng lỗi.**
- Kiểm chất lượng mã trên màn tính giá **đã đỏ sẵn từ trước** (đo được: y hệt nhau trước và sau đợt sửa) — không sửa vì ngoài phạm vi được duyệt.

---

*Mọi con số tự đo trong lượt này. Bản đầy đủ kèm bằng chứng chi tiết nằm ở kho riêng tư.*
