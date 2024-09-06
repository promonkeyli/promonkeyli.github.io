---
title: bun局部设置国内镜像源
tags:
  - bun
categories:
  - bun
date: 2024-09-06 09:39:54
---

1. 前端项目根目录下添加==bunconfig.toml==

2. 新增如下配置：

```toml
[install]
# 添加淘宝源（注意旧的淘宝源http://registry.npm.taobao.org已不再更新）
registry = "https://registry.npmmirror.com"
```

