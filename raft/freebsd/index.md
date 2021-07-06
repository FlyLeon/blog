# Thinkpad T400上安装配置Freebsd 12

> 最近把一台T400的操作系统从Archlinux换成freeBSD，发现freeBSD更稳定，资源占用也更低。可能是使用人少的缘故，网上中文资源很少，也不准确。好在英文资料还不少，也很详细，参照安装也不难。下面简要记录自己走过的弯路，下面命令均需root权限。

## 安装
### 安装iso选择

由于众所周知的原因，境外freebsd源速度极慢，使用bootonly.iso基本无法安装。我下载使用下载amd64-dvd1.iso来安装，安装过程中无需上网下载。
### 网络配置

安装完成可配置网络，可以选择配置ipv4、dhcp或手动配置ip。
### 使用国内源

#### 更换pkg源

- 编辑/usr/local/etc/pkg/repos/FreeBSD.conf

禁用系统级 pkg 源。
```
mkdir -p /usr/local/etc/pkg/repos
```
替换其中url为`pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/latest`

-禁用系统级 pkg 源
```
mv /etc/pkg/FreeBSD.conf /etc/pkg/FreeBSD.conf.back
pkg update -f
```
#### 更换update源
```
sed -i '' 's/ServerName update.FreeBSD.org/ServerName update.freebsd.cn/g' /etc/freebsd-update.conf
```
FreeBSD 安全补丁可以通过以下命令下载并安装补丁。
```
freebsd-update fetch
freebsd-update install #安装更新
pkg audit -F  #查找所有软件包是否有漏洞补丁（安全审计）
```
#### freeBSD国内源
```
ports mirror at ports.freebsd.cn

portsnap mirror at portsnap.freebsd.cn

update mirror at update.freebsd.cn

pkg mirror at pkg.freebsd.cn
```
## 桌面配置

参照[FreeBSD on a Laptop](https://www.c0ffee.net/blog/freebsd-on-a-laptop/)
### 编译使用suckless软件

`dmenu、dwm、st、slstatus`均可在freeBSD中使用，但需按照以下修改`config.mk`，以在freeBSD下编译。将X11INC和X11LIB路径中的`X11R6`改为`local`，把freetype2的路径改为`/usr/local/bin/`,安装`pkgconf`，`pkg install pkfconf`，将`PKG_CONFIG`中`pkg-config`改为`/usr/local/bin/pkgconf`。需使用github上LukeSmith修改的st，可以背景透明，显示systray和输入中文。
### 中文输入 

安装`zh-fcitx`，使用`zh-fcitx-configtool`配置，但无法配置具体的输入法，直接使用linux下配置。配置文件位置为.config/fcitx。

### 字体
```
pkg install wqy-fonts nerd-font
xset fp+ /usr/local/share/font/wqy
xset rehash
```
### 其他设置
在`.xinitrc`中使用`picom`设置背景透明，要把用户加入`video`组，`pw addgroup video -m 用户`，`feh`设置壁纸，`slim`登录。
## 声音设置

### 解决音量小问题

安装`mixer`调整音量，安装`sndio`改善音质。
`mixer` 显示各设备音量，`mixer -s vol 100`设置音量为`100`。

### 设置5.1声音
编辑`/etc/sysctl.conf`
```
dev.pcm.0.play.vchanformat=s16le:5.1
```
### 设置音量调节按键
修改dwm的`config.h`。
```
{0, XF86XK_AudioLowerVolume, spawn, SHCMD("mixer vol -5")},
{0, XF86XK_AudioRaiseVolume, spawn, SHCMD("mixer vol +5 ")},
{0, XF86XK_AudioMute, spawn, SHCMD("mixer -s 0")},
```
###  输出自动切换
编辑`/etc/sysctl.conf
```
hw.snd.degault_auto=2
```

## 升级

使用freebsd-update升级FreeBSD操作系统使用freebsd-update升级FreeBSD操作系统。
### 更新到最新补丁版本
```
freebsd-update fetch
freebsd-update install
```
### 升级版本
```
freebsd-update upgrade -r 13.0-BETA3
freebsd-update install
```
### 重新引导到新内核并继续安装.
```
reboot
freebsd-update install
```
### 重新构建pkg程序，完成升级
```
pkg-static upgrade -f
```

## chromium开启硬件加速

### 开启flags
- `chrome://flags/#ignore-gpu-blocklist`
- `chrome://flags/#enable-accelerated-video-decode`
### 安装vaapi驱动
`sudo pkg install libva-intel-driver` 
### 为不支持VP9显卡安装[h265ify](https://chrome.google.com/webstore/detail/h264ify/aleakchihdccplidncghkekgioiakgal)拓展
### 启动浏览器使用`--use-gl=desktop`标志

