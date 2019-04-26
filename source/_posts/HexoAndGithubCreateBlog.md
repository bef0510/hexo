---
title: 使用 Hexo 及 GitHub 建立 Blog
date: 2019-04-26 11:38:54
tags: 
 - Hexo
 - GitHub
categories: 
 - Blog
---

# 順序

1. 基本安基
2. **GitHub** 帳號注冊及新增一個 **Repository**
3. **Hexo** 建立
4. **Hexo** + **GitHub**
5. 部署

# 基本安基
### 安裝 Node
[Node.js](https://nodejs.org/en/) 下載並安裝

### 安裝 Git
[Git for Windows](https://git-scm.com/download/win) 下載並安裝

### 安裝 Hexo
透過 **npm** 安裝 **Hexo** (出錯的地方，無視它)
``` bash
$ npm install -g hexo-cli
```
# GitHub 新增 Repository
1. 開啟 [github.com](https://github.com/)
2. 建立一個 **Repository**
![New Repository](1.png)
3. 輸入自己的名字 `username`，後綴 `.github.io`
![Create Repository](2.png)

# Hexo 建立
``` bash
$ hexo init 文件名稱
$ cd 文件名稱
$ npm install
$ npm install hexo-deployer-git --save
```

# GitHub + Hexo
### 本機鏈結 GitHub
1. 在新建的目錄按右鍵，開啟 **git bash**
``` bash
$ git config --global user.name "name"
$ git config --global user.email "email"
```
2. 生成 **SSH** 密鑰
   輸入完指令後，連按三次 Enter
``` bash
$ ssh-keygen -t rsa -C "email"
```
3. 密鑰加至 **ssh-agent**
``` bash
$ eval "$(ssh-agent -s)"
```
4. 生成的 **SSH** 加至 **ssh-agent**
``` bash
$ ssh-add ~/.ssh/id_rsa
```
5. 開啟 [github.com](https://github.com/)
   點擊頭像下 **Settings**
![Settings](3.png)
6. 點擊左側 **SSH and GPG keys**
   點擊 **New SSH key**
![SSHandGPG](4.png)
7. 顯示 **SSH key** 及複製
``` bash
$ cat ~/.ssh/id_rsa.pub
```
8. 將 **SSH key** 貼至 **GitHub SSH key** 裡， `Title` 任意打
![Inputkey](5.png)
9. 確認 **GitHub** 鏈結成功
``` bash
$ ssh -T git@github.com
```
![GithubConnect](6.png)

### Hexo config 修改
1. 根目錄下開啟 **_config.yml**
修改最後一行的配置，如下
{% note default %}
deploy:
&emsp;type: git
&emsp;repository: https://github.com/username/repository.github.io
&emsp;branch: master
{% endnote %}
2. 設定 `Title`、`author`、`language`
修改 **_config.yml**
{% note default %}
title: your title
author: your name
language: zh-tw
{% endnote %}
3. `image` 相對應路徑
修改 **_config.yml**
{% note default %}
post_asset_folder: true
{% endnote %}
``` bash
$ npm install https://github.com/CodeFalling/hexo-asset-image --save
```

# 部署
1. **Commit Git**
``` bash
$ echo "# repository.github.io" >> README.md
$ git init
$ git add .
$ git commit -m "first commit"
$ git remote add origin git@github.com:username/repository.github.io.git
$ git push -u origin master
```
2. 清除快取檔案 **(db.json)** 和已產生的靜態檔案 **(public)**
``` bash
$ hexo clean
```
3. 產生靜態檔案
``` bash
$ hexo g 
```
4. 啟動伺服器
{% note default %}
預設是 `http://localhost:4000/`
{% endnote %}
``` bash
$ hexo s
```
5. 部署網站
``` bash
$ hexo d
```
6. **Blog URL**
{% note default %}
https://repository.github.io/
{% endnote %}

# 參考文章
[Hexo](https://hexo.io/zh-tw/docs/)
[超详细Hexo+Github博客搭建小白教程](https://godweiyang.com/2018/04/13/hexo-blog/)
[使用 Hexo + github 建立Blog](https://alumincan.github.io/2017/03/28/setup-blog-on-github/)