---
layout:     post
title:      "Extension of String"
subtitle:   "ES6 字符串的扩展 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-11-12 13:04:00
author:     "Carson"
catalog:    true
tags:
    - ECMAScript6
    - JavaScript
---


## 字符串的扩展

### 字符串的Unicode表示法

`'\uxxxx'` 反斜杠加码点表示一个字符，编码限定于`'\u0000'--'\uFFFF'`, 超出范围的字符按原样显示，比如`'\u20BB7'`=>`"₻7"`,此时应该用两个双字节的新式表示`'\uD842\uDFB7'`或者加上大括号`\u{20BB7}`。

```
"\u{20BB7}"
// "𠮷"

"\u{41}\u{42}\u{43}"
// "ABC"

let hello = 123;
hell\u{6F} // 123

// 大括号表示法与四字节的utf-16编码是等价的
'\u{1F680}' === '\uD83D\uDE80'
// true
```

**Javascript** 表示字符的六种方法：

```
'\z' === 'z'  // true
'\172' === 'z' // true  严格模式下会报错（node v6.9.0)，浏览器下不会（chrome 53.0）
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true
```

### UTF-16格式的 **codePointAt** 和 **fromCodePoint**

JavaScript内部，字符以UTF-16的格式储存，每个字符固定为2个字节。因此，4个字节储存的字符（Unicode码点大于0xFFFF的字符），JavaScript会认为是两个字符，所以，*charAt方法无法读取整个字符，charCodeAt方法只能分别返回前两个字节和后两个字节的值*。

```
var s = "𠮷";

s.length // 2
s.charAt(0) // ''
s.charAt(1) // ''
s.charCodeAt(0) // 55362
s.charCodeAt(1) // 57271
```

- **`codePointAt()`** 方法，可以解决4个字节字符问题，返回一个字符的码点(codePointAt方法会正确返回32位的UTF-16字符的码点。对于两个字节储存的常规字符，它的返回结果与charCodeAt方法相同)

```
var s = '𠮷a';

s.codePointAt(0) // 134071 𠮷
s.codePointAt(1) // 57271 𠮷这个字符的后两个字节

s.charCodeAt(2) // 97 a
```

- 用 `for of` 循环识别32位的UTF-16字符

```
var s = '𠮷a';

s.codePointAt(0).toString(16) // "20bb7"
s.charCodeAt(2).toString(16) // "61"  注意这里， 传参为 2 22222

for (let ch of s) {
  console.log(ch.codePointAt(0).toString(16));
}
// 20bb7
// 61
```

- 用 **codePointAt**方法来测试一个字符由两个字节还是由四个字节组成的

```
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}

is32Bit("𠮷") // true
is32Bit("a") // false
```

- 用 **`String.fromCodePoint`** 找到码点返回对应字符，但是这个方法不能识别32位的UTF-16字符（Unicode编号大于0xFFFF）。

```
String.fromCodePoint(0x20BB7)
// "𠮷"
String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
// true 多个参数，会被合并成一个字符串返回
```
> 注意，fromCodePoint方法定义在String对象上，而codePointAt方法定义在字符串的实例对象上

### 字符串的遍历器接口

ES6为字符串添加了遍历器接口，字符串可以被`for...of`循环遍历。

```
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"

// 可以识别大于0xFFFF的码点
// 传统的for循环无法识别这样的码
var text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]); // " " // " "
}

for (let i of text) {
  console.log(i); // "𠮷"
}

```

### 新增的三个字符串搜索函数

- **`includes(str, startPos)`**：返回布尔值，表示是否找到了参数字符串。
- **`startsWith(str, startPos)`**：返回布尔值，表示参数字符串是否在源字符串的头部。
- **`endsWith(str, startPos)`**：返回布尔值，表示参数字符串是否在源字符串的尾部

```
var s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true  区别：endsWith针对前n个字符，其他是开始位置到尾
s.includes('Hello', 6) // false
```

### 新增的字符串重复函数

**repeat(N)**方法返回一个新字符串，表示将原字符串重复n次。参数会被取整：0到-1之间的小数，则等同于0；`NaN`等同于0；负数或者`Infinity`，会报错。

```
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""

'na'.repeat(-0.9) // ""
'na'.repeat('3') // "nanana"
```

### 模板字符串

模板字符串是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```
// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 字符串中嵌入变量
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

var greeting = `\`Yo\` World!`;

// 运算
var x = 1;
var y = 2;

// 使用反引号，需要转义
`${x} + ${y} = ${x + y}` // "1 + 2 = 3"

// 调用函数
function fn() {
  return "Hello World";
}

`foo ${fn()} bar`  // foo Hello World bar

// 嵌套
const tmpl = addrs => `
  <table>
  ${addrs.map(addr => `
    <tr><td>${addr.first}</td></tr>
    <tr><td>${addr.last}</td></tr>
  `).join('')}
  </table>
`;
const data = [
    { first: '<Jane>', last: 'Bond' },
    { first: 'Lars', last: '<Croft>' },
];

console.log(tmpl(data));
// <table>
//
//   <tr><td><Jane></td></tr>
//   <tr><td>Bond</td></tr>
//
//   <tr><td>Lars</td></tr>
//   <tr><td><Croft></td></tr>
//
// </table>
```

## 书中的实例：模板编译

通过模板字符串，生成正式模板。

定义的模板：
```
// 模板使用<%...%>放置JavaScript代码
// 使用<%= ... %>输出JavaScript表达式
var template = `
<ul>
  <% for(var i=0; i < data.supplies.length; i++) { %>
    <li><%= data.supplies[i] %></li>
  <% } %>
</ul>
`;
```

目标：将其转换为JavaScript表达式字符串。
```
echo('<ul>');
for(var i=0; i < data.supplies.length; i++) {
  echo('<li>');
  echo(data.supplies[i]);
  echo('</li>');
};
echo('</ul>');
```

首先，使用正则表达式转换：
```
var evalExpr = /<%=(.+?)%>/g;
var expr = /<%([\s\S]+?)%>/g;

template = template
  .replace(evalExpr, '`); \n  echo( $1 ); \n  echo(`')
  .replace(expr, '`); \n $1 \n  echo(`');

template = 'echo(`' + template + '`);';
```

然后，将template封装在一个函数里面返回
```
var script =
`(function parse(data){
  var output = "";

  function echo(html){
    output += html;
  }

  ${ template }

  return output;
})`;

return script;
```

再拼装成模板编译函数compile
```
function compile(template){
  var evalExpr = /<%=(.+?)%>/g;
  var expr = /<%([\s\S]+?)%>/g;

  template = template
    .replace(evalExpr, '`); \n  echo( $1 ); \n  echo(`')
    .replace(expr, '`); \n $1 \n  echo(`');

  template = 'echo(`' + template + '`);';

  var script =
  `(function parse(data){
    var output = "";

    function echo(html){
      output += html;
    }

    ${ template }

    return output;
  })`;

  return script;
}
```

最后，通过compile函数实现：
```
var parse = eval(compile(template));
div.innerHTML = parse({ supplies: [ "broom", "mop", "cleaner" ] });
//   <ul>
//     <li>broom</li>
//     <li>mop</li>
//     <li>cleaner</li>
//   </ul>
```

### 标签模板

标签模板其实不是模板，而是函数调用的一种特殊形式。“标签”指的就是函数（该函数将被调用来处理这个模板字符串），紧跟在后面的模板字符串就是它的参数。

```
var [a, b] = [5, 10];

tag`Hello ${ a + b } world ${ a * b }`;
// 等同于
tag(['Hello ', ' world ', ''], 15, 50);

// 等同于
function tag(stringArr, ...values){
  // ...
}

// 比如
function tag(s, v1, v2) {
  console.log(s[0]);
  console.log(s[1]);
  console.log(s[2]);
  console.log(v1);
  console.log(v2);

  return "OK";
}

tag`Hello ${ a + b } world ${ a * b}`;
// "Hello "
// " world "
// ""
// 15
// 50
// "OK"
```

标签模板的应用：

- 过滤HTML字符串：

```
var message =
  SaferHTML`<p>${sender} has sent you a message.</p>`;

function SaferHTML(templateData) {
  var s = templateData[0];
  for (var i = 1; i < arguments.length; i++) {
    var arg = String(arguments[i]);

    // Escape special characters in the substitution.
    s += arg.replace(/&/g, "&amp;")
            .replace(/</g, "&lt;")
            .replace(/>/g, "&gt;");

    // Don't escape special characters in the template.
    s += templateData[i];
  }
  return s;
}
```

- 多国语言转换

```
i18n`Welcome to ${siteName}, you are visitor number ${visitorNumber}!`
// "欢迎访问xxx，您是第xxxx位访问者！"
```

- 自定义类似模板库的条件循环功能

```
// 自定义的模板处理函数
var libraryHtml = hashTemplate`
  <ul>
    #for book in ${myBooks}
      <li><i>#{book.title}</i> by #{book.author}</li>
    #end
  </ul>
`;
```

- 在JavaScript语言之中嵌入其他语言

```
// 将一个DOM字符串转为React对象
jsx`
  <div>
    <input
      ref='input'
      onChange='${this.handleChange}'
      defaultValue='${this.state.value}' />
      ${this.state.value}
   </div>
`

// 在JavaScript代码之中运行Java代码
java`
class HelloWorldApp {
  public static void main(String[] args) {
    System.out.println(“Hello World!”); // Display the string.
  }
}
`
HelloWorldApp.main();
```

- 取得转义之前的原始模板（模板处理函数的第一个参数（模板字符串数组），有一个raw属性指向一个数组，该数组的与strings数组一致，还预先把字符串中的斜杠都转义好了）

```
tag`First line\nSecond line`

function tag(strings) {
  console.log(strings.raw[0]);
  // "First line\\nSecond line"
}  
```


### 原生字符处理函数

`String.raw`方法可以作为处理模板字符串的基本方法，它会将所有变量替换，而且对斜杠进行转义，方便下一步作为字符串来使用。

```
String.raw`Hi\n${2+3}!`;
// "Hi\\n5!"

String.raw`Hi\u000A!`;
// 'Hi\\u000A!'

// 如果原字符串的斜杠已经转义，不会做任何处理
String.raw`Hi\\n`
// "Hi\\n"

// 作为正常函数来用
String.raw({ raw: 'test' }, 0, 1, 2);
// 't0e1s2t'

// 等同于
String.raw({ raw: ['t','e','s','t'] }, 0, 1, 2);
```

