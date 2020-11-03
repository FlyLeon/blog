---
title: "我使用的主要软件"
date: 2020-11-01T14:49:57+08:00
categories : ["linux"]
tags : ["software"]
---
>安装完系统后，你还需要选择基本的软件来方便使用。好在linux下软件众多，得益于开放性和自由免费的理念，你有众多选择，总可以选择适合你的。下面便是我目前使用的软件：
## 备份软件
clonezilla是一款linux下优秀的备份软件，基于debian构建，类似ghost,但比ghost更灵活。自身可以启动，支持多种模式备份，即可以整盘备份，也可以按分区备份。可通过ventoy制作启动USB，放入ISO即可。
## USB启动工具
ventoy是EFI启动模式下制作USB启动盘的工具，与以往不同，它不需要复杂操作，安装后会在USB上建立两个分区，一个用于启动，一个用于放置ISO文件。只需将ISO文件放入特定分区，ventoy会自动识别，加入启动选择。
## 终端
我使用的是kitty，支持硬件加速，速度很快。 
## shell
shell是fish，相比zsh，开箱即用，支持自动补全，历史记录，很是方便。
oh-my-fish是fish功能拓展，`curl -L https://get.oh-my.fish | fish`一键安装，可配套使用。
{{< admonition tip >}}
更换缺省shell
```
chsh /usr/bin/fish
```
{{< /admonition >}}
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
## **软件
v2raya是一款使用网页方便设置的**软件，支持全局代理，支持常用**协议。
## 配色
可以为fish，kitty，neovim均安装使用流行的dracula配色，这方面搜索dracula网站即可，说明很详细。
{{< admonition tip >}}
neovim颜色可按照vim配置说明操作即可。
{{< /admonition >}}
## 私有网盘
filebrowser是一款简单、方便的网盘软件，只需一个文件。
<<<<<<< HEAD
=======

>>>>>>> b18ea3f0ef63f2c36b2809fa17d94d754911f2aa
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
<<<<<<< HEAD
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

~                                                                                     
[Unit]
=======
>>>>>>> b18ea3f0ef63f2c36b2809fa17d94d754911f2aa
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
