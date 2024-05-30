---
title: ubuntu安装docker
tags:
  - docker
  - linux
  - ubuntu
categories:
  - docker
date: 2024-05-24 09:29:29
---

#### 查看是否安装wget
```shell
which wget
```
#### 没有安装，升级包管理器再安装
```shell
sudo apt-get update
sudo apt-get install wget
```
#### 安装docker
```shell
wget -qO- https://get.docker.com/ | sh
```
#### 验证是否安装成功
```shell
sudo docker run hello-world
```