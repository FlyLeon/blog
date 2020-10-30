---
title: "VPS配置之二Nginx"
date: 2020-10-27T17:10:45+08:00
categories:  ["linux"]
tags : ["vps"]
---
>nginx是常用服务器软件之一，高效，快速。我们的VPS需要它来建立网站、通过反向代理使用各种服务。从稳定性考虑，我的VPS使用Centos 7操作系统，而且它的网上资源也很丰富，便于搜索学习。

# 安装nginx
* 安装EPEL仓库
```
sudo yum install epel-release
```
* 安装nginx
```
sudo yum install nginx
```
* 启动nginx
```
sudo systemctl start nginx
sudo systemctl enable nginx
```
* 设置防火墙
```
sudo firewall-cmd --permanent --zone=public --add-service=http 
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```
# 设置nginx
* v2raya

* filebrowser反向代理

* frps反向代理
