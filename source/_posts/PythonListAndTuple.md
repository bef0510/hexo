---
title: Python List 及 Tuple
date: 2019-07-05 10:27:07
tags:
 - Python Basic
categories:
 - Python
---

# List 有序可變動列表
#### 基本運用
    grades = [12, 60, 15, 70, 90]
    print(grades)

#### 用索引取元素
    grades = [12, 60, 15, 70, 90]
    print(grades[3])

#### 變更列表元素值
    grades = [12, 60, 15, 70, 90]
    grades[0] = 55  # 把 55 放到列表中的第一個位置
    print(grades)

#### 用索引加起迄 (包含起，不包含迄)
    grades = [12, 60, 15, 70, 90]
    print(grades[1:4])

#### 刪除列表中的資料
    grades = [12, 60, 15, 70, 90]
    grades[1:4] = []    # 連續刪除列表中 1 ~ 4 (不包含)
    print(grades)

#### 列表加上資料
    grades = [12, 60, 15, 70, 90]
    grades = grades + [12, 33]
    print(grades)

#### 取列表長度
    grades = [12, 60, 15, 70, 90]
    length = len(grades) # 取長度
    print(length)

#### 列表中再放列表 (二維陣列)
    listData = [[3, 4, 5], [6, 7, 8]]
    print(listData[0])
    print(listData[0][0])

#### 變更列表元素值 (二維陣列)
    listData = [[3, 4, 5], [6, 7, 8]]
    print(listData)
    listData[0][0:2] = [5, 5, 5]
    print(listData)

# Tuple 有序不可變動列表 (使用方法，基本上跟 List一樣，差別是不可更改值)
#### 用索引加起迄 (包含起，不包含迄)
    tupleData = (3, 4, 5)
    print(tupleData[0:2])

#### 錯誤式範
    tupleData = (3, 4, 5)
    tupleData[0] = 5    # 錯誤：Tuple 資料不可變動
    print(tupleData[0:2])

# Reference
[Python 有序列表的基本運算 - List、Tuple By 彭彭](https://www.youtube.com/watch?v=JLU5oc4_VtA)