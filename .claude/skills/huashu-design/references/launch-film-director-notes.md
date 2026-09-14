# Quy trình Launch Film: Viết director's notes vạn chữ trước, làm animation sau

> Quy trình làm việc tiêu chuẩn dành cho tác phẩm thị giác quy chuẩn cao (≥ 20 giây, chứa tự sự thương hiệu, chứa slogan reveal, có khả năng đẩy lên X / WeChat Official Account / Bilibili để quảng bá).
>
> Điều kiện kích hoạt: Nhiệm vụ là "Phim quảng cáo nâng cấp sản phẩm / launch film thương hiệu / launch trailer / superbowl-tier ad / brand campaign / video hero animation", và **người dùng có kỳ vọng rõ ràng về chất lượng** (như "cảm giác chất lượng Super Bowl", "chi tiết 10x", "cấp độ Apple").
>
> Kích hoạt ngược: Đừng dùng quy trình này khi "làm nhanh một animation demo", "motion graphic đơn giản", "animation icon đơn lẻ"——sẽ bị quá tải kỹ thuật (over-engineering).

---

## 1. Tại sao phải viết director's notes trước

Bài học thực chiến (Dự án huashu-md-html v2.0 ngày 11-05-2026):

Vòng 1 trực tiếp ra tay viết HTML, thành phẩm là "animation từ góc nhìn lập trình viên"——mỗi capability trung bình dốc lực, nhịp điệu tốc độ đều, slogan va vào nhau, thiếu đi cung tự sự.
Vòng 2 nhận được chỉ thị của người dùng "Dừng lại, viết kịch bản phân cảnh 1 vạn chữ theo góc nhìn đạo diễn Apple trước", đã viết v5-director-notes.md (11500 chữ, 13 cảnh shot-by-shot spec), sau đó thực thi theo kịch bản——một lần qua luôn, mỗi khung hình pause đều chịu ngắm, nhịp điệu có thăng trầm climax.

**Sự chênh lệch cốt lõi**: Viết kịch bản là think, viết HTML là execute. Thấu hiểu think trước rồi thì execute chỉ là dịch thuật cơ khí. Execute trước thì mỗi shot đều là quyết định ứng biến tại chỗ, tất yếu sẽ loạn.

Viết director's notes không phải là "làm màu", mà là kết tinh tất cả quyết định thị giác **trước khi ra tay** thành tài liệu——mỗi một cảnh đều đã được visualize, reasoning và trace với ngữ cảnh trong đầu. Khi thực thi HTML không cần phải làm quyết định sáng tạo nữa, chỉ cần trung thực dịch thuật.

---

## 2. Đánh giá kích hoạt (Tự hỏi bản thân 3 câu hỏi trước)

Trước khi khởi động quy trình launch film hãy hỏi:

1. **Thước phim này có gánh vác tự sự thương hiệu không?** (Có thesis / slogan reveal / cảm giác nghi thức nâng cấp) —— Có → Đi theo quy trình director's notes
2. **Khán giả sẽ tạm dừng để xem chứ?** (Có thể chụp màn hình, làm poster X, làm ảnh bìa, review tốc độ chậm) —— Có → Mỗi khung hình phải chịu được sự ngắm nhìn
3. **Khách hàng/người dùng có tham chiếu "Tôi hy vọng giống như XXX"?** (Apple / Anthropic / Nike / Penguin / Đạo diễn nào đó) —— Có → Bắt buộc phải làm rõ ngữ cảnh thị giác

Bất kỳ câu nào là "Có" thì đi theo quy trình. Cả ba câu đều "Không" thì bỏ qua, trực tiếp dùng quy trình tiêu chuẩn của [animations.md](animations.md).

> 🔴 **Cửa tiền đề (Trước quy trình này)**: launch film cũng bắt buộc phải qua cửa cứng 3 hướng của SKILL.md trước——mỗi hướng một "bảng hướng đi" (khung hình tĩnh thực tế keyframe hero + bảng màu + câu khí chất + tham chiếu), sau khi người dùng chọn định hướng, director's notes vạn chữ mới triển khai xung quanh hướng đã chọn. Việc chỉ định các từ phong cách như "cấp Apple" không được miễn trừ (Được HuaStudio xác nhận thực tế ngày 18-07-2026).

---

## 3. Cấu trúc 5 phần lớn của Director's Notes

Director's notes vạn chữ (10000-12000 chữ Tiếng Trung / số lượng tương đương Tiếng Anh) bắt buộc phải bao gồm 5 phần lớn này. **Thiếu bất kỳ phần nào đều thuộc về không hoàn chỉnh, chất lượng sẽ bị ảnh hưởng**.

### Part I · Director's Statement (Tuyên ngôn sáng tác, khoảng 1500-2000 chữ)

Trả lời 5 câu hỏi:

1. **Bộ phim này không phải là cái gì?** (Loại trừ rõ ràng——như "Đây không phải là phim giới thiệu tính năng", "Không phải là demo")
2. **Thesis cốt lõi trong một dòng**——Khán giả xem xong chỉ nhớ một câu là câu nào?
3. **Đối thoại với ngữ cảnh của ai?**——Liệt kê 5-8 tham chiếu thị giác (Đạo diễn / Nhà thiết kế / Thương hiệu / Nhiếp ảnh gia / Tên tác phẩm + Năm), giải thích mỗi tham chiếu học được cái gì
4. **Chân dung 3 loại khán giả + Lời hứa cho mỗi loại**: Khán giả chính / Khán giả phụ / Khán giả bên ngoài, mỗi loại tương ứng một đoạn
5. **Triết lý nhịp điệu**——Diễn giải đường cong nhịp chậm / tăng tốc / đỉnh cao / thu chậm + emotional climax ở giây thứ mấy (**Không nhất thiết là giây cuối cùng**)

Cuối cùng thêm một đoạn anti-slop checklist: **Những việc bộ phim này KHÔNG LÀM** (Liệt kê cụ thể, không mờ mịt).

### Part II · Visual System (Toàn phổ hệ thống thị giác, khoảng 1500-2500 chữ)

Đây là spec thị giác đã kỹ thuật hóa. Sau khi hoàn chỉnh bất kỳ người thực thi nào nhận được đều có thể tạo ra thị giác thống nhất.

Các mục con bắt buộc phải có:

- **Bảng màu hoàn chỉnh**: Ít nhất 8-10 màu, mỗi màu bao gồm HEX + Định nghĩa chức năng + Giới hạn tỷ lệ chiếm màn hình
- **Hệ thống font chữ**: Ít nhất 6 cấp độ cỡ chữ, mỗi cấp độ bao gồm tên font + weight + size + letter-spacing + mục đích
- **Hệ thống grid**: Kích thước canvas + lề ngoài + column grid + baseline grid + vùng an toàn mấu chốt + mỏ neo tỷ lệ vàng
- **Hệ thống animation**: Thư viện easing (Trong vòng 4 đường) + Từ điển duration + Quy tắc stagger + Quy tắc chuyển cảnh scene
- **Phần tử Chrome**: Các chi tiết nhỏ chạy xuyên suốt toàn bộ phim (counter / chip / ticker / watermark / texture), mỗi cái bao gồm vị trí + thời điểm vào/thoát cảnh
- **Hệ thống âm thanh**: Đường hướng BGM 30 giây (Phân lớp) + Từ điển SFX (10+ cues bao gồm mã thời gian + âm lượng + cách ly dải tần)
- **Checklist anti-AI slop**: Bảng tự kiểm tra per-shot (10-15 mục)

Quy tắc sắt: **Tất cả quyết định thị giác đều suy ra từ Visual System, không tạm thời phát minh giá trị mới trong shot list**.

### Part III · Story Arc (Cung câu chuyện, khoảng 500-800 chữ)

Cấu trúc 3 hồi + Đường cong cảm xúc:

- **Act I · SETUP** (0 → Thời lượng 1/5 đầu, e.g. 0-6s cho 30s): Khán giả tiến vào, vấn đề được đưa ra
- **Act II · ESCALATION** (2/3 ở giữa): Đáp án triển khai, chủ đề trải ra
- **Act III · PAYOFF** (1/4 cuối): Thăng hoa, slogan reveal, con dấu thương hiệu

Bao gồm biểu đồ đường cong cảm xúc ASCII + Đánh dấu thời điểm emotional climax.

**Quyết định mấu chốt**: climax không nhất thiết ở cuối. Phim 30s climax thường ở 22-25s (Không phải 29s)——mấy giây cuối cùng là resolution / decay, không phải peak. Quy tắc này vi phạm tất yếu làm tác phẩm "đầu voi đuôi chuột".

### Part IV · Shot-by-Shot Storyboard (Kịch bản phân cảnh, khoảng 5000-7000 chữ · Chiếm 60% dung lượng)

Mỗi cảnh bao gồm 11 trường (Không thể thiếu cái nào):

```
SHOT NN · NAME
[TIMECODE]    Thời gian bắt đầu kết thúc + Thời lượng
[FUNCTION]    Chức năng của cảnh này trong cung câu chuyện (Một câu)
[VISUAL]      Bố cục màn hình + Vị trí phần tử + Hướng chuyển động
[CAMERA]      Cảnh biệt (Xa/Toàn/Trung/Gần/Đặc tả, tương ứng nấc zoom) + Hành động vận kính + Một câu động cơ; "Đứng yên" cũng phải viết tại sao đứng yên; push-in bắt buộc phải viết mỏ neo cụ thể (Từ vựng và ngân sách xem camera-language.md, hệ thống cảnh biệt xem storyboard-basics.md §3)
[TYPE]        Spec trình bày chữ (Font / Cỡ chữ / Khoảng cách chữ / Chiều cao dòng / Màu sắc / Căn chỉnh)
[ANIM]        Thời điểm in/out của mỗi phần tử + easing + duration + stagger + delay
[AUDIO]       music beat + SFX cue (Mỗi cảnh tương ứng nhịp BGM + Bắt buộc chứa thời gian biểu SFX)
[CHROME]      Trạng thái phần tử bốn góc (Chrome nào có mặt / Chrome nào fade in/out / Chrome nào pulse)
[ANTI-SLOP]   Cảnh này đã vượt qua những mục tự kiểm tra nào + Có chữ ký chi tiết 120% gì
[WHY]         Nối tiếp logic của cảnh trước + Thúc đẩy móc câu của cảnh sau
```

**Trường trung bình 30-80 chữ → Mỗi cảnh 400-700 chữ → 12-15 cảnh → 5000-7000 chữ**.

Kinh nghiệm thực chiến: Sau khi viết xong storyboard **tự mình đọc lại một lần**——Bất kỳ một cảnh nào xóa đi, toàn bộ thước phim có còn thành lập không? Nếu có thể xóa, cảnh đó chính là dư thừa, xóa đi.

### Part V · Production Manifest (Danh mục sản xuất, khoảng 800-1200 chữ)

Danh mục giao hàng kỹ thuật:

- URL tải font chữ (Chứa preconnect)
- CSS Variables (Có thể dán trực tiếp)
- Tiêu chuẩn lựa chọn nguồn BGM + Từ khóa prompt Suno/Udio + Thư viện dự phòng
- Từ điển SFX (Liệt kê đường dẫn file + âm lượng từng cue theo mã thời gian)
- **Kế hoạch xác minh khung hình khóa**: 12-15 mã thời gian khung hình khóa pause-and-check, liệt kê các mục xác minh từng khung hình (fonts / positions / chrome state)
- Tham số ghi hình (fps / codec / bitrate / preset)
- Lệnh trộn âm thanh ffmpeg (Chứa xác minh audio stream)
- Danh mục thành phẩm giao hàng (mp4 / mp4-60fps / gif / poster.png / silent.mp4 / shot-list.csv)
- Ước tính thời gian toàn chuỗi (Độ chính xác cấp giờ)

---

## 4. 5 Lời khuyên khi viết director's notes

**4.1 Dùng giọng điệu của đạo diễn, không dùng giọng điệu của PM**

❌ "This shot displays the product features."
✅ "This is the hero shot — if the audience pauses anywhere, I want it to be here."

Ghi chú đạo diễn là viết cho người thực thi đọc, nhưng cũng là viết cho chính bản thân trong tương lai đọc. Ngôi thứ nhất + diễn đạt judgment để lại nhiều manh mối quyết định hơn diễn đạt description.

**4.2 Trích dẫn tác phẩm cụ thể (Chứa năm), không chỉ là tên trường phái**

❌ "Apple-inspired"
✅ "Apple 'Designed by Apple in California' (2013, dir. Mark Romanek) — Học là nhịp chậm + font serif + nền trắng lớn"

Lợi ích của việc trích dẫn tác phẩm cụ thể: (a) Bất kỳ khán giả nào cũng có thể lên mạng tìm đối chiếu (b) Bạn tự ép bản thân nghĩ rõ ràng kỹ thuật cụ thể học được là gì (c) Phóng tránh "cảm hứng mờ mịt".

**4.3 Mỗi quyết định đều trace về first principle**

Toàn bộ thước phim có một câu first principle (Như "Markdown is the new typewriter."). Mỗi quyết định cụ thể——Phối màu / Font chữ / Nhịp điệu / Chrome——đều phải có thể trace về câu nói này.

Quyết định không trace về được chính là trang trí, xóa đi.

**4.4 Viết anti-slop quan trọng hơn viết do-this**

Danh mục "Những việc bộ phim này KHÔNG LÀM" (Chuyển sắc tím / emoji / Lorem ipsum / Inter display / SVG vẽ nhân vật / Card bo góc + viền nhấn bên trái) bảo vệ chất lượng tốt hơn danh mục "Những việc bộ phim này LÀM".

Quyết định hướng dương là vô hạn, checklist hướng âm là hữu hạn——nhưng checklist hướng âm một khi vi phạm chính là slop.

**4.5 Viết xong đừng thực thi ngay lập tức——Đọc lại sau 30 phút**

Khi viết bộ não ở "chế độ sản xuất", không nhìn thấy inconsistency. Đọc lại storyboard chính mình viết sau 30 phút, sẽ phát hiện:
- Hai cảnh nào đó chức năng bị trùng lặp (Xóa một cảnh)
- Cảnh nào đó tự sự nhảy vọt quá lớn (Thêm chuyển tiếp)
- Vị trí emotional climax bị sai (Dịch chuyển)
- Phần tử chrome và số lượng shot không khớp nhau (Căn chỉnh lại)

30 phút này tiết kiệm được 2 giờ làm lại ở giai đoạn sau.

---

## 5. Quy trình thực thi Director's Notes → HTML

Sau khi viết xong director's notes, các bước thực thi HTML:

1. **Tái sử dụng starter components** (`assets/animations.jsx` Stage/Sprite/Easing/interpolate) — Không phát minh lại
2. **Dán trực tiếp CSS Variables từ Visual System Part II** — Không tạm thời đổi màu trong HTML
3. **Đối chiếu mã thời gian Part IV theo timeline Sprite start/end** — Không tự ý thêm cảnh
4. **Tách phần tử chrome thành component độc lập** (ChromeA/B/C/D), dùng useTime() điều khiển chuyển trạng thái
5. **Nội dung destination cards bắt buộc phải thực tế đọc được** (Không phải fake bar lines) —— Đây là chữ ký chi tiết 120% được nhắc đi nhắc lại nhiều nhất trong dự án v5
6. **Mỗi khi viết xong một cảnh lập tức chụp khung hình khóa để xác minh** (Dùng tham số URL `?t=NN` + Playwright), đừng viết xong toàn bộ phim mới xác minh thống nhất

---

## 6. Quy trình xác minh khung hình khóa

Thực thi tham số URL (Bắt buộc phải thêm trong component Stage):

```js
const urlMatch = window.location.search.match(/[?&]t=([\d.]+)/);
const frozenTime = urlMatch ? parseFloat(urlMatch[1]) : null;
const [time, setTime] = useState(frozenTime != null ? frozenTime : 0);
const [playing, setPlaying] = useState(frozenTime == null);
```

→ Như vậy `file:///path/animation.html?t=14.5` sẽ freeze trực tiếp tại giây 14.5.

Chụp ảnh màn hình hàng loạt:

```bash
for t in 0.5 2.5 4.9 7.0 10.5 13.5 16.5 19.0 21.5 23.4 25.5 28.0 29.9; do
  npx -y playwright screenshot \
    "file://$PWD/animation.html?t=$t" \
    "keyframes/t-$t.png" \
    --viewport-size=1920,1136 \
    --wait-for-timeout=2500
done
```

Mỗi tấm ảnh chụp màn hình bắt buộc phải xác minh:
- [ ] Phần tử không tràn ra ngoài canvas 1920×1080
- [ ] Khoảng cách chữ, chiều cao dòng visually correct (Không chèn ép, không phân tán)
- [ ] Chi tiết typography mấu chốt (Màu dấu chấm / em-dash / italic / small caps) có thể nhận biết
- [ ] Vị trí + Trạng thái phần tử chrome đúng đắn
- [ ] Vượt qua checklist anti-AI slop
- [ ] Tồn tại chi tiết 120% "đáng xem khi pause"

---

## 7. Chiến lược song song nhiều góc nhìn (Nâng cao)

Dự án phức tạp (Như launch film không chọn ra được hướng / Muốn xem nhiều sự chênh lệch mỹ học / Khách hàng chưa chốt phong cách) có thể **khởi động nhiều subagent song song làm các phiên bản góc nhìn đạo diễn khác nhau**.

Cấu hình thực chiến (Dự án huashu-md-html ngày 11-05-2026, song song 6 phiên bản):

```
v5  · Đường cơ sở (Gu nhà xuất bản Anthropic / Penguin Classics)
v5a · Wes Anderson (Đối xứng + Hoài cổ + Thẻ chương)
v5b · Saul Bass (Cắt giấy + Chữ lớn thập niên 60 + Cắt gọt hình học)
v5c · Vương Gia Vệ (Font serif Tiếng Trung + Slow motion + Hoài niệm)
v5d · Massimo Vignelli (Grid chủ nghĩa hiện đại + Đỏ đen)
v5e · Hara Kenya (Cực giản kiểu Nhật + Khoảng trống)
v5f · Kusama Yayoi (Chấm tròn + Lặp lại + Một màu mạnh duy nhất)
```

Mỗi subagent nhận được brief độc lập:
- Bối cảnh dự án (Cùng một bản)
- Tham khảo bắt buộc đọc (Cùng bản v5-director-notes.md làm template phương pháp luận)
- **DNA nghệ sĩ được chỉ định** (Bảng màu / Font chữ / Ngôn ngữ thị giác / Nhịp điệu / Phần tử chiêu bài / Phiên bản tăng cường anti-slop, mỗi mục 30-50 chữ)
- Danh mục nhiệm vụ thống nhất (director-notes.md + animation.html + keyframes/ + README.md)
- Ràng buộc thống nhất (30s / 1920×1080 / file:// / Google Fonts)

Khởi động song song + Chạy ngầm, khoảng 30-60 phút ra 6 bộ phiên bản hoàn chỉnh.

Sau khi hoàn thành thẩm định đối chiếu:
1. Bảng quyết định mỹ học cốt lõi của các phiên bản
2. Đối chiếu khung hình khóa xếp song song (Mỗi bản một khung hình cùng thời điểm)
3. Bỏ phiếu: Cái nào phù hợp nhất với nhu cầu thực tế của người dùng

**Mấu chốt**: Đừng để các subagent tham khảo lẫn nhau——Chúng bắt buộc phải độc lập tạo ra, nếu không sẽ va vào "giá trị trung bình". Trong chỉ thị của mỗi subagent phải nói rõ "Đừng lặp lại mỹ học của v5".

---

## 8. Các kịch bản kích hoạt điển hình

| Kịch bản người dùng | Có kích hoạt không | Ghi chú |
|---------|---------|------|
| "Làm một phim quảng cáo nâng cấp SaaS" | ✅ Kích hoạt | Mặc định đi theo quy trình hoàn chỉnh |
| "Video cấp Apple / cảm giác chất lượng Super Bowl" | ✅ Kích hoạt + Nâng cấp | Khuyến nghị mạnh mẽ song song nhiều góc nhìn |
| "Brand launch film 30 giây" | ✅ Kích hoạt | |
| "Dự án này làm kịch bản 1 vạn chữ rồi làm animation" | ✅ Kích hoạt | Người dùng chỉ định rõ ràng |
| "Motion graphic đơn giản, xoay logo một chút" | ❌ Không kích hoạt | Dùng quy trình tiêu chuẩn animations.md |
| "Làm một animation demo onboarding" | ❌ Không kích hoạt | Dùng animations.md |
| "Video hướng dẫn có lồng tiếng" | ❌ Không kích hoạt | Đi theo voiceover-pipeline.md |
| "Một hero animation đơn lẻ" | ⚠️ Xem độ phức tạp | Nếu là hero quy chuẩn cao, kích hoạt; hero thông thường dùng hero-animation-case-study.md |

---

## 9. Mẫu tham khảo

Mẫu tham khảo director's notes hoàn chỉnh (self-contained, trong skill này):

`assets/director-notes-samples/launch-film-30s-sample.md` (Khoảng 78KB · 11500 chữ · 13 cảnh · 5 phần lớn đầy đủ)

Vị trí dự án gốc (Chứa HTML thực thi + Khung hình khóa tương ứng):

- v5-director-notes.md (director's notes, máy cục bộ tác giả, không phân phối theo kho lưu trữ)
- v5-six-forms.html (Thực thi HTML, máy cục bộ tác giả, không phân phối theo kho lưu trữ)
- v5-keyframes/ (Ảnh chụp xác minh khung hình khóa, máy cục bộ tác giả, không phân phối theo kho lưu trữ)

Khi viết dự án mới khuyến nghị mạnh mẽ **Read mẫu này trước**, hiểu được khối lượng công việc và mật độ chi tiết, rồi mới quyết định có đi theo quy trình toàn bộ hay không.

---

## 10. Anti-pattern (Đừng làm như thế này)

❌ **Viết bản director's notes tinh giản 1000 chữ rồi ra tay**
→ Bản tinh giản tất yếu bỏ sót một mục con nào đó của Visual System, dẫn đến khi thực thi HTML không ngừng quay lại bổ sung spec. Đã làm thì làm cấp vạn chữ, muốn tiết kiệm thì bỏ qua trực tiếp.

❌ **storyboard chỉ viết 5-8 cảnh**
→ Phim 30 giây ít nhất 12-15 cảnh (Mỗi cảnh 2-3 giây). Cảnh ít = Nhịp điệu tốc độ đều = Không có climax.

❌ **director's notes viết xong là giao hàng, không thực thi**
→ Tài liệu không phải thành phẩm giao hàng, animation mới là thành phẩm. Giao hàng tài liệu + animation cùng nhau, tài liệu làm phụ lục "Căn cứ thiết kế".

❌ **Khi song song nhiều góc nhìn để subagent xem phiên bản khác**
→ Các subagent bắt buộc phải độc lập, nếu không sẽ xu hướng giống nhau. Giai đoạn thẩm định mới đối chiếu.

❌ **Bỏ qua xác minh khung hình khóa trực tiếp ghi hình MP4**
→ Tất yếu phải làm lại. Xác minh khung hình khóa là quality gate rẻ nhất.

❌ **Trì hoãn quyết định chi tiết animation đến "Đợi lúc tôi ghi hình rồi nghĩ"**
→ Giai đoạn ghi hình là thực thi cơ khí, không thể làm quyết định sáng tạo. Tất cả quyết định bắt buộc phải viết chết trong director's notes.

---

*Hiệu chỉnh lần cuối: 11-05-2026*
*Trường hợp thực tế: huashu-md-html v2.0 launch film (v5-director-notes.md)*
