---
title: Python 使用 Scikit-learn 機器學習
date: 2019-07-15 12:27:52
tags:
 - Machine Learning
categories:
 - Python
---

# 常見的機器學習類別
1. 監督式學習 **(supervised learning)**
2. 非監督式學習 **(unsupervised learning)**
3. 半監督式學習 **(semi-supervised learning)**
4. 增強式學習 **(reinforcement learning)**

#### 監督式學習
    在訓練的過程中告訴機器答案、也就是「有標籤」的資料，比如給機器各看了 1000 張蘋果和橘子的照片後、詢問機器新的一張照片中是蘋果還是橘子
    若輸入資料有標籤，即為監督式學習

#### 非監督式學習
    訓練資料沒有標準答案、不需要事先以人力輸入標籤，故機器在學習時並不知道其分類結果是否正確。訓練時僅須對機器提供輸入範例，它會自動從這些範例中找出潛在的規則
    資料沒標籤、讓機器自行摸索出資料規律的則為非監督式學習，如集群（Clustering）演算法

#### 半監督式學習
    介於監督學習與非監督學習之間
    在將資料分群的過程當中，先使用有標籤過的資料先切出一條分界線，再利用剩下無標籤資料的整體分布，調整出兩大類別的新分界。如此不但具有非監督式學習高自動化的優點，又能降低標籤資料的成本

#### 增強學習
    透過觀察環境而行動，並會隨時根據新進來的資料逐步修正、以獲得最大利益

#### Scikit-learn
1. scikit-learn 套件是專門用來實作機器學習以及資料採礦
2. 安裝模組 **pip install -U scikit-learn**
![Architecture](1.png)

# Reference
[硬塞科技字典](https://www.inside.com.tw/article/9945-machine-learning)