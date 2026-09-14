# Xuất PPTX Chỉnh sửa được: Quy tắc cứng HTML + Quyết định Kích thước + Tra cứu Lỗi Thường gặp

Tài liệu này hướng dẫn cách **dùng `scripts/html2pptx.js` + `pptxgenjs` để dịch HTML từng phần tử sang khung văn bản PowerPoint có thể chỉnh sửa thực sự**, đây cũng là con đường duy nhất mà `export_deck_pptx.mjs` hỗ trợ.

> **Tiền đề cốt lõi**: Để đi theo con đường này, HTML phải tuân thủ 4 quy tắc cứng dưới đây ngay từ dòng đầu tiên. **Không phải viết xong mới chuyển** — Việc khắc phục hậu quả sau đó sẽ làm bạn tốn 2-3 giờ làm lại (Thực tế đã vấp phải ở dự án Option Private Board ngày 20-04-2026).
>
> Đối với kịch bản ưu tiên độ tự do thị giác (Animation / Web component / CSS gradient / SVG phức tạp), vui lòng chuyển sang con đường xuất PDF (`export_deck_pdf.mjs` / `export_deck_stage_pdf.mjs`), **đừng** trông chờ việc xuất pptx vừa bảo toàn thị giác vừa chỉnh sửa được — Đây là hạn chế vật lý của chính bản thân định dạng file PPTX (Xem chi tiết ở cuối bài "Tại sao 4 quy tắc không phải là Bug mà là hạn chế vật lý").

---

## Kích thước Canvas: Dùng 960×540pt (LAYOUT_WIDE)

Đơn vị của PPTX là **inch** (Kích thước vật lý), không phải px. Nguyên tắc quyết định: Kích thước computedStyle của body phải **khớp với kích thước inch của presentation layout** (±0.1", được kiểm tra bắt buộc bởi `validateDimensions` trong `html2pptx.js`).

### So sánh 3 lựa chọn kích thước ứng viên

| HTML body | Kích thước vật lý | Tương ứng layout PPT | Khi nào chọn |
|---|---|---|---|
| **`960pt × 540pt`** | **13.333″ × 7.5″** | **pptxgenjs `LAYOUT_WIDE`** | ✅ **Khuyến nghị mặc định** (Chuẩn 16:9 của PowerPoint hiện đại) |
| `720pt × 405pt` | 10″ × 5.625″ | Tùy chỉnh | Chỉ khi người dùng chỉ định template "PowerPoint Widescreen phiên bản cũ" |
| `1920px × 1080px` | 20″ × 11.25″ | Tùy chỉnh | ❌ Kích thước không chuẩn, khi chiếu font chữ sẽ trông bất thường và nhỏ |

**Đừng nghĩ kích thước HTML là độ phân giải.** PPTX là tài liệu vector, kích thước body quyết định **kích thước vật lý** chứ không phải độ sắc nét. Body quá lớn (20″×11.25″) không làm chữ sắc nét hơn — chỉ làm font-size pt nhỏ đi tương đối so với canvas, khi chiếu/in ấn ngược lại lại khó nhìn hơn.

### Ba cách viết body tương đương (Chọn 1 trong 3)

```css
body { width: 960pt;  height: 540pt; }    /* Rõ ràng nhất, khuyến nghị */
body { width: 1280px; height: 720px; }    /* Tương đương, thói quen px */
body { width: 13.333in; height: 7.5in; }  /* Tương đương, trực giác inch */
```

Code pptxgenjs đi kèm:

```js
const pptx = new pptxgen();
pptx.layout = 'LAYOUT_WIDE';  // 13.333 × 7.5 inch, không cần tùy chỉnh
```

---

## 4 Quy tắc cứng (Vi phạm sẽ báo lỗi trực tiếp)

`html2pptx.js` dịch DOM của HTML từng phần tử sang đối tượng PowerPoint. Các ràng buộc định dạng của PowerPoint khi chiếu lên HTML = 4 quy tắc dưới đây.

### Quy tắc 1: Trong DIV không được viết chữ trực tiếp — Bắt buộc bọc bằng `<p>` hoặc `<h1>`-`<h6>`

```html
<!-- ❌ Lỗi: Chữ nằm trực tiếp trong div -->
<div class="title">Q3 Doanh thu tăng trưởng 23%</div>

<!-- ✅ Đúng: Chữ nằm trong <p> hoặc <h1>-<h6> -->
<div class="title"><h1>Q3 Doanh thu tăng trưởng 23%</h1></div>
<div class="body"><p>Người dùng mới là động lực chính</p></div>
```

**Tại sao**: Văn bản trong PowerPoint bắt buộc phải tồn tại trong text frame, text frame tương ứng với các phần tử cấp đoạn văn trong HTML (p/h*/li). `<div>` trần không có hộp chứa văn bản tương ứng trong PPTX.

**Cũng không được dùng `<span>` để chứa văn bản chính** — span là phần tử inline, không thể tự căn chỉnh độc lập thành khung văn bản. span chỉ có thể **kẹp bên trong p/h\*** để làm style cục bộ (in đậm, đổi màu).

### Quy tắc 2: Không hỗ trợ CSS Gradient — Chỉ được dùng màu thuần (Solid Color)

```css
/* ❌ Lỗi */
background: linear-gradient(to right, #FF6B6B, #4ECDC4);

/* ✅ Đúng: Màu thuần */
background: #FF6B6B;

/* ✅ Nếu bắt buộc phải có sọc nhiều màu, dùng flex với các phần tử con mang màu thuần riêng */
.stripe-bar { display: flex; }
.stripe-bar div { flex: 1; }
.red   { background: #FF6B6B; }
.teal  { background: #4ECDC4; }
```

**Tại sao**: Shape fill của PowerPoint chỉ hỗ trợ solid/gradient-fill, nhưng `fill: { color: ... }` của pptxgenjs chỉ ánh xạ solid. Nền gradient dạng nguyên bản của PowerPoint cần viết cấu trúc khác, chuỗi công cụ hiện tại chưa hỗ trợ.

### Quy tắc 3: Nền/Đường viền/Bóng đổ chỉ được đặt ở DIV, không được đặt ở thẻ văn bản

```html
<!-- ❌ Lỗi: <p> có màu nền -->
<p style="background: #FFD700; border-radius: 4px;">Nội dung trọng tâm</p>

<!-- ✅ Đúng: Div bên ngoài gánh nền/đường viền, <p> chỉ chịu trách nhiệm phần chữ -->
<div style="background: #FFD700; border-radius: 4px; padding: 8pt 12pt;">
  <p>Nội dung trọng tâm</p>
</div>
```

**Tại sao**: Trong PowerPoint, shape (ô vuông/hình chữ nhật bo góc) và text frame là hai đối tượng riêng biệt. Thẻ `<p>` của HTML chỉ dịch thành text frame, nền/đường viền/bóng đổ thuộc về shape — bắt buộc phải viết ở **div bọc quanh text**.

### Quy tắc 4: DIV không được dùng `background-image` — Dùng thẻ `<img>`

```html
<!-- ❌ Lỗi -->
<div style="background-image: url('chart.png')"></div>

<!-- ✅ Đúng -->
<img src="chart.png" style="position: absolute; left: 50%; top: 20%; width: 300pt; height: 200pt;" />
```

**Tại sao**: `html2pptx.js` chỉ trích xuất đường dẫn ảnh từ phần tử `<img>`, không parse URL `background-image` của CSS.

---

## Gộp khung văn bản (`data-pptx-merge`)

**Hành vi mặc định**: Mỗi `<p>`/`<h1>`-`<h6>` trong HTML khi sang PPTX đều là **khung văn bản độc lập**. Trong thẻ card viết 3 thẻ `<p>` → Sang PPT thành 3 khung văn bản xếp chồng lên nhau, khi chỉnh sửa không thể xuống dòng Enter thêm đoạn liên tục, mà phải sửa từng font-size/căn chỉnh của từng ô.

**Cách giải quyết**: Thêm `data-pptx-merge="true"` vào div bên ngoài, tất cả `<p>/<h*>` bên trong container sẽ được gộp thành **một khung văn bản chỉnh sửa được**, giữa các đoạn ngăn cách bằng ký tự ngắt đoạn, trong PPT sẽ là các đoạn văn bản chỉnh sửa liên tục.

```html
<!-- ✅ Cách viết gộp: Cả 4 đoạn nằm chung trong 1 khung văn bản -->
<div class="card" data-pptx-merge="true"
     style="position: absolute; top: 60pt; left: 60pt; width: 420pt;
            background: #1A4A8A; border-radius: 8pt; padding: 20pt 24pt;">
  <h2 style="font-size: 24pt; color: #FFFFFF;">Tiêu đề</h2>
  <p  style="font-size: 14pt; color: #DDEEFF;">Đoạn văn bản thứ nhất.</p>
  <p  style="font-size: 14pt; color: #FFD166;">Đoạn thứ hai: Đổi màu để nhấn mạnh.</p>
  <p  style="font-size: 14pt; color: #DDEEFF;">Đoạn thứ ba: Tiếp tục viết trong cùng khung văn bản.</p>
</div>
```

**Các style được giữ lại** (được ghi vào dưới dạng run options từng đoạn): `font-size`, `color`, `font-family`, `font-weight` (bold), `font-style` (italic), `text-decoration: underline`, các inline style của `<b>/<i>/<u>/<strong>/<em>/<span>`.

**Được lấy từ đoạn đầu tiên, thống nhất cho toàn khung**: `text-align`, `line-height`. Vì căn chỉnh và khoảng cách dòng trong PowerPoint là ở cấp độ paragraph/textbox — trong một khung chỉ có thể có một loại căn chỉnh. Nếu căn chỉnh của các đoạn khác nhau, vui lòng không dùng merge, hãy để chúng độc lập.

**Bản thân `background`/`border`/`box-shadow`/`border-radius` của container** vẫn được render dưới dạng shape như bình thường, hành vi hoàn toàn giống div thường — nghĩa là nền thẻ màu xanh + text vẫn là hai lớp "shape + text frame", chỉ là lớp text từ 3-4 khung văn bản thu gọn còn 1.

**Hạn chế**:
- Không thể lồng `data-pptx-merge` (sẽ báo lỗi).
- Container không được dùng `background-image` (giống quy tắc 4 trong 4 quy tắc cứng).
- Bên trong container đừng đặt thêm div con có `background`/`border` — chúng vẫn sẽ được render thành shape độc lập, nhưng chữ bên trong đã bị gộp đi mất rồi, có thể gây ra lệch vị trí thị giác.

**Khi nào dùng**: Nội dung sẽ được sửa đi sửa lại, kịch bản cần tiếp tục chỉnh sửa trong PPT. Xuất lưu trữ một lần không cần thêm, hành vi nhất quán.

---

## Khung mẫu HTML Path A

Mỗi slide là một file HTML độc lập, scope cách ly lẫn nhau (tránh ô nhiễm CSS của deck đơn file).

```html
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    width: 960pt; height: 540pt;           /* ⚠️ Khớp với LAYOUT_WIDE */
    font-family: system-ui, -apple-system, "PingFang SC", sans-serif;
    background: #FEFEF9;                    /* Màu thuần, không gradient */
    overflow: hidden;
  }
  /* DIV chịu trách nhiệm layout/nền/đường viền */
  .card {
    position: absolute;
    background: #1A4A8A;                    /* Nền nằm ở DIV */
    border-radius: 4pt;
    padding: 12pt 16pt;
  }
  /* Thẻ văn bản chỉ chịu trách nhiệm style font chữ, không thêm nền/đường viền */
  .card h2 { font-size: 24pt; color: #FFFFFF; font-weight: 700; }
  .card p  { font-size: 14pt; color: rgba(255,255,255,0.85); }
</style>
</head>
<body>

  <!-- Khu vực tiêu đề: div ngoài định vị, thẻ văn bản bên trong -->
  <div style="position: absolute; top: 40pt; left: 60pt; right: 60pt;">
    <h1 style="font-size: 36pt; color: #1A1A1A; font-weight: 700;">Tiêu đề dùng câu khẳng định, không dùng từ chủ đề</h1>
    <p style="font-size: 16pt; color: #555555; margin-top: 10pt;">Tiêu đề phụ giải thích thêm</p>
  </div>

  <!-- Thẻ nội dung: div gánh nền, h2/p gánh chữ -->
  <div class="card" style="top: 130pt; left: 60pt; width: 240pt; height: 160pt;">
    <h2>Điểm chính 1</h2>
    <p>Chữ giải thích ngắn gọn</p>
  </div>

  <!-- Danh sách: Dùng ul/li, không nhập ký tự • thủ công -->
  <div style="position: absolute; top: 320pt; left: 60pt; width: 540pt;">
    <ul style="font-size: 16pt; color: #1A1A1A; padding-left: 24pt; list-style: disc;">
      <li>Điểm chính thứ nhất</li>
      <li>Điểm chính thứ hai</li>
      <li>Điểm chính thứ ba</li>
    </ul>
  </div>

  <!-- Hình minh họa: Dùng thẻ <img>, không dùng background-image -->
  <img src="illustration.png" style="position: absolute; right: 60pt; top: 110pt; width: 320pt; height: 240pt;" />

</body>
</html>
```

---

## Tra cứu lỗi thường gặp

| Thông báo lỗi | Nguyên nhân | Cách khắc phục |
|---------------|-------------|----------------|
| `DIV element contains unwrapped text "XXX"` | Div chứa chữ trần | Bọc chữ lại bằng `<p>` hoặc `<h1>`-`<h6>` |
| `CSS gradients are not supported` | Dùng linear/radial-gradient | Đổi thành màu thuần, hoặc dùng flex phần tử con phân đoạn |
| `Text element <p> has background` | Thẻ `<p>` thêm màu nền | Bọc `<div>` bên ngoài gánh nền, `<p>` chỉ viết chữ |
| `Background images on DIV elements are not supported` | Div dùng background-image | Đổi thành thẻ `<img>` |
| `HTML content overflows body by Xpt vertically` | Nội dung vượt quá 540pt | Giảm nội dung hoặc thu nhỏ font-size, hoặc dùng `overflow: hidden` cắt bớt |
| `HTML dimensions don't match presentation layout` | Kích thước body không khớp pres layout | Body dùng `960pt × 540pt` đi kèm `LAYOUT_WIDE`; hoặc dùng defineLayout tùy chỉnh kích thước |
| `Text box "XXX" ends too close to bottom edge` | Thẻ `<p>` font chữ lớn cách mép dưới body < 0.5 inch | Dịch lên trên, chừa đủ margin dưới; bản thân mép dưới PPT sẽ bị che mất một phần |

---

## Quy trình làm việc cơ bản (3 bước xuất PPTX)

### Step 1: Viết HTML độc lập từng trang theo quy tắc

```
MyDeck/
├── slides/
│   ├── 01-cover.html    # Mỗi file là một HTML 960×540pt hoàn chỉnh
│   ├── 02-agenda.html
│   └── ...
└── illustration/        # Tất cả hình ảnh được tham chiếu bởi <img>
    ├── chart1.png
    └── ...
```

### Step 2: Viết build.js gọi `html2pptx.js`

```js
const pptxgen = require('pptxgenjs');
const html2pptx = require('../scripts/html2pptx.js');  // Script của skill này

(async () => {
  const pres = new pptxgen();
  pres.layout = 'LAYOUT_WIDE';  // 13.333 × 7.5 inch, khớp với 960×540pt của HTML

  const slides = ['01-cover.html', '02-agenda.html', '03-content.html'];
  for (const file of slides) {
    await html2pptx(`./slides/${file}`, pres);
  }

  await pres.writeFile({ fileName: 'deck.pptx' });
})();
```

### Step 3: Mở lên kiểm tra

- Mở PPTX đã xuất bằng PowerPoint/Keynote
- Nhấp đôi vào bất kỳ văn bản nào xem có thể chỉnh sửa trực tiếp không (nếu là hình ảnh chứng tỏ vi phạm quy tắc 1)
- Kiểm tra overflow: Mỗi trang phải nằm trong phạm vi body, không bị cắt xén

---

## Con đường này vs Các lựa chọn khác (Khi nào chọn cái gì)

| Nhu cầu | Chọn cái gì |
|---------|-------------|
| Đồng nghiệp sẽ sửa chữ trong PPTX / Gửi cho nhân sự phi kỹ thuật tiếp tục chỉnh sửa | **Con đường trong bài này** (editable, cần viết HTML theo 4 quy tắc ngay từ đầu) |
| Chỉ dùng để thuyết trình / Gửi lưu trữ, không sửa nữa | `export_deck_pdf.mjs` (Đa file) hoặc `export_deck_stage_pdf.mjs` (Đơn file deck-stage), xuất PDF vector |
| Ưu tiên độ tự do thị giác (Animation, web component, CSS gradient, SVG phức tạp), chấp nhận không thể chỉnh sửa | **PDF** (Như trên) — PDF vừa bảo toàn vừa đa nền tảng, thích hợp hơn "PPTX ảnh" |

**Tuyệt đối không chạy gượng ép html2pptx trên HTML đã viết tự do theo phong cách thị giác** — Thử nghiệm thực tế HTML thuần thị giác có tỷ lệ pass qua html2pptx < 30%, phần còn lại cải tạo từng trang còn chậm hơn viết lại. Kịch bản đó nên xuất PDF, không phải gượng ép ra PPTX.

---

## Fallback: Đã có bản thiết kế thị giác nhưng khách hàng bắt buộc đòi editable PPTX

Thỉnh thoảng sẽ gặp kịch bản này: Bạn/Khách hàng đã viết xong một bản HTML thuần thị giác (dùng gradient, web component, SVG phức tạp), vốn dĩ xuất PDF là hợp nhất, nhưng khách hàng nói rõ "Không được, phải là PPTX chỉnh sửa được".

**Đừng gượng ép chạy `html2pptx` trông chờ nó pass** — Thử nghiệm thực tế HTML thuần thị giác chạy trên html2pptx có tỷ lệ pass <30%, 70% còn lại sẽ báo lỗi hoặc méo hình. Cách fallback đúng là:

### Step 1 · Báo trước các hạn chế (Giao tiếp minh bạch)

Nói rõ ba điều với khách hàng trong một câu:

> "Bản HTML hiện tại của bạn có dùng [Liệt kê cụ thể: Gradient / web component / SVG phức tạp / ...], chuyển trực tiếp sang editable PPTX sẽ fail. Tôi có hai phương án:
> - A. **Xuất PDF** (Khuyến nghị) — Giữ lại 100% thị giác, bên nhận xem được in được nhưng không sửa chữ được
> - B. **Lấy bản thiết kế thị giác làm mẫu, viết lại một bản editable HTML** (Giữ lại quyết định thiết kế về màu sắc/bố cục/văn bản, nhưng tổ chức lại cấu trúc HTML theo 4 quy tắc cứng, **hy sinh** các năng lực thị giác như gradient, web component, SVG phức tạp) → Sau đó xuất editable PPTX
>
> Bạn chọn cái nào?"

Đừng nói phương án B một cách nhẹ nhàng như không có gì — Báo rõ cho khách hàng **sẽ mất những gì**. Để khách hàng đưa ra lựa chọn.

### Step 2 · Nếu khách hàng chọn B: AI chủ động viết lại, không yêu cầu khách hàng tự viết

Triết lý ở đây là: **Khách hàng đưa ra ý đồ thiết kế, bạn chịu trách nhiệm dịch thành triển khai hợp chuẩn**. Không phải bảo khách hàng đi học 4 quy tắc cứng rồi tự viết lại.

Các nguyên tắc tuân thủ khi viết lại:
- **Giữ lại**: Hệ thống màu sắc (màu chính/màu phụ/màu trung tính), Phân cấp thông tin (tiêu đề/tiêu đề phụ/văn bản chính/ghi chú), Văn bản cốt lõi, Khung layout (trên giữa dưới / chia cột trái phải / lưới), Nhịp điệu trang
- **Hạ cấp**: CSS gradient → Màu thuần hoặc phân đoạn flex, web component → HTML cấp đoạn văn, SVG phức tạp → `<img>` đơn giản hóa hoặc hình học màu thuần, bóng đổ → Xóa hoặc giảm xuống cực yếu, custom font → Tiệm cận về font hệ thống
- **Viết lại**: Chữ trần → Bọc vào `<p>` / `<h*>`, `background-image` → thẻ `<img>`, nền border trên `<p>` → Div bên ngoài gánh

### Step 3 · Tạo danh sách đối chiếu (Bàn giao minh bạch)

Sau khi viết lại xong đưa cho khách hàng một bảng đối chiếu before/after, để họ biết những chi tiết thị giác nào đã được đơn giản hóa:

```
Thiết kế gốc → Điều chỉnh bản editable
- Khu vực tiêu đề màu tím gradient → Nền màu thuần màu chính #5B3DE8
- Bóng đổ thẻ dữ liệu → Xóa (Đổi thành đường viền 2pt để phân biệt)
- Biểu đồ đường SVG phức tạp → Đơn giản hóa thành PNG <img> (Tạo từ snapshot HTML)
- Hiệu ứng động web component khu vực Hero → Frame tĩnh đầu tiên (web component không dịch được)
```

### Step 4 · Xuất & Bàn giao định dạng đôi

- Bản HTML `editable` → Chạy `scripts/export_deck_pptx.mjs` xuất PPTX chỉnh sửa được
- **Khuyến nghị đồng thời giữ lại** bản thiết kế gốc → Chạy `scripts/export_deck_pdf.mjs` xuất PDF độ bảo toàn cao
- Bàn giao định dạng đôi cho khách hàng: PDF bản thị giác + PPTX bản chỉnh sửa được, mỗi bản thực hiện đúng vai trò riêng

### Trường hợp nào nên từ chối trực tiếp phương án B

Trong một số kịch bản giá trị改写 (viết lại) quá cao, nên khuyên khách hàng từ bỏ editable PPTX:
- Giá trị cốt lõi của HTML nằm ở animation hoặc tương tác (sau khi viết lại chỉ còn frame tĩnh đầu tiên, tổn thất thông tin 50%+)
- Số trang > 30, chi phí viết lại vượt quá 2 giờ
- Thiết kế thị giác phụ thuộc sâu vào SVG chính xác / filter tùy chỉnh (sau khi viết lại hầu như không liên quan tới hình gốc)

Lúc này báo với khách hàng: "Chi phí viết lại deck này quá cao, khuyến nghị xuất PDF thay vì PPTX. Nếu bên nhận thực sự bắt buộc định dạng pptx, hãy chấp nhận thị giác sẽ bị mộc mạc hóa đáng kể — Bạn có muốn đổi sang PDF không?"

---

## Tại sao 4 quy tắc không phải là Bug mà là hạn chế vật lý

4 quy tắc này không phải do tác giả `html2pptx.js` lười biếng — Chúng là **hạn chế của chính định dạng file PowerPoint (OOXML)** chiếu lên HTML:

- Văn bản trong PPTX bắt buộc phải nằm trong text frame (`<a:txBody>`), tương ứng với phần tử HTML cấp đoạn văn
- Shape và text frame của PPTX là hai đối tượng riêng biệt, không thể vẽ nền và viết chữ đồng thời trên cùng một element
- Shape fill của PPTX hỗ trợ gradient có hạn (Chỉ một số preset gradients nhất định, không hỗ trợ CSS gradient góc bất kỳ)
- Đối tượng picture của PPTX bắt buộc phải tham chiếu file ảnh thật, không phải thuộc tính CSS

Sau khi hiểu điều này, **đừng trông chờ công cụ trở nên thông minh hơn** — Là cách viết HTML phải thích ứng với định dạng PPTX, chứ không phải ngược lại.
