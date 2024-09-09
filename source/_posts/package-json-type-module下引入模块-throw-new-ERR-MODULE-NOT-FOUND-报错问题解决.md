---
title: package.json type module下引入模块 throw new ERR_MODULE_NOT_FOUND 报错问题解决
tags:
  - js-module
categories:
  - node
date: 2024-09-09 13:12:35
---

#### 前言

1. 目前js常用的两种模块系统分别是：CommonJs以及**ES Module**（推荐使用）
2. 在 CommonJS 模块系统中，模块的导出和引用使用 **module.exports** 和 **require**，这是 Node.js 的默认模块系统（如果 package.json 没有指定 "type": "module"）
3. 在ES Module中则使用我们目前常用到的export，import语法

#### 问题描述

1. 如果明确指定了package.json中**type="module"**的话，引用文件在导入时需要添加后缀，示例如下：

```js
// index.js
import xx from "xx" // 错误写法，node执行会抛出 ERR_MODULE_NOT_FOUND 错误
import xx from "xx.js" // 正确写法
```

2. 如果是在**TypeScript**项目中引用，开发阶段不编译的情况下也是可以直接引入**js**后缀文件，示例如下：

```js
// index.ts
import xx from "xx.js" // 可以使用xx.js写法引入xx.ts文件，代码是可以正常运行的
```

#### 原因

**ES**标准要求，在 **Node.js** 中，当你设置了 `"type": "module"`，要求开发者明确指定文件的路径和扩展名，这是为了遵循 ES 模块标准，与浏览器行为保持一致，并减少模块解析的歧义，与 **CommonJS** 的模块解析机制不同，Node.js 不会自动推断文件的扩展名

