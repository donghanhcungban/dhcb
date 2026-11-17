# 0705 — 2026-11-16 — Nâng nhóm development-dependencies (prettier 3.10.1) kèm định dạng lại 5 tài liệu

## Phạm vi

- Thay thế PR Dependabot #1496: cùng 11 bản nâng dev (`@playwright/test` 1.65.0, `@tailwindcss/postcss` và
  `tailwindcss` 4.3.5, `@typescript-eslint/*`/`typescript-eslint` 8.72.2, `@vitejs/plugin-react` 6.3.0,
  `dotenv` 18.0.10, `prettier` 3.10.1, `rollup-plugin-visualizer` 7.1.3, `vite` 8.3.8) — `package.json` và
  `package-lock.json` lấy nguyên từ commit của Dependabot.
- #1496 đỏ `Type + Lint + Format` vì `prettier` 3.10.1 tính lại độ rộng ô bảng Markdown có ký tự tổ hợp
  (mũi tên vectơ `⃗`) nên 5 tệp trong `docs/research/` lệch định dạng. Chạy `prettier --write` đúng 5 tệp đó;
  chỉ đổi khoảng trắng đệm trong bảng, không đổi nội dung.

## Kiểm chứng

- `npm run format:check`, `npm run typecheck`, `npm run lint`, `npm test` — kết quả ghi trong thân PR.
