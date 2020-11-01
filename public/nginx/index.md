# VPS配置之二Nginx

>nginx是常用服务器软件之一，高效，快速。我们的VPS需要它来建立网站、通过反向代理使用各种服务。从稳定性考虑，我的VPS使用Centos 7操作系统，而且它的网上资源也很丰富，便于搜索学习。

# 安装nginx
* 安装EPEL仓库
```
sudo yum install epel-release
```
* 安装nginx
```
sudo yum install nginx
```
* 启动nginx
```
sudo systemctl start nginx
sudo systemctl enable nginx
```
* 设置防火墙
```
sudo firewall-cmd --permanent --zone=public --add-service=http 
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```
# 设置nginx
* 查看nginx设置位置。`sudo nginx -t`
* v2raya
```
server
    {
        listen 80;
        #listen [::]:80;
        server_name 你的网站地址;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /home/wwwroot/你的网站地址;

        include rewrite/other.conf;
        #error_page   404   /404.html;

        # Deny access to PHP files in specific directory
        #location ~ /(wp-content|uploads|wp-includes|images)/.*\.php$ { deny all; }
        include enable-php-pathinfo.conf;
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }
        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }
        location ~ /.well-known {
            allow all;
        }
        location ~ /\.
        {
            deny all;
        }
        access_log  /home/wwwlogs/你的网站地址.log;
    }

```
* filebrowser反向代理
```
resolver 8.8.8.8;
upstream filebrowser {
server  你的网站IP:7000;
}

server
{
    listen 80 default_server;
    server_name 你的网站地址;

location / {
        client_max_body_size 1024m;
        proxy_set_header Host            $host;
        proxy_set_header X-Real-IP       $remote_addr;
        proxy_set_header X-Forward-For   $proxy_add_x_forwarded_for;
        proxy_pass http://filebrowser;
}
}
```
* frps反向代理
```

```

