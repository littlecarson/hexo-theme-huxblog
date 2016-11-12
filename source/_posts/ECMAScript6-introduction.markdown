---
layout:     post
title:      "ECMAScript 6 Introduction"
subtitle:   "ES6 简介 ——《ECMAScript 6 Primer》学习笔记"
date:       2016-10-30 10:00:00
author:     "Carson"
catalog:    true
header-img: "post-bg-js-version.jpg"
tags:
    - ECMAScript6
    - JavaScript
---


- **ECMA**: 国际标准化组织
- **ECMAScript**: 1996.11, **JavaScript**的创造公司——**Netscape**将 **JavaScript**提交给国际化标准组织（制定标准组织）于次年发布标准文件，规定为浏览器脚本标准语言，称为 **ECMAScript**（version 1.0），是 **JavaScript**（实现） 的规格标准
- **ES6**（ECMAScript 2015的简称）: 2015.6 发布的 **JavaScript** 语言的下一代标准

### ECMAScript 的历史

- **ECMAScript** 1.0: 1997
- **ECMAScript** 2.0: 1998.6
- **ECMAScript** 3.0: 1999.12 业界广泛支持，成为通行标准，是为**JavaScript**语言的基本语法
- **ECMAScript** 4.0(3.1)：2000-2007年，2007年发布草案（未通过），2008.7年被否决，中止开发，将改善的一小部分发布为 **ECMAScript 3.1**，后改名为 **ECMAScript 5**（2009.12正式发布）。（未通过的部分代号Harmony）
- **ECMAScript** 5.1：2011.6 发布，并且成为 **ISO**国际标准
- **ECMAScript** 6：2013.3 草案冻结，12月草案发布（后12个月讨论期吸取反馈意见），2015年6月正式通过，历时15年


### 标准的支持进度

目前，**Node.js**服务器运行环境高于浏览器运行环境。

- [**nvm**](https://github.com/creationix/nvm): 管理 **node** 的版本管理工具(https://github.com/creationix/nvm)
    - 安装 **node**：`nvm install node[version]`;
    - 切换版本：`nvm use node [version]`;
    - 查看 **node**所有已实现的ES6特性: `node --v8-options | grep harmony`;
- [ES-Checker](https://github.com/ruanyf/es-checker)(阮一峰老师：https://github.com/ruanyf/es-checker)：`npm install -g es-checker` && `es-checker`


### Babel 转码器

[Bable](https://babeljs.io)(https://babeljs.io)转码器，可以将ES6代码转换为ES5代码，从而在还不支持新标准环境下运行ES6代码

#### 命令行环境

- 创建 **babel**的配置文件
```
# 切换淘宝源
$ npm install -g cnpm --registry=https://registry.npm.taobao.org

$ subl .babelrc
> {"preset": ['es2015'],"plugins": []}

# presets字段设定转码规则，官方提供以下的规则集

## ES2015转码规则
$ npm install --save-dev babel-preset-es2015

## react转码规则
$ npm install --save-dev babel-preset-react

## ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

- 命令行转码 **babel-cli**
    - 基本用法
    ```
    # 转码结果输出到标准输出
    $ babel example.js

    # 转码结果写入一个文件
    # --out-file 或 -o 参数指定输出文件
    $ babel example.js --out-file compiled.js
    # 或者
    $ babel example.js -o compiled.js

    # 整个目录转码
    # --out-dir 或 -d 参数指定输出目录
    $ babel src --out-dir lib
    # 或者
    $ babel src -d lib

    # -s 参数生成source map文件
    $ babel src -d lib -s
    ```
    - action
    ```
    $ cnpm install --global babel-cli

    # 提供支持es6的 **REPL**环境
    $ babel-node
    > console.log([1, 3, 5].map(index => index+1));
    [2, 4, 6]

    # 直接运行es6脚本
    $ subl test-es6.js
    > console.log([1, 3, 5].map(index => index+1));
    $ babel-node test-es6.js
    [2, 4, 6]

    # 转换代码，结果输出到标准输出
    $ babel test-es6.js
    "use strict";

    // test `$ babel-node test-es6.js`
    console.log([1, 3, 5].map(function (index) {
      return index + 1;
    }));

    # 转换代码并重定向到文件 -o (--out-file)
    $ babel test-es6.js --out-file end-es5.js

    # 转换整个代码目录 -d(--out-dir) -s(生成source map文件)
    $ babel -d build-dir source-dir
    $ mkdir test-dir
    $ cd test-dir
    $ subl a.js
    ...
    $ subl b.js
    ...
    $ babel -d end-dir test-dir -s # 等价于 babel test-dir -d end-dir
    ```
    - 项目中使用（不依赖于全局环境）
    ```
    # 安装
    $ npm install --save-dev babel-cli

    #改写package.json。
    {
      // ...
      "devDependencies": {
        "babel-cli": "^6.0.0"
      },
      "scripts": {
        "build": "babel src -d lib"
      },
    }
    # 转码执行命令。
    $ npm run build
    ```

- 钩子转码 **babel-register**

    **babel-register**模块改写`require`命令，为它加上一个钩子。此后，每当使用`require`加载`.js`、`.jsx`、`.es`和`.es6`后缀名的文件，就会先用 **Babel**进行转码。

    - 安装

        `$ cnpm install --save-dev babel-register`

    - 引用
    ```
    require("babel-register");
    require("./index.js");
    ```
    - aciton
    ```
    $ subl test-babel-register.js
    > require('babel-register');
    > require('test');
    ```
- API模块 **babel-core**

    如果某些代码需要调用Babel的API进行转码，就要使用babel-core模块。

    - 安装
        
        `$ cnpm install babel-core --save`

    - 调用API
    ```
    var babel = require('babel-core');

    // 字符串转码
    babel.transform('code();', options);
    // => { code, map, ast }

    // 文件转码（异步）
    babel.transformFile('filename.js', options, function(err, result) {
      result; // => { code, map, ast }
    });

    // 文件转码（同步）
    babel.transformFileSync('filename.js', options);
    // => { code, map, ast }

    // Babel AST转码
    babel.transformFromAst(ast, code, options);
    // => { code, map, ast }

    // 配置对象options，可以参看官方文档http://babeljs.io/docs/usage/options/
    ```
    - action
    ```
    $ subl test-babel-core.js
    var es6Code = 'let x = n => n + 1';
    var es5Code = require('babel-core')
      .transform(es6Code, {
        presets: ['es2015']
      })
      .code;

    // 运出错。。。。？？？
    ```

- 垫片 **babel-polyfill**

    Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。如果要使用这些API，需要安装 **babel-polyfill**模块，然后在所有脚本头部引入：`require('babel-polyfill');`

#### 浏览器环境

- 直接引用（临时转换，对网页性能会产生影响）
    
    从 **Babel**6.0 版本开始，不直接提供浏览器版本，需要通过构建工具构建出来。所以，为方便可以先安装5.x版本的 **babel-core** 模块获取

    ```
    $ cnpm isntall babel-core@5 --save
    $ subl test-babel-browser.html
    <head>
    <script src="node_modules/babel-core/browser.js"></script>
    <script type="text/babel">
        //Here write es6 core
        console.log([1, 3, 6].map(index => index + 1));
    </script>
    </head>
    ```

- 配合 **Browserify** 使用

    配合 **Browserify** 使用，可以生成浏览器直接加载的脚本，需要首先安装 **babelify** 模块

    `cnpm install --save-dev babelify babel-preset-es2015`

### Traceur 转码器（在线转换器被墙）