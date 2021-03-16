---
title: "Debian桌面使用安装配置"
date: 2021-03-16T08:42:16+08:00
categories : ["linux"]
tags : ["debian"]
draft : true
---
> 笔记本一直使用archlinux，但有很长时间未更新，更新后挂了，于是想试试号称最稳定的debian。但网上如何桌面使用的文章很少。以下是我安装debian遇到的坑，简单记录。

## 安装
使用网络安装，网络一定要能科学上网环境。否则最后一步安装grub会失败，因为无法下载grub相关文件。安装中可手动设置网络，可先选择dhcp，到磁盘设置时返回选择网络配置。安装可选择中国，选择中科大(ustc)源。软件可只选择安装，因为我们会自己安装dwm。
## 安装后配置
### 配置中科大源
中科大源可参考网站配置。
### 配置无线网络
debian 默认使wpa_supplicant管理网络。iwd

/etc/network/interface.d

/etc/resove.conf

### 安装dwm
- 安装xorg
- 安装
- 

### 安装fcitx
fcitx-bin fcitx-
