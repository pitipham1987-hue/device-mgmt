# Thư viện Template Phân cảnh: Tổ chức theo loại đầu ra

> Sử dụng phối hợp với "DNA Prompt" của design-styles.md.
> Công thức: `[DNA Prompt phong cách] + [Template phân cảnh] + [Mô tả nội dung cụ thể]`

---

## 1. Bìa Bài viết WeChat Official Account / Hình Tiêu đề Bài viết

**Quy cách**:
- Hình bìa: 2.35:1 (900×383px hoặc 1200×510px)
- Hình minh họa nội dung: 16:9 (1200×675px) hoặc 4:3 (1200×900px)

**Yếu tố thiết kế mấu chốt**:
- Lực va chạm thị giác được ưu tiên (Người dùng lướt qua nhanh trong luồng thông tin)
- Chữ cực ít hoặc không có chữ (Tiêu đề bài viết sẽ đè lên trên)
- Độ bão hòa màu sắc vừa phải (Môi trường đọc WeChat thiên về trắng)
- Tránh chi tiết quá đà (Ảnh thu nhỏ thumbnail cũng phải nhận biết được)

**Phong cách khuyến nghị**: 01 Pentagram / 11 Build / 12 Sagmeister / 18 Kenya Hara / 07 Field.io

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Article cover image for WeChat subscription
- Landscape format, 2.35:1 aspect ratio
- Bold visual impact, minimal or no text
- Moderate color saturation for white reading environment
- Must remain recognizable as thumbnail
- Clean composition with clear focal point
```

---

## 2. Hình Minh họa Nội dung / Minh họa Khái niệm

**Quy cách**:
- 16:9 (1200×675px) thông dụng nhất
- 1:1 (800×800px) thích hợp để nhấn mạnh
- 4:3 (1200×900px) thích hợp cho thông tin dày đặc

**Yếu tố thiết kế mấu chốt**:
- Phục vụ cho luận điểm bài viết, không phải trang trí
- Tạo nhịp điệu thị giác với ngữ cảnh
- Bộc lộ rõ ràng một khái niệm cốt lõi
- Ưu tiên AI tạo hình, HTML screenshot chỉ dùng khi có bảng dữ liệu chuẩn xác

**Phong cách khuyến nghị**: Chọn theo điệu tính bài viết, thường dùng 01/04/10/17/18

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Article illustration, concept visualization
- [16:9 / 1:1 / 4:3] aspect ratio
- Single clear concept: [Mô tả khái niệm cốt lõi]
- Serve the argument, not decoration
- [Light/Dark] background to match article tone
```

---

## 3. Infographic / Trực quan hóa Dữ liệu

**Quy cách**:
- Bức hình dài bản dọc: 1080×1920px (Đọc trên điện thoại)
- Bản ngang: 1920×1080px (Nhúng trong bài viết)
- Vuông: 1080×1080px (Mạng xã hội)

**Yếu tố thiết kế mấu chốt**:
- Cấp độ thông tin rõ ràng (Tiêu đề → Dữ liệu cốt lõi → Chi tiết)
- Dữ liệu chuẩn xác, không bịa đặt
- Đường dẫn dắt thị giác (Đường dẫn đọc của người dùng)
- Sử dụng thích hợp icon/biểu đồ để hỗ trợ thấu hiểu

**Phong cách khuyến nghị**: 04 Fathom / 10 Müller-Brockmann / 02 Stamen / 17 Takram

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Infographic / data visualization
- [Vertical 1080x1920 / Horizontal 1920x1080 / Square 1080x1080]
- Clear information hierarchy: title → key data → details
- Visual flow guiding reader's eye path
- Icons and charts for comprehension
- Data-accurate, no decorative distortion
```

---

## 4. PPT / Thuyết trình Keynote

**Quy cách**:
- Tiêu chuẩn: 16:9 (1920×1080px)
- Màn hình rộng: 16:10 (1920×1200px)

**Yếu tố thiết kế mấu chốt**:
- Mỗi trang một thông tin cốt lõi (Không đè nén)
- Cấp độ cỡ chữ rõ ràng (Tiêu đề 40pt+ / Chữ chính 24pt+ / Chú thích 16pt+)
- Lượng lớn khoảng trắng, rõ ràng hơn khi chiếu
- Tỷ lệ hình chữ ít nhất 60:40
- Hệ thống thị giác nhất quán (Màu sắc, Font chữ, Khoảng cách)

**Phong cách khuyến nghị**: 01 Pentagram / 10 Müller-Brockmann / 11 Build / 18 Kenya Hara / 04 Fathom

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Presentation slide design, 16:9
- One core message per slide
- Clear type hierarchy (title 40pt+, body 24pt+)
- Generous whitespace for projection clarity
- Consistent visual system throughout
- [Light/Dark] theme
```

---

## 5. Bạch皮书 PDF / Báo cáo Kỹ thuật

**Quy cách**:
- A4 Bản dọc (210×297mm / 595×842pt)
- Letter Bản dọc (216×279mm / 612×792pt)

**Yếu tố thiết kế mấu chốt**:
- Tối ưu hóa đọc văn bản dài (Chiều rộng dòng 66 ký tự, Chiều cao dòng 1.5-1.8)
- Hệ thống điều hướng chương rõ ràng
- Thiết kế thống nhất của Header/Footer/Số trang
- Sự cộng tồn tinh tế giữa biểu đồ và chữ chính
- Hệ thống trích dẫn/Chú thích
- Thiết kế trang bìa tinh tế

**Phong cách khuyến nghị**: 10 Müller-Brockmann / 04 Fathom / 03 Information Architects / 17 Takram / 19 Irma Boom

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- PDF document / white paper design
- A4 portrait format (210×297mm)
- Long-form reading optimized (66 char line width, 1.5 line height)
- Clear chapter navigation system
- Elegant header/footer/page number design
- Charts integrated with body text
- Professional cover page
```

---

## 6. Landing Page / Trang web Chính thức Sản phẩm

**Quy cách**:
- Desktop: Thiết kế chiều rộng 1440px (Responsive xuống 320px)
- Chiều cao màn hình đầu tiên: 100vh

**Yếu tố thiết kế mấu chốt**:
- Trong 5 giây đầu màn hình thứ nhất truyền tải giá trị cốt lõi
- CTA rõ ràng (Nút hành động)
- Cấu trúc tự sự cuộn trang (Vấn đề→Giải pháp→Chứng minh→Hành động)
- Thích ứng mobile
- Tốc độ tải

**Phong cách khuyến nghị**: 05 Locomotive / 01 Pentagram / 11 Build / 08 Resn / 06 Active Theory

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Landing page / product website
- Desktop 1440px width, responsive
- Hero section 100vh, core value in 5 seconds
- Clear CTA button design
- Scroll narrative: problem → solution → proof → action
- Modern web aesthetic
```

---

## 7. App UI / Giao diện Prototype

**Quy cách**:
- iOS: 390×844pt (iPhone 15)
- Android: 360×800dp
- Máy tính bảng: 1024×1366pt (iPad Pro)

**Yếu tố thiết kế mấu chốt**:
- Thân thiện với cảm ứng (Khu vực click tối thiểu 44×44pt)
- Tính nhất quán của ngôn ngữ thiết kế hệ thống
- Xử lý tiêu chuẩn cho Thanh trạng thái / Thanh điều hướng / Thanh Tab
- Mật độ thông tin vừa phải (Trên mobile không nên quá dày)

**Phong cách khuyến nghị**: 17 Takram / 11 Build / 03 Information Architects / 01 Pentagram

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Mobile app UI design
- iOS [390×844pt] / Android [360×800dp]
- Touch-friendly (44pt minimum tap targets)
- Consistent design system
- Standard status bar / navigation / tab bar
- Moderate information density
```

---

## 8. Hình minh họa Tiểu Hồng Thư (Xiaohongshu / RED)

**Quy cách**:
- Bản dọc: 3:4 (1080×1440px) Tốt nhất
- Vuông: 1:1 (1080×1080px)
- Hình đầu tiên quyết định tỷ lệ nhấp

**Yếu tố thiết kế mấu chốt**:
- Sức hút thị giác là số 1 (Cạnh tranh trong luồng thác nước)
- Có thể có lượng chữ nhỏ (Nhưng không quá 20% màn hình)
- Màu sắc tươi sáng nhưng không phàm tục
- Cảm giác cuộc sống / Chất lượng / Không khí

**Phong cách khuyến nghị**: 12 Sagmeister / 11 Build / 20 Neo Shen / 09 Experimental Jetset

**Template Prompt phân cảnh**:
```
[DNA Phong cách chèn vào đây]
- Social media image for Xiaohongshu (RED)
- Vertical 3:4 (1080×1440px)
- Eye-catching in waterfall feed
- Minimal text overlay (under 20% of area)
- Vivid but tasteful colors
- Lifestyle/texture/atmosphere feel
```

---

## Ví dụ Kết hợp

**Phân cảnh**: Bìa bài viết WeChat, giới thiệu một công cụ lập trình AI, muốn chuyên nghiệp nhưng có nhiệt độ

**Step 1**: Chọn phong cách → 17 Takram (Chuyên nghiệp + Nhiệt độ)
**Step 2**: Lấy DNA Prompt Takram + Template bìa bài viết WeChat

```
Takram Japanese speculative design:
- Elegant concept prototypes and diagrams
- Soft tech aesthetic (rounded corners, gentle shadows)
- Charts and diagrams as art pieces
- Modest sophistication
- Neutral natural colors (beige, soft gray, muted green)
- Design as philosophical inquiry

Article cover image for WeChat subscription
- Landscape format, 2.35:1 aspect ratio (1200×510px)
- Bold visual impact, minimal text
- Moderate color saturation for white reading environment
- Must remain recognizable as thumbnail
- Clean composition with clear focal point

Content: An AI coding assistant tool, showing the concept of human-AI collaboration
in software development, warm and professional atmosphere
```

---

**Phiên bản**: v1.0
**Ngày cập nhật**: 13-02-2026
