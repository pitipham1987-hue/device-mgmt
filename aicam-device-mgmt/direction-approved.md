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

## Áp dụng cho phần còn lại

Hướng ② dùng làm nền tảng hệ thống thiết kế (token Ant Design 4 đã đổi màu chính sang `#0047BB`, bo góc 2px, phân tầng nền 3 mức, chip trạng thái ba trục, cây địa bàn trái + bảng phải) để tiếp tục dựng:

- M1 · Trang chủ (khuôn Hub tile — 6 ô chức năng lưới 3×2)
- M3 · Thêm thiết bị (Wizard 5 đường vào + bảng kết quả quét)
- M4 · Chi tiết một camera
- M5 · Cấu hình AI / thuật toán

M2 giữ nguyên bản `2-hien-thuc-tham-chieu.html` làm chuẩn tham chiếu component (sidebar, tree, table, resource strip, status tag).
