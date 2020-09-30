---
layout: post
title:  "how to setup Putty ssh tunnel "
date:   2020-09-30 12:13:11 +0800
categories: Putty
comments: true
---

[ref](http://cyrilwang.blogspot.com/2013/12/ssh-tunnel-mysql_28.html)

# 1. in Options controlling SSH port forwarding.
```bash
#for example
Source port :5001
Destination: 192.168.0.1:1433
```
# 2. in remote client option
```bash
#for example, such as microsoft sql server management studio.

server : 127.0.0.1,5001
logn: sa
pwd: ****
```
