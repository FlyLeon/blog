---
title: "GPGME  errors on archlinux"
date: 2021-09-29T15:06:19+08:00
categories : ["archlinux"]
tags : ["gpg"]
---

问题：
```
sudo pacman -Syyu
error: GPGME error: 无数据
error: GPGME error: 无数据
error: GPGME error: 无数据
```
Delete:

```
sudo rm /var/lib/pacman/sync/*db.sig*
```

Then:

```
sudo pacman-key --init

sudo pacman-key --populate archlinux

sudo pacman -Syy

sudo pacman -Syyu
```

Done!
