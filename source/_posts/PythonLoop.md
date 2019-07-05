---
title: Python while 及 for 迴圈
date: 2019-07-05 12:09:43
tags:
 - Python Basic
categories:
 - Python
---

# while 迴圈
#### 基本運用
    n = 1
    sum = 0
    while n <= 10:
        sum = sum + n
        n += 1
    print(sum)

#### while + break
    n = 0
    while n < 5:
        if n == 3:
            break
        print(n)
        n += 1
    print("last n：", n)

# for 迴圈
#### 基本運用
    for x in [3, 5, 1]:
        print(x)

    for x in "Hello":
        print(x)

    for x in range(5):  # 0 ~ 4
        print(x)

    for x in range(5, 10): # 5 ~ 9
        print(x)

    sum = 0
    for x in range(1, 11):
        sum = sum + x
    print(sum)

#### for + continue
    n = 0
    for x in [0, 1, 2, 3]:
        if x % 2 == 0:
            continue
        print(x)
        n += 1
    print("last n：", n)

#### for + else
    sum = 0
    for n in range(1, 11):
        sum += n
    else:
        print(sum)

#### 綜合範列：找出整數平方根
    # 輸入 9，得到 3
    # 輸入 11， 得到 沒有 平方根
    
    n = int(input("輸入一個正整數："))
    for i in range(n): # i 從 0 ~ n - 1
        if i * i == n:
            print("整數平方根", i)
            break # 用 break 強制結束迴圈時，不會執行 else 區塊
    else:
        print("沒有整數平方根")

# Reference
[Python 流程控制：迴圈進階控制，break、continue、else 命令 By 彭彭](https://www.youtube.com/watch?v=yBXlwOmLqZ4)