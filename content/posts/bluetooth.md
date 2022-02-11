---
title: "Big sur、windows10、archlinux三系统配置Bluetooth"
date: 2021-03-16T09:43:29+08:00
categories : ["linux,windows,mac"]
tags : [bluetooth]
---
> 我在`TkinkPad X1`上同时安装了`windows`、`Archlinux`和`Big Sur`，三个系统可共用蓝牙耳机。首先在三个系统分别连接蓝牙耳机，其次在 `Big Sur`中导出连接消息，查看连接密码。再次修改`windows`和`Archlinux`连接密码。注意`windows`和`Archlinux`连接密码`Big Sur`连接密码从后向前每两位为一组颠倒，同时`Archlinux`的连接密码字母需大写。
- 蓝牙耳机分别配对`windows`、`archlinux`和`mac`
- 蓝牙耳机配对`MAC`
* 下载`Hacktool`;
* 运行`Hacktool`-`工具`备份蓝牙密钥； 
- 修改`winodows`蓝牙密钥
* 下载`psexec`;
* 使用管理员权限运行`cmd`;
* 使用管理员权限运行`psexec si regedit`
打开`ControlSet001\Services\BTHPORT\Parameters\你的蓝牙mac地址`
- 修改`linux`蓝牙密钥
* 使用`su`进入超级用户。
* 进入`cd /var/lib/bluetooth/你的蓝牙mac地址`，运行`vim info`。
* 修改`LinkKey`下`key`。 

参考[Archlinux Wiki](https://wiki.archlinux.org/index.php/Bluetooth)
