# wayland下urxvt使用

> urxvt小巧快速，但缺乏缺省设置，设置起来比较麻烦，但设置好了，还是很好用的，而且只有它通过xwayland使用w3m支持ranger在wayland下中预览图像。
## fcitx支持
```
URxvt.preeditType:Root
URxvt.inputMethod:fcitx
```
{{< admonition tip>}}
* 一定要安装aur中的`urxvt-unicode-truecolor`来支持中文和真彩色。
* 在wayland中，urxvt可通过.Xdefaults设置，以下设置均在其中。
* 慎用URxvt.letterSpace设置字间距，在我机器上这为不同字体设置带来问题。
{{</admonition >}}

## 使用守护进程模式
urxvt可采用守护模式，使用urxvtc启动终端，节省内存，更加快速。这里采用在sway内启动urxvtd守护进程。
```
exec urvxtd -q -f
```
{{< admonition >}}
推荐sway内启动守护进程，可以采用桌面的语言设置，避免出现乱码。
{{< /admonition >}}
## 配色
使用Dracula配色
```
! Dracula Xresources palette
*.foreground: #F8F8F2
*.background: #282A36
*.color0:     #000000
*.color8:     #4D4D4D
*.color1:     #FF5555
*.color9:     #FF6E67
*.color10:    #5AF78E
*.color3:     #F1FA8C
*.color11:    #F4F99D
*.color4:     #BD93F9
*.color12:    #CAA9FA
*.color5:     #FF79C6
*.color13:    #FF92D0
*.color6:     #8BE9FD
*.color14:    #9AEDFE
*.color7:     #BFBFBF
*.color15:    #E6E6E6
```
## 背景透明
括号数字内为透明度。
```
URxvt.background:[80]#282A36
```
## 复制粘贴
使用linux缺省复制粘贴方式。
```
!! copy & paste
URxvt.keysym.Shift-Control-V: eval:paste_clipboard
URxvt.keysym.Shift-Control-C: eval:selection_to_clipboard
URxvt.keysym.Control-Meta-c: builtin-string:
URxvt.keysym.Control-Meta-v: builtin-string:
URxvt.iso14755: false
URxvt.iso14755: false
URxvt.iso14755_52: false
```
## 字体设置
我使用的是Hack。
```
URxvt.font:xft:Hack:style=Regular:antialias=True:pixelsize=23
URxvt.boldFont:xft:Hack:style=Bold:antialias=True:pixelsize=23
```
{{< admonition  tip >}}
字体名与安装软件包名一般并不一致，多为字体名首字母大写，可多试几次。
不是所有字体在urxvt中都可使用。
如archlnux中Hack字体，安装使用
```
yay -S ttf-hack
```
查看字体使用`fc-list | grep hack `并无结果。要使用Hack查询，`fc-list | grep Hack`，结果显示;
```
/usr/share/fonts/TTF/Hack-Italic.ttf: Hack:style=Italic
/usr/share/fonts/TTF/Hack-Bold.ttf: Hack:style=Bold
/usr/share/fonts/TTF/Hack-Regular.ttf: Hack:style=Regular
```
系统内字体名为style前名称，即Hack。 
{{< /admonition >}}




