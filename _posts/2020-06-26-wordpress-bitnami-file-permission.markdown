---
layout: post
title:  "Wordpress bitnami file permission"
date:   2020-06-24 12:13:11 +0800
categories: Wordpress
comments: true
---



Files and directories are owned by user bitnami and group daemon.
Directories are configured with permissions 775 by default.
Files are configured with permissions 664 by default.
The wp-config.php file is configured with permissions 640.
If permissions are wrong, use the chmod or chown commands to restore them to their initial state. For example, if TARGET is the WordPress application folder:

```bash
sudo chown -R bitnami:daemon TARGET
sudo find TARGET -type d -exec chmod 775 {} \;
sudo find TARGET -type f -exec chmod 664 {} \;
sudo chmod 640 TARGET/wp-config.php
```