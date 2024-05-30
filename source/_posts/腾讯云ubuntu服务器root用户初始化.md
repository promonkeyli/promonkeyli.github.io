---
title: 腾讯云ubuntu服务器root用户初始化
tags:
  - linux
  - ubuntu
categories:
  - linux
date: 2024-05-30 14:14:31
---

> 注：腾讯云ubuntu安装过程默认不设置root用户名以及密码，需要手动开启root用户登录

1. 使用初始化用户名密码（默认是ubuntu）登录云服务器
2. 执行下面的命令，设置root密码

```shell
sudo passwd root
```

3. 执行下面的命令，修改ssh配置文件

```shell
sudo vim /etc/ssh/sshd_config
```

4. 按i切换到编辑模式，找到#Authentication，将PermitRootLogin 修改为yes，然后保存

```shell
PermitRootLogin yes # 默认值是 prohibit-password
```

5. 重启ssh服务后，root用户登录验证配置是否生效

```shell
sudo service ssh restart
```

