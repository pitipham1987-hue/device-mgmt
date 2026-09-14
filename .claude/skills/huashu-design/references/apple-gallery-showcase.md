# Apple Gallery Showcase · Phong cách Animation Tường Trưng bày Phòng tranh

> Nguồn cảm hứng: Video hero trang web chính thức Claude Design + Trưng bày kiểu "Tường tác phẩm" trên trang sản phẩm Apple
> Xuất xứ thực chiến: hero animation v5 ra mắt của huashu-design
> Kịch bản áp dụng: **Hero animation ra mắt sản phẩm, Demo năng lực skill, Trưng bày portfolio**——Bất kỳ kịch bản nào cần trưng bày đồng thời "Nhiều thành phẩm chất lượng cao" và dẫn dắt sự chú ý của khán giả

---

## Đánh giá kích hoạt: Khi nào dùng phong cách này

**Phù hợp**:
- Có trên 10 hình thành phẩm thực tế cần hiển thị cùng màn hình (PPT, App, Trang web, Infographic)
- Khán giả là đối tượng chuyên nghiệp (Developer, Nhà thiết kế, Product Manager), nhạy cảm với "chất lượng"
- Hy vọng truyền tải khí chất "Kiềm chế, Kiểu triển lãm, Cao cấp, Có cảm giác không gian"
- Cần tiêu điểm và toàn cục cùng tồn tại (Xem chi tiết nhưng không mất đi tổng thể)

**Không phù hợp**:
- Tiêu điểm đơn sản phẩm (Dùng template hero sản phẩm của frontend-design)
- Animation hướng tới cảm xúc/Tính câu chuyện mạnh (Dùng template tự sự timeline)
- Màn hình nhỏ / Màn hình dọc (Góc nhìn nghiêng trên màn hình nhỏ sẽ bị mờ)

---

## Token Thị giác Cốt lõi

```css
:root {
  /* Bảng màu phòng tranh màu sáng */
  --bg:         #F5F5F7;   /* Nền canvas chính — Xám trang chính thức Apple */
  --bg-warm:    #FAF9F5;   /* Biến thể trắng kem ấm áp */
  --ink:        #1D1D1F;   /* Màu chữ chính */
  --ink-80:     #3A3A3D;
  --ink-60:     #545458;
  --muted:      #86868B;   /* Chữ cấp hai */
  --dim:        #C7C7CC;
  --hairline:   #E5E5EA;   /* Viền 1px của card */
  --accent:     #D97757;   /* Cam đất nung — Claude brand */
  --accent-deep:#B85D3D;

  --serif-cn: "Noto Serif SC", "Songti SC", Georgia, serif;
  --serif-en: "Source Serif 4", "Tiempos Headline", Georgia, serif;
  --serif-cn: "Noto Serif SC", "Songti SC", Georgia, serif;
  --serif-en: "Source Serif 4", "Tiempos Headline", Georgia, serif;
  --sans:     "Inter", -apple-system, "PingFang SC", system-ui;
  --mono:     "JetBrains Mono", "SF Mono", ui-monospace;
}
```

**Nguyên tắc mấu chốt**:
1. **Tuyệt đối không dùng nền đen thuần**. Nền đen sẽ làm tác phẩm trông giống như phim điện ảnh, không giống "Thành phẩm công việc có thể được áp dụng"
2. **Cam đất nung là sắc độ accent duy nhất**, còn lại tất cả là thang xám + trắng
3. **Ba stack font chữ** (serif Tiếng Anh+serif Tiếng Trung+sans+mono) tạo ra khí chất "Ấn phẩm" chứ không phải "Sản phẩm Internet"

---

## Mẫu Bố cục Cốt lõi

### 1. Card Lơ lửng (Đơn vị cơ bản của toàn bộ phong cách)

```css
.gallery-card {
  background: #FFFFFF;
  border-radius: 14px;
  padding: 6px;                          /* Padding lề trong là "Giấy đóng khung" */
  border: 1px solid var(--hairline);
  box-shadow:
    0 20px 60px -20px rgba(29, 29, 31, 0.12),   /* Bóng chính, mềm và dài */
    0 6px 18px -6px rgba(29, 29, 31, 0.06);     /* Lớp ánh sáng gần thứ hai, tạo cảm giác nổi */
  aspect-ratio: 16 / 9;                  /* Thống nhất tỷ lệ slide */
  overflow: hidden;
}
.gallery-card img {
  width: 100%; height: 100%;
  object-fit: cover;
  border-radius: 9px;                    /* Nhỏ hơn bo góc card một chút, lồng nhau thị giác */
}
```

**Giáo trình phản diện**: Đừng dán gạch dán sát mép (Không padding không border không shadow)——Đó là diễn đạt mật độ của Infographic, không phải triển lãm.

### 2. Tường Tác phẩm Nghiêng 3D

```css
.gallery-viewport {
  position: absolute; inset: 0;
  overflow: hidden;
  perspective: 2400px;                   /* Phối cảnh sâu hơn một chút, nghiêng không quá đà */
  perspective-origin: 50% 45%;
}
.gallery-canvas {
  width: 4320px;                         /* Canvas = 2.25× viewport */
  height: 2520px;                        /* Để lại không gian pan */
  transform-origin: center center;
  transform: perspective(2400px)
             rotateX(14deg)              /* Nghiêng về sau */
             rotateY(-10deg)             /* Xoay sang trái */
             rotateZ(-2deg);             /* Nghiêng nhẹ, bỏ đi sự quá chỉn chu */
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  gap: 40px;
  padding: 60px;
}
```

**Tham số sweet spot**:
- rotateX: 10-15deg (Nhiều hơn nữa giống như phông nền VIP tiệc rượu)
- rotateY: ±8-12deg (Cảm giác đối xứng trái phải)
- rotateZ: ±2-3deg (Cảm giác người làm "Cái này không phải máy xếp")
- perspective: 2000-2800px (Nhỏ hơn 2000 sẽ bị mắt cá, lớn hơn 3000 gần với chiếu chính diện)

### 3. 2×2 Hội tụ Bốn góc (Phân cảnh lựa chọn)

```css
.grid22 {
  display: grid;
  grid-template-columns: repeat(2, 800px);
  gap: 56px 64px;
  align-items: start;
}
```

Mỗi card trượt vào từ góc tương ứng (tl/tr/bl/br) hướng về trung tâm + fade in. Vector `cornerEntry` tương ứng:

```js
const cornerEntry = {
  tl: { dx: -700, dy: -500 },
  tr: { dx:  700, dy: -500 },
  bl: { dx: -700, dy:  500 },
  br: { dx:  700, dy:  500 },
};
```

---

## Năm Pattern Animation Cốt lõi

### Pattern A · Hội tụ Bốn góc (0.8-1.2s)

4 phần tử trượt vào từ bốn góc viewport, đồng thời thu phóng 0.85→1.0, tương ứng ease-out. Phù hợp cho mở màn "Hiển thị lựa chọn nhiều hướng".

```js
const inP = easeOut(clampLerp(t, start, end));
card.style.transform = `translate3d(${(1-inP)*ce.dx}px, ${(1-inP)*ce.dy}px, 0) scale(${0.85 + 0.15*inP})`;
card.style.opacity = inP;
```

### Pattern B · Phóng to Được chọn + Các card khác trượt ra (0.8s)

Card được chọn phóng to 1.0→1.28, các card khác fade out + blur + trôi về bốn góc:

```js
// Được chọn
card.style.transform = `translate3d(${cellDx*outP}px, ${cellDy*outP}px, 0) scale(${1 + 0.28*easeOut(zoomP)})`;
// Không được chọn
card.style.opacity = 1 - outP;
card.style.filter = `blur(${outP * 1.5}px)`;
```

**Mấu chốt**: Các card không được chọn phải blur, không phải thuần fade. blur mô phỏng độ sâu trường ảnh, về thị giác đẩy card được chọn "nổi ra".

### Pattern C · Triển khai Ripple Gợn sóng (1.7s)

Từ trung tâm ra ngoài, delay theo khoảng cách, mỗi card lần lượt mờ dần vào + thu nhỏ từ 1.25x xuống 0.94x ("Ống kính kéo xa"):

```js
const col = i % COLS, row = Math.floor(i / COLS);
const dc = col - (COLS-1)/2, dr = row - (ROWS-1)/2;
const dist = Math.sqrt(dc*dc + dr*dr);
const delay = (dist / maxDist) * 0.8;
const localT = Math.max(0, (t - rippleStart - delay) / 0.7);
card.style.opacity = easeOut(Math.min(1, localT));

// Đồng thời scale tổng thể 1.25→0.94
const galleryScale = 1.25 - 0.31 * easeOut(rippleProgress);
```

### Pattern D · Sinusoidal Pan (Trôi trượt duy trì)

Dùng tổ hợp sóng sin + trôi tuyến tính, tránh cảm giác vòng lặp "có điểm đầu có điểm cuối" của marquee:

```js
const panX = Math.sin(panT * 0.12) * 220 - panT * 8;    // Trôi sang trái theo chiều ngang
const panY = Math.cos(panT * 0.09) * 120 - panT * 5;    // Trôi lên trên theo chiều dọc
const clampedX = Math.max(-900, Math.min(900, panX));   // Ngăn lộ mép
```

**Tham số**:
- Chu kỳ sóng sin `0.09-0.15 rad/s` (Chậm, khoảng 30-50 giây một nhịp lắc)
- Trôi tuyến tính `5-8 px/s` (Chậm hơn chớp mắt của khán giả)
- Biên độ `120-220 px` (Đủ lớn để cảm nhận, đủ nhỏ để không bị chóng mặt)

### Pattern E · Focus Overlay (Chuyển tiêu điểm)

**Thiết kế mấu chốt**: focus overlay là một **phần tử phẳng** (Không nghiêng), nổi trên canvas nghiêng. Slide được chọn thu phóng từ vị trí ô (Khoảng 400×225) lên chính giữa màn hình (960×540), canvas nền không thay đổi độ nghiêng nhưng **làm tối xuống 45%**:

```js
// Focus overlay (flat, centered)
focusOverlay.style.width = (startW + (endW - startW) * focusIntensity) + 'px';
focusOverlay.style.height = (startH + (endH - startH) * focusIntensity) + 'px';
focusOverlay.style.opacity = focusIntensity;

// Card nền làm tối, nhưng vẫn nhìn thấy (Mấu chốt! Đừng dùng mặt nạ 100%)
card.style.opacity = entryOp * (1 - 0.55 * focusIntensity);   // 1 → 0.45
card.style.filter = `brightness(${1 - 0.3 * focusIntensity})`;
```

**Quy tắc sắt về độ rõ nét**:
- `<img>` của Focus overlay bắt buộc `src` kết nối trực tiếp hình gốc, **không tái sử dụng ảnh thu nhỏ nén trong gallery**
- Tiến hành preload trước tất cả hình gốc vào mảng `new Image()[]`
- `width/height` của overlay tự tính theo từng khung hình, trình duyệt resample hình gốc mỗi khung hình

---

## Kiến trúc Timeline (Khung xương có thể tái sử dụng)

```js
const T = {
  DURATION: 25.0,
  s1_in: [0.0, 0.8],    s1_type: [1.0, 3.2],  s1_out: [3.5, 4.0],
  s2_in: [3.9, 5.1],    s2_hold: [5.1, 7.0],  s2_out: [7.0, 7.8],
  s3_hold: [7.8, 8.3],  s3_ripple: [8.3, 10.0],
  panStart: 8.6,
  focuses: [
    { start: 11.0, end: 12.7, idx: 2  },
    { start: 13.3, end: 15.0, idx: 3  },
    { start: 15.6, end: 17.3, idx: 10 },
    { start: 17.9, end: 19.6, idx: 16 },
  ],
  s4_walloff: [21.1, 21.8], s4_in: [21.8, 22.7], s4_hold: [23.7, 25.0],
};

// Easing cốt lõi (Thực thi lịch sử v9 dùng cubic; Easing chính dự án mới mặc định expoOut, xem best-practices §2 / Hiệu chỉnh pattern 1 hero-case-study)
const easeOut = t => 1 - Math.pow(1 - t, 3);
const easeInOut = t => t < 0.5 ? 4*t*t*t : 1 - Math.pow(-2*t+2, 3)/2;
function lerp(time, start, end, fromV, toV, easing) {
  if (time <= start) return fromV;
  if (time >= end) return toV;
  let p = (time - start) / (end - start);
  if (easing) p = easing(p);
  return fromV + (toV - fromV) * p;
}

// Hàm render(t) đơn lẻ đọc mã thời gian t, viết tất cả các phần tử
function render(t) { /* ... */ }
requestAnimationFrame(function tick(now) {
  const t = ((now - startMs) / 1000) % T.DURATION;
  render(t);
  requestAnimationFrame(tick);
});
```

**Tinh hoa kiến trúc**: **Tất cả trạng thái do mã thời gian t suy ra**, không có máy trạng thái, không có setTimeout. Như vậy:
- Phát đến thời điểm bất kỳ `window.__setTime(12.3)` nhảy ngay lập tức (Thuận tiện cho playwright chụp từng khung hình)
- Vòng lặp tự nhiên mượt mà (t mod DURATION)
- Khi Debug có thể đóng băng khung hình bất kỳ

---

## Chi tiết Chất lượng (Dễ bị bỏ qua nhưng chí mạng)

### 1. SVG noise texture

Nền màu sáng sợ nhất là "quá phẳng". Đè thêm một lớp fractalNoise cực yếu:

```html
<style>
.stage::before {
  content: '';
  position: absolute; inset: 0;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='200' height='200'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='2' stitchTiles='stitch'/><feColorMatrix values='0 0 0 0 0.078  0 0 0 0 0.078  0 0 0 0 0.074  0 0 0 0.035 0'/></filter><rect width='100%' height='100%' filter='url(%23n)'/></svg>");
  opacity: 0.5;
  pointer-events: none;
  z-index: 30;
}
</style>
```

Nhìn thì không thấy khác biệt, bỏ đi mới biết là có.

### 2. Biểu trưng thương hiệu ở góc

```html
<div class="corner-brand">
  <div class="mark"></div>
  <div>HUASHU · DESIGN</div>
</div>
```

```css
.corner-brand {
  position: absolute; top: 48px; left: 72px;
  font-family: var(--mono);
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--muted);
}
```

Chỉ hiển thị ở scene tường tác phẩm, mờ dần vào mờ dần ra. Giống như nhãn triển lãm phòng tranh.

### 3. Wordmark thu khép thương hiệu

```css
.brand-wordmark {
  font-family: var(--sans);
  font-size: 148px;
  font-weight: 700;
  letter-spacing: -0.045em;   /* Khoảng cách chữ âm là mấu chốt, để chữ chặt chẽ thành logo */
}
.brand-wordmark .accent {
  color: var(--accent);
  font-weight: 500;           /* Ký tự accent ngược lại mảnh hơn một chút, chênh lệch thị giác */
}
```

`letter-spacing: -0.045em` là cách làm tiêu chuẩn cho chữ lớn trên trang sản phẩm Apple.

---

## Các dạng thất bại thường gặp

| Triệu chứng | Nguyên nhân | Giải pháp |
|---|---|---|
| Trông giống như template PPT | Card không có shadow / hairline | Thêm hai lớp box-shadow + 1px border |
| Cảm giác nghiêng rẻ tiền | Chỉ dùng rotateY không thêm rotateZ | Thêm ±2-3deg rotateZ phá vỡ sự chỉn chu |
| Pan có cảm giác "giật" | Dùng setTimeout hoặc vòng lặp CSS keyframes | Dùng rAF + hàm liên tục sin/cos |
| Khi Focus chữ nhìn không rõ | Tái sử dụng hình độ phân giải thấp của ô gallery | overlay độc lập + hình gốc src kết nối trực tiếp |
| Nền quá trống | Màu thuần `#F5F5F7` | Đè SVG fractalNoise opacity 0.5 |
| Font chữ quá "Internet" | Chỉ có Inter | Thêm Serif (Anh Trung mỗi cái một font) + mono ba stack |

---

## Trích dẫn

- Mẫu thực thi hoàn chỉnh: hero-animation-v5.html (Mẫu cục bộ tác giả, không phân phối theo kho lưu trữ)
- Cảm hứng gốc: Video hero claude.ai/design
- Thẩm mỹ tham khảo: Trang sản phẩm Apple, Trang tập hợp Dribbble shot

Khi gặp nhu cầu animation "Nhiều thành phẩm chất lượng cao cần trưng bày", hãy copy khung xương trực tiếp từ file này, đổi nội dung + điều chỉnh timing là được.
