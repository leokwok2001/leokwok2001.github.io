---
layout: post
title:  "Python Development Env"
date:   2018-05-01 17:00:11 +0800
categories: jekyll update1
---
## VM network configuration In windows 10
 1. wi fi properities / ICS (internet connection sharing)
 1. choose VMnet8(NAT) LAN card 

### In CentOs 7  

```bash
bashifup ens33
/etc/sysconfig/network-scripts/ifcfg-ens33
ONBOOT=yes
ifup  ens33
```

```bash
bashifup ens33
firwall-cmd --list-all
firwall-cmd --get-active-zones
firwall-cmd --zone=public --add-port=8000/tcp --permanet
```

[reference](https://janikarhunen.fi/how-to-install-python-3-6-1-on-centos-7.html)
## Install the necessary utilities
```bash
sudo yum update
sudo yum install yum-utils
sudo yum groupinstall development     
```

## Install Python 3.6.1

```bash
sudo yum install https://centos7.iuscommunity.org/ius-release.rpm
```
https://centos7.iuscommunity.org/ius-release.rpm


```bash
sudo yum install python36u
```

```bash
python3.6 -V
```

```bash
sudo yum install python36u-pip
sudo yum install python36u-devel
```

```bash
# This should return the system Python version
python –V
# output:
Python 2.7.5

# This should return the Python 3 version
python3.6 –V
# output:
Python 3.6.1
```

That’s it. Now I have Python 3.6 ready torun my apps! 

## Creating a virtualenv

```bash
python3.6 -m venv venv
```
To activate the virtualenv and start installing packages with pip:
```bash
. venv/bin/activate
pip install [package_name]
pip install -r requirements.txt
```