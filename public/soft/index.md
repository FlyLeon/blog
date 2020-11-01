# 我使用的主要软件

>安装完系统后，你还需要选择基本的软件来方便使用。好在linux下软件众多，得益于开放性和自由免费的理念，你有众多选择，总可以选择适合你的。下面便是我目前使用的软件：
## 备份软件
clonezilla是一款linux下优秀的备份软件，基于debian构建，类似ghost,但比ghost更灵活。自身可以启动，支持多种模式备份，即可以整盘备份，也可以按分区备份。可通过ventoy制作启动USB，放入ISO即可。
## USB启动工具
ventoy是EFI启动模式下制作USB启动盘的工具，与以往不同，它不需要复杂操作，安装后会在USB上建立两个分区，一个用于启动，一个用于放>置ISO文件。只需将ISO文件放入特定分区，ventoy会自动识别，加入启动选择。
## 终端
我使用的是kitty，支持硬件加速，速度很快。 
## shell
shell是fish，相比zsh，开箱即用，支持自动补全，历史记录，很是方便。
oh-my-fish是fish功能拓展，`curl -L https://get.oh-my.fish | fish`一键安装，可配套使用。
{{< admonition tip >}}
设置更换缺省shell
```
chsh /usr/binfish
```
{{< /admonition >}}
## 文本编辑 
使用neovim，功能多，更新快。
插件使用vim-plug管理.
```
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
## frp内网穿透
我们的宽带一般是没有公网地址的，为了能访问家中的nas服务器，需要安装内网穿透软件frp。

