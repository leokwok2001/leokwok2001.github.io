---
layout: post
title:  "MailScanner Spamassasin ClamAV"
date:   2018-09-19 10:45:11 +0800
categories: MailScanner Spamassasin ClamAV
---

# MailScanner Spamassasin ClamAV install

## download mailscanner
[MailScanner Spamassasin ClamAV 1](http://www.linuxmail.info/mailscanner-postfix-clamav-spamassassin-howto-ubuntu-10-04/)
[MailScanner Spamassasin ClamAV 2](https://blog.csdn.net/zstack_org/article/details/71514332)

```bash
wget https://s3.amazonaws.com/msv5/release/MailScanner-5.1.1-1.deb.tar.gz
cd ~
tar -zvf MailScanner-5.1.1-1.deb.tar.gz
cd MailScanner-5.1.1-1
source install.sh
```

## Edit the file /etc/MailScanner/virus.scanners.conf and change the path of clamav to /usr..


## Edit the file /etc/MailScanner/MailScanner.conf and update the lines below..
```bash
%org-name% = your organization name
%org-long-name% = your full organization name
%web-site% = your mail support website or company website
Run As User = postfix
Run As Group = postfix
Incoming Queue Dir = /var/spool/postfix/hold
Outgoing Queue Dir = /var/spool/postfix/incoming
Incoming Work Group = clamav
Incoming Work Permissions = 0640
MTA = postfix
#Virus Scanners = clamd
Virus Scanners = clamav  ## <---*** use clamav instand of clamd this to  remove the permission error!!**
Use SpamAssassin = yes
SpamAssassin User State Dir = /var/spool/MailScanner/spamassassin
```

## Edit the file /etc/mailScanner/defaults and update the line below 
```bash
run_mailscanner=1
```

## Edit the file /usr/sbin/mailscanner and add a -U parameter on the first line..

# Integrating MailScanner into Postfix

## Edit the file /etc/postfix/main.cf and add the line below 
```bash
header_checks = regexp:/etc/postfix/header_checks
```

##  Create the file /etc/postfix/header_checks and add the line below 

```bash
/^Received:/ HOLD
```
## This will now place all incoming mail into the holding area until released by MailScanner. .

## Use the commands below to start MailScanner
```bash
sudo service postfix restart
sudo service clamav-daemon start
sudo service mailscanner start
```