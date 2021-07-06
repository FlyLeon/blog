# VPS配置之二Nginx

>nginx是常用服务器软件之一，高效，快速。我们的VPS需要它来建立网站、通过反向代理使用各种服务。从稳定性考虑，我的VPS使用Centos 7操作系统，网上资源也很丰富，便于搜索学习。

## 安装nginx
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
## 设置nginx
* 查看nginx设置文件位置。
```
sudo nginx -t
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
server {
    listen 80;
    server_name *.yourdomain.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name *.yourdomain.com;

    ssl_certificate /usr/local/nginx/conf/ssl/yourdomain.com.crt;
    ssl_certificate_key /usr/local/nginx/conf/ssl/yourdomain.com.key;

    client_max_body_size 50m; 
    client_body_buffer_size 256k;
    client_header_timeout 3m;
    client_body_timeout 3m;
    send_timeout 3m;
    proxy_connect_timeout 300s; 
    proxy_read_timeout 300s; 
    proxy_send_timeout 300s;
    proxy_buffer_size 64k; 
    proxy_buffers 4 32k; 
    proxy_busy_buffers_size 64k;
    proxy_temp_file_write_size 64k; 
    proxy_ignore_client_abort on; 

    location / {
        proxy_pass http://127.0.0.1:1234; //frps vhost port
        proxy_redirect off;
        proxy_set_header Host $host:80;
        proxy_ssl_server_name on;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

