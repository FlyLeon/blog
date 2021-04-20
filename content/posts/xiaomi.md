---
title: "小米盒子3增强版优化"
date: 2021-04-20T12:11:57+08:00
categories : ["小米"]
tags : ["其他"]
draft: true
---
> 近来感觉小米盒子越來越慢，于是参照网上的教程优化一下。可能这个盒子比较老了，网上的教都是很老，需要一些调整。也`root`了，桌面也换了，还冻结了大部分站资源的小米程序。优化后感觉盒子飞快，下面记录一下优化过程。

## root
- kingroot
- kingroot4.8
- supersu
## 更换桌面
- 安装`沙发轻桌面`
- 设置自启动
## 冻结程序

## 自签名程序

### 安装签名程序
```
yay -S jdk-openjdk
```
下载[Uber Apk Signer](https://github.com/patrickfav/uber-apk-signer)。
### 去掉原有签名

其实很简单，用WinRAR打开apk，找到META-INF文件夹，删除MANIFEST.MF之外的所有其他文件即可。
### 自签名
```
java -jar uber-apk-signer-1.2.1.jar --apks xxx.apk
``` 

