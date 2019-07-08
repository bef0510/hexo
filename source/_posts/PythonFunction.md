---
title: Python 函式
date: 2019-07-08 09:21:15
tags:
 - Python Basic
categories:
 - Python
---

# function 函式
#### 基本運用
    # 定義函式
    def multiply():
        print(3 * 4)

    # 呼叫函式
    multiply()

#### 基本運用 (帶參數)
    def multiply(n1, n2):
        print(n1 * n2)

    multiply(3, 4)

#### 基本運用 (回傳值)
    def calculate(max):
        sum = 0
        for n in range(1, max + 1):
            sum += n
        return sum
    
    value = calculate(20)
    print(value)

#### 參數的預設資料
    def power(base, exp = 0):
        print(base ** exp)
    
    power(3, 2)
    power(4)

#### 使用參數名稱對應
    def divide(n1, n2):
        print(n1 / n2)
    
    divide(2, 4)
    divide(n2=2, n1=4)

#### 無限參數資料
    def avg(*ns):
        sum = 0
        for n in ns:
            sum += n
        print(sum / len(ns))
    
    avg(3, 4)
    avg(3, 5, 10)
    avg(1, 4, -1, -8)

# Reference
[Python 函式基礎：定義並呼叫函式 By 彭彭](https://www.youtube.com/watch?v=7qKFvUVpQXg)