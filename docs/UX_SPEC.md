# App Quản lý bộ nhớ — Mô tả UX + chức năng chi tiết (để tái tạo app)

Tài liệu này mô tả **đầy đủ giao diện và hành vi** của app Quản lý bộ nhớ
để một Muse khác tái tạo lại gần giống nhất chức năng app hiện tại.

> Bối cảnh quan trọng: app gốc là **artifact web_fullstack trên Muse**,
> mã nguồn của nó **không lấy ra được** (artifact fullstack không có chức
> năng export). Repo này vì vậy **không chứa mã nguồn gốc** mà chứa bản
> đặc tả chi tiết này — đủ để build lại app mới có cùng chức năng. Khi
> build lại, tạo artifact web_fullstack mới qua artifact builder của Muse.

## 1. App là gì

- Tên: **Quản lý bộ nhớ**. Ngôn ngữ: tiếng Việt. Kiểu hiển thị:
  mobile-first. Loại: web_fullstack **riêng tư** (không có public link).
- Quản lý **bộ nhớ máy chủ Muse** (không phải bộ nhớ điện thoại của người
  dùng): đo dung lượng các app, file, code, RAM của máy chủ; cho dọn dẹp
  có kiểm soát (khóa, thùng rác, khôi phục, xóa vĩnh viễn).
- Hoàn thành ngày 2026-10-25. Số liệu đo thật ngày đó: 28 mục, tổng
  1,56 GB (trong đó vieneu-tts-api 900 MB, asset-factory 470 MB). Đây là
  số liệu ví dụ, KHÔNG phải dữ liệu seed — app mới phải đo thật từ máy
  chủ của nó.

## 2. Cấu trúc giao diện — 7 tab (đúng thứ tự)

1. **Tổng quan** — toàn cảnh bộ nhớ: tổng dung lượng, các mục lớn nhất,
   trạng thái chung.
2. **App** — danh sách các app trên máy chủ (kèm dung lượng, số file).
3. **File** — duyệt file theo cây thư mục.
4. **Code** — duyệt mã nguồn.
5. **Giữ lại** — các mục đang bị khóa (không được xóa).
6. **RAM** — đo RAM máy chủ **mỗi lần mở tab** (không phải số liệu tĩnh).
7. **Lịch sử** — nhật ký thao tác (xem mục 5).

## 3. Hành vi cốt lõi

### 3.1. Tự quét khi mở app
- Mỗi lần mở app, app **tự quét app thật** trên máy chủ:
  - App mới thêm vào thì xuất hiện, mặc định **chưa khóa**.
  - App tự loại **chính nó** khỏi danh sách (không tự đo chính mình).
  - Không tạo mục trùng, không đổi trạng thái khóa/thùng rác cũ.

### 3.2. Xem bên trong nhiều cấp
- Bấm vào một mục để xem sâu nhiều cấp, có **breadcrumb** điều hướng.
- Mỗi cấp hiển thị **dung lượng thật và số file thật** của cấp đó.
- Tải được **từng mục con** riêng lẻ (không bắt buộc tải cả cây).

### 3.3. Thông tin thời gian — chỉ hiện cái có thật
- Mỗi mục hiện **"Sửa lần cuối" thật** (lấy từ hệ thống).
- **"Tạo" chỉ hiện khi có thật** (nếu hệ thống không có thì bỏ trống,
  không bịa).
- Giờ hiển thị theo **giờ Việt Nam (Asia/Ho_Chi_Minh)**.

### 3.4. Chọn để xóa — 3 trạng thái
- Tick chọn mục → mục chuyển sang trạng thái **chờ xóa** (chưa mất).
- Từ chờ xóa, đưa vào **thùng rác** (vẫn **khôi phục được**).
- **Xóa app** yêu cầu **2 lần xác nhận**, sau đó là **xóa vĩnh viễn**
  (không khôi phục được). Thùng rác và xóa vĩnh viễn là hai bước khác
  nhau, không gộp.

### 3.5. Khóa (Giữ lại)
- Mục bị khóa thì không xóa được, nằm ở tab **Giữ lại**.
- **Khóa mặc định** (theo quyết định người dùng): Asset Factory,
  vieneu-tts-api, và cron bản tin sáng. Khi tái tạo, giữ đúng 3 khóa
  mặc định này (người dùng có thể mở khóa thủ công sau).

### 3.6. Tải về máy
- Tải về bằng **file ZIP từ file gốc** (đóng gói đúng file thật trên máy
  chủ, không phải bản sao đã qua xử lý).

### 3.7. Tab Lịch sử
- Ghi lại thao tác của người dùng **từ thời điểm tab này có hiệu lực về
  sau**.
- **Không dựng lại lịch sử cũ** (không bịa log cho thời gian trước khi
  tính năng tồn tại).

## 4. Đối chiếu action (để implement đúng)

Các action phía server của app gốc (tên action thật, dùng làm hợp đồng
khi build lại):
- `scanapps` — quét app thật trên máy chủ.
- `listitems` — liệt kê các mục (app/file/code) kèm dung lượng, số file.
- `listparts` — liệt kê các mục con trong một mục (duyệt nhiều cấp).
- `listitemtimes` — lấy thời gian Sửa lần cuối / Tạo (thật) của mục.
- `getramstatus` — đo RAM máy chủ tại thời điểm gọi.
- `listhistory` — đọc nhật ký thao tác.
- `setlocked` — khóa / mở khóa một mục.
- `queueitems` — đưa mục vào hàng chờ xóa.
- `unqueueitems` — bỏ mục khỏi hàng chờ xóa.
- `trashqueued` — đưa các mục đang chờ xóa vào thùng rác.
- `restoreitems` — khôi phục mục từ thùng rác.
- `preparedownload` — chuẩn bị file ZIP tải về từ file gốc.
- `deleteappsforever` — xóa vĩnh viễn (sau 2 xác nhận).
- `stopjob` — dừng một job đang chạy (quét/tải).
- `seedinventory` — khởi tạo danh mục ban đầu (chỉ dùng khi app trống,
  dữ liệu vận hành sau đó lấy từ quét thật).

## 5. Quy tắc dữ liệu (bất di bất dịch)

- Đây là **bộ nhớ máy chủ Muse, không phải điện thoại** — ghi rõ điều này
  trong giao diện để người dùng không nhầm.
- Dung lượng, số file, thời gian: **chỉ hiện số thật đo được**, không
  ước lượng, không bịa.
- Tab RAM đo **mỗi lần mở**, không cache số cũ.
- Lịch sử **không backfill** (không dựng lại quá khứ).
- App **tự loại chính nó** khỏi kết quả quét.
- Khóa mặc định: Asset Factory, vieneu-tts-api, cron bản tin sáng.
- Xóa app: 2 xác nhận rồi mới vĩnh viễn; thùng rác thì khôi phục được.

## 6. Những gì KHÔNG có trong repo này (và không chuyển theo)

- Mã nguồn gốc của artifact (không lấy ra được — xem đầu tài liệu).
- Dữ liệu vận hành (`app.db`, danh sách đo thật của máy chủ cũ): app mới
  tự quét lại từ máy chủ của nó.
- File ZIP tải về của người dùng cũ.
