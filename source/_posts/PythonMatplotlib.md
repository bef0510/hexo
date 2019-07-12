---
title: Python 靜態資料視覺化 Matplotlib 模組
date: 2019-07-12 16:24:03
tags:
 - Python Module
 - Data Visualization
categories:
 - Python
---

# 靜態資料視覺化 Matplotlib 模組
1. 安裝模組 **pip install matplotlib**
![Architecture](1.png)

2. 圖形
    直方圖（Histogram）
    散佈圖（Scatter plot）
    線圖（Line plot）
    長條圖（Bar plot）
    盒鬚圖（Box plot）

#### 直方圖（Histogram）
    import numpy as np
    import matplotlib.pyplot as plt

    normal_samples = np.random.normal(size = 100000) # 生成 100000 組標準常態分配（平均值為 0，標準差為 1 的常態分配）隨機變數
    uniform_samples = np.random.uniform(size = 100000) # 生成 100000 組介於 0 與 1 之間均勻分配隨機變數

    plt.hist(normal_samples)
    plt.show()
    plt.hist(uniform_samples)
    plt.show()
![Architecture](2.png)
![Architecture](3.png)

#### 散佈圖（Scatter plot）
    import matplotlib.pyplot as plt

    speed = [4, 4, 7, 7, 8, 9, 10, 10, 10, 11, 11, 12, 12, 12, 12, 13, 13, 13, 13, 14, 14, 14, 14, 15, 15, 15, 16, 16, 17, 17, 17, 18, 18, 18, 18, 19, 19, 19, 20, 20, 20, 20, 20, 22, 23, 24, 24, 24, 24, 25]
    dist = [2, 10, 4, 22, 16, 10, 18, 26, 34, 17, 28, 14, 20, 24, 28, 26, 34, 34, 46, 26, 36, 60, 80, 20, 26, 54, 32, 40, 32, 40, 50, 42, 56, 76, 84, 36, 46, 68, 32, 48, 52, 56, 64, 66, 54, 70, 92, 93, 120, 85]

    plt.scatter(speed, dist)
    plt.show()
![Architecture](4.png)

#### 線圖（Line plot）
    import matplotlib.pyplot as plt

    speed = [4, 4, 7, 7, 8, 9, 10, 10, 10, 11, 11, 12, 12, 12, 12, 13, 13, 13, 13, 14, 14, 14, 14, 15, 15, 15, 16, 16, 17, 17, 17, 18, 18, 18, 18, 19, 19, 19, 20, 20, 20, 20, 20, 22, 23, 24, 24, 24, 24, 25]
    dist = [2, 10, 4, 22, 16, 10, 18, 26, 34, 17, 28, 14, 20, 24, 28, 26, 34, 34, 46, 26, 36, 60, 80, 20, 26, 54, 32, 40, 32, 40, 50, 42, 56, 76, 84, 36, 46, 68, 32, 48, 52, 56, 64, 66, 54, 70, 92, 93, 120, 85]

    plt.plot(speed, dist)
    plt.show()
![Architecture](5.png)

#### 長條圖（Bar plot）
    from matplotlib.ticker import FuncFormatter
    import matplotlib.pyplot as plt
    import numpy as np

    x = np.arange(4)
    money = [1.5e5, 2.5e6, 5.5e6, 2.0e7]

    def millions(x, pos):
        'The two args are the value and tick position'
        return '$%1.1fM' % (x * 1e-6)


    formatter = FuncFormatter(millions)

    fig, ax = plt.subplots()
    ax.yaxis.set_major_formatter(formatter)
    plt.bar(x, money)
    plt.xticks(x, ('Bill', 'Fred', 'Mary', 'Sue'))
    plt.show()
![Architecture](6.png)

#### 盒鬚圖（Box plot）
    import numpy as np
    import matplotlib.pyplot as plt

    normal_samples = np.random.normal(size = 100000) # 生成 100000 組標準常態分配（平均值為 0，標準差為 1 的常態分配）隨機變數

    plt.boxplot(normal_samples)
    plt.show()
![Architecture](7.png)

# Reference
[[第 18 天] 資料視覺化 matplotlib](https://ithelp.ithome.com.tw/articles/10186484)
[Custom Ticker1](https://matplotlib.org/3.1.1/gallery/ticks_and_spines/custom_ticker1.html#sphx-glr-gallery-ticks-and-spines-custom-ticker1-py)