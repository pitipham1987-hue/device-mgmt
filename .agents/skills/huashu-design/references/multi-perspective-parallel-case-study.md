# Thí nghiệm Song song Đa góc nhìn · Nghiên cứu tình huống (Case Study)

> Dự án huashu-md-html v2.0 launch film · 11-05-2026
> Đạo diễn notes (director's notes) + HTML + Thí nghiệm khung hình chính (keyframe) song song dưới góc nhìn của 6 nghệ sĩ

---

## Bối cảnh

Khi người dùng yêu cầu "Làm một video quảng bá nâng cấp 30 giây cho huashu-md-html v2.0", luồng chính (main thread) đã sản xuất bản v5 cơ sở trước (Gu thẩm mỹ Anthropic / Penguin Classics Publisher). Tuy nhiên người dùng cho rằng có thể làm tốt hơn nữa và đưa ra chỉ dẫn quan trọng (critical instruction):

> "Gọi các subagent khác nhau để tạo ra 6 phiên bản thể hiện và thiết kế thị giác hoàn toàn khác nhau. Bạn có thể thử kích hoạt các đạo diễn và nghệ sĩ khác nhau. Sau khi tất cả hoàn thành thì tiến hành đánh giá và thẩm định."

Đây là thí nghiệm "Đạo diễn notes song song đa góc nhìn" mang tính hệ thống lần đầu tiên, kiểm chứng một bộ quy trình làm việc có thể tái sử dụng.

---

## Logic Lựa chọn 6 Góc nhìn

Không chọn bừa 6 nhà thiết kế — Họ bắt buộc phải **có độ khác biệt thị giác cực cao**, tránh xu hướng giống nhau.

6 góc nhìn được lựa chọn cuối cùng (bao gồm lý do lựa chọn):

| Góc nhìn | Trường phái | Điểm neo thẩm mỹ | Sự khác biệt so với các góc nhìn khác |
|----------|-------------|------------------|---------------------------------------|
| **v5 Cơ sở** | Nhà xuất bản hiện đại | Anthropic Terracotta Orange + Penguin Classics Serif + Grid Vignelli | Lựa chọn "Gu thẩm mỹ" an toàn |
| **v5a Wes Anderson** | Thẩm mỹ chương phim | Cảm giác tạp chí The French Dispatch + Danh mục công nghiệp Olivetti 1960 | Bố cục đối xứng + Thẻ chương + Khung viền trang trí |
| **v5b Saul Bass** | Nghệ thuật tiêu đề phim 60s | cut-paper + Trajan caps + Hình học dòng chảy | Silhouette cắt giấy + Chữ lớn + Đường chéo mạnh |
| **v5c Vương Gia Vệ** | Chấn động mới Hồng Kông | *Tâm Trạng Khi Yêu*, *2046* letterboxing + Tiếng Trung Serif | Nhịp chậm + Quầng sáng sương mù + Tiếng Trung làm chủ đạo |
| **v5d Massimo Vignelli** | Chủ nghĩa hiện đại 1970 | Cẩm nang thương hiệu Knoll + Bản đồ tàu điện ngầm NYC | Lưới grid nghiêm ngặt + Quy tắc sắt 3 màu + Từ chối trang trí |
| **v5e Kenya Hara** | Nhật Bản tối giản | Poster MUJI + *White* | Triết lý khoảng trống + Không chrome + Khoảng lặng ma |
| **v5f Yayoi Kusama** | Nghệ thuật sắp đặt | Infinity Mirror Rooms + Polka Dot Obsession | Lặp đi lặp lại mang tính ám ảnh + Một màu mạnh đơn lẻ + Họa tiết chấm bi |

**Nguyên tắc lựa chọn**:
1. **3 nền văn hóa địa lý khác nhau** (Phim điện ảnh phương Tây / Thiết kế Nhật Bản / Tiếng Trung Hồng Kông)
2. **3 thời đại khác nhau** (1960s / 1970s / 2010s+)
3. **3 vật chứa khác nhau** (Phim điện ảnh / Thiết kế đồ họa / Nghệ thuật sắp đặt)
4. **Mỗi góc nhìn đều có "Chữ ký thị giác hoàn toàn trái ngược với thẩm mỹ SaaS thông thường trong dữ liệu huấn luyện"**

---

## Quy trình Triển khai

### Step 1 · Viết brief độc lập cho từng góc nhìn (Khoảng 15 phút)

Mỗi brief bao gồm 8 trường cố định:

```
1. Bối cảnh dự án (Giống nhau)
2. Tham khảo bắt buộc đọc (Cùng 1 file v5-director-notes.md làm template phương pháp luận)
3. Việc bạn cần làm (Danh sách bàn giao 4 mục)
4. DNA của nghệ sĩ đó (6 mục trường cốt lõi):
   - Bảng màu (Mã HEX cụ thể)
   - Font chữ (Tên cụ thể + Phương án thay thế)
   - Ngôn ngữ thị giác (Một vài đường nét cốt lõi)
   - Yếu tố thương hiệu (Chữ ký nhận diện)
   - Nhịp điệu (Khác biệt với góc nhìn khác)
   - Bản tăng cường chống AI slop (Vùng cấm trong ngữ cảnh phong cách đó)
5. Tham khảo cấu trúc 30 giây (Phác thảo 4-6 shot)
6. Yêu cầu thiết kế thẻ đích (Destination cards) (Giữ tính thực tế có thể đọc)
7. Ràng buộc quan trọng (30s / 1920×1080 / file:// / Google Fonts CDN)
8. Danh sách kiểm tra xuất ra + Định dạng báo cáo hoàn thành
```

**Quan trọng**: Mỗi brief bắt buộc phải nhấn mạnh "**Không được lặp lại thẩm mỹ của v5**" — Nếu không subagent sẽ bị v5 director-notes ảnh hưởng và có xu hướng giống nhau.

### Step 2 · Kích hoạt song song 6 subagent (6 tool calls Agent trong cùng 1 message)

```js
Agent({ subagent_type: "general-purpose", run_in_background: true, name: "v5a-anderson", ... })
Agent({ subagent_type: "general-purpose", run_in_background: true, name: "v5b-bass", ... })
// ... 6 subagents
```

Chạy trong background, dự kiến 30-60 phút.

### Step 3 · Công việc rảnh rỗi trong thời gian chờ (Idle work)

Đừng polling trạng thái agent. Subagent hoàn thành sẽ tự động gửi task-notification. Trong thời gian chờ làm:

- Sửa bug bản v5 cơ sở của luồng chính
- Viết review framework (Các chiều kích chấm điểm cho mỗi phiên bản / Q&A)
- Đúc kết phương pháp luận vào skill (Đây chính là nguồn gốc của case study này)
- Chuẩn bị khung sườn tài liệu final summary

### Step 4 · Xử lý thất bại (Tỷ lệ thất bại khoảng 16%, chấp nhận được)

Quan sát thực tế: 6 subagent sẽ có khoảng 1 agent bị thất bại do mạng hoặc vượt quá giới hạn token (Bass vòng đầu lỗi socket). Cách xử lý:

1. Khi nhận được thông báo hoàn thành **lập tức kiểm tra** thư mục output của agent đó
2. Thiếu sản phẩm bàn giao cốt lõi → Khởi động lại agent đó (Cùng brief, có thể đánh dấu "Lần trước thất bại, vui lòng thực thi lại")
3. Hoàn thành một phần (Như có html nhưng chưa chụp màn hình) → Luồng chính tự chụp bổ sung bằng Playwright, không khởi động lại agent

### Step 5 · Thẩm định hệ thống sau khi hoàn thành 6 phiên bản

Framework thẩm định (5 chiều kích + 3 câu hỏi cấp cao + Phân bổ use case):

```
Chấm điểm 5 chiều kích (Mỗi chiều 1-10):
- Distinctiveness: Sự biệt lập thị giác
- Coherence: Nhất quán thẩm mỹ
- Anti-slop: Thực thi chống AI slop
- Story arc: Nhịp điệu và cung câu chuyện
- Pause-and-look: Mật độ chi tiết khiến dừng lại xem

3 câu hỏi cấp cao:
- Q1 Chia sẻ ảnh chụp? (Kích hoạt hành động tạm dừng trên mạng xã hội)
- Q2 Ghi nhớ một câu? (Để lại ghi nhớ cấp mệnh đề)
- Q3 Vượt thời đại? (5 năm sau xem lại không thấy rẻ tiền)

Phân bổ use case (Theo nền tảng và độc giả):
- Bài viết truyền thông / X / YouTube / Zalo / Dribbble / Demo khách hàng / Nhóm riêng / ...
```

Chi tiết xem tại REVIEW.md cùng thư mục của `assets/director-notes-samples/launch-film-30s-sample.md`.

---

## Kết quả Thí nghiệm (Thực tế)

### Dung lượng tài liệu

- Director-notes bản v5 cơ sở: 11.500 từ
- Director-notes của 6 góc nhìn: Mỗi bản 4.000-12.000 từ
- Tổng dung lượng tài liệu: Khoảng 55.000-70.000 từ
- Đầy đủ cấu trúc 5 phần lớn: 6/6 phiên bản

### Triển khai HTML

- Mỗi bản một animation.html độc lập, 30 giây, 1920×1080
- Dung lượng file 28-74KB
- Tất cả đều mở được bằng file:// (Không phụ thuộc server)

### Khung hình chính (Keyframe)

- Mỗi bản 10-18 ảnh PNG, bao phủ toàn bộ cung câu chuyện 30 giây
- Tổng lượng ảnh chụp: 80+ ảnh
- Dung lượng trung bình mỗi ảnh PNG: 100-200KB

### Thời gian

- 6 subagent chạy song song: Khoảng 12-15 phút (Hiển thị duration_ms)
- Luồng chính làm việc rảnh rỗi song song (Sửa v5 + Viết phương pháp luận): Hoàn thành trong cùng thời gian
- Tổng thể "Từ khi kích hoạt 6 góc nhìn đến khi toàn bộ deliverable sẵn sàng": Khoảng 60 phút

---

## Chiêm nghiệm Cốt lõi (Dành cho người dùng tương lai của huashu-design)

### Chiêm nghiệm 1 · Phương pháp luận "Viết vạn từ director's notes trước" **hoàn toàn tái tạo được (reproducible)**

Cả 6 subagent đều sản xuất ra spec hoàn chỉnh 4.000-12.000 từ theo cấu trúc 5 phần lớn, và khi triển khai HTML đều đạt chất lượng sẵn sàng tiếp thị (marketing-ready). Điều này chứng minh bản thân phương pháp luận không phụ thuộc vào thiên tài của một người thực thi đơn lẻ — **Chỉ cần brief đưa ra rõ ràng, nhiều người thực thi độc lập có thể tạo ra kết quả chất lượng cao nhất quán**.

### Chiêm nghiệm 2 · "Góc nhìn" bắt buộc phải cụ thể đến "Tác phẩm + Năm"

Trong mỗi brief đều liệt kê đối thoại tác phẩm cụ thể:
- Anderson → *The French Dispatch* (2021) + *Moonrise Kingdom* (2012) + Bìa bọc Penguin Classics + Danh mục Olivetti 1960s
- Wong Kar-wai → *Tâm Trạng Khi Yêu* (2000) + *2046* (2004)
- Vignelli → Bản đồ tàu điện ngầm NYC 1972 + Cẩm nang thương hiệu Knoll + *The Vignelli Canon*
- Hara → Thương hiệu MUJI 1995-2023 + *White* + Tính trong suốt của Junya Ishigami
- Kusama → Infinity Mirrored Rooms (2013-2023) + Sắp đặt Polka Dot Obsession

**Kết quả thực tế**: Tất cả subagent đều bắt chính xác DNA thị giác cốt lõi của tác phẩm đó, chứ không phải "giá trị trung bình" của trường phái.

### Chiêm nghiệm 3 · "Phiên bản tăng cường phong cách" của chống AI slop là mấu chốt

Anti-slop chung (gradient tím / emoji / nhân vật SVG) áp dụng cho tất cả các phiên bản. Nhưng **mỗi phong cách còn phải viết "Anti-slop riêng"**:

- Bass: Không dùng Helvetica (Quá sạch sẻ, Bass là thô ráp)
- Vignelli: Không dùng bo góc (Tất cả corner 90°)
- Hara: Không dùng bất kỳ gradient nào + Không dùng sans display
- Kusama: Không dùng giao diện SaaS hiện đại
- Anderson: Không dùng phối màu cyber
- Wong Kar-wai: Không dùng Inter (Wong Kar-wai dùng Serif)

Thêm các yếu tố này vào, độ thuần khiết phong cách của 6 phiên bản cực kỳ cao, không bản nào bị trùng lặp.

### Chiêm nghiệm 4 · Giá trị thực sự của đa góc nhìn không phải là "Chọn ra bản chiến thắng"

Ban đầu dự định là A/B test chọn bản tốt nhất. Khi thẩm định thực tế phát hiện: **Cả 6 phiên bản đều có use case riêng rõ ràng**:
- v5 Cơ sở → Trang sản phẩm / Ứng dụng đọc sách (Mật độ thông tin cao)
- Anderson → Ảnh đầu bài viết truyền thông (Cảm giác lật tạp chí mạnh)
- Wong Kar-wai → Nền tảng video / Hướng văn hóa tiếng Trung (Ấm áp hoài niệm)
- Vignelli → Giới thiết kế / Dribbble (Mỗi frame đều là poster in ấn)
- Hara → Thuyết trình khách hàng / Ảnh chụp màn hình tĩnh (Triết lý tối giản)
- Kusama → Video ngắn X / Lan truyền (Tác động thị giác)

**Kết luận**: Tiếp thị (marketing) không phải là single-shot, mà là multiplex dành riêng cho từng nền tảng (platform-specific). Giá trị thực sự của 6 góc nhìn song song là **cho một dự án có 6 vũ khí biệt lập**, chứ không phải làm cho 5 phiên bản không thể lên sàn.

### Chiêm nghiệm 5 · Tỷ lệ thất bại của subagent ~16% là chấp nhận được

1 trong 6 bản bị thất bại (Bass vòng đầu lỗi socket). Chi phí xử lý: Khởi động lại + Brief đơn giản hóa 5 phút, đợi thêm 12-15 phút. **So sánh vs. Việc bắt 1 agent chạy tuần tự 6 phiên bản (90+ phút)** — Song song + Thử lại rõ ràng kinh tế hơn.

### Chiêm nghiệm 6 · Luồng chính bắt buộc phải làm việc thực chất trong thời gian chờ (Substantive idle work)

Subagent hoàn thành cần 12-15 phút. Khoảng thời gian này luồng chính tuyệt đối không được ngồi rảnh:

- **Sửa bug bản chính** (Người dùng đã phản hồi)
- **Viết review framework** (Điền vào khi đợi thẩm định)
- **Đúc kết phương pháp luận vào skill** (Như nghiên cứu tình huống này)
- **Chuẩn bị final summary** (Người dùng quay lại là thấy ngay)

Đây là "Trách nhiệm luồng chính" của quy trình làm việc multi-agent song song — Không phải PM ngồi đợi kết quả, mà là orchestrator đồng bộ đẩy tiến độ.

---

## Khi nào nên kích hoạt "Đa góc nhìn song song"

| Kịch bản | Có kích hoạt không | Lý do |
|----------|-------------------|-------|
| Người dùng nói rõ "Muốn xem các định hướng khác nhau", "Làm thêm vài phiên bản nữa" | ✅ Kích hoạt ngay | Nhu cầu trực tiếp |
| Phiên bản đầu tiên làm ra người dùng không hài lòng nhưng không nói rõ được muốn gì | ✅ Kích hoạt | Lựa chọn A/B tốt hơn việc "Tôi đoán bạn muốn gì" |
| Dự án chuẩn bị phân phối đa nền tảng (X / Bài viết truyền thông / YouTube / Zalo) | ✅ Kích hoạt | Mỗi nền tảng một phiên bản |
| Khách hàng chưa chốt phong cách nhưng có ngân sách (thời gian + token) | ✅ Kích hoạt | Sửa đi sửa lại = Chi phí gấp 5 lần |
| Người dùng đã đưa ra tham khảo phong cách rõ ràng và chỉ cần 1 phiên bản | ❌ Không kích hoạt | Lãng phí |
| Nhiệm vụ là motion graphic / icon animation đơn giản | ❌ Không kích hoạt | Kỹ thuật hóa quá mức |
| Thời gian gấp < 30 phút | ❌ Không kích hoạt | Subagent chạy không kịp |

---

## Sơ đồ Quy trình Phương pháp luận Hoàn chỉnh

```
Brief người dùng (Bao gồm kỳ vọng chất lượng)
       ↓
[Luồng chính] Viết director's notes v5 cơ sở (Cấp vạn từ 5 phần lớn)
       ↓
[Luồng chính] Triển khai v5 HTML + Chụp khung hình chính (marketing baseline)
       ↓
[Điểm quyết định] Có kích hoạt đa góc nhìn không?
       ↓ YES
[Luồng chính] Chọn 6 góc nhìn biệt lập + Viết 6 brief độc lập (Mỗi bản 8 trường)
       ↓
[6 subagents song song]
   ├── v5a brief → director-notes + html + keyframes + README
   ├── v5b brief → ...
   ├── v5c brief → ...
   ├── v5d brief → ...
   ├── v5e brief → ...
   └── v5f brief → ...
       ↓
[Luồng chính làm đồng thời] Sửa bug v5 · Viết review framework · Đúc kết phương pháp luận
       ↓
[Nhận đủ 6 thông báo]
       ↓
[Luồng chính] Phát hiện thất bại + Thử lại / Chụp ảnh bổ sung
       ↓
[Luồng chính] Chấm điểm 5 chiều kích + 3 câu hỏi cấp cao + Phân bổ use case
       ↓
[Luồng chính] Viết final REVIEW.md
       ↓
[Bàn giao] 6 phiên bản hoàn chỉnh + Review + Khuyến nghị phân phối nền tảng
```

---

## Tài liệu Liên quan

- Phương pháp luận hoàn chỉnh: `references/launch-film-director-notes.md`
- Mẫu góc nhìn đơn: `assets/director-notes-samples/launch-film-30s-sample.md` (v5 Cơ sở)
- Vị trí dự án thực tế: Thư mục demos tác giả cục bộ (Bao gồm trọn bộ file 6 + 1 góc nhìn)
- Thẩm định review: File REVIEW.md tác giả cục bộ

---

*Cập nhật lần cuối: 11-05-2026*
*Nghiên cứu tình huống thực tế: Thí nghiệm song song 6 góc nhìn huashu-md-html v2.0 launch film*
