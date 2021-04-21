---
title: "在xmonad下使用mpd播放音乐"
date: 2021-04-20T12:12:08+08:00
categories : ["linux"]
tags : ["xmonad,mpd"]
draft: true
---
> 
## 设置`mpd`
## 设置客户端 
## 设置`xmabar`显示
```
Config { 
    font = "xft:WenQuanYI Micro Hei:weight=bold:pixelsize=22:antialias=true:hinting=true,Hack Nerd Font:pixelsize=22",
    bgColor = "#292d3e",
    fgColor = "#f07178",
    lowerOnStart = True,
    hideOnStart = False,
    allDesktops = True,
    persistent = True,
    commands = [ 
        Run Date "  %d %b %Y %H:%M " "date" 600,
        Run Com "volume" [] "volume" 150,
        Run Com "battery" [] "battery" 600,
        Run Com "brightness" [] "brightness" 150,
        Run Com "bash" ["-c", "checkupdates | wc -l"] "updates" 3000,
        Run MPD ["-t",
                 "<composer> <title>  <statei> ",
                 "--", "-P", ">>", "-Z", "|", "-S", "><"] 10,
        Run Com "/home/leon/.config/xmobar/trayer-padding-icon.sh" [] "trayerpad" 600,
        Run UnsafeStdinReader
    ],
    alignSep = "}{",
        template = "<fc=#b303ff>   </fc>%UnsafeStdinReader% }{ \
        \<fc=#b8bb26> %mpd%  </fc>\
        \<fc=#c3e88d> %updates% </fc>\
        \<fc=#e1acff> %brightness%</fc>\
        \<fc=#FFB86C> %battery%</fc>\
        \<fc=#82AAFF> %volume% </fc>\
        \<fc=#8BE9FD> %date% </fc>\
        \%trayerpad%"
}
 
