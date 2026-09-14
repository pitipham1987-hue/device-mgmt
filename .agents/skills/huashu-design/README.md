<sub>🌐 <b>Tiếng Việt</b> · <a href="README.md">Tiếng Việt</a> · <a href="README.en.md">English</a></sub>

<div align="center">

# Huashu Design

> *「Gõ phím. Enter. Một bản thiết kế hoàn chỉnh sẵn sàng giao hàng.」*
> *"Type. Hit enter. A finished design lands in your lap."*

[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Agent-Agnostic](https://img.shields.io/badge/Agent-Agnostic-blueviolet)](https://skills.sh)
[![Skills](https://img.shields.io/badge/skills.sh-Compatible-green)](https://skills.sh)

<br>

**Gõ một câu lệnh trong agent của bạn, nhận lại một bản thiết kế đạt chuẩn giao hàng.**

<br>

Chỉ từ 3 đến 30 phút, bạn có thể hoàn thành (ship) một **hoạt ảnh ra mắt sản phẩm**, một prototype App có thể nhấp chọn, một bộ PPT có thể chỉnh sửa, hoặc một Infographic đạt chuẩn in ấn.

Không phải mức độ "AI làm cũng tạm được" — mà trông như sản phẩm từ đội ngũ thiết kế của các tập đoàn công nghệ lớn. Đưa cho skill các tài sản thương hiệu của bạn (logo, bảng màu, ảnh chụp màn hình UI), nó sẽ hiểu được khí chất thương hiệu; nếu không đưa gì cả, **cố vấn 3 bộ lô-gích + thư viện 60 phong cách HTML nguyên bản** cũng sẽ bọc lót để không bao giờ tạo ra AI slop.

**Mỗi hoạt ảnh bạn nhìn thấy trong tệp README này đều do huashu-design tự tạo ra.** Không dùng Figma, không dùng AE, chỉ một câu prompt + chạy skill. Lần phát hành sản phẩm tới cần làm phim quảng cáo? Giờ đây bạn cũng có thể tự làm.

```
npx skills add alchaincyf/huashu-design
```

Tương thích đa agent — Claude Code, Cursor, Codex, OpenClaw, Hermes đều có thể cài đặt.

> 📣 **Đã chuyển sang giấy phép MIT.** Từ ngày 14/05/2026, skill này hoàn toàn mở mã nguồn ([MIT License](LICENSE)), **miễn phí cho cá nhân lẫn thương mại**, không cần xin phép trước. Điều khoản cũ "Miễn phí cá nhân, thương mại cần cấp phép" đã bãi bỏ. ([Xem thay đổi](#license))

[Xem hiệu ứng](#demo-trình-diễn) · [Cài đặt](#cài-đặt-dùng-ngay) · [Tính năng](#có-thể-làm-gì) · [Cơ chế cốt lõi](#cơ-chế-cốt-lõi) · [Mối quan hệ với Claude Design](#mối-quan-hệ-với-claude-design)

</div>

---

<p align="center">
  <img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.gif" alt="huashu-design Hero · Gõ phím → Chọn hướng → Mở rộng thư viện → Hội tụ → Hiện thương hiệu" width="100%">
</p>

<p align="center"><sub>
  ▲ 25 giây · Terminal → 4 hướng → Gallery ripple → 4 lần Focus → Brand reveal<br>
  👉 <a href="https://www.huasheng.ai/huashu-design-hero/">Truy cập bản tương tác HTML có âm thanh</a> ·
  <a href="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.mp4">Tải MP4 (Gồm BGM+SFX · 10MB)</a>
</sub></p>

---

## 📺 Hướng dẫn cho người mới (Hoa Chú trực tiếp ghi hình)

Chưa biết cách dùng? Xem hướng dẫn nhập môn huashu-design do Hoa Chú ghi hình:

<p align="center">
  <a href="https://www.youtube.com/watch?v=m-_BlUdcIvw"><img src="https://img.youtube.com/vi/m-_BlUdcIvw/maxresdefault.jpg" alt="Hướng dẫn sử dụng huashu-design" width="70%"></a>
</p>

<p align="center"><sub>👉 <a href="https://www.youtube.com/watch?v=m-_BlUdcIvw">Xem hướng dẫn đầy đủ trên YouTube</a></sub></p>

---

## Cài đặt dùng ngay

```bash
npx skills add alchaincyf/huashu-design
```

> **Cài xong nhớ tự kiểm tra**: Skill này không chỉ có duy nhất một tệp SKILL.md, mà trong 4 thư mục con `references/`, `assets/`, `scripts/`, `demos/` có tới 99 công thức, script, tư liệu được tham chiếu, không thể thiếu cái nào. Cài xong hãy xem qua thư mục cài đặt (ví dụ `~/.claude/skills/huashu-design/`), nếu chỉ có SKILL.md mà không có các thư mục con đó, nghĩa là phiên bản `skills` CLI của bạn quá cũ (phiên bản ≤1.5.15 có lỗi chỉ đồng bộ đơn tệp, đã sửa ở 1.5.19). Nâng cấp rồi cài lại là được:
>
> ```bash
> npm i -g skills@latest        # Hoặc npx skills@latest add alchaincyf/huashu-design
> ```
>
> Nếu sau khi nâng cấp vẫn gặp sự cố, dùng `git clone` để cài bọc lót, clone kho lưu trữ vào bất kỳ thư mục skills nào:
>
> ```bash
> git clone https://github.com/alchaincyf/huashu-design.git ~/.claude/skills/huashu-design
> ```

Sau đó trong Claude Code / Codex / Cursor hoặc bất kỳ agent nào hỗ trợ skills, bạn chỉ cần nói trực tiếp:

```
"Làm một slide PPT bài thuyết trình về Tâm lý học AI, gợi ý 3 hướng phong cách cho tôi chọn"
"Làm một prototype iOS Đồng hồ Pomodoro AI, 4 màn hình chính phải thực sự nhấp chuyển được"
"Chuyển đoạn lô-gích này thành hoạt ảnh 60 giây, xuất file MP4 và GIF"
"Giúp tôi đánh giá chuyên gia 5 chiều cho bản thiết kế này"
```

Không có nút bấm, không có bảng điều khiển, không có plugin Figma.

---

## Xu hướng Star

<p align="center">
  <a href="https://star-history.com/#alchaincyf/huashu-design&Date">
    <img src="https://api.star-history.com/svg?repos=alchaincyf/huashu-design&type=Date" alt="Lịch sử Star huashu-design" width="80%">
  </a>
</p>

---

## Có thể làm gì

| Năng lực | Sản phẩm giao hàng | Thời gian trung bình |
|------|--------|----------|
| Prototype tương tác (App / Web) | HTML đơn tệp · Khung iPhone thật · Tương tác được · Xác thực bằng Playwright | 10–15 phút |
| Slide thuyết trình | HTML deck (Thuyết trình trên trình duyệt) + PPTX chỉnh sửa được (Giữ nguyên text frame) | 15–25 phút |
| Hoạt ảnh theo timeline | MP4 (25fps / 60fps nội suy khung hình) + GIF (tối ưu bảng màu) + BGM | 8–12 phút |
| Biến thể thiết kế | 3+ bản so sánh song song · Tinh chỉnh tham số Tweaks thời gian thực · Khám phá đa chiều | 10 phút |
| Infographic / Trực quan hóa | Dàn trang cấp in ấn · Có thể xuất PDF/PNG/SVG | 10 phút |
| Cố vấn Hướng Thiết kế | **Chạy song song 3 bộ lô-gích** (Vòng quay số giây + Ca tham chiếu đoạt giải + Nhà thiết kế giỏi nhất) · Xuất trực tiếp 3 bản thị giác thực tế | 5 phút |
| Đánh giá chuyên gia 5 chiều | Biểu đồ radar + Danh sách Keep/Fix/Quick Wins · Danh sách sửa chữa có thể thực thi | 3 phút |

---

## Demo trình diễn

### Cố vấn Hướng Thiết kế

Bọc lót khi nhu cầu mơ hồ: **Chạy song song 3 bộ lô-gích bổ trợ cho nhau** — Vòng quay số giây (20 chọn 1 để phá vỡ quán tính) + Tham chiếu thực tế (Chuyển giao từ website đoạt giải thế giới) + Nhà thiết kế giỏi nhất (Triết lý từ các studio hàng đầu), trực tiếp xuất ra 3 bản **thị giác thực tế** cho bạn nhìn chọn, không bắt bạn chọn phong cách qua chữ viết mù mịt. Phía sau là **thư viện 60 phong cách HTML nguyên bản** (Web 20 + PPT 20 + Infographic 20, thuần CSS không cần tạo ảnh AI).

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w3-fallback-advisor.gif" width="100%"></p>

### Prototype App iOS

Thân máy iPhone 15 Pro chuẩn xác (Dynamic Island / Status bar / Home Indicator) · Chuyển đổi nhiều màn hình theo trạng thái · Ảnh thật lấy từ Wikimedia/Met/Unsplash · Test nhấp tự động bằng Playwright.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c1-ios-prototype.gif" width="100%"></p>

### Engine Motion Design

Mô hình phân đoạn thời gian Stage + Sprite · 4 API `useTime` / `useSprite` / `interpolate` / `Easing` bao phủ mọi nhu cầu hoạt ảnh · Một dòng lệnh xuất MP4 / GIF / 60fps nội suy / Thành phẩm kèm BGM.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c3-motion-design.gif" width="100%"></p>

### HTML Slides → PPTX chỉnh sửa được

Thuyết trình bằng HTML deck trên trình duyệt · `html2pptx.js` đọc computedStyle của DOM dịch từng phần tử thành đối tượng PowerPoint · Xuất ra **khung chữ thật**, nhấp đôi trong PPT là chỉnh sửa trực tiếp được.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c2-slides-pptx.gif" width="100%"></p>

### Tweaks · Chuyển đổi biến thể thời gian thực

Tham số hóa phối màu / kiểu phông / độ đậm đặc thông tin · Chuyển đổi qua bảng điều khiển bên hông · Thuần frontend + lưu trữ `localStorage` · F5 không mất.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c4-tweaks.gif" width="100%"></p>

### Infographic / Trực quan hóa dữ liệu

Dàn trang cấp tạp chí · Phân cột chính xác bằng CSS Grid · Chi tiết dàn trang `text-wrap: pretty` · Dữ liệu thật dẫn dắt · Có thể xuất PDF vector / PNG 300dpi / SVG.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c5-infographic.gif" width="100%"></p>

### Đánh giá chuyên gia 5 chiều

Nhất quán triết lý · Phân cấp thị giác · Thực thi chi tiết · Tính chức năng · Tính sáng tạo mỗi chiều 0–10 điểm · Trực quan hóa bằng biểu đồ radar · Xuất danh sách Keep / Fix / Quick Wins.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c6-expert-review.gif" width="100%"></p>

### Quy trình làm việc Junior Designer

Không cắm đầu làm đòn lớn: Viết assumptions + placeholders + reasoning trước, cho bạn xem càng sớm càng tốt rồi mới lặp lại. Hiểu sai sửa sớm rẻ hơn sửa muộn 100 lần.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w2-junior-designer.gif" width="100%"></p>

### 5 bước cứng Giao thức tài sản thương hiệu

Bắt buộc thực hiện khi liên quan đến thương hiệu cụ thể: Hỏi → Tìm → Tải (3 đường bọc lót) → grep mã màu → Viết `brand-spec.md`.

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w1-brand-protocol.gif" width="100%"></p>

---

## Showcase · Ca lâm sàng thực tế

### Website Lịch sử Tiến hóa Chim Vẹt · Thực chiến 3 bộ lô-gích Cố vấn Hướng Thiết kế (2.0)

> **Live demo · [https://www.huasheng.ai/parrots/](https://www.huasheng.ai/parrots/)**

Chỉ một câu "Làm một website giới thiệu lịch sử tiến hóa của chim vẹt", không có yêu cầu thêm, skill tự động chạy trọn bộ quy trình cố vấn 2.0: Nhận định ảnh là nội dung bắt buộc → Bắt tranh minh họa cổ điển tài sản công cộng (Tranh chim vẹt của Edward Lear / John Gould) → **Chạy song song 3 bộ lô-gích** (Vòng quay số giây + Ca tham chiếu đoạt giải + Triết lý "Trắng" của Hara Kenya) mỗi bộ xuất 1 bản thị giác thực tế. **Tư liệu đủ rồi mới thiết kế, chứ không phải vừa thiết kế vừa dùng ô màu giữ chỗ.**

### 「Trò chuyện về skill」 · Slide thuyết trình PM After-Party

> **Live demo · [https://skill-huasheng.vercel.app](https://skill-huasheng.vercel.app)**

13 trang HTML deck, **toàn bộ hoàn thành bằng huashu-design**:

- Hệ thống thị giác chữ Serif tối giản trên nền đen (cover / about / hook / what / why / closing)
- 2 cinematic demo 22 giây có BGM + SFX (Nuwa skill workflow + Darwin skill workflow), mỗi cái dùng **ngôn ngữ thị giác hoàn toàn độc lập**:
  - **Nuwa**: Quỹ đạo kiến thức 3D + Cô đọng Pentagon + Đánh chữ SKILL.md + Cảnh lộ diện hero "21 phút"
  - **Darwin**: Vòng xoay autoresearch loop + So sánh song song v1/v5 + Đường cong toàn màn hình Hill-Climb + Khóa bánh răng Ratchet
- Mỗi cinematic mặc định hiển thị **dashboard workflow tĩnh hoàn chỉnh** (Khán giả bất kỳ lúc nào cũng thấy rõ skill chạy thế nào), bấm ▶ mới kích hoạt hoạt ảnh, chạy xong tự động mờ về dashboard
- Dữ liệu thật: Đường cong thực tế 14,495 stargazers (Kéo từ GitHub API) + Thông số thực tế DeepSeek V4 (Xác thực qua WebSearch)
- Tư liệu AI thật: Dùng `huashu-gpt-image` chạy lưới ảnh lớn 4×2, `extract_grid.py` tách ra 8 ảnh PNG trong suốt độc lập làm trôi 3D orbit

**Các trang thích hợp để tham khảo**:
- `/slides/slide-04b-nuwa-flow.html` · Kiến trúc 2 lớp dashboard tĩnh + cinematic overlay
- `/slides/slide-06b-darwin-flow.html` · Ca đối chiếu ngôn ngữ thị giác hoàn toàn độc lập
- `/slides/slide-03b-deepseek-cover.html` · Trang so sánh góc nhìn AI slop vs Nhà thiết kế thực thụ

Chi tiết mô hình cinematic xem `references/cinematic-patterns.md`.

---

## Cơ chế cốt lõi

### Giao thức tài sản thương hiệu

Quy tắc cứng nhất trong skill. Khi liên quan đến thương hiệu cụ thể (Stripe, Linear, Anthropic, công ty của bạn,...), bắt buộc thực hiện 5 bước:

| Bước | Hành động | Mục đích |
|------|------|------|
| 1 · Hỏi | Người dùng có brand guidelines không? | Tôn trọng tài nguyên đã có |
| 2 · Tìm trang thương hiệu chính thức | `<brand>.com/brand` · `brand.<brand>.com` · `<brand>.com/press` | Nắm bắt mã màu uy tín |
| 3 · Tải tài sản | Tệp SVG → HTML trang chủ chính thức → Trích màu ảnh chụp màn hình | 3 đường bọc lót, bước trước lỗi bước sau lập tức chạy |
| 4 · Trích xuất mã màu qua grep | Lấy tất cả `#xxxxxx` từ tài sản, sắp xếp theo tần suất, lọc đen trắng xám | **Tuyệt đối không đoán màu thương hiệu từ trí nhớ** |
| 5 · Cố định spec | Viết `brand-spec.md` + Biến CSS, mọi HTML đều tham chiếu `var(--brand-*)` | Không cố định sẽ bị quên |

Thử nghiệm A/B (v1 vs v2, mỗi bản chạy 6 agent): **Độ lệch ổn định của v2 thấp hơn 5 lần so với v1**. Độ ổn định của sự ổn định, đây mới chính là hào khí thực sự của skill.

### Cố vấn Hướng Thiết kế (Fallback)

Kích hoạt khi yêu cầu người dùng mơ hồ đến mức không thể bắt tay vào làm (Tái thiết kế trong 2.0):

- Hội thoại làm rõ trước + Chủ động xin tham chiếu (Tên / logo / màu thương hiệu / trang tham chiếu yêu thích)
- Thu thập đủ ảnh thật cần thiết cho nội dung (Kho công cộng / Miễn bản quyền, script bắt bằng một dòng lệnh), rồi mới làm
- **Chạy subagent song song 3 bộ lô-gích bổ trợ cho nhau**, mỗi bộ xuất 1 bản **thị giác thực tế**: ① Vòng quay số giây (Lấy số giây `date +%S`, 20 chọn 1, phá vỡ quán tính thiên vị tối giản của mô hình) ② Tham chiếu thực tế (Chuyển giao website / PPT / prototype iOS đoạt giải thế giới) ③ Nhà thiết kế giỏi nhất (Triết lý studio phù hợp nhất khi ngân sách không giới hạn)
- **Tuyệt đối không bắt bạn chọn phong cách mù mịt khi chưa thấy thị giác** — Bày 3 bản ra, nhìn thực tế mà chọn
- Chọn xong chuyển sang quy trình Junior Designer nhánh chính
- Nền tảng là **thư viện 60 phong cách HTML nguyên bản** (Web 20 + PPT 20 + Infographic 20, phân cấp Táo bạo / Trung tính / Yên tĩnh, thuần CSS không cần tạo ảnh AI) làm đạn dược, không phải giáo điều

### Quy trình làm việc Junior Designer

Chế độ làm việc mặc định, xuyên suốt mọi nhiệm vụ:

- Trước khi làm phát danh sách câu hỏi một lần cho người dùng, chờ trả lời xong mới tay làm
- Trong HTML viết assumptions + placeholders + reasoning comments trước
- Cho người dùng xem càng sớm càng tốt (Dù chỉ là các ô màu xám)
- Điền nội dung thực tế → variations → Tweaks 3 bước này mỗi bước cho xem lại một lần
- Trước khi giao hàng dùng Playwright mắt thấy qua trình duyệt một lần

### Quy tắc chống AI Slop

Né tránh bội số chung nhỏ nhất về thị giác nhìn phát biết ngay AI (Chuyển màu tím / Icon emoji / Bo góc+lề trái accent / SVG vẽ mặt người / Inter làm display). Dùng `text-wrap: pretty` + CSS Grid + Phông serif display chọn lọc kỹ càng và phối màu oklch.

---

## Mối quan hệ với Claude Design

Tôi công khai thừa nhận: Triết lý của Giao thức tài sản thương hiệu là học hỏi từ các prompt rò rỉ của Claude Design. Bản prompt đó liên tục nhấn mạnh **một thiết kế hi-fi tốt không phải bắt đầu từ tờ giấy trắng, mà mọc ra từ ngữ cảnh thiết kế sẵn có**. Nguyên tắc này là đường ranh giới phân chia giữa tác phẩm 65 điểm và tác phẩm 90 điểm.

Khác biệt về định vị:

| | Claude Design | huashu-design |
|---|---|---|
| Hình thái | Sản phẩm web (Dùng trên trình duyệt) | Skill (Dùng trong Claude Code) |
| Hạn mức | Quota đăng ký gói | Hạn mức API · Chạy agent song song không bị giới hạn quota |
| Sản phẩm | Trong canvas + Có thể xuất Figma | HTML / MP4 / GIF / PPTX chỉnh sửa được / PDF |
| Cách thao tác | GUI (Bấm, kéo, sửa) | Hội thoại (Nói chuyện, chờ agent làm xong) |
| Hoạt ảnh phức tạp | Hạn chế | Trục thời gian Stage + Sprite · Xuất 60fps |
| Đa agent | Tương thích riêng Claude.ai | Tương thích với bất kỳ agent nào hỗ trợ skill |

Claude Design là **công cụ đồ họa tốt hơn**, huashu-design là **làm cho lớp công cụ đồ họa biến mất**. Hai con đường, hai đối tượng người dùng khác nhau.

---

## An toàn và Dòng dữ liệu

Luồng cốt lõi (Thiết kế → Render → Xuất MP4/PDF/PPTX) **100% chạy cục bộ, không cần mạng, không cần key**. Năng lực cloud (Lồng tiếng Doubao TTS, AI review phim) toàn bộ được cách ly trong `scripts/cloud/`, hoàn toàn tự chọn: Dùng key của chính bạn, chỉ gửi tới API chính thức của nhà cung cấp, lần đầu gọi cần `--yes` xác nhận rõ ràng. Không có telemetry, không gửi bất kỳ dữ liệu nào về server tác giả. Khai báo chi tiết về toàn bộ domain ra ngoài, xử lý secret, ranh giới xóa xem tại [SECURITY.md](SECURITY.md), hoan nghênh bạn dùng agent đối soát từng dòng code.

---

## Hạn chế (Limitations)

- **Không hỗ trợ xuất PPTX sang Figma dạng layer chỉnh sửa được**. Xuất ra HTML, có thể chụp màn hình, quay màn hình, xuất ảnh, nhưng không thể kéo vào Keynote sửa vị trí chữ.
- **Không làm được hoạt ảnh phức tạp cấp độ Framer Motion**. 3D, mô phỏng vật lý, hệ thống hạt vượt quá ranh giới của skill.
- **Thương hiệu hoàn toàn trắng thiết kế từ đầu chất lượng sẽ giảm xuống 60–65 điểm**. Tự vẽ hi-fi từ không khí vốn dĩ là giải pháp cuối cùng (last resort).

Đây là một skill 80 điểm, không phải sản phẩm 100 điểm. Đối với những người không muốn mở giao diện đồ họa, một skill 80 điểm dễ dùng hơn một sản phẩm 100 điểm.

---

## Cấu trúc kho lưu trữ

```
huashu-design/
├── SKILL.md                 # Tài liệu chính (Cho agent đọc)
├── README.md                # README tiếng Việt (Mặc định, tệp này)
├── README.en.md             # README tiếng Anh
├── assets/                  # Starter Components
│   ├── animations.jsx       # Stage + Sprite + Easing + interpolate
│   ├── ios_frame.jsx        # iPhone 15 Pro bezel
│   ├── android_frame.jsx
│   ├── macos_window.jsx
│   ├── browser_window.jsx
│   ├── deck_stage.js        # Engine slide thuyết trình HTML
│   ├── deck_index.html      # Bộ ghép deck nhiều tệp
│   ├── design_canvas.jsx    # Hiển thị biến thể song song
│   ├── showcases/           # 24 mẫu dựng sẵn (8 kịch bản × 3 phong cách)
│   └── bgm-*.mp3            # 6 bài nhạc nền kịch bản
├── references/              # Tài liệu đọc sâu theo nhiệm vụ
│   ├── animation-pitfalls.md
│   ├── design-styles.md     # Thư viện 60 phong cách HTML nguyên bản (Web 20 + PPT 20 + Infographic 20)
│   ├── slide-decks.md
│   ├── editable-pptx.md
│   ├── critique-guide.md
│   ├── video-export.md
│   └── ...
├── scripts/                 # Chuỗi công cụ xuất file
│   ├── render-video.js      # HTML → MP4
│   ├── convert-formats.sh   # MP4 → 60fps + GIF
│   ├── add-music.sh         # MP4 + BGM
│   ├── export_deck_pdf.mjs
│   ├── export_deck_pptx.mjs
│   ├── html2pptx.js
│   └── verify.py
└── demos/                   # 9 demo năng lực (c*/w*), hai bản GIF/MP4/HTML Trung-Anh + hero v10
```

---

## Nguồn gốc

Ngày Anthropic ra mắt Claude Design, tôi đã trải nghiệm đến 4 giờ sáng. Mấy ngày sau tôi nhận ra mình chưa bao giờ mở lại nó nữa, không phải vì nó không tốt — nó là sản phẩm chín chắn nhất trong mảng này hiện tại — mà là vì tôi thà để agent giúp mình làm việc trong terminal hơn là mở bất kỳ giao diện đồ họa nào.

Thế là tôi để agent bóc tách bản thân Claude Design (Bao gồm các system prompt lan truyền trong cộng đồng, giao thức tài sản thương hiệu, cơ chế component), cô đọng thành spec cấu trúc, rồi viết thành skill cài vào Claude Code của mình.

Cảm ơn Anthropic đã viết prompt của Claude Design rất rõ ràng. Việc sáng tạo thứ cấp dựa trên cảm hứng từ sản phẩm khác chính là hình thái mới của văn hóa mã nguồn mở trong thời đại AI.

---

## Sản phẩm làm bằng huashu-design

Ba giao diện giao diện của **[FanBox · Khoang lái cho Coding Agent](https://github.com/alchaincyf/fanbox)** chính là được thiết kế bằng huashu-design. Chỉ huy Claude Code / Codex làm việc, nhìn rõ từng tệp nó đã chạm vào, từng dòng code đã sửa.

[![FanBox · Khoang lái cho Coding Agent](https://raw.githubusercontent.com/alchaincyf/fanbox/master/assets/promo-banner.jpg)](https://github.com/alchaincyf/fanbox)

---

## Phiên bản dịch từ cộng đồng

Các phiên bản dịch do cộng đồng bảo trì. Chất lượng dịch thuật và các điều khoản license của từng phiên bản do người bảo trì tương ứng chịu trách nhiệm, vui lòng xác nhận trước khi sử dụng.

| Ngôn ngữ | Người bảo trì | Kho lưu trữ |
|---|---|---|
| English | [@namandhakad712](https://github.com/namandhakad712) | [namandhakad712/huashu-design-en](https://github.com/namandhakad712/huashu-design-en) |
| 한국어 (Tiếng Hàn) | [@ktkarchive](https://github.com/ktkarchive) | [ktkarchive/ktk-design](https://github.com/ktkarchive/ktk-design) |
| Tiếng Việt | [@letrquan](https://github.com/letrquan) | [letrquan/huashu-design](https://github.com/letrquan/huashu-design) |

Muốn thêm ngôn ngữ của bạn? Fork kho lưu trữ, dịch `SKILL.md` + `README.md`, rồi mở một issue tại đây, tôi sẽ thêm link vào.

---

## Giấy phép (License)

**Từ ngày 14/05/2026 đổi sang giấy phép MIT.** Các phiên bản trước đó sử dụng Personal Use License "Miễn phí cá nhân, thương mại cần cấp phép", có giới hạn đối với thương mại — Giờ đây rào cản đó đã hoàn toàn được gỡ bỏ.

Theo [MIT License](LICENSE), bạn có thể **tự do sử dụng, chỉnh sửa, phân phối** skill này, **bao gồm cả mục đích thương mại** — Dùng nội bộ công ty, giao hàng cho khách, làm thành sản phẩm thu phí bán ra ngoài đều được. Không cần xin phép trước, không cần trả phí, không cần chào hỏi. Không bắt buộc ghi nguồn, nhưng luôn được hoan nghênh.

---

## Kết nối · Hoa Sinh (Hoa Chú)

Hoa Sinh là AI Native Coder, nhà phát triển độc lập, nhà sáng tạo nội dung AI. Tác phẩm tiêu biểu: Đèn trợ sáng Mèo Con (Top 1 trả phí AppStore), "Chinh phục DeepSeek trong một cuốn sách", Nữ Oa .skill (12,000+ star GitHub). Hơn 300,000+ người theo dõi trên các nền tảng mạng xã hội.

| Nền tảng | Tài khoản | Liên kết |
|---|---|---|
| X / Twitter | @AlchainHust | https://x.com/AlchainHust |
| Wechat Official | 花叔 | Tìm kiếm WeChat 「花叔」 |
| Bilibili | 花叔 | https://space.bilibili.com/14097567 |
| YouTube | 花叔 | https://www.youtube.com/@Alchain |
| Xiaohongshu | 花叔 | https://www.xiaohongshu.com/user/profile/5abc6f17e8ac2b109179dfdf |
| Trang chủ | huasheng.ai | https://www.huasheng.ai/ |
| Trang Developer | bookai.top | https://bookai.top |

Hợp tác tư vấn, viết bài truyền thông → Nhắn tin riêng cho Hoa Sinh trên bất kỳ nền tảng nào ở trên.
