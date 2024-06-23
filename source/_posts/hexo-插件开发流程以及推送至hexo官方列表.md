---
title: hexo 插件开发流程以及推送至hexo官方列表
tags:
  - hexo
  - plugins
categories:
  - hexo
date: 2024-06-23 14:11:49
---

### Hexo插件分类

|  插件类别  |                           插件作用                           |
| :--------: | :----------------------------------------------------------: |
|  渲染插件  | 界面渲染逻辑处理，例如：`hexo-renderer-ejs`、`hexo-renderer-pug` |
|  部署插件  |                         静态文件发布                         |
|  分析插件  |              数据分析：例如文章字数、阅读时间等              |
|  SEO插件   |                       搜素引擎优化插件                       |
| 生成器插件 |       生成特定的页面或者文件：例如标签页面、分类页面等       |
| 过滤器插件 | 过滤器插件在 Hexo 的生命周期的各个阶段对内容进行处理。例如，添加代码高亮、处理图片等。 |
|  显示插件  | 显示插件提供辅助功能，通常用于模板或布局中，例如生成导航菜单、分页等。 |
|  模版插件  |    模板插件提供主题和样式相关的功能，帮助美化博客的外观。    |
|  扩展插件  |  扩展插件增加或修改 Hexo 的核心功能，提供额外的特性和功能。  |
|  数据插件  | 数据插件用于处理外部数据源，例如从数据库或 API 获取数据，并在博客中展示。 |

#### Hexo插件使用

##### 安装插件

```shell
npm i hexo-create-issues
```

##### 配置插件

```yml
# _config.yml 文件
plugins:
  - hexo-create-issues # 你想要使用的插件名，需要安装依赖后使用
```

#### Hexo插件开发

> Tips 1: 建议使用commonjs方式编写，新的一些package大都是esmodule编写，commonjs中引入需要动态导入import()

> Tips 2: Hexo 插件需要以`hexo-`前缀开头，同时不能重名

##### 脚本模式开发

1. hexo工程目录下新建scripts文件夹
2. 根据需求编写对应脚本

##### npm包形式开发

1. 新建项目，按照npm包开发流程即可

2. 项目依赖安装，其他需要的依赖自行安装，hexo是必须的

   ```shell
   npm install hexo
   ```

3. 根据需求编写脚本

   * `hexo.on()` 可以监听hexo执行的生命周期钩子，可以编写相关逻辑

   * `hexo.config`可以获取`_config.yml`中的所有配置项，逻辑中可以调用获取

 4. 本地联调：建议使用`yalc`，具体使用方式可以参考博主的这篇文章，[yalc使用](/2024/06/23/使用yalc作为你的本地依赖管理工具/)

1. 调试完成，发布至npm

   ```shell
   npm login
   npm publish
   ```

2. 项目中使用：安装发布的npm包后，_config.yml中再配置一下即可(`plugins`以及所需`变量`配置)

#### Hexo插件发布至hexo列表

1. 需要先发布npm包

2. 然后fork [hexojs/site](https://github.com/hexojs/site)

3. clone fork后的仓库到本地

4. 在clone后的项目中的`source/_data/plugins/` 中创建一个新的 yaml 文件，使用您的插件名称作为文件名

5. 编辑yaml文件，按需求录入以下内容

   ```yaml
   description: Server module for Hexo.
   link: https://github.com/hexojs/hexo-server
   tags:
     - official
     - server
     - console
   ```

6. push修改，再向hexo site官方提交一个pull request，请注意提交的commit msg格式规范
