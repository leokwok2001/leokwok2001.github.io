
# 編譯安裝 apache 2.4.29


## 編譯安裝 apache 前必先安裝的軟体
```bash
yum install openssl-devel
yum update openssl
yum install net-tools 
yum install apr-devel apr-util-devel -y
```


## 安裝步驟

```bash
wget http://mirror.bit.edu.cn/apache//httpd/httpd-2.4.29.tar.gz
tar -xf httpd-2.4.29.tar.gz
cd httpd-2.4.29
./configure --prefi=/usr/local/apache2 --enable-so --enable-rewrite --enable-ssl 
make 
make install
/usr/local/apache2/bin/apachectl start
netstat -tnlp | grep 80
setenforce 0
firewall-cmd --add-port=80/tcp --permanent
systemctl reload firewalld.service
systemctl stop firewalld.service
```