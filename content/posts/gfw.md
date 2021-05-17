---
title: "科学上网设置"
date: 2020-12-28T16:35:59+08:00
categories : ["linux"]
tags : ["翻墙"]
draft: true
---
> 如今国内互联网的墙越来越高，如何翻墙已成为上网的基本技能，所以多掌握几种翻墙技巧还是必要的。

## 使用`v2raya`设置`trojan`
### 服务器端设置
#### 安装
```
sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"
```
#### 设置
编辑`/etc/trojan/config.json`。默认端口可以不用修改。
- 设置`run_type`为`server`
- 设置`password`
- 设置证书
  证书可参照[]()安装设置。
### 客户端设置
v2raya的设置很简单。
- 安装
`yay -S v2raya`。
- 设置自启动
```
sudo systemctl enable v2raya
sudo systemctl start v2raya
```
- 设置
使用浏览器打开`localhost:2017`进行设置。
- 添加`server`，选择类型为`trojan`。
- 连接
## 使用`heroku`
这里我们可以参照`github`项目[xrayku](https://github.com/mixool/xrayku)设置。
## 使用`xray`
### 安装
root用户`bash <(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh) install`
### 配置
配置`/usr/local/etc/xray/config.json`，将其中`xxx`部分改为你自己的，`uuid`可以使用`xray uuid`生成。
```
{
    "log": {
        "loglevel": "warning"         
    },
        "routing": {
                    "domainStrategy": "AsIs",
                    "rules": [
                    {
                     "type": "field",
                     "domain": [
                     "geosite:category-ads-all"
                                        ],
                     "outboundTag": "block"
                    },
                    {
                      "type": "field",
                      "domain": [
                      "geosite:cn"
                               ],
                      "outboundTag": "direct"
                    },
                    {
                       "type": "field",
                       "ip": [
                              "geoip:cn",
                              "geoip:private"
                             ],
                       "outboundTag": "direct"
                    },
                    {
                        "type": "field",
                        "domain": [
                         "geosite:geolocation-!cn"
                                        ],
                        "outboundTag": "proxy"
                    }
                    ]
        },
        "inbounds": [
        {
                       "tag": "socks-in",
                       "protocol": "socks",
                       "listen": "127.0.0.1", 
                       "port": 10800,     
                       "settings": {
                       "udp": true
                         }
        },
        {
                       "tag": "http-in",
                       "protocol": "http",
                       "listen": "127.0.0.1",  
                       "port": 10801
        },
        {
                       "tag": "gproxy-in",
                       "listen": "127.0.0.1",
                       "port": 1082,
                        "protocol": "dokodemo-door",
                        "settings": {
                           "followRedirect": true,
                            "network": "tcp,udp"
                      },
                        "sniffing": {
                           "destOverride": [
                           "http",
                            "tls"
                          ],
                        "enabled": true,
                      "streamSettings": {
                        "sockopt": {
                           "tproxy": "tproxy"
                          }
                      }
                  }
              }
        ],
        "outbounds": [
        {
                       "tag": "proxy",
                       "protocol": "vless",
                       "settings": {
                       "vnext": [
                                   {
                                       "address": "xxx.xxx.xxx",
                                       "port": 443,
                                       "users": [
                                                    {
                                                      "id": "xxx", 
                                                      "flow": "xtls-rprx-direct",
                                                      "encryption": "none",
                                                      "level": 0
                                                    }
                                               ]
                                      }
                                   ]
                                },
                        "streamSettings": {
                                          "network": "tcp",
                                          "security": "xtls",
                                          "xtlsSettings": {
                                          "serverName": "git.tiantian.cool", 
                                          "allowInsecure":false 
                                         }
                                    }
        },
        {
                        "tag": "direct",
                                   "protocol": "freedom"
        },
        {
                        "tag": "block",
                                   "protocol": "blackhole"
        }
        ]    
}
```
### 升级
`bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install`
### git加速
- 将`github.com`换为`github.com.cnpmjs.org`即可实现加速
- `ghproxy`
下载地址前添加`ghproxy.com`。
# NetWorkManager下dnsmasq设置
-  进入/etc/NetWorkManager/conf.d
编辑`vim 00-use-dnsmasq.conf`
```
[main]
dns=dnsmasq
- /etc/NetWorkManager/dnsmasq.d
```
编辑`vim 02-add-hosts.conf`
`addn-hosts=/etc/hosts`
# 使用mosh
- 连接 
`mosh --ssh='ssh -p xxxx' xxx@xxx.xxx.xxx.xxx`
- 开启防火墙
```
sudo ufw enable udp 60001
sudo ufw enable udp 60003
sudo ufw enable udp 60003
sudo ufw enable udp 60004
sudo ufw enable udp 60005
``` 
### nginx不同翻墙软件共用443端口
编辑 `/etc/nginx/nginx.conf`
```
stream {
    # 这里就是 SNI 识别，将域名映射成一个配置名
    map $ssl_preread_server_name $backend_name {
        aaa.xxx.xxx web;
        bbb.xxx.xxx vmess;
        ccc.xxx.xxx trojan;
    # 域名都不匹配情况下的默认值
        default web;
    }

    # web，配置转发详情
    upstream web {
        server 127.0.0.1:10240;
    }

    # trojan，配置转发详情
    upstream trojan {
        server 127.0.0.1:10241;
    }

    # vmess，配置转发详情
    upstream vmess {
        server 127.0.0.1:10242;
    }

    # 监听 443 并开启 ssl_preread
    server {
        listen 443 reuseport;
        listen [::]:443 reuseport;
        proxy_pass  $backend_name;
        ssl_preread on;
    }
}

http {
  # 这块保持不变即可
}
```
### vim关闭自动缩进
- 开启`paste`模式
`set paste`
- 关闭`paste`模式
`ser nopaste`
- 在`.vinrc`中设置
```
"Paste toggle - when pasting something in, don't indent.
set pastetoggle=<F3>"
```
这样就可以用F3来切换了。

### kakoune无缩进粘贴

按`\i`
## 透明代理

按照[透明代理（TProxy）配置教程](https://xtls.github.io/documents/level-2/tproxy/)配置，注意将`xray`设置文件`config.json`中相关内容改成自己的，要给`nftables.conf`设置可执行权限。

## jar证书
- 删除已有证书

 解压jar文件，删除`META-INF`中内容。
- 生成证书
 ` keytool -genkey -alias xxx  -keyalg RSA  -storepass xxx -keystore xxx  -keypass xxx`
- 自签名
` java -jar uber-apk-signer-1.2.1.jar -a xxx.apk --ks xxx  --ksAlias xxx`

