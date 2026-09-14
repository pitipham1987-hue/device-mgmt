# Quy trình Voiceover Pipeline · Animation điều khiển bởi Thuyết minh

> Quy trình công việc nâng cấp animation từ "Hình ảnh không tiếng + Lồng tiếng hậu kỳ" thành "**Có lời thuyết minh trước, rồi điều khiển hình ảnh theo thời lượng đo đạc thực tế của âm thanh**".
> Áp dụng: Video giải thích khái niệm 5-20 phút, video hướng dẫn, phổ cập kiến thức dài tập.
>
> Sử dụng phối hợp với `references/animation-best-practices.md`——Tài liệu này quản lý **làm thế nào để khớp lời thuyết minh và hình ảnh**,
> animation-best-practices quản lý **mỗi khung hình hình ảnh chuyển động như thế nào**.

---

## 🛑 Quy tắc sắt · Bắt buộc đọc trước khi viết một dòng code

> **Nhấn mạnh bao nhiêu lần cũng không đủ: Dạng thất bại #1 của animation thuyết minh là làm thành PowerPoint có lồng tiếng.**

### Điều 1 · Toàn bộ thước phim là một tự sự chuyển động liên tục, không phải một nhóm scene độc lập

PowerPoint là 7 trang slide. Cái chúng ta làm là **bộ phim liên tục dài X phút**.

**Chuyển đổi vai trò**:
- ❌ Bạn không phải "đang làm nội dung của 7 scene"
- ✅ Bạn là "đang để một hoặc vài hero element diễn kịch X phút trên màn hình"

**Khung xương thị giác = Một hoặc vài hero element chạy xuyên suốt toàn bộ phim**:
- Nó xuất hiện từ t=0, đến khi kết thúc mới rời cảnh
- Mỗi cue là **sự thay đổi trạng thái** của nó (vị trí / kích thước / màu sắc / phối cảnh / hình thái), không phải "đổi một phần tử mới"
- Ranh giới scene có trong kịch bản, **trong màn hình không nên có**——khán giả không nhìn ra "đây là scene thứ 3", chỉ nhìn thấy một đoạn chuyển động liên tục

**Ví dụ phản diện (Án lệ v1 của skill này · 10-05-2026)**:
- 7 `<Scene>` bố cục độc lập với nhau, chuyển đổi scene = opacity 1→0 toàn trang rồi chuyển sang trang tiếp theo
- Mỗi cue = `opacity: p, transform: translateY((1-p)*30px)` (Dùng mờ dần hiện lên một cách đơn điệu)
- Kết quả: Khán giả xem xong phản ứng đầu tiên "giống như từng trang keynote", chất lượng toàn bộ video trở về 0

**Mẫu đúng**:
- Chọn ra 1-2 hero element (Như demo của bài viết này nên chọn hai ký tự "md", "html" làm khung xương)
- Hai ký tự này **từ đầu phim đến cuối phim** luôn ở trên màn hình
- Mỗi đoạn "scene" thực chất là một lần thay đổi trạng thái của hero element
  - opening: Hai ký tự đối đầu ở chính giữa màn hình
  - md-side: md biến lớn biến đậm chiếm màn hình, html lùi về chữ nhỏ ở góc; Dữ liệu dạt vào xung quanh md
  - html-side: html đảo ngược thành nhân vật chính; md lùi về góc
  - the-real-question: Hai ký tự trở lại chính giữa, nhưng ở giữa xuất hiện phân cách "≠"
  - the-split: Hai ký tự đẩy ra hai bên, khoảng trống ở giữa mở ra
  - activity-proof: Hai ký tự nhấp nháy luân phiên trên timeline
  - closing: Hai ký tự hạ cánh xuống vị trí đáp án cuối cùng
- Như vậy toàn bộ thước phim là "md và html đã diễn X phút trên màn hình", không phải 7 trang PPT độc lập

**Khung xương thực thi tối thiểu** (Chép lại và sửa trực tiếp):

```jsx
// ── Step 1: Định nghĩa trạng thái mục tiêu của hero trong mỗi scene (vị trí/kích thước/độ không trong suốt) ──
const HERO_KEYS = {
  opening:    { md: { x: 50, y: 35, scale: 1.0, opacity: 1 }, html: { x: 50, y: 65, scale: 1.0, opacity: 1 } },
  'md-side':  { md: { x: 78, y: 50, scale: 1.6, opacity: 1 }, html: { x: 92, y: 8,  scale: 0.25, opacity: 0.4 } },
  'html-side':{ md: { x: 8,  y: 8,  scale: 0.25, opacity: 0.4 }, html: { x: 22, y: 50, scale: 1.6, opacity: 1 } },
  // ... Mỗi đoạn một entry, chuyển động liên tục từ final của đoạn trước → from của đoạn này
};

// ── Step 2: Công cụ easing + lerp ──
const expoOut = t => t === 1 ? 1 : 1 - Math.pow(2, -10 * t);
const lerp = (a, b, t) => a + (b - a) * t;
const lerpPos = (from, to, t) => ({
  x: lerp(from.x, to.x, t), y: lerp(from.y, to.y, t),
  scale: lerp(from.scale, to.scale, t),
  opacity: lerp(from.opacity ?? 1, to.opacity ?? 1, t),
});

// ── Step 3: Component HeroAnchor —— Gắn trực tiếp ở cấp con của <NarrationStage>, không đưa vào trong <Scene> ──
const HeroAnchor = () => {
  const { time, scene, timeline } = useNarration();
  if (!scene) return null;
  const idx = timeline.scenes.findIndex(s => s.id === scene.id);
  const prevId = idx > 0 ? timeline.scenes[idx - 1].id : scene.id;
  const from = HERO_KEYS[prevId];
  const to   = HERO_KEYS[scene.id];

  // ~45% thời gian đầu trong đoạn dùng để morph từ trạng thái prev sang trạng thái đoạn này, thời gian còn lại hold
  const transitionDur = Math.min(2.0, scene.duration * 0.45);
  const t = expoOut(Math.min(1, (time - scene.start) / transitionDur));
  const md   = lerpPos(from.md,   to.md,   t);
  const html = lerpPos(from.html, to.html, t);

  // Thêm subtle breathing để bất kỳ một khung hình nào cũng có chuyển động (Tương ứng Điều 3)
  const breath = 1 + Math.sin(time * 0.6) * 0.012;

  const renderHero = (label, pos, color) => (
    <div style={{
      position: 'absolute', left: `${pos.x}%`, top: `${pos.y}%`,
      transform: `translate(-50%, -50%) scale(${pos.scale * breath})`,
      opacity: pos.opacity, color, fontSize: 360, fontWeight: 800,
      lineHeight: 1, willChange: 'transform, opacity', pointerEvents: 'none',
    }}>{label}</div>
  );
  return <>
    {renderHero('md',   md,   '#1B4965')}
    {renderHero('html', html, '#C04A1A')}
  </>;
};

// ── Step 4: Component chính —— hero nằm ở cấp con của NarrationStage, phần tử phụ trong scene quản lý riêng ──
const App = () => (
  <NarrationStage timeline={TIMELINE} audioSrc="_narration/voiceover.mp3" width={1920} height={1080}>
    <HeroAnchor />  {/* ← Tồn tại liên tục qua các scene, khung xương thị giác toàn bộ phim */}
    {/* Phần tử phụ trong scene dùng useSceneFade kiểm soát mờ dần vào/ra mềm mại, đừng cắt cứng */}
    <MdSideAux />
    <HtmlSideAux />
    {/* ... */}
  </NarrationStage>
);
```

**Tham khảo hoàn chỉnh có thể chạy**: `demos/md-html-narration/md-html-demo.html` (3 phút 21 giây, 7 đoạn, 21 cue, đã xác minh thực chiến)

### Điều 2 · Giữa các scene không được "cắt cứng"

| Mẫu sai (PowerPoint slop) | Mẫu đúng (Cảm giác điện ảnh) |
|---|---|
| scene A tổng thể `opacity 1→0` đồng thời scene B `opacity 0→1` | Phần tử cốt lõi của scene A **morph vào** B (vị trí/kích thước/màu sắc biến đổi mượt mà) |
| Mỗi scene bố cục độc lập, phần tử xuất hiện/biến mất | Phần tử **tồn tại liên tục** trên màn hình, chỉ có vị trí và hình thái thay đổi |
| `keepMounted=false`, khoảnh khắc chuyển scene component bị unmount | hero dùng `keepMounted=true`, chia sẻ node DOM qua các scene |
| Thanh phụ đề/Card dữ liệu tự mình fade in fade out | Thanh phụ đề làm phần tử "không phải hero" duy nhất vào cảnh, sau khi hold **phối hợp với chuyển động của hero để cùng rút lui** |

Cấp độ thực thi:
- **Phần tử chia sẻ qua các scene** → Nâng hero lên làm con trực tiếp của `<NarrationStage>`, **không đặt trong bất kỳ `<Scene>` nào**
- Dùng hook `useNarration()` đọc `time`, `scene`, `isCueTriggered` trong hero, tự mình quyết định hình thái theo thời gian hiện tại
- `<Scene>` chỉ dùng để quản lý các phần tử phụ chỉ xuất hiện trong đoạn đó (card dữ liệu, khối trích dẫn v.v.), và **các phần tử phụ này cũng đừng cắt cứng**——vào cảnh dùng expoOut + stagger, rút lui dùng fade overlap đè lên đoạn tiếp theo

### Điều 3 · Mỗi một khung hình màn hình đều phải có chuyển động

**Phương pháp tự kiểm tra**: Chụp màn hình **bất kỳ một khung hình nào** trong lúc ghi hình (Không phải giây kích hoạt cue).
- Nếu màn hình trông có vẻ "**hoàn toàn đứng yên**" → Sai. Quay lại thêm chuyển động lớp đáy (background drift / hero subtle scale / camera pan / parallax)
- Luôn luôn có một **chuyển động lớp đáy** đang chạy (dù không phải tiêu điểm):
  - `scale: 1 ↔ 1.02` của hero element vòng lặp nhịp thở 5 giây
  - Nền `translateX: 0 ↔ -20px` trôi chậm
  - Card dữ liệu sau khi vào cảnh giữ lại rung nhẹ `translateY` (Perlin noise)
- Một màn hình đứng yên hoàn toàn = PowerPoint slop

### Điều 4 · Easing / Stagger / Hold là điểm tựa tối thiểu

| Mục | Bắt buộc | Cấm |
|---|---|---|
| Easing | Trục chính `expoOut` (`cubic-bezier(0.16, 1, 0.3, 1)`), nhấn mạnh `overshoot`, hạ cánh `spring` | `linear`, `ease`, mặc định của CSS |
| Nhiều phần tử vào cảnh | 30ms stagger (mỗi phần tử vào muộn 30ms) | Xuất hiện đồng loạt |
| Trước cue mấu chốt | hold 0.3-0.5s để khán giả "nhìn thấy" (phần tử đoạn trước đứng yên 0.3s rồi mới kích hoạt cue) | Nói xong một đoạn nối tiếp ngay đoạn sau |
| Kết bài | Dừng đột ngột, khung hình cuối cùng hold 1s | fade to black |

Quy tắc chi tiết tham khảo §1-§4 của `animation-best-practices.md`.

### Tự kiểm tra · Phản ứng của khán giả đầu tiên

Làm xong đưa cho một người chưa từng xem (hoặc chính mình xem lại sau 24 giờ), **phản ứng đầu tiên của họ** là gì?

| Phản ứng | Đánh giá | Hành động |
|---|---|---|
| "Đây là PPT có lồng tiếng" | Thất bại | Quay lại làm lại |
| "Màn hình đi theo âm thanh chuyển đổi" | Chưa đạt | Thiếu tự sự liên tục, hero element không tồn tại hoặc không xuyên suốt |
| "Cái này đang chuyển động" | Đạt | Nhưng không có điểm ghi nhớ |
| "Tôi muốn xem hết" | Khá | Nhịp điệu đúng rồi |
| "Đoạn này tôi muốn chụp màn hình" | Tuyệt vời | Bạn đã làm được |

---

## Quy trình công việc (Cấp cao)

```
                ┌──────────────────────────┐
                │  Bài thuyết minh .md (## │
                │  scene + [[cue:xx]] đánh │
                │  dấu câu mấu chốt)       │
                └──────────────┬───────────┘
                               │
                  narrate-pipeline.mjs
                               │
                               ▼
            ┌──────────────────────────────┐
            │ voiceover.mp3 (toàn đoạn ghép│
            │ timeline.json (thời lượng đo)│
            └──────────────┬───────────────┘
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
    ┌─────────────────┐      ┌──────────────────┐
    │ Animation HTML  │      │ Ghi MP4 + Trộn âm│
    │ (NarrationStage)│      │ render-narration │
    │ Phát thực tế    │      │ → MP4 phát hành  │
    └─────────────────┘      └──────────────────┘
       Dạng giao 1               Dạng giao 2
```

## Định dạng bài thuyết minh

Đặt ở bất kỳ vị trí nào dưới thư mục dự án, tên file khuyến nghị `script.md`:

```markdown
---
title: LLM là gì
voice: S_JSdgdWk22   # Tùy chọn, ghi đè voice mặc định của .env
speed: 1.0           # Tùy chọn, 0.5-2.0
gap: 0.4             # Số giây im lặng giữa các đoạn, mặc định 0.3
---

## intro
Xin chào mọi người, hôm nay chúng ta sẽ giải thích rõ ràng LLM là gì trong 5 phút.

## what-is
LLM tên đầy đủ là Large Language Model, [[cue:bigmodel]]nó là một mạng thần kinh có hàng trăm tỷ tham số.
Bản chất là một bộ dự đoán nối từ văn bản.

## demo
Ví dụ khi bạn nhập "Thời tiết hôm nay", [[cue:input]]model sẽ dự đoán từ tiếp theo có khả năng nhất là gì.
[[cue:predict]]Có thể là "rất tốt", có thể là "bình thường".
```

**Quy tắc**:
- Tiêu đề đoạn `## scene-id` là Tiếng Anh/Số + Dấu gạch nối (như `## what-is`, `## scene-1`)
- `[[cue:xx]]` đánh dấu ở **giữa câu mấu chốt**——script khi chạy sẽ cắt văn bản tại vị trí đó, khoảnh khắc sau cue chính là điểm kích hoạt của màn hình
- cue id trong animation HTML dùng `<Cue id="xx">` để lắng nghe
- Khi viết lời thuyết minh **chú ý nhịp điệu + câu ngắn**, câu dài khi xuất TTS sẽ bị phẳng lặng

## Schema của timeline.json

```ts
{
  title: string,
  voice: string | null,
  speed: number,
  gap: number,
  totalDuration: number,        // Số giây đo đạc thực tế của toàn bộ voiceover.mp3
  voiceover: 'voiceover.mp3',   // Đường dẫn tương đối so với timeline.json
  scenes: [
    {
      id: string,
      start: number,            // Thời gian bắt đầu của đoạn này trong toàn bộ âm thanh
      end: number,
      duration: number,
      audio: 'audio/<id>.mp3',  // Âm thanh riêng của đoạn này (các đoạn con trước khi gộp đã concat)
      text: string,             // Toàn bộ văn bản đã bóc tách nhãn [[cue:xx]]
      // chunks là nguồn hiển thị phụ đề——mỗi chunk là đoạn con bị cue cắt ra, chứa cửa sổ thời gian TTS đo thực tế
      chunks: [
        {
          text: string,            // Văn bản đoạn con
          start: number,           // Thời gian tương đối trong đoạn
          end: number,
          absoluteStart: number,   // Thời gian tuyệt đối trên toàn track (Căn chỉnh voiceover.mp3)
          absoluteEnd: number,
          // words: Mã thời gian cấp từ (TTS enable_subtitle đo đạc trả về thực tế, mặc định có; --no-timestamps tắt)
          // Lưu ý text là văn bản sau TN ("2025"→"hai không hai lăm"), dấu câu gắn vào từ phía trước
          words: [
            { text: string, start: number, end: number, absoluteStart: number, absoluteEnd: number }
          ],
        }
      ],
      cues: [
        {
          id: string,
          offset: number,       // Thời gian tương đối trong đoạn
          absoluteTime: number, // Thời gian tuyệt đối trên timeline toàn bộ thời gian
        }
      ]
    }
  ]
}
```

`absoluteTime` và `absoluteStart/End` đều là **đo đạc thực tế ra**——pipeline cắt văn bản trong đoạn thành các đoạn con theo cue rồi mới cho TTS riêng biệt, thời gian = cộng dồn thời lượng đo đạc thực tế của các đoạn con phía trước. **Không phải giá trị xấp xỉ ước tính tuyến tính theo số ký tự**.

## Phụ đề (Subtitles)

> **Phụ đề là mặc định mang theo**——Video thuyết minh dài không có phụ đề, tỷ lệ giữ chân sẽ giảm đáng kể. NarrationStage cung cấp `<Subtitles />` mở hộp dùng ngay.

### Cách dùng (Một dòng)

```jsx
const { NarrationStage, Subtitles } = NarrationStageLib;
<NarrationStage timeline={TIMELINE} audioSrc="...">
  {/* Nội dung hero / scene của bạn */}
  <Subtitles />  {/* ← Tự động lấy văn bản hoạt động từ timeline.scenes[].chunks */}
</NarrationStage>
```

### Quy tắc thị giác (Phong cách Bilibili · Anti-PowerPoint)

| Mục | Quy tắc | Ví dụ phản diện |
|---|---|---|
| Nền | **Không có nền** (Không dùng thanh ngang màu đen không dùng backdrop-blur) | Nền đen bán trong suốt + blur = Thanh phụ đề đè lên màn hình = Cảm giác PPT |
| Màu chữ | **Nền sáng dùng mực đậm `#1a1a1a` + Hào quang trắng**; Nền tối dùng chữ trắng + Hào quang đen | Nền sáng chữ trắng + viền đen = Chữ bị mờ |
| Cỡ chữ | 32px (Video 1080p) | <24px nhìn không rõ, >40px tranh thị giác chính |
| Font chữ | `PingFang SC` / `Noto Sans SC` (Không nét chân, tiêu chuẩn Bilibili) | Font có nét chân = Giống phụ đề phim |
| Vị trí | bottom: 90px (Không dán mép) | Dán mép dưới nhìn rẻ tiền |
| Độ dài dòng đơn | **≤ 12-13 chữ** (Trộn Trung Anh thì Tiếng Anh tính 0.5 chữ) | >15 chữ một dòng trên điện thoại đọc không hết |
| Quy tắc cắt câu | **Tuyệt đối không cắt ngang dấu chấm**: Cắt câu theo `。！？` trước, mỗi câu gộp theo `，、；：` đến ≤maxLen | Cắt cứng theo số chữ, cắt "đây là tốt" thành "đây là t" + "ố" |

`<Subtitles />` mặc định chạy theo quy tắc trên, không cần truyền props. Kịch bản nền tối: `<Subtitles color="#fff" haloColor="rgba(0,0,0,0.85)" />`.

### Chế độ Karaoke (Highlight cấp từ)

```jsx
<Subtitles karaoke />                          {/* Đọc đến từ nào từ đó đổi sang cam thương hiệu #e8590c */}
<Subtitles karaoke karaokeColor="#0a84ff" />   {/* Tùy chỉnh màu highlight */}
```

- Phụ thuộc vào mã thời gian cấp từ `words` trong timeline chunks (narrate-pipeline.mjs mặc định xuất ra; Đậu Bao TTS v3 `enable_subtitle`, cần tài nguyên 2.0, chỉ hỗ trợ Trung Anh)
- Hiển thị toàn dòng, đổi màu từng từ, phân chia dòng tái sử dụng ≤maxLen + Quy tắc không cắt ngang dấu chấm (Ghép dòng bởi words, căn chỉnh nghiêm ngặt với phát âm)
- Khi chunk không có words sẽ tự động rớt về chế độ chunk thông thường, bên gọi không cần tự đánh giá

### Thuật toán cắt câu (Đã内置 trong narration_stage.jsx)

```js
splitChunkToLines(text, maxLen = 13)
// 1. Cắt câu theo dấu câu mạnh (。！？\n)
// 2. Mỗi câu ≤ maxLen giữ lại trực tiếp
// 3. Nếu không sẽ cắt nhỏ theo dấu câu yếu (，、；：), gộp lại đến ≤ maxLen
// 4. Cắt cứng兜底 (Hiếm gặp)
// Trộn Trung Anh: Tiếng Anh/Số tính 0.5 chữ theo chiều rộng thị giác
```

Nếu chunk sau khi cắt xong có dòng rõ ràng quá dài hoặc quá ngắn, **hãy sửa vị trí cue trong bài thuyết minh** (cue cắt đoạn nhỏ hơn nữa), đừng điều chỉnh logic cắt câu ở frontend.

## NarrationStage API

```jsx
import 'assets/narration_stage.jsx';
const { NarrationStage, Scene, Cue, useNarration } = NarrationStageLib;

<NarrationStage
  timeline={TIMELINE}                  // Nội dung timeline.json
  audioSrc="_narration/voiceover.mp3"  // Đường dẫn tương đối so với HTML hiện tại
  width={1920} height={1080}
  background="#f5f1e8"
  controls={true}                      // Hiển thị thanh phát phía dưới khi phát thực tế
>
  {/* hero element: Tồn tại liên tục qua scene —— Trực tiếp đặt ở cấp con của NarrationStage */}
  <HeroAnchor />

  {/* Phần tử phụ trong scene: Chỉ xuất hiện trong đoạn đó */}
  <Scene id="intro">
    <Cue id="bigmodel">{(triggered, progress) => (
      <SomeElement style={{ opacity: progress }} />
    )}</Cue>
  </Scene>
</NarrationStage>
```

**Hooks**:
- `useNarration()` trả về `{ time, scene, sceneTime, isCueTriggered, cueProgress }`
- Đọc trực tiếp trong custom component, không cần truyền props

**Scene Component**:
- Mặc định chỉ mount khi `scene.id === id`
- Thêm `keepMounted` để mount liên tục (Dùng khi animation qua các scene liên tục)

**Cue Component**:
- children bắt buộc phải là `(triggered, progress) => ReactNode`
- progress là giá trị tăng dần 0→1 sau khi cue kích hoạt (Mặc định ramp 0.6s)

## Nguồn thời gian (Track đôi)

NarrationStage tự động phát hiện `window.__recording`:
- **Chế độ phát thực tế** (Mặc định): Đi theo currentTime của phần tử audio, người dùng tạm dừng/kéo seek đều có thể đồng bộ
- **Chế độ ghi video** (render-video.js đặt `window.__recording = true`): Đồng hồ rAF tự điều khiển bắt đầu từ 0, lộ ra `window.__seek(t)` cho render-video.js reset

## Ba Script

| Script | Đầu vào | Đầu ra |
|---|---|---|
| `scripts/cloud/tts-doubao.mjs` | Văn bản đơn đoạn | mp3 đơn lẻ + Thời lượng đo đạc thực tế |
| `scripts/narrate-pipeline.mjs` | Bài thuyết minh .md | voiceover.mp3 + timeline.json |
| `scripts/mix-voiceover.sh` | Video + voiceover.mp3 [+ BGM] | MP4 có âm thanh |
| `scripts/render-narration.sh` | HTML thuyết minh + timeline.json | MP4 cuối cùng (Ghi hình + Trộn âm trọn gói) |

## Cấu hình .env

> ⚠️ TTS là năng lượng đám mây tùy chọn: Văn bản bài thuyết minh sẽ được gửi tới interface chính thức của Đậu Bao TTS (openspeech.bytedance.com),
> sử dụng key của chính bạn. Lần gọi đầu tiên của script cần `--yes` hoặc `HUASHU_CLOUD_OK=1` để xác nhận rõ ràng,
> endpoint bắt buộc kiểm tra whitelist domain chính thức của Bytedance. Tuyên bố luồng dữ liệu xem `SECURITY.md` ở gốc kho lưu trữ.

`.env` dưới thư mục gốc của skill (Đã gitignore):

```
DOUBAO_TTS_API_KEY=<your_api_key>
DOUBAO_TTS_VOICE_ID=zh_female_xiaohe_uranus_bigtts
DOUBAO_TTS_ENDPOINT=https://openspeech.bytedance.com/api/v3/tts/unidirectional
```

Cũng có thể sử dụng App ID + Access Token của console để xác thực:

```
DOUBAO_APP_ID=<your_app_id>
DOUBAO_ACCESS_KEY=<your_access_token>
DOUBAO_TTS_VOICE_ID=zh_female_xiaohe_uranus_bigtts
```

`DOUBAO_TTS_RESOURCE_ID` mặc định tự động suy luận theo voice: Giọng clone `S_` dùng `seed-icl-1.0`, giọng chính thức `uranus` dùng `seed-tts-2.0`, các giọng chính thức khác dùng `seed-tts-1.0`.

## Quy trình công việc tiêu chuẩn (10 bước)

1. **Viết bài thuyết minh**: Bài thuyết minh là mã nguồn. Viết hoàn chỉnh toàn bộ đoạn lời nói trước, đánh dấu tiêu đề đoạn `## scene-id`, thêm `[[cue:xx]]` trước câu mấu chốt
2. **Chạy narrate-pipeline**: `node scripts/narrate-pipeline.mjs --script script.md --out-dir _narration --yes` (`--yes`=Xác nhận gửi văn bản tới Đậu Bao TTS)
3. **Nghe toàn bộ voiceover.mp3**: Nhịp điệu không đúng quay lại sửa bài. **Bước này quyết định giới hạn trên chất lượng toàn bộ phim**
4. **🛑 Trả lời quy tắc sắt trước khi thiết kế**: hero element là cái gì? Trạng thái của nó trong mỗi đoạn là gì? Morph qua các scene như thế nào? Không trả lời được đừng viết code
5. **Viết animation HTML**: Dùng NarrationStage + Một hoặc vài hero element diễn kịch qua các scene
6. **Xem trước phát thực tế**: Trình duyệt mở HTML, bấm ▶ Play, nghe hình ảnh+thuyết minh đồng bộ
7. **Tự kiểm tra bằng khán giả đầu tiên**: Dùng bảng "Tự kiểm tra · Phản ứng của khán giả đầu tiên" ở trên để chấm điểm. Thất bại quay lại Step 4 làm lại
8. **Ghi video**: `bash scripts/render-narration.sh demo.html --timeline=_narration/timeline.json` (Tự động ghi MP4 không tiếng + Trộn voiceover vào)
9. **BGM tùy chọn**: Thêm `--bgm-mood=educational` (Hoặc tech / tutorial v.v.) vào render-narration
10. **Giao hàng**: Trình duyệt HTML (Dùng cho demo thời gian thực) + MP4 cuối cùng (Dùng để phát hành)

## Xử lý ngoại lệ

| Vấn đề | Giải quyết |
|---|---|
| Lỗi TTS API | Kiểm tra `DOUBAO_TTS_API_KEY` trong .env, hoặc `DOUBAO_APP_ID` + `DOUBAO_ACCESS_KEY` có đúng không |
| Một đoạn âm thanh rõ ràng dài/ngắn hơn kịch bản | Văn bản đoạn đó có dấu câu kỳ lạ hoặc emoji, TTS phân tích bất thường → Sửa kịch bản |
| absoluteTime của cue không chuẩn | Khi ghép các đoạn con trong đoạn ffmpeg có vấn đề → Kiểm tra tính thống nhất mã hóa mp3 |
| Ghi video bị màn hình đen | render-video.js không nhận được tín hiệu `window.__ready` → Kiểm tra NarrationStage có mount bình thường không |
| Ghi video màn hình bị giật | Trong animation có tái bố cục nặng (Rất nhiều box-shadow / blur) → Đơn giản hóa hoặc tiền tổng hợp |
| Âm hình không đồng bộ khi phát thực tế | Độ trễ tải phần tử audio → Thêm `preload="auto"` hoặc tiền tải cục bộ |

## Khi nào KHÔNG DÙNG pipeline này

- **Animation ngắn <60s**: Trực tiếp làm animation không tiếng + Lồng tiếng hậu kỳ (`add-music.sh` + một đoạn TTS riêng) là được, không cần timeline điều khiển
- **Video BGM thuần túy**: Dùng `add-music.sh` thêm BGM thiết lập sẵn
- **Dùng ghi âm người thật thay thế TTS**: Thay thế `voiceover.mp3` thành ghi âm người thật, timeline tự viết tay hoặc dùng ffprobe đo thời lượng đoạn + script công cụ tạo ra → Các phần còn lại của quy trình là通用

---

**Nhắc nhở lần cuối**: Trước khi viết code hãy quay lại quy tắc sắt. **Đừng làm PowerPoint có lồng tiếng**.
