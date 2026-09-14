# Animations: Engine Hoạt ảnh Trục thời gian (Timeline Animation Engine)

Đọc tài liệu này khi làm hoạt ảnh HTML / motion design HTML. Nguyên lý, cách dùng, các mẫu mô hình điển hình.

## Mô hình cốt lõi: Stage + Sprite

Hệ thống hoạt ảnh của chúng ta (`assets/animations.jsx`) cung cấp một engine được dẫn dắt bởi trục thời gian:

- **`<Stage>`**: Container của toàn bộ hoạt ảnh, tự động cung cấp auto-scale (fit viewport) + scrubber + bộ điều khiển play/pause/loop
- **`<Sprite start end>`**: Phân đoạn thời gian. Một Sprite chỉ hiển thị trong khoảng thời gian từ `start` đến `end`. Bên trong có thể đọc tiến độ cục bộ `t` (0→1) của mình thông qua hook `useSprite()`
- **`useTime()`**: Đọc thời gian toàn cục hiện tại (tính bằng giây)
- **`Easing.easeInOut` / `Easing.easeOut` / ...**: Các hàm làm mượt (easing)
- **`interpolate(t, from, to, easing?)`**: Nội suy theo t

Bộ mô hình này học hỏi ý tưởng từ Remotion / After Effects, nhưng cực kỳ nhẹ nhàng và không phụ thuộc thư viện ngoài (zero dependency).

## Khởi đầu

```html
<script type="text/babel" src="animations.jsx"></script>
<script type="text/babel">
  const { Stage, Sprite, useTime, useSprite, Easing, interpolate } = window.Animations;

  function Title() {
    const { t } = useSprite();  // Tiến độ cục bộ 0→1
    const opacity = interpolate(t, [0, 1], [0, 1], Easing.easeOut);
    const y = interpolate(t, [0, 1], [40, 0], Easing.easeOut);
    return (
      <h1 style={{ 
        opacity, 
        transform: `translateY(${y}px)`,
        fontSize: 120,
        fontWeight: 900,
      }}>
        Hello.
      </h1>
    );
  }

  function Scene() {
    return (
      <Stage duration={10}>  {/* Hoạt ảnh 10 giây */}
        <Sprite start={0} end={3}>
          <Title />
        </Sprite>
        <Sprite start={2} end={5}>
          <SubTitle />
        </Sprite>
        {/* ... */}
      </Stage>
    );
  }

  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<Scene />);
</script>
```

## Các mô hình hoạt ảnh thường dùng

### 1. Fade In / Fade Out

```jsx
function FadeIn({ children }) {
  const { t } = useSprite();
  const opacity = interpolate(t, [0, 0.3], [0, 1], Easing.easeOut);
  return <div style={{ opacity }}>{children}</div>;
}
```

**Chú ý phạm vi**: `[0, 0.3]` có nghĩa là hoàn thành mờ dần vào trong 30% thời gian đầu của sprite, khoảng thời gian còn lại giữ opacity=1.

### 2. Slide In

```jsx
function SlideIn({ children, from = 'left' }) {
  const { t } = useSprite();
  const progress = interpolate(t, [0, 0.4], [0, 1], Easing.easeOut);
  const offset = (1 - progress) * 100;
  const directions = {
    left: `translateX(-${offset}px)`,
    right: `translateX(${offset}px)`,
    top: `translateY(-${offset}px)`,
    bottom: `translateY(${offset}px)`,
  };
  return (
    <div style={{
      transform: directions[from],
      opacity: progress,
    }}>
      {children}
    </div>
  );
}
```

### 3. Hiệu ứng gõ chữ (⚠️ Phân biệt 2 kịch bản trước, đừng dùng nhảy từng từ)

Typewriter gõ từng chữ đều đặn là ví dụ phản diện chính thức (Danh sách "AI slop" trong best-practices: Giống phụ đề phim cũ). Chọn cách làm đúng theo nội dung:

- **Đầu ra AI** (Dòng token xuất hiện) → Chunk Reveal: Xuất hiện dạng khối không đều, xem `animation-best-practices.md` §4.5 / `gsap-recipes.md` §3.4
- **Đầu vào người dùng** (Người thật gõ chữ trong ô nhập) → 3f/ký tự + Con trỏ sáng liên tục chuyển sang nhấp nháy + Đôi khi lùi ô xóa chữ, xem `ui-demo-animation.md` 8 kiểu③

### 4. Đếm số

```jsx
function CountUp({ from = 0, to = 100, duration = 0.6 }) {
  const { t } = useSprite();
  const progress = interpolate(t, [0, duration], [0, 1], Easing.easeOut);
  const value = Math.floor(from + (to - from) * progress);
  return <span>{value.toLocaleString()}</span>;
}
```

### 5. Giải thích phân đoạn (Hoạt ảnh giảng dạy điển hình)

```jsx
function Scene() {
  return (
    <Stage duration={20}>
      {/* Phase 1: Hiển thị vấn đề */}
      <Sprite start={0} end={4}>
        <Problem />
      </Sprite>

      {/* Phase 2: Hiển thị hướng giải quyết */}
      <Sprite start={4} end={10}>
        <Approach />
      </Sprite>

      {/* Phase 3: Hiển thị kết quả */}
      <Sprite start={10} end={16}>
        <Result />
      </Sprite>

      {/* Phụ đề hiển thị xuyên suốt */}
      <Sprite start={0} end={20}>
        <Caption />
      </Sprite>
    </Stage>
  );
}
```

## Các hàm Easing

Các đường cong easing dựng sẵn:

| Easing | Đặc tính | Dùng khi |
|--------|------|------|
| `linear` | Đều đặn | Chữ cuộn, Hoạt ảnh liên tục |
| `easeIn` | Chậm→Nhanh | Rút khỏi cảnh, mờ đi |
| `easeOut` | Nhanh→Chậm | Vào cảnh, xuất hiện |
| `easeInOut` | Chậm→Nhanh→Chậm | Thay đổi vị trí |
| **`expoOut`** ⭐ | **Chỉ số giảm dần** | **Easing chính cấp Anthropic** (Cảm giác trọng lượng vật lý)|
| **`overshoot`** ⭐ | **Hồi đàn đàn hồi** | **Toggle / Nút bấm nẩy ra / Tương tác nhấn mạnh** |
| `spring` | Lò xo | Phản hồi tương tác, Hình học về vị trí |
| `anticipation` | Ngược hướng trước rồi mới đúng hướng | Nhấn mạnh hành động |

**Easing chính mặc định dùng `expoOut`** (Không phải `easeOut`) —— Xem `animation-best-practices.md` §2.
Vào cảnh dùng `expoOut`, ra cảnh dùng `easeIn`, toggle dùng `overshoot` —— Quy luật nền tảng của hoạt ảnh cấp độ Anthropic.

## Hướng dẫn Nhịp điệu và Thời lượng

### Vi tương tác (Micro-interaction, 0.1-0.3 giây)
- Hover nút bấm
- Card expand
- Tooltip xuất hiện

### Quá độ UI (0.3-0.8 giây)
- Chuyển đổi trang
- Modal xuất hiện
- Item trong danh sách thêm vào

### Hoạt ảnh tự sự (2-10 giây mỗi đoạn)
- Một phase giải thích khái niệm
- Reveal biểu đồ dữ liệu
- Chuyển đổi cảnh

### Hoạt ảnh tự sự đơn đoạn dài nhất không quá 10 giây
Sự chú ý của con người có hạn. 10 giây nói một việc, nói xong chuyển sang việc tiếp theo.

## Thứ tự suy nghĩ khi thiết kế hoạt ảnh

### 1. Có nội dung/câu chuyện trước, rồi mới có hoạt ảnh

**Sai**: Muốn làm hoạt ảnh fancy trước, rồi nhét nội dung vào
**Đúng**: Nghĩ rõ xem muốn truyền tải thông điệp gì trước, rồi dùng phương tiện hoạt ảnh để phục vụ (serve) thông điệp đó

Hoạt ảnh là **signal**, chứ không phải **trang trí**. Một hiệu ứng fade-in nhấn mạnh "Ở đây rất quan trọng, xin hãy nhìn" —— Nếu cái gì cũng fade-in, signal sẽ mất hiệu lực.

### 2. Phân chia Scene viết trục thời gian

```
0:00 - 0:03   Vấn đề xuất hiện (fade in)
0:03 - 0:06   Vấn đề phóng to/mở rộng (zoom+pan)
0:06 - 0:09   Cách giải quyết xuất hiện (slide in từ bên phải)
0:09 - 0:12   Cách giải quyết mở rộng giải thích (typewriter)
0:12 - 0:15   Demo kết quả (counter up + chart reveal)
0:15 - 0:18   Tóm tắt một câu (static, đọc 3 giây)
0:18 - 0:20   CTA hoặc fade out
```

Viết xong trục thời gian rồi mới viết component.

### 3. Tài nguyên đi trước

Hình ảnh/Icon/Phông chữ cần dùng cho hoạt ảnh phải được chuẩn bị **trước**. Đừng vẽ được một nửa mới đi tìm tư liệu —— Làm đứt quãng nhịp điệu.

## Vấn đề thường gặp

**Hoạt ảnh bị giật (Lag)**
→ Chủ yếu do layout thrashing. Dùng `transform` và `opacity`, đừng động vào `top`/`left`/`width`/`height`/`margin`. Trình duyệt GPU sẽ tăng tốc cho `transform`.

**Hoạt ảnh quá nhanh, nhìn không rõ**
→ Con người đọc một ký tự cần 100-150ms, một từ cần 300-500ms. Nếu bạn dùng chữ để kể chuyện, câu đơn ít nhất phải để lại 3 giây.

**Hoạt ảnh quá chậm, khán giả nhàm chán**
→ Thay đổi thị giác thú vị phải đậm đặc. Màn hình tĩnh quá 5 giây sẽ bị nhàm.

**Nhiều hoạt ảnh ảnh hưởng lẫn nhau**
→ Dùng `will-change: transform` của CSS báo trước cho trình duyệt phần tử này sẽ chuyển động, giảm reflow.

**Ghi hình thành video**
→ Dùng chuỗi công cụ có sẵn của skill (Một dòng lệnh ra 3 định dạng): Xem `video-export.md`
- `scripts/render-video.js` — HTML → 25fps MP4 (Playwright + ffmpeg)
- `scripts/convert-formats.sh` — 25fps MP4 → 60fps MP4 + GIF tối ưu
- Muốn render khung hình chính xác hơn? Hãy làm cho `render(t)` thành pure function, xem điều 5 trong `animation-pitfalls.md`

## Phối hợp với các công cụ video

Skill này làm **hoạt ảnh HTML** (Chạy trong trình duyệt). Nếu thành phẩm cuối cùng làm tư liệu video:

- **Hoạt ảnh ngắn / Concept demo**: Dùng phương pháp ở đây làm hoạt ảnh HTML → Ghi màn hình
- **Video dài / Tự sự** (5-20 phút có thuyết minh): Đi theo quy trình thuyết minh dẫn dắt ở SKILL.md Step 9.5 (`voiceover-pipeline.md`), không cần đẩy ra công cụ khác
- **Motion graphics**: Các công cụ chuyên nghiệp như After Effects / Motion Canvas sẽ phù hợp hơn

## Khi cần hoạt ảnh vật lý (spring / decay)

Đừng dùng Popmotion (CDN dưới mạng bị hạn chế nhất định bị ngắt, vi phạm nguyên tắc tự chứa, xem `animation-pitfalls.md` #17). Nhu cầu spring đi theo GSAP: `elastic.out` / `back.out` và tùy biến ánh xạ springEase xem `gsap-recipes.md` §1.2; Dư chấn khi hạ đáp dùng giải pháp đóng dampedSettle (`camera-language.md` §9).
