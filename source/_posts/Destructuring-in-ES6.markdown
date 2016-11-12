---
layout:     post
title:      "Destructuring in ECMAScript 6"
subtitle:   "变量的解构赋值 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-11-1 17:48:00
author:     "Carson"
catalog:    true
header-img: "post-bg-js-version.jpg"
tags:
    - ECMAScript6
    - JavaScript
---


> **Distructuring**（解构赋值）：按指定的结构模式，从数组和对象中提取出值对变量赋值


### 数组的解构赋值

- 基本用法：如果赋值号两边的模式相同，左边的变量会被赋予对应右边位置的值

```
var [a, b, c] = [1, 3, 5];
let [foo, [[bar], baz]] = [a, [[b], c]];
console.log(a, foo); // 1
console.log(b, bar); // 3
console.log(c, baz); // 5
```

- 用于集合结构中(具备Iterator接口)

```
var a = new Set(['a', 'b', 'c']);
console.log(a); // Set {'a', 'b', 'c'}
```

- **默认值**：如果数组成员不严格等于`undefined`，将会覆盖默认值

```
var [x = 1] = [null];
console.log(x); // 1
```

```
// 默认值式函数表达式
console.log([1, 3][1]); // 3
function f() {
    return 'fun';
}
var [x = f()] = ['first'];
console.log(x); // first

if ([2, 3][1] === undefined) {
    x = f()
} else {
    x = [2, 3][1];
}
console.log(x);
```

```
// 默认值也可以是引用解构赋值的其他变量（必须已经申明）
let [x = 1, y = x] = []; // x = 1; y = 1;
let [a = 1, b = a] = []; // a = 1; b = 1;
let [c = d, d = 1] = []; // Referenceerror: d is not defiened
```


### 对象的解构赋值

> 对象的解构赋值可以很方便地将现有对象的方法赋值到某个变量上，这样使用某些对象的方法可以变得非常方便。

- 为同属性名的变量赋值，对顺序没有要求

```
//======= demo1: 为同属性名变量赋值， 对顺序没有要求
var {foo, bar} = {foo: 'aaa', bar: 'bbb'};
console.log(foo);
console.log(bar);
```

```
// ====== demo2: 变量名与属性名不不一致的情形下
var  {bar: foo} = {bar: "bbb"};
console.log(foo);  // bbb
```

> 实际上说明，对象的解构赋值是：`var {bar: bar} = {bar: 'bbb'}`
> 也就是说，解构赋值的内部机制是先找到同名属性，再赋给对应的变量，真正被赋值的不是前者

- 模式名与变量：模式用来匹配查询，不会被赋值

```
// ====== demo3: 解构方式的变量声明和赋值是一体的
// 对于let,const 注意不能重新声明
let baz;
//此处要用圆括号，表示为表达式整体，否则会被分开看作两个代码块
({bar:baz} = { bar: 'bbbb'}); // baz == 'bbb'; 
// 以下为错误重新对变量声明
// let {bar:baz} = { bar: 'bbbb'};
```

- 用于嵌套解构的对象

```
// ====== demo4: 用于嵌套解构的对象
var node = {
    loc: {
        start: {
            line: 1,
            column: 4
        }
    }
};
// 声明 a, c 两个变量
var {loc: {start: {line:a, column:c}}} = node;
console.log(a, c);

// 声明loc一个变量
var {loc} = node;
console.log(loc);
```

- **默认值**：生效条件是，对象的属性值严格等于`undefined`

```
var {x = 3, y = x} = {x: null};
```


### 字符串的解构赋值

- 字符串会被实质上是类似数组的对象，所以也有解构赋值的用法

```
const [a, b, c, d, e] = 'hello';
const {length} = 'hello'; // 字符串对象有长度属性，可用对象解构获取
console.log(a, b, c, d, e); // h e l l o
console.log(length); // 5
```


### 数值和布尔值的解构赋值

数值和布尔值解构会先转换为对象（解构规则是，只要等号右边值不是对象都会先转换为对象，`undefined`、`null`除外）

```
var {toString: num_s} = 1;
console.log(num_s === Number.prototype.toString); // true

var {toString: bool_s} = true;
console.log(bool_s === Boolean.prototype.toString); // true
```


### 函数参数的解构赋值

- 函数参数的解构赋值用法

```
// ======== demo1: 参数套用数组解构模式
// 实际上是通过解构得到两个变量
function add ([x,y]=[]) {
    return x + y;
}
let result = add();
console.log(result); // NaN
result = add([1,3]);
console.log(result); // 4
```

```
// ====== demo2: 参数套用对象解构模式
function add ({x,y}={}) {
    return x + y;
}
let result = add();
console.log(result); // NaN
result = add({x: 1, y: 3});
console.log(result); // 4
```

- 函数参数的默认值用法

```
// ====== demo3: 设置函数参数的解构默认值
// 设置函数参数的默认值不可忽略 赋值号和右值
function add ({x=0, y=0} = {}) {
    return x + y;
}
//注意 如果函数定义为add({x=0, y=0}); 咩有设定默认函数参数调用是如果传空会报错（不能解构）
let result = add({a: 'x'});
console.log(result); // 0
result = add({x: 1, y: 3});
console.log(result);
```

```
// ====== demo4: 区分函数的默认解构变量与函数默认参数变量
function add ({x, y} = {x:0, y:0}) { // 与 add ({x=0, y=0} = {}) 不同
    return x + y;
}
let result = add({a: 'x'});
console.log(result); // not 0 ,is NaN, because y equals undefined
result = add();
console.log(result); // absoluate 0 haha...
```


### 解构赋值的用途

- 交换变量的值

```
// ========== demo1: 交换变量的值
let x = 1, y =1;
[x, y] = [y, x];
console.log(x, y); // -1, 1
```

- 提取从函数返回的多个值

```
function test() {
    return {
        foo: 1,
        bar: 2
    }
}
var {foo, bar} = test();
console.log(foo, bar); // 1 2
```

- 函数参数的定义

```
// 参数已一组有次序的值
function f([x, y, z]=[]) {...}
f([1, 2, 3,]);

// 参数是一组无次序的值
function f({x, y, z}={}) {...}
f({y:2, x:1, z:3});
```

- 提取JSON数据

```
var jsonData = {
    id : 2,
    status : "OK",
    data: [123, 456]
};

let {id, status, data: number} = jsonData;
console.log(id, status, number); // 2 'OK' [ 123, 456 ]
```

- 遍历 **Map** 结构

```
// 实例化图型键值对数据结构
var myMap = new Map();
myMap.set('first', 'Hello');
myMap.set('second', 'World');

// 获取键值对
for(let [key, value] of myMap) {
    console.log(key + 'is' + value);
}
// 只获取键名
for(let [key] of myMpa) {
    console.log(key);
}
// 只获取键值
for(let [, value] fo myMap) {
    console.log(value);
}
```

- 输入模块的指定方法

```
const {SourceMapConsumer, SourceNode} = require("source-map");
```
