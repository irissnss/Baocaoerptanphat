# 📊 Báo Cáo ERP Tân Phát — Tổng Quan & Changelog

> **Repo công khai** để Notion và các công cụ AI có thể đọc, hỗ trợ Owner quản lý dự án ERP Tân Phát.
>
> ⚠️ **CHỈ chứa báo cáo** — KHÔNG chứa source code, schema, credentials, hay dữ liệu nhạy cảm.

---

## 📌 Thông Tin Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Tên dự án** | ERP Tân Phát (Tân Phát Packaging) |
| **Phiên bản mã nguồn** | `V0.323` |
| **Phiên bản đang chạy thật** | `V0.323` |
| **Mốc phiên bản hiện tại** | V0.323 |
| **Ngày bắt đầu** | 18/01/2026 |
| **Phát hành lên vận hành thật** | 23/07/2026 — Đợt R1/R1.1/R1.2 |
| **Cập nhật báo cáo này** | 24/07/2026 |
| **Tech Stack** | Next.js 16.1.6 · React 19.2.4 · Tailwind 4.2.1 · TypeScript 5.9.3 · MariaDB 10.11 (production) · MySQL 8.4 (local development) |
| **Architecture** | Server Actions + Server Components + SSE |
| **UI Framework** | Metronic (Demo 1 backbone) |
| **Tổng modules** | 11 modules (M0–M9, MC, MF) |
| **Tổng bảng DB** | 99 bảng (đã kiểm đếm trên môi trường vận hành 23/07/2026) |
| **Trạng thái** | Production đã go-live (chi tiết hạ tầng không công khai) |

---

## 📋 Trạng Thái Modules (cập nhật 22/07/2026)

| Module | Tên | Status | Mô tả chức năng | Sub-routes |
|--------|-----|--------|------------------|------------|
| M0 | Hệ Thống | ✅ Ready | Danh mục chung, phòng ban, quy trình, auth/session, RBAC | 6 sub-routes |
| M1 | Danh Mục | ✅ Ready | Khách hàng, sản phẩm, vật tư, nhân sự, vị trí, bảng giá công đoạn | 10 sub-routes |
| M3 | CRM & Bán Hàng | 🔨 In Dev | Báo giá, đơn hàng, CRM, tính giá thủ công/tự động, pricing admin | 7 sub-routes |
| M4 | Sản Xuất | 🔨 In Dev | Lệnh sản xuất, phiếu điều in, gộp/tách LSX | 2 sub-routes |
| M5 | Kho Hàng | 📋 Phase 1 | NCC, mua hàng, nhập/xuất kho, kiểm kê, giao hàng | 10 sub-routes (skeleton) |
| M6 | Vận Hành HR | 🔨 In Dev | Chấm công, nghỉ phép, biến động nhân sự | 1 client module |
| M7 | Tiền Lương | 🔨 In Dev | Payroll, BHXH, TNCN lũy tiến, phiếu chi lương | 1 client module |
| M8 | Công Việc | 🔨 In Dev | Task management, tích hợp CRM M3-M8 | 1 sub-route (tasks) |
| M9 | Cổng Thông Tin | 📋 Planned | Customer portal, multi-session (kế thừa auth M0) | Placeholder |
| MC | Quản Lý Hợp Đồng | 🔨 In Dev | Quản lý hợp đồng (mẫu, chi tiết, workflow trạng thái) | 1 client module |
| MF | Tài Chính | 🔨 In Dev | Phiếu thu/chi, công nợ, ngân hàng, đối chiếu, nghiệm thu | 7 sub-routes |

### Tổng kết trạng thái:
- **✅ Ready (production):** 2 modules (M0, M1)
- **🔨 In Dev (đang phát triển):** 7 modules (M3, M4, M6, M7, M8, MC, MF)
- **📋 Planned/Phase 1:** 2 modules (M5, M9)

> 📂 Xem chi tiết tiến độ từng module tại [MODULE-PROGRESS.md](MODULE-PROGRESS.md)
>
> 🏆 **MỚI NHẤT (V0.323 · phát hành 23/07/2026) — Đợt R1/R1.1/R1.2.** Gia cố an toàn hiển thị nội dung HTML và bản in; sửa lỗi **không tạo được nhân viên**; khoá **mã Phòng Ban thành bất biến**; sửa lỗi **ô chọn Khuôn / Kiểu In hiện trống** trên form in; chuẩn hoá cơ cấu tổ chức lên **6 phòng ban**; kiểm chứng khả năng khôi phục dữ liệu bằng cách phục hồi thật vào môi trường tách biệt. Đã đưa lên vận hành thật, **không gián đoạn dịch vụ**. Chi tiết ở mục Changelog bên dưới.
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

#### ✅ Kết quả kiểm chứng sau triển khai

| Hạng mục | Kết quả |
|---|---|
| Phiên bản đang chạy | **V0.323** |
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
| **Mốc phiên bản hiện tại** | V0.323 |
| **Thời gian phát triển** | 18/01/2026 → 23/07/2026 (~6 tháng) |
| **Modules hoạt động** | M0, M1, M3, M4, M6, M7, M8, MC, MF (9/11) |
| **Modules planned / skeleton** | M5, M9 (2/11) |
| **Tổng bảng DB** | 99 bảng (kiểm đếm trên môi trường vận hành 23/07/2026) |
| **Skills hỗ trợ** | 60+ |
| **Node.js** | v24.14.1 (engines: >=20 <25) |

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
- **Notion workspace:** Synced qua Notion MCP

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
