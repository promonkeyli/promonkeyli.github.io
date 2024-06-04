---
title: docker中nginx服务器ssl证书安装部署
date: 2024-06-04 06:01:42
tags:
  - nginx 
  - docker
categories:
  - linux
---

> <font color="#51aa38">Tip 1：</font>https需要默认开启443端口，配置前请先确认
> <font color="#51aa38">Tip 2：</font>nginx 服务器需要安装 http_ssl_module 模块

#### 一. 下载证书文件并解压,解压后有四个文件(以域名“xxx.com”为例)
* xxx.com_bundle.crt - 证书文件
* xxx.com_bundle.pem - 证书文件
* xxx.com_bundle.key - 私钥文件
* xxx.com_bundle.csr - csr文件，提供给CA机构的，安装时可忽略

#### 二. 证书以及私钥文件上传至云服务某一个目录存放（只需上传上面四个中.crt以及.key结尾的文件）
> 我的存放目录：/etc/my-nginx

#### 三. 运行nginx容器时指定nginx.conf 配置中证书以及私钥在宿主主机目录的映射
```shell
docker run -p 443:443 -v /etc/my-nginx:/etc/nginx/certs
```

#### 四. 具体ssl在nginx的配置可参考下面
```shell
server {
     #SSL 默认访问端口号为 443
     listen 443 ssl; 
     #请填写绑定证书的域名
     server_name cloud.tencent.com; 
     #请填写证书文件的相对路径或绝对路径
     ssl_certificate cloud.tencent.com_bundle.crt; 
     #请填写私钥文件的相对路径或绝对路径
     ssl_certificate_key cloud.tencent.com.key; 
     ssl_session_timeout 5m;
     #请按照以下协议配置
     ssl_protocols TLSv1.2 TLSv1.3; 
     #请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
     ssl_prefer_server_ciphers on;
     location / {
         #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
         #例如，您的网站主页在 Nginx 服务器的 /etc/www 目录下，则请修改 root 后面的 html 为 /etc/www。
         root html; 
         index  index.html index.htm;
     }
 }
server {
 listen 80;
 #请填写绑定证书的域名
 server_name cloud.tencent.com; 
 #把http的域名请求转成https
 return 301 https://$host$request_uri; 
}

```