---
layout: post
title:  "sublime text 3.1.1 Build 3176"
date:   2018-07-15 11:45:11 +0800
categories: SublimeText3
comments: true
---

### In hosts file
C:\Windows\System32\drivers\etc\hosts
Windows - SystemRoot > system32 > drivers > etc > hosts
Linux - /etc/hosts
Mac OS X - /private/etc/hosts


```bash
* Added these lines into  /etc/hosts 

127.0.0.1       www.sublimetext.com
127.0.0.1       license.sublimehq.com
```



### license key
----- BEGIN LICENSE -----

sgbteam

Single User License

EA7E-1153259

8891CBB9 F1513E4F 1A3405C1 A865D53F

115F202E 7B91AB2D 0D2A40ED 352B269B

76E84F0B CD69BFC7 59F2DFEF E267328F

215652A3 E88F9D8F 4C38E3BA 5B2DAAE4

969624E7 DC9CD4D5 717FB40C 1B9738CF

20B3C4F1 E917B5B3 87C38D9C ACCE7DD8

5F7EF854 86B9743C FADC04AA FB0DA5C0

F913BE58 42FEA319 F954EFDD AE881E0B

------ END LICENSE ------


###  sftp configure file in sftp-config.json 


```bash

{
    // The tab key will cycle through the settings when first created
    // Visit http://wbond.net/sublime_packages/sftp/settings for help
    
    // sftp, ftp or ftps
    "type": "sftp",

    "save_before_upload": true,
    "upload_on_save": true,
    "sync_down_on_open": false,
    "sync_skip_deletes": false,
    "sync_same_age": true,
    "confirm_downloads": false,
    "confirm_sync": true,
    "confirm_overwrite_newer": false,
    
    "host": "123.123.123.123",
    "user": "USERNAME",
    //"password": "PASSWORD",
    "port": "22",
    
    "remote_path": "/var/www/YOUR_REMOTE_FOLDER/",
    "ignore_regexes": [
        "\\.sublime-(project|workspace)", "sftp-config(-alt\\d?)?\\.json",
        "sftp-settings\\.json", "/venv/", "\\.svn/", "\\.hg/", "\\.git/",
        "\\.bzr", "_darcs", "CVS", "\\.DS_Store", "Thumbs\\.db", "desktop\\.ini"
    ],
    //"file_permissions": "664",
    //"dir_permissions": "775",
    
    //"extra_list_connections": 0,

    //"connect_timeout": 30,
    //"keepalive": 120,
    //"ftp_passive_mode": true,
    //"ftp_obey_passive_host": false,
    "ssh_key_file": "C://Users//leo//Desktop//YOUR_KEY_FILE.ppk",
    //"sftp_flags": ["-F", "/path/to/ssh_config"],
    
    //"preserve_modification_times": false,
    //"remote_time_offset_in_hours": 0,
    "remote_encoding": "utf-8",
    //"remote_locale": "C",
    //"allow_config_upload": false,
}


```
