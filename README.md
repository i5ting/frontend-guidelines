# Frontend Guidelines

## HTML

### Semantics 语义化

HTML5 provides us with lots of semantic elements aimed to describe precisely the content. Make sure you benefit from its rich vocabulary.

```html
<!-- 坏的方式 -->
<div id="main">
  <div class="article">
    <div class="header">
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- 好的方式 -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime="2015-02-21">21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

Make sure you understand the semantic of the elements you're using. It's worse to use a semantic
element in a wrong way than staying neutral.

```html
<!-- 坏的方式 -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- 好的方式 -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

### Brevity 精短

Keep your code terse. Forget about your old XHTML habits.

```html
<!-- 坏的方式 -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- 好的方式 -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### Accessibility 明了

Accessibility shouldn't be an afterthought. You don't have to be a WCAG expert to improve your
website, you can start immediately by fixing the little things that make a huge difference, such as:

* learning to use the `alt` attribute properly
* making sure your links and buttons are marked as such (no `<div class=button>` atrocities)
* not relying exclusively on colors to communicate information
* explicitly labelling form controls

```html
<!-- 坏的方式 -->
<h1><img alt="Logo" src="logo.png"></h1>

<!-- 好的方式 -->
<h1><img alt="My Company, Inc." src="logo.png"></h1>
```

### Language 指定语言

While defining the language and character encoding is optional, it's recommended to always declare
both at document level, even if they're specified in your HTTP headers. Favor UTF-8 over any other
character encoding.

```html
<!-- 坏的方式 -->
<!doctype html>
<title>Hello, world.</title>

<!-- 好的方式 -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### Performance 性能

Unless there's a valid reason for loading your scripts before your content, don't block the
rendering of your page. If your style sheet is heavy, isolate the styles that are absolutely
required initially and defer the loading of the secondary declarations in a separate style sheet.
Two HTTP requests is significantly slower than one, but the perception of speed is the most
important factor.

```html
<!-- 坏的方式 -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- 好的方式-->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### Semicolons 请带上分号

While the semicolon is technically a separator in CSS, always treat it as a terminator.

```css
/* 坏的方式 */
div {
  color: red
}

/* 好的方式 */
div {
  color: red;
}
```

### Box model 盒子模型

The box model should ideally be the same for the entire document. A global
`* { box-sizing: border-box; }` is fine, but don't change the default box model
on specific elements if you can avoid it.

```css
/* 坏的方式 */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* 好的方式 */
div {
  padding: 10px;
}
```

### Flow 流

Don't change the default behavior of an element if you can avoid it. Keep elements in the
natural document flow as much as you can. For example, removing the white-space below an
image shouldn't make you change its default display:

```css
/* 坏的方式 */
img {
  display: block;
}

/* 好的方式 */
img {
  vertical-align: middle;
}
```

Similarly, don't take an element off the flow if you can avoid it.

```css
/* 坏的方式 */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* 好的方式 */
div {
  width: 100px;
  margin-left: auto;
}
```

### Positioning 定位

There are many ways to position elements in CSS but try to restrict yourself to the
properties/values below. By order of preference:

```
display: block;
display: flex;
position: relative;
position: sticky;
position: absolute;
position: fixed;
```

### Selectors 选择器

Minimize selectors tightly coupled to the DOM. Consider adding a class to the elements
you want to match when your selector exceeds 3 structural pseudo-classes, descendant or
sibling combinators.

```css
/* 坏的方式 */
div:first-of-type :last-chid > p ~ *

/* 好的方式 */
div:first-of-type .info
```

Avoid overloading your selectors when you don't need to.

```css
/* 坏的方式 */
img[src$=svg], ul > li:first-child {
  opacity: 0;
}

/* 好的方式 */
[src$=svg], ul > :first-child {
  opacity: 0;
}
```

### Specificity

Don't make values and selectors hard to override. Minimize the use of `id`'s
and avoid `!important`.

```css
/* 坏的方式 */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* 好的方式 */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}
```

### Overriding 重写

Overriding styles makes selectors and debugging harder. Avoid it when possible.

```css
/* 坏的方式 */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* 好的方式 */
li + li {
  visibility: hidden;
}
```

### Inheritance 继承

Don't duplicate style declarations that can be inherited.

```css
/* 坏的方式 */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* 好的方式 */
div {
  text-shadow: 0 1px 0 #fff;
}
```

### Brevity 精短

Keep your code terse. Use shorthand properties and avoid using multiple properties when
it's not needed.

```css
/* 坏的方式 */
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}

/* 好的方式 */
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

### Language 语言

Prefer English over math.

```css
/* 坏的方式 */
:nth-child(2n + 1) {
  transform: rotate(360deg);
}

/* 好的方式 */
:nth-child(odd) {
  transform: rotate(1turn);
}
```

### Vendor prefixes  特定前缀（宿主也可以）

Kill obsolete vendor prefixes aggressively. If you need to use them, insert them before the
standard property.

```css
/* 坏的方式 */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* 好的方式 */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### Animations 动画

Favor transitions over animations. Avoid animating other properties than
`opacity` and `transform`.

```css
/* 坏的方式 */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* 好的方式 */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### Units 单位

Use unitless values when you can. Favor `rem` if you use relative units. Prefer seconds over
milliseconds.

```css
/* 坏的方式 */
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* 好的方式 */
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

### Colors 颜色

If you need transparency, use `rgba`. Otherwise, always use the hexadecimal format.

```css
/* 坏的方式 */
div {
  color: hsl(103, 54%, 43%);
}

/* 好的方式 */
div {
  color: #5a3;
}
```

### Drawing 绘图

Avoid HTTP requests when the resources are easily replicable with CSS.

```css
/* 坏的方式 */
div::before {
  content: url(white-circle.svg);
}

/* 好的方式 */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

### Hacks 少用骇客

Don't use them.

```css
/* 坏的方式 */
div {
  // position: relative;
  transform: translateZ(0);
}

/* 好的方式 */
div {
  /* position: relative; */
  will-change: transform;
}
```

## JavaScript

### Performance 性能

Favor readability, correctness and expressiveness over performance. JavaScript will basically never
be your performance bottleneck. Optimize things like image compression, network access and DOM
reflows instead. If you remember just one guideline from this document, choose this one.

```javascript
// 坏的方式 (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// 好的方式
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

### Statelessness 状态无关

Try to keep your functions pure. All functions should ideally produce no side-effects, use no outside data and return new objects instead of mutating existing ones.

```javascript
// 坏的方式
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// 好的方式
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

### Natives 少动，尽量保持原样

Rely on native methods as much as possible.

```javascript
// 坏的方式
const toArray = obj => [].slice.call(obj);

// 好的方式
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

### Coercion

Embrace implicit coercion when it makes sense. Avoid it otherwise. Don't cargo-cult.

```javascript
// 坏的方式
if (x === undefined || x === null) { ... }

// 好的方式
if (x == undefined) { ... }
```

### Loops 循环

Don't use loops as they force you to use mutable objects. Rely on `array.prototype` methods.

```javascript
// 坏的方式
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// 好的方式
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```
If you can't, or if using `array.prototype` methods is arguably abusive, use recursion.

```javascript
// 坏的方式
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// 坏的方式
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// 好的方式
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDiv(howMany - 1);
};
createDivs(5);
```

### Arguments 参数

Forget about the `arguments` object. The rest parameter is always a better option because:

1. it's named, so it gives you a better idea of the arguments the function is expecting
2. it's a real array, which makes it easier to use.

```javascript
// 坏的方式
const sortNumbers = () =>
  Array.prototype.slice.call(arguments).sort();

// 好的方式
const sortNumbers = (...numbers) => numbers.sort();
```

### Apply 

Forget about `apply()`. Use the spread operator instead.

```javascript
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// 坏的方式
greet.apply(null, person);

// 好的方式
greet(...person);
```

### Bind

Don't `bind()` when there's a more idiomatic approach.

```javascript
// 坏的方式
["foo", "bar"].forEach(func.bind(this));

// 好的方式
["foo", "bar"].forEach(func, this);
```
```javascript
// 坏的方式
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// 好的方式
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

### Higher-order functions 高阶函数

Avoid nesting functions when you don't have to.

```javascript
// 坏的方式
[1, 2, 3].map(num => String(num));

// 好的方式
[1, 2, 3].map(String);
```

### Composition 

Avoid multiple nested function calls. Use composition instead.

```javascript
// 坏的方式
const plus1 = a => a + 1;
const mult2 = a => a * 2;

mult2(plus1(5)); // => 12


// 好的方式
const pipeline = (...funcs) =>
  val => funcs.reduce((a, b) => b(a), val);

const plus1 = a => a + 1;
const mult2 = a => a * 2;
const addThenMult = pipeline(plus1, mult2);

addThenMult(5); // => 12
```

### Caching 缓存

Cache feature tests, large data structures and any expensive operation.

```javascript
// 坏的方式
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// 好的方式
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

### Variables 变量

Favor `const` over `let` and `let` over `var`.

```javascript
// 坏的方式
var obj = {};
obj["foo" + "bar"] = "baz";

// 好的方式
const obj = {
  ["foo" + "bar"]: "baz"
};
```

### Conditions 条件

Favor IIFE's and return statements over if, else if, else and switch statements.

```javascript
// 坏的方式
var grade;
if (result < 50)
  grade = "坏的方式";
else if (result < 90)
  grade = "好的方式";
else
  grade = "excellent";

// 好的方式
const grade = (() => {
  if (result < 50)
    return "坏的方式";
  if (result < 90)
    return "好的方式";
  return "excellent";
})();
```

### Object iteration 对象遍历

Avoid `for...in` when you can.

```javascript
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// 坏的方式
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// 好的方式
Object.keys(obj).forEach(prop => console.log(prop));
```

### Objects as Maps

While objects have legitimate use cases, maps are usually a better, more powerful choice. When in
doubt, use a `Map`.

```javascript
// 坏的方式
const me = {
  name: "Ben",
  age: 30
};
var meSize = Object.keys(me).length;
meSize; // => 2
me.country = "Belgium";
meSize++;
meSize; // => 3

// 好的方式
const me = Map();
me.set("name", "Ben");
me.set("age", 30);
me.size; // => 2
me.set("country", "Belgium");
me.size; // => 3
```

### Curry

Currying might have its place in other languages, but avoid it in JavaScript. It makes your code harder to read by introducing a foreign paradigm while the appropriate use cases are extremely unusual.

```javascript
// 坏的方式
const sum = a => b => a + b;
sum(5)(3); // => 8

// 好的方式
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

### Readability

Don't obfuscate the intent of your code by using seemingly smart tricks.

```javascript
// 坏的方式
foo || doSomething();

// 好的方式
if (!foo) doSomething();
```
```javascript
// 坏的方式
void function() { /* IIFE */ }();

// 好的方式
(function() { /* IIFE */ }());
```
```javascript
// 坏的方式
const n = ~~3.14;

// 好的方式
const n = Math.floor(3.14);
```

### Code reuse

Don't be afraid of creating lots of small, highly composable and reusable functions.

```javascript
// 坏的方式
arr[arr.length - 1];

// 好的方式
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```javascript
// 坏的方式
const product = (a, b) => a * b;
const triple = n => n * 3;

// 好的方式
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

### Dependencies

Minimize dependencies. Third-party is code you don't know. Don't load an entire library for just a couple of methods easily replicable:

```javascript
// 坏的方式
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// 好的方式
const compact = arr => arr.filter(el => el);
const unique = arr => [...Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```
