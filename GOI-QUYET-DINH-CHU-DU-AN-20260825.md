# GÓI QUYẾT ĐỊNH CHO CHỦ DỰ ÁN — 25/08/2026 · BẢN CÔNG KHAI

> ⚠️ **BẢN ĐÃ CHE.** Bản đầy đủ giữ ở kho riêng tư.
> Không nêu tên tệp, tên thư mục, tên cột, đường dẫn hay mã commit của dữ liệu nhạy cảm — giá trị vẫn còn trong lịch sử kho mã.
> Chỉ nêu **số lượng**, **cơ chế**, **phương án**.

**Yêu cầu:** gom mọi việc đang chờ chủ dự án vào một chỗ để xử lý một lượt, kèm phương án đề xuất.

---

## 0. Trạng thái công bố

| Kho | Việc chưa đẩy |
|---|---|
| Kho mã (riêng tư) | **không còn** — đã đẩy hết |
| Kho báo cáo (công khai) | **không còn** — đã đẩy hết |

Còn **3 tệp chưa lưu** trong kho riêng tư, nhưng đó là **việc đang dở của một phiên làm việc khác chạy song song** — trong đó có một **cổng kiểm mới đang được viết**. Phiên này **cố ý không đụng vào**, vì hai phiên cùng ghi một thư mục làm việc đã từng gây mất dữ liệu hôm 23/08.

**Về các báo cáo chưa công khai:** đã rà từng cái — **không cái nào bị bỏ sót do nhầm**. Phần lớn là **kế hoạch nội bộ** (kho công khai theo tiền lệ không đăng kế hoạch), số còn lại **tự khai "không cần công khai"** kèm lý do ngay trong báo cáo.

---

## 🔴 1. Việc gấp — nặng hơn báo cáo 23/08, và **chưa được xử lý**

Báo cáo ngày 23/08 nêu **một** tệp dữ liệu đang bị quản lý phiên bản dù quy định cấm. Đo lại hôm nay bằng cách khác: **thực ra là sáu tệp**, và **cổng kiểm vẫn chưa được sửa**.

### Bằng chứng đối chứng

| Cách cổng lấy danh sách tệp | Tổng tệp | Số tệp dữ liệu hàng loạt **nhìn thấy được** |
|---|---:|---:|
| Cách đang dùng (thiếu một tham số) | 1.530 | **0** |
| Cách đúng | 1.530 | **6** |

Kiểm trực tiếp: cổng **chưa có** tham số còn thiếu, và vẫn báo **đạt, 0 vi phạm**.

### Sáu tệp

| # | Loại | Kích thước | Dòng | Dữ liệu cá nhân |
|---|---|---:|---:|---|
| 1 | 🔴 danh mục đối tác | ~327 KB | 1.233 | **2.368 số điện thoại · 22 địa chỉ thư** + cột thông tin thanh toán |
| 2–4 | biểu mẫu nghiệp vụ | ~286 KB | 1.958 | **0** đo được |
| 5–6 | bảng nhập liệu hàng ngày | ~395 KB | 3.025 | **0** đo được |

**Cả sáu đều có dấu tiếng Việt trong đường dẫn** — đó chính xác là lý do cổng không nhìn thấy tệp nào. Chỉ tệp #1 chứa dữ liệu cá nhân; năm tệp còn lại là biểu mẫu, nhưng vẫn nằm ngoài tầm quét.

### Vì sao gấp

Cổng này **chạy tự động ở mỗi lần lưu thay đổi** ⇒ đang cho kết quả sai liên tục, và **mù với mọi đường dẫn có dấu tiếng Việt**. Trong một dự án Việt Nam đó là một lớp tệp rất lớn — bất kỳ tệp dữ liệu nào thêm vào thư mục có dấu sẽ tiếp tục lọt.

### Phương án đề xuất

1. **Sửa cổng trước** — thêm tham số còn thiếu (đúng một dòng), **bắt buộc kèm phép kiểm ngược**: đặt lại một tệp có dấu vào kho → cổng phải báo lỗi; gỡ ra → phải báo đạt. **Không sửa mà không kiểm ngược.**
2. **Gỡ tệp #1 khỏi quản lý phiên bản**, giữ tệp trên máy, bổ sung quy tắc loại trừ.
3. **Năm tệp biểu mẫu**: đề xuất **giữ lại** (là biểu mẫu chuẩn, cần lịch sử phiên bản) và **khai nhận vào bản đồ thư mục** để lần sau cổng báo thì biết là có chủ đích.
4. **Xoá khỏi lịch sử kho mã**: **không tự suy rộng** tiền lệ trước đó. Lần trước là bí mật nội bộ; lần này là **dữ liệu của đối tác**. Cần chủ dự án quyết riêng.

---

## 2. Sáu nhóm quyết định

Sổ nợ kỹ thuật hiện **107 dòng, 81 đang mở**, trong đó **23 dòng chờ chủ dự án**. Gom thành sáu nhóm để quyết theo nhóm thay vì từng dòng.

| Nhóm | Nội dung | Đề xuất |
|---|---|---|
| 🔴 **A — Bảo mật & dữ liệu cá nhân** | Tệp dữ liệu còn trong kho · cổng mù · trạng thái mật khẩu quản trị trong sổ chưa khớp thực tế | Sửa cổng **trước**, rồi gỡ tệp; việc xoá khỏi lịch sử để chủ dự án quyết riêng |
| 🟠 **B — Nạp dữ liệu & đối chứng** | Đợt nạp lên máy vận hành **đã chạy khi ngưỡng lỗi chưa được chốt** (quy định ghi rõ trường hợp này là chặn toàn bộ) · hai nợ có hạn *"trước khi nạp"* vẫn mở dù việc đã nạp xong · **1.692 đối tác đang gán tạm cho một người** | **Không hoàn tác** — dữ liệu đã đúng, đo được lỗi 0%. Ghi một ngoại lệ có lý do, **chốt ngưỡng ngay** cho đợt sau. Việc phân bổ người phụ trách chỉ chủ dự án làm được |
| 🟠 **C — Tính giá** *(đang chặn)* | 🔴 **Hai công thức khổ trải khác nhau cùng tồn tại trong mã** · thiếu dữ liệu cấu hình · bộ kiểm thử dựng dữ liệu giả | Xử lý **công thức trước tiên** — hai công thức khác nhau nghĩa là **một trong hai đang tính sai tiền**. Đây là nghiệp vụ, trợ lý không tự quyết |
| 🟡 **D — Sổ sách & quy tắc** | Sổ nợ dùng **hai trạng thái mà quy tắc không định nghĩa** · **mã nợ bị cấp trùng** (đã tái diễn lần hai) · sổ yêu cầu có một số mục trùng | **Sửa quy tắc, không sửa sổ** — hai trạng thái đó phản ánh vận hành đúng. **Không đánh số lại** (phá tham chiếu chéo); thêm một cổng chặn cấp trùng |
| 🟡 **E — Tài liệu & nguồn sự thật** | 🔴 **Lệch độ rộng hai đầu một quan hệ dữ liệu** (đo được ở môi trường chạy) · tài liệu xuất ra có hai lớp chồng nhau không gắn nhãn | Tách việc **lệch độ rộng** làm trước — có thể **cắt cụt dữ liệu khi ghi** |
| ⚪ **F — Việc nhỏ** | Bộ lọc danh sách bỏ sót nhân sự chưa có tài khoản · hai bản lưu tạm cũ · một phép kiểm lỗi thời · một nút còn ghi cứng mã vai trò | Gom **một phiên dọn**, sau A–C |

## Thứ tự đề xuất

| Ưu tiên | Nhóm | Vì sao |
|---|---|---|
| **1** | 🔴 A | Cổng đang cho kết quả sai **mỗi lần lưu**; dữ liệu đối tác đã lên kho từ xa |
| **2** | 🟠 C | Hai công thức khác nhau ⇒ **có thể đang tính sai tiền** |
| **3** | 🟠 B | Đã chạy rồi — cần hợp thức hoá + chốt ngưỡng cho đợt sau |
| **4** | 🟡 E | Lệch độ rộng cột có thể cắt cụt dữ liệu |
| **5** | 🟡 D | Không chặn ai, nhưng để lâu thì sổ mất tin cậy |
| **6** | ⚪ F | Gom một lượt |

---

## 3. Một việc đã xong, xin xác nhận lại

Chủ dự án hỏi: *có cần bổ sung quy tắc để chuẩn hoá việc **mã đi trước tài liệu** khi trao đổi trực tiếp liên tục, và việc **ghi nhận các yêu cầu / xác nhận / chia sẻ** để trợ lý Notion không bỡ ngỡ?*

**Quy tắc đó đã được ban hành rồi**, và ban hành đúng vì lý do vừa nêu:

- **Quy tắc quyết định trong phiên** (20/08) ghi thẳng: *mã **được** đi trước tài liệu khi đã có mục sổ hợp lệ; tài liệu chưa kịp là **nợ đồng bộ**, **không phải sai**.*
- **Quy tắc bàn giao về Notion** (25/08, hôm nay) bổ sung năm điều: sổ yêu cầu là **kênh vận chuyển** chứ không phải nguồn cạnh tranh với Notion · **mỗi chỉ thị riêng biệt là một mục sổ** · trường trạng thái đồng bộ chỉ nhận **năm giá trị** · kết thúc gói việc có quyết định mới thì **bắt buộc có gói bàn giao** · **ghi bù phải giữ mốc thật**.

Bộ quy tắc hiện ở bản **2.8**, năm tệp đồng nhất tới từng byte. Yêu cầu này đã vào sổ.

**Điểm còn yếu, nói cho đúng:** quy tắc mới ban hành hôm nay nên mới **19 trên 151** mục sổ có trường trạng thái đồng bộ — phần ghi bù còn lại chưa xong. Cổng thi hành trường này **đang được viết dở** ở phiên song song.

---

## 4. Tự đính chính

Báo cáo 23/08 nói **một** tệp. Con số đúng là **sáu**. Lệnh kiểm hôm đó của chính người rà cũng bỏ lọt vì đúng cái lỗi mà nó đang tố cáo — cách lấy danh sách tệp không xử lý được đường dẫn có dấu. Ghi lại để lần sau không ai lặp.
