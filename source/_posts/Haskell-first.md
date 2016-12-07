---
layout:     post
title:      "Haskell First"
subtitle:   "Haskell 之初"
date:       2016-12-7 22:00:00
author:     "Carson"
catalog:    true
tags:
    - Haskell
---


## 0

从 Haskell 的官网下载（ https://www.haskell.org ）包含 **GHC**、**cabal**（Haskell的包管理器）的 **Minimal installers**, 

- Glasgrow：格拉斯哥，英国的一座城市
- GHC（The Glasgow Compilier）：Haskell 编译运行实现
- cabal：Haskell的包管理工具，同时身兼自动化编译工具的功能
- 注意：不要轻易使用 Linux的自带包管理工具，因为会按照到全局空间
- The Haskell Report：haskell 最官方、最全面的参考文档

## 基本语法

### 注释

```
-- 行注释
{-
    段注释
-}
```

### 表达式

有值得式子就是表达式，如：

```
2
True
x + 1
3 :: Float
sort [3, 2, 1]
-- sort 是排序函数，后面是传参，调用不需要加括号，多个参数之间也不要加逗号
case Foo of True  -> 1
            False -> 2
```

### 声明

声明用于表达程序的结构，若干个声明组成一个完整的 Haskell 程序。

- 类型声明：使用 **::** 手动添加类型标注

```
welcomeMsg :: String      -- welcomeMsg 的类型是String
addOne :: Int -> Int
```

- 绑定声明：使用 **=** 把等号右侧的表达式绑定到名称上

```
-- 不存在变量，只存在绑定
welcomeMsg = "hello world"
-- 任何时刻一个名称对应的表达式都是唯一确定的
addOne x = x + 1
```

- 模块声明和导入声明

```
module Main where
import Data.List

main :: IO ()
main = print (nub [1, 2, 3, 2])

{--
    Haskell 默认会在所有模块中导入一个叫做 Prelude
--}

```
