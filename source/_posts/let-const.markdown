---
layout:     post
title:      "Let and Const Command"
subtitle:   "let adn const 命令 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-10-30 23:50:00
author:     "Carson"
catalog:    true
header-img: "post-bg-js-version.jpg"
tags:
    - ECMAScript6
    - JavaScript
---

### let 命令

let 申明的变量只在变量所在代码块内有效。

- 基本用法

```
{
    let a = 1;
    var b = 1;
}
console.log(a); // a is not defined

var arr = [];
for (let i = 0; i < 10; i++) {
    arr[i] = function() {
        console.log(i);
    }
}
arr[3](); // 3
```

- 不存在变量提升现象: 变量必须在声明后使用，typeof 操作不再安全

```
console.log(foo2);
console.log(foo);
let foo = 2;
var foo2 = 'foo2';
```

- 暂时性死区(temporal dead zone)：作用域内绑定变量，不受外部的影响，在声明语句前不能使用

```
var tmp = 21;
{
    tmp = '21';
    let tmp = 'tmp';
}

function test(x = y; y = 2) {
    console.log(x);
}
test(); // ReferenceError: y is not defined
```

### 块级作用域

- ES6 允许块级作用域任意嵌套，外级作用域无法读取内层作用域的变量

```
var tmp = new Date();

funtion f() {
    console.log(tmp);
    if (false) {
        var tmp = 'hello world'; 
        for ()
        // var 申明属于函数作用域，不是块级，会有变量提升从而覆盖外层作用域
    }
}

// for语句内循环计数变量会泄露为全局变量
for (var inner = 0; inner < 3; inner++) {
    console.log('for executing');
}

f(); // undefined
console.log(inner); // 3
```

```
{
    var a = 'outer_a';
    {
        // 内层作用域可以定义与外部相同名字的变量，互不干扰
        var a = 'inner_a';
    }
    console.log(a);
}
```

- 函数本身的作用域在其所在所在的块级作用域之内
```
f();  // outer fun

function f() {
    console.log('first outer fun');
}
// cover， havent tempoly dead zone
function f() {
    console.log('outer fun');
}

{
    f() // outer fun

    if (false) {
        // 不会函数提升而覆盖外层函数值
        function f() {
            console.log('first inner fun');
        }
    }
}

{
    f(); // second inner fun
    function f() {
        console.log('second inner fun');
    }
}

f(); // outer
```


### const 命令

- const 命令用于声明常量，一旦申明，必须立即初始化，且不能再改变。

```

const FOO; //SyntaxError: Missing initializer in const declaration

const PI = 3.1415;
console.log(pi);
pi = 4; // TypeError: Assignment to constant variable

```

- const 同 let 只在声明所在的块级作用域内有效，不提升，存在暂时性死区，不重复

```
if (true) {
    console.log(MAX); // error
    const MAX = 2;
}

console.log(MAX); // ReferenceError: MAX is not defined
```

- const 命令对于复合性变量，只指向地址

```
const a = [];
a.push("hello");
a.length = 0;
a = ['carson'];*/

// 如果想真的冻结对象，可以使用 Object.freeze 方法
const foo = Object.freeze({a: 'a', b: 'b'});
console.log(foo.a);
console.log(foo.b);

foo.a = 'hahah'; // ignore
console.log(foo.a); // a
```


### 全局对象的属性

- var, function 命令声明的全局变量依旧是全局对象的属性

```
// node.js repl 环境下（模块环境下，全局属性必须显式的声明为global对象的属性）
var a = 1
console.log(global.a) // 1

global.a = 2
console.log(a) // 2;
```

- let, const 命令声明的全局变量不属于全局对象的属性

```
// let a = 'let a';
// global.a // undefined
```
