# Gen8设置

>hp gen8是微型4盘位的服务器，比较适合做NAS。自己也海淘了一台，升级了CPU(1265v2)和内存(16M)，安装了Proxmox虚拟机，虚拟机内安装了windows server 2019,synology,centos 7,还是很稳定的。

## ilo静态ip地址设置
ilo是gen8的特色，方便服务器远程管理，无需连接显示器核键盘。使用自动获取ip，每次连接很不方便，可以为设置静态地址方便连接。目前我安装的ilo版本是2.75，支持使用html5远程控制管理，比较方便。
### 启动设置
常规可以在启动时按F9进入ilo设置界面内设置。
### linux内设置
如你的机器上已安装linux，也可用过ipmitool设置。ipmi是ibm发起的一个服务器设置协议，可以通过命令界面进行服务器的一些原需在BIOS中进行的一些设置。

- 首先查找网卡位置
```
for i in `seq 1 14`; do ipmitool lan print $i 2>/dev/null | grep -q ^Set && echo Channel $i; done

Channel 2
```
- 查看现有设置
```
ipmitool lan print 2

Set in Progress         : Set Complete
Auth Type Support       : 
IP Address Source       : DHCP Address
IP Address              : 0.0.0.0
Subnet Mask             : 0.0.0.0
MAC Address             : c0:ff:ee:c0:ff:ee
BMC ARP Control         : ARP Responses Enabled, Gratuitous ARP Disabled
Default Gateway IP      : 0.0.0.0
802.1q VLAN ID          : Disabled
802.1q VLAN Priority    : 0
Cipher Suite Priv Max   : Not Available
```
- 设置静态网址
```
ipmitool lan set 2 ipsrc static
ipmitool lan set 2 ipaddr 192.168.50.36
ipmitool lan set 2 netmask 255.255.255.0
ipmitool lan set 2 defgw ipaddr 192.168.50.1
ipmitool mc reset cold
```
## 升级BIOS
HP gen8的很多设着都可以在ilo下操作，升级BIOS也可以在ilo固件下进行，直接上传BIOS镜像即可，我使用的是2019年4月4日的。当然也可以制作usb来升级，不过没有ilo方便。另外gen8为了安全期间，有两个BIOS，一个备份，一个使用，正常升级只升级备份，没有问题的话，需要我们在BIOS手动交换来永久使用。
具体操作可上网搜索。

## 使用SPP升级系统
HP gen8可定期通过SPP升级系统，不过HP网站过质保期后就无法下载，我们可以搜索热心网友分享的下载。

{{< admonition tip  >}}
最新SPP下载地址：{{< link "https://technet24.ir/%D8%AF%D8%A7%D9%86%D9%84%D9%88%D8%AF-%D8%B3%D8%B1%D9%88%DB%8C%D8%B3-%D9%BE%DA%A9-hpe-service-pack-for-proliant-9872" >}}
{{< /admonition >}}
* 远程在ilo界面左上角选择CDROM图标挂载SPPiso镜像
* 重新启动按F1选择CD-ROM启动。
* 选择第一项自动安装，否则升级可能会出现问题。

## proxmox
proxmox 是一款免费虚拟机软件，基于debian，使用qemu，lxc虚拟化。提供专门的管理页面（端口8006）来建立管理虚拟机，支持集群，功能强大，我仅使用了它最基础的功能。
{{< admonition tip >}}
安装很简单，使用ilo远程服务台挂载安装ISO，选择CDROM启动即可。proxmox占用空间很小，可直接安装到gen8内置的SD或USB盘上，可以省去一个硬盘接口。
选择网卡时，一定要选择好，否则启动后无法连接控制台，解决也很简单，交换网线口即可。
{{< /admonition >}}

## synology
群辉是比较常用的NAS它的软件是基于linux开发，功能强大，使用模块化管理，拓展性强，稳定性高。我安装了一个它的破解后的虚拟机，主要用作NAS和照片管理。
最新资源在[xpenology](https://xpenology.com/forum/)上可以找到。
##  软路由
常见的pc软路由软件有openwart，ros，voxy，其中lede是基于openwrt整合了较多功能，我们一般使用lead即可。ros是收费商业软件，稳定性和性能据说不错，不过无法拓展功能。voxy也是开源软件，但也无法拓展。lede编译比较复杂，依赖很多，我们可以使用现成的[docker](https://github.com/jandelgado/lede-dockerbuilder)来编译，方便很多。

