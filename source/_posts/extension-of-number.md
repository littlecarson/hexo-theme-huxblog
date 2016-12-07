
---
layout:     post
title:      "Extension of Number"
subtitle:   "ES6 数值的扩展 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-11-25 10:00:00
author:     "Carson"
catalog:    true
tags:
    - ECMAScript6
    - JavaScript
---


## 数值的扩展

### 二进制、八进制、十六进制数值的表示

- 二进制：前缀加 **0b**

```
var a = 0b10;
var b = Number('0b11'); // 将字符串转换为十进制数值
console.log(a, b); // 2 3
```

- 八进制： 前缀加 **0o**

```
var a = 0o10;
var b = Number('0o11');
console.log(a, b); // 8 9
```

- 十六进制：前缀加 **0x**


## 检测特殊值 —— **Infinite**、**Nan**

Es6 在 **Number** 对象上新提供了 **`Number.isFinite()`**、**`Number.isNaN()`**两个方法检查  **Infinite**、**Nan** 两个特殊值。

- **`Number.isFinite()`**：检查一个数值是否非无穷（infinity）。

```
Number.isFinite(1); // true
Number.isFinite(Infinity) // false
```

## 转换类型函数被转移

ES5 的全局方法 `parseInt()`, `parseFloat()` 被转移到 **Number** 对象上

```
// ES5
parseInt('1.11');
parseFloat('123.45');

// ES6
Number.parseInt('1.11');
Number.parseFloat('123.45');
```

## 判断值是否为整数

JavaScript 中，整数和浮点数采用同样的存储方法，所以检测小数也会返回 `true`。

```
Number.isInteger('1.1'); // false
Number.isInteger(1.1)  // true
```


## 极小常量

极小常量（Number.EPSILON）：是为在浮点数计算时设置一个误差范围,当小于这个范围可以认为结果正确，误差在可接受范围。

```
0.1 + 0.2  // 0.30000000000000004
0.1 + 0.2 -0.3 < Number.EPSILON  // true
```

## 安全整数

安全整数范围的上下限：`Number.MAX_SAFE_INTEGER`、`Number.MIN_SAFE_INTEGER`。JavaScript 可以正确表示的整数范围为：(-2^53 ~ 2^53)。超过后无法精确表示

```
Math.pow(2, 53) === Math.pow(2, 53) + 2 // true

Number.MAX_SAFE_INTEGER === Math.pow(2, 53) // true
Number.MIN_SAFE_INTEGER === Math.pow(-2, 53) // true
```

- `Number.isSafeInteger()` 方法可以判断一个整数是否落在这个范围中。

```
Number.isSafeInteger('abc') // false
Number.isSafeInteger(null) // false
Number.isSafeInteger(NaN) // false
Number.isSafeInteger(Infinity) // false
Number.isSafeInteger(-Infinity) // false

Number.isSafeInteger(3) // true
Number.isSafeInteger(Number.MIN_SAFE_INTEGER) // true
```


## Math 对象的扩展

ES6 在`Math`对象上新增了17个与数学相关的静态方法，只能在对象上调用。

- Math.trunc() : 去除一个数的小数部分，返回整数部分（对于非数值先转为数值）

```
Math.trunc('123.456')  // 123
Math.trunc(3.1)  // 3
Math.trunc(3.9)  // 3
Math.trunc(-0.1234)  // -0
```

- Math.sign() ：判断一个数是正数还是负数还是零

```
Math.sign(5) // 1
Math.sign(-5) // -1
Math.sing(0) // 0
```

- Math.cbrt() : 计算一个数的立方根
- Math.clz32() : 放回一个数的32位无符号整数形式前有多少个零（左移运算符用到）

- Math.imul() : 返回两个数以32位带符号整数形式相乘的结果（32带符号）

```
Math.imul(2, 4) // 8
(0x7ffffff * 0x7ffffff) | 0 // 0  二进制数最低位原因
Math.imul(0x7ffffff, 0x7ffffff) // 1
```

- Math.fround() : 返回一个数的单精度浮点数形式（对于无法用64位二进制精确表示的小数可以得到最接近这个小数的单精度浮点数）
- Math.hypot() : 返回所有参数的平方和的平方根

```
Math.hypot(3, 4); // 5
Math.hypot(NaN); // NaN
Math.hypot(-3) // 3
```

- 对数方法：`Math.expm1()`, `Math.log1p()`, `Math.log10()`, `Math.log2()`

```
Math.expm1(1) // e^1-1  = 1.7182828459045
Math.log1p(0) // ln(1+0) = 0 
Math.log10(1) // log10^1 = 0  返回以10为底的x的对数
```

- 三角函数方法

```
Math.sinh(x) // 返回 x 的双曲正弦 hyperbolic sine
Math.cosh(x)
Math.tanh(x)
Math.asinh(x)
Math.aconh(x)
Math.atanh(x)
```


### 指数运算符

```
2 ** 2 // 4
2 ** 3 // 8
let a = 2;
a **= 3 // 8  === 2 * 2 * 2
```
