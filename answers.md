# PBT_03 - Answers

## Phần A - Đọc hiểu

### Câu A1 - 3 cách nhúng CSS

1. Inline CSS

```html
<p style="color: red; font-weight: bold;">Hello</p>
```

Ưu điểm: nhanh, dùng được ngay trên 1 element. Nhược điểm: khó bảo trì, lặp code, specificity cao, khó tái sử dụng. Nên dùng khi cần thử nhanh hoặc override rất cục bộ.

2. Internal CSS

```html
<style>
  p { color: blue; }
</style>
```

Ưu điểm: gom style cho 1 trang, dễ thử nghiệm. Nhược điểm: chỉ dùng trong 1 file HTML, không tái sử dụng tốt giữa nhiều trang. Nên dùng cho trang đơn hoặc demo nhỏ.

3. External CSS

```html
<link rel="stylesheet" href="style.css">
```

Ưu điểm: tách biệt nội dung và giao diện, tái sử dụng tốt, cache được. Nhược điểm: cần thêm file và request mạng. Nên dùng cho hầu hết dự án thực tế.

Nếu cùng 1 element có cả 3 cách cùng áp dụng thì inline thắng, vì CSS cascade ưu tiên style nội tuyến hơn internal/external khi specificity khác nhau.

### Câu A2 - CSS Selectors

1. `h1` → `ShopTLU`
2. `.price` → `25.990.000đ` và `45.990.000đ`
3. `#app header` → thẻ header chứa `ShopTLU`, `Home`, `Products`, `About`
4. `nav a:first-child` → `Home`
5. `.product.featured h2` → `MacBook Pro`
6. `article > p` → cả 4 thẻ p trực tiếp con của article: `25.990.000đ`, `Mô tả sản phẩm...`, `45.990.000đ`, `Mô tả sản phẩm...`
7. `a[href="/"]` → `Home`
8. `.top-bar.dark h1` → `ShopTLU`

### Câu A3 - Box Model

Trường hợp 1: `content-box`

```css
.box-1 {
    width: 400px;
    padding: 20px;
    border: 5px solid black;
    margin: 10px;
}
```

Chiều rộng hiển thị = `400 + 20*2 + 5*2 = 450px`

Không gian chiếm trên trang = `450 + 10*2 = 470px`

Trường hợp 2: `border-box`

```css
.box-2 {
    box-sizing: border-box;
    width: 400px;
    padding: 20px;
    border: 5px solid black;
    margin: 10px;
}
```

Chiều rộng hiển thị = `400px`

Kích thước content thực tế = `400 - 20*2 - 5*2 = 350px`

Không gian chiếm trên trang = `400 + 10*2 = 420px`

Margin collapse:

```css
.box-a { margin-bottom: 25px; }
.box-b { margin-top: 40px; }
```

Khoảng cách giữa box-a và box-b = `40px`

Không phải `65px` vì margin dọc của hai block element liền kề bị collapse và browser lấy giá trị lớn hơn.

Nâng cao: `margin-bottom: -10px` và `margin-top: 40px` thì khoảng cách = `30px`.

### Câu A4 - Specificity

1. Specificity score:

```css
p { color: black; }        /* (0,0,1) */
.price { color: blue; }    /* (0,1,0) */
#main-price { color: red; } /* (1,0,0) */
p.price { color: green; }  /* (0,1,1) */
```

2. Element sẽ có màu `red`, vì selector `#main-price` có specificity cao nhất.
3. Nếu thêm `style="color: orange;"` thì element có màu `orange`, vì inline style thắng các rule thông thường.
4. Nếu Rule A thêm `!important`, element có màu `black`, vì `!important` đẩy rule đó lên ưu tiên cao hơn các rule không quan trọng khác.

## Phần C - Debug & Suy luận

### Câu C1 - Debug CSS Layout

1. Kích thước thực tế khi dùng content-box:

```css
.sidebar { width: 300px; padding: 20px; border: 1px solid #ccc; }
.content { width: 660px; padding: 30px; border: 1px solid #ccc; }
```

Sidebar = `300 + 20*2 + 1*2 = 342px`

Content = `660 + 30*2 + 1*2 = 722px`

Tổng = `1064px`, lớn hơn container `960px`, nên content bị đẩy xuống dòng mới.

2. Tại sao layout bị vỡ: tổng chiều rộng thực tế của hai float vượt quá chiều rộng container.

3. Hai cách sửa:

```css
/* Cách 1: border-box */
* { box-sizing: border-box; }
```

```css
/* Cách 2: không dùng border-box, giảm width để bù padding + border */
.sidebar { width: 258px; }
.content { width: 598px; }
```

4. File chứng minh: `debug_layout.html` và `debug_layout.css`.

### Câu C2 - Cascade Puzzle

1. `Sản phẩm A`:

font-size = `20px`

color = `green`

Giải thích: `.card .title` đặt font-size 20px. `.highlight { color: green !important; }` thắng `#featured .title { color: red; }` vì `!important` cao hơn rule thường.

2. `Mô tả sản phẩm`:

color = `blue`

Giải thích: `.card p { color: inherit; }` nên p lấy color từ `.card`, mà `.card { color: blue; }`.

3. `Sản phẩm B`:

font-size = `20px`

color = `blue`

Giải thích: `.card .title` đặt font-size 20px. Màu kế thừa từ `.card { color: blue; }`.

4. `Mô tả sản phẩm B`:

color = `green`

Giải thích: class `.highlight` có `!important`, nên màu xanh lá thắng màu kế thừa.

## Bài B1 - Selector list dùng trong CSS

- `*`
- `body`
- `header`
- `nav a`
- `nav a:hover`
- `nav a.active`
- `table`
- `thead th`
- `tbody tr:nth-child(even)`
- `tbody tr:hover`
- `footer`

## Bài B2 - Box Model Lab

```text
Hộp 1 (content-box): chiều rộng thực tế = 350 px
Hộp 2 (border-box): chiều rộng thực tế = 300 px
Giải thích sự khác biệt: content-box cộng padding và border ra ngoài width, còn border-box gộp padding và border vào trong width đã khai báo.
```

Screenshot kiểm chứng: `screenshots/boxmodel_lab.png`

## Bài B3 - Specificity Battle

1. 10 rules + specificity score:

```css
* { color: #94a3b8; }                                 /* 0,0,0 */
p { color: #0f172a; }                                 /* 0,0,1 */
.text { color: #2563eb; }                             /* 0,1,0 */
.highlight { color: #f59e0b; }                        /* 0,1,0 */
p.text { color: #0f766e; }                            /* 0,1,1 */
p.highlight { color: #c026d3; }                       /* 0,1,1 */
.playground p { color: #dc2626; }                     /* 0,1,1 */
#demo { color: #7c3aed; }                             /* 1,0,0 */
.playground #demo { color: #14b8a6; }                 /* 1,1,0 */
body .frame .playground p#demo.text.highlight { color: #111827; } /* 1,4,2 */
```

2. Element cuối cùng hiển thị màu `#111827`.

3. Screenshot kiểm chứng: `screenshots/specificity.png`

4. Nếu thay đổi thứ tự các rule trong CSS file, kết quả chỉ đổi ở các rule có cùng specificity. Rule cuối cùng vẫn thắng vì có specificity cao nhất, nên màu vẫn không đổi.

Screenshot kiểm chứng: `screenshots/selectors_test.png`, `screenshots/debug_layout.png`, `screenshots/profile.png`
