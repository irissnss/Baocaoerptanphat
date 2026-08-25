# GÓI BÀN GIAO NOTION — PHIÊN 25/08/2026

> **Luật:** `GOV-NOTION-HANDOFF-001` §F1c mục 4 — kết thúc work package có quyết định Owner mới thì **bắt buộc** có gói bàn giao.
> **Người lập:** Agent IDE (Claude Code) · **Người nhận:** TanPhatAI / Agent Notion
> **Plan:** `PL-ERP-SINGLE-TRACK-RECOVERY-20260825` + `PL-ERP-TINH-GIA-20260825`
>
> ⛔ **Agent IDE KHÔNG sửa Notion và KHÔNG ghi `SYNCED_TO_NOTION`.** Gói này là **đầu vào** để TanPhatAI tự đối chiếu và tự cập nhật.
> ⚖️ **Sổ Yêu Cầu Owner là KÊNH VẬN CHUYỂN, không phải nguồn cạnh tranh với Notion** (`OD-01`). Khi Notion và gói này nói khác nhau về **cùng một điều** ⇒ **DỪNG, báo Owner** — đừng bên nào tự thắng.

---

## 0. TỔNG QUAN

| Mục | Giá trị |
|---|---|
| Số mục sổ trong phiên | **24** (`#155` → `#178`) |
| Chờ bàn giao Notion | **22** |
| Không áp dụng | **2** (`#157` câu treo nội bộ · `#175` chỉ dẫn đọc tài liệu) |
| Số chủ đề bàn giao | **9** |
| Mã phiên bản mã nguồn | **V1.00.355** — **KHÔNG bump** |
| Triển khai | **NOT_DEPLOYED** — chưa lên máy vận hành |
| Bằng chứng runtime máy vận hành | ⛔ **UNVERIFIED** — phiên này không được cấp kênh đo |

**Mốc mã nguồn:** kho riêng `f25c5b9` · kho công khai `fe3cdea`
**Bộ kiểm toàn phiên:** `test:gov-gates` **18/18** · `test:m1-rbac` **111/0** · `test:kho-trai` **56/0** · `test:ledger-allocator` **20/20** · `tsc --noEmit` **EXIT=0**

---

## CHỦ ĐỀ A — AN TOÀN DỮ LIỆU CÁ NHÂN & PHƠI NHIỄM LỊCH SỬ

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#156` · `#157` · `#166` |
| 2 | Nguyên nghĩa quyết định Owner | Dữ liệu **khách hàng · nhà cung cấp · nhân viên** thuộc đợt **mới nạp** là **BẢN CUỐI → GIỮ**; phần còn lại là **bản nháp → XOÁ**. Về **lịch sử kho mã**: **GIỮ NGUYÊN, chấp nhận rủi ro** — *"chưa có dữ liệu tuyệt mật lắm để bảo vệ ngoài các loại mật khẩu, ip, api key"* |
| 3 | Trạng thái TRƯỚC | 7 tệp dữ liệu bị kho mã theo dõi; tệp danh mục khách hàng/NCC có **1.232 dòng × 28 cột**, **2.667 chuỗi giống số điện thoại**, cột số tài khoản ngân hàng và cột còn nợ — **đã lên máy chủ từ xa** |
| 4 | Trạng thái SAU | **3 tệp đã gỡ khỏi kho mã** (tệp vẫn còn trên máy). Sổ đăng ký nợ **7 → 4 dòng**. Kho công khai dọn sạch: **0 email thật · 0 địa chỉ IP thật**. **Lịch sử KHÔNG bị viết lại** |
| 5 | Ánh xạ bảng/cột/route | Không đụng bảng nào — thao tác ở tầng kho mã |
| 6 | Nơi đọc/ghi | `.governance/registry/pii-known-tracked.md` · `.gitignore` |
| 7 | Chuyển trạng thái | Không có |
| 8 | Mã nguồn / mốc | `c385416` · `5ba3299` |
| 9 | Bằng chứng kiểm thử | `test:pii-scan` **PASS 0 vi phạm** · `test:secret-scan` **PASS** · kiểm ngược cổng PII **3/3 đạt** |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Không áp dụng |
| 12 | Báo cáo công khai | `BAO-CAO-HOP-NHAT-MOT-LUONG-20260825.md` |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | 🔴 Tệp danh mục khách hàng **KHÔNG phải bản nháp** — Agent đã nêu **ba lần**, Owner cân nhắc và vẫn chốt giữ lịch sử. **Đây là quyết định có ý thức của Owner.** · Việc **xoá bản ghi trong CSDL** theo nguyên tắc final/draft **CHƯA thi hành** — còn thiếu tiêu chí phân định đo được + sao lưu + đối chứng hai đầu. **Chưa xoá bản ghi nào** |
| 14 | Trang Notion đề xuất cập nhật | Trang chính sách bảo mật dữ liệu · trang vòng đời dữ liệu danh mục |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ B — CÁCH LÀM VIỆC VỚI OWNER

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#155` · `#168` · `#178` |
| 2 | Nguyên nghĩa quyết định Owner | **(a)** Mọi báo cáo/trao đổi phải **hoàn toàn tiếng Việt** — *"anh không hiểu báo cáo"*. **(b)** Mọi màn hình mới phải có **bản demo cho Owner xem trước khi code**. **(c)** Được phép đẩy **báo cáo công thức nghiệp vụ** lên kho công khai |
| 3 | Trạng thái TRƯỚC | Agent báo cáo bằng tiếng Anh; không có bước demo trước khi code |
| 4 | Trạng thái SAU | Toàn bộ báo cáo từ `#155` trở đi bằng tiếng Việt. Bước demo đưa vào Plan tính giá §7 làm **nguyên tắc thi hành** |
| 5 | Ánh xạ bảng/cột/route | Không áp dụng — quy tắc làm việc |
| 6 | Nơi đọc/ghi | `docs/reports/` · `docs/PLAN-TINH-GIA.md` §7 |
| 7 | Chuyển trạng thái | Không có |
| 8 | Mã nguồn / mốc | `e853ba6` (ghi sổ `#155`) · `f25c5b9` (nguyên tắc vào Plan) |
| 9 | Bằng chứng kiểm thử | Không áp dụng |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Không áp dụng |
| 12 | Báo cáo công khai | Toàn bộ báo cáo công khai của phiên |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | Yêu cầu tiếng Việt **chưa được nâng thành luật** trong bộ 5 tệp quản trị — hiện chỉ nằm ở sổ. Nếu Owner muốn ràng buộc mọi công cụ AI thì phải ban hành luật · Nguyên tắc **demo trước khi code** hiện áp cho Plan tính giá; **chưa xác nhận** có nâng thành nguyên tắc thường trực cho mọi màn hay không |
| 14 | Trang Notion đề xuất cập nhật | Trang quy ước làm việc với Agent · trang quy trình phát triển giao diện |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ C — QUYỀN XEM GIÁ VỐN

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#163` |
| 2 | Nguyên nghĩa quyết định Owner | Giá vốn chỉ **Quản trị** và **Tổng giám đốc** được xem. *"ngoài ra không nên public cái này nhiều"* — đề xuất thêm **Kế toán** bị **từ chối** |
| 3 | Trạng thái TRƯỚC | Giá vốn nằm ở **4 cột**; chỉ **2 cột** được che. Hai cột `material_item.gia_von_trung_binh` và `pricing_quote_history.gia_von` **hở hoàn toàn** — trang vật tư nạp thẳng từ kho dữ liệu rồi đẩy xuống trình duyệt, **không qua lớp quyền nào** |
| 4 | Trạng thái SAU | **Cả 4 cột** được che. Danh sách vai trò **giữ nguyên** `ADMIN` + `CEO` |
| 5 | **Ánh xạ bảng/cột** | `bao_gia_option.gia_von` · `don_hang_item.gia_von` · `material_item.gia_von_trung_binh` · `pricing_quote_history.gia_von`. Quyền: `can_view_cost_price` trong `role_action_permission` |
| 6 | Nơi đọc/ghi | Che ở tầng máy chủ trước khi dữ liệu rời tiến trình: trang vật tư + hàm lấy chi tiết vật tư + 3 đường đọc báo giá/đơn hàng |
| 7 | Chuyển trạng thái | Không có |
| 8 | Mã nguồn / mốc | **`9667995`** · V1.00.355 (không bump) |
| 9 | Bằng chứng kiểm thử | `test:m1-rbac` **111 đạt / 0 hỏng**, thêm **8 điều kiện mới**. **Kiểm ngược bắt buộc:** gỡ bản vá → **3 hỏng**; khôi phục → **0 hỏng** ⇒ bộ kiểm **có giá trị thật** |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** — trạng thái `PUSHED / DEPLOYMENT_PENDING` |
| 11 | Bằng chứng runtime | Chỉ máy phát triển. Máy vận hành **UNVERIFIED** |
| 12 | Báo cáo công khai | `THI-HANH-QUYET-DINH-OWNER-20260825.md` |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | ⚠️ **Đính chính báo cáo trước:** bản audit đầu nói *"giá vốn không được che ở phần lớn nơi hiển thị"* — **SAI**. Cơ chế che **có hoạt động**, chỉ thiếu 2 cột · Hai bảng đó hiện **0 dòng** nên **chưa từng rò dữ liệu thật**; lỗ hổng là **tương lai**, không phải đã xảy ra · **Chưa xác minh trên máy vận hành** |
| 14 | Trang Notion đề xuất cập nhật | Trang ma trận phân quyền · trang chính sách dữ liệu nhạy cảm |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ D — MẬT KHẨU QUẢN TRỊ

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#160` |
| 2 | Nguyên nghĩa quyết định Owner | *"nếu đổi mật khẩu thì anh đã đổi rồi đó em"* ⇒ mật khẩu quản trị **ĐÃ ĐỔI** |
| 3 | Trạng thái TRƯỚC | Hai dòng nợ bảo mật chồng lấn, một dòng ghi *"cần Owner xác nhận"* |
| 4 | Trạng thái SAU | Đóng dòng nợ theo **Owner chấp nhận**, đúng điều kiện chính dòng đó đặt ra |
| 5 | Ánh xạ bảng/cột/route | Không nêu — thuộc lớp thông tin rủi ro |
| 6 | Nơi đọc/ghi | Sổ nợ kỹ thuật |
| 7 | Chuyển trạng thái | Nợ: `ĐANG XỬ LÝ` → `ĐÃ XỬ LÝ` |
| 8 | Mã nguồn / mốc | `5ba3299` |
| 9 | Bằng chứng kiểm thử | Không áp dụng |
| 10 | Bằng chứng triển khai | Không áp dụng |
| 11 | Bằng chứng runtime | Không áp dụng |
| 12 | Báo cáo công khai | `BAO-CAO-HOP-NHAT-MOT-LUONG-20260825.md` |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | 🔴 **QUAN TRỌNG — Notion đừng nhận nhầm:** lớp bằng chứng ở đây là **LỜI XÁC NHẬN CỦA OWNER**, **không phải phép đo máy**. Agent **không đo được** và **không được phép đo** giá trị mật khẩu · Xác nhận này **KHÔNG phủ khoá hạ tầng A+B** — đó là dòng nợ **khác**, đã đóng 20/08 với *Owner chấp nhận rủi ro*. **Hai thứ khác nhau, đừng gộp** · Nợ **20+ tệp kho riêng còn chứa địa chỉ IP máy chủ** vẫn **MỞ** |
| 14 | Trang Notion đề xuất cập nhật | Trang trạng thái bảo mật / phơi nhiễm thông tin rủi ro |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ E — DỮ LIỆU BẢN CUỐI ↔ BẢN NHÁP

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#158` · `#159` |
| 2 | Nguyên nghĩa quyết định Owner | **(a)** Email đi kèm **dữ liệu đã nạp** = bản cuối → **giữ**; email còn lại trong tài liệu = bản nháp → **dọn**. Email `tan@…` là **tài khoản của chính Owner** (quản trị/phát triển). **(b)** Ba tệp **biểu mẫu trống** → **GIỮ** trong kho mã |
| 3 | Trạng thái TRƯỚC | **77 địa chỉ email định danh thật** rải trên **101 tệp** bị kho mã theo dõi — cổng không thấy vì dưới ngưỡng mật độ |
| 4 | Trạng thái SAU | Che **46 chỗ trên 10 tệp**. Đo lại: **0 tệp** còn email định danh người thật. Giữ có chủ đích: tài khoản Owner · email công ty · hộp thư chức năng · chỗ trống mẫu |
| 5 | Ánh xạ bảng/cột/route | Không đụng bảng — tài liệu trong kho mã |
| 6 | Nơi đọc/ghi | 10 tệp tài liệu · `.governance/registry/pii-known-tracked.md` |
| 7 | Chuyển trạng thái | Nợ biểu mẫu: `MỞ` → `KHÔNG CÒN HỢP LỆ` |
| 8 | Mã nguồn / mốc | `5ba3299` |
| 9 | Bằng chứng kiểm thử | `test:pii-scan` **PASS** |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Không áp dụng |
| 12 | Báo cáo công khai | `BAO-CAO-HOP-NHAT-MOT-LUONG-20260825.md` |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | Ngưỡng phát hiện email của cổng **vẫn là 15** — chưa sửa theo nguyên tắc Owner chốt (*không dùng ngưỡng mật độ*). Nợ riêng · Ba tệp biểu mẫu đo cấu trúc ra **0 dữ liệu**, nhưng **chưa mở đọc giá trị ô** nên không thể tuyệt đối |
| 14 | Trang Notion đề xuất cập nhật | Trang chính sách dữ liệu · trang danh mục biểu mẫu |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ F — 🔴 NGHIỆP VỤ TÍNH GIÁ: KHỔ TRẢI *(quan trọng nhất)*

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#161` · `#162` · `#164` · `#165` · `#169` · `#170` · `#171` |
| 2 | Nguyên nghĩa quyết định Owner | **(a) Quy ước trục:** đáy hộp = **Dài × Rộng**, thân cao bằng **Cao**. **(b)** Phụ cấp phải **linh hoạt thêm/xoá**, làm **màn cấu hình để Owner tự gán**; Owner **đang nợ** bốn giá trị. **(c)** Cấu hình phải theo **nhóm sản phẩm / kiểu dáng sản phẩm** — *"bậy bạ rồi"* khi Agent đề xuất khác. **(d)** Hai kiểu dáng ưu tiên: **Túi Xách Giấy · Hộp Nắp Cài**. **(e)** Danh mục phụ cấp **nên đưa vào danh mục dùng chung** |
| 3 | Trạng thái TRƯỚC | **6 chỗ** tính khổ trải viết cứng, rải rác, không chỗ nào dẫn nguồn. **Một công thức** áp cho **40 nhóm sản phẩm** |
| 4 | Trạng thái SAU | Dồn về **một nguồn duy nhất** theo quy ước Owner chốt. Phụ cấp thành **danh sách mở**. Thêm cảnh báo khi khổ trải **không lọt tờ giấy** *(trước đó âm thầm ép về 1 con/tờ — đáp án đắt nhất)* và **thử xoay 90°** *(trước đó không bao giờ thử)* |
| 5 | **Ánh xạ bảng/cột** | `dm_nhom_universal` danh mục `nhom_san_pham` — **40 nhóm, 2 tầng**; **tầng L1 chính là kiểu dáng**. `dm_blueprint.nhom_san_pham_id` · `dm_blueprint.blueprint_json` khoá `formula_flat_size` + `constants` |
| 6 | Nơi đọc/ghi | Màn tính giá thủ công · 2 tệp vẽ hình · đường ghi `bao_gia_option.gia_von` / `thanh_tien` |
| 7 | Chuyển trạng thái | Không có |
| 8 | Mã nguồn / mốc | **`8f14dee`** (một nguồn) · **`aea01ca`** (danh sách mở) · V1.00.355 |
| 9 | Bằng chứng kiểm thử | `test:kho-trai` **56 đạt / 0 hỏng**. **Kiểm ngược:** đảo trục → **9 hỏng**; khôi phục → **0 hỏng**. Chứng minh **bằng máy** rằng hai công thức cũ **cùng cấu trúc**: đặt phụ cấp của công thức B thì ra **đúng** công thức B |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Máy phát triển. Máy vận hành **UNVERIFIED** |
| 12 | Báo cáo công khai | `THI-HANH-QUYET-DINH-OWNER-20260825.md` · `KHO-TRAI-CONG-THUC-THAT-TAN-PHAT-20260825.md` · `PHU-CAP-LINH-HOAT-VA-BA-PHAT-HIEN-20260825.md` |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | 🔴 **Notion đừng nhận đây là "đã xong":** công thức thật rút từ tài liệu Tân Phát cho thấy **mỗi kiểu dáng một công thức KHÁC NHAU VỀ CẤU TRÚC** — Túi Xách dùng `(Dài+Rộng)×2`, Nắp Cài dùng `Dài + Cao×4`. Mô hình hiện tại **chưa phủ** · Phụ cấp **phụ thuộc kích thước** (`Rộng÷2`) là chuyện **thường xuyên** — **chưa phủ** · **Hộp cứng cần 4 mảnh × 3 lớp** — **không biểu diễn nổi** · **Khổ trải TỐI ƯU** là kết quả riêng — **chưa có** · **Đơn vị cm hay mm chưa xác định** — sai đơn vị thì lệch **10 lần** · Màn cấu hình **đã tồn tại** nhưng **không nối dây** |
| 14 | Trang Notion đề xuất cập nhật | Trang nghiệp vụ tính giá · trang danh mục kiểu dáng sản phẩm · trang blueprint |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ G — 🔴 NGHIỆP VỤ IN ẤN: TỜ RỚT · LƯỢT IN · MÁY IN

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#172` · `#173` · `#174` |
| 2 | **Nguyên nghĩa định nghĩa Owner** | **TỜ RỚT:** *"số tờ in vượt chuẩn mốc 3.000 lượt / tờ đó em ví dụ 3500 tờ thì 500 tờ được gọi là tờ rớt"*. **LƯỢT IN:** *"1 lượt = số kẽm tương đương số màu x đơn giá x 1 mặt"*. **MÁY IN:** *"máy in nhỏ lớn khác nhau về đơn giá, giá kẽm tất cả mọi thứ đều khá nhau"* |
| 3 | Trạng thái TRƯỚC | Ba khái niệm **không có chỗ đứng nào** trong hệ thống |
| 4 | Trạng thái SAU | **Chỉ mới ghi sổ.** Chưa thi hành gì |
| 5 | **Ánh xạ bảng/cột** | `phieu_dieu_in` có `may_in` *(lớn/nhỏ)* · `so_mat` · `so_mau` · `kieu_in` · `loai_bai` · `so_kem` · `khuon_id` · `so_khuon` — nhưng bảng **chỉ 2 dòng** và **không nối vào đường tính giá**. **Không có bảng danh mục máy in**, **không có bảng giá theo máy** |
| 6 | Nơi đọc/ghi | Hiện chưa có nơi nào tính theo ba khái niệm này |
| 7 | Chuyển trạng thái | Không có |
| 8 | Mã nguồn / mốc | `a4d27f0` (ghi sổ) — **không có mã thi hành** |
| 9 | Bằng chứng kiểm thử | Chưa có |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Chưa có |
| 12 | Báo cáo công khai | `PLAN-TINH-GIA-20260825.md` §2 |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | 🔴 **Còn 5 câu chưa có lời đáp:** mốc **3.000** có đổi theo máy không · tờ rớt có **đơn giá riêng** không · in **7.000 tờ** tính thế nào · *"đơn giá"* là giá **một lượt** hay **một kẽm** · **tiền kẽm** và **tiền công in** là một khoản hay hai · **Định nghĩa lượt in của Owner KHÁC cách mã đang tính** — mã tính theo **số tờ × giá lượt**, Owner định nghĩa theo **số màu × đơn giá × số mặt**. **Cần Owner phân xử** |
| 14 | Trang Notion đề xuất cập nhật | Trang nghiệp vụ in offset · trang định nghĩa thuật ngữ sản xuất · trang bảng giá máy in |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ H — TỔ CHỨC CÔNG VIỆC

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#176` · `#177` (+ `#175` không áp dụng) |
| 2 | Nguyên nghĩa quyết định Owner | Tính giá phải làm thành **một chuyên mục riêng biệt**; lập **Plan lớn riêng**; **không đi nhanh** trong một phiên. Owner cảnh báo *"nó phức tạp hơn em tưởng"* |
| 3 | Trạng thái TRƯỚC | Tính giá nằm lẫn trong Plan phục hồi chung |
| 4 | Trạng thái SAU | Tách thành workstream riêng, có Plan riêng **6 chặng T0→T6**. Đang ở **T0** |
| 5 | Ánh xạ bảng/cột/route | Không áp dụng |
| 6 | Nơi đọc/ghi | `docs/PLAN-TINH-GIA.md` · `docs/PLAN-OF-RECORD.md` §2b |
| 7 | Chuyển trạng thái | Chặng: `—` → `T0` |
| 8 | Mã nguồn / mốc | **`f25c5b9`** |
| 9 | Bằng chứng kiểm thử | `test:gov-gates` **18/18 PASS** |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Không áp dụng |
| 12 | Báo cáo công khai | `PLAN-TINH-GIA-20260825.md` |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | Hai Plan cùng `ACTIVE` — Agent khai là **hợp lệ vì khác workstream**; nếu Owner hoặc Notion thấy khác thì **phải phân xử**, không bên nào tự thắng · Chặng T1–T6 **chặn bởi 10 câu nghiệp vụ** chưa có lời đáp |
| 14 | Trang Notion đề xuất cập nhật | Trang kế hoạch dự án · trang lộ trình tính giá |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CHỦ ĐỀ I — YÊU CẦU SẢN PHẨM MỚI: QUẢN LÝ TÀI KHOẢN

| # | Trường | Nội dung |
|---|---|---|
| 1 | Mã sổ | `#167` |
| 2 | Nguyên nghĩa yêu cầu Owner | Xây **giao diện quản lý tài khoản bài bản**, **kể cả phân quyền**; và **trang cho người dùng tự quản lý tài khoản của mình** — được **đổi mật khẩu · thay ảnh đại diện · đặt bí danh**; **KHÔNG** được sửa **tên · email · tên đăng nhập** vì ảnh hưởng quản lý chung. Owner khai *"đã nhắc đi nhắc lại nhiều lần"* |
| 3 | Trạng thái TRƯỚC | Chưa đo |
| 4 | Trạng thái SAU | **Chỉ mới ghi sổ.** Chưa bắt đầu |
| 5 | Ánh xạ bảng/cột/route | Chưa audit. Liên quan: `dm_vai_tro` (9 vai trò) · `role_action_permission` · `role_menu_permission` · `user_role_mapping` · màn phân quyền hiện tại |
| 6 | Nơi đọc/ghi | Chưa audit |
| 7 | Chuyển trạng thái | Chưa audit |
| 8 | Mã nguồn / mốc | `a4d27f0` (ghi sổ) — **không có mã thi hành** |
| 9 | Bằng chứng kiểm thử | Chưa có |
| 10 | Bằng chứng triển khai | **NOT_DEPLOYED** |
| 11 | Bằng chứng runtime | Chưa có |
| 12 | Báo cáo công khai | `PHU-CAP-LINH-HOAT-VA-BA-PHAT-HIEN-20260825.md` phần C |
| 13 | **ĐIỀU CHƯA CHỨNG MINH ĐƯỢC** | 🔴 **Bối cảnh Agent khai trung thực:** yêu cầu này đến khi Agent hỏi về **một việc khác** (nút xoá trên màn cấu hình khổ trải). Owner mở đầu bằng *"vấn đề về quản lý tài khoản ah em?"* ⇒ **hai bên đang nói hai việc khác nhau**. Agent **không** coi đây là câu trả lời cho câu hỏi kia · **Có thể chồng lấn** với việc *"người dùng tự đổi mật khẩu"* đã làm ở một đợt phát hành trước, và với màn phân quyền hiện tại mà Owner từng gọi là *"rối mù"*. **Phải đo trước khi tách phạm vi** |
| 14 | Trang Notion đề xuất cập nhật | Trang yêu cầu sản phẩm · trang phân quyền · trang hồ sơ người dùng |
| 15 | Trạng thái đồng bộ đề xuất | `OWNER_APPROVED_PENDING_NOTION_SYNC` |

---

## CUỐI — BA ĐIỀU AGENT NOTION CẦN BIẾT TRƯỚC KHI ĐỐI CHIẾU

1. **Không mục nào ở trạng thái `SYNCED_TO_NOTION`.** Agent IDE **không được phép** ghi trạng thái đó. Sau khi TanPhatAI cập nhật xong và xác nhận, **TanPhatAI** mới đặt trạng thái đó.

2. **Trường số 13 của mỗi chủ đề là quan trọng nhất.** Nó liệt kê **điều CHƯA chứng minh được**, để Notion **không nhận nhầm** thành sự thật hiện hành. Đặc biệt: chủ đề **D** *(mật khẩu — bằng chứng là lời Owner, không phải phép đo)* và chủ đề **F** *(khổ trải — đã làm một phần, còn thiếu nhiều)*.

3. **Nếu Notion và gói này nói khác nhau về cùng một điều** ⇒ đặt `SYNC_CONFLICT`, **DỪNG, báo Owner**. Không bên nào tự thắng — đây là quy định của chính bộ luật dự án.
