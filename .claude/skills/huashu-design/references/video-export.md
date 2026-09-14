# Xuất Video: Animation HTML sang MP4/GIF

Sau khi hoàn thành animation HTML, người dùng thường muốn "Có thể xuất thành video không". Hướng dẫn này đưa ra quy trình hoàn chỉnh.

## Khi nào xuất video

**Thời điểm xuất video**:
- Animation đã hoàn thành chạy thông suốt, đã qua xác minh thị giác (Playwright chụp ảnh màn hình xác nhận trạng thái mốc thời gian đúng đắn)
- Người dùng đã xem trong trình duyệt ít nhất một lần, biểu thị hiệu ứng OK
- **Không** xuất video ở giai đoạn chưa sửa xong bug animation——xuất thành video rồi sửa lại sẽ đắt hơn

**Từ khóa kích hoạt người dùng có thể nói**:
- "Có thể xuất thành video không"
- "Chuyển thành MP4"
- "Làm thành GIF"
- "60fps"

## Quy cách thành phẩm

Mặc định cung cấp 3 loại định dạng một lúc để người dùng lựa chọn:

| Định dạng | Quy cách | Kịch bản phù hợp | Dung lượng điển hình (30s) |
|---|---|---|---|
| MP4 25fps | 1920×1080 · H.264 · CRF 18 | Nhúng bài viết WeChat, Video Channel, YouTube | 1-2 MB |
| MP4 60fps | 1920×1080 · Mặc định sao chép khung hình (tương thích ổn định) · H.264 · CRF 18; Chèn khung hình chất lượng cao cần rõ ràng `--minterpolate`; Chạy đồng hồ Stage dùng render-video-seek.js ghi hình trực tiếp 60fps thật | Hiển thị tốc độ khung hình cao, Bilibili, Portfolio | 1.5-3 MB |
| GIF | 960×540 · 15fps · Tối ưu hóa palette | Twitter/X, README, Preview Slack | 2-4 MB |

## Chuỗi công cụ (Toolchain)

Hai script nằm trong `scripts/`:

### 1. `render-video.js` — HTML → MP4

Ghi hình một phiên bản MP4 cơ bản 25fps. Phụ thuộc vào playwright toàn cục.

```bash
NODE_PATH=$(npm root -g) node /đường/dẫn/tới/claude-design/scripts/render-video.js <fileHTML>
```

Tham số tùy chọn:
- `--duration=30` Thời lượng animation (giây)
- `--width=1920 --height=1080` Độ phân giải
- `--trim=2.2` Số giây cắt bỏ ở đầu video (Xóa thời gian reload + tải font)
- `--fontwait=1.5` Thời gian chờ tải font (giây), điều chỉnh tăng khi có nhiều font
- `--recording-mode` Tham số mode ghi hình

Đầu ra: Cùng thư mục với HTML, cùng tên `.mp4`.

### 2. `add-music.sh` — MP4 + BGM → MP4

Trộn nhạc nền vào MP4 không tiếng, chọn từ thư viện BGM tích hợp theo kịch bản (mood), cũng có thể tự mang âm thanh riêng. Tự động khớp thời lượng, thêm hiệu ứng fade-in/fade-out.

```bash
bash add-music.sh <input.mp4> [--mood=<tên>] [--music=<đườngDẫn>] [--out=<đườngDẫn>]
```

**Thư viện BGM tích hợp** (Nằm tại `assets/bgm-<mood>.mp3`):

| `--mood=` | Phong cách | Kịch bản thích ứng |
|-----------|------|---------|
| `tech` (Mặc định) | Apple Silicon / Ra mắt Apple, Minimal Synth + Piano | Ra mắt sản phẩm, công cụ AI, quảng bá Skill |
| `ad` | Upbeat điện tử hiện đại, có build + drop | Quảng cáo mạng xã hội, trailer sản phẩm, phim khuyến mãi |
| `educational` | Ấm áp tươi sáng, Guitar nhẹ/Piano điện, inviting | Phổ cập khoa học, giới thiệu hướng dẫn, trailer khóa học |
| `educational-alt` | Dự phòng cùng loại, đổi bài khác thử | Giống như trên |
| `tutorial` | Âm thanh môi trường lo-fi, hầu như không có cảm giác tồn tại | Demo phần mềm, hướng dẫn lập trình, demo dài |
| `tutorial-alt` | Dự phòng cùng loại | Giống như trên |

**Hành vi**:
- Nhạc được cắt theo thời lượng video
- 0.3s fade-in + 1s fade-out (Tránh cắt cứng)
- Luồng video `-c:v copy` không mã hóa lại, âm thanh AAC 192k
- `--music=<đườngDẫn>` Ưu tiên cao hơn `--mood`, có thể chỉ định trực tiếp bất kỳ âm thanh bên ngoài nào
- Truyền sai tên mood sẽ liệt kê tất cả các tùy chọn khả thi, không thất bại âm thầm

**Luồng công việc điển hình** (Bộ ba xuất animation + Phối nhạc):
```bash
node render-video.js animation.html                        # Ghi hình màn hình
bash convert-formats.sh animation.mp4                      # Phái sinh 60fps + GIF
bash add-music.sh animation-60fps.mp4                      # Thêm BGM tech mặc định
# Hoặc nhắm vào kịch bản khác nhau:
bash add-music.sh tutorial-demo.mp4 --mood=tutorial
bash add-music.sh product-promo.mp4 --mood=ad --out=promo-final.mp4
```

### 3. `convert-formats.sh` — MP4 → 60fps MP4 + GIF

Tạo phiên bản 60fps và GIF từ MP4 đã có.

```bash
bash /đường/dẫn/tới/claude-design/scripts/convert-formats.sh <input.mp4> [gif_width] [--minterpolate]
```

Đầu ra (Cùng thư mục với đầu vào):
- `<name>-60fps.mp4` — Mặc định dùng sao chép khung hình `fps=60` (Tính tương thích rộng); Thêm `--minterpolate` để bật chèn khung hình chất lượng cao
- `<name>.gif` — GIF đã tối ưu hóa palette (Mặc định chiều rộng 960, có thể sửa)

**Lựa chọn chế độ 60fps**:

| Chế độ | Lệnh | Tính tương thích | Kịch bản sử dụng |
|---|---|---|---|
| Sao chép khung hình (Mặc định) | `convert-formats.sh in.mp4` | QuickTime/Safari/Chrome/VLC thông suốt | Giao hàng chung, tải lên nền tảng, mạng xã hội |
| minterpolate Chèn khung hình | `convert-formats.sh in.mp4 --minterpolate` | macOS QuickTime/Safari có thể từ chối phát | Kịch bản hiển thị cần chèn khung hình thật như Bilibili, **trước khi giao hàng bắt buộc kiểm tra cục bộ** trình phát mục tiêu |

Tại sao mặc định đổi thành sao chép khung hình? H.264 elementary stream do minterpolate xuất ra có bug tương thích đã biết——trước đây khi mặc định minterpolate đã nhiều lần dẫm bẫy "macOS QuickTime không mở được". Xem chi tiết tại `animation-pitfalls.md` §14.

Tham số `gif_width`:
- 960 (Mặc định) —— Chung cho nền tảng mạng xã hội
- 1280 —— Rõ nét hơn nhưng file lớn hơn
- 600 —— Ưu tiên tải cho Twitter/X

### 4. `render-video-seek.js` — 60fps Thật / Render Xác định (Khuyến nghị giao hàng chất lượng cao)

Đường dẫn recordVideo của `render-video.js` có 3 hạn chế vốn có: Tốc độ khung hình bị Chromium compositor khóa chết 25fps, đầu video có khung hình đen khi tải cần trim, 60fps chỉ có thể dựa vào minterpolate chèn khung hình sau đó (Có hiện tượng ghosting + bug tương thích macOS QuickTime, xem `animation-pitfalls.md §14`). Khi cần **60fps thật, đầu ra xác định, hoặc giao hàng cho Bilibili/Portfolio**, hãy đổi sang render seek.

Nó seek từng khung hình đến mốc thời gian để chụp ảnh màn hình, sau đó dùng ffmpeg mã hóa chuỗi PNG thành MP4. Nhân kỹ thuật học hỏi ý tưởng "Đóng băng đồng hồ + seek chụp màn hình" của HeyGen HyperFrames (Apache 2.0), nhưng không đưa vào bất kỳ package bên thứ ba nào——chỉ dùng playwright + ffmpeg sẵn có trong skill này, trung lập với runtime.

```bash
NODE_PATH=$(npm root -g) node /đường/dẫn/tới/claude-design/scripts/render-video-seek.js <fileHTML> --fps=60
```

Tham số: `--duration` · `--fps` (Mặc định 60) · `--width` · `--height` · `--concurrency` (Mặc định 4 worker song song) · `--settle` (Sau khi seek chờ mấy rAF rồi mới chụp ảnh màn hình, mặc định 2, animation tái bố cục nặng có thể điều chỉnh tăng) · `--keep-chrome`. Đầu ra cùng thư mục với HTML, cùng tên `.mp4`.

Giải quyết trực diện 3 nút thắt tử thần của recordVideo:
- **60fps nguyên bản thật**: `--fps=60` xuất 60fps thật (Mỗi khung hình đều là hình ảnh seek thực tế), không còn qua chèn khung hình minterpolate của `convert-formats.sh`, né được ghosting + bug tương thích macOS
- **Không có khung hình đen đầu video**: Không ghi màn hình, hoàn toàn không có khung hình đen thời kỳ tải, không cần `--trim` / `--fontwait`
- **Tính xác định**: Seek đến mốc thời gian để chụp ảnh màn hình, cùng đầu vào cho cùng đầu ra, không bị ảnh hưởng bởi tải máy/mất khung hình

**Ranh giới áp dụng (Quan trọng)**: Chỉ hỗ trợ animation chạy theo đồng hồ Stage——`<Stage>` của `assets/animations.jsx` hoặc `<NarrationStage>` của `narration_stage.jsx`, chúng sẽ phản hồi `window.__seekRender` để đóng băng đồng hồ tự điều khiển và lộ ra `window.__seek(t)`. Animation CSS `@keyframes` thuần túy / Lottie / Tự viết không qua Stage sẽ không ăn `__seek`, loại này tiếp tục dùng `render-video.js` (Script không phát hiện thấy `__seek` sẽ báo lỗi và gợi ý).

**Cái giá**: Chụp ảnh màn hình từng khung hình, tổng thời gian tiêu tốn cho video dài có thể lâu hơn ghi hình thời gian thực của recordVideo (Giảm bớt bằng `--concurrency` nhiều worker); Số lượng lớn PNG tạm chiếm đĩa, trước khi render khuyến nghị đóng các ứng dụng chiếm bộ nhớ lớn khác.

**Chiến lược chọn 1 trong 2**: Mặc định vẫn dùng `render-video.js` (Rủi ro bằng 0, che phủ tất cả các loại animation); Khi cần 60fps thật / Tính xác định / Giao hàng chất lượng cao, và animation chạy theo đồng hồ Stage, dùng `render-video-seek.js`. Animation dài có thuyết minh dùng `render-narration.sh --seek` một cú nhấp chạy render seek + trộn âm thanh.

## Quy trình hoàn chỉnh (Khuyến nghị tiêu chuẩn)

Sau khi người dùng nói "Xuất video":

```bash
cd <thưMụcDựÁn>

# Giả sử $SKILL trỏ tới thư mục gốc của skill này (Tự thay thế theo vị trí cài đặt)

# 1. Ghi hình MP4 cơ bản 25fps
NODE_PATH=$(npm root -g) node "$SKILL/scripts/render-video.js" my-animation.html

# 2. Phái sinh MP4 60fps và GIF
bash "$SKILL/scripts/convert-formats.sh" my-animation.mp4

# Danh mục thành phẩm:
# my-animation.mp4         (25fps · 1-2 MB)
# my-animation-60fps.mp4   (60fps · 1.5-3 MB)
# my-animation.gif         (15fps · 2-4 MB)
```

## Chi tiết kỹ thuật (Dùng để gỡ lỗi)

### Bẫy của Playwright recordVideo

- Tốc độ khung hình cố định 25fps, không thể trực tiếp ghi hình 60fps (Giới hạn trên compositor của Chromium headless)
- Bắt đầu ghi hình ngay từ khi tạo context, bắt buộc phải dùng `trim` cắt thời gian tải phía trước
- Mặc định định dạng webm, cần ffmpeg chuyển sang H.264 MP4 mới có thể phát chung

`render-video.js` đã xử lý các vấn đề trên.

### Tham số ffmpeg minterpolate

Cấu hình hiện tại: `minterpolate=fps=60:mi_mode=mci:mc_mode=aobmc:me_mode=bidir:vsbmc=1`

- `mi_mode=mci` — motion compensation interpolation (Bù trừ chuyển động)
- `mc_mode=aobmc` — adaptive overlapped block motion compensation
- `me_mode=bidir` — Ước tính chuyển động hai chiều
- `vsbmc=1` — Bù trừ chuyển động khối kích thước biến đổi

Đối với **animation transform** CSS (translate/scale/rotate) hiệu quả tốt.
Đối với **fade thuần túy** có thể tạo ra ghosting nhẹ——Nếu người dùng chê, thoái hóa thành sao chép khung hình đơn giản:

```bash
ffmpeg -i input.mp4 -r 60 -c:v libx264 ... output.mp4
```

### Tại sao GIF palette cần hai giai đoạn

GIF chỉ có 256 màu. GIF một pass sẽ nén màu sắc toàn bộ animation thành palette chung 256 màu, đối với phối màu tinh tế như nền kem + màu cam sẽ bị mờ.

Hai giai đoạn:
1. `palettegen=stats_mode=diff` —— Quét toàn bộ phim trước, tạo ra **optimal palette dành riêng cho animation này**
2. `paletteuse=dither=bayer:bayer_scale=5:diff_mode=rectangle` —— Dùng palette này để mã hóa, rectangle diff chỉ cập nhật vùng thay đổi, giảm đáng kể dung lượng file

Đối với chuyển đổi fade dùng `dither=bayer` mượt hơn `none`, nhưng file lớn hơn một chút.

## Pre-flight check (Trước khi xuất)

Tự kiểm tra 30 giây trước khi xuất:

- [ ] HTML đã chạy hoàn chỉnh một lần trong trình duyệt, không có lỗi console
- [ ] Khung hình thứ 0 của animation là trạng thái ban đầu hoàn chỉnh (Không phải đang tải khoảng trống)
- [ ] Khung hình cuối cùng của animation là trạng thái kết bài ổn định (Không phải dở dang)
- [ ] Font chữ/Hình ảnh/Emoji tất cả đều render bình thường (Tham khảo `animation-pitfalls.md`)
- [ ] Tham số Duration khớp với thời lượng animation thực tế trong HTML
- [ ] Trong HTML Stage phát hiện `window.__recording` ép buộc loop=false (Bắt buộc kiểm tra khi tự viết Stage; dùng sẵn từ `assets/animations.jsx`)
- [ ] Sprite kết thúc có `fadeOut={0}` (Khung hình cuối video không mờ dần)
- [ ] Có watermark "Created by Huashu-Design" (Bắt buộc thêm đối với phân cảnh chỉ làm animation; Tác phẩm thương hiệu bên thứ ba thêm tiền tố "Phiên bản không chính thức · ". Xem chi tiết SKILL.md § "Watermark quảng bá Skill")

## Thuyết minh đi kèm khi giao hàng

Định dạng thuyết minh tiêu chuẩn cho người dùng sau khi hoàn thành xuất video:

```
**Giao hàng hoàn chỉnh**

| File | Định dạng | Quy cách | Dung lượng |
|---|---|---|---|
| foo.mp4 | MP4 | 1920×1080 · 25fps · H.264 | X MB |
| foo-60fps.mp4 | MP4 | 1920×1080 · 60fps (Mặc định sao chép khung hình; Bản chèn khung hình sẽ ghi chú) · H.264 | X MB |
| foo.gif | GIF | 960×540 · 15fps · Tối ưu hóa palette | X MB |

**Thuyết minh**
- 60fps mặc định sao chép khung hình (Tính tương thích tốt); Chỉ khi yêu cầu rõ ràng mới dùng minterpolate chèn khung hình (Hiệu ứng animation transform tốt, màn hình phức tạp dễ ra bóng ma); 60fps thật dùng render-video-seek.js ghi hình trực tiếp seek từng khung hình
- GIF dùng tối ưu hóa palette, animation 30s có thể nén xuống khoảng 3MB

Cần đổi kích thước hoặc tốc độ khung hình hãy nhắn một tiếng.
```

## Thường gặp nhu cầu bổ sung của người dùng

| Người dùng nói | Ứng phó |
|---|---|
| "Dung lượng lớn quá" | MP4: Tăng CRF lên 23-28; GIF: Giảm độ phân giải xuống 600 hoặc fps xuống 10 |
| "GIF mờ quá" | Tăng `gif_width` lên 1280; Hoặc gợi ý dùng MP4 thay thế (WeChat Moments cũng hỗ trợ) |
| "Cần màn hình dọc 9:16" | Sửa nguồn HTML `--width=1080 --height=1920`, ghi hình lại |
| "Thêm watermark" | ffmpeg thêm `-vf "drawtext=..."` hoặc `overlay=` một PNG |
| "Cần nền trong suốt" | MP4 không hỗ trợ alpha; Dùng WebM VP9 + alpha hoặc APNG |
| "Cần không nén (lossless)" | CRF sửa thành 0 + preset veryslow (File sẽ lớn hơn 10 lần) |

## Template Watermark Quảng bá Skill (Chỉ dùng khi xuất animation)

SKILL.md quy định animation MP4/GIF mặc định mang watermark, template như sau (Nền tối đổi sang dùng `rgba(255,255,255,0.35)`; Animation thương hiệu bên thứ ba tiền tố "Phiên bản không chính thức · "):

```jsx
<div style={{
  position: 'absolute', bottom: 24, right: 32,
  fontSize: 11, color: 'rgba(0,0,0,0.4)',
  letterSpacing: '0.15em', fontFamily: 'monospace',
  pointerEvents: 'none', zIndex: 100,
}}>
  Created by Huashu-Design
</div>
```
