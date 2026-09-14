# Verification: Quy trình kiểm tra sản phẩm xuất ra

Một số môi trường design-agent nguyên bản (như Claude.ai Artifacts) tích hợp sẵn `fork_verifier_agent` để bật subagent kiểm tra ảnh chụp iframe. Hầu hết các môi trường agent (Claude Code / Codex / Cursor / Trae / v.v.) không có năng lực tích hợp sẵn này — Dùng Playwright thủ công là có thể đáp ứng cùng các kịch bản kiểm tra.

## Danh sách kiểm tra (Checklist)

Mỗi lần tạo ra file HTML xong, hãy làm theo danh sách kiểm tra này một lần:

### 1. Kiểm tra render trên trình duyệt (Bắt buộc làm)

Cơ bản nhất: **HTML có mở được không**? Trên macOS:

```bash
open -a "Google Chrome" "/path/to/your/design.html"
```

Hoặc dùng Playwright để chụp màn hình (xem phần tiếp theo).

### 2. Kiểm tra lỗi Console

Vấn đề phổ biến nhất trong file HTML là lỗi JS dẫn đến màn hình trắng. Chạy qua Playwright một lần:

```bash
python ~/.claude/skills/huashu-design/scripts/verify.py path/to/design.html
```

Script này sẽ:
1. Dùng headless chromium mở HTML
2. Chụp màn hình và lưu vào thư mục dự án
3. Bắt các lỗi trong console
4. Báo cáo status

Chi tiết xem tại `scripts/verify.py`.

### 3. Kiểm tra đa Viewport

Nếu là responsive design, chụp nhiều viewport:

```bash
python verify.py design.html --viewports 1920x1080,1440x900,768x1024,375x667
```

### 4. Kiểm tra tương tác

Tweaks, animation, chuyển đổi nút bấm — Chụp màn hình tĩnh mặc định sẽ không thấy được. **Khuyến nghị để người dùng tự mở trình duyệt lên bấm thử**, hoặc dùng Playwright quay video màn hình:

```python
page.video.record('interaction.mp4')
```

### 5. Kiểm tra từng trang Slide

Loại HTML dạng Deck, chụp từng trang một:

```bash
python verify.py deck.html --slides 10  # Chụp 10 trang đầu
```

Tạo ra `deck-slide-01.png`, `deck-slide-02.png`... thuận tiện cho việc xem nhanh.

## Playwright Setup

Lần đầu sử dụng cần:

```bash
# Nếu chưa cài
npm install -g playwright
npx playwright install chromium

# Hoặc bản Python
pip install playwright
playwright install chromium
```

Nếu người dùng đã cài Playwright toàn cục (global), có thể dùng trực tiếp.

## Best Practices khi Chụp màn hình

### Chụp toàn bộ trang (Full Page)

```python
page.screenshot(path='full.png', full_page=True)
```

### Chụp viewport

```python
page.screenshot(path='viewport.png')  # Mặc định chỉ chụp khu vực hiển thị
```

### Chụp phần tử cụ thể

```python
element = page.query_selector('.hero-section')
element.screenshot(path='hero.png')
```

### Chụp nét cao (High Definition)

```python
page = browser.new_page(device_scale_factor=2)  # retina
```

### Chờ animation kết thúc rồi mới chụp

```python
page.wait_for_timeout(2000)  # Chờ 2 giây cho animation settle
page.screenshot(...)
```

## Gửi ảnh chụp màn hình cho người dùng

### Ảnh chụp màn hình cục bộ mở trực tiếp

```bash
open screenshot.png
```

Người dùng sẽ xem trong Preview / Figma / VSCode / Trình duyệt của họ.

### Tải lên kho lưu trữ ảnh để chia sẻ link

Nếu cần gửi cho cộng sự ở xa xem (như Slack / Lark / Zalo / WeChat), để người dùng dùng công cụ kho ảnh hoặc MCP của họ tải ảnh lên, lấy một link vĩnh viễn có thể dán vào bất kỳ đâu.

## Khi kiểm tra phát sinh lỗi

### Trang bị màn hình trắng

Trong console chắc chắn có lỗi. Kiểm tra trước:

1. Integrity hash của script tag React+Babel có đúng không (xem `react-setup.md`)
2. Có phải do `const styles = {...}` xung đột tên gọi
3. Component giữa các file đã export ra `window` chưa
4. Lỗi cú pháp JSX (babel.min.js không báo lỗi, đổi sang bản không nén babel.js)

### Animation bị giật (lag)

- Dùng tab Performance của Chrome DevTools ghi lại một đoạn
- Tìm layout thrashing (reflow tần suất cao)
- Hiệu ứng chuyển động ưu tiên dùng `transform` và `opacity` (tăng tốc GPU)

### Font chữ hiển thị sai

- Kiểm tra url của `@font-face` có truy cập được không
- Kiểm tra font chữ fallback
- Font chữ tiếng Trung tải chậm: Hiển thị fallback trước, tải xong mới chuyển

### Lệch bố cục (Layout)

- Kiểm tra `box-sizing: border-box` đã áp dụng toàn cục chưa
- Kiểm tra reset `* { margin: 0; padding: 0; }`
- Mở gridlines trong Chrome DevTools để xem bố cục thực tế

## Kiểm tra = Đôi mắt thứ hai của nhà thiết kế

**Luôn luôn phải tự mình rà soát một lượt**. AI khi viết code rất thường xuất hiện:

- Trông có vẻ đúng nhưng tương tác có bug
- Chụp màn hình tĩnh thì đẹp nhưng cuộn trang lại bị lệch
- Màn hình rộng đẹp nhưng màn hình hẹp bị vỡ
- Quên test Dark mode
- Chuyển đổi Tweaks xong một số component không phản hồi

**1 phút kiểm tra cuối cùng có thể tiết kiệm 1 giờ làm lại**.

## Các lệnh script kiểm tra thường dùng

```bash
# Cơ bản: Mở + Chụp màn hình + Bắt lỗi
python verify.py design.html

# Nhiều viewport
python verify.py design.html --viewports 1920x1080,375x667

# Nhiều slide
python verify.py deck.html --slides 10

# Xuất ra thư mục chỉ định
python verify.py design.html --output ./screenshots/

# headless=false, mở trình duyệt thật cho bạn xem
python verify.py design.html --show
```

## Kiểm tra cứng sản phẩm Video (verify-video.sh)

File MP4/phim hoàn chỉnh render ra không dựa vào mắt thường để xem, mà dùng script kiểm tra cứng (phần kiểm tra phía tổng hợp HTML do kiểm toán 5 cửa của `hyperframes check` phụ trách, script này chỉ quản lý phía sản phẩm):

```bash
# Thành phẩm (Mặc định yêu cầu có luồng âm thanh)
bash scripts/verify-video.sh final.mp4 --duration=22 --fps=60 --width=1920 --height=1080

# Sản phẩm trung gian không tiếng
bash scripts/verify-video.sh raw.mp4 --duration=10 --fps=60 --no-audio

# Phong cách điện ảnh cố tình mở đầu bằng màn hình đen
bash scripts/verify-video.sh film.mp4 --duration=30 --fps=60 --allow-black-open
```

Các mục kiểm tra: Độ phân giải/tỷ lệ khung hình (fps), sai số thời lượng (±2%), sự tồn tại của audio stream (không có luồng âm thanh = quy tắc sắt về sản phẩm bán thành phẩm do máy thực thi), khung hình đen đầu cuối (blackdetect, triệu chứng điển hình của lệch điểm bắt đầu ghi/nảy lại loop), độ lớn âm thanh LUFS (mục tiêu thành phẩm -14±4). Exit code khác 0 thì tuyệt đối không cho phép bàn giao.
