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
安装non-free驱动iwlwifi
debian 默认ifup使
sudo vim /etc/network/interfaces
/etc/network/interface.d
ip a

/etc/resolv.conf




### 安装dwm
- 安装xorg
- 编译dwm
  安装编译依赖libxft-dev libx11-dev libxinerama-dev 
- 设置.xsessionrc
```
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"
export XMODIFIERS="@im=fcitx"
fcitx -r &
redshift &
feh --bg-fill ~/Downloads/sea-winter.jpg &
compton
exec dwm
```
### 安装fcitx
fcitx-bin fcitx-frontend-all
字体安装.fonts中
ubuntu nerd font 
wqy

### 开启vaapi 
#### 编译驱动
archlinux  intel-driver-g45-h264.tar.gz
./configure
libdrm-dev
libva-dev
sudo make clean install
30-40 20
#### 设置chromium
chrome://flags
acce
gpu
chrome://gpu
### 安装wps
www.wps.com
www.wps.cm
mui目录
字体安装
### 声音驱动
### 杂项
   壁纸
   feh
   背景透明
   comtpon
