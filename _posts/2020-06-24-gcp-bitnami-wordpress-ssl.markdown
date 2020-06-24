---
layout: post
title:  "Free SSL for WordPress on Google Cloud (Bitnami)"
date:   2020-06-24 12:13:11 +0800
categories: GCP
comments: true
---
#Setup Free SSL for WordPress on Google Cloud (Bitnami)

```bash
sudo /opt/bitnami/letsencrypt/scripts/generate-certificate.sh -m leo@cavo-net.com -d YOURDOMAIN.COM -d www.YOURDOMAIN.COM 
```