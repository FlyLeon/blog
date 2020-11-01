# Archlinux使用Wayland和sway

>根据维基百科，Wayland是一个通信协议，规定了显示服务器与其客户机之间的通信方式，而使用这个协议的显示服务器称为Wayland Compositor。是不是很不好理解，我们只需知道它可以代替xorg就可以了。sway是wayland上的Title Compositor，类似于xorg下的i3，设置基本相同，方便使用快捷键来操作。以下设置主要参考[archlinux wiki](https://wiki.archlinux.org/index.php/wayland)。

# wayland和sway安装
* 安装
```
sudo pacman -S wayland sway xorg-server-xwayland
```
* 设置环境变量
与 Xorg不同，wayland 使用.pam_environment设置环境变量。
```
XDG_CURRENT_DESKTOP=Unity          //桌面环境
LANG=zh_CN.UTF-8                      //语言设置
 
INPUT_METHOD  DEFAULT=fcitx       //fcitx设置
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
 
GDK_DPI_SCALE=1.65                   //hidpi 设置
```
# sway 设置
* 终端设置
```
set $term  kitty
```
* bemenu 设置
wayland使用bemenu快速启动程序
```
set $menu dmenu_path | dmenu | xargs swaymsg exec bemenu -b
```
* 护眼程序redshift
```
exec "kitty -e nohup redshift 2>&1
```
* 输出背景和分辨率
```
output eDP-1 {
        background ~/Downloads/2.jpg  fill
        resolution 1920x1080
            }
```
* 屏保
```
exec swayidle -w \
           timeout 300 'swaylock -f -c 000000' \
           timeout 600 'swaymsg "output * dpms off"' resume 'swaymsg "output * dpm    s on"' \
           before-sleep 'swaylock -f -c 000000'
```
* 程序快捷键绑定
```
bindsym $mod+d exec "wofi --show run"
bindsym $mod+Shift+f exec "kitty -e ranger"
```
* 缺省颜色
``` 
set $bgcolor    #00000033
set $ibgcolor   #4c7899
set $fws        #00000080
set $barcolor   #0000000D
set $textcolor  #ffffff
set $itextcolor #969696
set $ubgcolor   #ff0000
```
```
#       border      backgroud   text    indicate
client.focused   $bgcolor   $bgcolor      $textcolor  $bgcolor
client.unfocused  $ibgcolor $ibgcolor   $itextcolor $ibgcolor
client.focused_inactive  $ibgcolor $ibgcolor  $itextcolor $ibgcolor
client.urgent     $ubgcolor  $ubgcolor  $textcolor $ubgcolor
```
* i3status 状态栏
```
 bar {
     position top
     font xft:Font Awesome 5 16 
     # When the status_command prints a new line to stdout, swaybar updates.
     # The default just shows the current date and time.
 #    status_command waybar  -c ~/.config/waybar/config
 #     status_command while ~/.config/sway/sway_bar.sh; do sleep 1; done
 #      status_command while date +'%Y-%m-%d %l:%M:%S %p'; do sleep 1; done
 
      status_command i3status -c ~/.config/i3status/i3status.conf
      colors {
                  background $barcolor
                  separator $barcolor  
  
                  focused_workspace  $fws  $fws $textcolor
                  active_workspace   $bgcolor  $bgcolor $textcolor
                  inactive_workspace $ibgcolor $ibgcolor $itextcolor
           }
}
```
* 声音背光调节快捷键
```
bindsym XF86AudioRaiseVolume exec amixer -D pulse sset Master 2%+ #increase sound     volume
bindsym XF86AudioLowerVolume exec amixer -D pulse sset Master 2%- #decrease sound     volume
bindsym XF86AudioMute exec amixer -D pulse sset Master toggle # mute sound
bindsym XF86MonBrightnessUp exec brightnessctl -c backlight s 5%+
bindsym XF86MonBrightnessDown exec brightnessctl -c backlight set 5%-
```
{{< admonition >}}
先安装brighjtnessctl用于背光调节。
{{< /admonition >}}

* 窗口间隙
```
gaps inner 5
gaps outer 5 
smart_gaps on
```
* 指定特定程序窗口
```
bindsym $mod+x exec google-chrome-stable
for_window [instance=google-chrome]  move container to workspace number 2
for_window  [instance=google-chrome] 
```
* 设置关机锁屏等快捷键
```
set $Locker "swaylock  -f -c 000000" 
 
set $mode_system System (l) lock, (s) suspend, (h) hibernate, (r) reboot, (Shift    +s) shutdown
mode "$mode_system" {
      bindsym l exec --no-startup-id $Locker
   #  bindsym e exec --no-startup-id i3-msg exit, mode "default"
      bindsym s exec --no-startup-id $Locker && systemctl suspend, mode "default"
      bindsym h exec --no-startup-id $Locker && systemctl hibernate, mode "default    "
      bindsym r exec --no-startup-id systemctl reboot, mode "default"
      bindsym Shift+s exec --no-startup-id systemctl poweroff -i, mode "default"
 
      # back to normal: Enter or Escape
      bindsym Return mode "default"
      bindsym Escape mode "default"
}
 bindsym $mod+Shift+q mode "$mode_system"a
 ```
* 设置边框大小
 ```
 default_border pixel 1
 ```
* 设置键盘
参照[github](https://github.com/pfaion/x1carbon-xkb-geometry)设置
 ```
   input * {
     xkb_layout "us"
     xkb_variant ""
     xkb_options ""
 }
 
input <identifier> xkb_model "x1carbon"
```
* 全屏取消锁屏
```
for_window [instance=google-chrome] inhibit_idle fullscreen
```


