# REGISTRY — BẢNG PHÂN XỬ NGUỒN CHUẨN GIAO DIỆN (ui-standard-sources)


> ⚠️ **BẢN CHỤP MỘT THỜI ĐIỂM (19/08/2026) — HÃY KIỂM CÒN HẠN HAY KHÔNG.**
> **Mã băm BẢN GỐC trong kho riêng tư tại thời điểm chụp (SHA-256):** `494ab485b147bee12b7643c191db7f73b1520fdd9134a2ae387d602aba69a227`
> Đây là bản chụp của bảng phân xử nguồn chuẩn giao diện. Nội dung dưới đây **khớp đúng bản gốc** tại thời điểm chụp.
> **Cách kiểm:** chạy `sha256sum` trên **file gốc trong kho riêng tư**.
> Khác mã trên → bản công bố này đã **LỖI THỜI**, tra kho riêng tư để lấy bản đang có hiệu lực.
> *(Cố ý ghi mã của BẢN GỐC chứ không phải của chính file này — vì thêm dòng cảnh báo sẽ làm đổi mã của chính nó, thành ra không kiểm được.)*
> *(Lưu ý: file trong kho báo cáo có thể bị đổi kiểu xuống dòng khi tải về, nên mã băm của CHÍNH file này sẽ khác bản gốc — đó là lý do phải hash bản gốc.)*

> **Doc Version:** 1.0 · **Lập:** 18/08/2026 bởi Agent IDE · **Owner duyệt:** 18/08/2026 (quyết định **Q2** + **Q3**)
> **Mục đích:** ca thất bại 17–18/08/2026 có **10 nguồn** quy định giao diện, mỗi nguồn tự xưng chuẩn, **không nguồn nào có nhãn hiệu lực**. Bảng phân xử cũ (`legacy-rules-status.md`) **chỉ phủ mục có mã `GOV-*`** nên toàn bộ khối UI nằm ngoài. File này lấp đúng chỗ trống đó.
> **Nguyên tắc:** `GOV-EDIT-PRESERVE-001` — **GỘP TRƯỚC, HẠ NHÃN SAU. Không xoá bất kỳ nguồn nào.** Mọi nguồn bị hạ nhãn vẫn còn nguyên văn tại vị trí cũ.

**Chú giải nhãn:**

| Nhãn | Nghĩa | Được dùng làm chuẩn thi hành? |
|---|---|---|
| **SSOT** | Nguồn chân lý duy nhất. Rút từ code đang chạy. | ✅ CÓ — và **bắt buộc đọc TOÀN PHẦN** trước mọi sửa giao diện (`GOV-READ-STANDARD-001`) |
| **MERGED-INTO-SSOT** | Nội dung còn đúng **đã được gộp** vào SSOT. Bản gốc giữ làm lưu trữ. | ❌ KHÔNG — tra SSOT |
| **SUPERSEDED** | Bị thay bởi quy định mới hơn ở SSOT. Giữ nguyên văn làm lịch sử. | ❌ KHÔNG |
| **HISTORICAL** | Chỉ còn giá trị tham khảo ngữ cảnh. | ❌ KHÔNG |

---

## BẢNG PHÂN XỬ — 10/10 NGUỒN

| # | Nguồn | Vị trí | Cập nhật cuối | **NHÃN** | Căn cứ |
|---|---|---|---|---|---|
| **S1** | **UI STANDARD — ERP Tân Phát** | `docs/UI-STANDARD.md` | 18/08/2026 | 🟢 **SSOT** | Quyết định Owner **Q2** 18/08/2026: SSOT = nguồn rút từ code đang chạy. Bằng chứng định vị: (a) file khai "Rút TỪ CODE THẬT" ở dòng 6; (b) 5 file mã nó trỏ tới đều tồn tại thật; (c) brand `#f97316` khớp `src/app/globals.css` |
| **S2** | UI & FORMAT RULES | `.governance/ARCHIVE-LEGACY-RULESET.md:150–158` | archive 16/08/2026 | 🟡 **MERGED-INTO-SSOT** (phần tiền) + 🔴 **SUPERSEDED** (phần ngày) | Định dạng tiền `1.234.567,89` → gộp vào **SSOT §17.2** (khớp, không xung đột). Định dạng ngày `DD/MM/YY` → **SUPERSEDED** bởi **SSOT §17.1** `DD/MM/YYYY` (Owner chốt "ngày đầy đủ", Sổ #65/#67). Phân xử: SSOT §20.7 xung đột #7 |
| **S3** | **METRONIC UI MANDATORY PROTOCOL** (nền tảng UI trả phí) | `.governance/ARCHIVE-LEGACY-RULESET.md:159–177` | archive 16/08/2026 | ⚪ **HISTORICAL** | Quyết định Owner **Q3** 18/08/2026. Hai lý do: (1) **12 lượt quét giao diện 17–18/08 chưa lần nào tra Metronic**; (2) file mà nó khai "**bắt buộc phải đọc**" — `docs/METRONIC_UI_RESEARCH_PROTOCOL.md` — **KHÔNG TỒN TẠI** (`find . -name "METRONIC_UI_RESEARCH_PROTOCOL*"` → 0 kết quả), vi phạm `GOV-REF-EXISTS-001` |
| **S4** | Master List DataTable UI Standard | `.governance/ARCHIVE-LEGACY-RULESET.md:1993–2030` | archive 16/08/2026 | 🔴 **SUPERSEDED** | 3 điểm ngược SSOT: nền `<th>` opaque vs không-set-nền (§20.7 #8) · thanh lọc ở khu riêng vs trong card bảng (§20.7 #10) · `max-h-[60vh]` vs `100dvh` |
| **S5** | Detail Panel — bố cục nút thao tác | `.governance/ARCHIVE-LEGACY-RULESET.md:2032–2045` | archive 16/08/2026 | 🔴 **SUPERSEDED** | Ngược SSOT §10: nút bên trái + tên bên dưới vs tên trên cùng + nút bên phải. Owner nghiệm thu "ổn rồi đó" (Sổ #76) trên bản name-on-top (§20.7 #9) |
| **S6** | UI Typography Standard + Title Auto Case | `.governance/ARCHIVE-LEGACY-RULESET.md:2076–2110` | archive 16/08/2026 | 🟡 **MERGED-INTO-SSOT** | **KHỚP** SSOT §13 (font Inter · `toVietnameseTitleCase()`) và §8 (đầu bảng `text-[11px] font-bold uppercase`). Không xung đột |
| **S7** | TANPHAT ERP — UI STANDARD v1.0 | `docs/ke-thua-antigravity/TANPHAT_UI_STANDARD.md` | 27/04/2026 | 🔴 **SUPERSEDED** | 4 điểm ngược SSOT: card 12px vs `rounded-md` 6px (SSOT dòng 25 **CẤM** 12px trên card) · lề trang 30px vs lề shell 16px · dòng bảng 52px vs `py-2.5` · lưới 8px vs `gap-2`/`px-3`. Bản này theo nền tảng trả phí — cùng hướng đã hạ nhãn ở S3 (§20.7 #11) |
| **S8** | **11 kỹ năng UI** (mỗi cái tự xưng "SSOT") | `.cursor/skills/{master-list-page-template · detail-panel-layout · premium-table-styling · module-color-palette · ui-typography · status-color-mapping · icon-style-guideline · ui-components-usage · audit-ui · screenshot-verification · annotated-screenshot-review}/SKILL.md` | 03–08/2026 | 🟡 **MERGED-INTO-SSOT** (11/11) | Nội dung đã gộp vào **SSOT §20** (bảng đối chiếu kỹ năng→mục ở §20.1). **6 xung đột** phân xử ở §20.7 #1–#6, SSOT thắng. ⚠️ Nguyên nhân phải gộp: `.claude/` chỉ có `settings.local.json` → **Claude Code KHÔNG đọc được `.cursor/skills/`**. **Thư mục gốc GIỮ NGUYÊN làm lưu trữ — không xoá, không sửa** |
| **S9** | GLOBAL UX STANDARD G1 (form G2) + G2-ROLLOUT | `docs/AUDIT-GLOBAL-UX-STANDARD-G1-CORE.md` · `docs/AUDIT-GLOBAL-UX-STANDARD-G2-ROLLOUT.md` | 30/12/2025 | 🟡 **MERGED-INTO-SSOT** | Bộ G2 (`useFormDirtyTracker` · `useSafeClose` · `ConfirmExitDialog` · `ConfirmActionDialog`) đã có ở **SSOT §11** dòng "G2 bắt buộc". Hai file gốc giữ làm hồ sơ kiểm tra chi tiết API từng hook |
| **S10** | UI Grouped Tree View | `docs/UI-GROUPED-TREE-VIEW.md` | 30/12/2025 | ⚪ **HISTORICAL** | Mẫu cây phân nhóm cho danh mục nhiều cấp — **không xung đột** SSOT nhưng cũng **không thuộc** 4 trang nền tảng. Khi cần dựng cây phân nhóm thì tra file này, và kết quả phải khớp §1 §2 §3 của SSOT |

**Tổng kết nhãn:** SSOT **1** · MERGED-INTO-SSOT **4** (S2 phần tiền · S6 · S8 · S9) · SUPERSEDED **4** (S2 phần ngày · S4 · S5 · S7) · HISTORICAL **2** (S3 · S10) — *S2 xuất hiện ở hai nhãn vì hai phần nội dung khác nhau.*

---

## QUY TẮC ÁP DỤNG

1. **Đụng giao diện → đọc `docs/UI-STANDARD.md` TOÀN PHẦN.** Không đọc lỗ khoá. (`GOV-READ-STANDARD-001`)
2. **Hai nguồn lệch nhau → SSOT thắng.** Không tự chọn. Bảng phân xử chi tiết: SSOT **§20.7** (11 xung đột đã phân xử).
3. **SSOT lệch code đang chạy → DỪNG, báo Owner.** Không tự sửa SSOT theo code, cũng không tự sửa code theo SSOT — vì chính lỗi này đã gây lượt bác #75 (tài liệu ghi `rounded-full`, code mẫu dùng `rounded-md`, agent làm theo tài liệu nên làm sai).
4. **Nguồn HISTORICAL/SUPERSEDED vẫn đọc được** để tra ngữ cảnh, **nhưng không được dùng làm chuẩn thi hành**.
5. **Thêm nguồn mới → phải thêm một hàng vào bảng này** cùng lượt, kèm nhãn. Nguồn không có nhãn = không có hiệu lực.

---

## LỊCH SỬ SỬA ĐỔI

| Ngày | Doc Version | Người sửa | Lý do | Nội dung |
|---|---|---|---|---|
| 18/08/2026 | `1.0` | Agent IDE (Owner duyệt 18/08/2026) | Ca thất bại giao diện 17–18/08: 10 nguồn chuẩn, 0 nhãn hiệu lực, bảng phân xử cũ chỉ phủ mã `GOV-*` → toàn bộ khối UI không được phân xử (báo cáo `docs/reports/TU-SOAT-LUAT-UI-20260818.md` §2) | Tạo mới. Gán nhãn 10/10 nguồn theo Q2 + Q3. **Không xoá nguồn nào** |
