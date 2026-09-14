# Gallery Ripple + Multi-Focus · Triết lý Biên kịch Phân cảnh

> Cấu trúc biên kịch thị giác có thể tái sử dụng được trích xuất từ hero animation v9 (25 giây, 8 cảnh) của huashu-design.
> Không phải dây chuyền sản xuất animation, mà là **trong kịch bản nào cấu trúc biên kịch này là "đúng"**.
> Tham khảo thực chiến: [demos/hero-animation-v9.mp4](../demos/hero-animation-v9.mp4) · [https://www.huasheng.ai/huashu-design-hero/](https://www.huasheng.ai/huashu-design-hero/)

## Một câu nói trước

> **Khi bạn có trên 20 vật liệu thị giác đồng chất và phân cảnh cần "diễn đạt cảm giác quy mô và chiều sâu", hãy ưu tiên xem xét cấu trúc biên kịch Gallery Ripple + Multi-Focus này thay vì xếp chồng layout.**

Animation feature SaaS thông thường, hội nghị ra mắt sản phẩm, quảng bá skill, hiển thị portfolio tác phẩm theo series——chỉ cần số lượng vật liệu đủ và phong cách thống nhất, cấu trúc này hầu như đều mang lại hiệu quả cao.

---

## Thủ pháp này rốt cuộc đang diễn đạt điều gì

Không phải "khoe vật liệu"——mà là kể một câu chuyện tự sự thông qua **hai sự thay đổi nhịp điệu**:

**Nhịp thứ nhất · Triển khai Ripple (~1.5s)**: Từ trung tâm khuếch tán ra 48 card hướng ra xung quanh, khán giả bị khuất phục bởi "lượng"——"Ồ, cái này có nhiều thành phẩm đến vậy".

**Nhịp thứ hai · Multi-Focus (~8s, 4 lần vòng lặp)**: Trong khi ống kính di chuyển pan chậm, 4 lần đưa nền dim + desaturate, phóng to riêng một card bất kỳ ra chính giữa màn hình——khán giả chuyển từ "sự va đập của lượng" sang "sự ngắm nhìn của chất", mỗi lần 1.7s nhịp điệu ổn định.

**Cấu trúc tự sự cốt lõi**: **Quy mô (Ripple) → Ngắm nhìn (Focus × 4) → Mờ dần (Walloff)**. Ba nhịp này kết hợp lại diễn đạt "Breadth × Depth" (Chiều rộng × Chiều sâu)——không chỉ có thể làm ra rất nhiều, mà mỗi một cái đều đáng để dừng lại xem.

So sánh với ví dụ phản diện:

| Cách làm | Cảm nhận của khán giả |
|------|---------|
| 48 card sắp xếp tĩnh (Không có Ripple) | Đẹp nhưng không có tự sự, giống như một bức ảnh chụp màn hình grid |
| Chuyển nhanh từng hình một (Không có Gallery context) | Giống slideshow, mất đi "cảm giác quy mô" |
| Chỉ có Ripple không có Focus | Bị chấn động nhưng không làm người ta nhớ được bức hình cụ thể nào |
| **Ripple + Focus × 4 (Công thức này)** | **Đầu tiên chấn động bởi lượng, sau đó ngắm nhìn bởi chất, cuối cùng mờ dần bình yên——cung cảm xúc hoàn chỉnh** |

---

## Điều kiện tiên quyết (Bắt buộc phải thỏa mãn tất cả)

Cấu trúc biên kịch này **không phải là vạn năng**, 4 điều bên dưới không thể thiếu bất kỳ điều nào:

1. **Quy mô vật liệu ≥ 20 hình, tốt nhất là 30+**
   Ít hơn 20 hình Ripple sẽ trông "trống"——trong 48 ô mỗi ô đều chuyển động mới có cảm giác mật độ. v9 dùng 48 ô × 32 hình (lấp đầy vòng lặp).

2. **Phong cách thị giác vật liệu thống nhất**
   Tất cả là preview slide 16:9 / tất cả là ảnh chụp ứng dụng / tất cả là thiết kế bìa——tỷ lệ chiều dài rộng, tông màu, bố cục phải giống như "một bộ". Trộn lẫn sẽ làm Gallery trông giống như bảng tạm (clipboard).

3. **Vật liệu sau khi phóng to riêng biệt vẫn có thông tin đọc được**
   Focus là phóng to một card nào đó lên chiều rộng 960px, nếu hình gốc phóng to bị mờ hoặc thông tin mỏng thảnh, nhịp Focus này coi như bỏ. Xác minh ngược lại: Có thể chọn ra 4 hình từ 48 hình làm "đại diện nhất" không? Không chọn ra được chứng tỏ chất lượng vật liệu không đồng đều.

4. **Bản thân phân cảnh là landscape hoặc square, không phải màn hình dọc**
   Độ nghiêng 3D của Gallery (`rotateX(14deg) rotateY(-10deg)`) cần cảm giác trải dài theo chiều ngang, màn hình dọc sẽ làm hiệu ứng nghiêng trông hẹp và gượng gạo.

**Con đường dự phòng khi thiếu điều kiện**:

| Thiếu cái gì | Thoái hóa thành cái gì |
|-------|-----------|
| Vật liệu < 20 hình | Đổi sang "Hiển thị tĩnh 3-5 hình xếp hàng + focus từng cái" |
| Phong cách không thống nhất | Đổi sang keynote-style "Bìa + 3 hình lớn chương" |
| Thông tin mỏng thảnh | Đổi sang "data-driven dashboard" hoặc "Kim ngôn + Chữ lớn" |
| Phân cảnh màn hình dọc | Đổi sang "vertical scroll + sticky cards" |

---

## Công thức kỹ thuật (Tham số thực chiến v9)

### Cấu trúc 4-Layer

```
viewport (1920×1080, perspective: 2400px)
  └─ canvas (4320×2520, overflow siêu lớn) → 3D tilt + pan
      └─ 8×6 grid = 48 cards (gap 40px, padding 60px)
          └─ img (16:9, border-radius 9px)
      └─ focus-overlay (absolute center, z-index 40)
          └─ img (matches selected slide)
```

**Mấu chốt**: canvas lớn hơn viewport 2.25 lần, như vậy pan mới có cảm giác "nhìn trộm thế giới lớn hơn".

### Triển khai Ripple (Thuật toán độ trễ khoảng cách)

```js
// Thời gian vào cảnh của mỗi card = Khoảng cách tới trung tâm × 0.8s độ trễ
const col = i % 8, row = Math.floor(i / 8);
const dc = col - 3.5, dr = row - 2.5;       // offset tới trung tâm
const dist = Math.hypot(dc, dr);
const maxDist = Math.hypot(3.5, 2.5);
const delay = (dist / maxDist) * 0.8;       // 0 → 0.8s
const localT = Math.max(0, (t - rippleStart - delay) / 0.7);
const opacity = expoOut(Math.min(1, localT));
```

**Tham số cốt lõi**:
- Tổng thời lượng 1.7s (`T.s3_ripple: [8.3, 10.0]`)
- Độ trễ tối đa 0.8s (Trung tâm ra sớm nhất, góc ra muộn nhất)
- Thời lượng vào cảnh của mỗi card 0.7s
- Easing: `expoOut` (Cảm giác bùng nổ, không phải phẳng mượt)

**Việc làm đồng thời**: canvas scale từ 1.25 → 0.94 (zoom out to reveal) —— Cảm giác đẩy xa đồng bộ phối hợp xuất hiện.

### Multi-Focus (Nhịp điệu 4 lần)

```js
T.focuses = [
  { start: 11.0, end: 12.7, idx: 2  },  // 1.7s
  { start: 13.3, end: 15.0, idx: 3  },  // 1.7s
  { start: 15.6, end: 17.3, idx: 10 },  // 1.7s
  { start: 17.9, end: 19.6, idx: 16 },  // 1.7s
];
```

**Quy luật nhịp điệu**: Mỗi focus 1.7s, khoảng cách 0.6s nghỉ thở. Tổng cộng 8s (11.0–19.6s).

**Bên trong mỗi lần focus**:
- In ramp: 0.4s (`expoOut`)
- Hold: Giữa 0.9s (`focusIntensity = 1`)
- Out ramp: 0.4s (`easeOut`)

**Sự thay đổi của nền (Đây là mấu chốt)**:

```js
if (focusIntensity > 0) {
  const dimOp = entryOp * (1 - 0.6 * focusIntensity);  // dim to 40%
  const brt = 1 - 0.32 * focusIntensity;                // brightness 68%
  const sat = 1 - 0.35 * focusIntensity;                // saturate 65%
  card.style.filter = `brightness(${brt}) saturate(${sat})`;
}
```

**Không chỉ là opacity——đồng thời desaturate + làm tối**. Điều này giúp màu sắc của lớp overlay tiền cảnh "bật lên", chứ không chỉ là "sáng hơn một chút".

**Animation kích thước Focus overlay**:
- Từ 400×225 (vào cảnh) → 960×540 (trạng thái hold)
- Bên ngoài có 3 lớp shadow + 3px outline ring màu accent, thể hiện "cảm giác được đóng khung"

### Pan (Cảm giác duy trì giúp việc đứng yên không bị nhàm chán)

```js
const panT = Math.max(0, t - 8.6);
const panX = Math.sin(panT * 0.12) * 220 - panT * 8;
const panY = Math.cos(panT * 0.09) * 120 - panT * 5;
```

- Chuyển động hai lớp sóng sin + drift tuyến tính——không phải vòng lặp thuần túy, mỗi thời điểm vị trí đều khác nhau
- Tần số X/Y khác nhau (0.12 vs 0.09) tránh để thị giác nhìn ra "vòng lặp có quy luật"
- clamp trong ±900/500px ngăn không cho trôi ra ngoài

**Tại sao không dùng pan tuyến tính thuần túy**: Pan tuyến tính thuần túy khán giả sẽ "dự đoán" giây tiếp theo ở đâu; sóng sin+drift làm cho mỗi giây đều mới mẻ, dưới độ nghiêng 3D tạo ra "cảm giác say sóng nhẹ" (loại tốt), sự chú ý được giữ chặt.

---

## 5 Pattern có thể tái sử dụng (Chắt lọc từ lịch sử lặp v6→v9)

### 1. **expoOut làm easing chính, không phải cubicOut**

`easeOut = 1 - (1-t)³` (phẳng mượt) vs `expoOut = 1 - 2^(-10t)` (bùng nổ xong thu hẹp nhanh).

**Lý do lựa chọn**: 30% đầu của expoOut đạt 90% rất nhanh, giống ma sát vật lý hơn, phù hợp với bản năng "vật nặng hạ cánh". Đặc biệt thích hợp cho:
- Card vào cảnh (Cảm giác trọng lượng)
- Khuếch tán Ripple (Sóng va chạm)
- Brand nổi lên (Cảm giác định hình)

**Khi nào vẫn dùng cubicOut**: focus out ramp, vi chuyển động đối xứng.

### 2. **Màu nền cảm giác giấy + Accent cam đất nung (Dòng máu Anthropic)**

```css
--bg: #F7F4EE;        /* Giấy ấm */
--ink: #1D1D1F;       /* Gần như đen */
--accent: #D97757;    /* Cam đất nung */
--hairline: #E4DED2;  /* Đường nét ấm */
```

**Tại sao**: Màu nền ấm áp sau khi nén GIF vẫn có "cảm giác nhịp thở", không giống như màu trắng thuần sẽ nhìn ra "cảm giác màn hình". Cam đất nung làm accent duy nhất chạy xuyên suốt terminal prompt, chọn dir-card, cursor, brand hyphen, focus ring——tất cả các mỏ neo thị giác đều được nối lại bởi một màu này.

**Bài học v5**: Đã thêm noise overlay để mô phỏng "nét giấy", kết quả nén khung hình GIF hỏng hoàn toàn (mỗi khung hình đều khác nhau). v6 đổi thành "chỉ dùng màu nền + shadow ấm", cảm giác giấy giữ lại 90%, dung lượng GIF giảm 60%.

### 3. **Shadow hai nấc mô phỏng độ sâu, không dùng 3D thật**

```css
.gallery-card.depth-near { box-shadow: 0 32px 80px -22px rgba(60,40,20,0.22), ... }
.gallery-card.depth-far  { box-shadow: 0 14px 40px -16px rgba(60,40,20,0.10), ... }
```

Dùng thuật toán xác định `sin(i × 1.7) + cos(i × 0.73)` phân bổ shadow 3 nấc near/mid/far cho mỗi card——**về thị giác có "cảm giác xếp chồng 3D", nhưng transform mỗi khung hình hoàn toàn không đổi, GPU tiêu thụ 0**.

**Cái giá của 3D thật**: Mỗi card `translateZ` riêng, GPU mỗi khung hình đều đang tính 48 transform + shadow blur. v4 đã thử qua, Playwright ghi hình 25fps cũng chật vật. Shadow hai nấc của v6 hiệu quả mắt thường chênh lệch <5%, nhưng chi phí chênh lệch 10 lần.

### 4. **Thay đổi độ đậm chữ (font-variation-settings) mang lại cảm giác điện ảnh hơn thay đổi cỡ chữ**

```js
const wght = 100 + (700 - 100) * morphP;  // 100 → 700 qua 0.9s
wordmark.style.fontVariationSettings = `"wght" ${wght.toFixed(0)}`;
```

Brand wordmark từ Thin → Bold dùng biến đổi dần 0.9s, phối hợp tinh chỉnh letter-spacing (-0.045 → -0.048em).

**Tại sao tốt hơn phóng to thu nhỏ**:
- Phóng to thu nhỏ khán giả xem quá nhiều, kỳ vọng đóng cứng
- Thay đổi độ đậm chữ là "cảm giác sung sức nội tại", giống như quả bóng bay được thổi căng, chứ không phải "được đẩy gần"
- variable fonts là tính năng mới phổ biến từ 2020+, khán giả theo tiềm thức cảm thấy "hiện đại"

**Hạn chế**: Bắt buộc phải dùng font hỗ trợ variable font (Inter/Roboto Flex/Recursive v.v.). Font tĩnh thông thường chỉ có thể giả lập (chuyển đổi vài weight cố định sẽ bị nhảy).

### 5. **Corner Brand chữ ký duy trì cường độ thấp**

Giai đoạn Gallery góc trên bên trái có một logo nhỏ `HUASHU · DESIGN`, giá trị màu opacity 16%, cỡ chữ 12px, khoảng cách chữ rộng.

**Tại sao thêm cái này**:
- Sau khi Ripple bùng nổ khán giả dễ bị "mất tiêu điểm" không nhớ đang xem cái gì, logo nhẹ góc trên bên trái giúp định neo (anchor)
- Đẳng cấp hơn logo lớn toàn màn hình——người làm thương hiệu biết rằng, chữ ký thương hiệu không cần phải hét lên
- Khi GIF bị chụp màn hình chia sẻ vẫn để lại tín hiệu sở hữu

**Quy tắc**: Chỉ xuất hiện ở đoạn giữa (màn hình busy), mở màn tắt (không che terminal), kết thúc tắt (brand reveal là nhân vật chính).

---

## Phản diện: Khi nào KHÔNG NÊN dùng cấu trúc biên kịch này

**❌ Demo sản phẩm (Cần hiển thị tính năng)**: Gallery làm cho từng hình đều lướt qua nhanh chóng, khán giả không nhớ được tính năng nào. Đổi sang "focus màn hình đơn + chú thích tooltip".

**❌ Nội dung dựa trên dữ liệu**: Khán giả cần đọc con số, nhịp điệu nhanh của Gallery không cho thời gian đọc. Đổi sang "biểu đồ dữ liệu + reveal từng mục".

**❌ Tự sự câu chuyện**: Gallery là cấu trúc "song song", câu chuyện cần "nhân quả". Đổi sang chuyển đổi chương keynote.

**❌ Vật liệu chỉ có 3-5 hình**: Mật độ Ripple không đủ, nhìn giống như "miếng vá". Đổi sang "sắp xếp tĩnh + highlight từng hình".

**❌ Màn hình dọc (9:16)**: Độ nghiêng 3D cần trải dài theo chiều ngang, màn hình dọc sẽ làm cảm giác nghiêng bị "méo" chứ không phải "triển khai".

---

## Làm thế nào để đánh giá nhiệm vụ của mình có phù hợp với cấu trúc này không

Kiểm tra nhanh 3 bước:

**Step 1 · Số lượng vật liệu**: Đếm xem bạn có bao nhiêu vật liệu thị giác cùng loại. < 15 → Dừng; 15-25 → Gom; 25+ → Dùng trực tiếp.

**Step 2 · Kiểm tra tính thống nhất**: Đặt 4 hình ngẫu nhiên xếp hàng ngang, có giống "một bộ" không? Không giống → Thống nhất phong cách trước rồi làm, hoặc đổi phương án.

**Step 3 · Khớp tự sự**: Bạn muốn diễn đạt "Breadth × Depth" (Quy mô × Chiều sâu) phải không? Hay là "Quy trình", "Tính năng", "Câu chuyện"? Không phải cái trước thì đừng gượng ép.

Cả 3 bước đều yes, fork trực tiếp v6 HTML, sửa mảng `SLIDE_FILES` và timeline là có thể tái sử dụng. Bảng màu sửa `--bg / --accent / --ink`, thay da không thay xương toàn bộ.

---

## Reference liên quan

- Quy trình kỹ thuật hoàn chỉnh: [references/animations.md](animations.md) · [references/animation-best-practices.md](animation-best-practices.md)
- Dây chuyền xuất animation: [references/video-export.md](video-export.md)
- Cấu hình âm thanh (Track đôi BGM + SFX): [references/audio-design-rules.md](audio-design-rules.md)
- Tham khảo ngang phong cách Apple Gallery: [references/apple-gallery-showcase.md](apple-gallery-showcase.md)
- HTML nguồn (v6 + Bản tích hợp âm thanh): `www.huasheng.ai/huashu-design-hero/index.html`
