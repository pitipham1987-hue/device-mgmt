# Quy tắc Thiết kế Âm thanh · huashu-design

> Công thức ứng dụng âm thanh cho tất cả các animation demo. Sử dụng phối hợp với `sfx-library.md` (Danh mục tài sản).
> Tích lũy thấu đáo từ thực chiến: Lặp lại hero animation v1-v9 ra mắt của huashu-design · Phân tích mổ xẻ sâu bằng Gemini đối với 3 bộ phim chính thức của Anthropic · 8000+ lần đối chiếu A/B.

---

## Nguyên tắc Cốt lõi · Chế độ Âm thanh Hai Track (Quy tắc sắt)

Âm thanh animation **bắt buộc phải thiết kế hai lớp độc lập**, không thể chỉ làm một lớp:

| Lớp | Tác dụng | Thang thời gian | Mối quan hệ với thị giác | Tần số chiếm giữ |
|---|---|---|---|---|
| **SFX (Lớp nhịp điệu)** | Đánh dấu từng visual beat | 0.2-2 giây ngắn dồn | **Đồng bộ mạnh** (Căn chỉnh cấp khung hình) | **Tần số cao 800Hz+** |
| **BGM (Nền không khí)** | Lót nền cảm xúc, trường âm | Liên tục 20-60 giây | Đồng bộ yếu (Cấp phân đoạn) | **Tần số trung thấp <4kHz** |

**Animation chỉ làm BGM là bị tàn tật**——Tiềm thức của khán giả cảm nhận được "hình ảnh chuyển động nhưng không có phản hồi âm thanh", nguồn gốc của cảm giác rẻ tiền chính là ở đây.

---

## Tiêu chuẩn Vàng · Tỷ lệ Vàng

Các nhóm giá trị này là **tham số cứng kỹ thuật** thu được từ việc đo đạc thực tế 3 bộ phim chính thức của Anthropic + đối chiếu bản định hình v9 của chính chúng ta, áp dụng trực tiếp là được:

### Âm lượng
- **Âm lượng BGM**: `0.40-0.50` (Tương đối so với thang đo đầy 1.0)
- **Âm lượng SFX**: `1.00`
- **Chênh lệch độ vang**: BGM so với SFX peak **thấp hơn -6 đến -8 dB** (Không dựa vào âm lượng tuyệt đối của SFX để nổi bật, dựa vào chênh lệch độ vang)
- **Tham số amix**: `normalize=0` (Tuyệt đối không dùng normalize=1, sẽ nén phẳng dải động)

### Cách ly Dải tần (Tối ưu hóa cứng P1)
Bí quyết của Anthropic không phải là "âm lượng SFX lớn", mà là **phân lớp dải tần**:

```bash
[bgm_raw]lowpass=f=4000[bgm]      # BGM giới hạn ở tần số trung thấp <4kHz
[sfx_raw]highpass=f=800[sfx]      # SFX đẩy lên tần số trung cao 800Hz+
[bgm][sfx]amix=inputs=2:duration=first:normalize=0[a]
```

Tại sao: Tai người nhạy cảm nhất với khoảng 2-5kHz (Tức "dải tần presence"), nếu SFX đều ở khoảng này, BGM lại che phủ toàn dải tần, **SFX sẽ bị phần tần số cao của BGM che mất**. Dùng highpass đẩy SFX lên cao + lowpass nén BGM xuống, cả hai mỗi bên chiếm một phương trên phổ tần, độ rõ nét của SFX lập tức nâng lên một nấc.

### Fade
- BGM vào: `afade=in:st=0:d=0.3` (0.3s, tránh cắt cứng)
- BGM ra: `afade=out:st=N-1.5:d=1.5` (1.5s đuôi dài, cảm giác khép lại)
- SFX có sẵn envelope, không cần fade bổ sung

---

## Quy tắc thiết kế SFX cue

### Mật độ (Mỗi 10 giây bao nhiêu SFX)
Đo đạc thực tế mật độ SFX trong 3 bộ phim của Anthropic có ba nấc:

| Phim | Số SFX mỗi 10s | Tính cách sản phẩm | Kịch bản |
|---|---|---|---|
| Artifacts (ref-1) | **~9 cái/10s** | Tính năng dày đặc, nhiều thông tin | Demo công cụ phức tạp |
| Code Desktop (ref-2) | **0 cái** | Nền thuần túy, cảm giác thiền định | Trạng thái tập trung của công cụ phát triển |
| Word (ref-3) | **~4 cái/10s** | Cân bằng, nhịp điệu văn phòng | Công cụ năng suất |

**Gợi ý heuristic**:
- Tính năng sản phẩm trầm tĩnh/tập trung → Mật độ SFX thấp (0-3 cái/10s), BGM làm chủ đạo
- Tính năng sản phẩm sôi nổi/nhiều thông tin → Mật độ SFX cao (6-9 cái/10s), SFX thúc đẩy nhịp điệu
- **Đừng lấp đầy từng visual beat**——Khoảng trống cao cấp hơn dày đặc. **Xóa đi 30-50% cue sẽ làm những cái còn lại có tính kịch hơn**.

### Độ ưu tiên lựa chọn Cue
Không phải visual beat nào cũng phải phối SFX. Chọn theo độ ưu tiên này:

**P0 Bắt buộc phối** (Bỏ qua sẽ có cảm giác vi hòa):
- Gõ chữ (Terminal/Input)
- Click/Chọn (Thời điểm quyết định của người dùng)
- Chuyển tiêu điểm (Nhân vật chính thị giác dịch chuyển)
- Logo reveal (Thu khép thương hiệu)

**P1 Khuyến nghị phối**:
- Phần tử vào cảnh/thoát cảnh (modal / card)
- Phản hồi hoàn thành/thành công
- AI bắt đầu/kết thúc tạo dữ liệu
- Chuyển tiếp quan trọng (chuyển đổi scene)

**P2 Tùy chọn phối** (Nhiều quá sẽ loạn):
- hover / focus-in
- Tick tiến độ
- Ambient trang trí

### Độ chính xác căn chỉnh Mã thời gian
- **Căn chỉnh cùng khung hình** (Sai số 0ms): Click/Chuyển tiêu điểm/Logo định vị
- **Đặt trước 1-2 khung hình** (-33ms): Whoosh nhanh (Tạo kỳ vọng tâm lý cho khán giả)
- **Đặt sau 1-2 khung hình** (+33ms): Vật thể hạ cánh/impact (Phù hợp vật lý thực tế)

---

## Cây quyết định Lựa chọn BGM

huashu-design skill có sẵn 6 bài BGM (`assets/bgm-*.mp3`):

```
Tính cách animation là gì?
├─ Ra mắt sản phẩm / Demo kỹ thuật → bgm-tech.mp3 (minimal synth + piano)
├─ Hướng dẫn giải thích / Sử dụng công cụ → bgm-tutorial.mp3 (warm, instructional)
├─ Giáo dục học tập / Giải thích nguyên lý → bgm-educational.mp3 (curious, thoughtful)
├─ Tiếp thị quảng cáo / Tuyên truyền thương hiệu → bgm-ad.mp3 (upbeat, promotional)
└─ Cùng loại phong cách cần biến thể → bgm-*-alt.mp3 (Phiên bản thay thế tương ứng)
```

### Kịch bản không BGM (Đáng để cân nhắc)
Tham khảo Anthropic Code Desktop (ref-2): **0 SFX + BGM Lo-fi thuần túy** cũng có thể rất cao cấp.

**Khi nào chọn Không BGM**:
- Thời lượng animation <10s (BGM không thiết lập kịp)
- Tính cách sản phẩm là "Tập trung/Thiền định"
- Bản thân kịch bản có âm thanh môi trường/tiếng thuyết minh
- Khi mật độ SFX rất cao (Tránh quá tải thính giác)

---

## Công thức Kịch bản (Mở hộp dùng ngay)

### Công thức A · Hero ra mắt sản phẩm (Cùng loại huashu-design v9)
```
Thời lượng: 25 giây
BGM: bgm-tech.mp3 · 45% · Dải tần <4kHz
Mật độ SFX: ~6 cái/10s

cue:
  Terminal gõ chữ → type × 4 (Khoảng cách 0.6s)
  Enter           → enter
  Card hội tụ     → card × 4 (Lệch pha 0.2s)
  Chọn            → click
  Ripple          → whoosh
  4 lần tiêu điểm → focus × 4
  Logo            → thud (1.5s)

Âm lượng: BGM 0.45 / SFX 1.0 · amix normalize=0
```

### Công thức B · Demo tính năng công cụ (Tham khảo Anthropic Code Desktop)
```
Thời lượng: 30-45 giây
BGM: bgm-tutorial.mp3 · 50%
Mật độ SFX: 0-2 cái/10s (Cực ít)

Chiến lược: Để BGM + Thuyết minh voiceover thúc đẩy, SFX chỉ xuất hiện ở thời điểm quyết định (Lưu file/Hoàn thành thực thi lệnh)
```

### Công thức C · Demo tạo dữ liệu AI
```
Thời lượng: 15-20 giây
BGM: bgm-tech.mp3 hoặc Không BGM
Mật độ SFX: ~8 cái/10s (Mật độ cao)

cue:
  Người dùng nhập liệu → type + enter
  AI bắt đầu xử lý    → magic/ai-process (Vòng lặp 1.2s)
  Tạo hoàn thành       → feedback/complete-done
  Kết quả hiển thị    → magic/sparkle
  
Điểm sáng: ai-process có thể lặp 2-3 lần chạy xuyên suốt toàn bộ quá trình tạo dữ liệu
```

### Công thức D · Cảnh quay dài không khí thuần túy (Tham khảo Artifacts)
```
Thời lượng: 10-15 giây
BGM: Không
SFX: Sử dụng riêng biệt 3-5 cue được thiết kế tỉ mỉ

Chiến lược: Mỗi SFX đều là nhân vật chính, không có vấn đề BGM "dính dập vào nhau".
Phù hợp: Cảnh quay chậm đơn sản phẩm, hiển thị đặc tả
```

---

## Template ffmpeg Tổng hợp

### Template 1 · Đè đơn SFX lên Video
```bash
ffmpeg -y -i video.mp4 -itsoffset 2.5 -i sfx.mp3 \
  -filter_complex "[0:a][1:a]amix=inputs=2:normalize=0[a]" \
  -map 0:v -map "[a]" output.mp4
```

### Template 2 · Tổng hợp Timeline Nhiều SFX (Căn chỉnh theo thời gian cue)
```bash
ffmpeg -y \
  -i sfx-type.mp3 -i sfx-enter.mp3 -i sfx-click.mp3 -i sfx-thud.mp3 \
  -filter_complex "\
[0:a]adelay=1100|1100[a0];\
[1:a]adelay=3200|3200[a1];\
[2:a]adelay=7000|7000[a2];\
[3:a]adelay=21800|21800[a3];\
[a0][a1][a2][a3]amix=inputs=4:duration=longest:normalize=0[mixed]" \
  -map "[mixed]" -t 25 sfx-track.mp3
```
**Tham số mấu chốt**:
- `adelay=N|N`: Phía trước là độ trễ kênh trái(ms), phía sau là kênh phải, viết hai lần đảm bảo căn chỉnh stereo
- `normalize=0`: Giữ lại dải động, mấu chốt!
- `-t 25`: Cắt đoạn đến thời lượng chỉ định

### Template 3 · Video + Track SFX + BGM (Có cách ly dải tần)
```bash
ffmpeg -y -i video.mp4 -i sfx-track.mp3 -i bgm.mp3 \
  -filter_complex "\
[2:a]atrim=0:25,afade=in:st=0:d=0.3,afade=out:st=23.5:d=1.5,\
     lowpass=f=4000,volume=0.45[bgm];\
[1:a]highpass=f=800,volume=1.0[sfx];\
[bgm][sfx]amix=inputs=2:duration=first:normalize=0[a]" \
  -map 0:v -map "[a]" -c:v copy -c:a aac -b:a 192k final.mp4
```

---

## Tra nhanh Các dạng Thất bại

| Triệu chứng | Nguyên nhân gốc rễ | Sửa chữa |
|---|---|---|
| SFX không nghe thấy | Phần tần số cao của BGM che mất | Thêm `lowpass=f=4000` cho BGM + `highpass=f=800` cho SFX |
| Hiệu ứng âm thanh quá to chói tai | Âm lượng tuyệt đối của SFX quá lớn | Giảm âm lượng SFX xuống 0.7, đồng thời giảm BGM xuống 0.3, giữ nguyên khoảng chênh lệch |
| Xung đột nhịp điệu BGM và SFX | BGM chọn sai (Dùng nhạc có beat mạnh) | Đổi sang BGM dạng ambient / minimal synth |
| Animation kết thúc BGM bị ngắt đột ngột | Không làm fade out | `afade=out:st=N-1.5:d=1.5` |
| SFX chồng lấp thành dính dập | cue quá dày + Thời lượng mỗi SFX quá dài | Khống chế thời lượng SFX trong vòng 0.5s, khoảng cách cue ≥ 0.2s |
| Video mp4 trên WeChat Official Account không có tiếng | WeChat đôi khi tự động mute auto-play | Không cần lo lắng, người dùng bấm vào sẽ có tiếng; GIF vốn dĩ đã không có tiếng |

---

## Liên động với Thị giác (Nâng cao)

### Âm sắc SFX phải khớp với phong cách thị giác
- Thị giác cảm giác kem ấm/giấy → SFX dùng âm sắc **chất gỗ/mềm mại** (Morse, paper snap, soft click)
- Thị giác công nghệ đen lạnh → SFX dùng âm sắc **kim loại/kỹ thuật số** (beep, pulse, glitch)
- Thị giác vẽ tay/vui nhộn → SFX dùng âm sắc **hoạt hình/phóng đại** (boing, pop, zap)

Nền kem ấm của `apple-gallery-showcase.md` hiện tại của chúng ta → Phối hợp với `keyboard/type.mp3` (mechanical) + `container/card-snap.mp3` (soft) + `impact/logo-reveal-v2.mp3` (cinematic bass)

### SFX có thể dẫn dắt nhịp điệu thị giác
Kỹ thuật cao cấp: **Thiết kế timeline SFX trước, sau đó điều chỉnh animation thị giác để căn chỉnh theo SFX** (Không phải ngược lại).
Bởi vì mỗi cue của SFX đều là một "nhịp tick đồng hồ", animation thị giác thích ứng với nhịp điệu SFX sẽ rất ổn định——ngược lại SFX đuổi theo thị giác, thường ±1 khung hình không khớp là có cảm giác vi hòa.

---

## Checklist Chất lượng (Tự kiểm tra trước khi phát hành)

- [ ] Chênh lệch độ vang: SFX peak - BGM peak = -6 đến -8 dB?
- [ ] Dải tần: BGM lowpass 4kHz + SFX highpass 800Hz?
- [ ] amix normalize=0 (Giữ lại dải động)?
- [ ] BGM fade-in 0.3s + fade-out 1.5s?
- [ ] Số lượng SFX có phù hợp (Chọn mật độ theo tính cách kịch bản)?
- [ ] Mỗi SFX và visual beat căn chỉnh cùng khung hình (Trong vòng ±1 khung hình)?
- [ ] Hiệu ứng âm thanh Logo reveal đủ thời lượng (Khuyến nghị 1.5s)?
- [ ] Tắt BGM nghe một lần: SFX riêng lẻ có đủ cảm giác nhịp điệu không?
- [ ] Tắt SFX nghe một lần: BGM riêng lẻ có cảm xúc thăng trầm không?

Hai lớp bất kỳ lớp nào nghe riêng lẻ đều nên tự phát huy hiệu quả. Nếu chỉ có hai lớp đè lên nhau mới hay, chứng tỏ chưa làm tốt.

---

## Tham khảo

- Danh mục tài sản SFX: `sfx-library.md`
- Tham khảo phong cách thị giác: `apple-gallery-showcase.md`
- Phân tích âm thanh sâu 3 bộ phim của Anthropic: AUDIO-BEST-PRACTICES.md (Tài liệu cục bộ tác giả, không phân phối theo kho lưu trữ)
- Trường hợp thực chiến huashu-design v9: hero-animation-v9-final.mp4 (Mẫu cục bộ tác giả, không phân phối theo kho lưu trữ)
