---
layout: post
title:  "nginx"
date:   2018-06-20 10:45:11 +0800
categories: nginx installation
---

## Install 

```bash
yum install -y gcc gcc-c++ glibc pcre pcre-devel
yum install -y wget make openssl openssl-devel zlib zlib-devel
wget http://nginx.org/download/nginx-1.12.2.tar.gz
tar -xzf nginx-1.12.2.tar.gz
cd nginx-1.12.2
. /configure --prefix=/usr/local/nginx/
make
make install
/usr/local/nginx/sbin/nginx
ps -ef|grep nginx
netstat -tnlp |grep 80
service firewalld stop
settenforce 0
echo "/usr/local/nginx/sbin/nginx" >> /etc/rc.local

```


