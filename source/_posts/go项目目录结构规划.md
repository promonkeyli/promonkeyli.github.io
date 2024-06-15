---
title: go项目目录结构规划
tags:
  - go
categories:
  - go
date: 2024-06-15 16:26:18
---

#### [golang社区规范](https://github.com/golang-standards/project-layout)

#### 目录结构说明

```she
/your_project
├── cmd
│   └── your_project
│       └── main.go       # 主程序入口
├── configs
│   └── config.yaml       # 配置文件
├── internal              # 内部应用代码（不可导出）
│   ├── app
│   │   ├── controllers   # 控制器层
│   │   ├── models        # 数据模型层
│   │   ├── services      # 业务逻辑层
│   │   └── repositories  # 数据访问层
│   ├── pkg
│   │   ├── logger        # 日志包
│   │   └── middleware    # 中间件
│   └── routes            # 路由定义
├── pkg                   # 可重用的包
│   ├── util              # 通用工具包
│   └── ginutil           # Gin 相关工具
├── scripts               # 脚本文件（如数据库迁移、初始化等）
├── test                  # 测试文件
├── go.mod                # Go modules 依赖管理
└── go.sum                # Go modules 依赖管理
```

#### 示例

```shel
github.com/your_username/your_project
├── cmd
│   └── your_project
│       └── main.go
├── configs
│   └── config.yaml
├── internal
│   ├── app
│   │   ├── controllers
│   │   │   └── user_controller.go
│   │   ├── models
│   │   │   └── user.go
│   │   ├── services
│   │   │   └── user_service.go
│   │   └── repositories
│   │       └── user_repository.go
│   ├── pkg
│   │   ├── logger
│   │   │   └── logger.go
│   │   └── middleware
│   │       └── auth_middleware.go
│   └── routes
│       └── router.go
├── pkg
│   ├── util
│   │   └── util.go
│   └── ginutil
│       └── ginutil.go
├── scripts
│   └── migrate.sh
├── test
│   └── user_test.go
├── go.mod
└── go.sum

```

