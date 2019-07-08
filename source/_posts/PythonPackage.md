---
title: Python 封包
date: 2019-07-08 11:13:33
tags:
 - Python Basic
categories:
 - Python
---

# package 封包
1. 檔案配置
![Architecture](1.png)

2. 建立一個資料夾 **geometry** 及三個檔案 **\__init\__.py** 、**line.py** 、**point.py** 及一個主程式 **main.py**
![Architecture](2.png)

3. **\__init\__.py** 這個檔案一定要，讓 **python** 知道這個資料夾是一個封包，裡面不一定要有程式

#### line.py 寫入下方 code
    # 計算兩個點的距離
    def line(x1, y1, x2, y2):
        return (((x2 - x1) ** 2) + ((y2 - y1) ** 2)) ** 0.5

    # 計算線段的斜率
    def slope(x1, y1, x2, y2):
        return (y2 - y1) / (x2 - x1)

#### point.py 寫入下方 code
    # 平面座標與原點距離
    def distance(x, y):
        return (x ** 2 + y ** 2) ** 0.5

#### 主程式引用
    import geometry.point as point
    result = point.distance(3, 4)
    print("距離", result)

    import geometry.line as line
    result = line.slope(1, 1, 3, 3)
    print("斜率", result)

# Reference
[ython Package 封包的設計與使用 By 彭彭](https://www.youtube.com/watch?v=GGp-7VHgsKk)