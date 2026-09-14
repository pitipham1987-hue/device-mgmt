# Storyboard Basics · Phân cảnh Nhẹ và Bố cục Màn hình

> Phương pháp phân cảnh trước khi làm bất kỳ hoạt ảnh nào, bất kể 5 giây hay 50 giây. Cốt lõi trong một câu: **Mỗi cảnh trước hết là một bìa động**.
>
> Nguồn gốc phương pháp luận: Đúc kết từ thực tế 300+ ảnh bìa trong kho tư liệu ảnh minh họa Hoa Chú với các quy tắc khung hình đứng yên (S1-S11), án lệ khung năng lượng trong 106 thẻ ống kính video-shotcraft, công lý ngân sách ống kính từ Hệ thống Đạo diễn Quay phim HuaRec Studio.

---

## 0 · Định vị: Phân công với Nhật ký Đạo diễn Launch-Film

Tệp này là **bản nhẹ hàng ngày** của `launch-film-director-notes.md`. Nhật ký đạo diễn vạn chữ launch film là quy trình hạng nặng, hoạt ảnh hàng ngày không cần bộ đó, nhưng **bản thân khâu phân cảnh thì không được bỏ** —— Bỏ qua khâu này sẽ dẫn tới tư duy thời gian biểu (Giây thứ mấy ra phần tử gì), chứ không phải tư duy màn hình (Khung hình này trông như thế nào).

| Độ dài / Loại | Yêu cầu phân cảnh | Căn cứ |
|---|---|---|
| Hoạt ảnh < 20s, motion graphic, demo | **Thẻ phân cảnh nhẹ** của tệp này (§5, mỗi cảnh 1 hàng) | Dưới 20s không đáng viết vạn chữ, nhưng bố cục màn hình mỗi cảnh phải nghĩ rõ trước |
| Hoạt ảnh ≥ 20s | `NhatKyDaoDien.md` theo yêu cầu của Giao thức Gate File, **yêu cầu tối thiểu = Định dạng thẻ phân cảnh của tệp này** (§5 8 trường không được thiếu cái nào), trên cơ sở đó tự do viết dày thêm | SKILL.md "Giao thức Gate File" |
| Launch film / Quảng cáo thương hiệu / Kỳ vọng "Cấp Apple" | Nâng cấp thành director's notes vạn chữ trên cơ sở thẻ phân cảnh | `launch-film-director-notes.md`, Part IV 10 trường mỗi cảnh của nó là bản tăng cường 8 trường của tệp này |

Ranh giới kích hoạt: Khi người dùng nói "Làm nhanh một hoạt ảnh", "Demo đơn giản", đừng quẳng ra quy trình vạn chữ, nhưng **thẻ phân cảnh vẫn vẽ bình thường**. Chi phí của thẻ phân cảnh là mười mấy phút, chi phí của việc bỏ qua nó là làm lại toàn bộ phim (Thực tế B00 2026-07-17: Bỏ qua thiết kế màn hình viết code trực tiếp, hiệu ứng chuyển động xanh sạch, thị giác bị phủ quyết toàn bộ).

Mối quan hệ với các tệp đã có:

- `animation-best-practices.md` Quản "chuyển động thế nào" (Nhịp điệu, easing, ngôn ngữ chuyển động), tệp này quản "mỗi khung hình trông như thế nào, sắp xếp giữa các ống kính ra sao", hai cái vuông góc bổ trợ cho nhau
- Tự sự dựa trên Scene của `cinematic-patterns.md` là tiền đề của tệp này: Phải có phân chia scene trước, mới bàn đến khung hình đứng yên của mỗi scene
- Tham số hiện thực hóa quay phim (Camera rig, sự khác biệt giữa zoom và dolly, mượt ống kính) → `camera-language.md`, tệp này chỉ tham chiếu từ vựng của nó ở tầng thiết kế

---

## 1 · Lập luận cốt lõi: Mỗi cảnh trước hết là một bìa động

Kho ảnh minh họa làm xong 300+ ảnh bìa đã đúc kết ra một nhận định: **Một ảnh bìa tốt = 1 Nhân vật chính cụ thể + ≤3 Phần tử hoạt động + 1 Đường dẫn dắt ánh mắt rõ ràng**. Mỗi cảnh của hoạt ảnh, trước tiên hãy thiết kế thành một ảnh bìa tĩnh theo tiêu chuẩn này, rồi mới cho nó chuyển động.

**Bìa = Định vị phân cảnh (Khung hình đứng yên)**. Cách kiểm tra cực kỳ cụ thể: Tạm dừng ngẫu nhiên bất kỳ khung hình nào của thành phẩm, khung hình đó phải có thể dùng trực tiếp làm ảnh bìa. Khung hình không qua được bài test ảnh bìa chứng tỏ bố cục của cảnh đó chưa được thiết kế, chỉ là sự chất đống các phần tử tự chuyển động trên trục thời gian.

Tại sao hoạt ảnh lại dễ vi phạm điều này hơn: Hoạt ảnh sẽ tự bào chữa bằng việc "các phần tử xuất hiện theo thời gian", cái trước chưa rút khỏi cái sau đã vào cảnh, kết quả là khung hình nào cũng chật chội. Ảnh bìa tĩnh không có cái cớ này, cho nên kỷ luật của ảnh bìa chính là thứ kỷ luật mà hoạt ảnh đang thiếu nhất (Phát hiện cốt lõi #1 từ thực tế kho tư liệu).

Thứ tự vì thế được cố định: **Xếp khung hình đứng yên trước, rồi mới biên đạo chuyển động**. Nhánh thumbnail pass ở §6 là phiên bản có thể thực thi của thứ tự này.

---

## 2 · 11 Quy tắc Khung hình Đứng yên

Cải biên các quy tắc S1-S11 của kho ảnh minh họa sang ngữ cảnh hoạt ảnh. Đây là trái tim của tệp này, bố cục của từng cảnh sẽ duyệt từng quy tắc một.

### Quy tắc 1 · Bài test Ảnh bìa (S1)

Khung hình đứng yên của mỗi scene bắt buộc phải có: **1 Nhân vật chính cụ thể + ≤3 Phần tử hoạt động + 1 Tiêu điểm thị giác rõ ràng**.

"≤3" là **hằng số của mỗi khung hình**, không phải tổng lượng của mỗi scene. Phần tử mới vào cảnh, phần tử cũ bắt buộc phải rút khỏi cảnh (Fade out, lùi về hậu cảnh, blur+dim đều tính là rút khỏi), ngân sách phần tử hoạt động cùng màn hình là hằng định. Khi viết trục thời gian hãy đếm các phần tử cùng màn hình tại mỗi thời điểm, vượt quá thì cắt bớt phần tử vào cảnh hoặc cho rút khỏi sớm hơn.

> Tự kiểm tra: Tự chọn ngẫu nhiên 3 thời điểm tạm dừng, đếm phần tử hoạt động cùng màn hình, có khung hình nào vượt quá 3 không?

### Quy tắc 2 · Điểm cuối zoom bắt buộc phải là điểm neo cụ thể (S2)

Điểm cuối của push-in / zoom chỉ có thể là: Logo thương hiệu, Con số then chốt, Nút bấm đó trong UI, Dòng code đó, Biểu cảm nhân vật. **Vân chất nền, Phần tử bầu không khí, Hình học trang trí không xứng đáng được zoom**. Đẩy lại gần một thứ không có thông tin, đồng nghĩa với việc nói với khán giả "Ở đây thực ra chẳng có gì để xem".

HuaRec bồi thêm một đao: **Zoom có bội số < 1.25x không đáng để làm** (Sự thay đổi thị giác không đủ cảm nhận, thuần túy là rung lắc; Ngoại lệ duy nhất là đẩy nhẹ định cảnh 1.06x lúc mở màn). Khi thiết kế phân cảnh mỗi hành động push-in trong [CAMERA] đều viết rõ điểm neo là gì, không viết ra được điểm neo thì xóa lần quay phim đó đi.

> Tự kiểm tra: Điểm cuối push-in của cảnh này có thể nói ra bằng một danh từ không (Nút bấm đó / Con số đó / Logo đó)? Không nói ra được thì xóa quay phim.

### Quy tắc 3 · Ánh mắt và Mũi tên = Kịch bản chuyển động ống kính (S3)

Quy tắc "Hướng ánh mắt nhân vật hướng về chủ thể, một bức hình chỉ để 1 mũi tên" trong ảnh bìa tĩnh, khi dịch sang hoạt ảnh chính là: **Đường dẫn dắt trong khung hình đứng yên chính là chỉ thị chuyển động của ống kính cho cảnh đó**. Ống kính đi theo ánh mắt, đẩy dọc theo mũi tên, pan thuận theo hướng đọc của UI.

Một scene chỉ có 1 đường dẫn dắt. Trong khung hình đứng yên không tìm thấy đường dẫn dắt, chứng tỏ cảnh này hoàn toàn không biết nên động thế nào. Lúc này đừng cố bịa ra một chuyển động ống kính, hãy quay lại sửa bố cục (Phát hiện cốt lõi #3 từ kho tư liệu).

> Tự kiểm tra: Đưa khung hình đứng yên cho người khác xem 3 giây, đường đi ánh mắt của họ có nhất quán với chuyển động ống kính bạn kế hoạch không?

### Quy tắc 4 · Phân công Chữ và Tranh, 2 Đường riêng (S4 + Dòng C kho tư liệu)

Trục chữ và trục hình ảnh là 2 trục thời gian độc lập, phân công không chồng chéo:

| Quy tắc | Nội dung | Nguồn |
|---|---|---|
| Chữ gánh câu móc (Hook) | Mỗi cảnh chữ lớn 2-6 chữ là mức trần cứng, văn bản chính 2-4 chữ là tốt nhất; Chữ nói câu móc, hình ảnh gánh bầu không khí, không để hình ảnh diễn gượng ép câu chữ | Thực tế C1/C3 kho tư liệu |
| Vào cảnh 2 nhịp | Nhịp vào cảnh của chữ lớn cố định 2 nhịp: **Từ ngữ hành động nện xuống trước, Ô màu định ngữ bổ sung sau** ("Thực tế" nện kín màn hình trước, ô màu "Claude 4.8" dán vào sau) | C5 kho tư liệu |
| Chữ rơi vào khoảng âm | Chữ viết luôn rơi vào khoảng âm (negative space) dự lưu của bố cục, không đè lên nhân vật chính, không đè lên tiêu điểm | C2 kho tư liệu |
| Màu mạnh đặc | Chữ trang trí bắt buộc dùng màu mạnh đặc; **CẤM tuyệt đối chữ trắng rỗng + viền màu** | Vùng cấm G kho tư liệu |

> Tự kiểm tra: Che hình ảnh chỉ giữ lại chữ, câu móc còn không; Che chữ chỉ giữ lại hình ảnh, bố cục còn đứng vững không? Cả 2 câu hỏi đều qua mới tính là phân công sạch sẽ.

### Quy tắc 5 · Ngôn ngữ Không gian Phần trăm (S5)

Mô tả bố cục trong thẻ phân cảnh bắt buộc phải viết đến độ mịn phần trăm: "Ảnh chụp màn hình sản phẩm góc trên bên phải 60% có thấu thị, nhân vật góc dưới bên trái 25%, chữ lớn đè lên mép trên ảnh chụp màn hình, cách 4 phía 10% vùng an toàn".

Đây không phải là cảm giác hình thức, mà là giao diện kỹ thuật: **Ngôn ngữ phần trăm ánh xạ trực tiếp sang tham số bố cục CSS/GSAP**, agent nhận được là dịch được ngay, không cần phải đưa ra quyết định bố cục nữa. Viết độ mịn kiểu "Bên trái để sản phẩm bên phải để chữ" coi như chưa viết (Thực tế đối chiếu template bố cục gbro: Prompt độ mịn phần trăm và mô tả mơ hồ sản phẩm ra chênh nhau 1 đẳng cấp).

> Tự kiểm tra: Mô tả bố cục của cảnh này có chứa ít nhất 3 con số phần trăm không? Không có thì vẫn là mô tả văn học, không phải spec.

### Quy tắc 6 · Tiền Trung Hậu cảnh = 3 Lớp Parallax (S6)

Khi thiết kế khung hình đứng yên hãy viết rõ tiền cảnh / trung cảnh / hậu cảnh là gì, ai che ai. Sự phân lớp này tự nhiên chính là tư liệu parallax: 3 lớp 3 tốc độ, mối quan hệ che chắn thay đổi chính là nguồn gốc của cảm giác độ sâu.

Tham số hiện thực hóa chép trực tiếp án lệ shotcraft: Hệ số tốc độ tầng thấu thị 0.35 / 0.7 / 1.4, **tỷ lệ tốc độ giữa các tầng ≥2 lần mới phân biệt được**, số tầng ≤4.

> Tự kiểm tra: Nói ra được tiền trung hậu cảnh của khung hình này là gì, ai che ai không? Không nói ra được thì chưa phân lớp.

### Quy tắc 7 · setup → payoff, Khung mở màn để lại ẩn số (S7)

Khung hình đứng yên mở màn của mỗi scene chừa lại một khoảng trống (Cây cầu chưa làm xong, con số chưa hé lộ, nửa màn hình trống), chuyển động của cảnh này chịu trách nhiệm đóng kín nó lại. Cấu trúc "Từ A đến B" tự nhiên chính là tự sự chuyển cảnh: Payoff của cảnh trước có thể trực tiếp là setup của cảnh sau.

Mặt ngược lại là "Khung mở màn đã bày trọn bộ thông tin, rồi các phần tử làm hiệu ứng tại chỗ": Cái đó là poster biết động, chứ không phải ống kính.

> Tự kiểm tra: Đặt khung hình mở màn và khung hình kết thúc của cảnh này cạnh nhau, khán giả có nói ra được "cái gì đã được thực hiện" không?

### Quy tắc 8 · Vật thể hóa + Cảm giác quá trình để chọn ý tượng (S8)

Khái niệm trừu tượng bắt buộc phải vật thể hóa thành hành động đồ vật nhìn thấy được, và **ưu tiên chọn hành động có cảm giác quá trình**: "Kết nối" = Cây cầu bắc từng đoạn một, "Giảm giá" = Biển giá $89 gạch đi từng chữ thành $29, "Mùi AI nặng" = Trên tờ giấy nháp stagger mọc kín các ô vuông màu xám chỉnh tề.

Bản thân hành động chính là kịch bản hoạt ảnh: Chọn đúng ý tượng, tiêu điểm màn hình và chuyển động ống kính sẽ tự mọc ra từ ý tượng, không cần phải phát minh thêm hiệu ứng. Phương pháp là làm dịch thuật kiểu Feynman trước (Khái niệm này "xảy ra" thế nào?), dịch xong mới bàn đến phong cách vẽ (Phát hiện cốt lõi #4 từ kho tư liệu).

> Tự kiểm tra: Ý tượng này có thể khái quát bằng một động từ không (Bắc, Gạch, Mọc, Rơi)? Ý tượng chỉ mô tả được bằng danh từ không có cảm giác quá trình, đổi cái khác.

### Quy tắc 9 · Phong cách × Bố cục Vuông góc (S9)

Nhận thức kiến trúc quan trọng nhất của kho tư liệu ảnh minh họa, di chuyển nguyên bản: **Lớp da phong cách toàn phim định 1 lần** (Bảng màu, phông chữ, chất liệu, lô-gích nền), **template bố cục đổi theo từng scene** (Cảnh này đặc tả ở giữa, cảnh sau lưới 3 cột, cảnh sau nữa 2 vùng đường chéo).

Chuyển đổi scene = Chuyển đổi bố cục, tuyệt đối không phải chuyển đổi phong cách. Toàn phim đổi phong cách là thảm họa, toàn phim 1 bố cục là gây ngủ. Mối nối với cổng 3 hướng: Moodboard người dùng chọn chính là bức tranh "định 1 lần" của lớp da phong cách.

> Tự kiểm tra: Lấy ngẫu nhiên 2 cảnh so sánh, phong cách (Bảng màu / Phông chữ / Chất liệu) nên không nhìn ra sự khác biệt, bố cục nên nhìn phát khác ngay. Ngược lại tức là kiến trúc sai rồi.

### Quy tắc 10 · Kế thừa Vùng cấm (S10)

Vùng cấm thẩm mỹ của kho tư liệu ảnh minh họa toàn bộ được kế thừa, và cần đặc biệt cảnh giác với bệnh nghề nghiệp của hoạt ảnh: **Hạt phát sáng, Dòng dữ liệu, HUD toàn ảnh, Neon Cyber là những tư liệu tiện tay nhất của motion graphics, vừa hay đều nằm trong vùng cấm Hoa Chú**. Khi làm hoạt ảnh tay ngứa hơn làm hình tĩnh, phải giữ mình.

Lời giải đúng cho cảm giác công nghệ = Ảnh chụp màn hình UI thật (Hình thái card lơ lửng 3D) + Chữ lớn sạch sẽ + Nền sáng. Sự kết hợp Nền xanh đậm #0D1117 + Neon glow vẫn bị CẤM như cũ (Chi tiết giống SKILL.md §6.2).

> Tự kiểm tra: Trong màn hình có bất kỳ phần tử nào đang "phát sáng" không? Có thì trước tiên nghi ngờ mình đang dùng bệnh nghề nghiệp để lười biếng, luận chứng từng cái một để giữ hay xóa.

### Quy tắc 11 · Bài test 3 mét (S11)

Bất kỳ khung hình nào tạm dừng, từ lớn nhất trong màn hình đọc được ở khoảng cách 3 mét. **Không vì "dù sao cũng sẽ động" mà thu nhỏ cỡ chữ** —— Khán giả khi xem hoạt ảnh dành sự chú ý cho mỗi khung hình ít hơn xem hình tĩnh, cỡ chữ chỉ có thể to hơn chứ không thể nhỏ hơn. Quy tắc này và điều khoản độ đậm đặc thị giác trong `animation-best-practices.md` §6.5 làm mức trên và mức dưới cho nhau: Điều khoản độ đậm đặc chống trống trải, bài test 3 mét chống việc hy sinh khả năng đọc khi chật chội.

> Tự kiểm tra: Thu nhỏ ảnh chụp khung hình khóa lại bằng màn hình điện thoại, từ lớn nhất còn đọc được không?

---

## 3 · Hệ thống Cảnh biệt: Ánh xạ 5 Mức Zoom

5 mức xa, toàn, trung, cận, đặc tả của điện ảnh truyền thống, trong hoạt ảnh HTML tương ứng với 5 mức zoom (Giá trị mức nhất quán với `camera-language.md` §4.3, chi tiết hiện thực hóa lấy nó làm chuẩn):

| Cảnh biệt | Mức zoom | Xem cái gì | Kịch bản điển hình | Nguồn |
|---|---|---|---|---|
| Cảnh xa | 0.78x | Toàn cục + Khoảng trắng môi trường | Establishing mở màn, Ảnh tập thể tạ mạc | Góc máy toàn trang shotcraft 0.78 |
| Cảnh toàn | 1x (Đẩy nhẹ định cảnh 1.06x) | Giao diện hoàn chỉnh / Cảnh hoàn chỉnh | Mặt chuẩn tự sự, nhà của đa số ống kính | Án lệ định cảnh 1.06x HuaRec |
| Cảnh trung | 1.3-1.45x | Một khối chức năng | Lực lượng nòng cốt demo chức năng | Mức đẩy nhẹ / đẩy vừa HuaRec |
| Cảnh cận | 1.8x | Một component / Một dòng dữ liệu | Nhấn mạnh tương tác cụ thể | Mức đẩy mạnh HuaRec |
| Đặc tả | 2.3x (Mức trần) | Điểm neo cụ thể của Quy tắc 2 | Con số then chốt, nút bấm đó, logo | Mức trần bội số HuaRec 2.3x |

Hai điểm giải thích:

- Cùng một mức có thể dùng zoom (scale, không thấu thị) hoặc dolly (perspective + translateZ, có thấu thị) để hiện thực hóa, khí chất hoàn toàn khác nhau; Quy tắc chọn kiểu và cách viết camera rig của cả hai nằm ở `camera-language.md`, tầng phân cảnh chỉ cần viết rõ mức và động cơ trong cột [CAMERA]
- Mức là từ vựng thiết kế chứ không phải gông còng: 1.5x, 2.0x đều hợp pháp, tác dụng của mức là làm cho các từ "Cảnh trung", "Đặc tả" trong bảng phân cảnh có ý nghĩa số liệu xác định

### Nhịp điệu Cảnh biệt của các Ống kính liền kề

| Quy tắc | Nội dung | Căn cứ |
|---|---|---|
| Tránh nối liên tiếp cùng cảnh biệt | 2 cảnh liền kề cùng mức, chuyển đổi không có cảm giác thay đổi, đọc thành "Màn hình bị nháy một cái" chứ không phải "Đổi ống kính rồi"; Ít nhất chênh 1 mức | HuaRec "Bội số <1.25x không đáng quay" mở rộng ra giữa các cảnh |
| Tránh nhảy 2 cấp | Cảnh xa cắt thẳng sang đặc tả (0.78x → 2.3x) sẽ bị chóng mặt; Muốn nhảy bắt buộc phải là punch-in cố ý, và phối hợp chuyển cảnh (Nháy trắng / whip-pan) lót lại | Ngân sách thỏa mái HuaRec |
| Tiêu điểm gần thì gộp cảnh | Tiêu điểm 2 cảnh liền kề rất gần nhau, gộp thành 1 cảnh, khung bao kết hợp tính lại mức | Ngữ pháp giữa các cảnh HuaRec |
| Tiêu điểm trung bình thì bình移 | Thà dùng cảnh biệt thấp hơn một mức để bình移 qua trong 1 cảnh, không làm bơm đẩy "Kéo ra rồi đẩy vào" | Án lệ "Đổi sang bình移" HuaRec |
| Tiêu điểm xa thì bỏ cảnh | Hai tiêu điểm nhảy chéo ngang, tuyệt đối không quay liền 2 zoom, cắt bỏ một cái hoặc chèn quá độ cảnh toàn | Án lệ "Bỏ cảnh" HuaRec |
| Thời lượng co giãn theo biên độ | Thời lượng quá độ chuyển đổi cảnh biệt không phải hằng số: `duration = 0.55 × |ln(zoomTo/zoomFrom)| / ln2`, clamp [0.30, 0.94]s; Duration cố định là nguồn gốc cảm giác nghiệp dư | Công thức co giãn logarit HuaRec |
| Ngân sách nhịp điệu | Khoảng cách thay đổi ống kính liền kề ≥2.6s, trong cửa sổ 15s bất kỳ thay đổi cảnh biệt ≤4-5 lần | Ngân sách thỏa mái A2 HuaRec |
| Quy tắc thép Tạ mạc | Thành phẩm luôn kết thúc bằng cảnh toàn / cảnh xa, trước khi kết thúc ≥0.8s tạm dừng cảnh toàn; **Tuyệt đối không dừng đột ngột ở trạng thái đẩy lại gần** | Án lệ tạ mạc HuaRec |

---

## 4 · Khung Năng lượng: Dành Ngân sách Hold trước, rồi mới Sắp xếp Hiệu ứng

Sắp xếp ống kính của phim nhiều cảnh (≥10s) không phải phân bổ bình quân, áp dụng khung 4 đoạn vị promo-energy-arc của shotcraft:

| Đoạn vị | Tỷ lệ thời lượng | Năng lượng | Nội dung | Chỉ số cứng |
|---|---|---|---|---|
| ① Mở màn | 8-12% | Thấp | Thương hiệu / Chủ đề ra mắt | Chữ hiệu định vị hold ≥1s |
| ② Đơn nhân vật chính lập truyện | 12-15% | Thấp vừa · Toàn phim chậm nhất | Nhân vật chính một hành động hoàn chỉnh (Vào cảnh → Tạm dừng → Về vị trí) | Hành động ≥3s, chất cảm cao nhất |
| ③ Leo dốc chức năng | 55-65% | Cao vừa ⇄ Thấp xen kẽ | Mỗi cảnh gắn một chức năng độc đáo, năng lượng cao thấp xen kẽ | Sau mỗi 1-2 cảnh chức năng chèn 1 **thẻ chữ nhịp thở** (Năng lượng thấp, khoảng trắng lớn, 2-6 chữ) |
| ④ Kết màn | 13-16% | Đỉnh toàn phim | Ảnh tập thể + sign-off | Kết thúc hold, tạ mạc quay về cảnh toàn (§3 quy tắc thép) |

**Kỷ luật xếp cảnh: Dành ngân sách hold / rest trước, rồi mới xếp hiệu ứng** (Quy trình điền vào chỗ trống shotcraft). Thứ tự cụ thể:

1. Liệt kê danh sách chức năng, đếm ra số cảnh N
2. Dành thời gian tĩnh không thể xâm phạm ra khỏi ngân sách tổng trước: Chữ hiệu hold ≥1s, Hoạt ảnh hàng loạt kết thúc 0.5s tĩnh, Hành động mở màn ≥3s, Kết thúc ≥0.8s tạm dừng cảnh toàn, Tạm dừng 0.5s trước kết quả then chốt (best-practices §4.4)
3. Thời gian còn lại mới chia cho hiệu ứng, năng lượng cao thấp xếp xen kẽ, không cho phép 2 cảnh liên tiếp năng lượng cao
4. Chọn chuyển cảnh cho từng đường nối (§7), khung hình chuyển cảnh trích từ ngân sách của các cảnh liền kề, không cộng thêm thời gian
5. Thẻ chữ nhịp thở cũng có định thức bố cục: Chữ lớn 2-6 chữ + Khoảng âm toàn màn hình + Zero trang trí, bản thân nó đã là một khung hình đứng yên qua bài test ảnh bìa (Phần tử hoạt động = 1), tác dụng là hạ năng lượng cho đoạn leo dốc chức năng, cho khán giả thời gian tiêu hóa. Đừng làm thẻ chữ nhịp thở thành một cảnh thông tin nữa, thế thì coi như chưa chèn.

Mối quan hệ với `animation-best-practices.md` §1 5 đoạn tự sự: Slow-Fast-Boom-Stop là đường cong nhịp điệu của **đơn cảnh / phim ngắn** (≤15s một hơi), promo-energy-arc là khung xương của **phim nhiều cảnh**; Dưới 15s chọn 1 trong 2 là được, trên 20s dùng khung năng lượng xếp cảnh, bên trong mỗi cảnh lại dùng cảm giác tay của 5 đoạn tự sự.

---

## 5 · Thẻ Phân cảnh Nhẹ: Giao hàng của Tệp này

Mỗi cảnh một hàng, 8 trường không được thiếu cái nào. Bảng 4 cột của shotcraft (#|Thời gian|Ống kính|Hiệu ứng then chốt) là cấu hình thấp nhất, ở đây mở rộng thành 8 cột, trong đó [CAMERA] độc lập thành cột (10 trường của nhật ký đạo diễn launch film sau này sẽ mở rộng thành 11, trường mới thêm chính là trường này):

```
| # | Thời gian | Cảnh biệt | [CAMERA] Quay phim+Động cơ | Bố cục màn hình (Ngôn ngữ phần trăm) | Hiệu ứng then chốt | Chuyển cảnh sang cảnh tiếp | Số khung hình nghiệm thu |
```

Yêu cầu cách viết các trường:

- **Thời gian**: Giây bắt đầu kết thúc + Thời lượng ẩn; Thời gian chuyển cảnh bao hàm trong ngân sách cảnh này, không liệt kê riêng (Kỷ luật xếp cảnh §4 điều 4)
- **Cảnh biệt**: 1 trong 5 mức ở §3 + Giá trị zoom
- **[CAMERA]**: Hành động quay phim + Một câu động cơ; "Tĩnh" cũng là quay phim hợp pháp, nhưng phải viết tại sao tĩnh; Mỗi push-in bắt buộc viết điểm neo (Quy tắc 2)
- **Bố cục màn hình**: Ngôn ngữ phần trăm (Quy tắc 5), bao gồm phân lớp tiền trung hậu cảnh (Quy tắc 6)
- **Hiệu ứng then chốt**: Cảnh này chỉ nói một hiệu ứng (Án lệ shotcraft "Một cảnh một hiệu ứng")
- **Chuyển cảnh sang cảnh tiếp**: Chọn kiểu trong bảng quyết định §7, không được để trống, không được viết "Cắt trực tiếp" (Cắt trần phải viết thành hidden-cut cố ý mới hợp pháp)
- **Số khung hình nghiệm thu**: **Mỗi cảnh viết trước 1-2 số khung hình**, sau khi hiện thực hóa xong thì chụp đúng mấy khung hình này để tự kiểm tra (Án lệ shotcraft "Mỗi cảnh 3 lần đọc + Viết trước số khung hình nghiệm thu"). Ý nghĩa của việc viết trước: Tiêu chuẩn nghiệm thu được cố định trước khi tay làm, không chừa khoảng không gian mơ hồ "Nhìn có vẻ cũng ổn" sau khi làm xong

### Ví dụ: Bảng Phân cảnh Hoàn chỉnh cho Hoạt ảnh Sản phẩm 12s

Sản phẩm giả định: Công cụ dọn dẹp ảnh chụp màn hình PicSort. 1920×1080 · 30fps · 360 Khung hình.

| # | Thời gian | Cảnh biệt | [CAMERA] Quay phim+Động cơ | Bố cục màn hình (Ngôn ngữ phần trăm) | Hiệu ứng then chốt | Chuyển cảnh sang cảnh tiếp | Số khung hình nghiệm thu |
|---|---|---|---|---|---|---|---|
| 1 | 0-2.0s | Cảnh toàn 1x | Tĩnh, 0.3s cuối đẩy nhẹ lên 1.06x · Định cảnh + Đặt ẩn số | Đống ảnh chụp màn hình hỗn loạn chiếm 70% giữa, chữ lớn "3000 tấm ảnh chụp" rơi vào 20% khoảng âm đỉnh, cách 4 phía 10% vùng an toàn; Tiền cảnh 2 tấm ảnh chụp che nhẹ trung cảnh | Ảnh chụp 30ms stagger rơi xuống bàn; Chữ lớn vào cảnh 2 nhịp ("3000 tấm" nện xuống, ô màu "ảnh chụp" bổ sung sau) | Tiếp sức mất nét (Đống lộn xộn blur mờ ra) | f30 / f55 |
| 2 | 2.0-3.5s | Đặc tả 2.3x | push-in 1x→2.3x · Điểm neo = Góc nhãn ngày tháng góc dưới bên phải 1 tấm ảnh chụp | 1 tấm ảnh chụp chiếm 80% ở giữa có 2° thấu thị, góc nhãn ngày tháng góc dưới bên phải 15%; Các ảnh chụp khác lùi về hậu cảnh blur | Đẩy lại gần đồng bộ hậu cảnh blur+dim (Bộ 3 chuyển đổi tiêu điểm) | Phần tử chia sẻ về vị trí (Tấm ảnh chụp này thu nhỏ bay vào bên cạnh ô nhập cảnh tiếp) | f85 |
| 3 | 3.5-6.0s | Cảnh trung 1.4x | Pan ngang đi theo con trỏ · Đường dẫn dắt = Quỹ đạo đường cong con trỏ | UI sản phẩm chiếm 85% có thấu thị, logo góc trên bên trái 10%, ô tìm kiếm ngang ở giữa chiếm 55%; Con trỏ từ góc dưới bên trái 25% đường cong vào cảnh | Đánh chữ 3f/ký tự + Kết quả Chunk Reveal; Đánh xong nhịp thở 0.4s | mask-wipe (Mép panel kết quả mở rộng thành lưới cảnh tiếp) | f130 / f165 |
| 4 | 6.0-8.5s | Cảnh toàn 1x | pull-out 1.4x→1x · Động cơ = Trình diễn quy mô sau khi dọn dẹp | Lưới phân loại 3 cột chiếm 85%, mỗi đầu cột một thẻ màu; 15% khoảng âm đỉnh dành cho con số cảnh tiếp | Card theo cột stagger vào cột (Giữa các cột 30ms), đầy bảng xong tĩnh 0.5s | Bôi trắng | f210 / f250 |
| 5 | 8.5-10.5s | Cảnh cận 1.8x | Tĩnh · Kết quả then chốt hold, không tranh chấp với con số | "3000 → 12 loại" chiếm 60% ở giữa, từ ngữ hành động lớn nhất, ô màu định ngữ; 4 phía khoảng âm lớn | digit-roll về vị trí (tabular-nums), về vị trí xong hold 0.6s | Phần tử chia sẻ về vị trí (Con số thu nhỏ dịch lên nhường chỗ cho logo) | f290 |
| 6 | 10.5-12s | Cảnh toàn 1x | Tĩnh · Tạ mạc, cảnh toàn kết thúc | logo ở giữa chiếm 12%, slogan 1 dòng ở dưới 8%, còn lại toàn bộ để khoảng trắng | logo biến hình thu nạp (Phần tử trước sụp đổ → Mở rộng), khung hình cuối hold ≥1s | Không (Cuối phim) | f330 / f359 |

Đối chiếu kiểm tra bảng này có thể thấy khung xương: Cảnh 1 là setup đặt ẩn số (Quy tắc 7), Cảnh 2-4 là cảnh biệt luân phiên của leo dốc chức năng (Đặc tả → Trung → Toàn, không nối liên tiếp cùng mức, không nhảy 2 cấp), Cảnh 5 là đỉnh năng lượng + Ngân sách hold, Cảnh 6 tạ mạc cảnh toàn. Phần tử hoạt động cùng màn hình mỗi cảnh ≤3.

---

## 6 · Thumbnail Pass: Xác thực Khung Xám trước khi Viết Code Chính thức

Bảng phân cảnh là chữ viết, thumbnail pass biến nó thành xác thực bố cục nhìn thấy được. Chi phí dưới nửa giờ, là bảo hiểm cho chi phí làm lại.

**Step 1 · Dựng HTML khung xám**: Một HTML tạm thời, mỗi khung hình khóa một `<section>` 1920×1080. Chỉ dùng ô màu thuần + nhãn chữ để xếp bố cục: Nhân vật chính một khối xám đậm đánh dấu "UI sản phẩm 85%", Vùng chữ một ô màu đánh dấu "Chữ lớn: 3000 tấm ảnh chụp", Phần tử tiền cảnh khối xám nhạt. Không viết bất kỳ hiệu ứng nào, không chọn phông chữ, không chỉnh màu, chính là biến ngôn ngữ phần trăm của thẻ phân cảnh thành các khối nhìn thấy được.

Giai đoạn khung xám CẤM chỉnh cho đẹp (Chọn phông, phối màu, thêm bóng đều không được): Đẹp là việc Moodboard đã định xong, khung xám chỉ xác thực bố cục và nhịp điệu. Bắt đầu chỉnh cho đẹp = Bắt đầu trốn tránh vấn đề bố cục.

**Step 2 · Chỉ làm 3-5 khung hình khóa**: Không phải cảnh nào cũng làm, chọn các nút then chốt của khung năng lượng: Khung mở màn setup, Khung hero đoạn lập truyện, Một khung đại diện đoạn leo dốc, Khung đỉnh điểm, Khung tạ mạc.

**Step 3 · Playwright Chụp màn hình Hàng loạt**:

```bash
for i in 1 2 3 4 5; do
  npx -y playwright screenshot "file://$PWD/thumbnails.html#f$i" \
    "thumbs/f$i.png" --viewport-size=1920,1080
done
```

**Step 4 · 3 Câu hỏi Nghiệm thu** (Đối mặt với các ảnh thu nhỏ xếp song song để hỏi):

1. **Che tất cả nhãn chữ lại, sự khác biệt bố cục của 5 bức tranh còn nhìn ra không?** Không nhìn ra = Nhịp điệu chưa làm ra được, việc đổi template bố cục theo từng scene chưa thực hiện (Quy tắc 9; Cùng nguồn với tự kiểm tra thumbnail của best-practices §1)
2. **Mỗi bức tranh có riêng biệt qua được bài test ảnh bìa không?** (Quy tắc 1: 1 Nhân vật chính + ≤3 Phần tử + 1 Tiêu điểm)
3. **Đường dẫn dắt của mỗi bức tranh có chỉ ra được không?** Không chỉ ra được thì cảnh đó quay lại sửa bố cục, đừng đi tiếp xuống dưới (Quy tắc 3)

**Step 5 · Qua rồi mới viết code chính thức**. HTML khung xám giữ lại trong thư mục dự án làm chuẩn mực bố cục, khi thực hiện bị chệch hướng thì quay lại đối chiếu.

**Mối quan hệ với Cổng 3 hướng**: "Moodboard" của Cổng cứng 3 hướng (Khung hình khóa hero thực tế tĩnh + Bảng màu + Câu khí chất) về bản chất chính là **thumbnail bản đầu tiên**. Sau khi người dùng chọn hướng, thumbnail pass là việc mở rộng Moodboard đó thành chuỗi khung hình khóa của toàn phim: Moodboard định lớp da phong cách, thumbnail pass định bố cục từng cảnh, vừa hay là 2 tầng vuông góc của Quy tắc 9.

---

## 7 · Bảng Quyết định Chuyển cảnh: Chọn Kiểu theo Mối quan hệ Tự sự

Chuyển cảnh không phải trang trí, mà là lô-gích tự sự tại đường nối. Nhận định **mối quan hệ tự sự** của 2 cảnh liền kề trước, rồi mới chọn kiểu. Tham số hiện thực hóa từ vựng chuyển cảnh (Thời lượng, easing, cách viết mask) xem từ vựng chuyển cảnh 3 tầng trong `camera-language.md` §7.

| Mối quan hệ tự sự của 2 cảnh liền kề | Chuyển cảnh ưu tiên | Dự phòng | Căn cứ án lệ |
|---|---|---|---|
| Nhảy thời gian ("3 ngày sau", "Sau khi dọn dẹp xong") | Thẻ chữ nền đen / Bôi trắng | whip-pan | shotcraft 6 kiểu: Độ lệch lớn dùng trắng / đen lót |
| Dịch chuyển không gian (Các vùng khác nhau của cùng giao diện) | **Một cảnh bình移, hoàn toàn không cắt** | hidden-cut | HuaRec "Khoảng cách vừa đổi sang bình移": Thà rộng thêm 1 mức đi qua trong 1 cảnh, không làm bơm đẩy ra-vào |
| Đối chiếu khái niệm (before/after, A vs B) | mask-wipe | Chuyển màn hình rồi cắt cứng + Nháy trắng | Án lệ shotcraft mask-wipe xuyên cửa sổ |
| Tiệm tiến / Nhân quả (Đáp án của setup nằm ở cảnh tiếp) | Phần tử chia sẻ về vị trí (Phần tử cảnh trước bay thành nhân vật chính cảnh sau) | morph | shotcraft travel 2 kiểu; Quy tắc thép voiceover-pipeline "hero qua scene morph không cắt" cùng nguồn |
| Độ lệch năng lượng lớn (Thẻ chữ nhịp thở → Cảnh năng lượng cao) | Bôi trắng / Nháy trắng FlashCut | whip-pan | shotcraft: Đường nối chọn kiểu theo độ lệch năng lượng |
| Độ lệch năng lượng nhỏ (Các cảnh chức năng liền kề đoạn leo dốc) | Tiếp sức mất nét / Mờ đan xen ≥8f | hidden-cut | shotcraft graze-face-tour "Mờ đan xen giữa các đoạn chống nháy đen" |

Ba kỷ luật:

1. **Một đường nối chỉ dùng 1 kiểu**, không chồng chéo (Nháy trắng + whip-pan cùng lên là slop)
2. **Khung hình chuyển cảnh trích từ ngân sách của cảnh liền kề**, không tự dưng cộng thêm thời lượng; Cột thời gian của bảng phân cảnh đã bao hàm chuyển cảnh
3. **Toàn phim zero cắt trần**. Nguyên văn án lệ shotcraft: Phim quảng cáo phát hành xuất sắc công nhận toàn bộ không có một lần cắt trần nào. Muốn hiệu ứng "cắt" thì dùng hidden-cut (Mượn phần tử kín màn hình / Khung hình trắng / Đỉnh chuyển động để giấu điểm cắt), đó là cắt đã qua thiết kế, chứ không phải cắt không qua thiết kế

Phim có thuyết minh thêm 1 điều: **move on pause** (Án lệ HuaRec). Chuyển đổi ống kính và chuyển cảnh hút vào khoảng trống giọng nói, chỉ tiến trước không lùi sau, mức trần 0.8s, vì khán giả di chuyển tầm mắt trong khoảng trống thính giác có chi phí nhận thức thấp nhất. Khi đi theo voiceover-pipeline dùng khoảng trống thực tế của timeline.json để định điểm cắt.

---

## 8 · Checklist Bắt đầu làm (Định nghĩa Hoàn thành Phân cảnh)

Trước khi viết code, xác nhận tất cả các điều sau đều tồn tại:

- [ ] Bảng phân cảnh: Mỗi cảnh 1 hàng, 8 trường đầy đủ, lưu vào thư mục dự án (Khi ≥20s chính là mục cốt lõi của `NhatKyDaoDien.md`)
- [ ] Khung hình đứng yên mỗi cảnh đã qua các điều áp dụng trong 11 quy tắc, ít nhất đã kiểm tra rõ ràng Quy tắc 1 (Bài test ảnh bìa), Quy tắc 3 (Đường dẫn dắt), Quy tắc 5 (Bố cục phần trăm)
- [ ] Cột cảnh biệt không có nối liên tiếp cùng mức, không có nhảy 2 cấp (§3)
- [ ] Ngân sách hold / rest đã dành ra trước, thẻ chữ nhịp thở đã chèn (§4)
- [ ] Lựa chọn chuyển cảnh của mỗi đường nối đã viết trong bảng, toàn phim zero cắt trần (§7)
- [ ] 3-5 Ảnh chụp màn hình khung xám của thumbnail pass đã qua 3 câu hỏi nghiệm thu (§6)
- [ ] Mỗi cảnh đã viết trước số khung hình nghiệm thu, chờ sau khi hiện thực hóa sẽ đối chiếu từng khung hình (§5)

Bảy mục đều qua, giai đoạn phân cảnh kết thúc, đi vào hiện thực hóa. Khi hiện thực hóa bị trả về trước tiên xác định định vị theo tầng: Vấn đề của khung xương hiệu ứng hay vấn đề của công nghệ thị giác (Định thức sửa chữa trong best-practices §6.5), bản thân bảng phân cảnh thường không cần viết lại.
