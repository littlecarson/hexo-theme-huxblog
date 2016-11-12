---
layout:     post
title:      "I Don't Know Python"
subtitle:   "我不知道的Python"
date:       2016-11-5 14:23:00
author:     "Carson"
catalog:    true
tags:
    - Python
---


## 我不知道的 **Python**

记录一些跟我平常比较陌生的 **Python** 用法。

### 自文档化的 **Python** 语言

一直以来都忽视了这门语言的一友好性，**Python** 对大多函数和模块都自带简短的解释文档，可以省下去网上查阅文档和书籍的时间，真的是非常的方便。

- 列出模块中的所有函数—— `dir()`

```
>>> import math
>>> dir(math)
['__doc__', '__name__', '__package__', 'acos', 'acosh', 'asin', 'asinh', 
'atan', 'atan2', 'atanh', 'ceil', 'copysign', 'cos', 'cosh', 'degrees', 'e', 
'erf', 'erfc', 'exp', 'expm1', 'fabs', 'factorial', 'floor', 'fmod', 'frexp', 
'fsum', 'gamma', 'hypot', 'isinf', 'isnan', 'ldexp', 'lgamma', 'log', 'log10', 
'log1p', 'modf', 'pi', 'pow', 'radians', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 
'trunc']
```

- 查看完整的 **Python** 内置函数清单

`>>> dir(__builtins__)`

- 查看模块或函数的文档字符串

```
# 查看math模块的帮助文档
>>> help(math)
# 查看具体到函数的帮助文档
>>> help(math.log)
```

- 打印文档字符串

```
>>> print (math.__doc__)
This module is always available.  It provides access to the
mathematical functions defined by the C standard.

>>> print (math.log.__doc__)
log(x[, base])

Return the logarithm of x to the given base.
If the base not specified, returns the natural logarithm (base e) of x.
```

- 自定义文档字符串

```
def area(radius):
    """Returns the area of a circle with given radius.
    For example:
    >>> area(2)
    12.566370614359172
    """
    return math.pi * radius ** 2

print area.__doc__
```

### 类型转换

**Python** 使用内建函数来简化类型转换这一工作。*注意：2.x版本跟3.x版本有不一样的地方。*

- 整数、字符串、浮点数之间的转换

```
# Python 2.7.6 环境下
# 转换为浮点数
>>> float(3)
3.0
>>> float('3')
3.0
>>> float('3.2')
3.2

# 转换为字符串
>>> str(0)
'0'
>>> str(1.2)
'1.2'

# 转换为整数
>>> int(3.8)
3
>>> int('3')
3
>>> int('3.1')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '3.1'
```

- 圆整（总是向上圆整带来的偏差可能导致计算不准确，所以 **3.x**圆整到最接近的偶数）

```
>>> round(2.1)
2
>>> round(2.5)
2

>>> round(3.1)
3
>>> round(3.5)
4
```


### 赋值

- 多重赋值（**JavaScript** 中解构赋值）

```
>>> a, b, c = 'a', 'b', 'c'
>>> a
'a'
>>> b
'b'
```

- 交换变量的值

```
>>> a, b = 2, 5
>>> a
2
>>> a, b = b, a
>>> a
5
```

### 3.x版本中的input函数与2.x版本区别

- 3.x 中的 **input**对应于2.x中的 **raw_input**函数

```
# 2.7 test.py
name = raw_input('enter your name:').strip().capitalize()
print 'Hello ' + name
$ python test.py
enter your name:         Carson chen
Hello Carson chen
```

- 2.x 中的 **input** 函数，对用户输入的字符串求值

```
# 2.7 test.py
name = input('enter your name:').strip().capitalize()
print 'Hello ' + name
$ python test.py
enter your name:Carson
Traceback (most recent call last):
  File "test.py", line 1, in <module>
    name = input('enter your name:')
  File "<string>", line 1, in <module>
NameError: name 'Carson' is not defined
```


### 字符串相关

- 负数索引

```
>>> str = 'abc';
>>> str[-1]
'c'
>>> str[-3]
'a'
```

- 查看字符编码：`ord(char)`

```
>>> ord('a')
97
>>> ord('b')
98
```

- 查看编码字符：`chr(number)`

```
>>> chr(97)
'a'
>>> chr(98)
'b'
```

**有始（2016.11.4）有终**