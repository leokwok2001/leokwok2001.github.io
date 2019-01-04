---
layout: post
title:  "wordpress"
date:   2018-10-26 0:45:11 +0800
categories: wordpress 
comments: true
---


problem installtion failed: Download failed. cCURL error 35: error:0D0C50A1:asn1 encoding routines:ASN1_item_verify:unknow message digest alogrithm...
EDIT the /wp-includes/class-http.php - change SSL noted change in code  below
```bash

 sslverify=>false

```

pugin woocommerence clear all install data

Edit /wp-confg.php
```bash
define( 'WC_REMOVE_ALL_DATA', true);
```