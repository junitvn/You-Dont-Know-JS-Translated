# You Don't Know JS - Tập 1

## Phần 2: Về JavaScript

_Note: Ở phần này mình sẽ không dịch hết vì nhiều thứ khá đơn giản, mình chỉ note lại vài thứ hay ho thôi :D_

### So sánh giá trị

#### Ép kiểu (Coercion)

Có 2 loại ép kiểu trong JS:

- Rõ ràng (Explicit):

```js
var a = "42";

var b = Number(a);

a; //"42" -- string
b; // 42 -- Number
```

- Không rõ ràng (Implicit)

```js
var a = "42";

var b = a * 1; // a được ép kiểu bằng cách nhân với 1 số kiểu Number

a; //"42" -- string
b; // 42 -- Number
```

#### Truthy & Falsy

Khi một biến không phải là `boolean` được ép kiểu về `boolean` thì nó sẽ là `true` hay `false`?

Danh sách các giá trị `false` trong JS:

- `""`(empty string)
- `0`, `-0`, `NaN` (kiểu `number` không hợp lệ - invalid `number`)
- `null`, `undefined`
- `false`

Những giá trị còn lại, không phải là `false` thì sẽ là `true`:

- `"Hello"`
- `42`
- `true`
- `[]`, `[1, "2", 3]` (arrays)
- `{}`, `{a: 42}` (objects)
- `function foo(){...}` (functions)

#### Bất đẳng thức (inequality)

- Không có `strict inequality` giống `strict equality` (`===`)

```js
var a = 42;
var b = "foo";

a < b; // false
a > b; // false
a == b; // false
```

Tại sao cả 3 biểu thức trên lại đều là `false`? Vì `a` là `Number`, khi `b` được mang ra so sánh với `a` thì `b` sẽ được ép kiểu ngầm định sang `Number`:

```js
var b = "foo";
var c = Number(b); // NaN
```

Khi đó `b = NaN` và giá trị `NaN` _không lớn hơn hay nhỏ hơn_ bất kì giá trị nào và hiển nhiên nó cũng không thể bằng `42` được.

### Biểu thức hàm được gọi ngay lập tức (IIFEs) - Immediately Invoked Function Expressions (IIFEs)

```js
function foo() { .. }

// `foo` function reference expression,
// then `()` executes it
foo();

// `IIFE` function expression,
// then `()` executes it
(function IIFE(){ .. })()
```

- Hàm được gọi ngay sau khi khởi tạo, với cú pháp `(function IIFE(){ .. })` để khai báo hàm và được thực thi bởi `()` ngay sau đó.
- Vì `IIEF` là hàm bình thường nên biến được khai báo bên trong hàm sẽ không bảnh hưởng đến bên ngoài

```js
var a = 42;

(function IIFE() {
  var a = 10;
  console.log(a); // 10
})();

console.log(a); // 42
```

- `IIEFs` có thể trả về giá trị

```js
var x = (function IIFE() {
  return 42;
})();

x; // 42
```

### Closure

_Closure_ là một trong những khái niệm quan trọng nhất, nhưng thường ít được hiểu nhất trong JS. Nó sẽ được nó kỹ hơn trong tập sau `Scope & Closures`.
Ở đây, mình sẽ giới thiệu qua để bạn hiểu được những khái niệm chung nhất. Đây sẽ là 1 trong những kỹ thuật quan trọng nhất trong bộ kỹ năng JS của bạn.

Bạn có thể nghĩ _closure_ như là một cách để "nhớ" và tiếp tục truy cập vào scope của 1 function (các biến của nó) ngay cả khi function đó đã chạy xong.

Xem xét ví dụ sau:

```js
function makeAdder(x) {
  // `x` là 1 biến trong hàm
  // inner function `add()` sử dụng `x`, nên nó có một "closure"
  function add(y) {
    return y + x;
  }

  return add;
}
```
