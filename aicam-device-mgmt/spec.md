# AICAM — Quản lý thiết bị · SPEC

> Vòng làm việc mới (15/09/2026), viết lại từ đầu theo yêu cầu người dùng — không phục hồi bản spec cũ đã xoá.

## Sản phẩm là gì

AICAM là nền tảng quản lý thiết bị camera AI của VNPT AI, dùng cho các đơn vị vận hành hạ tầng giám sát cấp tỉnh/thành, khu công nghiệp, giao thông. Đây là phần mềm quản trị B2G/B2B — người dùng là kỹ thuật viên vận hành trung tâm điều hành (NOC), không phải người tiêu dùng cuối. Sản phẩm không có giao diện quản trị công khai tham khảo được (không giống Dahua IVSS đã có manual PDF phát hành công khai), nên bố cục phải tự thiết kế, chỉ tham chiếu **cấu trúc chức năng** của IVSS (loại màn hình, mô hình tương tác) chứ không sao chép thị giác.

## Người dùng và bối cảnh

- Kỹ thuật viên NOC theo dõi hàng trăm–hàng nghìn camera cùng lúc, ca làm việc dài, cần quét trạng thái nhanh.
- Quản trị viên hệ thống cấu hình mạng, thêm/xoá thiết bị hàng loạt.
- Người phụ trách AI cấu hình thuật toán, ngưỡng cảnh báo cho từng camera hoặc nhóm camera.
- Bối cảnh dùng: màn hình lớn tại phòng điều hành sáng đèn ban ngày, đôi khi laptop tại hiện trường lắp đặt.

## Quy mô

Phải chịu được **vài nghìn thiết bị** trên một tổ chức (khác IVSS chỉ thiết kế cho ~128 kênh/đầu ghi). Điều này quyết định cơ chế duyệt dữ liệu: cuộn ảo, lọc phía máy chủ, cây địa bàn nhiều cấp có số liệu tổng hợp theo nhánh, tổng hợp trước–chi tiết sau.

## Năm màn hình cốt lõi (vòng 1 tập trung dựng M2 trước, làm chuẩn hệ thống thiết kế)

- **M1 · Trang chủ** — điều hướng dạng hub, không sidebar phức tạp.
- **M2 · Danh sách thiết bị** — màn quan trọng nhất, tên trùng với tên nền tảng ("Device Management"). Kết hợp cây địa bàn, bảng dữ liệu mật độ cao, chỉ số tài nguyên hệ thống, ba trục trạng thái độc lập (kết nối / ghi hình / AI).
- **M3 · Thêm thiết bị** — wizard nhiều đường vào (quét nhanh, nhập tay, tự đăng ký, RTSP, nhập hàng loạt).
- **M4 · Chi tiết một camera** — thông tin thiết bị, luồng video, lịch sử sự kiện AI.
- **M5 · Cấu hình AI/thuật toán** — chọn thuật toán, đặt vùng phát hiện, ngưỡng cảnh báo.

Vòng 1 chỉ dựng **M2** để chốt hệ thống thiết kế (màu, typography, spacing, component pattern: thẻ chỉ số, cây điều hướng, bảng dữ liệu, chip trạng thái) — các màn còn lại kế thừa sau khi chốt hướng.

## Mô hình trạng thái — bắt buộc thống nhất

Ba trục độc lập, không gộp:
1. **Kết nối**: Trực tuyến / Mất kết nối / Kết nối thất bại (ba trạng thái, không phải hai — phân biệt "thiết bị tắt" với "có gì chặn ở giữa") / **Đang xuống cấp** (kết nối được nhưng mất khung hình hoặc độ trễ cao — bổ sung riêng cho AICAM, rất thật với camera ngoài trời Việt Nam).
2. **Ghi hình**: Đang ghi / Không ghi / Lỗi lưu trữ.
3. **AI**: Đang phân tích / Tắt / Lỗi tài nguyên — kèm thuật toán đang chạy. Đây là trục khác biệt của AICAM so với đầu ghi thường, phải thấy ngay ở danh sách, không giấu trong trang chi tiết.

## Mật độ thông tin — CAO

Đây là phần mềm vận hành hạ tầng, không phải SaaS tiêu dùng. Bảng dữ liệu ưu tiên hiển thị nhiều cột hơn là card đẹp thưa thớt. Chấp nhận mật độ cao có chủ đích — đối lập với "sự tối giản lười biếng" mà luật chống slop cấm, ở đây mật độ cao *là* yêu cầu chức năng thật.

## Tông và khí chất

Theo brand-spec.md: Chủ quyền công nghệ · Đáng tin · Có thẩm quyền dựa trên thành tích · Hạ tầng quốc gia — **không phải giọng startup**. Tránh mọi mô-típ "SaaS AI 2024" (gradient tím, glow neon, card bo tròn lớn). Nền sáng làm chủ đạo (theo phân tích ở `doc/reference-ivss-design.md` §3.3: nền sáng đọc tốt hơn trong phòng làm việc sáng đèn, đồng thời né được bẫy "nền xanh đậm + neon" của phần mềm giám sát Trung Quốc).

## Ngôn ngữ

Tiếng Việt, có dấu đầy đủ. Số liệu dùng font tabular (Be Vietnam Pro tabular hoặc Source Sans 3) để cột số thẳng hàng.

## Kích thước và thích ứng

Thiết kế cho màn hình desktop 1440×900 trở lên (bối cảnh NOC dùng màn lớn). Không cần tối ưu mobile ở vòng này.

## Mô-típ thị giác — trả lời trước khi thiết kế (form suy ra từ nội dung)

- **Vai trò tự sự của M2**: đây là màn "bảng điều khiển sự thật" — nơi kỹ thuật viên xác nhận hệ thống có ổn không trước khi đào sâu bất kỳ đâu khác.
- **Mô-típ riêng của nội dung này**: ba trục trạng thái độc lập là thứ không sản phẩm giám sát tiêu dùng nào có — nên chip trạng thái ba màu/ba icon xếp cạnh nhau (không gộp thành một chấm) chính là "chữ ký thị giác" của toàn bộ hệ thống, lặp lại nhất quán ở mọi màn.
- **Mật độ vs khoảng trắng**: khoảng trắng phải phục vụ việc quét nhanh hàng trăm dòng, không phải phục vụ cảm giác "sang trọng" — bất kỳ khoảng trắng nào không giúp mắt tìm ra dòng bất thường nhanh hơn đều là lãng phí.

## Phải đổi gì cho AICAM so với tham chiếu IVSS

Xem đầy đủ ở `doc/reference-ivss-design.md` §4. Tóm tắt bắt buộc:
1. Màu chính = VNPT AI Primary `#0047BB`, sinh dải 10 sắc độ, không dùng `#1890ff`.
2. Cơ chế duyệt dữ liệu phải chịu vài nghìn thiết bị (cuộn ảo, cây nhiều cấp có số liệu nhánh, tổng hợp trước–chi tiết sau).
3. Thêm trạng thái "đang xuống cấp", trục AI, dấu vết tuân thủ Nghị định 13 (dữ liệu sinh trắc học) — IVSS không có ba thứ này.

## Vùng cấm

- Không dùng logo/tên/màu Dahua, Hikvision, Milestone.
- Không giữ `#1890ff` làm màu chính.
- Không sao chép nguyên khung hình ảnh chụp màn hình từ manual IVSS.
- Không dùng ảnh `ui-tinhnang-*.png` của VNPT AI làm nguồn bố cục (thuộc tính năng giao thông/OCR khác, không phải camera management — xem `brand-spec.md`).
- Không dùng gradient tím/glow neon làm mô-típ chủ đạo (slop SaaS AI).

## Ba hướng thiết kế bắt buộc (vòng 1, chỉ M2)

Ba bản độc lập, khác nhau về **cấu trúc bố cục** chứ không chỉ đổi màu:

1. **🎲 Bánh xe giây** — random 20 phong cách web trong thư viện huashu-design, rơi vào *Friendly Geometric Candy* (nút nổi 3D, bo tròn, màu kẹo, hướng Duolingo). Áp dụng có tiết chế cho bối cảnh NOC nghiêm túc: giữ tinh thần "thân thiện, dễ quét bằng mắt, tap target lớn" nhưng bỏ hẳn màu kẹo bão hoà cao — thay bằng thẻ trạng thái lớn bo góc vừa phải, ưu tiên xem theo card/nhóm hơn bảng dày đặc.
2. **🏆 Hiện thực tham chiếu** — bám sát `doc/reference-ivss-design.md`: token Ant Design 4 (bo góc 2px gần vuông, đen bán trong suốt, phân tầng nền nông ba mức), màu chính đổi sang VNPT AI, cấu trúc chỉ số tài nguyên trên cùng + cây địa bàn trái + bảng phải, nâng cấp mục §4.2 cho quy mô nghìn thiết bị.
3. **🧠 Trung tâm điều hành (mission control)** — lấy cảm hứng từ các bảng điều khiển hạ tầng trọng yếu (Palantir Foundry, SpaceX Mission Control, phòng điều hành lưới điện): chrome tối làm khung, panel dữ liệu sáng nổi lên, bản đồ/số liệu tổng quan làm trung tâm thay vì bảng liệt kê tuyến tính. Khác biệt cấu trúc rõ rệt: điều hướng trái tối màu, khu trung tâm ưu tiên tổng quan trạng thái theo địa bàn (không phải bảng dòng-cột ngay từ đầu), bảng chi tiết là lớp đào sâu thứ hai.
