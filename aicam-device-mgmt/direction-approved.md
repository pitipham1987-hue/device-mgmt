# Hướng thiết kế đã chốt

**Ngày:** 15/09/2026

## Ba bản đã trình bày (vòng 1, M2 · Danh sách thiết bị)

| Logic | File | Screenshot |
|---|---|---|
| ① Bánh xe giây | `design-demos/1-banh-xe-giay.html` | `design-demos/1-banh-xe-giay.png` |
| ② Hiện thực tham chiếu | `design-demos/2-hien-thuc-tham-chieu.html` | `design-demos/2-hien-thuc-tham-chieu.png` |
| ③ Terminal mật độ cao | `design-demos/3-terminal-mat-do-cao.html` | `design-demos/3-terminal-mat-do-cao.png` |

## Lựa chọn của người dùng

Người dùng chọn qua AskUserQuestion: **"② Hiện thực tham chiếu"**.

> Bám mô hình IVSS quen thuộc (thanh chỉ số trên cùng · cây địa bàn trái · bảng phải), nâng cấp phần duyệt dữ liệu cho quy mô nghìn thiết bị. Đây là bản an toàn, người vận hành cũ nhận ra ngay.

## Áp dụng cho phần còn lại

Hướng ② dùng làm nền tảng hệ thống thiết kế (màu sắc, typography, spacing, component pattern: thẻ chỉ số, cây điều hướng, bảng dữ liệu, chip trạng thái ba trục) để tiếp tục dựng:

- M1 · Trang chủ (khuôn A — Hub tile)
- M3 · Thêm thiết bị (khuôn I — Wizard + khuôn C — Bảng, theo 5 đường thêm ở `reference-design.md` §3.4)
- M4 · Chi tiết một camera
- M5 · Cấu hình AI / thuật toán (đầu tư thị giác nhiều nhất — khuôn G+D)

M2 giữ nguyên bản `2-hien-thuc-tham-chieu.html` làm chuẩn, có thể tinh chỉnh nhỏ khi tích hợp vào bộ điều hướng chung 5 màn.
