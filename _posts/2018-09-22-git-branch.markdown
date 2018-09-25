---
layout: post
title:  "Git Branch"
date:   2018-09-24 12:13:11 +0800
categories: Git Branch
comments: true
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
### create remote branch
```bash
git push --set-upstream origin 20180924
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

### pull remote git branch
```bash
git checkout --track origin/<branch name>
```
--track is shorthand for git checkout -b [branch] [remotename]/[branch] where [remotename] is origin in this case and [branch] is twice the same, daves_branch in this case.

For git 1.5.6.5 you needed this: 
```bash
git checkout --track -b daves_branch origin/daves_branch
```
For git 1.7.2.3 and higher this is enough (might have started earlier but this is the earliest confirmation I could find quickly):
git checkout daves_branch

Note that with recent git versions, this command will not create a local branch and will put you in a 'detached HEAD' state. If you want a local branch, use the --track option. Full details here: http://git-scm.com/book/en/v2/Git-Branching-Remote-Branches#Tracking-Branches

OR;  
```bash
git fetch <remote> <RemoteBranch>:<LocalBranch> 
git checkout <LocalBranch>
```

### git stash 
如果工作目錄未完成, 但又想轉去其他工作區, 可以用 stash 暫存起來
```bash
git satsh # stage 區的檔案暫存起來 git add 左的檔案
git stash -u # 包括不在stage 區 的檔案
git stash save -u "我是註解" # 加注解
git stash list  #我們先來查詢看暫存是否還在
git stash pop #檔案又回來了，又可以繼續未完成的工作囉！
```

### Pull
執行 pull 命令可以取得遠端數據庫的歷史記錄，
執行 pull 時，如果內容沒有衝突，就會自動建立合併提交。如果發生衝突的話，需先解決衝突然後再手動提交。


### Fetch
執行pull，遠端數據庫的內容會自動合併。但是，有時候只是想確認遠端數據庫的內容卻不是真的想合併，在這種情況下，請使用 fetch。
執行 fetch，可以取得遠端數據庫的最新歷史記錄。取得的提交會導入在自動建立的分支中，並可以切換這個名為 FETCH_HEAD 的分支。

FETCH_HEAD 合併後，歷史記錄會和執行pull操作的結果相同。實際上，pull = fetch + merge的內容所組成的。




### 如果github, gitlab stored password 有問題可以試删除 Credential Manager內容

若要開認證管理員，請選取 [開始]  按鈕然後搜尋 [控制台]。
選取 [使用者帳戶] > [認證管理員] (Credential Manager)。 您可以從這個畫面管理 Web 認證和Windows 認證。

