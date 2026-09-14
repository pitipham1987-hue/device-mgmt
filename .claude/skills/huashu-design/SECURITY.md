# Tuyên bố An toàn & Dòng dữ liệu (Security & Data-Flow Statement)

Tài liệu này tuyên bố đầy đủ và minh bạch về mọi điểm đến mạng, điểm chạm chứng thư credential, tiến trình con (subprocess), và thao tác xóa tệp mà skill này có thể thực hiện, để người dùng và các công cụ đánh giá an ninh tự động có thể đối soát và xác minh code.

**TÓM TẮT DỄ HIỂU: Quy trình cốt lõi (Thiết kế HTML → render → xuất MP4/PDF/PPTX) chạy 100% cục bộ với zero cuộc gọi mạng và zero API key. Tất cả mã nguồn có chạm đến cloud đều được cách ly trong `scripts/cloud/`, hoàn toàn là tùy chọn, chỉ sử dụng key do chính bạn cung cấp, chỉ gửi dữ liệu đến API chính thức của nhà cung cấp tương ứng, và từ chối chạy nếu không có sự đồng ý rõ ràng (cờ `--yes` hoặc biến `HUASHU_CLOUD_OK=1`). Không có telemetry. Tuyệt đối không có dữ liệu nào được gửi về bất kỳ máy chủ nào do tác giả skill kiểm soát.**

## Danh sách đầy đủ các điểm đến mạng (Network Destinations)

| Host / Tên miền | Ở đâu | Dữ liệu gì được gửi | Khi nào |
|---|---|---|---|
| `ark.cn-beijing.volces.com` (Volcengine Ark, API chính thức của ByteDance) | `scripts/cloud/ai-review-video.py` | Phân đoạn nén của **video do chính bạn render**, dùng cho AI kiểm tra chất lượng, xác thực bằng `ARK_API_KEY` của **chính bạn** | Chỉ khi bạn chạy script này, và chỉ sau khi qua cổng xác nhận đồng ý |
| `openspeech.bytedance.com` (API TTS chính thức của ByteDance) | `scripts/cloud/tts-doubao.mjs` (cũng được gọi bởi `scripts/narrate-pipeline.mjs`) | Văn bản thuyết minh bạn muốn tổng hợp giọng nói, kèm key của **chính bạn**. Endpoint được đối soát với danh sách trắng hostname cứng (`*.bytedance.com` / `*.volces.com`) — tệp `.env` bị chỉnh sửa cũng không thể chuyển hướng key hoặc văn bản của bạn đi nơi khác | Chỉ khi bạn chạy script này, và chỉ sau khi qua cổng xác nhận đồng ý |
| `commons.wikimedia.org` (API chính thức của Wikimedia) | `scripts/fetch_images.py` | Từ khóa tìm kiếm hình ảnh; tải ảnh CC/tài sản công cộng kèm thông tin giấy phép được in ra để kiểm tra | Chỉ khi agent đi lấy ảnh kho tư liệu cho một thiết kế nội dung |
| Các website chính thức của thương hiệu, `simpleicons.org`, dịch vụ Google favicon | `references/brand-asset-protocol.md` (tài liệu hướng dẫn, không phải script) | Yêu cầu GET thông thường để tải logo/tài sản thương hiệu được phục vụ công khai | Chỉ khi bạn yêu cầu thiết kế cho một thương hiệu cụ thể |
| `fonts.googleapis.com`, `unpkg.com` và các CDN tương tự | Thẻ `<link>`/`<script>` tĩnh trong HTML demo/đầu ra | Các yêu cầu tải phông chữ/thư viện chuẩn từ trình duyệt khi *bạn* mở một tệp HTML đã tạo | Chỉ ở phía trình duyệt; các script render ưu tiên hoạt động offline |

Đó là toàn bộ danh sách. Bạn có thể dùng lệnh `grep -rn "https://" --include="*.py" --include="*.mjs" --include="*.js" --include="*.sh" scripts/` để đối soát.

## Quản lý API Key

- Không có bất kỳ key nào được mã hóa cứng (hardcoded) ở bất kỳ đâu; kho lưu trữ chỉ cung cấp placeholder `.env.example` (`.env` được gitignore).
- Key được đọc từ **tệp `.env` ở gốc của chính skill** hoặc từ biến môi trường của tiến trình — tuyệt đối không đọc từ các tệp ở nơi khác trên máy tính của bạn. `ai-review-video.py` chỉ trích xuất duy nhất biến `ARK_API_KEY`; nó không nạp toàn bộ tệp vào môi trường.
- Key chỉ được truyền duy nhất tới endpoint chính thức của nhà cung cấp tương ứng được liệt kê ở trên, qua kết nối HTTPS mã hóa, dưới dạng header xác thực.
- Tùy chọn B trong `references/react-setup.md` (dán key Anthropic vào ô input của trang demo) được đánh dấu rõ ràng là chỉ dành cho demo cục bộ và không khuyến nghị; các tùy chọn mặc định không cần bất kỳ key nào.

## Cổng xác nhận đồng ý rõ ràng (Explicit Consent Gate)

Cả hai script cloud đều in rõ chính xác dữ liệu nào sẽ được gửi tới host nào và lập tức thoát trước khi thực hiện bất kỳ lệnh gọi mạng nào, trừ khi bạn truyền cờ `--yes` hoặc đặt `HUASHU_CLOUD_OK=1`. Tất cả những phần còn lại trong skill này không bao giờ cần cổng này vì chúng không bao giờ rời khỏi máy tính của bạn.

## Tiến trình con (Subprocesses)

Tất cả các lệnh gọi tiến trình con chỉ triệu tập các công cụ phương tiện cục bộ: `ffmpeg`, `ffprobe`, `ffplay`, Playwright/Chromium để render HTML và chụp màn hình. Không có sự kết hợp shell-sang-mạng nào, không có mẫu lệnh nguy hiểm kiểu curl-pipe-sh.

## Thao tác Xóa Tệp (File Deletion)

Thao tác xóa đệ quy chỉ giới hạn trong các thư mục tạm do chính các script tạo ra với tên unique chứa dấu thời gian + PID (`.video-tmp-*`, `.seek-tmp-*`, `_narration/.tmp`, Python `tempfile.TemporaryDirectory`). Không có script nào xóa dữ liệu của người dùng hoặc bất kỳ thứ gì nằm ngoài vùng làm việc tạm thời của chính nó.

## Thư viện phụ thuộc (Dependencies)

Chỉ sử dụng các gói thư viện từ registry chính thống (`playwright`, `sharp`, `pptxgenjs`, `pdf-lib`, `requests`), được cài đặt qua `npm`/`pip`/`uv` tiêu chuẩn — không tải về binary từ các URL tùy tiện. Một ngoại lệ duy nhất cần lưu ý: `npx hyperframes init` (backend hoạt ảnh tùy chọn, xem `references/hyperframes-backend.md`) sẽ cài đặt 19 skill tài liệu của hyperframes vào thư mục `~/.claude/skills/`. Việc này được ghi chú cảnh báo rõ ràng trong tài liệu trước khi xuất hiện lệnh.

## Các Hook

Script `scripts/design-gate-hook.sh` **không bao giờ được tự động cài đặt** — không có gì trong skill này ghi vào `settings.json`. Nếu bạn chủ động bật thủ công, toàn bộ hành vi của nó chỉ là: chặn các lệnh render video dài (exit 2) cho đến khi tệp phê duyệt thiết kế (`direction-approved.md`) tồn tại. Nó không thực hiện lệnh gọi mạng, không ghi bất kỳ thứ gì, không xóa bất kỳ thứ gì.

## Ghi chú xử lý Proxy

`fetch_images.py` và `ai-review-video.py` tắt việc kế thừa các biến môi trường proxy (`trust_env = False` / xóa `ALL_PROXY` v.v...) cho các request của chính chúng. Việc này nhằm vượt qua các cấu hình proxy cục bộ bị cũ/lỗi làm hỏng kết nối TLS — chứ không phải để né tránh việc giám sát. Nếu bạn cần các request này đi qua proxy của bạn, hãy cấu hình rõ ràng trong lệnh gọi script.

## Báo cáo sự cố

Nếu bạn phát hiện bất kỳ điều gì đi ngược lại tài liệu này, vui lòng mở một Issue — sự sai lệch giữa tệp này và mã nguồn được xử lý nghiêm túc như một lỗi bảo mật (bug).
