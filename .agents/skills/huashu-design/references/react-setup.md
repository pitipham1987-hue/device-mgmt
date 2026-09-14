# Quy chuẩn Dự án React + Babel

Quy chuẩn kỹ thuật bắt buộc tuân thủ khi làm prototype bằng HTML+React+Babel. Không tuân thủ sẽ bị hỏng ứng dụng.

## Thẻ Pinned Script (Bắt buộc dùng các phiên bản này)

Trong `<head>` của HTML đặt 3 script tag này, sử dụng **phiên bản cố định + integrity hash**:

```html
<script src="https://unpkg.com/react@18.3.1/umd/react.development.js" integrity="sha384-hD6/rw4ppMLGNu3tX5cjIb+uRZ7UkRJ6BPkLpg4hAu/6onKUg4lLsHAs9EBPT82L" crossorigin="anonymous"></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.development.js" integrity="sha384-u6aeetuaXnQ38mYT8rp6sbXaQe3NL9t+IBXmnYxwkUI2Hw4bsp2Wvmx4yRQF1uAm" crossorigin="anonymous"></script>
<script src="https://unpkg.com/@babel/standalone@7.29.0/babel.min.js" integrity="sha384-m08KidiNqLdpJqLq95G/LEi8Qvjl/xUYll3QILypMoQ65QorJ9Lvtp2RXYGBFj1y" crossorigin="anonymous"></script>
```

**Không được** dùng các phiên bản unpinned như `react@18` hoặc `react@latest` — sẽ xảy ra vấn đề trôi phiên bản / cache.

**Không được** bỏ qua `integrity` — Một khi CDN bị chiếm quyền kiểm soát hoặc chỉnh sửa, đây chính là tuyến phòng thủ.

## Cấu trúc thư mục

```
TênDựÁn/
├── index.html               # HTML chính
├── components.jsx           # File component (Tải bằng type="text/babel")
├── data.js                  # File dữ liệu
└── styles.css               # CSS bổ sung (Tùy chọn)
```

Cách tải trong HTML:

```html
<!-- React + Babel trước -->
<script src="https://unpkg.com/react@18.3.1/..."></script>
<script src="https://unpkg.com/react-dom@18.3.1/..."></script>
<script src="https://unpkg.com/@babel/standalone@7.29.0/..."></script>

<!-- Sau đó là các file component của bạn -->
<script type="text/babel" src="components.jsx"></script>
<script type="text/babel" src="pages.jsx"></script>

<!-- Cuối cùng là entry chính -->
<script type="text/babel">
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<App />);
</script>
```

**Không được** dùng `type="module"` — sẽ bị xung đột với Babel.

## Ba quy tắc không được vi phạm

### Quy tắc 1: Đối tượng styles phải dùng đặt tên duy nhất (Unique Naming)

**Lỗi** (Khi có nhiều component chắc chắn bị hỏng):
```jsx
// components.jsx
const styles = { button: {...}, card: {...} };

// pages.jsx  ← Trùng tên đè lên!
const styles = { container: {...}, header: {...} };
```

**Đúng**: Dùng prefix duy nhất cho đối tượng styles của từng file component.

```jsx
// terminal.jsx
const terminalStyles = { 
  screen: {...}, 
  line: {...} 
};

// sidebar.jsx
const sidebarStyles = { 
  container: {...}, 
  item: {...} 
};
```

**Hoặc dùng inline styles** (Khuyến nghị cho component nhỏ):
```jsx
<div style={{ padding: 16, background: '#111' }}>...</div>
```

Quy tắc này là **không thương lượng**. Mỗi lần viết `const styles = {...}` đều phải replace thành đặt tên cụ thể (specific), nếu không khi tải nhiều component sẽ báo lỗi toàn bộ stack.

### Quy tắc 2: Scope không chia sẻ tự động, phải export thủ công

**Nhận thức quan trọng**: Mỗi `<script type="text/babel">` được Babel biên dịch độc lập, giữa chúng **scope không thông nhau**. Component `Terminal` định nghĩa trong `components.jsx` thì trong `pages.jsx` **mặc định là undefined**.

**Cách giải quyết**: Ở cuối mỗi file component, export các component/utility cần chia sẻ ra `window`:

```jsx
// Cuối file components.jsx
function Terminal(props) { ... }
function Line(props) { ... }
const colors = { green: '#...', red: '#...' };

Object.assign(window, {
  Terminal, Line, colors,
  // Tất cả những gì bạn muốn dùng ở nơi khác đều liệt kê ở đây
});
```

Sau đó `pages.jsx` có thể dùng trực tiếp `<Terminal />`, vì JSX sẽ tìm trong `window.Terminal`.

### Quy tắc 3: Không dùng scrollIntoView

`scrollIntoView` sẽ đẩy toàn bộ container HTML lên trên, làm hỏng bố cục của web harness. **Tuyệt đối không dùng**.

Phương án thay thế:
```js
// Cuộn đến vị trí nào đó trong container
container.scrollTop = targetElement.offsetTop;

// Hoặc dùng element.scrollTo
container.scrollTo({
  top: targetElement.offsetTop - 100,
  behavior: 'smooth'
});
```

## Gọi Claude API (Trong HTML)

Một số môi trường design-agent nguyên bản (như Claude.ai Artifacts) có sẵn `window.claude.complete` không cần cấu hình, nhưng đa số môi trường agent (Claude Code / Codex / Cursor / Trae / v.v.) cục bộ **không có**.

Nếu prototype HTML của bạn cần gọi LLM để làm demo (ví dụ làm giao diện chat interface), có 2 lựa chọn:

### Lựa chọn A: Không gọi thật, dùng mock

Khuyến nghị cho kịch bản Demo. Viết một helper giả, trả về response thiết lập sẵn:
```jsx
window.claude = {
  async complete(prompt) {
    await new Promise(r => setTimeout(r, 800)); // Giả lập độ trễ
    return "Đây là một response mock. Khi triển khai thật vui lòng thay bằng API thật.";
  }
};
```

### Lựa chọn B: Gọi API Anthropic thật (Không khuyến nghị, chỉ hạn chế cho demo cục bộ)

Yêu cầu có API key, người dùng bắt buộc phải nhập key của mình vào HTML mới chạy được. **Tuyệt đối không hardcode key trong HTML**.

⚠️ Ranh giới an toàn: Phương án này chỉ phù hợp khi mở bằng `file://` cục bộ, dùng xong đóng ngay để demo. Key sẽ nằm lại trong DOM/bộ nhớ —
**Không triển khai trang này lên web, không chụp màn hình/quay video truyền thông khi trang đã nhập key**. Kịch bản sản phẩm thực tế đều đi qua proxy backend cục bộ để forward,
phía browser không đụng tới key. Mặc định ưu tiên lựa chọn A/C (hoàn toàn không cần key).

```html
<input id="api-key" placeholder="Dán API key Anthropic của bạn" />
<script>
window.claude = {
  async complete(prompt) {
    const key = document.getElementById('api-key').value;
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'x-api-key': key,
        'anthropic-version': '2023-06-01',
        'content-type': 'application/json',
      },
      body: JSON.stringify({
        model: 'claude-haiku-4-5',
        max_tokens: 1024,
        messages: [{ role: 'user', content: prompt }]
      })
    });
    const data = await res.json();
    return data.content[0].text;
  }
};
</script>
```

**Lưu ý**: Trình duyệt gọi trực tiếp Anthropic API sẽ gặp vấn đề CORS. Nếu môi trường xem trước của người dùng không hỗ trợ CORS bypass, con đường này không chạy được. Lúc này hãy dùng lựa chọn A mock, hoặc báo cho người dùng biết cần một proxy backend.

### Lựa chọn C: Dùng năng lực LLM phía agent để tạo dữ liệu mock

Nếu chỉ phục vụ demo cục bộ, bạn có thể gọi tạm năng lực LLM của agent trong phiên làm việc hiện tại (hoặc các skill dạng multi-model người dùng cài) để tạo dữ liệu response mock trước, sau đó hardcode ghi vào HTML. Như vậy khi HTML runtime sẽ hoàn toàn không phụ thuộc vào bất kỳ API nào.

## Template HTML khởi đầu điển hình

Copy template này làm khung sườn cho prototype React:

```html
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your Prototype Name</title>

  <!-- React + Babel pinned -->
  <script src="https://unpkg.com/react@18.3.1/umd/react.development.js" integrity="sha384-hD6/rw4ppMLGNu3tX5cjIb+uRZ7UkRJ6BPkLpg4hAu/6onKUg4lLsHAs9EBPT82L" crossorigin="anonymous"></script>
  <script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.development.js" integrity="sha384-u6aeetuaXnQ38mYT8rp6sbXaQe3NL9t+IBXmnYxwkUI2Hw4bsp2Wvmx4yRQF1uAm" crossorigin="anonymous"></script>
  <script src="https://unpkg.com/@babel/standalone@7.29.0/babel.min.js" integrity="sha384-m08KidiNqLdpJqLq95G/LEi8Qvjl/xUYll3QILypMoQ65QorJ9Lvtp2RXYGBFj1y" crossorigin="anonymous"></script>

  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; width: 100%; }
    body { 
      font-family: -apple-system, 'SF Pro Text', sans-serif;
      background: #FAFAFA;
      color: #1A1A1A;
    }
    #root { min-height: 100vh; }
  </style>
</head>
<body>
  <div id="root"></div>

  <!-- File component của bạn -->
  <script type="text/babel" src="components.jsx"></script>

  <!-- Main entry -->
  <script type="text/babel">
    const { useState, useEffect } = React;

    function App() {
      return (
        <div style={{padding: 40}}>
          <h1>Hello</h1>
        </div>
      );
    }

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
  </script>
</body>
</html>
```

## Các báo lỗi thường gặp và cách xử lý

**`styles is not defined` hoặc `Cannot read property 'button' of undefined`**
→ Bạn đã định nghĩa `const styles` ở một file, và file khác đã ghi đè lên. Đổi đối tượng styles ở mỗi file thành đặt tên cụ thể (specific).

**`Terminal is not defined`**
→ Khi tham chiếu cross-file bị lỗi scope. Ở cuối file định nghĩa Terminal thêm `Object.assign(window, {Terminal})`.

**Toàn bộ trang bị trắng, console không có lỗi**
→ Đa phần là lỗi cú pháp JSX nhưng Babel không báo ra console. Tạm thời đổi `babel.min.js` thành bản không nén `babel.js`, thông tin lỗi sẽ rõ ràng hơn.

**ReactDOM.createRoot is not a function**
→ Phiên bản không đúng. Xác nhận đã dùng react-dom@18.3.1 (thay vì 17 hoặc bản khác).

**`Objects are not valid as a React child`**
→ Bạn đang render một object chứ không phải JSX/chuỗi. Thường là viết nhầm `{someObj}` thay vì `{someObj.name}`.

## Dự án lớn tách file như thế nào

Single file **>1000 dòng** rất khó bảo trì. Tư duy chia nhỏ:

```
DựÁn/
├── index.html
├── src/
│   ├── primitives.jsx      # Phân tử cơ sở: Button, Card, Badge...
│   ├── components.jsx      # Component nghiệp vụ: UserCard, PostList...
│   ├── pages/
│   │   ├── home.jsx        # Trang chủ
│   │   ├── detail.jsx      # Trang chi tiết
│   │   └── settings.jsx    # Trang cài đặt
│   ├── router.jsx          # Điều hướng đơn giản (Chuyển React state)
│   └── app.jsx             # Entry component
└── data.js                 # mock data
```

Tải theo thứ tự trong HTML:
```html
<script type="text/babel" src="src/primitives.jsx"></script>
<script type="text/babel" src="src/components.jsx"></script>
<script type="text/babel" src="src/pages/home.jsx"></script>
<script type="text/babel" src="src/pages/detail.jsx"></script>
<script type="text/babel" src="src/pages/settings.jsx"></script>
<script type="text/babel" src="src/router.jsx"></script>
<script type="text/babel" src="src/app.jsx"></script>
```

**Ở cuối mỗi file** đều phải dùng `Object.assign(window, {...})` để export các tài nguyên cần chia sẻ.
