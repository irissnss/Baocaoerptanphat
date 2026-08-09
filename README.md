# 📊 Báo Cáo ERP Tân Phát — Tổng Quan & Changelog

> **Repo công khai** để Notion và các công cụ AI có thể đọc, hỗ trợ Owner quản lý dự án ERP Tân Phát.
>
> ⚠️ **CHỈ chứa báo cáo** — KHÔNG chứa source code, schema, credentials, hay dữ liệu nhạy cảm.

---

## 📌 Thông Tin Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Tên dự án** | ERP Tân Phát (Tân Phát Packaging) |
| **Phiên bản mã nguồn** | `V0.333` |
| **Phiên bản đang chạy thật** | `V0.333` ✅ (đưa lên vận hành 25/07/2026) — ⚠️ các đợt cải tiến **sau** 25/07/2026 hiện **chỉ chạy trên máy nội bộ, chưa phát hành** |
| **Mốc phiên bản hiện tại** | V0.333 |
| **Ngày bắt đầu** | 18/01/2026 |
| **Phát hành lên vận hành thật** | 24/07/2026 — Đợt V0.326–V0.333 (gần nhất) · 23/07/2026 — Đợt R1/R1.1/R1.2 |
| **Cập nhật báo cáo này** | 09/08/2026 |
| **Tech Stack** | Next.js 16.1.6 · React 19.2.4 · Tailwind 4.2.1 · TypeScript 5.9.3 · MariaDB 10.11 (production) · MySQL 8.4 (local development) |
| **Architecture** | Server Actions + Server Components + SSE |
| **UI Framework** | Metronic (Demo 1 backbone) |
| **Tổng modules** | 11 modules (M0–M9, MC, MF) |
| **Tổng bảng DB** | 99 bảng (đã kiểm đếm trên môi trường vận hành 23/07/2026) |
| **Trạng thái** | Bản V0.333 đã go-live · công việc nội bộ sau đó **chưa triển khai** (chi tiết hạ tầng không công khai) |

---

## 📋 Trạng Thái Modules (cập nhật 09/08/2026)

| Module | Tên | Status | Mô tả chức năng | Sub-routes |
|--------|-----|--------|------------------|------------|
| M0 | Hệ Thống | ✅ Ready | Danh mục chung, phòng ban, quy trình, auth/session, RBAC | 6 sub-routes |
| M1 | Danh Mục | ✅ Ready *(bản đã vận hành)* · 🧪 Local UAT hoàn tất foundation / **chờ triển khai** | Khách hàng, sản phẩm, vật tư, nhân sự, vị trí, bảng giá công đoạn | 10 sub-routes |
| M3 | CRM & Bán Hàng | 🔨 In Dev | Báo giá, đơn hàng, CRM, tính giá thủ công/tự động, pricing admin | 7 sub-routes |
| M4 | Sản Xuất | 🔨 In Dev | Lệnh sản xuất, phiếu điều in, gộp/tách LSX | 2 sub-routes |
| M5 | Kho Hàng | 🔨 In Dev | NCC, mua hàng, nhập/xuất kho, giao hàng, kiểm kê, kho thành phẩm — CRUD đầy đủ + RBAC + realtime; còn thiếu tự động hoá tồn kho | 10 sub-routes |
| M6 | Vận Hành HR | 🔨 In Dev | Chấm công, nghỉ phép, biến động nhân sự | 1 client module |
| M7 | Tiền Lương | 🔨 In Dev | Payroll, BHXH, TNCN lũy tiến, phiếu chi lương | 1 client module |
| M8 | Công Việc | 🔨 In Dev | Task management, tích hợp CRM M3-M8 | 1 sub-route (tasks) |
| M9 | Cổng Thông Tin | 📋 Planned | Customer portal, multi-session (kế thừa auth M0) | Placeholder |
| MC | Quản Lý Hợp Đồng | 🔨 In Dev | Quản lý hợp đồng (mẫu, chi tiết, workflow trạng thái) | 1 client module |
| MF | Tài Chính | 🔨 In Dev | Phiếu thu/chi, công nợ, ngân hàng, đối chiếu, nghiệm thu | 7 sub-routes |

### Tổng kết trạng thái:
- **✅ Ready (đã vận hành):** 2 modules (M0, M1) — *riêng M1 có nền phân quyền mới hoàn tất kiểm thử nội bộ, **chưa** triển khai*
- **🔨 In Dev (đang phát triển):** 8 modules (M3, M4, M5, M6, M7, M8, MC, MF)
- **📋 Planned:** 1 module (M9)

> 📂 Xem chi tiết tiến độ từng module tại [MODULE-PROGRESS.md](MODULE-PROGRESS.md)
>
> ✅ **M1 — Chốt các quyết định cuối của chủ dự án, hoàn tất kiểm thử nội bộ (01/08/2026).**
> **Làm rõ vai trò:** Kế toán nay **tra cứu được toàn bộ danh bạ khách hàng ở mức chỉ đọc** (để liên hệ, làm hợp đồng, đối soát) nhưng **không sửa, không xoá, không chuyển khách sang người khác, không xem giá vốn hay lợi nhuận**. Nhân sự kinh doanh giữ phạm vi hẹp: chỉ khách và mặt hàng của khách mình phụ trách, **không vào kho vật tư chung, không xem giá vốn**. Cũng đã **tách hai quyền vốn bị dính nhau**: "được xem toàn bộ khách" trước đây tự động kéo theo "được chuyển khách cho người khác" — nay là hai quyền riêng biệt.
> **Dọn dữ liệu mẫu an toàn:** một mặt hàng mẫu **không dính chứng từ nào đã được xoá**; mặt hàng còn lại **giữ nguyên** vì đang nằm trong **chứng từ của khách hàng thật** — xoá sẽ làm hỏng chứng từ đó. Trước khi xoá đã **sao lưu toàn bộ dữ liệu, kiểm tra mã toàn vẹn và chuẩn bị kịch bản khôi phục**; sau khi xoá, toàn bộ chứng từ và danh sách khách hàng **giữ nguyên**.
> **Danh mục công đoạn:** phần lớn công đoạn chưa có đơn giá nay được đánh dấu rõ **"chưa cấu hình giá"**. Hệ thống **không tự bịa giá, không dùng số 0 thay cho giá trống, không lấy giá của công đoạn khác** — nơi nào cần giá mà chưa có thì báo rõ. Việc này **không cản trở** phần danh mục hoàn tất; chủ dự án bổ sung đơn giá thật dần.
> **Chỉ chạy trên máy nội bộ — chưa đưa lên vận hành.** Không đổi cấu trúc dữ liệu.
>
> ⏳ **Còn lại đúng một việc:** xác nhận mặt hàng mẫu còn lại — nếu các chứng từ liên quan cũng là dữ liệu thử thì dọn nốt, nếu là chứng từ thật thì giữ nguyên như hiện tại.

> 🧪 **M1 — Kiểm thử chi tiết từng nghiệp vụ danh mục: hoàn tất nội bộ (01/08/2026).** Lượt này đi sâu vào **nghiệp vụ từng màn hình**, không chỉ phân quyền. Phát hiện và sửa **4 lỗi thuộc loại âm thầm làm hỏng dữ liệu** (không báo lỗi nên rất khó thấy khi dùng tay):
> **(1)** Tạo khách hàng mà không chọn người phụ trách thì khách bị gán cứng cho một nhân viên mặc định — người tạo có thể **mất luôn quyền xem chính khách mình vừa tạo**; màn tạo cũng tin dữ liệu từ trình duyệt nên có thể gán khách cho người khác, lách quyền chuyển giao. Nay máy chủ tự quyết định người phụ trách.
> **(2)** Xoá mặt hàng/vật tư **không kiểm tra chúng có đang nằm trong chứng từ nào không** — xoá xong các chứng từ cũ trỏ vào khoảng không. Nay chặn và nêu rõ đang bị chứng từ nào tham chiếu.
> **(3)** Cho nhân viên nghỉ việc **làm "rơi" toàn bộ khách hàng họ đang phụ trách** — không ai trong đội kinh doanh còn nhìn thấy. Nay bắt buộc **bàn giao trước khi cho nghỉ**.
> **(4)** Xoá công đoạn đang có bảng giá chỉ báo lỗi kỹ thuật khó hiểu — nay báo rõ bằng tiếng Việt.
>
> Đã kiểm thật **6 vai trò người dùng trên toàn bộ màn hình danh mục**, chạy trên **bản dựng thật** (không phải bản chạy thử): mỗi vai trò chỉ thấy đúng phần dữ liệu của mình; mở thẳng đường dẫn của khách người khác đều bị chặn và **không lộ tên, mã số thuế hay hạn mức công nợ**. Cũng đã xác định **ranh giới bàn giao dữ liệu** sang các nhóm chức năng tiếp theo để làm tuần tự về sau. **Chỉ chạy trên máy nội bộ — chưa đưa lên vận hành. Không đổi cấu trúc dữ liệu, không thay đổi dữ liệu nghiệp vụ.**
>
> ⏳ **Cần chủ dự án quyết:** xử lý hai mặt hàng rác dữ liệu mẫu · duyệt gói quyền mặc định · cung cấp đơn giá cho phần lớn công đoạn hiện chưa có bảng giá (**không tự bịa giá**).

> 🔐 **M1 — Nền phân quyền linh hoạt: hoàn tất kiểm thử nội bộ (01/08/2026).** Mỗi mục trong nhóm Danh Mục nay có **quyền truy cập riêng** thay vì dùng chung một quyền tổng, nên có thể giao quyền đúng theo từng vị trí công việc. Quyền được tách thành nhiều tầng **độc lập**: vào được khu vực, xem, thêm, sửa, duyệt và xem thông tin nhạy cảm là các quyền khác nhau — cấp quyền này **không** tự mở quyền kia. Phạm vi công nợ được chỉnh theo đúng nghiệp vụ tài chính: kế toán theo dõi được **cả công nợ khách hàng lẫn công nợ đối tác/nhà cung cấp**, trong khi kinh doanh chỉ thấy phần khách mình phụ trách. Tài khoản chưa gắn hồ sơ nhân sự **không** xem được dữ liệu nghiệp vụ. Đã kiểm thử thật với **6 vai trò người dùng** trên toàn bộ khu vực của nhóm Danh Mục và khu vực Tài chính. **Chỉ chạy trên máy nội bộ — chưa đưa lên vận hành. Không đổi cấu trúc dữ liệu, không thay đổi dữ liệu nghiệp vụ.**
>
> ⏳ **Còn tồn (cần chủ dự án chọn hướng xử lý):** hai mặt hàng trong dữ liệu nội bộ đang gắn với một mã khách hàng không có trong danh mục. Truy vết kho lưu trữ đã **tìm ra nguyên nhân gốc**: mã đó thuộc về một **khách hàng giả trong bộ dữ liệu mẫu** thời kỳ đầu; khi chuyển sang cơ sở dữ liệu thật, các khách hàng thật được tạo còn khách mẫu này thì không, nhưng hai mặt hàng của nó vẫn được nhập vào. Nói cách khác đây là **rác dữ liệu mẫu, không phải hàng của khách thật** — nên **không gán sang khách nào cả**, vì làm vậy là tạo ra một quan hệ mua bán chưa từng có. Hai mặt hàng vẫn bị chặn khỏi giao dịch mới; chờ chủ dự án chọn xoá hẳn hay giữ nguyên.

> 🔐 **M1 Dữ liệu nền — khóa hợp đồng nghiệp vụ & phạm vi dữ liệu (30/07/2026):** đã bịt các lỗ hổng cho phép nhìn dữ liệu khách hàng **ngoài phạm vi được giao**, thống nhất **một quy tắc phân quyền dùng chung** cho Báo giá/CRM/Đơn hàng/Giao hàng/Công nợ, bảo toàn dữ liệu cấu hình cũ (không tự chuẩn hóa), và **kiểm thử thật với 5 vai người dùng**. Chỉ chạy trên máy nội bộ — **chưa** đưa lên vận hành, **không** đổi cấu trúc dữ liệu. Chi tiết: [AUDIT-M1-OWNER-CONTRACT-20260730.md](AUDIT-M1-OWNER-CONTRACT-20260730.md).

> 📦 **M5 Kho Hàng — đính chính (25/07/2026): KHÔNG phải "skeleton".** 10 khu vực đã có CRUD đầy đủ + phân quyền (RBAC) + realtime; khoảng trống còn lại là **tự động hoá tồn kho** (chưa tự ghi sổ giao dịch kho + chưa tự cập nhật tồn khi nhập/xuất/giao). Chi tiết: [AUDIT-M5-WAREHOUSE-20260725.md](AUDIT-M5-WAREHOUSE-20260725.md).
>
> 🚀 **MỚI NHẤT: V0.333 — Rà soát và sửa tràn ngang cho toàn bộ màn hình trên điện thoại (25/07/2026). ĐÃ ĐƯA LÊN VẬN HÀNH THẬT.**
>
> 🚀 **V0.332 — Tối ưu giao diện trên điện thoại và máy tính bảng, cả chiều ngang lẫn dọc (25/07/2026). ĐÃ ĐƯA LÊN VẬN HÀNH THẬT.**
>
> 🚀 **V0.330–V0.331 — Áp đồng loạt bo góc chuẩn cho toàn bộ màn hình (24/07/2026). ĐÃ ĐƯA LÊN VẬN HÀNH THẬT.**
>
> 🚀 **V0.329 — Chuẩn hoá không gian làm việc và bo góc theo một khuôn mẫu duy nhất (24/07/2026). ĐÃ ĐƯA LÊN VẬN HÀNH THẬT.**
>
> 🚀 **V0.328 — Sửa biểu tượng tiêu đề bị ẩn, chuẩn hoá màu sắc, trợ lý tạo khách hàng vào khung chung (24/07/2026). ĐÃ ĐƯA LÊN VẬN HÀNH THẬT.**
>
> 🚀 **V0.327 — Chuẩn hoá trang tính giá vào khung giao diện chung (24/07/2026). ĐÃ ĐƯA LÊN VẬN HÀNH THẬT**, không gián đoạn dịch vụ, dữ liệu nguyên vẹn.
>
> 🏆 **V0.326 — Menu chuyển trang mượt, khung giao diện dùng chung, tăng tốc máy phát triển (24/07/2026).** Phát hành cùng đợt với V0.327.
>
> 🚀 **Bản phát hành chức năng gần nhất: V0.323 · 23/07/2026 — Đợt R1/R1.1/R1.2.** Gia cố an toàn hiển thị nội dung HTML và bản in; sửa lỗi **không tạo được nhân viên**; khoá **mã Phòng Ban thành bất biến**; sửa lỗi **ô chọn Khuôn / Kiểu In hiện trống** trên form in; chuẩn hoá cơ cấu tổ chức lên **6 phòng ban**; kiểm chứng khả năng khôi phục dữ liệu bằng cách phục hồi thật vào môi trường tách biệt. Đã đưa lên vận hành thật, **không gián đoạn dịch vụ**. Chi tiết ở mục Changelog bên dưới.
>
> 🗂️ *Các mục 🏆/🔐/✅ bên dưới là báo cáo LỊCH SỬ (V0.218–V0.227) — lưu tham khảo, không phải bản mới nhất.*
>
> 🔐 [REAL-USER-PILOT-V0227-BATCH1-CEO.md](REAL-USER-PILOT-V0227-BATCH1-CEO.md) — Batch 1 CEO applied + smoke test
>
> ✅ [REAL-USER-PILOT-V0227-BATCH0-VERIFY.md](REAL-USER-PILOT-V0227-BATCH0-VERIFY.md) — Batch 0 admin verify PASS
>
> 🛡️ [REAL-USER-PILOT-V0226B-FINAL-GUARD.md](REAL-USER-PILOT-V0226B-FINAL-GUARD.md) — Final guard: batch-scoped rollback
>
> 🛡️ [REAL-USER-PILOT-V0226A-APPLY-HARDENING.md](REAL-USER-PILOT-V0226A-APPLY-HARDENING.md) — Apply plan hardened: rollback safe, password flow safe
>
> 📦 [REAL-USER-PILOT-V0226-APPLY-PLAN.md](REAL-USER-PILOT-V0226-APPLY-PLAN.md) — V0.226 Safe Apply Plan (5 batches, hardened V0.226B)
>
> 📋 [REAL-USER-PILOT-UNIQUE-EMPLOYEE-MASTER-V0225E.md](REAL-USER-PILOT-UNIQUE-EMPLOYEE-MASTER-V0225E.md) — 10 unique employees, shared email blocked
>
> 🔒 [RBAC-V0223-EARLY-STAFF-PILOT-GATE.md](RBAC-V0223-EARLY-STAFF-PILOT-GATE.md) — Early Staff Pilot Gate: 135/303 guarded, 10/42 files full
>
> 🔐 [RBAC-P1-GAP-CLOSEOUT-20260627.md](RBAC-P1-GAP-CLOSEOUT-20260627.md) — P1 Gap Closeout: MF/M6/M7 guarded
>
> 📋 [P01-RECONCILIATION-CLOSEOUT-V0218.md](P01-RECONCILIATION-CLOSEOUT-V0218.md) — Timeline + Employee Safety + Final Decision
>
> ✅ [P01-FINAL-SMOKE-TEST-V0218.md](P01-FINAL-SMOKE-TEST-V0218.md) — 15/15 PASS Browser + API + DB
>
> 🔧 [P01-FLEXIBLE-FOUNDATION-V0218.md](P01-FLEXIBLE-FOUNDATION-V0218.md) — 29 Guards, Architecture
>
> 🔒 [P01-SAFETY-VERIFICATION-V0218.md](P01-SAFETY-VERIFICATION-V0218.md) — Safety Report
>
> 📋 [GOLIVE-PLAN.md](GOLIVE-PLAN.md) — Kế hoạch Go-Live tổng quan

---

### V0.333 (09/08/2026) — Dời thư mục làm việc: nối lại toàn bộ liên kết + chặn tái diễn · CHƯA TRIỂN KHAI

> ℹ️ **Chỉ ảnh hưởng máy làm việc nội bộ.** Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật,
> không đổi phân quyền, không phát hành phiên bản mới, **không đụng máy chủ vận hành**.

**🔍 Chuyện gì xảy ra:** chủ dự án gom toàn bộ tài sản dự án vào một thư mục chung cho gọn.
Các tệp được chuyển **nguyên vẹn**, nhưng **bốn thứ không nằm trong thư mục đó thì đứt liên kết**:

| Thứ bị đứt | Hệ quả thực tế |
|---|---|
| Lịch sử trao đổi & bộ nhớ làm việc của trợ lý AI | Không tra cứu lại được việc đã làm |
| Hệ thống quản lý phiên bản mã nguồn | **Chặn hoàn toàn** mọi thao tác lưu/đẩy mã |
| Bộ nhớ đệm dựng ứng dụng | Trỏ về vị trí cũ không còn tồn tại |
| Các đoạn tự động hoá có ghi cứng đường dẫn | Chạy sai chỗ hoặc báo "không tìm thấy tệp" |

**🛠️ Đã khôi phục — có đối chiếu mã toàn vẹn từng tệp**

| Hạng mục | Kết quả |
|---|---|
| Lịch sử trao đổi & bộ nhớ làm việc | **274/274 tệp khớp mã toàn vẹn** · 0 thiếu · 0 sai |
| Hệ thống quản lý phiên bản | Hoạt động lại bình thường, đúng kho mã |
| Bộ nhớ đệm dựng ứng dụng | Đã dọn, tự sinh lại sạch |
| Ứng dụng chạy thử | Khởi động **1,5 giây**, mở được màn hình đăng nhập, **0 lỗi** |
| Cơ sở dữ liệu nội bộ | Kết nối tốt, **đầy đủ 99 bảng** |
| Dựng bản phát hành thử | **Thành công** |

> 🔐 **Bản cũ được giữ nguyên làm đường lùi** — chỉ sao chép sang chỗ mới, không xoá bản gốc.

**🧱 Chặn tái diễn (quan trọng nhất):** đây là **lần thứ 2** thư mục gốc bị dời chỗ. Nguyên nhân
gốc không phải "quên đường dẫn mới", mà là **thói quen ghi cứng đường dẫn** vào các đoạn tự động
hoá. Đã bổ sung một **quy tắc nội bộ bắt buộc**: cấm ghi cứng đường dẫn gốc, thay bằng cách **tự
xác định vị trí lúc chạy**; kèm **quy trình 8 bước** phải làm mỗi khi dời thư mục (trong đó có
bước chuyển lịch sử làm việc — bước dễ quên nhất). Có thêm **cổng kiểm tra tự động** chặn ngay
nếu ai đó lỡ ghi cứng đường dẫn trở lại.

**🐞 Lỗi tiềm ẩn phát hiện thêm:** một công cụ kiểm tra phân quyền nội bộ **luôn báo "không tìm
thấy tệp"** do dò sai vị trí — lỗi có sẵn từ trước, nay mới lộ ra và đã sửa.

**📚 Tài liệu được cập nhật cho khớp thực tế:** đối chiếu tài liệu nội bộ với hệ thống đang chạy,
phát hiện tài liệu **lạc hậu nhiều bậc** so với thực tế và đã sửa lại:

| Tài liệu ghi (sai) | Thực tế đang chạy |
|---|---|
| Next.js 14 | **Next.js 16.1.6** |
| React 18 | **React 19.2.4** |
| Tailwind 3.4 | **Tailwind 4.2.1** |
| Cả nội bộ và vận hành đều là "MySQL 8.0" | Nội bộ **MySQL 8.4**, vận hành **MariaDB 10.11** |
| "Dựng bản phát hành bị lỗi" | **Dựng thành công** |

> ⚠️ **Điểm đáng lưu ý nhất:** máy nội bộ và máy vận hành **dùng hai hệ quản trị cơ sở dữ liệu
> khác nhau**. Tài liệu trước đây ghi cả hai là một, dễ dẫn tới viết câu lệnh chạy được ở nội bộ
> nhưng **hỏng khi lên vận hành**. Đã thêm cảnh báo bắt buộc kèm quy tắc kiểm tra tương thích.

**⏳ Còn lại chờ chủ dự án:** quyết định có xoá bản lưu lịch sử cũ (đang giữ làm đường lùi) và
xoá một tệp báo cáo nháp bỏ dở bị trùng nội dung hay không.

---

### V0.333 (09/08/2026) — Siết bảo mật máy chủ: thu hẹp truy cập dịch vụ nội bộ · ĐÃ ÁP DỤNG TRÊN MÁY CHỦ

> ✅ **Đã áp dụng trên môi trường vận hành thật ngày 09/08/2026. Không gián đoạn dịch vụ.**
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền, không phát hành phiên bản mới.

**🔍 Vấn đề:** một dịch vụ nội bộ của hệ thống đang cho phép kết nối từ Internet trong khi lẽ ra
chỉ nên dùng nội bộ. Nhật ký ghi nhận **306 lần thử truy cập trái phép** từ bên ngoài
(**không có lần nào thành công**).

**🛠️ Đã xử lý:** thu hẹp phạm vi truy cập — chỉ cho phép kết nối **từ bên trong chính máy chủ**,
đúng đường mà ứng dụng đang dùng. Chọn cách can thiệp ở lớp lọc mạng thay vì đổi cấu hình dịch vụ,
để **không phải khởi động lại** và **không gây gián đoạn**.

**📊 Kết quả đo trước / sau (đo từ bên ngoài Internet)**

| Hạng mục | Trước | Sau |
|---|---|---|
| Dịch vụ nội bộ tiếp cận từ Internet | **Có** | **Đã chặn** |
| Website | Hoạt động | **Hoạt động bình thường** |
| Kênh quản trị & kênh web | Mở | **Vẫn mở đúng như cần** |
| Ứng dụng | Đang chạy | **Chạy liên tục 15 ngày, 0 lần khởi động lại** |
| Cơ sở dữ liệu | Bình thường | **Bình thường, đầy đủ dữ liệu** |
| Số lần thử truy cập trái phép | Tăng liên tục | **Dừng hẳn, không tăng thêm** |

**🔁 Sống qua khởi động lại máy:** máy chủ vốn không có sẵn cơ chế ghi nhớ cấu hình lọc mạng, nên
đã thiết lập một tiến trình tự khôi phục khi máy khởi động, và cho chạy **sau** phần mềm quản trị
máy chủ để tránh bị ghi đè. Đã kiểm chứng chạy lại nhiều lần **không sinh cấu hình trùng lặp**.

**🔙 Đường lùi:** đã sao lưu toàn bộ cấu hình lọc mạng **trước khi sửa**; có thể khôi phục nguyên
trạng bất cứ lúc nào.

**⏸️ Còn một lớp bảo vệ nữa chờ quyết định:** có thể siết thêm ở tầng cấu hình dịch vụ để phòng
trường hợp lớp lọc mạng bị xoá. Việc này cần khởi động lại dịch vụ (gián đoạn vài giây) nên
**chưa thực hiện**, chờ chủ sở hữu duyệt. Mức bảo vệ hiện tại đã đủ chặn.

---

### V0.333 (09/08/2026) — Phục hồi tệp & hoàn tất kiểm thử sau sự cố · CHƯA TRIỂN KHAI

> 🧪 **Kiểm thử nội bộ — chưa đưa lên môi trường vận hành thật.**
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền, không triển khai.

**🔧 Đã làm — khắc phục dứt điểm sự cố mất tệp**

| Việc | Kết quả |
|---|---|
| Phục hồi tệp từ khu vực lưu tạm | **464 / 464 tệp** đã về đúng chỗ, **không thiếu tệp nào** |
| Thư viện giao diện có bản quyền | Đã đầy đủ trở lại |
| Mã nguồn quản lý phiên bản | Không bị ảnh hưởng bởi thao tác phục hồi |
| Kiểm thử tự động phần Danh Mục | **Hoàn tất — tổng 753 điểm đạt / 0 lỗi thật** |

Sau khi phục hồi và bật lại cơ sở dữ liệu nội bộ, đã chạy nốt tám bộ kiểm thử còn thiếu —
tất cả đều đạt. Phân hệ Danh Mục được kiểm chứng lại đầy đủ sau sự cố, không phát sinh lỗi mới.

**⏸️ Một việc bảo mật đang chờ chủ sở hữu quyết định**

Ghi nhận một cấu hình dịch vụ nội bộ trên máy chủ đang để mở rộng hơn mức cần thiết, và đã có
truy cập lạ từ bên ngoài dò vào (chưa có trường hợp nào truy cập thành công). Việc thu hẹp lại
**không ảnh hưởng** đến hoạt động của ứng dụng. Đã ghi nhận và **chưa tự ý sửa** vì thuộc quyền
quyết định của chủ sở hữu.

**🛡️ Chống tái diễn:** đã thiết lập quy tắc bảo vệ để công cụ dọn ổ đĩa và công cụ hỗ trợ không
được thao tác vào thư mục dự án — nguyên nhân gốc của sự cố lần này.

---

### V0.333 (09/08/2026) — Rà soát tổng lực tính toàn vẹn sau sự cố mất tệp · CHƯA TRIỂN KHAI

> 🧪 **Kiểm thử nội bộ — chưa đưa lên môi trường vận hành thật.**
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền, không triển khai.

**🔍 Bối cảnh:** ngày 02/08 một số tệp trên máy phát triển biến mất. Đợt này rà soát toàn
diện để chắc chắn không còn thiếu sót nào trước khi làm tiếp.

**🔬 Cách kiểm:** sáu hướng rà soát chạy song song, và **mỗi phát hiện đều bị một bên độc
lập cố tình phản biện để bác bỏ** trước khi được ghi nhận. Kết quả: 27 nghi vấn ban đầu →
**10 xác nhận có thật, 17 bị bác bỏ**. Cách làm này tránh cả hai lỗi: báo động giả và bỏ sót.

**🔁 Đính chính chẩn đoán ban đầu**

Kết luận ban đầu cho rằng phần mềm diệt virus đã cách ly tệp. **Điều này sai.** Kiểm tra cho
thấy phần mềm diệt virus **đang tắt**, và các tệp **không bị xoá hẳn mà nằm trong Thùng Rác**.
Dấu vết thời gian gom trong 32 phút chiều 02/08, kèm bộ lọc chỉ xoá tệp lớn — đặc điểm của
**công cụ dò và xoá tệp trùng lặp**, không phải phần mềm diệt virus.

**✅ Kết quả — phần lõi nguyên vẹn 100%**

| Hạng mục | Kết quả |
|---|---|
| Mã nguồn quản lý bởi hệ thống phiên bản | **Không thiếu tệp nào**; đã băm lại toàn bộ và đối chiếu, khớp tuyệt đối |
| Bộ tệp quy tắc vận hành | Giống hệt nhau; số đề mục qua 12 phiên bản **chỉ tăng, chưa lần nào giảm** |
| Thư viện phần mềm | Quét toàn bộ, **không gói nào hỏng** |
| Cấu hình và tài sản không được quản lý phiên bản | Còn đủ, đối chiếu khớp với số liệu ghi nhận trước đó |
| Kho sao lưu | Nguyên vẹn, mã kiểm chứng khớp |
| Máy chủ vận hành thật | Ứng dụng chạy liên tục 14 ngày, **0 lần khởi động lại**; phiên bản khớp cả ba nơi |
| Kiểm thử tự động | **327 điểm đạt / 0 lỗi thật** |

**⚠️ Hai việc cần xử lý**

1. Còn một số tệp thư viện giao diện và công cụ hỗ trợ **chỉ còn tồn tại trong Thùng Rác** —
   phục hồi được, nhưng phải làm trước khi dọn Thùng Rác.
2. Phát hiện một **rủi ro bảo mật trên máy chủ**: một cổng dịch vụ nội bộ đang để mở ra
   Internet trong khi tường lửa chưa bật, và đã có truy cập lạ từ bên ngoài dò vào. Đã ghi
   nhận và **chưa tự ý sửa** vì cần chủ sở hữu duyệt. Việc đóng cổng **không ảnh hưởng** hoạt
   động của ứng dụng.

**📋 Việc mức thấp:** công cụ kiểm tra đồng bộ bộ tệp quy tắc có một điểm mù cần siết lại;
một bài kiểm thử báo lỗi giả do tự quét chính nó; và dọn bớt bộ nhớ đệm tạm trên máy phát triển.

---

### V0.333 (08/08/2026) — Rà soát tổng lực sau sự cố mất tệp · TÌM RA NGUYÊN NHÂN THẬT

> 🧪 **Kiểm thử nội bộ — không đưa lên môi trường vận hành thật.**
> Toàn bộ đợt rà soát là **chỉ đọc**: không sửa mã, không đổi dữ liệu, không đổi phân quyền,
> không triển khai. Môi trường vận hành thật **không bị ảnh hưởng**.

**🔍 Bối cảnh:** ngày 02/08/2026 một số tệp trên máy phát triển biến mất. Đợt trước đã khôi
phục phần trong hệ thống quản lý mã nguồn, nhưng chưa xác định được nguyên nhân gốc. Lần này
rà soát tổng lực bằng nhiều hướng độc lập, mỗi phát hiện đều bị một bên thứ hai phản biện để
loại trừ báo động giả.

**⚠️ Đính chính — chẩn đoán trước đó SAI**

Đợt trước kết luận nguyên nhân là phần mềm diệt vi-rút. **Kết luận đó sai.** Thủ phạm thật là
một **công cụ dọn tệp trùng lặp** quét toàn ổ đĩa, thấy tệp nào giống hệt nhau thì xoá bớt bản
"thừa" **vào Thùng Rác**. Bằng chứng loại trừ vi-rút: phần mềm diệt vi-rút của hệ điều hành
đang **tắt hoàn toàn**; nhật ký gần nhất cách sự cố 6 tuần; và phần mềm diệt vi-rút **không bao
giờ** xoá vào Thùng Rác mà chuyển vào kho cách ly riêng.

Điểm đáng suy nghĩ: **chính thiết kế đúng đắn đã khiến tệp bị xoá.** Năm tệp quy tắc của dự án
được yêu cầu phải giống hệt nhau từng ký tự — công cụ dọn tệp trùng nhìn thấy vậy liền coi bốn
tệp là bản thừa. Cả năm bị xoá trong cùng một giây.

**⏰ Việc cấp bách có hạn chót**

Phần lớn tệp vẫn **cứu được nguyên vẹn** từ Thùng Rác, đã kiểm chứng bằng mã băm và khớp tuyệt
đối. Nhưng hệ điều hành đang bật chế độ tự dọn Thùng Rác sau 30 ngày ⇒ nếu không khôi phục kịp
sẽ mất vĩnh viễn. Đang chờ chủ sở hữu duyệt việc khôi phục.

**✅ Tin tốt — mã nguồn và kho quản lý phiên bản lành tuyệt đối**

- Băm lại **toàn bộ tệp đang được quản lý** rồi đối chiếu: **chỉ 1 tệp khác biệt**, và đó là
  báo cáo đang soạn dở — nghĩa là phần còn lại giống hệt từng ký tự với bản chuẩn.
- Kiểm tra toàn vẹn kho mã: **không có lỗi**, không có dữ liệu hỏng hay thất lạc.
- Bộ thư viện phát triển: đối chiếu đầy đủ với danh sách khai báo — **không thiếu gói nào**.
- Lịch sử thao tác cho thấy sự cố **hoàn toàn nằm ngoài** hệ thống quản lý mã nguồn.
- **Môi trường vận hành thật đang chạy bản dựng từ trước ngày xảy ra sự cố** ⇒ chưa bao giờ
  bị ảnh hưởng.

**🔎 Kết quả rà soát khác**

- Phát hiện một điểm cấu hình máy chủ chưa khớp quy định nội bộ của dự án, đã ghi nhận và
  đang chờ duyệt phương án xử lý.
- Phát hiện một số điểm mù trong cơ chế tự kiểm tra nội bộ — đây là lý do sự cố không được
  báo động sớm. Đã đề xuất vá.
- **17 nghi vấn khác đã được kiểm tra và loại bỏ** vì đo sai hoặc thổi phồng — ghi rõ trong
  báo cáo nội bộ để không bỏ sót mà cũng không báo động giả.

**📌 Bài học ghi nhận:** tệp thì cứu được, nhưng điều đáng lo hơn là **hệ thống phòng vệ đã
không phát hiện ra sự cố**. Những vùng không nằm trong quản lý phiên bản hoàn toàn không có
bản đối chiếu, và ngay cả kho sao lưu dự phòng cũng bị bào mòn mà không ai biết.

---

### V0.333 (08/08/2026) — Rà soát toàn bộ 11 phân hệ & chốt hướng đi tiếp · CHƯA TRIỂN KHAI

> 🧪 **Kiểm thử nội bộ — chưa đưa lên môi trường vận hành thật.**
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền, không triển khai.
> Số hiệu phiên bản giữ nguyên V0.333 theo chính sách chỉ tăng khi phát hành.

**🔍 Bối cảnh:** sau khi phân hệ **Danh Mục (M1)** hoàn tất kiểm thử nội bộ, cần một bức
tranh tổng thể toàn hệ thống để quyết định làm tiếp phân hệ nào, thay vì chọn theo cảm tính.

**📊 Đã làm — rà soát 11 phân hệ bằng số đo**

Đo trực tiếp trên mã nguồn và lịch sử phát triển, không dựa vào tài liệu cũ:
- Quy mô từng phân hệ (số màn hình, khối lượng mã).
- Mức độ hoạt động 60 ngày gần nhất.
- Có bộ kiểm thử tự động riêng hay không.
- Dấu hiệu chưa hoàn thiện: ghi chú "còn phải làm", dữ liệu mẫu tạm, kiểu dữ liệu lỏng.
- Trạng thái bàn giao từ Danh Mục sang từng phân hệ phía sau.

**✅ Kết quả — Danh Mục (M1) đã chốt**

Hoàn tất các quyết định của chủ sở hữu, kiểm thử chấp nhận nội bộ đạt trên bản dựng thật
với 6 vai trò người dùng × 13 màn hình. Tổng **604 điểm kiểm thử đạt / 0 lỗi**.
Các hợp đồng bàn giao sang phân hệ sau đã được khoá. **Không** phân hệ nào phía sau được
coi là hoàn thành.

**🎯 Đề xuất phân hệ tiếp theo: CRM & Bán Hàng (M3)**

Sáu lý do dựa trên số đo:
1. Là nơi phát sinh doanh thu (báo giá → đơn hàng) — sai ở đây tốn tiền thật.
2. Dẫn đầu cả ba chỉ số chưa-hoàn-thiện trong nhóm phân hệ đang phát triển.
3. Là phân hệ lớn thứ hai toàn hệ thống nhưng **chưa có bộ kiểm thử tự động riêng nào**,
   trong khi Danh Mục đã có 10 bộ.
4. Ba luồng dữ liệu từ Danh Mục sang đây đã khoá hợp đồng, chỉ còn triển khai.
5. Là đích đến của phần kiểm soát giá vừa siết ở đợt trước.
6. Rủi ro tích hợp thấp nhất — nền khách hàng và sản phẩm đã nối và đã kiểm chứng.

Chia làm ba đợt nhỏ, mỗi đợt kiểm chứng được: dựng bộ kiểm thử nền → rà soát dữ liệu mẫu
tạm và kiểu dữ liệu lỏng → nối hai luồng dữ liệu đã khoá hợp đồng.

**⏸️ Vì sao chưa chọn phân hệ khác**

| Phân hệ | Lý do chưa vào |
|---|---|
| Tài Chính (MF) | Tài liệu nghiệp vụ còn lệch thực tế, đang chờ chủ sở hữu chốt |
| Kho Hàng (M5) | Còn phụ thuộc một quyết định triển khai chưa được duyệt |
| Sản Xuất (M4) · Công Việc (M8) · Hợp Đồng (MC) | Chưa nối nền Danh Mục, khối lượng lớn — để sau |
| Cổng Thông Tin (M9) | Gần như chưa có gì, là dự án mới hoàn toàn |

**⚠️ Ghi nhận: máy phát triển đang gặp sự cố môi trường**

Phát hiện một số tệp bị xoá khỏi thư mục làm việc và bộ thư viện phát triển bị thiếu thành
phần, khiến **không chạy lại được** bộ kiểm thử để xác nhận con số 604 điểm nói trên ngay
tại thời điểm rà soát. **Toàn bộ đều phục hồi được** và **không ảnh hưởng môi trường vận
hành thật**. Đang chờ chủ sở hữu xác nhận việc xoá là có chủ ý hay không trước khi khôi phục.

**📌 Việc còn chờ quyết định:** một sản phẩm mẫu trong danh mục cần xác nhận là dữ liệu
thử hay dữ liệu thật trước khi dọn.

---

### V0.333 (25/07/2026) — Rà soát & sửa tràn ngang cho toàn bộ màn hình trên điện thoại · ĐÃ VẬN HÀNH THẬT

> ✅ **Đã đưa lên môi trường vận hành thật ngày 25/07/2026.** Không gián đoạn dịch vụ.
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền.

**🔍 Bối cảnh:** đợt trước mới đo được các màn hình không cần đăng nhập. Lần này rà **toàn
bộ màn hình bên trong** để bảo đảm không màn hình nào bị **trôi ngang** trên điện thoại.

**🔬 Cách kiểm — bằng trình duyệt thật, tự động**

- Đo **toàn bộ 33 màn hình × 3 kích thước = 99 lượt** bằng trình duyệt mô phỏng điện thoại.
- **Phân biệt rõ hai tình huống** để không báo động nhầm: *bảng cuộn ngang bên trong khung
  của nó* (đúng thiết kế) khác với *cả trang bị trôi ngang* (lỗi thật). Nhờ vậy khoanh
  đúng **7 màn hình lỗi thật** cần sửa, không đụng nhầm những màn hình vốn đã đúng.
- *(Kỹ thuật: tạo một phiên đăng nhập tạm chỉ trên máy phát triển để đo, xoá ngay sau khi
  xong — không đổi mật khẩu, không đụng dữ liệu thật, không đụng môi trường vận hành.)*

**🛠️ Đã xử lý 7 màn hình**

- **Giao Hàng, Mua Hàng, NCC Vật Tư (Kho Hàng):** khung nội dung không co lại được + phần
  chừa chỗ cho bảng chi tiết (chỉ dành cho máy tính) vẫn áp trên điện thoại → đẩy trôi
  ngang. Đã cho khung co đúng bề ngang máy và chỉ chừa chỗ trên màn hình lớn.
- **CRM:** nền màu kéo ra ngoài khung bằng lề âm → trên điện thoại tràn ra ngoài. Đã đưa
  nền về gọn trong khung, đồng bộ với các màn hình khác.
- **Lệnh Sản Xuất:** bảng bị khung cắt thay vì cho cuộn → đã cho cuộn ngang.
- **Cấu Hình Tính Giá:** hàng thẻ điều hướng dài không cuộn được → đã cho cuộn.
- **Ba màn hình thiết kế riêng** còn vài chi tiết trang trí gây trôi → đã chặn trôi ngang
  **ở phạm vi từng màn hình** (không phải toàn hệ thống). Vì các bảng đều **đã có vùng cuộn
  riêng**, đã **kiểm chứng bằng trình duyệt** rằng **bảng vẫn cuộn được bình thường** —
  không mất chức năng.

**✅ Kiểm chứng sau phát hành**

- Ứng dụng **online, 0 lần khởi động lại bất thường**; các đường dẫn nghiệp vụ phản hồi đúng.
- **Đo lại toàn bộ 33 màn hình × 3 kích thước: không màn hình nào còn trôi ngang.**
- Bảy màn hình đã sửa: **màn hình đứng yên, bảng vẫn cuộn được** — xác nhận không mất chức năng.

**[Scope]** Giao diện trên điện thoại. Không đổi nghiệp vụ, không đổi dữ liệu.

---

### V0.332 (25/07/2026) — Tối ưu giao diện điện thoại & máy tính bảng (ngang + dọc) · ĐÃ VẬN HÀNH THẬT

> ✅ **Đã đưa lên môi trường vận hành thật ngày 25/07/2026.** Không gián đoạn dịch vụ.
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền.

**🔍 Người dùng phản ánh:** giao diện trên điện thoại bị *"méo mó xẹo xọ"*; **xoay ngang
màn hình thì bảng không còn dòng dữ liệu nào** để làm việc; nút "Tạo Mới" bị rớt dòng;
lề hai bên bảng còn rộng, lãng phí không gian.

**🛠️ Sáu nhóm đã xử lý**

1. **Bảng và thẻ "trôi dạt" khi cuộn ngang.** Truy được tận gốc: các ô bố cục **không co
   lại được** khi bảng bên trong rộng hơn màn hình → vùng cuộn riêng của bảng **mất tác
   dụng**, bảng đẩy rộng cả trang. Đã cho **17 ô bố cục** co lại đúng bề ngang máy, nhờ đó
   **chỉ riêng bảng cuộn ngang**, thẻ đứng yên.
2. **Xoay ngang không thấy dòng dữ liệu nào.** Công thức chiều cao cũ dựa trên chiều cao
   màn hình: khi xoay ngang chỉ còn ~390px, trừ đi phần cố định thì **vùng bảng chỉ còn
   ~30px — không đủ một dòng**. Nay **luôn dành tối thiểu ~5 dòng** cho vùng dữ liệu, và
   dùng chiều cao **thực tế** của vùng nhìn (đã trừ thanh địa chỉ trình duyệt).
3. **Nút "Tạo Mới" rớt dòng khi xoay ngang.** Ngưỡng chuyển giao diện đặt ở 768px, mà điện
   thoại xoay ngang rộng 844px → **vượt ngưỡng, dùng nhầm thanh công cụ bản máy tính 7 mục**
   nhồi vào màn hẹp. Đã nâng ngưỡng lên 1024px: điện thoại ngang dùng **thanh dưới gọn**.
4. **Trang tự phóng to trên iPhone.** Ô nhập dùng cỡ chữ nhỏ hơn 16px khiến Safari **tự động
   phóng to cả trang** khi chạm vào; lúc đó thanh trên và thanh dưới **văng khỏi tầm nhìn**,
   người dùng phải chụm tay thu nhỏ. Đã nâng cỡ chữ ô nhập trên điện thoại. **Cố ý không
   khoá thao tác phóng to** — vì như vậy sẽ gây khó cho người mắt kém.
5. **Tối ưu lề hai bên bảng.** Lề cũ chiếm **48px** trên màn 375px (12,8% bề ngang) chỉ để
   làm viền. Đã thu còn **24px** → bề ngang dành cho dữ liệu tăng từ **87,2% lên 93,6%**.
   **Giữ nguyên** lề rộng của phần tiêu đề để chữ không sát mép.
6. **Ảnh nền màn hình đăng nhập theo từng thiết bị.** Đơn vị đã thiết kế sẵn ba bản (điện
   thoại / máy tính bảng / máy tính) nhưng phần mềm chỉ dùng bản máy tính cho mọi thiết bị.
   Nay trình duyệt **tự chọn đúng bản** và **chỉ tải một ảnh**: điện thoại tải **1,5 MB**
   thay vì 4,7 MB — vào nhanh hơn, tốn ít dữ liệu di động hơn.

**🛡️ Một lỗi được chặn kịp nhờ kiểm chứng**

Sau khi gắn ảnh nền mới, kiểm tra phát hiện **hai ảnh mới bị bộ lọc truy cập chặn** (chỉ ảnh
cũ mới qua được). **Nếu không kiểm mà phát hành luôn thì nền trên điện thoại và máy tính bảng
sẽ mất trắng.** Đã mở đúng phạm vi cần thiết, **cố ý không mở cả thư mục** để tệp người dùng
tải lên vẫn được bảo vệ — và đã kiểm chứng lại điều đó sau khi phát hành.

**✅ Kiểm chứng sau phát hành**

- Ứng dụng **online, 0 lần khởi động lại bất thường**; các đường dẫn nghiệp vụ phản hồi đúng;
  **không đường dẫn nào lỗi máy chủ**.
- **Đo bằng trình duyệt thật ở 6 kích thước màn hình** (điện thoại, máy tính bảng, laptop,
  màn rộng — cả dọc lẫn ngang): **12/12 phép đo đạt — không tràn ngang, không phần tử nào
  vượt khung**.
- Đối chiếu tận gói giao diện đã phát hành: **7/7 thông số mới đều có mặt**.
- Ba ảnh nền đều tải được với đúng dung lượng; thư mục tệp người dùng vẫn được bảo vệ.

**[Scope]** Giao diện + trải nghiệm trên thiết bị di động. Không đổi nghiệp vụ, không đổi dữ liệu.

---

### V0.330–V0.331 (24/07/2026) — Áp đồng loạt bo góc chuẩn cho toàn bộ màn hình · ĐÃ VẬN HÀNH THẬT

> ✅ **Đã đưa lên môi trường vận hành thật ngày 24/07/2026.** Không gián đoạn dịch vụ.
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền.

**🔍 Người dùng phản ánh:** sau đợt trước, **một số màn hình vẫn còn góc bo lớn** —
điển hình là màn hình Khách Hàng.

**🔎 Nguyên nhân:** đợt trước mới chuẩn hoá **các thành phần nền tảng dùng chung**. Nhưng
nhiều màn hình **tự dựng giao diện riêng**, không dùng thành phần nền tảng, nên không được
hưởng thay đổi đó.

**🛠️ Đã xử lý**

- **Rà soát toàn bộ mã nguồn:** phát hiện **324 vị trí** dùng độ bo lớn hơn chuẩn, rải rác
  trong **54 tệp** — nhiều nhất ở các màn hình Đơn Hàng, Lệnh Sản Xuất, Kho Hàng, Báo Giá,
  Thiết Kế.
- **Hạ toàn bộ về đúng một thông số chuẩn** (theo màn hình mẫu người dùng chỉ định), xử lý
  cả các biến thể bo theo hướng (trên/dưới/từng góc). Kết quả: **320 vị trí / 53 tệp**.
- **Giữ nguyên** độ bo của ô nhập liệu, nút bấm và nhãn tròn — đúng như màn hình mẫu, nên
  giao diện không bị vuông cứng quá mức.
- **An toàn:** chỉ thay **bán kính bo góc**, **không đụng** cấu trúc, màu sắc hay logic
  nghiệp vụ. Bỏ qua các dòng ghi chú để không làm mất ý nghĩa tài liệu.

**✅ Kiểm chứng sau phát hành**

- Ứng dụng **online, 0 lần khởi động lại bất thường**; các đường dẫn nghiệp vụ phản hồi
  đúng; **không đường dẫn nào lỗi máy chủ**.
- **Rà lại toàn bộ mã nguồn: 0 vị trí** còn dùng độ bo lớn hơn chuẩn.
- Phân bố cuối cùng: bo chuẩn cho thẻ **571 vị trí** · nhãn tròn **397** · ô nhập/nút **377**.

**[Scope]** Giao diện. Không đổi nghiệp vụ, không đổi dữ liệu.

---

### V0.329 (24/07/2026) — Chuẩn hoá không gian làm việc & bo góc theo một khuôn mẫu duy nhất · ĐÃ VẬN HÀNH THẬT

> ✅ **Đã đưa lên môi trường vận hành thật ngày 24/07/2026.** Không gián đoạn dịch vụ.
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền.

**🔍 Người dùng phản ánh:** không gian làm việc của màn hình Tính Giá và Cấu Hình Tính Giá
chưa tối ưu, **viền còn quá lớn**, **góc bo quá tròn**, và **chưa nhất quán** với các màn
hình khác. Người dùng chỉ định một **màn hình mẫu** để đối chiếu.

**🔎 Truy được gốc rễ — 3 điểm lệch so với màn hình mẫu**

1. **Ba lớp viền chồng nhau.** Màn hình mẫu chỉ có một lớp viền, còn các màn hình khác bị
   bọc thêm hai lớp khung → chữ tiêu đề bị đẩy vào sâu **gần gấp ba** so với mẫu.
2. **Vùng nội dung của mẫu không có viền ngang trên màn hình lớn** — nhờ vậy bảng trải sát
   mép, tận dụng tối đa bề ngang. Đây là điểm mấu chốt và đã bị bỏ sót ở hai lượt sửa đầu.
3. **Bo góc**: mẫu dùng độ bo **nhỏ** cho thẻ chính, trong khi các màn hình khác dùng độ bo
   **gấp đôi** → cảm giác "quá tròn".

**🛠️ Đã xử lý**

- **Hai màn hình tính giá**: bỏ hoàn toàn hai lớp khung thừa, dùng tiêu đề phẳng sát mép và
  vùng nội dung không viền ngang — **giống hệt màn hình mẫu**. Nhãn trạng thái dùng **biến
  màu chuẩn của hệ thống** (không gán màu cứng) nên tự động đúng theo bộ màu chung.
- **Áp dụng đồng loạt cho các màn hình còn lại** bằng cách sửa tại **5 thành phần nền tảng
  dùng chung** thay vì sửa từng màn hình. Nhờ vậy **mọi màn hình đang có và sẽ có đều tự
  động theo một khuôn**, không còn tình trạng mỗi nơi một kiểu.
- **Thống nhất bo góc về đúng một thông số** trên cả 5 thành phần nền tảng.
- Điều chỉnh **trợ lý tạo khách hàng** về dạng bảng trượt cho **đồng bộ với các biểu mẫu
  nhập liệu khác**, với bề ngang nằm trong dải chuẩn (trước đó rộng gần gấp đôi các biểu
  mẫu khác nên che gần hết màn hình).

**✅ Kiểm chứng sau phát hành**

- Ứng dụng **online, 0 lần khởi động lại bất thường**; màn hình đăng nhập bình thường; các
  đường dẫn nghiệp vụ phản hồi đúng; **không đường dẫn nào lỗi máy chủ**.
- **Kiểm tra tận gói giao diện đã phát hành**: đối chiếu **10/10 thông số bố cục và bo góc**
  — tất cả đều thật sự có mặt trong bản dựng, bảo đảm hiển thị đúng trên môi trường vận
  hành chứ không chỉ đúng trên máy lập trình.

**[Scope]** Giao diện + trải nghiệm. Không đụng công thức tính giá, không đổi nghiệp vụ.

---

### V0.328 (24/07/2026) — Sửa biểu tượng bị ẩn, chuẩn hoá màu sắc, trợ lý tạo khách hàng vào khung chung · ĐÃ VẬN HÀNH THẬT

> ✅ **Đã đưa lên môi trường vận hành thật ngày 24/07/2026.** Không gián đoạn dịch vụ.
> Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền.

**🔍 Người dùng phản ánh:** biểu tượng ở tiêu đề trang bị ẩn; màu sắc chưa chuẩn, thiếu
logic; trợ lý tạo khách hàng mở tách rời khỏi thanh menu.

**🛠️ Đã xử lý**

- **Sửa biểu tượng bị ẩn — lỗi ở toàn hệ thống, không riêng một trang.** Truy được tận
  gốc: ô chứa biểu tượng ở thanh tiêu đề **không có nền**, trong khi **cả 10 màn hình**
  đều dùng biểu tượng **màu trắng** → trắng trên nền trắng nên **không nhìn thấy**. Đã sửa
  ngay tại thành phần dùng chung (một chỗ, cả 10 màn hình cùng hết lỗi).
- **Gán màu biểu tượng theo từng phân hệ** đúng bảng màu chuẩn của dự án (Hệ Thống — cam,
  Danh Mục — xanh dương, Bán Hàng — xanh ngọc, Sản Xuất — tím, Tài Chính — đỏ), thay vì
  dùng chung một màu. Nhìn vào là **nhận ra ngay đang ở phân hệ nào**.
- **Chuẩn hoá màu trang tính giá:** trước đây mỗi thẻ một màu (10 kiểu màu khác nhau,
  rối mắt, không theo quy luật). Nay gom về **4 nhóm theo vai trò**:
  *nhập liệu* — một màu thống nhất theo phân hệ · *kết quả & thông tin* — xanh dương ·
  *giá tiền* — cam thương hiệu (điểm nhấn) · *chức năng chưa mở* — xám.
  Người dùng nhìn màu là **đoán được vai trò của khối**, không phải đọc mới hiểu.
- **Trợ lý tạo khách hàng** trước đây mở dạng bảng trượt che kín màn hình nên **mất thanh
  menu**, thao tác rời rạc. Nay là **trang bình thường nằm trong khung chung**, vẫn thấy
  thanh menu, đi lại liền mạch. **Giữ nguyên** cơ chế hỏi xác nhận khi rời trang lúc form
  còn dở dang, và các bảng phụ (Liên hệ / Địa chỉ) vẫn hoạt động như cũ.

**✅ Kiểm chứng sau phát hành**

- Ứng dụng **online, 0 lần khởi động lại bất thường**; màn hình đăng nhập bình thường;
  các đường dẫn nghiệp vụ phản hồi đúng; **không đường dẫn nào lỗi máy chủ**.
- **Kiểm tra tận gói giao diện đã phát hành**: xác nhận **toàn bộ mã màu mới thật sự có
  mặt** trong bản dựng — bảo đảm biểu tượng hiện đúng màu, không phải chỉ đúng trên máy
  lập trình.

**[Scope]** Giao diện + trải nghiệm. Không đụng công thức tính giá, không đổi nghiệp vụ.

---

### V0.327 (24/07/2026) — Chuẩn hoá trang tính giá vào khung giao diện chung · ĐÃ VẬN HÀNH THẬT

> ✅ **Đã đưa lên môi trường vận hành thật ngày 24/07/2026**, cùng đợt với V0.326.
> **Không gián đoạn dịch vụ. Dữ liệu nguyên vẹn.**

**🎨 Nội dung thay đổi**

- **Các trang tính giá** trước đây được dựng như ứng dụng riêng biệt (có thanh tiêu đề
  riêng chiếm trọn bề ngang), nên sau khi đưa vào khung ERP thì bị **hai thanh tiêu đề
  chồng nhau**. Nay đã chuẩn hoá về đúng bố cục chung của hệ thống — nhìn liền mạch,
  thao tác thống nhất với các màn hình khác.
- **Trang cấu hình tính giá** dùng tông tối riêng (chữ sáng trên nền tối) nên không thể
  đổi sang nền sáng (sẽ mất chữ). Giải pháp: giữ phần nội dung tối gọn thành **một khối
  bo góc** nằm trong khung ERP, phía trên là thanh tiêu đề chuẩn — vừa liền mạch vừa
  **không phải sửa hàng loạt màu sắc** (tránh rủi ro làm hỏng màn hình đang dùng tốt).
- **Trang quản lý bảng giá công đoạn**: thay tiêu đề riêng bằng thanh tiêu đề chuẩn.
- Chỉ thay **lớp vỏ trình bày** — **không đụng công thức tính giá**, không đổi logic
  nghiệp vụ, không đổi dữ liệu.

**🔒 An toàn khi phát hành**

- **Sao lưu toàn bộ dữ liệu vận hành trước khi phát hành** (kiểm tra tệp sao lưu toàn vẹn,
  đủ 99 bảng, có dữ liệu thật). Bản sao lưu gần nhất trước đó đã cũ gần 3 tháng — đã bổ
  sung bản mới trước khi chạy nâng cấp cấu trúc.
- Chạy đầy đủ các cổng kiểm tra tương thích cấu trúc dữ liệu trước khi dựng bản chạy.
- **Đối chiếu sau phát hành:** tổng số bảng **không đổi**; số nhân viên, phòng ban, vai
  trò, khách hàng, tài khoản **khớp đúng** với số liệu đã ghi nhận trước đó → **không mất
  dữ liệu**.
- Kiểm chứng hoạt động: tiến trình ứng dụng **online, 0 lần khởi động lại bất thường**;
  màn hình đăng nhập truy cập được bình thường; các đường dẫn nghiệp vụ phản hồi đúng;
  **không có đường dẫn nào lỗi máy chủ**.

**ℹ️ Lưu ý cho người dùng:** sau mỗi lần phát hành, nếu đang mở sẵn tab cũ thì **tải lại
trang (F5) một lần** để dùng bản mới.

**[Scope]** Giao diện + phát hành. Không đổi cấu trúc dữ liệu, không đổi dữ liệu thật,
không đổi phân quyền.

---

### V0.326 (24/07/2026) — Menu chuyển trang mượt, khung giao diện dùng chung, tăng tốc máy phát triển

> 📌 **Ghi chú đánh số phiên bản:** đợt này từng bị tăng phiên bản rời rạc lên V0.331
> (sai chính sách V0.325 — *mỗi đợt phát hành chỉ tăng đúng một bậc*). Toàn bộ nội dung
> V0.326–V0.331 đã được **gộp lại thành một bậc V0.326** để số hiệu phiên bản giữa máy
> phát triển, kho chung và môi trường vận hành **không bị lệch**. Cổng kiểm tra trước
> khi ghi nhận đã tự động phát hiện và chặn sai sót này.

**🧱 Chuẩn hoá khung giao diện dùng chung (xử lý dứt điểm gốc rễ)**

- Trước đây khung giao diện (thanh menu, thanh trên cùng) **bị dựng lại từ đầu ở mỗi
  trang**; nay được đặt **một lần duy nhất ở lớp ngoài cùng** và **giữ nguyên khi
  chuyển trang** — chuyển giữa các module liền mạch, không dựng lại, không giật.
- **Gom cách bố trí khung về một mối:** trước đây khung được gắn rải rác ở nhiều nơi,
  không nhất quán; nay thống nhất về một điểm, dễ bảo trì.
- **Đưa các trang trước đây mở rời khỏi thanh menu vào chung khung ERP** (máy tính giá,
  trợ lý tạo khách hàng, phiếu nhập/xuất, quản lý bảng giá): nay đều có thanh menu và
  nằm trong bộ điều khiển chung, thao tác liền mạch. Chỉ màn hình đăng nhập và trang
  báo thiếu quyền là giữ dạng không có thanh menu.

**⚡ Chuyển trang mượt**

- Thông tin người dùng và quyền truy cập nay được **giữ lại trong phiên**, tự làm mới
  **ngầm** theo chu kỳ ngắn — menu không còn biến mất khi chuyển trang, quyền vẫn được
  cập nhật mà không cần tải lại trang.
- Bổ sung **màn hình chờ tạm** cho các module chính — bấm menu là thấy phản hồi ngay.
- **Giảm truy vấn lặp** khi kiểm tra quyền ở mỗi lần vào trang.
- **An toàn:** khi phiên đăng nhập hết hạn, hệ thống vẫn **khoá chặt và đưa về màn hình
  đăng nhập** đúng như trước; không có trường hợp nào được nới quyền. Không trang nào
  đổi địa chỉ.

**🛠️ Tăng tốc máy phát triển (không liên quan môi trường vận hành thật)**

- Chuyển công cụ dựng mã khi lập trình sang **bộ máy thế hệ mới** (nhanh hơn đáng kể),
  sau khi kiểm chứng lỗi tương thích cũ trên Windows đã không còn ở phiên bản nền hiện
  tại; vẫn giữ tuỳ chọn cũ để dự phòng.
- Bổ sung **tiện ích khởi động / khởi động lại nhanh** trên máy lập trình và gắn vào
  công cụ soạn thảo (nút bấm + phím tắt).

**✅ Kiểm chứng:** biên dịch đạt, dựng bản phát hành đạt, khởi động máy chủ không lỗi.

**[Scope]** Kiến trúc giao diện + trải nghiệm + môi trường phát triển. Không đổi cấu
trúc dữ liệu, không đổi dữ liệu thật, không đổi phân quyền.

---

### V0.325 (24/07/2026) — An toàn đường dẫn và chuẩn hóa chính sách phát hành

> 🔧 **Bản phát hành bảo trì.** Không thay đổi cấu trúc dữ liệu, không thay đổi dữ liệu thật.

- **Loại bỏ tham chiếu thư mục gốc đã lỗi thời** trong toàn bộ tài liệu và hướng
  dẫn đang dùng — trước đây một số nơi vẫn trỏ về vị trí dự án cũ, dễ khiến người
  vận hành thao tác nhầm chỗ.
- **Gia cố công cụ bảo trì kho mã:** công cụ dọn dung lượng nay **tự xác định**
  thư mục dự án và **đối chiếu lại** trước khi làm bất cứ điều gì; **mặc định chỉ
  xem trước**, phải nêu rõ ý định mới thực thi; in toàn bộ kế hoạch trước khi chạy
  và từ chối mọi mục tiêu nằm ngoài phạm vi đã xác thực.
- **Chuẩn hóa cách đánh phiên bản:** mỗi đợt phát hành chỉ tăng phiên bản **đúng
  một lần** và **có chủ đích**, thay cho cơ chế tự động tăng ở mỗi lần lưu thay đổi.
- **Thay đổi chỉ về tài liệu không còn làm tăng phiên bản ứng dụng** — nhờ vậy số
  hiệu phiên bản giữa máy phát triển, kho chung và môi trường vận hành không còn
  bị lệch.
- Bổ sung **cổng kiểm tra trước khi triển khai**: chỉ cho phép đưa lên vận hành một
  bản phát hành thật sự, không cho phép đẩy nhầm một bản trung gian.
- **[Scope]** Công cụ + tài liệu + quy trình phát hành. Không đổi cấu trúc dữ liệu,
  không đổi dữ liệu thật.

---

### V0.324 (24/07/2026) — Chuẩn hóa workspace và tài liệu vận hành

> 🧹 **Không thay đổi logic vận hành, cấu trúc dữ liệu hay dữ liệu thật.**

- Hợp nhất về **một thư mục dự án chuẩn duy nhất**.
- **Gỡ bỏ thư mục làm việc Git tạm thời** đã hết vai trò.
- **Sửa tài liệu đường dẫn cục bộ đang dùng** từ ổ `E:` sang ổ `D:`.
- **Đưa vào quản lý phiên bản** sơ đồ workspace và quy tắc dự án bắt buộc còn thiếu.
- **Lưu trữ và gỡ bỏ các nhánh đã bị thay thế**.
- **[Scope]** Workspace + tài liệu. Không đổi logic vận hành, không đổi cấu trúc dữ liệu, không đổi dữ liệu thật.

---

### V0.305–V0.323 (phát hành 23/07/2026) — Đợt R1/R1.1/R1.2

> 🚀 **Đã đưa lên vận hành thật. Không gián đoạn dịch vụ.**

#### An toàn hiển thị nội dung
- **Gia cố an toàn hiển thị nội dung HTML và bản in.** Rà soát toàn bộ các đường hiển thị nội dung do người dùng nhập trên hệ thống và bổ sung lớp bảo vệ đồng bộ.
- Nguyên tắc áp dụng: **giữ nguyên vẹn định dạng tài liệu đã duyệt** — lớp bảo vệ không được phép làm biến dạng mẫu in.
- Đã kiểm chứng trên trình duyệt thật và trên **toàn bộ mẫu in đang lưu**: mẫu hợp lệ vẫn xem, lưu và sửa bình thường, định dạng bản in không đổi.

#### Sửa lỗi — không tạo được nhân viên
- Chức năng thêm nhân viên không hoạt động do lỗi ở tầng ghi dữ liệu. Lỗi chưa lộ ra trước đó vì toàn bộ nhân sự hiện có được nạp bằng công cụ nền.
- Đã sửa và thử nghiệm trên môi trường vận hành, sau đó dọn sạch hoàn toàn, không để lại bất kỳ dữ liệu thử nghiệm nào.

#### Mã Phòng Ban trở thành bất biến
- Trước đây chức năng **Sửa** vẫn có thể ghi đè mã đã cấp.
- Nay mã chỉ được sinh **một lần duy nhất lúc tạo**; đổi tên phòng ban **không** làm đổi mã; phần viết tắt do hệ thống tự suy ra.

#### Sửa lỗi — ô chọn Khuôn / Kiểu In hiện trống
- Trên form in **Lệnh Sản Xuất** và **Phiếu Điều In**, hai ô chọn này luôn trống và không hiển thị thông báo lỗi nào cho người dùng.
- Nguyên nhân nằm ở điều kiện lọc đã lỗi thời trong danh mục dùng chung. Đã đối chiếu dữ liệu thật và sửa **cả hai** ô cùng lúc.

#### Chuẩn hoá cơ cấu tổ chức
- Bổ sung **3 phòng ban** (Ban Điều Hành, Phòng Thiết Kế, Phòng Kinh Doanh) → tổng **6 phòng ban**. Ba phòng ban cũ **giữ nguyên**, không đổi mã, không gộp.
- Quy tắc sinh mã bảo đảm chạy lại nhiều lần **không** tạo bản trùng.

#### An toàn dữ liệu
- **Kiểm chứng khả năng khôi phục:** phục hồi thật bản sao lưu vào một môi trường cơ sở dữ liệu **tách biệt hoàn toàn, đúng phiên bản** đang dùng thật; đối chiếu **từng bảng** cả số lượng bản ghi lẫn dấu vân tay nội dung; mọi khác biệt đều truy được nguyên nhân cụ thể. Môi trường tách biệt được **xoá bỏ** sau khi lấy bằng chứng.
- Quy mô dữ liệu vận hành hiện tại: **99 bảng**.

#### Chất lượng
- Kiểm tra kiểu dữ liệu **sạch**; các tệp thay đổi trong đợt đều **không còn lỗi quy chuẩn**.
- **126/126** lượt kiểm tra phân quyền theo tuyến — **0 lỗi**.
- Tài khoản **ngưng hoạt động** hoặc **bị khoá** đều bị từ chối đăng nhập đúng.
- Bản phát hành được dựng lại từ **bản sao sạch** — thành công.
- **[Scope]** An toàn dữ liệu + Logic + DevOps.

---

### V0.286–V0.304 (23/07/2026) — Hoàn Thiện PL4 + **ĐƯA LÊN VẬN HÀNH THẬT** (V0.240 → V0.303)

> 🚀 **Cột mốc:** môi trường vận hành thật được cập nhật từ **V0.240 → V0.303** — thu hẹp khoảng tồn đọng **~40 mốc thay đổi / 13 ngày** về **0**. **Không gián đoạn dịch vụ.**

#### 🧩 Nghiệp vụ — M1 Nhân Sự

- **Chọn Phòng Ban trước → Vị Trí lọc theo phòng ban.** Trước đây hai ô chọn hoạt động độc lập và Vị Trí lại đặt trước Phòng Ban, nên người nhập có thể tạo ra cặp **lệch nhau** (vị trí thuộc phòng A nhưng phòng ban ghi phòng B). Nay: chọn Phòng Ban là bước 1, danh sách Vị Trí **chỉ hiện các vị trí thuộc phòng ban đó** (kèm vị trí dùng chung); đổi phòng ban thì vị trí không hợp lệ **tự bỏ chọn**; ô Vị Trí bị khóa cho tới khi chọn phòng ban.
- **Bịt lỗ hổng ở đường Sửa.** Ràng buộc nhất quán trước đây chỉ áp dụng khi **Tạo mới**; đường **Sửa** vẫn ghi được cặp lệch. Đã bổ sung kiểm tra ở cả hai đường.
- **Hiển thị tên vị trí đầy đủ kèm phòng ban.** Hai vị trí trùng tên *"Trưởng Phòng"* ở hai phòng ban khác nhau trước đây hiển thị **y hệt nhau** trong danh sách chọn → dễ chọn nhầm. Nay ghép nhãn *"{Vị Trí} — {Phòng Ban}"* ở danh sách, bảng và trang chi tiết (chỉ hiển thị, **không thay đổi dữ liệu lưu trữ**).
- **Kiểm chứng:** sau khi sửa, **20/20 tên vị trí là duy nhất**; số nhân viên có dữ liệu lệch phòng ban/vị trí = **0**.

#### 🔢 Nghiệp vụ — Mã tự sinh

- Chuẩn hóa **mã tự sinh liên tiếp** cho Phòng Ban / Vị Trí / Nhân Sự bằng **bộ đếm giao dịch**: không trùng khi nhiều người thao tác cùng lúc, hủy giữa chừng thì **không tiêu số**, xóa bản ghi **không làm lùi bộ đếm**.
- Mã cũ theo định dạng khác **được giữ nguyên**, chỉ không tham gia cấp phát số mới.

#### 🖱️ Trải nghiệm nhập liệu — 3 lỗi

- **Kéo thanh cuộn bị hiểu nhầm là thoát form** → hiện cảnh báo oan. Đã lọc, áp dụng cho cả 3 trang Phòng Ban / Vị Trí / Nhân Sự.
- **Tiêu đề và cụm nút thao tác bị cuộn mất** trong biểu mẫu dài → phải cuộn tìm nút. Nay tiêu đề ghim đỉnh, cụm nút ghim đáy, luôn trong tầm tay.
- **Hộp xác nhận thoát không hiện đúng chuẩn** ở trang Nhân Sự (đối chiếu 8 màn hình khác đều đúng, riêng trang này là ngoại lệ) → đã đồng bộ về chuẩn chung.
- *Đã kiểm chứng không phải lỗi:* cơ chế theo dõi "đã sửa đổi" phủ **28/28 ô nhập** → **không có nguy cơ mất dữ liệu đang nhập**.

#### 🛡️ Vận hành — Cổng kiểm tra tương thích trước khi triển khai

- Bổ sung **cổng chặn tự động**: đối chiếu yêu cầu của mã nguồn với cấu trúc dữ liệu ở môi trường đích **trước bước khởi động lại**. Không đạt thì **dừng ngay**, môi trường thật vẫn chạy bản cũ.
- **Cổng này đã chặn thật một lần** ngay trong lần triển khai đầu tiên: một bước cập nhật dữ liệu nền đổ lỗi do **khác biệt cấu hình chặt/lỏng giữa môi trường phát triển và môi trường thật** — cùng một câu lệnh, môi trường phát triển chỉ cảnh báo còn môi trường thật báo lỗi cứng. Nhờ cổng chặn, **dịch vụ không bị gián đoạn**; sau khi vá mới triển khai lại.
- Bổ sung bước **cập nhật dữ liệu nền còn thiếu** vào chuỗi triển khai tự động (trước đây nằm ngoài chuỗi nên bị bỏ sót — nếu thiếu sẽ khiến không tạo được Phòng Ban / Vị Trí / Nhân Sự và người dùng không phải quản trị viên bị chặn truy cập 3 trang nhân sự).

#### 🧯 An toàn dữ liệu

- **Sao lưu đầy đủ trước khi triển khai**, có **kiểm chứng nội dung** (đếm số bảng, xác nhận có dữ liệu) — phát hiện các bản sao lưu cũ trên môi trường thật đều **rỗng, vô dụng**, nên đã lập nơi lưu mới.
- Có **điểm khôi phục** rõ ràng trước khi triển khai.
- **Không đụng tới dữ liệu nghiệp vụ** của môi trường thật.

#### 🔍 Phát hiện quan trọng

- Rà soát cho thấy chức năng **tạo Vị Trí trên môi trường thật đã hỏng sẵn từ trước** do lệch cấu trúc dữ liệu. Lần triển khai này **khắc phục luôn** lỗi tồn đọng đó.
- Đính chính: cảnh báo ban đầu về khả năng lệch cấu trúc diện rộng là **không đúng** — sau khi đối chiếu đầy đủ, hai môi trường **khớp nhau**, chỉ thiếu duy nhất phần dữ liệu nền của bộ đếm.

#### ✅ Kết quả kiểm chứng sau triển khai R1/R1.1/R1.2 (mốc 23/07/2026)

| Hạng mục | Kết quả |
|---|---|
| Phiên bản đang chạy khi nghiệm thu | **V0.323** |
| Trang đăng nhập | Hoạt động bình thường |
| Các trang có bảo vệ | Chuyển hướng đăng nhập đúng — **phân quyền nguyên vẹn** |
| Tiến trình ứng dụng | **Online**, **0 lần khởi động lại ngoài ý muốn** |
| Cổng kiểm tra tương thích | **Đạt** |
| Đồng bộ 3 nơi | Tại thời điểm nghiệm thu R1.2, mã nguồn, kho chung và môi trường vận hành cùng ở V0.323 |
| Chất lượng mã | Kiểm tra kiểu & quy chuẩn **sạch** |

- **[Scope]** UI + Logic + DevOps · đã hợp nhất nhánh, **đã đưa lên vận hành thật**, có hồ sơ triển khai đầy đủ lưu nội bộ để truy vết.

### V0.281–V0.285 (22/07/2026) — PL4 Phase 1: Nền Tảng Nhân Sự & Phân Quyền (HR-Org-RBAC)
- **[M1 Nhân sự]** Hoàn thiện quản lý nhân sự để phân quyền: liên kết Nhân viên ↔ Tài khoản đăng nhập ↔ Vai trò, theo mô hình **phân tách trách nhiệm** (HR quản hồ sơ + tài khoản; Bảo mật M0 quản vai trò + mật khẩu + kích hoạt).
- **[M0 Bảo mật]** Kích hoạt tài khoản có kiểm soát (chỉ Admin, bắt buộc đặt mật khẩu trước khi kích hoạt); giữ nguyên ranh giới gán vai trò admin-only + bảo vệ admin cuối.
- **[M1 Nhân sự]** Vòng đời **nghỉ việc thay cho xóa cứng** (giữ lịch sử nhân sự); offboarding: khóa tài khoản + thu hồi phiên đăng nhập (Admin).
- **[M0/M1]** Phân quyền **theo từng chức năng** (Phòng ban / Vị trí / Nhân sự riêng) thay vì gộp module; ghi vết người thao tác từ phiên đăng nhập (bỏ hardcode).
- **[UI]** Trang Nhân sự thêm khu "Tài Khoản Đăng Nhập" (liên kết/tạo/hủy, vai trò chỉ-xem) + nút "Chuyển Nghỉ Việc".
- **[Scope]** Logic + Data (seed quyền, không đổi cấu trúc DB) + UI · **đã hợp nhất và đã đưa lên vận hành thật ngày 23/07/2026** (xem mục V0.305–V0.323).

### V0.271–V0.280 (21–22/07/2026) — Đồng Bộ UI 4 Trang + Chuẩn Hóa Tailwind v4
- **[M0/M1/M5]** Thống nhất giao diện Phòng Ban / Vị Trí / Nhân Sự / Kho Thành Phẩm về cùng 1 chuẩn (detail panel, sắc độ, khoảng cách, bo góc).
- **[Toàn hệ]** Quét chuẩn hóa class Tailwind v4 canonical (CSS tương đương 100%, không đổi giao diện) + nâng cấp skill hỗ trợ triển khai.
- **[Scope]** UI + Tooling.

### V0.250 (18/07/2026) — Demo UI Visual Audit v1.1.0
- **[M1]** Button text đầy đủ 'Thêm Vị Trí' (không viết tắt), sizing chuẩn Metronic h-34px
- **[M1]** Table header border-b-2 + font-semibold (Metronic v9.5)
- **[M1]** ~~Badge cấp bậc color-coded: 8 cấp bậc → 8 màu phân biệt~~ → **ĐÃ BỊ THAY THẾ (23/07/2026):** không còn thang cấp bậc độc lập. **Vị Trí chính là cấp bậc** — đây là mô hình chuẩn hiện hành. Trường cấp bậc cũ được **giữ lại để bảo toàn lịch sử**, ngừng ghi mới.
- **[M1]** Action icons hover feedback (text color cam/đỏ)
- **[M1]** Null/empty → 'chưa gán' italic thay vì '-'
- **[M1]** Table card wrapper thêm shadow
- **[Infra]** Charset utf8mb4 đã confirm OK ở tầng kết nối DB
- **[Scope]** UI only — 7/12 visual bugs fixed (5 đã OK từ v1.0.0)

### V0.249 (18/07/2026) — Demo UI Standardization Phase 1
- **[Shared]** Tạo hệ thống design tokens CSS (Metronic v9.5 adapted + TanPhat brand colors)
- **[Shared]** Tạo auto-case utility (autoCase, displayName, displayStatus, displayLabel) cho Vietnamese Title Case chuẩn
- **[Shared]** Tạo status-styles utility (centralized 16-status badge system, 6 nhóm màu)
- **[M1]** Fix responsive bug P0: FormRow grid luôn 2 cột → 1 cột mobile, 2 cột desktop
- **[M1]** Upgrade StandardDataTable: thêm pagination (ellipsis), responsive columns, loading skeleton, empty state, row click
- **[M1]** Refactor trang Vị Trí Công Việc dùng shared components + design tokens + autoCase
- **[Scope]** UI only — không thay đổi business logic, API, database

### V0.248 (18/07/2026) — Control Audit + Worktree Preserve Checkpoint
- **[Audit]** Kiểm toán kỹ thuật read-only toàn hệ thống: xác minh baseline, đối chiếu Notion ↔ Source ↔ DB metadata
- **[RBAC]** Xác nhận RBAC guards đã gắn trên 200+ Server Actions (M0/M1/M3/M5/M8/MC/MF)
- **[Security]** Phát hiện 5 findings ưu tiên cao (transaction safety, persistence, audit trail) — chưa fix, chờ Owner duyệt
- **[Git]** Bảo toàn 37 file uncommitted thành 4 checkpoint commits, push backup lên origin
- **[Git]** Xác minh Draft PR #1 vẫn Open trên GitHub, chưa merge
- **[Git]** Sync main local = origin/main (V0.239)
- **[Scope]** Read-only audit + checkpoint preserve — Không thay đổi logic code, schema, Notion, hay deploy

### V0.242 (18/07/2026) — Kho Thành Phẩm Premium UI V2.1
- **[M5/KTP]** Redesign giao diện Kho Thành Phẩm theo premium style, header cam gradient matching M1 Khách Hàng
- **[M5/KTP]** Stat cards với colored top-border accent, table header orange gradient, detail panel với progress bar
- **[Scope]** UI only — Không thay đổi schema/logic/RBAC

### 🚀 Go-Live P0.1 — A*) READY FOR INTERNAL MINI PILOT (15/06/2026)

**Mục tiêu:** Phân quyền RBAC cho NV Kinh doanh & Thiết kế

| Phase | Nội dung | Status |
|-------|----------|--------|
| P0.1-1 | Roles + Menu + Action permissions | ✅ Done (7 roles, 38 menu, 31 action) |
| P0.1-2 | Page guards (14/14) | ✅ Verified |
| P0.1-3 | Customer ownership + cost masking | ✅ Done + Browser verified |
| P0.1-4 | LSX/PDI state guards (THIET_KE) | ✅ Done |
| P0.1-5 | Accounting packs (KE_TOAN) | ✅ Done (3 packs) |
| P0.1-6 | Smoke test 15/15 | ✅ PASS |
| P0.1-7 | Production deploy | ❌ NOT APPROVED |

> **Decision: A\*) INTERNAL MINI PILOT — TEST USERS ONLY**
> Page guard: 14/14 ✅ (không missing). Role canonical: SALES. THIET_KE đã tạo.
> Employee data: SAFE (original was dummy). Test users: 17, local only.

---

## 🏗️ Kiến Trúc Hệ Thống

### Stack chính:
| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | Next.js | 16.1.6 |
| UI Library | React | 19.2.4 |
| CSS | Tailwind CSS | 4.2.1 |
| Language | TypeScript | 5.9.3 |
| Database (production) | MariaDB | 10.11 |
| Database (local development) | MySQL | 8.4 |
| Form | React Hook Form + Zod 4 | Latest |
| UI Components | Radix UI + Shadcn | Multiple |
| Icons | Lucide React | 0.462.0 |
| Template | Metronic (Demo 1) | Licensed |

### Architecture patterns:
- **Server Actions + Server Components** (không tách FE/BE theo REST)
- **SSE** (Server-Sent Events) cho real-time
- **Foundation Components**: PageCanvas, PageHeader, StatCard, FilterBar, SectionPanel
- **Shell System**: AppShellProvider với presets (Owner Review, Operator, Focus Entry)
- **Auth**: Fail-closed session, expired warning, cookie-based

### Deployment:
- **Hạ tầng:** Production đã go-live (chi tiết máy chủ / domain / cấu hình **KHÔNG công khai** — theo Rule 14)
- **Git flow**: Local → GitHub (private) → Production deploy (quy trình có kiểm soát)

---

## 📊 Thống Kê Tổng Hợp (cập nhật 24/07/2026)

| Metric | Giá trị |
|--------|---------|
| **Mốc phiên bản hiện tại** | V0.333 |
| **Thời gian phát triển** | 18/01/2026 → 24/07/2026 (~6 tháng) |
| **Modules hoạt động** | M0, M1, M3, M4, M6, M7, M8, MC, MF (9/11) |
| **Modules planned / skeleton** | M5, M9 (2/11) |
| **Tổng bảng DB** | 99 bảng (kiểm đếm trên môi trường vận hành 23/07/2026) |
| **Skills hỗ trợ** | 60+ |
| **Node.js** | v20.20.0 (môi trường vận hành) · v24.18.0 (máy phát triển) · engines: >=20 <25 |

---

## 📂 Cấu Trúc Báo Cáo

| File | Nội dung |
|------|----------|
| **README.md** | Tổng quan dự án, modules, kiến trúc, thống kê (file này) |
| **[CHANGELOG-DETAIL.md](CHANGELOG-DETAIL.md)** | Changelog chi tiết V0.216 → V0.100 (gần đây) |
| **[CHANGELOG-ARCHIVE.md](CHANGELOG-ARCHIVE.md)** | Changelog archive V0.99 → V0.00 (cũ hơn) |
| **[MODULE-PROGRESS.md](MODULE-PROGRESS.md)** | Tiến độ chi tiết từng module + chức năng |
| **[GOVERNANCE-LOG.md](GOVERNANCE-LOG.md)** | Lịch sử governance, skills, architecture decisions |

---

## 🔗 Liên Kết

- **Main repo (private):** `irissnss/erptanphat`
- **Báo cáo này (public):** [`irissnss/Baocaoerptanphat`](https://github.com/irissnss/Baocaoerptanphat)
- **Notion workspace:** tài liệu quản trị nội bộ

---

## 📋 Ghi Chú Trạng Thái (24/07/2026)

- **Mới nhất:** Đợt R1/R1.1/R1.2 — **đã đưa lên vận hành thật ngày 23/07/2026**, không gián đoạn dịch vụ.
- **PL4 (Nhân sự – Tổ chức – Phân quyền):** đã hợp nhất và **đang chạy thật** (đính chính cho các bản báo cáo trước ghi "chưa deploy/merge").
- **Phân quyền:** đã kiểm chứng đầy đủ trên môi trường chạy thật — **126/126 đạt** (đính chính cho bản trước ghi "còn chờ, thiếu test identities").
- **Cơ cấu tổ chức:** 6 phòng ban; 3 phòng ban cũ giữ nguyên mã; mã đã cấp là **bất biến**.
- **Mô hình cấp bậc:** **Vị Trí chính là cấp bậc** (mô hình 8 cấp bậc độc lập đã bị thay thế).
- **Nợ kỹ thuật đã biết:** kho mã còn tồn cảnh báo quy chuẩn **có từ trước** đợt này. Đợt R1/R1.1/R1.2 **không làm phát sinh thêm**; các tệp đã sửa đều sạch.
- **Môi trường dev:** MySQL local cần bật khi phát triển.

---

> **Phát hành lên vận hành thật:** 23/07/2026 — V0.323 (Đợt R1/R1.1/R1.2)
>
> **Cập nhật báo cáo này:** 24/07/2026
