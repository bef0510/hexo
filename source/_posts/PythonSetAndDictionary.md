---
title: Python Set 及 Dictionary
date: 2019-07-05 10:54:29
tags:
 - Python Basic
categories:
 - Python
---

# Set 集合的運算
#### 判斷是否在列表內 (in)
    s1 = {3, 4, 5}
    print(3 in s1)

    s1 = {3, 4, 5}
    print(10 in s1)

#### 判斷是否在列表內 (not in)
    s1 = {3, 4, 5}
    print(10 not in s1)

#### 交集
    s1 = {3, 4, 5}
    s2 = {4, 5, 6, 7}
    s3 = s1 & s2
    print(s3)

#### 聯集
    s1 = {3, 4, 5}
    s2 = {4, 5, 6, 7}
    s3 = s1 | s2
    print(s3)

#### 差集
    s1 = {3, 4, 5}
    s2 = {4, 5, 6, 7}
    s3 = s1 - s2
    print(s3)

#### 反交集
    s1 = {3, 4, 5}
    s2 = {4, 5, 6, 7}
    s3 = s1 ^ s2
    print(s3)

#### 使用 set 函式轉換成集合資料
    s = set("Hello")
    print(s)

#### 使用 set 函式 加判斷
    s = set("Hello")
    print("H" in s)

    s = set("Hello")
    print("A" in s)

    s = set("Hello")
    print("A" not in s)

# 字典的運算
#### 基本運用
    dic = {"name":"姓名", "age":"年齡"}
    print(dic)

#### 修改資料
    dic = {"name":"姓名", "age":"年齡"}
    dic["name"] = "暱稱"
    print(dic["name"])

#### 判斷 key 是否存在
    dic = {"name":"姓名", "age":"年齡"}
    print("name" in dic)

    dic = {"name":"姓名", "age":"年齡"}
    print("nickName" in dic)

    dic = {"name":"姓名", "age":"年齡"}
    print("nickName" not in dic)

#### 刪除字典中的鍵值對
    dic = {"name":"姓名", "age":"年齡"}
    print(dic)
    del dic["name"]
    print(dic)

#### 以列表的資料當基礎產生字典
    # dic = {x:x*2 for x in 列表}
    dic = {x:x*2 for x in [3, 4, 5]}
    print(dic)

# Reference
[Python 集合、字典的基本運算 - Set、Dictionary By 彭彭](https://www.youtube.com/watch?v=L3-KuGYhw78)