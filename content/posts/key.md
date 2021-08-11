---
title: "签名"
date: 2021-08-11T17:42:29+08:00
categories : ["apk"]
tags : ["signed"]
draft: true
---
```
sudo apt install dfault-jre

keytool -genkey -keyalg rsa -keystore ~/.config/leon -alias leon
 
java -jar uber-apk-signer-1.2.1.jar -a ~/Downloads --ks ~/.config/leon --ksAlias leon
```
