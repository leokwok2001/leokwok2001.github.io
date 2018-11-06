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

## kill process by PID
It's a single line, but you'd need to put it into a shell alias or shell script in order to make it easy to use:
```bash
 kill $(lsof -i tcp:8080 | tail -n +2 | awk '{ print $2 }')
```
If you want to see and kill processes that don't belong to you, then sudo needs to get involved:
```bash
 sudo kill $(sudo lsof -i tcp:8080 | tail -n +2 | awk '{ print $2 }')
```

## ubuntu javascript default path
~~~
/usr/share/javascript 
~~~