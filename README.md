# App Quản lý bộ nhớ (bản đặc tả tái tạo)

Repo này chứa **bản đặc tả chi tiết** của app Quản lý bộ nhớ — app quản lý
bộ nhớ máy chủ Muse (tiếng Việt, mobile-first, 7 tab), hoàn thành ngày
2026-10-25.

## Vì sao repo này chỉ có đặc tả, không có mã nguồn?

App gốc là artifact web_fullstack trên Muse. Artifact fullstack **không có
chức năng export** nên mã nguồn gốc không lấy ra được. Repo này vì vậy
chứa đặc tả đủ chi tiết để một Muse khác build lại app mới có cùng chức
năng.

## Tài liệu trong repo

- `docs/UX_SPEC.md` — đặc tả đầy đủ: 7 tab (Tổng quan / App / File /
  Code / Giữ lại / RAM / Lịch sử), hành vi tự quét khi mở app, xem nhiều
  cấp có breadcrumb, tick → chờ xóa → thùng rác (khôi phục được), xóa app
  2 xác nhận rồi vĩnh viễn, khóa mặc định, tải ZIP từ file gốc, tab RAM đo
  mỗi lần mở, tab Lịch sử không backfill, đối chiếu 15 action thật của app
  gốc, và các quy tắc dữ liệu bất di bất dịch.

## Clone về Muse khác — hướng dẫn

1. Trên Muse mới, đăng nhập GitHub bằng tài khoản có quyền vào repo
   private này, clone repo.
2. Đọc kỹ `docs/UX_SPEC.md`.
3. Tạo artifact **web_fullstack** mới qua artifact builder của Muse, bám
   đúng đặc tả: 7 tab đúng thứ tự, 15 action đúng tên, các quy tắc dữ
   liệu (chỉ hiện số thật, RAM đo mỗi lần mở, lịch sử không backfill,
   app tự loại chính nó, khóa mặc định Asset Factory / vieneu-tts-api /
   cron bản tin sáng).
4. App mới tự quét máy chủ của nó khi mở — dữ liệu đo thật của máy chủ cũ
   (28 mục / 1,56 GB ngày 2026-10-25) chỉ là ví dụ, không phải dữ liệu seed.
5. Đây là **bộ nhớ máy chủ Muse, không phải điện thoại** — ghi rõ điều
   này trong giao diện.
