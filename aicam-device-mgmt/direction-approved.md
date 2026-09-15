# Hướng thiết kế đã chốt

**Ngày:** 15/09/2026 (vòng làm lại từ đầu theo yêu cầu người dùng)

## Ba bản đã trình bày (vòng 1, M2 · Danh sách thiết bị)

| Logic | File | Screenshot |
|---|---|---|
| ① Thẻ trạng thái thân thiện (bánh xe giây → Friendly Geometric, tiết chế) | `design-demos/1-the-trang-thai-than-thien.html` | `design-demos/1-the-trang-thai-than-thien.png` |
| ② Hiện thực tham chiếu (IVSS + Ant Design token + màu VNPT) | `design-demos/2-hien-thuc-tham-chieu.html` | `design-demos/2-hien-thuc-tham-chieu.png` |
| ③ Trung tâm điều hành (mission control, chrome tối) | `design-demos/3-trung-tam-dieu-hanh.html` | `design-demos/3-trung-tam-dieu-hanh.png` |

## Lựa chọn của người dùng

Người dùng chọn qua AskUserQuestion: **"② Hiện thực tham chiếu"**.

## Áp dụng cho phần còn lại — đã dựng xong cả 5 màn

Hướng ② dùng làm nền tảng hệ thống thiết kế (token Ant Design 4 đã đổi màu chính sang `#0047BB`, bo góc 2px, phân tầng nền 3 mức, chip trạng thái ba trục, cây địa bàn trái + bảng phải), hệ thống dùng chung đặt tại `mockups/shared.css`:

| Màn | File |
|---|---|
| M1 · Trang chủ (Hub tile — 6 ô chức năng lưới 3×2 + shortcut cấp hai + hàng cấu hình nhanh) | `mockups/M1-trang-chu.html` |
| M2 · Danh sách thiết bị (giữ nguyên bản demo đã chọn) | `design-demos/2-hien-thuc-tham-chieu.html` |
| M3 · Thêm thiết bị (Wizard 5 đường vào + bảng kết quả quét) | `mockups/M3-them-thiet-bi.html` |
| M4 · Chi tiết một camera (thông tin thiết bị + luồng video + dòng sự kiện AI + ghi chú tuân thủ NĐ13) | `mockups/M4-chi-tiet-camera.html` |
| M5 · Cấu hình AI / thuật toán (chọn thuật toán + vẽ vùng phát hiện SVG + ngưỡng cảnh báo + lịch kích hoạt) | `mockups/M5-cau-hinh-ai.html` |

Chưa dựng: điều hướng chuyển trang thật (mới là mockup tĩnh từng màn), trang Sự kiện & cảnh báo, Cấu hình hệ thống (nằm trong menu nhưng ngoài phạm vi 5 màn cốt lõi của vòng 1).
