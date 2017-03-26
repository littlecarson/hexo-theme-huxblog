---
layout:     post
title:      "Learning Typescript"
subtitle:   "学习 Typescript"
date:       2017-3-24 20:00:00
author:     "Carson"
catalog:    true
tags:
    - Typescript
    - JavaScript
---

## 什么是Typescript?

Typescript 是微软发布的带有类型系统与模块系统的 JavaScript 超集语言。通过向 JavaScript 增加可选的静态类型声明来把 JavaScript 变成超强类型的语言。

## 可选的静态类型声明

```
// ----------- 声明变量
var TestVar = null;
let isValid = false;
const apikey = 'hello';

// ---------- 基本类型
// 1. any 类型
var counter;
// 2. number 类型
var sum : number;
var height: number = 6;
// 3. boolean 类型
var isDone: boolean = false;
// 4. string
var xin: string = "Carl";
// 5. array
var list_char:string[] = ['a', 'b', 'c'];
var list_num:Array<number> = [1, 2, 3];
var list_any:any[] = ['a', 1, true];
// 6. enumuable
enum Color {Red, Green, Blue};
var c: Color = Color.Green;
// 7. void
function test(): void {
  alert("This is a test msg.")
}
// 8. union
var path : string[] | string;
path = '/temp/log.xml';
path = ['/temp/log.xml', '/temp/errors.xml'];
path = 1; // error

// ----------- 类型守护
var x : any = {/*...*/};
if(typeof x === 'string') {
  console.log(x.splice(3,1)); // error, string 上不存在 splice 方法
}
x.foo(); // valid

// ----------- 类型别名
// 使用 type 关键字 声明类型别名
type MyNumber = number;
type CallBack = () => void;

// ---------- 环境声明
interface ICustomConsole {
  log(arg : string) : void;
}
declare var customConsole : ICustomConsole;
customConsole.log('A log entry'); // success
```
