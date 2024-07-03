---
title: ubuntu中容器化mysql安装使用
tags:
  - ubuntu
  - linux
  - mysql
categories:
  - mysql
  - ubuntu
date: 2024-07-02 22:48:03
---

### Dockerfile编写

1. 随便新建一个文件夹(比如是：mysql文件夹)，使用`vim Dockerfile`并添加下面的内容

```dockerfile
# 使用官方MySQL 8.0镜像作为基础镜像
FROM mysql:8.0

# 设置环境变量
ENV MYSQL_ROOT_PASSWORD=mypwd
ENV MYSQL_DATABASE=mydb

# 拷贝初始化脚本到docker-entrypoint-initdb.d目录
COPY ./init.sql /docker-entrypoint-initdb.d/

# 暴露MySQL端口
EXPOSE 3306

```

2. 在mysql文件夹下，使用`vim init.sql`并添加下面的内容

```sql
-- 创建数据库（如果需要）
CREATE DATABASE IF NOT EXISTS mydb;
-- 选择要使用的数据库（如果需要）
USE mydb;
-- 你可以添加更多的SQL语句来初始化数据库，表结构等。
```

### 镜像构建&运行

在新建的mysql目录下执行下面的命令

```shel
sudo docker build -t my-mysql:8.0 .
```

运行该镜像

```shell
sudo docker run --name mysql-container -d -p 3306:3306 my-mysql:8.0
```

### 验证安装

```she
sudo docker exec -it mysql-container mysql -u root -p
```

### 数据库连接

比如`Navicat`远程连接该数据库，博主此处mysql是装在云服务器上的，所以可以使用ip端口号密码就可以连接；需要注意的是，服务器上一般不会默认开发`3306`端口，所以使用`Navicat`测试连接会失败，所以需要去对应的服务器控制台，开放对应的端口规则，访问才能生效
