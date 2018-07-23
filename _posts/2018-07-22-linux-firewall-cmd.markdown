---
layout: post
title:  "firewall-cmd"
date:   2018-07-22 10:45:11 +0800
categories: firewall-cmd
---


[ref](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)
[ref](https://firewalld.org/documentation/howto/reload-firewalld.html)

```bash
sudo firewall-cmd --zone=public --permanent --add-port=5000/tcp
sudo firewall-cmd --zone=public --permanent --add-port=4990-4999/udp
sudo firewall-cmd --zone=public --permanent --list-ports
sudo firewall-cmd  --reload # restart the firewall
```


