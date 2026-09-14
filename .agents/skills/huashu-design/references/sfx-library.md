# Thư viện SFX · huashu-design

> Tất cả được tạo bởi ElevenLabs Sound Generation API, chất lượng âm thanh cấp độ Apple Keynote.
> Thư viện tài sản SFX cấp sản phẩm, bao phủ toàn bộ các kịch bản animation/demo/sản phẩm demo của Hoa Thúc.

**Vị trí tài sản**: `assets/sfx/<danhMục>/<tên>.mp3`
**Tổng số**: 37 SFX (30 tạo hàng loạt + 7 giữ lại từ v7b)
**Model tạo**: ElevenLabs Sound Generation API (prompt_influence 0.4)
**Chất lượng âm thanh**: 44.1kHz MP3, độ rõ nét cấp độ Apple Keynote, không có vang (reverb) dư thừa

---

## Cấu trúc thư mục

```
assets/sfx/
├── keyboard/      type, type-fast, delete-key, space-tap, enter
├── ui/            click, click-soft, focus, hover-subtle, tap-finger, toggle-on
├── transition/    whoosh, whoosh-fast, swipe-horizontal, slide-in, dissolve
├── container/     card-snap, card-flip, stack-collapse, modal-open
├── feedback/      success-chime, error-tone, notification-pop, achievement
├── progress/      loading-tick, complete-done, generate-start
├── impact/        logo-reveal, logo-reveal-v2, brand-stamp, drop-thud
├── magic/         sparkle, ai-process, transform
└── terminal/      command-execute, output-appear, cursor-blink
```

---

## Mục lục tra nhanh

### ⌨️ Keyboard (Nhập bàn phím)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/keyboard/type.mp3` | 0.5s | Gõ đơn phím (mechanical keyboard single key) | mechanical keyboard single key press |
| `sfx/keyboard/type-fast.mp3` | 1.5s | Gõ chữ nhanh liên tục (Demo nhập prompt) | fast continuous typing rhythm, apple magic keyboard |
| `sfx/keyboard/delete-key.mp3` | 0.5s | backspace gõ lùi | single backspace key, low pitched thud |
| `sfx/keyboard/space-tap.mp3` | 0.5s | Phím space gõ nhẹ | soft spacebar tap, wide flat |
| `sfx/keyboard/enter.mp3` | 0.5s | Phím Enter xác nhận (Giữ lại từ v7b) | enter key press, crisp tactile |

### 🎯 UI (Tương tác giao diện)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/ui/click.mp3` | 0.5s | Click UI tiêu chuẩn (Giữ lại từ v7b) | crisp modern interface click |
| `sfx/ui/click-soft.mp3` | 0.5s | Click UI mềm mại (Nút phụ/Link) | soft gentle button click, mid pitched |
| `sfx/ui/focus.mp3` | 0.5s | Phần tử hội tụ/Được chọn (Giữ lại từ v7b) | subtle focus tone, element highlight |
| `sfx/ui/hover-subtle.mp3` | 0.5s | Gợi ý rê chuột (Phản hồi cấp microgiây) | barely audible tick, air whisper |
| `sfx/ui/tap-finger.mp3` | 0.5s | Tap trên mobile (Giao diện iOS) | finger tap on touchscreen, muted thud |
| `sfx/ui/toggle-on.mp3` | 0.5s | Bật công tắc toggle | ios toggle switch flip, satisfying click |

### 🌊 Transition (Chuyển cảnh)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/transition/whoosh.mp3` | 0.5s | Whoosh tiêu chuẩn (Giữ lại từ v7b) | air whoosh transition |
| `sfx/transition/whoosh-fast.mp3` | 0.6s | Whoosh nhanh (Tiêu đề chớp vào, chuyển tab) | quick fast air whoosh, cinematic |
| `sfx/transition/swipe-horizontal.mp3` | 0.7s | Vuốt ngang (Carousel, chuyển tab) | smooth left-to-right air movement |
| `sfx/transition/slide-in.mp3` | 0.6s | Phần tử trượt vào (side panel, ngăn kéo) | smooth soft whoosh with arrival |
| `sfx/transition/dissolve.mp3` | 0.8s | Hòa tan mềm mại (Hình ảnh mờ dần vào/ra) | soft dissolve, airy shimmer |

### 🃏 Container (Thẻ card/Container)

| File | Thời lượng | Mục đích | Yếu tố chính of Prompt |
|---|---|---|---|
| `sfx/container/card-snap.mp3` | 0.5s | Card hít vào/Định vị (Giữ lại từ v7b) | card snap into place |
| `sfx/container/card-flip.mp3` | 0.7s | Lật card (Chuyển đổi mặt trước/sau) | playing card flip, crisp snap |
| `sfx/container/stack-collapse.mp3` | 0.8s | Xếp chồng thu gọn (Gộp danh sách) | cards stacking, paper taps collapsing |
| `sfx/container/modal-open.mp3` | 0.6s | Modal bật mở | modal popping open, whoosh + thud |

### 🔔 Feedback (Thông báo/Phản hồi)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/feedback/success-chime.mp3` | 1.0s | Gợi ý thành công (Thanh toán thành công, hoàn thành bài tập) | two ascending bell tones, ios-style |
| `sfx/feedback/error-tone.mp3` | 0.7s | Gợi ý lỗi (Cảnh báo, thất bại) | descending two-note warning, soft |
| `sfx/feedback/notification-pop.mp3` | 0.6s | Tin nhắn bật lên (toast, thông báo) | notification bloop, ios message alert |
| `sfx/feedback/achievement.mp3` | 1.5s | Đạt được thành tựu (Cột mốc, huy hiệu) | triumphant rising arpeggio, game-style |

### ⏳ Progress (Tiến độ/Trạng thái)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/progress/loading-tick.mp3` | 0.5s | Báo giờ tải (Nhịp thanh tiến độ) | soft short pulse, minimal ambient |
| `sfx/progress/complete-done.mp3` | 0.8s | Xác nhận hoàn thành (Hoàn thành step) | two ascending satisfying tones |
| `sfx/progress/generate-start.mp3` | 0.8s | AI bắt đầu tạo dữ liệu | soft rising shimmer, magical whoosh |

### 💥 Impact (Thương hiệu/Xung kích)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/impact/logo-reveal.mp3` | 0.7s | Logo impact (Giữ lại từ v7b) | logo reveal thud |
| `sfx/impact/logo-reveal-v2.mp3` | 1.5s | Logo impact dài hơn (Cảm giác điện ảnh) | cinematic bass hit with shimmer tail |
| `sfx/impact/brand-stamp.mp3` | 1.0s | Đóng con dấu (Xác thực, đóng dấu) | rubber stamp thud, paper contact |
| `sfx/impact/drop-thud.mp3` | 0.7s | Vật thể hạ cánh (Chèn, đặt vào) | heavy thud, wood surface contact |

### ✨ Magic (Biến đổi AI)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/magic/sparkle.mp3` | 0.8s | Tỏa sáng phép thuật (AI highlight, bất ngờ) | bright twinkling stars, fairy dust |
| `sfx/magic/ai-process.mp3` | 1.2s | Âm thanh xử lý AI (Trạng thái thinking) | modulating digital hum with shimmer |
| `sfx/magic/transform.mp3` | 1.0s | Chuyển cảnh biến đổi (Hiệu ứng morph) | rising shimmer whoosh with sparkle tail |

### 💻 Terminal (Dòng lệnh)

| File | Thời lượng | Mục đích | Yếu tố chính của Prompt |
|---|---|---|---|
| `sfx/terminal/command-execute.mp3` | 0.5s | Thực thi lệnh | crisp digital beep with tick, hacker ui |
| `sfx/terminal/output-appear.mp3` | 0.6s | Đầu ra xuất hiện | rapid digital ticks, retro printout |
| `sfx/terminal/cursor-blink.mp3` | 0.5s | Con trỏ nhấp nháy | subtle soft digital pulse, rhythmic |

---

## Phối hợp gợi ý theo kịch bản

### 💻 Tương tác Terminal Demo
```
type (0.5s) → enter (0.5s) → command-execute (0.5s) → output-appear (0.6s)
```
Phần tử vòng lặp: `cursor-blink` làm âm thanh môi trường khi idle.

### 🃏 Quy trình chọn Card
```
hover-subtle (0.5s, UI hover) → click-soft (0.5s, click) → card-snap (0.5s, định vị)
```
Hoặc bản nâng cao: `card-flip` làm chuyển đổi mặt trước/sau.

### 🤖 Quy trình hoàn chỉnh AI tạo dữ liệu
```
generate-start (0.8s, khởi động) → ai-process (1.2s, xử lý) → sparkle (0.8s, chớp hiện) → complete-done (0.8s, hoàn thành)
```
Khi lỗi dùng `error-tone` thay thế cho `complete-done`.

### 🎬 Logo Reveal (Khoảnh khắc thương hiệu)
```
whoosh-fast (0.6s, lót nền) → logo-reveal-v2 (1.5s, điểm rơi) → sparkle (0.8s, dư vị)
```
Bản đơn giản: `whoosh → logo-reveal` (Bộ đôi v7b trực tiếp).

### 📱 UI Demo Tương tác (Mobile)
```
tap-finger (0.5s, click) → slide-in (0.6s, panel trượt vào) → toggle-on (0.5s, công tắc)
```
Sau khi hoàn thành: `success-chime` hoặc `notification-pop`.

### 📊 Trực quan hóa Dữ liệu/Dashboard
```
loading-tick (0.5s, nhịp) × N → complete-done (0.8s, dữ liệu vào vị trí) → achievement (1.5s, điểm rơi ấn tượng)
```

### 🎯 Quy trình gửi Form
```
click-soft (0.5s) → loading-tick ×2 (1.0s) → success-chime (1.0s)
```
Nhánh thất bại: `error-tone (0.7s)`.

### 🪄 Phân cảnh Magic Transform
```
whoosh-fast (0.6s) → transform (1.0s) → sparkle (0.8s)
```
Phù hợp: Biến dạng phần tử, so sánh hiệu ứng trước/sau, demo "AI viết lại".

---

## Quy chuẩn sử dụng

### Gợi ý âm lượng (Từ chế độ hai track âm thanh trong apple-gallery-showcase.md)
- **Track chính SFX**: `1.0` (Không suy giảm)
- **Track nền BGM**: `0.4 ~ 0.5` (SFX xuyên qua rõ ràng)
- **Nhiều SFX đè lên nhau**: Dùng `amix=inputs=N:duration=longest:normalize=0` để giữ lại dải động

### Template ghép nối ffmpeg
```bash
# Căn chỉnh mốc thời gian cho đơn SFX:
ffmpeg -i video.mp4 -itsoffset 2.5 -i sfx/ui/click.mp3 \
  -filter_complex "[0:a][1:a]amix=inputs=2:duration=longest:normalize=0[a]" \
  -map 0:v -map "[a]" output.mp4

# Nhiều SFX + BGM:
ffmpeg -i video.mp4 \
  -itsoffset 1.0 -i sfx/transition/whoosh-fast.mp3 \
  -itsoffset 1.6 -i sfx/impact/logo-reveal-v2.mp3 \
  -i bgm.mp3 \
  -filter_complex "[3:a]volume=0.4[bgm];[0:a][1:a][2:a][bgm]amix=inputs=4:normalize=0[a]" \
  -map 0:v -map "[a]" output.mp4
```

### Cây quyết định lựa chọn
1. **Có hành động tactile** (Gõ chữ/Click/Vuốt) → `keyboard/` or `ui/`
2. **Phần tử vào cảnh/thoát cảnh** → `transition/`
3. **Thao tác cấp container** (Card/Modal) → `container/`
4. **Phản hồi trạng thái** (Thành công/Thất bại/Thông báo) → `feedback/`
5. **Tiến độ/Thời gian trôi qua** → `progress/`
6. **Điểm rơi thương hiệu/Khoảnh khắc quan trọng** → `impact/`
7. **Phép thuật AI/Biến đổi** → `magic/`
8. **Dòng lệnh/Demo code** → `terminal/`

### Tránh tích tụ âm thanh đè lên nhau
- Cùng một thời điểm đồng thời `tối đa 2 SFX`
- BGM hạ xuống dưới 0.3 có thể đặt 3 cái
- Khi thương hiệu impact hãy làm trống các SFX khác (Để trống 0.2s rồi mới rơi vào điểm)

---

## Nguyên tắc soạn thảo Prompt (Cung cấp để tái sử dụng)

Phong cách tham khảo: `apple keynote, tight, minimal, no reverb unless ambient, crisp, elegant`

**Ba yếu tố của một prompt tốt**:
1. **Mô tả vật lý âm thanh**: Vật thể gì, hành động gì ("mechanical keyboard single key press")
2. **Giới định chất lượng/Phong cách**: apple-style / ios-style / cinematic / retro
3. **Loại trừ ví dụ phản diện**: no reverb / clean studio / minimal

❌ "click sound"
✅ "crisp ui button click, clean modern interface sound, apple-style, high pitched"

❌ "magic"
✅ "bright twinkling stars sound, high pitched glittery chime, fairy dust"

---

## Xem chi tiết
- Chế độ âm thanh hai track và ghép nối ffmpeg: `apple-gallery-showcase.md`
- Script tạo gốc: `/tmp/gen_sfx_batch.sh` (Bộ tạo hàng loạt một lần)
