# Giao thức Tài sản Cốt lõi (Bản hoàn chỉnh)

> Giao thức hoàn chỉnh hạ chìm từ SKILL.md "Triết lý cốt lõi #1.a" (Tinh giản 06-2026). SKILL.md giữ lại điều kiện kích hoạt + tiêu đề 5 bước + tự kiểm tra; ở đây là thao tác chi tiết 5 bước, lệnh tải về, template brand-spec, dự phòng thất bại toàn quy trình, so sánh ví dụ phản diện và cái giá phải trả.
> Kích hoạt: Bắt buộc thực thi khi nhiệm vụ liên quan đến thương hiệu/sản phẩm cụ thể. Quay lại SKILL.md xem bản tinh giản và ngữ cảnh.

#### 1.a Giao thức Tài sản Cốt lõi (Bắt buộc thực thi khi liên quan đến thương hiệu cụ thể)

> **Đây là ràng buộc cốt lõi nhất của v1, cũng là đường sống của sự ổn định.** Agent có đi thông giao thức này hay không, quyết định trực tiếp chất lượng đầu ra là 40 điểm hay 90 điểm. Đừng bỏ qua bất kỳ bước nào.
>
> **Tái cấu trúc v1.1 (20-04-2026)**: Nâng cấp từ "Giao thức tài sản thương hiệu" thành "Giao thức tài sản cốt lõi". Phiên bản trước đây quá tập trung vào giá trị màu và font chữ, bỏ sót mất logo / hình ảnh sản phẩm / ảnh chụp UI cơ bản nhất trong thiết kế. Nguyên văn của Hoa Thúc: "Ngoài cái gọi là màu thương hiệu, rõ ràng chúng ta nên tìm thấy và sử dụng logo của DJI, sử dụng hình ảnh sản phẩm của pocket4. Nếu là sản phẩm phi thực thể như website hay app, logo ít nhất phải là thứ bắt buộc. Đây có thể là logic cơ bản quan trọng hơn cả spec cái gọi là thiết kế thương hiệu. Nếu không, chúng ta đang thể hiện điều gì?"

**Điều kiện kích hoạt**: Nhiệm vụ liên quan đến thương hiệu cụ thể——Người dùng nhắc đến tên sản phẩm/tên công ty/khách hàng rõ ràng (Stripe, Linear, Anthropic, Notion, Lovart, DJI, công ty nhà v.v.), bất kể người dùng có chủ động cung cấp tư liệu thương hiệu hay không.

**Điều kiện cứng tiền đề**: Trước khi đi theo giao thức bắt buộc phải thông qua "#0 Xác minh thực tế đi trước giả định" xác nhận thương hiệu/sản phẩm tồn tại và trạng thái đã biết. Nếu bạn vẫn chưa chắc chắn sản phẩm đã phát hành/quy cách/phiên bản hay chưa, hãy quay lại tìm kiếm trước.

##### Triết lý cốt lõi: Tài sản > Quy chuẩn

**Bản chất của thương hiệu là "Nó được nhận ra"**. Nhận ra dựa vào cái gì? Sắp xếp theo độ đóng góp nhận diện:

| Loại tài sản | Đóng góp độ nhận diện | Tính bắt buộc |
|---|---|---|
| **Logo** | Cao nhất · Bất kỳ thương hiệu nào xuất hiện logo là nhận ra ngay | **Bất kỳ thương hiệu nào cũng bắt buộc phải có** |
| **Hình ảnh sản phẩm/Hình render sản phẩm** | Cực cao · "Nhân vật chính" của sản phẩm thực thể chính là bản thân sản phẩm | **Sản phẩm thực thể (Phần cứng/Bao bì/Hàng tiêu dùng) bắt buộc phải có** |
| **Ảnh chụp UI/Vật liệu giao diện** | Cực cao · "Nhân vật chính" của sản phẩm kỹ thuật số là giao diện của nó | **Sản phẩm kỹ thuật số (App/Website/SaaS) bắt buộc phải có** |
| **Giá trị màu** | Trung bình · Hỗ trợ nhận diện, thoát khỏi 3 mục trên rất dễ đụng hàng | Hỗ trợ |
| **Font chữ** | Thấp · Cần phối hợp với các mục trên mới thiết lập được nhận diện | Hỗ trợ |
| **Từ khóa khí chất** | Thấp · Dùng cho agent tự kiểm tra | Hỗ trợ |

**Dịch thành quy tắc thực thi**:
- Chỉ trích xuất giá trị màu + font chữ, không tìm logo / hình ảnh sản phẩm / UI → **Vi phạm giao thức này**
- Dùng hình bóng CSS/Vẽ tay SVG thay thế hình ảnh sản phẩm thực tế → **Vi phạm giao thức này** (Tạo ra chính là "animation công nghệ thông thường", thương hiệu nào trông cũng giống nhau)
- Không tìm thấy tài sản không nói cho người dùng, cũng không dùng AI tạo ra, cố làm → **Vi phạm giao thức này**
- Thà dừng lại hỏi người dùng xin vật liệu, còn hơn dùng generic để lấp đầy

##### Quy trình cứng 5 bước (Mỗi bước có fallback, tuyệt đối không âm thầm bỏ qua)

##### Step 1 · Hỏi (Hỏi đầy đủ một lần danh mục tài sản)

Đừng chỉ hỏi "Có brand guidelines không?"——quá rộng, người dùng không biết nên đưa cái gì. Hỏi từng mục theo danh mục:

```
Về <brand/product>, trong tay bạn hiện có những tư liệu nào dưới đây? Tôi liệt kê theo độ ưu tiên:
1. Logo (SVG / PNG HD) —— Bất kỳ thương hiệu nào cũng bắt buộc có
2. Hình ảnh sản phẩm / Hình render chính thức —— Sản phẩm thực thể bắt buộc có (Như ảnh sản phẩm DJI Pocket 4)
3. Ảnh chụp UI / Vật liệu giao diện —— Sản phẩm kỹ thuật số bắt buộc có (Như ảnh chụp các trang chính của App)
4. Danh mục giá trị màu (HEX / RGB / Bảng màu thương hiệu)
5. Danh mục font chữ (Display / Body)
6. Brand guidelines PDF / Figma design system / Link website chính thức thương hiệu

Cái nào có thì gửi trực tiếp cho tôi, cái nào không có tôi đi tìm/cào/tạo ra.
```

##### Step 2 · Tìm kiếm kênh chính thức (Theo loại tài sản)

| Tài sản | Đường dẫn tìm kiếm |
|---|---|
| **Logo** | `<brand>.com/brand` · `<brand>.com/press` · `<brand>.com/press-kit` · `brand.<brand>.com` · Inline SVG trên header trang chính thức |
| **Hình ảnh sản phẩm/Hình render** | Trang chi tiết sản phẩm `<brand>.com/<product>` hero image + gallery · Khung hình cắt video launch film chính thức trên YouTube · Hình đính kèm thông cáo báo chí chính thức |
| **Ảnh chụp UI** | Ảnh chụp trang sản phẩm App Store / Google Play · Section screenshots trên trang chính thức · Cắt khung hình video demo chính thức của sản phẩm |
| **Giá trị màu** | Inline CSS trang chính thức / Tailwind config / brand guidelines PDF |
| **Font chữ** | Trích dẫn `<link rel="stylesheet">` trang chính thức · Dấu vết Google Fonts · brand guidelines |

Từ khóa兜底 cho `WebSearch`:
- Không tìm thấy Logo → `<brand> logo download SVG`, `<brand> press kit`
- Không tìm thấy hình sản phẩm → `<brand> <product> official renders`, `<brand> <product> product photography`
- Không tìm thấy UI → `<brand> app screenshots`, `<brand> dashboard UI`

##### Step 3 · Tải tài sản · Ba con đường dự phòng theo loại

**3.1 Logo (Bắt buộc cho bất kỳ thương hiệu nào)**

> ⚠️ **Đừng chỉ thử `curl <brand>.com/logo.svg` rồi bỏ cuộc**——Website chính thức ngày nay hầu hết là SPA, đường dẫn tĩnh kết nối trực tiếp cơ bản trả về HTML khung rỗng (Thử nghiệm thực tế ngày 06-06-2026 trên website chính thức Trae 5 đường dẫn kết nối trực tiếp đều là khung rỗng). **Sản phẩm kỹ thuật số / SaaS / Nguồn công cụ AI ưu tiên dùng nguồn gom icon**, tỷ lệ trúng cao nhất, trực tiếp xuất SVG sạch.

Theo tỷ lệ thành công giảm dần:
0. **Nguồn gom icon (Ưu tiên số 1 cho sản phẩm kỹ thuật số/SaaS/Công cụ AI nổi tiếng, tỷ lệ trúng cao nhất)**:
   ```bash
   unset ALL_PROXY HTTP_PROXY HTTPS_PROXY all_proxy http_proxy https_proxy   # Xóa proxy, nếu không TLS dễ nổ
   # svgl —— Độ phủ thương hiệu AI/Developer đầy đủ nhất (Claude/Cursor/OpenAI/Copilot/Anthropic/Vercel…), chứa light/dark + wordmark
   curl -s "https://api.svgl.app?search=<brand>"   # Trả về JSON, lấy URL svg của route(.light/.dark) rồi tải về
   # simpleicons —— Glyph đơn sắc, có thể trực tiếp tô màu theo màu thương hiệu
   curl -o logo.svg "https://cdn.simpleicons.org/<slug>/<hexcolor>"
   ```
1. File SVG/PNG độc lập / Trang brand chính thức (Như `<brand>.com/brand`, `/press`):
   ```bash
   curl -A "Mozilla/5.0" -L -o assets/<brand>-brand/logo.svg "<official-logo-url>"
   ```
2. Trích xuất inline SVG toàn văn HTML trang chính thức:
   ```bash
   curl -A "Mozilla/5.0" -L https://<brand>.com -o assets/<brand>-brand/homepage.html
   # Sau đó grep <svg>...</svg> trích xuất node logo
   ```
3. **Dịch vụ Google favicon (Dự phòng mark thực tế của site, hầu như không thất bại)**:
   ```bash
   curl -o logo.png "https://www.google.com/s2/favicons?domain=<brand-domain>&sz=256"   # Icon site chính thức 256px
   ```
4. Avatar mạng xã hội chính thức (Biện pháp cuối cùng): Avatar công ty trên GitHub/Twitter/LinkedIn thường là PNG nền trong suốt 400×400 hoặc 800×800

Sau khi tải về **đối chiếu từng cái**: `file <logo>` xác nhận là SVG/PNG thật (Không phải giữ chỗ 106 byte hay HTML khung rỗng), `head -c 90 <logo.svg>` xem có phải `<svg` không.

**3.2 Hình ảnh sản phẩm/Hình render (Bắt buộc cho sản phẩm thực thể)**

Theo độ ưu tiên:
1. **Hero image trang sản phẩm chính thức** (Độ ưu tiên cao nhất): Chuột phải xem địa chỉ hình ảnh / curl lấy về. Độ phân giải thường 2000px+
2. **Press kit chính thức**: `<brand>.com/press` thường có tải về hình sản phẩm HD
3. **Cắt khung hình launch video chính thức**: Dùng `yt-dlp` tải video YouTube, ffmpeg trích xuất vài khung hình HD
4. **Wikimedia Commons**: Tền miền công cộng thường có
5. **Dự phòng AI tạo ra** (nano-banana-pro): Đưa hình ảnh sản phẩm thực tế làm tham khảo cho AI, để nó tạo ra biến thể phù hợp với phân cảnh animation. **Đừng dùng CSS/SVG vẽ tay thay thế**

```bash
# Ví dụ: Tải hero image sản phẩm trên website chính thức DJI
curl -A "Mozilla/5.0" -L "<hero-image-url>" -o assets/<brand>-brand/product-hero.png
```

**3.3 Ảnh chụp UI (Bắt buộc cho sản phẩm kỹ thuật số)**

- Ảnh chụp màn hình sản phẩm trên App Store / Google Play (Lưu ý: Có thể là mockup chứ không phải UI thật, cần đối chiếu)
- Section screenshots trên trang chính thức
- Cắt khung hình video demo sản phẩm
- Ảnh chụp phát hành trên Twitter/X chính thức của sản phẩm (Thường là phiên bản mới nhất)
- Khi người dùng có tài khoản, chụp màn hình trực tiếp giao diện sản phẩm thực tế

**3.4 · Ngưỡng chất lượng vật liệu - Quy tắc "5-10-2-8" (Quy tắc sắt)**

> **Quy tắc của Logo khác với các vật liệu khác**. Logo có là bắt buộc phải dùng (Không có thì dừng lại hỏi người dùng); các vật liệu khác (Hình sản phẩm/UI/Hình tham khảo/Hình minh họa) tuân theo ngưỡng chất lượng "5-10-2-8".
>
> Nguyên văn Hoa Thúc ngày 20-04-2026: "Nguyên tắc của chúng ta là tìm kiếm 5 vòng, tìm thấy 10 vật liệu, chọn ra 2 cái tốt. Mỗi cái cần chấm điểm 8/10 trở lên, thà ít một chút chứ không làm cho có để hoàn thành nhiệm vụ."

| Chiều | Tiêu chuẩn | Phản mô hình |
|---|---|---|
| **5 vòng tìm kiếm** | Tìm kiếm chéo nhiều kênh (Website chính thức / press kit / Mạng xã hội chính thức / Cắt khung YouTube / Wikimedia / Chụp màn hình tài khoản người dùng), không phải một vòng bắt 2 cái đầu tiên rồi dừng | Dùng trực tiếp kết quả trang đầu tiên |
| **10 ứng viên** | Gom ít nhất 10 cái dự phòng rồi mới bắt đầu lọc | Chỉ bắt 2 cái, không có sự lựa chọn |
| **Chọn 2 cái tốt** | Tuyển chọn tinh túy 2 cái từ 10 cái làm vật liệu cuối cùng | Dùng tất cả = Quá tải thị giác + Pha loãng gu thẩm mỹ |
| **Mỗi cái từ 8/10 điểm trở lên** | Không đủ 8 điểm **thà không dùng**, dùng placeholder trung thực (Khối xám+nhãn chữ) hoặc AI tạo ra (nano-banana-pro lấy tham khảo chính thức làm nền) | Vật liệu 7 điểm làm cho có tiến vào brand-spec.md |

**Chiều chấm điểm 8/10** (Ghi lại vào `brand-spec.md` khi chấm điểm):

1. **Độ phân giải** · ≥2000px (Kịch bản in ấn/Màn hình lớn ≥3000px)
2. **Độ rõ ràng bản quyền** · Nguồn chính thức > Tền miền công cộng > Vật liệu miễn phí > Nghi vấn ăn cắp hình (Nghi vấn ăn cắp hình cho 0 điểm trực tiếp)
3. **Độ ăn khớp với khí chất thương hiệu** · Thống nhất với "Từ khóa khí chất" trong brand-spec.md
4. **Tính thống nhất ánh sáng/Bố cục/Phong cách** · 2 vật liệu đặt cạnh nhau không chọi nhau
5. **Năng lực tự sự độc lập** · Có thể tự mình thể hiện một vai trò tự sự (Không phải trang trí)

**Tại sao ngưỡng này là quy tắc sắt**:
- Triết lý của Hoa Thúc: **Thà thiếu còn hơn ẩu**. Vật liệu làm cho có còn tồi tệ hơn là không có——Làm ô nhiễm gu thị giác, truyền tải tín hiệu "không chuyên nghiệp"
- **Bản định lượng của "Một chi tiết làm 120%, các chi tiết khác làm 80%"**: 8 điểm là mức sàn của "Các chi tiết khác 80%", vật liệu hero thực sự phải 9-10 điểm
- Người tiêu dùng khi xem tác phẩm, mỗi một phần tử thị giác đều đang **cộng điểm hoặc trừ điểm**. Vật liệu 7 điểm = Mục trừ điểm, thà để trống

**Ngoại lệ của Logo** (Nhắc lại): Có là bắt buộc phải dùng, không áp dụng "5-10-2-8". Bởi vì logo không phải vấn đề "chọn 1 trong nhiều", mà là vấn đề "gốc rễ độ nhận diện"——dù bản thân logo chỉ có 6 điểm, cũng mạnh hơn gấp 10 lần việc không có logo.

##### Step 4 · Xác minh + Trích xuất (Không chỉ grep giá trị màu)

| Tài sản | Hành động xác minh |
|---|---|
| **Logo** | File tồn tại + SVG/PNG có thể mở + Ít nhất hai phiên bản (Nền tối/Nền sáng) + Nền trong suốt |
| **Hình sản phẩm** | Ít nhất một hình độ phân giải 2000px+ + Tách nền hoặc nền sạch + Nhiều góc độ (Góc nhìn chính, Chi tiết, Phân cảnh) |
| **Ảnh chụp UI** | Độ phân giải thực tế (1x / 2x) + Là phiên bản mới nhất (Không phải bản cũ) + Không ô nhiễm dữ liệu người dùng |
| **Giá trị màu** | `grep -hoE '#[0-9A-Fa-f]{6}' assets/<brand>-brand/*.{svg,html,css} \| sort \| uniq -c \| sort -rn \| head -20`, lọc bỏ đen trắng xám |

**Cảnh giác ô nhiễm thương hiệu minh họa**: Trong ảnh chụp sản phẩm thường có màu thương hiệu demo của người dùng (Như ảnh chụp công cụ nào đó demo màu đỏ Heytea), đó không phải là màu của công cụ đó. **Khi xuất hiện đồng thời hai màu mạnh bắt buộc phải phân biệt**.

**Nhiều mặt cắt thương hiệu**: Màu marketing trên trang chính thức và màu UI sản phẩm của cùng một thương hiệu thường khác nhau (Lovart trang chính thức kem ấm+cam, UI sản phẩm là Charcoal + Lime). **Cả hai bộ đều là thật**——Chọn mặt cắt phù hợp dựa trên kịch bản giao hàng.

##### Step 5 · Đóng cứng thành file `brand-spec.md` (Template bắt buộc phải che phủ tất cả tài sản)

```markdown
# <Brand> · Brand Spec
> Ngày thu thập: YYYY-MM-DD
> Nguồn tài sản: <Liệt kê nguồn tải về>
> Độ hoàn chỉnh tài sản: <Hoàn chỉnh / Một phần / Suy luận>

## 🎯 Tài sản cốt lõi (Công dân hạng nhất)

### Logo
- Phiên bản chính: `assets/<brand>-brand/logo.svg`
- Phiên bản ngược màu nền sáng: `assets/<brand>-brand/logo-white.svg`
- Kịch bản sử dụng: <Đầu phim/Cuối phim/Watermark góc/Toàn cục>
- Cấm biến dạng: <Không được kéo giãn/Đổi màu/Thêm viền>

### Hình sản phẩm (Sản phẩm thực thể bắt buộc điền)
- Góc nhìn chính: `assets/<brand>-brand/product-hero.png` (2000×1500)
- Hình chi tiết: `assets/<brand>-brand/product-detail-1.png` / `product-detail-2.png`
- Hình phân cảnh: `assets/<brand>-brand/product-scene.png`
- Kịch bản sử dụng: <Đặc tả/Xoay/So sánh>

### Ảnh chụp UI (Sản phẩm kỹ thuật số bắt buộc điền)
- Trang chủ: `assets/<brand>-brand/ui-home.png`
- Tính năng cốt lõi: `assets/<brand>-brand/ui-feature-<name>.png`
- Kịch bản sử dụng: <Hiển thị sản phẩm/Dashboard hiện dần/Demo so sánh>

## 🎨 Tài sản hỗ trợ

### Bảng màu
- Primary: #XXXXXX  <Ghi chú nguồn>
- Background: #XXXXXX
- Ink: #XXXXXX
- Accent: #XXXXXX
- Màu cấm: <Hệ màu thương hiệu chỉ định rõ ràng không dùng>

### Font chữ
- Display: <font stack>
- Body: <font stack>
- Mono (Dùng cho HUD dữ liệu): <font stack>

### Chi tiết chữ ký
- <Những chi tiết nào được "làm 120%">

### Vùng cấm
- <Rõ ràng không được làm: Ví dụ Lovart không dùng màu xanh dương, Stripe không dùng màu ấm độ bão hòa thấp>

### Từ khóa khí chất
- <3-5 tính từ>
```

**Kỷ luật thực thi sau khi viết xong spec (Yêu cầu cứng)**:
- Tất cả HTML bắt buộc phải **trích dẫn** đường dẫn file tài sản trong `brand-spec.md`, không cho phép dùng hình bóng CSS/Vẽ tay SVG thay thế
- Logo làm `<img>` trích dẫn file thực tế, không vẽ lại
- Hình sản phẩm làm `<img>` trích dẫn file thực tế, không dùng hình bóng CSS thay thế
- Biến CSS bơm từ spec: `:root { --brand-primary: ...; }`, HTML chỉ dùng `var(--brand-*)`
- Điều này làm cho tính thống nhất thương hiệu từ "dựa vào tự giác" biến thành "dựa vào cấu trúc"——muốn tạm thời thêm màu phải sửa spec trước

##### Dự phòng thất bại toàn quy trình

Xử lý riêng theo loại tài sản:

| Thiếu sót | Xử lý |
|---|---|
| **Hoàn toàn không tìm thấy Logo** | **Dừng lại hỏi người dùng**, đừng cố làm (logo là gốc rễ độ nhận diện thương hiệu) |
| **Không tìm thấy hình sản phẩm (Sản phẩm thực thể)** | Ưu tiên nano-banana-pro AI tạo ra (Lấy hình tham khảo chính thức làm nền) → Thứ chọn là yêu cầu người dùng cung cấp → Cuối cùng mới là placeholder trung thực (Khối xám+nhãn chữ, ghi rõ "Hình sản phẩm chờ bổ sung") |
| **Không tìm thấy ảnh chụp UI (Sản phẩm kỹ thuật số)** | Yêu cầu người dùng chụp màn hình tài khoản của chính mình → Cắt khung hình video demo chính thức. Không dùng mockup generator ghép vào |
| **Hoàn toàn không tìm thấy giá trị màu** | Đi theo "Chế độ cố vấn hướng thiết kế", gợi ý 3 hướng cho người dùng và ghi chú assumption |

**Nghiêm cấm**: Không tìm thấy tài sản liền âm thầm dùng hình bóng CSS/Chuyển sắc thông thường để cố làm——đây là anti-pattern lớn nhất của giao thức. **Thà dừng lại hỏi, còn hơn làm cho có**.

##### Ví dụ phản diện (Những cái bẫy thực tế đã dẫm)

- **Animation Kimi**: Dựa vào ký ức đoán "chắc là màu cam", thực tế Kimi là màu xanh dương `#1783FF`——làm lại toàn bộ một lần
- **Thiết kế Lovart**: Lấy màu đỏ Heytea demo trong ảnh chụp màn hình sản phẩm làm màu của chính Lovart——suýt nữa phá hỏng toàn bộ thiết kế
- **Animation ra mắt DJI Pocket 4 (20-04-2026, trường hợp thực tế kích hoạt nâng cấp giao thức này)**: Đi theo giao thức phiên bản cũ chỉ trích xuất giá trị màu, không tải logo DJI, không tìm hình sản phẩm Pocket 4, dùng hình bóng CSS thay thế sản phẩm——làm ra là "Animation công nghệ nền đen thông thường+accent cam", không có độ nhận diện DJI. Nguyên văn Hoa Thúc: "Nếu không, chúng ta đang thể hiện điều gì?" → Nâng cấp giao thức.
- Trích xuất màu xong không viết vào brand-spec.md, đến trang thứ ba là quên mất giá trị màu chính, ứng biến thêm vào một hex "gần giống nhưng không phải" → Tính thống nhất thương hiệu sụp đổ
- **PPT So sánh 5 Coding Agent lớn (06-06-2026, trường hợp thực tế kích hoạt mở rộng điều kiện kích hoạt)**: agent đánh giá nhiệm vụ thành "PPT + Không có tham khảo phong cách" đi theo Cố vấn hướng thiết kế Fallback, chỉ trích xuất màu thương hiệu của 5 nhà rồi spawn 3 bộ logic thiết kế, **5 logo sản phẩm (Claude Code / Cursor / Codex / Copilot / Trae) không lấy một cái nào**——bị Hoa Thúc bắt quả tang "Tại sao chúng ta không đi lấy logo của những sản phẩm này". Nguyên nhân gốc rễ: Đánh giá nhầm "So sánh / Deck bảng xếp hạng" thành không kích hoạt §1.a (Tưởng rằng §1.a chỉ quản lý "Làm vật liệu cho một khách hàng đơn lẻ"), và trong đường dẫn Fallback không có bất kỳ điểm kiểm tra logo nào. → Sửa chữa: ①Điều kiện kích hoạt mở rộng thành 2 loại (Bao gồm "Chỉ tên trong thiết kế/Xếp hàng sản phẩm thực tế") ②Fallback không miễn trừ lấy logo ③Phase 3.5 thêm "Cửa phụ logo sản phẩm có tên" bắt buộc phải qua trước khi spawn ④Step 3.1 bổ sung chuỗi lấy hình tin cậy svgl/simpleicons/Google favicon.

##### Cái giá của giao thức vs Cái giá của việc không làm

| Phân cảnh | Thời gian |
|---|---|
| Đi thông giao thức đúng đắn | Tải logo 5 min + Tải 3-5 hình sản phẩm/UI 10 min + grep giá trị màu 5 min + Viết spec 10 min = **30 phút** |
| Cái giá của việc không làm giao thức | Làm ra animation thông thường không có độ nhận diện → Người dùng bắt làm lại 1-2 giờ, thậm chí làm lại từ đầu |

**Đây là khoản đầu tư rẻ nhất cho sự ổn định**. Đặc biệt đối với hợp đồng thương mại/hội nghị ra mắt/dự án khách hàng quan trọng, giao thức tài sản 30 phút là tiền giữ mạng.
