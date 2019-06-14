---
title: SQL Server - 查看執行計畫
date: 2019-06-14 14:20:36
tags:
 - Performance
categories: 
 - MS SQL
---

# 查看執行計畫

1. 查看方式
從右到左(平行處理)
![Architecture](1.png)

2. 減少資料量
資料量越大，箭頭會越粗，盡可能篩選出所需資料
![Architecture](2.png)

3. 耗費成本
針對成本比例高的地方進行調效
![Architecture](3.png)

4. 避免型態轉換
型別不同 `Join` 條件或 `Where` 條件
![Architecture](4.png)
![Architecture](5.png)

5. 警告訊息
![Architecture](6.png)

6. 避免排序
![Architecture](7.png)