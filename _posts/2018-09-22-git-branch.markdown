---
layout: post
title:  "Git Branch"
date:   2018-09-24 12:13:11 +0800
categories: Git Branch
---
[reference](https://backlog.com/git-tutorial/tw/)

### check branch status 

```bash
 git branch 
 # output
 #* master
```
### create branch 

```bash
git branch <branchname>
 or 
git checkout -b <branch>  ## create and change branch same time
```

### branch checkout 

```bash
 git checkout test1
```

### delete local branch 
```bash
git branch -D test1
```


### delete Remote Branch
```bash
git push origin --delete remote_branch
```

### merge branch
```bash
git checkout master
git merge tet1
##out put 
Updating 1257027..b2b23c4
Fast-forward
 myfile.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```
### Pull
執行 pull 命令可以取得遠端數據庫的歷史記錄，
執行 pull 時，如果內容沒有衝突，就會自動建立合併提交。如果發生衝突的話，需先解決衝突然後再手動提交。


### Fetch
執行pull，遠端數據庫的內容會自動合併。但是，有時候只是想確認遠端數據庫的內容卻不是真的想合併，在這種情況下，請使用 fetch。
執行 fetch，可以取得遠端數據庫的最新歷史記錄。取得的提交會導入在自動建立的分支中，並可以切換這個名為 FETCH_HEAD 的分支。

FETCH_HEAD 合併後，歷史記錄會和執行pull操作的結果相同。實際上，pull = fetch + merge的內容所組成的。