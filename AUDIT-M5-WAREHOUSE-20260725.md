# 📦 BÁO CÁO TRẠNG THÁI M5 — KHO HÀNG (cập nhật 25/07/2026)

> **Tầng thông tin:** LOCAL (code + DB thật) · **Do:** Agent IDE (Claude Code)
> **Mục đích:** Đối chiếu chéo 3 tầng (Local ↔ Notion ↔ Public Report) để AI tools/Owner điều chỉnh tài liệu cho khớp thực tế.
> **Phạm vi:** Chỉ báo cáo trạng thái/tiến độ chức năng — KHÔNG chứa source code, schema, tên bảng, credentials.

---

## 1. TÓM TẮT (đọc trước)

**M5 Kho Hàng KHÔNG phải "skeleton".** Toàn bộ **10 khu vực chức năng** đều đã có **CRUD đầy đủ, chạy trên dữ liệu thật**, có phân quyền (RBAC) và cập nhật thời gian thực (SSE).

Nhãn cũ "📋 Phase 1 — skeleton" trong `README.md` và `MODULE-PROGRESS.md` **đã lỗi thời** và được sửa trong đợt này.

Khoảng trống còn lại **không nằm ở giao diện/CRUD**, mà ở **tự động hoá liên chứng từ tồn kho** (xem mục 4).

---

## 2. HIỆN TRẠNG 10 KHU VỰC CHỨC NĂNG

| # | Khu vực | Trạng thái | Ghi chú |
|---|---------|-----------|---------|
| 1 | Nhà cung cấp | ✅ CRUD đầy đủ | Có kiểm tra ràng buộc trước khi xoá |
| 2 | Mua hàng (đơn mua) | ✅ CRUD đầy đủ | Quản lý cả đầu mục + dòng chi tiết |
| 3 | Nhập kho | ✅ CRUD đầy đủ | Đầu mục + dòng chi tiết |
| 4 | Xuất kho | ✅ CRUD đầy đủ | Đầu mục + dòng chi tiết |
| 5 | Giao hàng | ✅ CRUD đầy đủ (hoàn thiện nhất) | Có lọc theo phụ trách (ownership) cho sale |
| 6 | Sổ giao dịch kho | ✅ Ghi nhận (append-only) | Nhật ký kho, không sửa theo thiết kế |
| 7 | Kiểm kê kho | ✅ CRUD đầy đủ | |
| 8 | Kho thành phẩm | ✅ CRUD đầy đủ | Nhận thành phẩm tự động từ Sản Xuất (M4) khi lệnh hoàn thành |
| 9 | Vật tư ↔ Nhà cung cấp (mapping) | ✅ CRUD đầy đủ | Master data |
| 10 | Giá vật tư | ✅ CRUD đầy đủ | Lịch sử giá |

**Nền tảng kỹ thuật:** Server Components + Server Actions + SSE, phân quyền theo module, master data đầy đủ. Mức "một trang CRUD trên nghiệp vụ của nó" → **coi như đã dựng xong**.

---

## 3. CẬP NHẬT SO VỚI BÁO CÁO M5 NGÀY 09/07/2026

Báo cáo `AUDIT-M5-WAREHOUSE-20260706.md` (09/07) nêu một số khoảng trống — nay đã thay đổi:

| Vấn đề cũ (09/07) | Trạng thái mới (25/07) |
|---|---|
| Thiếu master data "Vật tư ↔ NCC" và "Giá vật tư" | ✅ **Đã có** — 2 khu vực này chạy CRUD thật |
| Thiếu phân quyền (RBAC) trên nghiệp vụ M5 | ✅ **Đã có** — thao tác M5 được gate theo quyền module |
| Tích hợp Sản Xuất (M4) → Kho thành phẩm | ✅ **Đã có** — lệnh sản xuất hoàn thành sinh thành phẩm vào kho |

→ M5 đã tiến bộ rõ so với đầu tháng 7. Trạng thái nên phân loại là **"Đang phát triển — CRUD đầy đủ, còn thiếu tự động hoá tồn kho"**, KHÔNG phải "skeleton/Phase 1".

---

## 4. KHOẢNG TRỐNG CÒN LẠI (ưu tiên cho đợt tới)

Đây là phần nghiệp vụ **tự động hoá**, không phải dựng trang mới:

1. **Chưa tự động cập nhật tồn kho giữa các chứng từ.** Nhập/Xuất kho hiện chưa **tự động ghi vào sổ giao dịch kho** và chưa **tự tăng/giảm tồn thành phẩm** — hiện phải ghi nhận thủ công.
2. **Giao hàng chưa tự trừ tồn thành phẩm** khi xác nhận giao.
3. **Liên kết ngược Mua hàng ↔ Nhập kho** (đã nhận / còn lại) chưa tự động cập nhật.
4. **Tích hợp Tài Chính (MF):** "Đơn mua hoàn tất → công nợ phải trả", "Giao hàng → công nợ phải thu" — chưa nối.
5. **In phiếu** (Nhập/Xuất/Giao) chưa gắn với mẫu in.

**Thứ tự đề xuất (theo chuỗi phụ thuộc NCC → Mua → Nhập → Tồn → Xuất → Giao):** dựng "sổ cái tồn kho" làm nguồn sự thật trước → nối tự động vào Nhập → Mua hàng → Xuất → Giao → Kiểm kê.

---

## 5. KẾT LUẬN

- **M5 = "Đang phát triển" (CRUD đầy đủ + RBAC + realtime), KHÔNG phải skeleton.**
- Việc còn lại là **tầng tự động hoá tồn kho + tích hợp liên module**, không phải giao diện.
- Đề nghị AI tools/Notion cập nhật lại mô tả M5 cho khớp trạng thái này.

---

> **Attribution:** Báo cáo do **Agent IDE (Claude Code)** lập từ **tầng LOCAL (code/DB thật)**. Việc điều chỉnh tài liệu **Notion** thuộc **Agent Notion / TanPhatAI**. Điều phối/tổng hợp thuộc **Cowork Coordinator**.
> *Báo cáo chỉ chứa trạng thái/tiến độ chức năng. Không chứa source code, schema, tên bảng, đường dẫn file, credentials.*
