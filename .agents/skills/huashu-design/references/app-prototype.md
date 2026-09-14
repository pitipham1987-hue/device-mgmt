# Quy tắc Dành riêng cho Prototype App / iOS · Sách hướng dẫn thao tác hoàn chỉnh

> Bản hoàn chỉnh hạ chìm từ SKILL.md. SKILL.md giữ lại 7 quy tắc cứng để tra nhanh, tài liệu này triển khai chi tiết từng quy tắc: Lựa chọn kiến trúc, Kênh lấy hình và code, Khung xương AppPhone JSX, Cách dùng 3 bước của ios_frame, Bảng mỏ neo gu thẩm mỹ đầy đủ.

Khi làm prototype app iOS/Android/Mobile (Kích hoạt: "App prototype", "iOS mockup", "Ứng dụng di động", "Làm một app"), bốn điều bên dưới **ghi đè** lên nguyên tắc placeholder thông thường——App prototype là hiện trường demo, việc xếp cảnh tĩnh và card vị trí kem trắng không có sức thuyết phục.

### 0. Lựa chọn kiến trúc (Bắt buộc phải quyết định trước)

**Mặc định React inline đơn file**——Tất cả JSX/data/styles viết trực tiếp vào thẻ `<script type="text/babel">...</script>` của HTML chính, **ĐỪNG** dùng nạp bên ngoài `<script src="components.jsx">`. Lý do: Dưới giao thức `file://` trình duyệt coi JS bên ngoài là跨 origin và chặn lại, cưỡng chế người dùng bật HTTP server vi phạm bản năng prototype "double-click là mở được". Trích dẫn hình ảnh cục bộ bắt buộc nhúng dạng base64 data URL, đừng giả định có server.

**Tách file bên ngoài chỉ trong hai trường hợp**:
- (a) Đơn file >1000 dòng khó bảo trì → Tách thành `components.jsx` + `data.js`, đồng thời ghi rõ thuyết minh giao hàng (Lệnh `python3 -m http.server` + URL truy cập)
- (b) Cần nhiều subagent song song viết các màn hình khác nhau → `index.html` + HTML độc lập từng màn hình (`today.html`/`graph.html`...), iframe gom lại, mỗi màn hình cũng đều là đơn file tự chứa

**Tra nhanh lựa chọn**:

| Kịch bản | Kiến trúc | Phương thức giao hàng |
|------|------|----------|
| Đơn nhân làm prototype 4-6 màn hình (Chủ đạo) | Đơn file inline | Một file `.html` double-click mở |
| Đơn nhân làm App lớn (>10 màn hình) | Nhiều jsx + server | Kèm lệnh khởi động |
| Nhiều agent song song | Nhiều HTML + iframe | `index.html` gom lại, mỗi màn hình độc lập mở được |

### 1. Tìm hình thực tế trước, không đặt placeholder cho có

Mặc định chủ động đi lấy hình ảnh thực tế để lấp đầy, đừng vẽ SVG, đừng dùng card màu kem xếp vào, đừng đợi người dùng yêu cầu. Các kênh thường dùng:

| Kịch bản | Kênh ưu tiên |
|------|---------|
| Mỹ thuật/Bảo tàng/Nội dung lịch sử | Wikimedia Commons (Tền miền công cộng), Met Museum Open Access, Art Institute of Chicago API |
| Đời sống/Nhiếp ảnh chung | Unsplash, Pexels (Miễn bản quyền) |
| Vật liệu người dùng đã có cục bộ | `~/Downloads`, dự án `_archive/` hoặc thư viện vật liệu cấu hình của người dùng |

Tránh bẫy khi tải Wikimedia (curl máy cục bộ đi qua proxy TLS sẽ nổ, Python urllib đi thông trực tiếp):

```python
# User-Agent hợp quy là yêu cầu cứng, nếu không sẽ bị 429
UA = 'ProjectName/0.1 (https://github.com/you; you@example.com)'
# Dùng MediaWiki API tra URL thực tế
api = 'https://commons.wikimedia.org/w/api.php'
# action=query&list=categorymembers lấy theo series / prop=imageinfo+iiurlwidth lấy thumburl chiều rộng chỉ định
```

**CHỈ KHI** tất cả các kênh đều thất bại / Bản quyền không rõ ràng / Người dùng yêu cầu rõ ràng, mới rớt về placeholder trung thực (Vẫn không vẽ SVG tồi).

**Thử nghiệm tính trung thực của hình thực tế** (Mấu chốt): Trước khi lấy hình tự hỏi bản thân——"Nếu bỏ hình này đi, thông tin có bị tổn hại không?"

| Phân cảnh | Đánh giá | Hành động |
|------|------|------|
| Ảnh bìa danh sách bài viết/Essay, Hình đầu trang phong cảnh của trang Profile, Banner trang trí trang Cài đặt | Trang trí, không có liên kết nội tại với nội dung | **ĐỪNG THÊM**. Thêm vào chính là AI slop, tương đương chuyển sắc tím |
| Chân dung nội dung bảo tàng/nhân vật, Vật thể thực tế của chi tiết sản phẩm, Địa điểm của card bản đồ | Bản thân nội dung, có liên kết nội tại | **BẮT BUỘC THÊM** |
| Texture cực nhạt làm nền đồ phổ/trực quan hóa | Không khí, phục vụ nội dung không tranh diễn | Thêm, nhưng opacity ≤ 0.08 |

**Ví dụ phản diện**: Phối "Hình cảm hứng" Unsplash cho Essay chữ, phối stock photo người mẫu cho App ghi chú——đều là AI slop. Giấy phép lấy hình thực tế không đồng nghĩa với thẻ thông hành lạm dụng hình thực tế.

### 2. Hình thái giao hàng: Mặc định "Trải phẳng + Có thể thao tác", đừng hỏi người dùng

Hình thái giao hàng mặc định của prototype iOS App **chỉ có một loại, đừng hỏi người dùng "Muốn trải phẳng hay có thể thao tác" nữa**: **Trải phẳng 4-6 màn hình giao diện chính, và mỗi chiếc đều có thể tương tác**. Một ánh nhìn thấy toàn cảnh (Nhiều chiếc iPhone xếp song song), lại vừa mỗi chiếc đều có thể bấm chuyển tab, làm các thao tác cơ bản trên giao diện (Mở rộng, Chuyển đổi, Chọn, Mở lớp popup). Hai lợi ích trao cùng lúc, đừng để người dùng chọn 1 trong 2.

| Chiều | Cách làm mặc định |
|------|---------|
| **Số màn hình** | Trải phẳng **4-6 màn hình giao diện chính** (Bao phủ các mặt tính năng cốt lõi của app, không phải tiện tay xếp vài cái). Nhiều hơn 6 màn hình thì bắt 4-6 cái chính nhất, phần còn lại có thể truy cập thông qua tab/điều hướng trong đơn chiếc |
| **Bố cục** | Nhiều chiếc iPhone độc lập xếp ngang `flexWrap` song song, phía trên mỗi chiếc một dòng chữ nghiêng nhỏ làm nhãn thuyết minh đây là giao diện nào |
| **Tương tác mỗi chiếc** | Mỗi chiếc đều là máy trạng thái mini độc lập: Thanh tab có thể chuyển, Nút/Card/Công tắc trong giao diện có thể bấm, Có thể bật modal——không phải xếp cảnh tĩnh |

**Chỉ có hai ngoại lệ mới lệch khỏi mặc định** (Người dùng nói rõ ràng mới đi theo, nếu không nhất loạt mặc định):
- Người dùng nói rõ "Chỉ cần ảnh chụp màn hình tĩnh / Không cần bấm được / Chỉ xem layout" → Rớt về overview tĩnh thuần túy (Mỗi chiếc chỉ render `ScreenComponent`, không gắn máy trạng thái)
- Người dùng nói rõ "Chỉ demo một quy trình / Đi qua một lượt onboarding / Demo đơn chiếc" → Đơn chiếc `AppPhone` đi hết flow hoàn chỉnh

**Khung xương mặc định** (Trải phẳng nhiều chiếc, mỗi chiếc tự mình một AppPhone mang state):

```jsx
// Mỗi chiếc = Một máy trạng thái độc lập, ban đầu rơi vào giao diện chính phụ trách của mình
function AppPhone({ initial }) {
  const [screen, setScreen] = React.useState(initial);
  const [modal, setModal] = React.useState(null);
  // Render ScreenComponent tương ứng theo screen, truyền vào các callback onTabChange/onOpen/onClose/onToggle
  return (
    <IosFrame>
      <ScreenComponent
        screen={screen}
        onTabChange={setScreen}
        onOpen={setModal}
        onClose={() => setModal(null)}
      />
    </IosFrame>
  );
}

// Trải phẳng: 4-6 chiếc xếp song song, mỗi chiếc initial rơi vào giao diện chính khác nhau
<div style={{display: 'flex', gap: 32, flexWrap: 'wrap', padding: 48, alignItems: 'flex-start'}}>
  {mainScreens.map(s => (
    <div key={s.id}>
      <div style={{fontSize: 13, color: '#666', marginBottom: 8, fontStyle: 'italic'}}>{s.label}</div>
      <AppPhone initial={s.id} />
    </div>
  ))}
</div>
```

Component Screen nhận callback props (`onTabChange`, `onOpen`, `onClose`, `onToggle`, `onAnnotation`), không mã hóa cứng trạng thái. TabBar, Nút, Card tác phẩm, Công tắc thêm `cursor: pointer` + phản hồi hover. Mỗi chiếc rơi vào giao diện chính khác nhau, nhưng sau khi chuyển tab có thể tới được nhau——Trải phẳng cho toàn cảnh, click cho chiều sâu.

### 3. Chạy thử nghiệm click thực tế trước khi giao hàng

Ảnh chụp màn hình tĩnh chỉ có thể xem layout, bug tương tác phải bấm qua mới phát hiện. Dùng Playwright chạy 3 mục thử nghiệm click tối thiểu: Tiến vào chi tiết / Điểm chú thích mấu chốt / Chuyển tab. Kiểm tra `pageerror` bằng 0 rồi mới giao hàng. Playwright có thể dùng `npx playwright` gọi, hoặc theo đường dẫn cài đặt toàn cục máy cục bộ (`npm root -g` + `/playwright`).

### 4. Mỏ neo gu thẩm mỹ (pursue list, lựa chọn hàng đầu fallback)

Khi không có design system mặc định đi theo các hướng này, tránh va phải AI slop:

| Chiều | Ưu tiên | Tránh |
|------|------|------|
| **Font chữ** | Display nét chân (Newsreader/Source Serif/EB Garamond) + `-apple-system` body | Toàn trường SF Pro hoặc Inter——Quá giống mặc định hệ thống, không có phong cách |
| **Màu sắc** | Một màu nền có nhiệt độ + **Một** accent duy nhất xuyên suốt toàn trường (cam rust/xanh mực/đỏ sâu) | Gom cụm nhiều màu (Trừ khi dữ liệu thực sự có ≥3 chiều phân loại) |
| **Mật độ thông tin·Loại kiềm chế** (Mặc định) | Bớt một lớp container, bớt một border, bớt một icon **trang trí**——Để lại khoảng thở cho nội dung | Mỗi card đều phối icon vô nghĩa + tag + status dot |
| **Mật độ thông tin·Loại mật độ cao** (Ngoại lệ) | Khi điểm bán cốt lõi của sản phẩm là "Thông minh / Dữ liệu / Nhận biết ngữ cảnh" (Công cụ AI, Dashboard, Tracker, Copilot, Đồng hồ Pomodoro, Theo dõi sức khỏe, Ghi chép thu chi), mỗi màn hình cần **ít nhất 3 chỗ thông tin khác biệt hóa sản phẩm có thể nhìn thấy**: Dữ liệu không mang tính trang trí, Phân đoạn đối thoại/suy luận, Suy luận trạng thái, Liên kết ngữ cảnh | Chỉ đặt một nút một đồng hồ——Cảm giác thông minh của AI không thể hiện ra, không khác gì App thông thường |
| **Chi tiết chữ ký** | Để lại một chỗ chất lượng "Đáng chụp màn hình": Vân nền tranh dầu cực nhạt / Lời dẫn chữ nghiêng serif / Dải sóng ghi âm nền đen toàn màn hình | Nơi nào cũng dốc lực trung bình, kết quả nơi nào cũng phẳng lặng |

**Hai nguyên tắc cùng có hiệu lực**:
1. Gu thẩm mỹ = Một chi tiết làm 120%, các chi tiết khác làm 80%——Không phải tất cả mọi nơi đều tinh tế, mà là tinh tế đủ ở nơi thích hợp
2. Phép trừ là fallback, không phải luật phổ quát——Khi điểm bán cốt lõi sản phẩm cần mật độ thông tin chống đỡ (Loại AI / Dữ liệu / Nhận biết ngữ cảnh), phép cộng được ưu tiên hơn kiềm chế. Xem chi tiết "Phân loại mật độ thông tin" bên dưới

### 5. Khung thiết bị iOS bắt buộc dùng `assets/ios_frame.jsx`——Cấm tự viết Dynamic Island / status bar

Khi làm iPhone mockup **ràng buộc cứng** `assets/ios_frame.jsx`. Đây là vỏ tiêu chuẩn đã đối chiếu với quy cách chuẩn xác của iPhone 15 Pro: bezel, Dynamic Island (124×36, top:12, căn giữa), status bar (Thời gian/Tín hiệu/Pin, hai bên nhường đường cho đảo, vertical center căn chỉnh đường trung tâm đảo), Home Indicator, padding top khu vực content đều đã được xử lý xong.

**Cấm tự viết trong HTML của bạn** bất kỳ mục nào dưới đây:
- `.dynamic-island` / `.island` / `position: absolute; top: 11/12px; width: ~120; hình chữ nhật bo góc đen căn giữa`
- `.status-bar` with Icon thời gian/tín hiệu/pin tự viết
- `.home-indicator` / Thanh home bar phía dưới
- Khung ngoài bo góc của iPhone bezel + Viền đen + shadow

Tự mình viết 99% sẽ dẫm bug vị trí——Thời gian/Pin của status bar bị đảo ép chèn, hoặc padding top content tính sai dẫn đến dòng nội dung đầu tiên bị che dưới đảo. Tai thỏ của iPhone 15 Pro là **cố định 124×36 pixel**, chiều rộng khả dụng chừa cho hai bên status bar rất hẹp, không phải bạn tự nhiên ước lượng được.

**Cách dùng (Nghiêm ngặt 3 bước)**:

```jsx
// Bước 1: Read assets/ios_frame.jsx của skill này (Đường dẫn tương đối so với SKILL.md này)
// Bước 2: Dán toàn bộ hằng số iosFrameStyles + component IosFrame vào trong <script type="text/babel"> của bạn
// Bước 3: Component màn hình của chính bạn bọc trong <IosFrame>...</IosFrame>, không đụng tới island/status bar/home indicator
<IosFrame time="9:41" battery={85}>
  <YourScreen />  {/* Nội dung render từ top 54, phía dưới chừa cho home indicator, bạn không cần quản lý */}
</IosFrame>
```

**Ngoại lệ**: Chỉ khi người dùng yêu cầu rõ ràng "Giả vờ là tai thỏ không phải Pro của iPhone 14", "Làm Android không phải iOS", "Hình thái thiết bị tùy chỉnh" mới né qua——lúc này đọc `android_frame.jsx` tương ứng hoặc sửa hằng số của `ios_frame.jsx`, **ĐỪNG** tự tạo một bộ island/status bar khác trong HTML dự án.
