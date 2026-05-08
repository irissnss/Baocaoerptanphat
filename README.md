# 📊 Báo Cáo ERP Tân Phát — Tổng Quan & Changelog

> **Repo công khai** để Notion và các công cụ AI có thể đọc, hỗ trợ Owner quản lý dự án ERP Tân Phát.
>
> ⚠️ **CHỈ chứa báo cáo** — KHÔNG chứa source code, schema, credentials, hay dữ liệu nhạy cảm.

---

## 📌 Thông Tin Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Tên dự án** | ERP Tân Phát |
| **Version hiện tại** | `V0.216` |
| **Tổng cập nhật** | 216+ lần |
| **Ngày bắt đầu** | 18/01/2026 |
| **Cập nhật lần cuối** | 08/05/2026 |
| **Tech Stack** | Next.js 16 · React 19 · Tailwind 4 · MySQL |
| **Architecture** | Server Actions + Server Components + SSE |
| **UI Framework** | Metronic (Demo 1 backbone) |
| **Tổng modules** | 10 modules (M0–M9, MF) |
| **Tổng bảng DB** | 90+ bảng |

---

## 📋 Trạng Thái Modules

| Module | Tên | Status | Mô tả chức năng |
|--------|-----|--------|-----------------|
| M0 | Hệ Thống | ✅ Ready | Danh mục chung, phòng ban, quy trình, auth/session |
| M1 | Danh Mục | ✅ Ready | Khách hàng, sản phẩm, vật tư, bảng giá công đoạn |
| M3 | CRM & Bán Hàng | 🔨 In Dev | Báo giá, đơn hàng, CRM, tính giá tự động |
| M4 | Sản Xuất | 🔨 In Dev | Lệnh SX, phiếu điều in, gộp/tách LSX |
| M5 | Kho Hàng | 📋 Phase 1 | Nhà cung cấp, mua hàng, giao hàng |
| M6 | Nhân Sự | 📋 Planned | Vận hành HR, chấm công |
| M7 | Tiền Lương | 🔨 In Dev | Payroll, BHXH, TNCN lũy tiến |
| M8 | Công Việc | 🔨 In Dev | Task management, CRM integration |
| M9 | Cổng Thông Tin | 📋 Planned | Customer portal |
| MF | Tài Chính | 🔨 In Dev | Phiếu thu/chi, công nợ |

> 📂 Xem chi tiết tiến độ từng module tại [MODULE-PROGRESS.md](MODULE-PROGRESS.md)

---

## 📂 Cấu Trúc Báo Cáo

| File | Nội dung |
|------|----------|
| **README.md** | Tổng quan dự án, modules, thống kê (file này) |
| **[CHANGELOG-DETAIL.md](CHANGELOG-DETAIL.md)** | Changelog chi tiết V0.216 → V0.100 (gần đây) |
| **[CHANGELOG-ARCHIVE.md](CHANGELOG-ARCHIVE.md)** | Changelog archive V0.99 → V0.00 (cũ hơn) |
| **[MODULE-PROGRESS.md](MODULE-PROGRESS.md)** | Tiến độ chi tiết từng module + chức năng |
| **[GOVERNANCE-LOG.md](GOVERNANCE-LOG.md)** | Lịch sử governance, skills, architecture decisions |

---

## 📊 Thống Kê Tổng Hợp

| Metric | Giá trị |
|--------|---------|
| **Tổng version updates** | 216+ |
| **Thời gian phát triển** | 18/01/2026 → hiện tại (~4 tháng) |
| **Modules hoạt động** | M0, M1, M3, M4, M7, M8, MF (7/10) |
| **Modules planned** | M5, M6, M9 (3/10) |
| **DB migrations** | 50+ |
| **Skills tạo** | 60+ |
| **Governance files** | 5 files (5-way sync, SHA256 verified) |
| **Deploy iterations** | 20+ |

---

## 🔗 Liên Kết

- **Main repo (private):** `irissnss/tanphat-erp`
- **Báo cáo này (public):** [`irissnss/Baocaoerptanphat`](https://github.com/irissnss/Baocaoerptanphat)
- **Notion workspace:** Synced qua Notion MCP

---

> **Cập nhật lần cuối:** 08/05/2026 — V0.216
