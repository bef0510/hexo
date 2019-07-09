---
title: Python 抓取網頁及 WebApi 資料
date: 2019-07-09 11:27:09
tags:
 - Python Basic
categories:
 - Python
---

# 抓取網頁
要先 **import urllib.request**

基本語法
![Architecture](1.png)

#### 取得網頁原始碼
    import urllib.request as request
    src = "https://www.ntu.edu.tw/"
    with request.urlopen(src) as response:
        data = response.read().decode("utf-8") # 因為有中文，要 decode
    print(data)

# call WebApi
要先 **import urllib.request**
因為 **json** 格式，要先 **import json**

1. [找一個公開的 **api**，例：行政院環境保護署](https://opendata.epa.gov.tw/)
![Architecture](2.png)

2. 選一個 **web api**
![Architecture](3.png)
![Architecture](4.png)
![Architecture](5.png)

3. 複製網址
![Architecture](6.png)

4. 開啟分頁 → 貼上網址 → 按 **Enter**
![Architecture](7.png)

#### 基本運用
    import urllib.request as request
    import json
    src = "https://opendata.epa.gov.tw/api/v1/PM25?%24skip=0&%24top=1000&%24format=json" # 貼上剛剛複製的綱址
    with request.urlopen(src) as response:
        data = json.load(response) # 利用 json 模組處理 json 資料格式

    print(data)

#### 一筆一筆印出資料
    import urllib.request as request
    import json
    src = "https://opendata.epa.gov.tw/api/v1/PM25?%24skip=0&%24top=1000&%24format=json"
    with request.urlopen(src) as response:
        data = json.load(response)
    
    for _data in data:
        print(_data["SiteName"])

#### 將資料寫入檔案
    import urllib.request as request
    import json
    src = "https://opendata.epa.gov.tw/api/v1/PM25?%24skip=0&%24top=1000&%24format=json"
    with request.urlopen(src) as response:
        data = json.load(response)

    with open("data.txt", "w", encoding="utf-8") as file:
        for _data in data:
            file.write(_data["SiteName"] + "\n")

產生 **data.txt** 檔
![Architecture](8.png)

# Reference
[Python 網路連線程式、公開資料串接 By 彭彭](https://www.youtube.com/watch?v=sUzR3QVBKIo)