# VNPT AI · Brand Spec
> Ngày thu thập: 14/09/2026
> Nguồn tài sản: vnptai.io (trang chủ + trang giới thiệu `/vi/about`), file CSS build chính thức `app-4aca83ce.css`, smartvision.vnpt.vn (ảnh minh hoạ tính năng)
> Độ hoàn chỉnh tài sản: **Một phần** — logo và màu chính đã xác minh từ 3 nguồn độc lập; ảnh UI chỉ có tài liệu marketing, không có ảnh chụp màn quản lý thiết bị thật (sản phẩm B2B/B2G không công khai giao diện quản trị)

## 🎯 Tài sản cốt lõi (Công dân hạng nhất)

### Logo
- Phiên bản chính: `assets/vnptai-brand/logo.svg` (208×54, full-color, đã xác minh mở được, tải trực tiếp từ `vnptai.io/img/update/logo.svg`)
- Phiên bản ngược màu nền tối: **không tìm thấy bản mono/trắng chính thức công khai**. Không tự chế biến thể màu từ logo gốc (vi phạm cấm biến dạng). → Vì hướng thiết kế đã chốt nền sáng (spec Phần 4.4), dùng logo gốc trên mọi nền sáng/trung tính; nếu một phương án cần mảng nền đậm (vd sidebar tối), đặt logo trong khối nền trắng/thẻ trắng riêng thay vì đổi màu logo.
- Kịch bản sử dụng: header/topbar M1–M5, màn hình đăng nhập (nếu có), watermark góc tài liệu xuất
- Cấm biến dạng: không kéo giãn sai tỷ lệ, không đổi màu, không thêm viền/đổ bóng

### Hình sản phẩm
- Không áp dụng — AICAM là nền tảng phần mềm (sản phẩm phi thực thể), không có hình render phần cứng.

### Ảnh chụp UI (tham khảo có giới hạn)
- `assets/vnptai-brand/ui-tinhnang-01.png` (966×740) — minh hoạ tính năng nhận diện biển số/vi phạm giao thông, dạng mockup trong khung laptop
- `assets/vnptai-brand/ui-tinhnang-04.png` (1072×730) — minh hoạ tính năng bóc tách văn bản (OCR)
- ⚠️ **Giới hạn quan trọng**: đây là ảnh minh hoạ marketing (feature illustration), KHÔNG phải ảnh chụp màn hình giao diện quản lý thiết bị/camera thật. Không dùng làm nguồn bố cục. Chỉ dùng để đối chiếu khí chất màu (nền trung tính sáng, thẻ bo góc trắng nổi trên nền xám nhạt, nút xanh dương đậm bo góc vừa phải, icon đường nét mảnh đơn sắc) — nhất quán với hướng nền sáng đã chọn ở Phần 4.4 của spec.
- Kịch bản sử dụng: tham khảo khí chất màu/hình khối khi cân nhắc chi tiết trang trí, không trích dẫn làm bố cục

## 🎨 Tài sản hỗ trợ

### Bảng màu
- Primary: `#0047BB` — xanh dương đậm. Trích trực tiếp, trùng khớp ở 3 nguồn độc lập: `logo.svg` (23 lần, màu path đậm nhất), `about.html` (4 lần), `app-4aca83ce.css` (10 lần, cả dạng hoa/thường). Đây là màu chính thức, không suy đoán.
- Accent/gradient: `#02AAFA` / `#00B3FF` — xanh dương sáng, dùng làm điểm nhấn/gradient phụ trợ trong hệ thống hiện có của VNPT AI (8 lần trong CSS)
- Navy đậm (tham khảo, không bắt buộc dùng): `#00173D` / `#001250` — xuất hiện lặp lại trong CSS, có thể dùng cho text heading rất đậm nếu cần tương phản cao hơn `rgba(0,0,0,0.85)`
- Background: `#FFFFFF` (nền chính), `#F7F8F9` / `#F0F2F5`-tương đương (nền phụ, xám rất nhạt — khớp token Ant Design `--bg-body` ở reference-design.md)
- Màu cấm: không dùng `#1890ff` (màu mặc định Ant Design — spec Phần 5.1 cấm giữ nguyên); không dùng màu đỏ/cam nóng làm màu chính (đây là màu chức năng cảnh báo/lỗi, giữ nguyên theo token Ant Design ở reference-design.md, không lấn sang vai trò màu thương hiệu)

### Font chữ
- Trang marketing chính thức của VNPT AI dùng **Inter** — nhưng luật chống slop của huashu-design cấm Inter làm font tiêu đề.
- Áp dụng cho AICAM theo gợi ý ở spec Phần 4.5: **Be Vietnam Pro** (Display + Body ưu tiên, hỗ trợ dấu tiếng Việt đầy đủ, có biến thể tabular cho số).
- Mono/tabular cho dữ liệu (IP, serial, cổng, số kênh): biến thể số tabular của Be Vietnam Pro, hoặc `Source Sans 3` / hệ mono chuẩn nếu cần phân biệt rõ với chữ thân.

### Chi tiết chữ ký
- Gradient xanh dương (`#0047BB` → `#02AAFA`) là mô-típ lặp lại nhất quán trong toàn bộ tài sản VNPT AI (logo, nút CTA, khối nhấn mạnh) — có thể dùng làm chi tiết "làm 120%" ở một điểm nhấn duy nhất mỗi màn (vd thanh chỉ số tài nguyên, biểu đồ trạng thái tổng quan), không lạm dụng tràn lan.

### Vùng cấm
- Không dùng logo, tên, hoặc màu nhận diện của Dahua/Hikvision/Milestone (spec Phần 7)
- Không giữ `#1890ff` làm màu chính
- Không đoán thêm màu thương hiệu ngoài các mã đã trích xuất và xác minh ở trên
- Không dùng ảnh `ui-tinhnang-*.png` làm nguồn bố cục màn quản lý thiết bị — chúng thuộc phạm vi tính năng khác (giao thông, OCR), không phải camera management

### Từ khóa khí chất
- Chủ quyền công nghệ · Đáng tin · Có thẩm quyền dựa trên thành tích · Lấy con người làm trung tâm · Hạ tầng quốc gia (không phải giọng startup)
- Nguồn: tagline chính thức "Đồng hành cùng tổ chức, doanh nghiệp xây dựng chiến lược ứng dụng AI" + 6 giá trị cốt lõi công bố (Technology, Quality, Commitment, Experience, Security, Cost)
