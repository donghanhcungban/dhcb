# 0707 — 2026-11-19 — `formatDateVN` không làm sập trang khi gặp ngày hỏng (AUD-CODE-003)

## Vấn đề

Audit delta 2026-11-06 (`docs/ops/AUDIT-REPORT-2026-11-06-delta.md`): `formatDateVN(iso)` gọi
`vnDateStr(new Date(iso))`; với ngày không hợp lệ, `Intl` ném `RangeError: Invalid time value`. Hàm được
gọi thẳng trong lúc render ở Ghi chú (`Notes.tsx`), Kanban (`NotesKanban.tsx`), banner khuyến mãi
(`PromoEndingBanner.tsx`) và bộ chọn giọng (`VoicePicker.tsx`) → một giá trị xấu làm sập cả trang. Rủi ro
thấp (server kiểm `z.string().datetime()`, cột `timestamptz`) nhưng dữ liệu ngoài không đáng làm sập UI.

## Sửa

- `apps/dhcb/src/lib/promoEndingBanner.ts`: ngày không hợp lệ → `—`. Sửa ở hàm dùng chung nên phủ cả
  4 nơi gọi; ngày hợp lệ giữ nguyên hành vi (giờ Việt Nam).

## Kiểm chứng

- Đỏ trước: 3 ca mới (`'bad'`, `''`, `'2026-13-45T00:00:00Z'`) đều ném `RangeError`. Sau sửa: xanh.
- `vitest` `promoEndingBanner` + `pages/domains/notes` + `components`: 84 file / 1.248 test xanh;
  `tsc -p apps/dhcb/tsconfig.json`, eslint `--max-warnings 0`: đạt.
- Lưu ý môi trường: `tsc` ban đầu báo lỗi ở `srs.ts` do `node_modules` cũ (`ts-fsrs` 5.x, lockfile đã
  6.0.0 sau dependabot) — `npm ci` xử lý, không phải lỗi của thay đổi này.
