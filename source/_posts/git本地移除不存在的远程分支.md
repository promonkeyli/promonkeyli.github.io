---
title: git本地移除不存在的远程分支
tags:
  - git
categories:
  - git
date: 2024-09-12 14:57:54
---

#### 开发时间久了，本地的远程分支会很多，因此可以用下面的命令

```javascript
git branch -a // 查看所有分支
```

```javascript
git remote show origin // 查看本地分支对应的远程分支状态
```

```javascript
git remote prune origin // 删除远程没有与本地关联的所有分支
```
