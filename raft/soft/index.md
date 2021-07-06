# linux常用软件

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
* 慎用URxvt.letterSpace设置字间距，在我机器上这为不同字体设置带来问题。
{{< /admonition >}}
## shell
shell是fish，相比zsh，开箱即用，支持自动补全，历史记录，很是方便。
oh-my-fish是fish功能拓展，`curl -L https://get.oh-my.fish | fish`一键安装，可配套使用。
{{< admonition tip >}}
更换缺省shell
```
chsh /usr/bin/fish
```
{{< /admonition >}}

oh-my-fish使用`omf`配置
  - 列出所有安装包`omf list`。
  - 查看主题`omf theme`。
  - 安装主题`omf install **`。
  - 更新    `omf update omf`
  - 搜索包  `omf search **`。
  - 移除包  `omf remove **`。
  - 排错    `omf doctor`
  - 卸载    `omf destroy`
  - 显示帮助`omf -h`
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
filebrowser是一款简单、方便的网盘软件，安装只需一个文件。
* 设置

编辑`/etc/filebrowser/filebrowser.json`
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
  ]
}
```
* 自启动

编辑`/etc/systemd/system/filebrowser.service`
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
* 文件目录根设置

登录后，点击[设置]->[全局设置]->[用户默认设置]，将[目录范围]改为你想要的文件夹。
## frp内网穿透
我们的宽带一般是没有公网地址的，为了能访问家中的nas服务器，需要安装内网穿透软件frp。
* 自启动

编辑`/etc/systemd/system/frps.service`。
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
* 设置

编辑`/etc/frps/frps.ini`。
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
# set dashboard_addr and dashboard_port to view dashboard of frps
# dashboard_addr's default value is same with bind_addr
# dashboard is available only if dashboard_port is set
dashboard_addr = 0.0.0.0
dashboard_port = 6443
# dashboard user and passwd for basic auth protect, if not set, both default value is admin
dashboard_user = 
dashboard_pwd = 
# AuthenticationMethod specifies what authentication method to use authenticate frpc with frps.
# If "token" is specified - token will be read into login message.
# If "oidc" is specified - OIDC (Open ID Connect) token will be issued using OIDC settings. By default, this value is "token".
authentication_method = token
# auth token
token = 
# if subdomain_host is not empty, you can set subdomain when type is http or https in frpc's configure file
# when subdomain is test, the host used by routing is test.frps.com
subdomain_host = frps.com
```
## 字体设置
中文字体我安装了免费的文泉驿正黑、文泉驿微米黑、方正书宋;英文终端字体选择了Hack。字体大多可通过`yay`自动安装，方正书宋选择下载后手动安装。archlinux中文设置已比较成熟，我仅在chrome和urxvt中自定义了字体。chrome在设置中设置自定义字体，标准字体为方正书宋，Serif 字体为方正书宋，Sans-serif 字体为文泉驿微米黑，宽度固定的字体为文泉驿等宽微米黑。urxvt使用Hack字体，具体见urvxt设置部分。
* 字体手动安装
将欲安装字体拷入`/usr/share/fonts`目录中，运行`fv-cache`完成安装。
```
sudo cp 字体.ttf /usr/share/fonts/字体.ttf
sudo fv-cache -v -f
```
* 查看已安装字体
```
fc-list | grep 字体名
```


