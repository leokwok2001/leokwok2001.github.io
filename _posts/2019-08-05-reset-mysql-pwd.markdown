---
layout: post
title:  "Reset Mysql Password"
date:   2019-08-05 12:13:11 +0800
categories: Mysql
comments: true
---


[Reset the MySQL 5.7 root password in Ubuntu 16.04 LTS](https://coderwall.com/p/j9btlg/reset-the-mysql-5-7-root-password-in-ubuntu-16-04-lts)


```bash
# Stop MySQL
sudo service mysql stop
# Make MySQL service directory.
sudo mkdir /var/run/mysqld
# Give MySQL user permission to write to the service directory.
sudo chown mysql: /var/run/mysqld
# Start MySQL manually, without permission checks or networking.
sudo mysqld_safe --skip-grant-tables --skip-networking &
# Log in without a password.
mysql -uroot mysql
```
Update the password for the root user.
```bash
UPDATE mysql.user SET authentication_string=PASSWORD('YOURNEWPASSWORD'), plugin='mysql_native_password' WHERE User='root' AND Host='%';
#UPDATE mysql.user SET authentication_string=PASSWORD('YOURNEWPASSWORD'), plugin='mysql_native_password' WHERE User='root' AND Host='localhost';
EXIT;
```

```bash
# Turn off MySQL.
sudo mysqladmin -S /var/run/mysqld/mysqld.sock shutdown
# Start the MySQL service normally.
sudo service mysql start
```