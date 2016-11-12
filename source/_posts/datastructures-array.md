---
layout:     post
title:      "Datastructures of Array in Java && JS"
subtitle:   "基础数据结构——数组（Java&&JS实现）"
date:       2016-11-8 12:43:00
author:     "Carson"
catalog:    true
tags:
    - Datastructures
    - JavaScript
    - Java
---


## 基本数据结构 —— 数组

按顺序排列的有限个元素，用一个变量命名的，通过顺序编号访问的一个有序集合。

**Java**中：数组是一个特殊对象（其类不存在，不能扩展），元素类型必须相同，数组长度不可更改。声明数组不会创建数组，只是创建了一个对象引用。创建数组的方式有两种：1. `new type[size]`; 2. `{element, element, element, ...}`

> **Java** 中把一个数组传给一个方法需要小心，如`average({1,3}) // illegal`,必须单独实例化数组，或者`average(new int[] {1,2})`


### 创建和初始化数组

- 只声明长度没有初始化元素，**Java**中，会赋值默认值，整形为0，引用类型为null, 布尔值为falwe.

```
// java
int[] a = new int[20];
int a[] = new int[20];
int [] a = new int[20]; // a[3] == 0 => true

// javascript
var a = new Array(20);
var b = []; b.length = 20;
```

- 声明了长度但只初始化部分值

```
// java 
int [] a = new int [20];
a[1] = 2;

// javascript
var a = [2, , , 6];
```

- 声明长度并且初始化所有的值
```
// java
int [][] a = { {2, 6, 7}, {1, 3, 5}, {7, 8, 9} };

// javascript
var a = [ [2, 6, 7], [1, 3, 5], [7, 8, 9] ];
```


### 修改数组的大小

Java 中必须创建一个新的数组，然后用旧的数组填充

```
// byself
int[] a = new int[8];
int[] tem = new int[20];
for (int i = 0; i < a.length; i++) {
    tem[i] = a[i];
}
a = tem;

// by java.util.Arrays.copyOf()
int[] a = new int[8];
int[] newA = Arrays.copyOf(a, 16);
a  = newA;

// javascript
var fibonacci = [];
fibnacci[1] = 1;
fibnacci[2] = 2;

for (let i = 3; i < 20; i++) {
    fibonacci[i] = fibonacci[i-1] + fibonacci[i-2];
}
console.log(fibonacci.length);
```

### 添加和删除数组的元素

```
// javascript (concat不影响原数组)

var numbers = [0, 1];
numbers[numbers.length] = 3;

// 添加元素到数组末尾 —————— push(element) return array.length
numbers.push(4); // [0, 1, 3, 4]

// 插入元素到开头 —————— unshift(element) return array.length
numbers.unshift(-1); // [-1, 0, 1, 3, 4]

// 删除最后一个元素 —————— pop() return array[length-1]
numbers.pop()  // [-1, 0, 1, 3] return 4

// 删除第一个元素 —————— shift() return array[0]
numbers.shift() // [0, 1, 3, 4] return -1

// 删除指定范围位置的元素 —————— splice(begin, delNumbers, addEle, addEle,...)
numbers.splice(2, 0, 2) // [0, 1, 2, 3, 4]
numbers.splice(2, 0, 0) // [0, 1, 0, 2, 3, 4] return []
numbers.splice(2, 3) // [0, 1, 4] return [0, 2, 3]

// 多个数组的合并 ————— concat(arr, arr2, number, string,...);
numbers.concat([5, 7, 9], 2, 3); // return [0, 1, 4, 5, 7, 9, 2, 3]
console.log(numbers) // [0, 1, 4]

// 迭代函数 —— every, some, forEach, map, filter, reduce
numbers.every(function (item) { // 遍历数组直到返回false
    return (index % 2 == 0)
});
numbers.some(function (item) { // 遍历数组直到返回true
    return (index % 2 == 0)
});
numbers.forEach(function (item) { // 遍历整个数组
    console.log((item % 2 == 0)); 
});
var myMap = numbers.map(function (item) { // 返回遍历过后的新数组
    return (item % 2 == 0);
});
numbers.reduce(function (previous, current, index) {
    return previous + current;    
})

// 复制一个数组
var a = ['apple', 'orange'];
var b = a.slice();

```

### 数组的搜索和排序

```
// 反序 —— reverse() return arr.reverse()
var numbers = [1, 2, 3, 4, 5];
numbers.reverse();
console.log(numbers); // [5, 4, 3, 2, 1]

// 字典排序 —— sort() returnt arr.sort()
var numbers = [1, 10, 2, 11, 3];
numbers.sort(); // [1, 10, 11, 2, 3]  ？？？
numbers.sort(function(a, b) {
    // sort函数会会根据 返回的正负值和零值来排序
    // 升序反负值， 降序反正值，零值不升降
    returnt a - b; 
}); // [1, 2, 3, 10, 11] 
// 忽略字母大小写的字典排序
var names = ['ab', 'Casron', 'Ac', 'she'];
names.sort(); // [ 'Ac', 'Carson', 'ab', 'she' ]
names.sort(function(a, b) {
    if (a.toLowerCase() < b.toLowerCase()) {
        return -1;
    }
    if (a.toLowerCase() > b.toLowerCase()) {
        return 1;
    }
    return 0;
});

// 搜索 —— indexOf(item), lastIndexOf(item)
var numbers = [ 0, 1, 2, 22, 5 ];
numbers.indexOf(5); // return 4
numbers.splice(2, 0 , 5);
numbers.indexOf(5); // return 2
numbers.lastIndexOf(5); // return 5
numbers.lastIndexOf(8); // return -2
```

### 输出数组为字符串

```
// toStrint()
var numbers = [ 0, 1, 2, 22, 5 ];
console.log(numbers.toString());  // 0,1,5,2,22,5

// join()
console.log(numbers.join()); // 0,1,5,2,22,5
console.log(numbers.join('')); // 0152225
console.log(numbers.join(' ')); // 0 1 5 2 22 5
console.log(numbers.join('-')); // 0-1-5-2-22-5
```


### 数组的特点以及使用场景

*数组必须从第一个元素开始访问，直到指定位置的元素*，适用于不会出现变化的业务上。