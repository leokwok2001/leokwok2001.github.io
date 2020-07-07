---
layout: post
title:  "Wordpress bitnami remove logo"
date:   2020-06-29 12:13:11 +0800
categories: Wordpress
comments: true
---

# Remove banner
sudo /opt/bitnami/apps/APPNAME/bnconfig --disable_banner 1

#Apache restart
sudo /opt/bitnami/ctlscript.sh restart apache

# NGINX restart
sudo /opt/bitnami/ctlscript.sh restart nginx

# Wordpres path
/opt/bitnami/apps/wordpress/htdocs#
