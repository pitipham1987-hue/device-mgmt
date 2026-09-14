# AICAM — Quản lý thiết bị · SPEC

## Sản phẩm là gì

AICAM là nền tảng quản lý camera AI của VNPT AI, triển khai cho khách hàng doanh nghiệp và khối chính quyền tại Việt Nam. Phần cần thiết kế là **cụm màn quản lý thiết bị** — nơi kỹ thuật viên và quản trị viên đưa camera vào hệ thống, theo dõi tình trạng, và cấu hình phân tích AI trên từng camera.

Đây là xương sống vận hành của nền tảng. Nếu màn này khó dùng thì mọi năng lực AI phía sau đều không tới được người dùng.

## Người dùng và bối cảnh

Ba nhóm, độ thành thạo rất khác nhau. Màn phải phục vụ cả ba mà không bắt nhóm nào chịu giao diện tối ưu cho nhóm khác.

| Nhóm | Bối cảnh | Việc chính |
|---|---|---|
| Kỹ thuật viên triển khai | Ngồi tại site, laptop màn nhỏ, mạng chập chờn | Thêm thiết bị hàng loạt, xử lý thiết bị không lên |
| Nhân viên trực NOC | Màn hình lớn hoặc màn ghép, trực 8 tiếng | Quét trạng thái, khoanh vùng sự cố — cần biết trong 3 giây hôm nay bao nhiêu camera chết và chết ở đâu |
| Quản trị viên hệ thống | Văn phòng | Cấu hình thuật toán AI, phân quyền, kiểm tra dung lượng |

## Quy mô

Từ **vài chục camera một site** đến **hàng nghìn camera cấp tỉnh/thành**. Ràng buộc cứng, quyết định cấu trúc:

- Không dùng bảng phân trang thường cho danh sách lớn — cuộn ảo hoặc lọc phía máy chủ.
- Cây thiết bị nhiều cấp: tỉnh → quận/huyện → phường/xã → site → camera. **Mỗi nút phải hiện số liệu tổng hợp của nhánh** (vd: 240 thiết bị · 7 lỗi) để khoanh vùng mà không cần mở ra.
- Ở quy mô lớn, **lối vào mặc định là bảng tổng hợp trạng thái, không phải danh sách phẳng**. Danh sách phẳng là chế độ đào sâu.
- Phải xử lý được trạng thái "đang tải một phần" — 5.000 dòng không tải hết cùng lúc.

## Năm màn

**M1 · Trang chủ (kiểu tile)** — lối vào toàn nền tảng. Ô chức năng lớn, mỗi ô kèm tối đa 3 shortcut vào thẳng màn con. Một hàng cấu hình riêng bên dưới. Trên cùng là tình trạng hệ thống tổng quan.

**M2 · Danh sách thiết bị** — màn trung tâm, làm trước. Gồm: thanh chỉ số tài nguyên trên cùng (kênh đã dùng/tổng, băng thông đã dùng/tổng, dung lượng lưu trữ), cây thiết bị nhiều cấp có số liệu tổng hợp, khu vực chính hiển thị thiết bị, thanh tác vụ hàng loạt chỉ sáng khi có dòng được chọn, bộ lọc theo trạng thái / site / hãng / model / có bật AI.

**M3 · Thêm thiết bị** — năm đường thêm (xem reference-design.md §3.4) trình bày sao cho người dùng chọn đúng đường ngay lần đầu, không phải thử từng cái. Gồm luồng khởi tạo thiết bị chưa đặt mật khẩu, và đổi IP hàng loạt theo bước tăng dần có xử lý trùng IP.

**M4 · Chi tiết một camera** — thông tin nhận dạng, tình trạng kết nối và ghi hình theo thời gian, thông số luồng, lịch sử sự kiện gần đây, khung xem trực tiếp, và các thao tác (sửa, thử kết nối, khởi động lại, gỡ khỏi hệ thống).

**M5 · Cấu hình AI / thuật toán** — gán bài toán phân tích cho camera (nhận diện khuôn mặt, biển số, đếm người, xâm nhập vùng…), cấu hình tham số, vẽ vùng quan tâm trên khung hình, quản lý tài nguyên tính toán đã cấp phát. **Đây là màn thể hiện giá trị khác biệt của AICAM so với đầu ghi thường — đầu tư thị giác nhiều nhất vào màn này.**

## Mô hình trạng thái — bắt buộc thống nhất trên cả 5 màn

**Ba trục độc lập, không được gộp:**

| Trục | Các giá trị |
|---|---|
| Kết nối | trực tuyến · mất kết nối · kết nối thất bại · **đang xuống cấp** (kết nối được nhưng mất khung hình hoặc độ trễ cao) |
| Ghi hình | đang ghi · không ghi · lỗi ghi |
| AI | chưa bật · đang chạy · lỗi thuật toán · thiếu tài nguyên tính toán |

Camera trực tuyến nhưng không ghi là sự cố khác hẳn camera mất kết nối. Camera đang ghi nhưng thuật toán chết là sự cố thứ ba. Thiết kế phải thể hiện cả ba cùng lúc mà không rối.

🔴 **Không được mã hoá trạng thái chỉ bằng màu.** Mọi chỉ báo phải kèm hình dạng hoặc nhãn chữ — màn trực NOC thường bị ám màu, và có người dùng mù màu.

## Tông và khí chất

Đáng tin, điềm tĩnh, có thẩm quyền. Đây là công cụ người ta nhìn 8 tiếng một ngày, không phải trang bán hàng. Không hào nhoáng, không hoạt hoạ trang trí. Nhưng **không được nhạt** — phải nhìn ra ngay đây là sản phẩm có người thiết kế, không phải bảng Bootstrap mặc định.

Khí chất thương hiệu VNPT AI (rút từ trang giới thiệu chính thức, xem `brand-spec.md`): **chủ quyền công nghệ**, giọng hạ tầng quốc gia chứ không phải giọng startup; có thẩm quyền dựa trên thành tích đã kiểm chứng; lấy con người làm trung tâm, coi trọng đạo đức AI và bảo mật dữ liệu. Gần với viễn thông hơn là với SaaS.

## Mật độ thông tin — CAO

Đây là sản phẩm dữ liệu và giám sát. Áp chế độ **高密度型** theo bảng xử lý ngoại lệ trong `SKILL.md` — mỗi màn tối thiểu 3 điểm thông tin *có nội dung*. **Nguyên tắc tiết chế mặc định của skill không áp dụng ở đây.**

Nhưng "mật độ cao" nghĩa là **thêm thông tin thật**, không phải thêm trang trí. Icon trang trí vẫn bị cấm như thường.

## Kích thước và thích ứng

Thiết kế ở **1920×1080**, nhưng bố cục theo **chiều rộng container, không theo chiều rộng khung nhìn** — màn này còn được nhúng làm module trong nền tảng IOC hiện có, nơi nó chỉ chiếm một phần màn hình. Kiểm tra ở cả 1920×1080 và 1440×900.

Đáy cứng: chữ thân ≥14px, nhãn phụ ≥12px, tương phản chữ thân ≥4.5:1. Không phong cách nào được phá.

## Ngôn ngữ

Toàn bộ nhãn, tiêu đề, thông báo bằng **tiếng Việt có dấu đầy đủ**. Thuật ngữ kỹ thuật đã quen giữ nguyên tiếng Anh (RTSP, ONVIF, IP, serial, stream). Không viết tắt kiểu chat.

## Mô-típ thị giác — trả lời trước khi thiết kế

Nội dung này có một đặc thù không sản phẩm nào khác có: **camera là vật thể có vị trí trong không gian thật, và nó hoặc đang nhìn thấy thứ gì đó, hoặc đang mù**. Mọi màn phải mọc ra từ hai ý niệm đó — *phủ sóng không gian* và *trạng thái nhìn thấy / mù*.

Đừng thiết kế nó như một bảng CRM có thêm cột trạng thái. Mỗi phương án phải nêu được: hình thức của nó mọc ra từ chỗ nào trong nội dung. Trả lời không được câu đó = đang áp khuôn mẫu.

## Phải đổi gì cho AICAM so với tham chiếu IVSS

Không sao chép nguyên si. Ba nhóm thay đổi bắt buộc — chi tiết đầy đủ ở `reference-design.md` §5, tóm tắt:

1. **Màu chính phải là màu VNPT AI** (`#0047BB`, xem `brand-spec.md`) — không giữ `#1890ff` mặc định Ant Design. Giữ nguyên 4 màu chức năng (success/warning/error + info).
2. **Cơ chế duyệt dữ liệu phải chịu được quy mô nghìn thiết bị** — IVSS chỉ thiết kế cho ~128 kênh. Bảng phân trang → cuộn ảo + lọc phía máy chủ; cây một cấp → cây nhiều cấp có số liệu tổng hợp; lối vào danh sách phẳng → lối vào bảng tổng hợp trạng thái.
3. **Bổ sung mà IVSS không có**: trạng thái "đang xuống cấp"; trục AI (bật/tắt, thuật toán, tài nguyên tính toán); dấu vết tuân thủ Nghị định 13 (camera nào xử lý dữ liệu sinh trắc học, lưu ở đâu, thời hạn lưu).

## Vùng cấm

- ❌ Dùng logo, tên, hoặc màu nhận diện của Dahua / Hikvision / Milestone ở bất kỳ đâu.
- ❌ Sao chép nguyên ảnh chụp giao diện từ manual vào bản thiết kế.
- ❌ Giữ `#1890ff` làm màu chính.
- ❌ Để logic ① và ③ (xem dưới) bắt chước IVSS.
- ❌ Đoán màu thương hiệu theo trí nhớ — dùng đúng mã trong `brand-spec.md`.
- ❌ Bo góc 8px+ — mất chất công cụ vận hành.
- ❌ Mã hoá trạng thái chỉ bằng màu.
- ❌ Gradient tím, emoji làm icon.
- ❌ Hiển thị mật khẩu thiết bị dạng chữ, kể cả trong bản mẫu.
- ❌ Ảnh khuôn mặt thật trong dữ liệu mẫu — dùng ảnh đã che mặt hoặc khối giữ chỗ có nhãn (ràng buộc Nghị định 13 về dữ liệu sinh trắc học).
- ❌ "Lorem ipsum", "Channel1 / Channel2", địa danh nước ngoài. Dùng địa danh Việt Nam thật và tên site tiếng Việt.

## Ba hướng thiết kế bắt buộc (vòng 1, chỉ M2)

Ba bản **bắt buộc khác nhau về cấu trúc bố cục**, không chỉ khác bảng màu.

| Logic | Neo vào | Kỳ vọng |
|---|---|---|
| ② Hiện thực tham chiếu | **IVSS** — đọc `reference-design.md` | Bản an toàn, người vận hành cũ nhận ra ngay. Giữ mô hình đã kiểm chứng, nâng cấp phần duyệt dữ liệu cho quy mô nghìn |
| ① Bánh xe giây | Bốc ngẫu nhiên từ thư viện phong cách | Bản phá khuôn. Có thể ra thứ không giống NVR nào. Cứ để chạy — mục đích là thấy còn đường nào khác |
| ③ Nhà thiết kế giỏi nhất | Triết lý terminal mật độ cao (Bloomberg Terminal / Datadog / Linear) | Bàn phím là chính, thông tin dày, không thừa pixel, dành cho người dùng chuyên nghiệp cả ngày |

🔴 **Chỉ logic ② được đọc `reference-design.md`.** Nếu cả ba đều bắt chước IVSS thì cửa ba phương án mất sạch ý nghĩa — không còn gì để chọn.
