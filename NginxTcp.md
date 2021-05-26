---
title: Nginx 反向代理 TCP
date: 2021-05-27
---

## 简介
Nginx 除了可以代理 HTTP 以外，其实还可以转发 TCP / UDP，但之前的版本需要通过源码编译安装 Nginx 的时候启用 Stream 模块，目前新版本的 Nginx 默认已经启用了 Stream 模块。 

<!--more-->

- 检查 Nginx 是否启用了 Stream 模块

``` bash
nginx -V | grep with-stream # 如果有启用 Stream 模块，则会显示相关内容，这里的 Nginx 版本为 1.20.X
nginx version: nginx/1.20.1
built by gcc 10.2.1 20201203 (Alpine 10.2.1_pre1)
built with OpenSSL 1.1.1i  8 Dec 2020 (running with OpenSSL 1.1.1k  25 Mar 2021)
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-perl_modules_path=/usr/lib/perl5/vendor_perl --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-Os -fomit-frame-pointer -g' --with-ld-opt=-Wl,--as-needed
```

- 修改 nginx.conf，添加 Stream 配置

``` bash

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}


# 启用流转发
stream {

    log_format proxy  '$remote_addr [$time_local] '
                      '$protocol $status $bytes_sent $bytes_received '
                      '$session_time "$upstream_addr" '
                      '"$upstream_bytes_sent" "$upstream_bytes_received" "$upstream_connect_time"';

    access_log /var/log/nginx/tcp-access.log proxy ;
    open_log_file_cache off;
    include /etc/nginx/tcp.d/*.conf;
}

``` 

- 新建一个 tcp.d 目录，用于存放 TCP 转发配置

``` bash
mkdir tcp.d && cd tcp.d
``` 

- MySQL 转发示例配置

``` bash
vi mysql.conf

upstream tcp {
    server {目标机器 IP}:3306;
}

server {
    listen 3306;
    proxy_connect_timeout 8s;
    proxy_timeout 24h;
    proxy_pass tcp;
}
```

- Redis 转发示例配置

``` bash
vi redis.conf

upstream tcp {
    server {目标机器 IP}:6379;
}

server {
    listen 6379;
    proxy_connect_timeout 8s;
    proxy_timeout 24h;
    proxy_pass tcp;
}
```

- 启动指令

``` bash
docker run --name tcp-nginx -d -p 3306:3306 -p 6379:6379 -v $PWD/nginx.conf:/etc/nginx/nginx.conf -v $PWD/tcp.d:/etc/nginx/tcp.d nginx:1.20-alpine
```
