---
title: "我使用的主要软件"
date: 2020-11-01T14:49:57+08:00
categories : ["linux"]
tags : ["software"]
---
>安装完系统后，你还需要安装基本的软件来方便使用。好在linux下软件众多，得益于开放性和自由免费的理念，你有众多选择，总可以选择适合你的。下面便是我目前使用的软件：
## 备份软件
clonezilla是一款linux下优秀的备份软件，基于debian构建，类似ghost,但比ghost更灵活。自身可以启动，支持多种模式备份，即可以整盘备份，也可以按分区备份。可通过ventoy制作启动USB，放入ISO即可。
## USB启动工具
ventoy是EFI启动模式下制作USB启动盘的工具，与以往不同，它不需要复杂操作，安装后会在USB上建立两个分区，一个用于启动，一个用于放置ISO文件。只需将ISO文件放入特定分区，ventoy会自动识别，加入启动选择。
## 终端
我使用的是urxvt，termite不能在ranger中预览图像。urxvt设置较为繁琐，在wayland下可以使用fcitx，alacritty和kitty均无法使用。网上说alacritty可以，但我怎么设置都不行，只好放弃。
{{< admonition tip>}}
* 一定要安装aur中的`urxvt-unicode-truecolor`来支持中文和真彩色。
* 在wayland中，urxvt可通过.Xdefaults设置，以下设置均在其中。
{{</admonition >}}
* fcitx支持
```
URxvt.preeditType:Root
URxvt.inputMethod:fcitx
```
* 使用守护进程模式
urxvt可采用守护模式使用，使用urxvtc启动终端，节省内存，更加快速。这里采用systemd方式启动urxvtd守护进程。

  + 新增`sudo /etc/systemd/system/urxvtd@.service`服务
  + 启动服务
```
[Unit]
Description=RXVT-Unicode Daemon

[Service]
User=%i
ExecStart=/usr/bin/urxvtd -q -o

[Install]
WantedBy=multi-user.target
```
```
systemctl start urxvtd@username.service
systemctl enable urxvtd@username.service
```

* Dracula 配色
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
* 背景透明
```
URxvt.background:[80]#282A36
```
* 复制粘贴
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
* 字体设置
```
URxvt.font:xft:JetBrains Mono:style=Regular:antialias=True:pixelsize=22
URxvt.boldFont:xft:JetBrains Mono:style=Bold:antialias=True:pixelsize=22
```
## shell
shell是fish，相比zsh，开箱即用，支持自动补全，历史记录，很是方便。
oh-my-fish是fish功能拓展，`curl -L https://get.oh-my.fish | fish`一键安装，可配套使用。
{{< admonition tip >}}
更换缺省shell
```
chsh /usr/bin/fish
```
{{< /admonition >}}

oh-my-fish使用`omf'配置
  - 列出所有安装包`omf list`。
  - 查看主题`omf theme`。
  - 安装主题`omf install **`。
## 文本编辑 
使用neovim，功能多，更新快。 插件使用vim-plug管理。

.config/vim/init.vim
```
call plug#begin('~/.local/share/nvim/plugged')
Plug 'vim-airline/vim-airline'
" Plug 'scrooloose/nerdtree'
Plug 'scrooloose/nerdtree'
Plug 'morhetz/gruvbox'
Plug 'dracula/vim', { 'as': 'dracula' }
call plug#end()
"colorscheme gruvbox
colorscheme dracula

" autocmd vimenter * NERDTree  "自动开启Nerdtree
let g:NERDTreeWinSize = 25 "设定 NERDTree 视窗大小
let NERDTreeShowBookmarks=1  " 开启Nerdtree时自动显示Bookmarks
" 打开vim时如果没有文件自动打开NERDTree
" autocmd vimenter * if !argc()|NERDTree|endif
" 当NERDTree为剩下的唯一窗口时自动关闭
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
" 设置树的显示图标
let g:NERDTreeDirArrowExpandable = '+'
let g:NERDTreeDirArrowCollapsible = '-'
let NERDTreeIgnore = ['\.pyc$']  " 过滤所有.pyc文件不显示
let g:NERDTreeShowLineNumbers=0 " 是否显示行号
let g:NERDTreeHidden=0     "不显示隐藏文件
""Making it prettier
let NERDTreeMinimalUI = 1
let NERDTreeDirArrows = 1
nnoremap <F3> :NERDTreeToggle<CR> " 开启/关闭nerdtree快捷键

" set number 
set termguicolors
set background=dark
highlight Normal guibg=NONE ctermbg=None

" autocmd vimenter * NERDTree  "自动开启Nerdtree
let g:NERDTreeWinSize = 25 "设定 NERDTree 视窗大小
let NERDTreeShowBookmarks=1  " 开启Nerdtree时自动显示Bookmarks
"打开vim时如果没有文件自动打开NERDTree
" autocmd vimenter * if !argc()|NERDTree|endif
"当NERDTree为剩下的唯一窗口时自动关闭
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
" 设置树的显示图标
let g:NERDTreeDirArrowExpandable = '+'
let g:NERDTreeDirArrowCollapsible = '-'
let NERDTreeIgnore = ['\.pyc$']  " 过滤所有.pyc文件不显示
let g:NERDTreeShowLineNumbers=0 " 是否显示行号
let g:NERDTreeHidden=0     "不显示隐藏文件
""Making it prettier
let NERDTreeMinimalUI = 1
let NERDTreeDirArrows = 1
nnoremap <F3> :NERDTreeToggle<CR> " 开启/关闭nerdtree快捷键

" set number 
set termguicolors
set background=dark
highlight Normal guibg=NONE ctermbg=None
```
## 配色
可以为fish，kitty，neovim均安装使用流行的dracula配色，这方面搜索dracula网站即可，说明很详细。
{{< admonition tip >}}
neovim颜色可按照vim配置说明操作即可。
{{< /admonition >}}
## 私有网盘
filebrowser是一款简单、方便的网盘软件，只需一个文件。

/etc/filebrowser/filebrowser.json
```
  "port": 7000,
  "noAuth": false,
  "baseurl":"/",
  "address": "204.15.72.113",
  "alternativeReCaptcha": false,
  "reCaptchaKey": "",
  "reCaptchaSecret": "",
  "database": "/etc/filebrowser/filebrowser.db",
  "log": "stdout",
  "plugin": "",
  "scope": ".",
  "allowCommands": true,
  "allowEdit": true,
  "allowNew": true,
  "commands": [
    "wget"
```
/etc/systemd/system/filebrowser.service
```
[Unit]
Description=The filebrowser Process Manager
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/filebrowser -c /etc/filebrowser/filebrowser.json
ExecStop=/bin/killall filebrowser
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
## frp内网穿透
我们的宽带一般是没有公网地址的，为了能访问家中的nas服务器，需要安装内网穿透软件frp。

/etc/systemd/system/frps.service
```
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/bin/frps -c /etc/frps/frps.ini

[Install]
WantedBy=multi-user.target
```
/etc/frps/frps.ini
```
# [common] is integral section
[common]
# A literal address or host name for IPv6 must be enclosed
# in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80"
bind_addr = 0.0.0.0
bind_port = 5500

# udp port to help make udp hole to penetrate nat
#bind_udp_port =

# udp port used for kcp protocol, it can be same with 'bind_port'
# if not set, kcp is disabled in frps
#kcp_bind_port =

# specify which address proxy will listen for, default value is same with bind_addr
# proxy_bind_addr = 127.0.0.1

# if you want to support virtual host, you must set the http port for listening (optional)
# Note: http port and https port can be same with bind_port
vhost_http_port = 8080
vhost_https_port = 4433

# response header timeout(seconds) for vhost http server, default is 60s
# vhost_http_timeout = 60

# TcpMuxHttpConnectPort specifies the port that the server listens for TCP
# HTTP CONNECT requests. If the value is 0, the server will not multiplex TCP
# requests on one single port. If it's not - it will listen on this value for
# HTTP CONNECT requests. By default, this value is 0.
# tcpmux_httpconnect_port = 1337

# set dashboard_addr and dashboard_port to view dashboard of frps
# dashboard_addr's default value is same with bind_addr
# dashboard is available only if dashboard_port is set
dashboard_addr = 0.0.0.0
dashboard_port = 6443

# dashboard user and passwd for basic auth protect, if not set, both default value is admin
dashboard_user = leon
dashboard_pwd = Gjbyhy@72

# enable_prometheus will export prometheus metrics on {dashboard_addr}:{dashboard_port} in /metrics api.
enable_prometheus = true

# dashboard assets directory(only for debug mode)
# assets_dir = ./static
# console or real logFile path like ./frps.log
log_file = ./frps.log

# trace, debug, info, warn, error
log_level = info

log_max_days = 3

# disable log colors when log_file is console, default is false
disable_log_color = false

# DetailedErrorsToClient defines whether to send the specific error (with debug info) to frpc. By default, this value is true.
detailed_errors_to_client = true

# AuthenticationMethod specifies what authentication method to use authenticate frpc with frps.
# If "token" is specified - token will be read into login message.
# If "oidc" is specified - OIDC (Open ID Connect) token will be issued using OIDC settings. By default, this value is "token".
authentication_method = token

# AuthenticateHeartBeats specifies whether to include authentication token in heartbeats sent to frps. By default, this value is false.
authenticate_heartbeats = false

# AuthenticateNewWorkConns specifies whether to include authentication token in new work connections sent to frps. By default, this value is false.
authenticate_new_work_conns = false

# auth token
token = tiantian.cool

# OidcClientId specifies the client ID to use to get a token in OIDC authentication if AuthenticationMethod == "oidc".
# By default, this value is "".
oidc_client_id =

# OidcClientSecret specifies the client secret to use to get a token in OIDC authentication if AuthenticationMethod == "oidc".
# By default, this value is "".
oidc_client_secret =

# OidcAudience specifies the audience of the token in OIDC authentication if AuthenticationMethod == "oidc". By default, this value is "".
oidc_audience =
 OidcTokenEndpointUrl specifies the URL which implements OIDC Token Endpoint.
# It will be used to get an OIDC token if AuthenticationMethod == "oidc". By default, this value is "".
oidc_token_endpoint_url =

# heartbeat configure, it's not recommended to modify the default value
# the default value of heartbeat_timeout is 90
# heartbeat_timeout = 90

# only allow frpc to bind ports you list, if you set nothing, there won't be any limit
allow_ports = 2000-3000,3001,3003,4000-50000

# pool_count in each proxy will change to max_pool_count if they exceed the maximum value
max_pool_count = 5

# max ports can be used for each client, default value is 0 means no limit
max_ports_per_client = 0

# TlsOnly specifies whether to only accept TLS-encrypted connections. By default, the value is false.
tls_only = false

# if subdomain_host is not empty, you can set subdomain when type is http or https in frpc's configure file
# when subdomain is test, the host used by routing is test.frps.com
subdomain_host = frps.com

# if tcp stream multiplexing is used, default is true
tcp_mux = true
# custom 404 page for HTTP requests
# custom_404_page = /path/to/404.h

```
## 中文输入法
使用fcitx，在.pam_environmentv中设置。
```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
MODIFIERS="@im=fcitx"
```
{{< admonition tip >}}
更换Adwaita/缺省难看的键盘图标，可以安装其他主题替换/usr/share/icons/Adwaita/32x32/legacy/input-keyboard.png文件。
{{< /admonition >}}
# 文件管理器 
ranger是一个终端文件管理器，使用键盘操作，在wayland下需安装w3m使用图片预览功能。
