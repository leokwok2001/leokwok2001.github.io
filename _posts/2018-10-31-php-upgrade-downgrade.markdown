---
layout: post
title:  "php downgrade upgrade "
date:   2018-10-31 0:45:11 +0800
categories: htaccess 
comments: true
---

[ref:](https://www.digitalocean.com/community/questions/how-to-downgrade-php7-to-php5-x)

## From php5.6 to php7.0:
```bash
#Apache:
sudo a2dismod php5.6 ; sudo a2enmod php7.0 ; sudo service apache2 restart
#CLI:
sudo update-alternatives --set php /usr/bin/php7.0
```


## From php7.0 to php5.6:
```bash
#Apache:
sudo a2dismod php7.0 ; sudo a2enmod php5.6 ; sudo service apache2 restart
#CLI:
sudo update-alternatives --set php /usr/bin/php5.6
```


## problem may be happen in phpmyadmin, missing mbstring, solve below:
```bash
sudo apt-get install php5.6-mbstring
sudo apt-get install php5.6-mysql
sudo service apache2 restart
```


## php安装错误 (node.c:1953:error dereferencing pointer to incomplete type) 解决办法

make: *** [ext/dom/node.lo] Error 1
在国外的一个网站上找到了这个，其他版本的php也同样适用。
```bash
 curl -o php-5.x.x.patch https://mail.gnome.org/archives/xml/2012-August/txtbgxGXAvz4N.txt
 cd php-5.x.x
 patch -p0 -b < ./php-5.x.x.patch 

#   patching file ext/dom/node.c

#   patching file 

#   ext/dom/documenttype.c

#   patching file ext/simplexml/simplexml.c
```
再次编译即可。


## php.ini Loaded Configuration File :none  觖決方法

在linux下可以使用bash命令脚本：
```bash
sudo find / -name 'php.ini'
```


实在没有办法，只能使用strace去追踪一下：
```bash
strace  /usr/local/php/bin/php -i 2> /tmp/1.log
```
此时，发现问题，php竟然去/usr/local/php/bin/ 下去找php.ini了。


