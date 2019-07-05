---
title: Python 判斷式
date: 2019-07-05 11:29:22
tags:
 - Python Basic
categories:
 - Python
---

# 判斷式
目前只有 if，沒有 switch

#### 基本判斷
    if True:
        print("true")
    else:
        print("false")

#### 抓取輸入值判斷
    x = input("輸入數字：")
    x = int(x)

    if x > 200:
        print("大於 200")
    elif x > 100:
        print("大於 100， 小於等於 200")
    else:
        print("小於等於 100")

#### 四則運算
    n1 = int(input("輸入數字一："))
    n2 = int(input("輸入數字二："))
    op = input("輸入運算：+, -, *, /")

    if op == "+":
        print(n1 + n2)
    elif op == "-":
        print(n1 - n2)
    elif op == "*":
        print(n1 * n2)
    elif op == "/":
        print(n1 / n2)
    else:
        print("沒有這運算符號")

#### 三元運算子
    # 條件為真, 反回真 否則 反回假
    # condition_is_true if condition else condition_is_false

    is_fat = True
    state = "fat" if is_fat else "not fat"
    print(state)

    #(反回假, 反回真)[真或假]
    #(if_test_is_false, if_test_is_true)[test]
    
    fat = False
    fitness = ("skinny", "fat")[fat]
    print("Ali is", fitness)

# Reference
[Python 流程控制：if 判斷式 By 彭彭](https://www.youtube.com/watch?v=A93BsHB-lWo)