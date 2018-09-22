---
layout: post
title:  "AWS instance volume resize"
date:   2018-09-22 18:09:11 +0800
categories: AWS instance volume resize
---

# How to AWS instance volume resize

### In AWS console
- 1. ELASTIC BLOCK STORAGE /Volumes /Actions/ Modify Volume/Size
- 2. input Size value

### ssh  remote to ec2 instances 
AWS instance volume resize

```bash
 sudo resize2fs /dev/xvda1
```

### In AWS console
reboot ec2 instance 

### ssh  remote to ec2 instances 
AWS instance volume resize

```bash
df -h
```
```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            2.0G     0  2.0G   0% /dev
tmpfs           396M  5.8M  390M   2% /run
/dev/xvda1       39G  3.3G   36G   9% /   ##<------- resize to 40GB done!
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
tmpfs           2.0G   40K  2.0G   1% /var/spool/MailScanner/incoming
/dev/loop0       13M   13M     0 100% /snap/amazon-ssm-agent/295
/dev/loop1       13M   13M     0 100% /snap/amazon-ssm-agent/495
/dev/loop2       87M   87M     0 100% /snap/core/4830
/dev/loop4       17M   17M     0 100% /snap/amazon-ssm-agent/734
/dev/loop3       87M   87M     0 100% /snap/core/5145
/dev/loop5       88M   88M     0 100% /snap/core/5328
tmpfs           396M     0  396M   0% /run/user/1000
```