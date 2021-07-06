# Archlinux设置窗口管理器(wm)

>最近试试在archlinux下使用titling窗口管理器，包括大名鼎鼎的`i3、bspwm、dwm、qtile`和`xmonad`，发现打开了新世界。键盘快捷键的使用极大便利了操作，提高了效率，再使用传统的窗口管理器，极不适应。而且这些管理器普遍占用资源小，快速，便捷，使用方便。不过网上可参考的中文资源很少，资源往往分布在`github、youtube`和`achlinux wiki`上，有众多网友分享自己的配置，介绍自己的操作。他们原理和操作接近，用熟了其实差别很小。大家可以跟着安装试一下，你一定会发现一个新天地，爱上它的。

## 设置Xprofile
* 设置多屏显示
使用`xrandr`设置显示。
```
# Screens
hdmi=`xrandr | grep ' connected' | grep 'HDMI1' | awk '{print $1}'`

if [ "$hdmi" = "HDMI1" ]; then
xrandr --output eDP1 --primary --mode 1920x1080 --pos 1920x0 --rotate  normal --output DP1 --off --output DP2 --off --output HDMI1 --mode 1920x1080 --pos 0x0 --rotate normal --left-of eDP1 --output HDMI2 --off --output VIRTUAL1 --off
else
xrandr --output eDP1 --primary --mode 1920x1080  --pos 0x0 --rotate normal --output HDMI1 --off --output DP1 --off &
fi
```
* 设置运行fcitx

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=\@im=fcitx

fcitx &
```
* 设置壁纸
使用`feh`来设置壁纸。在终端运行`feh --scale-bg /PATH/TO/WALLPAPER &`后，在`.Xprofile`中添加`~/.fehbg &`
* 设置透明
使用`picom`设置窗口透明，添加`picom -f'
* 使用系统状态栏
网络、蓝牙、usb挂载。
```
nm-applet &
blueman-applet &
udiskie -t &
```
* 护眼
添加`redshift &`
## lightdm使用
参照[Archlinux](),设置壁纸、主题、 图标和头像
## 设置窗口管理器
最简单的方法是直接使用别人在`github`上上传的`dotfiles`，再根据自己的情况做一些调整。我主要参考[dotfiles](https://github.com/antoniosarosi/dotfiles)设置的窗口管理器设置，st使用[LuckSmithxyx](https://github.com/LukeSmithxyz/st)的。

