# 0708 · Vòng sửa và kiểm nhanh của X-Templates

DHCB nhận bộ điều phối ba giai đoạn và CLI từ X-Templates `3a1cd54`. `loop verify` gom Gate tương đương theo thứ tự đầu tiên; bộ chia shard của CLI giao lớp có trọng số lớn trước trong từng shard. CI ứng dụng tiếp tục dùng Vitest và các workflow riêng của DHCB.

Khi dùng `/factory` trong monorepo, chạy `npm run dev` và `npm run test:watch -- <file-test>` để phản hồi ngay khi sửa. Bản trình kiểm lại bằng lệnh build, preview và bộ nghiệm thu của dự án.
