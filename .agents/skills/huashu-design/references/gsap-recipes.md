# GSAP Recipes · Tầng Dịch thuật từ Ngôn ngữ Thiết kế sang GSAP Timeline

> Tệp này chỉ làm một việc: Chuyển toàn bộ ngôn ngữ thiết kế hoạt ảnh đã được đúc kết của huashu-design
> (Cấu trúc 5 đoạn tự sự, hệ thống easing, 8 quy tắc chuyển động, công thức kịch bản trong `animation-best-practices.md`,
> cũng như template 5-scene 22 giây trong `cinematic-patterns.md`) thành các công thức hiện thực hóa
> GSAP timeline có thể dán trực tiếp để chạy trên backend render HyperFrames.
>
> **Nhận định thiết kế lấy các references của chính skill này làm chuẩn, GSAP chỉ là công cụ hiện thực hóa.**
> Khi nào nên tạm dừng, nên dùng đường cong tự sự nào, thế nào là đẹp, hãy đọc `animation-best-practices.md` §0;
> Tệp này chỉ trả lời câu hỏi "Quy tắc này dùng GSAP viết thế nào".
> Hợp đồng tổng hợp của HyperFrames (Thuộc tính composition root, đánh dấu `.clip`, lệnh render, kiểm toán check)
> xem tại `references/hyperframes-backend.md`, tệp này chỉ tham chiếu chứ không nhắc lại.

---

## 0 · Mẫu cơ bản (Mỗi bản tổng hợp đều bắt đầu từ đây)

```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
<script>
  window.__timelines = window.__timelines || {};

  const tl = gsap.timeline({
    paused: true,                                   // Bắt buộc. HyperFrames chịu trách nhiệm seek
    defaults: { ease: "expo.out", duration: 0.6 },  // Easing chính của skill này (Xem §1)
  });

  // ... Tất cả tween đều treo trên timeline này ...

  window.__timelines["main"] = tl;  // key bắt buộc bằng data-composition-id của root tổng hợp
</script>
```

Ràng buộc cứng (Vi phạm bất kỳ điều nào, kết quả render không xác định):

- Timeline bắt buộc `paused: true`, **không bao giờ gọi `tl.play()`** để làm hoạt ảnh render then chốt
- Timeline bắt buộc phải dựng sẵn trong code đồng bộ, không đặt vào async / timer / event callback
- Thời lượng render đến từ `data-duration` của root tổng hợp, không phải độ dài timeline. Đừng dùng tween rỗng để lót độ dài
- Cấm `repeat: -1`. Hành động lặp lại dùng thời lượng nhìn thấy được tính ra số lần repeat hữu hạn
- Chú ý: `defaults: { ease: "expo.out" }` Khác với house default `power3.out` trong tài liệu hyperframes-animation.
  Đó là gu thẩm mỹ của họ, quy tắc có sẵn của skill này là "expoOut là easing chính mặc định", tầng dịch thuật tuân thủ ngôn ngữ thiết kế của chính mình

---

## 1 · Bảng Ánh xạ Easing · Easing Tự nghiên cứu → GSAP

Các hàm Easing tự nghiên cứu trong `assets/animations.jsx`, tương ứng từng cái một với cách viết trong GSAP.
3 cái đầu tiên về mặt toán học là **hoàn toàn cùng một đường cong**, không phải xấp xỉ.

| Easing Tự nghiên cứu | Định nghĩa Toán học | Cách viết GSAP | Mối quan hệ | Mục đích (Quy tắc có sẵn) |
|---|---|---|---|---|
| `expoOut` | `1 - 2^(-10t)` | `"expo.out"` | Hoàn toàn nhất quán | **Easing chính mặc định**. Card rise-in, Panel vào cảnh, Terminal fade, Focus overlay |
| `overshoot` | easeOutBack, c1=1.70158 | `"back.out"` (Mặc định 1.70158) hoặc `"back.out(1.7)"` | Hoàn toàn nhất quán | Chuyển đổi Toggle, Nút bấm nẩy ra, Tương tác nhấn mạnh |
| `spring` | easeOutElastic, chu kỳ 2π/3 | `"elastic.out(1, 0.3)"` (Mặc định `"elastic.out"`) | Hoàn toàn nhất quán | Hình học về vị trí, Rơi vật lý về vị trí, UI rung nảy |
| `easeIn` | `t²` | `"power1.in"` | Hoàn toàn nhất quán | Ra cảnh, Đoạn dự bị Anticipation |
| `easeOut` | `1-(1-t)²` | `"power1.out"` | Hoàn toàn nhất quán | Chuyển động nhẹ phần tử phụ (Chữ giải thích fade,...) |
| `easeInOut` | quad inOut | `"power1.inOut"` | Hoàn toàn nhất quán | Chuyển động liên tục (Nội suy quỹ đạo chuột và các chuyển động đối xứng) |
| `linear` | `t` | `"none"` | Hoàn toàn nhất quán | Chỉ dùng cho proxy điều khiển / Máy ảnh di chuyển đều. **CẤM dùng trên hiệu ứng phần tử** |
| `anticipation` | Đường cong phân đoạn, hạ trước -0.3 rồi mới tăng | Không có tương đương dựng sẵn, dùng hàm ease (Xem bên dưới) |  | Vào cảnh có hành động dự bị |

### 1.1 anticipation · Hàm ease

GSAP chấp nhận bất kỳ `(p) => number` nào làm ease, chỉ cần bê nguyên văn định nghĩa tự nghiên cứu sang:

```js
// Nhất quán từng điểm với Easing.anticipation của animations.jsx
const anticipation = (t) => {
  if (t < 0.2) return -0.3 * (t / 0.2) * (t / 0.2);   // 20% đầu: Ngược hướng hạ xuống
  const a = (t - 0.2) / 0.8;
  return -0.012 + 1.012 * a * a * (3 - 2 * a);         // 80% sau: smoothstep tăng lên
};

tl.fromTo("#card", { y: 40 }, { y: 0, duration: 0.7, ease: anticipation }, "s2");
```

Chú ý: Đường cong này sẽ vượt qua 0 (Vùng giá trị âm), **chỉ có thể dùng trên transform** (y / scale / rotation),
đừng dùng trên opacity hoặc màu sắc (Sẽ đẩy ra ngoài phạm vi hợp pháp).

### 1.2 Tùy chọn khác của spring · Lò xo nướng (Spring vật lý thật seek-safe)

`"elastic.out(1, 0.3)"` Là tương đương chính xác của spring tự nghiên cứu, dùng trực tiếp không có vấn đề gì.
Khi bạn muốn cảm giác tay lò xo thật **có thể chỉnh ma sát** (Ví dụ "Về vị trí hầu như không quá chớn, chỉ là đuôi dài"),
dùng giải pháp đóng `springEase` do hyperframes-animation cung cấp (`adapters/gsap-easing-and-stagger.md`
có hiện thực hóa 40 dòng hoàn chỉnh, giải pháp đóng là hàm thuần của thời gian, seek-safe):

```js
// dampingFraction 1.0 = Về vị trí trầm ổn không quá chớn; 0.6-0.7 ≈ Cảm giác rung nảy của spring tự nghiên cứu
const settle = springEase({ response: 0.4, dampingFraction: 0.65 });
tl.fromTo("#hero", { scale: 0 }, { scale: 1,
  duration: settle.duration, ease: settle.ease }, "s4");   // duration bắt buộc dùng cùng, nó là một phần của vật lý
```

**CẤM** đưa vào bất kỳ thư viện lò xo thời gian thực nào (react-spring,...): Trạng thái tích lũy từng khung hình, không thể seek mang tính xác định.

---

## 2 · Cấu trúc Khung 5 đoạn Tự sự · Slow-Fast-Boom-Stop (15/15/40/20/10%)

Tại sao: Hoạt ảnh nhịp điệu đều đặn là trình diễn kỹ thuật, hoạt ảnh có nhịp điệu mới là tự sự (best-practices §1).

Template khung timeline kèm label, sửa `D` là có thể thích ứng với thời lượng bất kỳ:

```js
const D = 15;   // Tổng thời lượng (giây), nhất quán với data-duration của root tổng hợp
const at = (p) => D * p;

const tl = gsap.timeline({
  paused: true,
  defaults: { ease: "expo.out", duration: 0.6 },
});

// ── 5 label đoạn, tỷ lệ 15 / 15 / 40 / 20 / 10 ──────────────────
tl.addLabel("s1_trigger",  at(0));     // Chậm · Kích hoạt: Cho con người thời gian phản ứng, thiết lập cảm giác chân thực
tl.addLabel("s2_generate", at(0.15));  // Vừa · Tạo ra: Điểm kinh ngạc thị giác xuất hiện
tl.addLabel("s3_process",  at(0.30));  // Nhanh · Quá trình: Thể hiện tính khống chế được/Mật độ/Chi tiết
tl.addLabel("s4_boom",     at(0.70));  // Boom · Bùng nổ: Kéo xa/3D pop-out/Trồi lên nhiều panel
tl.addLabel("s5_hold",     at(0.90));  // Tĩnh · Kết thúc: Logo biến hình + Dừng lại đột ngột

// ── S1 Kích hoạt (Nhịp điệu chậm: Hành động đơn lẻ + Khoảng trắng lớn) ─────────────────────
tl.fromTo("#terminal", { y: 48, autoAlpha: 0 },
  { y: 0, autoAlpha: 1, duration: 0.8 }, "s1_trigger+=0.1");

// ── S2 Tạo ra (Một điểm kinh ngạc rõ ràng, không chất đống hành động) ─────────────────────
tl.fromTo("#result-panel", { scale: 0.92, autoAlpha: 0 },
  { scale: 1, autoAlpha: 1, duration: 0.7 }, "s2_generate");

// ── S3 Quá trình (Mật độ cao nhất: stagger, typewriter, chuyển focus đều ở đây) ──
tl.fromTo(".row", { y: 10, autoAlpha: 0 },
  { y: 0, autoAlpha: 1, duration: 0.4, stagger: 0.03 }, "s3_process");

// ── S4 Bùng nổ (Hành động cấp ống kính: Kéo xa / rotationX / Nhiều phần tử trồi lên) ───────
tl.to("#stage", { scale: 0.82, rotationX: 8, duration: 1.2,
  ease: "expo.inOut" }, "s4_boom");

// ── S5 Kết thúc (Logo biến hình thu nạp, xem §3.6; Sau đó không có gì xảy ra) ────────
// Khoảng ~0.5s cuối cùng là sự tĩnh lặng hold có ý đồ: Không thêm bất kỳ tween nào, cũng tuyệt đối không fade to black

window.__timelines["main"] = tl;
```

Các điểm then chốt:

- **Khoảng trắng sau S5**: `data-duration` bao phủ tới cuối cùng, nhưng trên timeline không có tween,
  màn hình hold ở khung hình cuối cùng. Đây chính là cách hiện thực hóa của "dừng lại đột ngột" (Cấm kết thúc kiểu fade out)
- Template 5-scene 22 giây (cinematic-patterns Pattern B) đồng cấu: Thay tỷ lệ thành
  Invoke 3-4s / Process 5-6s / Insight 4-5s / Output 3-4s / Hero 4-5s, label cùng phương pháp
- Chuyển đổi toàn màn hình giữa các scene dùng autoAlpha chồng chéo + dịch chuyển, không dùng chuyển đổi display
  (`display` / `visibility` trần là vùng cấm của renderer, show/hide toàn bộ dùng `autoAlpha`)

---

## 3 · Ngôn ngữ Chuyển động 8 Điều · Dịch từng điều một

### 3.1 Nền không dùng đen thuần/trắng thuần

Quy tắc phi-timeline: Nền là CSS tĩnh, màu trung tính mang nhiệt độ màu, mã màu cụ thể đi theo brand spec.
Mối liên hệ GSAP duy nhất: Khi cần đổi màu nền giữa các scene, tween `backgroundColor` (Nằm trong danh sách cho phép),
và màu nền của 2 scene nên cùng hệ màu (Ràng buộc nhất quán màu sắc trong cinematic-patterns §2):

```js
tl.to("#stage", { backgroundColor: "#F4EFE6", duration: 0.8, ease: "sine.inOut" }, "s4_boom");
```

### 3.2 Easing tuyệt đối không dùng linear

Tại sao: `linear` Làm phần tử kỹ thuật số giống như máy móc, `expoOut` Trao cảm giác trọng lượng vật lý (best-practices §2).

Hiện thực hóa: Timeline `defaults` Viết `ease: "expo.out"` (Xem mẫu §0),
từng tween riêng lẻ đè lên theo bảng ánh xạ §1. `ease: "none"` Chỉ cho phép xuất hiện ở 2 nơi:
Proxy điều khiển tween (§7) và chuyển động máy móc có ý đồ (Máy ảnh di chuyển đều pan).

### 3.3 Slow-Fast-Boom-Stop

Xem khung §2, không lặp lại.

### 3.4 Hiển thị "Quá trình" chứ không phải "Kết quả phép thuật"

Tại sao: Sản phẩm là người cộng tác không phải ảo thuật gia, hiển thị tweak / sửa lỗi / redline đánh vào AI slop "phép thuật một bấm"
(best-practices §3.4).

Hai công thức "Cảm giác quá trình" thường dùng nhất:

**Chunk Reveal (Mô phỏng dòng token xuất hiện)**. Công thức gốc dùng `setTimeout + Math.random`,
cả hai dưới dạng render seek đều phi pháp. Dịch thành "Thời gian biểu tính toán trước + proxy điều khiển", hai hướng seek an toàn:

```js
// Tại sao không dùng tl.call(): Callbacks không thể đảo ngược, trong preview kéo ngược lại sẽ bị dính trạng thái
const rand = mulberry32(42);                              // Ngẫu nhiên hạt giống, xem §7.4
const text = "Đã tạo ra 3 phương án ứng viên cho bạn, phương án đầu tiên táo bạo nhất.";
const chunks = text.split(/(?=[，。、；])|(?<=[，。、；])/); // Tiếng Trung/Việt cắt chunk theo dấu câu
const times = []; let acc = 0;
chunks.forEach(() => { acc += 0.04 + rand() * 0.08; times.push(acc); }); // Không đều 40-120ms

const tw = { t: 0 };
tl.to(tw, {
  t: acc, duration: acc, ease: "none",
  onUpdate: () => {   // Mỗi khung hình từ t tính lại văn bản hiển thị hoàn chỉnh: Hàm thuần, kéo ngược lại vẫn đúng
    let n = 0;
    while (n < times.length && times[n] <= tw.t) n++;
    document.querySelector("#stream").textContent = chunks.slice(0, n).join("");
  },
}, "s2_generate+=0.3");
```

**Bộ đếm số counter (Hiển thị dữ liệu thực tế đang tăng)**:

```js
// snap Đảm bảo số nguyên; innerText Là cách viết counter được HyperFrames công nhận
tl.fromTo("#metric", { innerText: 0 },
  { innerText: 237, snap: { innerText: 1 }, duration: 1.2, ease: "expo.out" }, "s3_process");
```

Kèm phân cách phần ngàn / format hậu tố thì đổi sang dùng proxy + onUpdate (`tw.v` Suy ra `toLocaleString`), cách làm tương tự như trên.

### 3.5 Quỹ đạo con trỏ chuột · Đường cong + Rung tay

Tại sao: Con trỏ chuột nội suy đường thẳng có cảm giác máy móc trong tiềm thức, người thật là "Tăng tốc, Đường cong, Giảm tốc sửa hướng"
(best-practices §3.5).

Nội suy đường cong Bezier không thể biểu đạt bằng tween thuộc tính thông thường, dùng proxy điều khiển. Rung tay không dùng Perlin
(Hiện thực hóa gốc phụ thuộc vào nhiễu thời gian thực), dùng 2 đường hình sin tần số không thể cộng hưởng đè lên nhau, tương đương mang tính xác định:

```js
const mouse = { p: 0 };
const P0 = [100, 100];                       // Điểm đầu
const P2 = [tx, ty];                          // Điểm cuối (Mục tiêu nhấp)
const P1 = [tx - 200, ty + 80];               // Điểm điều khiển: Lệch trung điểm, tạo đường cong

tl.to(mouse, {
  p: 1, duration: 1.1, ease: "power1.inOut",  // Easing đối xứng: Khởi động tăng tốc + Đến nơi giảm tốc
  onUpdate: () => {
    const t = mouse.p;
    let x = (1-t)*(1-t)*P0[0] + 2*(1-t)*t*P1[0] + t*t*P2[0];
    let y = (1-t)*(1-t)*P0[1] + 2*(1-t)*t*P1[1] + t*t*P2[1];
    x += Math.sin(t * 47.13) * 2 * (1 - t);   // ±2px Rung tay, thu hẹp khi lại gần mục tiêu
    y += Math.sin(t * 33.7 + 1.3) * 2 * (1 - t);
    gsap.set("#cursor", { x, y });            // Tất cả do p suy ra, seek-safe
  },
}, "s1_trigger+=0.5");

// Phản hồi nhấp: Anticipation Thu nhỏ lại rồi mới hồi đàn
tl.to("#cursor", { scale: 0.85, duration: 0.08, ease: "power1.in" }, ">");
tl.to("#cursor", { scale: 1, duration: 0.25, ease: "back.out" }, ">");
```

### 3.6 Logo Biến hình thu nạp (Morph)

Tại sao: Logo fade-in không có cảm giác thu nạp tự sự, phải để phần tử thị giác trước đó "sụp đổ" rồi mới "phình ra" thành Logo,
làm cho tự sự sụp đổ trên điểm thương hiệu (best-practices §3.6).

blur đi theo biến CSS (`filter` Là paint-only, seek-safe, cách làm được quy tắc depth-of-field-blur chính thức công nhận):

```css
#lastVisual, #logo { --blur: 0px; filter: blur(var(--blur)); will-change: filter; }
```

```js
tl.addLabel("morph", "s5_hold-=0.3");

// Sụp đổ: Phần tử thị giác trước thu thành ô màu, motion blur nổi lên
tl.to("#lastVisual", { scale: 0.1, "--blur": "6px",
  duration: 0.5, ease: "expo.out" }, "morph");

// Phình ra: Logo từ tâm ô màu nẩy ra, blur thu hẹp về sắc nét
tl.fromTo("#logo",
  { scale: 0.1, "--blur": "6px", autoAlpha: 0 },
  { scale: 1, "--blur": "0px", autoAlpha: 1, duration: 0.6, ease: "back.out" },
  "morph+=0.35");                              // Chồng chéo mức 150ms = Chuyển nhanh

tl.to("#lastVisual", { autoAlpha: 0, duration: 0.15 }, "morph+=0.5");
// Sau đó: hold, không tween, dừng lại đột ngột
```

### 3.7 Phông chữ kép Serif + Sans-serif

Quy tắc phi-timeline: CSS tĩnh, lựa chọn phông chữ đi theo brand spec.
Trình biên dịch HyperFrames sẽ tự động bắt Google Fonts và inject @font-face mang tính xác định
(Thực tế Phase 0, bẫy chuỗi thời gian phông chữ của ống dẫn tự nghiên cứu không tồn tại trong backend mới), trong CSS引 Google Fonts bình thường là được.

### 3.8 Chuyển đổi tiêu điểm = Nền giảm + Tiền cảnh sắc nét + Flash dẫn dắt

Tại sao: Chỉ giảm opacity thì phần tử ngoài tiêu điểm vẫn sắc nét, bắt buộc phải thêm blur mới thực sự lùi về hậu cảnh
(best-practices §3.8).

Bộ 3 filter toàn bộ đi theo biến CSS, GSAP tween bản thân biến đó:

```css
.tile {
  --f: 0;   /* focusIntensity 0→1 */
  filter: brightness(calc(1 - 0.5 * var(--f)))
          saturate(calc(1 - 0.3 * var(--f)))
          blur(calc(var(--f) * 4px));          /* ← Phím chốt: blur làm cho vùng ngoài tiêu điểm thực sự lùi sau */
  will-change: filter;
}
```

```js
tl.addLabel("focus", "s3_process+=1.5");

// Phần tử ngoài tiêu điểm: 3 filter + dim hoàn thành trong 1 lần tween
tl.to(".tile:not(.focus-target)", {
  "--f": 1, opacity: 0.4, duration: 0.5, ease: "expo.out",
}, "focus");

// Flash highlight dẫn dắt ánh mắt quay lại.
// Chú ý: Công thức gốc dùng element.animate() (WAAPI), cái đó đi theo giờ đồng hồ tường, dưới seek không xác định, bắt buộc dịch thành tween
tl.fromTo("#focusFlash",
  { backgroundColor: "rgba(255,255,255,0.3)" },
  { backgroundColor: "rgba(255,255,255,0)", duration: 0.15, ease: "power1.out" },
  "focus+=0.5");

// Giải phóng tiêu điểm: settle sharp. Trước khi giao cho scene tiếp theo bắt buộc phải thu blur về 0,
// Dừng ở trạng thái nửa mờ sẽ bị khán giả đọc thành "Render bị lỗi rồi"
tl.to(".tile", { "--f": 0, opacity: 1, duration: 0.5, ease: "power2.inOut" }, "focus+=2.5");
```

Ràng buộc hiệu năng (Đến từ quy tắc DoF chính thức): Bán kính blur trên phần tử diện tích lớn ≤24px; Ưu tiên "dim + blur vừa phải"
chứ không phải kéo blur kịch trần; `will-change: filter` Chỉ thêm trên phần tử thực sự động blur.

---

## 4 · Kỹ thuật Chuyển động Cụ thể · Bản GSAP của các đoạn code §4

### 4.1 FLIP / Shared Element (Nút bấm phình thành ô nhập liệu)

Tại sao: Cùng một phần tử quá độ giữa 2 trạng thái, không phải 2 phần tử cross-fade (best-practices §4.1).

Công thức gốc dùng layoutId của Framer Motion, phía GSAP không đưa vào plugin Flip (Chưa xác thực dưới HyperFrames),
tự tính thủ công: Canvas tổng hợp là cố định (data-width/height), hình học của 2 trạng thái đều là hằng số thiết kế bản vẽ,
dùng fromTo viết cứng là được. Dịch chuyển co giãn toàn bộ đi theo transform, phần tử giữ ở vị trí bố cục cuối cùng:

```css
/* Phần tử bố cục ở "trạng thái cuối", trạng thái đầu do transform biểu đạt */
#search-box { width: 560px; height: 56px; }   /* Trạng thái cuối tĩnh, không tween kích thước */
```

```js
// Hình học trạng thái đầu: Nút bấm 120x44 tại (400, 300), trạng thái cuối ô nhập 560x56 tại (200, 300)
tl.fromTo("#search-box",
  { x: 200, y: 0, scaleX: 120/560, scaleY: 44/56, transformOrigin: "left top" },
  { x: 0,   y: 0, scaleX: 1, scaleY: 1, duration: 0.6, ease: "expo.out" },
  "s2_generate");
// Chữ tầng trong bù ngược hướng hoặc vào cảnh trễ hơn, tránh bị scaleX kéo giãn (Xử lý giống §4.2)
tl.fromTo("#search-box .placeholder", { autoAlpha: 0 },
  { autoAlpha: 1, duration: 0.3 }, "s2_generate+=0.4");
```

### 4.2 Mở rộng kiểu nhịp thở (Mở rộng trước, bơm nước sau)

Tại sao: Panel không nên kéo đồng thời width và height, mở rộng chiều ngang trước rồi mới chống chiều dọc mới giống thế giới vật lý
(best-practices §4.2).

Công thức gốc tween trực tiếp width/height, cái này trong HyperFrames là vùng cấm reflow (Snap pixel số nguyên,
đoạn tốc độ chậm nhìn thấy giật bằng mắt thường, §7.2). Dịch thành scaleX/scaleY, thời gian lệch giữ nguyên:

```js
// L = Tổng thời lượng mở rộng; 40% thời gian đầu kéo ngang, 30% bắt đầu chống dọc, 2 đoạn chồng chéo
const L = 0.9;
tl.fromTo("#panel",
  { scaleX: 0, scaleY: 0.12, transformOrigin: "left top" },
  { scaleX: 1, duration: 0.4 * L, ease: "expo.out" }, "open");
tl.to("#panel", { scaleY: 1, duration: 0.7 * L, ease: "expo.out" }, "open+=" + 0.3 * L);

// Nội dung sau khi vỏ mở rộng xong mới nổi lên: Vừa phù hợp意象 "Mở rộng trước bơm nước sau",
// Vừa làm cho sự biến dạng kéo giãn nội dung trong quá trình scale không nhìn thấy được
tl.fromTo("#panel .content", { autoAlpha: 0, y: 8 },
  { autoAlpha: 1, y: 0, duration: 0.35 }, "open+=" + 0.75 * L);
```

Chú ý bản scale không phải từng pixel trung thực (Bo góc và đường viền sẽ biến dạng theo tỷ lệ). Khi vỏ mở rộng là màu thuần / panel bo góc lớn
thì không nhận thấy được; Nếu chi tiết đường viền panel quan trọng, đổi sang dùng phương án "Vỏ cố định + Nội dung hé lộ clip-path" và test chụp màn hình thực tế.

### 4.3 Staggered Fade-up (stagger 30ms)

Tại sao: Danh sách từng cái vào cảnh có "cảm giác vật thể" hơn xuất hiện cả mảng, 30ms là khoảng cách đã định (best-practices §4.3).

```js
tl.fromTo(".row",
  { y: 10, autoAlpha: 0 },
  { y: 0, autoAlpha: 1, duration: 0.4, ease: "expo.out", stagger: 0.03 },
  "s3_process");

// Biến thể: Trồi lên từ trung tâm ra 2 bên (Thường dùng khi trồi nhiều panel của S4 bùng nổ)
tl.fromTo(".panel",
  { y: 24, autoAlpha: 0, scale: 0.96 },
  { y: 0, autoAlpha: 1, scale: 1, duration: 0.5, ease: "expo.out",
    stagger: { each: 0.03, from: "center" } },
  "s4_boom");
```

Dùng `fromTo` Không dùng `from`: sub-composition sẽ bị re-seek lặp đi lặp lại, `from` Tại khoảnh khắc đăng ký chụp nhanh trạng thái bắt đầu,
khi kéo ngược lại có thể bị lệch vị trí; `fromTo` Khai báo rõ ràng 2 đầu, vĩnh viễn nhất quán.

### 4.4 Tạm dừng 0.5s trước kết quả then chốt

Tại sao: Máy móc thực thi nhanh và liên tục, nhưng não người cần thời gian phản ứng, dừng 0.5 giây trước kết quả then chốt là nhường bước cho khán giả
(best-practices §4.4, §0.2 Niềm tin cốt lõi điều 3).

Trong GSAP "Tạm dừng" chính là một khoảng trống trên tham số position, dùng label viết sự tạm dừng thành quyết định thiết kế rõ ràng:

```js
// Khoảnh khắc tạo xong
tl.addLabel("generated", "s2_generate+=1.2");
// Trạng thái loading dừng 0.5s: Trong 0.5s này không có bất kỳ tween nào, khán giả nhìn trạng thái loading
tl.addLabel("reveal", "generated+=0.5");

tl.fromTo("#result", { scale: 0.94, autoAlpha: 0 },
  { scale: 1, autoAlpha: 1, duration: 0.7, ease: "expo.out" }, "reveal");
```

### 4.5 Anticipation → Action → Follow-through

Tại sao: Hoạt ảnh chỉ có Action là hoạt ảnh PowerPoint, 3 đoạn của Disney trao sự sống cho hành động
(best-practices §4.6).

3 đoạn tween theo thứ tự, easing theo ánh xạ §1 (Dự bị power1.in, Chính expo.out, Hồi đàn elastic):

```js
tl.addLabel("pop", "s2_generate+=0.2");
tl.to("#card", { scale: 0.95, duration: 0.12, ease: "power1.in"  }, "pop");        // Dự bị
tl.to("#card", { scale: 1.05, duration: 0.30, ease: "expo.out"   }, ">");          // Chính
tl.to("#card", { scale: 1.00, duration: 0.35, ease: "elastic.out(1, 0.3)" }, ">"); // Hồi đàn
```

Bản tween đơn: `ease: anticipation` (§1.1) Một bước hoàn thành "Dự bị + Chính", hồi đàn bổ sung một đoạn.

### 4.6 3D Perspective + translateZ Phân lớp

Tại sao: rotateX 8° / rotateY -4° Mô phỏng góc nhìn tự nhiên natural angle máy ảnh nhìn xuống ở góc trên bên trái mặt bàn
(best-practices §4.7).

Thấu thị và phân lớp là CSS tĩnh (Chép nguyên công thức gốc, perspective / translateZ không cần động);
Phần động (Đứng dậy khi vào cảnh, kéo xa S4) dùng alias 3D transform của GSAP:

```css
.stage-wrap { perspective: 2400px; perspective-origin: 50% 30%; }
.card-grid  { transform-style: preserve-3d; }
.card:nth-child(3n) { transform: translateZ(30px); }
.card:nth-child(5n) { transform: translateZ(-20px); }
.card:nth-child(7n) { transform: translateZ(60px); }
```

```js
// Vào cảnh: Từ nhìn thẳng chậm rãi đứng lên góc vàng
tl.fromTo("#card-grid", { rotationX: 0, rotationY: 0 },
  { rotationX: 8, rotationY: -4, duration: 1.4, ease: "expo.out" }, "s2_generate");
```

### 4.7 Pan nghiêng · Động đồng thời XY, Tần số khác nhau

Tại sao: X và Y dùng tần số khác nhau tránh lặp lại Lissajous bị quy luật hóa, mô phỏng sự trôi nghiêng của máy ảnh cầm tay
(best-practices §4.8).

Công thức gốc là `Math.sin(flowT * ...)` Tính từng khung hình, bản GSAP dùng 2 tween yoyo duration khác nhau đè lên nhau
(GSAP theo dõi x / y độc lập, 2 tween không đánh nhau). repeat Bắt buộc phải hữu hạn:

```js
// Chu kỳ khác nhau (4.6s vs 2.9s) = Tần số khác nhau, đường đi không khép kín
// Số repeat tính từ thời lượng nhìn thấy được: Math.ceil(D / dur) Đảm bảo bao phủ toàn phim
tl.to("#stage", { x: 40, duration: 4.6, ease: "sine.inOut",
  yoyo: true, repeat: Math.ceil(D / 4.6) }, 0);
tl.to("#stage", { y: 30, duration: 2.9, ease: "sine.inOut",
  yoyo: true, repeat: Math.ceil(D / 2.9) }, 0);
```

### 4.8 Kết thúc Dừng lại đột ngột

Tại sao: fade out Không có cảm giác quyết định, khung hình cuối cùng phải rõ ràng, khẳng định (best-practices §0.3 Khoảng trắng).

Hiện thực hóa là "Không viết code": Sau khi Logo của S5 về vị trí, trên timeline không còn bất kỳ tween nào nữa,
`data-duration` Dài hơn thời điểm kết thúc của tween cuối cùng 0.5-1s, màn hình hold ở trạng thái cuối.
Nếu có BGM, dùng volume tween ở đuôi để thu âm (volume Nằm trong danh sách cho phép):

```js
tl.to("#bgm", { volume: 0, duration: 0.4 }, "s5_hold+=0.8");  // Âm thanh cắt dừng, màn hình không động
```

---

## 5 · Công thức Kịch bản A/B/C · Các Điểm Then Chốt Cấu Trúc Timeline

Nhận định thiết kế (Chọn loại nào, Mật độ SFX, Phong cách BGM) xem best-practices §5, ở đây chỉ đưa ra sự khác biệt phía timeline.

### Công thức A · Kịch tính kiểu Apple Keynote

- Khung xương: §2 Khung xương 5 đoạn giữ nguyên bản gốc, Boom của S4 làm cho tới
- defaults: `ease: "expo.out"`, Nhấn mạnh tương tác đè lên `"back.out"`
- Hành động biểu tượng S4: Ống kính kéo xa gấp + drop. `tl.to("#stage", { scale: 0.78, y: -40, duration: 1.1, ease: "expo.inOut" }, "s4_boom")`
- S5: Logo Morph (§3.6) + Âm đơn không linh + hold

### Công thức B · Một máy đến cùng kiểu công cụ

- Khung xương: **Không dùng** cấu trúc đỉnh 5 đoạn, một đường flow liên tục. label Đánh theo tiểu tiết BGM:
  `tl.addLabel("bar1", 0); tl.addLabel("bar2", 60/88*4);` (88 BPM, một tiểu tiết ≈ 2.73s)
- Tham số position của các hành động UI then chốt viết trực tiếp trên khoảnh khắc kick/snare, nhịp điệu âm nhạc chính là âm hiệu tương tác
- easing: `springEase` (§1.2) + `"expo.out"`, Cảm giác về vị trí nhiều hơn cảm giác bùng nổ
- Không có Boom kiểu S4, kết thúc cũng dừng lại đột ngột

### Công thức C · Tự sự hiệu suất văn phòng

- Khung xương: Cắt cứng nhiều scene. Mỗi scene một label, giữa scene autoAlpha chuyển nhanh (0.15s)
  Chứ không phải chồng chéo dài; Phối hợp với Dolly In/Out:
  `tl.fromTo("#scene2", { scale: 1.06 }, { scale: 1, duration: 1.2, ease: "expo.out" }, "sc2")`
- Tương tác loại toggle toàn bộ `"back.out"`, panel toàn bộ `"expo.out"`
- Toàn phim nhất định có một chỗ điểm sáng: 3D pop-out (§4.6 rotationX + translateZ Phần tử nổi lên),
  Chỉ làm một lần, khoe kỹ xảo khắp nơi là tín hiệu rẻ tiền (§0.3 Kiềm chế)

---

## 6 · Quy tắc An toàn Seek (Thực tế Phase 0, Đạp qua toàn bộ)

Render HyperFrames là seek từng khung hình + Chụp màn hình. Bất kỳ trạng thái nào không phải "Hàm thuần của thời gian" đều sẽ xuất hiện
kết quả không xác định khi render, hơn nữa **trong preview nhìn thường là tốt**, chỉ có sản phẩm render mới lộ ra.

### 6.1 Cấm CSS transition + chuyển class · Toàn bộ biểu đạt bằng tween

CSS `transition` Đi theo giờ đồng hồ tường của trình duyệt, không đi theo trục thời gian. Khi seek từng khung hình mỗi khung hình đều là một "Đột biến trạng thái",
transition Hoặc là không kích hoạt, hoặc là điểm bắt đầu hỗn loạn, thực tế di chuyển c3 ở Phase 0 đạp bẫy.

```css
/* ✗ Cách viết cũ: Trong JS classList.add('lit'), dựa vào transition để quá độ */
.capsule { transition: transform 0.3s ease; }
.capsule.lit { transform: scale(1.06); }
```

```js
/* ✓ Cách viết mới: Bản thân thay đổi trạng thái là một đoạn tween trên timeline */
tl.to("#capsule", { scale: 1.06, duration: 0.3, ease: "expo.out" }, "lit_at");
tl.to("#capsule", { scale: 1.0,  duration: 0.3, ease: "expo.out" }, "lit_at+=1.2");
```

Vùng cấm cùng loại: `element.animate()` (WAAPI, Cũng đi theo đồng hồ tường, Flash §3.8 Đã có bản dịch)、
CSS `@keyframes` animation Dùng cho hoạt ảnh render then chốt.
Quét một lượt trước khi giao hàng: `grep -n "transition:\|animation:\|\.animate(" index.html`,
Mỗi chỗ trúng phải hoặc là xóa đi, hoặc là dịch thành tween.

### 6.2 Cấm animate thuộc tính kích hoạt reflow · Dùng transform thay thế

Thuộc tính layout trong giai đoạn layout của trình duyệt snap về pixel thiết bị số nguyên. Tween tốc độ nhanh không nhìn ra được;
Tween tốc độ chậm đoạn cuối ease-out mỗi khung hình di chuyển không đủ 1px, sẽ bị "Nín vài khung hình, nhảy 1px", mắt thường nhìn thấy giật.
Lint Phase 0 bắt tại trận letterSpacing giật từng khung hình, chính là loại visual bug không cảnh báo này.

| ✗ Cấm tween | ✓ Thay thế trung thực |
|---|---|
| `width` / `height` | `scaleX` / `scaleY` + `transformOrigin` (Xử lý nội dung xem §4.2) |
| `top` / `left` / `right` / `bottom` | Phần tử dừng ở vị trí trạng thái cuối CSS, tween độ lệch `x` / `y` |
| `fontSize` | `scale` (Tương đương thị giác, mượt sub-pixel) |
| `letterSpacing` / `wordSpacing` | Split từng chữ rồi tween `x` Của từng ký tự (uniform scale Không phải cùng một hiệu ứng, nó co giãn hình chữ chứ không phải khoảng cách chữ) |
| `margin*` / `padding*` | Bố cục viết cứng, động `x` / `y` |

Nguyên tắc sửa chữa: **Tái hiện cùng một thị giác, chỉ xóa đi sự rung giật**. Qua lint không phải tiêu chuẩn, so sánh từng khung hình với hoạt ảnh gốc mới phải.

### 6.3 t=0 onUpdate không kích hoạt · Tween proxy bắt buộc bổ thủ công khung hình đầu

Timeline seek về 0 proxy tween `onUpdate` Có thể không kích hoạt, khung hình đầu chính là màn hình trắng / DOM ban đầu.
Tất cả kịch bản do proxy dẫn dắt (§3.4 chunk reveal, §3.5 Con trỏ, §7 Adapter demo cũ),
Sau khi đăng ký xong timeline gọi thủ công một lần:

```js
window.__timelines["main"] = tl;
render(0);   // Bảo hiểm khung hình đầu: Hiển thị rõ ràng bức tranh t=0
```

### 6.4 Cấm Math.random / Date.now · Ngẫu nhiên dùng hàm hạt giống

Cùng một khung hình mỗi lần seek bắt buộc nhận được cùng một bức tranh. Ngẫu nhiên lúc runtime = Mỗi lần render khác nhau = Không thể render từng khung hình.
Khi cần "Cảm giác ngẫu nhiên" (Hạt, Rung lắc, Khoảng cách không đều) dùng mulberry32, **Trước khi dựng timeline**
Tạo tất cả giá trị ngẫu nhiên trong 1 lần (Cách viết thực tế trong demo hạt 3D Phase 0):

```js
function mulberry32(seed) {
  return function () {
    seed |= 0; seed = (seed + 0x6d2b79f5) | 0;
    let t = Math.imul(seed ^ (seed >>> 15), 1 | seed);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}
const rand = mulberry32(20260717);   // Hạt giống viết cứng, đổi hạt giống = Đổi 1 bản ngẫu nhiên

// Cách dùng: Tạo trước, không rút tại chỗ trong onUpdate
const offsets = Array.from({ length: 40 }, () => (rand() - 0.5) * 24);
```

Cấm dùng tương tự: `Date.now()`, `performance.now()`, Bất kỳ trạng thái nào do sự kiện dẫn dắt (Chế độ render không có sự kiện đầu vào).

---

## 7 · Công thức Adapter Demo Cũ · render(t) Treo vào GSAP

Cốt lõi hoạt ảnh của 21 demo cũ engine tự nghiên cứu đều là hàm thuần `render(t)`. Di chuyển không viết lại lô-gích hoạt ảnh,
Dùng một tween proxy treo render(t) lên GSAP timeline (Thực tế Phase 0: Một demo
20-30 phút, code hoạt ảnh một dòng không đổi, c3 demo cấp điện ảnh 1134 dòng xác thực thông qua).

### 7.1 Template tween proxy (12 Dòng, Bản gốc thực tế c3)

```js
// =============== HyperFrames adapter ===============
// Tween proxy dẫn dắt render(t) gốc. Mỗi khung hình đều là hàm thuần của thời gian trục thời gian:
// Không rAF, Không đồng hồ, Không trạng thái đầu vào.
window.__timelines = window.__timelines || {};
const proxy = { t: 0 };
const tl = gsap.timeline({ paused: true });
tl.to(proxy, {
  t: T.DURATION,            // Hằng số tổng thời lượng của demo cũ
  duration: T.DURATION,
  ease: "none",             // Thời gian bắt buộc ánh xạ đều, easing ở bên trong render(t)
  onUpdate: () => render(proxy.t),
}, 0);
window.__timelines["main"] = tl;

// Bảo hiểm khung hình đầu (Timeline dừng tại t=0 onUpdate không kích hoạt, §6.3)
render(0);
```

### 7.2 Bốn bước di chuyển

1. **Bọc root / clip**: Thêm thuộc tính root tổng hợp cho container ngoài cùng
   (`data-composition-id="main"` + `data-duration` + Kích thước),
   Phần tử sân khấu thêm `.clip` Và `data-start` / `data-duration` / `data-track-index`.
   Hợp đồng đầy đủ xem `hyperframes-backend.md`
2. **Xóa tự dẫn dắt**: Xóa vòng lặp rAF, `setInterval`, Lô-gích tự động play,
   Điểm bắt đầu `performance.now()`. `render(t)` Chỉ ăn tham số t, không còn tự tìm thời gian nữa
3. **Treo proxy**: Dán template §7.1, `T.DURATION` Khớp với `data-duration`, Cuối cùng `render(0)`
4. **Quét transition**: `grep -n "transition:\|animation:\|\.animate(\|Math.random\|Date.now\|performance.now"`
   Từng dòng về 0. Các hiệu ứng kiểu chuyển class đổi thành hàm thuần của t theo §6.1 (Tàn dư thường gặp nhất của demo cũ
   Chính là kết hợp "classList.add + transition")

Di chuyển xong chạy 1 lần `npx hyperframes check` (Phim tối màu dùng `--no-contrast`,
4 cổng còn lại bắt buộc 0 error), rồi rút 3-4 khoảnh khắc then chốt chụp màn hình so sánh với bản cũ.

### 7.3 Khi nào không dùng adapter

Adapter là phương án **di chuyển mã nguồn cũ**. Hoạt ảnh viết mới trực tiếp dùng cách viết timeline nguyên bản §0-§5 của tệp này:
label Đọc được, stagger Mang tính khai báo, GSAP inspector Có thể kiểm tra từng tween,
Hoạt ảnh trong hộp đen proxy không trong suốt đối với công cụ kiểm toán.

---

## 8 · Tự kiểm tra trước khi giao hàng (Phía GSAP, bổ sung danh sách best-practices §7)

- [ ] timeline `paused: true`, Đăng ký key bằng `data-composition-id`?
- [ ] defaults Là `expo.out`, Không có `linear` / `ease` Trọc xuất hiện trên hiệu ứng phần tử?
- [ ] 5 label đoạn đầy đủ, Sau S5 Có hold khoảng trắng (Không có fade out)?
- [ ] Kết quả `grep "transition:\|\.animate(\|Math.random\|Date.now"` Bằng 0?
- [ ] Không có tween width / height / top / left / letterSpacing / fontSize?
- [ ] Tất cả `repeat` Là con số hữu hạn?
- [ ] Kịch bản proxy cuối cùng có bổ sung `render(0)`?
- [ ] blur / filter Toàn bộ đi theo biến CSS, Phần tử có động blur Có `will-change: filter`?
- [ ] Vào cảnh trong sub-composition toàn bộ dùng `fromTo` Không dùng `from`?
- [ ] `npx hyperframes check` Thông qua (Phim tối màu `--no-contrast`, Còn lại 0 error)?

---

## 9 · Công thức Camera Rig · Tầng Hiện thực hóa Chuyển động Ống kính

Tại sao: Chuyển động ống kính và hoạt ảnh phần tử tranh giành cùng một transform là gốc rễ kỹ thuật của sự hỗn loạn quay phim
(camera-language.md §3). Tất cả tween cấp ống kính thu về rig container chuyên trách,
Trạng thái máy ảnh dùng một đối tượng proxy gánh vác, mỗi khung hình do nó suy ra toàn bộ trạng thái DOM máy ảnh, seek-safe.

### 9.1 Cấu trúc rig container (Khung tĩnh)

```html
<div id="viewport">                <!-- Viewport cố định -->
  <div id="camera">                <!-- Tầng ống kính: Chỉ có transform máy ảnh -->
    <div id="world">...</div>      <!-- Tầng thế giới: Hoạt ảnh phần tử chỉ xảy ra ở bên trong này -->
  </div>
  <div id="hud">...</div>          <!-- Phụ đề/Góc nhãn: Anh em của #camera, tự nhiên tĩnh lặng -->
</div>
```

```css
#viewport { position: relative; width: 1920px; height: 1080px; overflow: hidden; }
#camera   { position: absolute; inset: 0; perspective-origin: 960px 540px; }
#world    { position: absolute; transform-origin: 0 0; will-change: transform; }
/* Bảo hiểm pan lộ mép: Kích thước #world ≥ Viewport + Biên độ pan tối đa + 8% lề (camera-language §3.3) */
```

### 9.2 Proxy Máy ảnh + Dịch Khung hình khóa PageCam

Máy ảnh là một đối tượng thông thường, GSAP tween các trường của nó, `onUpdate` Viết trạng thái vào DOM.
Mọi thứ do cam suy ra, kéo ngược lại vẫn đúng (Tương tự tư ý proxy chunk reveal §3.4):

```js
const cam = { cx: 960, cy: 540, zoom: 1, rotX: 0, rotY: 0, rotZ: 0, persp: 1200 };
const camEl = document.querySelector("#camera");
const world = document.querySelector("#world");

// ── Chế độ mặt phẳng (Thuần zoom + pan, không xoay) ──────────────────────────
function applyCam() {
  world.style.transform =
    `translate(${960 - cam.cx * cam.zoom}px, ${540 - cam.cy * cam.zoom}px) scale(${cam.zoom})`;
  applyCounter();
}

// ── Chế độ 3D (Có rotX/rotY/rotZ) · Phóng đại đi theo thuộc tính CSS zoom, không đi theo scale ──
// Co giãn cấp bố cục làm Chromium rasterize theo kích thước sau phóng đại, gốc rễ chữa chữ 3D bị mờ
// (camera-language §3.4, Tri thức đắt nhất toàn thư viện). zoom Thay đổi hệ tọa độ, translate phải chia cho zoom.
function applyCam3d() {
  camEl.style.perspective = `${cam.persp * cam.zoom}px`;
  world.style.zoom = cam.zoom;
  world.style.transformOrigin = `${cam.cx}px ${cam.cy}px`;
  world.style.transform =
    `translate(${960 / cam.zoom - cam.cx}px, ${540 / cam.zoom - cam.cy}px)` +
    ` rotateY(${cam.rotY}deg) rotateX(${cam.rotX}deg) rotateZ(${cam.rotZ}deg)`;
  applyCounter();
}
```

Chú ý: CSS `zoom` Mỗi khung hình kích hoạt re-layout, là **ngoại lệ hợp pháp duy nhất** Của lệnh cấm reflow §6.2,
Chỉ cho phép dùng trên tầng máy ảnh `#world`. Dưới dạng render từng khung hình offline của HyperFrames / Playwright thời gian đơn khung hình không ảnh hưởng sản phẩm;
Preview thời gian thực rớt khung hình là bình thường, lấy sản phẩm render làm chuẩn.

### 9.3 Helper thời lượng logarit (Thời lượng cố định là nguồn gốc cảm giác nghiệp dư)

```js
// camera-language §4.2: 1→2x Vừa đúng 0.55s, Tốc độ thị giác zoom ở biên độ bất kỳ là nhất quán
function zoomDur(z1, z2) {
  return gsap.utils.clamp(0.30, 0.94,
    0.55 * Math.abs(Math.log(z2 / z1)) / Math.LN2);
}
```

### 9.4 Cách viết đoạn ống kính (Đẩy tới → hold → Bình移 → Tạ mạc kéo ra)

Tween ống kính toàn bộ điều khiển cam, easing theo camera-language §4.1:
Chủ động đẩy kéo `power3.inOut`, Dạng đi theo `cubic-bezier(0.33,0,0.15,1)` (Ease tùy biến xem bên dưới).

```js
const followEase = gsap.parseEase("0.33,0,0.15,1");   // Mặc định máy ảnh shotcraft

// Đẩy nhẹ định cảnh: Mở máy tức là 1.06x, 3s mượt ra lùi về toàn cảnh (Độ dài phim >14s và cảnh đầu >7s mới thêm)
tl.fromTo(cam, { zoom: 1.06 },
  { zoom: 1, duration: 3.0, ease: "power2.out", onUpdate: applyCam }, 0);

// Đẩy tới đặc tả: Điểm mục tiêu (1240, 430), 1 → 1.8x, Thời lượng do công thức đưa ra
tl.to(cam, { cx: 1240, cy: 430, zoom: 1.8,
  duration: zoomDur(1, 1.8), ease: "power3.inOut", onUpdate: applyCam },
  "s2_generate");
// Ống kính đến vị trí hold ≥1.2s rồi mới đi (Không viết tween chính là hold)

// Dịch chuyển tiêu điểm tầm trung: Không quay về 1x, trực tiếp bình移 qua (Ngữ pháp giữa các cảnh: 0.22-0.45 đổi bình移)
tl.to(cam, { cx: 880, cy: 620,
  duration: 0.7, ease: followEase, onUpdate: applyCam }, "s3_process+=1.5");

// Tạ mạc: 0.55s Kéo ra + ≥0.8s Tạm dừng toàn cảnh, data-duration bao phủ tới cuối đoạn tạm dừng
tl.to(cam, { cx: 960, cy: 540, zoom: 1,
  duration: 0.55, ease: "power3.inOut", onUpdate: applyCam }, "s5_hold");

window.__timelines["main"] = tl;
applyCam();   // Bảo hiểm khung hình đầu: Timeline dừng tại t=0 onUpdate không kích hoạt (§6.3)
```

Ngân sách ống kính không viết trong code, viết lúc xếp cảnh: Điểm bắt đầu tween ống kính liền kề cách nhau ≥2.6s、
Cửa sổ 15s ≤4-5 cái, Zoom <1.25x Không xếp (camera-language §0/§4.4).

### 9.5 counter-transform · Đi theo phụ đề/Chú thích giữ cỡ chữ hằng định

Phụ đề và chrome ưu tiên đặt ở `#hud` (Không đi theo ống kính, chi phí 0). Bắt buộc treo trong world,
Đi theo phần tử nhưng cỡ chữ phải hằng định chú thích, triệt tiêu ngược hướng co giãn ống kính:

```js
const counters = gsap.utils.toArray(".cam-counter");   // Chú thích cần cỡ chữ hằng định
function applyCounter() {
  const inv = 1 / cam.zoom;
  counters.forEach((el) => { el.style.transform = `scale(${inv})`; });
}
```

Hoạt ảnh vào cảnh của bản thân `.cam-counter` Viết trên **phần tử con** của nó, Tránh tranh giành transform với counter scale.

### 9.6 Parallax Nhiều tầng · Toàn bộ suy ra từ cam

Mỗi tầng không cho tween độc lập, Hệ số tốc độ nhân với cùng một dịch chuyển máy ảnh (Tỷ lệ hệ số giữa các tầng ≥2 lần, ≤4 tầng,
camera-language §8.1), Tự nhiên đồng bộ, tự nhiên seek-safe:

```js
const LAYERS = [
  { el: document.querySelector("#bg"),  k: 0.35 },
  { el: document.querySelector("#mid"), k: 0.7  },
  { el: document.querySelector("#fg"),  k: 1.4  },
];
function applyParallax() {
  const dx = 960 - cam.cx, dy = 540 - cam.cy;    // Dịch chuyển máy ảnh
  LAYERS.forEach(({ el, k }) => {
    el.style.transform = `translate(${dx * k}px, ${dy * k}px)`;
  });
}
// Thêm applyParallax() vào cuối applyCam() là được
```

### 9.7 Tự kiểm tra Camera Rig (Bổ sung vào danh sách §8)

- [ ] Tween ống kính chỉ động cam proxy, Phần tử bên trong `#world` Không bị tween máy ảnh đụng vào?
- [ ] Sau khi đăng ký timeline có bổ sung khung hình đầu `applyCam()`?
- [ ] Đặc tả chữ 3D dùng CSS `zoom`, Không có `scale()` Phóng đại bị mờ?
- [ ] Thuộc tính `zoom` Chỉ xuất hiện trên `#world` (Ngoại lệ reflow không lan rộng)?
- [ ] Thời lượng đẩy kéo toàn bộ đến từ `zoomDur()`, Không có hằng số viết tay?
