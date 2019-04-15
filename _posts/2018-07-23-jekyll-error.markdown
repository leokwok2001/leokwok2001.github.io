---
layout: post
title:  "jekyll"
date:   2018-07-22 10:45:11 +0800
categories: jekyll
comments: true
comments: true
---


## jekyll error 
```bash
C:\Users\yourname\xxxx.github.io>bundle exec jekyll serve
Configuration file: C:/Users/yourname/xxxx.github.io/_config.yml
  Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!
jekyll 3.8.2 | Error:  jekyll-paginate
```

## solution 
```bash
# in Gemfile add this two line 
gem "jekyll-sitemap"
gem "jekyll-paginate"
```

## jekyll install 
```bash
#https://jekyllrb.com/
#Quick-start Instructions
gem install bundler jekyll
jekyll new my-awesome-site
cd my-awesome-site
bundle exec jekyll serve
# => Now browse to http://localhost:4000
```

##  tempalte resource
- [jekyllthemes.org](http://jekyllthemes.org)
- [jekyllthemes.io](http://jekyllthemes.io)

## for DNS add CNAME file
```bash
bible.leo-kwok.com
```


## create a repro on github
### setting

![Image setting](https://blog.leo-kwok.com/images/github01.png)

![Image setting](https://blog.leo-kwok.com/images/github02.png)

## DNS on godaddy.com settings choose cname
```bash
# the project is store like this   yourname.github.io/repo
cname  bible  leokwok2001.github.io  1 Hour 
# done!
```
