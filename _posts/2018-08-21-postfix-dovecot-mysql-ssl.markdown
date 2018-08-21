---
layout: post
title:  "postfix dovecot ssl mysql"
date:   2018-08-13 0:45:11 +0800
categories: postfix dovecot ssl mysql
---
## mail server ubuntu (Squirrelmail postfix courier pop imap)

### 1. check the hostname 
```bash
hostname -f
```
```bash
vi /etc/hostname
```
```
mail.leo-kwok.com
```

### 2. change hosts

```bash
vi /etc/hosts
```
```
vi /etc/hosts
127.0.0.1 localhost
127.0.1.1 mail.leo-kwok.com
```
### 3. MySQL
```bash
apt-get install mysql-server
#input root password
```
```bash
#login mysql server with root user
mysql -u root -p
#create database 
create database mailserver character set utf8;
create user mailserver@'localhost' identified by 'mailserver123';
grant all on mailserver.* to mailserver@'localhost' identified by 'mailserver123';
#exit mysql
exit;
mysql -u mailserver -p
#give password 
use mailserver;

#create table virtual_domains
CREATE TABLE `virtual_domains` (  
  `id` int(11) NOT NULL auto_increment,  
  `name` varchar(50) NOT NULL,  
  PRIMARY KEY (`id`))  
  ENGINE=InnoDB DEFAULT CHARSET=utf8;

#create table virtual_users
CREATE TABLE `virtual_users` (  
`id` int(11) NOT NULL auto_increment,  
`domain_id` int(11) NOT NULL,  
`password` varchar(106) NOT NULL,  
`email` varchar(100) NOT NULL,  
PRIMARY KEY (`id`),  
UNIQUE KEY `email` (`email`),  
FOREIGN KEY (domain_id) REFERENCES virtual_domains(id) ON DELETE CASCADE)  
ENGINE=InnoDB DEFAULT CHARSET=utf8;

#create virtual_aliases
CREATE TABLE `virtual_aliases` (  
`id` int(11) NOT NULL auto_increment,  
`domain_id` int(11) NOT NULL,  
`source` varchar(100) NOT NULL,  
`destination` varchar(100) NOT NULL,  
PRIMARY KEY (`id`),  
FOREIGN KEY (domain_id) REFERENCES virtual_domains(id) ON DELETE CASCADE)  
ENGINE=InnoDB DEFAULT CHARSET=utf8
# insert data  virtual_domains
insert into virtual_domains(id,name) values(1,'mail.mydomain.com');     
insert into virtual_domains(id,name) values(2,'mydomain.com');

#insert data virtual_users
insert into virtual_users(id,domain_id,password,email)  
values (1,2,ENCRYPT('123456', CONCAT('$6$', SUBSTRING(SHA(RAND()), -16))),'test1@leo-kwok.com');

insert into virtual_users(id,domain_id,password,email)  
values (2,2,ENCRYPT('123456', CONCAT('$6$', SUBSTRING(SHA(RAND()), -16))),'test2@leo-kwok.com');

#insert data virtual_aliases
insert into virtual_aliases(id,domain_id,source,destination)  
values (1,1,'all@leo-kwok.com','test1@leo-kwok.com');

insert into virtual_aliases(id,domain_id,source,destination)  
values (1,1,'all@leo-kwok.com','test2@leo-kwok.com');
#if somebody email to all@leo-kwok.com , will forward mail to 
#test1@leo-kwok.com , test2@leo-kwok.com

#select mysql result
select * from virtual_domains;  
select * from virtual_users;  
select * from virtual_aliases;

```
### Postfix的配置 

```bash
apt-get install postfix postfix-mysql

#choose
#Internet Site：

# backup main.cf file
cp /etc/postfix/main.cf /etc/postfix/main.cf_backup_20150511


```
remark below lines
```bash
# TLS parameters  
  #smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem  
  #smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key  #smtpd_use_tls=yes  
  #smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache  
  #smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
```
insert below lines
```bash
smtpd_tls_cert_file=/etc/dovecot/dovecot.pem  
smtpd_tls_key_file=/etc/dovecot/private/dovecot.pem  
smtpd_use_tls=yes  
smtpd_tls_auth_only = yes  

#Enabling SMTP for authenticated users, and handing off authentication to Dovecot  
smtpd_sasl_type = dovecot  
smtpd_sasl_path = private/auth  
smtpd_sasl_auth_enable = yes  
smtpd_recipient_restrictions =  permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination
```
insert below lines
```bash
mydestination =mydestination = localhost, mail.leo-kwok.com, localhost.$mydomain
```
insert below lines for LDA「Local Delivery Agent」
```bash
#Handing off local delivery to Dovecot's LMTP, and telling it where to store mail  
virtual_transport = lmtp:unix:private/dovecot-lmtp

```
insert below lines for postfix to mysql
```bash
#Virtual domains, users, and aliases  
virtual_mailbox_domains = mysql:/etc/postfix/mysql-virtual-mailbox-domains.cf  
virtual_mailbox_maps = mysql:/etc/postfix/mysql-virtual-mailbox-maps.cf  
virtual_alias_maps = mysql:/etc/postfix/mysql-virtual-alias-maps.cf
```


```bash
# /etc/postfix/mysql-virtual-mailbox-domains.cf
user = mailserver   
password = mailserver123  
hosts = 127.0.0.1  
dbname = mailserver  
query = SELECT 1 FROM virtual_domains WHERE name='%s'
```
```bash
# restart postfix
service postfix restart
```
```bash
# test result if return 1 done!
postmap -q leo-kwok.com mysql:/etc/postfix/mysql-virtual-mailbox-domains.cf
```

```bash
#/etc/postfix/mysql-virtual-mailbox-maps.cf
user = mailserver    
password = mailserver123  
hosts = 127.0.0.1  
dbname = mailserver  
query = SELECT 1 FROM virtual_users WHERE email='%s'
```
```bash
# restart postfix
service postfix restart
```
```bash
# test result if return 1 done!
postmap -q test1@leo-kwok.com mysql:/etc/postfix/mysql-virtual-mailbox-maps.cf
```

```bash
# /etc/postfix/mysql-virtual-alias-maps.cf
user = mailserver    
password = mailserver123  
hosts = 127.0.0.1  
dbname = mailserver  
query = SELECT destination FROM virtual_aliases WHERE source='%s'
```
```bash
# restart postfix
service postfix restart
```
```bash
# test result if return 1 done!
postmap -q all@leo-kwok.com mysql:/etc/postfix/mysql-virtual-alias-maps.cf
```

/etc/postfix/master.cf
1 找到submission和smtps所在的兩行，並將其註釋去掉。
這樣做的目的是允許Postfix通過587和465端口發送郵件

```bash
# restart postfix
service postfix restart
```

### Dovecot 
```bash
apt-get install dovecot-core dovecot-impd dovecot-pop3d dovecot-lmtpd dovecot-mysql
```

/etc/dovecot/dovecot.confDovecot的主配置文檔
/etc/dovecot/conf.d/10-mail.confDovecot將要操作的磁盤路徑相關配置信息
/etc/dovecot/conf.d/10-auth.conf用户驗證相關配置信息
/etc/dovecot/conf.d/auth-sql.conf.extSQL-Type驗證相關配置信息
/etc/dovecot/dovecot-sql.conf.extDovecot與數據庫連接相關配置信息
/etc/dovecot/conf.d/10-master.confDovecot本地socket相關配置信息
/etc/dovecot/conf.d/10-ssl.conf關於SSL的相關配置信息

```bash
# /etc/dovecot/dovecot.conf
!include conf.d/*.conf

# Enable installed  
include_try /usr/share/dovecot/protocols.d/*.protocol  
protocols = imap pop3 lmtp
```

```bash
# /etc/dovecot/conf.d/10-mail.conf
mail_location = maildir:/var/mail/vhosts/%d/%n
mail_privileged_group = mail
```

```bash
# look up the rights
ls -ld /var/mail
#drwxrwsr-x 2 root mail 4096 May  11 15:08 /var/mail
```

```bash
# /var/mail/vhosts/
mkdir -p /var/mail/vhosts/leo-kwok.com
```

```bash
groupadd -g 5000 vmail  
useradd -g vmail -u 5000 vmail -d /var/mail
```

```bash
chown -R vmail:vmail /var/mail
```


```bash
# /etc/dovecot/conf.d/10-auth.conf
disable_plaintext_auth = yes
auth_mechanisms = plain login
```

默認情況下，Dovecot是允許Ubuntu系統用户登錄使用的，我們需要將其禁用。找到文檔種如下內容並將其註釋
```bash
#!include auth-system.conf.ext
```
開啟Dovecot的MySQL支持，取消!include auth-sql.conf.ext的註釋符號：
```bash
#!include auth-system.conf.ext  
!include auth-sql.conf.ext  
#!include auth-ldap.conf.ext  
#!include auth-passwdfile.conf.ext  
#!include auth-checkpassword.conf.ext  
#!include auth-vpopmail.conf.ext  
#!include auth-static.conf.ext
```
