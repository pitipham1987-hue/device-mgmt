# Tweaks: Điều chỉnh tham số trực tiếp các biến thể thiết kế

Tweaks là năng lực rất cốt lõi trong skill này — Giúp người dùng chuyển đổi các variations / điều chỉnh tham số trực tiếp mà không cần sửa code.

**Thích ứng môi trường đa agent**: Một số môi trường design-agent nguyên bản (như Claude.ai Artifacts) phụ thuộc vào postMessage của host để ghi ngược giá trị tweak vào code nguồn nhằm lưu trữ lâu dài. Skill này áp dụng **phương án localStorage thuần frontend** — Hiệu quả nhất quán (giữ nguyên trạng thái khi refresh), nhưng lưu trữ lâu dài diễn ra ở localStorage trình duyệt chứ không phải file code nguồn. Phương án này hoạt động tốt ở bất kỳ môi trường agent nào (Claude Code / Codex / Cursor / Trae / v.v.).

## Khi nào nên thêm Tweaks

- Người dùng yêu cầu rõ ràng "có thể chỉnh tham số" / "chuyển đổi nhiều phiên bản"
- Thiết kế có nhiều variations cần so sánh
- Người dùng không nói rõ, nhưng bạn chủ quan đánh giá **thêm vài tweaks có tính gợi mở sẽ giúp người dùng thấy được nhiều khả năng**

Khuyến nghị mặc định: **Mỗi thiết kế đều thêm 2-3 tweaks** (chủ đề màu sắc / font-size / biến thể layout) ngay cả khi người dùng không yêu cầu — Giúp người dùng nhìn thấy không gian khả năng là một phần của dịch vụ thiết kế.

## Cách triển khai (Bản thuần Frontend)

### Cấu trúc cơ bản

```jsx
const TWEAK_DEFAULTS = {
  "primaryColor": "#D97757",
  "fontSize": 16,
  "density": "comfortable",
  "dark": false
};

function useTweaks() {
  const [tweaks, setTweaks] = React.useState(() => {
    try {
      const stored = localStorage.getItem('design-tweaks');
      return stored ? { ...TWEAK_DEFAULTS, ...JSON.parse(stored) } : TWEAK_DEFAULTS;
    } catch {
      return TWEAK_DEFAULTS;
    }
  });

  const update = (patch) => {
    const next = { ...tweaks, ...patch };
    setTweaks(next);
    try {
      localStorage.setItem('design-tweaks', JSON.stringify(next));
    } catch {}
  };

  const reset = () => {
    setTweaks(TWEAK_DEFAULTS);
    try {
      localStorage.removeItem('design-tweaks');
    } catch {}
  };

  return { tweaks, update, reset };
}
```

### UI Panel Tweaks

Panel nổi ở góc dưới bên phải. Có thể thu gọn:

```jsx
function TweaksPanel() {
  const { tweaks, update, reset } = useTweaks();
  const [open, setOpen] = React.useState(false);

  return (
    <div style={{
      position: 'fixed',
      bottom: 20,
      right: 20,
      zIndex: 9999,
    }}>
      {open ? (
        <div style={{
          background: 'white',
          border: '1px solid #e5e5e5',
          borderRadius: 12,
          padding: 20,
          boxShadow: '0 10px 40px rgba(0,0,0,0.12)',
          width: 280,
          fontFamily: 'system-ui',
          fontSize: 13,
        }}>
          <div style={{ 
            display: 'flex', 
            justifyContent: 'space-between', 
            alignItems: 'center',
            marginBottom: 16,
          }}>
            <strong>Tweaks</strong>
            <button onClick={() => setOpen(false)} style={{
              border: 'none', background: 'none', cursor: 'pointer', fontSize: 16,
            }}>×</button>
          </div>

          {/* Màu sắc */}
          <label style={{ display: 'block', marginBottom: 12 }}>
            <div style={{ marginBottom: 4, color: '#666' }}>Màu chính</div>
            <input 
              type="color" 
              value={tweaks.primaryColor} 
              onChange={e => update({ primaryColor: e.target.value })}
              style={{ width: '100%', height: 32 }}
            />
          </label>

          {/* Slider Font-size */}
          <label style={{ display: 'block', marginBottom: 12 }}>
            <div style={{ marginBottom: 4, color: '#666' }}>Cỡ chữ ({tweaks.fontSize}px)</div>
            <input 
              type="range" 
              min={12} max={24} step={1}
              value={tweaks.fontSize}
              onChange={e => update({ fontSize: +e.target.value })}
              style={{ width: '100%' }}
            />
          </label>

          {/* Tuỳ chọn mật độ */}
          <label style={{ display: 'block', marginBottom: 12 }}>
            <div style={{ marginBottom: 4, color: '#666' }}>Mật độ</div>
            <select 
              value={tweaks.density}
              onChange={e => update({ density: e.target.value })}
              style={{ width: '100%', padding: 6 }}
            >
              <option value="compact">Gọn gàng</option>
              <option value="comfortable">Thoải mái</option>
              <option value="spacious">Thoáng đãng</option>
            </select>
          </label>

          {/* Toggle Dark mode */}
          <label style={{ 
            display: 'flex', 
            alignItems: 'center',
            gap: 8,
            marginBottom: 16,
          }}>
            <input 
              type="checkbox" 
              checked={tweaks.dark}
              onChange={e => update({ dark: e.target.checked })}
            />
            <span>Chế độ tối</span>
          </label>

          <button onClick={reset} style={{
            width: '100%',
            padding: '8px 12px',
            background: '#f5f5f5',
            border: 'none',
            borderRadius: 6,
            cursor: 'pointer',
            fontSize: 12,
          }}>Đặt lại</button>
        </div>
      ) : (
        <button 
          onClick={() => setOpen(true)}
          style={{
            background: '#1A1A1A',
            color: 'white',
            border: 'none',
            borderRadius: 999,
            padding: '10px 16px',
            fontSize: 12,
            cursor: 'pointer',
            boxShadow: '0 4px 12px rgba(0,0,0,0.15)',
          }}
        >⚙ Tweaks</button>
      )}
    </div>
  );
}
```

### Áp dụng Tweaks

Dùng Tweaks trong component chính:

```jsx
function App() {
  const { tweaks } = useTweaks();

  return (
    <div style={{
      '--primary': tweaks.primaryColor,
      '--font-size': `${tweaks.fontSize}px`,
      background: tweaks.dark ? '#0A0A0A' : '#FAFAFA',
      color: tweaks.dark ? '#FAFAFA' : '#1A1A1A',
    }}>
      {/* Nội dung của bạn */}
      <TweaksPanel />
    </div>
  );
}
```

Dùng biến trong CSS:

```css
button.cta {
  background: var(--primary);
  color: white;
  font-size: var(--font-size);
}
```

## Các tùy chọn Tweak điển hình

Thêm tweaks gì cho các loại thiết kế khác nhau:

### Dùng chung
- Màu chính (color picker)
- Cỡ chữ (slider 12-24px)
- Font chữ (select: display font vs body font)
- Chế độ tối (toggle)

### Slide deck
- Chủ đề (light/dark/brand)
- Kiểu nền (solid/gradient/image)
- Tương phản font chữ (trang trí hơn vs kiềm chế hơn)
- Mật độ thông tin (minimal/standard/dense)

### Prototype sản phẩm
- Biến thể layout (layout A / B / C)
- Tốc độ tương tác (tốc độ animation 0.5x-2x)
- Lượng dữ liệu (số dòng dữ liệu mock 5/20/100)
- Trạng thái (empty/loading/success/error)

### Animation
- Tốc độ (0.5x-2x)
- Vòng lặp (once/loop/ping-pong)
- Easing (linear/easeOut/spring)

### Landing page
- Phong cách Hero (image/gradient/pattern/solid)
- Nội dung CTA (một vài biến thể)
- Cấu trúc (single column / two column / sidebar)

## Nguyên tắc thiết kế Tweaks

### 1. Tùy chọn có ý nghĩa, không phải hành hạ người dùng

Mỗi tweak bắt buộc phải thể hiện **lựa chọn thiết kế thực sự**. Đừng thêm những tweak mà không ai thực sự chuyển đổi (ví dụ slider border-radius 0-50px — Người dùng chỉnh xong thấy tất cả các giá trị ở giữa đều xấu).

Tweak tốt sẽ bộc lộ **những variations rời rạc, đã qua suy nghĩ**:
- "Phong cách bo góc": Không bo / Bo nhẹ / Bo lớn (3 lựa chọn)
- Không phải: "Bo góc": 0-50px slider

### 2. Ít hơn là nhiều hơn (Less is More)

Panel Tweaks của một thiết kế **tối đa 5-6** tùy chọn. Nhiều hơn nữa sẽ biến thành "Trang cấu hình", làm mất đi ý nghĩa của việc nhanh chóng khám phá các variations.

### 3. Giá trị mặc định là một thiết kế hoàn chỉnh

Tweaks là **điểm cộng điểm tô**. Giá trị mặc định bắt buộc bản thân nó phải là một thiết kế hoàn chỉnh, sẵn sàng xuất bản. Người dùng đóng panel Tweaks lại thì những gì nhìn thấy chính là sản phẩm bàn giao.

### 4. Nhóm hợp lý

Khi có nhiều tùy chọn thì hiển thị theo nhóm:

```
---- Thị giác ----
Màu chính | Cỡ chữ | Chế độ tối

---- Bố cục ----
Mật độ | Vị trí thanh bên

---- Nội dung ----
Hiển thị số lượng dữ liệu | Trạng thái
```

## Tương thích ngược với host lưu trữ cấp mã nguồn (Source-level persistence host)

Nếu sau này bạn muốn đưa thiết kế lên môi trường hỗ trợ tweaks cấp mã nguồn (như Claude.ai Artifacts) cũng chạy được, hãy giữ lại **khối đánh dấu EDITMODE**:

```jsx
const TWEAK_DEFAULTS = /*EDITMODE-BEGIN*/{
  "primaryColor": "#D97757",
  "fontSize": 16,
  "density": "comfortable",
  "dark": false
}/*EDITMODE-END*/;
```

Khối đánh dấu này **không có tác dụng** trong phương án localStorage (chỉ là comment thường), nhưng trong host hỗ trợ ghi ngược mã nguồn sẽ được đọc để thực hiện lưu trữ lâu dài cấp mã nguồn. Thêm cái này không gây hại cho môi trường hiện tại, đồng thời giữ được tính tương thích ngược.

## Các câu hỏi thường gặp

**Panel Tweaks che mất nội dung thiết kế**
→ Cho phép đóng/mở. Mặc định đóng, hiển thị nút nhỏ, người dùng bấm mới mở ra.

**Người dùng chuyển đổi tweaks xong sau đó phải cài đặt lại**
→ Đã dùng localStorage. Nếu F5 xong không giữ được, kiểm tra localStorage có khả dụng không (chế độ ẩn danh sẽ bị lỗi, cần catch).

**Nhiều trang HTML muốn chia sẻ tweaks**
→ Thêm project name vào localStorage key: `design-tweaks-[projectName]`.

**Tôi muốn giữa các tweak có quan hệ liên động với nhau**
→ Thêm logic vào trong `update`:

```jsx
const update = (patch) => {
  let next = { ...tweaks, ...patch };
  // Liên động: Khi chọn dark mode tự động chuyển màu font chữ
  if (patch.dark === true && !patch.textColor) {
    next.textColor = '#F0EEE6';
  }
  setTweaks(next);
  localStorage.setItem(...);
};
```
