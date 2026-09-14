# Content Guidelines: Anti-AI slop, Quy chuẩn nội dung, Quy phạm Scale

Cái bẫy dễ rơi vào nhất trong thiết kế AI. Đây là một danh mục "Những việc KHÔNG LÀM", quan trọng hơn cả "Những việc LÀM"——vì AI slop là giá trị mặc định, nếu bạn không chủ động tránh nó sẽ xảy ra.

## Bảng đen AI Slop Hoàn chỉnh

### Bẫy thị giác

**❌ Nền chuyển sắc (gradient) dữ dội**
- Nền chuyển sắc toàn màn hình Tím → Hồng → Xanh dương (Mùi vị điển hình của trang web AI tạo ra)
- Rainbow gradient theo bất kỳ hướng nào
- Mesh gradient phủ kín nền
- ✅ Nếu muốn dùng chuyển sắc: subtle, hệ đơn sắc, điểm xuyết có ý đồ (ví dụ button hover)

**❌ Thẻ bo góc (Card rounded) + Màu accent viền trái (border-left)**
```css
/* Đây là chữ ký điển hình của thẻ mang vị AI */
.card {
  border-radius: 12px;
  border-left: 4px solid #3b82f6;
  padding: 16px;
}
```
Loại thẻ này tràn lan trong Dashboard do AI tạo ra. Muốn làm nhấn mạnh? Dùng cách làm có cảm giác thiết kế hơn: Đối lập màu nền, Đối lập độ đậm/cỡ chữ, đường phân cách plain, hoặc đơn giản là không phân chia thẻ.

**❌ Trang trí bằng Emoji**
Trừ khi bản thân thương hiệu sử dụng emoji (ví dụ Notion, Slack), nếu không đừng đặt emoji lên UI. **Đặc biệt KHÔNG NÊN**:
- 🚀 ⚡️ ✨ 🎯 💡 trước tiêu đề
- ✅ trong danh sách Feature
- → trong nút CTA (Mũi tên xuất hiện riêng lẻ OK, mũi tên emoji KHÔNG OK)

Không có icon thì dùng thư viện icon thật (Lucide/Heroicons/Phosphor), hoặc dùng placeholder.

**❌ Dùng SVG vẽ imagery**
Đừng cố gắng dùng SVG để vẽ: Nhân vật, Bối cảnh, Thiết bị, Đồ vật, Nghệ thuật trừu tượng. Imagery SVG do AI vẽ nhìn qua là biết vị AI ngay, ấu trĩ và rẻ tiền. **Một hình chữ nhật xám + nhãn chữ "Vị trí hình minh họa 1200×800" mạnh hơn gấp 100 lần một hình hero illustration SVG vụng về**.

Kịch bản duy nhất có thể dùng SVG:
- Icon thực sự (Cấp độ 16×16 đến 32×32)
- Hình học làm phần tử trang trí
- Chart của Data viz

**❌ Sử dụng quá nhiều iconography**
Không phải tiêu đề/feature/section nào cũng cần icon. Lạm dụng icon sẽ làm giao diện trông như đồ chơi. Less is more.

**❌ "Data slop"**
Trang trí bằng chỉ số thống kê bịa đặt:
- "10,000+ happy customers" (Bạn còn không biết có hay không)
- "99.9% uptime" (Không có dữ liệu thật thì đừng viết)
- Trang trí "metric cards" cấu thành bởi icon + con số + từ ngữ
- Dữ liệu giả trong Mock table được trang trí lòe loẹt

Nếu không có dữ liệu thật, hãy để placeholder hoặc hỏi xin người dùng.

**❌ "Quote slop"**
Đánh giá người dùng bịa đặt, danh ngôn người nổi tiếng trang trí trang web. Hãy để placeholder hỏi xin người dùng quote thật.

### Bẫy font chữ

**❌ Tránh những font chữ nát đường phố này**:
- Inter (Mặc định của trang web do AI tạo ra)
- Roboto
- Arial / Helvetica
- System font stack thuần túy
- Fraunces (AI phát hiện ra cái này liền lạm dụng)
- Space Grotesk (Yêu thích gần đây của AI)

**✅ Dùng phối hợp display+body có đặc trưng**. Hướng cảm hứng:
- Display nét chân (serif) + body không nét chân (sans) (editorial feel)
- Display nét đơn (mono) + body không nét chân (sans) (technical feel)
- Display đậm (heavy) + body mảnh (light) (contrast)
- Variable font làm animation độ đậm mỏng của hero

Tài nguyên font chữ:
- Tùy chọn độc đáo ít người biết trên Google Fonts (Instrument Serif, Cormorant, Bricolage Grotesque, JetBrains Mono)
- Trạm font chữ mã nguồn mở (Font chữ anh em của Fraunces, Adobe Fonts)
- Đừng tự nhiên bịa ra tên font chữ

### Bẫy màu sắc

**❌ Tự nhiên bịa ra màu sắc**
Đừng thiết kế một bộ màu sắc không quen thuộc từ đầu. Điều này thường không hài hòa.

**✅ Chiến lược**:
1. Có màu thương hiệu → Dùng màu thương hiệu, color token thiếu dùng oklch nội suy
2. Không có màu thương hiệu nhưng có tham khảo → Hút màu từ ảnh chụp sản phẩm tham khảo
3. Hoàn toàn từ con số 0 → Chọn một hệ thống phối màu đã biết (Radix Colors / Tailwind palette mặc định / Anthropic brand), đừng tự mình điều chỉnh

**Định nghĩa màu sắc bằng oklch** là cách làm hiện đại nhất:
```css
:root {
  --primary: oklch(0.65 0.18 25);      /* Đất nung terracotta ấm áp */
  --primary-light: oklch(0.85 0.08 25); /* Màu nhạt cùng hệ màu */
  --primary-dark: oklch(0.45 0.20 25);  /* Màu đậm cùng hệ màu */
}
```
oklch có thể đảm bảo khi điều chỉnh độ sáng sắc độ không bị trôi, dễ dùng hơn hsl.

**❌ Chế độ ban đêm (Dark mode) tiện tay thêm màu đảo ngược**
Không phải đơn giản là invert màu sắc. Dark mode tốt cần điều chỉnh lại độ bão hòa, độ tương phản, màu accent. Không muốn làm dark mode thì đừng làm.

### Bẫy Bố cục (Layout)

**❌ Bento grid bị lạm dụng quá đà**
Mỗi landing page do AI tạo ra đều muốn làm bento. Trừ khi cấu trúc thông tin của bạn thực sự phù hợp với bento, nếu không hãy dùng layout khác.

**❌ Hero lớn + 3-column features + testimonials + CTA**
Template landing page này đã bị dùng nát rồi. Muốn sáng tạo thì hãy thực sự sáng tạo.

**❌ Mỗi card trong Card grid trông giống hệt nhau**
Asymmetric, cards kích thước khác nhau, có cái mang hình ảnh có cái chỉ có chữ, có cái băng qua các cột——đây mới giống do nhà thiết kế thật làm.

## Quy chuẩn nội dung

### 1. Don't add filler content

Mỗi phần tử bắt buộc phải tự giành lấy vị trí cho mình (earn its place). Khoảng trống là vấn đề thiết kế, giải quyết bằng **bố cục** (đối lập, nhịp điệu, khoảng trắng), **KHÔNG PHẢI** dựa vào việc lấp đầy nội dung.

**CÂU HỎI ĐÁNH GIÁ FILLER**:
- Nếu bỏ đoạn nội dung này đi, thiết kế có bị tồi đi không? Nếu câu trả lời là "Không", hãy bỏ đi.
- Phần tử này giải quyết vấn đề thực tế gì? Nếu là "để trang web bớt khoảng trống", hãy xóa đi.
- Chỉ số thống kê/quote/feature này có dữ liệu thật hỗ trợ không? Không có thì đừng tự nhiên viết ra.

"One thousand no's for every yes".

### 2. Ask before adding material

Bạn cảm thấy thêm một đoạn/một trang/một section sẽ tốt hơn? Hỏi người dùng trước, đừng đơn phương thêm vào.

Lý do:
- Người dùng hiểu rõ khán giả của họ hơn bạn
- Thêm nội dung có chi phí, người dùng có thể không muốn
- Đơn phương thêm nội dung vi phạm mối quan hệ "Junior designer báo cáo công việc".

### 3. Create a system up front

Sau khi khám phá xong design context, **nói ra hệ thống bạn muốn dùng bằng miệng trước**, để người dùng xác nhận:

```markdown
Hệ thống thiết kế của tôi:
- Màu sắc: Thể chính #1A1A1A + Nền #F0EEE6 + Accent #D97757 (Đến từ thương hiệu của bạn)
- Font chữ: Instrument Serif làm display + Geist Sans làm body
- Nhịp điệu: Section title dùng nền màu full-bleed + Chữ trắng; Section thông thường dùng nền trắng
- Hình ảnh: Hero dùng ảnh full-bleed, feature section dùng placeholder đợi bạn cung cấp
- Tối đa dùng 2 loại màu nền, tránh hỗn loạn

Xác nhận hướng này tôi sẽ bắt đầu làm.
```

Người dùng xác nhận xong mới ra tay. Bước check-in này giúp tránh "làm xong một nửa mới phát hiện sai hướng".

## Quy phạm Scale

### Slide (1920×1080)

- Chữ chính tối thiểu **24px**, lý tưởng 28-36px
- Tiêu đề 60-120px
- Section title 80-160px
- Hero headline có thể dùng chữ lớn 180-240px
- Không bao giờ dùng chữ <24px đặt lên slide

### Tài liệu in ấn

- Chữ chính tối thiểu **10pt** (≈13.3px), lý tưởng 11-12pt
- Tiêu đề 18-36pt
- Caption 8-9pt

### Web và Mobile

- Chữ chính tối thiểu **14px** (Thân thiện với người cao tuổi dùng 16px)
- Chữ chính mobile **16px** (Tránh iOS tự động thu phóng)
- Hit target (Phần tử có thể click) tối thiểu **44×44px**
- Chiều cao dòng (line-height) 1.5-1.7 (Tiếng Trung 1.7-1.8)

### Độ tương phản

- Chữ chính vs Nền **ít nhất 4.5:1** (WCAG AA)
- Chữ lớn vs Nền **ít nhất 3:1**
- Dùng công cụ accessibility của Chrome DevTools để kiểm tra

## Thần khí CSS

**Tính năng CSS nâng cao** là người bạn tốt của nhà thiết kế, hãy mạnh dạn sử dụng:

### Trình bày chữ

```css
/* Giúp tiêu đề xuống dòng tự nhiên hơn, không bị dòng cuối mồ côi một từ */
h1, h2, h3 { text-wrap: balance; }

/* Xuống dòng chữ chính, tránh từ mồ côi (widows & orphans) */
p { text-wrap: pretty; }

/* Thần khí trình bày chữ Tiếng Trung: Nén dấu câu, kiểm tra đầu dòng cuối dòng */
p { 
  text-spacing-trim: space-all;
  hanging-punctuation: first;
}
```

### Bố cục (Layout)

```css
/* CSS Grid + named areas = Độ đọc bùng nổ */
.layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 240px 1fr;
  grid-template-rows: auto 1fr auto;
}

/* Subgrid căn chỉnh nội dung thẻ */
.card { display: grid; grid-template-rows: subgrid; }
```

### Hiệu ứng thị giác

```css
/* Thanh cuộn có cảm giác thiết kế */
* { scrollbar-width: thin; scrollbar-color: #666 transparent; }

/* Giả thủy tinh (Sử dụng kiềm chế) */
.glass {
  backdrop-filter: blur(20px) saturate(150%);
  background: color-mix(in oklch, white 70%, transparent);
}

/* View transitions API giúp chuyển đổi trang mượt mà */
@view-transition { navigation: auto; }
```

### Tương tác

```css
/* Selector :has() giúp style điều kiện trở nên dễ dàng */
.card:has(img) { padding-top: 0; } /* Thẻ có hình ảnh không có padding đỉnh */

/* Container queries giúp component thực sự responsive */
@container (min-width: 500px) { ... }

/* Hàm color-mix mới */
.button:hover {
  background: color-mix(in oklch, var(--primary) 85%, black);
}
```

## Tra nhanh quyết định: Khi bạn do dự

- Muốn thêm chuyển sắc (gradient)? → Xác suất cao là KHÔNG THÊM
- Muốn thêm emoji? → KHÔNG THÊM
- Muốn thêm bo góc+border-left accent cho thẻ? → KHÔNG THÊM, đổi cách khác
- Muốn dùng SVG vẽ hình minh họa hero? → KHÔNG VẼ, dùng placeholder
- Muốn thêm một đoạn quote trang trí? → Hỏi người dùng trước xem có quote thật không
- Muốn thêm một hàng icon features? → Hỏi trước xem có cần icon không, có thể không cần
- Dùng Inter? → Đổi sang font chữ khác có đặc trưng hơn
- Dùng chuyển sắc màu tím? → Đổi sang phối màu có căn cứ
- **Khi bạn cảm thấy "Thêm vào một chút sẽ đẹp hơn"——đó thường là điềm báo của AI slop**. Hãy làm phiên bản đơn giản nhất trước, chỉ thêm khi người dùng yêu cầu.
