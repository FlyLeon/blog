---
title: "黑苹果Thinkpad X1 7th carbon安装Big Sur"
date: 2020-11-18T15:48:55+08:00
categories : ["mac"]
tags : ["bigsur"]
draft: true
---
> 我的笔记本电脑是Thinkpad X1 7th carbon，使用EFI启动，同时安装了windiws10，Big Sur和archlinux三套系统。其中黑苹果的安装最为繁琐，OpenCore安装完成使用还是很流畅。显示、声音、屏幕背光、电源管理和声音快捷键是最容易出问题的地方，这里简单记录备忘。
 
主要参考:
* [使用OpenCore引导黑苹果](https://blog.xjn819.com/post/opencore-guide.html)
* [OpenCore Vanilla Guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/)
* [精解OpenCore](https://blog.daliansky.net/OpenCore-BootLoader.html)
* [OpenCore 0.5+ 部件补丁](https://github.com/daliansky/OC-littl)
* [macOS on Thinkpad X1 Carbon 6th Generation, Model 20KH](https://github.com/tylernguyen/x1c6-hackintosh)

## 我的配置
|CPU|内存|硬盘|无线网卡|声卡|显卡|显示|
|---|----|----|--------|----|----|----|
|i5-8250U|8G|Samsung EVO plus 1T|BCM3260|ALC285|HD 620|1920*1080|
## 未实现功能

主要有Thunderbolt 3、指纹、SD卡读取等，自己平时也用不上，有些网上有实现方法，就懒得能了。

## apci
|名称|功能|
|----|----|
|SSDT-ALS0.aml|
|SSDT-DMAC.aml|
|SSDT-DTPG.aml|
|SSDT-EXT3-LedReset-TP.aml|
|SSDT-EXT4-WakeScreen.aml|
|SSDT-GPRW.aml|
|SSDT-HPET.aml|
|SSDT-Keyboard-X1C6.aml|
|SSDT-MCHC.aml|
|SSDT-OCBAT0-TP_re80_tx70-80_x1c5th-6th_s12017_p51.aml|电池|
|SSDT-PLUG-_PR.PR00.aml|
|SSDT-PMCR.aml|
|SSDT-PNLF-R.aml|
|SSDT-PTSWAK.aml|
|SSDT-PWRB.aml|
|SSDT-SBUS.aml|
|SSDT-XOSI.aml|
## kext
|名称|功能|
|----|----|
|AppleALC.kext|声卡驱动|
|CPUFriend.kext|CPU调频驱动|
|CPUFriendDataProvider.kext|CPU调频数据|
|HibernationFixup.kext|休眠补丁|
|IntelMausiEthernet.kext|有线网卡|
|Lilu.kext|Acidanthera驱动全家桶|
|NVMeFix.kext|NVMe节电补丁|
|SMCBatteryManager.kext|电池传感器|
|SMCLightSensor.kext|光传感器|
|SMCProcessor.kext|CPU传感器|
|SMCSuperIO.kext|IO传感器|
|VirtualSMC.kext|传感器驱动依赖|
|USBPorts.kext|USB定制补丁|
|VoodooPS2Controller.kext|键盘鼠标驱动|
|WhateverGreen.kext|显卡驱动｜
## drivers
|名称|功能|
|----|----|
|ApfsDriverLoader.efi|读取apfs分区|
|OpenRuntime.efi|必需插件|
## config.plist

