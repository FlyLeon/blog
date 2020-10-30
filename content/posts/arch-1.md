---
title: "Archlinux 优化之一"
date: 2020-10-22T10:17:33+08:00
tags: ["archlinux"]
categories: ["linux"]
---
archlinux 安装需联网。broadcom 4360 网卡需手动安装驱动。
# bcm 4360 网卡
```
rmmod b43 ssb
rmmod bcma
modprober wl
rmmod wl
modprober wl 
ip link
iwctl
station wlan0 get-networks
station wlan0 connect  **
```
# pgp密钥导入
yay需要pgp密钥，使用代理添加。
```
proxychains  gpg --keyserver keyserver.ubuntu.com --recv-keys **
```
# 临时增加 tmp大小
编译xanmod需要

```
mount -o remount,size=6G,noatime /tmp
```
# timeshift恢复

  *可进入命令行  
```
sudo timeshift --list
sudo timeshift --snapshot-device /dev/sdb4
sudo timeshift --restore --snapshot '2019-07-16_16-35-42' --skip-grub
```
   * 无法进入系统，制作Linux Mint 启动盘，打开 Timeshift 软件，，点击设置按钮，设置快照的存储位置。
# 加快启动速度
   关闭 watchdog

# 安装 xanmod 内核
优化桌面反应。

```
yay -S linux-xanmod linux-xanmod-header
grub-mkconfig -o /boot/grub/grub.cfg
```
# EFI STUB 启动
加速启动。
```
sudo efibootmgr --disk /dev/nvme0n1 --part 1 --create --label "Arch" --loader /vmlinuz-linux-xanmod --unicode 'root=UUID=0f949b62-98bf-4787-b8b9-1f21d0889691  rw initrd=\initramfs-linux-xanmod.img loglevel=3 quiet nowatchdog'

```

