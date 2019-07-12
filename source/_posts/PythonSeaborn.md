---
title: Python 靜態資料視覺化 Seaborn 模組
date: 2019-07-12 16:56:16
tags:
 - Python Module
 - Data Visualization
categories:
 - Python
---

# 靜態資料視覺化 Seaborn 模組
1. 安裝模組 **pip install seaborn**
![Architecture](1.png)

2. 以 **matplotlib** 為基礎建構的高階繪圖套件

3. 圖形
    直方圖（Histogram）
    散佈圖（Scatter plot）
    線圖（Line plot）
    長條圖（Bar plot）
    盒鬚圖（Box plot）

#### 直方圖（Histogram）
    import seaborn as sns
    import numpy as np
    import matplotlib.pyplot as plt

    normal_samples = np.random.normal(size = 100000) # 生成 100000 組標準常態分配（平均值為 0，標準差為 1 的常態分配）隨機變數
    sns.distplot(normal_samples)

    plt.show()
![Architecture](2.png)

#### 散佈圖（Scatter plot）
    import seaborn as sns
    import pandas as pd
    import matplotlib.pyplot as plt

    speed = [4, 4, 7, 7, 8, 9, 10, 10, 10, 11, 11, 12, 12, 12, 12, 13, 13, 13, 13, 14, 14, 14, 14, 15, 15, 15, 16, 16, 17, 17, 17, 18, 18, 18, 18, 19, 19, 19, 20, 20, 20, 20, 20, 22, 23, 24, 24, 24, 24, 25]
    dist = [2, 10, 4, 22, 16, 10, 18, 26, 34, 17, 28, 14, 20, 24, 28, 26, 34, 34, 46, 26, 36, 60, 80, 20, 26, 54, 32, 40, 32, 40, 50, 42, 56, 76, 84, 36, 46, 68, 32, 48, 52, 56, 64, 66, 54, 70, 92, 93, 120, 85]

    cars_df = pd.DataFrame({
                "speed": speed,
                "dist": dist
            })

    sns.jointplot(x = "speed", y = "dist", data = cars_df)

    plt.show()
![Architecture](3.png)

#### 線圖（Line plot）
    import seaborn as sns
    import pandas as pd
    import matplotlib.pyplot as plt

    speed = [4, 4, 7, 7, 8, 9, 10, 10, 10, 11, 11, 12, 12, 12, 12, 13, 13, 13, 13, 14, 14, 14, 14, 15, 15, 15, 16, 16, 17, 17, 17, 18, 18, 18, 18, 19, 19, 19, 20, 20, 20, 20, 20, 22, 23, 24, 24, 24, 24, 25]
    dist = [2, 10, 4, 22, 16, 10, 18, 26, 34, 17, 28, 14, 20, 24, 28, 26, 34, 34, 46, 26, 36, 60, 80, 20, 26, 54, 32, 40, 32, 40, 50, 42, 56, 76, 84, 36, 46, 68, 32, 48, 52, 56, 64, 66, 54, 70, 92, 93, 120, 85]

    cars_df = pd.DataFrame({
                    "speed": speed,
                    "dist": dist
                })

    sns.factorplot(data = cars_df, x="speed", y="dist", ci = None)

    plt.show()
![Architecture](4.png)

#### 長條圖（Bar plot）
    import seaborn as sns
    import pandas as pd
    import matplotlib.pyplot as plt

    cyl = [6 ,6 ,4 ,6 ,8 ,6 ,8 ,4 ,4 ,6 ,6 ,8 ,8 ,8 ,8 ,8 ,8 ,4 ,4 ,4 ,4 ,8 ,8 ,8 ,8 ,4 ,4 ,4 ,8 ,6 ,8 ,4]
    cyl_df = pd.DataFrame({"cyl": cyl})

    sns.countplot(x = "cyl", data=cyl_df)

    plt.show()
![Architecture](5.png)

#### 盒鬚圖（Box plot）
    import seaborn as sns
    import numpy as np
    import matplotlib.pyplot as plt

    normal_samples = np.random.normal(size = 100000) # 生成 100000 組標準常態分配（平均值為 0，標準差為 1 的常態分配）隨機變數
    sns.boxplot(normal_samples)

    plt.show()
![Architecture](6.png)

# Reference
[[第 19 天] 資料視覺化（2）Seaborn](https://ithelp.ithome.com.tw/articles/10186624)