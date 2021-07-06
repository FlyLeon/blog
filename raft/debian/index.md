# Debian桌面使用安装配置

> 笔记本一直使用`archlinux`，但有很长时间未更新，更新后挂了，于是想试试号称最稳定的`debian`。但网上如何桌面使用的文章很少。以下是我安装debian遇到的坑，简单记录。

## 安装
使用有线网络安装。网络一定要能科学上网环境。否则最后一步安装grub会失败，因为无法下载grub相关文件。安装中可手动设置网络，可先选择dhcp，到磁盘设置时返回选择网络配置。安装可选择中国，选择中科大(ustc)源。软件可只选择安装`standard system utilities`，因为我们会自己安装dwm。
## 安装后配置
### 配置中科大源
中科大`non-free`源可参考其网站配置。
### 配置无线网络
`Intel Wireless`需要先安装`non-free`驱动。
```
sudo apt-get update 
sudo apt-get install firmware-iwlwifi
```
debian 默认使用ifup设置。
- 编辑`sudo vim /etc/network/interfaces`
```

allow-hotplug wlan0
iface wlan0 inet static
```
- 编辑/etc/network/interface.d具体网络
使用`ip a`查看可用网络名。编辑网络名文件。
```
address xxx.xxx.xxx.xxx/24
gateway xxx.xxx.xxx.xxx
DNS     xxx.xxx.xxx.xxx
wpa-ssid your_ssid
wpa-psk your_password


```
- 设置DNS
编辑`sudo vim /etc/resolv.conf`。
### 安装dwm
- 安装xorg
- 安装依赖 
  安装`gcc make libxft-dev libx11-dev libxinerama-dev `。
- 设置字体
```
static const char *fonts[] = { "Hack Nerd Font:size=12:weight=bold:
 antialias=true:autohint:true","WenQuanYi Micro Hei Mono:size=12" };
```
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
安装`fitcx fcitx-frontend-all`。
下载`hack nerd font`，解压于~/.fonts。 
下载`font-wqy-microhei xfonts-wqy`
更新字体`fc-cache -vf`
### 开启vaapi 
#### 编译驱动
使用archlinux的intel-driver-g45-h264.tar.gz解压编译。
./configure
安装`libdrm-dev libva-dev`，进入解压目录。
```
./configure
sudo make clean install
```
#### 设置chromium
- 设置chrome://flags
`Hardware-accelerated video decode=enabled`
- 设置gpu
```
GPU rasterization=enabled
Zero-copy rasterizer=enabled
```
- 查看chrome://gpu
### 安装wps
- 下载`wps`
分别于`www.wps.com`和`www.wps.cn`下载英文和中文wps安装包，安装英文版。
- 解压中文`wps`,`dpkg -X wps.tar.gz wps`
- 复制`opt`下中文`mui`目录
- `wps`字体安装
### 声音驱动
分别安装`ALSA`和`pluseaudio`驱动。
### 杂项
使用`feh`设置壁纸使用`compon`背景透明。

