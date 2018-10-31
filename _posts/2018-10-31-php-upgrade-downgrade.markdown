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