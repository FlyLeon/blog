---
title: "安卓程序签名"
date: 2021-10-11T14:56:35+08:00
categories : ["安卓"]
tags : ["sign"]
---
> 下面讲的是如何为我们的删除现有Apk签名并重新签名，我们后续的教程使用的是'Uber Apk Signer'。

## 去除签名
- 将后缀apk改为zip
- 删除签名 
 
删除目录中'META_INF"目录除'MAIFEST.MF'文件。

- 改回后缀

## 生成自签名
'keytool -genkey -alias my_alias -keyalg RSA -validity 20000 -keystore /path/yuorrname.keystore'

## 下载'Uber Apk Signer'      
'git clone --depth=1 https://github.com/patrickfav/uber-apk-signer.git'

## 自签名
'java -jar uber-apk-signer.jar -a /path/to/apks --ks /path/yourname.keystore' --ksAlias my_alias'
