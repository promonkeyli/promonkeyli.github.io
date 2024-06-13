---
title: 前端开发规范梳理:husky/lint-staged/biome/commitlint
tags:
  - biome
  - husky
  - lint-staged
  - commitlint
  - commizen
  - cz-conventional-changelog
categories:
  - frontend standard
date: 2024-06-13 11:18:30
---

#### 简介

* [Husky](https://typicode.github.io/husky/)
* [Lint-Staged](https://github.com/lint-staged/lint-staged)
* [Biome](https://biomejs.dev/)
* [Commitlint](https://commitlint.js.org/)
* [Commizen](https://commitizen.github.io/cz-cli/)
* [Cz-conventional-changelog](https://github.com/conventional-changelog/conventional-changelog)

#### 规范流程

![](../images/svg/front-standard.svg)

#### 配置流程详情

1. husky

   * 作用：自定义git hooks执行后的事件处理

   * 常用的hook：pre-commit、commit-msg：pre-commit通常与lint-staged搭配使用，校验与格式化git暂存区内容；commit-msg通常与commitlint搭配校验commit msg的格式是否符合规范。

   * .husky/pre-commit hook 文件配置

     ```shell
     #!/usr/bin/env sh
     . "$(dirname "$0")/_/husky.sh"
     npx lint-staged
     ```

   * .husky/commit hook 文件配置

     ```shell
     #!/usr/bin/env sh
     . "$(dirname "$0")/_/husky.sh"
     npx --no-install commitlint --edit "$1"
     ```

2. lint-staged

   * 作用：提交代码前，对暂存区的代码做语法检测以及格式化修复使用，常搭配eslint以及prettier，最新的可以搭配biome，以下都是使用biome

   * 支持的配置文件有多种：可以放在package.json中也可单独使用配置文件，我们都使用单独的`.lintstagedrc.json`文件，保持package.json的整洁，lint-staged搭配biome的配置如下

     ```json
     {
      "*": ["biome check --apply --no-errors-on-unmatched --files-ignore-unknown=true"]
     }
     ```

   * eslint+prettier配置方法太繁琐，博主已不再使用

3. commitizen

   * 作用：命令行交互式的方式录入commit-msg：包括 type、scope、subject、short desc、long desc、break change、fix issues等

   * 配置如下：

     ```she
     # 安装
     pnpm ad d -D commitizen
     
     # 替换 git commit ===> git cz
     git cz
     ```

   * commitizen适配器：用与扩展commitizen，咋们使用`cz-conventional-changelog`适配器，常用的还有git-cz、cz-git等

   * `cz-conventional-changelog`在package.json中的配置如下：

     ```json
     "config": {
         "commitizen": {
           "path": "cz-conventional-changelog"
         }
       }
     ```

4. commitlint

   * 作用：校验commit msg是否与配置的规范一致

   * 安装：commitlint/cli（必须）、commitlint/config-conventional（可选，该npm包是已经写好的一些lint规则，可以选择不使用自己在commitlint配置文件中自定义规则）

   * 配置如下：我们直接使用默认的`commitlint/config-conventional`规则即可，使用.commitlintrc.json文件配置，也支持其他格式的配置文件，详见官方文档。

     ```json
     {
       "extends": ["@commitlint/config-conventional"]
     }
     ```

   * 规范中常用的type-enum配置如下（行业中常用合计11种，也可以定义自己团队的type）：

     |   type   |                          desc                          |
     | :------: | :----------------------------------------------------: |
     |   feat   |                       添加新功能                       |
     |   fix    |                        缺陷修复                        |
     |   perf   |                     提高性能的改动                     |
     |   docs   |                      文档内容修改                      |
     |  style   | 不影响代码含义的改动，例如去掉空格、改变缩进、增删分号 |
     |  build   |     构造工具的或者外部依赖的改动，例如webpack，npm     |
     | refactor |                        代码重构                        |
     |  revert  |                        代码回滚                        |
     |   test   |                添加测试或者修改现有测试                |
     |    ci    |              CI（持续集成服务）有关的改动              |
     |  chore   |                构建过程或辅助工具的变动                |

5. change-log

   * 作用：使用标准的commit msg生成change log

   * 配置（我们使用`conventional-changelog-cli`）：

     ```she
     # 安装
     pnpm add -g conventional-changelog-cli
     ```

     ```she
     # CHANGELOG.md 生成
     
     # 该语句不会覆盖之前的change log，向之前的文件中追加新的change
     conventional-changelog -p angular -i CHANGELOG.md -s
     
     # 第一次生成change log
     conventional-changelog -p angular -i CHANGELOG.md -s -r 0
     ```

   * npm pkg version 关联详见[官网](https://www.npmjs.com/package/conventional-changelog-cli)
