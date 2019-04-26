---
title: Git 常用指令
date: 2019-04-26 13:57:44
tags:
 - GitHub
categories: 
 - Git
---

1. 設定 git (第一次使用)
~~~ bash
$ git config --global user.name "userName"
$ git config --global user.email "userEmail"
~~~
2. 設定遠端路徑
~~~ bash
$ git remote add origin git@github.com:username/repository.git
~~~
3. 查看變更狀態
~~~ bash
$ git status
~~~
4. 放棄變更檔案
~~~ bash
$ git checkout 檔案名
~~~
5. 查看 **commit** 紀錄
~~~ bash
$ git log
~~~
6. 檔案加入版控 (單一及所有)
~~~ bash
$ git add 檔案名稱
$ git add .
~~~
7. **commit** (先 **add** 後才能 **commit**)
~~~ bash
$ git commit -m "first commit"
~~~

# Reference
[git](https://git-scm.com/docs/user-manual.html)
[新手專用的git常用指令](https://ithelp.ithome.com.tw/articles/10156301)