---
title: "我使用过的wm"
date: 2021-02-06T21:29:02+08:00
categories : ["linux"]
tags : ["wm"]
draft: true
---
>最近试试在linux下使用title窗口管理器，包括大名鼎鼎的i3、bspwm、dwm、qtile和xmonad，发现打开了新世界。键盘快捷键的使用极大便利了操作，提高了效率，再使用传统的窗口管理器，极不适应。而且这些管理器普遍占用资源小，快速，便捷，使用方便。不过网上可参考的中文资源很少，资源往往分布在github、youtube和achlinux wiki上，有众多网友分享自己的配置，介绍自己的操作。他们原理和操作接近，用熟了其实差别很小。大家可以跟着安装试一下，你一定会发现一个新天地，爱上它的。

## 善用Xprofile
* 设置多屏显示
`xrande`
* 设置运行fcitx
```
export PATH=$HOME/.local/bin:$PATH
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=\@im=fcitx

fcitx &
```
* 设置壁纸
`feh --scale-bg /PATH/TO/WALLPAPER &`
* 设置透明
`picom -f'
* 运行系统状态栏项目
```
nm-applet &
blueman-applet &
udiskie -t &
```
* 运行`redshift &`
## lightdm 使用
* 设置壁纸
* 主题
* 图标
* 头像
## 多显示器设置
## i3
## bspwm 
## dwm 
## xmonad
