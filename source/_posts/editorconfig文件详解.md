---
title: .editorconfig文件详解
tags:
  - editor
categories:
  - tools
date: 2024-06-08 13:33:03
---

#### [editorconfig](https://editorconfig.org/) 作用

> 让同一项目在不同编辑器下格式保持一致（例如：缩进、换行符等）

#### 原理

> 支持的编辑器在打开项目时会读取该文件，应用到代码中，同时与版本控制完美契合

#### 配置内容解读

```
# 顶级 EditorConfig 文件
root = true

# Unix 风格的换行符，并在每个文件末尾添加一个换行符
[*]
end_of_line = lf
insert_final_newline = true

# 使用大括号扩展符号匹配多个文件
# 设置默认字符集
[*.{js,py}]
charset = utf-8

# 使用 4 个空格缩进
[*.py]
indent_style = space
indent_size = 4

# 使用制表符缩进（未指定大小）
[Makefile]
indent_style = tab

# 重写 lib 目录下所有 JS 文件的缩进
[lib/**.js]
indent_style = space
indent_size = 2

# 精确匹配 package.json 或 .travis.yml 文件
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2

```
