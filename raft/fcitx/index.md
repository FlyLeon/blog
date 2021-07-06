# wanland下使用Fcitx输入中文

>fcitx是linux下常用的输入法，如何在wayland下使用，网上却缺乏准确的说法。实际上，fcitx在wayland下还是可以使用的，下面是我在wayland下的实践，主要是一些关键要点，以供参考。 
## 安装 
这里我们只使用拼音，只需安装fcitx即可，另外为方便设置可安装`fcitx-configtool`，使用图形界面设置。
```
sudo pacman -S fcitx fcitx-configtool
```
## 设置
fcitx在linux下的设置比较繁琐，设置的项目多，稍有不慎，就可能出错，出现问题要逐项进行排查。
### fcitx设置
首先打开`fcitx-configtool`,添加键盘。
- 首先在[输入法]中选择你要用的输入法，按下面加号添加键盘，放在第一列，英文输入键盘可删除。
- 在[设置]中选择输入提示字体和大小。
### 设置环境变量
wayland是在.pam_environment中设置。要设置桌面字体为中文，否则fcitx是无法添加中文输入法的。其次设置GTK、QT和im均使用fcitx输入。
```
LANG=zh_CN.UTF-8

GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
```
### urxvt
* 一定要安装aur中的`urxvt-unicode-truecolor`来支持中文和真彩色。
* 在wayland中，urxvt可通过.Xdefaults设置。
```
URxvt.preeditType:Root      
URxvt.inputMethod:fcitx
```       
* 如仍无法切换输入模式可尝试重装fcitx，我的就是>这样解决的，`sudo pacman -S fcitx`
### 添加自启动
可在sway中设置fcitx自启动。
```
exec --no-startup-id  fcitx -r
```
## 系统图标使用
如使用swaybar，我们需要设置系统托盘图标，否则会显示红脸，切换为汉字后无法返回。
* 查看图标文件
```
find /usr/share/icons/ -name 'input-keyboard.*'
```
* 设置主题
为正确显示图标我们需要在sway设置文件bar部分增加主题设置。
```
icon_theme Adwaita
```
{{< admonition >}}
如你觉得Adwaita的键盘图标不好看，可安装其他主题，更换图标，也可以仅替换/usr/share/icons/Adwaita/32x32/legacy/input-keyboard.png文件。
{{< /admonition >}}


