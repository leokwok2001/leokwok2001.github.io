---
layout: post
title:  "jekyll error"
date:   2018-07-22 10:45:11 +0800
categories: jekyll error
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
