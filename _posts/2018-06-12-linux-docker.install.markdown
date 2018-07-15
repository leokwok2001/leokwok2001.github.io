---
layout: post
title:  "Docker installation"
date:   2018-06-14 14:45:11 +0800
categories: Docker installation
---




```bash

# 基于CentOS7.x（7.0+)
sed -i 'SELINUX/s/enforcing/disabled/g' /etc/selinux/config
set enfoce 0
yum install -y epel-release
yum install -y docker*
```




```bash
apt-get update  
#ifconfig  
apt install net-tools        
#ping 
apt install iputils-ping  
#vim
apt install vim
```



```bash
# stop all docker process
docker stop $(docker ps -a -q)
# remove all docker 
docker rm $(docker ps -a -q)
```

