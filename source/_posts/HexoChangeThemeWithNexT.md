---
title: Hexo 更換主題 NexT
date: 2019-04-26 11:39:07
tags: 
 - Hexo
categories: 
 - Blog
---

# 更換主題
### 安裝 NexT
使用 **Git checkout**
``` bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

### config 設定
根目錄 `_config.yml` 修改 **theme**
{% note default %}
theme: next
{% endnote %}

### Schemes 設定
`themes/next/_config.yml` 修改 **Schemes**
{% note default %}
#Schemes
&emsp;&emsp;scheme: Muse
&emsp;&emsp;#scheme: Mist
&emsp;&emsp;#scheme: Pisces
&emsp;&emsp;#scheme: Gemini
{% endnote %}

### menu 設定
將需要的打開
{% note default %}
menu:
&emsp;&emsp;home: / || home
&emsp;&emsp;#about: /about/ || user
&emsp;&emsp;tags: /tags/ || tags
&emsp;&emsp;categories: /categories/ || th
&emsp;&emsp;archives: /archives/ || archive
&emsp;&emsp;#schedule: /schedule/ || calendar
&emsp;&emsp;#sitemap: /sitemap.xml || sitemap
&emsp;&emsp;#commonweal: /404/ || heartbeat
{% endnote %}
 
### sidebar 設定
{% note default %}
sidebar:
&emsp;&emsp;position: left
{% endnote %}

### 新增 menu 對應資料
增加 **tags**，在 `/source/` 下產生相對應資料
``` bash
$ hexo new page "tags"
```
{% note default %}
修改 `title: 標籤`
/source/tags/index.md 加入 `type: "tags"`
{% endnote %}

增加 **categories**，在 `/source/` 下產生相對應資料
``` bash
$ hexo new page "categories"
```
{% note default %}
修改 `title: 分類`
/source/categories/index.md 加入 `type: "categories"`
{% endnote %}

### 新增文章
``` bash
$ hexo new post "Title-1"
```

# 參考
[Hexo](https://hexo.io/zh-tw/docs/)
[NexT](https://theme-next.iissnan.com/)
[Hexo - 前端也能建置部落格！更換主題與發表文章篇](https://ithelp.ithome.com.tw/articles/10207997)