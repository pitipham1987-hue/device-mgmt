# Typography: Hệ thống Suy luận Trình bày Chữ

> **Đây không phải là danh mục font chữ, mà là quy tắc suy luận về việc phối hợp và trình bày chữ.** `design-styles.md` đã đưa ra tên font chữ riêng cho 60 loại phong cách; tài liệu này trả lời "Tại sao phối hợp như vậy", "Nhận được nội dung bất kỳ làm sao suy luận ra cỡ chữ/chiều dài dòng/độ đậm chữ". Mục tiêu: Cùng một nhãn phong cách, áp dụng lên các nội dung khác nhau, có thể suy luận ra các kết quả trình bày chữ khác nhau, chứ không phải lần nào cũng chép cùng một bộ cỡ chữ.
>
> Kỷ luật tiền đề không đổi: Có design context hãy lift font chữ của chính người dùng trước (Xem `design-context.md`), tất cả nội dung tài liệu này chỉ kích hoạt khi "Người dùng không có quy chuẩn font chữ".

## 0. Thứ tự Quyết định Trình bày Chữ

Sau khi nhận được nội dung hãy suy luận theo thứ tự này, mỗi bước đều do bước trước quyết định, không cho phép nhảy sang "Trực tiếp chọn một font chữ đẹp mắt":

1. **Loại nội dung** → Văn bản dài đọc / Dữ liệu dày đặc / Chữ lớn marketing / Giao diện UI, quyết định tỷ lệ âm階 và cỡ chữ chính
2. **Cấu thành ngôn ngữ** → Tiếng Trung thuần / Trộn Trung Tây / Tiếng Tây thuần, quyết định cách viết chuỗi fallback và cơ chuẩn chiều cao dòng
3. **Nhiệt độ phong cách** (Căn chỉnh với 3 nấc Trầm tĩnh/Trung tính/Táo bạo của `design-styles.md`) → Quyết định nguồn tương phản của việc phối hợp font chữ
4. **Sau cùng mới là tên font chữ** → Chọn từ bảng phối hợp chương 3 bên dưới, hoặc lấy từ điều khoản tương ứng của thư viện phong cách

Tại sao: Cách làm chọn tên font chữ trước sẽ khiến "Nội dung là gì" có tác động bằng 0 đối với việc trình bày chữ, đây chính là căn bệnh gốc rễ của việc ngàn người như một.

## 1. Âm阶 Cỡ chữ (modular scale)

Cỡ chữ không phải suy đoán lung tung, mà suy luận từng cấp từ cỡ chữ chính nhân với một tỷ lệ cố định. Tỷ lệ quyết định "Tính kịch" của trang web:

| Tỷ lệ | Tên | Tính cách | Áp dụng |
|------|------|------|------|
| 1.2 | Nấc 3 nhỏ | Êm đềm, Cấp độ nhiều mà không ồn ào | dashboard, trang tài liệu, UI dữ liệu dày đặc |
| 1.25 | Nấc 3 lớn | Thông dụng, An toàn | Đa số trang web, landing page sản phẩm |
| 1.333 | Nấc 4 thuần | Tiêu đề nhảy ra rõ ràng | Văn bản dài editorial, trang marketing, báo cáo |
| 1.5 | Nấc 5 thuần | Tính kịch, Cấp độ cực ít | Báo chữ lớn, slides, hero một màn hình một câu |

**Quy tắc suy luận**: Chữ chính ấn định 16-18px (Chữ chính Tiếng Trung/Chữ Hán khuyến nghị 17-18px, nét chữ CJK dày đặc, cùng cỡ chữ hiển thị chật chội hơn Tiếng Tây), sau đó nhân theo tỷ lệ suy lên tiêu đề, suy xuống caption. Cấp độ vượt quá 5 nấc là mất kiểm soát, hãy cắt bỏ.

| Nấc | Giá trị tham khảo dưới tỷ lệ 1.25 | Mục đích |
|------|--------------------|------|
| caption | 12-13px | Chú thích hình, Thông tin meta, Chữ nhỏ kiểu EXIF |
| small | 14px | Giải thích hỗ trợ, Bảng biểu |
| body | 16-18px | Chữ chính, Cơ chuẩn của tất cả |
| h3 | ≈1.25x | Tiêu đề mục nhỏ |
| h2 | ≈1.56x | Tiêu đề chương |
| h1 | ≈1.95x | Tiêu đề trang |
| display | 3x-8x, Thoát khỏi âm阶 phát huy tự do | Chữ khổng lồ hero, quyết định bởi bố cục chứ không phải âm阶 |

**Cách viết cỡ chữ dạng luồng** (Nấc display bắt buộc dùng, tránh màn hình lớn cứng nhắc màn hình nhỏ tràn ra):

```css
/* clamp(giá trị nhỏ nhất, giá trị ưu tiên, giá trị lớn nhất): giá trị ưu tiên = rem cơ sở + hệ số viewport */
h1 { font-size: clamp(2rem, 1.2rem + 3.5vw, 4.5rem); }
.display { font-size: clamp(3rem, 1rem + 9vw, 9rem); }
/* Chữ chính đừng clamp ra biến động lớn, khoảng hẹp 16→18 là được */
body { font-size: clamp(1rem, 0.95rem + 0.3vw, 1.125rem); }
```

Tại sao display thoát khỏi âm阶: Chữ khổng lồ hero là phần tử bố cục chứ không phải cấp độ văn bản, kích thước của nó do "chiếm mấy phần viewport" quyết định, dùng vw suy luận hợp lý hơn dùng âm阶 suy luận.

## 2. Chiều dài dòng và Chiều cao dòng

### Chiều dài dòng (Ảnh hưởng đến khả năng đọc hơn cả việc chọn font chữ)

| Ngôn ngữ | Vùng thoải mái | Thực thi CSS |
|------|--------|----------|
| Chữ chính Tiếng Tây | 45-75 ký tự, tốt nhất 66 | `max-width: 65ch` |
| Chữ chính Tiếng Trung/Việt | Một dòng 22-38 chữ, tốt nhất 28-32 chữ | `max-width: 36em` (em co giãn theo cỡ chữ) |
| Chữ nhỏ chú thích/Thanh bên | Ngắn hơn, 15-20 chữ | Container hẹp tự nhiên giới hạn |

Tại sao Tiếng Trung ngắn hơn: Chữ Hán là chữ khối vuông dày đặc không có khoảng trắng, dung lượng thông tin mang theo dưới cùng chiều rộng rõ ràng cao hơn Tiếng Tây, cùng số lần nhảy mắt Tiếng Trung đọc vào nhiều nội dung hơn, dòng quá dài khi xuống dòng sẽ không tìm thấy đầu dòng tiếp theo.

### Chiều cao dòng liên động theo chiều dài dòng

Chiều cao dòng không phải hằng số, nó là hàm số của chiều dài dòng. Dòng càng dài, khoảng cách mắt quay lại dòng càng xa, cần khoảng cách dòng lớn hơn làm "đường ray":

| Phân cảnh | Tiếng Tây | Tiếng Trung/Việt |
|------|------|------|
| Chữ lớn display (1-2 dòng) | 0.95-1.1 | 1.1-1.25 |
| Tiêu đề (h1-h3) | 1.1-1.3 | 1.3-1.4 |
| Chữ chính dòng ngắn (<30 chữ/dòng) | 1.4-1.5 | 1.6-1.7 |
| Chữ chính dòng dài (Gần tới giới hạn) | 1.6 | 1.8-2.0 |

Tiếng Trung toàn tuyến cao hơn Tiếng Tây khoảng 0.2: Chữ Hán là khối vuông đầy ô, không có khe hở tự nhiên giữa các chữ cái thường Tiếng Tây, khoảng cách dòng không đủ sẽ bị dính dập thành một mảng.

### text-wrap (Trình duyệt từ 2024+ đều hỗ trợ, chất lượng trình bày chữ miễn phí)

```css
h1, h2, h3 { text-wrap: balance; }  /* Tiêu đề nhiều dòng chiều dài các dòng cân bằng, tiêu diệt dòng chữ mồ côi */
p { text-wrap: pretty; }            /* Chữ chính tiêu diệt từ mồ côi cuối dòng (Hiệu quả Tiếng Tây rõ ràng, Tiếng Trung nhẹ) */
```

balance chỉ dùng cho tiêu đề ≤4 dòng (Thuật toán giới hạn 6 dòng và có chi phí hiệu năng); pretty toàn cục cho chữ chính không có tác dụng phụ.

## 3. Mười bộ Phối hợp Font chữ Mã nguồn mở (Tiếng Tây)

Ba nguồn tương phản của việc phối hợp, trước khi phối hãy nghĩ rõ ràng dùng loại nào:

- **Tương phản hình thức**: Display nét chân x body không nét chân (Kinh điển nhất, nhưng cần khớp x-height, nếu không cỡ chữ thị giác bị nhảy)
- **Khớp cùng hệ**: superfamily cùng một khung xương thiết kế (Rủi ro bằng 0, cái giá là bình nhạt)
- **Tương phản thời đại**: Hình chữ cổ điển x Hình chữ hiện đại (Hệ谱 lệch nhau 200 năm trở lên mới có sức căng, lệch 50 năm chỉ thấy loạn)

| # | Phối hợp (display + body) | Logic phối hợp | Nhiệt độ | Lấy về |
|---|------------------------|----------|------|------|
| 1 | Newsreader + Geist | Tương phản hình thức: Serif chuyển tiếp tối ưu cho màn hình, x-height cao, khớp tốt với Geist; **Bản thay thế chính chủ của Fraunces** | Trầm tĩnh | Google Fonts / Kho lưu trữ chính thức Vercel |
| 2 | Source Serif 4 + Source Sans 3 | Khớp cùng hệ: Cùng hệ thống thiết kế Adobe, nhịp điệu chiều cao và độ đậm chữ hoàn toàn căn chỉnh, báo cáo và tài liệu không lật kèo | Trầm tĩnh | Google Fonts |
| 3 | EB Garamond + IBM Plex Sans | Tương phản thời đại: Old style serif Pháp thế kỷ 16 x Grotesque lý tính 2017, sức căng chênh lệch 400 năm; Lưu ý x-height Garamond thấp, dùng chung cùng dòng cần bù cỡ chữ (Kinh nghiệm bắt đầu +8%, giải pháp hệ thống dùng `font-size-adjust`, xem chương 4) | Trầm tĩnh·Văn khí | Google Fonts |
| 4 | Lora + Hanken Grotesk | Tương phản hình thức: Serif cảm giác cọ Lora tương phản trung bình, chịu ngắm trên màn hình; Hanken là họ hàng mã nguồn mở mang khí chất Söhne | Trung tính | Google Fonts |
| 5 | Instrument Serif + Geist | Tương phản hình thức: Chỉ có một nấc độ đậm 400, bẩm sinh display-only, chữ chính bắt buộc phải giao cho sans. ⚠️ Đang trên đường bị các công cụ AI dùng nát, 2026 thận trọng dùng trong các trường hợp "Muốn tỏ ra độc đáo" | Trung tính | Google Fonts |
| 6 | Schibsted Grotesk + Source Serif 4 | Đảo ngược cấu trúc: grotesque làm display, serif làm chữ chính, cảm giác truyền thông; **Bản thay thế sau khi Space Grotesk tràn lan** (Báo chí Schibsted Na Uy tùy chỉnh mã nguồn mở, mang dòng máu tin tức) | Trung tính | Google Fonts |
| 7 | Bricolage Grotesque + Newsreader | Tương phản hình thức: Chi tiết ink trap và không quy tắc của Bricolage chỉ hiện ra ở cỡ chữ lớn, bẩm sinh display; Phối với chữ chính serif trầm tĩnh tạo thành thô mộc x văn nhã | Táo bạo | Google Fonts |
| 8 | Archivo (Expanded/Black) + Inter | Cấu trúc báo chữ lớn: Archivo bản rộng đậm đặn đè trường, Inter chỉ làm thợ chữ chính 14-16px (Đây là cách dùng đúng của Inter, xem mô hình phản diện) | Táo bạo | Google Fonts |
| 9 | Cormorant Garamond + Work Sans | Cảm giác xa xỉ tương phản cao: Nét chữ Cormorant cực mảnh, **bắt buộc ≥40px mới thành lập**, cỡ chữ nhỏ nét chữ sẽ bị đứt; Phù hợp phong cách thời trang/catalogue vũ trụ | Táo bạo | Google Fonts |
| 10 | Geist Mono / JetBrains Mono + Geist | Bằng nhau làm nhân vật chính: Cảm giác dòng lệnh, cảm giác kỹ thuật; Chữ bằng nhau chỉ dùng cho nhãn/số hiệu/code, toàn đoạn chữ chính dùng chữ bằng nhau là thảm họa (Chiều dài dòng phình to 30%) | Trung tính·Kỹ thuật | Vercel / JetBrains chính thức, đều là OFL |

**Danh sách đã bị dùng nát** (Dấu vân tay của trang web AI tạo ra, dùng nghĩa là tự bóc phốt):

| Nát đường phố | Tại sao nát | Bản thay thế |
|--------|----------|------|
| Fraunces làm display | Tùy chọn mặc định "có gu" của tất cả công cụ thiết kế AI giai đoạn 2023-2025 | Newsreader, Libre Caslon Text |
| Inter làm display | Inter được thiết kế cho chữ UI nhỏ, cỡ chữ lớn đồng đều không biểu cảm | Archivo, Anton, Schibsted Grotesk |
| Space Grotesk | Đáp án lười biếng của "Cảm giác công nghệ", tràn lan trên landing page tiền mã hóa/AI | Schibsted Grotesk, Familjen Grotesk |
| Playfair Display | Đáp án lười biếng của "Thanh lịch", thiệp mời đám cưới既视感 | Cormorant (Cực đoan hơn), DM Serif Display (Thô hơn) |

## 4. Trình bày chữ Tiếng Trung/CJK (Chương quan trọng nhất tài liệu này)

Tiếng Tây có chuỗi công cụ trưởng thành trăm năm, Tiếng Trung thì không. Công cụ thiết kế AI đầu hàng tập thể trên Tiếng Trung (Mặc định giao cho font hệ thống, trực tiếp áp quy tắc Tiếng Tây), đây chính là nơi tạo sự biệt lập.

### 4.1 Bản đồ Font chữ Tiếng Trung Mã nguồn mở / Miễn phí Thương mại

| Font chữ | Thể loại | Khí chất | Nhiệt độ | Lấy về |
|------|------|------|------|------|
| Tư Nguyên Tống Thể (Noto Serif SC) | Tống thể | Chính thống xuất bản, 7 độ đậm đầy đủ, Heavy có thể làm display | Trầm tĩnh-Trung tính | Google Fonts, OFL |
| Tư Nguyên Hắc Thể (Noto Sans SC) | Hắc thể | Inter của giới Tiếng Trung: Đáng tin, không biểu cảm, làm chữ chính mặc định không sai nhưng không có cá tính | Dự phòng toàn nhiệt độ | Google Fonts, OFL |
| Hà Vụ Văn Khải (LxgwWenKai) | Khải thể | Nhiệt độ viết tay, thân thiết, phù hợp văn bản chính và trích dẫn văn nghệ/giáo dục/blog cá nhân | Trầm tĩnh·Ấm | GitHub lxgw/LxgwWenKai, OFL |
| Hà Vụ Tân Tích Hắc | Hắc thể | Hắc thể hiển thị gầy hơn và thoáng khí hơn Tư Nguyên Hắc, chữ chính đọc lâu không mệt | Trầm tĩnh | GitHub lxgw/LxgwNeoXiHei |
| Đắc Ý Hắc (Smiley Sans) | 斜Hắc thể | **Chữ nghiêng nguyên bản hiếm gặp trong giới Tiếng Trung**, cảm giác chuyển động, chuyên dùng cho tiêu đề; Chữ chính dùng nó sẽ bị chóng mặt | Táo bạo | GitHub atelier-anchor/smiley-sans, OFL |
| Hối Văn Minh Triều Thể | Hình chữ Minh cổ | Khí chất chữ chì in cổ, xuất bản hoài cổ, phù hợp bìa sách/display loại văn hóa | Trung tính-Táo bạo·Hoài cổ | MaoKne/GitHub, Miễn phí thương mại |
| Kinh Hoa Lão Tống Thể | Lão Tống | Tiêu đề Tống nét chữ vuông cứng, cảm giác đầu báo | Táo bạo·Hoài cổ | MaoKne, Miễn phí thương mại |
| Nguyên Lưu Minh Thể / Nguyên Dạng Minh Thể | Minh triều thể (Hướng Phồn) | Khắc lại từ Tư Nguyên Tống, giữ lại chi tiết hình chữ truyền thống, lựa chọn hàng đầu cho nội dung Phồn thể | Trầm tĩnh·Cổ điển | GitHub ButTaiwan, OFL |
| Vị Lai Huỳnh Hắc (Glow Sans) | Hắc hình học | Hắc hình học hiện đại phái sinh từ Tư Nguyên Hắc, nhiều chiều rộng (Compressed có thể làm display hẹp dài) | Trung tính-Táo bạo·Hiện đại | GitHub welai/glow-sans, OFL |
| MiSans / HarmonyOS Sans / OPPO Sans | Hắc UI hãng | Hắc UI có một chút cá tính hơn Tư Nguyên Hắc, thích hợp cho App prototype | Trung tính | Website chính thức các hãng, Miễn phí thương mại |

Suy luận lựa chọn: **Chữ chính chỉ chọn trong Tống/Hắc/Khải** (Còn lại đều là font display, dùng toàn đoạn sẽ mệt); display muốn có cá tính mới động đến Đắc Ý Hắc/Lão Tống/Minh triều thể. Font chữ Tiếng Trung một font chấp 10 font Tiếng Tây (File đơn 5-15MB), một trang tối đa 2 họ font Tiếng Trung, vì hai lý do tải và tính thống nhất.

### 4.2 Quy tắc Trộn Trung Tây

**Chuỗi fallback là đòn bẩy số 1**: Ký tự Tiếng Tây đi kèm font chữ Tiếng Trung nhìn chung xấu (Ký tự Latinh của Tư Nguyên Hắc ngây ngô), đặt font chữ Tiếng Tây ở phía trước, ký tự Latinh và con số được nó đón lấy, chữ Hán tự động rơi vào font chữ Tiếng Trung phía sau:

```css
/* Tiếng Tây phía trước, Tiếng Trung phía sau, Hệ thống Tiếng Trung dự phòng, Loại chung kết thúc */
font-family: "Geist", "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
/* Serif tương tự */
font-family: "Newsreader", "Noto Serif SC", "Songti SC", serif;
```

Tại sao thứ tự này: font-family khớp từng ký tự, font chữ Tiếng Tây không chứa CJK, chữ Hán tự nhiên xuyên qua đến font chữ Tiếng Trung. Viết ngược lại (Tiếng Trung phía trước) ký tự Tiếng Tây bị font chữ Tiếng Trung nuốt hết, bằng như phối hợp vô ích.

**Bù cỡ chữ**: Cùng cỡ chữ Tiếng Tây chữ thường thị giác thiên nhỏ (x-height chỉ chiếm một nửa thân chữ, chữ Hán chiếm đầy). Hai giải pháp:

```css
/* Giải pháp 1: font-size-adjust để font fallback quy về 1 theo x-height (Chrome 127+/FF/Safari 17+) */
:root { font-size-adjust: from-font; }
/* Giải pháp 2: Chọn font Tiếng Tây có x-height cao (Geist/Inter/Source Sans đều cao), trộn lẫn tự nhiên đều */
```

**Căn chỉnh baseline**: Khi baseline Trung Tây không nhất quán triệu chứng là từ Tiếng Anh bị "chìm" trong dòng Tiếng Trung. Ưu tiên đổi sang font Tiếng Tây có x-height cao hơn; Kịch bản display riêng lẻ dùng `vertical-align: -0.02em~-0.06em` tinh chỉnh span Tiếng Tây, chữ chính đừng sửa như vậy (Chi phí bảo trì lớn hơn lợi ích).

**Quy tắc con số**: Con số nhất loạt đi theo font chữ Tiếng Tây (Chuỗi fallback đã đảm bảo), bảng dữ liệu bắt buộc phải thêm `font-variant-numeric: tabular-nums`, nếu không 1 và 8 chiều rộng khác nhau, cột sẽ bị rung lắc.

**Không thêm khoảng trắng giữa Trung Anh**: Đây là quy chuẩn kho lưu trữ này (Hoa Thúc chỉ định rõ ràng không dùng Bàn Cổ chi Bạch), dựa vào bản thân font chữ của chuỗi fallback để lại khoảng trắng, không dựa vào gõ khoảng trắng thủ công.

### 4.3 Tiếng Trung không có Chữ nghiêng

Hình chữ Tiếng Trung không có truyền thống italic, trình duyệt gặp `font-style: italic` sẽ nghiêng chữ Hán cơ khí (faux italic), nét chữ biến dạng, cực xấu. Bảng thay thế phương thức nhấn mạnh:

| Thói quen Tiếng Tây | Thay thế Tiếng Trung | CSS |
|----------|----------|-----|
| italic nhấn mạnh | Đổi độ đậm chữ | `font-weight: 600` (Tiền đề: Font chữ thực sự có nấc độ đậm này) |
| italic Tên sách/Trích dẫn | Highlight màu nền | `background: linear-gradient(transparent 60%, #FFE9A8 60%)` Kiểu bút huỳnh quang |
| italic Khối trích dẫn | Đổi font chữ | Trích dẫn toàn đoạn đổi sang Hà Vụ Văn Khải, Khải thể bản thân nó chính là "giọng điệu trích dẫn" của Tiếng Trung |
| italic Tên riêng | Màu sắc/Dấu nhấn mạnh | `text-emphasis: dot` (Dấu nhấn mạnh, nhấn mạnh nguyên bản Tiếng Trung, độ hỗ trợ đã sẵn sàng) |

Cầu chì: `font-synthesis: none;` Toàn cục cấm chữ nghiêng tổng hợp và chữ đậm tổng hợp, thà không nhấn mạnh chứ không chấp nhận chữ biến dạng.

### 4.4 Quy chuẩn Dấu câu

| Quy tắc | Cách làm | Tại sao |
|------|------|--------|
| Dấu ngoặc kép | Ngoặc vuông 「」『』, không dùng ngoặc cong "" | Dấu ngoặc cong trong font Tiếng Trung chiếm ô toàn góc nhưng hình dạng là Tiếng Tây, thị giác trôi nổi; 「」 là quy chuẩn cứng của kho này |
| Tránh đầu đuôi | `line-break: strict;` | Cấm dấu chấm dấu phẩy xuất hiện ở đầu dòng, ngoặc mở xuất hiện ở cuối dòng, đây là mức sàn của trình bày chữ Tiếng Trung |
| Dấu câu treo | `hanging-punctuation: first allow-end;` (Chỉ Safari); Kháo trình duyệt dùng `text-indent: -0.5em` xử lý ngoặc mở đầu đoạn | Ngoặc mở đầu đoạn không treo sẽ làm dòng đầu tiên nhìn như lùi vào nửa ô, mép trái thị giác không đều |
| Nén dấu câu liên tiếp | `font-feature-settings: "halt";` (Nén cuối dòng) hoặc `"palt"` (Chiều rộng tỷ lệ toàn phần, cần phối hợp letter-spacing) | Dấu câu toàn góc xếp liên tiếp (Như 「）。」) sẽ xuất hiện lỗ trống rộng một chữ rưỡi, halt thu hẹp nó |

### 4.5 Khoảng cách chữ (letter-spacing) Tiếng Trung

| Phân cảnh | Khoảng | Tại sao |
|------|------|--------|
| Chữ chính | 0 đến 0.05em | Thêm nhẹ khoảng cách chữ tăng độ thoáng khí; Vượt quá 0.05em sự hoàn chỉnh của từ bị đánh tan, tốc độ đọc giảm |
| Tiêu đề (24-48px) | 0 | Khoảng cách chữ khối vuông Tiếng Trung tự nhiên đều đặn, không cần điều chỉnh tracking kiểu Tiếng Tây |
| Chữ khổng lồ display (>60px) | -0.02em đến 0 | Dưới cỡ chữ lớn khoảng trống giữa các mặt chữ bị phóng đại, thu nhẹ chặt chẽ hơn; Âm thêm nữa nét chữ đánh nhau |
| Nhãn nhỏ Tiếng Tây viết hoa toàn bộ | 0.08-0.15em | Kịch bản duy nhất cần khoảng cách chữ dương lớn, và chỉ có hiệu lực với Tiếng Tây viết hoa |

**Tiếng Trung không bao giờ dùng bộ "display thu -0.05em" của Tiếng Tây**: Chữ Hán là thiết kế đầy ô, khoảng cách chữ âm trực tiếp làm nét chữ đánh nhau.

### 4.6 Chữ lớn display Tiếng Trung

Tiếng Trung không có hệ sinh thái font display từ Ultra Thin đến Black như Tiếng Tây, tính kịch của chữ lớn phải dựa vào suy luận để tạo ra:

- **Tương phản độ đậm chữ là vũ khí chính**: Tư Nguyên Tống Heavy 900 đè Light 300, cùng một font chữ hai độ đậm cực đoan cùng màn hình, có sức căng hơn đổi font chữ và chi phí tải bằng 0
- **Mật độ nét chữ quyết định giới hạn dưới cỡ chữ khả dụng**: Font chữ nét mảnh/tương phản lớn (Thanh ngang mảnh của Tống thể, kiểu Cormorant) chỉ thành lập ở cỡ chữ lớn; Nhỏ hơn 24px nét mảnh bắt đầu đứt nét, chữ chính bắt buộc quay lại Hắc thể/Nét trung bình
- **Hướng ngược lại cũng thành lập**: Font chữ nét dày (Hắc thể Black, Lão Tống) dưới cỡ chữ siêu lớn lượng mực quá lớn, chênh lệch lượng mực giữa "一" và "灥" bị phóng đại, tiêu đề mật độ không đều cân nhắc đổi sang nấc độ đậm thấp hơn một nấc
- **Xếp dọc là vũ khí display độc nhất của Tiếng Trung**: `writing-mode: vertical-rl` làm tiêu đề kiểu gáy sách, thơ ca, mục lục, Tiếng Tây không làm được; Lưu ý Tiếng Tây và con số trong xếp dọc dùng `text-orientation: upright` hoặc `text-combine-upright: all` (Con số hai chữ số hợp thể đứng thẳng)

## 5. Danh sách Mô hình Phản diện

| ❌ Mô hình Phản diện | Tại sao sai |
|-----------|----------|
| Toàn trường Inter (display+body một cân tất) | Inter là công cụ chữ UI nhỏ, làm display đồng đều không biểu cảm; Đây là dấu vân tay số 1 của "Trang web AI tạo ra" |
| Tiếng Trung giao cho `sans-serif` hệ thống mặc định | Windows rơi vào Trung Dịch Tống Thể/Nhã Hắc, macOS rơi vào Bính Phương, cùng một trang web跨 thiết bị hoàn toàn hai mặt khác nhau, bằng như không làm thiết kế |
| faux italic / faux bold | Trình duyệt tổng hợp biến dạng: Chữ nghiêng làm vặn vẹo chữ Hán, tổng hợp加粗 làm nét chữ dính thành cục mực; Dùng `font-synthesis: none` cắt tận gốc |
| Khoảng cách chữ tiêu đề lớn quá lỏng | Display Tiếng Tây cần thu chặt (Khoảng trống cỡ chữ lớn bị phóng đại), AI thường làm ngược lại thêm +0.05em, tiêu đề lỏng lẻo như giữ chỗ tạm thời |
| Chiều dài dòng mất kiểm soát (Không có max-width) | Màn hình lớn một dòng 60 chữ Hán, người đọc quay lại dòng tất bị lạc đường; Trong vấn đề khả năng đọc chiều dài dòng mất kiểm soát xếp thứ nhất, gây hại hơn chọn sai font chữ |
| Nấc cỡ chữ >6 nấc | Cấp độ mất giá, người đọc không phân biệt được cái gì quan trọng; Ý nghĩa của âm阶 chính là cưỡng chế kiềm chế |
| Chỉ có 2 nấc độ đậm 400/700 | Cấp độ toàn dựa vào cỡ chữ chống đỡ, trang web phẳng; Thời đại variable font 300-900 đều là chiều diễn đạt miễn phí |
| Bảng biểu/Dữ liệu không dùng tabular-nums | Chiều rộng con số không bằng nhau, cột bên trái bên phải rung lắc, cảm giác tin cậy dữ liệu trực tiếp bị giảm trừ |
| Chữ chính Tiếng Trung dùng font display (Đắc Ý Hắc/Lão Tống xếp toàn đoạn) | Cá tính của font display trong chữ chính biến thành lực cản khả năng đọc, sau 200 chữ là mệt |
| Font chữ Tiếng Trung đặt ở đầu chuỗi fallback khi trộn Trung Tây | Ký tự Latinh bị chữ Tiếng Tây xấu đi kèm font Tiếng Trung nuốt hết, font Tiếng Tây đã phối tốt không bao giờ đến lượt xuất trường |

## 6. Điểm mấu chốt Thực thi CSS

```css
:root {
  /* 1. Chuỗi fallback: Tiếng Tây → Tiếng Trung → Hệ thống Tiếng Trung → Loại chung (Thứ tự tức quy tắc, xem 4.2) */
  --font-body: "Geist", "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
  --font-display: "Newsreader", "Noto Serif SC", "Songti SC", serif;

  /* 2. Cấm tổng hợp: Không chấp nhận chữ nghiêng/加粗 do trình duyệt làm giả (Bắt buộc mở trong kịch bản Tiếng Trung) */
  font-synthesis: none;

  /* 3. Mức sàn xuống dòng Tiếng Trung */
  line-break: strict;        /* Tránh đầu đuôi */
  overflow-wrap: break-word; /* URL dài/Chuỗi Tiếng Anh không làm tràn container */
}

body {
  font-family: var(--font-body);
  font-size: 17px;           /* Cơ chuẩn chữ chính Tiếng Trung, xem chương 1 */
  line-height: 1.8;          /* Cơ chuẩn chiều cao dòng Tiếng Trung, xem chương 2 */
  /* Chữ chính mở ligatures tiêu chuẩn, đóng các tính năng hoa mỹ */
  font-feature-settings: "liga" 1, "calt" 1;
}

/* Kịch bản dữ liệu: Con số bằng nhau + Số không có gạch chéo (0 và O không nhầm lẫn) */
.data, table { font-variant-numeric: tabular-nums slashed-zero; }

/* Nhãn nhỏ Tiếng Tây: Viết hoa toàn bộ + Khoảng cách chữ lớn kịch bản hợp pháp duy nhất */
.label { text-transform: uppercase; letter-spacing: 0.1em; font-size: 12px; }

/* Nén dấu câu: Thu hẹp lỗ trống dấu câu toàn góc trong chữ lớn display Tiếng Trung */
.display-cjk { font-feature-settings: "halt" 1; }
```

**Tải font chữ Tiếng Trung** (File đơn 5-15MB, trích dẫn trực tiếp toàn lượng sẽ phá hỏng màn hình đầu tiên):

- Ưu tiên hệ Noto SC của Google Fonts (Đã tự động cắt thành hàng trăm phân đoạn theo unicode-range, trình duyệt chỉ tải chữ được dùng)
- Self-host font chữ cá tính (Hà Vụ/Đắc Ý Hắc v.v.) bắt buộc phải cắt nhỏ tập hợp trước: `cn-font-split` hoặc `pyftsubset` của fonttools, font chữ chính cắt theo 3500 chữ thường dùng, font display cắt theo ký tự thực tế xuất hiện (Một tấm poster thường chỉ có 20 chữ, tập hợp nhỏ có thể nén xuống trong vòng 50KB)
- `font-display: swap` giữ đáy, font chữ Tiếng Trung tải chậm, màn hình trắng đợi font chữ là trải nghiệm tồi tệ nhất
