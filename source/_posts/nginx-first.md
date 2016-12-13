---
layout:     post
title:      "Nginx First"
subtitle:   "使用 Nginx 作为 Web 服务器"
date:       2016-12-11 10:15:00
author:     "Carson"
catalog:    true
tags:
    - Nginx
---

## install nginx 1.10.02

```
# download nginx_signing key
wget http://nginx.org/keys/nginx_signing.key

# add nginx_singning key to the apt program keyring
sudo apt-get add nginx_signing.key

# view system's code name (ubuntu14.04: trusty)
lsb_release -c

# add nginx source to '/etc/apt/sources.list'
$ echo "deb http://nginx.org/packages/ubuntu/ trusty nginx" >> /etc/apt/sources.list
$ echo "deb-src http://nginx.org/package/ubuntu/ trusty nginx" >> /etc/apt/sources.list

# install nginx
apt-get update
apt-get install nginx
```

## config nginx

```
# view nginx config setting
vim /etc/nginx/nginx.conf

# view nginx server setting
vim /etc/nginx/conf.d/*.conf

# seting proxy by express app
vim /etc/nginx/conf.d/default.conf
vim:
    server {
        location ~ ^/wechat/.+ {
            proxy_pass http://127.0.0.1:3000;
        }
    }
```
