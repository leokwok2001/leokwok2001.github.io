# Docker installation


```bash

# 基于CentOS7.x（7.0+)
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

