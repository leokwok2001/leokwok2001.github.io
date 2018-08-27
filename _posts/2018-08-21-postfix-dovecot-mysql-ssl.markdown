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
修改/etc/dovecot/conf.d/auth-sql.conf.ext文檔
```bash
passdb {  
    driver = sql  
    args = /etc/dovecot/dovecot-sql.conf.ext  
}  

userdb {  
    driver = static  
    args = uid=vmail gid=vmail home=/var/mail/vhosts/%d/%n  
}
```
修改/etc/dovecot/dovecot-sql.conf.ext文檔
```bash
driver = mysql
```

取消文檔中connect行的註釋，並將其修改為如下：
```bash
connect = host=127.0.0.1 dbname=mailserver user=mailserver password=mailserver123
```
取消文檔中default_pass_scheme行的註釋，並將其修改為如下：
```bash
default_pass_scheme = SHA512-CRYPT
```
取消文檔中password_query行的註釋，並將起修改為如下：
```bash
password_query = SELECT email as user, password FROM virtual_users WHERE email='%u';
```
保存退出
在命令行種輸入如下內容以修改目錄權限：
```bash
chown -R vmail:dovecot /etc/dovecot
chmod -R o-rwx /etc/dovecot
```
```bash
修改/etc/dovecot/conf.d/10-master.conf文檔
打開文檔做如下修改「通過將端口設置為0，以禁用非SSL加密的IMAP和POP3協議」：

service imap-login {  
    inet_listener imap {  
        port = 0   
    }  
    ...  
}  

service pop3-login {  
    inet_listener pop3 {  
        port = 0  
    }  
    ...  
}
```

找到文檔中的service lmtp並將其修改如下：.
```bash
service lmtp {  
        unix_listener /var/spool/postfix/private/dovecot-lmtp {  
        mode = 0600  
        user = postfix  
        group = postfix  
  }  

  # Create inet listener only if you can't use the above UNIX socket  
  #inet_listener lmtp {  
        #Avoid making LMTP visible for the entire internet  
        #address =  
        #port =  
        #}  
 }

```
找到文檔中service auth並將其內容修改如下：
```bash
service auth {  
    # auth_socket_path points to this userdb socket by default. It's typically  
    # used by dovecot-lda, doveadm, possibly imap process, etc. Its default  
    # permissions make it readable only by root, but you may need to relax these  
    # permissions. Users that have access to this socket are able to get a list  
    # of all usernames and get results of everyone's userdb lookups.  

    unix_listener /var/spool/postfix/private/auth {  
            mode = 0666  
            user = postfix  
            group = postfix  
    }  

    unix_listener auth-userdb {  
            mode = 0600  
            user = vmail  
            #group =  
    }  

    # Postfix smtp-auth  
    #unix_listener /var/spool/postfix/private/auth {  
    #       mode = 0666  
    #}  

    # Auth process is run as this user.  
    user = dovecot  
}
```

找到文檔中service auth-worker內容並修改如下：
```bash
service auth-worker {  
    # Auth worker process is run as root by default, so that it can access  
    # /etc/shadow. If this isn't necessary, the user should be changed to  
    # $default_internal_user.  

    user = vmail  
}
```
}
修改/etc/dovecot/conf.d/10-ssl.conf文檔
找到文檔中ssl_cert並修改內容如下「請確保dovecot的pem文檔已經存在，如果大家使用了自己的SSL文檔，請將如下內容修改為相應的路徑」：

```bash
ssl_cert = </etc/dovecot/dovecot.pem  
ssl_key = </etc/dovecot/private/dovecot.pem
```
強制用户使用SSL加密：
```bash
ssl = required
```

重新啟動Dovecot服務：
```bash
service dovecot restar
```
### 測試郵件服務器是否正常
設置一個帳號，可以向MySQL對應的表中插入數據，接下來使用郵件客户端來測試一下。推薦使用Thunderbird郵件客户端或者Foxmail客户端等。請注意以下內容：
郵箱的全稱「包括後面的@mydomain.com」將作為用户名
郵箱密碼是MySQL數據庫種對應郵箱的密碼
服務器相關接口是否全部開放？993、995、25等
郵件收發的所有協議，包括IMAP、POP3、SMTP全部都需要開啟SSL加密
配置好客户端之後即可連接獲取、發送郵件。

### remarks
```bash
openssl req -new -x509 -days 3650 -nodes -out /etc/postfix/ssl/cacert.pem -keyout /etc/postfix/ssl/server.key
```
```bash
#/etc/postfix/main.cf
smtpd_tls_key_file = /etc/postfix/ssl/server.key
smtpd_tls_cert_file = /etc/postfix/ssl/cacert.pem
smtpd_tls_CAfile = /etc/postfix/ssl/cacert.pem
```
```bash
#/etc/dovecot/conf.d/10-ssl.conf
ssl_cert = </etc/postfix/ssl/cacert.pem
ssl_key = </etc/postfix/ssl/server.key
#ssl_ca =  </etc/postfix/ssl/cacert.pem
```
## full configure files 

### /etc/postfix/main.cf  full version
```bash
# See /usr/share/postfix/main.cf.dist for a commented, more complete version


# Debian specific:  Specifying a file name will cause the first
# line of that file to be used as the name.  The Debian default
# is /etc/mailname.
#myorigin = /etc/mailname

smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
biff = no

# appending .domain is the MUA's job.
append_dot_mydomain = no

# Uncomment the next line to generate "delayed mail" warnings
#delay_warning_time = 4h

readme_directory = no

# TLS parameters
#smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
#smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
#smtpd_use_tls=yes
#smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
#smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
#smtpd_tls_cert_file=/etc/dovecot/dovecot.pem  
smtpd_tls_cert_file = /etc/postfix/ssl/cacert.pem

#smtpd_tls_key_file=/etc/dovecot/private/dovecot.pem  
smtpd_tls_key_file = /etc/postfix/ssl/server.key
smtpd_tls_CAfile = /etc/postfix/ssl/cacert.pem
smtpd_use_tls=yes
smtpd_tls_auth_only = yes



# See /usr/share/doc/postfix/TLS_README.gz in the postfix-doc package for
# information on enabling SSL in the smtp client.


smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
smtpd_sasl_auth_enable = yes
smtpd_recipient_restrictions =  permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination


smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination
myhostname = mail.leo-kwok.com
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
myorigin = /etc/mailname
#mydestination = $myhostname, mail.leo-kwok.com, localhost.leo-kwok.com, , localhost

mydestination = localhost, mail.leo-kwok.com, localhost.$mydomain # <--this must have
#Handing off local delivery to Dovecot's LMTP, and telling it where to store mail  
virtual_transport = lmtp:unix:private/dovecot-lmtp



#Virtual domains, users, and aliases  
virtual_mailbox_domains = mysql:/etc/postfix/mysql-virtual-mailbox-domains.cf
virtual_mailbox_maps = mysql:/etc/postfix/mysql-virtual-mailbox-maps.cf
virtual_alias_maps = mysql:/etc/postfix/mysql-virtual-alias-maps.cf
relayhost =
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 10.0.0.0/26
mailbox_size_limit = 0
recipient_delimiter = +
inet_interfaces = all
inet_protocols = ipv4
#compatibility_level = 2
```

### /etc/dovecot/conf.d/10-ssl.conf
```bash
ssl_cert = </etc/postfix/ssl/cacert.pem
ssl_key = </etc/postfix/ssl/server.key
#ssl_ca =  </etc/postfix/ssl/cacert.pem
```
## restart service command 
```bash
systemctl enable saslauthd.service
systemctl restart salsauthd.service
systemctl restart dovecot.service
systemctl restart postfix.service
systemctl status postfix
# or 
service postfix restart
service dovecot restart
```

## Firewall Access 
You will need to set your firewall(s) to allow access to the following ports:
~~~
SMTP: 25
POP3: 110
IMAP: 143
SMTP Secure: 465
MSA: 587
IMAP Secure: 993
POP3 Secure: 995
~~~
```bash
sudo dovecot -n 
ufw allow  22
ufw allow  993
ufw allow 995
```