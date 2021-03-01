---
title: "Gfw"
date: 2020-12-28T16:35:59+08:00
categories : ["linux"]
tags : ["vps"]
draft: true
---
>ssl /usr/local/ssl/tiantian.crt tiantian.key

# ssl 证书
# trojan
'sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"'
'/usr/local/etc/trojan/config.json'
'''
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "password1",
        "password2"
    ],
    "log_level": 1,
    "ssl": {
        "cert": "/path/to/certificate.crt",
        "key": "/path/to/private.key",
        "key_password": "",
        "cipher": "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384",
        "cipher_tls13": "TLS_AES_128_GCM_SHA256:TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384",
        "prefer_server_cipher": true,
        "alpn": [
            "http/1.1"
        ],
        "reuse_session": true,
        "session_ticket": false,
        "session_timeout": 600,
        "plain_http_response": "",
        "curves": "",
        "dhparam": ""
    },
    "tcp": {
        "prefer_ipv4": false,
        "no_delay": true,
        "keep_alive": true,
        "reuse_port": false,
        "fast_open": false,
        "fast_open_qlen": 20
    },
    "mysql": {
        "enabled": false,
        "server_addr": "127.0.0.1",
        "server_port": 3306,
        "database": "trojan",
        "username": "trojan",
        "password": "",
        "key": "",
        "cert": "",
        "ca": ""
    }
}
'''
# trojan-go 
'''
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.5.1/trojan-go-linux-amd64.zip
unzip -o trojan-go-linux-amd64.zip -d /usr/local/bin/trojan-go/trojan-go
rm trojan-go-linux-amd64.zip
'''

'vim /etc/systemd/system/trojan-go.service'

'''
[Unit]
Description=Trojan-Go
After=network.target nss-lookup.target
Wants=network-online.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/trojan-go/trojan-go -config /usr/local/etc/trojan-go/config.json
Restart=on-failure
RestartSec=15

[Install]
WantedBy=multi-user.target
'''

'systemctl enable trojan-go'

'''
mkdir -p /usr/local/etc/trojan-go
vim /usr/local/etc/trojan-go/config.json
'''

'''
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "fuckgfw"
    ],
    "ssl": {
        "cert": "/etc/ssl/certs/example.com.cer",
        "key": "/etc/ssl/certs/example.com.key",
        "sni": "example.com"
    },
    "router":{
        "enabled": true,
        "block": [
            "geoip:private"
        ]
    }
}
'''
'./trojan-go -config config.json'
# Cloudflare 设置
将域名的 Namesever 指向 Cloudflare 所提供的地址，等待生效
NS 记录更新后，将 Cloudflare 中域名的 A 记录指向服务器 IP，确保云朵为橙色（Proxied）
在 SSL/TLS 版块中的 Overview 里，将加密模式调整为 Full (strict)
在 SSL/TLS 版块中的 Edge Certificates 里，将 Minimum TLS Version 调整为 TLS 1.3，并在下方确保开启对 TLS 1.3 的支持
在 Firewall 版块中的 Firewall Rules 里，添加一个规则，允许 /random 路径的访问（Allow URI path）
在 Cloudflare 上获取域名的 Zone ID，记录之
在 Cloudflare 的 My Profile 中生成一个 API Token，权限为 Zone Zone Read 和 Zone DNS Edit，Zone Resources 特指区域为 example.com，完成后记下 Token
根据自己的需要在 Cloudflare 上进行其他设置（可选），例如配置 Always Use HTTPS、HSTS、Automatic HTTPS Rewrites、Auto Minify 等等，主要影响浏览器访问网站的效果
'''
f1g1ns1.dnspod.net

f1g1ns2.dnspod.net
'''
'd6f39e137d9e8e7355274680ed1e962e'

'''
{"result":{"id":"9d6f1a4c7e2de89986478f1ed9c09428","status":"active"},"success":true,"errors":[],"messages":[{"code":10000,"message":"This API Token is valid and active","type":null}]}
'''
# xray
root用户'bash <(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh) install'
使用*VLESS over TCP with XTLS + 回落 & 分流 to WHATEVER（终极配置）*
配置’/usr/local/etc/xray/config.json‘。

'''
{
    "log": {
        "loglevel": "warning"
    },
    "inbounds": [
        {
            "port": 443,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "", // 填写你的 UUID
                        "flow": "xtls-rprx-direct",
                        "level": 0,
                        "email": "love@example.com"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": 1310, // 默认回落到 Xray 的 Trojan 协议
                        "xver": 1
                    },
                    {
                        "path": "/websocket", // 必须换成自定义的 PATH
                        "dest": 1234,
                        "xver": 1
                    },
                    {
                        "path": "/vmesstcp", // 必须换成自定义的 PATH
                        "dest": 2345,
                        "xver": 1
                    },
                    {
                        "path": "/vmessws", // 必须换成自定义的 PATH
                        "dest": 3456,
                        "xver": 1
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "xtls",
                "xtlsSettings": {
                    "alpn": [
                        "http/1.1"
                    ],
                    "certificates": [
                        {
                            "certificateFile": "/path/to/fullchain.crt", // 换成你的证书，绝对路径
                            "keyFile": "/path/to/private.key" // 换成你的私钥，绝对路径
                        }
                    ]
                }
            }
        },
        {
            "port": 1310,
            "listen": "127.0.0.1",
            "protocol": "trojan",
            "settings": {
                "clients": [
                    {
                        "password": "", // 填写你的密码
                        "level": 0,
                        "email": "love@example.com"
                    }
                ],
                "fallbacks": [
                    {
                        "dest": 80 // 或者回落到其它也防探测的代理
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "none",
                "tcpSettings": {
                    "acceptProxyProtocol": true
                }
            }
        },
        {
            "port": 1234,
            "listen": "127.0.0.1",
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "", // 填写你的 UUID
                        "level": 0,
                        "email": "love@example.com"
                    }
                ],
                "decryption": "none"
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "acceptProxyProtocol": true, // 提醒：若你用 Nginx/Caddy 等反代 WS，需要删掉这行
                    "path": "/websocket" // 必须换成自定义的 PATH，需要和分流的一致
                }
            }
        },
        {
            "port": 2345,
            "listen": "127.0.0.1",
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "", // 填写你的 UUID
                        "level": 0,
                        "email": "love@example.com"
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "none",
                "tcpSettings": {
                    "acceptProxyProtocol": true,
                    "header": {
                        "type": "http",
                        "request": {
                            "path": [
                                "/vmesstcp" // 必须换成自定义的 PATH，需要和分流的一致
                            ]
                        }
                    }
                }
            }
        },
        {
            "port": 3456,
            "listen": "127.0.0.1",
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "", // 填写你的 UUID
                        "level": 0,
                        "email": "love@example.com"
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "security": "none",
                "wsSettings": {
                    "acceptProxyProtocol": true, // 提醒：若你用 Nginx/Caddy 等反代 WS，需要删掉这行
                    "path": "/vmessws" // 必须换成自定义的 PATH，需要和分流的一致
                }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
'''
使用UUID'5049f982-68e5-02f7-e977-e335bf417966'
注意不同协议使用不同路径
