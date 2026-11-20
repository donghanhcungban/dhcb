---
name: factory
description: Điều phối toàn vòng đời sản phẩm, từ một câu ý tưởng webapp mới hoặc repo đang chạy tới MVP dùng thử trên trình duyệt, hoàn thiện, phát hành và cải tiến có số đo. Dùng khi muốn chạy xuyên ba giai đoạn, tiếp tục dự án đang dở hoặc dừng ở MVP để nhận phản hồi.
argument-hint: '[ý tưởng hoặc mục tiêu] [thư mục sản phẩm]'
---

# /factory — từ ý tưởng tới sản phẩm đang vận hành

Yêu cầu thử MVP trước thì dừng ở bản dev. Khi làm toàn vòng đời, tiếp tục theo sổ và quyền đã giao.

## Bắt đầu hoặc tiếp tục

- **Tiếp tục DHCB.** Đọc `CLAUDE.md`, `PROJECT.md`, `PROGRESS.md`, `xt.yaml`, PLAN và trạng thái Git. Dùng Node 24 và npm theo lockfile của monorepo. Nếu chưa có sổ giai đoạn, xác định phần việc hiện tại theo tài liệu dự án trước khi khởi tạo hoặc nhận lại sổ.
- Từ gốc sản phẩm, dùng `<XT>` cho `python3 .xtemplates/scripts/xt.py`. Khi bắt đầu hoặc nối phiên, đọc `python3 .xtemplates/scripts/xt_flow.py --json` một lần. Dùng `<XT> stage status --json` khi cần SHA và lịch sử duyệt. Gắn JSON từ `pr next --pr <N> --json` và `release-check --json` qua `--prs` và `--release` khi cần. Đọc lại sổ sau merge/ngắt phiên. Sửa sổ hỏng trước.
- Khi PLAN có việc độc lập, chạy `<XT> plan waves --plan <PLAN> --max 4 --json`. Trong phiên, chỉ chia task độc lập, có tập file không trùng. Giao một agent phụ trách mỗi file chung. Khi PR chờ CI, làm task độc lập đã mở. Một luồng dùng thử là một lát và một PR. Gom UI, API, dữ liệu và test phục vụ cùng kết quả. Chạy Gate cho từng task. Review PR theo `pr next`.

## Giai đoạn 1 — làm và thử MVP

Trong lúc viết mã, dùng `npm run dev` để Vite nạp thay đổi; `npm run test:watch -- scripts/skills-mirror.test.ts` chạy lại test khi sửa skill. Với thay đổi khác, truyền đường dẫn test tương ứng. Khi trình PR, điền đủ các mục trong `.github/pull_request_template.md`.

1. Với tính năng, chốt SPEC và test nghiệm thu độc lập trước PR triển khai theo `docs/AI_DELIVERY_LOOP.md`. Ghim SHA của SPEC và test đã review. Dùng checkout khung đã duyệt chạy `python3 <khung>/scripts/xt_acceptance.py check --base <SHA-mốc> --project <repo-sản-phẩm> <file-SPEC> <file-test>`; truyền đầy đủ các file cấu hình nghiệm thu đã duyệt. Trong PLAN, ghi cả phép thử nghiệp vụ với dữ liệu mẫu và ca từ chối.
2. Commit lát đã kiểm, dùng `npm run build` và `npm run preview` cho bản thử cục bộ. Chạy `npm run test:e2e` theo phạm vi khi sửa UI/routing/auth/API hoặc luồng học. Ghi URL, SHA và kết quả. Thử lại sau khi mở phiên mới. PR dùng quy tắc review và phát hành trong `CLAUDE.md`.
3. Trình đúng bản trên cây sạch bằng `<XT> stage submit --evidence <bằng-chứng>`. Với MVP quy mô S ở pha build có smoke, máy dời audit tổng thể và các MUST ngoài PLAN sang pha hoàn thiện. Gửi cách thử và xin phản hồi một lượt. Nếu người dùng yêu cầu xem MVP, giữ `awaiting_approval` dù chính sách cho phép agent duyệt. Khi họ yêu cầu sửa, dùng `stage reopen --reason`, sửa, thử và trình lại. Người duyệt hoặc agent đủ quyền dùng `stage approve --sha <SHA> --note <ý-kiến>`. Khi thiếu remote, giao bundle Git đã kiểm bằng `git bundle verify`.

## Giai đoạn 2 — audit và chuẩn bị ứng viên

Chạy `/complete` trên PLAN harden, `<XT> admin-plan` sau receipt dev xanh và `<XT> release-check --json`. Sửa lỗi audit, diễn tập bằng `<XT> rollout-drill --env prod --release <file>`. Gom kết quả và quyền còn thiếu, dùng `stage submit` trình giai đoạn 2. Mặc định giữ ứng viên tới sau tối ưu và kiểm thử ở giai đoạn 3. Phát hành sớm chỉ khi chính sách giao hàng của sản phẩm yêu cầu rõ và đủ điều kiện production dưới đây.

## Giai đoạn 3 — tối ưu, production, vận hành, tăng trưởng

1. Chọn chỉ số trong `PROJECT.md`, đo baseline trên dev. Với sản phẩm đã chạy, lấy baseline field của bản đang phục vụ. Dùng `/optimize` theo mục tiêu và ngân sách; đổi hành vi qua PLAN giai đoạn 3 bằng `/autopilot`. So trước/sau cùng điều kiện, chạy Gate E2E, smoke và lấy receipt dev cho đúng artifact.
2. Với ứng viên sau tối ưu, chạy lại `release-check --json` và diễn tập. Khi được giao triển khai, kiểm grant, HEAD trên nhánh chính remote, chữ ký §5 GO-NO-GO và `promotion.state` = `approved` đúng ứng viên. Chạy runbook, xác nhận digest và smoke **tại đích** mới ghi đã phát hành. Thiếu quyền hay guard chặn thì giao ứng viên cùng mục chờ.
3. Đối chiếu receipt với field cùng bản bằng `<XT> ops-check --env <môi-trường> --field <file> --max-p95-ms <ms> --max-error-pct <%> --json`. Giao SLO và runbook sự cố. Thử nghiệm tăng trưởng cần mục tiêu, quyền dữ liệu và ngân sách. Trình SHA, số đo và tách số lab/field. Khi `completed`, báo bản đã thử/ký/phát hành, thời gian, chi phí (`null` nếu chưa đo), người phụ trách việc chờ. Mục tiêu mới mở chu kỳ mới.
