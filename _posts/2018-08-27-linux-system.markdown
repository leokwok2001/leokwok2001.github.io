---
layout: post
title:  "linux system "
date:   2018-08-27 10:45:11 +0800
categories: linux system
comments: true
---



## if /var/log/mail.log is empty or not work try:

```bash
#if /var/log/mail.log is empty or not work try:
service syslog restart 
#or 
sudo service rsyslog restart
```
## if system datatime incorrect
```bash
# if system datatime incorrect
sudo apt-get install ntp
tzselect # set time zone
sudo dpkg-reconfigure tzdata
sudo timedatectl set-ntp on
sudo timedatectl set-timezone Asia/Hong_Kong
sudo ntpq -p
```

### apache start / stop
```bash
# /etc/init.d/apache2 restart
OR
$ sudo /etc/init.d/apache2 restart

OR
$ sudo service apache2 restart
```
## change to root user
```bash
sudo -s
```

## firewall-cmd
```bash
firewall-cmd --reload

```
## restart apache
```bash
systemctl restart apache2
```