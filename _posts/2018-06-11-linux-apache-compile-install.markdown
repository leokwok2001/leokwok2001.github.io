---
layout: post
title:  "編譯安裝 apache 2.4.29"
date:   2018-06-11 10:45:11 +0800
categories: apache 
comments: true
---



## 編譯安裝 apache 前必先安裝的軟体
```bash
yum install openssl-devel
yum update openssl
yum install net-tools 
yum install apr-devel apr-util-devel -y
```


## 安裝步驟

```bash
wget http://mirror.bit.edu.cn/apache//httpd/httpd-2.4.29.tar.gz
tar -xf httpd-2.4.29.tar.gz
cd httpd-2.4.29
./configure --prefi=/usr/local/apache2 --enable-so --enable-rewrite --enable-ssl 
make 
make install
/usr/local/apache2/bin/apachectl start
netstat -tnlp | grep 80
setenforce 0
firewall-cmd --add-port=80/tcp --permanent
systemctl reload firewalld.service
systemctl stop firewalld.service
```



## enable web site  
```bash
#/etc/apache2/sites-available
a2ensite <site>
```
## disable web site
```bash
#/etc/apache2/sites-available
a2dissite <site>

## reload apache
```bash
service apache2 reload
```

### Sample sites-available config 

```bash
#/etc/apache2/sites-available
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName leo-kwok.com
    ServerAlias phpmyadmin.leo-kwok.com
    DocumentRoot /usr/share/phpmyadmin2
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

<Directory /usr/share/phpmyadmin2>
    Options FollowSymLinks
    AllowOverride All
    Order deny,allow
    Allow from all
     DirectoryIndex index.html index.php

</Directory>

    ErrorDocument 403 /custom_403.html
    ErrorDocument 404 /custom_404.html
    ErrorDocument 500 /custom_50x.html
    ErrorDocument 502 /custom_50x.html
    ErrorDocument 503 /custom_50x.html
    ErrorDocument 504 /custom_50x.html

<IfModule security2_module>
    SecRuleEngine Off
</IfModule>
</VirtualHost>
```