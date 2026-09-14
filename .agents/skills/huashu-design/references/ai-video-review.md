# Vòng khép kín Đánh giá Xem video bằng AI (scripts/cloud/ai-review-video.py)

> Đưa MP4 render cuối cùng cho model hiểu video (seed-2.0-lite), xuất báo cáo đánh giá cấu trúc hóa theo checklist cố định.
> Định vị: **Sau khi render cuối, trước khi giao hàng** - lớp kiểm tra chất lượng cuối cùng, thay thế việc người xem lại toàn bộ phim bằng mắt. Không thay thế verify-video.sh từng khung hình.
> ⚠️ Năng lực đám mây tùy chọn: Các phân đoạn video sau khi nén sẽ được gửi tới interface chính thức của Volcengine Ark (ark.cn-beijing.volces.com),
> sử dụng ARK_API_KEY của chính bạn, cần `--yes` hoặc `HUASHU_CLOUD_OK=1` để xác nhận rõ ràng. Xem `SECURITY.md` tại gốc kho lưu trữ.
> Không muốn dùng đám mây: `scripts/verify-video.sh` chụp khung hình xem bằng mắt thủ công, toàn bộ cục bộ.

## Khi nào dùng

- Sau khi xuất thành phẩm 60fps render cuối, trước khi giao hàng/trộn âm thanh, hãy chạy một lượt
- Sau khi có bản trộn âm thanh SFX chạy lại một lượt (Kiểm tra đối chiếu onset chỉ có hiệu lực khi có track âm thanh)
- Sau khi sửa xong các vấn đề lớn render lại thì kiểm tra lại
- Đừng chạy ở giai đoạn render thử 30fps (Độ phân giải/Nhịp điệu chưa cố định, lãng phí lượt gọi)

## Cách dùng

```bash
cd ThưMụcDựÁn && unset ALL_PROXY   # Trong script đã miễn dịch proxy, unset là bảo hiểm đôi
uv run ~/.claude/skills/huashu-design/scripts/cloud/ai-review-video.py \
  --video ThànhPhẩm.mp4 \
  --context KịchBảnĐạoDiễn.md \   # Khuyến nghị mạnh mẽ mang theo: Model dựa vào đó để phân biệt "ý đồ thiết kế" và "bug"
  --yes                          # Xác nhận phân đoạn video gửi tới Volcengine Ark (Hoặc HUASHU_CLOUD_OK=1)
```

- ARK_API_KEY cấu hình tại `.env` (Đã gitignore) dưới thư mục gốc của skill hoặc biến môi trường, script chỉ trích xuất một biến này
- Báo cáo hạ đĩa: Cùng thư mục với video `<TênVideo>-AI评审.md` (`--output` có thể sửa)
- `--segment-len` mặc định 60 giây một đoạn; `--model` mặc định doubao-seed-2-0-lite-260215
- Thử nghiệm thực tế video 210 giây: 6 lần gọi API, 6-10 phút, tokens khoảng 18 vạn in/2 vạn out (Nấc lite, chi phí cấp vài xu)

## Chuỗi gọi (Hỗn hợp 3 lớp, không phải model thuần túy)

1. **Kiểm tra khách quan bằng ffmpeg** (Tính xác định, không bỏ sót):
   - `silencedetect` → Bảng thời gian onset của hiệu ứng âm thanh (Model **không nghe thấy** track âm thanh video, thử nghiệm thực tế 17-07-2026)
   - `freezedetect` → Danh mục các phân đoạn hoàn toàn đứng yên ≥3 giây
2. **Model xem phim theo phân đoạn**: Nén 60s/đoạn rồi gửi thẩm định (Chiều rộng 1280/15fps/crf28, animation phẳng khoảng 0.5MB/phút),
   prompt mỗi đoạn bao gồm checklist+kịch bản đạo diễn+dữ liệu onset/đứng yên của đoạn đó, mốc thời gian quy đổi thành thời gian phim gốc
3. **Pass độ phân giải thấp toàn bộ phim của model**: Phim toàn bộ chiều rộng 960/10fps gửi thẩm định riêng, chuyên kiểm tra tính mạch lạc tự sự qua các đoạn/hero xuyên suốt/nhịp điệu tổng thể
4. Gọi tổng hợp văn bản gộp theo ①-⑧; Nhật ký gốc phân đoạn + Dữ liệu kiểm tra khách quan tất cả được giữ lại trong phụ lục báo cáo

## Checklist và Mức độ nghiêm trọng

①Khung hình đen/Render tàn khuyết ②Cắt chữ/Sai chữ ③Phần tử chồng lấp che khuất ④Mạch lạc tự sự (Chuyển cảnh nhận biết theo 3 lớp từ vựng §7 camera-language.md: Bát thức[xóa trắng/xuyên trường tối/chuyển tiếp nhòe nét/chữ card trường đen/whip-pan/mask-wipe], hidden-cut, travel[phần tử chia sẻ về vị trí/xuyên qua khoảng trống chữ]; Cắt trần = Cắt cứng chưa đóng gói, ghi ⚡)
⑤Tính xuyên suốt của hero ⑥Đoạn chết nhịp điệu (Danh mục khách quan + Model đánh giá là cố tình hold hay đoạn chết thật) ⑦Đánh điểm hiệu ứng âm thanh (Kiểm tra đối chiếu onset + Sự kiện màn hình)
⑧Mất cân bằng bố cục/Khoảng trống

⚠️Chết người = Bắt buộc phải sửa trước khi giao hàng | ⚡Quan trọng = Cảm nhận bị tổn hại rõ ràng | 💡Gợi ý = Thêm hoa trên gấm

## Hạn chế (Bắt buộc đọc trước khi dùng báo cáo)

- **Model không nghe thấy âm thanh**: ⑦ là kiểm tra đối chiếu một chiều "Tại thời điểm onset track âm thanh màn hình có sự kiện hay không",
  không đánh giá được hiệu ứng âm thanh chọn đúng hay không, âm lượng đúng hay không, cảm xúc BGM đúng hay không
- **Không nhìn thấy chi tiết cấp khung hình**: Rung lắc 1-2 khung hình, rung nhẹ chi tiết, sai lệch giá trị màu chính xác, căn chỉnh subpixel không bắt được,
  những cái này vẫn dựa vào verify-video.sh chụp khung hình xem bằng mắt thủ công
- **Đánh giá loại chuyển cảnh thiên về nghiêm ngặt**: Sau khi nén xuống 15fps, mờ dần ra nhanh có thể bị báo thành "cắt cứng",
  khi phân đoạn mâu thuẫn với pass toàn bộ phim bản tổng hợp sẽ đánh dấu "Nghi vấn"——mục nghi vấn tự mình rút khung hình xác nhận rồi mới sửa
- **"Cố tình hold vs Đoạn chết" là ý kiến của model**: Định hình dài lót lời nói b-roll thường được cho qua, thành phẩm khi xem độc lập phải tự mình đánh giá lại
- Gọi thất bại (Mạng/key/hạn mức) sẽ viết trung thực vào đầu báo cáo, tuyệt đối không bịa đặt kết quả đánh giá; Phạm vi thời gian của đoạn thất bại sẽ được đánh dấu

## Tiêu chuẩn thử nghiệm thực tế

Đối tượng chạy đầu tiên: B00-前三分钟主线-SFX.mp4 (210s). Model tự chủ phát hiện vấn đề chuyển cảnh giữa các hồi và hướng đứt đoạn hero đúng đắn
Nhưng báo nhầm fade thành cắt cứng; Model thuần túy bắt đoạn chết chỉ trúng 3/14, sau khi nối thêm freezedetect đã che phủ hoàn toàn. Kết luận: Lớp kiểm tra khách quan là
đảm bảo giới hạn dưới của vòng khép kín này, model chịu trách nhiệm đánh giá ngữ nghĩa.
