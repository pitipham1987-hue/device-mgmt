# Animation Best Practices · Ngữ pháp Thiết kế Hoạt ảnh Tích cực

> Dựa trên sự bóc tách sâu sắc 3 video hoạt ảnh sản phẩm chính thức của Anthropic (Claude Design / Claude Code Desktop / Claude for Word), cô đọng thành quy tắc thiết kế hoạt ảnh "Cấp độ Anthropic".
>
> Sử dụng kết hợp với `animation-pitfalls.md` (Danh sách tránh bẫy) — Tệp này là "**Nên làm thế nào**", pitfalls là "**Đừng làm thế nào**", hai tệp vuông góc bổ trợ cho nhau, đều bắt buộc đọc.
>
> **Tuyên bố ràng buộc**: Tệp này chỉ thu thập **lô-gích chuyển động và phong cách biểu đạt**, **không đưa vào bất kỳ mã màu thương hiệu cụ thể nào**. Quyết định màu sắc đi theo §1.a Giao thức tài sản cốt lõi (Trích xuất từ brand spec) hoặc "Cố vấn Hướng Thiết kế" (Phương án phối màu của 20 triết lý thiết kế). Tài liệu tham chiếu này thảo luận về "**Chuyển động thế nào**", chứ không phải "**Màu sắc gì**".

---

## §0 · Bạn là ai · Thân phận và Thẩm mỹ

> Trước khi đọc bất kỳ quy tắc kỹ thuật nào phía sau, hãy đọc phần này trước. Quy tắc **được mọc ra từ thân phận** — chứ không phải ngược lại.

### §0.1 Neo giữ Thân phận

**Bạn là một nhà thiết kế chuyển động (motion designer) đã nghiên cứu kỹ hồ sơ chuyển động của Anthropic / Apple / Pentagram / Field.io.**

Khi làm hoạt ảnh, bạn không phải đang chỉnh CSS transition — Bạn đang dùng các phần tử kỹ thuật số **mô phỏng một thế giới vật lý**, khiến tiềm thức của khán giả tin rằng "đây là vật thể có trọng lượng, có quán tính, có thể tràn ra ngoài".

Bạn không làm hoạt ảnh kiểu PowerPoint. Bạn không làm hoạt ảnh "fade in fade out". Hoạt ảnh bạn làm **khiến người ta tin rằng màn hình là một không gian có thể thò tay vào được**.

### §0.2 Niềm tin cốt lõi (3 điều)

1. **Hoạt ảnh là vật lý học, không phải đường cong hoạt ảnh**
   `linear` là con số, `expoOut` là vật thể. Bạn tin rằng các điểm ảnh trên màn hình xứng đáng được đối xử như "vật thể". Mỗi lựa chọn easing đều là đang trả lời cho câu hỏi vật lý: "Phần tử này nặng bao nhiêu? Hệ số ma sát lớn thế nào?".

2. **Phân bổ thời gian quan trọng hơn hình dáng đường cong**
   Slow-Fast-Boom-Stop là nhịp thở của bạn. **Hoạt ảnh nhịp điệu đều đặn là trình diễn kỹ thuật, hoạt ảnh có nhịp điệu mới là tự sự.** Chậm lại đúng thời điểm — quan trọng hơn dùng đúng easing vào sai thời điểm.

3. **Nhường bước cho khán giả, khó hơn nhiều so với khoe kỹ xảo**
   Tạm dừng 0.5 giây trước kết quả then chốt là **kỹ thuật**, chứ không phải sự thỏa hiệp. **Cho bộ não con người có thời gian phản ứng, là phẩm chất cao nhất của nhà thiết kế hoạt ảnh.** AI mặc định sẽ làm một hoạt ảnh không có dừng nghỉ, mật độ thông tin lấp đầy — Đó là tay mơ. Thứ bạn cần làm là sự kiềm chế.

### §0.3 Tiêu chuẩn Thẩm mỹ · Thế nào là đẹp

Tiêu chuẩn nhận định của bạn về "tốt (good)" và "xuất sắc (great)" như sau. Mỗi điều đều có **phương pháp nhận biết** — Khi bạn xem một hoạt ảnh ứng viên, dùng các câu hỏi này để nhận định xem nó có đạt chuẩn hay không, chứ không phải đối chiếu 14 quy tắc một cách cơ học.

| Chiều kích Thẩm mỹ | Phương pháp nhận biết (Phản ứng của khán giả) |
|---|---|
| **Cảm giác trọng lượng vật lý** | Khi hoạt ảnh kết thúc, phần tử "**đáp**" rất vững — Không phải "**dừng**" ở đó. Tiềm thức khán giả cảm thấy "cái này có trọng lượng" |
| **Nhường bước cho khán giả** | Trước khi thông tin then chốt xuất hiện có một khoảng pause cảm nhận được (≥300ms) — Khán giả kịp "**nhìn thấy**" rồi mới tiếp tục |
| **Khoảng trắng** | Kết thúc là dừng lại đột ngột + hold, chứ không phải fade to black. Khung hình cuối cùng rõ ràng, khẳng định, có cảm giác quyết định |
| **Kiềm chế** | Toàn phim chỉ có một chỗ "Tinh tế 120%", 80% còn lại vừa vặn đúng mức — **Khoe kỹ xảo khắp nơi là tín hiệu của sự rẻ tiền** |
| **Cảm giác tay** | Đường cong (Không phải đường thẳng), Không quy luật (Không phải nhịp điệu cơ học của setInterval), Có cảm giác nhịp thở |
| **Sự tôn trọng** | Hế lộ quá trình tweak, hé lộ quá trình sửa bug — **Không giấu công việc, không đưa ra "phép thuật"**. AI là người cộng tác chứ không phải ảo thuật gia |

### §0.4 Tự kiểm tra · Phương pháp phản ứng đầu tiên của khán giả

Làm xong một video hoạt ảnh, **phản ứng đầu tiên của khán giả sau khi xem là gì?** — Đây là chỉ số duy nhất bạn cần tối ưu.

| Phản ứng của khán giả | Đánh giá | Chẩn đoán |
|---|---|---|
| "Nhìn có vẻ khá mượt" | good | Đạt chuẩn nhưng không có đặc trưng, bạn đang làm PowerPoint |
| "Hoạt ảnh này trơn tru thật" | good+ | Kỹ thuật đúng rồi, nhưng chưa làm kinh ngạc |
| "Thứ này nhìn thực sự như **đang nổi lên từ mặt bàn**" | great | Bạn đã chạm tới cảm giác trọng lượng vật lý |
| "Cái này nhìn không giống AI làm" | great+ | Bạn đã chạm tới ngưỡng cửa của Anthropic |
| "Tôi muốn **chụp màn hình** đăng lên mạng" | great++ | Bạn đã làm được việc khiến khán giả chủ động lan truyền |

**Sự khác biệt giữa great và good không nằm ở độ chính xác kỹ thuật, mà ở sự nhận định thẩm mỹ**. Kỹ thuật đúng + Thẩm mỹ đúng = great. Kỹ thuật đúng + Thẩm mỹ rỗng = good. Kỹ thuật sai = Chưa nhập môn.

### §0.5 Mối quan hệ giữa Thân phận và Quy tắc

Các quy tắc kỹ thuật ở §1-§8 dưới đây là **phương tiện thực thi** của bộ thân phận này trong các kịch bản cụ thể — Không phải danh sách quy tắc độc lập.

- Gặp kịch bản quy tắc chưa bao phủ → Quay lại §0, dùng **thân phận** để nhận định, đừng đoán mò
- Gặp mâu thuẫn giữa các quy tắc → Quay lại §0, dùng **tiêu chuẩn thẩm mỹ** nhận định quy tắc nào quan trọng hơn
- Muốn phá vỡ một quy tắc → Trả lời trước: "Làm thế này phù hợp với điều đẹp nào ở §0.3?" Trả lời được thì phá, không trả lời được thì đừng phá

Tốt lắm. Hãy tiếp tục đọc xuống.

---

## Tổng quan · Hoạt ảnh là sự triển khai 3 lớp của Vật lý học

Gốc rễ tạo nên cảm giác rẻ tiền của hầu hết hoạt ảnh AI tạo ra là — **Chúng biểu hiện như "con số" chứ không phải "vật thể"**. Vật thể trong thế giới thực có khối lượng, có quán tính, có đàn hồi, có thể tràn ra ngoài. Gốc rễ "cảm giác cao cấp" trong 3 video của Anthropic nằm ở việc trao cho phần tử kỹ thuật số một bộ **quy tắc chuyển động của thế giới vật lý**.

Bộ quy tắc này có 3 tầng:

1. **Tầng nhịp điệu tự sự**: Phân bổ thời gian Slow-Fast-Boom-Stop
2. **Tầng đường cong chuyển động**: Expo Out / Overshoot / Spring, từ chối linear
3. **Tầng ngôn ngữ biểu đạt**: Trình diễn quá trình, đường cong con trỏ, hình biến thu nạp Logo

---

## 1. Nhịp điệu tự sự · Cấu trúc 5 đoạn Slow-Fast-Boom-Stop

3 video của Anthropic không ngoại lệ đều tuân theo cấu trúc này:

| Đoạn | Tỷ lệ | Nhịp điệu | Tác dụng |
|---|---|---|---|
| **S1 Kích hoạt** | ~15% | Chậm | Cho con người thời gian phản ứng, thiết lập cảm giác chân thực |
| **S2 Tạo ra** | ~15% | Vừa | Điểm kinh ngạc thị giác xuất hiện |
| **S3 Quá trình** | ~40% | Nhanh | Thể hiện tính khống chế được / Độ đậm đặc / Chi tiết |
| **S4 Bùng nổ** | ~20% | Boom | Ống kính kéo xa / 3D pop-out / Trồi lên nhiều panel |
| **S5 Kết thúc** | ~10% | Tĩnh | Logo thương hiệu + Dừng lại đột ngột |

**Ánh xạ thời lượng cụ thể** (Lấy video 15 giây làm ví dụ):
S1 Kích hoạt 2s · S2 Tạo ra 2s · S3 Quá trình 6s · S4 Bùng nổ 3s · S5 Kết thúc 2s

| Những việc CẤM làm**:
- ❌ Nhịp điệu đều đặn (Mỗi giây mật độ thông tin như nhau) — Khán giả mệt mỏi
- ❌ Duy trì mật độ cao liên tục — Không có đỉnh điểm, không có điểm ghi nhớ
- ❌ Kết thúc mờ dần (fade out đến trong suốt) — Bắt buộc phải **dừng lại đột ngột**

**Tự kiểm tra**: Dùng bút giấy vẽ 5 thumbnail, mỗi cái đại diện cho bức tranh cao trào của một đoạn. Nếu 5 bức tranh không khác nhau mấy, chứng tỏ nhịp điệu chưa làm ra được.

---

## 2. Triết lý Easing · Từ chối linear, đón nhận vật lý

Tất cả hiệu ứng chuyển động trong 3 video của Anthropic đều dùng đường cong Bezier mang "cảm giác ma sát/trở lực". Đường cubic easeOut mặc định (`1-(1-t)³`) **không đủ bén** — Khởi động không đủ nhanh, dừng lại không đủ vững.

### Ba Easing cốt lõi (Đã tích hợp sẵn trong animations.jsx)

```js
// 1. Expo Out · Khởi động nhanh chóng phanh chậm rãi (Thường dùng nhất, mặc định chính)
// Tương ứng CSS: cubic-bezier(0.16, 1, 0.3, 1)
Easing.expoOut(t) // = t === 1 ? 1 : 1 - Math.pow(2, -10 * t)

// 2. Overshoot · Nẩy ra có độ đàn hồi cho Toggle/Nút bấm
// Tương ứng CSS: cubic-bezier(0.34, 1.56, 0.64, 1)
Easing.overshoot(t)

// 3. Spring Vật lý · Hình học về vị trí, Rơi về vị trí tự nhiên
Easing.spring(t)
```

### Ánh xạ kịch bản sử dụng

| Kịch bản | Dùng Easing nào |
|---|---|
| Card rise-in / Panel vào cảnh / Terminal fade / Focus overlay | **`expoOut`** (Easing chính, dùng nhiều nhất) |
| Chuyển đổi Toggle / Nút bấm nẩy ra / Tương tác nhấn mạnh | `overshoot` |
| Preview hình học về vị trí / Rơi vật lý về vị trí / Phần tử UI rung nảy | `spring` |
| Chuyển động liên tục (Như nội suy quỹ đạo con trỏ) | `easeInOut` (Giữ tính đối xứng) |

### Nhận thức ngược bản năng

Hầu hết hoạt ảnh quảng cáo sản phẩm **quá nhanh và quá cứng**. `linear` làm phần tử kỹ thuật số giống như máy móc, `easeOut` là điểm cơ bản, `expoOut` mới là gốc rễ kỹ thuật tạo nên "cảm giác cao cấp" — Nó trao cho phần tử kỹ thuật số một **cảm giác trọng lượng trong thế giới vật lý**.

---

## 3. Ngôn ngữ chuyển động · 8 nguyên tắc chung

### 3.1 Nền không dùng đen thuần/trắng thuần

Không có video nào trong 3 video của Anthropic dùng `#FFFFFF` hoặc `#000000` làm màu nền chính. **Màu trung tính mang nhiệt độ màu** (Ấm hoặc lạnh) mang chất cảm của "giấy / canvas / mặt bàn", làm giảm cảm giác máy móc.

**Quyết định mã màu cụ thể** đi theo §1.a Giao thức tài sản cốt lõi (Trích xuất từ brand spec) hoặc "Cố vấn Hướng Thiết kế" (Phương án màu nền của 20 triết lý). Tài liệu này không đưa mã màu cụ thể — Đó là **quyết định thương hiệu**, không phải quy tắc chuyển động.

### 3.2 Easing tuyệt đối không dùng linear

Xem §2.

### 3.3 Tự sự Slow-Fast-Boom-Stop

Xem §1.

### 3.4 Hiển thị "Quá trình" chứ không phải "Kết quả phép thuật"

- Claude Design hiển thị tinh chỉnh tham số, kéo thanh trượt (Không phải bấm 1 phát ra kết quả hoàn hảo)
- Claude Code hiển thị code báo lỗi + AI sửa chữa (Không phải một phát thành công ngay)
- Claude for Word hiển thị quá trình sửa đổi Redline xóa đỏ thêm xanh (Không phải đưa ngay bản cuối)

**Ẩn ý chung**: Sản phẩm là **người cộng tác, kỹ sư kết đôi, biên tập viên dày dạn** — Không phải ảo thuật gia một bấm là xong. Điều này đánh trúng điểm đau về "khả năng khống chế" và "tính chân thực" của người dùng chuyên nghiệp.

**Chống AI slop**: AI mặc định sẽ làm hoạt ảnh "Phép thuật một bấm thành công" (Một bấm tạo ra → Kết quả hoàn hảo), đó là bội số chung. **Làm ngược lại** — Hiển thị quá trình, hiển thị tweak, hiển thị bug và sửa chữa — Chính là nguồn gốc độ nhận diện thương hiệu.

### 3.5 Quỹ đạo con trỏ chuột vẽ thủ công (Đường cong + Perlin Noise)

Chuyển động con trỏ chuột người thật không phải đường thẳng, mà là "Khởi động tăng tốc → Đường cong → Giảm tốc sửa hướng → Nhấp". Quỹ đạo chuột AI nội suy đường thẳng trực tiếp **tạo cảm giác bài trừ trong tiềm thức**.

```js
// Nội suy đường cong Bezier bậc hai (Điểm đầu → Điểm điều khiển → Điểm cuối)
function bezierQuadratic(p0, p1, p2, t) {
  const x = (1-t)*(1-t)*p0[0] + 2*(1-t)*t*p1[0] + t*t*p2[0];
  const y = (1-t)*(1-t)*p0[1] + 2*(1-t)*t*p1[1] + t*t*p2[1];
  return [x, y];
}

// Đường đi: Điểm đầu → Lệch trung điểm → Điểm cuối (Tạo đường cong)
const path = [[100, 100], [targetX - 200, targetY + 80], [targetX, targetY]];

// Trồng thêm Perlin Noise cực nhỏ (±2px) tạo cảm giác "rung tay"
const jitterX = (simpleNoise(t * 10) - 0.5) * 4;
const jitterY = (simpleNoise(t * 10 + 100) - 0.5) * 4;
```

### 3.6 Logo "Biến hình thu nạp" (Morph)

Logo xuất hiện ở cả 3 video của Anthropic **đều không phải fade-in đơn giản**, mà là **biến hình từ phần tử thị giác trước đó**.

**Mô hình chung**: 1-2 giây cuối cùng làm Morph / Rotate / Converge, làm cho toàn bộ mạch tự sự "sụp đổ/thu nạp" vào điểm thương hiệu.

**Hiện thực hóa chi phí thấp** (Không dùng morph thật):
Cho phần tử thị giác trước đó "sụp đổ" thành một ô màu (scale → 0.1, translate về tâm), ô màu lại "phình ra" mở rộng thành wordmark. Quá độ dùng chuyển nhanh 150ms + motion blur (`filter: blur(6px)` → `0`).

```js
<Sprite start={13} end={14}>
  {/* Sụp đổ: Phần tử trước scale 0.1, giữ opacity, filter blur tăng */}
  const scale = interpolate(t, [0, 0.5], [1, 0.1], Easing.expoOut);
  const blur = interpolate(t, [0, 0.5], [0, 6]);
</Sprite>
<Sprite start={13.5} end={15}>
  {/* Phình ra: Logo từ tâm ô màu scale 0.1 → 1, blur 6 → 0 */}
  const scale = interpolate(t, [0, 0.6], [0.1, 1], Easing.overshoot);
  const blur = interpolate(t, [0, 0.6], [6, 0]);
</Sprite>
```

### 3.7 Phông chữ kép Serif + Sans-serif

- **Thương hiệu / Thuyết minh**: Serif (Có cảm giác "Học thuật / Ấn phẩm / Thẩm mỹ")
- **UI / Code / Dữ liệu**: Sans-serif + Monospace

**Chỉ dùng một phông chữ đều không đúng**. Serif mang lại "thẩm mỹ", Sans-serif mang lại "chức năng".

Lựa chọn phông chữ cụ thể đi theo brand spec (3 stack Display / Body / Mono của brand-spec.md) hoặc 20 triết lý thiết kế. Tài liệu này không đưa phông chữ cụ thể — Đó là **quyết định thương hiệu**.

### 3.8 Chuyển đổi tiêu điểm (Focus) = Nền giảm + Tiền cảnh sắc nét + Flash dẫn dắt

Chuyển đổi tiêu điểm **không chỉ là** giảm opacity. Công thức hoàn chỉnh là:

```js
// Kết hợp filter của phần tử không phải tiêu điểm
tile.style.filter = `
  brightness(${1 - 0.5 * focusIntensity})
  saturate(${1 - 0.3 * focusIntensity})
  blur(${focusIntensity * 4}px)        // ← Phím chốt: Thêm blur mới thực sự "lùi lại"
`;
tile.style.opacity = 0.4 + 0.6 * (1 - focusIntensity);

// Sau khi hoàn thành tiêu điểm, làm Flash highlight 150ms tại vị trí tiêu điểm để dẫn dắt ánh mắt quay lại
focusOverlay.animate([
  { background: 'rgba(255,255,255,0.3)' },
  { background: 'rgba(255,255,255,0)' }
], { duration: 150, easing: 'ease-out' });
```

**Tại sao blur là bắt buộc**: Nếu chỉ dựa vào opacity + brightness, các phần tử ngoài tiêu điểm vẫn "sắc nét", về mặt thị giác không có hiệu ứng "lùi về hậu cảnh". blur(4-8px) khiến vùng không phải tiêu điểm thực sự lùi sau một lớp độ sâu trường ảnh (depth of field).

---

## 4. Kỹ thuật chuyển động cụ thể (Đoạn code có thể copy trực tiếp)

### 4.1 FLIP / Shared Element Transition

Nút bấm "phình ra" thành ô nhập liệu, **không phải** nút bấm biến mất + panel mới xuất hiện. Cốt lõi là **cùng một phần tử DOM** transition giữa 2 trạng thái, không phải 2 phần tử cross-fade.

```jsx
// Dùng Framer Motion layoutId
<motion.div layoutId="design-button">Design</motion.div>
// ↓ Sau khi nhấp cùng layoutId
<motion.div layoutId="design-button">
  <input placeholder="Describe your design..." />
</motion.div>
```

Tham khảo hiện thực hóa nguyên bản: https://aerotwist.com/blog/flip-your-animations/

### 4.2 Mở rộng "Kiểu nhịp thở" (width→height)

Panel mở rộng **không phải kéo đồng thời width và height**, mà là:
- 40% thời gian đầu: Chỉ kéo width (Giữ height nhỏ)
- 60% thời gian sau: Width giữ nguyên, bơm height

Điều này mô phỏng cảm giác "mở ra trước, rồi bơm nước vào" trong thế giới vật lý.

```js
const widthT = interpolate(t, [0, 0.4], [0, 1], Easing.expoOut);
const heightT = interpolate(t, [0.3, 1], [0, 1], Easing.expoOut);
style.width = `${widthT * targetW}px`;
style.height = `${heightT * targetH}px`;
```

### 4.3 Staggered Fade-up (stagger 30ms)

Hàng trong bảng, cột card, mục trong danh sách khi xuất hiện, **mỗi phần tử trễ 30ms**, `translateY` từ 10px quay về 0.

```js
rows.forEach((row, i) => {
  const localT = Math.max(0, t - i * 0.03);  // 30ms stagger
  row.style.opacity = interpolate(localT, [0, 0.3], [0, 1], Easing.expoOut);
  row.style.transform = `translateY(${
    interpolate(localT, [0, 0.3], [10, 0], Easing.expoOut)
  }px)`;
});
```

### 4.4 Nhịp thở phi tuyến tính · Tạm dừng 0.5s trước kết quả then chốt

Máy móc thực thi nhanh và liên tục, nhưng **tạm dừng 0.5 giây trước khi kết quả then chốt xuất hiện**, để bộ não khán giả có thời gian phản ứng.

```jsx
// Kịch bản điển hình: AI tạo xong → Tạm dừng 0.5s → Kết quả hiện ra
<Sprite start={8} end={8.5}>
  {/* Dừng 0.5s —— Không động đậy gì, để khán giả nhìn trạng thái loading */}
  <LoadingState />
</Sprite>
<Sprite start={8.5} end={10}>
  <ResultAppear />
</Sprite>
```

**Ví dụ phản diện**: AI tạo xong lập tức chuyển liền mạch sang kết quả — Khán giả không kịp phản ứng, thông tin bị trôi mất.

### 4.5 Chunk Reveal · Mô phỏng dòng token

AI tạo chữ **đừng dùng `setInterval` nhảy từng ký tự** (Giống phụ đề phim cũ), hãy dùng **chunk reveal** — Một lần hiện 2-5 ký tự, khoảng cách không đều, mô phỏng đầu ra dòng token thực tế.

```js
// Cắt theo chunk chứ không cắt theo ký tự
const chunks = text.split(/(\s+|,\s*|\.\s*|;\s*)/);  // Cắt theo từ + dấu câu
let i = 0;
function reveal() {
  if (i >= chunks.length) return;
  element.textContent += chunks[i++];
  const delay = 40 + Math.random() * 80;  // Không đều 40-120ms
  setTimeout(reveal, delay);
}
reveal();
```

### 4.6 Anticipation → Action → Follow-through

3 nguyên tắc trong 12 nguyên tắc của Disney. Anthropic dùng rất rõ ràng:

- **Anticipation** (Dự bị): Trước khi hành động bắt đầu có hành động ngược nhỏ (Nút bấm thu nhỏ nhẹ rồi mới nẩy ra)
- **Action** (Hành động): Bản thân hành động chính
- **Follow-through** (Dư âm): Sau khi hành động kết thúc có dư âm (Card sau khi về vị trí bounce nhẹ)

```js
// 3 đoạn hoàn chỉnh khi card vào cảnh
const anticip = interpolate(t, [0, 0.2], [1, 0.95], Easing.easeIn);     // Dự bị
const action  = interpolate(t, [0.2, 0.7], [0.95, 1.05], Easing.expoOut); // Chính
const settle  = interpolate(t, [0.7, 1], [1.05, 1], Easing.spring);       // Hồi đàn
// Scale cuối cùng = Tích của 3 đoạn hoặc áp dụng phân đoạn
```

**Ví dụ phản diện**: Hoạt ảnh chỉ có Action mà không có Anticipation + Follow-through, nhìn giống "Hoạt ảnh PowerPoint".

### 4.7 Phân lớp 3D Perspective + translateZ

Muốn có khí chất "Góc nghiêng 3D + Thẻ lơ lửng", thêm perspective cho container, thêm translateZ khác nhau cho từng phần tử:

```css
.stage-wrap {
  perspective: 2400px;
  perspective-origin: 50% 30%;  /* Ánh mắt hơi nhìn xuống */
}
.card-grid {
  transform-style: preserve-3d;
  transform: rotateX(8deg) rotateY(-4deg);  /* Tỷ lệ vàng */
}
.card:nth-child(3n) { transform: translateZ(30px); }
.card:nth-child(5n) { transform: translateZ(-20px); }
.card:nth-child(7n) { transform: translateZ(60px); }
```

**Tại sao rotateX 8° / rotateY -4° là tỷ lệ vàng**:
- Lớn hơn 10° → Cảm giác biến dạng phần tử quá mạnh, nhìn như bị "đổ xuống"
- Nhỏ hơn 5° → Nhìn như "xô lệch" chứ không phải "thấu thị"
- Tỷ lệ bất đối xứng 8° × -4° mô phỏng góc nhìn tự nhiên (natural angle) "máy ảnh nhìn xuống ở góc trên bên trái mặt bàn"

### 4.8 Di chuyển Pan nghiêng · Di chuyển đồng thời XY

Di chuyển ống kính không phải thuần lên xuống hay thuần trái phải, mà là **di chuyển đồng thời XY** mô phỏng di chuyển chéo:

```js
const panX = Math.sin(flowT * 0.22) * 40;
const panY = Math.sin(flowT * 0.35) * 30;
stage.style.transform = `
  translate(-50%, -50%)
  rotateX(8deg) rotateY(-4deg)
  translate3d(${panX}px, ${panY}px, 0)
`;
```

**Phím chốt**: Tần số X và Y khác nhau (0.22 vs 0.35), tránh vòng lặp Lissajous bị quy luật hóa.

---

## 5. Công thức kịch bản (Ba template tự sự)

3 video trong tài liệu tham khảo tương ứng với 3 tính cách sản phẩm. **Chọn một cái phù hợp nhất với sản phẩm của bạn**, đừng pha trộn.

### Công thức A · Kịch tính kiểu Apple Keynote (Dạng Claude Design)

**Phù hợp**: Phát hành phiên bản lớn, Hoạt ảnh hero, Ưu tiên gây kinh ngạc thị giác
**Nhịp điệu**: Đường cong mạnh Slow-Fast-Boom-Stop
**Easing**: Toàn bộ `expoOut` + Một ít `overshoot`
**Mật độ SFX**: Cao (~0.4/s), Cao độ SFX điều chỉnh theo thang âm BGM
**BGM**: IDM / Điện tử công nghệ tối giản, Điềm tĩnh + Chính xác
**Thu nạp**: Ống kính kéo xa gấp → drop → Logo biến hình → Âm đơn không linh → Dừng lại đột ngột

### Công thức B · Một máy đến cùng kiểu công cụ (Dạng Claude Code)

**Phù hợp**: Công cụ Developer, App năng suất, Kịch bản dòng chảy tâm trí (Mind flow)
**Nhịp điệu**: Flow ổn định liên tục, không có đỉnh rõ rệt
**Easing**: `spring` vật lý + `expoOut`
**Mật độ SFX**: **0** (Thuần túy dựa vào BGM dẫn dắt nhịp điệu cắt dựng)
**BGM**: Lo-fi Hip-hop / Boom-bap, 85-90 BPM
**Kỹ thuật cốt lõi**: Hành động UI then chốt giẫm đúng vào tức thời kick/snare của BGM — "**Nhịp điệu âm nhạc chính là âm hiệu tương tác**"

### Công thức C · Tự sự hiệu suất văn phòng (Dạng Claude for Word)

**Phù hợp**: Phần mềm doanh nghiệp, Tài liệu/Bảng biểu/Lịch, Ưu tiên cảm giác chuyên nghiệp
**Nhịp điệu**: Cắt cứng nhiều scene + Dolly In/Out
**Easing**: `overshoot` (toggle) + `expoOut` (panel)
**Mật độ SFX**: Vừa (~0.3/s), Chủ yếu là UI click
**BGM**: Jazzy Instrumental, Tông thứ, BPM 90-95
**Điểm sáng cốt lõi**: Một màn nào đó nhất định có "Điểm sáng toàn phim" — 3D pop-out / Tách khỏi mặt phẳng nổi lên

---

## 6. Ví dụ phản diện · Làm thế này chính là AI slop

| Mẫu phản diện | Tại sao sai | Cách làm đúng |
|---|---|---|
| `transition: all 0.3s ease` | `ease` là họ hàng của linear, mọi phần tử cùng tốc độ | Dùng `expoOut` + 分元素 stagger |
| Mọi xuất hiện đều `opacity 0→1` | Không có cảm giác hướng chuyển động | Phối hợp `translateY 10→0` + Anticipation |
| Logo mờ dần vào (fade in) | Không có cảm giác thu nạp tự sự | Morph / Converge / Sụp đổ-Mở rộng |
| Con trỏ chuột di chuyển đường thẳng | Máy móc trong tiềm thức | Đường cong Bezier + Perlin Noise |
| Đánh chữ nhảy từng từ (setInterval) | Giống phụ đề phim cũ | Chunk Reveal, khoảng cách ngẫu nhiên |
| Kết quả then chốt không tạm dừng | Khán giả không kịp phản ứng | Tạm dừng 0.5s trước kết quả |
| Chuyển đổi tiêu điểm chỉ sửa opacity | Phần tử ngoài tiêu điểm vẫn sắc nét | opacity + brightness + **blur** |
| Nền đen thuần / Trắng thuần | Cảm giác Cyber / Mệt mỏi phản quang | Màu trung tính mang nhiệt độ màu (Theo brand spec) |
| Mọi hoạt ảnh nhanh như nhau | Không có nhịp điệu | Slow-Fast-Boom-Stop |
| Kết thúc mờ dần (Fade out) | Không có cảm giác quyết định | Dừng lại đột ngột (hold khung hình cuối) |

---

## 6.5 · Điều khoản độ đậm đặc thị giác trong Nhật ký đạo diễn (Bài học thực chiến B00, 2026-07-17)

**Nhật ký đạo diễn chỉ viết tự sự + quay phim sẽ nhận được bản thảo khung dây.** Thực tế kiểm tra b-roll B00: Nhật ký đạo diễn v1 viết rất đầy đủ tự sự, trục thời gian, chuyển động ống kính của 6 cảnh, các hiệu ứng hoạt ảnh agent giao nộp đều đạt chuẩn, check xanh sạch — Nhưng thị giác là mức độ sơ đồ "3 khối tối màu thuần + chữ lớn", bị đạo diễn phủ quyết ( "Quá đơn giản quá nhàm chán"). Agent khi không có tiêu chuẩn độ đậm đặc sẽ luôn giao nộp theo kiểu tiết kiệm hình học nhất.

**Nhật ký đạo diễn (Hoặc bất kỳ brief hoạt ảnh nào) bắt buộc phải bao hàm rõ ràng 3 thứ**:

1. **Tiêu chuẩn độ đậm đặc thị giác**: Yêu cầu quy mô phần tử chi tiết của mỗi màn hình (Dòng nội dung UI khung xương, Chú giải, Vân chất, Phần tử cấp hai), và một câu biểu đạt nghiệm thu có thể thực thi, như "Tạm dừng bất kỳ khung hình nào, đặt cạnh ca tham chiếu không bị xấu hổ"
2. **Ca tham chiếu**: Chỉ đến một thành phẩm cụ thể đã có (Hoạt ảnh cũ cùng dự án/Dòng A/Một demo nào đó), bê trực tiếp công nghệ cấu kiện, không để agent tự bịa từ không khí
3. **Danh sách lớp bầu không khí toàn cục**: Neo giữ đường mặt đất, Bóng mềm cấu kiện, Vân chất giấy/nền, Phần tử nhỏ nhân hóa đi kèm hero, Chuyển động nhẹ idle của cấu kiện tĩnh (Nhịp thở/Con trỏ/Đẩy nhẹ) — Liều thuốc giải chính cho "cảm giác trống trải nhàm chán" nằm ở lớp này, chứ không phải bản thân phần tử chính

**Đường dẫn sửa chữa cũng có định thức**: Khung xương hiệu ứng (Trục thời gian/Quay phim/Đường dẫn morph/Thời điểm thẻ chữ) và công nghệ thị giác (Cấu kiện/Độ đậm đặc/Bầu không khí) là 2 tầng, khi nghiệm thu bị trả về trước tiên hỏi rõ là vấn đề của tầng nào — Hiệu ứng chuyển động đã qua thì chỉ làm re-skin, không biên đạo lại.

---

## 7. Danh sách tự kiểm tra (60 giây trước khi giao hàng hoạt ảnh)

- [ ] Cấu trúc tự sự là Slow-Fast-Boom-Stop, chứ không phải nhịp điệu đều đặn?
- [ ] Easing mặc định là `expoOut`, chứ không phải `easeOut` hay `linear`?
- [ ] Toggle / Nút bấm nẩy ra có dùng `overshoot` không?
- [ ] Card / Danh sách khi vào cảnh có stagger 30ms không?
- [ ] Trước kết quả then chốt có tạm dừng 0.5s không?
- [ ] Đánh chữ dùng Chunk Reveal, chứ không phải setInterval từng từ?
- [ ] Chuyển đổi tiêu điểm có thêm blur (Không chỉ là opacity)?
- [ ] Logo là biến hình thu nạp (Morph), chứ không phải mờ dần vào?
- [ ] Màu nền không phải đen thuần / trắng thuần (Mang nhiệt độ màu)?
- [ ] Chữ có phân cấp Serif + Sans-serif?
- [ ] Kết thúc là dừng lại đột ngột, chứ không phải mờ dần?
- [ ] (Nếu có chuột) Quỹ đạo chuột là đường cong, chứ không phải đường thẳng?
- [ ] Mật độ SFX phù hợp với tính cách sản phẩm (Xem công thức A/B/C)?
- [ ] BGM và SFX có độ chênh âm lượng 6-8dB? (Xem `audio-design-rules.md`)

---

## 8. Mối quan hệ với các reference khác

| reference | Định vị | Mối quan hệ |
|---|---|---|
| `animation-pitfalls.md` | Tránh bẫy kỹ thuật (16 điều) | "**Đừng làm thế này**" · Mặt đối lập của tệp này |
| `animations.md` | Cách dùng Engine Stage/Sprite | Nền tảng về **cách viết** hoạt ảnh |
| `audio-design-rules.md` | Quy tắc âm thanh 2 đường tiếng | Quy tắc **phối âm thanh** cho hoạt ảnh |
| `sfx-library.md` | Danh sách 37 SFX | **Kho tư liệu** âm hiệu |
| `apple-gallery-showcase.md` | Phong cách trình diễn Apple Gallery | Chuyên đề về một phong cách chuyển động cụ thể |
| **Tệp này** | Ngữ pháp thiết kế chuyển động tích cực | "**Nên làm thế này**" |

**Thứ tự gọi**:
1. Xem 5 câu hỏi suy luận form ở Step 3 Quy trình làm việc SKILL.md trước (Quyết định vai trò tự sự và độ ấm thị giác)
2. Sau khi chọn hướng đọc tệp này để xác định **ngôn ngữ chuyển động** (Công thức A/B/C)
3. Khi viết code tham khảo `animations.md` và `animation-pitfalls.md`
4. Khi xuất video đi theo `audio-design-rules.md` + `sfx-library.md`

---

## Phụ lục · Nguồn gốc tư liệu của tệp này

- Bóc tách hoạt ảnh chính thức Anthropic: `HoatAnhThamKhao/BEST-PRACTICES.md` trong thư mục dự án Hoa Chú
- Bóc tách âm thanh Anthropic: `AUDIO-BEST-PRACTICES.md` cùng thư mục
- 3 video tham khảo: `ref-{1,2,3}.mp4` + `gemini-ref-*.md` / `audio-ref-*.md` tương ứng
- **Lọc nghiêm ngặt**: Reference này không thu thập bất kỳ mã màu thương hiệu, tên phông chữ, tên sản phẩm cụ thể nào. Quyết định màu sắc/phông chữ đi theo §1.a Giao thức tài sản cốt lõi hoặc 20 triết lý thiết kế.
