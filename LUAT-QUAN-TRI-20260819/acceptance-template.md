# MẪU BẢNG TIÊU CHÍ NGHIỆM THU


> ⚠️ **BẢN CHỤP MỘT THỜI ĐIỂM (19/08/2026) — HÃY KIỂM CÒN HẠN HAY KHÔNG.**
> **Mã băm BẢN GỐC trong kho riêng tư tại thời điểm chụp (SHA-256):** `979daa9ad99ed5c3e1c4312f9f0495f694562c7fc7b0ea12301763d27429b0ac`
> Đây là bản chụp của mẫu bảng tiêu chí nghiệm thu. Nội dung dưới đây **khớp đúng bản gốc** tại thời điểm chụp.
> **Cách kiểm:** chạy `sha256sum` trên **file gốc trong kho riêng tư**.
> Khác mã trên → bản công bố này đã **LỖI THỜI**, tra kho riêng tư để lấy bản đang có hiệu lực.
> *(Cố ý ghi mã của BẢN GỐC chứ không phải của chính file này — vì thêm dòng cảnh báo sẽ làm đổi mã của chính nó, thành ra không kiểm được.)*
> *(Lưu ý: file trong kho báo cáo có thể bị đổi kiểu xuống dòng khi tải về, nên mã băm của CHÍNH file này sẽ khác bản gốc — đó là lý do phải hash bản gốc.)*

> **Doc Version:** 1.0 · **Ban hành:** 18/08/2026 · **Thuộc luật:** `GOV-ACCEPTANCE-FIRST-001`
> **Dùng khi nào:** việc thuộc nhóm **KHÔNG CÓ TIÊU CHÍ TỰ THÂN** — chuẩn hoá · nhất quán · dọn dẹp · tối ưu · làm mượt · đồng bộ · rà soát.
> **Cách dùng:** sao file này sang `.governance/acceptance/<ngày>-<tên-việc>.md`, điền, xin Owner duyệt, **rồi mới sửa dòng mã đầu tiên**.

---

## 0) ĐẦU TRANG — DẤU DUYỆT OWNER (bắt buộc điền trước khi bắt đầu)

```
TÊN VIỆC        : <...>
NGƯỜI ĐỀ XUẤT   : Agent IDE
NGÀY ĐỀ XUẤT    : <dd/mm/yyyy hh:mm>
PHẠM VI ĐỤNG    : <file / thư mục / bảng cụ thể>
PHẠM VI KHÔNG ĐỤNG: <src? DB? deploy? version?>
LỚP BẰNG CHỨNG CHÍNH: <UI_PROVEN | DB_PROVEN | RUNTIME_PROVEN>   ← xem mục 2

OWNER DUYỆT     : [ ] ĐÃ DUYỆT — ngày <dd/mm/yyyy hh:mm> — nguyên văn: "<...>"
                  [ ] CHƯA DUYỆT  → CẤM bắt đầu sửa (GOV-ACCEPTANCE-FIRST-001, FAILURE: BLOCK_ALL)
```

---

## 1) BẢNG TIÊU CHÍ — MỖI DÒNG PHẢI ĐO ĐƯỢC

Mỗi tiêu chí **một dòng**. Cột `ngưỡng đạt` phải là **con số hoặc so-sánh-được**, không phải cảm nhận.

| # | tiêu chí | cách đo | ngưỡng đạt | bằng chứng | trạng thái |
|---|---|---|---|---|---|
| 1 | <việc phải đạt được, nêu cụ thể> | <lệnh / công cụ / thao tác đo> | <con số hoặc so sánh được> | <đường dẫn ảnh / kết quả lệnh> | ⬜ CHƯA · ✅ ĐẠT · ❌ KHÔNG ĐẠT |
| 2 | | | | | ⬜ |
| 3 | | | | | ⬜ |

---

## 2) DÒNG BẮT BUỘC — LỚP BẰNG CHỨNG NƠI LỖI THỰC SỰ LỘ RA

⛔ Bảng **KHÔNG hợp lệ** nếu thiếu tối thiểu **MỘT** dòng ở lớp dưới đây, chọn theo bản chất việc:

| Loại việc | Lớp bắt buộc | Nghĩa là | Ví dụ dòng tiêu chí |
|---|---|---|---|
| **Giao diện** | `UI_PROVEN` | Lớp người dùng nhìn thấy — **ảnh chụp** | "Ảnh chụp trang X ở 1920px đặt cạnh trang mẫu Y: bo góc · lề · mật độ dòng · vị trí pill khớp mẫu — đối chiếu từng mục, không mục nào lệch" |
| **Dữ liệu** | `DB_PROVEN` | Lớp dữ liệu thật — **kết quả truy vấn** | "Truy vấn `SELECT COUNT(*) …` trên DB thật trả đúng N dòng; 0 dòng sai kiểu/NULL ngoài dự kiến" |
| **Vận hành** | `RUNTIME_PROVEN` | Lớp môi trường vận hành — **bằng chứng môi trường** | "Tiến trình đứng vững sau restart; N bảng khớp; route Z trả mã đúng" |

> **Lý do có mục này:** ca giao diện 17–18/08/2026 nghiệm thu bằng `build 0 · tsc 0 · đăng nhập 200 · 99 bảng khớp` — **cả 5 đều PASS kể cả khi giao diện sai hoàn toàn**, vì không có dòng nào ở lớp người dùng nhìn thấy. 12 gói việc, 9 lượt Owner bác liên tiếp.

---

## 3) TỪ NGỮ BỊ CẤM LÀM TIÊU CHÍ

`GOV-ACCEPTANCE-FIRST-001` cấm dùng chữ định tính. Nếu tiêu chí chứa các từ dưới đây mà **không kèm cách đo + ngưỡng số**, bảng bị trả lại:

> gọn gàng · hài hoà · đẹp hơn · sạch hơn · mượt hơn · chuẩn hơn · nhất quán hơn · trông ổn · hợp mắt · chuyên nghiệp

**Cách chữa:** đổi thành so sánh được với một **mốc cụ thể**.

| ❌ Cấm | ✅ Được |
|---|---|
| "giao diện gọn gàng hơn" | "lề trái/phải ≤ 16px ở 1920px, đo trên ảnh chụp" |
| "nhất quán với trang mẫu" | "12/12 mục trong bảng đối chiếu với trang mẫu `<đường dẫn>` đều KHỚP" |
| "bảng nhìn dễ hơn" | "≥ 15 dòng hiện cùng lúc ở màn 1080p, không cuộn" |

---

## 4) KẾT LUẬN — "XONG" LÀ GÌ

```
XONG = ĐẠT HẾT mọi dòng ở mục 1 (không dòng nào ⬜ hoặc ❌)
     + dòng bắt buộc ở mục 2 ĐẠT với bằng chứng đúng lớp
     + Owner đã duyệt bảng này TRƯỚC khi bắt đầu

CẤM tuyên bố xong khi: còn dòng ⬜ · còn dòng ❌ · thiếu bằng chứng của dòng bắt buộc
CẤM tự nới ngưỡng đạt giữa chừng — muốn nới phải xin Owner duyệt lại, ghi rõ ngày/lý do
```

| Tổng kết | Giá trị |
|---|---|
| Số tiêu chí | <n> |
| ĐẠT | <n> |
| KHÔNG ĐẠT | <n> |
| Kết luận | ⬜ XONG · ⬜ CHƯA XONG |
| Lần lặp thứ (`GOV-ITERATION-LIMIT-001`) | <n> — nếu **≥ 3** → DỪNG CÁCH LÀM CŨ, báo Owner đề xuất cách mới |

---

## 5) LỊCH SỬ SỬA ĐỔI (file mẫu này)

| Ngày | Người sửa | Lý do | Nội dung |
|---|---|---|---|
| 18/08/2026 | Agent IDE | Ban hành theo `GOV-ACCEPTANCE-FIRST-001` (Owner duyệt 18/08/2026) — điều kiện nạp của luật L1 | Tạo mới |
