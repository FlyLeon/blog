# Archlinux 优化之一

>archliux是一个滚动更新的linux发行版，它有庞大软件库和最新的系统，适合尝鲜使用.事实上你只要经常更新，它也是比较稳定的。我曾使用过manjaro，安装确实省事。但你无法完全控制，不适用我这样追求完美注意的人，gentoo又似乎太麻烦，为了那一点性能提高，好像也划不来。所以我还是使用archlinux，定制性强又不失快速。自从使用EFI STUB启动后，BiOS自检完，进入登录画面只需1秒多，可以说是飞速。使用wayland加sway和xammod内核进入系统内存占用不到300M，我已经满意了。


## bcm 4360 网卡驱动
archlinux 安装需联网。broadcom 4360 网卡需手动安装驱动，是安装前首先要做的。archlinux安装盘自带bcm4360驱动,不过缺省使用的b43quds，我们需要卸载后，加载wl驱动。
```
rmmod b43 ssb
rmmod bcma
modprober wl
rmmod wl
modprober wl 
ip link
iwctl
station wlan0 get-networks
station wlan0 connect  **
```
{{< admonition type=tip title="技巧" >}}
如果加载wl模块后，仍无法找到无线网卡，可卸载重新加载一次
{{< /admonition >}}

## pgp密钥导入
部分yay软件需要pgp密钥，可使用代理添加。
```
proxychains  gpg --keyserver keyserver.ubuntu.com --recv-keys **
```
## 临时增加 tmp大小
编译xanmod需要。
```
sudo mount -o remount,size=6G,noatime /tmp
```
## timeshift备份

使用timeshift可随时给系统做快照，实现增量备份。
```
sudo timeshift --list
sudo timeshift --snapshot-device /dev/sdb4
sudo timeshift --restore --snapshot '2019-07-16_16-35-42' --skip-grub
```
{{< admonition >}}
若无法进入系统，可使用manjaro安装盘启动，在终端中`sudo timeshift`，选择快照的恢复。
{{< /admonition >}}
## 加快启动速度
   关闭 watchdog。
## 安装 xanmod 内核
可以优化桌面反应速度，启动速度，减少内存占用。
```
yay -S linux-xanmod linux-xanmod-header --editmenu
grub-mkconfig -o /boot/grub/grub.cfg
```
{{< admonition tip >}}
* 可以通过编辑PKGBUILD文件，将其中`_microarchitectrue=0`改为自己的CPU类型，来简单优化一下。CPU类型缺省会在编译过程中提示，如没有自己cpu类型，可以选择最接近的。
* 如想使用cachy调度器，需要在PKGBUULD文件中将`cachy=no`改为`yes`。
* 如需自定义kernel编译内容，可在PKGBUILD文件build内加入`make menuconfig`。
* 如需精简内核，可插上所有使用的硬件、打开所需服务后，在PKGBUILD中build内加入`make localmodconfig`。使用后最大的变化就是编译时间和内核大小大为减少，我的机器xanmod内核编译时间由近1小时减少到10份分钟，非常明显，缺点是如需增加新硬件需重新编译。
{{< /admonition >}}
## EFI STUB 启动
可使用EFI STUB加快启动速度。
{{< admonition >}}
若使用EFI STUB, 安装时需将EFI安装到boot分区。请将part换为自己的BOOT分区，将UUID改为自己BOOT分区。
{{< /admonition >}}

```
sudo efibootmgr --disk /dev/nvme0n1 --part 1 --create --label "Arch" --loader /vmlinuz-linux-xanmod --unicode 'root=UUID=0f949b62-98bf-4787-b8b9-1f21d0889691  rw initrd=\initramfs-linux-xanmod.img loglevel=3 quiet nowatchdog'

```
## 清理
- 清理孤立包
```
sudo pacman -Rns $(pacman -Qtdq)
```
- 设置日志大小
```
journalctl --vacuum-size=50M
```
- 清理pacman缓存

慎用`sudo pacman -Scc`暴力清理，可以安装pacman-contrib包使用paccache清理。paccache清理缺省保留最近3次的升级的软件包，方便今后降级，而且通过参数可以控制清理内容。如`paccache -d`可以先查看删除内容,`paccache -r`删除缓冲包，`paccahche -rk1`删除最近一次升级外缓存包等等。
## makepkg速度优化
新建 ~/.makepkg.conf, 写入如下内容，优化生成的二进制文件， 加快编译速度， 加快软件包生成速度。
```
CFLAGS="-march=native -O2 -pipe -fno-plt"
CXXFLAGS="-march=native -O2 -pipe -fno-plt"

MAKEFLAGS="-j$(nproc)"

BUILDENV=(!distcc color ccache !check !sign)
BUILDDIR=/tmp/makepkg

COMPRESSXZ=(xz -c -z - --threads=0)
```

