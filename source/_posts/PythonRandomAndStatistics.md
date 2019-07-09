---
title: Python 隨機模組、統計模組
date: 2019-07-09 10:25:58
tags:
 - Python Basic
categories:
 - Python
---

# 隨機模組
要先 **import random**

#### 從列表中隨機選取
    import random
    data = random.choice([1, 5, 6, 10, 20])
    print(data)

#### 從列表中隨機選取 N 筆資料，不能大於列表長度
    data = random.sample([1, 5, 6, 10, 20], 3)
    print(data)

#### 隨機調換順序，洗牌的概念
    data = [1, 5, 8, 20]
    random.shuffle(data)
    print(data)

#### 0 ~ 1 之間的隨機亂數
    data = random.random()
    print(data)

#### 自訂義亂數
    data = random.uniform(0.0, 1.0) # 0.0 ~ 1.0 之間的隨機亂數
    print(data)
    
    data = random.uniform(60, 100) # 60 ~ 100 之間的隨機亂數
    print(data)

#### 取得常態分配亂數
    # 平均數 100，標準差 10，多數落在 90 ~ 110 之間
    data = random.normalvariate(100, 10)
    print(data)

    # 平均數 0，標準差 5，多數落在 -5 ~ 5 之間
    data = random.normalvariate(0, 5)
    print(data)

# 統計模組
要先 **import statistics**

#### 平均數
    import statistics as stat
    data = stat.mean([1, 4, 5, 8])
    print(data)
    
    data = stat.mean([1, 2, 3, 4, 5, 8, 100])
    print(data)

#### 中位數
    data = stat.median([1, 2, 3, 4, 5, 8, 100])
    print(data)

#### 標準差
    data = stat.stdev([1, 2, 3, 4, 5, 8, 100])
    print(data)
    
    data = stat.stdev([1, 2, 3, 4, 5, 8, 10])
    print(data)

# Reference
[Python 亂數與統計模組 By 彭彭](https://www.youtube.com/watch?v=-xwCu6PN1jU)