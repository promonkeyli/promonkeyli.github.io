---
title: ubuntu中非容器化mysql安装
tags:
  - ubuntu
  - linux
  - mysql
categories:
  - mysql
  - ubuntu
date: 2024-07-02 21:15:32
---

### 安装(mysql8.0)

升级源并安装mysql8.0

```shell
sudo apt update
sudo apt install mysql-server
```

安装后，mysql服务会自己启动，验证方式如下

```shell
sudo systemctl status mysql
```

### 运行安全安装脚本

MySQL 安装文件附带了一个名为`mysql_secure_installation`的脚本，它允许你很容易地提高数据库服务器的安全性。

```shell
sudo mysql_secure_installation
```

你将会被要求配置`VALIDATE PASSWORD PLUGIN`，它被用来测试 MySQL 用户密码的强度，并且提高安全性：

```shell
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 2
```

如果你设置了验证密码插件，这个脚本将会显示你的新密码强度。输入`y`确认密码：

```shell
Estimated strength of the password: 50 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
```

下一步，你将被要求移除任何匿名用户，限制 root 用户访问本地机器，移除测试数据库并且重新加载权限表。你应该对所有的问题回答`y`。

### 登录mysql

密码是运行安全安装脚本时设置的密码

```shell
sudo mysql -u root -p
```

