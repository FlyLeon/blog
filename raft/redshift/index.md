# wayland使用redshift护眼程序

>redshift是linux护眼软件，可以根据你的地理位置，随时间自动调节屏幕屏幕色温，减少蓝光伤害，达到护眼目的。
### 安装redshift-wayland-git
我们需要使用yay安装wayland版本的redshift，缺省的redshift并不适用于wayland。
```
yay -S redshift-wayland-git
```
### 设置
编辑 .config/redshift/redshift.conf来设置缺省色温、调节方式和位置等。 
```
[redshift]
; 设置白天和晚上的屏幕温度（中性为6500k）
; 在2700K/6300K色温下更容易造成视疲劳
temp-day=5800
temp-night=4500

; 逐渐增强或降低屏幕的温度，平滑过渡
transition=1

; 设置位置提供者为manual
location-provider=manual

; 设置randr调整方法
adjustment-method=wayland

[manual]
; 经纬度
lon=
lat= 

; 调整屏幕'0',从0开始
[randr]
screen=0
```
### 启动
* 使用usr方式systemd启动，新建`/usr/lib/systemd/user/redshit.service`文件。
```
[Unit]
Description=Redshift display colour temperature
adjustment
Documentation=http://jonls.dk/redshift/
After=graphical-session.target

[Service]
ExecStart=/usr/bin/redshift
Restart=always
RestartSec=5
[Install]
WantedBy=graphical-session.target
```
* 自启动
```
systemctl --user start redshift.service
systemctl --user enable redsfift.service
```
{{< admonition >}}
* 安装wayland版本`redshift-wayland-git`
* 设置redshift的`adjustment-method=wayland`
* 使用user方式systemd启动，加入`RestartSec=5 `，否则会启动失败。
{{< /admonition >}}

