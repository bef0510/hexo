---
title: Python 模組
date: 2019-07-08 09:53:03
tags:
 - Python Basic
categories:
 - Python
---

# module 函式
#### 基本運用
    # 內建 sys 模組取得資訊
    import sys
    print(sys.platform)
    print(sys.maxsize)

    # 使用別名
    import sys as system
    print(system.platform)
    print(system.maxsize)

## 自訂義模組，載入使用
新增 **geometry.py** 檔案
![Architecture](1.png)

#### 在 geometry 模組中定義幾何運算功能
    # 計算兩個點的距離
    def distance(x1, y1, x2, y2):
        return ((x2 - x1) ** 2 + (y2 - y1) ** 2) ** 0.5

    # 計算線段的斜率
    def slope(x1, y1, x2, y2):
        return (y2 - y1) / (x2 - x1)

#### 在主程式端 載入使用
    import geometry as geo

    result = geo.distance(1, 1, 5, 5)
    print(result)

    result = geo.slope(1, 2, 5, 6)
    print(result)

## 調整搜尋模組的路徑
1. 新增 **modules** 資料夾
![Architecture](2.png)

2. 將 **geometry.py** 檔案移至 **modules** 資料夾底下
![Architecture](3.png)

#### 在主程式端 新增路徑
    import sys
    sys.path.append("modules") # 在模組的搜尋路徑列表中 新增路徑
    print(sys.path) # 印出模組的搜尋路徑列表
    
    import geometry
    print(geometry.distance(1, 1, 5, 5))

印出的路徑可看到 **modules** 在最後
![Architecture](4.png)

#### 程式正常，但 import 時有紅色波浪覺得討厭解法
1. 紅色波浪
![Architecture](5.png)

2. 按組合鍵 **Ctrl+Shift+P** → 輸入 **Preferences:Configure language specific settings**
![Architecture](6.png)

3. 輸入 **TypeScript**
![Architecture](7.png)

4. **settings.json** 輸入 **"python.linting.pylintArgs": ["--disable-msg=C0103"]** → 存檔
![Architecture](8.png)

# Reference
[Python Module 模組的載入與使用 By 彭彭](https://www.youtube.com/watch?v=Et0DjY2cGiE)
[User and Workspace Settings](https://code.visualstudio.com/docs/getstarted/settings)
[Visual Studio Code中修改Pylint设置，及python版本切换](https://blog.csdn.net/yzh201612/article/details/68491707)