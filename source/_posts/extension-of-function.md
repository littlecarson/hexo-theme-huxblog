
---
layout:     post
title:      "Extension of Function"
subtitle:   "ES6 函数的扩展 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-11-26 15:00:00
author:     "Carson"
catalog:    true
tags:
    - ECMAScript6
    - JavaScript
---

## 函数扩展

ES6 可直接为函数参数指定默认值, 可采用结构赋值的方式传递参数

```
function log(x = 0, y = 0) {
    console.log(x + y);
}

functio add ([x, y]) {
    console.log(x * y);
}

var [a, b] = [1, 3];
add([a, b]); // 3

```

### 函数的 length 属性

这个属性的含义为该函数预期传入的参数个数。注意，当指定了默认值以后，函数的 length 属性，将返回没有指定默认值的参数个数。也 就是说，指定了默认值后， length 属性将失真。

```
(fuction(a, b, c=3) {}).length // 2
(fuction(a, b, c) {}).length // 3

// 如设置了默认值的参数不是尾参数，length 属性也不再计入后面的参数


(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1

```

- 作用域规则：优先当前作用域，其次是全局作用域

```
// 函数作用域先生成变量 x
var x = 1;
function fun(x , y = x) {
    console.log(y);
}
fun(); // undefined
fun(3); // 3

// 函数作用域内未生成变量 x
let x = 1;
function fun(y = x) {
    console.log(y);
}
fun(); // 1
fun(3); // 3

// 若两个作用域都没有
function fun(y = x) {
    var x = 2;
    console.log(y);
}
fun(); // referenceError....

// 参数暂时性死区
var x = 1;
function fun(x = x) {
    var x = 2;
    console.log(x);
}
fun(); // referenceError...

// 参数是函数，该函数的作用域是其声明时的作用域
let foo = 'outer';
function bar (func = x => foo) {
    let foo = 'inner';
    console.log(func());
}
bar(); // outer

function bar2 (func = () => foo2) {
    let foo2 = 'inner2';
    console.log(func());
}
bar2(); // ReferenceError 匿名函数作用域在外层，但是并没有 foo2

// 一个更复杂的例子
var x = 1;
function foo(x, y = function() {x = 2;}) {
    var x = 3;
    y();
    console.log(x);
}
foo(); // 3
console.log(x); // 1

// ============= 应用利用参数默认值，指定某一个参数不得省略，如省略就抛出错误
function throwIfMissing() {
    throw new Error('Missing parameter');
}

function foo(mustBeProvided = throwIfMissing()) {
    return mustBeProvided;
}

foo();
```

### rest 参数

rest参数搭配的变量是一个数组，该变量将多余的参数放入
数组中

```
function add(first,...value) {
    console.log(first); // 2
    let sum = 0;
    for (let val of value) {
        sum += val;
    }
    return sum;
}

var result = add(2, 3, 5, 7);
console.log(result); //15

// 个rest参数代替arguments变量的例子
function sortNumbers() {
    return Array.prototype.slice.call(arguments).sort();
}
const sortNumbers = (..numbers) => numbers.sort();
```

- rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错

```
function f(a, ...b, c) {
    // ...
}
```

- 函数的length属性，不包括rest参数

```
var firstOne = ((a, b, c) => {}).length;
var secondOne = ((...c) => {}).length;
var thirdOne = ((a, ...c) => {}).length;
console.log(firstOne, secondOne, thirdOne); // 3 0 1
```

- 扩展运算符：rest 参数的逆运用

```
function test (a, b, c) {...}
var arg = [1, 3, 5];
test(..arg);

// ------------- 合并数组
// es5
[1, 2].concat(more);
// es6
[1, 2, ...more]

// -------------- 与结构赋值
// es5
a = list[0], rest = list.slice(1);
// es6
[a, ...rest] = list
//如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错
const [...butLast, last] = [1, 2, 3, 4, 5]; // 报错
const [first, ...middle, last] = [1, 2, 3, 4, 5]; // 报错

// -------------- 函数返回值
var dataFields = readDateFields(database);
var d = new Date(...dateFields);

// -------------- 字符串
[...'hello'] // ['h', 'e', 'l', 'l', 'o'] 
// 能够正确识别32位的Unicode字符
'x\uD83D\uDE80y'.length // 4
[...'x\uD83D\uDE80y'].length // 3

// 实现了Iterator接口的对象
var nodeList = document.querySelectorAll('div');
var arr = [...nodeList];

// Map 和 Set 结构
```


### 箭头函数

```
// 单个参数
var f = v => v;
var f = function(v) {
    return v;
}

// 零个参数或多个参数
var f = () => 2;
var f = function() {return 2};

var sum = (a, b) => a + b;
var sum = function(a, b) {
    return a + b;
};

// 箭头函数代码块多于一条语句，需要用大括号包括起来，并且使用return
var sum　= (a, b) => {a *= b; return a + b};

// 如果返回一个对象，必须加上括号
var getTempItem = id => ({id: id, name: 'Temp'});

// 结合变量结构赋值
const full = ({first, last}) => first + ' ' + last;
function full(person) {
    return person.id + ' ' + person.last;
}

// 用法： 简化工具函数
const isEven = n => n % 2 == 0;
const square = n => n * n;

// 用法： 简化回调函数
[1, 2, 3].map(x => x * x);
[1, 2, 3].map(function (x) {
      return x * x;
});

// 结合 rest 参数
const numbers = (...nums) => nums;
const numbers = function(...nums) {
    return nums;
};
numbers(1, 2, 3, 4); // [1, 2, 3, 4]
```

### 函数绑定

```
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);

const hasOwnProperty = Object.prototype.hasOwnProperty;
function hasOwn(obj, key) {
    return obj::hasOwnProperty(key);
}

```
