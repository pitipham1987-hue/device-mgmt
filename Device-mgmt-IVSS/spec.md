# Device-mgmt-IVSS · Spec thiết kế

## Sản phẩm là gì

Device-mgmt-IVSS là phần mềm quản trị thiết bị camera giám sát/AI, phạm vi chức năng bám theo cấu trúc thông tin của Dahua IVSS V7.1.0 (manual đã trích xuất 92 ảnh tham khảo, 20 nhóm, tại `refs/`), nhưng thiết kế lại toàn bộ thị giác dưới thương hiệu VNPT AI/AICAM (xem `brand-spec.md`) — không sao chép giao diện Dahua, không dùng logo/màu/tên Dahua. Đây là phần mềm B2B/B2G vận hành hạ tầng giám sát, người dùng là kỹ thuật viên/quản trị viên trung tâm điều hành (NOC), không phải người tiêu dùng cuối.

## Quan hệ với aicam-device-mgmt

Cùng thương hiệu VNPT AI, nhưng là **hệ thống thiết kế độc lập** — không bắt buộc dùng lại hướng "② Hiện thực tham chiếu" đã chốt cho M2 của aicam. Ba hướng thiết kế mới sẽ được đề xuất riêng cho dự án này.

## Phạm vi vòng 1

20 nhóm chức năng tham chiếu từ `refs/INDEX.md` + `refs/02-van-hanh-ai/12-canh-bao-khac` (nhóm có ảnh nhưng chưa ghi vào INDEX.md — cảnh báo bổ sung khác, 3 ảnh). Toàn bộ 20 nhóm sẽ được dựng mockup, nhưng vòng 1 chỉ chốt **một màn neo (anchor)** để xác lập hệ thống thiết kế chuẩn (màu, typography, spacing, component pattern), sau đó áp dụng cho 19 nhóm còn lại.

## Màn neo: Quản lý thiết bị (tương đương `refs/04-cau-hinh/1-quan-ly-thiet-bi/`)

Đây là màn tổng thể nhất — danh sách kênh/thiết bị camera đang kết nối vào hệ thống, có vai trò tương tự M2 "Danh sách thiết bị" trong aicam-device-mgmt. Từ 7 ảnh tham khảo (fig8-01, 8-02, 8-07, 8-08, 8-11, 8-12, 8-13), kiến trúc thông tin rút ra:

- **Thanh chỉ số tài nguyên trên cùng**: hai progress bar "Remaining Channels/Total Channels" và "Remaining Bandwidth/Total Bandwidth" — cho biết hệ thống còn bao nhiêu dung lượng nhận thêm thiết bị.
- **Thanh công cụ**: Add (nút chính, nổi bật), Modify IP, Export, Batch Import, Delete (disabled khi chưa chọn dòng nào), Filter (góc phải).
- **Bảng dữ liệu mật độ cao**: checkbox chọn dòng, Channel No., Status (chấm màu), Record Status (chấm màu — độc lập với Status), Channel Name, Address, Registration No., Port, Username, Password (ẩn dạng ●●●●●●), Manufacturer, Model, SN, Remote CH No., Operation (Edit/Delete dạng link). Cột có thể sort (mũi tên lên/xuống). Có thanh cuộn ngang khi nhiều cột, phân trang "Total N items" ở chân bảng.
- **Modal "Add Device"**: 4 tab đường vào (Quick Add / Manual Add / RTSP / Batch Import), bên trong Quick Add có nút "Start Search", input mật khẩu kết nối, nút Initialize/Modify IP, bảng kết quả quét (Initialization Status dạng badge màu xanh, Address, Device Model, Manufacturer, Port, Product Type, SN, Operation), progress bar bandwidth ở chân modal, nút OK/Cancel.
- **Modal "Modify IP"**: bảng SN/Address đang chọn, form Static IP / Subnet Mask / Default Gateway (dạng 4 ô octet), Incremental Value, Username/Password, cảnh báo màu vàng amber về điều kiện hỗ trợ, nút Cancel/Next.
- **Cấu hình luồng video** (fig8-12): tab Main/Sub Stream 1/Sub Stream 2, preview ảnh camera bên trái, form Encode Mode/Resolution/Frame Rate/Bit Rate bên phải, toggle SVG, nút Copy/Refresh/Save.
- **Cấu hình OSD** (fig8-13): preview ảnh camera có overlay kéo-thả (khung vàng đánh dấu vị trí overlay), toggle Device Name/Time Overlay/Location/Privacy Masking, bảng danh sách Privacy Masking đã thêm.

Ba trục trạng thái độc lập (kết nối / ghi hình) đã thấy rõ ở bảng — cân nhắc có nên bổ sung trục AI thứ ba (đang phân tích/tắt/lỗi) như đã làm ở aicam-device-mgmt, vì đây là điểm khác biệt của sản phẩm AI camera so với đầu ghi thường; để ba hướng thiết kế tự quyết định có đưa vào màn neo vòng 1 hay để lại cho màn cấu hình AI riêng.

## Đối tượng dùng & bối cảnh

Kỹ thuật viên NOC, quản trị viên hệ thống — màn hình lớn, phòng sáng đèn, phiên làm việc dài, cần quét trạng thái nhanh qua nhiều dòng.

## Mật độ thông tin

Cao — bảng dữ liệu ưu tiên nhiều cột hơn card thưa thớt, đúng như ảnh gốc Dahua đã thể hiện. Đây là yêu cầu chức năng thật, không phải chọn lựa thẩm mỹ.

## Ngôn ngữ & kích thước

Tiếng Việt có dấu đầy đủ (dịch nhãn từ tiếng Anh trong ảnh gốc, không giữ nguyên tiếng Anh trừ thuật ngữ kỹ thuật như SN, IP, RTSP, ONVIF). Desktop 1440×900 trở lên.

## Vùng cấm

Xem `brand-spec.md` §Vùng cấm — đặc biệt: không đưa ảnh chụp màn hình gốc `refs/*.png` vào file mockup, không tái tạo pixel-for-pixel, không dùng logo/tên Dahua.

## Ba hướng thiết kế (vòng 1, chỉ màn neo Quản lý thiết bị)

Ba bản độc lập khác nhau về **cấu trúc bố cục**, dùng chung tài sản thương hiệu VNPT AI (`brand-spec.md`), theo đúng ba logic bắt buộc của huashu-design Fallback: 🎲 bánh xe giây ngẫu nhiên, 🏆 tham chiếu thực tế (một dashboard/NOC console thực sự xuất sắc), 🧠 thiết kế sư hàng đầu (tưởng tượng studio phù hợp nhất cho bối cảnh hạ tầng giám sát trọng yếu). Sau khi người dùng chọn, hướng đó trở thành hệ thống thiết kế chuẩn áp dụng cho 19 nhóm còn lại.
