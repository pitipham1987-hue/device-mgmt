# Playbook Animation Hiển thị UI Sản phẩm

> **Đây là lối vào duy nhất khi "sản phẩm quảng cáo có giao diện UI".** Dù là video thương mại, animation ra mắt sản phẩm, hay demo tính năng, chỉ cần nhân vật chính trên màn hình là một giao diện, hãy đọc tài liệu này trước khi bắt tay vào làm.
>
> Tuyên ngôn cốt lõi trong một câu: **Nguồn gốc chất lượng lớn nhất của animation sản phẩm là "UI thực tế + Vận kính điện ảnh", không phải hiệu ứng kỹ xảo (VFX).** Cảm giác công nghệ đến từ việc giúp khán giả nhận ra "đây chính là sản phẩm đó", dựa vào cách ống kính quan sát nó, chứ không dựa vào hạt (particles), phát sáng (glow), hay dải màu cyber. Một ảnh chụp màn hình thực tế kết hợp với một cú đẩy ống kính (zoom in) kiềm chế tốt hơn mười lớp giao diện giả tự vẽ tay.
>
> Quy ước trích dẫn tham số: (shotcraft·tên thẻ) = giá trị thực tế từ thẻ ống kính video-shotcraft; (huarec) = hệ thống đạo diễn vận kính 花录 Studio; (best-practices §x) (gsap-recipes §x) = reference có sẵn trong skill này. Trong ngữ cảnh 30fps, 1f ≈ 33ms.
>
> Phân công: Tài liệu này quản lý "nhân vật chính UI diễn xuất như thế nào"; từ vựng ống kính và động cơ vận kính xem tại `camera-language.md`; cú pháp chuyển động cấp phần tử (easing, stagger, FLIP, Chunk Reveal) xem tại `animation-best-practices.md`, bài viết này chỉ dẫn chiếu chứ không viết lại.

---

## §0 Hai Công lý (Vượt lên trên mọi tham số của Bát thức)

| Công lý | Nội dung | Nguồn |
|---|---|---|
| **Bất biến thể tính khả kiến** | Tại bất kỳ thời điểm nào, con trỏ và thao tác UI đang diễn ra phải nằm trong vùng hiển thị (bao gồm 8% lề an toàn). Ống kính vi phạm thà giảm tỷ lệ phóng đại, gộp ống kính hoặc không quay | huarec A1 |
| **Ngôn ngữ thị giác phát triển từ sản phẩm** | Trích xuất design tokens của chính sản phẩm trước (font chữ/bo góc/bảng màu/grid), toàn bộ video chỉ cho phép tái sử dụng hoặc mở rộng kiềm chế. Thẻ ống kính chỉ thừa kế cú pháp chuyển động và nhịp điệu, giao diện được khoác lại theo sản phẩm mục tiêu | shotcraft Công lý 1 |

---

## §1 Cây quyết định: Vận kính trên ảnh chụp UI thực tế vs Tái dựng UI bằng HTML

Đây là **quyết định ảnh hưởng lớn nhất đến khối lượng công việc** trong tài liệu này, hai đường đi chênh lệch nhau cả một cấp độ. Mặc định bắt đầu đánh giá từ con đường tiết kiệm nhất:

```
UI sản phẩm cần xuất hiện
 │
 ├─ Giao diện chỉ cần "được nhìn" (đẩy gần/tuần tra/nổi/so sánh)?
 │   └─ Có → 【Con đường 1】Ảnh chụp đưa vào khung device + Vận kính 2.5D. Dừng ở đây, đừng tái dựng
 │
 ├─ Chỉ có một vài phần tử cần chuyển động riêng biệt (một card nổi lên, một dòng dữ liệu cuộn)?
 │   └─ Có → 【Con đường 3】Hỗn hợp: Ảnh chụp làm nền + Cắt lớp phần tử khóa để tái dựng
 │
 └─ Bản thân giao diện là chủ thể tự sự, các phần tử cần xuất hiện/phản hồi thao tác/đổi trạng thái từng cái một?
     └─ Có → 【Con đường 2】Tái dựng HTML. Đi theo Bát thức ② build-up
```

| Con đường | Cách làm | Khối lượng công việc | Tiêu chí áp dụng |
|---|---|---|---|
| 1 · Vận kính ảnh chụp | Đặt ảnh chụp thực tế vào khung thiết bị `browser_window.jsx` / `macos_window.jsx`, thực hiện zoom/rotate/pan trên container | Cấp giờ | Giao diện là "đối tượng được quan sát"; khán giả không cần thấy sự thay đổi bên trong giao diện |
| 2 · Tái dựng HTML | Tái dựng cấu trúc DOM có thể chuyển động theo từng pixel của ảnh chụp | Cấp ngày | Các phần tử cần timeline độc lập: xuất hiện từng bước, typing, chuyển trạng thái, phản hồi hover |
| 3 · Hỗn hợp | Ảnh chụp toàn trang làm texture lớp đáy, các phần tử cần chuyển động được cắt thành lớp nền trong suốt đè lên tọa độ gốc để chuyển động | Cấp nửa ngày | 90% màn hình tĩnh, 10% phần tử cần sống động. **Đáp án đúng cho đa số hợp đồng thương mại** |

**Điểm then chốt của chiến lược hỗn hợp**: Phần tử cắt lớp sau khi chuyển động xong bắt buộc phải trở về vị trí slot thực tế trên ảnh chụp. Nếu thẻ mục tiêu nổi trên lưới mà không hạ cánh về lại layout, khán giả sẽ lập tức nhận ra "giả" (shotcraft·type-and-filter, án lệ Q9 từng vì lý do này mà suýt phải viết lại gần như toàn bộ file).

Năm bước thao tác của con đường hỗn hợp:

1. Trải ảnh chụp toàn trang 2x làm nền, đưa vào khung thiết bị (sản phẩm web dùng `browser_window.jsx`, ứng dụng desktop dùng `macos_window.jsx`)
2. Cắt các phần tử cần chuyển động thành lớp nền trong suốt theo tọa độ layout.json
3. Lớp đáy ảnh chụp tại vị trí gốc của mảnh cắt được phủ một "miếng vá màu nền trang" để che đi phần tử gốc đã nướng chìm (kỹ thuật miếng vá vị trí gốc của spotlight-hero-card: sau khi card cất cánh, phủ miếng vá màu nền vị trí gốc + viền nhịp thở màu nhấn, biến mất tăng sáng ngay khoảnh khắc hạ cánh)
4. Mảnh cắt đè lên miếng vá để làm animation, điểm kết thúc trở về slot layout.json
5. Phân đoạn đẩy đặc tả dùng mảnh cắt HD 4x, đè lên texture độ phân giải thấp bằng crossfade 6f (kỹ thuật đi kèm shotcraft·PageCam)

**Lựa chọn khung thiết bị**: Khung là tín hiệu ngữ cảnh chứng minh "đây là phần mềm thật", ảnh chụp trần lơ lửng trên canvas trông giống như hình dán. Nhưng khung cũng nuốt mất diện tích màn hình, khi đặc tả đẩy đến zoom 2x trở lên thì khung đã ra khỏi màn hình, lúc này có thể dùng trực tiếp mảnh cắt không khung.

### Bộ ba vật liệu (Thu thập đầy đủ trước khi đi theo Con đường 1/3)

Vật liệu thu thập tiêu chuẩn trong giai đoạn 1 của shotcraft pipeline (shotcraft·pipeline 6 giai đoạn):

| Vật liệu | Quy cách | Mục đích |
|---|---|---|
| Ảnh chụp toàn trang 2x | Tỷ lệ pixel thiết bị từ 2 trở lên, chụp toàn bộ trang dài | Texture lớp đáy; ngưỡng tối thiểu để không bị mờ khi đẩy gần |
| Mảnh cắt phần tử nền trong suốt | Cắt riêng từng phần tử cần chuyển động độc lập, 4x càng tốt | "Diễn viên" của con đường hỗn hợp; đè lên texture độ phân giải thấp bằng crossfade 6f trong giai đoạn đẩy đặc tả |
| Bảng tọa độ layout.json | `{x,y,w,h}` của từng mảnh cắt trong hệ tọa độ toàn trang | Căn cứ "slot thực tế" để trở về vị trí sau khi chuyển động; neo vị trí cho hộp ghi chú/highlight |

Nguồn ảnh chụp tuân theo giao thức thu thập ảnh chụp UI trong `brand-asset-protocol.md` (ảnh chụp App Store, screenshots trang web chính thức, cắt khung hình video demo, ảnh chụp thực tế tài khoản người dùng), ngưỡng chất lượng cũng áp dụng quy tắc "5-10-2-8". Phương pháp khắc phục triệt để chữ bị mờ khi đẩy đặc tả gần (dùng CSS `zoom` thu phóng cấp layout thay cho transform scale) xem tại `camera-language.md`.

---

## §2 Bát thức Hiển thị UI · Tổng quan

| # | Thức | Một câu tóm tắt | Con đường | Thẻ nguồn |
|---|---|---|---|---|
| ① | Sàn hiển thị 3D / hero đặc tả | Đưa một card thành nhân vật chính toàn bộ video: spotlight→đẩy gần→lơ lửng→trở về vị trí | 1/3 | spotlight-hero-card |
| ② | Giao diện xuất hiện từng bước build-up | Giao diện từ không đến có: khung xương→nội dung→dữ liệu, xuất hiện là tự sự | 2 | skeleton-reveal / row-embed / document-typewriter-reveal |
| ③ | Mô phỏng typing của người dùng | Tốc độ gõ tay người thật, con trỏ từ sáng liên tục chuyển sang nhấp nháy | 2/3 | type-and-filter |
| ④ | Tự sự thao tác con trỏ | Con trỏ đóng vai diễn viên: di chuyển đường cong, click ripple, tương tác hover | Tất cả | type-and-filter / collab-cursor-moves |
| ⑤ | Timeline hóa chuyển trạng thái UI | Chuyển đổi tab/modal/route được viết thành một phân đoạn kịch trên timeline | 2 | command-palette-summon |
| ⑥ | Tuần tra 3D giao diện | Giao diện dài đặt nghiêng lướt qua, hoặc ống kính áp sát tham quan | 1 | steep-tilt-glide / graze-face-tour |
| ⑦ | Tự sự cuộn trang dài | Trang dài cuộn nhanh phanh gấp, dừng lại tại dòng mục tiêu | 1/3 | scroll-brake-moves |
| ⑧ | Chú thích feature callout | Đường chú thích phát triển, hộp highlight, thẻ giải thích, so sánh before/after | Tất cả | before-after-slider-scrub v.v. |

Kỷ luật lựa chọn: **Một ống kính chỉ kể một thức, một thức toàn bộ video chỉ làm nhân vật chính một lần** (shotcraft Công lý 5). Bát thức có thể xuất hiện nhiều thức trong cùng một video, nhưng mỗi thức chiếm một ống kính riêng.

---

## §3 Chi tiết Bát thức

### ① Sàn hiển thị 3D / hero đặc tả

Dựng một đối tượng cốt lõi (card/panel/module) thành đơn vị nguyên tử của sản phẩm. Đây là cảnh có chất lượng cao nhất, nhịp điệu chậm nhất, thích hợp đặt ở đoạn "dựng tiểu sử đơn nhân vật" sau mở màn.

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Vị trí máy ảnh | rotY 34° chủ đạo + rotX chỉ 8°, perspective 1200px | Góc quay nghiêng tốt hơn góc quay từ trên xuống, "quay từ bên trái thay vì từ bên dưới"; rotX chỉ cần lớn một chút là thành nhìn mặt bàn |
| Đẩy ống kính | Zoom 0.78 toàn trang đứng yên 1 beat, sau đó 16f đẩy đến zoom 2.6 | Đứng yên trước khi đẩy là "để khán giả thấy toàn cục trước"; mở màn đẩy ngay sẽ mất cảm giác không gian |
| Cung chuyển động | rise 10f (`cubic-bezier(0.2,1.25,0.3,1)` overshoot) → lơ lửng 54f (sin bob biên độ 4px chu kỳ 40f, translateZ 110px) → reseat 18f hạ cánh press scale 0.997 | Khóa cho đến khi hạ cánh khoảng 3.3s, cảnh quay chất lượng phải chậm đến mức này; bản đầu tiên hầu như luôn bị nhanh |
| Vệt sáng đường viền | SVG rounded-rect viền chạy hai vòng: vòng 1 14f nhanh và sáng, vòng 2 20f chậm và yếu (opacity 0.62) | Hai vòng nhanh chậm khác nhau mới đọc ra là "quét liên tục", một vòng chỉ là chớp mắt; vệt sáng toàn bộ video chỉ trao cho nhân vật chính một lần |
| Bóng hai lớp | `0 8·lift px …, 0 46·lift px 90·lift px` phát triển theo độ cao lơ lửng | Bóng không phát triển theo độ cao thì sự lơ lửng không thành lập |
| Dẫn hướng đèn spotlight | Ánh sáng di chuyển qua 4 điểm trung gian để khóa tâm card, bán kính vùng sáng thu hẹp 620→420→360, khoảnh khắc khóa +6% xung lực; vignette bên ngoài 0.16→0.42 làm tối | Các điểm trung gian giúp "chiếu ngẫu nhiên" đáng tin, lao thẳng tới mục tiêu sẽ đọc ra là lập trình; vignette là một nửa còn lại của spotlight |
| Chú thích lơ lửng (tùy chọn) | Cạnh card hiện ra hai dòng chú thích serif, translateZ 92px + bob chu kỳ 44f (gần giống nhưng không đồng bộ với 40f của card) | "Đồng cảm" chứ không phải đồng bộ phản chiếu; chú thích bắt buộc phải sống trong cùng không gian 3D và cùng phối cảnh máy ảnh, chữ đè phẳng sẽ phá hỏng sự thống nhất không gian |

(Tất cả giá trị thực tế ở trên từ shotcraft·spotlight-hero-card)

Bẫy đã biết: Kết bài cấm trôi đuôi kiểu "zoom 2.6→2.58", nhịp thở phải là đứng yên thực sự; mở màn nhiều card nhảy múa không gánh nổi ấn tượng đầu tiên, phác thảo trực tiếp từ đơn nhân vật + cung chuyển động hoàn chỉnh; glint từng card bị án lệ phủ quyết hai lần, hiệu ứng ánh sáng nghiêm ngặt chỉ trao cho nhân vật chính.

Vận kính phối hợp: dolly-in đẩy gần + đứng yên hold sau khi khóa (xem `camera-language.md`). Khi biên độ tỷ lệ lớn hơn 2x, nhấn giữ co giãn lôgarit, cấm overshoot lò xo (huarec).

### ② Giao diện xuất hiện từng bước build-up

Tự sự xuất hiện "từ không đến có" của giao diện. **Thứ tự build có cú pháp**: chrome (khung cửa sổ/thanh tiêu đề) → khung xương (vị trí giữ chỗ bằng thanh xám) → stagger khối nội dung → dữ liệu (con số/biểu đồ sống động sau cùng). Khán giả có kỳ vọng miễn phí với skeleton screen, thanh xám vừa xuất hiện là biết nội dung sắp tới.

Ba thẻ nguồn phân công theo đối tượng: **một giao diện** biến thành thật từng cấp dùng skeleton-reveal; **một nhóm dòng/card** nhúng vào danh sách dùng row-embed; **một tài liệu** được viết ra dùng document-typewriter-reveal.

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Hiện hình ba cấp | Nét vẽ nguệch ngoạc (mỗi 5f đổi seed "sôi dạt") → đổi sang thật 1 beat 8f tăng tốc thu nhỏ + khung xương spring nảy vào → các dòng khung xương lệch pha 6f cuộn vào → các dòng hiện hình lệch pha 13f, trong dòng 12f | Đổi sang thật 1 beat phải nhanh và dứt khoát, kéo dài thành crossfade "nhảy vọt" sẽ mất cảm giác; bố cục ba cấp phải đồng cấu nghiêm ngặt, lệch vị trí sẽ đọc ra là đổi trang |
| Tiến vào từng từ | 2.5f/từ nổi lên 14px; từ cuối dòng cuối +14f trễ nửa nhịp | Trễ nửa nhịp là dấu chấm câu của "tải hoàn tất"; tất cả rơi cùng lúc sẽ phẳng lặng |
| Nhúng dòng | Dòng thứ i cue = 12 + i·9, bay 12f; `perspective(900px) translateY(−120·air) rotateX(16°·air)` | **rotateX thu phẳng là cảm giác đọc then chốt của "nhúng"**, translateY thuần túy chỉ là "rơi xuống" |
| Khe nhấn mạnh nhúng | Khe màu nhấn 2px ở mép dưới xòe ra từ trung tâm 5f, mờ dần 8f | Cho mỗi lần nhúng một điểm xác nhận, nhưng phải mờ dần nhanh |
| Tài liệu từng khối | Khối thứ g cue = 6 + g·3.5, mỗi khối wipe 8f; caret màu nhấn chỉ đi theo khối mới nhất | **"Số khối × nhịp điệu căn chỉnh trước ngân sách" là phép toán cốt lõi**, khối quá nhiều thì cắt khối trước chứ không tăng tốc; "luôn chỉ có một đầu bút", hai caret cùng nhấp nháy là hai tác giả |

(Giá trị thực tế từ 3 thẻ skeleton-reveal / row-embed / document-typewriter-reveal)

Bẫy đã biết: Khi lớp nội dung thật dán ảnh chụp sản phẩm, chiều cao dòng thanh xám khung xương và slot phải đo theo ảnh chụp, đừng xếp theo tưởng tượng; cấp độ nét vẽ nguệch ngoạc không vẽ chi tiết, quá giống UI thì cấp 1 và cấp 2 không khác gì nhau; văn bản mock không xuất hiện tên thật của khách hàng/thành viên.

Vận kính phối hợp: Phân đoạn hiện hình đi kèm với đẩy chậm 1→1.34 (tạo động cơ thị giác "ghé sát nhìn rõ"); toàn bộ quá trình build không thêm chuyển động ngang của ống kính, khi giao diện đang thay đổi thì ống kính phải vững (huarec: thay đổi cấp toàn màn hình thì không đẩy).

### ③ Mô phỏng typing của người dùng

Mô phỏng người thật gõ chữ trong ô nhập liệu/terminal. **Khác hoàn toàn với Chunk Reveal (đầu ra dạng stream của AI) đã có**: Đầu ra AI là các chunk xuất hiện không đều (best-practices §4.5, gsap-recipes §3.4, bộ đó tiếp tục dùng, không viết lại ở đây); người dùng nhập liệu là nhịp điệu tay người gõ từng ký tự, đều đặn, có sự ngập ngừng. Lầm tưởng ngữ cảnh là sự cố tần suất cao: làm nhập liệu của người dùng thành chunk sẽ đọc ra là "ô nhập liệu tự mình tạo ra".

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Tốc độ gõ | Văn bản chính 3f/ký tự; terminal 2f/ký tự; chữ nhỏ trang trí 0.7f/ký tự | "Giá trị chốt sau khi bản đầu bị chê nhanh phải làm lại"; demo tương tác theo tốc độ thao tác người thật, đây là quy tắc sắt cấp án lệ |
| Trạng thái con trỏ | Khi gõ **sáng liên tục**, gõ xong chuyển sang nhấp nháy chu kỳ 8f | Nhấp nháy trong lúc gõ sẽ đọc ra là giật lag; chuyển từ sáng liên tục→nhấp nháy chính là tín hiệu "đã gõ xong" |
| Sửa lỗi backspace | Thi thoảng xảy ra một lần: gõ thừa 1-2 ký tự, dừng 4-6f, backspace, gõ lại đúng | "Người sẽ gõ sai" là cảm giác chân thực rẻ nhất; nhưng bắt buộc phải viết trước vào kịch bản (tính xác định khung hình), không phải random lúc runtime |
| Tạm dừng xác nhận | Gõ xong đến khi trang phản hồi chừa 11f (0.37s) nhịp thở | Gõ xong phản hồi ngay lập tức sẽ đọc ra là máy tự động, khán giả không theo kịp nhân quả |
| Gõ code | Terminal 2f/ký tự; tô màu cú pháp tô dần theo lượt gõ (token hiện tại gõ xong là tô màu ngay), không phải gõ xong toàn đoạn mới đổi màu thống nhất | Đổi màu thống nhất là "paste" không phải "viết code"; độ trễ tô màu trong vòng nửa token khán giả không cảm nhận thấy |
| Gõ trên ảnh chụp | Miếng vá màu nền che đi placeholder đã nướng chìm trong ảnh chụp (giữ lại icon), lớp chữ đè lên trên để gõ | Đè chữ trực tiếp sẽ bị bóng ma trùng với placeholder đã nướng chìm |

(Giá trị thực tế từ shotcraft·type-and-filter)

Cách viết xác định cho sửa lỗi backspace (viết kịch bản trước, không rút thăm lúc runtime):

```js
// Biên dịch "gõ sai→dừng→lùi→sửa" thành bảng sự kiện ký tự, timeline chỉ playback
const script = typeScript("nano-lab", {
  perChar: 3 / 30,                       // 3f/ký tự
  typo: { at: 5, wrong: "0", pauseF: 5 } // Tại ký tự thứ 5 gõ sai một chữ "0", dừng 5f rồi lùi
});
// script = [{t:0, text:"n"}, {t:0.1, text:"na"}, ... {t, text:"nano-0"},
//           {t+0.17, text:"nano-"}, {t+0.27, text:"nano-l"}, ...]
// Lớp render tra bảng theo t để lấy text, an toàn cho seek hai chiều
```

Vận kính phối hợp: Trước khi gõ chữ, máy ảnh dịch chuyển lên trên/đẩy gần vào ô nhập liệu trước (cho ống kính trước rồi mới ra tay, công lý tính khả kiến); trong lúc gõ chữ ống kính khóa chặt.

### ④ Tự sự thao tác con trỏ

Con trỏ là "con người" duy nhất trong demo UI. Component dùng `assets/cursor.jsx` (CursorSprite / ClickRipple / hook tương tác hover, hai loại clock drive, API xem tại ghi chú đầu file đó).

Thuật toán quỹ đạo không viết lại ở đây: Đường cong Bézier + tay run hội tụ xem best-practices §3.5, cách viết GSAP proxy xem gsap-recipes §3.5. Thức này bổ sung **các tham số click và tương tác**:

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Ripple vòng đôi | Hai vòng đồng tâm, **điểm xuất phát lệch 3f**, bán kính 14→54 / 14→78px | Vòng đơn quá nhẹ không thấy được; lệch 3f là "gợn sóng của một lần click", lệch nhiều quá trông như click hai lần |
| Giải nén khuếch tán/tan biến | Khuếch tán out-cubic 22f, tan biến tuyến tính 26f | Khuếch tán phải có lực, tan biến phải đều; cùng một đường cong quản lý hai việc sẽ bị "chớp một cái rồi mất". Scenarios thu gọn có thể nén xuống 10f mỗi cái (bản nén type-and-filter dùng) |
| Chuẩn bị click | Con trỏ scale 0.85 (power1.in khoảng 3f) → nảy về back.out | Anticipation giúp cú click có trọng lượng "ấn xuống" (cùng loại gsap-recipes §3.5) |
| Tương tác hover | Con trỏ vào vùng mục tiêu, mục tiêu sáng lên cùng khung hình (brightness +6% hoặc hiện viền hairline), con trỏ rời đi lập tức rút về | Mục tiêu không phản hồi thì con trỏ chỉ là hình dán; cửa sổ tương tác khai báo theo tiến độ quỹ đạo, không làm hit-testing lúc runtime |
| Phân công vai trò con trỏ | Con trỏ thao tác (click có payload) vs Con trỏ diễn xuất (dịch chuyển là kịch bản, vũ điệu đôi/quần diễn của con trỏ cộng tác có tên) | Dùng thức này khi con trỏ cần click đồ; con trỏ cộng tác thuần tự sự là một cảnh kịch khác (shotcraft·collab-cursor-moves), có thể tồn tại song song trong cùng video nhưng đừng trộn lẫn |

Tra nhanh hai thức con trỏ diễn xuất (dùng khi có chủ đề cộng tác/nhiều người, tham số từ collab-cursor-moves):

| Thức | Cơ chế | Tham số cốt lõi |
|---|---|---|
| dialogue-duet Vũ điệu đôi | Hai con trỏ có tên màu xanh dương/xanh lá tiến lại gần đối thoại, tách thành cung trên/dưới xoay quanh đổi vị trí (R≈270px), bảng tên một sáng một tối bàn giao ánh sáng, con trỏ xanh lá easeIn phóng to hàng chục lần làm che chuyển cảnh | Tất cả dịch chuyển Bézier bậc ba, không linear; hai con trỏ cùng cung cùng hướng sẽ có cảm giác va chạm, tách cung trên dưới là "nhường đường" |
| cast-ensemble Quần diễn | 5 con trỏ màu delay 0/5/9/13/17f lệch pha spring bay vào, trôi trượt sóng sin hai tần số duy trì (0.055/0.021 rad/f, biên độ ±46/30px), một con trỏ gõ chữ cameo | Bảng tên mờ dần vào trễ 12f mới là "người đến tự báo danh xưng"; sau khi tụ lại thì độ trôi suy giảm giữ ở 25%, nhóm con trỏ đứng yên hoàn toàn sẽ đọc ra là treo máy |

Bẫy đã biết: Toàn bộ quá trình con trỏ di chuyển phải tuân thủ công lý tính khả kiến, mục tiêu ở ngoài màn hình thì di chuyển ống kính trước rồi mới di chuyển con trỏ; màu bảng tên của con trỏ cộng tác = mã hóa danh tính, thống nhất toàn bộ video, giữa chừng đổi màu khán giả tưởng đổi người; con trỏ trôi trượt không bao giờ được che nội dung chính đang đọc.

Vận kính phối hợp: Sau khi click xác nhận, máy ảnh 16f đẩy gần (zoom≈2.2) xuyên qua vào trang chi tiết, đây là cú chuyền gậy tiêu chuẩn của "click→tiến vào" (type-and-filter); cách nối chuyển cảnh xem `camera-language.md`.

### ⑤ Timeline hóa chuyển trạng thái UI

Chuyển đổi tab, modal bật lên, đẩy route trang. **Replay được điều khiển bởi timeline và prototype tương tác là hai dạng code khác nhau**, khi sửa từ prototype sang bản render hãy dịch từng mục:

| | Prototype tương tác | Timeline replay (Dùng để render) |
|---|---|---|
| Kích hoạt | `addEventListener('click')` | Tham số label / position trên timeline |
| Chuyển trạng thái | `classList.add` + CSS transition | Tween rõ ràng (Quy tắc vùng cấm gsap-recipes §6.1) |
| Mở/Đóng | Chuyển đổi `display: none` | Tween `autoAlpha` |
| Hover | Giả lớp `:hover` | Cửa sổ tương tác khai báo theo tiến độ quỹ đạo (Bát thức ④) |
| Ngẫu nhiên | `Math.random()` | Tạo trước bằng seed mulberry32 (gsap-recipes §6.4) |

Sau khi sửa kịch bản `grep "addEventListener\|classList\|transition:"` xóa sạch từng dòng. Trạng thái bắt buộc phải là hàm thuần túy của thời gian, tất cả các bug nhìn bình thường trong preview nhưng render mới lộ hàng đều xuất phát từ đây.

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| modal / Bảng lệnh | Nền 10f làm tối xuống rgba(20,20,20,0.45) + blur 10px; panel −20px→overshoot +8px (9f)→rơi về (6f); các dòng ứng viên lệch pha i·4f | Nền không làm tối thì panel không có cảm giác "nổi lên trên"; lượng overshoot 8px là "hạ cánh nhẹ", lớn hơn nữa sẽ thành đồ chơi |
| Chuyển tab | Thanh chỉ thị dùng FLIP trượt (best-practices §4.1), nội dung cũ 5f mờ dần chìm xuống 8px, nội dung mới 8f mờ dần nổi lên | Thanh chỉ thị và nội dung không đồng bộ là nguồn gốc cảm giác rẻ tiền: thanh đi trước, nội dung theo sau nửa nhịp |
| Đẩy route | Trang mới từ bên phải đẩy vào toàn trang 12-16f, trang cũ lùi cùng hướng khoảng cách 30% + làm tối | Trang cũ lùi khoảng cách nhỏ (thị sai) cho cảm giác sâu hơn đẩy bằng khoảng cách; đẩy bằng khoảng cách là "băng chuyền" |

(Tham số modal là giá trị thực tế từ shotcraft·command-palette-summon)

Bẫy đã biết: Chuyển trạng thái là khu vực thiên tài của "một ống kính một hiệu ứng", trong một lần chuyển cảnh mà tab vừa chuyển, toast vừa bật, dữ liệu vừa cuộn thì khán giả chẳng nhìn rõ được cái gì; một ống kính chỉ diễn một lần thay đổi trạng thái.

Vận kính phối hợp: Tại khoảnh khắc chuyển trạng thái ống kính bắt buộc phải đứng yên, chuyển xong mới di chuyển (Cú pháp giữa các ống kính huarec: sự thay đổi là mức tiêu thụ sự chú ý, đừng tiêu thụ đè lên chuyển động ống kính).

### ⑥ Tuần tra 3D giao diện

Cảnh quay thể hiện "cảm giác không gian" của một giao diện dài/nhiều màn hình. Phân công hai thẻ rõ ràng, chọn sai thẻ là sự cố chính của thức này:

| Thẻ | Cơ chế | Áp dụng |
|---|---|---|
| steep-tilt-glide | **Ống kính tĩnh, trang chuyển động**: Trang đặt nghiêng rotateY −60°, tự mình trượt đều qua màn hình | Trang làm "vật trưng bày" diễu hành qua; nội dung không cần đọc rõ, cái nhìn là thể lượng và chất lượng |
| graze-face-tour | **Ống kính chuyển động, trang tĩnh**: Nhóm trang lơ lửng cố định, ống kính áp sát tham quan | Cần nhìn rõ nội dung cục bộ trong lúc tuần tra; ống kính có nhân cách "người tham quan" |

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Góc đặt nghiêng | rotateY −60°, vùng phán quyết thực tế 55-65° | −45° bị chê "chưa đủ nghiêng", >−70° nội dung đọc không rõ |
| Độ cao lơ lửng | 120-180px (graze-face-tour) | Thấp quá dán mặt đất không có cảm giác "trưng bày", cao quá bóng bị mất kết nối |
| Lệch pha nhóm | Lệch điểm xuất phát nhưng rơi xuống chồng lấp song song | Rơi hoàn toàn theo thứ tự là "xếp hàng điểm danh", chồng lấp mới là "một nhóm tới hàng" |
| Nối giữa các đoạn | Crossfade ≥8f | Cắt cứng sẽ bị chớp đen; tuần tra là không gian liên tục, cấm jump cut |

(Giá trị thực tế từ shotcraft·steep-tilt-glide / graze-face-tour)

Biến thể thứ ba của trưng bày nhiều trang cùng màn hình (shotcraft·page-waterfall-wall): Tường thác nước trang 3 cột, rotateX 20° + perspective 1000px, chênh lệch chu kỳ loop cột相邻 ≥25% (như 12/9/14s) và cột giữa đảo ngược. Thích hợp cho đoạn kết bài trình bày thể lượng "sản phẩm có rất nhiều trang", nội dung không cần đọc rõ. Khi chênh lệch chu kỳ nhỏ hơn 25%, ba cột sẽ căn chỉnh định kỳ, "bức tường" lập tức biến thành "bảng biểu".

Bẫy đã biết: Cơ chế hai thẻ không được trộn lẫn, trang và ống kính di chuyển cùng lúc khán giả sẽ mất hệ quy chiếu (chóng mặt); texture trang dùng cho tuần tra bắt buộc từ 2x trở lên, phóng to sau khi đặt nghiêng sẽ bị mờ nhanh nhất.

Vận kính phối hợp: Bản thân thức này đã là nhân vật chính của vận kính, ống kính trước và sau phải tĩnh (luân phiên năng lượng); đường đi ống kính và từ vựng orbit xem `camera-language.md`.

### ⑦ Tự sự cuộn trang dài

"Cuộn nhanh phanh gấp" của trang landing page dài/tài liệu/timeline. Bản thân việc cuộn không phải là nội dung, **điểm phanh mới là nội dung**.

| Tham số | Giá trị điển hình | Cảm giác điều chỉnh |
|---|---|---|
| Đường cong phanh | `Easing.out(Easing.exp)` một đường cong 50f đi hết toàn bộ hành trình | Giảm tốc phân đoạn sẽ bị "bơm"; một đường cong expo tự nhiên là "cuộn mạnh dừng dần" |
| Motion blur | Blur được điều khiển bởi sai phân dịch chuyển giữa các khung hình, tốc độ lớn thì mờ, dừng là sắc nét | Blur cố định là "máy ảnh bị bẩn"; chỉ mờ tại khoảnh khắc chuyển động, khung hình đứng yên luôn luôn sắc nét (kết luận cùng loại huarec) |
| Nhấn mạnh dòng mục tiêu | Sau khi dừng vị trí, dòng mục tiêu scale 1.03, phần còn lại lùi làm tối 0.38 | Làm tối đừng đến 0, ngữ cảnh phải còn đó; 1.03 là "nhịp thở" không phải "bật ra" |

(Giá trị thực tế từ shotcraft·scroll-brake-moves)

Quản lý ngân sách nhiều điểm phanh (Ngân sách thoải mái huarec, bê nguyên sang): Khoảng cách giữa hai lần "cuộn+phanh" liền kề ≥2.6-3.0s; trong bất kỳ cửa sổ 15s nào ≤4-5 lần; mỗi điểm phanh dừng lại ≥1.2s rồi mới cuộn tiếp. Điểm phanh là tiêu thụ sự chú ý, đắt hơn bản thân việc cuộn.

Bẫy đã biết: Cấm đẩy gần trong lúc cuộn, "vừa cuộn vừa đẩy gần sẽ bị chóng mặt" (huarec: thay đổi cấp toàn màn hình thì không đẩy ống kính); khi điểm phanh không có nhấn mạnh nội dung (dòng mục tiêu không nâng lên, phần còn lại không lùi làm tối), khán giả sẽ không biết tại sao lại dừng ở đây.

Vận kính phối hợp: Sau khi phanh gấp có thể nối một cú đẩy gần nhẹ (nấc 1.3x) vào dòng mục tiêu, nhưng bắt buộc phải sau khi đã dừng hoàn toàn ổn định; bảng nấc xem `camera-language.md`.

### ⑧ Chú thích feature callout

"Chỉ cho khán giả xem" trên ảnh chụp thực tế. Bốn công thức phụ, nguyên tắc chung: **Chú thích là hướng dẫn viên, không phải một phần của giao diện**, về phong cách phải tách biệt một lớp với UI sản phẩm (chữ serif/cảm giác viết tay/màu nhấn).

| Công thức phụ | Tham số | Cảm giác điều chỉnh |
|---|---|---|
| Đường chú thích phát triển | SVG path stroke-dashoffset vẽ đường, 12-18f, out-cubic; đường trước chữ sau, chữ mờ dần vào 5f sau khi đường tới nơi | Đường và chữ xuất hiện cùng lúc sẽ đọc ra là hình dán; đường là thời gian "ngón tay vuốt qua" |
| Hộp highlight | Vùng mục tiêu rounded-rect viền xòe ra 8f + bên ngoài hộp làm tối 0.3 | Chỉ vẽ viền mà không làm tối thì ánh nhìn không hội tụ; làm tối vượt quá 0.5 biến thành đèn thẩm vấn |
| Kính phóng đại | Hình tròn loupe nhúng mảnh cắt HD 2-3x (không phải CSS phóng to nền ảnh chụp), viền 2px + bóng mềm, 8f overshoot bật ra, đi theo mục tiêu 10-14f di chuyển chậm | Trong loupe bắt buộc phải là mảnh cắt độ phân giải cao, phóng to texture mờ bằng tự bóc phốt vật liệu kém; một ống kính tối đa một loupe |
| Thẻ giải thích nối dây | Thẻ giải thích lơ lửng trong không gian 3D, translateZ tầm 90px + chu kỳ bob gần giống nhưng không đồng bộ với chủ thể | Cùng không gian 3D cùng máy ảnh; chữ đè phẳng bị án lệ phủ quyết (ràng buộc cùng loại với chú thích lơ lửng spotlight-hero-card) |
| before/after slider | Quật nhanh 12f (out-cubic 8%→76% overshoot nảy về 70%) → dừng 18f → quét chậm 48f đến 40% cố định; tỷ lệ tốc độ 5:1; lớp after clip-path đi theo cần; sai phân tốc độ tay cầm điều khiển scaleX kéo giãn nhẹ đỉnh 1.18 | Tỷ lệ tốc độ <3:1 sự tương phản nhịp điệu không cảm nhận được; quật nhanh tuyên bố "đã thay đổi", quét chậm chứng minh "thay đổi ở đâu"; quét chậm dừng 40% để after ở lại trong khung định hình |

(Slider là giá trị thực tế từ shotcraft·before-after-slider-scrub)

Bẫy đã biết: Hai bản before/after bắt buộc phải cùng bố cục cùng vị trí máy ảnh, nếu không sẽ đọc ra là hai trang; before dùng trạng thái cũ thực tế (bơm dữ liệu cũ/tắt tính năng), không tự vẽ tay before "cố tình xấu"; badge đặt ở vùng nội dung, đừng đè avatar thanh bên.

Vận kính phối hợp: Trước khi chú thích xuất hiện, ống kính đẩy trước đến nấc 1.3-1.45x để vùng mục tiêu chiếm màn hình (huarec: nội dung chiếm 80% vùng hiển thị), trong lúc chú thích ống kính khóa chặt.

---

## §3.9 Bố trí Bát thức trong khung xương năng lượng toàn bộ video

Đơn thức chỉ là ống kính, hoàn thiện video phải xếp theo 4 phân đoạn promo-energy-arc (shotcraft·Bộ ba cấp đạo diễn; kỷ luật phân cảnh "ngân sách khung hình hold/rest vạch đi trước rồi mới xếp hiệu ứng" cũng áp dụng):

| Phân đoạn năng lượng | Tỷ lệ thời lượng | Đặt những thức nào | Lý do |
|---|---|---|---|
| ① Mở màn thương hiệu | 8-12% | Không đặt UI thức | Wordmark hold ≥1s, UI đừng tranh mở màn |
| ② Dựng tiểu sở đơn nhân vật | 12-15% | ①hero đặc tả hoặc ②build-up | Cảnh quay chất lượng cao nhất nhịp điệu chậm nhất, toàn bộ video chỉ có một vị trí |
| ③ Leo thang tính năng | 55-65% | ③④⑤⑦⑧ luân phiên, mỗi cảnh gắn với một tính năng độc đáo | Luân phiên năng lượng cao trung thấp; ⑧callout là vị trí điền tốt cho vị trí hít thở năng lượng thấp |
| ④ Kết bài hội nghị ra mắt | 13-16% | ⑥Tuần tra 3D (Biến thể ảnh tập thể) | Đỉnh cao năng lượng toàn bộ video, trưng bày nhiều màn hình cùng khung hình sau đó sign-off |

Tra nhanh móc âm thanh (Chỉ làm sau khi hình ảnh đã khóa, đóng đinh khung hình viết biểu thức tương đối, shotcraft giai đoạn 5):

| Thức | Đóng đinh âm thanh gì |
|---|---|
| ① hero đặc tả | Nẩy lên whoosh-big, vệt sáng sparkle, reseat một tiếng snap, ba chuyển động ba âm thanh riêng |
| ② build-up | Đổi sang thật 1 tiếng pop, từng dòng hiện hình mỗi dòng 1 tiếng tick cực nhẹ, từ cuối trễ nửa nhịp một tiếng chime nhẹ khép lại |
| ③ typing | Âm thanh keyboard có chiều dài khớp nghiêm ngặt với phân đoạn gõ chữ (cắt theo số ký tự); thiếu thì cắt, quá dài thì xén |
| ④ click | Click là tiếng to nhất toàn bộ video (đỉnh của phân cấp âm lượng), ripple không có tiếng |
| ⑤ Chuyển trạng thái | Modal bật ra 1 tiếng pop mềm, đẩy route 1 tiếng whoosh-fast |
| ⑥⑦ Tuần tra/Cuộn | Phân đoạn tốc độ đều không tiếng hoặc tiếng hum cực nhẹ, phanh gấp 1 tiếng thud trầm |
| ⑧ callout | Đường chú thích phát triển đi kèm tiếng ma sát draw cực nhẹ; slider quật nhanh whoosh + nảy về tick, quét chậm không tiếng |

---

## §4 Kỷ luật nhịp điệu sắt (Thừa kế từ án lệ shotcraft, dành riêng cho demo UI)

1. **Demo tương tác đi theo tốc độ thao tác người thật.** Gõ chữ 3f/ký tự, trước khi click có di chuyển, trước khi phản hồi có nhịp thở. Bản đầu tiên của cảnh tương tác hầu như luôn bị nhanh, phác thảo là dùng tốc độ người (Án lệ R3, type-and-filter vì lý do này mà phải làm lại).
2. **Hiệu ứng hàng loạt kết bài đứng yên 0.5s.** Lưới thu hẹp xong, danh sách nhúng xong, panel hạ cánh xong, toàn bảng đứng yên nửa giây rồi mới sang cảnh tiếp theo.
3. **Thoát màn hình bắt buộc phải lệch pha.** Các phần tử không phải mục tiêu mờ dần ra lệch pha khoảng cách 0.4f theo thứ tự đọc, "biến mất cùng lúc sẽ đọc ra là trang web bị crash", dù chỉ lệch 0.4f cũng đủ (type-and-filter).
4. **Một ống kính một hiệu ứng.** Một ống kính chỉ kể một hành vi UI; cùng màn hình hai hiệu ứng đang tranh diễn, khán giả chẳng nhìn thấy cái nào.
5. **Kết bài đứng yên thực sự.** Vị trí nhịp thở, khung hình hold cấm bất kỳ trôi đuôi nào (zoom thay đổi nhỏ, opacity điều chỉnh nhỏ đều tính); "tĩnh" là một beat được thiết kế ra, không phải chưa xếp chuyển động.

---

## §5 Nối tiếp với Cửa ba hướng / Giao thức tài sản

**Bảng ba hướng ra như thế nào**: Ba hướng của animation UI sản phẩm không phải ba bộ skin thị giác, mà là **ba cách diễn giải tự sự ống kính của cùng một vật liệu UI**. Bộ ba vật liệu thu thập một lần, ba hướng tái sử dụng. Ví dụ cùng một ảnh chụp sản phẩm: Hướng A đi theo Bát thức ① (Dựng tiểu sử hero đặc tả đơn nhân vật), Hướng B đi theo Bát thức ② (build-up từ không đến có kể năng lượng tạo ra), Hướng C đi theo Bát thức ⑥+⑦ (Tuần tra+tự sự cuộn trang kể thể lượng). Bảng hướng đi mỗi cái kèm 2-3 khung hình keyframe thumbnail, để Hoa Thúc chọn là "kể giao diện này như thế nào", không phải "hình nào đẹp". Cửa ba hướng là cửa cứng 100%, chỉ định phong cách cũng không miễn trừ (Quy tắc sẵn có trong SKILL.md).

**Lấy vật liệu ảnh chụp UI đi theo `brand-asset-protocol.md`**: Ảnh chụp UI của sản phẩm kỹ thuật số là tài sản công dân hạng nhất trong giao thức đó (đóng góp độ nhận diện cực cao), kênh thu thập, ngưỡng chất lượng 5-10-2-8, đóng cứng brand-spec.md tất cả đều tuân thủ. Giá trị tăng thêm của tài liệu này chỉ có một dòng: Khi thu thập hãy thực hiện theo quy cách bộ ba vật liệu của §1 (2x toàn trang + mảnh cắt nền trong suốt + layout.json), thu thập đầy đủ một lần, cả con đường vận kính và tái dựng đều đủ dùng.

**Cấm vẽ tay UI giả**: Khi không tìm thấy ảnh chụp thực tế hãy làm theo giao thức兜底 (yêu cầu người dùng cung cấp ảnh chụp thực tế / cắt khung hình video demo chính thức), không dùng mockup generator ghép vào, không dùng CSS vẽ một giao diện "trông có vẻ giống". Chúng ta đang thể hiện sản phẩm này, không phải "một sản phẩm".

**Ngoại lệ chỉ có một**: Khi đi theo Con đường 2 tái dựng HTML, bản tái dựng chính là "bản phục chế lấy ảnh chụp thực tế làm chuẩn", bắt buộc phải đối chiếu ảnh chụp hiệu chỉnh từng khu vực (font chữ, bo góc, khoảng cách, icon đều đo từ ảnh chụp), sau khi tái dựng xong chụp khung hình so sánh song song với ảnh chụp. Tái dựng ra một giao diện "đại loại giống", còn gây hại hơn dùng trực tiếp ảnh chụp, khán giả cực kỳ nhạy cảm với sai lệch giao diện sản phẩm họ dùng hàng ngày.

---

## §6 Tự kiểm tra trước khi giao hàng (Chuyên mục demo UI, bổ sung best-practices §7)

- [ ] Đã đi theo cây quyết định §1? Không tái dựng HTML trong kịch bản "ảnh chụp là đủ"?
- [ ] Phần tử mảnh cắt sau khi chuyển động trở về vị trí slot thực tế của layout.json?
- [ ] Con trỏ và thao tác toàn bộ quá trình nằm trong vùng hiển thị (bao gồm 8% lề)?
- [ ] User typing là nhịp điệu 3f từng ký tự, AI output mới là Chunk Reveal, hai cái không dùng lẫn?
- [ ] Con trỏ khi gõ chữ sáng liên tục, gõ xong mới nhấp nháy? Click có ripple vòng đôi?
- [ ] Trong chuyển trạng thái không còn tàn dư addEventListener / classList / CSS transition?
- [ ] Thoát màn hình hàng loạt lệch pha ≥0.4f, kết bài toàn bảng đứng yên 0.5s?
- [ ] Toàn bộ video chỉ có một ống kính dùng viền sáng/glint, và chỉ trao cho nhân vật chính?
- [ ] Before/after cùng bố cục cùng vị trí máy ảnh, before là trạng thái cũ thực tế?
- [ ] Bảng ba hướng là ba cách diễn giải ống kính của cùng một vật liệu UI, không phải ba bộ skin?

---

## §7 Tra nhanh các dạng thất bại thường gặp

| Triệu chứng | Nguyên nhân gốc rễ | Quay lại mục nào |
|---|---|---|
| "Chưa đủ cảm giác công nghệ" nên thêm hạt/phát sáng | Nhầm hướng rồi, khoảng trống chất lượng ở UI thực tế và vận kính | Tuyên ngôn mở đầu + §1 |
| Giao diện như hình dán lơ lửng trên canvas | Không gắn khung thiết bị, không có ngôn ngữ bóng | §1 Khung thiết bị + Bát thức ① Bóng hai lớp |
| Chữ bị mờ sau khi đẩy gần | Tỷ lệ phóng đại ảnh chụp không đủ hoặc vấn đề độ phân giải rasterization | §1 Bộ ba vật liệu + camera-language.md |
| Tương tác "giống kịch bản không giống người" | Gõ chữ/click/phản hồi đều chạy theo tốc độ máy | Bát thức ③④ + §4 Quy tắc sắt 1 |
| Preview bình thường, render mới lộ hàng | Trạng thái do sự kiện điều khiển bị trộn vào timeline | Bát thức ⑤ Bảng đối chiếu + gsap-recipes §6 |
| Khán giả nói "hơi chóng mặt" | Ống kính và trang di chuyển cùng lúc, hoặc điểm phanh vượt ngân sách | Bát thức ⑥⑦ + Ngân sách huarec |

---

## Phụ lục · Mối quan hệ với các tài liệu khác

| Tài liệu | Mối quan hệ |
|---|---|
| `camera-language.md` | Từ vựng ống kính, động cơ vận kính, thực thi camera rig. Tài liệu này nói "phối ống kính gì", bên đó nói "làm ống kính như thế nào" |
| `animation-best-practices.md` | Cú pháp chuyển động cấp phần tử tổng cương. §3.5 quỹ đạo chuột, §4.1 FLIP, §4.5 Chunk Reveal được tài liệu này dẫn chiếu |
| `gsap-recipes.md` | Dịch thuật cấp thực thi. §3.4/§3.5 công thức proxy, §6 quy tắc an toàn seek là tiền đề thực thi của tất cả công thức trong tài liệu này |
| `brand-asset-protocol.md` | Giao thức lấy vật liệu ảnh chụp UI. §1 bộ ba vật liệu của tài liệu này là mở rộng quy cách hóa của nó |
| `assets/cursor.jsx` | Thực thi component của Bát thức ④, sử dụng phối hợp với `browser_window.jsx` / `macos_window.jsx` |
| `apple-gallery-showcase.md` | Trưng bày nhiều sản phẩm cùng màn hình đi theo bên đó; tự sự UI đơn sản phẩm đi theo tài liệu này |
