---
title: Python Number 及 String
date: 2019-07-05 09:59:05
tags:
 - Python Basic
categories: 
 - Python
---

# Number 數字運算
#### 加法
    x = 3 + 6
    print(x)

#### 減法
    x = 3 - 6
    print(x)

#### 乘法
    x = 3 * 6
    print(x)

#### 除法 (小數)
    x = 6 / 7
    print(x)

#### 除法 (整數)
    x = 6 // 7
    print(x)

#### 次方
    x = 2 ** 3

#### 開根號
    x = 2 ** 0.5
    print(x)

#### 取餘數
    x = 7 % 3
    print(x)

#### 簡寫
    x += 1 # x = x + 1
    print(x)

    x -= 1 # x = x - 1
    print(x)

    x *= 2 # x = x * 2
    print(x)

    x /= 2 # x = x / 2
    print(x)

# String 字串運算
#### 基本運用
    s = "Hello"
    print(s)

#### 跳脫
    s = "Hell\"o" # 跳脫
    print(s)

#### 字串相加 (有加號)
    s = "Hello" + "World"
    print(s)

#### 字串相加 (省略加號)
    s = "Hello" "World"
    print(s)

#### 換行，使用 \n
    s = "Hello\nWorld"
    print(s)

#### 換行
    s = """Hello

    World"""
    print(s)

#### 連續加 N 次
    s = "Hello" * 3
    print(s)

#### 連續加 N 次後，加上其他值
    s = "Hello" * 3 + "World"
    print(s)

#### 用索引取字元
    print(s[1])
    s = "Hello"

#### 用索引加起迄 (包含起，不包含迄)
    print(s[1:4])
    s = "Hello"

#### 用索引加起迄 (省略迄)
    print(s[1:])
    s = "Hello"

#### 用索引加起迄 (省略起)
    print(s[:4])
    s = "Hello"

# Reference
[Python 數字、字串的基本運算 By 彭彭](https://www.youtube.com/watch?v=bLRa4TZ99aY)