# Cinematic Patterns · Best Practice cho Workflow Demo

> 5 mẫu pattern then chốt để nâng cấp từ "Hoạt ảnh PPT" lên "Cinematic cấp độ sự kiện ra mắt".
> Cô đọng từ thực chiến 2 cinematic demo (Nuwa workflow + Darwin workflow) trong slide deck "Trò chuyện về skill" tháng 04/2026, đã kiểm tra thực tế có thể tái hiện.

---

## 0 · Tài liệu này giải quyết vấn đề gì

Khi bạn cần làm một "hoạt ảnh demo trình diễn một quy trình công việc" (Kịch bản điển hình: Quy trình công việc của skill, Onboarding sản phẩm, Quy trình gọi API, Thực thi tác vụ agent), có 2 cách làm phổ biến:

| Mẫu hình (Paradigm) | Trông như thế nào | Hậu quả |
|---|---|---|
| **Hoạt ảnh PPT** (Dở) | step 1 fade in → step 2 fade in → step 3 fade in, 4 ô xếp ngang cùng màn hình | Khán giả cảm thấy "Chỉ là một cái PPT thêm hiệu ứng fade", không có khoảnh khắc wow |
| **Cinematic** (Tốt) | Dựa trên Scene, mỗi lần chỉ focus một việc, giữa các scene là dissolve / focus pull / morph | Khán giả cảm thấy "Đây là một phân cảnh sự kiện ra mắt sản phẩm", muốn chụp màn hình chia sẻ |

Gốc rễ của sự khác biệt **không nằm ở kỹ thuật hoạt ảnh**, mà nằm ở **mẫu hình tự sự**. Tài liệu này hướng dẫn cách nâng cấp từ loại trước lên loại sau.

---

## 1 · 5 mẫu pattern cốt lõi

### Pattern A · Cấu trúc 2 tầng Dashboard + Cinematic Overlay

**Vấn đề**: Bản cinematic thuần túy mặc định là màn hình đen + một nút ▶, nếu người dùng lật đến trang này mà không bấm thì không thấy gì cả.

**Giải pháp**:
```
Trạng thái DEFAULT (Luôn hiển thị): Dashboard quy trình tĩnh hoàn chỉnh
  └── Khán giả nhìn một phát thấy ngay skill / quy trình công việc này chạy thế nào

Kích hoạt POINT ▶ (overlay nổi lên): Cinematic 22 giây
  └── Chạy xong tự động fade quay về DEFAULT
```

**Các điểm hiện thực hóa**:
- `.dash` mặc định visible, `.cinema` mặc định `opacity: 0; pointer-events: none`
- `.play-cta` là nút nhỏ màu vàng kim ở góc dưới bên phải (Không phải lớp phủ lớn ở trung tâm)
- Nhấp chuột → `cinema.classList.add('show')` + `dash.classList.add('hide')`
- Dùng `requestAnimationFrame` chạy một lần (Không phải vòng lặp), kết thúc `endCinematic()` đảo ngược trạng thái

**Mẫu phản diện**: Mặc định = Nút ▶ lớn ở trung tâm đè lên tất cả, trước khi nhấp trang web trắng tinh.

---

### Pattern B · Dựa trên Scene (Scene-based), KHÔNG phải dựa trên Step (Step-based)

**Vấn đề**: Chia hoạt ảnh thành "step 1 hiển thị → step 2 hiển thị → ..." chính là tư duy PPT.

**Giải pháp**: Chia thành 5 scene, mỗi scene là một **ống kính độc lập**, toàn màn hình chỉ focus vào một việc:

| Loại Scene | Nhiệm vụ | Thời lượng |
|---|---|---|
| 1 · Invoke | Kích hoạt bởi đầu vào người dùng (Typewriter trong terminal)| 3-4s |
| 2 · Process | Trực quan hóa quy trình làm việc cốt lõi (Ngôn ngữ thị giác độc đáo)| 5-6s |
| 3 · Result/Insight | Sản phẩm then chốt cô đọng được (Trực quan hóa)| 4-5s |
| 4 · Output | Trình diễn sản phẩm thực tế (Tệp / diff / con số)| 3-4s |
| 5 · Hero Reveal | Khoảnh khắc hero kết thúc (Chữ lớn + Tuyên ngôn giá trị)| 4-5s |

**Tổng thời lượng ≈ 22 giây** —— Đây là độ dài vàng đã qua thử nghiệm:
- Ngắn hơn 18 giây: PM chưa kịp vào trạng thái đã kết thúc
- Dài hơn 25 giây: Mất kiên nhẫn
- 22 giây vừa đủ để "Bắt lấy → Triển khai → Thu nạp → Để lại ấn tượng"

**Các điểm hiện thực hóa**:
- `T = { DURATION: 22.0, s1_in: [0, 0.7], s2_in: [3.8, 4.6], ... }` Trục thời gian toàn cục
- Một `requestAnimationFrame(render)` duy nhất chạy tính toán opacity / transform của tất cả scene
- Đừng dùng chuỗi setTimeout (Dễ bị đứt, khó debug)
- Easing bắt buộc dùng `expoOut` / `easeOut` / cubic-bezier, **CẤM dùng linear**

---

### Pattern C · Ngôn ngữ thị giác của mỗi demo phải độc lập

**Vấn đề**: Làm xong cinematic thứ nhất, khi làm cái thứ hai lười biếng tái sử dụng cùng một template (Cùng kiểu orbit + pentagon + typewriter + chữ lớn hero), chỉ thay chữ.

**Hậu quả**: Khán giả phát hiện hai skill "nhìn y hệt nhau", đồng nghĩa với việc nói rằng "Hai skill này chẳng có gì khác biệt".

**Giải pháp**: Ẩn dụ cốt lõi của mỗi quy trình công việc là khác nhau, nên ngôn ngữ thị giác bắt buộc phải khác nhau.

**Ca đối chiếu**:

| Chiều kích | Nuwa (Người cô đọng) | Darwin (Tối ưu skill) |
|---|---|---|
| Ẩn dụ cốt lõi | Thu thập → Cô đọng → Viết | Vòng lặp → Đánh giá → Bánh răng |
| Chuyển động thị giác | Trôi bồng bềnh / Bức xạ / pentagon | Vòng lặp / Thăng tiến / So sánh |
| Scene 2 | 3D Orbit · 8 tệp hồ sơ trôi trên hình elip thấu thị | Spin Loop · Token chạy 5 vòng quanh nhẫn 6 nút |
| Scene 3 | Pentagon · 5 token bức xạ từ trung tâm | v1 vs v5 · Diff song song (Bản đỏ vs Bản vàng) |
| Scene 4 | Typewriter SKILL.md | Hill-Climb · Vẽ đường cong toàn màn hình |
| Scene 5 hero | Chữ Serif in nghiêng lớn "21 phút" | Bánh răng xoay ⚙ + Nhãn vàng "KEPT +1.1" |

**Tiêu chuẩn nhận định**: Che chữ lại, chỉ nhìn thị giác, có phân biệt được đây là demo nào không? Không phân biệt được tức là lười biếng.

---

### Pattern D · Dùng tư liệu thật AI tạo ra, không dùng emoji hay SVG vẽ tay

**Vấn đề**: Trong 3D orbit / gallery cần mảnh tư liệu trôi bồng bềnh, dùng emoji (📚🎤) xấu và không có thương hiệu, SVG vẽ gáy sách tay bao giờ nhìn cũng không giống sách thật.

**Giải pháp**: Dùng `huashu-gpt-image` chạy một lưới ảnh lớn 4×2 (8 đồ vật liên quan đến chủ đề · Nền trắng · 60px breathing space · Unified style), dùng `extract_grid.py --mode bbox` tách thành 8 ảnh PNG trong suốt độc lập.

**Các điểm Prompt** (Prompt patterns chi tiết xem skill `huashu-gpt-image`):
- Neo giữ IP ("1960s Caltech archive aesthetic" / "Hearthstone-style consistent treatment")
- Nền trắng (Thuận tiện tách ảnh, nền xám bầu không khí tốt nhưng tách nền trong suốt khó)
- 4×2 Đừng dùng 5×5 (Tránh bug nén dòng cuối)
- Persona finishing ("You are a Wired magazine curator preparing an exhibition photo")

**Mẫu phản diện**: Dùng emoji làm icon, dùng bóng CSS thay thế cho ảnh sản phẩm.

---

### Pattern E · Chế độ 2 đường tiếng BGM + SFX

**Vấn đề**: Chỉ có hoạt ảnh mà không có âm thanh, tiềm thức khán giả cảm thấy "Cái này giống như một cái demo nghèo nàn".

**Giải pháp**: Nhạc nền BGM dài + 11 cue âm hiệu SFX.

**Công thức cue SFX thông dụng** (Áp dụng cho demo quy trình công việc):

| Thời điểm | SFX | Kịch bản kích hoạt |
|---|---|---|
| 0.10s | whoosh | Terminal nhô lên từ phía dưới |
| 3.0s | enter | Typewriter hoàn thành, nhấn enter |
| 4.0s | slide-in | Phần tử scene 2 vào cảnh |
| 5-9s × 5 lần | sparkle | Nút quá trình then chốt (Mỗi thế hệ / Mỗi token / Mỗi điểm dữ liệu)|
| 14s | click | Chuyển sang output scene |
| 17.8s | logo-reveal | Khoảnh khắc hero reveal |
| typewriter | type | Cứ 2 ký tự kích hoạt 1 lần (Mật độ đừng quá cao)|

**Cách ly dải tần**: BGM volume 0.32 (Nền tần số thấp), SFX volume 0.55 (Nhát đấm tần số trung cao), sparkle 0.7 (Cần nổi bật), logo-reveal 0.85 (Khoảnh khắc hero mạnh nhất).

**Khống chế của người dùng**:
- Bắt buộc có lớp phủ kích hoạt ▶ (Ràng buộc autoplay trình duyệt)
- Nút mute nhỏ góc trên bên phải (Người dùng chuyển giữ im lặng bất kỳ lúc nào)
- Đừng làm thành "Lật đến trang này là ép buộc phát tiếng"

---

## 2 · Điểm then chốt Thiết kế Dashboard Tĩnh

Dashboard là Layer 1 của cấu trúc 2 tầng, PM không nhấp ▶ cũng có thể hiểu được skill này.

**Bố cục**: Lưới 3 cột (Hoặc 1 lớn + 2 nhỏ), mỗi panel giải quyết một vấn đề:

| Loại Panel | Giải quyết vấn đề gì | Ví dụ |
|---|---|---|
| **Pipeline / Flow Diagram** | "Quy trình làm việc của skill này là gì?"| Nuwa 4 giai đoạn pipeline · Darwin autoresearch loop |
| **Snapshot / State** | "Dữ liệu thật chạy ra trông như thế nào?"| Darwin snapshot 8 chiều rubric |
| **Trajectory / Evolution** | "Sau nhiều lần chạy thay đổi ra sao?"| Darwin 5 thế hệ đường cong hill-climb |
| **Examples / Gallery** | "Đã xuất ra được những thứ gì rồi?"| Nuwa 21 personas gallery |
| **Strip · Example I/O** | "Đầu vào cái gì → Đầu ra cái gì"| Nuwa example strip: `› nuwa 蒸馏 费曼 → feynman.skill (21 min)` |

**Ràng buộc then chốt**:
- Mật độ thông tin phải đủ (Mỗi panel đều phải gánh thông tin khác biệt)
- Nhưng không được nhét data slop (Mỗi con số đều phải có ý nghĩa)
- Phối màu thống nhất với cinematic (Cùng hệ màu, chuyển đổi không bị đường đột)

---

## 3 · Công cụ Debug và Phát triển

Bất kỳ hoạt ảnh dài nào cũng bắt buộc phải trang bị 3 công cụ dev, nếu không debug sẽ bị nổ tung.

### Công cụ 1 · `?seek=N` Đóng băng tại giây thứ N

```js
const seek = parseFloat(params.get('seek'));
if (!isNaN(seek)) {
  started = true; muted = true;
  frozenT = seek;  // render() dùng t này chứ không dùng elapsed
  cinema.classList.add('show'); dash.classList.add('hide');
}

// Trong render():
let t = frozenT !== null ? frozenT : (elapsed % T.DURATION);
```

Cách dùng: `http://.../slide.html?seek=12` Trực tiếp xem hình ảnh giây thứ 12, không cần chờ phát.

### Công cụ 2 · `?autoplay=1` Bỏ qua overlay ▶

Tiện cho Playwright tự động chụp màn hình test, cũng tiện cho việc ép kích hoạt khi nhúng vào iframe.

### Công cụ 3 · Nút REPLAY thủ công

Nút nhỏ góc trên bên phải, người dùng/khi debug có thể phát lại bất kỳ số lần nào. CSS:

```css
.replay{position:absolute;top:18px;right:18px;background:rgba(212,165,116,0.1);
  border:1px solid rgba(212,165,116,0.3);color:#D4A574;
  font-family:monospace;font-size:10px;letter-spacing:.28em;text-transform:uppercase;
  padding:6px 12px;border-radius:1px;cursor:pointer;backdrop-filter:blur(6px);z-index:6}
```

---

## 4 · Bẫy khi nhúng iframe (Nếu cinematic nhúng trong deck)

### Bẫy 1 · Click zone của cửa sổ cha đánh chặn nút bấm bên trong iframe

Nếu index.html của deck thêm "Click zone trong suốt 22vw trái phải để lật trang", sẽ **đè lên nút ▶ play bên trong iframe** —— Người dùng nhấp nút bấm bị nuốt thành "Trang tiếp theo".

**Sửa chữa**: Click zone thêm `top: 12vh; bottom: 25vh`, chừa ra 25% đỉnh và đáy không đánh chặn, để nút ▶ ở trung tâm và nút ▶ ở góc dưới bên phải bên trong iframe đều nhấp được.

### Bẫy 2 · iframe cướp focus làm mất sự kiện bàn phím

Sau khi người dùng nhấp vào iframe, focus nằm trong iframe, sự kiện bàn phím ←/→ của cửa sổ cha không nhận được nữa.

**Sửa chữa**:
```js
iframe.addEventListener('load', () => {
  // Inject bộ chuyển tiếp bàn phím
  const doc = iframe.contentDocument;
  doc.addEventListener('keydown', (e) => {
    window.dispatchEvent(new KeyboardEvent('keydown', { key: e.key, ... }));
  });
  // Sau khi nhấp lôi focus quay lại cửa sổ cha
  doc.addEventListener('click', () => setTimeout(() => window.focus(), 0));
});
```

### Bẫy 3 · Khác biệt hành vi giữa file:// vs https://

Cinematic test tốt ở file:// máy cục bộ triển khai lên có thể bị lỗi, vì:
- Dưới file:// iframe contentDocument cùng nguồn (same-origin)
- Dưới https:// cũng cùng nguồn (Nếu cùng host), nhưng hạn chế autoplay âm thanh nghiêm ngặt hơn

**Sửa chữa**:
- Trước khi triển khai dùng `python3 -m http.server` bật HTTP cục bộ test một lượt
- BGM bắt buộc chờ sau khi người dùng nhấp ▶ mới `bgm.play()`, đừng phát ngay khi page-load

---

## 5 · Bảng tra nhanh Mẫu phản diện

| ❌ Mẫu phản diện | ✅ Mẫu đúng |
|---|---|
| Mặc định = Màn hình đen overlay ▶ | Mặc định = Dashboard tĩnh, ▶ là bổ trợ |
| 4 step xếp ngang cùng màn hình fade in | 5 scene chuyển đổi toàn màn hình, mỗi màn chỉ focus một việc |
| Tái sử dụng template đổi chữ làm demo khác | Mỗi demo ngôn ngữ thị giác độc lập (Che chữ vẫn phân biệt được) |
| emoji / SVG vẽ tay làm tư liệu | Ảnh lớn gpt-image-2 + extract_grid tách ảnh |
| Không BGM không SFX | Chế độ 2 đường tiếng BGM + 11 SFX cues |
| Dùng chuỗi setTimeout để schedule | requestAnimationFrame + Đối tượng T trục thời gian toàn cục |
| Hoạt ảnh linear | Expo / cubic-bezier easing |
| Không có công cụ dev | `?seek=N` + `?autoplay=1` + Nút REPLAY |
| Nút bên trong iframe bị click zone của cha nuốt | Click zone thêm margin top/bottom nhường chỗ cho nút bấm |

---

## 6 · Ngân sách Thời gian

Theo bộ pattern này, một cinematic demo hoàn chỉnh (Gồm cả dashboard):

| Nhiệm vụ | Thời gian |
|---|---|
| Thiết kế tự sự 5-scene + Ngôn ngữ thị giác | 30 phút (Cần thận trọng, quyết định tính độc lập)|
| Bố cục tĩnh Dashboard + Nội dung | 1 giờ |
| Thực hiện 5 scenes Cinematic | 1.5 giờ |
| Chỉnh nhịp thời gian Audio cues + Nút replay | 30 phút |
| Playwright chụp màn hình xác thực 5 khoảnh khắc then chốt | 15 phút |
| **Tổng cộng cho một demo** | **3-4 giờ** |

Demo thứ hai tái sử dụng framework nhưng **ngôn ngữ thị giác bắt buộc phải độc lập**, thời gian khoảng 2-3 giờ.
