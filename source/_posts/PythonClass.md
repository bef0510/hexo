---
title: Python 類別
date: 2019-07-08 11:44:26
tags:
 - Python Basic
categories:
 - Python
---

# class 類別
1. 類別與類別屬性
2. 類別與實體物體、實體屬性、實體方法

## 類別與類別屬性
#### 基本運用
    # 定義一個類別 IO，有兩個屬性 supportedSrcs 和 read
    class IO:
        supportedSrcs = ["console", "file"]
        def read(src):
            print("Read from", src)

    # 使用類別
    print(IO.supportedSrcs)
    IO.read("file")

#### 加入判斷式
    class IO:
    supportedSrcs = ["console", "file"]
    def read(src):
        if src not in IO.supportedSrcs:
            print("Not Supported")
        else:
            print("Read from", src)

    print(IO.supportedSrcs)
    IO.read("internet")
    IO.read("file")

## 類別與實體物體、實體屬性
1. 基本定義
![Architecture](1.png)

2. 進階定義
![Architecture](2.png)

#### 基本運用
    class Point:
        # 初始化，construct 的概念
        def __init__(self):
            self.x = 3
            self.y = 4

    # 建立第一個實體物件
    p1 = Point()
    print(p1.x, p1.y)

    # 建立第二個實體物件
    p2 = Point()
    print(p2.x, p2.y)

#### 實體化物件時，給參數
    class Point:
        # 初始化，construct 的概念
        def __init__(self, firstName, lastName):
            self.firstName = firstName
            self.lastName = lastName

    p1 = Point("Joey", "Tribbiani")
    print(p1.firstName, p1.lastName)

    p2 = Point("Chandler", "Bing")
    print(p2.firstName, p2.lastName)

#### 加入實體化方法
    class Point:
        def __init__(self, x, y):
            self.x = x
            self.y = y

        def show(self):
            print(self.x, self.y)

        def distance(self, targetX, targetY):
            return(((self.x - targetX) ** 2) + ((self.y - targetY) ** 2)) ** 0.5
    
    p = Point(3, 4)
    p.show()
    result = p.distance(0, 0)
    print(result)

## 包裝檔案讀取的程式
建立兩個 **txt** 檔，任意輸入文字
![Architecture](3.png)

#### File 實體物件的設計
    class File:
        def __init__(self, name):
            self.name = name
            self.file = None # 尚末開啟檔案：初始值是 None

        def open(self):
            self.file = open(self.name, mode="r", encoding="utf-8")
    
        def read(self):
            return self.file.read()

    # 讀取第一個檔案
    f1 = File("data1.txt")
    f1.open()
    data = f1.read()
    print(data)

    # 讀取第二個檔案
    f2 = File("data2.txt")
    f2.open()
    data = f2.read()
    print(data)

# Reference
[Python Package 封包的設計與使用 By 彭彭](https://www.youtube.com/watch?v=GGp-7VHgsKk)
[Python 實體物件的建立與使用 - 上篇 - 實體屬性 Instance Attributes By 彭彭](https://www.youtube.com/watch?v=Lr5N2r1rmtM)
[Python 實體物件的建立與使用 - 下篇 - 實體方法 - Instance Methods By 彭彭](https://www.youtube.com/watch?v=MZtTClJ74NU)