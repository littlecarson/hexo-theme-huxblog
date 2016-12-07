
---
layout:     post
title:      "Extension of Array"
subtitle:   "ES6 数组的扩展 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-11-27 20:00:00
author:     "Carson"
catalog:    true
tags:
    - ECMAScript6
    - JavaScript
---

## 数组的扩展

### 把类似数组结构转化为数组 —— Array.from()

`Array.from(object, mapCallBack)`

`Array.from` 可以把 类数组的对象和可遍历对象转化为数组，以便进行一些数组的方法操作。

- 类数组对象：拥有 **length** 属性的对象，如DOM节点集合、arguments对象。

```
// querySelectorAll 返回的是类数组对象，没有遍历方法
let ps = documents.querySelectorAll('p');  
Array.from(ps, (p) => console.log(p));

// 原始数据结构规范转化为数组结构
Array.from({length: 3}, (n) => n || o); // [0, 0, 0]

// 将字符串转为数组，返回字符串长度，可以处理Unicode字符，避免算2个字符的bug
var countSymbos = function(string) {
    return Array.from(string).length;
}
```

- 可遍历对象：部署了 **Iterator** 接口的数据结构，如 arguments 对象

```
// arguments 具有迭代器接口
function = foo() {
    var args = Array.from(arguments);
    // do sth ...
}

// 扩张运算符（...）具有一样的功能
funtion foo() {
    var args = [...arguments];
}

// 字符串和Set、Map 数据结构都具有迭代器接口
let testSet = new Set(['a', 'b']);
Array.from(testSet) // ['a', 'b']
Array.from('hello') // ['h', 'e', 'l', 'l', 'o']

```


### 弥补构造函数的不足 —— Array.of()

构造函数由于参数不同会导致重载，行为不统一。`Array.of(value, [...])` 总是返回数组。 

```
var a = new Array(3); // [ , , ,]
var b = Array.of(3);  // [3,]
b.length // 1

var c = Array.of()  // []
```


### 内部复制覆盖内部 —— copyWithin()

`Array.prototype.copyWithin(target, start=0, end=this.length)`

从当前数组内部制定位置的成员复制到当前数组内部指定位置，返回当前修改后的数组。

```
[1,2,3,4,5].copyWithin(0, 3, 5); // [4,5,3,4,5]

// 从3号位置
[1,2,3,4,5].copyWithin(0, 3, 4); // [4,2,3,4,5]

```

### 搜索数组值和位置

`find(function(value, index, arr){})`找到第一个返回true的成员，否则返回`undefined`

`findIndex()`，返回第一个符合成员的位置，否则返回-1

```
[1,3,5].find((n) => n > 3); // 5

[1, 3, 5].findIndex(function(value, index, arr) {
    return value >3;
});
```

### 数组填充 —— fill()

`fill(value，[begin, end])`使用给定值填充一个数组

```
// 可用于空数组的初始化
new Array(2).fill(8) // [8, 8]

['a', 'b', 'c'].fill(8)
// [7, 7, 7]

['a', 'b', 'c'].fill(7, 1, 2);
// ['a', 7, 'c']

```

### 遍历数组的新方法 —— entries(), keys(), values()

都用于遍历数组，都返回一个遍历器对象，可以用`for...of`循环进行遍历，唯一区别是，`keys` 是对键名的遍历，`values` 是对键值的遍历，`entries` 是对键值对的遍历。

```
for(let index of ['a', 'b', 'c'].keys()) {
    console.log(index)
}
// 0
// 1
// 2

for (let elem of ['a', 'b'].values()) {
    console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
}
// 0 'a'
// 1 'b'

```


### 检测是否包含元素 —— includes()

`includes(value, [beginIndex])` 从起始位置搜索给定的值，避免了`indexOf()`方法的缺陷： 不够语义化（比较是否不等于-1），严格检测（误判NaN）。

```
// 不够语义化
if(arr.indexOf(el) !== -1) {
    // ...
}

// 严格检测的缺陷
[NaN].indexOf(NaN) // -1

// includes 方法返回布尔值
[NaN].includes(NaN) // ture

// 注意区分Map,Set数据结构的has方法
// Map.has 用来查找键名的
// Set.has 用来查找值的

```

### 数组空位

数组的空位指，数组的某一个位置没有任何值。注意，空位不是 undefined ，一个位置的值等于 undefined ，依然是有值的。Es5 对空位的处理大多情况都会忽略。

- forEach() , filter() , every() 和 some() 都会跳过空位。
- map() 会跳过空位，但会保留这个值
- join() 和 toString() 会将空位视为 undefined ，
而 undefined 和 null 会被处理成空字符串。

```
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1
// filter方法
['a',,'b'].filter(x => true) // ['a','b']
// every方法
[,'a'].every(x => x==='a') // true
// some方法
[,'a'].some(x => x !== 'a') // false
// map方法
[,'a'].map(x => 1) // [,1]
// join方法
[,'a',undefined,null].join('#') // "#a##"
// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

ES6 明确将数组的空位转为 `undeifined`

- Array.from 方法会将数组的空位，转为 undefined 
- 扩展运算符（ ... ）也会将空位转为 undefined
- copyWithin() 会连空位一起拷贝
- fill() 会将空位视为正常的数组位置
- for...of 循环也会遍历空位
- entries() 、 keys() 、 values() 、 find() 和 findIndex() 会将空位处
理成 undefined 

```
Array.from(['a',,'b'])    // [ "a", undefined, "b" ]

[...['a',,'b']]    // [ "a", undefined, "b" ]

[,'a','b',,].copyWithin(2,0)    // [,"a",,"a"]

new Array(3).fill('a')    // ["a","a","a"]

let arr = [, ,];
for (let i of arr) {
    console.log(1);
}   // 1   // 1

// entries()
[...[,'a'].entries()] // [[0,undefined], [1,"a"]]

// keys()
[...[,'a'].keys()] // [0,1]

// values()
[...[,'a'].values()] // [undefined,"a"]

// find()
[,'a'].find(x => true) // undefined

// findIndex()
[,'a'].findIndex(x => true) // 0

```

