# Thư viện Phong cách Thiết kế: Web 20 loại + PPT 20 loại + Infographic 20 loại (Ưu tiên HTML nguyên bản)

> **Tái cấu trúc 06-2026**. Dựa trên nghiên cứu suy luận ngược đối với top 5 thiết kế được công nhận tốt nhất thuộc 10 loại website lớn + 10 loại thuyết trình lớn trên toàn cầu (Tổng cộng 100 trường hợp thực tế).
> Vấn đề chí mạng của thư viện 20 loại "Triết lý nhà thiết kế đồ họa/lắp đặt" phiên bản cũ: Các phong cách táo bạo hầu như đều là AI-generate-only (hạt/quang ảnh/vẽ tay), **khi người dùng mặc định không có năng lực tạo hình AI, default đều đi theo HTML, thì nửa sân chơi táo bạo bị xóa sổ trực tiếp, chỉ còn lại cực giản——đây là nguyên nhân gốc rễ của "default nghìn bài một điệu"**. Thư viện này đối với mỗi loại đều đánh dấu **độ phục hồi** dưới điều kiện "HTML/CSS thuần túy không tạo hình AI".
>
> ⚖️ **Nhưng hãy nhớ kỹ định vị**: Đây là **"đạn dược lật xem khi không có ý tưởng", không phải danh mục "bắt buộc phải chọn từ đây"**. Người dùng đưa ra nội dung/thương hiệu/tham khảo, thiết kế sẽ triển khai từ đó, đừng áp đặt thư viện. Trách nhiệm của skill là giúp người dùng né tránh cái tồi nhất, không phải quy định thiết kế tốt trông như thế nào——thiết kế tốt lớn lên từ nhu cầu thực tế của người dùng.

## Thư viện này sử dụng như thế nào

1. **Chọn phân khu theo loại đầu ra trước (Chọn 1 trong 3, không phải chọn 1 trong 2)**: Làm web/landing page/website chính thức → Web 20 loại; Làm PPT/deck/thuyết trình → PPT 20 loại; Làm Infographic/Trực quan hóa dữ liệu/Bức hình dài đơn lẻ → Infographic 20 loại.
   - Tiêu chí là **hình thái thành phẩm chứ không phải đề tài**: Site có thể click đi theo khu Web, cần lật trang đi theo khu PPT, **một hình (hoặc một nhóm hình) lấy dữ liệu làm nhân vật chính, có thể thoát khỏi tương tác đọc độc lập đi theo khu Infographic**.
   - Hai trường hợp thường gặp không chắc chắn: Prototype Dashboard đi theo khu Web (Nó là giao diện sản phẩm); Trang dữ liệu nhúng trong một trang deck vẫn đi theo khu PPT (Nó cần lật trang).
2. **Hệ thống nhiệt độ**: Mỗi loại đều đánh dấu `Táo bạo / Trung tính / Trầm tĩnh`. **Cố tình để các mẫu táo bạo chiếm đa số**——Độ lệch xác định của model tự nhiên thiên về trầm tĩnh cực giản, tỷ lệ phối hợp của thư viện phải đẩy nó về phía táo bạo.
   - Hướng A (Đáy chắc chắn) chọn từ trầm tĩnh/trung tính theo nhu cầu; Hướng B lấy nhiệt độ khác nhau tạo sự tương phản; **Hướng C do "Vòng xoay số giây" của SKILL cưỡng chế bơm vào mẫu táo bạo**.
   - ❌ Ba hướng đừng đều rơi vào "Kem trắng + Khoảng trắng + Một màu điểm xuyết"——Đó là mẫu hình thất bại thường gặp nhất.
3. **Độ phục hồi**: ≥90% nhắm mắt làm; 70-90% chủ thể có thể làm, một vài chi tiết hạ cấp; <70% (Như Memphis texture làm cũ) bắt buộc phải **ghi rõ trong thành phẩm phần nào dùng khối màu thuần để hạ cấp**, không giả vờ làm ra được chất lượng bản gốc.
4. **Font chữ**: Mỗi loại đều đưa ra phương án thay thế mã nguồn mở (Inter/Geist/Manrope/Space Grotesk/Fraunces/Playfair v.v.), đừng viết font chữ trả phí (Söhne/Circular v.v.).
5. Phối hợp: SKILL "Cố vấn hướng thiết kế" Phase 3-5 dùng thư viện này suy ra 3 hướng; `assets/showcases/` có gallery ảnh chụp màn hình dựng sẵn.

---

## Giao thức Suy luận Màu sắc (Đi qua 3 bước này trước khi dùng bất kỳ phong cách nào)

> ⚠️ **Tất cả các hex trong các điều khoản phong cách dưới đây là điểm neo ví dụ, không phải công thức.** Cùng một phong cách dùng cho nội dung khác nhau, nên suy luận ra các giá trị màu khác nhau thông qua giao thức này——sao chép trực tiếp hex trong điều khoản chỉ là đang sản xuất slop có gu tốt hơn. Tại sao: Viết chết công thức làm 100 người dùng nhận được 100 thành phẩm cùng màu, lượng thông tin màu sắc trở về 0; Suy luận làm cho màu sắc trở thành bằng chứng "độc nhất của nội dung này".
>
> **Font chữ cũng tương tự**: Tên font chữ trong các điều khoản cũng là điểm neo ví dụ. Sau khi chọn xong phong cách, phối hợp display+body qua logic phối hợp của `references/typography.md` và "Danh sách đã bị dùng nát" trước——**Khi danh sách xung đột với điều khoản lấy typography.md làm chuẩn** (Như điều khoản viết Fraunces, theo danh sách đổi sang Newsreader làm bản thay thế tương đương).

### Phương pháp 3 bước: Lấy mẫu → Thu hẹp → Luận chứng

| Bước | Làm gì | Tại sao |
|------|--------|--------|
| **1. Lấy mẫu** | Màu chính lấy từ 3 nguồn, không tự nhiên bịa ra: ①Tài sản thương hiệu (logo/VI hiện có hút màu trực tiếp) ②Hình ảnh thực tế nội dung (Màu chủ đạo trong ảnh chụp sản phẩm/vật liệu nhiếp ảnh) ③Ngữ cảnh văn hóa (Ký ức màu sắc có sẵn của chủ đề nội dung, xem bảng dưới) | Tự nhiên chọn màu=Rút thăm từ tiền đề của model, rút ra mãi mãi là mấy màu mạng nổi tiếng; Màu lấy từ nội dung tự nhiên mang theo chữ "Tại sao" |
| **2. Thu hẹp** | Dùng oklch nén bảng màu xuống **2-3 màu có sắc + 1 nhóm màu trung tính**. Màu trung tính viết thành chuỗi độ sáng (Như L 0.15/0.35/0.65/0.92/0.98), giữa các màu có sắc kéo giãn góc sắc độ oklch H ≥60° hoặc chênh lệch độ sáng L ≥0.3 | Màu nhiều tất loạn; Kênh L của oklch cảm nhận đều đặn, chuỗi độ sáng viết ra chính là hệ thống cấp độ, dễ suy luận hơn một đống hex cô lập |
| **3. Luận chứng** | Viết ra một câu "Tại sao lại là màu này", viết vào ghi chú thành phẩm hoặc thuyết minh giao hàng. Ví dụ: "Màu chính lấy từ màu đỏ thổ hoàng trên logo của người dùng, nén chroma xuống 0.08 để mô phỏng mực in" | **Không viết ra được câu này=Bạn đang chép công thức.** Luận chứng là cửa tự kiểm tra chống slop, không phải nghi thức |

### Chất lượng màu in: Tại sao độ bão hòa thấp cao cấp hơn màu màn hình thuần túy

Mực in trên giấy không bao giờ đạt được độ bão hòa tối đa của RGB màn hình——Không gian màu CMYK hẹp hơn, giấy hút mực, ánh sáng môi trường phản xạ, đều sẽ "nén xám" màu sắc. "Cảm giác cao cấp" mà mắt người được huấn luyện bởi ấn phẩm trong mấy chục năm qua, bản chất là lớp độ xám vật lý này. Cho nên trong thiết kế màn hình cố tình nén chroma, tương đương với việc mượn ký ức chất lượng của in ấn.

| Mục đích | Tham khảo oklch chroma | Hiệu quả |
|------|------------------|------|
| Màu nền diện tích lớn | 0.01–0.04 | Cảm giác giấy, không chói mắt |
| Màu chính thương hiệu/Nhấn mạnh | 0.08–0.15 | Cảm giác mực in, đủ bắt mắt nhưng không nhựa |
| Điểm xuyết diện tích nhỏ (Button/Link) | 0.15–0.22 | Giữ lại sức sống, chỉ giới hạn diện tích nhỏ |
| >0.25 Trải toàn bản | Thận trọng | Cảm giác huỳnh quang màn hình, chỉ hợp phong cách cố tình "nguyên bản điện tử" như Wrapped/Candy |

### Tra nhanh Ngữ cảnh Văn hóa: Cùng một sắc độ, Ngữ cảnh khác nhau

Chọn màu không chỉ là chọn sắc độ, mà là chọn tọa độ văn hóa phía sau nó. Cùng là "Đỏ", điểm rơi khác nhau một trời một vực:

| Sắc độ | Ngữ cảnh A | Ngữ cảnh B | Khác ở đâu |
|------|--------|--------|--------|
| Đỏ | Đỏ chu sa Cố Cung (Thên cam, mang xám, oklch L thấp C thấp, tối hơn và đục hơn đỏ Coca) → Truyền thống/Trang trọng | Đỏ Coca (Đỏ tươi bão hòa cao) → Tiêu dùng/Hưng phấn | chroma vừa giảm, từ kệ hàng nhảy lên tường cung điện |
| Xanh dương | Xanh nhuộm Nhật Bản/Lưu ly cảm (Thâm, thên tím xám) → Thủ công/Trầm tĩnh | Xanh công nghệ hệ #0066FF → SaaS/Hiệu suất | Cái sau là xanh mặc định yêu thích nhất của model, trước khi dùng hãy tự hỏi bản thân có phải đang rút thăm không |
| Xanh lá | Matcha/Xanh rêu (Thên vàng, bão hòa thấp) → Tự nhiên/Kiểu Nhật | Xanh huỳnh quang #39FF14 → Terminal/hacker | Cùng là xanh, một cái uống trà một cái gõ code |
| Vàng | Đằng hoàng/Mù tạt (Mang nâu xám) → In ấn hoài cổ | Vàng cảnh báo/Vàng Mailchimp → Bắt mắt/Vui nhộn | Độ xám quyết định nó là trang sách cũ hay mũ bảo hộ |
| Trắng | Trắng giấy kem #F5F0E8 → Ấn phẩm/Ấm | Trắng thuần #FFF → Phòng thí nghiệm/Thụy Sĩ | Chênh lệch nhiệt độ màu 2% của màu nền chính là phân định khí chất |

---

## Thư viện Phong cách Web (20 loại)

#### Phái Táo bạo

**Editorial Brutalism Chủ nghĩa Thô mộc Báo chí (Chữ Helvetica khổng lồ đè chữ chính nhỏ)** `Táo bạo · Phục hồi 98%`
- Tham khảo: Bloomberg Businessweek (Richard Turley 2010-2014 cải bản, Code and Theory đạo diễn); Hệ Neue Haas Grotesk
- Thích ứng: Báo chí/Xuất bản nội dung, Ra mắt sản phẩm AI, Hero trang web chính thức thương hiệu, Bìa báo cáo khảo sát, Hình đầu bài viết quan điểm
- DNA thị giác: Phối màu đen thuần #000+trắng thuần #FFF+xanh siêu liên kết #0000EE, điểm xuyết cam đỏ tín hiệu #FF433D/xanh terminal #00A33E. Font chữ Helvetica/Neue Haas Grotesk, headline cực lớn 120px+ căn trái khoảng cách chữ chặt đè trực tiếp lên chữ chính nhỏ 14px, tương phản cỡ chữ cực đoan. Bố cục grid mô đun hóa+đường phân chia 1px cắt cột, mật độ thông tin cao cố tình không để trống. Phần tử biểu tượng: rule line phân cột, gạch chân xanh siêu liên kết, khối màu lớn đáy đen trắng.
- Thực thi HTML: CSS thuần có thể phục hồi 1:1. CSS Grid làm grid mô đun+border làm đường phân chia cột, clamp() làm cỡ chữ responsive cực lớn+letter-spacing thu chặt, font hệ thống Helvetica/Arial hoặc Inter dự phòng, siêu liên kết gạch chân trực tiếp #0000EE. Không phụ thuộc vật liệu.
- Font chữ: Inter (Thay Helvetica/Neue Haas Grotesk), Code dùng Geist Mono

**Neo-Brutalism Chủ nghĩa Thô mộc Mới Va chạm màu Feed thông tin (Card viền đen dày+Màu va chạm bão hòa cao)** `Táo bạo · Phục hồi 95%`
- Tham khảo: The Verge 2022 redesign (in-house team, PolySans + Mānuka)
- Thích ứng: Truyền thông/Trang nội dung, Trang gom sản phẩm AI, Landing sự kiện, Trang bảng xếp hạng cộng đồng, Card thông tin phong cách Tiểu Hồng Thư
- DNA thị giác: Phối màu tím điện #5200FF~đỏ sẫm #E1306C màu chính bão hòa cao+vàng sáng #F8E000 nhấn mạnh+đen thuần #08080D+trắng, khối màu va chạm diện tích lớn cố tình không mềm mại. Font chữ tương phản giữa tiêu đề không nét chân hình học+chữ chính có nét chân. Bố cục feed card hóa, viền đen dày 2-4px, phân khu khối màu cứng, hầu như không bo góc. Phần tử biểu tượng: Card viền dày hover đổi màu lật, khí chất giao diện chưa hoàn thành.
- Thực thi HTML: Điểm mạnh của CSS thuần. border:3px solid #000 viền dày+box-shadow bóng cứng lệch (4px 4px 0 #000)+grid/flex luồng card+:hover chuyển đổi background đổi màu lật. Không có rào cản 3D/Quang ảnh.
- Font chữ: Space Grotesk (Thay PolySans) + Bất kỳ font serif nào như Fraunces

**Memphis Maximalism Tối đa hóa Cắt dán Hoài cổ Memphis (Khối màu va chạm+Xếp chồng lệch vị trí+Font chữ hoài cổ)** `Táo bạo · Phục hồi 72%`
- Tham khảo: Gucci Vault concept store (Alessandro Michele); Phong trào thiết kế Memphis / Gen nổi loạn Sagmeister
- Thích ứng: Concept store thương mại điện tử, Trang sự kiện sáng tạo, Campaign thử nghiệm thương hiệu, Chủ đề Y2K hoài cổ, Trang marketing lễ hội
- DNA thị giác: Phối màu đỏ hoài cổ/vàng mù tạt/xanh bảo thạch/tím/xanh ô liu va chạm diện tích lớn đặt cạnh nhau+nền ấm kem làm cũ, đậm đà cố tình không hài hòa. Font chữ pha trộn giữa serif hoài cổ+chữ trang trí, chất lượng in ấn, phá vỡ grid xếp chồng lệch vị trí. Bố cục bài trí cắt dán ngược grid, mô đun lớn nhỏ không đều xếp chồng đè lên nhau, giống như đi dạo phòng kỹ thuật số. Phần tử biểu tượng: Khối màu va chạm, xếp chồng lệch vị trí, trứng phục sinh điều hướng không quy chuẩn.
- Thực thi HTML: transform:rotate() làm xếp chồng lệch vị trí+position:absolute đè lên nhau+khối màu background bão hòa cao va chạm+Google Fonts hoài cổ. Texture làm cũ thực tế không thể phục hồi bằng CSS, hạ cấp thành khối màu thuần+mix-blend-mode/contrast filter mô phỏng vân bề mặt, bản cắt dán hình học thành lập, bản archival làm cũ sẽ bị hạ cấp.
- Font chữ: DM Serif Display + Bungee (Trang trí) + Space Mono

**Friendly Geometric Candy Nút nổi 3D màu kẹo Game hóa** `Táo bạo · Phục hồi 85%`
- Tham khảo: Duolingo (Johnson Banks + Monotype, font Feather Bold); Cực giản ngược Thung lũng Silicon
- Thích ứng: Học ngôn ngữ giáo dục, Landing App cấp tiêu dùng, Sản phẩm game hóa, Sản phẩm thân thiện hướng đại chúng, Trang đăng ký sự kiện
- DNA thị giác: Phối màu Duo xanh #58CC02+vàng vịt #FFC800+xanh da trời #1CB0F6 kẹo bão hòa cao+nền trắng, bo tròn thân thiện. Font chữ siêu dày bo tròn (Cảm giác Feather Bold). Bố cục card bo góc lớn, nút 3D nổi lên (Bóng cứng phía dưới=Cảm giác có thể ấn), vị trí linh vật+bóng tiến độ. Phần tử biểu tượng: Nút nổi 3D bóng đáy thực 3px, animation dịch chuyển khi ấn xuống, siêu bo góc.
- Thực thi HTML: CSS thuần. box-shadow:0 4px 0 bóng đáy cứng làm nút nổi+:active translateY(4px) triệt tiêu bóng mô phỏng ấn xuống, border-radius bo góc lớn, khối màu thuần. Khi không có linh vật dùng hình học CSS hoặc emoji làm vị trí (Hạ cấp nhẹ).
- Font chữ: Baloo 2 / Nunito (Font bo tròn siêu dày thay Feather)

**Pure-CSS Art Minh họa Hình học thuần CSS+Trứng phục sinh Biến dạng Responsive** `Táo bạo · Phục hồi 80%`
- Tham khảo: Lynn Fisher (lynnandtonic.com, huyền thoại nghệ thuật CSS thuần, Adobe có bài viết chuyên đề)
- Thích ứng: Trang cá nhân, Trang 404/trứng phục sinh sáng tạo, Landing thương hiệu vui nhộn, Hình đầu bài blog kỹ thuật, Hiển thị bản thân của nhà thiết kế
- DNA thị giác: Phối màu 2-4 màu mặt phẳng tương phản cao (Mỗi breakpoint đổi một bộ màu). Font chữ tiêu đề không nét chân hình học dày. Bố cục cốt lõi là "Hình ảnh biến dạng theo viewport"——Một nhóm hình dạng CSS tái cấu trúc thành các hình ảnh khác nhau tại các breakpoint khác nhau (Như tòa nhà đổi số tầng theo chiều rộng màn hình). Phần tử biểu tượng: Minh họa hình học vẽ bằng CSS thuần, trứng phục sinh tái sắp xếp điều khiển bởi breakpoint, 0 hình ảnh.
- Thực thi HTML: Chiến trường khoe kỹ năng của CSS thuần, 0 vật liệu là ưu thế. div+border-radius/clip-path/transform/box-shadow xếp chồng hình hình học, @media breakpoint thay đổi vị trí kích thước hình dạng thực hiện biến dạng. Độ khó ở ý tưởng thiết kế chứ không phải kỹ thuật, nhưng cần thủ công tỉ mỉ từng hình dạng.
- Font chữ: Rubik / Archivo (Hình học dày thay tùy chỉnh)

**Bold Big-Type Editorial Báo chữ lớn Thời trang Khổng lồ Tương phản cao Đen Trắng** `Táo bạo · Phục hồi 88%`
- Tham khảo: Jacquemus official site / Rik Oostenbroek / Domestika; Báo chữ lớn tạp chí thời trang
- Thích ứng: Thương mại điện tử thời trang, Portfolio, Chuyên đề truyền thông, Trang tuyên ngôn thương hiệu, Bìa khóa học video, Bản chữ lớn báo cáo khảo sát
- DNA thị giác: Phối màu đen trắng cực giản+màu điểm xuyết kiềm chế duy nhất (Hồng nude #E8C4C0 hoặc đỏ tươi). Font chữ Display không nét chân khổng lồ/Serif tương phản cao, tiêu đề chiếm trọn màn hình. Bố cục grid toàn khung, chữ khổng lồ đấu trí với khoảng trắng, chia 1:1 hình và chữ. Phần tử biểu tượng: Headline khổng lồ tỷ lệ chiếm màn hình, khoảng trắng cấp xa xỉ, trình bày đối vị trái phải.
- Thực thi HTML: CSS thuần phục hồi hoàn hảo. clamp() chữ khổng lồ+CSS Grid phân chia toàn khung+lượng lớn padding khoảng trắng+đơn vị vh làm tiêu đề chiếm trọn viewport. Khi không có hình dùng khối màu thuần/khối chữ thay thế vị trí hình ảnh thời trang (Hạ cấp nhẹ nhưng bố cục thành lập).
- Font chữ: Archivo Expanded / Anton (Display) + Playfair Display (Serif tương phản cao)

**Cosmic Retro-Futurism Catalogue Vũ trụ Viễn tưởng Hoài cổ** `Táo bạo · Phục hồi 75%`
- Tham khảo: Trang ra mắt trình duyệt Perplexity Comet (The Brand Identity: Black/Blue/Cream; Khí chất 《2001: A Space Odyssey》)
- Thích ứng: Trang ra mắt sản phẩm AI, Trang tuyên ngôn thương hiệu công nghệ, Trang đếm ngược sự kiện, Landing cảm giác tương lai, Hội nghị ra mắt concept
- DNA thị giác: Phối màu đen thuần #0A0A0A+trắng kem giấy cream #F0EAD8+một vệt xanh cô-ban-xanh khổng quạt #2B4F91, bão hòa thấp giống như catalogue thiên văn cổ. Font chữ Serif tương phản cao (Cảm giác sách thiên văn cổ)+khoảng trắng. Bố cục đường quỹ đạo/parabol SVG vẽ nét, chấm tròn hành tinh, nền kem đè chữ đen, trình bày chữ kiểu sách cổ. Phần tử biểu tượng: Đường quỹ đạo thiên văn SVG, ba màu kem+xanh+đen, chữ lớn Serif hoài cổ, chất lượng catalogue thiên văn.
- Thực thi HTML: CSS thuần+SVG phục hồi 80% khí chất tĩnh. SVG path vẽ quỹ đạo parabol+CSS định vị tâm đường hành tinh+biến 3 màu+Serif tương phản cao. Thiếu sót là chuyển cảnh video toàn màn hình "Từ vũ trụ đáp xuống trái đất" (Phần linh hồn)——hạ cấp thành CSS scroll thị sai+SVG quỹ đạo xoay xấp xỉ.
- Font chữ: Cormorant Garamond / EB Garamond (Serif tương phản cao) + Space Mono

**Cinematic Sound-Viz Dark Tuần tra 3D Âm thanh Cảm giác Điện ảnh** `Táo bạo · Phục hồi 72%`
- Tham khảo: ElevenLabs; Title sequence phim điện ảnh (Cực giản động thái kiểu Saul Bass) × Giao diện kỹ thuật âm thanh
- Thích ứng: Sản phẩm AI âm thanh/giọng nói, Trang công cụ âm nhạc, Nền tảng podcast, Trang phát hành truyền thông, Hero thương hiệu cấp rạp chiếu phim
- DNA thị giác: Phối màu nền đen thuần #000+chữ trắng thuần+dải sóng accent chuyển sắc xanh tím. Font chữ tiêu đề không nét chân lớn cực giản kiểu Saul Bass. Bố cục toàn khung trường tối, trực quan hóa sóng âm/phổ tần chạy xuyên suốt, tiêu đề khổng lồ đè sóng âm, khu vực tính năng card. Phần tử biểu tượng: Dải sóng audio-waveform nhiều màu, cực giản kiểu title sequence phim, đen trắng tương phản cao+chuyển sắc đơn, chủ đề trực quan hóa âm thanh.
- Thực thi HTML: CSS thuần+SVG phục hồi 70% khí chất (Khung xương hoàn hảo, dải sóng là điểm hạ cấp). SVG polyline vẽ dải sóng tĩnh hoặc mảng cột div không bằng nhau+CSS animation làm "dải sóng giả" nhảy mỏng xấp xỉ. Thiếu sót: Phổ tần Web Audio/Canvas thời gian thực nhảy theo âm thanh không thể phục hồi bằng CSS thuần, bản tĩnh thì giống, linh hồn động thái không trả lại được.
- Font chữ: Inter / Sora (Không nét chân lớn)

**Pixel-Game Side-Scroller Tự sự Cuộn ngang Game Pixel** `Táo bạo · Phục hồi 70%`
- Tham khảo: Sơ yếu lý lịch tương tác Robby Leonardi (Tự sự game hành động 8/16-bit, tri ân Nintendo SNES)
- Thích ứng: Sơ yếu lý lịch/Portfolio sáng tạo, Campaign vui nhộn thương hiệu, Landing game hóa, Trang trứng phục sinh sự kiện, Trang cá nhân thú vị
- DNA thị giác: Phối màu game hoài cổ nhiều đoạn phân khu——Xanh rừng #4CAF50 thảm cỏ+Xanh da trời #5DADE2, chuyển tiếp Tím vũ trụ #2C2A4A, Đỏ cam núi lửa #E8743B, Xanh ngọc đáy biển #1ABC9C, mỗi "phụ bản" đổi một bộ phối màu bão hòa cao hoạt hình. Font chữ Pixel (Cảm giác 8-bit)+Không nét chân dày. Bố cục cuộn ngang/dọc phân khu màn chơi, thị sai phân lớp, scroll kích hoạt dịch chuyển. Phần tử biểu tượng: Đổi màu theo màn chơi, mỹ học pixel, cuộn thị sai, UI kiểu HUD game.
- Thực thi HTML: CSS thuần+một lượng nhỏ JS phục hồi khung xương (Bản gốc chính là HTML+CSS+jQuery không có WebGL). Thị sai phân lớp position+scroll dịch chuyển, image-rendering:pixelated, CSS animation background-position từng khung hình làm sprite, đổi màu nền theo đoạn. Thiếu sót: Minh họa pixel thủ công nhân vật/bối cảnh gốc——khi không có tạo hình AI dùng khối vuông CSS ghép icon pixel đơn giản thay thế (Hạ cấp mỹ thuật, kỹ thuật không hạ).
- Font chữ: Press Start 2P / VT323 (Chữ pixel) + Inter


#### Phái Trung tính

**Bauhaus Geometric Biểu tượng Hình học Bauhaus+Hệ thống Minh họa Phẳng** `Trung tính · Phục hồi 90%`
- Tham khảo: Khan Academy rebrand (Hình lục giác+logomark cánh hoa + Hệ thống thiết kế Wonder Blocks); Cấu thành hình học Bauhaus
- Thích ứng: Trang khóa học giáo dục, Hệ thống logo thương hiệu, Infographic, Sản phẩm hướng tới trẻ em thân thiện, KV sự kiện
- DNA thị giác: Phối màu hệ ba màu gốc——Đỏ Bauhaus #E63946/Vàng #FFB703/Xanh #0077B6+đen trắng, khối màu thuần ghép nối. Font chữ không nét chân hình học (Cảm giác hình học bo tròn). Bố cục đơn vị hình học cơ bản tròn/tam giác/vuông dựng minh họa, đối chiếu grid, ghép hình mô đun hóa. Phần tử biểu tượng: Logomark hình thái hình học thuần túy, minh họa phẳng không chuyển sắc, cấu thành khối màu gốc.
- Thực thi HTML: CSS hình học toàn năng. border-radius:50% làm hình tròn, clip-path/border tam giác, div khối vuông ghép minh họa hình học, CSS Grid đối chiếu grid, fill màu thuần không cần vật liệu. Minh họa dùng hình dạng CSS hoặc SVG path hình học inline tự vẽ.
- Font chữ: Poppins / Manrope (Hình học bo tròn thay Wonder Blocks)

**Dark Editorial Portfolio Lập trình viên Thanh bên Hai màu Tối (Nền tối+Accent huỳnh quang đơn+Chữ bằng nhau)** `Trung tính · Phục hồi 96%`
- Tham khảo: Brittany Chiang (brittanychiang.com v4, tiêu chuẩn thực tế dev portfolio)
- Thích ứng: Trang cá nhân portfolio, Sản phẩm hướng tới developer, Trang thương hiệu kỹ thuật, Trang sơ yếu lý lịch, Landing công cụ AI
- DNA thị giác: Phối màu nền xanh mực sâu/hải quân #0A192F+văn bản xám đá #8892B0+accent xanh ngọc huỳnh quang duy nhất #64FFDA. Font chữ không nét chân cho chữ chính+chữ bằng nhau (Số hiệu/Nhãn). Bố cục thanh điều hướng cố định bên trái+khu vực chính cuộn bên phải hai cột, số hiệu section 01/02, hover link gạch chân trượt vào. Phần tử biểu tượng: Màu accent đơn, nhãn số hiệu bằng nhau, mỏ neo thanh bên highlight.
- Thực thi HTML: CSS thuần phục hồi hoàn toàn. position:sticky làm thanh bên cố định+CSS Grid hai cột+biến accent đơn+font chữ bằng nhau nhãn+:hover gạch chân transform trượt vào. 0 vật liệu, thuần bố cục và vi tương tác.
- Font chữ: Inter + JetBrains Mono (Bằng nhau)

**Warm Editorial Ấn phẩm Màu ấm (Nền giấy kem+Cam đất nung+Pha trộn Serif và Sans-serif)** `Trung tính · Phục hồi 97%`
- Tham khảo: Anthropic / Claude (DBCo + Geist Studio, Styrene×Tiempos); Trình bày chữ sách bỏ túi Penguin/Pelican
- Thích ứng: Trang sản phẩm AI, Trang web chính thức thương hiệu, Trang đọc văn bản dài, Sách điện tử cam, Báo cáo khảo sát, Tài liệu đào tạo
- DNA thị giác: Phối màu nền giấy kem #F5F0E8+cam đất nung #CC785C/#D97757 điểm xuyết+văn bản gần như đen #191919, bão hòa thấp ấm áp. Font chữ tiêu đề Serif (Cảm giác Tiempos)×Chữ chính Sans-serif (Cảm giác Styrene) pha trộn. Bố cục luồng đọc đơn cột kiểu sách, chiều cao dòng thoải mái, đường phân cách kiềm chế. Phần tử biểu tượng: Nền ấm cảm giác giấy, cam đất nung, nhịp điệu trình bày chữ cấp xuất bản.
- Thực thi HTML: CSS thuần 100% phục hồi, 0 vật liệu. Biến màu nền+font stack Serif Sans-serif pha trộn+max-width giới hạn chiều rộng đọc+line-height 1.7 chiều cao dòng thoải mái. Đây là sân nhà an toàn của bản màu ấm cam đất nung Anthropic.
- Font chữ: Fraunces / Newsreader (Thay Tiempos serif) + Inter (Thay Styrene)

**Glassmorphism Bento Nổi phát sáng Tối Linear** `Trung tính · Phục hồi 85%`
- Tham khảo: Linear / Cursor (Hiện tượng 'The Linear Look', Frontend Horse có công thức code)
- Thích ứng: Trang sản phẩm SaaS/AI, Công cụ developer, Hero thương hiệu kỹ thuật, Hiển thị tính năng sản phẩm, Demo dashboard màu tối
- DNA thị giác: Phối màu nền gần như đen #08090A+thương hiệu xanh tím giảm bão hòa #5E6AD2+chuyển sắc ánh nhẹ xanh tím bão hòa thấp #4EA7FC→#B59AFF. Font chữ không nét chân hình học khoảng cách chữ âm thu chặt. Bố cục chia khối grid bento hộp cơm, đường phân chia sợi tóc, card thủy tinh mờ. Phần tử biểu tượng: Viền chuyển sắc phát sáng nền tối, chia khối bento, luồng sáng streamer, thủy tinh mờ.
- Thực thi HTML: CSS thuần phục hồi mạnh. box-shadow/filter blur+radial-gradient làm quầng sáng, backdrop-filter:blur thủy tinh mờ, conic/linear-gradient viền, CSS Grid ghép bento. Thiếu sót chỉ ở "Ảnh chụp UI sản phẩm thực tế"——dùng khối màu+chữ ghép UI giả tinh giản thay thế (Phần này hạ cấp).
- Font chữ: Inter / Geist (Khoảng cách chữ âm) + Geist Mono

**Angled Fluid Gradient Dải chuyển sắc Dòng chảy Cắt nghiêng** `Trung tính · Phục hồi 92%`
- Tham khảo: Stripe (Dải banner angled gradient biểu tượng, font Söhne tùy chỉnh của Klim)
- Thích ứng: Landing page SaaS/Fintech, Hero trang web chính thức thương hiệu, Trang ra mắt sản phẩm, Banner sự kiện, Trang marketing sản phẩm AI
- DNA thị giác: Phối màu chuyển sắc dòng chảy nhiều màu (Xanh chàm #635BFF→Xanh ngọc→Hồng→Tông ấm cam) làm nền hero+khu vực nội dung trắng thuần+văn bản gần như đen. Font chữ không nét chân tinh tế (Cảm giác Söhne). Bố cục phân khu cắt nghiêng (Phân khu góc cắt skew), hero chuyển sắc đè grid正文 cấu trúc hóa. Phần tử biểu tượng: Ranh giới cắt nghiêng angled, chuyển sắc dòng chảy nhiều màu, grid lý tính đè chuyển sắc diễn đạt.
- Thực thi HTML: CSS thuần. transform:skewY() hoặc clip-path:polygon() làm phân khu cắt nghiêng, linear-gradient đè nhiều màu (Có thể thêm CSS animation chảy chậm) làm dải chuyển sắc dòng chảy, Grid làm văn bản chính cấu trúc hóa bên dưới. 0 vật liệu.
- Font chữ: Inter / Hanken Grotesk (Thay Söhne)

**Utility-First Colorful Docs Tài liệu Phân loại Cầu phồng Thực dụng** `Trung tính · Phục hồi 98%`
- Tham khảo: Tailwind CSS Docs (Màu thương hiệu Sky/Cyan+Dải sắc độ phân loại tính năng cầu vồng)
- Thích ứng: Tài liệu kỹ thuật, API reference, Trang hệ thống thiết kế, Trang hướng dẫn, Knowledge base cho developer, Trung tâm hỗ trợ SaaS
- DNA thị giác: Phối màu Sky xanh #38BDF8 thương hiệu+teal→cyan→sky chuyển sắc xanh ngọc+Thang xám Slate #0F172A/#64748B/#F8FAFC, tài liệu dùng dải sắc độ cầu vồng phân biệt loại tính năng (Hồng #EC4899/Tím #A855F7/Xanh lá #10B981/Cam). Font chữ không nét chân thanh thoát+code bằng nhau. Bố cục thanh điều hướng bên trái+chữ chính ở giữa+TOC bên phải ba cột, khối code highlight màu, nhãn màu phân loại. Phần tử biểu tượng: Hero chuyển sắc xanh ngọc, màu phân loại cầu vồng, khung ba cột tài liệu, khối code tô màu cú pháp.
- Thực thi HTML: CSS thuần phục hồi 98% (Bản thân nó chính là tài liệu CSS framework). Grid ba cột+linear-gradient hero xanh ngọc+biến màu phân loại+tô màu cú pháp khối code dùng span. Inter mã nguồn mở, chỉ có chuyển đổi sáng tối/copy cần JS nhẹ. 0 quang ảnh/3D/vẽ tay.
- Font chữ: Inter + JetBrains Mono / Fira Code (Code)

**Terminal-Core Soft-Futurism Tương lai Mềm Cốt lõi Terminal (Chữ bằng nhau+Hình lập phương đẳng hướng)** `Trung tính · Phục hồi 80%`
- Tham khảo: Cursor (Anysphere); Mỹ học terminal developer × Cực giản công nghiệp Teenage Engineering
- Thích ứng: Trang công cụ lập trình AI, Landing sản phẩm CLI, Cơ sở hạ tầng developer, Hero thương hiệu kỹ thuật, Sản phẩm loại terminal
- DNA thị giác: Phối màu nền đen than #0B0D14+văn bản trắng ấm #F2F0EF+nhấn mạnh chuyển sắc xanh tím kiềm chế điểm xuyết nút và quầng sáng. Font chữ lấy chữ bằng nhau làm nhân vật chính (Cảm giác dòng lệnh)+không nét chân hỗ trợ. Bố cục tiền cảnh dòng lệnh/khối code, phân khu bento, minh họa cube đẳng hướng 2.5D. Phần tử biểu tượng: Dòng lệnh chữ bằng nhau, hình lập phương chiếu đẳng hướng, trắng ấm×đen than, quầng sáng chuyển sắc kiềm chế, cực giản công nghiệp.
- Thực thi HTML: CSS thuần phục hồi 80%. Khối code chữ bằng nhau+bento màu tối+box-shadow quầng sáng; cube đẳng hướng 2.5D dùng CSS 3D transform(rotateX/Y+skew) hoặc SVG chiếu đẳng hướng tự vẽ. Thiếu sót: Demo nhiều giao diện cần click chuyển đổi cần JS+ghép UI giả. Không có yêu cầu cứng WebGL.
- Font chữ: Geist Mono / JetBrains Mono (Nhân vật chính) + Inter (Hỗ trợ)


#### Phái Trầm tĩnh

**Functional Brutalism Cộng đồng Grid Tính năng (Chia đường xám+Font hệ thống+Link xanh)** `Trầm tĩnh · Phục hồi 98%`
- Tham khảo: Are.na / Lobsters / Quartz; Trực quan hóa grid Müller-Brockmann + Mật độ thông tin Tufte
- Thích ứng: Cộng đồng/Nền tảng UGC, Trang gom nội dung, Kho kiến thức tài liệu, Luồng nội dung ưu tiên mobile, Sản phẩm hướng tới geek
- DNA thị giác: Phối màu nền gần như trắng #FBFBFB+văn bản đen+đường phân cách xám 1px #E0E0E0+link cổ điển xanh #0000EE/đã truy cập tím. Font chữ hệ thống (-apple-system/Không trang trí). Bố cục danh sách thông tin mật độ cao, phân cột đường xám mảnh, khoảng trắng cực nhỏ, khoảng cách dòng thu chặt. Phần tử biểu tượng: Đường phân cách xám sợi tóc, link xanh, font hệ thống, ưu tiên mật độ thông tin.
- Thực thi HTML: CSS thuần dễ phục hồi nhất, đây là bản sắc của Brutalist Web. border-bottom:1px danh sách đường xám+font stack system-ui+padding thu chặt+link xanh. Hầu như không cần bất kỳ vật liệu hoặc JS nào, thuần cấu trúc.
- Font chữ: Font stack system-ui / IBM Plex Sans (Dự phòng)

**Gallery Dark Đóng khung Phòng tranh Tối (Khoảng trắng âm đen sâu+Hình lớn đơn cột+Chữ nhỏ EXIF)** `Trầm tĩnh · Phục hồi 75%`
- Tham khảo: Glass (glass.photo) / Bottega Veneta；Phòng tối bảo tàng mỹ thuật + Apple Photos nội dung là trên hết
- Thích ứng: Portfolio nhiếp ảnh, Thương mại điện tử hàng xa xỉ, Hiển thị đắm chìm nội dung thị giác, Trang phòng tranh cá nhân, Trưng bày sản phẩm cao cấp
- DNA thị giác: Phối màu nền đen thuần #0A0A0A+bản thân tác phẩm cung cấp màu sắc duy nhất+chữ nhỏ EXIF xám cực nhạt #666. Font chữ nhỏ không nét chân cực mảnh. Bố cục hình lớn căn giữa đơn cột, đóng khung khoảng trắng âm siêu lớn, metadata chữ nhỏ dưới hình. Phần tử biểu tượng: Nền đen phòng tối, UI nội dung là trên hết ẩn đi, chú thích chữ nhỏ kiểu EXIF, hình lớn chiếm trọn viewport.
- Thực thi HTML: CSS thuần phục hồi khung xương bố cục. Nền đen thuần+căn giữa max-width đơn cột+padding lớn đóng khung khoảng trắng+metadata chữ nhỏ. Thiếu sót là bản thân "Tác phẩm nhiếp ảnh thực tế"——dùng hình giữ chỗ/khối màu thuần thay thế sẽ mất linh hồn, nhưng không khí phòng tối và bố cục 100% có thể dựng được.
- Font chữ: Inter (Độ đậm mảnh 300) / Cormorant (Serif cảm giác xa xỉ tùy chọn)

**Swiss Monochrome Đen Trắng Cực giản Thụy Sĩ (Đen trắng thuần kiểu Vercel+Geist+Góc cạnh sắc nét)** `Trầm tĩnh · Phục hồi 98%`
- Tham khảo: Vercel / Next.js Docs (Geist tự nghiên cứu đã mã nguồn mở); Massimo Vignelli ít tức là nhiều
- Thích ứng: Tài liệu công cụ developer, Trang web chính thức thương hiệu kỹ thuật, Trang sản phẩm AI, Landing page SaaS, Báo cáo khảo sát cực giản
- DNA thị giác: Phối màu đen thuần #000+trắng thuần #FFF+thang xám #888, 0 màu sắc hoặc chỉ một vệt link xanh. Font chữ Geist không nét chân hình học+Geist Mono. Bố cục góc vuông sắc nét (Không bo góc hoặc cực nhỏ), tương phản cao, grid chính xác, khoảng trắng kiềm chế. Phần tử biểu tượng: Đen trắng thuần, góc cạnh sắc nét, font chữ Geist, ký hiệu hình học tam giác/mũi tên.
- Thực thi HTML: CSS thuần 100% phục hồi, Geist mã nguồn mở có thể trích dẫn trực tiếp. CSS Grid grid chính xác+biến đen trắng thuần+border-radius:0 góc nhọn+viền sợi tóc. Đây là sân nhà cực giản thoải mái nhất của HTML, 0 phụ thuộc vật liệu.
- Font chữ: Geist + Geist Mono (Bản gốc mã nguồn mở Vercel)

**Kenya Hara White Gallery Phòng tranh Hộp trắng Khoảng trắng kiểu Nhật** `Trầm tĩnh · Phục hồi 80%`
- Tham khảo: Cosmos (cosmos.so) / Aesop official site; Cái tịch mịch của 'Trắng' Hara Kenya × Lai tạo grid Thụy Sĩ
- Thích ứng: Thương mại điện tử cao cấp, Phòng tranh sáng tạo, Nền tảng tuyển chọn nội dung, Portfolio nhà thiết kế, Cửa hàng thương hiệu tinh tế, Trang moodboard
- DNA thị giác: Phối màu nền gần như trắng hoàn toàn #FAFAFA+văn bản đen thuần #0A0A0A+phân cách xám cực nhạt #EFEFEF, hình ảnh nội dung cung cấp toàn bộ màu sắc, UI lùi về làm nền. Font chữ hệ thống cực giản/Không nét chân hình học chữ nhỏ, khoảng cách chữ lớn. Bố cục grid thác nước masonry, khoảng trắng cực hạn, phân cách sợi tóc xám nhạt, tịch mịch phương Đông. Phần tử biểu tượng: Mỹ học hộp trắng, khoảng trắng xa xỉ, nội dung là trên hết UI ẩn đi, tuyển chọn luồng thác nước.
- Thực thi HTML: CSS thuần phục hồi bố cục tĩnh (Phân biệt với phòng tranh màu tối ở chữ 'Trắng'). CSS columns hoặc Grid làm masonry+biến gần như trắng+padding lớn khoảng trắng+phân cách xám nhạt. Thiếu sót là cuộn quán tính mượt mà Lenis/GSAP và gia tốc vào cảnh của hình ảnh (60% cảm giác cao cấp ở đây), CSS chỉ có transition cơ bản, lớp động thái bị hạ cấp.
- Font chữ: Inter (Độ đậm mảnh) / Cooper Hewitt (Mã nguồn mở cùng loại Aesop)


## Thư viện Phong cách PPT (20 loại)

#### Phái Táo bạo

**Neo-Swiss Billboard Editorial Báo chữ lớn Thụy Sĩ Mới** `Táo bạo · Phục hồi 98%`
- Tham khảo: Dòng Big-Number Editorial của các deck gọi vốn AI/SaaS như Scribe $75M, Flock Safety $47M; Infographic Bloomberg Businessweek; Pentagram
- Thích ứng: Gọi vốn đầu tư, QBR/Review kinh doanh, Phục盘 xu hướng năm, Trang mấu chốt ra mắt sản phẩm
- DNA thị giác: Phối màu=Nền trắng thuần (#FFFFFF) hoặc gần như đen (#0A0A0A)+Màu nhấn mạnh bão hòa cao duy nhất (Xanh điện #2D5BFF/Xanh huỳnh quang #00E676/Cam thương hiệu #FF6B2C)+Đường grid trung tính #E5E5E5. Font chữ=Không nét chân dày cực lớn, tiêu đề chiếm nửa màn hình, con số tabular-nums bằng nhau thu chặt khoảng cách chữ. Master=①Trang chương khối màu lớn một từ ②Con số khổng lồ chiếm nửa màn hình (3.2x)+Ghi chú nhỏ ③Phân cột trái phải đối lập ④Biểu đồ đường/cột phẳng toàn khung. Biểu tượng=billboarding chữ lớn, grid đường cơ sở nghiêm ngặt, trang chương khối màu lớn
- Thực thi HTML: Con số siêu lớn dùng clamp(); Grid nghiêm ngặt dùng CSS Grid; Trang chương khối màu lớn background-color; Đường và cột dùng div thuần+CSS hoặc SVG inline (Sắc nét hơn dán hình); Con số căn chỉnh font-variant-numeric:tabular-nums. 0 minh họa 0 3D
- Font chữ: Inter / Geist / Söhne thay thế Neue Haas Grotesk; Con số phối Geist Mono

**Black Big-Number Stage Sân khấu Con số Khổng lồ Nền đen** `Táo bạo · Phục hồi 97%`
- Tham khảo: Steve Jobs 2007 iPhone Keynote, Hội nghị ra mắt Xiaomi SU7 Ultra Lê Quân, Spotify Wrapped, Presentation Zen (Garr Reynolds)
- Thích ứng: Diễn thuyết chủ đề ra mắt sản phẩm, Thuyết trình tư tưởng, Town hall toàn thể nhân viên, Phục盘 năm hướng tới cảm xúc
- DNA thị giác: Phối màu=Nền đen thuần #000000+Chữ trắng thuần #FFFFFF tương phản cao, một trang chỉ highlight một màu nhấn mạnh thương hiệu (Cam Xiaomi #FF6900/Xanh Spotify #1ED760/Xanh Apple #2997FF). Font chữ=Không nét chân hình học dày, một màn hình một từ hoặc một con số siêu lớn chiếm trọn tầm mắt, khoảng cách chữ thu chặt. Master=①Trang tiêu đề nền đen căn giữa một dòng chữ lớn ②Trang cao trào dữ liệu con số khổng lồ+Đơn vị+Một dòng ghi chú ③Hai cột đối lập tham số trái phải (Màu nhấn mạnh vs Xám) ④Trang đơn slogan. Lượng lớn khoảng trắng âm
- Thực thi HTML: Nền đen chữ trắng vài dòng CSS; Con số khổng lồ clamp()+flex căn giữa; Màu nhấn mạnh highlight span riêng biệt; Đối lập trái phải CSS Grid hai cột+Thanh highlight; tabular-nums. Bỏ hình sản phẩm đổi sang chữ thuần ngược lại càng gần với bản chất Zen hơn
- Font chữ: Geist / Inter / Tư Nguyên Hắc thay SF Pro

**Mono-Brand Type-as-Hero Poster Màu Va chạm Thương hiệu Đơn sắc Bão hòa cao** `Táo bạo · Phục hồi 96%`
- Tham khảo: Hệ thống thị giác Spotify Wrapped, Mailchimp Brand Book (Collins), Netflix đỏ đen phục chế hiện đại, Hệ thống thương hiệu COLLINS
- Thích ứng: Thương hiệu/Chiến lược marketing, Presentation campaign, Trang văn hóa town hall, Visual chính sự kiện
- DNA thị giác: Phối màu=Màu chính thương hiệu duy nhất trải toàn bản làm nền (Xanh Spotify #1ED760/Vàng Mailchimp #FFE01B/Đỏ Netflix #E50914)+Chữ phản quang đen hoặc trắng, hai lớp va chạm màu. Font chữ=Font chữ siêu lớn tức là visual chính (type-as-hero) chọc trời cắm đất. Master=①Nền khối màu đầy+Chữ khổng lồ phản quang ②Khối màu đôi trên dưới/trái phải phân chia ③Con số khổng lồ chống đỡ. Biểu tượng=Đơn sắc toàn bản, font chữ làm hình, va chạm màu tương phản cao
- Thực thi HTML: background-color toàn bản; Chữ siêu lớn clamp() chiếm trọn; Màu đôi dùng hai khối màu 100vh; Font chữ làm hình dựa vào font-weight 900+letter-spacing âm. Khối màu thuần 0 vật liệu, HTML nguyên bản sướng nhất
- Font chữ: Inter / Manrope / Archivo (Siêu dày) thay Circular/Cavendish

**Full-Bleed Gradient Manifesto Bản trình bày Tuyên ngôn Chuyển sắc Toàn khung** `Táo bạo · Phục hồi 82%`
- Tham khảo: Deck bán hàng Zuora 『Tell a Different Story』 (Andy Raskin mổ xẻ), Campaign Nike 『Just Do It』, Trang đôi National Geographic
- Thích ứng: Trang viễn cảnh đề xuất bán hàng, Tuyên ngôn thương hiệu, Trang chuyển ngoặt keynote, Trang đơn sứ mệnh viễn cảnh
- DNA thị giác: Phối màu=Chuyển sắc CSS toàn khung (Cam ấm→Đỏ sẫm/Xanh sâu→Xanh ngọc) hoặc màu thuần tràn lề+Chữ tuyên ngôn phản quang đại biểu+Khẩu hiệu hashtag (#shifthappens). Font chữ=Khẩu hiệu không nét chân dày dặn viết hoa toàn bộ chạy ngang. Master=①Chuyển sắc toàn khung+Tuyên ngôn phản quang căn giữa ②Trang viễn cảnh đất hứa ③Tường logo khách hàng. Biểu tượng=full-bleed tràn lề, khẩu hiệu đại biểu phản quang, khẩu hiệu hashtag
- Thực thi HTML: linear-gradient/radial-gradient toàn khung (Không làm hạt/Quang ảnh, chuyển sắc CSS thuần được phép); Chữ phản quang position căn giữa; Tường logo dùng grid SVG xám/Chữ làm vị trí. Ban đầu dựa vào hình ảnh kỷ thực lớn bị hạ cấp thành chuyển sắc CSS trải nền+Chữ lớn, thiếu hình ảnh mục này độ phục hồi giảm khoảng 15%
- Font chữ: Archivo / Anton / Manrope (Siêu dày)

**Candy-Color Lecture Stage Sân khấu Đơn Khái niệm Kẹo CS50** `Táo bạo · Phục hồi 94%`
- Tham khảo: Harvard CS50 (David Malan), Phương pháp Lessig/Dòng Takahashi, Presentation Zen
- Thích ứng: Bài giảng giáo dục, Diễn thuyết kỹ thuật, Giải thích khái niệm, Giảng dạy code
- DNA thị giác: Phối màu=Nền đen sâu #0A0A0A+Chữ lớn màu kẹo bão hòa cao luôn phiên (Đỏ sẫm #FF2D95/Xanh ngọc #00E5FF/Vàng sáng #FFD500/Xanh lá #39FF14). Font chữ=Không nét chân chữ siêu lớn lơ lửng căn giữa, một màn hình một khái niệm, chữ cực ít. Master=①Nền đen sâu một từ lớn màu kẹo ②Khối code bằng nhau tô màu cú pháp ③Chữ lớn cảm giác đèn spotlight sân khấu. Biểu tượng: Chữ lớn màu kẹo lơ lửng nền đen sâu, tô màu code bằng nhau, spotlight sân khấu mạnh, cực ít chữ
- Thực thi HTML: Nền đen sâu+Chữ siêu lớn đơn màu clamp() căn giữa; Khối code dùng pre+Chữ bằng nhau+span tô màu làm tô màu cú pháp; Cảm giác spotlight dùng radial-gradient góc tối cực nhạt (Không phải hiệu ứng quang ảnh). Độ phục hồi cao
- Font chữ: Inter siêu dày + JetBrains Mono (Code)

**Playful Maximalist Editorial Cực giản Thủ công Vui nhộn (Kiểu Collins)** `Táo bạo · Phục hồi 75%`
- Tham khảo: Mailchimp Brand Book (Collins 2018), Khí chất hoạt hình New Yorker, Serif bo tròn Cooper, Vàng huỳnh quang Cavendish
- Thích ứng: Deck thương hiệu có thái độ, Đề xuất agency sáng tạo, Trang town hall hướng văn hóa, Trang marketing phản cực giản SaaS
- DNA thị giác: Phối màu=Vàng huỳnh quang Cavendish #FFE01B diện tích lớn+Đen+Một lượng nhỏ va chạm màu, phản cực giản SaaS. Font chữ=Tiêu đề Serif bo tròn kiểu Cooper (playful)+Trình bày chữ khoảng trắng kiểu tạp chí. Master=①Nền vàng huỳnh quang đầy+Tiêu đề quái đản ②Trình bày khoảng trắng không quy tắc kiểu tạp chí ③Văn bản chữ lớn chơi chữ. Biểu tượng=Vàng huỳnh quang, Serif bo tròn, trình bày playful, khí chất thủ công quái đản (Hạ cấp thành khối màu hình học/emoji thay thế hình minh họa thật)
- Thực thi HTML: background Vàng huỳnh quang; font-family Serif bo tròn; Khoảng trắng tạp chí dùng Grid không đối xứng. Khỉ đột thủ công/Minh họa phần tử cốt lõi này nếu không có AI tạo hình không thể làm, hạ cấp thành khối màu hình học CSS+emoji cỡ lớn+khối chữ xoay transform không quy tắc thay thế, thiếu hình minh họa độ phục hồi giảm khoảng 20%
- Font chữ: Fraunces (Có thể điều chỉnh bo tròn)/ Bree Serif thay Cooper; Chữ chính Inter

**Irreverent Pop Không Kiềm chế Chơi chữ Pop (Kiểu Reddit)** `Táo bạo · Phục hồi 80%`
- Tham khảo: Reddit Ads sales deck (Được Dock xếp vào loại có tính cách nhất), Trình bày chữ không kiềm chế kiểu David Carson, Web Y2K hoài cổ, Memphis vui nhộn
- Thích ứng: Thương hiệu Gen Z, Deck marketing chơi chữ, Hướng tới cộng đồng/creator, Đề xuất dám không nghiêm túc
- DNA thị giác: Phối màu=Đỏ cam Reddit #FF4500+Va chạm màu, màu hoài cổ 90s web. Font chữ=Trình bày chữ pha trộn/Phá vỡ grid kiểu David Carson, văn bản khẩu ngữ chơi chữ. Master=①Trang fun chữ lớn chơi chữ ②Trang facts nhịp điệu chuyển ngoặt dữ liệu nghiêm túc ③Tiêu đề khẩu ngữ. Biểu tượng=Phá vỡ grid pha trộn, Đỏ cam, Khẩu ngữ chơi chữ, Đảo ngược nhịp điệu fun→facts, Chất lượng web hoài cổ
- Thực thi HTML: Cố tình phá vỡ grid dùng transform xoay/Định vị chồng lấp/Pha trộn cỡ chữ; Đỏ cam+Khối màu va chạm; Chất lượng hoài cổ dùng border viền đen dày+box-shadow bóng cứng (Không blur). Minh họa meme tùy chỉnh hạ cấp thành emoji+Cắt dán hình học, nhưng bản thân trình bày pha trộn HTML có thể phục hồi
- Font chữ: Archivo / Space Grotesk + Pha trộn Inter tạo tương phản

**Maximalist 3D-Type Chữ 3D Bơm phồng Y2K (Kiểu Wrapped)** `Táo bạo · Phục hồi 78%`
- Tham khảo: Spotify Wrapped 2022/2023/2025, Va chạm màu Memphis, Y2K/Maximalism, Chuyển sắc chân dung duotone
- Thích ứng: Phục盘 năm (Hướng tới giật gân cảm xúc), Card dữ liệu cá nhân hóa, Card dọc chia sẻ mạng xã hội, Cuối năm thương hiệu
- DNA thị giác: Phối màu=Nền va chạm màu bão hòa cao toàn bản (Đỏ sẫm+Xanh ngọc+Cam)+Xanh Spotify làm điểm nhấn+Chuyển sắc hai màu duotone. Font chữ=Con số khổng lồ chọc trời cắm đất, Năm/Con số làm 3D bơm phồng/Chất lượng kim loại. Master=①Nền va chạm màu toàn bản+Con số khổng lồ bơm phồng ②Chân dung duotone/Nền khối màu+Chữ lớn phản quang ③Card màn hình dọc có thể chia sẻ. Biểu tượng=Con số 3D bơm phồng khổng lồ, Va chạm màu toàn bản, Chuyển sắc duotone, Chất lượng kim loại của năm, Card story màn hình dọc
- Thực thi HTML: background va chạm màu toàn bản; Con số 3D bơm phồng dùng CSS text-shadow đè nhiều lớp+transform:perspective hoặc SVG+stroke tạo khối 3D (Không phải render 3D thật); duotone dùng mix-blend-mode+Chuyển sắc đè lên khối giữ chỗ hình ảnh thang xám. Chất lượng kim loại hạ cấp thành chữ điền chuyển sắc background-clip:text, độ phục hồi giảm khoảng 15%
- Font chữ: Archivo Black / Anton siêu dày + Con số Clash Display


#### Phái Trung tính

**Bento Grid Grid Mô đun Bento** `Trung tính · Phục hồi 95%`
- Tham khảo: Thời đại Apple Keynote Bento Grid, Bento/Big-Type deck MBB thế hệ mới (2024-2026), Matrix card chỉ số báo cáo năm Stripe, Template QBR Pitch.com
- Thích ứng: Gom nhóm tính năng sản phẩm, Báo cáo dữ liệu tư vấn/QBR, Trang thành quả bán hàng, Trang chỉ số town hall
- DNA thị giác: Phối màu=Nền xám nhạt/Trắng kem (#F5F5F7/cream) hoặc nền gần như đen+Màu chính thương hiệu+1-2 màu nhấn mạnh, card phân khu màu nhạt+Bo góc+Micro-border/Micro-shadow. Font chữ=Tiêu đề display siêu lớn+Chữ chính thông thường, tương phản độ đậm chữ mạnh mẽ, con số KPI tabular figures. Master=①Trang tiêu đề câu đơn khổng lồ+Khoảng trắng ②Trang bento 2×2/3 cột card không bằng nhau mỗi card một insight (Con số/Icon tuyến tính/sparkline) ③Trang con số khổng lồ one-insight. Biểu tượng=Grid card không bằng nhau, Bo góc micro-border, Cảm giác nhịp thở
- Thực thi HTML: CSS Grid grid-template-areas làm bento không bằng nhau; Card border-radius+box-shadow bóng nhỏ+1px hairline; sparkline dùng SVG inline; Icon tuyến tính dùng inline SVG stroke. 0 dán hình
- Font chữ: Inter / Geist + Con số Geist Mono

**Neo-Swiss Dark Hairline Terminal Mỹ học Terminal Tối Neo-Swiss** `Trung tính · Phục hồi 94%`
- Tham khảo: Linear pitch deck, Ngôn ngữ thiết kế Vercel, Bài giảng sân khấu đen sâu CS50; Font chữ Inter Tight+JetBrains Mono
- Thích ứng: Ra mắt sản phẩm công cụ developer/kỹ thuật, Đường lối kỹ thuật, Báo cáo hướng kỹ thuật
- DNA thị giác: Phối màu=Nền gần như đen (#0D0D0F/#111113)+Grid đường sợi tóc hairline #262629+Nhấn mạnh tím xanh duy nhất (#5B5BD6/#7C7CFF). Font chữ=Inter Tight tiêu đề lớn+JetBrains Mono làm nhãn/dữ liệu. Master=①Trang tiêu đề cực giản một câu+Nhãn mono nhỏ ②Grid dữ liệu phân cách hairline ③Danh sách đặc tính của nhãn mono. Biểu tượng=Grid đường 1px, Nhãn bằng nhau mono đơn, Khoảng trắng cực hạn, Gần như đen không phải đen thuần
- Thực thi HTML: Nền gần như đen+grid hairline border:1px solid; Nhãn mono dùng font-family bằng nhau; Ánh sáng nhẹ dùng box-shadow/border highlight cực nhạt chứ không dùng quang ảnh thật (Hạ cấp né khu vực cấm cyber neon). Lưu ý né khu vực cấm xanh sâu #0D1117, dùng gần như đen trung tính
- Font chữ: Inter Tight + JetBrains Mono / IBM Plex Mono

**Two-Font Consulting Bản Tư vấn Font đôi (Kiểu Bower)** `Trung tính · Phục hồi 90%`
- Tham khảo: Hệ thống thương hiệu McKinsey 2019 (Wolff Olins thiết kế, Bower serif+sans-serif), BCG Executive Perspectives, Pattern đường xanh sâu
- Thích ứng: Báo cáo tư vấn, Báo cáo cấp cao, Nghiên cứu ngành, Đề xuất cơ quan uy tín
- DNA thị giác: Phối màu=Xanh sâu (#051C2C/Xanh sâu McKinsey)×Trắng nhị nguyên+Màu highlight thương hiệu duy nhất (Xanh BCG #00805A), Nền xám ấm mang cảm giác nhịp thở. Font chữ=Tiêu đề Serif characterful (Kiểu Bower) và chữ chính sans-serif đặt cạnh nhau tương phản cao. Master=①Action-title dạng kết luận góc trên bên trái ②Trang trí pattern đường xanh sâu ③Phân công trái phải kiểu tạp chí (Văn bản kết luận+Thị giác) ④Card data-point con số lớn. Biểu tượng=Serif×Sans-serif tương phản cao, Pattern đường xanh sâu, Action-title, Cảm giác cao cấp xám ấm
- Thực thi HTML: Đặt cạnh nhau font-family font đôi (Tiêu đề Serif+Chữ chính Sans-serif); Pattern đường dùng repeating-linear-gradient hoặc SVG line; Card data-point dùng CSS thuần; Xử lý hình ảnh thang xám mục này không có hình ảnh có thể tiết kiệm. Ánh mờ mép xanh tím hạ cấp thành viền màu thuần
- Font chữ: Playfair Display / Fraunces tiêu đề serif + Inter chữ chính (Thay thế Bower)

**Diagram-Driven Isotype Bản Doanh nghiệp Mũi tên Sơ đồ** `Trung tính · Phục hồi 88%`
- Tham khảo: Deck bán hàng Salesforce, Hệ thống Isotype (Otto Neurath), Gene Zelazny 《Say It With Charts》, Hans Rosling/Gapminder
- Thích ứng: Giải thích nền tảng/kiến trúc, Hành trình khách hàng, Quy trình phương pháp luận, Bản đồ sinh thái
- DNA thị giác: Phối màu=Khối màu xanh doanh nghiệp+Phân màu dòng sản phẩm phân biệt+Grid năng lực icon hóa. Font chữ=Không nét chân rõ ràng. Master=①Luồng mũi tên hành trình khách hàng ngang ②Sơ đồ kiến trúc nền tảng phân lớp ③Grid năng lực icon hóa ④Sơ đồ cấu trúc 2×2/Thác nước/Kim tự tháp. Biểu tượng=Quy trình mũi tên, Hộp kiến trúc phân lớp, Grid icon Isotype, Quy trình tức là tự sự
- Thực thi HTML: Quy trình mũi tên dùng Flexbox+CSS clip-path tam giác hoặc SVG arrow; Phân lớp kiến trúc dùng div bọc viền lồng nhau; Icon dùng inline SVG stroke nét vẽ thống nhất; Thác nước/Kim tự tháp dùng Grid+Cắt nghiêng. Sơ đồ bong bóng có thể dùng CSS hình tròn+Định vị. Vẽ vectơ thuần túy
- Font chữ: Inter / IBM Plex Sans (Thân thiện với biểu đồ)

**Diagrammatic Minimalism Sơ đồ Khái niệm Mẹ** `Trung tính · Phục hồi 95%`
- Tham khảo: Vòng tròn hoàng kim (Golden Circle) TED của Simon Sinek, Cấu thành trừu tượng hình học Bauhaus, Kiến trúc thông tin 『Một hình định toàn bộ hội trường』
- Thích ứng: Giải thích khung lý thuyết, Truyền bá tư tưởng kiểu TED, Trực quan hóa mô hình/phương pháp luận, Keynote đơn khái niệm
- DNA thị giác: Phối màu=Nền trắng/nhạt cực giản+Đen+1 màu nhấn mạnh, Hình học màu thuần. Font chữ=Không nét chân, Nhãn viết hoa nhúng vào hình dạng. Master=①Hình mẹ hình học duy nhất (Vòng tròn đồng tâm/Tam giác/Ma trận) gánh vác toàn bộ khái niệm ②Mũi tên từ trong ra ngoài ③Trường hợp so sánh. Biểu tượng=Hình mẹ hình học duy nhất, Vòng tròn/Tam giác lồng nhau, Nhãn viết hoa, Một hình gánh vác khái niệm
- Thực thi HTML: Vòng tròn đồng tâm dùng border-radius:50% div lồng nhau hoặc SVG circle; Tam giác dùng clip-path/SVG polygon; Mũi tên SVG marker; Nhãn absolute định vị dán lên hình dạng. Hình học thuần túy, HTML phục hồi hoàn hảo
- Font chữ: Manrope / Futura系 (Jost thay thế mã nguồn mở) cảm giác hình học

**Narrative Sparkline Dải sóng Tự sự (Kiểu Duarte)** `Trung tính · Phục hồi 91%`
- Tham khảo: Sơ đồ tự sự Sparkline 《Resonate》 của Nancy Duarte, Al Gore 《An Inconvenient Truth》, Duarte Inc. tự sự dữ liệu
- Thích ứng: Thiết kế cấu trúc diễn thuyết, Tự sự biến革, Đối chiếu before/after, Cung câu chuyện dữ liệu
- DNA thị giác: Phối màu=Nền tối hoặc nền trắng+Cam thương hiệu nhấn mạnh điểm chuyển ngoặt+Xám hóa đối chiếu. Font chữ=Không nét chân, Điểm chú thích annotation. Master=①Đường sóng dao động chạy ngang toàn màn hình ②Điểm text chú thích trên đường sóng ③Đường sóng đối chiếu đặt cạnh nhau trên dưới ④Đường dữ liệu đơn lẻ lơ lửng nền toàn đen ⑤Reveal từng bước. Biểu tượng=Đường sóng chạy ngang, Điểm chú thích đường sóng, Điểm chuyển ngoặt màu cam, Đường sóng đối chiếu, Đường cong bò ra khỏi hình
- Thực thi HTML: Đường sóng dùng SVG path inline (Bézier mượt mà); Điểm chú thích dùng SVG circle+text định vị; Đường sóng đối chiếu hai đường path trên dưới; reveal dùng animation CSS stroke-dashoffset. Vẽ SVG thuần túy không vật liệu
- Font chữ: Inter + Con số Geist Mono


#### Phái Trầm tĩnh

**Khẳng định-Bằng chứng / Trực quan hóa Thông tin Tufte** `Trầm tĩnh · Phục hồi 93%`
- Tham khảo: Michael Alley Assertion-Evidence (Xác minh thực tế Penn State), McKinsey/BCG action-title, Tỷ lệ mực dữ liệu Edward Tufte, Nguyên lý kim tự tháp Barbara Minto
- Thích ứng: Báo cáo học thuật/Kỹ thuật, Trang tư vấn loại dữ liệu nghiêm ngặt, Báo cáo chính sách, Thẩm định kỹ thuật
- DNA thị giác: Phối màu=Nền trắng/xám cực nhạt+Chữ chính đen+Màu nhấn mạnh kiềm chế duy nhất (Xanh sâu/Đỏ gạch). Font chữ=Tiêu đề nguyên một câu (Không phải cụm từ danh từ), Dưới tiêu đề chiếm riêng một hình, Chú thích chữ nhúng vào trong hình. Master=①Action-title nguyên câu ②Bằng chứng hình đơn dưới tiêu đề ③Zero bullet. Biểu tượng=Tiêu đề nguyên câu, Bằng chứng hình đơn, Chú thích nhúng, Zero chartjunk, Tỷ lệ mực dữ liệu cao
- Thực thi HTML: Tiêu đề nguyên câu dựa vào cấp độ trình bày chữ; Biểu đồ dùng CSS thuần/SVG inline vẽ đường/phân tán cực giản (Bỏ đường grid bỏ chú thích hình, chú thích trực tiếp text định vị bên cạnh điểm dữ liệu); Zero trang trí. Sự kiềm chế của Tufte chính là điểm mạnh của HTML
- Font chữ: Source Serif / Lora tiêu đề + Inter chữ chính (Font đôi cấp độ đọc)

**Institutional Swiss Minimal Cực giản Cơ quan Thụy Sĩ** `Trầm tĩnh · Phục hồi 96%`
- Tham khảo: Template pitch 10 trang chính thức của Sequoia, Deck vòng seed 2009 của Airbnb, Grid Müller-Brockmann, Massimo Vignelli
- Thích ứng: Gọi vốn đầu tư, Đề xuất thương mại tiêu chuẩn, Tự sự Vấn đề-Giải pháp, Đề xuất thương hiệu bỏ trang trí
- DNA thị giác: Phối màu=Nền trắng thuần+Chữ chính đen xám+Màu nhấn mạnh thương hiệu duy nhất (Đỏ san hô Airbnb #FF5A3C/Xanh trung tính). Font chữ=Hệ Helvetica không nét chân, Tiêu đề cỡ vừa đậm một câu, Chữ chính câu ngắn khoảng cách lớn. Master=①Logo căn giữa+slogan ②Dải tiêu đề một câu phía trên+3 cột đối xứng phía dưới (Problem/Solution 3 điểm) ③Phân lớp con số lớn TAM ④Ma trận đối thủ 2×2. Biểu tượng=Dải tiêu đề phía trên, 3 cột đối xứng, Nhấn mạnh đơn sắc, Ma trận 2×2
- Thực thi HTML: Flexbox 3 cột đối xứng; Ma trận 2×2 dùng CSS Grid thuần+border vẽ; Phân lớp TAM dùng div lồng nhau hoặc khối vuông đồng tâm; Một trang một thông tin. Hầu như là grid trình bày chữ thuần túy, đối tượng lý tưởng của HTML
- Font chữ: Inter / Helvetica Now thay thế Helvetica; Chữ chính Inter

**Editorial Longform Luồng Văn bản dài Tạp chí** `Trầm tĩnh · Phục hồi 95%`
- Tham khảo: Stripe Annual Letter ($1.9T), Thư ngỏ tự sự 6 trang của Amazon, Benedict Evans 『X eats the world』, Stripe Press
- Thích ứng: Thư/Phục盘 năm, Văn bản dài tư tưởng sâu sắc, Cập nhật nội dung nội bộ, Vật phẩm đọc kiểu báo cáo
- DNA thị giác: Phối màu=Nền trắng kem/trắng gạo (#FBFAF8)+Chữ mực sâu+Màu điểm xuyết thương hiệu (Tím Stripe #635BFF). Font chữ=Serif hoặc Không nét chân chất lượng cao, Đoạn văn bản tự do+Card dữ liệu inline, Con số display siêu lớn đan xen. Master=①Tiêu đề lớn đầu báo ②Văn bản tự do nhiều cột+Card chỉ số inline ③Mỏ neo đoạn con số siêu lớn. Biểu tượng=Nhịp điệu đọc ấn phẩm, Card dữ liệu inline, Khoảng trắng kiềm chế, Văn bản tự do chứ không phải bullet
- Thực thi HTML: Nhiều cột column-count hoặc Grid; Card dữ liệu inline float/inline-block nhúng vào chữ chính; Chữ chính serif max-width kiểm tra chiều rộng dòng 65ch; Con số siêu lớn đan xen. Trình bày chữ thuần túy, 0 vật liệu
- Font chữ: Newsreader / Source Serif chữ chính + Inter hỗ trợ; Con số tabular

**Humanist Rounded Cards Card Bo góc Nhân văn (Kiểu Khan)** `Trầm tĩnh · Phục hồi 80%`
- Tham khảo: Hệ thống thiết kế Khan Academy Wonder Blocks, Source Serif Pro serif, Thương hiệu xanh rừng, Nhân văn chủ nghĩa thân thiện
- Thích ứng: Sản phẩm giáo dục, Bài giảng có sức hút, Deck công ích/Phi lợi nhuận, Đề xuất thương hiệu ấm áp
- DNA thị giác: Phối màu=Xanh rừng #14BF96/#0A5C4B+Nền trắng kem+Hỗ trợ màu ấm, Mềm mại không chói mắt. Font chữ=Source Serif tiêu đề serif (Mang khí chất nhân văn)+Chữ chính không nét chân. Master=①Nhóm component card bo góc ②Tiêu đề serif+Chữ chính thân thiện ③Vị trí nhiếp ảnh thực tế (Hạ cấp thành khối màu hình học/Bo góc hệ xanh). Biểu tượng=Xanh rừng, Tiêu đề serif, Card bo góc lớn, Nhân văn ấm áp, Chất lượng thân thiện không hoàn hảo
- Thực thi HTML: Card border-radius bo góc lớn+box-shadow mềm mại; Tiêu đề serif font-family; Nền trắng kem ấm. Nhiếp ảnh thầy trò thực tế mục này nếu không có AI tạo hình, hạ cấp thành khối hình học hệ xanh/Khối giữ chỗ màu thuần bo góc lớn+emoji nhân vật, thiếu hình ảnh độ phục hồi giảm khoảng 18%
- Font chữ: Source Serif 4 tiêu đề + Nunito Sans / Inter chữ chính (Nunito bo tròn tương ứng nhân văn)

**Dense Research Report Báo cáo Nghiên cứu Dày đặc (Kiểu Meeker)** `Trầm tĩnh · Phục hồi 92%`
- Tham khảo: Mary Meeker 《Internet Trends》 (BOND), CB Insights 《State of AI》, McKinsey Global Institute 《Year in Charts》, Báo chí dữ liệu FT/Bloomberg
- Thích ứng: Báo cáo xu hướng, Phục盘 dữ liệu ngành, Báo cáo dữ liệu dày đặc, Bản đồ thị trường
- DNA thị giác: Phối màu=Nền trắng+Màu thương hiệu (BOND/CB Insights xanh sáng #0066FF) Thang bậc đơn sắc highlight phần còn lại xám hóa, Hầu như zero khoảng trắng. Font chữ=Tiêu đề câu dạng kết luận, Mỗi trang 1 hình mật độ, Chú thích nguồn cực nhỏ. Master=①Tiêu đề câu kết luận+Hình đơn toàn trang ②Grid logo market map ③Card KPI con số lớn ④Grid nhiều hình dày đặc+Chú thích. Biểu tượng=Tiêu đề câu kết luận, Zero khoảng trắng cảm giác báo cáo, Highlight thang bậc đơn sắc, Bản đồ thị trường logo, Quy chuẩn chú thích nguồn
- Thực thi HTML: Biểu đồ dày đặc tất cả dùng CSS thuần/SVG inline vẽ (Cột/Đường/Xếp chồng/Phân tán); logo market map dùng Grid+Khối giữ chỗ chữ/SVG; Card KPI CSS; Chú thích chữ nhỏ đoạn văn. Mật độ thông tin cực hạn chính là điểm mạnh của HTML, 0 vật liệu
- Font chữ: Inter + IBM Plex Sans + Con số tabular Geist Mono

**All-Text Manifesto Bản ghi nhớ Tuyên ngôn Chữ thuần (Kiểu Netflix/Amazon)** `Trầm tĩnh · Phục hồi 97%`
- Tham khảo: Netflix Culture Deck (2009, 125 trang), Thư ngỏ tự sự 6 trang Amazon (Bezos), Chủ trương phản PowerPoint Tufte, Trình bày chữ cấp độ đọc Matthew Carter
- Thích ứng: Tuyên ngôn văn hóa, Diễn thuyết giá trị quan, Bản ghi nhớ sâu sắc, Thuyết trình chữ thuần phản PPT
- DNA thị giác: Phối màu=Nền trắng thuần hoặc đen thuần+Màu nhấn mạnh duy nhất (Đỏ Netflix #E50914) làm highlight duy nhất, Cực hạn kiềm chế. Font chữ=Trình bày chữ cấp độ đọc, Một trang một quan điểm kim ngôn khẳng định/Văn bản tự do thuần zero bullet zero hình. Master=①Nền toàn bản+Kim ngôn khẳng định ②Đoạn văn khẩu ngữ bộc bạch ③Khái niệm thể chế highlight (Keeper Test) ④6 trang văn bản tự do+Bảng phụ lục. Biểu tượng=Chữ thuần một trang một quan điểm, Zero hình zero bullet, Highlight đơn sắc kim ngôn, Khẩu ngữ bộc bạch, Cảm giác văn bản silent-read
- Thực thi HTML: Trình bày chữ thuần: Kim ngôn dùng chữ lớn clamp() căn trái cấp độ; Văn bản tự do max-width kiểm soát chiều rộng dòng; Màu nhấn mạnh duy nhất span highlight cụm từ mấu chốt; Phụ lục dùng table cực giản. Zero vật liệu zero hình, chữ thuần là bản phục hồi ổn định nhất của HTML
- Font chữ: Newsreader / Source Serif (Cấp độ đọc) hoặc Inter (Dạng tuyên ngôn); Tiêu đề có thể Archivo siêu dày


---

## Thư viện Phong cách Infographic (20 loại)

> **Bổ sung 08-2026**. Trước đây thư viện này chỉ có hai nửa sân Web và PPT, nhưng "Infographic/Trực quan hóa" trong SKILL là một trong 4 kịch bản áp dụng lớn——Khi làm Infographic vòng xoay chỉ có thể rơi vào nửa sân Web, rút ra là phong cách của trang cộng đồng/landing page, rồi gượng ép áp vào. Phân khu này bổ sung chính là cái lỗ đó.
> Tiêu chí: **Thành phẩm là một hình (hoặc một nhóm hình) lấy dữ liệu làm nhân vật chính, có thể thoát khỏi tương tác đọc độc lập**, thì đi theo đây; Là site có thể click đi theo phân khu Web, là cần lật trang đi theo phân khu PPT.

#### Phái Táo bạo

**Personal Annual Report Báo cáo Năm In ấn Dữ liệu Cá nhân (Kiểu Feltron)** `Táo bạo · Phục hồi 94%`
- Tham khảo: Nicholas Felton 《Feltron Annual Report》 2005–2014 (Bản 2006-2011 được đưa vào bộ sưu tập vĩnh viễn của MoMA); Stefanie Posavec; Đặc tập năm Bloomberg Businessweek
- Thích ứng: Tổng kết năm cá nhân hoặc team, quantified-self, Phục盘 năm sản phẩm, Phục盘 loại Wrapped, Tự kiểm toán chu kỳ dài
- DNA thị giác: Phối màu=Nền trắng ấm giấy chưa phủ+Cam chu sa duy nhất làm accent xuyên suốt+Xanh đá bảng chuỗi dữ liệu thứ hai+Màu thứ ba chỉ gắn một ngữ nghĩa tuyệt đối không tái sử dụng. Font chữ=Hệ Helvetica xếp chặt, Con số khổng lồ đè đỉnh, Chữ nhỏ chú thích dày đặc. Bộ master 4 món: ①Thanh mô đun con số khổng lồ ②Biểu đồ chu kỳ tọa độ cực (24h/12 tháng) ③Ô nhiệt độ lịch ④Dải thời gian hoặc địa lý chạy xuyên toàn khung. Biểu tượng=Sự tương phản khi làm dữ liệu vụn vặt cá nhân thành báo cáo năm doanh nghiệp, Grid in ấn mô đun hóa, Đường phân cách 1px, In đen trắng vẫn đọc được
- Thực thi HTML: CSS Grid phân mô đun+1px border cắt grid; Biểu đồ tọa độ cực SVG inline tự tính góc cực (Không trích dẫn thư viện biểu đồ); Lịch dùng Grid+Chiều cao phần tử con lấp đầy. Trình bày chữ thuần+SVG, 0 vật liệu, điểm cực mạnh của HTML
- Font chữ: Archivo / Helvetica Neue (Con số khổng lồ xếp chặt) + Inter (Chú thích chữ nhỏ)

**Explanation Graphics Sơ đồ Giải thích (Kiểu Nigel Holmes)** `Táo bạo · Phục hồi 76%`
- Tham khảo: Nigel Holmes (1978–1994 Giám đốc biểu đồ TIME, 1994 thành lập Explanation Graphics); Chủ trương dùng tranh vẽ và sự hài hước giải thích con số trừu tượng, cũng là tâm điểm của cuộc tranh luận chartjunk
- Thích ứng: Giải thích phổ cập khoa học, Giảng giải khái niệm phức tạp cho người ngoài ngành, Hình phối hợp chuyên mục truyền thông đại chúng, Hướng tới trẻ em và giáo dục
- DNA thị giác: Phối màu=Phủ màu phẳng bão hòa cao ba bốn màu+Viền đen. Font chữ=Không nét chân bo tròn+Chú thích cảm giác viết tay. Master=Vẽ bản thân biểu đồ thành hình ảnh ẩn dụ thực thể (Tiền giấy xếp thành cột, Nhiệt kế làm biểu đồ đo, Đường chạy làm thanh tiến độ). Biểu tượng=Biểu đồ mô phỏng thực thể, Hài hước, Tượng người nhỏ, Viền dày, Zero chuyển sắc
- Thực thi HTML: Khung biểu đồ CSS/SVG có thể làm, **linh hồn ở hình minh họa vẽ tay**——dưới HTML thuần chỉ có thể hạ cấp thành khối màu hình học, bắt buộc phải ghi rõ hạ cấp; Khi có năng lực tạo hình AI dùng huashu-gpt-image tạo phần tử minh họa rồi tổng hợp
- Font chữ: Nunito / Baloo 2 (Bo tròn) + Caveat (Chú thích viết tay)

**Cross-Section Epic Cắt dọc Khổng lồ Vẽ tay (Kiểu SCMP Arranz)** `Táo bạo · Phục hồi 55%`
- Tham khảo: Adolfo Arranz (Biên tập viên biểu đồ thâm niên SCMP, nhiều huy chương vàng giải thưởng Infographic quốc tế Malofiej, tác phẩm đại diện 《City of Anarchy》 cắt dọc Cửu Long Thành Trại); Malofiej được gọi là giải Pulitzer của ngành Infographic
- Thích ứng: Giải phẫu kiến trúc/Lịch sử/Dụng cụ, Bức cuộn dài đọc 10 phút một tấm, Phổ cập khoa học cấp bảo tàng
- DNA thị giác: Phối màu=Nền tối (Xanh mực sâu/Nâu sâu)+Highlight màu ấm+Chất lượng giấy làm cũ. Bố cục=Đơn tấm khổng lồ, Góc nhìn đẳng hướng hoặc cắt dọc chính diện, Chú thích đường dẫn dày đặc bao quanh chủ thể. Biểu tượng=Chi tiết vẽ tay, Chú thích đường dẫn, Góc nhìn cắt dọc, Một bức hình kể xong toàn bộ câu chuyện
- Thực thi HTML: 🔴 **HTML thuần không làm được cắt dọc vẽ tay**——Phong cách này bắt buộc phải có vật liệu minh họa, khi không có vật liệu đừng giả vờ. HTML chỉ gánh vác lớp chú thích đường dẫn và tương tác thu phóng cuộn. Không lấy được vật liệu thì đổi phong cách
- Font chữ: Source Serif (Tiêu đề) + Inter (Chú thích)

**Magazine Pop Data Trang Dữ liệu Va chạm Màu Tạp chí (Kiểu Businessweek)** `Táo bạo · Phục hồi 90%`
- Tham khảo: Bloomberg Businessweek (Thời kỳ Richard Turley), Trang biểu đồ WIRED, Chuyên mục Graphic Detail của The Economist
- Thích ứng: Biểu đồ truyền thông thương mại/kỹ thuật, Hình phối hợp chuyên mục quan điểm, Hình vuông mạng xã hội, Hình dữ liệu nhúng bài viết WeChat
- DNA thị giác: Phối màu=Hai màu chính va chạm (Vàng huỳnh quang+Đen, Đỏ sẫm+Xanh chàm)+Khoảng trắng giấy, Không dùng màu thứ ba điều hòa. Font chữ=Tiêu đề lớn kiểu siêu dày nén+Chữ giải thích cực nhỏ, tương phản cỡ chữ trên 10 lần. Master=Một hình một luận điểm, Bản thân biểu đồ chính là nhân vật chính của trình bày, Tiêu đề nói trực tiếp kết luận. Biểu tượng=Tương phản cỡ chữ cực đoan, Va chạm màu, Biểu đồ tràn lề ra ngoài khung, Tiêu đề dạng kết luận
- Thực thi HTML: CSS thuần có thể phục hồi hoàn toàn; Biểu đồ dùng SVG inline hoặc thanh CSS Grid; Tràn lề dựa vào margin âm. 0 vật liệu
- Font chữ: Archivo Black / Anton (Nén dày) + IBM Plex Sans (Giải thích)

**ISOTYPE Thống kê Hình họa (Neurath–Arntz)** `Táo bạo · Phục hồi 88%`
- Tham khảo: Hệ thống giáo dục hình họa quốc tế ISOTYPE do Otto Neurath và Gerd Arntz thành lập vào thập niên 1920 tại Vienna
- Thích ứng: Dữ liệu dân số/Xã hội/Chính sách công, Truyền thông công cộng hướng tới ngưỡng biết chữ thấp, Poster giáo dục
- DNA thị giác: Phối màu=Bộ màu giới hạn (Đen+Đỏ+Xanh+Vàng đất) phủ phẳng, Không chuyển sắc không bóng. Master=Cùng một icon lặp lại N lần thể hiện số lượng——**Phóng to icon thể hiện nhiều hơn là sai**, đây là quy tắc cốt lõi nhất của hệ thống này. Biểu tượng=Mảng icon hình bóng, Sắp xếp theo chiều ngang, Nhãn chữ bên trái, Cảm giác trật tự cực mạnh
- Thực thi HTML: Icon dùng SVG inline hình bóng+bố cục CSS repeat, HTML tự nhiên tương thích. Icon có thể tự vẽ hình bóng hình học, không cần vật liệu bên ngoài
- Font chữ: Jost / Archivo (Thay thế Futura)

**Data Humanism Đồ phổ Vẽ tay Chủ nghĩa Nhân văn Dữ liệu (Kiểu Lupi)** `Táo bạo · Phục hồi 80%`
- Tham khảo: Giorgia Lupi (Partner Pentagram) và Stefanie Posavec 《Dear Data》; Tuyên ngôn Data Humanism của Lupi chủ trương "Dữ liệu là con người chứ không phải con số"
- Thích ứng: Dữ liệu cá nhân hóa cảm xúc hóa, Mẫu nhỏ miêu tả sâu, Làm trải nghiệm cá nhân thành đồ phổ có thể đọc, Đề tài có nhiều chiều không lượng hóa
- DNA thị giác: Phối màu=Nền kem sổ tay+4-5 màu mềm mại tự ràng buộc ngữ nghĩa (San hô/Xanh thông/Vàng mù tạt/Tím mực), **Không có một màu nào là trang trí**. Master=①Xác định một bộ ngôn ngữ thị giác trước (Kích thước/Hình dạng/Gai/Đôi mỗi cái mã hóa một chiều) ②Bắt buộc phối một bảng chú giải dạy người đọc giải mã ③Các phần tử sắp xếp theo đường dẫn hữu cơ không dùng grid. Biểu tượng=Ký hiệu tùy chỉnh có thể giải mã, Bắt buộc mang chú giải, Sắp xếp hữu cơ
- Thực thi HTML: Ký hiệu dùng SVG inline tham số hóa tạo ra (Bán kính/Số gai/Độ dài đuôi gắn với field dữ liệu); Đường dẫn dùng đường cong Bézier xuyên điểm. SVG thuần 0 vật liệu, cái duy nhất không làm được là nét vẽ tay thực sự
- Font chữ: Georgia / Source Serif (Tiêu đề) + Inter (Chú giải)

**Scrollytelling Bức cuộn Dữ liệu Tự sự Cuộn (Kiểu The Pudding)** `Táo bạo · Phục hồi 85%`
- Tham khảo: The Pudding (Tạp chí báo chí dữ liệu), NYT The Upshot, Chuyên đề cuộn Reuters Graphics
- Thích ứng: Lập luận phức tạp cần hé lộ từng bước, Câu chuyện dữ liệu dài tập, Chuyên đề phía web
- DNA thị giác: Phối màu chuyển đổi theo chương nhưng giữ lại accent đơn xuyên suốt. Master=Chữ bên trái bước tiến, Hình bên phải biến dạng theo scroll; Mỗi một màn hình chỉ thúc đẩy một biến số. Biểu tượng=Hình không đổi chỉ biến dạng, Chữ và hình ràng buộc nghiêm ngặt, Đổi màu chương, Cuối cùng đưa ra toàn cảnh hoàn chỉnh
- Thực thi HTML: IntersectionObserver kích hoạt chuyển đổi trạng thái+CSS transition hoặc nội suy thuộc tính SVG, frontend thuần có thể phục hồi hoàn chỉnh. ⚠️ Hình thái giao hàng bắt buộc phải là trang web, **Xuất PDF/PNG sẽ mất toàn bộ tự sự**——Khi người dùng cần hình tĩnh đừng chọn nó
- Font chữ: Inter / Source Serif (Ưu tiên khả năng đọc của văn bản dài)

**Cartographic Lead Bản đồ làm Nhân vật chính (Kiểu Stamen)** `Táo bạo · Phục hồi 65%`
- Tham khảo: Stamen Design (Do Eric Rodenbeck thành lập năm 2001 tại San Francisco, khách hàng gồm National Geographic), gạch bản đồ Watercolor/Toner của nó là kinh điển công khai
- Thích ứng: Dữ liệu phân bố địa lý, Đề tài thành phố/Giao thông/Môi trường, Vị trí tức là tự sự
- DNA thị giác: Phối màu=Bản đồ nền định tông (Màu nước hoặc Toner đơn sắc)+Lớp dữ liệu dùng đường điểm tương phản cao. Master=Bản đồ chiếm trọn bản tâm, Dữ liệu đè lên theo mật độ điểm hoặc luồng tuyến, Chú giải cực nhỏ đè góc. Biểu tượng=Bản thân bản đồ nền có tính tác giả, Lớp dữ liệu kiềm chế, Hình dạng địa lý tức là bố cục
- Thực thi HTML: 🔴 **Cần dữ liệu địa lý thực tế (GeoJSON) và bản đồ nền**, HTML thuần không thể tự nhiên tạo ra địa hình đúng đắn——Không lấy được dữ liệu thì đổi phong cách, **Tuyệt đối không vẽ tay bản đồ giả**. Khi có dữ liệu có thể dùng SVG inline chiếu để vẽ
- Font chữ: Inter / IBM Plex Sans (Ghi chú địa danh cần số lượng lớn chữ nhỏ)


#### Phái Trung tính

**Newsroom Chart System Quy chuẩn Biểu đồ Báo chí Dữ liệu (Kiểu FT)** `Trung tính · Phục hồi 96%`
- Tham khảo: Team Chart Doctor của Financial Times và Visual Vocabulary công khai (Phân loại chọn biểu đồ theo Deviation/Correlation/Ranking/Distribution/Change-over-Time/Part-to-Whole/Magnitude/Spatial)
- Thích ứng: Dữ liệu tài chính và ngành, Trường hợp "Chọn đúng loại biểu đồ" quan trọng hơn "Đẹp", Khi chuỗi biểu đồ cần quy chuẩn thống nhất
- DNA thị giác: Phối màu=Nền giấy báo hồng cam biểu tượng+Một bộ dải màu có trật tự (Chuyển sắc đơn sắc thể hiện lượng liên tục, Màu đôi tương phản thể hiện độ lệch). Master=①Tiêu đề tức là kết luận ②Tiêu đề phụ giải thích phạm vi ③Bản thân biểu đồ bỏ viền bỏ grid ④Góc dưới bên trái bắt buộc ghi nguồn dữ liệu. Biểu tượng=Chọn đúng loại hình biểu đồ trước rồi mới bàn đến vẻ đẹp, Ghi chú nguồn không thể bỏ qua, Trục tọa độ cực giản
- Thực thi HTML: CSS thuần/SVG vẽ đường và cột; Mấu chốt là **Tra cứu Visual Vocabulary chọn đúng loại hình biểu đồ trước** rồi mới ra tay. 0 vật liệu
- Font chữ: Inter / Source Sans (Chữ chính) + Font bằng nhau đánh dấu con số

**Computational Portrait Chân dung Dữ liệu Tính toán (Kiểu Fathom)** `Trung tính · Phục hồi 78%`
- Tham khảo: Ben Fry và studio Fathom Information Design của ông tại Boston (Fry là đồng sáng tạo Processing, tác giả 《Visualizing Data》, tác phẩm từng vào triển lãm hai năm một lần Whitney)
- Thích ứng: Tập dữ liệu quy mô siêu lớn, Đề tài cần "Để dữ liệu tự lớn lên thành hình dạng", Chuỗi thời gian/Gen/Giao thông
- DNA thị giác: Phối màu=Nền trắng hoặc gần như đen+Đường nét cực mảnh+Độ trong suốt đơn sắc đè lên nhau tạo ra mật độ. Master=Không làm tóm tắt mà thể hiện toàn lượng, Dùng mật độ đè lên nhau của hàng loạt phần tử mảnh tạo thành hình ảnh. Biểu tượng=Đường sợi tóc, Đè lên nhau bằng độ trong suốt, Không trang trí, Hình dạng do thuật toán quyết định chứ không phải bố cục
- Thực thi HTML: Canvas hoặc số lượng lớn SVG path vẽ theo kịch bản, lượng dữ liệu lớn bắt buộc phải dùng Canvas. **Yêu cầu dữ liệu toàn lượng thực tế**, mẫu nhỏ không làm ra được cảm giác mật độ của phong cách này
- Font chữ: Inter / Roboto Mono (Ghi chú dữ liệu)

**Information is Beautiful Thông tin Tuyệt đẹp (Kiểu McCandless)** `Trung tính · Phục hồi 90%`
- Tham khảo: David McCandless 《Information is Beautiful》 và website cùng tên, nổi tiếng với việc "Làm các tập dữ liệu lớn thành hình ảnh màu sắc dễ so sánh trong một ánh nhìn"
- Thích ứng: So sánh phổ cập khoa học, Bảng xếp hạng, Trực quan hóa dữ liệu hướng tới đại chúng, Biểu đồ dạng truyền thông mạng xã hội
- DNA thị giác: Phối màu=Vòng màu hài hòa nhiều màu nhưng cùng độ sáng cùng độ bão hòa (Không phải va chạm màu ngẫu nhiên). Master=Các loại hình biểu đồ "Diện tích tức là số lượng" như biểu đồ bong bóng/treemap/sankey làm chủ đạo, Nhãn dán trực tiếp lên khối màu. Biểu tượng=Mã hóa diện tích, Nhiều màu cùng tông, Chú giải nhúng bên trong, Một bức hình chứa hàng chục mục
- Thực thi HTML: Treemap và bong bóng dùng CSS Grid hoặc SVG tính toán bố cục (Cần tự viết thuật toán đóng gói đơn giản); Sankey dùng SVG Bézier. 0 vật liệu
- Font chữ: Nunito Sans / Inter

**Open Research Data Dữ liệu Nghiên cứu Mở (Kiểu Our World in Data)** `Trung tính · Phục hồi 94%`
- Tham khảo: Our World in Data (Global Change Data Lab Oxford), Quy chuẩn là biểu đồ có thể tương tác, Phạm vi ghi rõ ràng, Dữ liệu có thể tải về
- Thích ứng: Đề tài nghiêm ngặt, Hiển thị dữ liệu cần đứng vững trước sự nghi ngờ, So sánh xu hướng dài hạn
- DNA thị giác: Phối màu=Nền trắng+Một bộ màu phân loại độ nhận diện cao nhưng không chói mắt+Màu xám làm chuỗi không trọng tâm. Master=①Tiêu đề kết luận một câu ②Phạm vi và khoảng thời gian viết ở tiêu đề phụ ③Trong hình đánh nhãn trực tiếp ở cuối đường (Không dùng chú giải) ④Dưới cùng ghi chú nguồn và giấy phép. Biểu tượng=Nhãn cuối đường thay thế chú giải, Xám hóa phần không trọng tâm, Phạm vi minh bạch, Kiềm chế
- Thực thi HTML: SVG đường thuần+text định vị ở đoạn cuối, loại ổn định nhất của HTML. 0 vật liệu
- Font chữ: Inter / Lato

**Speculative Tech Diagram Hình Biểu diễn Công nghệ Tư biện Phương Đông (Kiểu Takram)** `Trung tính · Phục hồi 84%`
- Tham khảo: Takram (Studio thiết kế kỹ thuật Tokyo và London, thực tiễn speculative design băng qua thiết kế và kỹ thuật)
- Thích ứng: Sơ đồ khái niệm kỹ thuật, Suy luận kịch bản tương lai, Hình phối hợp bạch bì thư loại nghiên cứu, Tự sự kiến trúc sản phẩm
- DNA thị giác: Phối màu=Nền xám kem cát+Màu tự nhiên bão hòa thấp (Rêu xanh/Đất nung)+Một chỗ xám kim loại. Font chữ=Độ đậm mảnh, Khoảng cách chữ lớn, Trộn Trung Anh tinh tế. Master=Biểu đồ được bài trí như tác phẩm nghệ thuật, Lượng lớn khoảng trắng, Hình hình học có cảm giác chính xác. Biểu tượng=Cảm giác công nghệ mềm mại, Hình học chính xác, Màu tự nhiên kiềm chế, Biểu đồ như thiết bị
- Thực thi HTML: CSS thuần/SVG có thể phục hồi; Mấu chốt ở tỷ lệ khoảng trắng và độ mảnh của đường (0.5-1px). 0 vật liệu
- Font chữ: Inter (Độ đậm mảnh) / Noto Sans JP + Cormorant (Tiêu đề tùy chọn)

**Pictogram System Trực quan hóa Biểu tượng Hệ thống (Kiểu Otl Aicher)** `Trung tính · Phục hồi 92%`
- Tham khảo: Hệ thống biểu tượng Otl Aicher thiết kế cho Olympic Munich 1972 và phương pháp grid trường Ulm
- Thích ứng: Sơ đồ quy trình, Infographic loại dẫn đường và hướng dẫn, Kịch bản đa ngôn ngữ, Khi cần một bộ biểu tượng giữ tính thống nhất
- DNA thị giác: Phối màu=Một bộ bảng màu giới hạn (Bản Olympic là Xanh nhạt/Xanh lá/Bạc)+Đen. Master=Tất cả biểu tượng dùng chung một grid và cùng một góc nét vẽ (Chỉ 0/45/90°), Biểu tượng và nhãn ngắn xuất hiện thành cặp. Biểu tượng=Ràng buộc góc nghiêm ngặt, Tính thống nhất hệ thống, Nhãn ngắn không nét chân, Grid có thể nhìn thấy
- Thực thi HTML: Icon dùng SVG inline tự vẽ theo ràng buộc 45°; Grid dùng CSS Grid. 0 vật liệu, nhưng phải tự mình giữ kỷ luật góc
- Font chữ: Jost / Archivo (Thay thế Univers)


#### Phái Trầm tĩnh

**Small Multiples Ma trận Bội số Nhỏ Tufte** `Trầm tĩnh · Phục hồi 95%`
- Tham khảo: Khái niệm small multiples và sparkline trong 《Envisioning Information》 của Edward Tufte
- Thích ứng: So sánh ngang nhiều chiều, Nhóm chuỗi thời gian, Biểu hiện của một biến số trên hàng chục lát cắt
- DNA thị giác: Phối màu=Nền trắng+Đường đen+Một màu nhấn mạnh đánh dấu mục bất thường. Master=Cùng một hình nhỏ lặp lại N lần chỉ đổi dữ liệu, **Dùng chung phạm vi trục tọa độ——Phạm vi không thống nhất sẽ mất đi tính so sánh, đây là lỗi chí mạng duy nhất của trường phái này**; Nhãn cực nhỏ đè bên cạnh hình. Biểu tượng=Hình nhỏ lặp lại dạng grid, Chia sẻ tỷ lệ, Zero chú giải, Zero đường grid, Tỷ lệ mực dữ liệu cao
- Thực thi HTML: CSS Grid xếp hình nhỏ+SVG đường inline trong mỗi ô. Nhất định phải thống nhất domain. 0 vật liệu
- Font chữ: Source Serif (Tiêu đề) + Inter (Nhãn nhỏ)

**Topological Transit Map Bản đồ Mạng lưới Lộ trình Đơn giản hóa Thể tạo** `Trầm tĩnh · Phục hồi 86%`
- Tham khảo: Bản đồ tàu điện ngầm London 1933 của Harry Beck, Bản đồ tàu điện ngầm New York 1972 của Massimo Vignelli
- Thích ứng: Mạng lưới quy trình và mối quan hệ, Kiến trúc tổ chức, Thể tạo hệ thống, Bất kỳ hình nào "Mối quan hệ kết nối quan trọng hơn khoảng cách thực tế"
- DNA thị giác: Phối màu=Nền trắng hoặc nhạt+Mỗi đường một màu thuần bão hòa cao. Master=Tất cả đoạn đường chỉ đi 0/45/90°, Trạm cách đều——**Hy sinh tính chính xác địa lý để đổi lấy khả năng đọc** là chỗ đứng của trường phái này; Điểm chuyển tuyến dùng hình tròn rỗng. Biểu tượng=Ràng buộc 8 hướng, Node cách đều, Đường màu thuần, Node rỗng
- Thực thi HTML: SVG polyline inline ràng buộc nghiêm ngặt góc; Node dùng circle. Hình học thuần túy, HTML tự nhiên tương thích
- Font chữ: Jost / Inter (Tên trạm có thể viết hoa toàn bộ)

**Ma & Emptiness Infographic Khoảng trắng của Trắng (Kiểu Hara Kenya)** `Trầm tĩnh · Phục hồi 82%`
- Tham khảo: 《Trắng》 của Hara Kenya và hệ thống thị giác MUJI, "Khoảng trắng không phải là trống rỗng, mà là vật chứa đựng sự tưởng tượng"
- Thích ứng: Báo cáo năm thương hiệu, Tự sự nhịp điệu chậm, Dữ liệu ít nhưng quan trọng, Trường hợp cần cảm giác trang trọng
- DNA thị giác: Phối màu=Trắng thuần hoặc Trắng giấy tuyên+Xám cực nhạt+Một điểm màu mực hoặc màu chu sa cực nhỏ. Master=Một màn hình một dữ liệu, Khoảng trắng siêu lớn, Phần tử đặt ở mép hoặc vị trí hoàng kim, Tuyệt đối không lấp đầy. Biểu tượng=Khoảng trắng cực đoan, Nhấn mạnh đơn điểm, Đường mảnh như không có, Cảm giác "Khoảng" kiểu phương Đông
- Thực thi HTML: Trình bày CSS thuần. 🔴 **Loại có rủi ro cao nhất trong thư viện này**——Khoảng trắng bắt buộc phải là bố cục (Có điểm neo thị giác rõ ràng), làm quá đà chính là "Trang web bị hỏng render"; Chữ chính vẫn phải ≥14px, không được vì khí chất mà thu nhỏ cỡ chữ
- Font chữ: Noto Serif SC / Source Han Serif + Inter

**Bookcraft Data Trình bày Dữ liệu Cấp sách (Kiểu Irma Boom)** `Trầm tĩnh · Phục hồi 80%`
- Tham khảo: Irma Boom (Nhà thiết kế sách Hà Lan, tác phẩm vào bộ sưu tập vĩnh viễn của MoMA, nổi tiếng với trình bày chữ cực đoan và thử nghiệm chất liệu)
- Thích ứng: Báo cáo dữ liệu dài tập, Báo cáo năm cần được sưu tầm như ấn phẩm, Nội dung sâu sắc pha trộn hình chữ
- DNA thị giác: Phối màu=Màu giấy+Một đến hai màu mực in, Khối màu đè diện tích lớn. Font chữ=Bản thân trình bày chữ chính là nhân vật chính, Biến độ cỡ chữ cực lớn, Lề trang không đối xứng, Văn bản có thể xếp dọc hoặc xoay. Master=Đưa dữ liệu nhúng vào luồng văn bản, Trang chương dùng khối màu toàn bản phân cách. Biểu tượng=Bản tâm không đối xứng, Biến độ cỡ chữ cực đoan, Thử nghiệm trình bày, Chất lượng ấn phẩm
- Thực thi HTML: CSS nhiều cột column-count hoặc writing-mode có thể làm xếp dọc; Bản tâm không đối xứng dùng Grid. **Không làm được là chất liệu giấy và cắt xén**, trên màn hình phải dựa vào sức căng trình bày để bù đắp
- Font chữ: Fraunces / EB Garamond + Archivo (Tương phản)

**Scientific Figure Plate Bản hình Khoa học** `Trầm tĩnh · Phục hồi 96%`
- Tham khảo: Quy chuẩn figure của Nature và Science, Bản hình khoa học công khai của USGS và NASA
- Thích ứng: Kết luận nghiên cứu, So sánh phương pháp, Dữ liệu nghiêm ngặt cần chất lượng phản biện đồng nghiệp, Tổ hợp nhiều hình con
- DNA thị giác: Phối màu=Nền trắng+Bảng màu thân thiện với mù màu (Dùng tương phản xanh cam chứ không dùng đỏ xanh lá)+Thang xám. Master=Hình con dùng đánh số (a)(b)(c), Chú thích hình thành đoạn dưới hình, Thanh sai số và dung lượng mẫu bắt buộc ghi, Trục tọa độ mang đường vạch. Biểu tượng=Đánh số hình con, Chú thích hình dài dưới hình, Thanh sai số, An toàn với mù màu, Zero trang trí
- Thực thi HTML: CSS Grid xếp hình con+SVG vẽ hình và thanh sai số; Chú thích hình dùng đoạn văn chữ nhỏ. 0 vật liệu, HTML hoàn toàn gánh vác được
- Font chữ: Inter / Source Sans + Roboto Mono (Con số)

**Swiss Grid Report Báo cáo Năm Grid Thụy Sĩ (Kiểu Müller-Brockmann)** `Trầm tĩnh · Phục hồi 97%`
- Tham khảo: Josef Müller-Brockmann 《Grid Systems in Graphic Design》, Trường phái Ulm, Truyền thống báo cáo năm chủ nghĩa quốc tế Thụy Sĩ
- Thích ứng: Báo cáo năm doanh nghiệp, Báo cáo cơ quan, Tài liệu series cần dùng lâu dài một bộ bố cục
- DNA thị giác: Phối màu=Nền trắng+Đen+Màu nhấn mạnh duy nhất. Master=Grid mô đun nghiêm ngặt (Thường 12 cột), Tất cả phần tử hít vào đường cột, Căn trái bằng đầu không bằng đuôi, Lượng lớn khoảng trắng có nhịp thở nhưng không trống. Biểu tượng=Logic grid có thể nhìn thấy, Hệ Helvetica, Trình bày không căn giữa, Cấp độ dựa vào cỡ chữ và khoảng cách chứ không dựa vào trang trí
- Thực thi HTML: CSS Grid ánh xạ trực tiếp grid cột, là loại đồng cấu nhất với HTML trong thư viện này. 0 vật liệu
- Font chữ: Inter / Archivo (Thay thế Helvetica)


---

## ⚠️ Phong cách Dành riêng cho Tạo hình AI (Chỉ tiến cử khi xác nhận người dùng có năng lực tạo hình, default KHÔNG ĐƯỢC chọn)

Linh hồn của những phong cách dưới đây nằm ở **Tạo thị giác động / 3D / Hạt / Quang ảnh cấp điện ảnh / Minh họa vẽ tay**, dưới HTML/CSS thuần không có tạo hình AI chỉ có thể làm ra bản mock suy thoái nghiêm trọng, **loại khỏi pool tiến cử mặc định**. Chỉ khi người dùng chỉ định rõ ràng có năng lực tạo hình (Đi theo `huashu-gpt-image`) mới làm ứng viên:

| Phong cách | Linh hồn | Tại sao HTML không làm được |
|------|------|------------------|
| Active Theory (WebGL Hạt) | Hệ thống hạt 3D/Render thời gian thực | CSS thuần không thể |
| Field.io (Nghệ thuật Tạo ra) | Hình ảnh thuật toán tạo ra | SVG tĩnh chỉ có thể làm bản đơn giản hóa cứng nhắc |
| Resn (Tương tác Minh họa) | Minh họa nhân vật+Game hóa | Phụ thuộc vào vật liệu vẽ tay |
| Zach Lieberman (Tạo ra Thời gian thực) | Nét vẽ creative coding | Phụ thuộc vào tạo ra thời gian thực |
| Raven Kwok (Tham số Phân hình) | Phân hình đệ quy | CSS không làm ra được độ phức tạp |
| Ash Thorp (Quang ảnh Điện ảnh) | Ánh sáng thể tích cấp điện ảnh/Mỹ thuật concept | Quang ảnh CSS là suy thoái |
| Territory Studio (FUI Toàn hình) | Giao diện toàn hình viễn tưởng | Phụ thuộc lượng lớn vật liệu phát sáng xếp chồng |
| Neo Shen (Thủy墨 Loang) | Thủy墨 loang hữu cơ | Chuyển sắc CSS≠Thủy墨 |
| Sagmeister & Walsh (Bùng nổ Màu sắc) | Đồ thực thủ công+Trình bày thử nghiệm | Khung va chạm màu có thể làm (Đã gộp vào Web 「Memphis」 và PPT 「Poster Va chạm Màu Đơn sắc」), Chất lượng thủ công không làm được |

> Những mẫu này không phải "Không tốt", mà là "Vật chứa không đúng"——Vật chứa nguyên bản của chúng là hình ảnh AI xuất trực tiếp, không phải DOM trình duyệt.

---

## Khu vực Cấm Thẩm mỹ Mặc định (Người dùng có thể override theo thương hiệu của mình)

- ❌ **Giải pháp lười biếng GitHub-dark**: Nền xanh sâu đều (#0D1117)+Glow neon xanh ngọc/tím thông thường——Chỉ cấm bộ tổ hợp nát đường phố này, không phải "Màu tối cấm toàn bộ"
- ✅ **Không nằm trong khu vực cấm**: Quang ảnh kịch tính cấp điện ảnh, Cyber màu ấm (Cam/Xanh ngọc Ash Thorp), Tự sự trường tối thơ ca chuyển động——Màu tối có ý đồ tác giả được giữ lại (Trong thư viện này 「Linear 暗色发光」 「黑底数字剧场」 「CS50 糖果舞台」 đều là màu tối hợp pháp)
- ❌ Công thức vạn năng chuyển sắc tím dữ dội, emoji làm icon, card bo góc+accent viền màu bên trái (Trừ khi bản thân thương hiệu sử dụng)
- ❌ Hình bìa thêm chữ ký cá nhân/watermark

---

## Tâm pháp Prompt khi có Năng lực Tạo hình AI (Mood, Not Layout)

> Chỉ áp dụng khi đi theo đường dẫn tạo hình AI; Đường dẫn HTML trực tiếp viết code theo "Thực thi HTML" của từng phong cách ở trên.

Prompt ngắn > Prompt dài. Mô tả cảm xúc và nội dung, hiệu quả hơn chất 30 dòng chi tiết bố cục.

| Cách viết giết chết sự đa dạng | Cách viết kích hoạt sức sáng tạo |
|----------------|----------------|
| Chỉ định tỷ lệ màu sắc (60%/25%/15%) | Mô tả cảm xúc ("warm like Sunday morning") |
| Quy định vị trí bố cục | Trích dẫn mỹ học cụ thể ("Pentagram editorial feel") |
| Liệt kê tất cả phần tử thị giác | Mô tả khán giả nên cảm nhận được điều gì |

Phương pháp luận tạo hình AI hoàn chỉnh → skill `huashu-gpt-image`.

---

**Phiên bản**: v3.1 (06-2026 Tái cấu trúc thành thư viện HTML nguyên bản; 08-2026 Bổ sung phân khu Infographic, 40 → 60 loại)
**Áp dụng**: Đường dẫn HTML mặc định của tất cả các thiết kế thị giác như Web/PPT/PDF/Infographic/Bìa/App v.v.
