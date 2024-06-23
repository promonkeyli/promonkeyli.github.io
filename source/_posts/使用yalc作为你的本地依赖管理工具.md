---
title: 使用yalc作为你的本地依赖管理工具
tags:
  - yalc
  - npm
categories:
  - npm
date: 2024-06-23 15:19:34
---

#### yalc介绍

该工具用于本地包开发联调，以前常用的是`npm link`，但是npm link调试方式相对繁琐，yalc简直是本地开发包的神奇，简单高效

#### yalc使用

##### 全局安装

```shell
npm i yalc -g
```

##### 使用

1. 开发包下执行该命令，发布该依赖

   ```shell
   yalc publish
   ```

2. 需要使用该包的项目中，添加需要调试的包

   ```shell
   yalc add xxx
   ```

3. 开发的包有更新的话，执行该命令，全局add了该包的项目都会更新包内容

   ```shell
   yalc push 
   ```

4. 移除依赖

   ```shell
   yalc remove xxx
   ```

   

