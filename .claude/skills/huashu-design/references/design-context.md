# Design Context: Xuất phát từ bối cảnh đã có

**Đây là The One Thing quan trọng nhất của skill này.**

Một thiết kế hi-fi tốt nhất định phải được lớn lên từ design context đã có. **Tự nhiên làm hi-fi từ hư không là last resort, nhất định sẽ tạo ra tác phẩm generic**. Cho nên mỗi lần nhiệm vụ thiết kế bắt đầu, hãy hỏi trước: Có cái gì có thể tham khảo không?

## Design Context là gì

Theo độ ưu tiên từ cao đến thấp:

### 1. Design System/UI Kit của người dùng
Thư viện component, màu sắc token, quy chuẩn font chữ, hệ thống icon sẵn có trong sản phẩm của chính người dùng. **Trường hợp hoàn hảo nhất**.

### 2. Codebase của người dùng
Nếu người dùng cung cấp codebase, bên trong sẽ có thực thi component sống động. Read những file component đó:
- `theme.ts` / `colors.ts` / `tokens.css` / `_variables.scss`
- Các component cụ thể (Button.tsx, Card.tsx)
- Layout scaffold (App.tsx, MainLayout.tsx)
- Global stylesheets

**Đọc code chép exact values**: hex codes, spacing scale, font stack, border radius. Đừng vẽ lại dựa vào ký ức.

### 3. Sản phẩm đã phát hành của người dùng
Nếu người dùng có sản phẩm đã online nhưng không đưa code, dùng Playwright hoặc nhờ người dùng cung cấp ảnh chụp màn hình.

```bash
# Dùng Playwright chụp ảnh màn hình một URL công khai
npx playwright screenshot https://example.com screenshot.png --viewport-size=1920,1080
```

Giúp bạn nhìn thấy visual vocabulary thực tế.

### 4. Hướng dẫn thương hiệu/Logo/Vật liệu sẵn có
Người dùng có thể có: File Logo, quy chuẩn màu thương hiệu, vật liệu marketing, template slide. Những cái này đều là context.

### 5. Tham khảo sản phẩm đối thủ
Người dùng nói "Giống như trang web XX đó"——nhờ họ cung cấp URL hoặc ảnh chụp màn hình. **ĐỪNG** làm dựa vào ấn tượng mờ mịt trong dữ liệu huấn luyện của bạn.

### 6. Design system đã biết (fallback)
Nếu những cái trên đều không có, dùng hệ thống thiết kế được công nhận làm base:
- Apple HIG
- Material Design 3
- Radix Colors (Phối màu)
- shadcn/ui (Component)
- Tailwind palette mặc định

Nói rõ với người dùng hệ thống bạn sử dụng, để họ biết đây là điểm xuất phát chứ không phải bản chốt.

## Quy trình lấy Context

### Step 1: Hỏi người dùng

Danh mục bắt buộc hỏi khi nhiệm vụ bắt đầu (Đến từ `workflow.md`):

```markdown
1. Bạn có sẵn design system/UI kit/thư viện component không? Ở đâu?
2. Có hướng dẫn thương hiệu, quy chuẩn màu sắc/font chữ không?
3. Có thể cho tôi ảnh chụp màn hình hoặc URL của sản phẩm hiện tại không?
4. Có codebase tôi có thể đọc không?
```

### Step 2: Khi người dùng nói "Không có", giúp họ tìm

Đừng bỏ cuộc trực tiếp. Thử nghiệm:

```markdown
Để tôi xem có manh mối nào không:
- Dự án trước đây của bạn có thiết kế liên quan không?
- Website marketing của công ty dùng màu sắc/font chữ gì?
- Logo sản phẩm của bạn phong cách gì? Có thể cho tôi một tấm không?
- Có sản phẩm nào bạn thưởng thức để làm tham khảo không?
```

### Step 3: Read tất cả context có thể tìm thấy

Nếu người dùng đưa đường dẫn codebase, bạn đọc:
1. **List cấu trúc file trước**: Tìm các file liên quan đến style/theme/component
2. **Đọc file theme/token**: lift các giá trị hex/px cụ thể
3. **Đọc 2-3 component đại diện**: Xem visual vocabulary (hover state, shadow, border, padding node pattern)
4. **Đọc global stylesheet**: Reset cơ bản, font loading
5. **Nếu có link Figma/Ảnh chụp màn hình**: Xem hình, nhưng **tin tưởng code hơn**

**Quan trọng**: **ĐỪNG** nhìn một cái rồi làm dựa vào ấn tượng. Đọc xuống có 30+ giá trị cụ thể mới thực sự lift được tới nơi.

### Step 4: Vocalize hệ thống bạn muốn dùng

Sau khi xem xong context, nói cho người dùng biết hệ thống bạn muốn dùng:

```markdown
Dựa vào codebase và ảnh chụp màn hình sản phẩm của bạn, hệ thống thiết kế tôi trích xuất ra:

**Màu sắc**
- Primary: #C27558 (Từ tokens.css)
- Background: #FDF9F0
- Text: #1A1A1A
- Muted: #6B6B6B

**Font chữ**
- Display: Instrument Serif (Từ @font-face của global.css)
- Body: Geist Sans
- Mono: JetBrains Mono

**Spacing** (Đến từ hệ thống scale của bạn)
- 4, 8, 12, 16, 24, 32, 48, 64

**Shadow pattern**
- `0 1px 2px rgba(0,0,0,0.04)` (subtle card)
- `0 10px 40px rgba(0,0,0,0.1)` (elevated modal)

**Border-radius**
- Component nhỏ 4px, Card 12px, Nút 8px

**component vocabulary**
- Button: filled primary, outlined secondary, ghost tertiary, tất cả bo góc 8px
- Card: Nền trắng, subtle shadow, không border

Tôi sẽ bắt đầu làm theo bộ hệ thống này. Xác nhận không có vấn đề gì chứ?
```

Người dùng xác nhận xong mới ra tay.

## Thiết kế từ hư không (Fallback khi không có Context)

**Cảnh báo mạnh mẽ**: Chất lượng đầu ra trong trường hợp này sẽ giảm đáng kể. Nói rõ cho người dùng biết.

```markdown
Bạn không có design context, tôi chỉ có thể làm dựa trên bản năng thông thường.
Thành phẩm sẽ là thứ "trông có vẻ OK nhưng thiếu tính độc đáo".
Bạn muốn tiếp tục, hay là bổ sung một số vật liệu tham khảo trước?
```

Người dùng khăng khăng muốn bạn làm, hãy đưa ra quyết định theo thứ tự này:

### 1. Chọn một aesthetic direction
Đừng đưa ra kết quả generic. Chọn một hướng đi rõ ràng:
- brutally minimal
- editorial/magazine
- brutalist/raw
- organic/natural
- luxury/refined
- playful/toy
- retro-futuristic
- soft/pastel

Nói cho người dùng biết bạn đã chọn cái nào.

### 2. Chọn một design system đã biết làm khung xương
- Dùng Radix Colors làm phối màu (https://www.radix-ui.com/colors)
- Dùng shadcn/ui làm component vocabulary (https://ui.shadcn.com)
- Dùng Tailwind spacing scale (Bội số của 4)

### 3. Chọn phối hợp font chữ có đặc trưng

Đừng dùng Inter/Roboto. Gợi ý kết hợp (Lấy miễn phí từ Google Fonts):
- Instrument Serif + Geist Sans
- Cormorant Garamond + Inter Tight
- Bricolage Grotesque + Söhne (Trả phí)
- Fraunces + Work Sans (Lưu ý Fraunces đã bị AI dùng nát)
- JetBrains Mono + Geist Sans (technical feel)

### 4. Mỗi quyết định mấu chốt đều có reasoning

Đừng âm thầm chọn. Viết trong comment của HTML:

```html
<!--
Design decisions:
- Primary color: warm terracotta (oklch 0.65 0.18 25) — fits the "editorial" direction  
- Display: Instrument Serif for humanist, literary feel
- Body: Geist Sans for cleanness contrast
- No gradients — committed to minimal, no AI slop
- Spacing: 8px base, golden ratio friendly (8/13/21/34)
-->
```

## Chiến lược Import (Người dùng đưa codebase)

Nếu người dùng nói "import codebase này làm tham khảo":

### Loại nhỏ (<50 file)
Read tất cả, nội hóa context.

### Loại trung bình (50-500 file)
Focus vào:
- `src/components/` hoặc `components/`
- Tất cả các file liên quan đến styles/tokens/theme
- 2-3 component toàn trang đại diện (Home.tsx, Dashboard.tsx)

### Loại lớn (>500 file)
Nhờ người dùng chỉ định rõ focus:
- "Tôi muốn làm trang settings" → Đọc các file liên quan đến settings hiện có
- "Tôi muốn làm một feature mới" → Đọc shell tổng thể + Tham khảo gần nhất
- Không cầu toàn bộ, cầu sự chuẩn xác

## Phối hợp với Figma/Bản thiết kế

Nếu người dùng đưa link Figma:

- **ĐỪNG** kỳ vọng bạn có thể trực tiếp "chuyển Figma thành HTML"——cái đó cần công cụ bổ sung
- Link Figma thường không truy cập công khai được
- Nhờ người dùng: Xuất thành **ảnh chụp màn hình** gửi cho bạn + Nói cho bạn biết giá trị color/spacing cụ thể
- Nếu chỉ đưa ảnh chụp màn hình Figma, nói với người dùng:
  - Tôi có thể nhìn thấy thị giác, nhưng không lấy được giá trị chuẩn xác
  - Con số mấu chốt (hex, px) xin hãy nói cho tôi biết, hoặc export as code (Figma hỗ trợ)

## Nhắc nhở cuối cùng

**Giới hạn trên chất lượng thiết kế của một dự án, được quyết định bởi chất lượng context bạn lấy được**.

Dành 10 phút thu thập context, có giá trị hơn dành 1 giờ tự nhiên vẽ hi-fi từ hư không.

**Gặp trường hợp không có context, ưu tiên hỏi xin người dùng, chứ đừng cố làm**.
