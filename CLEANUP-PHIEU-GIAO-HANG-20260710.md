# 🧹 CLEANUP — Phiếu Giao Hàng + Full System Scan (V0.238 sót)

> **Plan ID:** PLAN-20260706-chung-tu-ke-toan-audit (cleanup bổ sung)  
> **Date:** 10/07/2026  
> **Mode:** CLEANUP — Dọn test data  

---

## R1 — phieu_giao_hang Test Data

### Trước xóa:
| Table | Records | Detail |
|-------|---------|--------|
| phieu_giao_hang | 1 | PGH-TEST-001, "Cong Ty TNHH SXTM Quang Cao Tan Phat", status=delivered |
| phieu_giao_hang_item | 2 | PGHI-TEST-001 (Hop Carton 3 Lop), PGHI-TEST-002 (Hop Carton 5 Lop) |

### Sau xóa:
| Table | Records |
|-------|---------|
| phieu_giao_hang | 0 ✅ |
| phieu_giao_hang_item | 0 ✅ (FK CASCADE) |

---

## R2 — Full System Scan (51 tables with ghi_chu column)

Quét toàn bộ 51 bảng có cột `ghi_chu` tìm `%TEST-DATA-CLEANUP-OK%`:

| # | Table | Status |
|---|-------|--------|
| 1-5 | bao_gia, bao_gia_item, bao_gia_option, bien_ban_nghiem_thu, chung_tu_ke_toan | ✅ 0 |
| 6 | **cong_no** | ❌ **2 records** → **ĐÃ XÓA** → ✅ 0 |
| 7 | cong_no_doi_chieu | ✅ 0 |
| 8-51 | 44 bảng còn lại (dm_*, don_hang*, phieu_*, hr_*, kho_*, ...) | ✅ Tất cả 0 |

### Phát hiện bất ngờ:
- **cong_no**: 2 test records còn sót từ V0.238 cleanup → đã xóa sạch

---

## Final Verification

```
cong_no                 0 ✅
phieu_giao_hang         0 ✅
phieu_giao_hang_item    0 ✅
phieu_thu               0 ✅
phieu_chi               0 ✅
chung_tu_ke_toan        0 ✅
bien_ban_nghiem_thu     0 ✅
cong_no_doi_chieu       0 ✅
```

**Kết luận:** TOÀN BỘ test data `TEST-DATA-CLEANUP-OK` = **0 records** trong 51 bảng. Hệ thống sạch.

---

## Bonus Fix: db.ts Pool Auto-Recovery

Trong quá trình khởi chạy local, phát hiện bug: `db.ts` pool bị cache trong `globalThis` khi MySQL chưa sẵn sàng, gây ECONNREFUSED vĩnh viễn. Đã fix:
- **Trước:** `query()` throw error, pool vẫn cached → lần sau vẫn lỗi
- **Sau:** `query()` catch ECONNREFUSED → destroy pool → retry 1 lần với pool mới

*Không chứa source code, schema, credentials.*
