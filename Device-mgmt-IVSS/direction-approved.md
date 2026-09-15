# Hướng thiết kế đã chốt — Device-mgmt-IVSS

**Ngày:** 15/09/2026

## Ba bản đã trình bày (màn neo: Quản lý thiết bị, `refs/04-cau-hinh/1-quan-ly-thiet-bi/`)

| Logic | File | Screenshot |
|---|---|---|
| ① 🎲 Bánh xe giây (`date +%S`=30 → style #11 "Dark Editorial", đổi màu sang navy/accent VNPT) | `design-demos/1-banh-xe-giay-dark-editorial.html` | `design-demos/1-banh-xe-giay-dark-editorial.png` |
| ② 🏆 Hiện thực tham chiếu (Datadog Infrastructure Monitoring — xác minh thật qua WebSearch, được Information is Beautiful Awards vinh danh) | `design-demos/2-hien-thuc-tham-chieu.html` | `design-demos/2-hien-thuc-tham-chieu.png` |
| ③ 🧠 Thiết kế sư hàng đầu (triết lý Edward Tufte — data-ink ratio, small multiples, tổng quan trước/chi tiết sau) | `design-demos/3-thiet-ke-su-hang-dau.html` | `design-demos/3-thiet-ke-su-hang-dau.png` |

## Lựa chọn của người dùng

Người dùng chọn qua AskUserQuestion: **"③ Thiết kế sư hàng đầu (Tufte)"**.

## Áp dụng cho phần còn lại

Hướng ③ dùng làm nền tảng hệ thống thiết kế chuẩn cho toàn bộ 20 nhóm màn hình tham chiếu ở `refs/`. Token và component pattern (topbar, resource-strip bullet graph, tree-pane cây địa bàn nhiều cấp có small-multiple, toolbar, bảng dữ liệu nhóm theo địa bàn, modal/tabs/form-grid, card, wizard-rail, matrix) được tách ra `mockups/shared.css` — mọi màn hình mới PHẢI include file này, không tự định nghĩa lại token màu/spacing.

| Màn | File |
|---|---|
| Quản lý thiết bị (04-cau-hinh/1) — màn neo, giữ nguyên bản đã chọn, chỉ tách CSS | `mockups/04-cau-hinh-1-quan-ly-thiet-bi.html` |

## Toàn bộ 20/20 nhóm đã dựng xong (15/09/2026)

| Nhóm refs | File mockup |
|---|---|
| 01-thiet-lap-ban-dau/2-cau-hinh-nhanh | `mockups/01-1-cau-hinh-nhanh.html` |
| 01-thiet-lap-ban-dau/3-dang-nhap | `mockups/01-2-dang-nhap.html` |
| 01-thiet-lap-ban-dau/4-trang-chu | `mockups/01-3-trang-chu.html` |
| 01-thiet-lap-ban-dau/5-them-camera | `mockups/01-4-them-camera.html` |
| 02-van-hanh-ai/01-tong-quan | `mockups/02-01-tong-quan-ai.html` |
| 02-van-hanh-ai/02-tuan-tra-theo-lich | `mockups/02-02-tuan-tra-theo-lich.html` |
| 02-van-hanh-ai/03-acupick | `mockups/02-03-acupick.html` |
| 02-van-hanh-ai/04-tim-kiem-nang-cao | `mockups/02-04-tim-kiem-nang-cao.html` |
| 02-van-hanh-ai/05-phat-hien-khuon-mat | `mockups/02-05-phat-hien-khuon-mat.html` |
| 02-van-hanh-ai/06-nhan-dien-khuon-mat | `mockups/02-06-nhan-dien-khuon-mat.html` |
| 02-van-hanh-ai/07-video-metadata | `mockups/02-07-video-metadata.html` |
| 02-van-hanh-ai/08-ivs-hanh-vi | `mockups/02-08-ivs-hanh-vi.html` |
| 02-van-hanh-ai/09-anpr-bien-so | `mockups/02-09-anpr-bien-so.html` |
| 02-van-hanh-ai/10-doi-sanh-bien-so | `mockups/02-10-doi-sanh-bien-so.html` |
| 02-van-hanh-ai/12-canh-bao-khac (chưa có trong INDEX.md gốc, đã bổ sung) | `mockups/02-12-canh-bao-khac.html` |
| 04-cau-hinh/1-quan-ly-thiet-bi | `mockups/04-cau-hinh-1-quan-ly-thiet-bi.html` (màn neo) |
| 04-cau-hinh/2-quan-ly-mang | `mockups/04-2-quan-ly-mang.html` |
| 04-cau-hinh/3-quan-ly-su-kien | `mockups/04-3-quan-ly-su-kien.html` + `mockups/04-3b-lien-ket-ai-lich-truc.html` |
| 04-cau-hinh/6-quan-ly-he-thong | `mockups/04-6a-he-thong-co-ban.html` + `mockups/04-6b-tai-khoan-bao-mat.html` |
| 05-bao-tri | `mockups/05-bao-tri-he-thong.html` |

Tất cả đều kế thừa `mockups/shared.css` (không sửa), dùng chung dữ liệu mẫu tiếng Việt nhất quán bối cảnh camera Hà Nội/KCN Bắc Thăng Long/cao tốc HN-HP đã thiết lập ở màn neo. Mỗi file đã tự QA bằng Playwright (0 console error). Xem chi tiết giả định/nguồn tham chiếu trong comment đầu mỗi file HTML.

**Chưa làm**: điều hướng chuyển trang thật giữa các file (hiện là mockup tĩnh từng trang/nhóm), hợp nhất sidebar điều hướng toàn cục xuyên suốt tất cả màn (mỗi file đang tự có topbar/breadcrumb riêng), rà soát đồng bộ 100% chi tiết dữ liệu giữa các file được dựng song song (vd tên kênh/IP có thể lệch nhẹ giữa vài file do các agent chạy độc lập).
