# Spec: Pomodoro iOS Prototype

## Sản phẩm
Prototype iOS (không phải web) cho một app đếm giờ Pomodoro. Tên placeholder: **"TOMA"** (dễ đổi sau). Đây là app năng suất cá nhân giúp người dùng làm việc theo chu kỳ 25 phút tập trung / 5 phút nghỉ, gắn với danh sách công việc và thống kê tiến độ.

## Đối tượng & bối cảnh dùng
Người đi làm / sinh viên tự làm việc, mở app trên iPhone, thao tác nhanh bằng ngón tay cái (one-handed), thường bấm Start rồi để điện thoại xuống bàn — cần đọc số liệu từ xa ~50cm nên số đếm giờ phải rất lớn, rõ.

## 4 màn hình chính (bắt buộc, đều phải bấm/tương tác được, chuyển qua lại bằng tab bar dưới cùng)
1. **Timer (Đồng hồ đếm giờ)** — màn hình mặc định khi mở app.
   - Vòng tròn/khối hình học lớn hiển thị thời gian đếm ngược (mm:ss), thực sự chạy (setInterval), có thể Start / Pause / Reset.
   - Hiển thị trạng thái phiên: Tập trung (Focus) / Nghỉ ngắn (Short break) / Nghỉ dài (Long break), có thể bấm chuyển tay giữa các loại.
   - Hiển thị task đang làm (lấy từ Task List, nếu có task được chọn làm "current task").
   - Chấm tròn đếm số pomodoro đã hoàn thành trong phiên hôm nay (vd 3/4 chấm sáng).
2. **Task List (Việc cần làm)** — danh sách task gắn với số pomodoro ước tính mỗi task.
   - Thêm task mới (nút + mở input/modal, thêm thật vào list bằng JS state, không giả).
   - Bấm checkbox để đánh dấu hoàn thành (có hiệu ứng, task hoàn thành gạch ngang/mờ đi).
   - Bấm vào 1 task để chọn làm "current task" cho màn Timer (có chỉ báo task nào đang được chọn).
3. **Thống kê (Stats)** — vì đây là app dạng tracking/năng suất, áp dụng "high density type": mỗi màn cần ≥3 thông tin khác biệt có thực chất (không phải icon trang trí).
   - Tổng thời gian tập trung hôm nay/tuần này (số lớn).
   - Biểu đồ cột 7 ngày gần nhất (số phút mỗi ngày) — dựng bằng CSS/SVG, có thể bấm đổi giữa "Tuần" / "Tháng".
   - Streak (số ngày liên tục có ít nhất 1 pomodoro) + tổng số pomodoro đã hoàn thành.
4. **Cài đặt (Settings)** — các thiết lập thực sự đổi được state (toggle/slider hoạt động, có phản hồi thị giác ngay khi bấm):
   - Thời lượng Focus / Short break / Long break (dùng stepper hoặc slider).
   - Số phiên Focus trước khi vào Long break.
   - Toggle: âm thanh khi hết giờ, tự động bắt đầu phiên tiếp theo, chế độ tối (dark mode — nếu bấm phải đổi theme thật).

## Tông & cảm xúc
Tập trung, yên tĩnh, đáng tin cậy nhưng không lạnh lẽo — đây là công cụ dùng hàng ngày, không phải sản phẩm trình diễn. Tránh trẻ con quá mức (không phải app cho trẻ em) nhưng có thể có 1 điểm nhấn ấm áp/vui vẻ nhẹ.

## Định dạng & kích thước output
- Mỗi hướng thiết kế = **1 file HTML độc lập, tự chứa** (`file://` mở trực tiếp bằng double-click, không cần server).
- Kiến trúc: React 18 UMD + Babel standalone qua CDN (unpkg hoặc cdnjs), toàn bộ JSX viết inline trong `<script type="text/babel">`.
- Dùng khung `IosFrame` chuẩn (bezel + Dynamic Island + status bar + home indicator, iPhone 15 Pro 393×852) — **không tự vẽ tay** dynamic island/status bar.
- Bố cục giao hàng: **4 chiếc iPhone xếp ngang cạnh nhau** trên 1 trang, mỗi chiếc là 1 state machine độc lập, mỗi chiếc mở mặc định ở 1 trong 4 màn hình chính (để nhìn thấy toàn cảnh), nhưng **mỗi chiếc đều bấm tab bar để đi hết cả 4 màn** (không phải ảnh tĩnh). Có label italic nhỏ phía trên mỗi máy ghi tên màn hình.

## Ràng buộc đã biết
- Không có brand/logo thật cần dùng — đây là sản phẩm ý tưởng gốc, không mô phỏng app có sẵn nào.
- Không cần ảnh thật (không có nội dung nào bắt buộc phải có ảnh — task/timer/stats đều là dữ liệu/hình học, ảnh trang trí sẽ là slop, KHÔNG thêm ảnh).
- Icon dùng SVG geometric tối giản tự vẽ (không dùng emoji làm icon chính, không dùng icon font ảnh ngoài).
- Dữ liệu mẫu (task, thống kê 7 ngày...) là dữ liệu giả định hợp lý, không bịa số liệu trông như thật/không đánh lừa.

## Giả thuyết mô-típ hình ảnh (visual motif) — mỗi hướng tự diễn giải theo phong cách được giao, nhưng gốc chung là:
Vòng tròn/chu kỳ (cycle) — pomodoro về bản chất là các chu kỳ lặp lại (làm việc → nghỉ → làm việc). Có thể thể hiện qua: vòng tròn tiến trình, chấm tròn đếm phiên, đường cong lặp, hình quả cà chua cách điệu hình học (không vẽ cà chua tả thực).
