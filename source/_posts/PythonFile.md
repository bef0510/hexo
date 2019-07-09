---
title: Python 文字檔案讀取與儲存
date: 2019-07-09 09:25:04
tags:
 - Python Basic
categories:
 - Python
---

# 文字檔案讀取與儲存
1. 基本定義
![Architecture](1.png)

## 文字檔案
新增一個 **data.txt** 檔
![Architecture](2.png)

#### 基本運用
    file = open("data.txt", mode="w", encoding="utf-8") # 開啟
    file.write("Hello File") # 基本操作
    file.write("Hello File\nSecond Line\n") # 換行
    file.write("測試中文") # 中文一定要設 encoding
    file.close() # 關閉

#### 建議寫法，自動關閉檔案 (省略 .close())
    with open("data.txt", mode="w", encoding="utf-8") as file:
        file.write("測試中文\nSecond Line")

#### 省略 mode
    with open("data.txt", "w", encoding="utf-8") as file:
        file.write("測試中文\nSecond Line")

#### 讀取檔案
    with open("data.txt", "r", encoding="utf-8") as file:
        data = file.read()
    print(data)

#### 把檔案中的數字資料，一行一行的讀出來，計算總合
    # 先寫入
    with open("data.txt", "w", encoding="utf-8") as file:
        file.write("5\n3")

    # 讀出及計算
    sum = 0
    with open("data.txt", "r", encoding="utf-8") as file:
        for line in file:
            sum += int(line)
    print(sum)

## json 檔案
新增一個 **config.json** 檔，及輸入 **json** 字串
![Architecture](3.png)

要先 **import json**

#### 基本運用
    import json
    with open("config.json", "r") as file:
        data = json.load(file)
    
    print(data) # data 是一個 Dictionary
    print(data["name"])
    print(data["version"])

#### 讀取及寫入檔案
    import json
    with open("config.json", "r") as file:
        data = json.load(file)

    print(data)
    data["name"] = "New Name" # 修改變數中的資料

    # 把更新的資料複寫回檔案中
    with open("config.json", "w") as file:
        json.dump(data, file)

# Reference
[Python 文字檔案的讀取和儲存 By 彭彭](https://www.youtube.com/watch?v=C4OkV6DrVRs)