# Backend Render HyperFrames · Ranh giới lựa chọn & Sách hướng dẫn thao tác

> Được đưa vào sau khi xác minh thử nghiệm thực tế thông qua (Cả 5 mục Toolchain/Font chữ Tiếng Trung/Môi trường Proxy/Di chuyển/3D đều qua, dữ liệu mấu chốt đã được nhúng trong bài viết này) ngày 17-07-2026.
> HyperFrames là framework chuyển đổi HTML→Video mã nguồn mở của HeyGen (Apache 2.0): HTML thuần + timeline GSAP tạm dừng, trình duyệt headless seek từng khung hình để render xác định.

## Ranh giới lựa chọn (Xem bảng này trước khi mở máy)

| Kịch bản | Dùng tuyến render nào |
|---|---|
| Dự án animation mới (Mặc định) | **HyperFrames**. Bộ audit miễn phí, giải khóa toàn bộ 3D/GSAP/Lottie/shader |
| Cần 3D / Hạt (particles) / Quán tính vật lý / Shader chuyển cảnh | HyperFrames (Stage tự phát triển không làm được) |
| Cần tái sử dụng/cải bản Stage demo cũ | Di chuyển tiện tay (Công thức adapter xem bên dưới, 20-30 phút/cái); Chỉ render lại không sửa thì vẫn dùng render-video-seek.js |
| Runtime yếu (Không npm / Không thể cài dependency / Giao file đơn cho người dùng double-click mở) | Stage tự phát triển (assets/animations.jsx), quy trình cũ không đổi |
| Demo tương tác (Người dùng chơi trong trình duyệt, không xuất video) | Stage tự phát triển hoặc HTML thông thường, HyperFrames là render pipeline không phải framework tương tác |
| Video dài có thuyết minh (Step 9.5, điều khiển bởi narration_stage) | **Tuyến narration tự phát triển** (voiceover-pipeline.md + render-narration.sh), tạm thời không đi theo HyperFrames——Nguồn thời gian đôi/Phụ đề/Timeline TTS liên kết sâu với Stage tự phát triển; Khi trùng khớp đồng thời hai dòng với "Animation mặc định HyperFrames" thì phán quyết theo dòng này |
| Video tham số hóa hàng loạt (Nhiều người nhiều giao diện/Mẫu thay chữ) | Remotion (Xem hướng quy hoạch 5, độc lập với quy trình chính của skill này) |

**Ngôn ngữ thiết kế luôn là Bên A**: Cấu trúc tự sự, hệ thống easing, chế độ hai track SFX/BGM theo quy tắc cũ vẫn hoàn toàn có hiệu lực (animation-best-practices.md / audio-design-rules.md), HyperFrames chỉ là công cụ thực thi và render. Công thức thực thi GSAP xem `references/gsap-recipes.md`.

## Giàn giáo dự án (Project Scaffold)

> ⚠️ Cảnh báo cài đặt: `hyperframes init` ngoài việc tạo ra các file dự án, còn cài đặt **19 hyperframes skills vào `~/.claude/skills/`** (Tài liệu hợp đồng tổng hợp của backend render, thuần tài liệu không có hook thực thi). Nếu để ý thì chạy `npx hyperframes docs` xem danh mục tài liệu cục bộ trước rồi mới quyết định có init hay không.

```bash
npx -y hyperframes init TênDựÁn --example blank   # Bắt buộc phải mang --example nếu không tương tác
cd TênDựÁn && npm install
```

Tạo ra index.html / hyperframes.json / meta.json / package.json (Đã pin phiên bản CLI) + CLAUDE.md cấp dự án. init sẽ cài 19 hyperframes skills vào `~/.claude/skills/` (Máy cục bộ đã cài). Hợp đồng cách viết tổng hợp đọc SKILL.md của hyperframes-core skill (init cài vào thư mục skill của từng runtime, Claude Code mặc định `~/.claude/skills/`; Runtime không có cơ chế skill thì trực tiếp đọc tài liệu cục bộ `npx hyperframes docs` để thay thế), tài liệu cục bộ `npx hyperframes docs <topic>` (data-attributes / gsap / rendering / troubleshooting).

**Chiến lược phiên bản**: package.json của dự án sẽ pin phiên bản chính xác (Hiện tại đã thử nghiệm thực tế là 0.7.61). Nó lặp rất nhanh (300+ releases), khi nâng cấp hãy chạy `npx hyperframes@latest upgrade --project . --check` xem delta trước, chạy lại một lượt regression demo rồi mới động thủ.

## Tra nhanh hợp đồng tổng hợp (Bản hoàn chỉnh đọc hyperframes-core)

- Container gốc: `data-composition-id` + `data-start` + `data-duration` + `data-width/height`
- Mỗi phần tử đếm thời gian: `class="clip"` + `data-start` + `data-duration` + `data-track-index`
- timeline bắt buộc phải paused và đăng ký: `window.__timelines["idTổngHợp"] = gsap.timeline({paused:true})`
- Vật liệu video dùng `muted`, track âm thanh tách riêng phần tử `<audio>`
- **Chỉ cho phép logic xác định**: Cấm `Date.now()` / `Math.random()` / fetch mạng lúc runtime; Ngẫu nhiên dùng hàm seed
- Font chữ: Google Fonts sẽ được trình biên dịch tự động bắt lấy và bơm @font-face xác định (Cache `~/.cache/hyperframes/fonts/`); Font hệ thống thuần túy (PingFang SC v.v.) thêm một dòng `@font-face { font-family:"PingFang SC"; src: local("PingFang SC"); }` để qua lint
- Three.js đi theo adapter sự kiện `hf-seek` (`~/.claude/skills/hyperframes-animation/adapters/three.md`), container gốc bắt buộc phải có `data-duration` rõ ràng

## Di chuyển demo cũ · Công thức adapter (Thử nghiệm thực tế 20-30 phút/cái)

Animation dùng Stage tự phát triển/render(t) thuần túy không cần viết lại, 4 bước:

1. **Bọc container**: Bên ngoài bọc `#root` mang thuộc tính data tổng hợp; Toàn bộ `.stage` làm clip duy nhất tiết kiệm việc nhất (`class="stage clip"` + data-start/duration/track-index); `.stage` sửa từ fixed căn giữa sang absolute inset:0, html/body cố định chết 1920×1080
2. **Xóa tự điều khiển**: Vòng lặp rAF tick, listener fitStage/resize, nút replay, giao thức `__ready/__setTime/__seek` xóa toàn bộ (Render engine không cần)
3. **Gắn proxy tween** (12 dòng cốt lõi):
   ```js
   const proxy = { t: 0 };
   const tl = gsap.timeline({ paused: true });
   tl.to(proxy, { t: DURATION, duration: DURATION, ease: "none",
     onUpdate: () => render(proxy.t) }, 0);
   window.__timelines = window.__timelines || {};
   window.__timelines["main"] = tl;
   render(0);   // Bắt buộc: Khi timeline dừng ở t=0 thì onUpdate không kích hoạt, không bù câu này khung hình đầu tiên có thể chưa khởi tạo
   ```
4. **Quét transition**: Tìm kiếm khai báo `transition:` trong toàn bộ văn bản. CSS transition + chuyển đổi class đi theo đồng hồ tường, dưới sự seek từng khung hình là không xác định, bắt buộc phải sửa thành hàm thuần túy của t (lerp) trong render(t)

## Kiểm tra và Render

```bash
npm run check                        # Audit 5 cửa lint+runtime+layout+motion+contrast
npx hyperframes check --no-contrast  # Dành riêng cho phong cách điện ảnh tối (Xem bên dưới)
npx -y hyperframes@<phiênBảnPin> render --fps 60   # Render cuối; Mặc định 30fps
```

- **check bắt buộc phải 0 error mới render** (Trừ cửa contrast). lint có thể chặn rung lắc letterSpacing, thiếu font chữ, không xác định và cả một loại "bug thị giác không cảnh báo"
- **Đánh đổi cửa contrast**: Nó kiểm tra theo WCAG 4.5:1, xung đột hoàn toàn với watermark/chữ trang trí độ tương phản thấp (độ trong suốt 16-40%) của phong cách điện ảnh tối, và không có miễn trừ từng phần tử. Thành phẩm cinematic tối thống nhất `--no-contrast`, 4 cửa còn lại vẫn bắt buộc phải 0 error. Thành phẩm loại thông tin nền sáng đừng bỏ qua, báo lỗi contrast thường là vấn đề thật
- **Render hai cấp**: Mặc định 30fps xuất video nhanh trước, sau khi kiểm tra mắt thường + chụp khung hình qua rồi mới `--fps 60` render cuối. 60fps 600 khung hình 1080p thử nghiệm thực tế khoảng 20 giây
- Kiểm tra phía thành phẩm render (audio stream / khung hình đen / độ vang / thời lượng) dùng `scripts/verify-video.sh` (Xem verification.md)

## Kênh trong suốt (Alpha Channel - Overlay chữ trang trí/nhãn dán đè trực tiếp lên track dựng)

`npx hyperframes render --format mov` xuất ProRes 4444 (yuva444p12le, có alpha, thử nghiệm thực tế ngày 17-07-2026 đè lên nền màu ngay cả bóng mềm cũng bán trong suốt chính xác); `--format webm` cũng mang tính trong suốt, dung lượng nhỏ; `--format png-sequence` xuất chuỗi khung hình RGBA cho AE/DaVinci. Điểm mấu chốt phía tổng hợp: Nền html/body đặt `transparent`, không phủ màu nền. Các vật liệu overlay như chữ trang trí/badge/lower-third từ nay tiến trực tiếp vào track dựng, không cần tách nền (chroma key). Lưu ý dung lượng MOV lớn (cấp ProRes không nén, 4 giây dung lượng tầm 15MB), dùng cho giao hàng dựng phim; truyền tải qua mạng dùng webm.

## Âm thanh

Trong tổng hợp HyperFrames phần tử `<audio>` có thể tiến trực tiếp vào timeline (BGM/Thuyết minh render đi theo video). Quy trình âm thanh hiện tại không đổi: Chế độ hai track SFX/BGM tuân theo audio-design-rules.md, dùng add-music.sh / mix-voiceover.sh trộn luồng ở giai đoạn hậu kỳ cũng được. Con đường nào tốt hơn sẽ quyết định trong thực chiến, tạm thời không ép buộc. Đánh điểm SFX dùng `scripts/sfx-cues.sh <video> <bảngCue.tsv> <đầuRa>` (Bảng cue=Số giây/đường dẫn sfx/âm lượng dB ba cột, B00 tích lũy thực chiến, sửa bảng chạy lại 10 giây ra video).

## Bổ sung Pitfalls (So với pipeline tự phát triển)

Pitfalls của pipeline tự phát triển (animation-pitfalls.md §7/10/12/13 loại giao thức ghi hình, §6 thời序 font chữ, §15/17 loại mạng) trên backend HyperFrames **không áp dụng**: Giao thức ghi hình được xử lý bên trong framework, font chữ được bắt lấy ở thời điểm biên dịch, CDN thử nghiệm thực tế dưới proxy có thể thông. 4 bẫy mới thêm đã được ghi vào animation-pitfalls.md §18-21: CSS transition không xác định, proxy tween khung hình đầu tiên, xung đột cửa contrast, bóng ma immediateRender của fromTo.
