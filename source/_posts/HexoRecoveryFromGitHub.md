---
title: Hexo 從 GitHub 還原
date: 2019-04-26 13:12:02
tags: 
 - Hexo
 - GitHub
categories: 
 - Blog
---

# 基本安裝
### 確認 已安裝
[Node.js](https://nodejs.org/en/)
[Git for Windows](https://git-scm.com/download/win)
``` bash
$ npm install -g hexo-cli
```

或
``` bash
$ npm version
$ git version
$ hexo version
```

# Hexo 建立
``` bash
$ hexo init 文件名稱
$ cd 文件名稱
$ npm install
$ npm install hexo-deployer-git --save
```

# 取出 GitHub 檔案
1. **clone** 一份到 **temp**
``` bash
$ git clone https://github.com/username/repository temp
```
2. **copy temp** 裡的檔案至新建立的文件名稱裡，除了 **.git**
3. 刪除 **temp** 檔

# 參考文章
[Hexo部署环境迁移--多台电脑搭建Hexo环境](http://whatbeg.com/2016/09/17/hexo-migrate.html)
[Hexo－還原](http://lonka.github.io/blog/2016/04/27/hexo/hexo-revert/)