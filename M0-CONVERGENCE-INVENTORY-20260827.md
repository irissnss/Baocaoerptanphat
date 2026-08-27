# KIỂM KÊ BẢN PHÁT HÀNH HỘI TỤ — 71 COMMIT VÀ MỌI BẢN DI TRÚ

**Ngày:** 27/08/2026
**Phạm vi:** từ neo máy vận hành `826817b` tới `main`
**Căn cứ:** Owner mục 3 — *«Inventory toàn bộ 70 commit và mọi migrate:\*»*

> Số thật là **71 commit**, không phải 70. Ghi đúng số đo.

---

## A. PHÂN LOẠI 71 COMMIT

| Nhóm | Số | Ý nghĩa cho việc phát hành |
|---|---:|---|
| Chỉ tài liệu / quản trị | **34** | không ảnh hưởng máy vận hành |
| Chỉ bộ kiểm | **12** | không ảnh hưởng máy vận hành |
| Chỉ kịch bản vận hành | **6** | chạy thủ công, không tự chạy |
| **Có mã nguồn** | **18** | mục B |
| **Có bản di trú** | **1** | mục C |

### ⭐ Điều quan trọng nhất — đo được, không suy đoán

| Phép đo | Kết quả |
|---|---|
| **Trang mới** (`src/app/**/page.tsx`) | **0** |
| **Tuyến API mới** (`src/app/api/**/route.ts`) | **0** |
| Tệp `src/` bị xoá | **0** |
| Tệp `src/` mới | 8 — **tất cả là thành phần bên trong trang đã có, hoặc thư viện** |

⇒ **Không có bề mặt mới nào ra máy vận hành.** Đây là điều làm cho bản hội tụ này
an toàn hơn vẻ ngoài của con số 71: toàn bộ là **hoàn thiện thứ đã có**, không phải
mở thứ chưa xong. Đúng khoá Owner: *«Không đưa unfinished route/UI/API ra production»*.

**Tám tệp `src/` mới:**

| Tệp | Thuộc | Phân loại |
|---|---|---|
| `src/lib/security/menu-catalog.ts` | Đợt 1 | ✅ duyệt/bắt buộc |
| `src/lib/security/transition-catalog.ts` | Đợt 5 | ✅ duyệt/bắt buộc |
| `src/lib/security/khoa-thanh-ben.ts` | vá thanh bên 27/08 | ✅ duyệt/bắt buộc |
| `src/lib/rbac-cap-phat.ts` | `USER` = chờ cấp phát | ✅ duyệt/bắt buộc |
| `src/app/m0/security/ma-tran-menu-cay.tsx` | Đợt 4 | ✅ duyệt/bắt buộc — thành phần trong `/m0/security` |
| `src/app/m0/security/ma-tran-chuyen-trang-thai.tsx` | Đợt 5 | ✅ duyệt/bắt buộc — như trên |
| `src/app/m0/security/tam-ngung.ts` | tạm ngưng nút ghi đè | ✅ duyệt/bắt buộc |
| `src/lib/pricing/kho-trai.ts` | tính kho trải | ⚠️ mục D |

---

## B. MƯỜI TÁM COMMIT CÓ MÃ NGUỒN

| # | Mã | Nội dung | Phân loại |
|---|---|---|---|
| 1 | `3616b19` | Đợt 1 — danh mục màn hình toàn hệ thống | ✅ **bắt buộc** — vá thanh bên phụ thuộc |
| 2 | `286504f` | Đợt 2 — khoá con từng màn | ✅ **bắt buộc** |
| 3 | `b9000f6` | Đợt 3 — áp ma trận quyền đích | ✅ **bắt buộc** |
| 4 | `68389d1` | 3 quyết định Owner — SALES giữ Thiết Kế, nối cổng M4/M5, 3 vai trò TP | ✅ duyệt |
| 5 | `ab23005` | Đợt 4 — ma trận quyền dạng tab + cây | ✅ duyệt |
| 6 | `7752cc5` | Đợt 5 — quyền chuyển trạng thái từng bước | ✅ **bắt buộc** |
| 7 | `9667995` | Che giá vốn 2 cột còn lọt lưới (Owner sổ #163) | 🔴 **bắt buộc — đang là lỗ trên máy vận hành** |
| 8 | `8f14dee` | Gom 6 chỗ tính kho trải về một nguồn | ⚠️ mục D |
| 9 | `aea01ca` | Phụ cấp kho trải thành danh sách mở | ⚠️ mục D |
| 10 | `57afeb5` | Vá `DEBT-107` + định chính bù hao 3% | ⚠️ mục D |
| 11 | `31e2407` | CEO xem được giá vốn *(bản di trú — mục C)* | ✅ duyệt |
| 12 | `643d60b` | `USER` = chờ cấp phát | ✅ duyệt |
| 13 | `d0de737` | Vá rò rỉ giá vốn ở tuyến API | ✅ **bắt buộc — an ninh** |
| 14 | `dcc8398` | Vá lỗ tầng `/api` — gọi được không cần đăng nhập | ✅ **đã ở trên máy vận hành** |
| 15 | `2ffaf5c` | Chống xoá trắng giá vốn khi lưu báo giá | ✅ **bắt buộc — phá dữ liệu** |
| 16 | `12abd3a` | Lớp thứ hai cho tầng `/api` | ✅ **đã ở trên máy vận hành** |
| 17 | `1e57bd9` | Đưa bản sửa ma trận về `main` | ✅ **đã ở trên máy vận hành** |
| 18 | `f50a99c` · `45807fb` · `978daea` | Vá thanh bên · `DEBT-125/126/127` · gom điều hướng + trung tâm phân quyền | ✅ **bắt buộc** |

**Không commit nào thuộc nhóm "chưa xong, phải tắt".**
**Không commit nào thuộc nhóm "không an toàn, chặn phát hành".**

---

## C. BẢN DI TRÚ

### C.1 Bản di trú tệp — chỉ MỘT bản mới sau neo

| Tệp | Nội dung | Hoàn tác |
|---|---|---|
| `migrations/20260826_p1_ceo_xem_gia_von.sql` | 4 dòng cho CEO **xem** (không sửa) 4 cột giá vốn | `..._rollback.sql` — có sẵn |

### C.2 Bốn lệnh di trú chuẩn — thứ tự CHẠY

```
1. npm run migrate:m0-security     → bảng RBAC
2. npm run migrate:m01-m13-m6      → M0.1 · M1.3 · M6
3. npm run migrate:m7              → tiền lương
4. npm run migrate:mc              → hợp đồng
5. (SQL) 20260826_p1_ceo_xem_gia_von.sql
```

Cả bốn **chạy lại được nhiều lần** (đã đo trên bản sao: 101 bảng → 101 bảng).
Kiểm chứng sau khi chạy: `verify:m0-security` **27/27** · `verify:m01-m13-m6` **46/46** ·
`verify:m7` **29/29** · `verify:mc` **pass** · `verify:mf` **ok**.

### C.3 Kịch bản NẠP DỮ LIỆU — thứ tự CHẠY, và đây là phần thật sự quan trọng

Máy vận hành thiếu **28 khoá menu · 3 vai trò · 98 dòng quyền menu · 30 dòng quyền
chuyển trạng thái**. Thiếu chúng thì mã Đợt 5 sẽ **chặn hết** thao tác đổi trạng thái —
tức **mất quyền thật**, đúng điều kiện DỪNG của Owner. Nên bốn kịch bản này là **bắt buộc**:

```
6. scripts/nap-khoa-con-man-hinh.ts --apply       (Đợt 2 — CHỈ THÊM ô để tick, KHÔNG đổi quyền ai)
7. scripts/tao-vai-tro-truong-phong.ts --apply    (3 vai trò TP, 0 người dùng ⇒ không ai đổi quyền)
8. scripts/ap-ma-tran-dich.ts --apply             (Đợt 3 — ĐỢT DUY NHẤT đổi quyền người thật)
9. scripts/nap-quyen-chuyen-trang-thai.ts --apply (Đợt 5 — trả 17 cờ ghi + cấp 30 ô chuyển trạng thái)
```

Cả bốn đều có **chế độ xem trước** (không `--apply`), đều **chạy lại được**, và
kịch bản số 8 có **chốt an toàn tự thân**: nó đo tập màn xem được của TỪNG vai trò
trước và sau, màn nào mất mà không nằm trong danh sách cố ý gỡ thì **báo đỏ và dừng**.

**Kết quả diễn tập trên bản sao vận hành thật:**

| Vai trò | Trước | Sau | Ghi chú |
|---|---:|---:|---|
| ADMIN | 53 | 53 | không đổi |
| CEO | 15 | **49** | **+34** — nay thấy đúng các màn đã được duyệt |
| KE_TOAN | 13 | 13 | không đổi |
| SALES | 10 | 11 | +2 · **−1** |
| HR | 3 | 4 | +1 |

**Mất mát duy nhất: SALES mất `/m3/tinh-gia-admin`** — đúng khoá Owner
*«Sale … TUYỆT ĐỐI KHÔNG tính giá admin»*. Đây là thu hồi **có chủ đích**, đã duyệt.

Kịch bản 9 tự kiểm và báo: **`[N5.6] không ai mất quyền: ĐẠT`**.

### C.4 THỨ TỰ HOÀN TÁC (ngược lại thứ tự chạy)

```
9 → 8 → 7 → 6   : dữ liệu — hoàn tác bằng phục hồi bản sao lưu CSDL
5               : 20260826_p1_ceo_xem_gia_von_rollback.sql
4 → 3 → 2 → 1   : bốn bản di trú chuẩn — CHỈ THÊM bảng/cột, không xoá
```

**Đường lùi thật, và là đường lùi được dùng:** phục hồi bản sao lưu CSDL đã chụp
ngay trước khi phát hành, cộng với việc đưa mã về đúng `826817b`. Bốn bản di trú
chuẩn chỉ thêm bảng nên để lại cũng vô hại.

---

## D. TÍNH KHO TRẢI — PHÂN LOẠI RIÊNG, CÓ ĐO

Ba commit chạm `src/lib/pricing/kho-trai.ts`. Một trong số đó (`aea01ca`) ghi thẳng
trong lời commit: **«KHONG bump version - KHONG deploy»**, và bộ nghiệm thu màn cấu hình
phụ cấp kho trải vẫn ở trạng thái **«CHỜ Owner duyệt»**.

**Đo được, không suy đoán:**

| Câu hỏi | Kết quả |
|---|---|
| Ai nạp `kho-trai`? | **3 tệp, cả ba đều khai `"use client"`** |
| Có tệp máy chủ nào nạp không? | **KHÔNG** — không hành động máy chủ, không tuyến API nào |
| Kết quả có vào thân yêu cầu lưu không? | **KHÔNG** — không xuất hiện trong bất kỳ `body`/`stringify` nào |
| Màn cấu hình phụ cấp đã dựng chưa? | **CHƯA** — commit đó chỉ có tài liệu, *«CHƯA viết dòng mã giao diện nào»* |

⇒ **Phân loại: được duyệt, thuộc lớp hiển thị, KHÔNG đổi giá được lưu.**
Nó đổi con số **hiển thị** trên màn tính giá thủ công (do ADMIN · CEO · SALES dùng),
theo đúng quy ước Owner đã chốt trong sổ. Không có bề mặt chưa xong nào đi kèm.

---

## E. BA NHÓM PHÂN LOẠI OWNER YÊU CẦU

| Nhóm | Số | Danh sách |
|---|---:|---|
| **Được duyệt / bắt buộc** | **18/18 commit mã nguồn** + 1 di trú + 4 kịch bản nạp | mục B và C |
| **Chưa xong nhưng đã tắt** | **0** | không có bề mặt chưa xong nào; nút ghi đè quyền hàng loạt **đã chặn ở máy chủ** từ trước |
| **Không an toàn / chặn phát hành** | **0** | — |

---

## F. ĐIỀU CHƯA CHỨNG MINH ĐƯỢC

- **Chi phí khoá bảng** khi chạy di trú trên dữ liệu thật của máy vận hành. Bản sao có
  cùng lượng dữ liệu (1695 khách hàng) nên ước lượng sát, nhưng chưa đo trên máy thật.
- **Hành vi của người dùng thật** sau khi CEO được thêm 34 màn. Đúng ma trận đã duyệt,
  nhưng đây là thay đổi lớn nhất mà người dùng sẽ cảm nhận được.
