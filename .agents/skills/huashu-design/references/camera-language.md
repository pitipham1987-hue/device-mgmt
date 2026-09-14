# Camera Language · Hệ thống Đạo diễn Quay phim (Camera Language & Direction)

> **Khi nào đọc tệp này**: Trước khi trong màn hình xuất hiện bất kỳ chuyển động nào ở "cấp độ ống kính" —— zoom / pan / orbit / parallax / chuyển cảnh / định cảnh tạ mạc, chỉ cần cái chuyển động là "ống kính" chứ không phải "phần tử", hãy đọc ở đây trước rồi mới viết timeline.
> Phần tử chuyển động thế nào (vào cảnh/stagger/cảm giác vật lý) thuộc về `animation-best-practices.md`;
> Tệp này trả lời câu hỏi **ống kính khi nào động, động rộng bao nhiêu, động bao lâu, giữa các ống kính nối với nhau thế nào**.
> Hiện thực hóa có thể chạy được ở phía GSAP (rig container, dịch chuyển PageCam, helper thời lượng logarit) xem tại mục "Công thức Camera Rig" của `gsap-recipes.md`, tệp này chỉ đưa ra nhận định thiết kế và công thức.
>
> Quy ước đánh dấu nguồn gốc tham số: **(HuaRec)** = Tham số đo thực tế từ Hệ thống Đạo diễn Quay phim Hoa Lục Studio;
> **(shotcraft)** = Hệ thống 106 thẻ ống kính video-shotcraft; **(Thực tế)** = Thực chiến dự án skill này;
> **(Dự đoán)** = Học hỏi từ từ vựng điện ảnh chung, tham số chờ đo thực tế hiệu chỉnh.

---

## §0 · Lập luận · Quay phim là Chế độ Ngân sách, không phải Chế độ Kỹ xảo

Hầu hết chuyển động ống kính của hoạt ảnh do AI tạo ra đều mang tư duy "Chế độ Kỹ xảo": Chỗ nào thêm được zoom là thêm zoom, ống kính chuyển động càng nhiều càng "cao cấp".
Đây là nguồn gốc chung của sự chóng mặt và cảm giác rẻ tiền. Mô hình tư duy đúng đắn đến từ 2 công lý (HuaRec):

- **A1 Hằng số Nhìn thấy được**: Tại bất kỳ thời điểm nào, thứ khán giả nên xem bắt buộc phải nằm trong vùng nhìn thấy được (Bao gồm 8% lề an toàn).
  Ống kính vi phạm thà hạ bội số, gộp ống kính chứ không quay.
- **A2 Ngân sách Thỏa mái**: Mỗi lần thay đổi ống kính đều là một khoản tiêu dùng sự chú ý, bắt buộc phải quản lý theo ngân sách.
  Ngân sách tiêu hết rồi, ống kính tốt đến mấy cũng không quay.

| Mục ngân sách | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Khoảng cách thay đổi ống kính liền kề | ≥2.6-3.0s (Mức kiềm chế 5.0s) | Dưới 2.6s khán giả bắt đầu chóng mặt; Cắt nhanh kiểu MTV không áp dụng cho demo sản phẩm (HuaRec) |
| Trong cửa sổ 15s bất kỳ thay đổi ống kính | ≤4-5 lần (Mức kiềm chế 3 lần) | Vượt quá thì cắt bỏ cảnh có động cơ yếu nhất, chứ không nén khoảng cách (HuaRec) |
| Mức sàn bội số đẩy tới | 1.25x | Dưới 1.25x sự thay đổi thị giác của zoom không đủ cảm nhận, thuần túy là rung lắc, không quay (Định cảnh 1.06x là ngoại lệ duy nhất) (HuaRec) |
| Mức trần bội số đẩy tới | 2.3x (Mức kiềm chế 1.8x) | Cao hơn nữa độ đậm đặc pixel không gánh nổi, đổi tư liệu trước rồi mới bàn bội số (HuaRec) |
| Số ống kính mỗi phút | Kiềm chế ≤4 cảnh/phút, Thông thường ≤6 cảnh/phút | "Giữ nhịp thở, không phải MTV" (HuaRec) |

Chồng thêm 2 công lý phong cách (shotcraft):

1. **Cảm giác điện ảnh = Quay phim × Ánh sáng & Bóng tối × Nhịp điệu × Âm thanh, không đồng nghĩa với hoạt ảnh khoe kỹ xảo**. 4 chiều kích mỗi cái đạt chuẩn, thắng hẳn một chiều kích kéo kịch trần.
2. **Thiên vị nhịp điệu một chiều: Thà chậm chứ không nhanh**. Phản hồi của người dùng trong lịch sử toàn bộ hướng về "làm chậm lại / tạm dừng", không có cái nào hướng về "làm nhanh lên".
   Khi không chắc chắn về thời lượng, chọn cái dài hơn; Khi không chắc có nên động ống kính hay không, chọn không động.

2 điều này đồng nguồn với "Nhường bước cho khán giả" trong best-practices §0.2: Ống kính là thay mặt cho đôi mắt của khán giả để đưa ra quyết định,
quyết định đưa ra càng ít, càng chuẩn, khán giả càng tin tưởng bạn.

---

## §1 · Từ vựng Ngôn ngữ Ống kính · Bảng Quyết định Động cơ Quay phim

Mỗi hành động của ống kính phải hỏi động cơ trước: **Cảnh này thay mặt khán giả trả lời câu hỏi gì?** Không trả lời được thì không động.

| Ống kính | Động cơ (Khi nào đưa ra) | Khoảng tham số | Điều cấm |
|---|---|---|---|
| **push in đẩy tới** | "Tiếp theo hãy nhìn vào đây": Hội tụ một phần tử UI / Dữ liệu / Từ khóa cụ thể; Độ căng thẳng leo leo | Bội số các mức 1.3 / 1.45 / 1.8 / 2.3 (Xem §4); Thời lượng đi theo công thức logarit; Vào cảnh trước 0.15s (HuaRec) | <1.25x Không đẩy; Trong lúc cuộn / chuyển trang / phát video dạng toàn màn hình không đẩy ("Vừa cuộn vừa đẩy sẽ bị chóng mặt"); Giữa đoạn thuyết minh không động ống kính (Xem §6 move on pause) (HuaRec) |
| **pull out kéo xa** | Hé lộ toàn cảnh và ngữ cảnh: "Hóa ra nó thuộc về một hệ thống lớn hơn"; Kết thúc tạ mạc | Thời lượng giống công thức logarit; Tạ mạc kéo ra 0.55s + ≥0.8s tạm dừng toàn cảnh (HuaRec) | Cấm bơm đẩy "Ra-Vào-Ra": Khi khoảng trống giữa các cảnh nhỏ hơn thời lượng quá độ thì nối trực tiếp sang cảnh tiếp theo, không quay về 1x (Xem §5) (HuaRec) |
| **pan bình移** | Di chuyển giữa 2 tiêu điểm tầm trung (Khoảng cách chuẩn hóa 0.22-0.45); Một cảnh quét qua nhiều phần tử song song | Bội số kết hợp ≥1.25 mới đáng để bình移; Slanted pan dùng hình sin tần số kép (Tỷ lệ tần số X/Y 0.22:0.35, biên độ 30-40px) (HuaRec / Skill này có sẵn) | Khoảng cách tiêu điểm >0.45 không bình移 (Nhảy chéo), bỏ cảnh; Pan đơn trục thuần túy mang cảm giác máy móc, ưu tiên pan chéo |
| **orbit xoay vòng** | Đặc tả chất cảm đơn nhân vật chính, một cảnh có "cảm giác thực thể" mạnh nhất; Lập truyện cho phần tử hero | rotY chủ đạo + persp 1100-1200px; Góc máy thực tế rotX46/rotY−30/rotZ9 → rotX42/rotY26/rotZ−7 (shotcraft) | Một thủ pháp toàn phim chỉ làm nhân vật chính một lần (shotcraft); Màn hình mật độ thông tin cao CẤM dùng (Góc máy phục vụ tính đọc được, chữ nhiều thì nhìn thẳng) |
| **dolly zoom** | Khoảnh khắc hé lộ "Thế giới quan đảo ngược": Chủ thể không đổi, ngữ cảnh đổi lớn | Công thức giả xem §8: Chủ thể đóng đinh, nền scale 1→2.0-2.5 + opacity ≤0.6 (shotcraft) | Toàn phim tối đa một lần; Không có độ lệch tự sự mà dùng nó = Thuần khoe kỹ xảo |
| **Tĩnh** | Đọc chữ, Demo tương tác tốc độ người thật, Ống kính mật độ thông tin cao; Đáp án mặc định khi ngân sách không đủ | Chữ hiệu thương hiệu định vị hold ≥1s; Hoạt ảnh hàng loạt kết thúc 0.5s tĩnh; Biên độ hành động chủ thể mở màn ≥3s (shotcraft) | Không có. Tĩnh không động luôn là lựa chọn hợp pháp, "Không đưa ống kính" bản thân nó đã là quyết định của đạo diễn |

Hai quy tắc chiều ngang:

- **Cảm giác tốc độ đến từ gia tốc, không phải tốc độ nhanh đều** (shotcraft). Chuyển động đều đọc ra là PPT rẻ tiền;
  Muốn có cảm giác "nhanh", dùng đoạn tăng tốc ngắn dồn dập + phanh dài, chứ không phải cắt đôi toàn bộ duration.
- **Góc máy phục vụ tính đọc được** (shotcraft): Ống kính mật độ thông tin cao nhìn thẳng; Đặc tả chữ dùng góc máy nằm ngang hướng nghiêng
  (rotY chủ đạo, rotX rất nhỏ); Cấm nghiêng một dao hạ thủ toàn cục; Video quảng cáo sản phẩm mặc định không thêm rung lắc cầm tay.

---

## §2 · Chọn lựa zoom vs dolly · Phán quyết 3D Thật Giả

"Đẩy tới" có 2 cách hiện thực hóa, cảm giác quan sát hoàn toàn khác nhau, chọn kiểu trước rồi mới viết code:

| Chiều kích | zoom (Co giãn scale) | dolly (perspective + translateZ tiến tới) |
|---|---|---|
| Thấu thị (Parallax) | Không có. Tất cả các lớp phóng đại cùng tỷ lệ, màn hình là "Một bức ảnh bị phóng đại" | Có. Lớp gần nhanh, lớp xa chậm, màn hình là "Ống kính đang tiến lên trong không gian" |
| Cảm giác xem | Sạch sẽ, Dạng thông tin, Phù hợp đặc tả UI / Hội tụ dữ liệu | Cảm giác không gian, Cảm giác điện ảnh, Phù hợp hiển thị hero / Đoạn bầu không khí |
| Chi phí | Thấp: Một transform duy nhất | Cao: Cần cấu trúc phân lớp + preserve-3d, và có vấn đề mờ vỡ nét rasterize (§3.4) |
| Quy tắc chọn | Nội dung là thông tin mặt phẳng (Giao diện, Tài liệu, Biểu đồ) → zoom | Nội dung có phân cấp rõ ràng "Tiền cảnh/Chủ thể/Hậu cảnh", và phân cấp này đáng để được nhìn thấy → dolly |

**Phán quyết 3D Thật Giả** (Giải tỏa ranh giới mâu thuẫn 2 văn bản cũ trong skill này):

- Phần tử tham gia 3D **≤8 cái** → Dùng phân lớp translateZ thật (Công thức góc vàng của best-practices §4.7 cứ dùng)
- Phần tử **≥20 cái** → Từ bỏ 3D thật, dùng shadow / blur / chênh lệch độ sáng làm độ sâu giả (Lập trường của hero-case-study)
- Từ 8-20 cái → Hỏi một câu: Đoạn ống kính này có cần thấu thị (parallax) không? Cần mới lên 3D thật, không cần thì độ sâu giả.
  Chi phí của 3D thật không nằm ở lúc viết, mà nằm ở lúc chỉnh: Mỗi lần thêm 1 lớp là thêm 1 nhóm "Biến dạng thấu thị + Chữ bị mờ + Đâm xuyên phân cấp" phải tra cứu.

---

## §3 · Quy ước Hiện thực hóa Camera Rig

Chuyển động ống kính và hoạt ảnh phần tử **không được phép tranh giành cùng một transform**. Tất cả chuyển động cấp ống kính thu về một container chuyên trách.

### 3.1 Cấu trúc Container Phân lớp

```html
<div id="viewport">          <!-- Viewport cố định, overflow: hidden, giữ perspective -->
  <div id="camera">          <!-- Tầng ống kính: Chỉ gánh transform máy ảnh, ngoài ra không làm gì khác -->
    <div id="world">         <!-- Tầng thế giới: Tất cả nội dung màn hình ở đây, hoạt ảnh phần tử chỉ động bên trong world -->
      ...Nội dung cảnh...
    </div>
  </div>
  <div id="hud">             <!-- Phụ đề / Góc nhãn / chrome: Cùng cấp với #camera, tự nhiên không đi theo ống kính -->
  </div>
</div>
```

Quy tắc phân công thép:

- Trên `#camera` chỉ xuất hiện tween ống kính (Các thuộc tính translate / scale / rotate / zoom), phần tử vào cảnh, stagger,
  trạng thái hover đều viết trên phần tử bên trong `#world`. Hai tầng không biết nhau, ống kính bất kỳ lúc nào cũng có thể dàn xếp lại toàn bộ mà không đụng tới hoạt ảnh phần tử
- Phụ đề và chrome **ưu tiên đặt ở `#hud`**, chi phí bằng 0 để giữ tĩnh; Chỉ khi các chú thích "bắt buộc đi theo một phần tử nào đó trong world,
  nhưng kích thước chữ phải giữ nguyên" (Như tooltip đi theo), mới làm counter-transform trên phần tử đó:
  `scale(1/zoom)` ngược hướng triệt tiêu sự co giãn của ống kính, mỗi khung hình cập nhật đồng bộ với máy ảnh
- **transform-origin chính là điểm mục tiêu đẩy tới**: Zoom mặt phẳng đặt origin vào tâm phần tử mục tiêu rồi scale,
  tương đương với "Ống kính nhắm vào nó đẩy lại gần". Chế độ PageCam gánh vác cùng một trách nhiệm thông qua cx/cy

### 3.2 Mô hình Khung hình khóa PageCam (shotcraft, Toán học Máy ảnh 2.5D)

Định nghĩa trạng thái ống kính thành đối tượng khung hình khóa, chuyển động ống kính = Nội suy giữa các khung hình khóa:

```
{ frame, cx, cy, zoom, rotX, rotY, rotZ, persp }
```

cx/cy là **điểm ống kính nhắm vào trong hệ tọa độ thế giới**, zoom là bội số. Lấy canvas 1920×1080 làm ví dụ
(Kích thước khác đổi 960/540 thành W/2, H/2):

**Chế độ mặt phẳng** (Không xoay, thuần zoom + pan):

```
transform: translate(960 − cx·zoom, 540 − cy·zoom) scale(zoom)
transform-origin: 0 0
```

**Chế độ 3D** (Có rotX/rotY/rotZ):

```
Tầng ngoài (#camera): perspective: persp·zoom;  perspective-origin: 960px 540px
Tầng trong (#world):  zoom: {zoom};                        /* Chú ý là thuộc tính CSS zoom, xem §3.4 */
                 Tx = 960/zoom − cx;  Ty = 540/zoom − cy
                 transform: translate(Tx, Ty) rotateY() rotateX() rotateZ()
                 transform-origin: cx cy
                 transform-style: preserve-3d
```

Tham số góc máy điển hình (shotcraft thực tế): Zoom toàn trang 0.78 → Đặc tả 2.6;
Quay nghiêng rotY34 / rotX8 / persp1200 (Quay nghiêng tốt hơn quay từ trên xuống, rotY chủ đạo + rotX chỉ cho một chút);
Góc máy khởi đầu/kết thúc của orbit xem bảng §1.

### 3.3 Thi công rig cần chú ý (Bẫy riêng của ống kính)

- **pan lộ mép**: `#world` bắt buộc phải lớn hơn viewport (4 phía mở rộng bleed ≥ Biên độ pan tối đa + 8% lề),
  nếu không khi bình移 sẽ lộ ra khoảng trắng ngoài canvas. Đây là mặt ngược lại của công lý A1 khả năng nhìn thấy: Thứ không nên xem cũng không được nhìn thấy
- **perspective bị ngắt quãng**: Bất kỳ tầng trung gian nào giữa `#camera` và `#world` thêm một trong các thuộc tính
  `overflow: hidden`, `filter`, `opacity <1` đều tạo ra stacking context mới,
  giết chết preserve-3d, phân lớp 3D tức thì biến thành phẳng
- **Bọc lót vượt ranh giới từng khung hình** (HuaRec): Khi tiêu điểm ống kính đi theo điểm chuyển động, mỗi khung hình kiểm tra xem mục tiêu có vượt ra ngoài vùng nhìn thấy được không,
  vượt ra thì giữ nguyên bội số, chỉ làm chỉnh sửa nhỏ nhất dọc theo trục vượt ranh giới để kéo lại biên giới. Thà ống kính "nhường một bước", không cho phép mục tiêu ra khỏi màn hình

### 3.4 Kỹ thuật Rasterize CSS zoom · Gốc rễ chữa chữ 3D bị mờ (Tri thức đắt nhất toàn thư viện, shotcraft)

**Vấn đề**: Dưới chế độ 3D dùng `transform: scale()` để phóng đại trang, Chromium rasterize theo **kích thước bố cục** của phần tử,
nếu phóng đại bitmap, chữ nhất định bị mờ. Bội số càng cao mờ càng nặng, trên 2x không thể giao hàng.

**Giải pháp**: Phóng đại không đi theo `transform: scale`, đi theo **thuộc tính CSS `zoom`**. `zoom` là co giãn cấp bố cục,
Chromium layout và rasterize lại theo kích thước sau khi phóng đại, chữ giữ được độ sắc nét cấp vector ở bất kỳ bội số nào.
Công thức chế độ 3D ở §3.2 tầng trong viết `zoom: {zoom}` chứ không viết `scale({zoom})`, chính là vì điều này.

Các điểm phối hợp:

| Điểm phối hợp | Cách làm | Nguồn |
|---|---|---|
| Bù tọa độ | `zoom` thay đổi hệ tọa độ bố cục, lượng translate phải chia cho zoom: `Tx = 960/zoom − cx` (Công thức §3.2 đã bao hàm) | (shotcraft) |
| Mối quan hệ với lệnh cấm reflow | gsap-recipes §6.2 cấm tween thuộc tính bố cục là vì rung giật snap số nguyên; `zoom` là co giãn cấp toàn trang, lượng snap không cảm nhận được, và lợi ích sắc nét của chữ lớn hơn nhiều. **Kỹ thuật này là ngoại lệ hợp pháp duy nhất của §6.2, chỉ dùng trên tầng máy ảnh `#world`** | (Thực tế) |
| Môi trường render | Hoàn toàn áp dụng dưới dạng render seek từng khung hình offline của HyperFrames / Playwright: Chi phí thời gian re-layout mỗi khung hình không ảnh hưởng đến sản phẩm, chỉ ảnh hưởng thời lượng render. Preview trình duyệt thời gian thực có thể rơi khung hình, là bình thường, lấy sản phẩm render làm chuẩn | (shotcraft) |
| Tăng cường tư liệu bitmap | Chụp màn hình toàn trang dùng lấy mẫu 2x; Phần tử đặc tả chuẩn bị thêm ảnh chụp màn hình 4x riêng, trong thời kỳ đẩy tới dùng 6f fade đan xen che đi vân chất bội số thấp | (shotcraft) |
| Bầu không khí độ sâu trường ảnh | DoF chỉ làm bầu không khí: Dải chuyển màu đỉnh + blur + mask, không làm độ sâu trường ảnh thật từng tầng | (shotcraft) |

---

## §4 · Hướng mượt và Thời lượng Ống kính

### 4.1 Từ vựng Hướng mượt (Easing)

| Kịch bản | easing | Cảm giác điều chỉnh |
|---|---|---|
| Quay phim chủ động (Đẩy tới/Kéo xa, có điểm đầu điểm kết thúc rõ ràng) | `cubic-bezier(0.65,0,0.35,1)` = GSAP `power3.inOut` | Hai đầu đều vững, cảm giác "Đạo diễn đưa ống kính"; **Tuyệt đối không tuyến tính, tuyệt đối không lò xo quá chớn** (HuaRec) |
| Quay phim dạng đi theo (Ống kính đuổi theo một hành động đã bắt đầu) | `cubic-bezier(0.33,0,0.15,1)` | Xuất phát nhẹ nhàng, phanh cực dài, ống kính giống như "đuổi theo" chứ không phải "cắt qua"; Mặc định máy ảnh shotcraft |
| Trôi liên tục (idle drift, tuần tra đều tốc) | `sine.inOut` yoyo hoặc `none` | Cảnh duy nhất cho phép máy ảnh đều tốc (Quy tắc có sẵn gsap-recipes §1); Hành động có điểm đầu kết thúc CẤM dùng |
| Làm mượt đi theo con trỏ/Tiêu điểm | `quickTo` + ~0.15s làm mượt; Nội suy đường đi Catmull-Rom | Tư duy pha 0 của EMA hướng trước + hướng sau: Theo sát nhưng không rung (HuaRec) |

Phán quyết khi 2 bộ mặc định xung đột: Đẩy kéo đơn lần tin HuaRec (power3.inOut), di chuyển phức hợp,
nhiều đoạn ống kính liên tục tin shotcraft (0.33,0,0.15,1).

### 4.2 Công thức logarit thời lượng zoom (Thời lượng cố định là nguồn gốc của cảm giác nghiệp dư)

Thời lượng tất cả đẩy kéo do lượng thay đổi bội số quyết định, đảm bảo "tốc độ thị giác" của zoom ở bất kỳ biên độ nào đều nhất quán:

```
duration = 0.55 × |ln(zoom₂ / zoom₁)| / ln 2      clamp vào [0.30, 0.94] giây
```

1→2x Đẩy tới vừa đúng 0.55s; 1→1.3x Khoảng 0.30s (Chạm đáy); 0.78→2.6x Chạm đỉnh 0.94s. (HuaRec)
Cảnh lớn quá độ lâu hơn, cảnh nhỏ ngắn hơn, phòng "Một phát vèo tới nơi" cũng phòng "Lề mề".

### 4.3 Bảng mức zoom

| Mức | Bội số | Mục đích | Cảm giác điều chỉnh |
|---|---|---|---|
| Đẩy nhẹ định cảnh | 1.06x | Chỉ dùng cho định cảnh mở màn (§6), khán giả không cảm nhận được zoom, chỉ cảm nhận "Màn hình đang sống" | Mức duy nhất cho phép thấp hơn 1.25x (HuaRec) |
| Đẩy nhẹ | 1.3x | Hội tụ mang tính gợi ý: Không ngắt đoạn đọc toàn cục, chỉ là "Chú ý vùng này" | |
| Đẩy vừa | 1.45x | Đặc tả UI tiêu chuẩn: Một panel / Một đoạn code | |
| Đẩy mạnh | 1.8x | Đặc tả đơn phần tử: Một nút bấm / Một con số | Mức trần của mức kiềm chế (HuaRec) |
| Mức trần | 2.3x | Đặc tả cực hạn, tư liệu bắt buộc phải gánh nổi (Ảnh chụp 2x / Slice 4x, §3.4) | Vượt quá thì đổi tư liệu, không đẩy cố (HuaRec) |

Công thức định cảnh (HuaRec): `scale = 0.8 / max(Chiều rộng chuẩn hóa khung bao mục tiêu, Chiều cao)`, rồi clamp vào khoảng mức.
Nội dung chiếm 80% vùng nhìn thấy được, chừa 20% nhịp thở, không đụng trần.

### 4.4 Ngân sách Nhịp điệu (Liên động với Bảng ngân sách §0)

- Khoảng cách thay đổi ống kính liền kề ≥2.6-3.0s; Cửa sổ 15s ≤4-5 lần (HuaRec)
- Trước hành động 0.15s vào ống kính (Ống kính đến trước, hành động xảy ra sau), sau khi hành động kết thúc dừng 1.2s rồi mới đi (HuaRec)
- Hành động nhỏ cô lập thời lượng <1.2s không đáng để đưa ống kính riêng, phòng "Nhấp một cái bơm một cái" (HuaRec)

---

## §5 · Ngữ pháp giữa các Ống kính · Cốt lõi Chống Chóng mặt

**Chóng mặt không phải do một ống kính đơn lẻ gây ra, mà do cách nối giữa các ống kính gây ra.** Bơm đẩy "Ra-Vào-Ra" và nhảy ngang liên tục các tiêu điểm xa
đóng góp phần lớn cảm giác chóng mặt (HuaRec). Đối với 2 cảnh liền kề (Khoảng cách <1.5s), chia làm 3 theo khoảng cách tiêu điểm:

| Khoảng cách chuẩn hóa tiêu điểm | Cách nối | Giải thích |
|---|---|---|
| <0.22 (Gần) | **Gộp cảnh** | Gộp thành 1 cảnh: Lấy khung bao kết hợp của 2 mục tiêu tính lại bội số, một cảnh xem hết |
| 0.22-0.45 (Vừa) | **Đổi sang bình移** | Thà bội số rộng hơn một chút (Bội số kết hợp ≥1.25), một cảnh bình移 qua, không làm "Ra rồi lại Vào" |
| >0.45 (Xa/Đường chéo) | **Bỏ cảnh** | Cắt bỏ cảnh có động cơ yếu hơn. **Tuyệt đối không quay liền 2 tiêu điểm xa**, nhảy chéo ngang là cách nối gây chóng mặt nhất |

Bổ sung 2 quy tắc chuỗi thời gian (HuaRec):

- **Khoảng trống ngắn thì nối trực tiếp**: Khoảng trống giữa các ống kính liền kề nhỏ hơn thời lượng quá độ thì không quay về 1.0x, chuyển trực tiếp từ bội số hiện tại sang
  bội số và tiêu điểm của cảnh tiếp theo. Quay về 1x rồi đẩy tiếp là nguồn gốc trực tiếp của "Cảm giác bơm đẩy"
- **Khoảng trống ≥1.5s mới cho phép "Kéo ra rồi lại đẩy vào"**: Khán giả có đủ thời gian định vị lại trong toàn cảnh, Ra-Vào mới không bị chóng mặt

---

## §6 · Mở & Bế mạc Điện ảnh · Định cảnh, Tạ mạc, move on pause

3 ngữ pháp chi phí cực thấp, cảm giác tác phẩm tăng lên rõ rệt (HuaRec):

1. **Định cảnh (establishing shot)**: Độ dài phim >14s và cảnh chính thức đầu tiên sau 7s,
   mở màn chèn **đẩy nhẹ tâm 1.06x** trong [0, 3.0s]: Bật máy tức là ở trạng thái đẩy nhẹ lại gần, trong 3 giây mượt ra lùi về toàn cảnh.
   Cảm nhận đầu tiên của khán giả là "Ống kính đang sống", chứ không phải "PPT bắt đầu phát rồi"
2. **Quy tắc thép Tạ mạc Toàn cảnh**: Cảnh cuối cùng thu nạp trước, chừa ra 0.55s quá độ kéo ra + **≥0.8s tạm dừng toàn cảnh**.
   Thành phẩm luôn kết thúc bằng sự tĩnh lặng toàn cảnh, **tuyệt đối không dừng đột ngột ở trạng thái đẩy lại gần**. Hoàn toàn tương thích với kết thúc "Dừng lại đột ngột + hold"
   của best-practices: Khung hình hold bắt buộc phải là toàn cảnh
3. **Move on pause**: cut on action, move on pause. Trong hoạt ảnh có thuyết minh, nếu di chuyển ống kính va vào
   giữa đoạn đang nói, hút về điểm im lặng giọng nói gần nhất ở phía trước (Tối đa tiến về trước 0.8s, **chỉ tiến trước không lùi sau**).
   Khán giả di chuyển tầm mắt trong khoảng trống thính giác có chi phí nhận thức thấp nhất. Quy trình thuyết minh (voiceover-pipeline.md) khi sắp xếp ống kính
   trực tiếp lấy khoảng trống ngắt câu của narration làm điểm hút

---

## §7 · Bảng Ngữ pháp Chuyển cảnh · Từ vựng 3 Tầng

Chuyển cảnh là một tầng độc lập: Đường nối chọn kiểu theo **Độ lệch năng lượng**, một đường nối chỉ dùng 1 kiểu, khung hình chuyển cảnh trích từ ngân sách ống kính liền kề.
"Phim quảng cáo phát hành xuất sắc công nhận toàn bộ không có một lần cắt trần nào" (shotcraft).

### 7.1 shot-transitions 6 Kiểu (Chuyển cảnh có cảm giác tồn tại, dùng cho đường nối độ lệch năng lượng lớn)

| Kiểu | Độ lệch năng lượng áp dụng | Tham số | Bẫy |
|---|---|---|---|
| Bôi trắng flash-wash | Cao→Cao, Chuyển đoạn mạnh | Đỉnh trắng 2-4f, 2 bên mỗi bên 5f mờ dần | Trắng nháy chỉ che điểm cắt, không dùng làm trang trí lặp đi lặp lại |
| Xuyên cảnh tối dip-to-dark | Cao→Thấp, Hạ mức cảm xúc | Ép tối xuống mức rgba(20,20,20,0.9), tổng độ dài ≤0.6s | Trong cảnh tối đừng dừng lại, khán giả tưởng phim kết thúc |
| Tiếp sức mất nét defocus-handoff | Vừa→Vừa, Chuyển đổi chủ đề cùng cấp | Blur ra cảnh 0→8px và blur vào cảnh 8px→0 chồng chéo ≥8f | blur diện tích lớn ≤24px (Ràng buộc hiệu năng DoF) |
| Thẻ chữ nền đen title-card | Phân đoạn cấp chương | Hold thẻ chữ ≥1s, trước sau mỗi bên 0.3s quá độ | Toàn phim ≤2 tấm, nhiều quá giống slide |
| Whip-pan Vung ống kính | Thấp→Cao, Năng lượng tăng vọt | 2 đầu hold ≥20f → 8f vung 1.5 màn hình, đỉnh ≥300px/f mới mờ thấu (shotcraft) | Chậm thì thành pan thông thường, không mờ thấu lại thành lộ vụng |
| mask-wipe Xuyên cửa sổ | Chuyển dịch không gian, "Xuyên qua một giao diện đi vào giao diện khác" | Easing mép mask `(0.4,0,0.6,1)` (shotcraft) | Hình dạng mask bắt buộc đến từ phần tử đã có trong màn hình (Cửa sổ/Bo góc card), hình dạng từ không khí là slop |

### 7.2 hidden-cut 3 Kiểu (Chuyển cảnh khán giả không nhận ra là đã cắt)

| Kiểu | Cách làm | Nguồn |
|---|---|---|
| Cắt chéo nháy trắng flash-cut | Nháy trắng đè lên điểm cắt cứng, 2 bên mỗi bên 5f, chỉ che điểm cắt | (shotcraft, tham số thực chứng) |
| Cắt che tiền cảnh | Một phần tử tiền cảnh (Card/Panel/Con trỏ tay) quét qua toàn màn hình trong khoảnh khắc đổi cảnh | (Dự đoán: Từ vựng điện ảnh chung, tham số chờ đo thực tế) |
| Cắt mờ chuyển động | Cắt cứng trên khung hình đỉnh chuyển động tốc độ cao, 2 bên hướng chuyển động nhất quán, motion blur nuốt mất điểm cắt | (Dự đoán: Từ vựng điện ảnh chung, tham số chờ đo thực tế) |

### 7.3 travel 2 Kiểu (Chuyển cảnh liên tục không gian, năng lượng không lệch, cảnh đang di chuyển)

| Kiểu | Cách làm | Bẫy |
|---|---|---|
| Phần tử chia sẻ về vị trí | Một phần tử nào đó của cảnh trước (logo/card) chuyển động liên tục đến vị trí mới của nó trong cảnh sau, bản ống kính của tư duy FLIP | Phần tử trong 2 cảnh bắt buộc cùng thân phận, biến dạng quá lớn sẽ đứt đoạn nhận thức "Cùng một thứ" |
| Xuyên khoảng trống chữ | Ống kính đẩy tới xuyên qua khoảng trống của chữ lớn (Lỗ hổng của O/口/0) đi vào cảnh tiếp theo | Kích thước khoảng trống chữ phải đủ (≥1/3 chiều cao màn hình), đoạn xuyên qua zoom đi theo mức trần thời lượng logarit 0.94s |

Tra nhanh chọn kiểu: Độ lệch năng lượng lớn → Chọn 1 trong 6 kiểu; Không muốn bị nhận ra → hidden-cut; 2 cảnh liên tục về không gian → travel.
Cắt cứng không phải bị cấm, mà là "Bắt buộc được đóng gói bởi bất kỳ tầng nào ở trên".

---

## §8 · Công thức Parallax Nhiều tầng · Dolly-zoom Giả

### 8.1 Hệ số tốc độ tầng parallax (shotcraft)

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Hệ số tốc độ tầng | Cảnh xa 0.35 / Cảnh vừa 0.7 / Cảnh gần 1.4 (Bội số dịch chuyển máy ảnh) | Tỷ lệ tốc độ tầng liền kề **≥2 lần** mới phân biệt được, chênh lệch 1.2 lần khán giả không đọc ra |
| Số tầng | ≤4 tầng | Vượt quá 4 tầng thấu thị không ai nhìn ra được, thuần túy lãng phí ngân sách |
| Hiện thực hóa | Các tầng dựa theo cùng một x/y máy ảnh nhân với hệ số riêng để translate | Toàn bộ do trạng thái máy ảnh suy ra, seek-safe; Đừng cho mỗi tầng tween độc lập |

### 8.2 Dolly-zoom Giả (Thuần CSS, không cần 3D thật)

```
Chủ thể: Đóng đinh không động (Hoặc chỉ làm nhịp thở ≤1.02x)
Nền: scale 1 → 2.0-2.5, đồng thời opacity giảm xuống ≤0.6
```

Chủ thể không đổi, nền tràn về phía khán giả, tạo ra cảm giác đảo ngược "Thế giới đang tiến lại gần mà nhân vật chính đóng đóng băng" (shotcraft).
Thời lượng cho đủ (≥1.2s), mục đích xem §1: Toàn phim tối đa 1 lần, dành cho khoảnh khắc hé lộ thực sự.

---

## §9 · Tín hiệu Phái sinh Chuyển động · blur / Đi theo / Dư chấn

"Cảm giác tốc độ" của ống kính không dựa vào duration nhanh hơn, mà dựa vào tín hiệu cấp hai **phái sinh** từ tốc độ (shotcraft / HuaRec):

| Tín hiệu | Công thức | Tham số |
|---|---|---|
| Blur dẫn dắt bởi tốc độ | `v = velocityAt(f)` (Sai phân trung tâm: `(pos(f+1)−pos(f−1))/2`), độ mạnh blur ∝ v | Tốc độ zoom >0.6/s mới kích hoạt, độ mạnh `min(10, v×5)`px. **Chỉ trong khoảnh khắc động mới có mờ, khung hình tĩnh luôn sắc nét** (HuaRec) |
| Tầng đi theo lagged | Tầng đi theo = Chủ thể lấy mẫu tại `f − delay` | Bóng trễ 2f, Dư ảnh trễ 4f (shotcraft). Dư ảnh thay thế bằng phẳng cho motion blur: 5% Trễ đường đi + blur(6px) + opacity 0.25·(1−t) |
| Dư chấn dampedSettle | `e^(−d·t) · sin(2π·f·t)`, f≈0.1, damping≈0.15 | 1-2 lắc nhẹ còn lại sau khi ống kính dừng gấp, biên độ ≤3px; Quay phim chủ động (Họ inOut §4.1) không thêm, chỉ cho vung ống kính/phanh gấp |

Cả 3 đều là hàm thuần của thời gian (Sai phân, Lấy mẫu trễ, Suy giảm dạng đóng), tự nhiên seek-safe,
phù hợp với yêu cầu tính xác định của gsap-recipes §6.

---

## §10 · Danh sách Tự kiểm tra Quay phim (60 giây sau khi viết xong timeline)

- [ ] Mỗi hành động của ống kính đều trả lời được "Thay mặt khán giả trả lời câu hỏi gì"? Không trả lời được đã xóa chưa?
- [ ] Không có zoom <1.25x (Trừ đẩy nhẹ định cảnh 1.06x)?
- [ ] Khoảng cách giữa các ống kính liền kề ≥2.6s, cửa sổ 15s ≤4-5 lần?
- [ ] Thời lượng tất cả đẩy kéo đi theo công thức logarit, không có duration cố định tự nghĩ ra?
- [ ] Easing đẩy kéo là power3.inOut, không có linear / lò xo quá chớn?
- [ ] Các ống kính liền kề đã qua phán quyết chia 3 theo khoảng cách tiêu điểm (Gộp cảnh/Bình移/Bỏ cảnh)? Không có 2 lần liên tiếp nhảy ngang tiêu điểm xa?
- [ ] Các ống kính khoảng trống ngắn nối trực tiếp, không có bơm đẩy quay về 1x?
- [ ] Phim dài >14s và cảnh đầu muộn hơn 7s: Đã thêm đẩy nhẹ định cảnh 1.06x?
- [ ] Kết thúc là tĩnh toàn cảnh ≥0.8s, không kết thúc ở trạng thái đẩy lại gần?
- [ ] Có thuyết minh: Di chuyển ống kính hút vào khoảng trống giọng nói (Chỉ tiến trước ≤0.8s)?
- [ ] Chuyển động ống kính toàn bộ thu về tầng `#camera`, không tranh giành transform với hoạt ảnh phần tử?
- [ ] Đặc tả chữ 3D đi theo CSS zoom rasterize, không có scale phóng đại bị mờ?
- [ ] Mỗi đường nối chuyển cảnh chỉ dùng 1 kiểu, toàn phim không có cắt trần?
- [ ] Tỷ lệ tốc độ tầng liền kề parallax ≥2 lần, ≤4 tầng?
- [ ] blur chỉ xuất hiện ở trạng thái tức thời chuyển động, khung hình tĩnh toàn bộ sắc nét?

---

## §11 · Mối quan hệ với các reference khác

| reference | Phân công | Ranh giới |
|---|---|---|
| `animation-best-practices.md` | Phần tử động thế nào, Nhịp điệu tự sự, Tiêu chuẩn thẩm mỹ | Nó quản "Diễn viên", tệp này quản "Máy ảnh"; "Kéo xa ống kính" ở đoạn S4 bùng nổ định tham số theo §4 tệp này |
| `gsap-recipes.md` | Hiện thực hóa có thể chạy được bằng GSAP cho tất cả quy tắc tệp này | Mục "Công thức Camera Rig": Rig container, Dịch PageCam, Helper thời lượng logarit, Counter-transform |
| `animation-pitfalls.md` | Danh sách bẫy | Bẫy riêng của ống kính (scale bị mờ / perspective bị ngắt / pan lộ mép) §3.3-3.4 đã bao phủ phía thiết kế, pitfalls thu thập tái hiện phía kỹ thuật |
| `hyperframes-backend.md` | Hợp đồng backend render | Tính áp dụng của kỹ thuật CSS zoom dưới dạng render seek từng khung hình offline xem §3.4 |
| `voiceover-pipeline.md` | Video dài do thuyết minh dẫn dắt | Dữ liệu điểm im lặng của move on pause (§6.3) đến từ khoảng trống phân câu của narration |
| `ai-video-review.md` | Đánh giá thành phẩm | Phân loại chuyển cảnh của checklist đánh giá mở rộng theo từ vựng 3 tầng §7 |

**Thứ tự gọi**: Giai đoạn nhật ký đạo diễn / Phân cảnh đọc §0-§2 định ngân sách và từ vựng → Trước khi viết timeline đọc §3-§7 định quy ước hiện thực hóa
và đường nối → Trước khi giao hàng duyệt danh sách §10.
