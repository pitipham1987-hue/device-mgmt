# Slide Decks: Quy chuẩn làm HTML Slide Presentation

Tạo slide presentation (trình chiếu) là kịch bản công việc tần suất cao trong thiết kế. Tài liệu này hướng dẫn cách tạo HTML Slide xuất sắc — từ lựa chọn kiến trúc, thiết kế trang đơn, đến quy trình hoàn chỉnh để xuất sang PDF/PPTX.

**Phạm vi năng lực của skill này**:
- **Bản trình chiếu HTML (Sản phẩm cơ sở, LUÔN LUÔN mặc định phải làm)** → Mỗi trang là một HTML độc lập + `assets/deck_index.html` tổng hợp, chuyển trang bằng bàn phím trong trình duyệt, thuyết trình toàn màn hình
- HTML → Xuất PDF → `scripts/export_deck_pdf.mjs` / `scripts/export_deck_stage_pdf.mjs`
- HTML → Xuất PPTX có thể chỉnh sửa → `references/editable-pptx.md` + `scripts/html2pptx.js` + `scripts/export_deck_pptx.mjs` (yêu cầu HTML tuân thủ 4 quy tắc cứng)

> **⚠️ HTML là nền tảng, PDF/PPTX là sản phẩm dẫn xuất.** Dù định dạng bàn giao cuối cùng là gì, **bắt buộc** phải làm bản trình chiếu tổng hợp HTML trước (`index.html` + `slides/*.html`), đó là "nguồn gốc" của tác phẩm slide. PDF/PPTX chỉ là ảnh chụp (snapshot) được xuất ra từ HTML bằng một dòng lệnh.
>
> **Tại sao HTML lại ưu tiên hàng đầu**:
> - Dùng tốt nhất khi thuyết trình trực tiếp (máy chiếu / chia sẻ màn hình trực tiếp toàn màn hình, chuyển trang bằng bàn phím, không phụ thuộc vào phần mềm Keynote/PPT)
> - Trong quá trình phát triển, từng trang có thể nhấp đôi mở trực tiếp để kiểm tra, không cần mỗi lần phải chạy lại lệnh xuất
> - Là nguồn upstream duy nhất để xuất PDF/PPTX (tránh vòng lặp luẩn quẩn "xuất xong mới phát hiện cần sửa HTML lại phải xuất lại")
> - Sản phẩm bàn giao có thể là "HTML + PDF" hoặc "HTML + PPTX", bên nhận thích dùng bản nào thì dùng
>
> Thực tế dự án moxt brochure 22-04-2026: Sau khi làm xong 13 trang HTML + tổng hợp `index.html`, chỉ một dòng lệnh `export_deck_pdf.mjs` xuất ra PDF không cần chỉnh sửa gì thêm. Bản thân bản HTML đã là sản phẩm bàn giao có thể dùng thuyết trình trực tiếp trên trình duyệt.

---

## 🛑 Xác nhận định dạng bàn giao trước khi bắt đầu (Checkpoint cứng nhất)

**Quyết định này ưu tiên hơn việc chọn "đơn file hay đa file".** Thực tế dự án期权私董会 (Option Private Board) 20-04-2026: **Không xác nhận định dạng bàn giao trước khi làm = tốn 2-3 giờ làm lại.**

### Cây quyết định (Kiến trúc HTML-first)

Tất cả các sản phẩm bàn giao đều bắt đầu từ cùng một bộ trang tổng hợp HTML (`index.html` + `slides/*.html`). Định dạng bàn giao chỉ quyết định **quy tắc viết HTML** và **lệnh xuất**:

```
【LUÔN MẶC ĐỊNH · BẮT BUỘC LÀM】 Bản trình chiếu tổng hợp HTML (index.html + slides/*.html)
   │
   ├── Chỉ thuyết trình trên trình duyệt / Lưu trữ HTML cục bộ → Đến đây đã hoàn thành, độ tự do thị giác HTML cao nhất
   │
   ├── Cần thêm PDF (In ấn / Gửi nhóm / Lưu trữ) → Chạy export_deck_pdf.mjs xuất trong 1 cú nhấp
   │                                               Cách viết HTML tự do, thị giác không hạn chế
   │
   └── Cần thêm PPTX có thể chỉnh sửa (Đồng nghiệp cần sửa chữ) → Bắt đầu từ dòng HTML đầu tiên viết theo 4 quy tắc cứng
                                               Chạy export_deck_pptx.mjs xuất trong 1 cú nhấp
                                               Hy sinh gradient / web component / SVG phức tạp
```

### Lời thoại xác nhận trước khi làm (Dùng được ngay)

> Dù sản phẩm bàn giao cuối cùng là HTML, PDF hay PPTX, tôi đều sẽ làm một bản tổng hợp HTML chạy trên trình duyệt (chuyển trang bằng bàn phím trong `index.html`) trước — đây luôn là sản phẩm nền tảng mặc định. Sau đó mới hỏi bạn xem có cần xuất thêm snapshot PDF / PPTX không.
>
> Bạn cần định dạng xuất nào?
> - **Chỉ cần HTML** (Thuyết trình/Lưu trữ) → Độ tự do thị giác hoàn toàn
> - **Cần thêm PDF** → Giống như trên, thêm một lệnh xuất
> - **Cần thêm PPTX có thể chỉnh sửa** (Đồng nghiệp sẽ sửa chữ trong PPT) → Tôi phải viết theo 4 quy tắc cứng ngay từ dòng HTML đầu tiên, sẽ phải hy sinh một số khả năng thị giác (không gradient, không web component, không SVG phức tạp).

### Tại sao "Cần PPTX thì phải tuân thủ 4 quy tắc cứng ngay từ đầu"

Điều kiện để PPTX có thể chỉnh sửa là `html2pptx.js` có thể dịch từng phần tử DOM sang đối tượng PowerPoint. Nó yêu cầu **4 quy tắc cứng**:

1. body cố định 960pt × 540pt (khớp với `LAYOUT_WIDE`, 13.333″ × 7.5″, không phải 1920×1080px)
2. Tất cả văn bản phải nằm trong `<p>`/`<h1>`-`<h6>` (Cấm div chứa văn bản trực tiếp, cấm dùng `<span>` chứa văn bản chính)
3. Bản thân `<p>`/`<h*>` không được có background/border/shadow (đặt ở div bên ngoài)
4. `<div>` không được dùng `background-image` (dùng thẻ `<img>`)
5. Không dùng CSS gradient, không dùng web component, không dùng trang trí SVG phức tạp

**HTML mặc định của skill này có độ tự do thị giác rất cao** — dùng nhiều span, flex lồng nhau, SVG phức tạp, web component (như `<deck-stage>`), CSS gradient — **hầu như không có cái nào tự nhiên vượt qua quy tắc của html2pptx** (thử nghiệm thực tế HTML thuần thị giác đưa trực tiếp vào html2pptx, tỷ lệ pass < 30%).

### So sánh chi phí hai con đường thực tế (Thực tế vấp phải ngày 20-04-2026)

| Con đường | Cách làm | Kết quả | Chi phí |
|-----------|----------|---------|---------|
| ❌ **Viết HTML tự do trước, sửa chữa PPTX sau** | Đơn file deck-stage + nhiều trang trí SVG/span | Muốn PPTX chỉnh sửa chỉ còn 2 đường:<br>A. Tự viết vài trăm dòng code pptxgenjs hardcode tọa độ<br>B. Viết lại 17 trang HTML sang định dạng Path A | Tốn 2-3 giờ làm lại, và bản tự viết **chi phí bảo trì vĩnh viễn** (HTML sửa 1 chữ, PPTX phải đồng bộ thủ công) |
| ✅ **Viết theo quy tắc Path A ngay từ bước đầu** | Mỗi trang HTML độc lập + 4 quy tắc cứng + 960×540pt | Một dòng lệnh xuất PPTX chỉnh sửa 100%, đồng thời thuyết trình toàn màn hình trên trình duyệt được luôn (Path A HTML chính là HTML chuẩn chạy được trên trình duyệt) | Tốn thêm 5 phút khi viết HTML để nghĩ "làm sao bọc văn bản vào `<p>`", 0 tốn công làm lại |

### Xử lý việc bàn giao hỗn hợp

Khách hàng nói "Tôi muốn cả HTML thuyết trình **và** PPTX chỉnh sửa được" — **Đây không phải hỗn hợp**, mà là yêu cầu PPTX bao trùm yêu cầu HTML. HTML viết theo Path A bản thân nó đã có thể thuyết trình toàn màn hình trên trình duyệt (chỉ cần thêm bộ ghép `deck_index.html`). **Không có thêm chi phí.**

Khách hàng nói "Tôi muốn PPTX **và** animation / web component" — **Đây mới là mâu thuẫn thật**. Hãy báo với khách hàng: Muốn PPTX chỉnh sửa được thì phải hy sinh những tính năng thị giác đó. Để khách hàng lựa chọn, đừng âm thầm chọn phương án tự viết code pptxgenjs (sẽ trở thành nợ bảo trì vĩnh viễn).

### Phát hiện cần PPTX sau khi đã hoàn thành thì làm sao (Cứu hộ khẩn cấp)

Trường hợp cực kỳ hiếm: HTML đã viết xong mới phát hiện cần PPTX. Khuyến nghị đi theo **quy trình fallback** (xem chi tiết ở cuối `references/editable-pptx.md` phần "Fallback: Đã có bản thiết kế thị giác nhưng khách hàng bắt buộc đòi editable PPTX"):

1. **Ưu tiên 1: Chuyển sang PDF** (Giữ lại 100% thị giác, đa nền tảng, bên nhận xem được và in được) — Nếu nhu cầu thực tế của bên nhận là "thuyết trình/lưu trữ", PDF chính là sản phẩm bàn giao tốt nhất
2. **Ưu tiên 2: AI lấy bản thiết kế thị giác làm mẫu, viết lại một bản editable HTML** → Xuất editable PPTX — Giữ lại quyết định thiết kế về màu sắc/bố cục/văn bản, hy sinh gradient, web component, SVG phức tạp
3. **Không khuyến nghị: Tự viết pptxgenjs dựng lại** — Vị trí, font chữ, căn chỉnh đều phải chỉnh bằng tay, chi phí bảo trì cao, và sau này HTML sửa 1 chữ cũng phải đồng bộ bằng tay lại

Luôn đưa ra lựa chọn cho khách hàng để họ quyết định. **Đừng bao giờ phản ứng đầu tiên là nhảy vào tự viết pptxgenjs** — đó là biện pháp giải quyết cuối cùng.

---

## 🛑 Trước khi làm hàng loạt: Làm 2 trang showcase định hình grammar trước

**Chỉ cần deck ≥ 5 trang, tuyệt đối không được viết trực tiếp từ trang 1 đến trang cuối cùng.** Thứ tự đúng đã được kiểm chứng thực tế qua moxt brochure ngày 22-04-2026:

1. Chọn **2 loại trang có sự khác biệt thị giác lớn nhất** làm showcase trước (như "Bìa" + "Trang cảm xúc/Trích dẫn", hoặc "Bìa" + "Trang giới thiệu sản phẩm")
2. Chụp màn hình để khách hàng xác nhận grammar (masthead / font chữ / màu sắc / khoảng cách / cấu trúc / tỷ lệ song ngữ Trung-Anh)
3. Định hướng được duyệt rồi mới làm hàng loạt N-2 trang còn lại, mỗi trang tái sử dụng grammar đã thiết lập
4. Sau khi hoàn thành tất cả thì tổng hợp lại thành HTML tổng hợp + sản phẩm dẫn xuất PDF / PPTX

**Tại sao**: Viết thẳng 13 trang đến cuối → Khách hàng nói "sai định hướng" = Làm lại 13 lần. Làm 2 trang showcase trước → Sai định hướng = Làm lại 2 lần. Một khi visual grammar đã được xác lập, không gian quyết định cho N trang sau sẽ thu hẹp đáng kể, chỉ còn lại "đưa nội dung vào như thế nào".

**Nguyên tắc chọn trang showcase**: Chọn hai trang có cấu trúc thị giác khác nhau nhất. Hai trang này qua = Các trang trung gian khác đều sẽ qua.

| Loại Deck | Tổ hợp trang showcase khuyến nghị |
|-----------|----------------------------------|
| B2B brochure / Quảng bá sản phẩm | Trang bìa + Trang nội dung (Triết lý/Cảm xúc) |
| Ra mắt thương hiệu | Trang bìa + Trang tính năng sản phẩm |
| Báo cáo dữ liệu | Trang biểu đồ dữ liệu lớn + Trang kết luận phân tích |
| Giáo trình / Bài giảng | Trang bìa chương + Trang kiến thức cụ thể |

---

## 📐 Template出版物 (Publication Grammar) (moxt đã kiểm chứng tái sử dụng)

Phù hợp cho B2B brochure / quảng bá sản phẩm / deck báo cáo dài. Mỗi trang tái sử dụng bộ cấu trúc này = 13 trang thị giác hoàn toàn nhất quán, 0 tốn công làm lại.

### Khung sườn mỗi trang

```
┌─ masthead (Dải trên cùng + đường kẻ ngang) ────────┐
│  [logo 22-28px] · A Product Brochure                Issue · Date · URL │
├──────────────────────────────────────────┤
│                                          │
│  ── kicker (Đường kẻ ngang ngắn xanh + thẻ uppercase) │
│  CHAPTER XX · SECTION NAME                 │
│                                          │
│  H1 (Tiếng Trung Noto Serif SC 900)      │
│  Từ trọng tâm dùng màu chính của thương hiệu │
│                                          │
│  English subtitle (Lora italic, tiêu đề phụ) │
│  ─────────── Đường phân cách ──────────  │
│                                          │
│  [Nội dung cụ thể: 2 cột 60/40 / grid 2x2 / danh sách] │
│                                          │
├──────────────────────────────────────────┤
│ section name                     XX / total │
└──────────────────────────────────────────┘
```

### Quy ước Style (Dùng được ngay)

- **H1**: Tiếng Trung Noto Serif SC 900, font-size 80-140px tùy lượng thông tin, từ trọng tâm phủ màu thương hiệu chính (không nhồi nhét màu toàn văn)
- **Tiêu đề phụ tiếng Anh**: Lora italic 26-46px, từ chữ ký thương hiệu (như "AI team") in đậm + màu chính nghiêng
- **Văn bản chính**: Noto Serif SC 17-21px, line-height 1.75-1.85
- **Accent highlight**: Trong văn bản chính dùng màu thương hiệu in đậm đánh dấu từ khóa, mỗi trang không quá 3 chỗ (nhiều quá sẽ mất tác dụng điểm neo)
- **Background**: Nền kem ấm #FAFAFA + noise radial-gradient cực nhạt (`rgba(33,33,33,0.015)`) tăng cảm giác chất liệu giấy

### Nhân vật thị giác chính phải khác biệt

13 trang nếu toàn là "chữ + 1 ảnh chụp màn hình" thì quá đơn điệu. **Thay đổi linh hoạt loại nhân vật thị giác chính của mỗi trang**:

| Loại thị giác | Section phù hợp |
|---------------|----------------|
| Dàn trang bìa (Chữ lớn + masthead + pillar) | Trang chủ / Trang bìa chương |
| Portrait đơn nhân vật (Momo đơn siêu lớn,...) | Giới thiệu khái niệm/nhân vật đơn lẻ |
| Chụp chung nhiều nhân vật / Thẻ avatar xếp hàng | Team / Case study khách hàng |
| Tiến trình thẻ timeline | Hiển thị "Quan hệ lâu dài", "Phát triển" |
| Sơ đồ tri thức / Đồ thị nút kết nối | Hiển thị "Hợp tác", "Dòng chảy" |
| Thẻ so sánh Before/After + Mũi tên ở giữa | Hiển thị "Thay đổi", "Khác biệt" |
| Ảnh chụp UI sản phẩm + Khung thiết bị đường viền | Hiển thị chức năng cụ thể |
| Dấu ngoặc kép lớn big-quote (Nửa trang chữ lớn) | Trang cảm xúc / Trang vấn đề / Trang trích dẫn |
| Avatar người thật + Thẻ trích dẫn (2×2 hoặc 1×4) | Khách hàng đánh giá / Kịch bản sử dụng |
| Trang bìa sau chữ lớn + Nút oval URL | CTA / Kết bài |

---

## ⚠️ Các lỗi thường gặp (Tổng kết thực tế moxt)

### 1. Emoji không render khi xuất bằng Chromium / Playwright

Chromium mặc định không đi kèm font emoji màu, khi chạy `page.pdf()` hoặc `page.screenshot()` emoji sẽ hiển thị thành ô vuông trống.

**Giải pháp**: Dùng ký tự Unicode (`✦` `✓` `✕` `→` `·` `—`) thay thế, hoặc chuyển trực tiếp thành chữ thuần ("Email · 23" thay vì "📧 23 emails").

### 2. `export_deck_pdf.mjs` báo lỗi `Cannot find package 'playwright'`

Nguyên nhân: Việc phân tích module ESM tìm `node_modules` ngược lên từ vị trí file script. Script nằm ở `~/.claude/skills/huashu-design/scripts/`, ở đó không có package dependencies.

**Giải pháp**: Copy script vào thư mục dự án deck (ví dụ `brochure/build-pdf.mjs`), tại root dự án chạy `npm install playwright pdf-lib`, sau đó chạy `node build-pdf.mjs --slides slides --out output/deck.pdf`.

### 3. Chụp màn hình trước khi Google Fonts tải xong → Chữ tiếng Trung hiển thị font Sans-serif mặc định hệ thống

Trước khi Playwright chụp màn hình/PDF tối thiểu phải `wait-for-timeout=3500` để webfont tải xong và render. Hoặc tự host font vào `shared/fonts/` để giảm phụ thuộc mạng.

### 4. Mất cân bằng mật độ thông tin: Nhồi nhét quá nhiều vào trang nội dung

Trang moxt philosophy phiên bản đầu dùng 2×2 = 4 đoạn + 3 tín điều ở dưới = 7 khối nội dung, bị chật chội và trùng lặp. Chuyển thành 1×3 = 3 đoạn thì không gian thoáng đãng trở lại ngay.

**Giải pháp**: Mỗi trang khống chế ở "1 thông tin cốt lõi + 3-4 điểm phụ trợ + 1 nhân vật thị giác chính", vượt quá thì tách sang trang mới. **Less is more** — Khán giả xem 1 trang trong 10 giây, cho họ 1 điểm ghi nhớ sẽ dễ nhớ hơn 4 điểm.

---

## 🛑 Xác định kiến trúc trước: Đơn file hay Đa file?

**Lựa chọn này là bước đầu tiên khi làm slide, chọn sai sẽ liên tục vấp lỗi. Hãy đọc hết phần này trước khi bắt tay làm.**

### So sánh hai loại kiến trúc

| Tiêu chí | Đơn file + `deck_stage.js` | **Đa file + Bộ ghép `deck_index.html`** |
|----------|----------------------------|----------------------------------------|
| Cấu trúc code | Một file HTML, tất cả slide là `<section>` | Mỗi trang là HTML độc lập, `index.html` dùng iframe ghép lại |
| Scope CSS | ❌ Toàn cục, style một trang có thể ảnh hưởng tất cả trang | ✅ Cách ly tự nhiên, mỗi iframe là một bầu trời riêng |
| Mức độ kiểm tra | ❌ Phải có JS goTo mới chuyển sang trang nào đó | ✅ Nhấp đôi từng file trang là xem được ngay trên trình duyệt |
| Phát triển song song | ❌ Một file, nhiều agent sửa sẽ xung đột | ✅ Nhiều agent làm song song các trang khác nhau, merge không xung đột |
| Độ khó debug | ❌ Một chỗ CSS lỗi, toàn bộ deck bị vỡ layout | ✅ Một trang lỗi chỉ ảnh hưởng chính nó |
| Tương tác nhúng | ✅ Chia sẻ state giữa các trang rất đơn giản | 🟡 Giữa các iframe cần dùng postMessage |
| In PDF | ✅ Tích hợp sẵn | ✅ Bộ ghép beforeprint duyệt qua từng iframe |
| Điều hướng bàn phím | ✅ Tích hợp sẵn | ✅ Bộ ghép tích hợp sẵn |

### Chọn cái nào? (Cây quyết định)

```
│ Hỏi: Dự kiến deck có bao nhiêu trang?
├── ≤10 trang, cần animation in-deck hoặc tương tác giữa các trang, pitch deck → Đơn file
└── ≥10 trang, bài giảng học thuật, giáo trình, deck dài, nhiều agent làm song song → Đa file (Khuyến nghị)
```

**Mặc định đi theo con đường Đa file**. Nó không phải "phương án phụ", mà là **con đường chính cho deck dài và làm việc nhóm**. Lý do: Mỗi ưu điểm của kiến trúc đơn file (điều hướng bàn phím, in ấn, scale) đa file đều có, trong khi tính cách ly scope và khả năng kiểm tra của đa file thì đơn file không thể bù đắp được.

### Tại sao quy tắc này lại cứng như vậy? (Ghi chép sự cố thực tế)

Kiến trúc đơn file đã từng liên tiếp mắc 4 lỗi trong dự án bài giảng Tâm lý học AI:

1. **CSS specificity đè nén**: `.emotion-slide { display: grid }` (specificity 10) đè bẹp `deck-stage > section { display: none }` (specificity 2), dẫn đến tất cả các trang bị render đè lên nhau cùng lúc.
2. **Quy tắc Shadow DOM slot bị CSS bên ngoài áp chế**: `::slotted(section) { display: none }` không đè được outer rule, làm sections không chịu ẩn.
3. **Điều hướng localStorage + hash bị race condition**: F5 xong không nhảy đến vị trí hash, mà dừng ở vị trí cũ ghi trong localStorage.
4. **Chi phí kiểm tra cao**: Bắt buộc phải `page.evaluate(d => d.goTo(n))` mới chụp được một trang, chậm gấp đôi so với truy cập trực tiếp `goto(file://.../slides/05-X.html)`, lại hay báo lỗi.

Tất cả nguyên nhân gốc rễ là **namespace toàn cục duy nhất** — kiến trúc đa file loại bỏ hoàn toàn những vấn đề này từ cấp độ vật lý.

---

## Path A (Mặc định): Kiến trúc đa file

### Cấu trúc thư mục

```
MyDeck/
├── index.html              # Copy từ assets/deck_index.html, sửa MANIFEST
├── shared/
│   ├── tokens.css          # Design token dùng chung (bảng màu/font-size/chrome thường dùng)
│   └── fonts.html          # <link> nhúng Google Fonts (mỗi trang include)
└── slides/
    ├── 01-cover.html       # Mỗi file là một trang HTML 1920×1080 hoàn chỉnh
    ├── 02-agenda.html
    ├── 03-problem.html
    └── ...
```

### Khung mẫu cho mỗi slide

```html
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>P05 · Chapter Title</title>
<link href="https://fonts.googleapis.com/css2?family=..." rel="stylesheet">
<link rel="stylesheet" href="../shared/tokens.css">
<style>
  /* Style riêng của trang này. Dùng bất kỳ tên class nào cũng không làm bẩn trang khác. */
  body { padding: 120px; }
  .my-thing { ... }
</style>
</head>
<body>
  <!-- Nội dung 1920×1080 (được khóa bởi width/height của body trong tokens.css) -->
  <div class="page-header">...</div>
  <div>...</div>
  <div class="page-footer">...</div>
</body>
</html>
```

**Ràng buộc quan trọng**:
- `<body>` chính là canvas, dàn trang trực tiếp trên đó. Không bọc thêm `<section>` hay wrapper khác.
- `width: 1920px; height: 1080px` được khóa bởi quy tắc `body` trong `shared/tokens.css`.
- Nhúng `shared/tokens.css` để dùng chung design token (bảng màu, font-size, page-header/footer,...).
- Thẻ `<link>` font chữ mỗi trang tự viết riêng (import fonts không đắt, và đảm bảo mỗi trang mở độc lập được).

### Bộ ghép: `deck_index.html`

**Copy trực tiếp từ `assets/deck_index.html`**. Bạn chỉ cần sửa một chỗ — mảng `window.DECK_MANIFEST`, liệt kê tên file tất cả slide và label dễ đọc theo thứ tự:

```js
window.DECK_MANIFEST = [
  { file: "slides/01-cover.html",    label: "Bìa" },
  { file: "slides/02-agenda.html",   label: "Mục lục" },
  { file: "slides/03-problem.html",  label: "Đặt vấn đề" },
  // ...
];
```

Bộ ghép đã tích hợp sẵn: Điều hướng bàn phím (←/→/Home/End/phím số/phím P để in), scale + letterbox, bộ đếm góc dưới bên phải, ghi nhớ localStorage, nhảy trang bằng hash, chế độ in (duyệt iframe xuất PDF từng trang).

#### Hai chế độ tổng quan (Tự điều chỉnh + Chống vấp lỗi, viết lại tháng 06-2026)

Mở deck mặc định vào chế độ **tổng quan (overview)**, khi người dùng không chỉ định sẽ chọn ngẫu nhiên theo giây: **Lưới grid 60% / Gallery vô tận 40%** (có thể cố định bằng URL `?ov=grid|gallery` hoặc `window.DECK_OVERVIEW='grid'|'gallery'`).

- **Lưới grid (Chủ lực mặc định)**: Dùng **iframe render trang con thực tế** (sắc nét, WYSIWYG, không cần ảnh thumbnail). **Tự điều chỉnh**: Màn hình chứa đủ → nghiêng chéo căn giữa lấp đầy; số trang quá nhiều không chứa đủ → thẻ giữ kích thước thoải mái, **cuộn dọc** (tuyệt đối không ép hàng chục trang vào một màn hình thu nhỏ như tem thư).
- **Gallery vô tận**: Tất cả các trang **lát gạch vô tận không vết nối + trôi chậm + co giãn nhẹ nhàng**, một tile chứa toàn bộ các trang (xếp ngẫu nhiên, xem hết tất cả các trang mới lặp lại). Số lượng tile nhiều, **bắt buộc dùng thumbnail `<img>`** để gánh hiệu năng (xem ở dưới), khi không có thumb sẽ fallback về iframe.

🛑 **Ba quy tắc cứng từ thực tế (Phải đọc trước khi sửa file này, nếu không sẽ lặp lại sai lầm)**:
1. **Tường tổng quan tuyệt đối không dùng `transform-style: preserve-3d` để làm tường thẻ**. Trong không gian 3D của preserve-3d, hit-test của trình duyệt đối với "thẻ lùi về sau" (hàng trên cùng) không tin cậy → Hàng trên cùng không nhấp được, hàng giữa lúc được lúc không. **Cách giải quyết đúng**: Toàn bộ tường làm thành **một mặt phẳng đơn lẻ bị nghiêng 3D** (không mở preserve-3d), tất cả các thẻ nằm trên cùng mặt phẳng, click chiếu ngược về một mặt phẳng → tin cậy. Hover dùng 2D `scale` không dùng `translateZ`.
2. **Số trang bất kỳ đều phải tự điều chỉnh**: Số cột cố định + nghiêng mạnh tường, số trang nhiều lên sẽ bị tràn/méo góc nhìn. Bắt buộc tính số cột theo số trang + viewport, số hàng nhiều thì giảm độ nghiêng, một màn hình không đủ thì cuộn.
3. **Độ phân giải thumbnail đừng quá thấp**: Thumbnail gallery < 1000px, khi hover phóng to sẽ bị mờ. Mặc định 1600px.

**Tạo thumbnail cho gallery**: Dùng `scripts/gen_deck_thumbs.mjs` (playwright chụp từng trang + sharp giảm sample rate):
```bash
npm install playwright sharp
node gen_deck_thumbs.mjs --slides slides --out thumbs --width 1600
```
Sau đó thêm `thumb: "thumbs/<tên_tương_ứng>.jpg"` vào mỗi item của MANIFEST. Chế độ lưới bỏ qua thumb (luôn dùng iframe), chỉ chế độ gallery mới dùng thumb.

### Kiểm tra đơn trang (Ưu điểm sát thủ của kiến trúc đa file)

Mỗi slide là một HTML độc lập. **Làm xong trang nào nhấp đôi mở trang đó trên trình duyệt để xem**:

```bash
open slides/05-personas.html
```

Chụp màn hình bằng Playwright cũng truy cập trực tiếp `goto(file://.../slides/05-personas.html)`, không cần JS chuyển trang, cũng không bị CSS của trang khác làm phiền. Việc này giúp chi phí cho quy trình "sửa tới đâu kiểm tra tới đó" tiến về 0.

### Phát triển song song

Chia nhiệm vụ mỗi slide cho các agent khác nhau chạy cùng lúc — các file HTML độc lập với nhau, khi merge không có xung đột. Deck dài dùng cách làm song song này có thể nén thời gian làm xuống 1/N.

### `shared/tokens.css` nên đặt những gì

Chỉ đặt những thứ **thực sự dùng chung giữa các trang**:

- CSS variables (bảng màu, thang font-size, thang khoảng cách)
- Khóa canvas như `body { width: 1920px; height: 1080px; }`
- Chrome trang giống hệt nhau trên mỗi trang như `.page-header` / `.page-footer`

**Đừng** nhét class layout của đơn trang vào — việc đó sẽ thoái hóa về lại vấn đề ô nhiễm toàn cục của kiến trúc đơn file.

---

## Path B (Deck nhỏ): Đơn file + `deck_stage.js`

Phù hợp cho ≤10 trang, cần chia sẻ state giữa các trang (ví dụ một panel React tweaks điều khiển tất cả các trang), hoặc kịch bản làm pitch deck demo yêu cầu cực kỳ gọn nhẹ.

### Cách dùng cơ bản

1. Đọc nội dung từ `assets/deck_stage.js`, nhúng vào `<script>` của HTML (hoặc `<script src="deck_stage.js">`)
2. Trong body dùng `<deck-stage>` bọc slide lại
3. 🛑 **Thẻ script bắt buộc phải đặt sau `</deck-stage>`** (xem quy tắc cứng bên dưới)

```html
<body>

  <deck-stage>
    <section>
      <h1>Slide 1</h1>
    </section>
    <section>
      <h1>Slide 2</h1>
    </section>
  </deck-stage>

  <!-- ✅ Đúng: script đặt sau deck-stage -->
  <script src="deck_stage.js"></script>

</body>
```

### 🛑 Quy tắc cứng về vị trí Script (Thực tế vấp phải ngày 20-04-2026)

**Không được đặt `<script src="deck_stage.js">` trong `<head>`.** Dù nó có thể định nghĩa `customElements` trong `<head>`, parser khi phân tích đến thẻ mở `<deck-stage>` sẽ kích hoạt `connectedCallback` ngay lập tức — lúc này các `<section>` con chưa được parse, `_collectSlides()` nhận được mảng rỗng, counter hiển thị `1 / 0`, tất cả các trang bị render đè lên nhau cùng lúc.

**Ba cách viết chuẩn tuân thủ** (chọn 1 trong 3):

```html
<!-- ✅ Khuyến nghị nhất: script đặt sau </deck-stage> -->
</deck-stage>
<script src="deck_stage.js"></script>

<!-- ✅ Cũng được: script ở head nhưng thêm defer -->
<head><script src="deck_stage.js" defer></script></head>

<!-- ✅ Cũng được: script module tự động defer -->
<head><script src="deck_stage.js" type="module"></script></head>
```

Bản thân `deck_stage.js` đã tích hợp cơ chế phòng thủ trì hoãn thu thập `DOMContentLoaded`, dù script đặt ở head cũng không bị hỏng hoàn toàn — nhưng `defer` hoặc đặt ở cuối body vẫn là cách làm sạch sẽ hơn, tránh phụ thuộc vào nhánh phòng thủ.

### ⚠️ Bẫy CSS của kiến trúc đơn file (Bắt buộc đọc)

Lỗi phổ biến nhất của kiến trúc đơn file — **Thuộc tính `display` bị style của đơn trang lấy mất**.

Tư thế viết lỗi phổ biến 1 (Viết trực tiếp display: flex vào section):

```css
/* ❌ Specificity CSS bên ngoài là 2, đè mất ::slotted(section){display:none} của shadow DOM (cũng là 2) */
deck-stage > section {
  display: flex;            /* Tất cả các trang sẽ bị render đè lên nhau! */
  flex-direction: column;
  padding: 80px;
  ...
}
```

Tư thế viết lỗi phổ biến 2 (section có class có specificity cao hơn):

```css
.emotion-slide { display: grid; }   /* Specificity: 10, tệ hơn nữa */
```

Cả hai cách viết trên đều làm **tất cả slide bị render đè lên nhau cùng lúc** — counter có thể hiển thị `1 / 10` giả vờ bình thường, nhưng về thị giác thì trang thứ nhất đè lên trang thứ hai đè lên trang thứ ba.

### ✅ Starter CSS (Copy làm ngay, không vấp lỗi)

**Bản thân section** chỉ quản lý "hiển thị/ẩn"; **layout (flex/grid,...) viết vào `.active`**:

```css
/* section chỉ định nghĩa style chung không chứa display */
deck-stage > section {
  background: var(--paper);
  padding: 80px 120px;
  overflow: hidden;
  position: relative;
  /* ⚠️ Đừng viết display ở đây! */
}

/* Khóa chết "không active thì ẩn" — bảo hiểm đôi về specificity + weight */
deck-stage > section:not(.active) {
  display: none !important;
}

/* Trang active mới viết display + layout cần thiết */
deck-stage > section.active {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

/* Chế độ in: Tất cả các trang đều phải hiển thị, đè lên :not(.active) */
@media print {
  deck-stage > section { display: flex !important; }
  deck-stage > section:not(.active) { display: flex !important; }
}
```

Phương án thay thế: **Viết flex/grid của trang vào `<div>` wrapper bên trong**, bản thân section vĩnh viễn chỉ là bộ chuyển đổi `display: block/none`. Đây là cách làm sạch sẽ nhất:

```html
<deck-stage>
  <section>
    <div class="slide-content flex-layout">...</div>
  </section>
</deck-stage>
```

### Kích thước tùy chỉnh

```html
<deck-stage width="1080" height="1920">
  <!-- Dọc 9:16 -->
</deck-stage>
```

---

## Slide Labels

Cả deck_stage và deck_index đều đánh label cho từng trang (hiển thị trên bộ đếm). Đặt label **có ý nghĩa hơn** cho chúng:

**Đa file**: Trong `MANIFEST` viết `{ file, label: "04 Đặt vấn đề" }`
**Đơn file**: Thêm vào section `<section data-screen-label="04 Problem Statement">`

**Quan trọng: Đánh số Slide bắt đầu từ 1, không bắt đầu từ 0**.

Khi khách hàng nói "slide 5", họ đang ám chỉ trang thứ 5, không bao giờ là vị trí mảng `[4]`. Con người không nói kiểu 0-indexed.

---

## Speaker Notes

**Mặc định không thêm**, chỉ thêm khi khách hàng yêu cầu rõ ràng.

Khi thêm speaker notes, bạn có thể giảm chữ trên slide xuống mức tối thiểu để tập trung vào hình ảnh ấn tượng — notes sẽ gánh toàn bộ script phát biểu.

### Định dạng

**Đa file**: Trong `<head>` của `index.html` viết:

```html
<script type="application/json" id="speaker-notes">
[
  "Script trang 1...",
  "Script trang 2...",
  "..."
]
</script>
```

**Đơn file**: Vị trí tương tự như trên.

### Các điểm cần lưu ý khi viết Notes

- **Hoàn chỉnh**: Không phải dàn ý, mà là lời sẽ nói thực tế
- **Kiểu trò chuyện**: Giống như nói chuyện bình thường, không phải văn viết
- **Tương ứng**: Phần tử thứ N trong mảng tương ứng với slide thứ N
- **Độ dài**: 200-400 từ là tốt nhất
- **Mạch cảm xúc**: Đánh dấu trọng âm, ngắt nghỉ, điểm nhấn

---

## Các Pattern thiết kế Slide

### 1. Xây dựng một hệ thống (Bắt buộc làm)

Sau khi khám phá xong design context, **nói trước hệ thống bạn định dùng bằng lời**:

```markdown
Hệ thống Deck:
- Màu nền: Tối đa 2 loại (90% trắng + 10% section divider màu tối)
- Font chữ: Display dùng Instrument Serif, body dùng Geist Sans
- Nhịp điệu: Section divider dùng full-bleed màu sắc + chữ trắng, slide thường nền trắng
- Hình ảnh: Hero slide dùng ảnh full-bleed, data slide dùng biểu đồ

Tôi sẽ làm theo hệ thống này, có vấn đề gì hãy báo cho tôi.
```

Khách hàng xác nhận rồi mới làm tiếp.

### 2. Các Slide Layout thường dùng

- **Title slide**: Nền màu thuần + Tiêu đề lớn + Tiêu đề phụ + Tác giả/Ngày tháng
- **Section divider**: Nền màu + Số chương + Tiêu đề chương
- **Content slide**: Nền trắng + Tiêu đề + 1-3 bullet points
- **Data slide**: Tiêu đề + Biểu đồ/Con số lớn + Chú thích ngắn
- **Image slide**: Ảnh full-bleed + Caption nhỏ ở dưới
- **Quote slide**: Chừa không gian trống + Quote lớn + Nguồn trích dẫn (attribution)
- **Two-column**: So sánh hai bên (vs / before-after / problem-solution)

Trong một deck tối đa dùng 4-5 loại layout.

### 3. Scale (Nhấn mạnh lần nữa)

- Chữ nội dung nhỏ nhất **24px**, lý tưởng 28-36px
- Tiêu đề **60-120px**
- Chữ Hero **180-240px**
- Slide là cho người đứng xa 10 mét xem, chữ phải đủ lớn

### 4. Nhịp điệu thị giác

Deck cần có **intentional variety (Sự đa dạng có mục đích)**:

- Nhịp điệu màu sắc: Phần lớn nền trắng + Thỉnh thoảng section divider có màu + Thỉnh thoảng đoạn màu tối (dark)
- Nhịp điệu mật độ: Mấy trang nhiều chữ + Mấy trang nhiều hình + Mấy trang quote chừa khoảng trống
- Nhịp điệu font-size: Tiêu đề bình thường + Thỉnh thoảng chữ hero khổng lồ

**Đừng làm trang nào cũng giống hệt nhau** — Đó là template PPT, không phải thiết kế.

### 5. Khoảng thở không gian (Trang mật độ dữ liệu cao bắt buộc đọc)

**Lỗi người mới dễ mắc nhất**: Nhồi nhét tất cả thông tin có thể vào một trang.

Mật độ thông tin ≠ Truyền tải thông tin hiệu quả. Slide dạng học thuật/thuyết trình càng phải kiềm chế:

- Trang danh sách/ma trận: Đừng vẽ N phần tử thành cùng một kích thước. Dùng **phân cấp chính phụ** — 5 phần tử cần nói hôm nay phóng to làm nhân vật chính, 16 phần tử còn lại thu nhỏ làm hint nền.
- Trang con số lớn: Bản thân con số là nhân vật thị giác chính. Caption xung quanh không quá 3 dòng, nếu không mắt khán giả sẽ nhảy qua nhảy lại.
- Trang trích dẫn: Giữa lời dẫn và attribution phải có khoảng trống phân cách, đừng dính liền vào nhau.

Đối chiếu hai câu tự kiểm tra "Dữ liệu có phải là nhân vật chính không", "Chữ có bị ép vào nhau không", sửa cho đến khi khoảng trống làm bạn cảm thấy hơi bất an mới thôi.

---

## In ra PDF

**Đa file**: `deck_index.html` đã xử lý sự kiện `beforeprint`, xuất PDF từng trang.

**Đơn file**: `deck_stage.js` cũng xử lý tương tự.

Style in đã được viết sẵn, không cần viết thêm CSS `@media print`.

---

## Xuất ra PPTX / PDF (Script tự phục vụ)

Ưu tiên HTML là công dân hạng nhất. Nhưng khách hàng thường cần bàn giao PPTX/PDF. Cung cấp hai script dùng chung, **bất kỳ deck đa file nào cũng dùng được**, nằm trong thư mục `scripts/`:

### `export_deck_pdf.mjs` — Xuất PDF Vector (Kiến trúc đa file)

```bash
node scripts/export_deck_pdf.mjs --slides <slides-dir> --out deck.pdf
```

**Đặc điểm**:
- Văn bản **giữ nguyên vector** (có thể copy, có thể tìm kiếm)
- Thị giác bảo toàn 100% (Playwright nhúng Chromium render rồi in)
- **Không cần sửa bất kỳ chữ nào trong HTML**
- Mỗi slide dùng `page.pdf()` độc lập, sau đó dùng `pdf-lib` gộp lại

**Package phụ thuộc**: `npm install playwright pdf-lib`

**Hạn chế**: PDF không thể chỉnh sửa văn bản được nữa — muốn sửa phải quay lại sửa HTML.

### `export_deck_stage_pdf.mjs` — Dành riêng cho kiến trúc deck-stage đơn file ⚠️

**Khi nào dùng**: Deck là một file HTML đơn + web component `<deck-stage>` bọc N thẻ `<section>` (tức kiến trúc Path B). Lúc này cách làm "mỗi HTML chạy `page.pdf()` một lần" của `export_deck_pdf.mjs` không chạy được, phải dùng script dành riêng này.

```bash
node scripts/export_deck_stage_pdf.mjs --html deck.html --out deck.pdf
```

**Tại sao không thể tái sử dụng export_deck_pdf.mjs** (Ghi chép thực tế vấp phải ngày 20-04-2026):

1. **Shadow DOM đè bẹp `!important`**: Trong shadow CSS của deck-stage có `::slotted(section) { display: none }` (chỉ trang active mới `display: block`). Dù ở light DOM có dùng `@media print { deck-stage > section { display: block !important } }` cũng không đè được — Sau khi `page.pdf()` kích hoạt print media, Chromium render cuối cùng chỉ có đúng một trang active, kết quả **toàn bộ PDF chỉ có 1 trang** (lặp lại slide active hiện tại).

2. **Vòng lặp goto từng trang vẫn chỉ ra 1 trang**: Cách giải quyết trực giác "navigate tới từng `#slide-N` một lần rồi `page.pdf({pageRanges:'1'})`" cũng thất bại — vì print CSS ở ngoài shadow DOM cũng có quy tắc `deck-stage > section { display: block }` bị override, render cuối cùng luôn luôn là phần tử đầu tiên trong danh sách section (không phải trang bạn navigate tới). Kết quả vòng lặp 17 lần nhận được 17 trang bìa P01.

3. **Phần tử con absolute bị nhảy sang trang sau**: Dù có làm cho tất cả section render ra được, nếu bản thân section là `position: static`, các `cover-footer`/`slide-footer` định vị absolute sẽ định vị tương đối theo initial containing block — khi section bị print ép thành chiều cao 1080px, absolute footer có thể bị đẩy sang trang sau (biểu hiện là PDF nhiều hơn số section 1 trang, trang thừa ra đó chỉ chứa footer mồ côi).

**Chiến lược sửa lỗi** (Script đã triển khai):

```js
// Sau khi mở HTML, dùng page.evaluate bốc section ra khỏi deck-stage slot,
// gắn trực tiếp vào một div thường dưới body, và inline style đảm bảo position:relative + kích thước cố định
await page.evaluate(() => {
  const stage = document.querySelector('deck-stage');
  const sections = Array.from(stage.querySelectorAll(':scope > section'));
  document.head.appendChild(Object.assign(document.createElement('style'), {
    textContent: `
      @page { size: 1920px 1080px; margin: 0; }
      html, body { margin: 0 !important; padding: 0 !important; }
      deck-stage { display: none !important; }
    `,
  }));
  const container = document.createElement('div');
  sections.forEach(s => {
    s.style.cssText = 'width:1920px!important;height:1080px!important;display:block!important;position:relative!important;overflow:hidden!important;page-break-after:always!important;break-after:page!important;background:#F7F4EF;margin:0!important;padding:0!important;';
    container.appendChild(s);
  });
  // Trang cuối cấm ngắt trang, tránh trang trắng ở đuôi
  sections[sections.length - 1].style.pageBreakAfter = 'auto';
  sections[sections.length - 1].style.breakAfter = 'auto';
  document.body.appendChild(container);
});

await page.pdf({ width: '1920px', height: '1080px', printBackground: true, preferCSSPageSize: true });
```

**Tại sao cách này chạy được**:
- Rút section từ shadow DOM slot ra div thường của light DOM — Nhảy hoàn toàn qua quy tắc `::slotted(section) { display: none }`
- Inline `position: relative` làm cho các phần tử con absolute định vị tương đối theo section, không bị tràn ra ngoài
- `page-break-after: always` giúp trình duyệt khi print tự động ngắt thành từng trang độc lập cho mỗi section
- `:last-child` không ngắt trang tránh tạo ra trang trắng ở đuôi

**Lưu ý khi kiểm tra bằng `mdls -name kMDItemNumberOfPages`**: Spotlight metadata của macOS có cache, sau khi ghi lại PDF phải chạy `mdimport file.pdf` để ép refresh, nếu không sẽ hiển thị số trang cũ. Dùng `pdfinfo` hoặc `pdftoppm` đếm số file mới là số thực.

---

### `export_deck_pptx.mjs` — Xuất PPTX có thể chỉnh sửa

```bash
# Chế độ duy nhất: Thẻ văn bản có thể chỉnh sửa nguyên bản (Font chữ sẽ fallback về font hệ thống)
node scripts/export_deck_pptx.mjs --slides <dir> --out deck.pptx
```

Nguyên lý hoạt động: `html2pptx` đọc computedStyle từng phần tử để dịch DOM thành đối tượng PowerPoint (text frame / shape / picture). Chữ sẽ biến thành khung văn bản thật, trong PPT nhấp đôi là có thể chỉnh sửa.

**Ràng buộc cứng** (HTML bắt buộc phải đáp ứng, nếu không trang đó sẽ skip, xem giải thích chi tiết tại `references/editable-pptx.md`):
- Tất cả văn bản phải nằm trong `<p>`/`<h1>`-`<h6>`/`<ul>`/`<ol>` (cấm div chứa văn bản trần)
- Thẻ `<p>`/`<h*>` bản thân không được có background/border/shadow (đặt ở div ngoài)
- Không dùng `::before`/`::after` để chèn chữ trang trí (pseudo-element không bốc ra được)
- Phần tử inline (span/em/strong) không được có margin
- Không dùng CSS gradient (không thể render)
- Div không dùng `background-image` (dùng `<img>`)

Script đã tích hợp sẵn **Bộ tiền xử lý tự động** — Tự động bọc văn bản trần trong div lá thành `<p>` (giữ nguyên class). Việc này giải quyết lỗi vi phạm phổ biến nhất (văn bản trần). Nhưng các vi phạm khác (p có border, span có margin,...) vẫn cần nguồn HTML tuân thủ chuẩn.

**Lưu ý về Font fallback**:
- Playwright dùng webfont để đo kích thước text-box; PowerPoint/Keynote dùng font máy cục bộ để render
- Khi hai bên khác nhau sẽ bị **tràn chữ hoặc lệch vị trí** — Mỗi trang đều phải soi bằng mắt thường
- Khuyến nghị máy mục tiêu nên cài sẵn font chữ dùng trong HTML, hoặc fallback về `system-ui`

**Kịch bản ưu tiên thị giác đừng đi theo con đường này** → Chuyển sang dùng `export_deck_pdf.mjs` xuất PDF. PDF bảo toàn 100% thị giác, dạng vector, đa nền tảng, tìm kiếm được chữ — là đích đến thực sự cho deck ưu tiên thị giác, không phải sự "nửa vời không chỉnh sửa được".

### Làm cho HTML thân thiện với việc xuất ngay từ đầu

Đối với deck cần ổn định hiệu năng nhất: **Ngay từ khi viết HTML hãy tuân thủ 4 quy tắc cứng của editable**. Như vậy `export_deck_pptx.mjs` có thể pass trực tiếp toàn bộ. Chi phí thêm không lớn:

```html
<!-- ❌ Không tốt -->
<div class="title">Phát hiện quan trọng</div>

<!-- ✅ Tốt (p bọc lại, class kế thừa) -->
<p class="title">Phát hiện quan trọng</p>

<!-- ❌ Không tốt (border nằm trên p) -->
<p class="stat" style="border-left: 3px solid red;">41%</p>

<!-- ✅ Tốt (border nằm ở div bên ngoài) -->
<div class="stat-wrap" style="border-left: 3px solid red;">
  <p class="stat">41%</p>
</div>
```

### Khi nào chọn cái nào

| Kịch bản | Khuyến nghị |
|----------|-------------|
| Gửi cho ban tổ chức/Lưu trữ hồ sơ | **PDF** (Phổ biến, độ bảo toàn cao, tìm kiếm chữ được) |
| Gửi cho cộng sự để họ tinh chỉnh chữ | **PPTX editable** (Chấp nhận font fallback) |
| Thuyết trình tại chỗ, không sửa nội dung | **PDF** (Vector bảo toàn, đa nền tảng) |
| HTML là phương tiện hiển thị ưu tiên | Thuyết trình trực tiếp trên trình duyệt, xuất chỉ là bản backup |

## Con đường chuyên sâu xuất PPTX chỉnh sửa được (Chỉ dự án dài hạn)

Nếu deck của bạn cần duy trì lâu dài, sửa đổi liên tục, làm việc nhóm — khuyến nghị **ngay từ đầu hãy viết HTML theo quy tắc html2pptx**, như vậy `export_deck_pptx.mjs` có thể pass 100%. Chi tiết xem `references/editable-pptx.md` (4 quy tắc cứng + HTML template + Tra cứu lỗi thường gặp + Quy trình fallback khi đã có bản thiết kế thị giác).

---

## Các câu hỏi thường gặp

**Đa file: Trang trong iframe không mở được / Màn hình trắng**
→ Kiểm tra đường dẫn `file` trong `MANIFEST` có đúng tương đối so với `index.html` không. Dùng DevTools của trình duyệt xem src của iframe có truy cập trực tiếp được không.

**Đa file: Style một trang bị xung đột với trang khác**
→ Không thể nào (cách ly iframe). Nếu cảm thấy xung đột, đó là do cache — Cmd+Shift+R để hard refresh.

**Đơn file: Nhiều slide bị render đè lên nhau cùng lúc**
→ Vấn đề CSS specificity. Xem phần "Bẫy CSS của kiến trúc đơn file" ở trên.

**Đơn file: Co giãn nhìn không đúng**
→ Kiểm tra xem tất cả slide có nằm trực tiếp dưới `<deck-stage>` làm thẻ `<section>` không. Ở giữa không được bọc `<div>`.

**Đơn file: Muốn nhảy đến slide cụ thể**
→ Thêm hash vào URL: `index.html#slide-5` để nhảy đến trang thứ 5.

**Cả hai kiến trúc đều gặp: Chữ ở các màn hình khác nhau vị trí không nhất quán**
→ Dùng kích thước cố định (1920×1080) và đơn vị `px`, không dùng `vw`/`vh` hoặc `%`. Co giãn xử lý thống nhất.

---

## Checklist kiểm tra (Làm xong deck bắt buộc phải qua)

1. [ ] Mở trực tiếp `index.html` (hoặc HTML chính) trên trình duyệt, kiểm tra trang chủ không bị vỡ ảnh, font chữ đã tải
2. [ ] Bấm phím → chuyển qua từng trang, không có trang trắng, không bị lệch bố cục
3. [ ] Bấm phím P xem trước bản in, mỗi trang vừa đúng một trang A4 (hoặc 1920×1080) và không bị cắt xén
4. [ ] Chọn ngẫu nhiên 3 trang bấm Cmd+Shift+R hard refresh, bộ nhớ localStorage hoạt động bình thường
5. [ ] Playwright chụp màn hình hàng loạt (Kiến trúc đa file: Duyệt `slides/*.html`; Kiến trúc đơn file: Dùng goTo chuyển trang), dùng mắt thường kiểm tra lại một lượt
6. [ ] Tìm kiếm xem còn vướng `TODO` / `placeholder` không, xác nhận đã dọn dẹp sạch sẻ
