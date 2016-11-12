---
layout:     post
title:      "JavaScript Reviewing Notes"
subtitle:   "JavaScript 重温笔记"
date:       2016-09-01 17:00:00
author:     "Carson"
catalog:    true
header-img: "post-bg-js-version.jpg"
tags:
    - JavaScript
    - 前端开发
---

### JavaScript 数据结构

- ### Undefined

    * 类型说明 值：undefined
    * 出现场景
        - 已经声明但未赋值的变量
        - 获取对象不存在的属性
        - 无返回值的函数执行结果
        - 函数的参数没有传入
        - void(expression)
    * 类型转换
    ```
    Boolean(undefined)  // 0
    Number(undefined)   // NaN
    String(undefined)   // "undefined"
    ```
