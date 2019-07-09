---
title: Python 網路爬蟲
date: 2019-07-09 11:59:56
tags:
 - Python Basic
categories:
 - Python
---

# 網路爬蟲
1. 盡可能讓程式模仿一般使用者的樣子
2. 使用內建的 **json** 模組
3. 瞭解基本 **HTML** 格式
![Architecture](1.png)
4. 使用第三方套件 BeautifulSoup 解析資料
    4.1 使用 PIP 套件管理工具
    4.2 pip install beautifulsoup4

#### 未模仿一般使用者 (錯誤範例)
    import urllib.request as req
    url = "https://www.ptt.cc/bbs/movie/index.html" # PTT 電影版
    with req.urlopen(url) as response:
        data = response.read().decode("utf-8")
    print(data)

出錯 403, 沒有權限訪問此站
![Architecture](2.png)

## 模仿一般使用者
1. **Chrome** 按 **F12** → **Network**
![Architecture](3.png)

2. 重新整理 → 拉到最上面點 **index.html**
![Architecture](4.png)

3. **Headers** → **Request Headers** → **User-agent** 複製值
![Architecture](5.png)

#### 抓取程式
    import urllib.request as req
    url = "https://www.ptt.cc/bbs/movie/index.html"
    request = req.Request(url, headers={
        "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36"
    })
    
    with req.urlopen(request) as response:
        data = response.read().decode("utf-8")
    print(data)

正常取得 **HTML**
![Architecture](6.png)

## 使用 BeautifulSoup
1. 安裝 **pip install beautifulsoup4**
![Architecture](7.png)

2. 安裝完成
![Architecture](8.png)

3. 要先 **import bs4**

#### 使用 BeautifulSoup 解析原始碼
    # 抓取網頁
    import urllib.request as req
    url = "https://www.ptt.cc/bbs/movie/index.html"
    request = req.Request(url, headers={
        "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36"
    })

    with req.urlopen(request) as response:
        data = response.read().decode("utf-8")

    # 解析原始碼
    import bs4
    root = bs4.BeautifulSoup(data, "html.parser")

    # html 裡的 <title> 值 </title>
    print(root.title) 
    print(root.title.string)

    # 尋找 class="title" 的 div 標籤
    titles = root.find("div", class_="title") 
    print(titles)
    print(titles.a.string) # <a> 值 </a>

    # 尋找所有 class="title" 的 div 標籤
    titles = root.find_all("div", class_="title") 
    print(titles) # 以列表方式找出 [<div> 值 </div>, <div> 值 </div>, <div> 值 </div>]

    # 一筆一筆印出值
    for title in titles:
        if title.a != None: # a 標籤沒有被刪除的資料
            print(title.a.string)

# Reference
[Python 網路爬蟲 Web Crawler 基本篇 By 彭彭](https://www.youtube.com/watch?v=9Z9xKWfNo7k)