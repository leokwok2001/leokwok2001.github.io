---
layout: post
title:  "Setup SendGrid Domain Authentication on Cloudflare"
date:   2020-09-14 12:13:11 +0800
categories: Wordpress
comments: true
---

## Cloudflare side
Login to your console and click "DNS" tab. Here we'll add those CNAME records.

One thing you do need to pay attention is that because Cloudflare is a CDN, when you add a CNAME entry, by default it'll proxy it. Make sure you toggle the arrow-cloud button to tell Cloudflare to not proxy it; only treat it as "DNS only". Otherwise you'll get DNS Validation Error (Code: 1004) and won't be able to add the records.