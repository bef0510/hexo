---
title: Python Numpy 模組
date: 2019-07-09 15:19:04
tags:
 - Python Module
categories:
 - Python
---

# Numpy 模組
1. 安裝模組 **pip install numpy**
![Architecture](1.png)
2. **Numpy** 的 **ndarray** 支援多維度陣列；**python** 的 **array** 只支援單維度(1維)
3. 支援矩陣 **(Martix)** 的類別

#### 基本運用
    import numpy as np
    
    a = np.array([1, 2, 3, 4, 5, 6])
    print("陣列 => {0}".format(a))

    # 陣列轉成 m * n 距陣
    a.shape = (3, 2)
    print("距陣 => {0}".format(a))

    # 距陣轉成陣列
    b = a.ravel()
    print("陣列 => {0}".format(b))

#### 距陣串接
    a = np.array([[1, 2], [3, 4]])
    b = np.array([[5, 6], [7, 8]])
    print("矩陣 => {0}".format(a))
    print("矩陣 => {0}".format(b))

    c = np.vstack([a, b])
    print("堆疊串接 => {0}".format(c))
    c = np.hstack([a, b])
    print("水平串接 => {0}".format(c))

#### 基本函式
    a = np.exp(2)
    print("exp => {0}".format(a))
    a = np.exp2(2)
    print("exp2 => {0}".format(a))
    a = np.sqrt(2)
    print("sqrt => {0}".format(a))
    a = np.sin([2,3])
    print("sin => {0}".format(a))
    a = np.log(2)
    print("log => {0}".format(a))
    a = np.log10(2)
    print("log10 => {0}".format(a))
    a = np.log2(2)
    print("log2 => {0}".format(a))
    a = np.max([1, 2, 3, 4])
    print("max => {0}".format(a))

#### 二維陣列操作
    a = np.array([[1, 2], [-1, 4]])
    b = np.array([[2, 0], [3, 4]])

    c = a * b
    d = np.dot(a, b) # 或者 a.dot(b)

    print("元素相乘 => {0}".format(c))
    print("矩陣相乘 => {0}".format(d))

#### 線性代數
    import numpy.linalg as ling 
    # from numpy import linalg 與上方引用相同，建議用上方的寫法

    a = np.array([[1, 2], [-1, 4]])
    b = np.array([[2, 0], [3, 4]])

    print("轉置 a => {0}".format(a.transpose()))
    print("逆矩陣 a => {0}".format(ling.inv(a)))

    eigenvalues, eigenvectors = linalg.eig(a)
    print("特徵值 a => {0}".format(eigenvalues))
    print("特徵矩陣 a => {0}".format(eigenvectors))

# Reference
[numpy 常用操作](https://blog.csdn.net/Jerr__y/article/details/54968548)    