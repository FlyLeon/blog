# Archlinux 优化之二

>archlinux 安装配置的文章网上很多，大多涉及源设置，输入法，字体美化，终端设置等等，但仍有很多细节缺失，为此将自己完善Thinkpad X1 carbon 7th 的过程简要记录备忘。

## BIOS 设置
   * 禁止 "Secure Boot" Security -> Secure Boot - Set to "Disabled". 
   * 节能  禁止 Config -> Thunderbolt BIOS Assist Mode - Set to "Enabled"
   * 休眠模式 Config -> Power -> Sleep State
     引用自archlinux wiki
## 安装网卡驱动 
   broadcom-wl-dkms，每次升级内核后，都自动安装。
## 音质提升
### 设置pulseaudio
编辑 ~/.config/pulse/daemon.conf
```
     default-sample-format = float32le
     default-sample-rate = 48000
     alternate-sample-rate = 44100
     default-sample-channels = 4
     default-channel-map = front-left,front-right,rear-left,rear-right
     resample-method = soxr-vhq
     enable-lfe-remixing = no
     high-priority = yes
     nice-level = -11
     realtime-scheduling = yes
     realtime-priority = 9
     rlimit-rtprio = 9
     daemonize = no
```
### 设置ALSA
编辑 ~/.asoundrc
```
     pcm!default {
     type plug
     slave.pcm hw   
    }
```
## SSD开启trim
```
sudo systemctl enable fstrim.timer
sudo systemctl start fstrim.service
```
## 升级固件版本
安装fwupd
```
sudo fwupdmgr update
```
## BOSS Q35 蓝牙连接
### 设置
编辑 vim /etc/bluetooth/main.conf  
```
# ControllerMode = dual改为ControllerMode = bredr，重启bluetooth服务。
```
### 音质提升    
```
pacmd set-card-profile 1 a2dp_sink
```
## dock
安装plank，将状态栏放于顶部
## 全局菜单
vala-appmenu

