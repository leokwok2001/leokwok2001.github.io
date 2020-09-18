---
layout: post
title:  "Google cloud platform Docker MSSQL "
date:   2020-09-18 12:13:11 +0800
categories: Google cloud platform
comments: true
---
# Reference
[SSH for Google Cloud Platform](https://chrome.google.com/webstore/detail/ssh-for-google-cloud-plat/ojilllmhjhibplnppnamldakhpmdnibd)
[Docs.Microsoft](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash)
[Install Docker](https://docs.docker.com/engine/install/)

# MSSQL 2019 Requirment 
- Memory	2 GB
- File System	XFS or EXT4 (other file - systems, such as BTRFS, are unsupported)
- Disk space	2 GB
- Processor speed	2 GHz
- Processor cores	2 cores
- Processor type	x64-compatible only

# Install Docker method
## Uninstall old versions
```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```
## Set Up The REPOSITORY

```bash
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
## Add Docker’s official GPG key:
```bash
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
```

## add repository
```bash
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

```

## INSTALL DOCKER ENGINE
```bash
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## Verify that Docker Engine is installed correctly

```bash
$ sudo docker run hello-world
```
# Install MSSQL container

## Pull and run the 2019 container image
```bash
sudo docker pull mcr.microsoft.com/mssql/server:2019-latest
```

## To run the container image with Docker, 
```bash
$ sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" 
   -p 1433:1433 --name sql1 -h sql1 
   -d mcr.microsoft.com/mssql/server:2019-latest
```

## To view your Docker containers, use the docker ps command.
```bash
$ sudo docker ps -a
```

## config the GCP firewall to allow Tcp 1433 port access MSSQL for external.