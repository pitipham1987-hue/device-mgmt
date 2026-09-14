# reference-ivss-design.md

> **Vai trò**: anchor cho **logic ② (hiện thực tham chiếu)** trong quy trình ba phương án của huashu-design. Không phải `brand-spec.md`.
>
> `brand-spec.md` = tài sản thương hiệu của AICAM (logo, màu VNPT AI).
> File này = ngôn ngữ thiết kế tham chiếu (cấu trúc, mật độ, mô hình tương tác).
>
> Agent phải đọc cả hai. Khi hai file mâu thuẫn về màu sắc hay nhận diện, **`brand-spec.md` thắng**.

---

## 1. Phát hiện nền tảng

**Giao diện IVSS V7.1.0 (PC client / web) dựng trên Ant Design 4.x, dùng gần như nguyên theme mặc định.**

Bằng chứng — trích màu trực tiếp từ ảnh chụp giao diện trong manual, render ở 300 DPI rồi lượng tử hoá:

| Đo được từ manual | Token Ant Design 4 | Dùng ở đâu trong IVSS |
|---|---|---|
| `#1890fe` | `@primary-color: #1890ff` | Nút Add, link Edit, thanh tiến trình tài nguyên |
| `#51c319` | `@success-color: #52c41a` | Chấm trạng thái trực tuyến |
| `#fdaa15` | `@warning-color: #faad14` | Chấm trạng thái cảnh báo |
| `#fc5352` | danger `#ff4d4f` | Chấm lỗi, link Delete |
| `#f1f1f1` | `#f0f0f0` | Nền hàng tiêu đề bảng |
| `#d8d8d8` | `#d9d9d9` | Viền bảng, viền nút phụ |
| `#ffffff` (56% diện tích) | `colorBgContainer` | Nền chính |

Sai lệch 1–2 đơn vị là nhiễu nén ảnh in, không phải tuỳ biến thiết kế.

### Vì sao điều này quan trọng

Ba hệ quả thực tế:

1. **Không có vấn đề bản quyền ở tầng thị giác.** Ant Design giấy phép MIT, ai cũng dùng được kể cả thương mại. Phần riêng của Dahua chỉ còn là logo và cách sắp xếp màn hình — logo thì anh thay bằng VNPT AI, cách sắp xếp thì không ai độc quyền được.
2. **Bàn giao rẻ hơn hẳn.** Prototype dựng theo token Ant Design thì đội frontend cài `antd` là ra gần đúng, không phải viết lại CSS từ đầu.
3. **Cảm giác quen thuộc là thật, không phải tưởng tượng.** Khách hàng Việt Nam đã quen giao diện Dahua và Hikvision suốt nhiều năm. Cảm giác đó phần lớn đến từ Ant Design, và anh lấy được nó một cách hợp pháp.

### Lưu ý phiên bản

Ant Design 5 đổi màu chính từ `#1890ff` sang `#1677ff` và chuyển từ biến Less sang design token. Nếu muốn khớp đúng "cảm giác IVSS" thì dùng bảng màu v4 (`#1890ff`); nếu ưu tiên dùng thư viện hiện hành thì dùng v5 và chấp nhận lệch nhẹ. **Khuyến nghị: không dùng màu mặc định của Ant Design làm màu chính** — xem mục 4.

---

## 2. Token thiết kế tham chiếu

Đây là token công khai của Ant Design, không phải tài sản Dahua.

```css
:root {
  /* Nền — phân tầng rất nông, chỉ 3 mức */
  --bg-body:        #f0f2f5;   /* nền ngoài cùng */
  --bg-container:   #ffffff;   /* thẻ, bảng, panel */
  --bg-header:      #fafafa;   /* hàng tiêu đề bảng, hàng hover */

  /* Viền — hai mức, mảnh */
  --border-base:    #d9d9d9;
  --border-split:   #f0f0f0;   /* đường chia giữa các dòng */

  /* Chữ — không dùng màu xám đặc, dùng đen bán trong suốt */
  --text-heading:   rgba(0,0,0,0.85);
  --text-body:      rgba(0,0,0,0.65);
  --text-secondary: rgba(0,0,0,0.45);
  --text-disabled:  rgba(0,0,0,0.25);

  /* Chức năng */
  --color-success:  #52c41a;
  --color-warning:  #faad14;
  --color-error:    #ff4d4f;
  --color-info:     #1890ff;

  /* Hình học */
  --radius:         2px;       /* gần như vuông — đây là chi tiết quan trọng */
  --font-size-base: 14px;
  --font-size-sm:   12px;
  --control-height: 32px;
  --spacing-unit:   8px;        /* mọi khoảng cách là bội của 8 */
}
```

**Ba chi tiết quyết định "cảm giác IVSS", đừng bỏ qua:**

- **Bo góc 2px, gần như vuông.** Đây là thứ phân biệt phần mềm hạ tầng với SaaS tiêu dùng. Bo 8–12px lập tức thành "sản phẩm web hiện đại", mất chất công cụ vận hành.
- **Chữ dùng đen bán trong suốt, không dùng xám đặc.** `rgba(0,0,0,0.65)` trên nền trắng nhìn khác `#595959` — nó tự thích ứng khi nền đổi, và tạo cảm giác tầng lớp mềm hơn.
- **Phân tầng nền rất nông.** Chỉ ba mức, chênh nhau rất ít. Không đổ bóng nặng, không thẻ nổi. Giao diện phẳng và yên, để dữ liệu nổi lên chứ không phải khung.

---

## 3. Mẫu bố cục trích từ manual

Cấu trúc thông tin, không phải tài sản thị giác. Đây là phần đáng học thật.

### 3.1 Khung màn quản lý thiết bị

```
┌──────────────────────────────────────────────────────────────────┐
│ [Kênh đã dùng ▓▓▓▓▓░░ 100/128]  [Băng thông ▓▓▓▓▓▓░ 417/512Mbps] │ ← chỉ số tài nguyên, TRÊN CÙNG
├──────────────────────────────────────────────────────────────────┤
│ [Thêm] [Sửa IP] [Xuất] [Nhập hàng loạt] [Xoá]         ▽ Lọc      │ ← tác vụ hàng loạt, mờ khi chưa chọn
├─────────────┬────────────────────────────────────────────────────┤
│             │ ☐ │Kênh│Trạng thái│Ghi hình│Tên│Địa chỉ│…│Thao tác │
│  Cây        │ ☐ │ 1  │    ●     │   ●    │…                      │
│  thiết bị   │ ☐ │ 2  │    ●     │   ●    │…                      │
│             │                                                     │
│ [+]         │                                                     │ ← nút thêm thứ hai, góc dưới trái
└─────────────┴────────────────────────────────────────────────────┘
```

Bốn quyết định đáng giữ:

**a) Chỉ số tài nguyên đặt trên cùng, không giấu trong trang cấu hình.** Kênh đã dùng trên tổng, băng thông đã dùng trên tổng, dạng thanh tiến trình. Người vận hành luôn thấy mình còn bao nhiêu dư địa. Đây là quyết định tốt và hiếm — hầu hết sản phẩm chôn thông tin này ở đâu đó trong System Info.

**b) Hai trục trạng thái độc lập, không gộp.** Trạng thái kết nối và trạng thái ghi hình là hai cột riêng, hai chấm riêng. Camera trực tuyến nhưng không ghi là sự cố khác hẳn camera mất kết nối. Gộp làm một là mất thông tin chẩn đoán.

**c) Ba trạng thái kết nối, không phải hai.** Trực tuyến / mất kết nối / **kết nối thất bại**. Tách "không kết nối được" khỏi "đang offline" phản ánh khác biệt vận hành thật: một cái là thiết bị tắt, một cái là có gì đó chặn ở giữa.

**d) Hai lối vào cho cùng một hành động.** Nút "Thêm" ở thanh tác vụ cho người mới, nút "+" ở góc dưới trái cho người quen tay. Không phải thừa — phục vụ hai nhịp làm việc khác nhau.

### 3.2 Trang chủ kiểu tile

Không dùng sidebar. Sáu ô chức năng xếp lưới 3×2, mỗi ô gồm tiêu đề, một dòng mô tả, hình minh hoạ, và **tối đa ba shortcut đi thẳng vào màn con**. Bên dưới là một hàng cấu hình tách riêng (Camera · Mạng · Lưu trữ · Thuật toán · Sự kiện · Hệ thống).

Điểm hay: shortcut cấp hai ngay trên tile cho phép bỏ qua một cấp điều hướng. Người vận hành đi thẳng, người mới vẫn có tile để định hướng.

### 3.3 Màn xem trực tiếp

Đáng chú ý: **IVSS V7 dùng nền sáng cho cả màn xem trực tiếp**, không phải nền tối như đa số NVR. Chrome giao diện là xám nhạt, chỉ khung video là tối — và nó tối vì nội dung video tối, không phải vì thiết kế.

Bố cục ba cột: cây thiết bị và nhóm khung nhìn bên trái, lưới video ở giữa, dòng sự kiện AI bên phải (ảnh chụp kèm nhãn và thời gian). Thanh dưới cùng chia ba cụm chức năng rõ ràng: điều khiển bên trái, chọn kiểu lưới ở giữa, công cụ bên phải.

Với AICAM đây là lựa chọn đáng cân nhắc nghiêm túc: nền sáng cho toàn bộ chrome giúp giao diện đọc được trong phòng làm việc sáng đèn, còn tránh được luôn cái bẫy "nền xanh đậm đều + neon" mà luật chống slop của Huashu cấm.

### 3.4 Năm đường thêm thiết bị

Manual định nghĩa rõ điều kiện dùng từng đường. Nên bê nguyên vì nó phản ánh thực tế triển khai:

| Cách | Dùng khi |
|---|---|
| Quét nhanh | Không biết IP chính xác |
| Nhập tay | Vài thiết bị, đã biết IP và tài khoản |
| Tự đăng ký | Thiết bị nằm sau NAT, chủ động gọi về |
| RTSP | Thiết bị stream không chuẩn hãng |
| Nhập hàng loạt theo template | Nhiều thiết bị, mỗi cái một thông tin khác nhau |

Kèm: luồng khởi tạo thiết bị chưa đặt mật khẩu đi trước khi thêm; đổi IP hàng loạt theo bước tăng dần có tự bỏ qua IP trùng.

---

## 4. Phải đổi gì cho AICAM

**Không sao chép nguyên si.** Ba chỗ bắt buộc khác:

### 4.1 Màu chính — phải là màu VNPT AI, không phải `#1890ff`

Đây là thay đổi quan trọng nhất. Nếu giữ nguyên xanh mặc định Ant Design, sản phẩm sẽ nhìn giống mọi phần mềm Trung Quốc dựng trên antd — và giống Dahua đủ để gây khó xử.

Cách làm: lấy màu chính từ `brand-spec.md`, rồi sinh dải 10 sắc độ bằng thuật toán của Ant Design:

```bash
npm i @ant-design/colors
node -e "const {generate}=require('@ant-design/colors'); console.log(generate('#MÀU_VNPT'))"
```

Giữ nguyên bốn màu chức năng (`#52c41a`, `#faad14`, `#ff4d4f`) — đó là quy ước ngành, đổi chỉ gây nhầm lẫn. **Nếu màu chính VNPT trùng vùng màu của một màu chức năng thì phải đổi màu chính**, không đổi màu chức năng.

### 4.2 Cơ chế duyệt dữ liệu — IVSS không scale tới hàng nghìn

Mô hình IVSS thiết kế cho một đầu ghi ~128 kênh. Với quy mô cấp tỉnh/thành thì vỡ ở ba chỗ:

| Của IVSS | Thay bằng |
|---|---|
| Bảng phân trang thường | Cuộn ảo, lọc phía máy chủ, ô tìm kiếm luôn hiện |
| Cây thiết bị một cấp | Cây nhiều cấp (tỉnh → quận → phường → site), **mỗi nút hiện số liệu tổng hợp của nhánh** |
| Lối vào là danh sách phẳng | Lối vào là bảng tổng hợp trạng thái; danh sách phẳng là chế độ đào sâu |
| Quét trạng thái theo dòng | Tổng hợp trước, chi tiết sau — "240 thiết bị, 7 lỗi" rồi mới bung ra |

### 4.3 Bổ sung cho AICAM

Ba thứ IVSS không có mà AICAM cần:

- **Trạng thái "đang xuống cấp"** — kết nối được nhưng mất khung hình hoặc độ trễ cao. Rất thật với camera ngoài trời ở Việt Nam.
- **Trục thứ ba: trạng thái AI.** Camera có bật phân tích không, thuật toán nào, tài nguyên tính toán đã cấp bao nhiêu. Đây là giá trị khác biệt của AICAM so với đầu ghi thường, phải nhìn thấy ngay ở danh sách.
- **Dấu vết tuân thủ Nghị định 13.** Camera nào đang xử lý dữ liệu sinh trắc học, dữ liệu lưu ở đâu, thời hạn lưu bao lâu. IVSS không có vì không chịu ràng buộc này.

---

## 5. Vẫn không được làm

- ❌ Dùng logo, tên, hoặc màu nhận diện của Dahua ở bất kỳ đâu.
- ❌ Sao chép nguyên ảnh chụp giao diện từ manual vào bản thiết kế.
- ❌ Giữ nguyên `#1890ff` làm màu chính (xem 4.1).
- ❌ Để logic ① và ③ cũng bắt chước IVSS. File này **chỉ** áp cho logic ②. Hai logic còn lại phải khác về cấu trúc bố cục, nếu không thì ba phương án thành ba bản đổi màu và mất sạch ý nghĩa của cửa ba phương án.

---

## 6. Cách nạp vào Huashu

Thêm vào prompt ba phương án:

```
Logic ② hiện thực tham chiếu: đọc ./reference-ivss-design.md và làm theo.
Dùng token Ant Design 4 nêu trong đó, NHƯNG màu chính lấy từ brand-spec.md
(không dùng #1890ff). Giữ nguyên bốn màu chức năng.
Áp mục 4.2 — cơ chế duyệt dữ liệu phải chịu được vài nghìn thiết bị.

Logic ① và ③ KHÔNG đọc file này. Hai bản đó phải khác IVSS về cấu trúc
bố cục, không chỉ khác màu.
```

Chia file như vậy để cửa ba phương án còn nguyên ý nghĩa: một bản an toàn mà người vận hành cũ nhận ra ngay, hai bản để anh thấy còn đường nào khác. Nếu cả ba đều bắt chước IVSS thì không còn gì để chọn.

---

## Nguồn

- Ngôn ngữ thiết kế: trích từ `Dahua_IVSS_User_Manual_V7_1_0.pdf`, chương 5.4, 5.5.2, 7.1, 8.1. Màu lấy bằng cách render trang 191 và 227 ở 300 DPI rồi lượng tử hoá vùng ảnh giao diện.
- Token Ant Design: tài liệu chính thức `ant.design` và `4x.ant.design`, giấy phép MIT.
- Manual thuộc bản quyền Dahua. File này ghi lại **quan sát về cấu trúc và mô hình tương tác** để tham khảo thiết kế, không tái tạo nội dung manual.
