---
layout: post
title:  "phpMyAdmin on Ubuntu 16.04"
date:   2018-09-19 10:45:11 +0800
categories: phpMyAdmin
comments: true
---

## How To Install and Secure phpMyAdmin on Ubuntu 

```bash
sudo apt-get update
sudo apt-get install phpmyadmin php-mbstring php-gettext

```

- For the server selection, choose apache2.
- Select yes when asked whether to use dbconfig-common to set up the database
- You will be prompted for your database administrator's password
- You will then be asked to choose and confirm a password for the phpMyAdmin application itself

The installation process actually adds the phpMyAdmin Apache configuration file into the /etc/apache2/conf-enabled/ directory, where it is automatically read.
The only thing we need to do is explicitly enable the PHP mcrypt and mbstring extensions, which we can do by typing:

```bash
sudo phpenmod mcrypt
sudo phpenmod mbstring

```
Afterwards, you'll need to restart Apache for your changes to be recognized:
```bash
sudo systemctl restart apache2
```

You can now access the web interface by visiting your server's domain name or public IP address followed by /phpmyadmin:
~~~
https://domain_name_or_IP/phpmyadmin
~~~

## Secure your phpMyAdmin Instance

We were able to get our phpMyAdmin interface up and running fairly easily. However, we are not done yet. Because of its ubiquity, phpMyAdmin is a popular target for attackers. We should take extra steps to prevent unauthorized access.
One of the easiest way of doing this is to place a gateway in front of the entire application. We can do this using Apache's built-in .htaccess authentication and authorization functionalities.

## Configure Apache to Allow .htaccess Overrides

First, we need to enable the use of .htaccess file overrides by editing our Apache configuration file.
We will edit the linked file that has been placed in our Apache configuration directory:
```bash
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
```

vi /etc/apache2/conf-available/phpmyadmin.conf
```bash
<Directory /usr/share/phpmyadmin>
    Options FollowSymLinks
    DirectoryIndex index.php
    AllowOverride All   ## <-----**
    . . .
```

When you have added this line, save and close the file.
To implement the changes you made, restart Apache:

```bash
sudo systemctl restart apache2
```
## Create an .htaccess File
```bash
sudo vi /usr/share/phpmyadmin/.htaccess
```

/usr/share/phpmyadmin/.htaccess
```bash
AuthType Basic
AuthName "Restricted Files"
AuthUserFile /etc/phpmyadmin/.htpasswd
Require valid-user
```

Let's go over what each of these lines mean:
- AuthType Basic: This line specifies the authentication type that we are implementing. This type will implement password authentication using a password file.
- AuthName: This sets the message for the authentication dialog box. You should keep this generic so that unauthorized users won't gain any information about what is being protected.
- AuthUserFile: This sets the location of the password file that will be used for authentication. This should be outside of the directories that are being served. We will create this file shortly.
- Require valid-user: This specifies that only authenticated users should be given access to this resource. This is what actually stops unauthorized users from entering.


## Create the .htpasswd file for Authentication

The location that we selected for our password file was "/etc/phpmyadmin/.htpasswd". We can now create this file and pass it an initial user with the htpasswd utility:

```bash
sudo htpasswd -c /etc/phpmyadmin/.htpasswd username
```

You will be prompted to select and confirm a password for the user you are creating. Afterwards, the file is created with the hashed password that you entered.
If you want to enter an additional user, you need to do so without the -c flag, like this:
```bash
sudo htpasswd /etc/phpmyadmin/.htpasswd additionaluser
```

After entering the Apache authentication, you'll be taken to the regular phpMyAdmin authentication page to enter your other credentials. This will add an additional layer of security since phpMyAdmin has suffered from vulnerabilities in the past.

## Conclusion
You should now have phpMyAdmin configured and ready to use on your Ubuntu 16.04 server. Using this interface, you can easily create databases, users, tables, etc., and perform the usual operations like deleting and modifying structures and data.