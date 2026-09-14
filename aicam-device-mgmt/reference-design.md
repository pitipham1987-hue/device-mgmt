# AICAM — Ngôn ngữ thiết kế tham chiếu (chỉ dùng cho logic ② Hiện thực tham chiếu)

> Nguồn: quan sát cấu trúc và mô hình tương tác từ `Dahua_IVSS_User_Manual_V7_1_0.pdf` (chương 5.4, 5.5.2, 7.1, 8.1) + token thị giác Ant Design 4.x (MIT license). Đây là kiến trúc thông tin và token thị giác, không phải tài sản độc quyền — logo/tên/màu Dahua-Hikvision-Milestone tuyệt đối không dùng.

## 3. Kiến trúc thông tin trích từ IVSS

### 3.1 Khung màn quản lý thiết bị

```
┌──────────────────────────────────────────────────────────────────┐
│ [Kênh ▓▓▓▓▓░░ 100/128]   [Băng thông ▓▓▓▓▓▓░ 417/512 Mbps]      │ ← chỉ số tài nguyên, TRÊN CÙNG
├──────────────────────────────────────────────────────────────────┤
│ [Thêm] [Sửa IP] [Xuất] [Nhập hàng loạt] [Xoá]          ▽ Lọc     │ ← mờ khi chưa chọn dòng
├─────────────┬────────────────────────────────────────────────────┤
│             │ ☐ │Kênh│Kết nối│Ghi hình│Tên│Địa chỉ│…│Thao tác     │
│  Cây        │ ☐ │ 1  │   ●   │   ●    │…                          │
│  thiết bị   │ ☐ │ 2  │   ●   │   ●    │…                          │
│             │                                                     │
│ [+]         │                                                     │ ← lối thêm thứ hai
└─────────────┴────────────────────────────────────────────────────┘
```

Bốn quyết định đáng giữ:

**a) Chỉ số tài nguyên đặt trên cùng, không giấu trong trang cấu hình.** Người vận hành luôn thấy mình còn bao nhiêu dư địa. Hiếm — hầu hết sản phẩm chôn thông tin này đâu đó trong System Info.

**b) Hai trục trạng thái tách riêng thành hai cột.** Đã mở rộng thành ba trục cho AICAM (xem `spec.md`).

**c) Ba trạng thái kết nối, không phải hai.** Tách "kết nối thất bại" khỏi "mất kết nối" phản ánh khác biệt thật: một cái là thiết bị tắt, một cái là có gì đó chặn ở giữa.

**d) Hai lối vào cho cùng một hành động.** Nút "Thêm" ở thanh tác vụ cho người mới, nút "+" góc dưới trái cho người quen tay. Phục vụ hai nhịp làm việc khác nhau.

### 3.2 Trang chủ kiểu tile

Không dùng sidebar. Sáu ô chức năng lưới 3×2, mỗi ô gồm tiêu đề, một dòng mô tả, hình minh hoạ, và **tối đa ba shortcut đi thẳng vào màn con**. Bên dưới là hàng cấu hình tách riêng.

Điểm hay: shortcut cấp hai ngay trên tile cho phép bỏ qua một cấp điều hướng. Người vận hành đi thẳng, người mới vẫn có tile để định hướng.

### 3.3 Màn xem trực tiếp

Bố cục ba cột: cây thiết bị và nhóm khung nhìn bên trái, lưới video ở giữa, dòng sự kiện AI bên phải (ảnh chụp kèm nhãn và thời gian). Thanh dưới chia ba cụm rõ ràng: điều khiển trái, chọn kiểu lưới giữa, công cụ phải.

**Đáng chú ý: IVSS V7 dùng nền sáng cho cả màn xem trực tiếp**, không phải nền tối như đa số NVR. Chrome là xám nhạt, chỉ khung video tối — và tối vì nội dung video tối, không phải vì thiết kế.

### 3.4 Năm đường thêm thiết bị

Bê nguyên vì nó phản ánh thực tế triển khai:

| Cách | Dùng khi |
|---|---|
| Quét nhanh (dò mạng LAN) | Không biết IP chính xác |
| Nhập tay | Vài thiết bị, đã biết IP và tài khoản |
| Tự đăng ký | Thiết bị sau NAT, không IP cố định, chủ động gọi về |
| RTSP | Thiết bị stream không chuẩn hãng |
| Nhập hàng loạt theo template | Nhiều thiết bị, mỗi cái một thông tin khác nhau |

Kèm: luồng khởi tạo thiết bị chưa đặt mật khẩu đi trước; đổi IP hàng loạt theo bước tăng dần, tự bỏ qua IP trùng; bộ lọc theo trạng thái khởi tạo.

## 4. Ngôn ngữ thiết kế tham chiếu

### 4.1 Phát hiện nền tảng

**Giao diện IVSS V7.1.0 dựng trên Ant Design 4.x, gần như theme mặc định.**

Bằng chứng — trích màu trực tiếp từ ảnh giao diện trong manual, render 300 DPI rồi lượng tử hoá:

| Đo được | Token Ant Design 4 | Dùng ở đâu |
|---|---|---|
| `#1890fe` | `@primary-color: #1890ff` | Nút Add, link Edit, thanh tiến trình |
| `#51c319` | `@success-color: #52c41a` | Chấm trực tuyến |
| `#fdaa15` | `@warning-color: #faad14` | Chấm cảnh báo |
| `#fc5352` | danger `#ff4d4f` | Chấm lỗi, link Delete |
| `#f1f1f1` | `#f0f0f0` | Nền hàng tiêu đề |
| `#d8d8d8` | `#d9d9d9` | Viền bảng, viền nút phụ |

Lệch 1–2 đơn vị là nhiễu nén ảnh in, không phải tuỳ biến.

**Ba hệ quả:**

1. **Không có vấn đề bản quyền ở tầng thị giác.** Ant Design giấy phép MIT. Phần riêng của Dahua chỉ còn logo và cách sắp xếp màn hình — logo thay bằng VNPT AI, cách sắp xếp không ai độc quyền được.
2. **Bàn giao rẻ hơn hẳn.** Prototype theo token Ant Design thì đội frontend cài `antd` là ra gần đúng.
3. **Cảm giác quen thuộc là thật.** Khách hàng Việt Nam đã quen giao diện Dahua và Hikvision nhiều năm; cảm giác đó phần lớn đến từ Ant Design, và lấy được một cách hợp pháp.

*Lưu ý phiên bản*: Ant Design 5 đổi màu chính sang `#1677ff` và chuyển sang design token. Dùng bảng màu v4 nếu muốn khớp cảm giác IVSS. Nhưng đằng nào cũng phải thay màu chính — xem §5.1.

### 4.2 Token tham chiếu

```css
:root {
  /* Nền — phân tầng rất nông, chỉ 3 mức */
  --bg-body:        #f0f2f5;
  --bg-container:   #ffffff;
  --bg-header:      #fafafa;   /* hàng tiêu đề bảng, hàng hover */

  /* Viền — hai mức, mảnh */
  --border-base:    #d9d9d9;
  --border-split:   #f0f0f0;

  /* Chữ — đen bán trong suốt, KHÔNG dùng xám đặc */
  --text-heading:   rgba(0,0,0,0.85);
  --text-body:      rgba(0,0,0,0.65);
  --text-secondary: rgba(0,0,0,0.45);
  --text-disabled:  rgba(0,0,0,0.25);

  /* Chức năng — GIỮ NGUYÊN, đây là quy ước ngành */
  --color-success:  #52c41a;
  --color-warning:  #faad14;
  --color-error:    #ff4d4f;

  /* Màu chính — LẤY TỪ brand-spec.md, không dùng #1890ff */
  --brand-primary:  #0047BB;   /* VNPT AI, xem brand-spec.md */
  --brand-accent:   #02AAFA;   /* gradient/accent phụ trợ, xem brand-spec.md */

  /* Hình học */
  --radius:         2px;
  --font-size-base: 14px;
  --font-size-sm:   12px;
  --control-height: 32px;
  --spacing-unit:   8px;   /* mọi khoảng cách là bội của 8 */
}
```

### 4.3 Ba chi tiết quyết định "cảm giác", đừng bỏ qua

- **Bo góc 2px, gần như vuông.** Đây là thứ phân biệt phần mềm hạ tầng với SaaS tiêu dùng. Bo 8–12px lập tức thành "web app hiện đại", mất chất công cụ vận hành.
- **Chữ dùng đen bán trong suốt, không dùng xám đặc.** `rgba(0,0,0,0.65)` nhìn khác `#595959` — mềm hơn, và tự thích ứng khi nền đổi.
- **Phân tầng nền rất nông.** Ba mức, chênh nhau rất ít. Không đổ bóng nặng, không thẻ nổi. Giao diện phẳng và yên, để dữ liệu nổi lên chứ không phải khung.

### 4.4 Quyết định nền sáng

IVSS dùng nền sáng cho toàn bộ chrome, kể cả màn xem trực tiếp. Với AICAM đây là lựa chọn đáng theo: đọc được trong phòng làm việc sáng đèn, và tránh luôn cái bẫy "nền xanh đậm đều + neon" mà luật chống slop của Huashu cấm đích danh.

Nếu một phương án muốn đi tông tối thì phải là **dark có tác giả**: phân tầng nền rõ ràng, accent lấy từ màu trạng thái chứ không phải màu trang trí. Không chấp nhận `#0D1117` + neon xanh/tím.

### 4.5 Chữ

Font phải thoả hai tiêu chí bắt buộc:

- **Hỗ trợ đầy đủ dấu tiếng Việt** — kiểm tra thật với chuỗi `ầ ế ộ ữ ỹ ằ ọ`. Nhiều font mở gãy ở tổ hợp hai dấu.
- **Có biến thể tabular cho số** — màn này đầy IP, cổng, serial, số kênh. Số không thẳng cột thì bảng thành mớ hỗn độn.

Theo `brand-spec.md`: **Be Vietnam Pro** (Display + Body). ⚠️ Luật chống slop cấm dùng Inter làm font tiêu đề — nếu chọn Inter thì chỉ dùng cho chữ thân.

## 5. Phải đổi gì cho AICAM (so với IVSS)

Không sao chép nguyên si. Ba nhóm thay đổi bắt buộc.

### 5.1 Màu chính — phải là màu VNPT AI

Thay đổi quan trọng nhất. Giữ nguyên xanh mặc định Ant Design thì sản phẩm nhìn giống mọi phần mềm dựng trên antd — và giống Dahua đủ để gây khó xử trong hồ sơ thầu.

Màu chính AICAM: **`#0047BB`** (xem `brand-spec.md` để biết nguồn trích xuất). Giữ nguyên bốn màu chức năng — quy ước ngành, đổi chỉ gây nhầm. Màu chính VNPT không trùng vùng màu chức năng nào (success xanh lá, warning cam, error đỏ) nên không cần điều chỉnh thêm.

### 5.2 Cơ chế duyệt dữ liệu — IVSS không scale tới hàng nghìn

Mô hình IVSS thiết kế cho một đầu ghi ~128 kênh. Với quy mô cấp tỉnh/thành thì vỡ:

| Của IVSS | Thay bằng |
|---|---|
| Bảng phân trang thường | Cuộn ảo, lọc phía máy chủ, ô tìm kiếm luôn hiện |
| Cây thiết bị một cấp | Cây nhiều cấp, mỗi nút hiện số liệu tổng hợp của nhánh |
| Lối vào là danh sách phẳng | Lối vào là bảng tổng hợp trạng thái; danh sách phẳng là chế độ đào sâu |
| Quét trạng thái theo dòng | Tổng hợp trước, chi tiết sau — "240 thiết bị · 7 lỗi" rồi mới bung |

### 5.3 Bổ sung mà IVSS không có

- **Trạng thái "đang xuống cấp"** — kết nối được nhưng mất khung hình hoặc độ trễ cao. Rất thật với camera ngoài trời ở Việt Nam.
- **Trục AI** — camera có bật phân tích không, thuật toán nào, tài nguyên tính toán đã cấp bao nhiêu. Giá trị khác biệt của AICAM, phải nhìn thấy ngay ở danh sách.
- **Dấu vết tuân thủ Nghị định 13** — camera nào đang xử lý dữ liệu sinh trắc học, dữ liệu lưu ở đâu, thời hạn lưu bao lâu. IVSS không có vì không chịu ràng buộc này. Đây là điểm mạnh bán hàng ở thị trường Việt Nam, không phải gánh nặng tuân thủ.

## Nguồn

- **Mô hình tương tác và kiến trúc thông tin**: `Dahua_IVSS_User_Manual_V7_1_0.pdf`, chương 5.4, 5.5.2, 7.1, 8.1.
- **Bảng màu**: đo trực tiếp từ ảnh giao diện trong manual — render trang 191 và 227 ở 300 DPI, crop vùng ảnh, lượng tử hoá median-cut, tách màu bão hoà và màu trung tính.
- **Token Ant Design**: tài liệu chính thức `ant.design` và `4x.ant.design`, giấy phép MIT.

Manual IVSS thuộc bản quyền Dahua. Tài liệu này ghi lại **quan sát về cấu trúc và mô hình tương tác** để tham khảo thiết kế, không tái tạo nội dung manual.
