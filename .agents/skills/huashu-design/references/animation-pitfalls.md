# Animation Pitfalls: Những Bài Học và Quy Tắc Bẫy Cần Tránh Khi Làm Hoạt Ảnh HTML

Các bug thường gặp nhất khi làm hoạt ảnh và cách phòng tránh. Mỗi quy tắc đều đến từ ca thất bại thực tế.

Đọc xong tệp này trước khi viết hoạt ảnh có thể tiết kiệm một vòng lặp làm lại.

## 1. Bố cục lồng lớp (Stacking Layout) —— `position: relative` là nghĩa vụ mặc định

**Bẫy đạp phải**: Một phần tử sentence-wrap bọc 3 bracket-layer (`position: absolute`). Không đặt `position: relative` cho sentence-wrap, kết quả các bracket absolute lấy `.canvas` làm hệ tọa độ, trôi ra ngoài màn hình ở phía dưới 200px.

**Quy tắc**:
- Bất kỳ container nào chứa phần tử con `position: absolute`, **bắt buộc** phải khai báo rõ `position: relative`
- Ngay cả khi về mặt thị giác không cần "dịch chuyển" (offset), cũng phải viết `position: relative` làm điểm neo hệ tọa độ
- Nếu bạn đang viết `.parent { ... }`, mà phần tử con có `.child { position: absolute }`, hãy theo bản năng thêm relative cho parent
- **Kiểm tra nhanh**: Mỗi khi xuất hiện một `position: absolute`, đếm ngược lên các ancestor, đảm bảo tổ tiên được positioned gần nhất là hệ tọa độ mà bạn *mong muốn*.

## 2. Bẫy ký tự —— Không phụ thuộc vào Unicode hiếm

**Bẫy đạp phải**: Muốn dùng `␣` (U+2423 OPEN BOX) để trực quan hóa "dấu cách token". Noto Serif SC / Cormorant Garamond đều không có glyph này, render ra khoảng trắng/ô vuông rỗng (tofu), khán giả hoàn toàn không nhìn thấy.

**Quy tắc**:
- **Mỗi ký tự xuất hiện trong hoạt ảnh bắt buộc phải tồn tại trong phông chữ bạn đã chọn**
- Danh sách đen ký tự hiếm thường gặp: `␣ ␀ ␐ ␋ ␨ ↩ ⏎ ⌘ ⌥ ⌃ ⇧ ␦ ␖ ␛`
- Để biểu đạt các nguyên ký tự như "Khoảng trắng / Enter / Tab", hãy dùng **hộp ngữ nghĩa dựng bằng CSS**:
  ```html
  <span class="space-key">Space</span>
  ```
  ```css
  .space-key {
    display: inline-flex;
    padding: 4px 14px;
    border: 1.5px solid var(--accent);
    border-radius: 4px;
    font-family: monospace;
    font-size: 0.3em;
    letter-spacing: 0.2em;
    text-transform: uppercase;
  }
  ```
- Emoji cũng cần xác thực: Một số emoji ngoài phông Noto Emoji sẽ fallback thành khung màu xám, tốt nhất nên dùng `emoji` font-family hoặc SVG

## 3. Template Grid/Flex dẫn dắt bởi dữ liệu

**Bẫy đạp phải**: Trong code có `const N = 6` token, nhưng CSS viết cứng `grid-template-columns: 80px repeat(5, 1fr)`. Kết quả token thứ 6 không có column, toàn bộ trận hình bị lệch vị trí.

**Quy tắc**:
- Khi count đến từ mảng JS (`TOKENS.length`), template CSS cũng phải được dẫn dắt bởi dữ liệu
- Phương án A: Dùng biến CSS inject từ JS vào
  ```js
  el.style.setProperty('--cols', N);
  ```
  ```css
  .grid { grid-template-columns: 80px repeat(var(--cols), 1fr); }
  ```
- Phương án B: Dùng `grid-auto-flow: column` để trình duyệt tự động mở rộng
- **CẤM kết hợp "Con số cố định + Hằng số JS"**, N đổi mà CSS không đồng bộ cập nhật

## 4. Đứt gãy quá độ —— Chuyển cảnh phải liên tục

**Bẫy đạp phải**: Giữa zoom1 (13-19s) → zoom2 (19.2-23s), câu chính đã hidden, zoom1 fade out (0.6s) + zoom2 fade in (0.6s) + stagger delay (0.2s+) = Khoảng 1 giây màn hình trắng tinh. Khán giả tưởng hoạt ảnh bị đứng hình.

**Quy tắc**:
- Khi chuyển cảnh liên tục, fade out và fade in phải **chồng chéo đan xen (cross-fade)**, chứ không phải cái trước biến mất hoàn toàn rồi mới bắt đầu cái sau
  ```js
  // Dở:
  if (t >= 19) hideZoom('zoom1');      // 19.0s out
  if (t >= 19.4) showZoom('zoom2');    // 19.4s in → Ở giữa 0.4s màn hình trắng

  // Tốt:
  if (t >= 18.6) hideZoom('zoom1');    // Bắt đầu fade out sớm 0.4s
  if (t >= 18.6) showZoom('zoom2');    // Đồng thời fade in (cross-fade)
  ```
- Hoặc dùng một "Phần tử điểm neo" (như câu chính) làm kết nối thị giác giữa các cảnh, nó hiển thị lại ngắn hạn trong lúc chuyển zoom
- Phối hợp với duration của CSS transition tính cho rõ ràng, tránh việc transition chưa kết thúc đã kích hoạt cái tiếp theo

## 5. Nguyên tắc Pure Render —— Trạng thái hoạt ảnh phải seek được

**Bẫy đạp phải**: Dùng `setTimeout` + `fireOnce(key, fn)` để kích hoạt trạng thái hoạt ảnh theo chuỗi. Phát bình thường thì không sao, nhưng khi ghi hình từng khung/seek tới thời điểm bất kỳ, setTimeout trước đó đã chạy rồi nên không thể "quay lại quá khứ".

**Quy tắc**:
- Hàm `render(t)` về mặt lý tưởng là **pure function**: Đưa vào t xuất ra trạng thái DOM duy nhất
- Nếu bắt buộc dùng tác dụng phụ (như chuyển class), dùng set `fired` phối hợp với reset rõ ràng:
  ```js
  const fired = new Set();
  function fireOnce(key, fn) { if (!fired.has(key)) { fired.add(key); fn(); } }
  function reset() { fired.clear(); /* Xóa tất cả .show class */ }
  ```
- Xuất giao diện `window.__seek(t)` cho Playwright / Debug dùng:
  ```js
  window.__seek = (t) => { reset(); render(t); };
  ```
- setTimeout liên quan đến hoạt ảnh đừng kéo dài >1 giây, nếu không khi seek nhảy ngược lại sẽ bị loạn

## 6. Đo kích thước trước khi phông chữ nạp xong = Đo sai

**Bẫy đạp phải**: Trang vừa DOMContentLoaded đã gọi `charRect(idx)` đo vị trí bracket, phông chữ chưa nạp xong, độ rộng mỗi ký tự là độ rộng của phông fallback, vị trí sai sạch. Chờ phông nạp xong (khoảng 500ms sau), `left: Xpx` của bracket vẫn là giá trị cũ, lệch vĩnh viễn.

**Quy tắc**:
- Bất kỳ code bố cục nào phụ thuộc vào đo đạc DOM (`getBoundingClientRect`, `offsetWidth`), **bắt buộc** phải bọc trong `document.fonts.ready.then()`
  ```js
  document.fonts.ready.then(() => {
    requestAnimationFrame(() => {
      buildBrackets(...);  // Lúc này phông chữ đã sẵn sàng, đo đạc chính xác
      tick();              // Hoạt ảnh bắt đầu
    });
  });
  ```
- Lệnh `requestAnimationFrame` bổ sung cho trình duyệt 1 khung hình thời gian để submit layout
- Nếu dùng Google Fonts CDN, thêm `<link rel="preconnect">` để tăng tốc nạp lần đầu

## 7. Chuẩn bị ghi hình —— Dự phòng điểm bám cho xuất video

**Bẫy đạp phải**: Playwright `recordVideo` mặc định 25fps, bắt đầu ghi ngay từ khi tạo context. Trang nạp, phông nạp 2 giây đầu đều bị ghi vào. Khi giao hàng video bị 2 giây đầu màn hình trắng/nháy trắng.

**Quy tắc**:
- Cung cấp công cụ `render-video.js` xử lý: warmup navigate → reload khởi động lại hoạt ảnh → chờ duration → ffmpeg trim head + chuyển H.264 MP4
- Khung hình **thứ 0** của hoạt ảnh phải là trạng thái ban đầu hoàn chỉnh đã vào vị trí bố cục cuối cùng (Không phải màn hình trắng hay đang nạp)
- Muốn 60fps? Dùng hậu xử lý ffmpeg `minterpolate`, không trông chờ vào tốc độ khung hình nguồn của trình duyệt
- Muốn GIF? Bảng màu 2 giai đoạn (`palettegen` + `paletteuse`), đối với hoạt ảnh 30s 1080p có thể nén xuống 3MB

Xem `video-export.md` để lấy cách gọi script đầy đủ.

## 8. Xuất hàng loạt —— Thư mục tmp bắt buộc kèm PID chống xung đột đồng thời

**Bẫy đạp phải**: Dùng `render-video.js` 3 tiến trình ghi song song 3 HTML. Vì TMP_DIR chỉ đặt tên theo `Date.now()`, 3 tiến trình khởi động cùng miligiây dùng chung 1 thư mục tmp. Tiến trình xong sớm nhất dọn dẹp tmp, hai tiến trình kia khi đọc thư mục bị `ENOENT`, nổ sạch.

**Quy tắc**:
- Bất kỳ thư mục tạm nào nhiều tiến trình có thể dùng chung, đặt tên bắt buộc kèm **PID hoặc hậu tố ngẫu nhiên**:
  ```js
  const TMP_DIR = path.join(DIR, '.video-tmp-' + Date.now() + '-' + process.pid);
  ```
- Nếu thực sự muốn chạy song song nhiều tệp, dùng `&` + `wait` của shell chứ không fork trong 1 script node
- Khi ghi nhiều HTML hàng loạt, cách làm bảo thủ: Chạy **nối tiếp** (Dưới 2 cái có thể song song, trên 3 cái ngoan ngoãn xếp hàng)

## 9. Trong màn hình ghi có thanh tiến độ/nút replay —— Phần tử Chrome làm ô nhiễm video

**Bẫy đạp phải**: HTML hoạt ảnh thêm thanh tiến độ `.progress`, nút replay `.replay`, timestamp `.counter`, tiện cho con người debug khi xem. Khi ghi thành MP4 giao hàng các phần tử này xuất hiện ở đáy video, giống như chụp cả công cụ developer vào.

**Quy tắc**:
- Trong HTML phân biệt rõ "phần tử chrome" dành cho con người dùng (progress bar / replay button / footer / masthead / counter / phase labels) và bản thân nội dung video
- **Quy ước class name** `.no-record`: Bất kỳ phần tử nào mang class này, script ghi màn hình tự động ẩn đi
- Phía script (`render-video.js`) mặc định inject CSS ẩn các class name chrome thường gặp:
  ```
  .progress .counter .phases .replay .masthead .footer .no-record [data-role="chrome"]
  ```
- Dùng `addInitScript` của Playwright để inject (Sẽ có hiệu lực trước mỗi lần navigate, reload vẫn vững)
- Khi muốn xem HTML nguyên bản (có chrome) thì thêm flag `--keep-chrome`

## 10. Mấy giây đầu video hoạt ảnh bị lặp lại —— Rò rỉ khung hình Warmup

**Bẫy đạp phải**: Quy trình cũ của `render-video.js` là `goto → wait fonts 1.5s → reload → wait duration`. Ghi hình bắt đầu từ khi tạo context, giai đoạn warmup hoạt ảnh đã phát một đoạn, sau khi reload phát lại từ t=0. Kết quả mấy giây đầu video là "Đoạn giữa hoạt ảnh + Chuyển cảnh + Hoạt ảnh bắt đầu từ 0", cảm giác lặp lại rất mạnh.

**Quy tắc**:
- **Warmup và Record bắt buộc dùng context độc lập**:
  - Warmup context (Không có tùy chọn `recordVideo`): Chỉ chịu trách nhiệm load url, chờ phông, rồi close
  - Record context (Có `recordVideo`): Bắt đầu ở trạng thái fresh, animation bắt đầu ghi từ t=0
- ffmpeg `-ss trim` chỉ có thể cắt một chút startup latency của Playwright (~0.3s), **không thể** dùng để che đậy khung hình warmup; Nguồn phải sạch
- Đóng context ghi hình = Tệp webm được ghi vào đĩa, đây là ràng buộc của Playwright
- Mẫu code liên quan:
  ```js
  // Phase 1: warmup (dùng xong bỏ)
  const warmupCtx = await browser.newContext({ viewport });
  const warmupPage = await warmupCtx.newPage();
  await warmupPage.goto(url, { waitUntil: 'networkidle' });
  await warmupPage.waitForTimeout(1200);
  await warmupCtx.close();

  // Phase 2: record (mới tinh)
  const recordCtx = await browser.newContext({ viewport, recordVideo });
  const page = await recordCtx.newPage();
  await page.goto(url, { waitUntil: 'networkidle' });
  await page.waitForTimeout(DURATION * 1000);
  await page.close();
  await recordCtx.close();
  ```

## 11. Đừng vẽ "Chrome giả" trong màn hình —— UI player trang trí đụng hàng với Chrome thật

**Bẫy đạp phải**: Hoạt ảnh dùng component `Stage`, đã tự mang scrubber + mã thời gian + nút pause (Thuộc về `.no-record` chrome, tự động ẩn khi xuất). Tôi lại vẽ thêm ở đáy màn hình một thanh tiến độ trang trí kiểu trang tạp chí "`00:60 ──── CLAUDE-DESIGN / ANATOMY`", tự thấy rất đẹp. **Kết quả**: Người dùng nhìn thấy 2 thanh tiến độ — Một cái là bộ điều khiển Stage, một cái là thanh trang trí tôi vẽ. Về thị giác đụng hàng hoàn toàn, bị coi là bug. "Trong video sao lại có thêm một thanh tiến độ nữa?"

**Quy tắc**:

- Stage đã cung cấp: scrubber + mã thời gian + nút pause/replay. **Trong màn hình đừng vẽ thêm** chỉ báo tiến độ, mã thời gian hiện tại, dải chữ ký bản quyền, bộ đếm chương — Chúng hoặc là đụng hàng với chrome, hoặc là filler slop (Vi phạm nguyên tắc "earn its place").
- "Cảm giác số trang", "Cảm giác tạp chí", "Dải chữ ký ở đáy" là những **nhu cầu trang trí** mà AI tự động thêm vào như filler tần suất cao. Mỗi khi xuất hiện phải cảnh giác — Nó có thực sự truyền tải thông tin không thể thay thế không? Hay chỉ đơn thuần lấp đầy khoảng trắng?
- Nếu bạn tin chắc một dải ở đáy nào đó bắt buộc phải tồn tại (Ví dụ: Chủ đề hoạt ảnh chính là giảng về player UI), thì nó phải **bắt buộc cho tự sự**, và **phân biệt rõ ràng về thị giác với Stage scrubber** (Vị trí khác, hình thức khác, tông màu khác).

**Bài test thuộc tính phần tử** (Mỗi phần tử vẽ vào canvas phải trả lời được):

| Nó thuộc về cái gì | Xử lý |
|------------|------|
| Nội dung tự sự của một cảnh nào đó | OK, giữ lại |
| Chrome toàn cục (Dùng để điều khiển/debug) | Thêm class `.no-record`, ẩn khi xuất |
| **Vừa không thuộc về cảnh nào, vừa không phải chrome** | **Xóa**. Đây là vật vô chủ, nhất định là filler slop |

**Tự kiểm tra (3 giây trước khi giao hàng)**: Chụp một bức ảnh tĩnh, tự hỏi —

- Trong màn hình có "Thứ nhìn giống UI video player" (Thanh tiến độ đường ngang, mã thời gian, kiểu dáng nút điều khiển) không?
- Nếu có, xóa nó đi nội dung tự sự có bị tổn hại không? Không tổn hại thì xóa.
- Cùng một loại thông tin (Tiến độ/Thời gian/Chữ ký) có xuất hiện 2 lần không? Gộp lại một chỗ ở chrome.

**Ví dụ phản diện**: Vẽ ở đáy `00:42 ──── PROJECT NAME`, vẽ ở góc dưới bên phải màn hình "CH 03 / 06" đếm chương, vẽ ở mép màn hình số phiên bản "v0.3.1" —— Đều là filler chrome giả.

## 12. Khoảng trắng trước khi ghi屏 + Lệch điểm bắt đầu ghi屏 —— Bẫy 3 vòng `__ready` × tick × lastTick

**Bẫy đạp phải (A · Khoảng trắng phía trước)**: Hoạt ảnh 60 giây xuất MP4, 2-3 giây đầu là trang trắng. `ffmpeg --trim=0.3` không cắt được.

**Bẫy đạp phải (B · Lệch điểm bắt đầu, sự cố thực tế 2026-04-20)**: Xuất video 24 giây, người dùng cảm nhận "Video đến giây 19 mới bắt đầu phát khung hình đầu tiên". Thực tế hoạt ảnh từ t=5 mới bắt đầu ghi, ghi đến t=24 xong loop quay lại t=0, ghi tiếp 5 giây đến end — Cho nên 5 giây cuối của video mới là khởi đầu thực sự của hoạt ảnh.

**Nguyên nhân gốc rễ** (Hai bẫy dùng chung một nguyên nhân gốc rễ):

Playwright `recordVideo` từ khoảnh khắc `newContext()` đã bắt đầu ghi WebM, lúc này Babel/React/Phông chữ nạp mất tổng cộng L giây (2-6s). Script ghi màn hình chờ `window.__ready = true` làm điểm neo "Hoạt ảnh từ đây bắt đầu" — Nó và `time = 0` của hoạt ảnh phải pair nghiêm ngặt. Có 2 cách làm sai thường gặp:

| Cách làm sai | Triệu chứng |
|------|------|
| `__ready` đặt ở `useEffect` hoặc giai đoạn setup đồng bộ (Trước khung hình tick đầu tiên) | Script ghi màn hình tưởng hoạt ảnh bắt đầu rồi, thực tế WebM vẫn đang ghi trang trắng → **Khoảng trắng phía trước** |
| `lastTick = performance.now()` của tick được khởi tạo ở **cấp cao nhất của script** | L giây nạp phông chữ bị tính vào `dt` của khung hình đầu, `time` tức thì nhảy lên L → Ghi hình toàn bộ bị trễ L giây → **Lệch điểm bắt đầu** |

**✅ Template starter tick hoàn chỉnh chính xác** (Viết hoạt ảnh thủ công bắt buộc dùng khung này):

```js
// ━━━━━━ state ━━━━━━
let time = 0;
let playing = false;   // ❗ Mặc định không phát, chờ phông ready mới khởi động
let lastTick = null;   // ❗ sentinel——dt khung hình đầu của tick ép bằng 0 (Đừng dùng performance.now())
const fired = new Set();

// ━━━━━━ tick ━━━━━━
function tick(now) {
  if (lastTick === null) {
    lastTick = now;
    window.__ready = true;   // ✅ pair: "Điểm bắt đầu ghi hình" và "Hoạt ảnh t=0" cùng một khung hình
    render(0);               // Render lại một lần đảm bảo DOM đã sẵn sàng (Lúc này phông đã ready)
    requestAnimationFrame(tick);
    return;
  }
  const dt = (now - lastTick) / 1000;   // Sau khung hình đầu dt mới bắt đầu tăng
  lastTick = now;

  if (playing) {
    let t = time + dt;
    if (t >= DURATION) {
      t = window.__recording ? DURATION - 0.001 : 0;  // Khi ghi hình không loop, giữ 0.001s giữ lại khung hình cuối
      if (!window.__recording) fired.clear();
    }
    time = t;
    render(time);
  }
  requestAnimationFrame(tick);
}

// ━━━━━━ boot ━━━━━━
// Đừng rAF ngay ở cấp cao nhất —— Chờ phông chữ nạp xong mới khởi động
document.fonts.ready.then(() => {
  render(0);                 // Vẽ bức tranh ban đầu ra trước (Phông chữ đã sẵn sàng)
  playing = true;
  requestAnimationFrame(tick);  // tick lần đầu sẽ pair __ready + t=0
});

// ━━━━━━ Giao diện seek (Dùng cho render-video hiệu chỉnh phòng thủ) ━━━━━━
window.__seek = (t) => { fired.clear(); time = t; lastTick = null; render(t); };
```

**Tại sao template này đúng**:

| Khâu | Tại sao bắt buộc phải thế này |
|------|-------------|
| `lastTick = null` + Khung hình đầu `return` | Tránh L giây "từ khi nạp script đến lần thực thi tick đầu tiên" bị tính vào thời gian hoạt ảnh |
| `playing = false` mặc định | Trong thời gian nạp phông `tick` dù chạy cũng không tăng time, tránh render lệch vị trí |
| `__ready` đặt ở khung hình tick đầu tiên | Script ghi hình từ khoảnh khắc này bắt đầu tính giờ, bức tranh tương ứng là t=0 thực sự của hoạt ảnh |
| Khởi động tick trong `document.fonts.ready.then(...)` | Tránh đo đạc độ rộng phông fallback, tránh nhảy phông khung hình đầu |
| Sự tồn tại của `window.__seek` | Cho phép `render-video.js` có thể chủ động hiệu chỉnh — Tấm lá chắn thứ hai |

**Phòng thủ tương ứng phía script ghi hình**:
1. `addInitScript` inject `window.__recording = true` (Trước `page.goto`)
2. `waitForFunction(() => window.__ready === true)`, ghi lại độ lệch khoảnh khắc này làm ffmpeg trim
3. **Bổ sung**: Sau `__ready` chủ động `page.evaluate(() => window.__seek && window.__seek(0))`, ép độ lệch time có thể có của HTML về 0 — Đây là tấm lá chắn thứ hai, đối phó với HTML không tuân thủ nghiêm ngặt template starter

**Phương pháp xác thực**: Sau khi xuất MP4
```bash
ffmpeg -i video.mp4 -ss 0 -vframes 1 frame-0.png
ffmpeg -i video.mp4 -ss $DURATION-0.1 -vframes 1 frame-end.png
```
Khung hình đầu bắt buộc là trạng thái ban đầu t=0 của hoạt ảnh (Không phải đoạn giữa, không phải đen), khung hình cuối bắt buộc là trạng thái kết thúc của hoạt ảnh (Không phải một khoảnh khắc nào đó của vòng loop thứ hai).

**Tham khảo hiện thực hóa**: Component Stage của `assets/animations.jsx`, `scripts/render-video.js` đều đã hiện thực hóa theo giao thức này. HTML viết tay bắt buộc áp template starter tick — Mỗi dòng đều là phòng tránh bug cụ thể.

## 13. Khi ghi hình CẤM loop —— Tín hiệu `window.__recording`

**Bẫy đạp phải**: Hoạt ảnh Stage mặc định `loop=true` (Trong trình duyệt tiện xem hiệu ứng). `render-video.js` ghi xong duration giây còn chờ thêm 300ms đệm mới dừng, 300ms này làm Stage đi vào vòng lặp tiếp theo. ffmpeg `-t DURATION` khi cắt, 0.5-1s cuối rơi vào vòng lặp tiếp theo — Video kết thúc tự nhiên quay về khung hình đầu (Scene 1), khán giả tưởng video bị lỗi.

**Nguyên nhân gốc rễ**: Giữa script ghi hình và HTML không có giao thức bắt tay "Tôi đang ghi hình". HTML không biết mình đang bị ghi, vẫn lặp theo kịch bản tương tác trình duyệt.

**Quy tắc**:

1. **Script ghi hình**: Trong `addInitScript` inject `window.__recording = true` (Trước `page.goto`):
   ```js
   await recordCtx.addInitScript(() => { window.__recording = true; });
   ```

2. **Component Stage**: Nhận biết tín hiệu này, ép loop=false:
   ```js
   const effectiveLoop = (typeof window !== 'undefined' && window.__recording) ? false : loop;
   // ...
   if (next >= duration) return effectiveLoop ? 0 : duration - 0.001;
   //                                                       ↑ Giữ 0.001 tránh Sprite end=duration bị tắt
   ```

3. **fadeOut của Sprite kết thúc**: Trong kịch bản ghi hình nên đặt `fadeOut={0}`, nếu không cuối video sẽ chuyển dần sang trong suốt/màu tối — Kỳ vọng của người dùng là dừng ở khung hình cuối rõ ràng, chứ không phải mờ dần. Khi viết HTML thủ công khuyến nghị Sprite kết thúc đều dùng `fadeOut={0}`.

**Tham khảo hiện thực hóa**: Stage của `assets/animations.jsx` / `scripts/render-video.js` đều đã tích hợp sẵn bắt tay. Stage viết tay bắt buộc hiện thực hóa kiểm tra `__recording` — Nếu không khi ghi hình nhất định đạp bẫy này.

**Xác thực**: Sau khi xuất MP4 `ffmpeg -ss 19.8 -i video.mp4 -frames:v 1 end.png`, kiểm tra 0.2 giây cuối cùng có còn là khung hình cuối cùng dự kiến hay không, không bị chuyển đột ngột sang scene khác.

## 14. Video 60fps mặc định dùng nhân bản khung hình —— minterpolate tính tương thích kém

**Bẫy đạp phải**: MP4 60fps tạo bởi `convert-formats.sh` dùng `minterpolate=fps=60:mi_mode=mci...`, dưới một số phiên bản macOS QuickTime / Safari không thể mở được (Một mảng đen hoặc từ chối mở trực tiếp). VLC / Chrome mở được.

**Nguyên nhân gốc rễ**: H.264 elementary stream đầu ra của minterpolate chứa một số trường SEI / SPS mà một số trình phát phân tích bị lỗi.

**Quy tắc**:

- Mặc định 60fps dùng filter `fps=60` đơn giản (Nhân bản khung hình), tính tương thích rộng (QuickTime/Safari/Chrome/VLC đều mở được)
- Nội suy khung hình chất lượng cao dùng flag `--minterpolate` để bật rõ ràng — Nhưng **bắt buộc phải test cục bộ** trên trình phát mục tiêu rồi mới giao hàng
- Giá trị của nhãn 60fps là **sự nhận biết thuật toán của nền tảng tải lên** (Trên Bilibili / YouTube nhãn 60fps sẽ được ưu tiên đẩy luồng), độ mượt mà cảm nhận thực tế đối với hoạt ảnh CSS tăng lên rất nhỏ
- Thêm `-profile:v high -level 4.0` nâng cao tính tương thích chung của H.264

**`convert-formats.sh` Đã đổi mặc định sang chế độ tương thích**. Nếu bạn cần chất lượng cao nội suy khung hình, thêm flag `--minterpolate`:
```bash
bash convert-formats.sh input.mp4 --minterpolate
```

## 15. Bẫy CORS của `file://` + `.jsx` bên ngoài —— Giao hàng đơn tệp bắt buộc inline engine

**Bẫy đạp phải**: Trong HTML hoạt ảnh dùng `<script type="text/babel" src="animations.jsx"></script>` nạp engine từ bên ngoài. Nhấp đôi máy cục bộ để mở (Giao thức `file://`) → Babel Standalone đi XHR kéo `.jsx` → Chrome báo lỗi `Cross origin requests are only supported for protocol schemes: http, https, chrome, chrome-extension...` → Cả trang đen thui, không báo `pageerror` chỉ báo console error, rất dễ bị chẩn đoán nhầm là "Hoạt ảnh không kích hoạt".

Bật HTTP server chưa chắc đã cứu được — Máy cục bộ có proxy toàn cục thì `localhost` cũng đi qua proxy, trả về 502 / kết nối thất bại.

**Quy tắc**:

- **Giao hàng đơn tệp (HTML nhấp đôi mở dùng ngay)** → `animations.jsx` bắt buộc **inline** vào trong thẻ `<script type="text/babel">...</script>`, đừng dùng `src="animations.jsx"`
- **Dự án nhiều tệp (Bật HTTP server demo)** → Có thể nạp bên ngoài, nhưng khi giao hàng viết rõ lệnh `python3 -m http.server 8000`
- Tiêu chuẩn nhận định: Giao cho người dùng là "Tệp HTML" hay "Thư mục dự án có server"? Cái trước dùng inline
- Component Stage / animations.jsx thường 200+ dòng — Dán vào khối HTML `<script>` hoàn toàn chấp nhận được, đừng sợ dung lượng

**Xác thực tối thiểu**: Nhấp đôi HTML bạn tạo ra, **ĐỪNG** mở qua bất kỳ server nào. Nếu Stage hiển thị bình thường khung hình đầu của hoạt ảnh, mới tính là vượt qua.

## 16. Ngữ cảnh ngược màu qua các scene —— Các phần tử trong màn hình đừng hardcode màu sắc

**Bẫy đạp phải**: Khi làm hoạt ảnh nhiều scene, các phần tử **xuất hiện qua nhiều scene** như `ChapterLabel` / `SceneNumber` / `Watermark`, trong component viết cứng `color: '#1A1A1A'` (Chữ màu tối). 4 scene đầu nền sáng OK, đến scene thứ 5 nền đen "05" và watermark biến mất trực tiếp — Không báo lỗi, không kích hoạt bất kỳ kiểm tra nào, thông tin then chốt bị tàng hình.

**Quy tắc**:

- **Các phần tử trong màn hình tái sử dụng qua nhiều scene** (Nhãn chapter / Số hiệu scene / Mã thời gian / Watermark / Dải bản quyền) **CẤM hardcode giá trị màu**
- Đổi sang một trong ba cách:
  1. **Kế thừa `currentColor`**: Phần tử chỉ viết `color: currentColor`, container scene cha đặt `color: Giá trị tính toán`
  2. **Prop invert**: Component nhận `<ChapterLabel invert />` chuyển đổi sáng tối thủ công
  3. **Tự động tính dựa trên màu nền**: `color: contrast-color(var(--scene-bg))` (API mới CSS 4, hoặc JS nhận định)
- Trước khi giao hàng dùng Playwright rút **khung hình đại diện của mỗi scene**, mắt người duyệt qua một lượt xem "Các phần tử qua scene" có nhìn thấy được không

Độ ẩn nấp của bẫy này nằm ở chỗ —— **Không có cảnh báo bug**. Chỉ có mắt người hoặc OCR mới phát hiện ra.

## 17. Thực·Tự chứa không cần mạng/không CDN —— React/Babel inline toàn bộ, và engine cũng phải transpile

**Bẫy đạp phải (Hoạt ảnh quảng cáo MieuYu 2026-05)**: HTML hoạt ảnh dùng `<script src="https://unpkg.com/react...">` + `<script src=".../@babel/standalone">` đi CDN. Máy cục bộ có proxy toàn cục, khi Playwright ghi hình chromium kết nối unpkg / Google Fonts đều `net::ERR_CONNECTION_CLOSED`:

1. React/ReactDOM không nạp được → `window.React undefined`
2. Babel không nạp được → JSX trong `<script type="text/babel">` chạy như JS thông thường → `Unexpected token '<'`

Sửa xong React/Babel lại đạp bẫy thứ hai: **Đưa engine `animations.jsx` làm `<script>` thông thường inline vào, vẫn báo `Unexpected token '<'` → `window.Animations is undefined`**. Nguyên nhân gốc rễ: **Bản thân engine `animations.jsx` chứa JSX** (Component `Stage`/`Sprite` `return (<div>...)`), thiết kế ban đầu của nó là dùng `<script type="text/babel">` để Babel biên dịch nạp vào. Chỉ transpile code app, quên transpile engine → Đoạn JSX của engine không được biên dịch.

**Quy tắc** (Khi muốn làm đơn tệp thực·tự chứa "Nhấp đôi là mở / Offline / Có thể được Playwright ghi"):

- **React + ReactDOM inline cục bộ**: Dùng `curl` tải `react.production.min.js` (~10KB) + `react-dom.production.min.js` (~131KB) về cục bộ, inline vào `<script>`, không đi CDN
- **Biên dịch trước Babel lúc build, không mang Babel lúc runtime**: Dùng `@babel/standalone` (Tải 1 lần, chỉ dùng khi build) trong node `Babel.transform(src,{presets:['react']}).code`, chuyển JSX → `React.createElement`. **Cả 2 đoạn code app và engine `animations.jsx` đều phải qua transform** —— Engine chứa JSX, bỏ sót nó nhất định báo `Unexpected token '<'`
- **Phông chữ đổi sang phông hệ thống**: Google Fonts CDN cũng sẽ bị proxy ngắt kết nối. Hoạt ảnh tiếng Trung/Việt dùng phông hệ thống `'PingFang SC'` (sans) / `'Songti SC'` (serif) / Arial / Times New Roman, không phụ thuộc vào mạng. `document.fonts.ready` đối với phông hệ thống lập tức resolve, ghi hình không bị kẹt
- **Ảnh tư liệu inline base64**: Đường dẫn tương đối `<img src="png/x.png">` dưới `file://` có thể render, nhưng muốn thực sự di động (Di chuyển tệp không mất ảnh) thì inline base64 data URL; Ảnh nền lớn chuyển sang JPEG nén trước rồi mới base64
- **Template hóa khi build**: Template HTML để lại các token `__REACT__/__REACTDOM__/__ASSETS__/__ENGINE__` + Một đoạn mã nguồn app `type="text/jsx-source"`, script build node đọc token inject vào (vendor giữ nguyên, engine+app qua Babel) → Viết ra đơn tệp cuối cùng. Sửa hoạt ảnh chỉ sửa template chạy lại build

**Xác thực**: Playwright `page.evaluate(()=>({React:typeof window.React, Animations:typeof window.Animations}))` —— Cả hai nên là `object`. Bất kỳ cái nào `undefined` → `<script>` tương ứng đã bắn lỗi (Phần nhiều là JSX chưa transpile).

**Mối quan hệ với bẫy #15**: #15 nói "Đơn tệp đừng dùng `src=` liên kết ngoài `.jsx` (file:// CORS)"; Bẫy này tiến thêm một bước —— Ngay cả **CDN từ xa của React/Babel/Phông chữ dưới mạng bị hạn chế cũng sẽ ngắt**, muốn làm thực·tự chứa bắt buộc inline toàn bộ + transpile lúc build.

## 18. 【HyperFrames】CSS transition + chuyển class không xác định dưới dạng render seek

CSS `transition` đi theo giờ đồng hồ tường, chứ không phải trục thời gian. Khi render seek từng khung hình, mỗi khung hình là một ảnh màn hình độc lập, trạng thái giữa của transition phụ thuộc vào "Khi seek đến khung hình này đã qua bao nhiêu thời gian đồng hồ tường" — Hoàn toàn không xác định, có thể dừng mãi ở giá trị ban đầu, cũng có thể dừng ngẫu nhiên ở giữa. Thực tế di chuyển c3 (2026-07-17): `.watermark-br` Dùng `transition: opacity 0.6s` + chuyển class, dưới dạng render seek độ trong suốt không nghe lời.

**Cách sửa**: Tất cả thay đổi trạng thái trên đường dẫn render đều dùng tween hoặc hàm thuần của t để biểu đạt. Khi di chuyển demo cũ tìm kiếm toàn văn `transition:`, từng cái đổi thành lerp trong `render(t)`; Tổng hợp viết mới ngay từ đầu không viết transition. Transition của trạng thái tương tác như hover không sao cả (Không kích hoạt khi render).

## 19. 【HyperFrames】Khung hình đầu tween proxy không kích hoạt —— Bổ sung thủ công `render(0)`

Khi dùng tween proxy treo `render(t)` vào GSAP timeline (Tuyến adapter demo cũ), timeline dừng ở trạng thái t=0 `onUpdate` không nhất định được gọi — Khung hình đầu có thể là trạng thái tĩnh chưa khởi tạo của HTML chứ không phải bức tranh của `render(0)`.

**Cách sửa**: Sau khi đăng ký timeline gọi đồng bộ thủ công một lần `render(0)`. Toàn văn công thức xem `references/hyperframes-backend.md`.

## 20. 【HyperFrames】Cổng contrast xung đột với phong cách điện ảnh tối —— Dùng `--no-contrast`, 4 cổng còn lại bắt buộc 0 error

Cổng contrast của `npm run check` kiểm tra tất cả chữ theo WCAG AA 4.5:1. Trong thiết kế điện ảnh tối (cinematic), watermark độ trong suốt 16-40%, nhãn mono, chữ trang trí là **cố ý** để độ tương phản thấp (Một phần của cảm giác điện ảnh), sẽ báo lỗi hàng loạt, và framework không có cơ chế miễn trừ từng phần tử. Thực tế kiểm tra c3 42 lỗi contrast đều là ý đồ thiết kế.

**Cách sửa**: Sản phẩm phong cách điện ảnh tối dùng `npx hyperframes check --no-contrast`, 4 cổng lint/runtime/layout/motion vẫn bắt buộc 0 error. **Sản phẩm dạng thông tin nền sáng đừng bỏ qua contrast** — Lỗi báo trong kịch bản đó thường là vấn đề khả năng đọc thực sự (Mức sàn khả năng đọc xem mục Fallback SKILL.md).

## 21. 【HyperFrames/GSAP】Bóng ma immediateRender của fromTo —— Phần tử xuất hiện sớm vài giây

GSAP `fromTo()` mặc định `immediateRender: true`: Khi build timeline đã render trạng thái from lên phần tử. Nếu bản thân trạng thái from nhìn thấy được (`autoAlpha > 0`), phần tử sẽ xuất hiện trong màn hình trước khi tween của nó bắt đầu — Các hiệu ứng "ngắn dồn dập" như tia lửa, vòng nhấp, gợn sóng, bụi dính bẫy này nhất (Thực tế B00 dính 4 chỗ 1 lúc: Hiệu ứng treo trên màn hình vài giây trước thời điểm về vị trí).

**Cách sửa**: Tất cả `fromTo()` có trạng thái from nhìn thấy được thêm rõ ràng `immediateRender: false`; Hoặc đổi thành "set ẩn ban đầu + to". Cách tự kiểm tra: Sau khi render rút khung hình đầu mỗi cảnh, xem có "phần tử hiệu ứng không nên có mặt" hay không.

## 22. 【Ống kính】Chữ bị mờ dưới chế độ 3D/Phóng đại —— Phóng đại đi theo CSS zoom không đi theo transform scale

**Triệu chứng**: Dùng `transform: scale()` đẩy lại gần trang (Đặc biệt dưới chế độ 3D perspective), chữ bị mờ, tỷ lệ càng cao càng mờ, trên 2x không thể giao hàng.

**Nguyên nhân gốc rễ**: Chromium dựa theo **kích thước bố cục** của phần tử để rasterize, rồi phóng đại bitmap. scale chỉ phóng đại bitmap.

**Giải pháp** (Án lệ shotcraft, tri thức đắt nhất toàn thư viện): Sự phóng đại của tầng máy ảnh đi theo **thuộc tính CSS `zoom`** (Co giãn cấp bố cục, dàn trang và rasterize lại theo kích thước sau khi phóng đại, chữ sắc nét ở bất kỳ tỷ lệ nào). Quy đổi tọa độ và công thức đầy đủ xem `camera-language.md` §3.4, `gsap-recipes.md` §9.2. Chú ý: `zoom` mỗi khung hình kích hoạt re-layout, là ngoại lệ hợp pháp duy nhất của "Cấm thuộc tính bố cục tween", chỉ được dùng trên tầng máy ảnh `#world`; Dưới dạng render từng khung hình offline thời lượng render chậm đi là bình thường, chất lượng sản phẩm ưu tiên. Phối hợp: Chụp màn hình toàn trang từ 2x, đặc tả chuẩn bị thêm 4x slice fade đan xen trong 6f thời kỳ đẩy tới.

## 23. 【Ống kính】perspective bị tầng trung gian ngắt quãng —— 3D tức thì biến thành phẳng

**Triệu chứng**: Đã đặt xong `perspective` + `preserve-3d`, render ra hoàn toàn không có cảm giác 3D, tất cả các tầng dán phẳng.

**Nguyên nhân gốc rễ**: Giữa `#camera` và phần tử con 3D **bất kỳ tầng trung gian nào** thêm một trong các thuộc tính `overflow: hidden`, `filter`, `opacity < 1`, `clip-path`, đều sẽ tạo ra stacking context mới, flatten mất preserve-3d.

**Giải pháp**: Dưới chế độ 3D hiệu ứng filter/opacity chỉ thêm trên **phần tử tầng trong cùng**; Trên chuỗi container kiểm tra từng tầng 4 loại thuộc tính nói trên. Thần chú tra cứu: Từ `#camera` đến phần tử có vấn đề, mỗi tầng ở giữa đều `getComputedStyle` tra một lượt 4 mục này.

## 24. 【Ống kính】pan lộ mép —— Khi bình移 lộ ra khoảng trắng ngoài canvas

**Triệu chứng**: Khi ống kính bình移/quay phim mép màn hình lộ ra viền trắng hoặc viền đen.

**Nguyên nhân gốc rễ**: Kích thước `#world` chỉ làm to bằng viewport, ống kính nhúc nhích là ra ngoài ranh giới.

**Giải pháp**: 4 phía xung quanh `#world` mở rộng bleed ≥ Biên độ pan tối đa + 8% lề an toàn (camera-language §3.3). Tầng nền/Tầng bầu không khí phải trải đầy vùng bleed theo, đừng chỉ trải viewport. Tự kiểm tra: Seek timeline đến 2 điểm mút của mỗi đoạn pan chụp màn hình, xem 4 phía.

## Danh sách tự kiểm tra nhanh (5 giây trước khi bắt tay làm)

- [ ] Mỗi phần tử cha của `position: absolute` đều có `position: relative`?
- [ ] Các ký tự đặc biệt trong hoạt ảnh (`␣` `⌘` `emoji`) đều tồn tại trong phông chữ?
- [ ] Count của template Grid/Flex nhất quán với length của dữ liệu JS?
- [ ] Giữa các cảnh chuyển đổi có cross-fade, không có màn hình trắng thuần >0.3s?
- [ ] Code đo đạc DOM được bọc trong `document.fonts.ready.then()`?
- [ ] `render(t)` là pure, hoặc có cơ chế reset rõ ràng?
- [ ] Khung hình thứ 0 là trạng thái ban đầu hoàn chỉnh, không phải màn hình trắng?
- [ ] Trong màn hình không có trang trí "chrome giả" (Thanh tiến độ/Mã thời gian/Dải chữ ký ở đáy đụng hàng với Stage scrubber)?
- [ ] Khung hình tick đầu tiên của hoạt ảnh đồng bộ đặt `window.__ready = true`? (Dùng sẵn animations.jsx; HTML viết tay tự thêm)
- [ ] Stage phát hiện `window.__recording` ép loop=false? (HTML viết tay bắt buộc thêm)
- [ ] `fadeOut` của Sprite kết thúc đặt thành 0 (Cuối video dừng ở khung hình rõ ràng)?
- [ ] MP4 60fps mặc định dùng chế độ nhân bản khung hình (Tương thích), nội suy khung hình chất lượng cao mới thêm `--minterpolate`?
- [ ] Sau khi xuất rút khung hình thứ 0 + khung hình cuối xác thực là trạng thái ban đầu/cuối cùng của hoạt ảnh?
- [ ] Liên quan đến thương hiệu cụ thể (Stripe/Anthropic/Lovart/...): Đã đi xong "Giao thức tài sản thương hiệu" (SKILL.md §1.a 5 bước)? Có viết `brand-spec.md` không?
- [ ] HTML giao hàng đơn tệp: `animations.jsx` là inline, chứ không phải `src="..."`? (Dưới file:// .jsx bên ngoài sẽ bị CORS đen màn hình)
- [ ] Các phần tử xuất hiện qua scene (Nhãn chapter/Watermark/Số scene) không hardcode màu sắc? Đều nhìn thấy được dưới màu nền của mỗi scene?
- [ ] Muốn offline/Thực tự chứa: React+ReactDOM inline cục bộ, **cả app và engine `animations.jsx` đều qua Babel transpile**, phông chữ dùng phông hệ thống? (Xem bẫy #17; Engine chứa JSX, bỏ sót transpile nhất định báo `Unexpected token '<'`)
- [ ] 【HyperFrames】Trên đường dẫn render không có CSS `transition`? Thay đổi trạng thái toàn bộ là tween hoặc hàm thuần của t? (Bẫy #18)
- [ ] 【HyperFrames】Kịch bản tween proxy sau khi đăng ký có bổ sung `render(0)`? (Bẫy #19)
- [ ] 【HyperFrames】Check đã qua? Phong cách điện ảnh tối dùng `--no-contrast`, 4 cổng còn lại 0 error? (Bẫy #20)
- [ ] 【HyperFrames/GSAP】`fromTo()` có trạng thái from nhìn thấy được toàn bộ đã thêm `immediateRender:false`? (Bẫy #21, B00 thực tế kiểm tra 4 chỗ bóng ma)
- [ ] 【Ống kính】3D/Đặc tả phóng đại đi theo CSS `zoom`, không có scale phóng đại bị mờ? (Bẫy #22)
- [ ] 【Ống kính】Tầng trung gian từ `#camera` đến phần tử 3D không có overflow/filter/opacity/clip-path? (Bẫy #23)
- [ ] 【Ống kính】`#world` đã mở rộng bleed, chụp màn hình điểm mút pan 4 phía không lộ khoảng trắng? (Bẫy #24)
